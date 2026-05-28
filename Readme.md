[English](#english) | [Русский](#русский)

<a name="english"></a>
# GL-E750 MCU Display Daemon (English)

This project contains the source code for the `gl-e750-mcu` daemon. This daemon runs on your OpenWrt router and acts as a bridge between the router's operating system and the built-in OLED display, sending it information to show (battery, Wi-Fi, VPN status, etc.).

> **Developer Note**: API details, technical theory, system parameters, and advanced examples have been moved to [release_notes.md](release_notes.md).

## Step-by-Step Installation Guide

### Step 1: Getting the installation package (.apk or .ipk)
To make the program run on your router, the C source code needs to be compiled into a package.

**Option A: Download a ready-made package (Recommended)**
1. Go to the **Releases** section on the right side of this GitHub repository page.
2. Download the pre-compiled `.apk` file (e.g. `gl-e750-MCU_display_for_OWRT_25_12.apk`).

**Option B: Compile it automatically (If you modified the code)**
GitHub Actions will compile the code for you. You don't need Linux or Docker.
1. Go to the **Actions** tab in your GitHub repository.
2. Select "Build OpenWrt Package" and click **Run workflow**. 
3. Wait 2-3 minutes for the build to finish.
4. Download the generated `.apk` file from the **Artifacts** section at the bottom of the successful run.

> **Important: What is an .apk in OpenWrt?**
> Starting with version 25.12, OpenWrt transitioned from `.ipk` (opkg) packages to `.apk` (Alpine Package Keeper). **These are NOT Android applications!** An OpenWrt `.apk` file is strictly a package for Linux systems and routers.

### Step 2: Transfer the package to the router
1. Use an SCP client like WinSCP (Windows) or the `scp` command (Linux/Mac).
2. Connect to your router (usually IP `192.168.8.1`, username `root`, password is the one you set for the web admin panel).
3. Copy the downloaded `.apk` file into the `/tmp/` folder on the router.

### Step 3: Install the package
1. Connect to your router via SSH (using PuTTY or the terminal `ssh root@192.168.8.1`).
2. Run the installation command:
   ```bash
   apk add /tmp/gl-e750-MCU_display_for_OWRT_25_12.apk
   ```
   *(If you are on an older OpenWrt version that uses opkg, the command is `opkg install /tmp/filename.ipk`)*

### Step 4: Verify it works
1. **Check if the process is running:**
   ```bash
   ps | grep e750-mcu
   ```
   You should see the daemon process running.
2. **Check the system logs:**
   ```bash
   logread | grep e750-mcu
   ```
   If there are any connection issues or errors, they will be printed here.
3. **Manually test the display communication:**
   ```bash
   ubus call mcu get
   ```
   This command should return the current battery level, temperature, and other MCU data.

---

<a name="русский"></a>
<br/>
<br/>

# Демон управления дисплеем GL-E750 MCU (Русский)

Этот проект содержит исходный код демона `gl-e750-mcu`. Эта программа работает на вашем роутере OpenWrt и служит мостом между операционной системой роутера и встроенным OLED-экраном. Она постоянно отправляет на экран актуальную информацию (заряд батареи, статус Wi-Fi, VPN и т.д.).

> **Примечание для разработчиков**: Документация по API, техническая теория, системные параметры и продвинутые примеры перенесены в файл [release_notes.md](release_notes.md).

## Пошаговая инструкция установки

### Шаг 1: Получение установочного пакета (.apk или .ipk)
Чтобы программа заработала на роутере, исходный код нужно скомпилировать. 

**Вариант А: Скачать готовый релиз (Самый простой)**
1. Перейдите в раздел **Releases** (Релизы) в правой части страницы этого репозитория на GitHub.
2. Скачайте уже скомпилированный файл пакета (например, `gl-e750-MCU_display_for_OWRT_25_12.apk`).

**Вариант Б: Автоматическая компиляция (Если вы изменили код)**
Сборку можно запустить прямо на серверах GitHub без установки Linux на ваш ПК.
1. Перейдите на вкладку **Actions** в вашем репозитории на GitHub.
2. Выберите "Build OpenWrt Package" и нажмите **Run workflow**. (Сборка также запускается сама при любых изменениях (коммитах) в коде).
3. Подождите 2-3 минуты.
4. Когда сборка успешно завершится (загорится зеленая галочка), откройте её и скачайте архив из раздела **Artifacts** в самом низу. Внутри архива вы найдете готовый файл `.apk`.

> **Важно: Что такое .apk в OpenWrt?**
> Начиная с версии OpenWrt 25.12, система перешла с формата пакетов `.ipk` (opkg) на `.apk` (Alpine Package Keeper). **Это НЕ Android-приложения!** Формат файлов случайно совпадает с установщиками для смартфонов, но внутри это совершенно разные вещи. Файл `.apk` от OpenWrt предназначен исключительно для Linux-роутеров.

### Шаг 2: Загрузка файла на роутер
Вам нужно скопировать скачанный файл в память роутера.
1. Скачайте программу WinSCP (для Windows) или используйте встроенную команду `scp` в Linux/Mac.
2. Подключитесь к роутеру (обычно IP `192.168.8.1`, логин `root`, пароль от вашей админки).
3. Переместите файл `.apk` в папку `/tmp/` на вашем роутере.

### Шаг 3: Установка пакета в OpenWrt
1. Подключитесь к роутеру по SSH (например, через программу PuTTY или терминал Windows командой `ssh root@192.168.8.1`).
2. Введите ваш пароль от админки (при вводе он не отображается).
3. Выполните команду установки:
   ```bash
   apk add /tmp/gl-e750-MCU_display_for_OWRT_25_12.apk
   ```
   *(Примечание: Если вы собираете пакет для более старых версий OpenWrt, использующих opkg, команда будет `opkg install /tmp/имя_файла.ipk`)*

### Шаг 4: Как проверить, что всё работает
После установки вы можете убедиться, что программа запущена и нормально общается с экраном.

1. **Проверка процесса:**
   Введите команду:
   ```bash
   ps | grep e750-mcu
   ```
   Вы должны увидеть запущенный процесс `e750-mcu` в списке.
   
2. **Просмотр логов (журнала событий):**
   Выполните команду:
   ```bash
   logread | grep e750-mcu
   ```
   Здесь вы увидите сообщения о том, что демон успешно запустился. Если возникают какие-то ошибки связи с дисплеем, они тоже появятся здесь.

3. **Ручной запрос к дисплею:**
   Вы можете проверить живую связь с микроконтроллером дисплея через системную шину ubus. Введите:
   ```bash
   ubus call mcu get
   ```
   В ответ роутер должен вернуть текущие данные от дисплея (процент заряда батареи, температуру и статус).
