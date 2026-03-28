# Отчет по лабораторной работе
## Тема: "Настройка WAN. Протоколы PPP, OSPF, BGP, DHCP Relay, IPv6, OSPFv3, EIGRPv6"

**Студент:** Геращенко Е.С.  
**Группа:** 9СА-324К  
**Дата выполнения:** 25.03.2026

---

## Цель работы
Настройка протоколов маршрутизации (OSPF, BGP, EIGRP), DHCP Relay, IPv6 и аутентификации в сети предприятия.

---

## Топология сети

<img width="984" height="359" alt="Image" src="https://github.com/user-attachments/assets/4ae1c510-2347-4c89-813b-62a6102c9c3b" />

---


## Часть 1: Базовая настройка IP-адресов (IPv4)

### Настройка R1

<img width="699" height="828" alt="Image" src="https://github.com/user-attachments/assets/eadbf644-d100-473f-89c7-ef87e504c8f5" />

### Настройка R2

<img width="709" height="797" alt="Image" src="https://github.com/user-attachments/assets/0ba8ba37-e77e-412e-b78f-8c7b1cc74767" />

### Настройка R3

<img width="704" height="1037" alt="Image" src="https://github.com/user-attachments/assets/323c9df0-74aa-4e6d-a4a6-b2ec3bd1bc4d" />

### Настройка R1973

<img width="708" height="864" alt="Image" src="https://github.com/user-attachments/assets/adecc796-37b9-4873-942c-2d91bda1cd12" />




## Часть 2: Настройка PPP с CHAP аутентификацией

### Настройка R3

<img width="693" height="1034" alt="Image" src="https://github.com/user-attachments/assets/0daafda3-e585-4728-9c38-febfbbc56745" />

### Настройка R1973

<img width="697" height="1034" alt="Image" src="https://github.com/user-attachments/assets/6267a8bd-5196-46ef-9451-dc4466a70c17" />



## Часть 3: Настройка OSPFv2 (IPv4)

### Настройка R1

<img width="708" height="773" alt="Image" src="https://github.com/user-attachments/assets/c63c4c9c-452f-4ddc-a4a9-c310d5e72226" />

### Настройка R2

<img width="699" height="712" alt="Image" src="https://github.com/user-attachments/assets/a0464bac-c423-48ba-8ba1-c6a8b7188b1f" />

### Настройка R3

<img width="706" height="872" alt="Image" src="https://github.com/user-attachments/assets/f1ba02a3-d08d-448a-a6d7-f4977f10a5cf" />




## Часть 4: Настройка BGP

### Настройка R3

<img width="707" height="713" alt="Image" src="https://github.com/user-attachments/assets/37aad3e6-e848-4216-ac32-219c19783ee9" />

### Настройка R1973

<img width="708" height="939" alt="Image" src="https://github.com/user-attachments/assets/f35512f8-7d2d-4f42-8b8e-7f07b6d3faae" />




## Часть 5: Установка лицензий (VOIP/Security)

<img width="696" height="1033" alt="Image" src="https://github.com/user-attachments/assets/9c635fb1-4fdc-4186-b7fa-e36b918a3eb4" />




## Часть 6: Настройка DHCP Relay

### Настройка DHCP-сервера

<img width="706" height="719" alt="Image" src="https://github.com/user-attachments/assets/51b8b46d-9c05-4234-b5db-7fbf733a5749" />

### Настройка R1 как DHCP Relay

<img width="711" height="834" alt="Image" src="https://github.com/user-attachments/assets/3eb2baa7-417d-42be-b363-668c9a5ce11d" />

### Проверка, ping 10.23.23.100 с PC0

<img width="707" height="721" alt="Image" src="https://github.com/user-attachments/assets/7a25b739-fb5b-4a94-9c3e-363a53836645" />




## Часть 7: Настройка IPv6 адресов

### Настройка R1

<img width="707" height="717" alt="Image" src="https://github.com/user-attachments/assets/a9950e55-5cbc-466c-8c6d-7371448c7845" />

### Настройка R2

<img width="707" height="719" alt="Image" src="https://github.com/user-attachments/assets/57b5fb4d-e227-456d-ac9f-0f8ff4184fc1" />

### Настройка R3

<img width="706" height="717" alt="Image" src="https://github.com/user-attachments/assets/e25b6c3a-7f46-4b41-b308-1c94180053a4" />

### Настройка R1973

<img width="704" height="713" alt="Image" src="https://github.com/user-attachments/assets/c5489de0-c844-4832-bd39-9f9a20a02c46" />




## Часть 8: Настройка OSPFv3 (IPv6)

### Настройка R1

<img width="704" height="717" alt="Image" src="https://github.com/user-attachments/assets/89e7b9da-7656-4a7c-97ca-a2650432abb9" />

### Настройка R2

<img width="709" height="719" alt="Image" src="https://github.com/user-attachments/assets/d70bad8d-8edf-41e5-a8fe-89d056c7b5f1" />

### Настройка R3

<img width="703" height="717" alt="Image" src="https://github.com/user-attachments/assets/b624c6f1-71b7-4c07-8789-ae8a67fe1aca" />




## Часть 9: Настройка EIGRPv6

### Настройка R3

<img width="704" height="718" alt="Image" src="https://github.com/user-attachments/assets/47b644c5-b7e8-4906-afef-363531baa1e6" />

### Настройка R1973

<img width="704" height="718" alt="Image" src="https://github.com/user-attachments/assets/5eb60fd5-e059-4caf-a16a-a34672206500" />


Заключение
1.В ходе выполнения лабораторной работы были настроены:

2.Базовая IP-адресация (IPv4) на всех маршрутизаторах

3.PPP с CHAP аутентификацией между R3 и R1973

4.OSPFv2 с распределением по зонам (Area 0, 1, 23)

5.BGP между AS 3 и AS 1973

6.DHCP Relay с сервером 10.23.23.100

7.IPv6 адресация на всех устройствах

8.OSPFv3 для IPv6 маршрутизации

9.EIGRPv6 между R3 и R1973

Все протоколы работают корректно, связность между всеми сегментами сети обеспечена. Проверка показала успешное получение IP-адреса PC0 по DHCP, успешную маршрутизацию IPv4 и IPv6 трафика, а также корректную работу протоколов динамической маршрутизации.
