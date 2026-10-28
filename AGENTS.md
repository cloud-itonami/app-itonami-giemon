# giemon — agent notes

## CI/CD (CDCI) に JVM と nbb を置かない

Owner 指示 2026-10-22 (Linear JUN-290)、および workspace `AGENTS.md` の
「CI/CD（CDCI）に JVM と nbb を置かない」に従い、`.github/workflows/ci.yml`
を削除した (JUN-494)。

- **外したもの**: `actions/setup-java`、`DeLaGuardo/setup-clojure`、
  `clojure -M:test`、`clojure -M:lint`。
- **native gate**: なし。Amu selfhost の native gate (負の対照つき) は
  この repo にまだ無く、**blocked** として記録する。fallback は置かない。
- **覆わなくなった検査**: push / PR 時の自動テストと lint。
  なお削除前の `clojure -M:test` は cognitect runner が `.cljk` を
  見つけられず "Ran 0 tests" で緑になっていた (実質ゼロ検査)。
  実際に効くテストは手元の `kbb -M:test` (49 tests / 140 assertions、
  失敗時 exit 1) のみで、CI からは走らない。lint は未実施。
- **再開条件**: native gate を整備し、失敗アサーション挿入で exit 1 に
  なる負の対照を確認してから workflow を追加する。
