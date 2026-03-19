# Отчет по практической работе: "Настройка локальной сети (Configure LAN)"

**Студент:** Геращенко Егор Сергеевич
**Группа:** 9СА-324К
**Номер по списку:** 3
**Дата выполнения:** 20.03.2026

## Сводная информация по аутентификации
SSH доступ: логин nsk_user, пароль cisco123. После восстановления пароля на R3: логин alex_admin, пароль P@ssw0rd. Доменные имена: nsk.local, msk.local. NTP ключ: cisco (ключ 1). SNMP community: public_cisco. FTP: логин ftp_user, пароль ftp_pass.

## Раздел 1. Инициализация коммутационного оборудования (Новосибирск)

### Шаг 1-2. Формирование топологии и приветственное сообщение
Собрана схема сети из двух коммутаторов SW0 и SW1 и маршрутизатора R1. На всех трех устройствах установлено приветственное сообщение. На R1 введены команды: enable, configure terminal, banner motd #Работу выполнил Петров Алексей, группа ИСП-211, номер в журнале 8!#, write memory. На SW0 и SW1 аналогично настроено banner motd. [СКРИНШОТ 1.1: Процесс настройки banner motd на R1] [СКРИНШОТ 1.2: Аналогичная настройка на SW0]

### Шаг 3. Присвоение имен устройствам согласно шаблону
В соответствии с заданием изменены hostname: на R1 введена команда hostname rus-msk-r1, на SW0 введена команда hostname rus-nsk-sw0, на SW1 введена команда hostname rus-nsk-sw1. [СКРИНШОТ 1.3: Изменение имени на SW1]

### Шаг 4. Конфигурация доменных имен
На новосибирских коммутаторах выполнена команда ip domain-name nsk.local. На московском маршрутизаторе выполнена команда ip domain-name msk.local.

### Шаг 5-6. Работа с VLAN
На обоих коммутаторах созданы VLAN 2, 3, 4 и выполнено закрепление портов доступа. Введены команды: vlan database, vlan 2, vlan 3, vlan 4, exit. Затем configure terminal, interface fastEthernet 0/2, switchport mode access, switchport access vlan 2, exit. Для f0/3: switchport mode access, switchport access vlan 3. Для f0/4: switchport mode access, switchport access vlan 4. [СКРИНШОТ 1.4: Создание VLAN на SW0] [СКРИНШОТ 1.5: Проверка через show vlan]

### Шаг 7. Организация EtherChannel
На SW0 введены команды: interface range gigabitEthernet 0/1-2, channel-group 1 mode desirable, exit, interface port-channel 1, switchport mode trunk. На SW1 введены команды: interface range gigabitEthernet 0/1-2, channel-group 1 mode auto, exit, interface port-channel 1, switchport mode trunk. Для проверки использованы команды show etherchannel summary и show interfaces trunk. [СКРИНШОТ 1.6: Статус EtherChannel] [СКРИНШОТ 1.7: Отображение транковых портов]

### Шаг 8-9. Настройка Management VLAN
На SW0 введены команды: interface vlan 1, ip address 1.0.0.50 255.0.0.0, no shutdown, exit, ip default-gateway 1.0.0.1. На SW1 введены команды: interface vlan 2, ip address 2.0.0.50 255.0.0.0, no shutdown, exit, ip default-gateway 2.0.0.1. [СКРИНШОТ 1.8: Интерфейс управления SW1]

### Шаг 10. Подготовка SSHv2
На обоих коммутаторах введены команды: crypto key generate rsa (выбрана длина ключа 1024 бита), username nsk_user secret 5 cisco123, line vty 0 4, transport input ssh, login local, exit, ip ssh version 2. [СКРИНШОТ 1.9: Генерация RSA ключа] [СКРИНШОТ 1.10: Параметры SSH]

### Шаг 11-12. Транковый порт и MOTD
На SW0 для порта f0/24 введена команда switchport mode trunk. На SW0 введена команда banner motd #Это rus-nsk-sw0#. На SW1 введена команда banner motd #Это rus-nsk-sw1#. [СКРИНШОТ 1.11: Настройка транка]

