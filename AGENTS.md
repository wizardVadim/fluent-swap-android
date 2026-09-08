# Fluent Swap Android — правила для AI-агентов

## Цель

Собрать и выпустить Android-клиент Fluent Swap для поиска языкового партнёра и
текстового чата через WebSocket API `fluent-swap-core`.

Текущий приоритет — небольшой, надёжный Shipaton MVP. Голосовой/видеочат,
аккаунты, история сообщений и сложная многомодульность не входят в V1.

## Перед началом любой задачи

1. Прочитать `.agents/CONTEXT.md`, GitHub Issue и связанный контракт ядра.
2. Убедиться, что рабочая ветка создана от актуального `main`.
3. Кратко пересказать задачу, предполагаемые файлы и проверки.
4. Не расширять scope Issue без согласования.

## Технологические решения V1

- Kotlin, native Android, Jetpack Compose и Material 3.
- Single Activity, Navigation Compose, ViewModel, immutable `UiState` и UDF.
- Coroutines/Flow для асинхронности.
- Один WebSocket на пользовательскую сессию; transport не зависит от UI.
- Простая структура по feature/package. Не вводить multi-module, DI framework,
  database или repository abstraction без реальной потребности.
- Секреты, production URL и ключи нельзя коммитить. RevenueCat public SDK key
  передаётся через локальную/build-конфигурацию.

## Правила реализации

- Одна Issue — одна ветка — один Pull Request.
- Сначала минимальное корректное решение, затем тесты критичной логики.
- Не менять публичный WebSocket-контракт молча. При несовпадении остановиться и
  описать расхождение владельцу core.
- Не придумывать reconnect/доставку сообщений: в V1 core их не гарантирует.
- Не хранить текст переписки в логах и аналитике.
- Не использовать `GlobalScope`, blocking I/O на main thread и mutable state,
  доступный одновременно UI и transport.
- Все видимые строки должны быть в Android resources.
- Перед PR запустить доступные format/lint/unit tests и debug build.

## Как помогать начинающему разработчику

- Объяснять причину решения простыми словами.
- Давать небольшие изменения и явно перечислять изменённые файлы.
- Не переписывать соседний код без необходимости.
- Если агент сгенерировал код, разработчик обязан прочитать diff, запустить
  приложение и уметь объяснить основные решения до создания PR.

## Code review

Замечания делить на:

- **Critical** — crash, утечка данных, нарушение протокола, race/leak, сломанный flow.
- **Should fix** — архитектура, lifecycle, обработка ошибок, тестируемость, UX.
- **Optional** — улучшения без риска для MVP.

Проверять correctness, lifecycle, cancellation, state restoration, WebSocket
cleanup, ошибки/повторные нажатия, accessibility, тесты и соответствие Issue.

## Источники контекста

- Изменяемое состояние Android-проекта: `.agents/CONTEXT.md`.
- План Issues: `.agents/ISSUE_PLAN.md`.
- Git-процесс: `docs/GIT_WORKFLOW.md`.
- Контракт backend: `../fluent-swap-core/docs/api/websocket.md` (локально) или
  соответствующий файл в репозитории core.

После merge задачи обновить `.agents/CONTEXT.md`. `AGENTS.md` хранит постоянные
правила, а не текущий прогресс.
