#  Дипломная работа по профессии «Системный администратор»
# SYS-24

Ключевая задача — разработать отказоустойчивую инфраструктуру для сайта, включающую мониторинг, сбор логов и резервное копирование основных данных. Инфраструктура должна размещаться в Yandex Cloud и отвечать минимальным стандартам безопасности.

### 1\. Для выполнения задания был написан манифест terraform [main.tf](https://github.com/RaffaelX/sys-gitlab-hw/blob/main/_diplom/main.tf), котрый созает следующие ресурсы:

#### 1.1 Виртуальные машины.

  - bastion-hostt
  - elast
  - kibana
  - web-server-nginx-2
  - web-server-nginx-1
  - zabbix

<details>
<summary> Скриншот(-ы) </summary>

![01_vm](https://github.com/znak72/Diplom/blob/main/img/yandexcloud.png)

</details>


#### 2.5 Установка zabbix-agent на ВМ 
  - добавляет репозиторий zabbix
  - устанавливает zabbix agent на все хосты
  - вносит корректировку в файл конфигурации  


<details>
<summary> Скриншот(-ы) </summary>

![25_install_zabbix_agent](https://github.com/znak72/Diplom/blob/main/img/zabbix-agent.png)

</details>

#### 2.6 Установка zabbix-server 
  
  - добавляет репозиторий zabbix
  - устанавливает на хост zabbix -  zabbix server, zabbix agent, mysql, nginx
  - создает базу данных, пользователя, задает пароль
**[Админка zabbix-server](http://51.250.38.227:8080)**

<details>
<summary> Скриншот(-ы) </summary>

![24_install_zabbix_server](https://github.com/znak72/Diplom/blob/main/img/Ansible_Playbook_Zabbix-server_1.png)

![26_ installzabbix_server](https://github.com/znak72/Diplom/blob/main/img/Ansible_Playbook_Zabbix-server_2.png)

![27_ installzabbix_server](https://github.com/znak72/Diplom/blob/main/img/zabbix.png)

![27_ installzabbix_server](https://github.com/znak72/Diplom/blob/main/img/zabbix2.png)

</details>

### 3. Резервное копирование 
#### 3.1 Резервное копирование было настроено через terraform, подробрнее в файле main.tf

<details>
<summary> Скриншот(-ы) </summary>

![99_Snapshot_1](https://github.com/znak72/Diplom/blob/main/img/snapshot.png)

![99_Snapshot_2](https://github.com/znak72/Diplom/blob/main/img/snapshot2.png)

</details>

## Инфраструктура готова к эксплуатации
