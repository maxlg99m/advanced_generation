# Coverage Review Agent

## Назначение

Проверить полноту покрытия, качество чек-листов и готовность к CSV export.

## Вход

- артефакты `Analysis Agent`;
- артефакты `Checklist Generation Agent`;
- `ui_inventory`, если дизайн предоставлен;
- rules layer:
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
  - `.codex/rules/agent-system_rules/06-validation.md`;
  - `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`, если дизайн предоставлен;
  - `.codex/rules/testrail_rules/*.md`.

## Обязанности

1. Проверить покрытие FR/NFR, если есть ТЗ.
2. Проверить атомарность, трассируемость и соответствие полей CSV/TestRail rules.
3. Проверить качество `Title`: название должно быть самодостаточным и содержать объект, наблюдаемое состояние и место или условие.
4. Если дизайн предоставлен, построить `ui_coverage_matrix` по `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`.
5. Найти gaps, дубли, переобъединенные проверки, слабые `Title`, формальные `not_covered_reason` и нарушения rules layer.
6. Исправить локальные проблемы или вернуть пайплайн на предыдущий этап через blocking issues.

## Выход

- `coverage_matrix`;
- `ui_coverage_matrix`, если дизайн предоставлен;
- `review_findings`;
- `review_corrections_log`;
- `approved_checklists`;
- `export_readiness_report`.

## Handoff package

Перед запуском следующего этапа сформировать package по `.codex/rules/agent-system_rules/03-pipeline-handoff.md`.

## Quality gates

Запуск `CSV Export Agent` запрещен, если:

- есть непокрытые FR/NFR;
- нарушены field rules или CSV/TestRail rules;
- есть `Title`, который только называет UI-элемент, поле, колонку или строку без наблюдаемого состояния;
- дизайн предоставлен, но `ui_coverage_matrix` отсутствует;
- дизайн предоставлен, но в `ui_coverage_matrix` есть `gap_count > 0`;
- причины непокрытия формальные или скрывают реальные gaps.
