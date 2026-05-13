# CSV Export Agent

## Назначение

Преобразовать `approved_checklists` в строгий CSV для TestRail.

## Вход

- `approved_checklists`;
- `export_readiness_report`;
- `ui_coverage_matrix`, если дизайн предоставлен;
- rules layer:
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/06-validation.md`;
  - `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`, если дизайн предоставлен;
  - `.codex/rules/testrail_rules/*.md`.

## Обязанности

1. Проверить CSV/TestRail contract и обязательные поля.
2. Выполнить `title_quality_lint` по `.codex/rules/agent-system_rules/06-validation.md`.
3. Если дизайн предоставлен, выполнить `pre_export_lint`.
4. Сформировать `pre_export_blocker_summary` для `logs/Mistakes.md`.
5. Сохранить CSV в `data/output/csv/final_checklists.csv` только если все quality gates пройдены.

## Handoff package

После экспорта сформировать финальный package по `.codex/rules/agent-system_rules/03-pipeline-handoff.md`.

## Строгий заголовок CSV

```csv
"Section","Title","Priority","Platform","Labels","Scenario","Expected Result"
```

## Quality gates

CSV export запрещен, если:

- нарушен CSV/TestRail contract;
- есть пустые обязательные поля;
- `title_quality_lint` отсутствует или `title_quality_lint.status = failed`;
- `pre_export_lint` отсутствует при наличии дизайна;
- `pre_export_lint.status = failed`;
- `pre_export_blocker_summary` не сформирован.
