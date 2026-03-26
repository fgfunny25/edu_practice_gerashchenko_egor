# Отчет по практической работе: "Настройка локальной сети (Configure LAN)"

**Студент:** Геращенко Егор Сергеевич
**Группа:** 9СА-324К
**Номер по списку:** 3
**Дата выполнения:** 20.03.2026

## Сводная информация по аутентификации
SSH доступ: логин nsk_user, пароль cisco123. После восстановления пароля на R3: логин alex_admin, пароль P@ssw0rd. Доменные имена: nsk.local, msk.local. NTP ключ: cisco (ключ 1). SNMP community: public_cisco. FTP: логин ftp_user, пароль ftp_pass.

## Часть 1. Инициализация коммутационного оборудования (Новосибирск)

### Шаг 1-2. Формирование топологии и приветственное сообщение
Собрана схема сети из двух коммутаторов SW0 и SW1 и маршрутизатора R1. На всех трех устройствах установлено приветственное сообщение. На R1 введены команды: enable, configure terminal, banner motd #Работу выполнил Петров Алексей, группа ИСП-211, номер в журнале 8!#, write memory. На SW0 и SW1 аналогично настроено banner motd. <img width="570" height="104" alt="2026-03-20_16-12-27" src="https://github.com/user-attachments/assets/a7e6745f-b1c7-4520-a6b8-9448608be18b" /> <img width="602" height="146" alt="2026-03-20_16-15-33" src="https://github.com/user-attachments/assets/4d9aa594-6110-439c-b158-89bcfb01ed1d" /> <img width="566" height="120" alt="2026-03-20_16-17-06" src="https://github.com/user-attachments/assets/cdbcbc8b-5aa9-474e-8361-dd1809212cbf" />


### Шаг 3. Присвоение имен устройствам согласно шаблону
В соответствии с заданием изменены hostname: на R1 введена команда hostname rus-msk-r1, на SW0 введена команда hostname rus-nsk-sw0, на SW1 введена команда hostname rus-nsk-sw1. <img width="1164" height="452" alt="s6" src="https://github.com/user-attachments/assets/3e9e8554-ef76-4418-a744-c9c15c7e7703" />


### Шаг 4. Конфигурация доменных имен
На новосибирских коммутаторах выполнена команда ip domain-name nsk.local. На московском маршрутизаторе выполнена команда ip domain-name msk.local. <img width="640" height="518" alt="2026-03-17_14-30-04" src="https://github.com/user-attachments/assets/44c9ec30-66a7-443e-931c-ab7fc308c54f" /> <img width="530" height="310" alt="2026-03-17_14-31-12" src="https://github.com/user-attachments/assets/7c4e97f6-aa97-4f3f-a133-87b96d08d03d" />


### Шаг 5-6. Работа с VLAN
На обоих коммутаторах созданы VLAN 2, 3, 4 и выполнено закрепление портов доступа. Введены команды: vlan database, vlan 2, vlan 3, vlan 4, exit. Затем configure terminal, interface fastEthernet 0/2, switchport mode access, switchport access vlan 2, exit. Для f0/3: switchport mode access, switchport access vlan 3. Для f0/4: switchport mode access, switchport access vlan 4. <img width="1164" height="452" alt="s5-6" src="https://github.com/user-attachments/assets/af303d55-a1d2-48a3-9bce-7cb33bb2c7f0" />


### Шаг 7. Организация EtherChannel
На SW0 введены команды: interface range gigabitEthernet 0/1-2, channel-group 1 mode desirable, exit, interface port-channel 1, switchport mode trunk. На SW1 введены команды: interface range gigabitEthernet 0/1-2, channel-group 1 mode auto, exit, interface port-channel 1, switchport mode trunk. Для проверки использованы команды show etherchannel summary и show interfaces trunk.   <img width="1145" height="202" alt="s7" src="https://github.com/user-attachments/assets/c12ab60a-7cac-4c78-95da-ee2a9781dafa" />



### Шаг 8-9. Настройка Management VLAN
На SW0 введены команды: interface vlan 1, ip address 1.0.0.50 255.0.0.0, no shutdown, exit, ip default-gateway 1.0.0.1. На SW1 введены команды: interface vlan 2, ip address 2.0.0.50 255.0.0.0, no shutdown, exit, ip default-gateway 2.0.0.1.  <img width="1346" height="548" alt="s8" src="https://github.com/user-attachments/assets/464a803d-1ead-4198-84c4-8eeac57e8186" />  <img width="556" height="192" alt="s9" src="https://github.com/user-attachments/assets/4581df0c-0400-4a2f-821b-e82612e9b03d" />


