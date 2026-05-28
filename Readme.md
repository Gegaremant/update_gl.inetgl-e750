[English](#english) | [Русский](#русский)

<a name="english"></a>
# English

**Since the microcontroller only recognizes the string type in the JSON format, the following parameters, even if the parameter type is INT, will be converted to a string and sent**

### WIFI Related

| Parameter name | Type | Necessity | Default | Description                                            | Possible value                                |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|ssid|string|yes|" "|2G WiFi SSID|A string of up to 32 characters|
|up|string|yes|"0"|Indicates whether 2G WIFI is enabled. If it is not enabled, the LCD will not display the 2G WIFI page.|0 or 1|
|key|string|no|" "|2G WiFi password, if it is empty, it means no encryption, LCD shows OPEN|A string of up to 64 characters|
|ssid_5g|string|yes|" "|5G WiFi SSID|A string of up to 32 characters|
|up_5g|string|yes|"0"|Indicates whether 5G WIFI is enabled. If it is not enabled, the LCD will not display the 5G WIFI page.|0 or 1|
|key_5g|string|no|" "|5G WiFi password, if it is empty, it means no encryption, LCD shows OPEN|A string of up to 64 characters|
|hide_psk|string|no|"0"|Whether to hide the wifi password on the LCD|0 or 1|

### Modem related

| Parameter name |  Type  | Necessity | Default  | Description                                                  | Possible value                                               |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|SIM|string|no|"NO_SIM"| SIM card status, there is no SIM parameter normally, if there is SIM parameter, other parameters will not be transferred | NO_SIM (No SIM card detected), PIN_SIM (PIN code required), NO_REG (No service) |
|carrier|string|no|"0"|Carrier name|A string of up to 16 characters|
|sms|string|no|"0"|/Number of text messages. If this parameter is greater than 0, the LCD displays the text message icon.|Numbers greater than 0|
|signal|string|no|"0"|Signal strength|0~4|
|modem_mode|string|no|" "|Network mode|2G，3G, 4G, 4G+|
|modem_up|string|no|"0"|Whether the modem data is enabled|0 or 1|

### Network Related

| Parameter name |  Type  | Necessity | Default | Description                                                  | Possible value                                               |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|work_mode|string|yes|" "|Router network mode|Router,AP,WDS,Extender|
|lan_ip|string|yes|" "| Router gateway address, or the IP address of the router under bridge mode |Legal IP address|
|method_nw|string|yes|" "|Router's current Internet access| cable,repeater,modem,tethering, if there is extra information, use "\|" to separate them. For example, repeater&#124;GL-AR750S-081 |

### VPN related

| Parameter name |  Type  | Necessity | Default | Description            | Possible value                   |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|vpn_type|string|yes|" "|VPN protocol|openvpn,wireguard|
|vpn_status|string|yes|" "|VPN connection status|connected，connecting，off|
|vpn_server|string|yes|" "|VPN configuration name|A string of up to 128 characters|

### Client related

| Parameter name |  Type  | Necessity | Default | Description       | Possible value                     |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|clients|string|yes|"0"|Number of clients|Numbers greater than or equal to 0|

### customization related

| Parameter name |  Type  | Necessity | Default | Description                                                  | Possible value                  |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
| display_mask | string | no | "1f" | This value indicates whether the 1-5 screen is displayed. You need to convert this value to the corresponding binary when setting. For example, 0x03 converted to binary is 00011, which means that only the first screen and the second screen are displayed; the default 1f, that is, 11111, displays 5 screen contents |0x0-0x1f|
| custom_en | string | no | "0" | This value indicates whether the user is using a custom page, 0 means not use, 1 means use |0 or 1|
| content | string| no |" "| Display content | A string of up to 64 characters |
|msg|string|no|" "|Display content on the screen for 20 seconds|A string of up to 64 characters|

### system related

| Parameter name |  Type  | Necessity | Default | Description                                   | Possible value                                               |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|button|string|no|"0"|The time the reset button was pressed|Numbers greater than or equal to 0|
|system|string|no|"boot"|Show system status on screen|reboot (reboot), reft (restore factory settings), adding (system upgrade), gouboot (enter uboot mode), boot (boot), Calibrate stage (calibration stage), Flash stage (waiting to upgrade standard firmware stage), Test stage 1 (Test Phase 1), Test stage 2 （Test Phase 2）|
|disk|string|no|"0"| Is there a disk                               |0 or 1|
|tor|string|no|"0"|Is it the Tor firmware|0 or 1|
|debug|string|no|"0"|Whether to print debug information in logread|0 or 1|

### MCU status related

| Parameter name |  Type  | Necessity | Default | Description                                                  | Possible value |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|mcu_status|string|no|NO|Get the status of the microcomputer, send the command within 1 second, the microcomputer will return the relevant data through the serial port, which are the percentage of power, the temperature of the coulometer, the state of charge, the number of battery charging cycles, and the battery voltage|  |

### screen test

| Parameter name |  Type  | Necessity | Default | Description                    | Possible value                            |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|lcd_test|string|no|NO|Test the screen for bad pixels|1 (light up all pixels) or 0 (off screen)|

### coulometer parameter query

| Parameter name |  Type  | Necessity | Default | Description                                            | Possible value |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|QEN|string|no|NO| Check if the coulometer algorithm is enabled ||
|chemid|string|no|NO| Check coulometer file version                          ||
|high_temp|string|no|72| Set high temperature shutdown value, don't set too low ||

### MCU firmware version query

| Parameter name |  Type  | Necessity | Default | Description                | Possible value |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|version|string|no|NO| Check MCU firmware version ||

### example1: How to control the OLED display
**Use the echo command directly to send data in json format to the system serial port. This example contains basic WIFI information, SIM card information, VPN status, client status, time, etc. The MCU_status parameter is included in the example, which indicates that the microcontroller is required to return status**

```
echo '{ "ssid_5g": "GL-E750-719", "up_5g": "1", "key_5g": "goodlife", "ssid": "GL-E750-719", "up": "1", "key": "goodlife", "SIM": "NO_SIM", "work_mode": "Router", "lan_ip": "192.168.82.1",  "vpn_status": "off", "clients": "1", "clock": "02:30", "mcu_status": "1" }' >/dev/ttyS0
```
**After the command is executed, the serial port will return the status within 1 second. The return value is as follows. Each parameter is separated by a comma, where {OK} indicates successful execution, 99 indicates that 99% of the current power is left, and 42.4 indicates that the current coulometer Temperature, 1 means charging, 2 means the battery has two charge and discharge cycles****

```
{OK},99,42.4,1,2
```
### example2: How to check the information
If you want to check the MCU firmware version, please following these steps.
1. Open the first terminal useing SSH protocol

2. In the terminal, execute the ***uci set mcu.global.debug=1 && uci commit*** command to open the debug mode

3. Execute the ***/etc/init.d/e750_mcu restart*** command to restart the mcu process

4. Execute the ***logread -f*** command to monitor the system log

5. Open the second terminal useing SSH protocol, and then execute the ***echo {\"version\": \"1\"} >  /tmp/mcu_message  &&  killall -17 e750-mcu*** command

6. In the first terminal, you will see the **e750-mcu recived:xxx** message

### Compile .ipk
### Compile the installation package (.ipk or .apk format)

**Why is this needed?**
To make the program run on your router, the source code (written in C) needs to be compiled (translated into machine code) by a specific compiler that understands your router's processor architecture. Setting up this environment manually is complicated, so we use the pre-built "OpenWrt SDK" — a toolset provided by OpenWrt developers. The result is an installation package that can be easily transferred to the router and installed with a single command.

> **Important: What is an .apk in OpenWrt?**
> Starting with version 25.12, OpenWrt transitioned from `.ipk` (opkg) package formats to `.apk` (Alpine Package Keeper). **These are NOT Android applications!** The file extension happens to be identical to Android smartphone installers, but internally they are completely different. An OpenWrt `.apk` file is strictly for Linux systems and routers.

#### 1. Automatic Compilation via GitHub Actions (Recommended)
This is the easiest method: you don't need Linux, WSL, or Docker. GitHub's free servers will do all the heavy lifting.

1. **Start the build:** Go to the **Actions** tab in your GitHub repository, select "Build OpenWrt MCU Package" and click **Run workflow**. (Or the build will trigger automatically when you make changes to the code).
2. **Wait:** The GitHub server will download the OpenWrt SDK, insert our code, and compile it. This takes about 2-3 minutes.
3. **Download the ready package:** When the build finishes successfully (a green checkmark appears), open it and download the archive from the **Artifacts** section at the bottom. Inside, you will find the ready `gl-e750-MCU_display_for_OWRT_25_12.apk` file.
4. Transfer this file to your router (e.g., via SCP or WinSCP) and install it with the command: `apk add /path/to/gl-e750-MCU_display_for_OWRT_25_12.apk`

#### 2. Compile on the glinet openwrt source (Linux)
	$cd openwrt_root          #go to your openwrt source root
	$./scripts/feeds update -f -a
	$./scripts/feeds install -f -a
	$make menuconfig
	  GL.iNet packages choice shortcut  ---> 
	    Select MCU  --->
	      <*> Support GL_E750_MCU
	$make package/feeds/gli_pub/gl-e750-mcu/{clean,compile} V=s
	$ls bin/packages/mips_24kc/gli_pub/gl-e750-mcu_2020-06-08-f8c77bdb-1_mips_24kc.ipk
  
#### 3. Compile on the other openwrt source (Linux, e.g. OpenWrt 25.12)
	$cd openwrt_root          #go to your openwrt source root (e.g. OpenWrt 25.12 SDK)
	$cd package
	$git clone https://github.com/gl-inet/GL-E750-MCU-instruction.git
	$cd ..
	$make menuconfig
	  gl-inet  ---> 
	    <*> gl-e750-mcu........................................ GL iNet mcu interface
	$make package/GL-E750-MCU-instruction/{clean,compile} V=s
	$ls bin/packages/mips_24kc/base/gl-e750-mcu_*-1_mips_24kc.apk

### How to upgrade the mcu firmware
1. Get the mcu firmware from GL sales or compile the firmware by youself use the source code

2. Use the TFTP or SCP protocol to upload the MCU firmware to a directory on E750 file system. For example, my firmware name is **e750-mcu-V1.0.5.bin** and I chose the directory is **/tmp**, so the firmware path is **/tmp/e750-mcu-V1.0.5.bin**

3. Open the first terminal useing SSH protocol, execute the ***ubus  call  service delete '{"name":"e750_mcu"}'*** command to stop the mcu process, don't care the rerurn message

4. Execute the ***mcu_update /tmp/e750-mcu-V1.0.5.bin*** command to upgrade the MCU firmware

<a name="русский"></a>
<br/>
<br/>

# Русский

**Поскольку микроконтроллер (MCU) распознает только строковый тип данных в формате JSON, следующие параметры будут конвертироваться в строки и отправляться именно как строки, даже если их тип указан как INT**

### Настройки WIFI

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|ssid|string|да|" "|SSID для 2G WiFi|Строка до 32 символов|
|up|string|да|"0"|Показывает включен ли 2G WIFI. Если выключен, на дисплее не будет отображаться страница 2G WIFI.|0 или 1|
|key|string|нет|" "|Пароль 2G WiFi. Если пусто, это означает отсутствие шифрования, и на дисплее будет OPEN|Строка до 64 символов|
|ssid_5g|string|да|" "|SSID для 5G WiFi|Строка до 32 символов|
|up_5g|string|да|"0"|Показывает включен ли 5G WIFI. Если выключен, на дисплее не будет отображаться страница 5G WIFI.|0 или 1|
|key_5g|string|нет|" "|Пароль 5G WiFi. Если пусто, это означает отсутствие шифрования, и на дисплее будет OPEN|Строка до 64 символов|
|hide_psk|string|нет|"0"|Скрывать ли пароль от wifi на дисплее|0 или 1|

### Настройки Модема

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|SIM|string|нет|"NO_SIM"| Статус SIM-карты. Обычно параметра SIM нет, но если он есть, остальные параметры не передаются | NO_SIM (Сим карта не найдена), PIN_SIM (Требуется PIN код), NO_REG (Нет обслуживания) |
|carrier|string|нет|"0"|Название оператора связи|Строка до 16 символов|
|sms|string|нет|"0"|Количество текстовых сообщений (SMS). Если значение больше 0, на дисплее появится значок сообщения.|Число больше 0|
|signal|string|нет|"0"|Уровень сигнала|0~4|
|modem_mode|string|нет|" "|Сетевой режим|2G, 3G, 4G, 4G+|
|modem_up|string|нет|"0"|Включена ли передача данных через модем|0 или 1|

### Настройки Сети

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|work_mode|string|да|" "|Режим работы роутера|Router, AP, WDS, Extender|
|lan_ip|string|да|" "| Адрес шлюза роутера или IP-адрес роутера в режиме моста |Валидный IP-адрес|
|method_nw|string|да|" "|Текущий способ подключения к интернету| cable, repeater, modem, tethering. Дополнительную информацию можно отделить знаком "\|". Например: repeater&#124;GL-AR750S-081 |

### Настройки VPN

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|vpn_type|string|да|" "|Протокол VPN|openvpn, wireguard|
|vpn_status|string|да|" "|Статус подключения VPN|connected, connecting, off|
|vpn_server|string|да|" "|Имя конфигурации VPN|Строка до 128 символов|

### Настройки Клиентов

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|clients|string|да|"0"|Количество подключенных клиентов|Число больше или равное 0|

### Кастомизация

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
| display_mask | string | нет | "1f" | Значение указывает, отображать ли экраны с 1 по 5. При настройке это значение конвертируется в бинарный код. Например, 0x03 в бинарном коде это 00011, что означает отображение только первого и второго экрана; по умолчанию 1f (т.е. 11111) — отображаются все 5 экранов. |0x0-0x1f|
| custom_en | string | нет | "0" | Указывает, использует ли пользователь кастомную страницу (0 — нет, 1 — да) |0 или 1|
| content | string| нет |" "| Отображаемый контент | Строка до 64 символов |
|msg|string|нет|" "|Отображать контент на экране в течение 20 секунд|Строка до 64 символов|

### Системные настройки

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|button|string|нет|"0"|Время удержания кнопки reset (сброса)|Число больше или равное 0|
|system|string|нет|"boot"|Отображать статус системы на экране|reboot (перезагрузка), reft (восстановление заводских настроек), adding (системное обновление), gouboot (вход в uboot режим), boot (загрузка), Calibrate stage (стадия калибровки), Flash stage (стадия ожидания прошивки), Test stage 1, Test stage 2|
|disk|string|нет|"0"| Подключен ли накопитель |0 или 1|
|tor|string|нет|"0"|Является ли прошивкой Tor|0 или 1|
|debug|string|нет|"0"|Выводить ли отладочную информацию в logread|0 или 1|

### Статус MCU

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|mcu_status|string|нет|NO|Запрашивает статус микроконтроллера. Отправьте команду, и в течение 1 секунды микроконтроллер вернёт соответствующие данные через последовательный порт: процент заряда, температуру контроллера батареи, состояние зарядки, количество циклов заряда-разряда и напряжение батареи.| |

### Тест экрана

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|lcd_test|string|нет|NO|Тестирует экран на битые пиксели|1 (включить все пиксели) или 0 (выключить экран)|

### Запрос параметров кулонометра (батареи)

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|QEN|string|нет|NO| Проверка включен ли алгоритм кулонометра ||
|chemid|string|нет|NO| Проверка версии файла кулонометра ||
|high_temp|string|нет|72| Установка значения температуры для отключения устройства (не задавайте слишком низкое) ||

### Запрос версии прошивки MCU

| Имя параметра | Тип | Обязательно | По умолчанию | Описание | Возможные значения |
| :----------: | :-----: | :----------------: | ----------------- | ----------------- | ----------------- |
|version|string|нет|NO| Запрос версии прошивки MCU ||

### Пример 1: Управление OLED дисплеем
**Используйте команду echo напрямую, чтобы отправить данные в формате JSON в системный последовательный порт. Этот пример содержит основную информацию: WIFI, SIM-карта, статус VPN, количество клиентов, время и т.д. В примере передан параметр mcu_status, который запрашивает у микроконтроллера вернуть статус обратно.**

```
echo '{ "ssid_5g": "GL-E750-719", "up_5g": "1", "key_5g": "goodlife", "ssid": "GL-E750-719", "up": "1", "key": "goodlife", "SIM": "NO_SIM", "work_mode": "Router", "lan_ip": "192.168.82.1",  "vpn_status": "off", "clients": "1", "clock": "02:30", "mcu_status": "1" }' >/dev/ttyS0
```
**После выполнения команды последовательный порт вернёт статус в течение 1 секунды. Пример ответа приведён ниже. Каждый параметр разделён запятой, где {OK} указывает на успешное выполнение команды, 99 означает 99% оставшегося заряда, 42.4 — текущая температура, 1 означает, что идёт зарядка, а 2 означает, что у батареи было два полных цикла заряда и разряда.**

```
{OK},99,42.4,1,2
```
### Пример 2: Как проверить информацию
Если вы хотите проверить версию прошивки MCU, выполните следующие шаги:
1. Откройте первый терминал по протоколу SSH.

2. В терминале выполните команду ***uci set mcu.global.debug=1 && uci commit***, чтобы включить режим отладки.

3. Выполните команду ***/etc/init.d/e750_mcu restart*** для перезапуска процесса MCU.

4. Выполните команду ***logread -f***, чтобы отслеживать системный журнал (лог).

5. Откройте второй терминал по SSH и выполните команду: ***echo {\"version\": \"1\"} >  /tmp/mcu_message  &&  killall -17 e750-mcu***

6. В первом терминале вы увидите сообщение вида **e750-mcu recived:xxx**.

### Компиляция пакета для установки (формат .ipk или .apk)

**Зачем это нужно?**
Чтобы программа заработала на вашем роутере, исходный код (написанный на языке Си) нужно скомпилировать (перевести в машинный код) специальным компилятором, который понимает архитектуру процессора вашего роутера. Так как настроить такую среду на обычном компьютере сложно, мы используем готовый "OpenWrt SDK" — набор инструментов от разработчиков OpenWrt. В результате мы получаем установочный пакет, который можно легко закинуть на роутер и установить одной командой.

> **Важно: Что такое .apk в OpenWrt?**
> Начиная с версии OpenWrt 25.12, система перешла с формата пакетов `.ipk` (opkg) на `.apk` (Alpine Package Keeper). **Это НЕ Android-приложения!** Формат файлов случайно совпадает с установочными файлами для смартфонов Android, но внутри это совершенно разные вещи. Файл `.apk` от OpenWrt предназначен исключительно для Linux-систем и роутеров.

#### 1. Автоматическая компиляция через GitHub Actions (Рекомендуется)
Это самый простой способ: вам не нужен Linux, WSL или Docker. Всю тяжелую работу по сборке сделают бесплатные серверы GitHub.

1. **Запустите сборку:** Перейдите на вкладку **Actions** в вашем репозитории на GitHub, выберите "Build OpenWrt MCU Package" и нажмите **Run workflow**. (Либо сборка запустится автоматически при внесении изменений в код).
2. **Подождите:** Сервер GitHub скачает OpenWrt SDK, поместит туда наш код и скомпилирует его. Это занимает около 2-3 минут.
3. **Скачайте готовый пакет:** Когда сборка успешно завершится (загорится зеленая галочка), откройте её и в самом низу в разделе **Artifacts** скачайте архив. Внутри архива вы найдете готовый файл `gl-e750-MCU_display_for_OWRT_25_12.apk`.
4. Перенесите этот файл на роутер (например, через SCP или WinSCP) и установите командой: `apk add /путь/к/файлу/gl-e750-MCU_display_for_OWRT_25_12.apk`

#### 2. Компиляция на исходниках OpenWrt от GL.iNet (Для Linux)
	$cd openwrt_root          # перейдите в корень исходников openwrt
	$./scripts/feeds update -f -a
	$./scripts/feeds install -f -a
	$make menuconfig
	  GL.iNet packages choice shortcut  ---> 
	    Select MCU  --->
	      <*> Support GL_E750_MCU
	$make package/feeds/gli_pub/gl-e750-mcu/{clean,compile} V=s
	$ls bin/packages/mips_24kc/gli_pub/gl-e750-mcu_2020-06-08-f8c77bdb-1_mips_24kc.ipk
  
#### 3. Компиляция на других исходниках OpenWrt (Для Linux, например OpenWrt 25.12)
	$cd openwrt_root          # перейдите в корень исходников (например, OpenWrt 25.12 SDK)
	$cd package
	$git clone https://github.com/gl-inet/GL-E750-MCU-instruction.git
	$cd ..
	$make menuconfig
	  gl-inet  ---> 
	    <*> gl-e750-mcu........................................ GL iNet mcu interface
	$make package/GL-E750-MCU-instruction/{clean,compile} V=s
	$ls bin/packages/mips_24kc/base/gl-e750-mcu_*-1_mips_24kc.apk

### Как обновить прошивку MCU
1. Получите файл прошивки MCU в отделе продаж GL или скомпилируйте прошивку самостоятельно из исходного кода.

2. Используйте протокол TFTP или SCP для загрузки прошивки MCU в какую-либо папку на файловой системе роутера E750. Например, имя моей прошивки **e750-mcu-V1.0.5.bin** и я выбрал директорию **/tmp**, следовательно, полный путь до прошивки: **/tmp/e750-mcu-V1.0.5.bin**

3. Откройте первый терминал через SSH, выполните команду ***ubus call service delete '{"name":"e750_mcu"}'***, чтобы остановить процесс MCU (можно не обращать внимания на возвращаемое сообщение).

4. Выполните команду ***mcu_update /tmp/e750-mcu-V1.0.5.bin*** для запуска процесса обновления прошивки MCU.
