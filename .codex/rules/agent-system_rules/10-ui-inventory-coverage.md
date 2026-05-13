# UI inventory coverage

Этот контракт применяется только если входные данные содержат PNG/JPEG дизайн.

## Purpose

`ui_inventory` — основной артефакт покрытия дизайна. Он нужен, чтобы агент сначала составил реестр значимых UI-элементов, затем покрыл каждый обязательный элемент чек-листом или явно объяснил, почему отдельная проверка не нужна.

## UI inventory item

Каждый значимый UI-элемент фиксируется как item.

Обязательные поля:

- `component_id`
- `screen`
- `component_type`
- `visible_name`
- `affordances`
- `coverage_required`
- `risk_level`
- `checklist_ids`
- `not_covered_reason`

Опциональные поля, если они помогают покрытию:

- `parent_component`
- `states`
- `data_displayed`
- `interaction_expected`
- `coverage_reason`

Допустимые группы `component_type`:

- `navigation`
- `input`
- `action`
- `data_display`
- `collection`
- `overlay`
- `feedback_state`
- `system_control`

## Coverage obligations

`coverage_required=true`, если элемент:

- запускает действие;
- принимает ввод;
- меняет состояние;
- открывает экран, модалку, drawer или список;
- ищет, фильтрует, сортирует или меняет данные;
- отображает бизнес-значимые данные;
- является навигацией;
- показывает состояние empty, loading, error, disabled или selected;
- может привести к потере данных, неверному действию или обходу доступа.

Декоративные, неинтерактивные и не несущие данных элементы могут иметь `coverage_required=false`, если указана конкретная причина.

Повторяющиеся группы покрываются по каждому уникальному элементу, если элементы имеют разные данные, действия, состояния, риски или affordance-признаки. Обобщенная проверка группы не заменяет покрытие отдельных уникальных элементов.

## Risk-based depth

- `critical`: ядро пользовательского сценария, доступ, деньги, данные, необратимые действия. Нужны positive, negative или blocked-state проверки, а также access/persistence/network checks если применимо.
- `high`: важные пользовательские действия, создание, изменение, поиск, фильтрация. Нужны positive и минимум один negative или state check.
- `medium`: отображение данных, состояния и UX-affordance. Нужен минимум один точечный checklist.
- `low`: декоративные или вторичные элементы. Отдельный checklist не обязателен при конкретном `not_covered_reason`.

## UI coverage matrix

Coverage Review Agent строит `ui_coverage_matrix` по группам компонентов:

- `component_type`
- `found_count`
- `coverage_required_count`
- `covered_count`
- `not_covered_with_reason_count`
- `gap_count`

`gap_count` должен быть `0` для каждой группы.

## Quality gates

CSV export запрещен, если:

- дизайн предоставлен, но `ui_inventory` отсутствует или пустой;
- `ui_inventory` не содержит items для каждого выявленного экрана;
- item с `coverage_required=true` не имеет `checklist_ids` и не имеет конкретного `not_covered_reason`;
- item без checklist имеет формальный или пустой `not_covered_reason`;
- повторяющаяся группа покрыта выборочно без конкретной причины;
- `ui_coverage_matrix` отсутствует;
- в `ui_coverage_matrix` есть `gap_count > 0`;
- `pre_export_lint.status = failed`.

## Pre-export lint

Перед CSV export обязателен `pre_export_lint`.

`pre_export_lint` должен вернуть:

- `status`: `passed` или `failed`
- `blockers`: список блокеров
- `summary`: краткий итог проверки

Минимальные проверки lint:

- все обязательные UI items покрыты или имеют конкретный `not_covered_reason`;
- все интерактивные элементы имеют positive checklist;
- ввод данных имеет validation checklist, если в дизайне или ТЗ видны ограничения, обязательность или состояние ошибки;
- действия имеют negative или blocked-state checklist, если действие может быть запрещено;
- видимые empty/loading/error states покрыты;
- навигационные элементы имеют проверку перехода или сохранения контекста;
- элементы, отображающие данные, имеют проверку отображения данных;
- повторяющиеся группы не покрыты выборочно без причины.

## Required workflow

1. Найти screens.
2. Для каждого screen найти значимые UI items.
3. Для каждого item определить affordances, `coverage_required` и `risk_level`.
4. Сгенерировать checklists и заполнить `checklist_ids`.
5. Для непокрытых items заполнить конкретный `not_covered_reason`.
6. Построить `ui_coverage_matrix`.
7. Выполнить `pre_export_lint`.
8. Если любой quality gate нарушен, CSV export запрещен.
