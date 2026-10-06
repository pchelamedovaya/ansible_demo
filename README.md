# ansible_demo

Учебный проект, чтобы попробовать Ansible

## Что внутри

| Путь                    | Зачем                                  |
|-------------------------|----------------------------------------|
| `ansible.cfg`           | Настройки Ansible                      |
| `inventory/hosts.yaml`  | Список хостов                          |
| `inventory/host_vars/`  | Параметры подключения к хостам         |
| `inventory/group_vars/` | Переменные групп и секреты (Vault)     |
| `roles/`                | Роли                                   |
| `playbooks/`            | Кейсы                                  |
| `requirements.yaml`     | Коллекции Ansible, которые нужны ролям |

> Все команды запускаются **из корня проекта**

## Требования

- Python 3
- Ansible: `pip install ansible` (или `sudo apt install ansible`)
- ОС на ВМ: Ubuntu или Debian, у пользователя на ВМ есть sudo, у ВМ есть доступ в интернет
- Для каждой ВМ файл `inventory/host_vars/<имя_хоста>.yaml`. Файлы можно создать из шаблонов:
  ```bash
  cp inventory/host_vars/<имя_хоста>.yaml.example inventory/host_vars/<имя_хоста>.yaml
  ```
- SSH-доступ к каждой ВМ по ключу без пароля (настраивается один раз на машину: `ssh-copy-id <user>@<vm_ip>`)
- Коллекции Ansible:
  ```bash
  ansible-galaxy collection install -r requirements.yaml
  ```

## Пароли при запуске

Плейбук запрашивает два пароля:

| Флаг               | Что спрашивает                 |
|--------------------|--------------------------------|
| `-K`               | пароль sudo пользователя на ВМ |
| `--ask-vault-pass` | пароль от Ansible Vault        |

## memos

Разворачивает сервис заметок [Memos](https://github.com/usememos/memos) в Docker на двух ВМ: база на `vm1`, приложение
на `vm2`

| Хост  | Роли                   | Что делает                                                             |
|-------|------------------------|------------------------------------------------------------------------|
| `vm1` | `docker`, `postgresql` | Ставит Docker, запускает контейнер PostgreSQL                          |
| `vm2` | `docker`, `memos`      | Ставит Docker, запускает контейнер Memos, подключенный к базе на `vm1` |

Пароль базы лежит в `inventory/group_vars/memos/vault.yaml`, зашифрованном через Ansible Vault

Если вы склонировали проект, создайте свой файл с переменной `vault_memos_db_password`:

```bash
rm inventory/group_vars/memos/vault.yaml
ansible-vault create inventory/group_vars/memos/vault.yaml
```

Запуск:

```bash
ansible-playbook playbooks/memos.yaml -K --ask-vault-pass
```

> После запуска Memos доступен по адресу `http://<vm2_ip>:5230`

## Полезные ссылки

- [Документация Ansible](https://docs.ansible.com/)
- [Список модулей](https://docs.ansible.com/ansible/latest/collections/index_module.html)