### Шаг 13. Повышение безопасности портов доступа
Для интерфейсов F0/2-4 на обоих коммутаторах применены команды: interface range fastEthernet 0/2-4, spanning-tree portfast, spanning-tree bpduguard enable, no cdp enable, switchport port-security, switchport port-security maximum 1, switchport port-security mac-address sticky, switchport port-security violation shutdown. [СКРИНШОТ 1.12: Параметры port-security]

### Шаг 14-17. Доработка консольного доступа
Выполнена настройка консоли и VTY командами: line console 0, login local, exec-timeout 0 0, logging synchronous, history size 256. Для VTY линий: line vty 0 4, exec-timeout 0 0. [СКРИНШОТ 1.13: Конфигурация консоли]

## Раздел 2. Маршрутизатор R1: Router-on-a-Stick и DHCP

### Шаг 1. Базовая настройка физического интерфейса
На R1 введены команды: interface fastEthernet 0/1, ip address 40.40.40.1 255.255.255.0, no shutdown.

### Шаг 2. Формирование сабинтерфейсов под VLAN
На R1 введены команды: interface fastEthernet 0/1.1, encapsulation dot1Q 1, ip address 1.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.2, encapsulation dot1Q 2, ip address 2.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.3, encapsulation dot1Q 3, ip address 3.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.4, encapsulation dot1Q 4, ip address 4.0.0.1 255.0.0.0, no shutdown. [СКРИНШОТ 2.1: Листинг сабинтерфейсов]

### Шаг 3. Настройка роли DHCP-сервера
Сначала исключены статические адреса командами: ip dhcp excluded-address 1.0.0.1 1.0.0.99, ip dhcp excluded-address 2.0.0.1 2.0.0.99, ip dhcp excluded-address 3.0.0.1 3.0.0.99, ip dhcp excluded-address 4.0.0.1 4.0.0.99. Затем созданы пулы адресов: ip dhcp pool VLAN1_POOL, network 1.0.0.0 255.0.0.0, default-router 1.0.0.1, dns-server 1.0.0.1, exit. ip dhcp pool VLAN2_POOL, network 2.0.0.0 255.0.0.0, default-router 2.0.0.1, dns-server 2.0.0.1, exit. ip dhcp pool VLAN3_POOL, network 3.0.0.0 255.0.0.0, default-router 3.0.0.1, dns-server 3.0.0.1, exit. ip dhcp pool VLAN4_POOL, network 4.0.0.0 255.0.0.0, default-router 4.0.0.1, dns-server 4.0.0.1, exit. [СКРИНШОТ 2.2: Параметры DHCP пулов]

### Шаг 4. Тестирование работоспособности
С R1 отправлен эхо-запрос на адрес 3.0.0.101 командой ping 3.0.0.101. [СКРИНШОТ 2.3: Результат ping]

## Раздел 3. Конфигурация многоуровневого коммутатора MLS

### Шаг 1-2. Первичные настройки
Устройству присвоено имя и активирована маршрутизация командами: hostname MLS, ip routing.

### Шаг 3. Создание VLAN 100 и 200 с именами
Введены команды: vlan 100, name Sales_dept, exit, vlan 200, name IT_dept, exit.

### Шаг 4. Привязка портов доступа
Введены команды: interface fastEthernet 0/4, switchport mode access, switchport access vlan 100, no shutdown, exit. interface fastEthernet 0/5, switchport mode access, switchport access vlan 200, no shutdown.

### Шаг 5. Конфигурация SVI для межвлановой маршрутизации
Введены команды: interface vlan 100, ip address 100.0.0.50 255.0.0.0, no shutdown, exit. interface vlan 200, ip address 200.0.0.50 255.255.255.0, no shutdown. [СКРИНШОТ 3.1: Состояние SVI интерфейсов]

