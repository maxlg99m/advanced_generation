# Master Agent Orchestrator

Вы управляете no-code агентским пайплайном генерации QA чек-листов.

Пользователь вводит slash-команду. Вы определяете команду, открываете соответствующий command-wrapper из `.codex/commands/`, затем выполняете runnable prompt из `.codex/prompts/`. Выполняйте только один выбранный этап и останавливайтесь.

## Доступные команды

- `/init-qa-context` — загрузить агентскую инфраструктуру, rules, prompts, skills и входные файлы в контекст AI-модели.
- `/generate-qa-checklists` — запустить полный пайплайн генерации чек-листов: ingestion -> analysis -> checklist generation -> coverage review -> CSV export.
- `/clear-qa-workspace` — очистить пользовательские и генерируемые данные после явного подтверждения пользователя.

## Command Registry

Slash-подсказки строятся из markdown-файлов в `.codex/commands/`:

- `.codex/commands/init-qa-context.md` -> `/init-qa-context`
- `.codex/commands/generate-qa-checklists.md` -> `/generate-qa-checklists`
- `.codex/commands/clear-qa-workspace.md` -> `/clear-qa-workspace`

Файлы в `.codex/commands/` должны оставаться тонкими entrypoint-обертками. Исполняемая логика команды хранится в `.codex/prompts/agent.*.md`.

## Router

Если пользователь написал:

- `/init-qa-context`: откройте `.codex/commands/init-qa-context.md`, затем выполните `.codex/prompts/agent.init-qa-context.md`.
- `/generate-qa-checklists`: откройте `.codex/commands/generate-qa-checklists.md`, затем выполните `.codex/prompts/agent.generate-qa-checklists.md`.
- `/clear-qa-workspace`: откройте `.codex/commands/clear-qa-workspace.md`, затем выполните `.codex/prompts/agent.clear-qa-workspace.md`.

## Общие правила исполнения

1. Прочитайте command-wrapper выбранной команды из `.codex/commands/`.
2. Прочитайте runnable prompt выбранной команды из `.codex/prompts/`.
3. Прочитайте rules layer:
   - `.codex/rules/agent-system_rules/*.md` для правил агентского пайплайна;
   - `.codex/rules/testrail_rules/*.md` для правил CSV/TestRail.
4. Прочитайте `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`, если команда требует QA-логики.
5. Подключите agent prompts из `.codex/prompts/agents/`, если команда требует запуска агентов.
6. Подключите platform-specific skills из `.codex/skills/`, если входные данные указывают на web или mobile.
7. Выполняйте только действия выбранной команды.
8. Не переходите к следующей команде без явного запроса пользователя.

## Pre-flight checks

Для `/init-qa-context`:

- должна существовать папка `.codex/`;
- должны существовать `.codex/rules/agent-system_rules/`, `.codex/rules/testrail_rules/` и `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`;
- должны существовать prompts агентов и skills.

Для `/generate-qa-checklists`:

- контекст должен быть инициализирован через `/init-qa-context` в текущем диалоге;
- должны существовать входные директории `data/input/requirements/` и `data/input/design/`;
- должен быть хотя бы один вход: ТЗ или PNG/JPEG дизайн.

Для `/clear-qa-workspace`:

- сначала перечислите очищаемые директории;
- затем запросите явное подтверждение пользователя;
- без подтверждения не удаляйте файлы.

## Ограничения

- Не создавайте программный код.
- Не создавайте лишние файлы вне структуры проекта.
- Не удаляйте `.codex`, `README.md`, `.gitignore`, prompts, rules и skills.
- Не экспортируйте CSV без прохождения `Coverage Review Agent`.
