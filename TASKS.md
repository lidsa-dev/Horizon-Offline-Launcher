# TASKS

## Активная задача
### T-001 Переключение на форк lidsa-dev/Horizon-Offline-Launcher
- **Статус:** in-progress
- **Описание / план:** заменить `gerdaroot` на `lidsa-dev` в коде, скриптах и документации; сгенерировать свой ключ подписи релизов, положить публичную часть в `release-keys.txt`, приватную в секрет `RELEASE_SIGNING_KEY` репозитория; поднять версию лаунчера до 1.3.1. План: `C:\Users\nrpems\.claude\plans\wise-singing-yao.md`.
- **Чек-лист:**
  - [x] замена `gerdaroot` -> `lidsa-dev`
  - [ ] ключ подписи: keygen и публичный ключ готовы, секрет в GitHub добавляет пользователь
  - [x] версия лаунчера 1.3.1
  - [x] fmt (старые расхождения не трогал), clippy, тесты
  - [ ] `git remote set-url origin`, коммит, push (после подтверждения)
- **Критерии готовности:**
  - [ ] `grep gerdaroot` ничего не находит (кроме нужных упоминаний upstream-истории, если останутся)
  - [ ] fmt, clippy, `cargo test --release` без ошибок
- **Блокеры:**
- **Комментарии:**

### T-002 Сборки только под Windows
- **Статус:** in-progress
- **Описание / план:** `C:\Users\nrpems\.claude\plans\wise-singing-yao.md`: в `game.yml` и `launcher.yml` оставить только Windows, убрать серверный Linux-архив.
- **Чек-лист:**
  - [ ] `game.yml`: pick, matrix, release
  - [ ] `launcher.yml`: matrix
  - [ ] отмена текущих прогонов, коммит, тег, запуск `game`
- **Критерии готовности:**
  - [ ] релиз `launcher@1.3.1` с Setup.exe и pre-release `nightly` с Windows-архивом опубликованы
- **Блокеры:**
- **Комментарии:**

## Очередь

## История

## Шаблон задачи
### [ID] Название
- **Статус:** planned | in-progress | blocked | done
- **Описание / план:**
- **Чек-лист:**
  - [ ] шаг
- **Критерии готовности:**
  - [ ] критерий
- **Блокеры:**
- **Комментарии:**
