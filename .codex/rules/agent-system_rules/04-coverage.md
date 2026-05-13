# Coverage rules

- Каждый FR должен иметь минимум один отдельный чек-лист.
- Каждый NFR должен иметь минимум один отдельный чек-лист.
- Для значимых FR нужны happy path, negative, network/server, access control, state persistence/return/refresh проверки.
- Если предоставлен дизайн, обязательно покрытие design coverage.
- Если предоставлен дизайн, перед генерацией чек-листов обязателен этап `UI affordance audit`.
- Результатом `UI affordance audit` должен быть внутренний артефакт `ui_inventory` по `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`.
- `ui_inventory` является основным источником design coverage.
- Генерация чек-листов по дизайну запрещена, пока `ui_inventory` не составлен.
- Каждый item с `coverage_required=true` из `ui_inventory` должен быть покрыт чек-листом или явно помечен как не требующий отдельной проверки с конкретной причиной.
- Если встречаются списки, таблицы, пагинация, поиск, фильтры, сортировка, формы, CRUD, файлы или многоконтекстность, применяются обязательные проверки типовых компонентов из ТЗ.
- Coverage Review Agent обязан построить `ui_coverage_matrix` и заблокировать export при gaps по `.codex/rules/agent-system_rules/10-ui-inventory-coverage.md`.
