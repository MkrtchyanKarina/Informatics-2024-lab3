Отчет по лабораторной работе №3:
1. Установим серверную опреационную систему на базе ядра Linux - Ubuntu Server LTS

2. Создадим 3 виртуальные машины с одинаковыми характеристиками и выберем в ISO-образ файл с нужной ОС
   
3. Создадим две сети NAT, которые будут связывать "A" и "B", а также "A" и "C" соответственно. Для этого зайдем в меню "файл", раздел "Инструменты" и раздел "Менеджер Сетей". Там откроем вкладку "Сети NAT" и создадим новые сети. Сеть "AB" с IPv4 = 192.168.100.0 и сеть "AC" с IPv4 = 192.168.200.0 (оставим обеим сетям 24 порт по умолчанию)
   ![image](https://github.com/user-attachments/assets/b6a2a353-f594-49c3-b759-d0577507053d)
   
4. В настройках виртуальной машины "A" зайдем в раздел "Сеть". Для первого адаптера выберем тип - "Сеть NAT" - и созданную "AB", а для второго - "AC"
   
   ![image](https://github.com/user-attachments/assets/0c313084-36ef-4ee4-af39-be55493a961a)

   Адаптер 1
   
   ![image](https://github.com/user-attachments/assets/ac295916-2889-4aa9-a84b-46685c0e41b0)

   Адаптер 2
   
5. В настройках виртуальной машины "B" в том же разделе для первого адаптера выберем сеть "AB"
   ![image](https://github.com/user-attachments/assets/2b807796-d4d5-46d6-b8a9-29eaf9cb4528)
   
6. В настройках виртуальной машины "C" в том же разделе для первого адаптера выберем сеть "AC"
   ![image](https://github.com/user-attachments/assets/5990f355-80f5-44a4-9ae1-fe404acca8d0)

7. Запустим все виртуальные машины. Можно выбрать утилиту net-tools при установке ОС или установить ее через терминал командой sudo apt install net-tools

8. Так как мы выбрали тип подключения "Сеть NAT", то у всех 3-х машин будет доступ в интернет. Проверим наличие соединение и качество сети, отправив пакеты на какой-либо сайт, с помощью команды ping
   
   ![image](https://github.com/user-attachments/assets/149a11af-800f-4ecc-bec8-9567047759e3)

9. С помощью команды ifconfig мы можем увидеть информацию о назначенных сетевому адаптеру(интерфейсу) адресах

   Для машины "B":
   ![image](https://github.com/user-attachments/assets/2d54ebf0-2366-4aed-8386-e26b00590c8b)

   Для машины "C":
   ![image](https://github.com/user-attachments/assets/88d68d5b-5b6a-4591-ab41-fafcdb6c4a90)

   Для машины "A":
   ![image](https://github.com/user-attachments/assets/c9b7fce6-bd71-44a6-804e-428f7172d194)

10. У сервера "A" не установлен адрес для второго адаптера. Это может произойти по разным причинам, включая ошибки VirtualBox. Путей решения этой проблемы также несколько: можно изменить файл "interfaces" или настроить сеть через утилиту nmap.

11. Я выбрала самый простой способ - установить ip-адрес через консоль. Напишем для этого команду sudo ifconfig <имя_адаптера> <нужное значение ip>
    
    ![image](https://github.com/user-attachments/assets/96604a34-a925-4402-9f8c-dbe7ddb45a65)

12. Снова выполним команду ifconfig и убедимся, что теперь второму адаптеру соответствует адрес 192.168.200.5

    ![image](https://github.com/user-attachments/assets/7e3f1cd4-4c44-4572-a462-7f3c42ab3d51)

13. Проверим соединение между "A" и "C"

   ![image](https://github.com/user-attachments/assets/c9ef9653-887a-47aa-a822-973a288c5935)

14. Проверим соединение между "A" и "B"

    ![image](https://github.com/user-attachments/assets/a5e3bd15-6240-41f2-b76c-98ba1dbe482c)

15. Убедимся, что между серверами "B" и "C" отсутствует

    ![image](https://github.com/user-attachments/assets/d57cc68b-9bba-47e1-ab81-030d6da3c9c2)

    Как мы видим, все пакеты были переданы, но никакого ответа от другого сервера мы не получили

16. Скриншот терминалов всех 3-х машин
   ![image](https://github.com/user-attachments/assets/3c1afcb5-4e8d-4615-bd94-e176e394989f)


Дополнение: Связь машин друг с другом без доступа в интернет
1. В настройках машин "B" и "C" поменяем тип подключения на внутреннюю сеть и зададим ей соответствующие названия "AB" и "AC" 

   ![image](https://github.com/user-attachments/assets/ba2b9c00-8629-4da2-8bc8-76f2d4df7264)

   ![image](https://github.com/user-attachments/assets/3d30c1d7-b1d1-4564-8dec-486f0fbd0a01)

2. Выключим машину "A", чтобы добавить 3-й адаптер. Первым будет адаптер с типом подключения NAT (чтобы у этого сервера был доступ в интернет), второй и третий - с типом подключения "внутренняя сеть"
   ![image](https://github.com/user-attachments/assets/5a34c41f-10bb-4ced-aee5-cea8885d12f2)
   ![image](https://github.com/user-attachments/assets/418635f8-f5e3-467b-b2be-af50015e353c)
   ![image](https://github.com/user-attachments/assets/85c0929b-4ff8-4d1c-8928-7896bcbc5153)

3. Запустим машину "A" и выведем информацию о всех подключениях, если у адаптеров 2 и 3 нет адресов, то назначим их с помощью команды sudo ifconfig <имя_адаптера> <нужное значение ip> (enp0s8 - имя второго адаптера, enp0s9 - третьего)

   ![image](https://github.com/user-attachments/assets/df856398-c061-471e-b44f-e289ae6f4588)
4. Проверим наличие доступа в интернет для машины "A"

   ![image](https://github.com/user-attachments/assets/96d0eee3-7d9d-4227-88b6-0ed2bba19ceb)

5. Убедимся, что машины "B" и "C" не имеют доступа в интернет

   ![image](https://github.com/user-attachments/assets/20fb289f-d870-45f9-95d3-62f9eb7fb796)

6. Проверим наличие и отсутствие соединения между машинами
   ![image](https://github.com/user-attachments/assets/bb6bb1c9-1a37-4930-902a-52f4b8f8e699)

   Мы видим, что есть доступ из машины "B" в "A" и из "C" в "A", но нет доступа из "B" в "C" и наоборот. Также можем заметить, что к "чужому" адаптеру машины "A" они доступа также не имеют, тк находятся в разных внутренних сетях

7. Проверим доступ из "A" в "B" и "C"

   ![image](https://github.com/user-attachments/assets/92709d4d-a991-4b60-b2cf-cbc4e5f6bb36)

