# Инструкция по запуску плейбука Ansible

## Требования

### Сервер
- Виртуальная машина на Debian 11 с созданным пользователем для Ansible (по умолчанию: `user`), имеющим права суперпользователя, и установленным SSH-клиентом.
- IP-адрес виртуальной машины.

### Рабочее окружение
- Установленный Ansible на рабочей машине. Подробности установки можно найти [здесь](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
- Пара SSH-ключей для доступа к серверу. Если ключи отсутствуют, ознакомьтесь с инструкцией по их созданию.

## Инструкция по запуску (для MacOS и Linux)

1. **Настройка конфигурации Ansible**
   - Сгенерируйте пару SSH-ключей (приватный и публичный):
     ```bash
     ssh-keygen
     ```
   - Скопируйте публичный ключ на удалённый сервер:
     ```bash
     ssh-copy-id <user>@<server_ip>
     ```
   - Для выполнения инструкций с повышенными привилегиями, отключите запрос пароля для `sudo`:
     ```bash
     echo "<user> ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/ansible
     ```

2. **Подготовка и запуск Ansible плейбука**
   1. Клонируйте репозиторий с Ansible плейбуком на вашу локальную машину и перейдите в его директорию:
      ```bash
      git clone <repository_url>
      cd <repository_directory>
      ```
   2. Настройте файл Inventory:
      - Отредактируйте файл `./inventory/hosts`:
        - Замените `<server_ip>` на IP-адрес вашего сервера.
        - Замените `<ansible_ssh_private_key_file>` на путь к вашему приватному ключу.
   3. Настройте файл `ansible.cfg`:
      - Отредактируйте файл, заменив `<user>` на имя созданного вами пользователя на сервере.

3. **Проверка доступа к хосту**
   - Для проверки доступа используйте команду:
     ```bash
     ansible host1 -m ping
     ```

4. **Запуск плейбука Ansible**
   - Запустите плейбук с помощью команды:
     ```bash
     ansible-playbook -i ./inventory/hosts ./playbooks/main.yml
     ```
     где `./inventory/hosts` — путь до файла с указанием хостов, а `./playbooks/main.yml` — путь до плейбука.

## P.S.
SSH-ключи для задания хранятся в директории `./playbooks/keys.yml` в незашифрованном виде для простоты доступа. Однако лучшей практикой является хранение подобных секретов в зашифрованном виде с использованием `ansible-vault`.
