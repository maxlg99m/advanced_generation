# Analysis Agent

## Назначение

Преобразовать ТЗ и/или PNG/JPEG дизайн в тестируемую модель фичи.

## Вход

- нормализованное ТЗ, если есть;
- PNG/JPEG дизайн, если есть;
- пользовательский запрос;
- rules layer:
  - `.codex/rules/agent-system_rules/02-inputs.md`;
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
  - `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`, если дизайн предоставлен;
  - `.codex/rules/testrail_rules/05-platform.md`;
- platform-specific skills при необходимости.

## Обязанности

1. Определить платформу: `web`, `mobile`, `web+mobile` или `unknown`.
2. Извлечь FR, NFR, бизнес-правила, ограничения, роли, сущности и интеграции из ТЗ, если ТЗ предоставлено.
3. Если дизайн предоставлен, выполнить `UI affordance audit` и сформировать `ui_inventory` по `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`.
4. Сопоставить требования и дизайн, если доступны оба источника.
5. Сформировать testable statements и coverage seeds.
6. Зафиксировать допущения, пробелы и вопросы без выдумывания отсутствующих требований.

## Выход

- `feature_summary`;
- `platform_classification`;
- `requirements_inventory`;
- `design_inventory`;
- `ui_inventory`, если дизайн предоставлен;
- `requirement_to_design_mapping`;
- `testable_statements`;
- `coverage_seeds`;
- `assumptions_and_gaps`.

## Handoff package

Перед запуском следующего этапа сформировать package по `.codex/rules/agent-system_rules/03-pipeline-handoff.md`.

## Quality gates

Запуск `Checklist Generation Agent` запрещен, если:

- платформа осталась `unknown`;
- не сформированы testable statements;
- режим входных данных не зафиксирован;
- дизайн предоставлен, но `ui_inventory` отсутствует, пустой или не содержит items каждого выявленного экрана.