### Шаг 6. Перевод физических портов в маршрутизируемый режим
Введены команды: interface fastEthernet 0/1, no switchport, ip address 11.0.0.50 255.0.0.0, no shutdown, exit. interface fastEthernet 0/2, no switchport, ip address 12.0.0.50 255.0.0.0, no shutdown, exit. interface fastEthernet 0/3, no switchport, ip address 40.40.40.50 255.255.255.0, no shutdown. [СКРИНШОТ 3.2: Таблица IP-адресов MLS]

### Шаг 7. Проверка
Выполнена команда ping 200.0.0.100. [СКРИНШОТ 3.3: Успешный пинг]

## Раздел 4. Маршрутизаторы R2, R3 и отказоустойчивость HSRP

### Шаг 1. Настройка интерфейсов R2
На R2 введены команды: interface f0/0, ip address 10.0.0.2 255.0.0.0, no shutdown, exit. interface f0/1, ip address 11.0.0.2 255.0.0.0, no shutdown.

### Шаг 2. Настройка интерфейсов R3
На R3 введены команды: interface f0/0, ip address 10.0.0.3 255.0.0.0, no shutdown, exit. interface f0/1, ip address 12.0.0.3 255.0.0.0, no shutdown. [СКРИНШОТ 4.1: IP-адресация R2, R3]

### Шаг 3. Протокол HSRP
На R2 введены команды: interface f0/0, standby version 2, standby 1 ip 10.0.0.1, standby 1 priority 110, standby 1 preempt, standby 1 track f0/1 30. На R3 введены команды: interface f0/0, standby version 2, standby 1 ip 10.0.0.1, standby 1 priority 100, standby 1 preempt. Проверка выполнена командами show standby brief и show standby. [СКРИНШОТ 4.2: Проверка HSRP статуса]

## Раздел 5. Протокол динамической маршрутизации EIGRP

### Шаг 1. Включение EIGRP AS 100
На R1 введены команды: router eigrp 100, network 1.0.0.0 0.255.255.255, network 2.0.0.0 0.255.255.255, network 3.0.0.0 0.255.255.255, network 4.0.0.0 0.255.255.255, network 40.40.40.0 0.0.0.255, no auto-summary. На MLS введены команды: router eigrp 100, network 11.0.0.0 0.255.255.255, network 12.0.0.0 0.255.255.255, network 40.40.40.0 0.0.0.255, network 100.0.0.0 0.255.255.255, network 200.0.0.0 0.0.0.255, no auto-summary. На R2 введены команды: router eigrp 100, network 10.0.0.0 0.255.255.255, network 11.0.0.0 0.255.255.255, no auto-summary. На R3 введены команды: router eigrp 100, network 10.0.0.0 0.255.255.255, network 12.0.0.0 0.255.255.255, no auto-summary. [СКРИНШОТ 5.1: Таблица маршрутизации R1]

### Шаг 2-3. Финальная проверка связности
С сервера выполнена проверка доступности коммутатора SW1 командами ping 2.0.0.50 и ssh -l nsk_user 2.0.0.50. [СКРИНШОТ 5.2: Результат SSH-подключения]

## Раздел 6. Реализация политик безопасности (ACL)

### Шаг 2. Ограничение доступа к веб-серверу
Создан список доступа 101, разрешающий только узлу 2.0.0.100 обращаться к серверу 10.0.0.100 по 80 порту командами: access-list 101 permit tcp host 2.0.0.100 host 10.0.0.100 eq 80, access-list 101 deny tcp any host 10.0.0.100 eq 80, access-list 101 permit ip any any. Затем применен на интерфейс: interface fastEthernet 0/1.2, ip access-group 101 in. [СКРИНШОТ 6.1: Правила ACL 101]

### Шаг 3. Запрет на ответ ICMP (ping)
На R2 настроен фильтр, блокирующий исходящие echo-reply командами: access-list 102 deny icmp any any echo-reply, access-list 102 permit ip any any, interface fastEthernet 0/0, ip access-group 102 in. [СКРИНШОТ 6.2: Попытка пинга R2 (timeout)]

## Раздел 7. Организация туннеля и RIPv2

