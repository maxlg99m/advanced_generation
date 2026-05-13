# Ingestion Agent

## Назначение

Принять входные артефакты, проверить форматы и подготовить их для анализа.

## Вход

- файлы из `data/input/requirements`;
- файлы из `data/input/design`;
- пользовательский запрос;
- rules layer:
  - `.codex/rules/agent-system_rules/02-inputs.md`;
  - `.codex/rules/agent-system_rules/03-pipeline-handoff.md`.

## Обязанности

1. Проверить наличие входных файлов.
2. Определить режим работы:
   - `requirements only`;
   - `design only`;
   - `requirements + design`;
   - `no inputs`.
3. Проверить форматы ТЗ.
4. Проверить форматы дизайна.
5. Заблокировать неподдерживаемые дизайн-форматы.
6. Заблокировать `no inputs` до добавления ТЗ или PNG/JPEG дизайна.
7. Для `.docx` и `.pdf` ТЗ проверить доступность читаемого текста.
8. Подготовить нормализованное описание входных артефактов для `Analysis Agent`.
9. Зафиксировать отсутствующие, неподдерживаемые или неоднозначные входные данные.

## Форматы

Допустимые форматы определяются только в `.codex/rules/agent-system_rules/02-inputs.md`.

## Выход

- `input_inventory`;
- `input_validation_result`;
- `normalized_requirements_reference`;
- `normalized_design_images_reference`;
- `blocking_input_issues`;
- `recommended_next_action`.

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

Запуск `Analysis Agent` разрешён только если входные данные валидны или отсутствующие данные явно зафиксированы как допустимое ограничение режима работы.
