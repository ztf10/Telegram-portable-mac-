# Telegram Portable для macOS

🇷🇺 Русский | [🇬🇧 English](#telegram-portable-for-macos)

Портативная версия официального Telegram Desktop для Mac. Не требует установки. Все данные (сессия, кэш, настройки) хранятся внутри папки пакета, а не в системе. Обычный Telegram, если он установлен, не затрагивается и не мешает.

Скачать можно поссылке: https://github.com/ztf10/Telegram-portable-mac-/releases/download/1.0.0/Telegram.Portable.Mac.zip

**Один пакет = один аккаунт.**

## Содержание

- [Состав пакета](#состав-пакета)
- [Системные требования](#системные-требования)
- [Быстрый старт](#быстрый-старт)
- [Первый запуск (защита macOS)](#первый-запуск-защита-macos)
- [Установка аккаунтов через tdata](#установка-аккаунтов-через-tdata)
- [Как это работает](#как-это-работает)
- [Несколько аккаунтов](#несколько-аккаунтов)
- [Перенос на другой Mac или флешку](#перенос-на-другой-mac-или-флешку)
- [Обновление](#обновление)
- [Удаление](#удаление)
- [Частые проблемы](#частые-проблемы)
- [Примечания](#примечания)

## Состав пакета

```text
Telegram Portable/
├── Telegram.app         # официальный клиент Telegram Desktop
├── Launcher.command    # лаунчер
├── Data/                # все данные аккаунта (создаётся и пополняется автоматически)
└── README.md            # эта инструкция
```

## Системные требования

| Параметр | Значение |
|---|---|
| Чип | Apple Silicon (M1, M2, M3, M4 и новее) или Intel (сборка универсальная) |
| macOS | 10.13 или новее (точный минимум зависит от версии Telegram в пакете) |
| Место на диске | ~500 МБ + место под кэш Telegram |
| Интернет | Нужен для работы Telegram |

## Быстрый старт

1. Скачайте и распакуйте архив в любую папку (рабочий стол, документы, внешний диск).
2. Запустите `Launcher.command` (первый раз см. [Первый запуск](#первый-запуск-защита-macos)).
3. Войдите в аккаунт по номеру телефона или QR-коду. Либо добавьте готовый аккаунт через [tdata](#установка-аккаунтов-через-tdata).

## Первый запуск (защита macOS)

При первом запуске macOS может заблокировать запуск. Подойдёт любой из способов:

- **Правый клик** по `Launcher.command` → **Открыть** → **Открыть**.
- **Терминал:**
  ```bash
  xattr -dr com.apple.quarantine "/путь/к/Telegram Portable"
  ```
  Путь можно не набирать: перетащите папку пакета в окно Терминала после слова `quarantine`.
- **Системные настройки** → **Конфиденциальность и безопасность** → внизу нажмите **Всё равно открыть**.

После первого успешного запуска предупреждения не повторяются.

## Установка аккаунтов через tdata

`tdata` — папка с сессией Telegram Desktop. Положив её в пакет, вы входите в аккаунт без кода.

### Установка

1. Закройте Telegram, если он запущен.
2. Положите папку `tdata` в `Data`, чтобы получился путь:
   ```text
   Telegram Portable/Data/tdata/
   ```
   Внутри `tdata` должны быть файл `key_datas` и подпапки с длинными именами вида `D877F783D5D3EF8C`.
3. Запустите `Launcher.command`.

Если `tdata` не нужен, пропустите этот раздел: Telegram запустится чистым, войдите по телефону или QR-коду. Сессия сохранится в `Data/`.

### Важно

- **Нельзя** запускать один и тот же `tdata` на двух устройствах (или в двух копиях Telegram) одновременно. Иначе Telegram может завершить сессию (ошибка `AUTH_KEY_DUPLICATED`).
- Если на аккаунте стоит локальный код-пароль, его нужно ввести при запуске.
- `tdata` даёт полный доступ к аккаунту. Храните его как пароль: не передавайте посторонним и не выкладывайте в открытый доступ (в том числе в репозиторий).
- Версия Telegram в пакете не должна быть старше версии, в которой создан `tdata`.
- Если Telegram разлогинил сессию, этот `tdata` больше не работает: войдите заново по коду или QR.

### Если вход не произошёл

- Проверьте путь: должно быть `Data/tdata/`, а не `Data/tdata/tdata/`.
- Убедитесь, что `tdata` не от более новой версии Telegram.
- При ошибке `AUTH_KEY_DUPLICATED` закройте другие копии с этим `tdata` или войдите заново.

## Как это работает

Лаунчер запускает Telegram с параметром `-workdir`, который указывает на папку `Data` внутри пакета. Приложение читает и пишет данные только туда. Запуск идёт как отдельный экземпляр (`open -n`), поэтому обычный Telegram может быть открыт одновременно.

## Несколько аккаунтов

Один пакет = один аккаунт. Для второго аккаунта скопируйте папку пакета целиком, переименуйте (например, `Telegram Portable 2`) и добавьте другой аккаунт (вход или [tdata](#установка-аккаунтов-через-tdata)).

## Перенос на другой Mac или флешку

Скопируйте папку пакета целиком. Не запускайте две копии с одной и той же сессией одновременно.

## Обновление

1. Скачайте новый пакет.
2. Скопируйте свою папку `Data` из старого пакета в новый (с заменой).
3. Старый пакет можно удалить.

Встроенное автообновление Telegram в портативном режиме может не сработать. Это нормально.

## Удаление

Закройте Telegram и удалите папку пакета. Следов в системе не остаётся. Чтобы завершить сессию на серверах Telegram: **Настройки** → **Устройства** → завершить сессию.

## Частые проблемы

| Проблема | Решение |
|---|---|
| Лаунчер не запускается, «нет прав» | `chmod +x "/путь/к/Telegram Portable/Launcher.command"` |
| Открывается обычный Telegram, а не портативный | Запускайте только через `Launcher.command`, не двойным кликом по `Telegram.app` |
| Ссылки `tg://` открываются в обычном Telegram | Ограничение macOS, на работу портативной версии не влияет |
| macOS пишет «не удалось проверить разработчика» | См. [Первый запуск](#первый-запуск-защита-macos) |

## Примечания

- `Telegram.app` внутри пакета не изменён (оригинал с [desktop.telegram.org](https://desktop.telegram.org/)). Пакет не подписан и не нотариализован автором, поэтому первый запуск требует действий из раздела «Первый запуск».
- Пакет неофициальный. Telegram — товарный знак Telegram FZ-LLC.

---

# Telegram Portable for macOS

[🇷🇺 Русский](#telegram-portable-для-macos) | 🇬🇧 English

A portable build of the official Telegram Desktop for Mac. No installation needed. All data (session, cache, settings) is stored inside the package folder, not in the system. A regular Telegram installation is not affected and does not interfere.

Download via link: https://github.com/ztf10/Telegram-portable-mac-/releases/download/1.0.0/Telegram.Portable.Mac.zip

**One package = one account.**

## Contents

- [Package contents](#package-contents)
- [System requirements](#system-requirements)
- [Quick start](#quick-start)
- [First launch (macOS protection)](#first-launch-macos-protection)
- [Adding accounts via tdata](#adding-accounts-via-tdata)
- [How it works](#how-it-works)
- [Multiple accounts](#multiple-accounts)
- [Moving to another Mac or USB drive](#moving-to-another-mac-or-usb-drive)
- [Updating](#updating)
- [Removal](#removal)
- [Troubleshooting](#troubleshooting)
- [Notes](#notes)

## Package contents

```text
Telegram Portable/
├── Telegram.app         # official Telegram Desktop client
├── Launcher.command    # launcher
├── Data/                # all account data (created and filled automatically)
└── README.md            # this file
```

## System requirements

| Item | Value |
|---|---|
| Chip | Apple Silicon (M1, M2, M3, M4 and newer) or Intel (universal build) |
| macOS | 10.13 or newer (exact minimum depends on the bundled Telegram version) |
| Disk space | ~500 MB plus room for Telegram's cache |
| Internet | Required for Telegram |

## Quick start

1. Download and unzip the archive to any folder (desktop, documents, external drive).
2. Run `Launcher.command` (see [First launch](#first-launch-macos-protection) the first time).
3. Sign in with your phone number or QR code. Or add a ready account via [tdata](#adding-accounts-via-tdata).

## First launch (macOS protection)

At first launch macOS may block it. Any one of these works:

- **Right-click** `Launcher.command` → **Open** → **Open**.
- **Terminal:**
  ```bash
  xattr -dr com.apple.quarantine "/path/to/Telegram Portable"
  ```
  You can drag the package folder into the Terminal window after the word `quarantine` instead of typing the path.
- **System Settings** → **Privacy & Security** → click **Open Anyway** at the bottom.

The warning does not repeat after the first successful launch.

## Adding accounts via tdata

`tdata` is the Telegram Desktop session folder. Placing it in the package signs you in without a code.

### Installation

1. Quit Telegram if it is running.
2. Put the `tdata` folder into `Data` so the path is:
   ```text
   Telegram Portable/Data/tdata/
   ```
   Inside `tdata` there should be a `key_datas` file and subfolders with long names like `D877F783D5D3EF8C`.
3. Run `Launcher.command`.

If you don't need `tdata`, skip this section: Telegram starts clean, sign in with your phone or QR code. The session is saved in `Data/`.

### Important

- **Never** run the same `tdata` on two devices (or in two Telegram copies) at once, or Telegram may terminate the session (`AUTH_KEY_DUPLICATED` error).
- If the account has a local passcode, you will be asked for it at launch.
- `tdata` gives full access to the account. Treat it like a password: never share it or publish it (including in a repository).
- The bundled Telegram version must not be older than the one that created the `tdata`.
- If Telegram has logged the session out, that `tdata` no longer works: sign in again via code or QR.

### If sign-in did not happen

- Check the path: it must be `Data/tdata/`, not `Data/tdata/tdata/`.
- Make sure the `tdata` is not from a newer Telegram version.
- On `AUTH_KEY_DUPLICATED`, close other copies using this `tdata` or sign in again.

## How it works

The launcher starts Telegram with the `-workdir` parameter pointing to the `Data` folder inside the package. The app reads and writes data only there. It runs as a separate instance (`open -n`), so a regular Telegram can stay open at the same time.

## Multiple accounts

One package = one account. For another account, copy the whole package folder, rename it (e.g. `Telegram Portable 2`) and add a different account (sign-in or [tdata](#adding-accounts-via-tdata)).

## Moving to another Mac or USB drive

Copy the whole package folder. Do not run two copies with the same session at once.

## Updating

1. Download the new package.
2. Copy your `Data` folder from the old package into the new one (replace).
3. Delete the old package.

Telegram's built-in auto-update may not work in portable mode. This is expected.

## Removal

Quit Telegram and delete the package folder. Nothing is left in the system. To end the session on Telegram servers: **Settings** → **Devices** → terminate session.

## Troubleshooting

| Problem | Solution |
|---|---|
| Launcher won't run, permission denied | `chmod +x "/path/to/Telegram Portable/Launcher.command"` |
| Regular Telegram opens instead of the portable one | Always start via `Launcher.command`, not by double-clicking `Telegram.app` |
| `tg://` links open in the regular Telegram | macOS limitation, does not affect the portable version |
| macOS says "cannot verify developer" | See [First launch](#first-launch-macos-protection) |

## Notes

- `Telegram.app` inside is unmodified (original from [desktop.telegram.org](https://desktop.telegram.org/)). The package is not signed or notarized by its author, so the first launch needs the steps in "First launch".
- This package is unofficial. Telegram is a trademark of Telegram FZ-LLC.
