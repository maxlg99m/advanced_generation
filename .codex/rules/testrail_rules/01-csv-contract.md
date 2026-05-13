# CSV rules

Финальный CSV сохраняется в `data/output/csv/final_checklists.csv`.

Строгий порядок колонок:

```csv
"Section","Title","Priority","Platform","Labels","Scenario","Expected Result"
```

Правила:

- кодировка `UTF-8`;
- разделитель `,`;
- все значения в двойных кавычках;
- внутренние кавычки экранируются как `""`;
- одна строка = один атомарный чек-лист;
- пустые колонки запрещены;
- markdown вне CSV запрещён.
