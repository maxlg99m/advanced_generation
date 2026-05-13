# Agent: Generate QA Checklists

## Role
Ты запускаешь полный no-code пайплайн генерации QA чек-листов: ingestion -> analysis -> checklist generation -> coverage review -> CSV export.

## Goal
Сформировать атомарные QA чек-листы на основе ТЗ и/или PNG/JPEG дизайна, проверить покрытие и сохранить итоговый CSV.

## Inputs
- `data/input/requirements/`
- `data/input/design/`
- rules layer:
  - `.codex/rules/agent-system_rules/*.md`;
  - `.codex/rules/testrail_rules/*.md`
- `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`
- `.codex/prompts/agents/Ingestion_Agent.md`
- `.codex/prompts/agents/Analysis_Agent.md`
- `.codex/prompts/agents/Checklist_Generation_Agent.md`
- `.codex/prompts/agents/Coverage_Review_Agent.md`
- `.codex/prompts/agents/CSV_Export_Agent.md`
- релевантные skills из `.codex/skills/`

## Outputs
- `data/output/csv/final_checklists.csv`
- `logs/Questions.md`
- `logs/Mistakes.md`
- `logs/Details_and_Improve.md`

## Required Context To Read First
1. `.codex/rules/agent-system_rules/*.md`
2. `.codex/rules/testrail_rules/*.md`
3. `.codex/prompts/system/MASTER_QA_SYSTEM_PROMPT.md`
4. `.codex/prompts/agents/Ingestion_Agent.md`
5. `.codex/prompts/agents/Analysis_Agent.md`
6. `.codex/prompts/agents/Checklist_Generation_Agent.md`
7. `.codex/prompts/agents/Coverage_Review_Agent.md`
8. `.codex/prompts/agents/CSV_Export_Agent.md`
9. `.codex/skills/WEB_QA_CHECKLIST_EXTENSION.md` или `.codex/skills/MOBILE_QA_CHECKLIST_EXTENSION.md`, если платформа определена как web/mobile.

## Pre-flight
1. Убедитесь, что пользователь ранее выполнил `/init-qa-context` в текущем диалоге. Если нет — остановитесь и попросите сначала выполнить `/init-qa-context`.
2. Проверьте наличие входных директорий:
   - `data/input/requirements/`;
   - `data/input/design/`.
3. Проверьте, что есть хотя бы один входной файл: ТЗ или PNG/JPEG дизайн.
4. Если нет ни ТЗ, ни дизайна, остановитесь и попросите добавить входные данные.
5. Если в `data/input/design/` есть дизайн не в формате `.png`, `.jpg`, `.jpeg`, остановитесь и зафиксируйте блокирующую проблему по `.codex/rules/agent-system_rules/02-inputs.md`.
6. Создайте целевые директории `data/output/csv/` и `logs/`, если они отсутствуют.

## Algorithm
1. Выполните `Ingestion Agent`:
   - примените `.codex/rules/agent-system_rules/02-inputs.md` и `.codex/rules/agent-system_rules/03-pipeline-handoff.md`;
   - получите handoff package;
   - остановитесь, если handoff содержит блокирующий статус.
2. Выполните `Analysis Agent`:
   - используйте handoff от `Ingestion Agent`;
   - примените `.codex/rules/agent-system_rules/04-coverage.md`, `.codex/rules/agent-system_rules/05-traceability.md` и `.codex/rules/testrail_rules/05-platform.md`;
   - получите handoff package;
   - остановитесь, если handoff содержит блокирующий статус.
3. Выполните `Checklist Generation Agent`:
   - используйте handoff от `Analysis Agent`;
   - примените `.codex/rules/agent-system_rules/04-coverage.md`, `.codex/rules/agent-system_rules/05-traceability.md` и `.codex/rules/testrail_rules/*.md`;
   - получите handoff package;
   - остановитесь, если handoff содержит блокирующий статус.
4. Выполните `Coverage Review Agent`:
   - используйте handoff от `Checklist Generation Agent`;
   - примените `.codex/rules/agent-system_rules/04-coverage.md`, `.codex/rules/agent-system_rules/06-validation.md` и `.codex/rules/testrail_rules/*.md`;
   - получите handoff package;
   - если есть блокеры, сохраните замечания в `logs/` и не создавайте финальный CSV;
   - остановитесь, если handoff содержит блокирующий статус.
5. Выполните `CSV Export Agent`:
   - используйте handoff от `Coverage Review Agent`;
   - примените `.codex/rules/testrail_rules/01-csv-contract.md`, `.codex/rules/testrail_rules/02-sections.md`, `.codex/rules/testrail_rules/03-priority.md`, `.codex/rules/testrail_rules/04-scenario.md`, `.codex/rules/testrail_rules/05-platform.md`, `.codex/rules/testrail_rules/06-labels.md`, `.codex/rules/testrail_rules/07-title.md`, `.codex/rules/testrail_rules/08-expected-results.md` и `.codex/rules/agent-system_rules/06-validation.md`;
   - сохраните итоговый CSV в `data/output/csv/final_checklists.csv`.
6. Сохраните post-run логи:
   - примените `.codex/rules/agent-system_rules/08-logs.md`;
   - `logs/Questions.md` — открытые вопросы;
   - `logs/Mistakes.md` — найденные проблемы и блокеры;
   - `logs/Details_and_Improve.md` — детали покрытия и предложения улучшений.
7. Сообщите пользователю краткий финальный отчет по `.codex/rules/agent-system_rules/07-final-report.md`.

## Completion Criteria
- `data/output/csv/final_checklists.csv` создан только после успешного coverage review.
- Все обязательные поля CSV заполнены и экранированы согласно rules.
- В `logs/` сохранены только разрешенные markdown-логи согласно `.codex/rules/agent-system_rules/08-logs.md`.
- Агент кратко сообщил результат пользователю и остановился.

## Forbidden
- Экспортировать CSV без прохождения `Coverage Review Agent`.
- Придумывать требования, которых нет во входных материалах.
- Создавать программный код.
- Переходить к очистке workspace.
