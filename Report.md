# Операционные системы UNIX/Linux (Базовый).


## Part 1. Установка ОС
**1. Установлен **Ubuntu 22.04 Server LTS** без графического интерфейса. (Использую программу для виртуализации - VirtualBox).Команда для просмотра`cat /etc/issue`**

![linux_wer](skreenshots/Part1/1.png)

## Part 2. Создание пользователя

**1. Создаю нового пользователя командой ``sudo adduser tmp_user``**

![linux_wer](skreenshots/Part2/2_1.png)

**2. Просматриваю нового пользователя ``cat /etc/passwd``**

![linux_wer](skreenshots/Part2/2_2.png)

## Part 3. Настройка сети ОС

**1. Задаю название машины вида user-1  с помощью команды `sudo hostnamectl set-hostname user-1` и проверяю, что название изменилось`hostnamectl`**

![linux_wer](skreenshots/Part3/3_1.png)

**2. Устанавливаю временную зону командой  `sudo timedatectl set-timezone Europe/Moscow`** 

![linux_wer](skreenshots/Part3/3_2.png)

**3.  Вывожу названия сетевых интерфейсов с помощью консольной команды `` ifconfig``**

![check_ifconfig](skreenshots/Part3/3_3.png)

- lo (loopback device) – виртуальный интерфейс, присутствующий по умолчанию в любом Linux. Он используется для отладки сетевых программ и запуска серверных приложений на локальной машине.

**4. Сбрасываю свой старый ip адрес командой `sudo dhclient -r enp0s3`**

![reset ip](skreenshots/Part3/3_4.png)

**5. И получаю новый командой `sudo dhclient -v enp0s3`**

![getting new ip](skreenshots/Part3/3_5.png)

**6. Проверяю все это командой `ifconfig`**

![checking ip](skreenshots/Part3/3_6.png)

- Dynamic Host Configuration Protocol (DHCP) — автоматический предоставляет IP адреса и прочие настройки сети (маску сети, шлюз и т.п) компьютерам и различным устройствам в сети. Клиент настроенный на получение адреса по протоколу DHCP посылает запрос к серверу, и тот в свою очередь предоставляет свободный IP адрес клиенту во временное пользование.

**7. Определяю и вывожу на экран внешний ip-адрес шлюза (ip) командой  `wget -O - -q icanhazip.com` либо curl ident.me (предварительно установивив  curl ) и внутренний IP-адрес шлюза, он же ip-адрес по умолчанию (gw) командой `route -n`**

![ip_adresses](skreenshots/Part3/3_7.png)


**8. Задаю статичные настройки ip, gw, dns, для этого открываю файл /00-installer-config.yaml для редактирования командой `sudo vim /etc/netplan/00-installer-config.yaml`, отключаю получение адресов от DHCP и присваиваю свой статический адрес**

- файл 00-installer-config.yaml - было: 

![yam_file](skreenshots/Part3/3_8.png)

- файл 00-installer-config.yaml - стало: 

![yam_file](skreenshots/Part3/3_9.png)

**9. `sudo netplan apply` - для применения конфигураций**

**10. `sudo netplan try` - для применения изменений**

**11. `ifconfig` - проверяем получили ли мы статистические настройки**

![check_static_adresses](skreenshots/Part3/3_12.png)

**12. `ping -c 10 ya.ru` - и убеждаемся что все работает и нет потери пакетов**

![checking ping](skreenshots/Part3/3_13.png)

## Part 4. Обновление ОС

**1. `sudo apt update` - обновляю Ubuntu**

![updating ubuntu](skreenshots/Part3/4_1.png)

**2. `sudo apt dist-upgrade` - обновляю версию пакетов**

![updating ubuntu](skreenshots/Part3/4_2.png)

**3. И `sudo apt update` проверю на обновление**

![checking updates](skreenshots/Part3/4_3.png)


## Part 5. Использование команды **sudo**

