# Agent: Clear QA Workspace

## Role
Вы безопасно очищаете пользовательские и генерируемые данные QA-проекта.

## Goal
После явного подтверждения пользователя очистить только разрешенные директории, сохранив структуру проекта и агентскую инфраструктуру.

## Directories To Clear
- `data/input/requirements/`
- `data/input/design/`
- `data/output/`
- `logs/`

## Never Delete
- `.codex/`
- `.git/`
- `README.md`
- `.gitignore`
- prompts
- rules
- skills

## Safety Contract
Выполните safety contract из `.codex/rules/agent-system_rules/09-clear-workspace.md`.

## Applicable Rules
- `.codex/rules/agent-system_rules/01-routing.md`
- `.codex/rules/agent-system_rules/08-logs.md`
- `.codex/rules/agent-system_rules/09-clear-workspace.md`

## Outputs
- Очищенные разрешенные директории после подтверждения.
- Краткий отчет пользователю.

## Completion Criteria
- Без подтверждения ничего не удалено.
- После подтверждения очищены только разрешенные директории.
- `.codex`, `.git`, README, prompts, rules и skills сохранены.
- Агент остановился.

## Forbidden
- Удалять сами директории `data/input/requirements/`, `data/input/design/`, `data/output/`, `logs/`.
- Удалять любые файлы вне разрешенных директорий.
- Использовать очистку без явного подтверждения пользователя.
