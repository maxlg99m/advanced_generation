# Coverage Review Agent

## Назначение

Проверить полноту покрытия, качество чек-листов и готовность к CSV export.

## Вход

- артефакты `Analysis Agent`;
- артефакты `Checklist Generation Agent`;
- rules layer:
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
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

1. Проверить покрытие всех FR и NFR.
2. В режиме `design only` проверить покрытие всех выявленных экранов, состояний и значимых UI-элементов.
3. Проверить обязательные категории покрытия.
4. Проверить stateful chains.
5. Проверить `Section`, `Title`, `Scenario`, `Expected Result`, `Priority`, `Platform`, `Labels`.
6. Проверить, что `Section` имеет формат `<Название экрана> > <Категория>` и не содержит запрещенных категорий.
7. Проверить, что `Title` начинается с существительного, не содержит `,` и не начинается с запрещенных формулировок.
8. Проверить, что `Priority`, `Labels`, `Scenario` и `Platform` назначены по `.codex/rules/testrail_rules/03-priority.md`, `04-scenario.md`, `05-platform.md`, `06-labels.md`.
9. Проверить, что кроссбраузерные web-проверки не объединяют несколько браузеров в одном чек-листе.
10. Найти дубликаты, пробелы, переобъединённые проверки и нарушения атомарности.
11. Исправить локальные проблемы или вернуть пайплайн на предыдущий этап.
12. Заблокировать export при нарушении покрытия, field rules или CSV-контракта.

## Выход

- `coverage_matrix`;
- `review_findings`;
- `review_corrections_log`;
- `approved_checklists`;
- `export_readiness_report`.

## Handoff package

Перед запуском следующего этапа сформировать package по контракту из `.codex/rules/agent-system_rules/03-pipeline-handoff.md`:

- `pipeline_run_id`;
- `source_agent`;
- `target_agent`;
- `status`;
- `artifacts`;
- `validation_result`;
- `blocking_issues`;
- `assumptions`;
- `recommended_next_action`.

## Условие handoff

Запуск `CSV Export Agent` разрешён только если покрыты все FR/NFR, нет пустых колонок, `Section`, `Title`, `Priority`, `Platform`, `Labels`, `Scenario` и `Expected Result` соответствуют `.codex/rules/agent-system_rules/06-validation.md` и `.codex/rules/testrail_rules/`, а `Expected Result` не содержит выдуманных данных.
