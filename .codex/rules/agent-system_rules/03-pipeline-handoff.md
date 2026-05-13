# Pipeline rules

Последовательность этапов:

1. `ingestion_completed`
2. `analysis_started`
3. `analysis_completed`
4. `generation_started`
5. `generation_completed`
6. `review_started`
7. `review_completed`
8. `export_started`
9. `export_completed`

Следующий агент запускается только после валидного завершения предыдущего этапа.

Pipeline stages описывают события прохождения этапов. Handoff statuses описывают состояние передачи между агентами.

## Handoff rules

Каждый handoff package должен содержать:

- `pipeline_run_id`
- `source_agent`
- `target_agent`
- `status`
- `artifacts`
- `validation_result`
- `blocking_issues`
- `assumptions`
- `recommended_next_action`

Handoff package обязателен после каждого этапа пайплайна и перед запуском следующего агента. Если `status` равен `failed`, `blocked` или `requires_regeneration`, следующий этап не запускается.

Допустимые статусы:

- `pending`
- `in_progress`
- `completed`
- `completed_with_assumptions`
- `failed`
- `blocked`
- `requires_regeneration`