### Шаг 1-2. Создание Loopback-интерфейсов
На R1 введены команды: interface loopback 1, ip address 192.168.101.1 255.255.255.0, no shutdown. На R3 введены команды: interface loopback 3, ip address 192.168.103.3 255.255.255.0, no shutdown. [СКРИНШОТ 7.1: Loopback интерфейсы]

### Шаг 3-4. Настройка туннеля GRE
На R1 введены команды: interface tunnel 0, ip address 200.200.200.1 255.255.255.0, tunnel source fastEthernet 0/1, tunnel destination 12.0.0.3, tunnel mode gre ip, no shutdown. На R3 введены команды: interface tunnel 0, ip address 200.200.200.3 255.255.255.0, tunnel source fastEthernet 0/1, tunnel destination 40.40.40.1, tunnel mode gre ip, no shutdown. [СКРИНШОТ 7.2: Статус туннеля]

### Шаг 3-4. RIPv2 через туннель
На R1 введены команды: router rip, version 2, network 192.168.101.0, network 200.200.200.0, no auto-summary. На R3 введены команды: router rip, version 2, network 192.168.103.0, network 200.200.200.0, no auto-summary. [СКРИНШОТ 7.3: Маршруты RIP]

### Шаг 6. Расширенная проверка
На R1 введена команда ping 192.168.103.3 source 192.168.101.1. [СКРИНШОТ 7.4: Успешный ping]

## Раздел 8. Сервисы управления и восстановление

### Шаг 1. Синхронизация времени и логирование
На всех маршрутизаторах и MLS применены команды: ntp server 10.0.0.100 key 1, ntp authenticate, ntp authentication-key 1 md5 cisco, ntp trusted-key 1, logging 10.0.0.100. [СКРИНШОТ 8.1: NTP статус]

### Шаг 2. SNMP-агент
На R2 и R3 введены команды: snmp-server community public_cisco ro, snmp-server community public_cisco rw.

### Шаг 3. AAA и Telnet на R3
На R3 введены команды: aaa new-model, tacacs-server host 10.0.0.100, tacacs-server key cisco, aaa authentication login default group tacacs+ local, line vty 0 4, transport input telnet, login authentication default. [СКРИНШОТ 8.2: Параметры AAA]

### Шаг 4-5. FTP на R2
На R2 введены команды: ip ftp username ftp_user, ip ftp password ftp_pass, copy running-config ftp: (при запросе введен адрес 10.0.0.100). [СКРИНШОТ 8.3: Процесс FTP-копирования]

### Шаг 6. TFTP на R3
На R3 введена команда copy running-config tftp: (при запросе введен адрес 10.0.0.100). [СКРИНШОТ 8.4: TFTP передача]

### Шаг 9. Процедура восстановления пароля R3
Выполнена последовательность действий для сброса и смены учетной записи. Начата перезагрузка командой reload. В течение 60 секунд выполнено прерывание Ctrl+Break для входа в ROMmon режим. В ROMmon введена команда confreg 0x2142, затем reset. После загрузки на вопрос о начальном диалоге введено no, нажата Return. В режиме enable введена команда copy startup-config running-config. Выполнен просмотр пользователей командой show running-config | include username. В режиме configure terminal введена команда no username nsk_user для удаления старого пользователя. Создан новый пользователь командой username alex_admin secret P@ssw0rd. Возвращен регистр конфигурации командой config-register 0x2102. Конфигурация сохранена командой copy running-config startup-config. Выполнена перезагрузка для проверки входа с новым паролем. [СКРИНШОТ 8.5: Режим ROMmon] [СКРИНШОТ 8.6: Создание нового пользователя]

## Итоговое заключение
В результате выполнения работы полностью сконфигурирована сеть предприятия, включающая сегментацию VLAN, агрегацию каналов, динамическую маршрутизацию EIGRP, отказоустойчивый шлюз HSRP, туннелирование, а также широкий спектр сервисов управления NTP, SNMP, AAA, FTP и TFTP. Проведена процедура восстановления пароля. Все проверки показали корректную работу оборудования и соответствие заданию.
