# Fluent Swap Android — рабочий контекст

## Состояние на 8 сентября 2026

- Репозиторий содержит только начальные README/LICENSE; Android-проект ещё не создан.
- Backend V1 предоставляет единый WebSocket endpoint `/ws/matchmaking`.
- Поддержаны `find_partner`, `cancel_search`, `send_message`; сервер присылает
  `search_waiting`, `search_cancelled`, `match_found`, `receive_message`, `error`.
- В V1 нет аккаунтов, истории, retries и гарантированной доставки сообщений.
- В core продолжается Redis/reservation lifecycle. Перед end-to-end задачами
  нужно подтвердить доступный staging `wss://` URL и стабильность контракта.

## MVP flow

`Start → выбор native/learning языка → поиск → ожидание/отмена → найден партнёр → чат → выход`

## Решения

- Android-only MVP: Kotlin + Jetpack Compose. Kotlin Multiplatform откладывается:
  до дедлайна важнее надёжно опубликовать одну платформу.
- Один Gradle app module на MVP.
- UI: screen-level ViewModel, immutable state, UDF.
- Data: отдельные protocol DTO/codec и lifecycle-aware WebSocket client.
- Монетизация: RevenueCat entitlement `pro`; бесплатный основной flow остаётся
  тестируемым, а небольшая premium-функция определяется в соответствующей Issue.

## Definition of Done для каждой Issue

- acceptance criteria выполнены;
- нет несвязанных изменений;
- developer проверил diff и ручной сценарий;
- unit tests/lint/debug build зелёные (если уже настроены);
- PR связан с Issue и содержит скриншоты для UI-изменений;
- review Critical/Should fix закрыты.

## Активная задача

Issue #2 — создать минимальный Android skeleton.

## Блокеры/решения владельца

- Указать application ID и финальное store name до настройки RevenueCat/Play Console.
- Предоставить staging `wss://` endpoint, доступный из США.
- Выбрать premium benefit, который не блокирует проверку базового matchmaking/chat.
- Зарегистрировать проект на Devpost и начать Play Console listing заранее.
