# ansible_demo

Учебный проект, чтобы попробовать Ansible

## Что внутри

| Файл                   | Зачем                                                     |
|------------------------|-----------------------------------------------------------|
| `ansible.cfg`          | Настройки Ansible: где inventory, где роли, параметры SSH |
| `inventory/hosts.yaml` | Список хостов                                             |
| `inventory/host_vars/` | Шаблоны параметров подключения к хостам                   |
| `playbooks/`           | Playbook'и                                                |

### Хосты

- **local**: `localhost`
- **vms**: ВМ `vm1` и `vm2`, к которым Ansible подключается по SSH

## Требования

- Python 3
- Ansible: `pip install ansible` (или `sudo apt install ansible`)
- Для каждой ВМ файл `inventory/host_vars/<имя_хоста>.yaml`. Файлы можно создать из шаблонов:
  ```bash
  cp inventory/host_vars/<имя_хоста>.yaml.example inventory/host_vars/<имя_хоста>.yaml
  ```
- SSH-доступ к каждой ВМ по ключу без пароля (настраивается один раз на машину: `ssh-copy-id <user>@<vm_ip>`)

## Быстрый старт

Проверить, что Ansible видит все хосты:

```bash
ansible-inventory --graph
```

>Все команды запускаются из корня проекта

## Playbook'и

### hello_nginx

Устанавливает nginx на ВМ и создает страницу, на которой написано имя хоста

Установить:

```bash
ansible-playbook playbooks/hello_nginx.yaml -K
```

Удалить:

```bash
ansible-playbook playbooks/hello_nginx.yaml -K -e hello_state=absent
```

> После установки страница доступна по адресу ВМ, например `http://<vm_ip>`

## Полезные ссылки

- [Документация Ansible](https://docs.ansible.com/)
- [Список модулей](https://docs.ansible.com/ansible/latest/collections/index_module.html)
