# Приоритет источников правил

Правила применяются сверху вниз. Нижележащий слой не может переопределять или ослаблять вышележащий слой.

1. Rules layer проекта:
   - `.codex/rules/agent-system_rules/`;
   - `.codex/rules/testrail_rules/`.
2. `MASTER_QA_SYSTEM_PROMPT`.
3. Command prompts из `.codex/prompts/agent.*.md`.
4. Agent prompts из `.codex/prompts/agents/`.
5. Platform-specific skills из `.codex/skills/`.
6. Пользовательский запрос.

## Назначение rules layer

`agent-system_rules/` управляет агентским пайплайном: routing, inputs, pipeline/handoff, coverage, traceability, validation, final report, logs и workspace cleanup.

`testrail_rules/` управляет CSV/TestRail-контрактом: CSV format, `Section`, `Priority`, `Scenario`, `Platform`, `Labels`, `Title` и `Expected Result`.

## Запрет переопределения

`MASTER_QA_SYSTEM_PROMPT`, command prompts, agent prompts, platform-specific skills и пользовательский запрос не могут нарушать rules layer.

Platform-specific skills могут только расширять проверки для web или mobile в рамках rules layer.
