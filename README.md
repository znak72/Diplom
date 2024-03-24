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


</details>

#### 1.2 Группы безопасности соответствующих сервисов на входящий трафик только к нужным портам

![09_20SG](https://github.com/znak72/Diplom/blob/main/img/uu.png)

<details>
<summary> Скриншот(-ы) </summary>

![09_20SG](https://github.com/znak72/Diplom/blob/main/img/fdfd.png)

</details>

#### 1.3 Балансировщик нагрузки для распределения запросов на сайт и обеспечения безопасности

<details>
<summary> Скриншот(-ы) </summary>

![02_target-group](https://github.com/znak72/Diplom/blob/main/img/tg1.png)

![03_backend-group](https://github.com/znak72/Diplom/blob/main/img/bg1.png)

![7](https://github.com/znak72/Diplom/blob/main/img/alb1.png)

![7](https://github.com/znak72/Diplom/blob/main/img/alb11.png)

![7](https://github.com/znak72/Diplom/blob/main/img/w.png)

![7](https://github.com/znak72/Diplom/blob/main/img/bl.png)

</details>

### 2. Установка и настройка серверов на ВМ производилась с помощью плэйбуков  Ansible:

#### 2.1 Установка и настройка производилась через установленный на bastion host Ansible по ssh 

**[файл host inventory](https://github.com/znak72/Diplom/blob/main/ansible/hosts)**

<details>
<summary> Скриншот(-ы) </summary>

![00_Bastion-host](https://github.com/znak72/Diplom/blob/main/img/hosts.png)

</details>


####  2.2 Установка Elasticsearch и Kibana 

**[elasticsearch_playbook.yaml](https://github.com/znak72/Diplom/blob/main/ansible/elastik_playbook.yaml)**

* скачивает elasticsearch deb
* устанавливает elasticsearch
* корректирует конфигурационный файл

**[kibana_playbook.yaml](https://github.com/znak72/Diplom/blob/main/ansible/kibana_playbook.yaml)**

* устанавливает kibana
* корректирует конфигурационный файл
    Админка **[Kinbana](http://178.154.220.202:5601)**
   
<details>
<summary> Скриншот(-ы) </summary>

![28_ install](https://github.com/znak72/Diplom/blob/main/img/kibana.png)
![28_ install](https://github.com/znak72/Diplom/blob/main/img/elastic.png)
![28_ install](https://github.com/znak72/Diplom/blob/main/img/filebeat111.png)
![28_ install](https://github.com/znak72/Diplom/blob/main/img/filebeat222.png)
</details>

####  2.3 Установка NGINX на web сервера [nginx-playbook.yaml](https://github.com/RaffaelX/sys-gitlab-hw/blob/main/_diplom/ansible/nginx-playbook.yaml), [main.yml](https://github.com/RaffaelX/sys-gitlab-hw/blob/main/_diplom/ansible/nginx/tasks/main.yml)

* устанавливает nginx на ВМ linux-nginx-1, linux-nginx-2
* устанавливает начальную страницу сайта по шаблону j2, доступ через балансировщик **[ссылка](http://158.160.62.31:80)**

<details>
<summary> Скриншот(-ы) </summary>

![21_ install_nginx](https://github.com/znak72/Diplom/blob/main/img/nginx_playbook22.png)

![22_ install_nginx](https://github.com/znak72/Diplom/blob/main/img/nginx_playbook3.png)

![23_ install_nginx](https://github.com/znak72/Diplom/blob/main/img/nginx_playbook4.png)

</details>

#### 2.4 Установка filebeat на web сервера для сбора логов NGINX [filebeat_playbook.yaml](https://github.com/znak72/Diplom/blob/main/ansible/filebeat_playbook.yaml)

* скачивает filebeat deb
* устанавливает filebeat на сервера web-server-nginx-2 and web-server-nginx-1

<details>
<summary> Скриншот(-ы) </summary>

![28_20](https://github.com/znak72/Diplom/blob/main/img/filebeat.png)

</details>

#### 2.5 Установка zabbix-agent на ВМ [zabbix_agent_playbook.yaml](https://github.com/znak72/Diplom/blob/main/ansible/zabbix_agent_playbook.yaml), [main.yml](https://github.com/znak72/Diplom/blob/main/ansible/roles/zabbix-agent/tasks/main.yml)
  - добавляет репозиторий zabbix
  - устанавливает zabbix agent на все хосты
  - вносит корректировку в файл конфигурации  


<details>
<summary> Скриншот(-ы) </summary>

![25_install_zabbix_agent](https://github.com/znak72/Diplom/blob/main/img/zabbix-agent.png)

</details>

#### 2.6 Установка zabbix-server [zabbix_server_playbook.yaml](https://github.com/znak72/Diplom/blob/main/ansible/zabbix_server_playbook.yaml), [main.yml](https://github.com/znak72/Diplom/blob/main/ansible/roles/zabbix-server/tasks/main.yml)
  
  - добавляет репозиторий zabbix
  - устанавливает на хост zabbix -  zabbix server, zabbix agent, mysql, nginx
  - создает базу данных, пользователя, задает пароль
**[Админка zabbix-server](http://178.154.223.6:8080)**

<details>
<summary> Скриншот(-ы) </summary>

![24_install_zabbix_server](https://github.com/znak72/Diplom/blob/main/img/Ansible_Playbook_Zabbix-server_1.png)

![26_ installzabbix_server](https://github.com/znak72/Diplom/blob/main/img/Ansible_Playbook_Zabbix-server_2.png)

![27_ installzabbix_server](https://github.com/znak72/Diplom/blob/main/img/zabbix.png)

### Настраиваем дешборды с отображением метрик, минимальный набор — по принципу USE (Utilization, Saturation, Errors) для CPU, RAM, диски, сеть, http запросов к веб-серверам.

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