**1. Команда sudo - позволяет строго определенным пользователям выполнять указанные программы с административными привилегиями без ввода пароля суперпользователя root**

**2. `sudo touch /etc/sudoers.d/new` - создаю файл для нового пользователя**

**3. `sudo vim /etc/sudoers.d/new` - открываю в вим и записываю такую строку `new ALL=(ALL:ALL) ALL`**

![new ALL](skreenshots/Part5/5_3.png)

**4. Потом перехожу в нового пользователя командой su new_user**

**5. `sudo vim /etc/hostname` - меняю название машины на  new_user**

**6. Перезапускаю виртуальную машину и вижу, что все изменилось как нам и надо было.**

 ![reboot system](skreenshots/Part5/5_7.png)


## Part 6. Установка и настройка службы времени

**1. `sudo apt install -y ntp` - Устанавливаю NTP протокол для синхронизации времени**

![installing ntp](skreenshots/Part6/6_1.png)

**2. `ntpq -p` - Проверяю, что `ntp` подключён к серверам времен**

![connecting to time server](skreenshots/Part6/6_2.png)

**5. `sudo systemctl stop ntp` - остановила нашу команду**

**6. `sudo ntpd -gq` - и принудительно синхронизирую все это**

![synchron ntp&d](skreenshots/Part6/6_4.png)

**7. `sudo systemctl start ntp` - снова все запускаю**

![restart and checking](skreenshots/Part6/6_5.png)

**8. `apt install systemd-timesyncd` - устанавливаю утилиту запуска синхронизации**

![restart and checking](skreenshots/Part6/6_6.png)

**9. `timedatectl set-ntp true` - этой командой запускаю синхронизацию**

![launch](skreenshots/Part6/6_7.png)

**10. `timedatectl show` - и в конце проверяю, что все работает и синхронизируется**

![show time](skreenshots/Part6/6_8.png)


## Part 7. Установка и использование текстовых редакторов 

###VIM-------------------------------------------

**1. `sudo vim test_vim.txt` - создаю `.txt` файл и открываю его для записи**

![file creation](skreenshots/Part7/7_1.png)

**2. Нажимаю `i` чтобы войти в режим редактирования**

**3. Пишу свой ник `Styxdune`**

![write file](skreenshots/Part7/7_2.png)

**4. Нажимаю `:wq` чтобы оно сохранилось и вышло. Затем проверяю сохранилась ли надпись с помощью команды `cat test_vim.txt`**

![check file](skreenshots/Part7/7_3.png)

**5. Снова открываю файл как в пункте 1 и вхожу в режим редактирования как в пункте 2**

**6. Меняю `Styxdune` на `21 school 21`**

![change file](skreenshots/Part7/7_4.png)

**8. Нажимаю `escape`**

**9.  Нажимаю `:q!` и выхожу без сохранения**

**10. Проверяю командой `cat test_vim.txt` что изменения не были сохранены**

![check file](skreenshots/Part7/7_6.png)

**11. Для поиска и замены нам понадобится открыть файл**

**12. Нажать `escape` и ввести `:s/искомое слово/слово на которую мы заменим/g` - этот флаг отвечает за то чтобы во всем текстовом файле искомое слово заменили. В моем файле я поменяла "dune" на  "rock".**

![search in file](skreenshots/Part7/7_7.png)

**13. Сохраняем и выходим**

![check file](skreenshots/Part7/7_8.png)


###JOE-------------------------------------------

**1. `sudo apt install joe` - скачиваю `Joe` текстовый редактор.**

![instaling Joe](skreenshots/Part7/7_9.png)

**2. `sudo joe test_joe.txt` - создаю .txt файл и открываю его для записи**

![ulling file](skreenshots/Part7/7_11.png)

**Записала все что мне нужно**

**3. `control+k` а потом нажимаю `Q` и он спросит о сохранении изменений.**

