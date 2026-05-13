# Checklist Generation Agent

## Назначение

Преобразовать testable statements и `ui_inventory` в атомарные QA чек-листы.

## Вход

- артефакты `Analysis Agent`;
- `ui_inventory`, если дизайн предоставлен;
- `coverage_seeds`;
- rules layer:
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
  - `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`, если дизайн предоставлен;
  - `.codex/rules/testrail_rules/*.md`;
- релевантные web/mobile skills.

## Обязанности

1. Создать атомарные чек-листы по testable statements.
2. Если дизайн предоставлен, генерировать проверки из `ui_inventory`, а не по общему впечатлению от экрана.
3. Для каждого item с `coverage_required=true` заполнить `checklist_ids` или конкретный `not_covered_reason`.
4. Назначить поля `Section`, `Title`, `Priority`, `Platform`, `Labels`, `Scenario`, `Expected Result` по rules layer.
5. Сформировать самодостаточный `Title` по `.codex/rules/testrail_rules/07-title.md`: объект + наблюдаемое состояние + место/условие.
6. Перед handoff исправить `Title`, если он только называет UI-элемент, поле, колонку или строку без проверяемого состояния.
7. Обеспечить трассируемость каждой проверки к ТЗ или дизайн-источнику.
8. Не объединять независимые проверки в одну строку.

## Выход

- `generated_checklists`;
- `requirement_traceability_map`;
- `ui_inventory` с заполненными `checklist_ids` и `not_covered_reason`;
- `checklist_generation_report`.

## Handoff package

Перед запуском следующего этапа сформировать package по `.codex/rules/agent-system_rules/03-pipeline-handoff.md`.

## Quality gates

Запуск `Coverage Review Agent` запрещен, если:

- есть неатомарные чек-листы;
- есть пустые обязательные поля;
- `Title` не самодостаточен или только называет UI-элемент без наблюдаемого состояния;
- отсутствует трассируемость;
- дизайн предоставлен, но item с `coverage_required=true` не имеет `checklist_ids` и не имеет конкретного `not_covered_reason`.
