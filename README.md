# QA Checklist Agent System

> No-code агентская система для генерации QA чек-листов из ТЗ и PNG/JPEG дизайнов с экспортом в CSV для TestRail.

![No-code](https://img.shields.io/badge/no--code-agentic-blue)
![QA](https://img.shields.io/badge/QA-checklists-green)
![TestRail](https://img.shields.io/badge/export-TestRail%20CSV-orange)

## 🤖 Что это такое

Этот репозиторий содержит prompt-based инфраструктуру для AI-агента, который помогает:

- анализировать ТЗ;
- анализировать PNG/JPEG дизайн;
- выделять тестируемые требования, состояния и UI-компоненты;
- генерировать атомарные QA чек-листы;
- проверять покрытие FR/NFR и дизайна;
- экспортировать итоговый CSV для импорта в TestRail.

Проект не требует программного кода: основная логика описана в `.codex` через prompts, rules, skills и command-wrapper файлы.

## 🚀 Быстрый старт

1. Положите ТЗ в:

```text
data/input/requirements/
```

2. Положите компактный файл дизайна в формате `.png`, `.jpg` или `.jpeg` в:

```text
data/input/design/
```

3. Выполните проектную команду:

```text
/init-qa-context
```

4. После успешной инициализации выполните:

```text
/generate-qa-checklists
```

5. Итоговый CSV будет сохранён в:

```text
data/output/csv/final_checklists.csv
```

## ⌨️ Проектные команды

Команды являются no-code entrypoints для AI-модели:

| Команда | Назначение |
|---|---|
| `/init-qa-context` | Загружает README, `.codex`, rules, prompts, skills и входные файлы. Ничего не генерирует. |
| `/generate-qa-checklists` | Запускает полный пайплайн: ingestion -> analysis -> checklist generation -> coverage review -> CSV export. |
| `/clear-qa-workspace` | Очищает пользовательские и генерируемые данные после явного подтверждения. |

> Важно: в некоторых IDE эти команды могут не отображаться в slash-подсказках. В таком случае отправьте их как обычный текст, например: `Выполни /init-qa-context`.

## 📥 Входные данные

Поддерживаемые форматы ТЗ:

- `.md`
- `.txt`
- `.docx`
- `.pdf`

Поддерживаемые форматы дизайна:

- `.png`
- `.jpg`
- `.jpeg`

Неподдерживаемые дизайн-форматы, например `.fig`, `.webp`, Figma-ссылки и PDF-макеты, считаются блокирующей проблемой.

## 🗂️ Структура проекта

```text
📁 .codex/
  📄 README.md         # внутреннее описание агентской инфраструктуры
  📁 commands/         # тонкие command-wrapper файлы для проектных команд
  📁 orchestrator/
    📄 master-agent.md # основная точка входа для команд
  📁 prompts/
    📄 agent.*.md      # исполняемые prompt-файлы команд
    📁 system/         # системные промпты
    📁 agents/         # промпты агентов пайплайна
  📁 rules/
    📁 agent-system_rules/ # правила агентского пайплайна
    📁 testrail_rules/     # правила CSV/TestRail
  📁 skills/           # platform-specific расширения для web/mobile

📁 data/
  📁 input/
    📁 requirements/ # входные ТЗ
    📁 design/       # входные PNG/JPEG дизайны
  📁 output/
    📁 csv/          # итоговый CSV

📁 logs/             # только Questions.md, Mistakes.md, Details_and_Improve.md

📄 README.md           # основная инструкция проекта
```
