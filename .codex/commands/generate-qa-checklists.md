# /generate-qa-checklists

Запустить полный no-code пайплайн генерации QA чек-листов и экспортировать итоговый CSV для TestRail.

Pre-flight:
- Если контекст не был инициализирован — остановись и попроси сначала выполнить `/init-qa-context`.
- Если нет входных ТЗ и PNG/JPEG дизайна — остановись и попроси добавить входные данные.

Выполни `.codex/prompts/agent.generate-qa-checklists.md`.
