# ansible_demo

Учебный проект, чтобы попробовать Ansible

## Что внутри

| Файл             | Зачем                                                     |
|------------------|-----------------------------------------------------------|
| `ansible.cfg`    | Настройки Ansible: где inventory, где роли, параметры SSH |
| `inventory.yaml` | Список хостов                                             |
| `host_vars/`     | Шаблоны параметров подключения к хостам                   |

### Хосты

- **local**: `localhost`
- **vms**: виртуалки `vm1` и `vm2`, к которым Ansible подключается по SSH

## Требования

- Python 3
- Ansible: `pip install ansible` (или `sudo apt install ansible`)
- Для каждой виртуалки файл `host_vars/<имя_хоста>.yaml`. Файлы можно создать из шаблонов:
  ```bash
  cp host_vars/<имя_хоста>.yaml.example host_vars/<имя_хоста>.yaml
  ```
- SSH-доступ к каждой виртуалке по ключу без пароля (настраивается один раз на машину: `ssh-copy-id <user>@<vm_ip>`)

## Быстрый старт

Проверить, что Ansible видит все хосты:

```bash
ansible-inventory --graph
```

Пингануть все хосты

```bash
ansible all -m ping
```

Выполнить разовую команду `uptime` на всех хостах группы `vms` (показывает, сколько машина работает и ее нагрузку):

```bash
ansible vms -a "uptime"
```

## Полезные ссылки

- [Документация Ansible](https://docs.ansible.com/)
- [Список модулей](https://docs.ansible.com/ansible/latest/collections/index_module.html)