**я нажимаю кнопку `y` и он все сохранит и выйдет**

**Тут же проверяю сохранилась ли запись прочитав ее с помощью `cat`**

![check file](skreenshots/Part7/7_12.png)

**4. Снова открывю файл и меняю содержимое на `21 School 21`, но уже без сохраниния -  буква y**

![change file without saving](skreenshots/Part7/7_13.png)

**Проверяю, что файл, остался без изменений**

![checking file](skreenshots/Part7/7_14.png)

**5. Для поиска я нажимаю `control+k` и потом `f`**

**6. Далее пишу искомое слово и `enter` И нам показывают еще несколько команд, из них нам понадобиться только `R`**

**7.Пишу слово на которое мы заменим, в моем случае это `Joe`**

![last vers](skreenshots/Part7/7_15.png)

**8. Сохраняю и выхожу**

**Проверяю, что текст изменился**
![checking file](skreenshots/Part7/7_16.png)

###NANO------------------------------------------------
**1. `sudo nano test_nano.txt` - создаю `.txt` файл и открываю его для записи**

![writing file](skreenshots/Part7/7_18.png)

**2. Записываю. Нажимаю команду `control+x`**

**3. Меня спрашивают сохранить изменения? Я пишу `y` и нажимаю кнопку `enter`**

**И он сохраняет и выходит.**

**Проверяю сохранилась ли запись**

![checking file](skreenshots/Part7/7_19.png)

**4.Снова открываю файл и переписываю его на 21 Shooll 21**

![change file](skreenshots/Part7/7_20.png)

**затем нажимаю кнопку `n` и `enter` чтобы сохранение не произошло**

**Сразу проверяю содержимое файла**

![checking file](skreenshots/Part7/7_21.png)