### Шаг 10. Подготовка SSHv2
На обоих коммутаторах введены команды: crypto key generate rsa (выбрана длина ключа 1024 бита), username nsk_user secret 5 cisco123, line vty 0 4, transport input ssh, login local, exit, ip ssh version 2. <img width="640" height="518" alt="2026-03-17_14-30-04" src="https://github.com/user-attachments/assets/e4eea88a-8864-43e5-803a-187abb472590" />  <img width="530" height="310" alt="2026-03-17_14-31-12" src="https://github.com/user-attachments/assets/ffcda586-ba34-4c87-a95a-89fd987912c0" />


### Шаг 11-12. Транковый порт и MOTD
На SW0 для порта f0/24 введена команда switchport mode trunk. На SW0 введена команда banner motd #Это rus-nsk-sw0#. На SW1 введена команда banner motd #Это rus-nsk-sw1#.  <img width="564" height="60" alt="s11" src="https://github.com/user-attachments/assets/df43f73b-e6be-4a50-8300-cfe105c0e205" />  <img width="1096" height="42" alt="s12" src="https://github.com/user-attachments/assets/2016e0ac-f27b-4ce9-9999-9369c060ab6b" />


### Шаг 13. Повышение безопасности портов доступа
Для интерфейсов F0/2-4 на обоих коммутаторах применены команды: interface range fastEthernet 0/2-4, spanning-tree portfast, spanning-tree bpduguard enable, no cdp enable, switchport port-security, switchport port-security maximum 1, switchport port-security mac-address sticky, switchport port-security violation shutdown. <img width="1366" height="698" alt="s13" src="https://github.com/user-attachments/assets/5d80b6c1-bf62-47e5-8385-f21771b46a32" />


### Шаг 14-17. Доработка консольного доступа
Выполнена настройка консоли и VTY командами: line console 0, login local, exec-timeout 0 0, logging synchronous, history size 256. Для VTY линий: line vty 0 4, exec-timeout 0 0. <img width="570" height="868" alt="2026-03-17_14-54-43" src="https://github.com/user-attachments/assets/4f090bb6-2920-4d2c-bc7f-df21b3774f2d" />  <img width="640" height="812" alt="2026-03-17_14-56-12" src="https://github.com/user-attachments/assets/134d50ee-0f74-455e-bbc6-57e5f6a794e6" />




## Часть 2. Маршрутизатор R1: Router-on-a-Stick и DHCP

### Шаг 1. Базовая настройка физического интерфейса
На R1 введены команды: interface fastEthernet 0/1, ip address 40.40.40.1 255.255.255.0, no shutdown.

### Шаг 2. Формирование сабинтерфейсов под VLAN
На R1 введены команды: interface fastEthernet 0/1.1, encapsulation dot1Q 1, ip address 1.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.2, encapsulation dot1Q 2, ip address 2.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.3, encapsulation dot1Q 3, ip address 3.0.0.1 255.0.0.0, no shutdown, exit. interface fastEthernet 0/1.4, encapsulation dot1Q 4, ip address 4.0.0.1 255.0.0.0, no shutdown. <img width="620" height="164" alt="s1" src="https://github.com/user-attachments/assets/ae95d0cb-133c-4434-b46b-c2ab2a618af8" />


### Шаг 3. Настройка роли DHCP-сервера
Сначала исключены статические адреса командами: ip dhcp excluded-address 1.0.0.1 1.0.0.99, ip dhcp excluded-address 2.0.0.1 2.0.0.99, ip dhcp excluded-address 3.0.0.1 3.0.0.99, ip dhcp excluded-address 4.0.0.1 4.0.0.99. Затем созданы пулы адресов: ip dhcp pool VLAN1_POOL, network 1.0.0.0 255.0.0.0, default-router 1.0.0.1, dns-server 1.0.0.1, exit. ip dhcp pool VLAN2_POOL, network 2.0.0.0 255.0.0.0, default-router 2.0.0.1, dns-server 2.0.0.1, exit. ip dhcp pool VLAN3_POOL, network 3.0.0.0 255.0.0.0, default-router 3.0.0.1, dns-server 3.0.0.1, exit. ip dhcp pool VLAN4_POOL, network 4.0.0.0 255.0.0.0, default-router 4.0.0.1, dns-server 4.0.0.1, exit. <img width="632" height="500" alt="s2" src="https://github.com/user-attachments/assets/8196113d-5add-456e-8f69-df4280c817a4" />

