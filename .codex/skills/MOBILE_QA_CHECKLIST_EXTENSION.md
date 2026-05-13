# MOBILE_QA_CHECKLIST_EXTENSION

## Назначение

Расширяет базовые правила для мобильных приложений.

## Когда подключать

- ТЗ или запрос указывает на mobile.
- Указаны `Android`, `iOS` или обе платформы.
- Дизайн описывает мобильный интерфейс.

## Platform

Допустимые значения:

- `Android`
- `iOS`
- `Android;iOS`

Если логика идентична на Android и iOS, использовать `Android;iOS`. Если поведение отличается, разделять проверки.

## Title

Title должен соответствовать rules слоя и начинаться с существительного. Для платформенно-специфичных проверок допускается технический префикс перед существительным:

- Android: `(A)`;
- iOS: `(I)`.

Префикс используется только для чек-листов, которые относятся к одной конкретной платформе. Если логика идентична для Android и iOS, префикс не используется, а в `Platform` указывается `Android;iOS`.

## Обязательное покрытие

- Deep links / universal links / app links.
- Background / foreground.
- Kill app / relaunch.
- Permissions.
- Push notifications, если применимо.
- Мобильные сетевые сценарии.
- Secure storage / app storage / cache.
- Install / update / migration.
- System back / gesture / keyboard / safe area, если применимо.

## Ограничения

Этот skill не заменяет `MASTER_QA_SYSTEM_PROMPT` и не переопределяет CSV-контракт.
