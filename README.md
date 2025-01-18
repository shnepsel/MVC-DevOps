[![Deploy API](https://github.com/shnepsel/MVC-DevOps/actions/workflows/deploy_api.yml/badge.svg)](https://github.com/shnepsel/MVC-DevOps/actions/workflows/deploy_api.yml)

Для автоматического развертывания раннера необходимо:
1. Установить Ansible.
2. В inventory-файле `hosts` указать данные серверов, на которых необходимо развернуть раннеры.
3. Находясь в директории проекта `/MVC-DevOps/MVC-DevOps/ansible/` запустить команду `ansible-playbook playbook.yml -i hosts -u <user_name>`, где `<user_name>` — имя пользователя, от имени которого будет выполняться автоматизация.

Для запуска пайплайнов необходимо:
1. На github.com в репозитории проекта перейти в раздел 'Actions'.
2. Выбрать нужный workflow и далее, нажав кнопку `Run workflow`, запустить worflow с нужной ветки.