### Шаг 4. Тестирование работоспособности
С R1 отправлен эхо-запрос на адрес 3.0.0.101 командой ping 3.0.0.101. 



## Часть 3. Конфигурация многоуровневого коммутатора MLS

### Шаг 1-2. Первичные настройки
Устройству присвоено имя и активирована маршрутизация командами: hostname MLS, ip routing. 

### Шаг 3. Создание VLAN 100 и 200 с именами
Введены команды: vlan 100, name Sales_dept, exit, vlan 200, name IT_dept, exit. <img width="462" height="176" alt="s1 - s3 " src="https://github.com/user-attachments/assets/014d69a4-196f-468c-bef5-d7186815ce61" />


### Шаг 4. Привязка портов доступа
Введены команды: interface fastEthernet 0/4, switchport mode access, switchport access vlan 100, no shutdown, exit. interface fastEthernet 0/5, switchport mode access, switchport access vlan 200, no shutdown. <img width="398" height="106" alt="s4" src="https://github.com/user-attachments/assets/92daf0ac-fe5d-4523-89cf-7e2455d45849" />


### Шаг 5. Конфигурация SVI для межвлановой маршрутизации
Введены команды: interface vlan 100, ip address 100.0.0.50 255.0.0.0, no shutdown, exit. interface vlan 200, ip address 200.0.0.50 255.255.255.0, no shutdown. <img width="596" height="202" alt="s5" src="https://github.com/user-attachments/assets/a86cc668-13cf-4508-8c68-6d3e8a544fbd" />


### Шаг 6. Перевод физических портов в маршрутизируемый режим
Введены команды: interface fastEthernet 0/1, no switchport, ip address 11.0.0.50 255.0.0.0, no shutdown, exit. interface fastEthernet 0/2, no switchport, ip address 12.0.0.50 255.0.0.0, no shutdown, exit. interface fastEthernet 0/3, no switchport, ip address 40.40.40.50 255.255.255.0, no shutdown. <img width="534" height="184" alt="s6" src="https://github.com/user-attachments/assets/bd36be78-b7ba-40ea-bfb3-9a5a9a566fbf" />

### Шаг 7. Проверка
Выполнена команда ping 200.0.0.100.  <img width="594" height="586" alt="ping" src="https://github.com/user-attachments/assets/6e88a071-0b27-4ad8-88b8-443d2e58b23d" />




## Часть 4. Маршрутизаторы R2, R3 и отказоустойчивость HSRP

### Шаг 1. Настройка интерфейсов R2
На R2 введены команды: interface f0/0, ip address 10.0.0.2 255.0.0.0, no shutdown, exit. interface f0/1, ip address 11.0.0.2 255.0.0.0, no shutdown. <img width="625" height="317" alt="2026-03-17_15-57-33" src="https://github.com/user-attachments/assets/6f9d324b-0ea8-4d39-99f5-7822bc09c04e" />


### Шаг 2. Настройка интерфейсов R3
На R3 введены команды: interface f0/0, ip address 10.0.0.3 255.0.0.0, no shutdown, exit. interface f0/1, ip address 12.0.0.3 255.0.0.0, no shutdown. <img width="636" height="507" alt="2026-03-17_16-02-58" src="https://github.com/user-attachments/assets/c0c949d8-4869-42da-b940-d9efbcac2410" />


### Шаг 3. Протокол HSRP
На R2 введены команды: interface f0/0, standby version 2, standby 1 ip 10.0.0.1, standby 1 priority 110, standby 1 preempt, standby 1 track f0/1 30. На R3 введены команды: interface f0/0, standby version 2, standby 1 ip 10.0.0.1, standby 1 priority 100, standby 1 preempt. Проверка выполнена командами show standby brief и show standby. <img width="642" height="523" alt="2026-03-17_16-15-04" src="https://github.com/user-attachments/assets/914adf34-7966-49a4-a2f1-265af0142838" /> <img width="634" height="521" alt="2026-03-17_16-23-54" src="https://github.com/user-attachments/assets/7619533c-952d-43fb-92b6-e96911f2aa24" />



