# Labels rules

`Labels`: только `design`, `smoke`, `regression`, `smoke;regression`.

Правила назначения:

- `design`: только проверки дизайна.
- `smoke`: только ключевые позитивные сценарии.
- `regression`: все проверки, кроме `design`.
- `smoke;regression`: ключевые позитивные regression-проверки, а также позитивные проверки с `Priority = Critical` или `Priority = High`, если это не design-проверки.
- Negative-проверки не получают `smoke` или `smoke;regression`; для них используется `regression`, если это не design-проверки.
