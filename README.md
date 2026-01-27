# HA Infra Template

Этот проект демонстрирует настройку высокодоступной (High Availability) инфраструктуры с использованием Docker, Ansible, Nginx, Keepalived и простого бэкенд-приложения.

## Архитектура

- **2 балансировщика нагрузки (lb1, lb2):** Nginx + Keepalived для обеспечения отказоустойчивости.
- **2 бэкенд-сервера (backend1, backend2):** Apache для обслуживания контента.
- **1 узел управления Ansible (ansible):** контейнер для запуска плейбуков.
- **Мониторинг (victoria-metrics, grafana):** Grafana и Victoria Metrics для сбора и визуализации метрик.

## Начало работы

### 1. Подготовка

1.  **Клонируйте репозиторий:**
    ```bash
    git clone https://github.com/railgorail/ha-infra-template.git
    cd ha-infra-template
    ```

2.  **Сгенерируйте SSH-ключи:**
    Ключи необходимы для подключения Ansible к контейнерам по SSH.
    ```bash
    ssh-keygen -t ed25519 -f ./.ssh/id_ed25519 -N ""
    ```

### 2. Запуск окружения

Используйте Docker Compose для сборки и запуска всех контейнеров в фоновом режиме.

```bash
docker compose up --build -d
```

### 3. Настройка через Ansible

1.  **Войдите в контейнер `ansible`:**
    ```bash
    docker exec -it ansible /bin/bash
    ```

2.  **Перейдите в рабочую директорию:**
    Внутри контейнера файлы проекта находятся в `~/ansible`.
    ```bash
    cd ./ansible
    ```

3.  **Запустите плейбуки Ansible:**
    Выполняйте плейбуки в указанном порядке для корректной настройки инфраструктуры.
    ```bash
    ansible-playbook playbook_lb.yaml && \
    ansible-playbook playbook_ha.yaml && \
    ansible-playbook playbook_backend.yaml && \
    ansible-playbook playbook_monitoring.yaml
    ```

## Проверка работоспособности

### Балансировка и HA

Проверьте, что балансировщик распределяет запросы между бэкенд-серверами. 
Плейбук `debug.yaml` содержит инструкции и проверки для отладки кластера. **(Обязательно прочитать!)**
(Запускать внутри контейнера ansible)

```bash
ansible-playbook debug.yaml
```

### Мониторинг

1.  Откройте Grafana в браузере: [http://localhost:3000](http://localhost:3000) (или IP вашего хоста).
2.  Войдите, используя учетные данные по умолчанию:
    -   **Логин:** `admin`
    -   **Пароль:** `admin`
3.  Исследуйте дашборды для просмотра метрик.

# WIP

Кастомные дашборды