## Часть 5. Протокол динамической маршрутизации EIGRP

### Шаг 1. Включение EIGRP AS 100
На R1 введены команды: router eigrp 100, network 1.0.0.0 0.255.255.255, network 2.0.0.0 0.255.255.255, network 3.0.0.0 0.255.255.255, network 4.0.0.0 0.255.255.255, network 40.40.40.0 0.0.0.255, no auto-summary. На MLS введены команды: router eigrp 100, network 11.0.0.0 0.255.255.255, network 12.0.0.0 0.255.255.255, network 40.40.40.0 0.0.0.255, network 100.0.0.0 0.255.255.255, network 200.0.0.0 0.0.0.255, no auto-summary. На R2 введены команды: router eigrp 100, network 10.0.0.0 0.255.255.255, network 11.0.0.0 0.255.255.255, no auto-summary. На R3 введены команды: router eigrp 100, network 10.0.0.0 0.255.255.255, network 12.0.0.0 0.255.255.255, no auto-summary.  
<img width="510" height="164" alt="2026-03-17_16-19-37" src="https://github.com/user-attachments/assets/c55f0662-94e6-41cd-8e51-ef41b457db7f" /> <img width="557" height="170" alt="2026-03-17_16-22-05(1)(1)" src="https://github.com/user-attachments/assets/cd2c49ab-b28b-4229-93c4-9b106eafdb20" />



### Шаг 2-3. Финальная проверка связности
С сервера выполнена проверка доступности коммутатора SW1 командами ping 2.0.0.50 и ssh -l nsk_user 2.0.0.50. 
<img width="557" height="170" alt="2026-03-17_16-22-05" src="https://github.com/user-attachments/assets/6eebdbd9-a3c7-4a40-b178-d620468c7e5b" /> <img width="636" height="345" alt="2026-03-17_16-26-51" src="https://github.com/user-attachments/assets/1801d879-afc1-4a42-be16-671ae934641c" />



## Часть 6. Реализация политик безопасности (ACL)

### Шаг 2. Ограничение доступа к веб-серверу
Создан список доступа 101, разрешающий только узлу 2.0.0.100 обращаться к серверу 10.0.0.100 по 80 порту командами: access-list 101 permit tcp host 2.0.0.100 host 10.0.0.100 eq 80, access-list 101 deny tcp any host 10.0.0.100 eq 80, access-list 101 permit ip any any. Затем применен на интерфейс: interface fastEthernet 0/1.2, ip access-group 101 in.  <img width="635" height="126" alt="создание ACL на R1" src="https://github.com/user-attachments/assets/50fc41df-24bf-4cd8-9480-f33032612169" />


### Шаг 3. Запрет на ответ ICMP (ping)
На R2 настроен фильтр, блокирующий исходящие echo-reply командами: access-list 102 deny icmp any any echo-reply, access-list 102 permit ip any any, interface fastEthernet 0/0, ip access-group 102 in. <img width="525" height="126" alt="запрет на ответ ping на R2" src="https://github.com/user-attachments/assets/e105175a-f27d-4043-b48e-d12a497eacc1" /> <img width="502" height="125" alt="запрет на ответ ping на R3" src="https://github.com/user-attachments/assets/4d5923bf-de9f-4c81-b758-49082edbe620" />



## Часть 7. Организация туннеля и RIPv2

### Шаг 1-2. Создание Loopback-интерфейсов
На R1 введены команды: interface loopback 1, ip address 192.168.101.1 255.255.255.0, no shutdown. На R3 введены команды: interface loopback 3, ip address 192.168.103.3 255.255.255.0, no shutdown. <img width="476" height="178" alt="Loopback и RIPv2 на R1" src="https://github.com/user-attachments/assets/11d18bc3-f854-4371-a403-c35d7099743d" />


### Шаг 3-4. Настройка туннеля GRE
На R1 введены команды: interface tunnel 0, ip address 200.200.200.1 255.255.255.0, tunnel source fastEthernet 0/1, tunnel destination 12.0.0.3, tunnel mode gre ip, no shutdown. На R3 введены команды: interface tunnel 0, ip address 200.200.200.3 255.255.255.0, tunnel source fastEthernet 0/1, tunnel destination 40.40.40.1, tunnel mode gre ip, no shutdown. <img width="634" height="228" alt="Loopback и RIPv2 на R3" src="https://github.com/user-attachments/assets/faf98bfa-df18-4c46-8e5b-d5b4457f056f" />

