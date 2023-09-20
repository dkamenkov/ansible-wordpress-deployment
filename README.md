
# Ansible Wordpress Deployment

Этот проект содержит конфигурацию Ansible для автоматической установки и настройки Wordpress на CentOS 7.X.

## Требования

- Ansible (версия 2.9 или выше)
- Целевая машина на CentOS 7.X

## Инструкция по установке

1. Клонировать этот репозиторий:
```
git clone <URL репозитория>
cd ansible-wordpress-deployment
```

2. Настройте файл `hosts.ini`, указав IP-адрес вашего сервера.

3. 
Укажите ваши настройки базы данных в файле `group_vars/webserver/vars.yml`. Для шифрования паролей используйте `ansible-vault`:

3. Шифрование паролей с использованием `ansible-vault`:
   - Установите пароль для `ansible-vault` (лучше всего сохранить его в безопасном месте, так как он потребуется при выполнении playbook):
     ```
     echo "your_vault_password" > vault-password.txt
     ```
   - Шифруйте каждый пароль отдельно с помощью следующей команды:
     ```
     ansible-vault encrypt_string 'your_plain_text_password' --name 'variable_name' --vault-password-file vault-password.txt
     ```
   - Вставьте зашифрованный результат в ваш файл `group_vars/webserver/vars.yml`.
   - При запуске playbook используйте флаг `--vault-password-file vault-password.txt` для предоставления пароля.
    

4. Запустите playbook:
```
ansible-playbook -i hosts.ini wordpress-playbook.yml --ask-vault-pass
```

## Структура проекта

- `hosts.ini`: Файл с инвентаризацией, содержит IP-адреса целевых машин.
- `wordpress-playbook.yml`: Главный playbook, который применяет все роли к целевым машинам.
- `group_vars/`: Директория с групповыми переменными. Здесь хранятся настройки для базы данных.
- `roles/`: Директория с ролями. Включает в себя роль для установки Wordpress и другие возможные роли.

## Заметки по реализации


Проект использует роль из Ansible Galaxy для установки и настройки MySQL. Для использования этой роли:

1. Установите роль из Ansible Galaxy:
```
ansible-galaxy install geerlingguy.mysql
```

2. Добавьте эту роль в ваш playbook перед ролью установки Wordpress:

```yaml
roles:
  - geerlingguy.mysql
  - wordpress-role
```

3. Настройте переменные для роли MySQL в `group_vars/webserver/vars.yml`:

```yaml
mysql_root_password: "your_root_password"
mysql_databases:
  - name: wordpress_db
mysql_users:
  - name: wordpress_user
    password: "your_db_password"
    priv: "wordpress_db.*:ALL"
```

4. Примените playbook:
```
ansible-playbook -i hosts.ini wordpress-playbook.yml --ask-vault-pass
```
     Роль для установки Wordpress написана вручную и находится в директории `roles/wordpress-role`.

## Обратная связь и вклад

Если у вас есть предложения или замечания к этому проекту, создайте issue или pull request в репозитории.
