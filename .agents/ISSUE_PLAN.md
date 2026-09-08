# GitHub Issues — Shipaton MVP

Issues идут последовательно. В работу берётся одна Issue; зависимости указаны
явно. Оценка — ориентир для начинающего разработчика с AI и review.

## Milestone 1 — Foundation

### Issue 2: Bootstrap native Android app with Compose

**Labels:** `type:feature`, `area:setup`, `priority:p0`  
**Estimate:** 1 день  
**Depends on:** —

Создать Kotlin Android app, один `app` module, Compose Material 3, min SDK 26,
package-by-feature skeleton и placeholder start screen. Добавить Gradle wrapper,
version catalog, корректный `.gitignore` и README-команды запуска.

**Acceptance criteria**

- clean clone открывается в Android Studio и собирает debug APK;
- placeholder отображается на emulator/device;
- `./gradlew test lint assembleDebug` проходит;
- в репозитории нет local paths, generated artifacts и secrets.

### Issue 3: Add CI quality gate

**Labels:** `type:chore`, `area:ci`, `priority:p0`  
**Estimate:** 0.5 дня  
**Depends on:** #2

Добавить GitHub Actions для PR/push в `main`: JDK setup, Gradle cache,
unit tests, lint и debug build.

**Acceptance criteria:** зелёный workflow на PR; broken test даёт красный run;
README содержит локальный эквивалент CI.

### Issue 4: Add app theme and navigation shell

**Labels:** `type:feature`, `area:ui`, `priority:p0`  
**Estimate:** 1 день  
**Depends on:** #2

Создать Material 3 theme и destinations `LanguageSelection`, `Searching`, `Chat`.
Пока использовать placeholder content, без WebSocket.

**Acceptance criteria:** навигация компилируется; back behavior определён;
dark/light preview; строки в resources; базовые content descriptions.

## Milestone 2 — Matchmaking

### Issue 5: Model and test WebSocket protocol

**Labels:** `type:feature`, `area:network`, `priority:p0`  
**Estimate:** 1–2 дня  
**Depends on:** #2

Описать typed envelopes/DTO для всех V1 сообщений core, request ID generator,
JSON codec и mapping server errors. UI/network library не смешивать.

**Acceptance criteria:** fixtures из core docs decode/encode; unknown type и
malformed payload обрабатываются без crash; tests покрывают byte/text limits,
`request_id` и все публичные message types.

### Issue 6: Implement lifecycle-aware WebSocket client

**Labels:** `type:feature`, `area:network`, `priority:p0`  
**Estimate:** 2 дня  
**Depends on:** #5

Реализовать connect/send/events/close поверх OkHttp WebSocket. Только одна активная
сессия; URL приходит из BuildConfig; cancellation закрывает socket.

**Acceptance criteria:** состояния connecting/open/closed/error доступны как
Flow; concurrent sends безопасны; close идемпотентен; тексты сообщений не логируются;
fake/integration tests проверяют lifecycle и protocol errors.

### Issue 7: Build language selection screen

**Labels:** `type:feature`, `area:ui`, `priority:p0`  
**Estimate:** 1–2 дня  
**Depends on:** #4

Экран выбора native/learning language из небольшого фиксированного списка.
Нельзя выбрать одну и ту же пару; CTA запускает поиск.

**Acceptance criteria:** state переживает recomposition; validation видима;
double tap не создаёт два поиска; UI test покрывает happy path и invalid pair.

### Issue 8: Connect matchmaking state machine to UI

**Labels:** `type:feature`, `area:matchmaking`, `priority:p0`  
**Estimate:** 2 дня  
**Depends on:** #6, #7

ViewModel переводит protocol events в `Idle/Connecting/Waiting/Matched/Error`,
отправляет `find_partner`/`cancel_search` и сохраняет текущий `match_id`.

**Acceptance criteria:** waiting, cancel, match and retry/error UX работают;
одноразовые нажатия идемпотентны на UI; disconnect показан пользователю;
unit tests покрывают transitions и stale request IDs.

## Milestone 3 — Chat and monetization

### Issue 9: Implement in-room text chat

**Labels:** `type:feature`, `area:chat`, `priority:p0`  
**Estimate:** 2 дня  
**Depends on:** #8

Добавить список сообщений, input и send action. Использовать текущий `match_id`;
локально различать outgoing/incoming. История только in-memory.

**Acceptance criteria:** 1..4096 UTF-8 byte validation совпадает с core;
blank message не отправляется; send error видим и не теряет draft; keyboard/back
UX работает; ViewModel и Compose tests покрывают основной flow.

### Issue 10: Add session exit and failure UX

**Labels:** `type:feature`, `area:chat`, `priority:p0`  
**Estimate:** 1 день  
**Depends on:** #9

Добавить подтверждённый выход, cleanup WebSocket и понятные экраны network/server
error. Не обещать автоматический reconnect, которого нет в V1.

**Acceptance criteria:** уход со screen/app закрывает session по выбранной policy;
повторный старт создаёт чистую session; rotation/background scenarios проверены;
нет зависших loading states.

### Issue 11: Integrate RevenueCat and one honest Pro benefit

**Labels:** `type:feature`, `area:monetization`, `priority:p0`  
**Estimate:** 2 дня  
**Depends on:** #8; owner decision on Pro benefit

Настроить RevenueCat SDK, anonymous App User ID, offering/paywall, purchase,
restore и entitlement `pro`. Базовый matchmaking/chat остаётся доступен судьям.
Возможный узкий benefit: расширенный набор тем/акцентов или поддержка проекта;
не делать фиктивную блокировку основной функции.

**Acceptance criteria:** Test Store/sandbox purchase и restore работают;
cancel/error/loading обработаны; ключ не захардкожен; privacy disclosure обновлён;
есть бесплатный путь или promo/trial для judges.

## Milestone 4 — Ship

### Issue 12: End-to-end test against deployed core

**Labels:** `type:test`, `area:e2e`, `priority:p0`  
**Estimate:** 1–2 дня  
**Depends on:** #9, staging WSS

Проверить двумя реальными Android-клиентами полный flow и оформить smoke checklist.

**Acceptance criteria:** reverse language pair matches; cancel works; chat in both
directions; invalid/network failure не crash; результаты и backend version записаны.

### Issue 13: Accessibility, privacy and release hardening

**Labels:** `type:chore`, `area:release`, `priority:p0`  
**Estimate:** 2 дня  
**Depends on:** #10, #11, #12

Release config, R8 smoke test, accessibility pass, network security config,
privacy policy/data safety answers, analytics/log audit и basic crash-free testing.

**Acceptance criteria:** signed AAB собирается вне VCS secrets; TalkBack usable;
нет cleartext production traffic; no chat text/keys in logs; release checklist зелёный.

### Issue 14: Publish to Google Play and prepare Devpost submission

**Labels:** `type:chore`, `area:release`, `priority:p0`  
**Estimate:** начать сразу, завершить до 23 сентября 2026  
**Depends on:** параллельная store preparation; final build from #13

Создать listing, icon 1024×1024, required screenshots, privacy URL, testing track,
production rollout и Shipaton assets: ≤2 min public demo video и description.

**Acceptance criteria:** первая публичная версия доступна в Google Play в США до
30 сентября 2026 23:45 PDT; RevenueCat purchase testable; Devpost submission
содержит store URL, видео, icon, screenshot, description и promo/trial при необходимости.

## После Shipaton (не P0)

- reconnect/heartbeat contract совместно с core;
- message delivery/history;
- profiles/authentication;
- voice/video spike с отдельным backend contract;
- Kotlin/Compose Multiplatform только после отдельного решения о продукте.
