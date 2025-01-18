[![Deploy API](https://github.com/shnepsel/MVC-DevOps/actions/workflows/deploy_api.yml/badge.svg)](https://github.com/shnepsel/MVC-DevOps/actions/workflows/deploy_api.yml)

### Для автоматического развертывания раннера необходимо:
1. Установить Ansible.
2. В inventory-файле `hosts` указать данные серверов, на которых необходимо развернуть раннеры.
3. Находясь в директории проекта `/MVC-DevOps/MVC-DevOps/ansible/` запустить команду `ansible-playbook playbook.yml -i hosts -u <user_name>`, где `<user_name>` — имя пользователя, от имени которого будет выполняться автоматизация.

### Для запуска пайплайнов необходимо:
1. На github.com в репозитории проекта перейти в раздел 'Actions'.
2. Выбрать нужный workflow и далее, нажав кнопку `Run workflow`, запустить worflow с нужной ветки.



### Процесс запуска системы мониторинга

1. Подготовка DNS:
   - Добавьте записи grafana.infra.norafith.ru и prometheus.infra.norafith.ru в DNS.
   - Временно можно использовать /etc/hosts для локальной настройки.

2. Выпуск SSL-сертификатов:
   - Установите Certbot:  
          sudo apt update && sudo apt install certbot
     
   - Выпустите сертификаты:  
          sudo certbot certonly --standalone -d grafana.infra.norafith.ru -d prometheus.infra.norafith.ru
     
   - Скопируйте сертификаты в рабочую директорию:  
          sudo cp /etc/letsencrypt/live/<домен>/fullchain.pem /var/certs/infra-norafith.crt
     sudo cp /etc/letsencrypt/live/<домен>/privkey.pem /var/certs/infra-norafith.pem
     

3. Запуск контейнеров:
      docker-compose up -d
   

4. Доступ к интерфейсам:
   - Grafana: [https://grafana.infra.norafith.ru](https://grafana.infra.norafith.ru)
   - Prometheus: [https://prometheus.infra.norafith.ru](https://prometheus.infra.norafith.ru)