# .codex

Эта папка содержит no-code агентскую инфраструктуру для генерации QA чек-листов.

Содержимое:

- `orchestrator/` — master-agent, основная точка входа для команд.
- `commands/` — command-wrapper файлы для запуска этапов.
- `prompts/` — системные prompts и prompts агентов.
- `rules/` — обязательные правила маршрутизации, покрытия, CSV и валидации.
- `skills/` — platform-specific skills для web и mobile.

Rules layer:

- `rules/agent-system_rules/` — правила агентского пайплайна: routing, inputs, handoff, coverage, traceability, validation, final report, logs и workspace cleanup.
- `rules/testrail_rules/` — правила CSV/TestRail: CSV contract, Section, Priority, Scenario, Platform, Labels, Title и Expected Result.
