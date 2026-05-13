# Checklist Generation Agent

## Назначение

Преобразовать testable statements в атомарные QA чек-листы.

## Вход

- все артефакты `Analysis Agent`;
- `coverage_seeds`;
- rules layer:
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
  - `.codex/rules/testrail_rules/02-sections.md`;
  - `.codex/rules/testrail_rules/03-priority.md`;
  - `.codex/rules/testrail_rules/04-scenario.md`;
  - `.codex/rules/testrail_rules/05-platform.md`;
  - `.codex/rules/testrail_rules/06-labels.md`;
  - `.codex/rules/testrail_rules/07-title.md`;
  - `.codex/rules/testrail_rules/08-expected-results.md`;
- релевантные web/mobile skills.

## Обязанности

1. Создать атомарные проверки.
2. Покрыть positive, negative, edge-case, network/server, permission/security и stateful сценарии.
3. Применить platform-specific правила.
4. Назначить поля `Section`, `Title`, `Priority`, `Platform`, `Labels`, `Scenario`, `Expected Result`.
5. Формировать `Section` по `.codex/rules/testrail_rules/02-sections.md`.
6. Формировать `Title` по `.codex/rules/testrail_rules/07-title.md`.
7. Назначать `Priority`, `Labels`, `Scenario` и `Platform` по `.codex/rules/testrail_rules/03-priority.md`, `04-scenario.md`, `05-platform.md`, `06-labels.md`.
8. Для web-кроссбраузерных проверок создавать отдельный чек-лист на каждый браузер и указывать браузер в `Title`.
9. Обеспечить трассируемость каждой проверки к требованиям или к дизайн-источнику в режиме `design only`.
10. Не объединять несколько независимых проверок в одну строку.

## Выход

- `generated_checklists`;
- `requirement_traceability_map`;
- `checklist_generation_report`.

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

Запуск `Coverage Review Agent` разрешён только если все чек-листы атомарны, обязательные поля заполнены, трассируемость есть, platform-specific правила применены.
