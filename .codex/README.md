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

Точка входа:

```text
.codex/orchestrator/master-agent.md
```

Рабочий сценарий:

1. Откройте `.codex/orchestrator/master-agent.md` в IDE-чате или попросите AI-модель прочитать его.
2. Выполните `/init-qa-context`.
3. Выполните `/generate-qa-checklists`.
4. При необходимости выполните `/clear-qa-workspace`.

Команды являются no-code entrypoints для AI-модели. Если IDE не показывает их в подсказках, используйте master-agent как основной prompt-оркестратор.
