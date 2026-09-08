# Git для разработки Fluent Swap Android

Эта инструкция рассчитана на работу над одной GitHub Issue за раз.

## Один раз на компьютере

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git clone git@github.com:wizardVadim/fluent-swap-android.git
cd fluent-swap-android
```

Проверить, что всё настроено:

```bash
git remote -v
git status
```

## Начать Issue

Никогда не писать feature прямо в `main`.

```bash
git switch main
git pull --ff-only origin main
git switch -c issue-1/android-skeleton
```

Имя ветки: `issue-N/короткое-описание`, только латиницей. Одновременно вести
одну задачу. Если в рабочей папке уже есть чужие изменения — не удалять их,
сначала спросить владельца.

## Во время работы

```bash
git status
git diff
git add path/to/changed-file
git diff --staged
git commit -m "feat(setup): create Android project skeleton"
```

Не использовать `git add .`, пока не просмотрены все файлы. Не коммитить
`.idea` user settings, `.gradle`, `build`, `local.properties`, ключи и токены.

Рекомендуемые префиксы commit: `feat`, `fix`, `test`, `docs`, `build`, `ci`,
`refactor`. Commit должен описывать одно логическое изменение.

## Перед push

1. Прочитать Issue ещё раз.
2. Запустить указанные в ней проверки.
3. Просмотреть `git diff main...HEAD`.
4. Убедиться через `git status`, что секреты и мусор не попали в commit.

```bash
git push -u origin issue-1/android-skeleton
```

## Pull Request

На GitHub нажать **Compare & pull request** и заполнить:

```markdown
Closes #1

## Что сделано
- ...

## Как проверено
- [ ] `./gradlew test`
- [ ] `./gradlew lint`
- [ ] `./gradlew assembleDebug`
- [ ] ручной сценарий: ...

## Скриншоты
Для UI приложить before/after или новый экран.

## Известные ограничения
- ...
```

Не нажимать merge до code review. После замечаний исправить код в той же ветке,
сделать новый commit и `git push`; PR обновится автоматически. Не закрывать
review thread, пока исправление не отправлено.

## После merge

```bash
git switch main
git pull --ff-only origin main
git branch -d issue-1/android-skeleton
```

Удалить remote branch можно кнопкой GitHub. Если возник конфликт или Git просит
force push — остановиться и попросить помощь. Не применять `--force`, reset,
rebase или удаление файлов, не понимая последствий.

## Быстрое восстановление

- Не сохранённые изменения: сначала `git diff`, затем спросить владельца.
- Ошибка в последнем commit, который ещё не pushed: попросить review команды.
- Случайно committed secret: немедленно сообщить владельцу; удалить commit
  недостаточно, ключ нужно отозвать.
