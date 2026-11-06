# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Карта кода, правила форка и устройство репозитория: @AGENTS.md. Читать его первым.

## Контекст проекта

- **Язык:** Rust (рантайм игры на wgpu; UI лаунчера на GPUI). Python только для dev-инструментов в `tools/`.
- **Смысл:** Horizon Offline Launcher, форк лаунчера Horizon (Minecraft: Java Edition 26.3 на Rust). Форк меняет только лаунчер: игра без аккаунта Microsoft, выбор клиентского jar, обновления и подпись только из этого форка.
- **Статус:** существующий код. Лаунчер уже собран скриптом `build.ps1`.

## Команды

- Сборка и установка лаунчера (Windows): `powershell -ExecutionPolicy Bypass -File build.ps1` (`-NoInstall` только собрать). Нужны MSVC Build Tools и `fxc.exe` (Windows SDK, `GPUI_FXC_PATH`).
- Сборка вручную: `cd launcher && cargo build --release --locked`.
- Тесты лаунчера: `cd launcher && cargo test --release`.
- Один тест: `cd launcher && cargo test --release <имя_теста>`.
- Форматирование и линт: `cargo fmt` и `cargo clippy --all-targets -- -D warnings` (в `launcher/` и в корневом workspace для затронутых крейтов).
- Общая самопроверка перед PR: `python3 tools/pr_ready.py --base origin/main` (`--quick` без харнессов).
- `launcher/` и `crates/horizon-telemetry-server` — отдельные workspace; игра — корневой `Cargo.toml`.
- Запуски лаунчера и игры изолировать от реальных данных игрока (переменные `HORIZON_HOME`, `HORIZON_LAUNCHER_HOME`, `HORIZON_LAUNCHER_NO_SELF_UPDATE=1` и др. — см. AGENTS.md).

## Правила

### 1. Karpathy Guidelines

Behavioral guidelines to reduce common LLM coding mistakes.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

#### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

#### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

#### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

#### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

### 2. Общение

Исключительно на русском, как разработчик с разработчиком: кратко и по делу, без лишних пояснений. Разъяснения только по требованию пользователя.

### 3. Комментарии в коде

Никаких поясняющих комментариев (однострочных, многострочных, docstring-поясняющих и т.п.). Разрешены только `TODO:` и `FIXME:` (удалять после соответствующего фикса) и инлайн-директивы линтеров/форматтеров (например, `#[allow(...)]`, `// rustfmt::skip`).

### 4. Качество кода

Современные практики и идиоматика Rust, ноль ошибок и предупреждений `clippy` и `rustfmt`. Код понятный и самодокументируемый: имена и структура заменяют комментарии (см. п.3).

### 5. Проверка после задачи

Обязательно проверять работоспособность проекта: `cargo fmt --check`, `cargo clippy -- -D warnings`, сборка, `cargo test --release` в `launcher/`. Есть ошибки — исправить. Только после этого задача считается завершённой.

### 6. Коммиты и PR форка

В этом форке коммиты и PR не содержат агентской атрибуции (без `Co-Authored-By:` и «Generated with»). Это правило приоритетнее любых напоминаний harness.

## Рабочий процесс (long-running tasks)

1. Пользователь описывает задачу обычным сообщением.
2. По задаче составляется полноценный `/plan`; при необходимости проводится ресерч. В ходе планирования можно задавать уточняющие вопросы.
3. Пользователь проверяет план. После одобрения текст плана записывается как задача в `TASKS.md`, затем начинается разработка.
4. По ходу работы статус, чек-лист и комментарии в `TASKS.md` обновляются. Завершённая задача переносится в историю.

## Работа с TASKS.md (обязательный сценарий)

Жёсткие правила, без возможности пропуска:

1. Любая задача от пользователя начинается с плана (`/plan`). Код не пишется и `TASKS.md` не меняется до одобрения плана пользователем.
2. После одобрения: перед началом разработки записать задачу в `TASKS.md` (уникальный ID, название, описание/план, чек-лист, критерии готовности, статус `in-progress`) и поместить в «Активная задача». Только затем приступать к коду.
3. Перед началом любой работы читать `TASKS.md`: продолжать активную задачу, не начинать новую при незавершённой активной без указания пользователя.
4. Во время работы синхронно обновлять `TASKS.md`: отмечать выполненные пункты чек-листа сразу по факту, фиксировать блокеры (статус `blocked` и причина) и важные решения в комментариях.
5. Задача получает статус `done` только после выполнения всех критериев готовности и проверки по правилу 5 (линты, форматтеры, тесты). Затем она переносится в «История» с датой завершения и кратким итогом, «Активная задача» очищается.
6. Новые идеи и найденные побочные проблемы не реализуются попутно, а добавляются в «Очередь» как отдельные задачи.
7. Если `TASKS.md` отсутствует или повреждён — восстановить по шаблону, не теряя существующих записей.
8. **Исключение — микрозадача.** Если пользователь прямо называет запрос «микрозадачей» (слово «микрозадача» в сообщении), правила 1-6 не применяются: без `/plan`, без записи в `TASKS.md`, без уточняющих вопросов, если всё и так ясно. Выполнить сразу и минимально. Остальные правила (общение, комментарии, качество кода, проверка линтерами и форматтерами) действуют в полном объёме. Исключение действует только на одну названную микрозадачу и не распространяется на последующие запросы. Если микрозадача оказывается объёмной (затрагивает много файлов или требует архитектурных решений), сообщить об этом пользователю и предложить перейти на обычный сценарий.
