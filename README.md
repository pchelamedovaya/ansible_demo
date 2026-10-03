# ansible_demo

Учебный проект, чтобы попробовать Ansible

## Что внутри

| Путь                          | Зачем                                                     |
|-------------------------------|-----------------------------------------------------------|
| `shared/inventory/hosts.yaml` | Список хостов, общий для всех кейсов                      |
| `shared/inventory/host_vars/` | Параметры подключения к хостам (шаблоны `*.yaml.example`) |
| `taskNN-<название>/`          | Кейсы                                                     |

Каждый кейс самостоятельный: `ansible.cfg` внутри папки кейса указывает, где inventory и роли

> Все команды запускаются **из папки кейса**

### Хосты

- **local**: `localhost`
- **vms**: ВМ `vm1` и `vm2`, к которым Ansible подключается по SSH

## Требования

- Python 3
- Ansible: `pip install ansible` (или `sudo apt install ansible`)
- Для каждой ВМ файл `shared/inventory/host_vars/<имя_хоста>.yaml`. Файлы можно создать из шаблонов:
  ```bash
  cp shared/inventory/host_vars/<имя_хоста>.yaml.example shared/inventory/host_vars/<имя_хоста>.yaml
  ```
- SSH-доступ к каждой ВМ по ключу без пароля (настраивается один раз на машину: `ssh-copy-id <user>@<vm_ip>`)

## Быстрый старт

Проверить, что Ansible видит все хосты:

```bash
cd task00-hello_nginx
ansible-inventory --graph
```

## Кейсы

### task00-hello_nginx

Устанавливает nginx на ВМ и создает страницу, на которой написано имя хоста

Установить:

```bash
cd task00-hello_nginx
ansible-playbook playbook.yaml -K
```

Удалить:

```bash
cd task00-hello_nginx
ansible-playbook playbook.yaml -K -e hello_state=absent
```

> После установки страница доступна по адресу ВМ, например `http://<vm_ip>`

## Полезные ссылки

- [Документация Ansible](https://docs.ansible.com/)
- [Список модулей](https://docs.ansible.com/ansible/latest/collections/index_module.html)
