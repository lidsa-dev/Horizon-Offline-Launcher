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

### T-003 Синхронизация форка с Nightly 2026-11-05 (66f3981)
- **Статус:** in-progress
- **Описание / план:** `C:\Users\nrpems\.claude\plans\wise-singing-yao.md`: перенос дельты форка (`git diff 4aa6854 main`) на дерево апстрима `66f3981`, чтобы лаунчер читал `.hnmod` с `assets/` (Orbital Laser).
- **Чек-лист:**
  - [ ] partial clone апстрима, fetch `4aa6854` и `66f3981`
  - [ ] ветка `sync/nightly-2026-11-05`, применение дельты, разбор конфликтов
  - [ ] Cargo.lock, версия 1.3.2
  - [ ] fmt, clippy, тесты, `cargo check` клиента и сервера
  - [ ] `Bundle::read` принимает orbital.hnmod
  - [ ] коммит в main, тег `launcher@1.3.2`, запуск `game`
- **Критерии готовности:**
  - [ ] мод Orbital Laser включается в нашем лаунчере
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
