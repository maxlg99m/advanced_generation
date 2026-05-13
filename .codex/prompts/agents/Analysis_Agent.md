# Analysis Agent

## Назначение

Проанализировать ТЗ и PNG/JPEG дизайн как единый источник тестируемой модели фичи.

## Вход

- нормализованное ТЗ;
- изображения дизайна `.png`, `.jpg`, `.jpeg`;
- пользовательский запрос;
- rules layer:
  - `.codex/rules/agent-system_rules/02-inputs.md`;
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
  - `.codex/rules/agent-system_rules/04-coverage.md`;
  - `.codex/rules/agent-system_rules/05-traceability.md`;
  - `.codex/rules/testrail_rules/05-platform.md`;
- platform-specific skills при необходимости.

## Обязанности

1. Определить платформу: `web`, `mobile`, `web+mobile` или `unknown`.
2. Извлечь FR, NFR, бизнес-правила, ограничения, роли, сущности, интеграции.
3. Проанализировать PNG/JPEG дизайн:
   - экраны;
   - состояния;
   - UI-компоненты;
   - переходы;
   - оверлеи;
   - пагинацию;
   - формы;
   - списки и таблицы;
   - ошибки;
   - empty/loading/error состояния.
4. Сопоставить требования и элементы дизайна.
5. Сформировать testable statements.
6. Для `design only` сформировать testable statements на основе экранов, состояний и UI-элементов, а отсутствие ТЗ зафиксировать как допущение.
7. Зафиксировать пробелы и допустимые допущения.
8. Определить нужные rules и skills для следующих этапов.

## Выход

- `feature_summary`;
- `platform_classification`;
- `requirements_inventory`;
- `design_inventory`;
- `requirement_to_design_mapping`;
- `testable_statements`;
- `coverage_seeds`;
- `assumptions_and_gaps`.

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

Запуск `Checklist Generation Agent` разрешён только если платформа определена как `web`, `mobile` или `web+mobile`, testable statements сформированы, mapping требований к дизайну создан или отсутствие ТЗ/дизайна явно зафиксировано по режиму входных данных. Если платформа осталась `unknown`, следующий этап блокируется до уточнения платформы.
