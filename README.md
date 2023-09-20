
# Ansible Wordpress Deployment

Этот проект содержит конфигурацию Ansible для автоматической установки и настройки Wordpress на CentOS 7.X.

## Требования

- Ansible (версия 2.9 или выше)
- Целевая машина на CentOS 7.X

## Инструкция по установке

1. **Клонирование репозитория**:
   ```bash
   git clone <URL репозитория>
   cd ansible-wordpress-deployment
   ```
   
2. **Установка необходимых ролей из Ansible Galaxy**:
   ```bash
   ansible-galaxy install geerlingguy.mysql
   ```

3. **Настройка инвентарного файла**:
   Отредактируйте файл `hosts.ini`, указав IP-адрес вашего сервера.

4. **Настройка базы данных**:
   Укажите ваши настройки базы данных в файле `group_vars/webserver/vars.yml`. Для шифрования паролей используйте `ansible-vault`:

   - Установите пароль для `ansible-vault`:
     ```bash
     echo "your_vault_password" > vault-password.txt
     ```
   - Шифруйте каждый пароль отдельно:
     ```bash
     ansible-vault encrypt_string 'your_plain_text_password' --name 'variable_name' --vault-password-file vault-password.txt
     ```
   - Вставьте зашифрованный результат в ваш файл `group_vars/webserver/vars.yml`.

5. **Запуск playbook**:
   ```bash
   ansible-playbook -i hosts.ini wordpress-playbook.yml --ask-vault-pass
   ```

## Структура проекта

- `hosts.ini`: Файл с инвентаризацией.
- `wordpress-playbook.yml`: Главный playbook.
- `group_vars/`: Директория с групповыми переменными.
- `roles/`: Директория с ролями.

## Обратная связь и вклад

Если у вас есть предложения или замечания к этому проекту, создайте issue или pull request в репозитории.
