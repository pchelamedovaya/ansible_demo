# ansible_demo

Учебный проект, чтобы попробовать Ansible

## Что внутри

| Путь                    | Зачем                                  |
|-------------------------|----------------------------------------|
| `ansible.cfg`           | Настройки Ansible                      |
| `inventory/hosts.yaml`  | Список хостов                          |
| `inventory/host_vars/`  | IP-адреса хостов                       |
| `inventory/group_vars/` | Переменные групп и секреты (Vault)     |
| `roles/`                | Роли                                   |
| `playbooks/`            | Кейсы                                  |
| `requirements.yaml`     | Коллекции Ansible, которые нужны ролям |

> Все команды запускаются **из корня проекта**

## Требования

- Python 3
- Ansible
- sshpass на управляющей машине (нужен только для bootstrap)
- SSH-ключ `~/.ssh/id_ed25519` на управляющей машине. Если его нет:
  ```bash
  ssh-keygen -t ed25519
  ```
- ОС на ВМ: Ubuntu с SSH-сервером, пользователь с паролем и sudo, у ВМ есть доступ в интернет
- Для каждой ВМ файл `inventory/host_vars/<имя_хоста>.yaml`. Файлы можно создать из шаблонов:
  ```bash
  cp inventory/host_vars/<имя_хоста>.yaml.example inventory/host_vars/<имя_хоста>.yaml
  ```
- Коллекции Ansible:
  ```bash
  ansible-galaxy collection install -r requirements.yaml
  ```

## bootstrap

Готовит ВМ к работе с Ansible. Запускается один раз на новых ВМ:

```bash
ansible-playbook playbooks/bootstrap.yaml -e bootstrap_user=<user> -k -K --ask-vault-pass
```

После bootstrap все плейбуки работают под `ansible` по ключу, пароль sudo не нужен

Вход по паролю после bootstrap отключен, поэтому повторный запуск — под `ansible`:

```bash
ansible-playbook playbooks/bootstrap.yaml -e bootstrap_user=ansible --ask-vault-pass
```

## memos

Разворачивает сервис заметок [Memos](https://github.com/usememos/memos): база на `vm1`, по копии приложения на `vm2` и
`vm3`

| Хост         | Роли                   | Что делает                                                             |
|--------------|------------------------|------------------------------------------------------------------------|
| `vm1`        | `docker`, `postgresql` | Ставит Docker, запускает контейнер PostgreSQL                          |
| `vm2`, `vm3` | `docker`, `memos`      | Ставит Docker, запускает контейнер Memos, подключенный к базе на `vm1` |

Пароль базы лежит в `inventory/group_vars/memos/vault.yaml`, зашифрованном через Ansible Vault

Если вы склонировали проект, создайте свой файл с переменной `vault_memos_db_password`:

```bash
rm inventory/group_vars/memos/vault.yaml
ansible-vault create inventory/group_vars/memos/vault.yaml
```

Запуск:

```bash
ansible-playbook playbooks/memos.yaml --ask-vault-pass
```

> После запуска Memos доступен по адресам `http://<vm2_ip>:5230` и `http://<vm3_ip>:5230`

## Полезные ссылки

- [Документация Ansible](https://docs.ansible.com/)
- [Список модулей](https://docs.ansible.com/ansible/latest/collections/index_module.html)