### Шаг 3-4. RIPv2 через туннель
На R1 введены команды: router rip, version 2, network 192.168.101.0, network 200.200.200.0, no auto-summary. На R3 введены команды: router rip, version 2, network 192.168.103.0, network 200.200.200.0, no auto-summary. <img width="580" height="176" alt="создание туннеля RIPv2 на R1" src="https://github.com/user-attachments/assets/dde736db-707c-410a-9a2c-dca7a6cc76c8" />

### Шаг 6. Расширенная проверка
На R1 введена команда ping 192.168.103.3 source 192.168.101.1. 



## Часть 8. Сервисы управления и восстановление

### Шаг 1. Синхронизация времени и логирование
На всех маршрутизаторах и MLS применены команды: ntp server 10.0.0.100 key 1, ntp authenticate, ntp authentication-key 1 md5 cisco, ntp trusted-key 1, logging 10.0.0.100. <img width="462" height="146" alt="NTP, SYSLOG R1" src="https://github.com/user-attachments/assets/710ec923-9058-408d-a6bb-45f61105535d" /> <img width="700" height="226" alt="NTP, SYSLOG R2" src="https://github.com/user-attachments/assets/fde9db7f-93f9-490a-8c14-48d60bf1aaca" /> <img width="506" height="170" alt="NTP, SYSLOG R3" src="https://github.com/user-attachments/assets/15a8f64a-a3d9-44bb-be8c-285d51f6f69e" />




### Шаг 2. SNMP-агент
На R2 и R3 введены команды: snmp-server community public_cisco ro, snmp-server community public_cisco rw. <img width="498" height="92" alt="SNMP R2" src="https://github.com/user-attachments/assets/796e0bae-30fb-4bba-b737-27db8076bd0a" />  <img width="470" height="84" alt="SNMP R3" src="https://github.com/user-attachments/assets/d02cc128-4902-48a6-8c2b-0e7dd10838f5" />


### Шаг 3. AAA и Telnet на R3
На R3 введены команды: aaa new-model, tacacs-server host 10.0.0.100, tacacs-server key cisco, aaa authentication login default group tacacs+ local, line vty 0 4, transport input telnet, login authentication default. <img width="564" height="108" alt="Telnet +AAA R3" src="https://github.com/user-attachments/assets/857f4a0a-c2d2-4826-90f8-c20124fa4d82" />


### Шаг 4-5. FTP на R2
На R2 введены команды: ip ftp username ftp_user, ip ftp password ftp_pass, copy running-config ftp: (при запросе введен адрес 10.0.0.100). <img width="506" height="258" alt="FTP R2" src="https://github.com/user-attachments/assets/5d349180-b1a4-4355-b6e9-e2875850de7a" />

### Шаг 6. TFTP на R3
На R3 введена команда copy running-config tftp: (при запросе введен адрес 10.0.0.100). 

### Шаг 9. Процедура восстановления пароля R3
Выполнена последовательность действий для сброса и смены учетной записи. Начата перезагрузка командой reload. В течение 60 секунд выполнено прерывание Ctrl+Break для входа в ROMmon режим. В ROMmon введена команда confreg 0x2142, затем reset. После загрузки на вопрос о начальном диалоге введено no, нажата Return. В режиме enable введена команда copy startup-config running-config. Выполнен просмотр пользователей командой show running-config | include username. В режиме configure terminal введена команда no username nsk_user для удаления старого пользователя. Создан новый пользователь командой username alex_admin secret P@ssw0rd. Возвращен регистр конфигурации командой config-register 0x2102. Конфигурация сохранена командой copy running-config startup-config. Выполнена перезагрузка для проверки входа с новым паролем. <img width="852" height="934" alt="TFTP R3" src="https://github.com/user-attachments/assets/c1b45461-b39b-4cb9-8ad9-050670980923" />


## Итоговое заключение
В результате выполнения работы полностью сконфигурирована сеть предприятия, включающая сегментацию VLAN, агрегацию каналов, динамическую маршрутизацию EIGRP, отказоустойчивый шлюз HSRP, туннелирование, а также широкий спектр сервисов управления NTP, SNMP, AAA, FTP и TFTP. Проведена процедура восстановления пароля. Все проверки показали корректную работу оборудования и соответствие заданию.