**5. Для того чтобы он произвел поиск и замену текста я нажимаю `control+\`**

**6. Внизу (как на скриншоте) появится поле для слова, которое мы ищем. В моем случае это `dune`**

**7. И нажимаю `enter`**

![swap text in file](skreenshots/Part7/7_22.png)

**8. У нас появилось еще одно меню. В этот раз в то же место мы записываем слово, на которое хотим поменять, в моем случае это `Nano`**

![swap text in file](skreenshots/Part7/7_23.png)

**9. Спрашивает заменить? Мы говорим да и жмем кнопку `y`**

**10. Сохраняем и выходим, как в пунктах выше**
**И смотрим наш конечный файл**

![checking file](skreenshots/Part7/7_24.png)


## Part 8. Установка и базовая настройка сервиса **SSHD**

**1. Устананавливаю службу SSHd с помощью команды: `sudo apt-get install openssh-server`**

![install openssh-server](skreenshots/Part8/8_1.png)

**2. Добавляю автостарт службы при загрузке системы.**

**Для включения автостарта службы воспользуемся командой: `sudo systemctl enable ssh`**

![active autostart](skreenshots/Part8/8_2.png)

**3.Перенастраиваю службу SSHd на порт 2022.**
**Для этого открываю файл конфигурации с помощью команды: `vim /etc/ssh/sshd_config`**
**Нашел строку, определяющую порт: Port 22, поменял его на 2022 и раскомментировал строку.**

![Port 22->2022](skreenshots/Part8/8_4.png)

> ps (от англ. process status) — программа в Unix-подобных операционных системах, выводящая отчёт о работающих процессах. Ключ -А (-е) позволяет вывести все процессы.

**4. Сохранила и перезапустила командой `sudo service sshd restart` и проверю командой `sudo service sshd status`**

![restart system](skreenshots/Part8/8_5.png)

**5.Проверяю статус фаервола командой `sudo ufw status` он был  отключен ( по умолчанию) `inactive`, я исправила это командой `sudo ufw enable`**

![checking firewall](skreenshots/Part8/8_6.png)

**6. Открываю порт командой `sudo ufw allow 2022` и еще раз проверяю статус**

![checking status](skreenshots/Part8/8_7.png)

**9. Перезапускаю систему командой  `sudo reboot`**

**10. Скачиваю утилиты netstat `sudo apt install net-tools`**

**11. Вывод команды  `netstat -tan`**

![netstan -tan](skreenshots/Part8/8_9.png)

>netstat (network statistics) — утилита командной строки, выводящая на дисплей состояние TCP-соединений (как входящих, так и исходящих), таблицы маршрутизации, число сетевых интерфейсов и сетевую статистику по протоколам. Основное назначение утилиты — поиск сетевых проблем и определение производительности сети.

#### Используемые ключи:
* t (--tcp) - показывать только TCP порты.
* a (--all) - показывать состояние всех сокетов.
* -n (--numeric) - показывать сетевые адреса как числа (например 127.0.0.53:53 вместо localhost:domain)

#### Значения столбцов 
* Proto - протокол, используемый сокетом. Так как была использована опция [-t|--tcp], в выводе пристутвуют только TCP-сокеты.
* Recv-Q - счётчик байт, не скопированных программой пользователя из этого сокета.
* Send-Q - счётчик байтов, не подтверждённых удалённым узлом.
* Local Address - адрес и номер порта локального конца сокета. Если указана опция [-n|--numeric], вывод в формате [адрес сокета:номер порта], иначе - [каноническое имя узла:соответствующее имя службы]. В интересующей нас строчке 0.0.0.0 - адрес локального конца сокета, 2022 - номер порта, который мы поменяли с 22 на 2022. Адрес 0.0.0.0 означает, что удаленный конец сокета будет доступен всем локальным ip-адресам.
* Foreign Address - адрес и номер порта удалённого конца сокета.
* State - состояние сокета. Состояние LISTEN означает, что сокет ожидает входящих подключений.
  

## Part 9. Установка и использование утилит **top**, **htop**

**1. Установка `sudo apt install top/htop`.**

**2. Запускаю `top`.**

![top](skreenshots/Part9/9_2.png)

* uptime - 5 min
* количество авторизованных пользователей - 1
* общую загрузку системы(load avarage) - 0.00, 0.01, 0.00
* общее количество процессов(Tasks) - 107 total, 1 running, 106 sleeping, 0 stopped, 0 zombie
* загрузка cpu - 0,3 us, 0,0 su, 0,0 ni, 94,8 id, 4,8 wa, 0,0 hi, 0,0 si, 0,0 st
* загрузка памяти (Mib Mem) - 3912,3 total, 3443,3 free, 182,3 used, 286,7 buff/cache
* pid процесса занимающего больше всего памяти - 1132
* pid процесса, занимающего больше всего процессорного времени - 1

**2. Cортировка по `PID`**
**Для сортировки по нужному параметру нажимаю `shift + F`, стрелочками перемещаюсь к нужному параметру и нажимаю `S` и `Q`.**

![top](skreenshots/Part9/9_3.png)

**3. отсортировка по `PERCENT_CPU`**
![cpu%](skreenshots/Part9/9_4.png)

**4. отсортировка по `PERCENT_MEM`**
![mem](skreenshots/Part9/9_5.png)

**5. отсортировка по `TIME`**
![time](skreenshots/Part9/9_6.png)

**Для фильтрации по нужному параметру нажимаю `O`, пишу команду `command=sshd` и нажимаю ENTER. Анологично фильтрую и по другим процессам.**

![sshd](skreenshots/Part9/9_7.png)

**6. отфлиртованный процесс `sshd`**
![filtr_sshd](skreenshots/Part9/9_8.png)

**7. `syslog`**
![syslog](skreenshots/Part9/9_10.png)

**8. с добавлением `hostname, clocl, uptime`**
![time](skreenshots/Part9/9_11.png)
 

## Part 10. Использование утилиты **fdisk**

**1. Запускаю команду `fdisk -l`** 

**`/dev/sda 10.53GIB, 11296309248 bytes, 22063104 sector`**

![fdisk](skreenshots/Part10/10_1.png)

* Имя /dev/sda
* Размер 30 Гб
* Колличество секторов 62914560
* Размер swap 2.7 Gb 
> Узнать размер swap можно с помощью команды `free -h`
![fdisk](skreenshots/Part10/10_3.png)


## Part 11. Использование утилиты **df** 

**1. Для корневого раздела запускаю команду `df /`**

![df](skreenshots/Part11/11_1.png)

* размер раздела - 14339080
* размер занятого пространства - 7121844
* размер свободного пространства - 6467056
* процент использования - 53%
* Единица измерения в выводе: Килобайт

**2. Для корневого раздела с командой `df -Th /`**

![df -Th](skreenshots/Part11/11_2.png)

* размер раздела - 14G
* размер занятого пространства - 6,8G
* размер свободного пространства - 6,2G
* процент использования - 53%
* тип файловой системы для раздела : ext4

## Part 12. Использование утилиты **du**
-Команда du позволяет задействовать одноименную утилиту, предназначенную для вывода информации об объеме дискового пространства, занятого файлами и директориями. Она принимает путь к элементу файловой системы и выводит информацию о количестве байт дискового пространства или блоков диска, задействованных для его хранения.

**1. Вывожу в байтах без без символьных ссылок командой `du -B1 -d0 /home /var /var/log`**

![/home /var /var/log](skreenshots/Part12/12_1.png)

**Вывожу в человеческом виде без без символьных ссылок командой `du -h -d0 /home /var /var/log`**

![/home /var /var/log](skreenshots/Part12/12_2.png)

**2. Вывожу размер всего содержимого в /var/log.**

![/var/log](skreenshots/Part12/12_3.png)


## Part 13. Установка и использование утилиты **ncdu**

**1. Устанавливаю  командой `sudo apt install ncdu`**
![install ncdu](skreenshots/Part13/13_1.png)

**2. `/home`**
![home](skreenshots/Part13/13_2.png)

**3. `/var`**
![var](skreenshots/Part13/13_4.png)

**4. `/var/log`**
![var/log](skreenshots/Part13/13_5.png)


## Part 14. Работа с системными журналами

**`/var/log/dmesg` содержит информацию о драйверах устройств**

![/](skreenshots/Part14/14_2.png)

**`/var/log/auth.log` информация об авторизации пользователей, включая удачные и неудачные попытки входа в систему, а также задействованные механизмы аутентификации.**

![auth](skreenshots/Part14/14_4.png)

**`/var/log/syslog` содержит глобальный системный журнал, в котором пишутся сообщения от ядра Linux, различных служб, сетевых интерфейсов и т.д. с момента запуска системы.**

![syslog](skreenshots/Part14/14_6.png)

**Информация о последней успешной авторизации: `sudo cat /var/log/auth.log | grep login`**

![login](skreenshots/Part14/14_8.png)

**Перезапустила SSHd службу `sudo systemctl restart ssh` и нашла логи в `/var/log/syslog`**

[sshd](skreenshots/Part14/14_10.png)


## Part 15. Использование планировщика заданий **CRON**

**1. Устанавливаю cron `sudo apt install cron`**

**2. Для создания задачи я открыла файл планировщик командой `crontab -e` и вписала следующую строку */2 * * * * uptime. Далее `crontab -l` позволяет посмотреть этот файл**

![crontab](skreenshots/Part15/15_3.png)

**2. Записи в /var/log/syslog:**
![logcron](skreenshots/Part15/15_5.png)

**3. Удаляю конфигурационный файл: `crontab -r`  и пытаюсь вывести список задач после удаления**
![delcron](skreenshots/Part15/15_6.png)

**Для меня нет списка задач, все отработало корректно.**