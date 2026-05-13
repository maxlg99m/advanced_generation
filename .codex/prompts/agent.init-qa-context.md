# Agent: Init QA Context

## Role
Ты инициализируешь no-code QA-контекст для генерации чек-листов. Команда только загружает и проверяет контекст, ничего не генерирует.

## Goal
Прочитать доступную инфраструктуру проекта, проверить входные директории, определить режим работы и платформу, затем остановиться.

## Inputs
- `README.md`
- `.codex/README.md`
- `.codex/orchestrator/master-agent.md`
- rules layer:
  - `.codex/rules/agent-system_rules/*.md`;
  - `.codex/rules/testrail_rules/*.md`
- `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`
- `.codex/prompts/agents/*.md`
- `.codex/skills/*.md`
- файлы из `data/input/requirements/`
- файлы из `data/input/design/`

## Outputs
- Только краткий отчет пользователю в чате.
- Файлы не создавать и не изменять.

## Required Context To Read First
1. `README.md`
2. `.codex/README.md`
3. `.codex/orchestrator/master-agent.md`
4. `.codex/rules/agent-system_rules/*.md`
5. `.codex/rules/testrail_rules/*.md`
6. `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`
7. `.codex/prompts/agents/Ingestion_Agent.md`
8. `.codex/prompts/agents/Analysis_Agent.md`
9. `.codex/prompts/agents/Checklist_Generation_Agent.md`
10. `.codex/prompts/agents/Coverage_Review_Agent.md`
11. `.codex/prompts/agents/CSV_Export_Agent.md`
12. `.codex/skills/WEB_QA_CHECKLIST_EXTENSION.md`
13. `.codex/skills/MOBILE_QA_CHECKLIST_EXTENSION.md`

## Algorithm
1. Проверьте наличие обязательной структуры `.codex/`.
2. Проверьте наличие `data/input/requirements/` и `data/input/design/`.
3. Перечислите найденные входные файлы ТЗ и дизайна.
4. Определите режим:
   - `requirements only`;
   - `design only`;
   - `requirements + design`;
   - `no inputs`.
5. Определите платформу по ТЗ, дизайну и запросу пользователя:
   - `web`;
   - `mobile`;
   - `web+mobile`;
   - `unknown`.
6. Укажите активные prompts, rules groups (`agent-system_rules`, `testrail_rules`), skills и доступные slash-команды.
7. Сообщите, готов ли проект к `/generate-qa-checklists`.

## Completion Criteria
- Контекст прочитан настолько полно, насколько позволяют доступные файлы.
- Пользователь получил краткий отчет о режиме, платформе, входах и готовности к генерации.
- Агент остановился.

## Forbidden
- Генерировать чек-листы.
- Создавать CSV.
- Очищать рабочую область.
- Изменять файлы.
