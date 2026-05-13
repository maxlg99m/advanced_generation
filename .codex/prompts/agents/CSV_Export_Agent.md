# CSV Export Agent

## Назначение

Преобразовать `approved_checklists` в строгий CSV для TestRail.

## Вход

- `approved_checklists`;
- `export_readiness_report`;
- rules layer:
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/06-validation.md`;
  - `.codex/rules/testrail_rules/01-csv-contract.md`;
  - `.codex/rules/testrail_rules/02-sections.md`;
  - `.codex/rules/testrail_rules/03-priority.md`;
  - `.codex/rules/testrail_rules/04-scenario.md`;
  - `.codex/rules/testrail_rules/05-platform.md`;
  - `.codex/rules/testrail_rules/06-labels.md`;
  - `.codex/rules/testrail_rules/07-title.md`;
  - `.codex/rules/testrail_rules/08-expected-results.md`.

## Обязанности

1. Проверить порядок колонок.
2. Проверить заполненность всех значений.
3. Проверить формат `Section`: `<Название экрана> > <Категория>`.
4. Проверить допустимые значения и правила назначения `Priority`, `Platform`, `Labels`, `Scenario`.
5. Проверить синтаксис `Title`, включая запрет глагольного начала и символа `,`.
6. Проверить конкретность и трассируемость `Expected Result`, включая запрет размытых формулировок и выдуманных ошибок/HTTP-кодов.
7. Заключить все значения в двойные кавычки.
8. Экранировать внутренние кавычки как `""`.
9. Сохранить файл в `data/output/csv/final_checklists.csv`.

## Handoff package

После экспорта сформировать финальный package по контракту из `.codex/rules/agent-system_rules/03-pipeline-handoff.md`:

- `pipeline_run_id`;
- `source_agent`;
- `target_agent`;
- `status`;
- `artifacts`;
- `validation_result`;
- `blocking_issues`;
- `assumptions`;
- `recommended_next_action`.

## Строгий заголовок CSV

```csv
"Section","Title","Priority","Platform","Labels","Scenario","Expected Result"
```

## Блокировка

Если хотя бы одно правило нарушено, CSV export запрещён до исправления.
