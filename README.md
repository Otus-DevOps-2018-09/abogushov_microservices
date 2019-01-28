# Репозитарий для микросервисов

## Содержание

- [Домашняя работа 13](#домашняя-работа-13)
- [Домашняя работа 14](#домашняя-работа-14)

## Домашняя работа 14

- Создан новый проект для docker в GCE.
- Выполнена авторизация в GCE.
- Создан docker-host через docker-machine.
- Проработано демо PID namespace, net namespace, user namespaces.
- Сборка монолитного образа с приложением.
- Деплой приложения на удаленный хост.


**Необходимо активировать доступ через GCE API для вновь созданного проекта.**

```bash
export GOOGLE_PROJECT=<project_id>
docker-machine create --driver google \
    --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
    --google-machine-type n1-standard-1 \
    --google-zone europe-west1-b \
    docker-host
```

Активируем созданный докер:

```bash
eval $(docker-machine env docker-host)
```

Запуск docker-in-docker:

```bash
docker run --privileged -d docker:dind
```

Сборка докер образа с приложением и БД:

```bash
docker build -t reddit:latest .
```

Запуск образа с приложением:

```bash
docker run --name reddit -d --network=host reddit:latest
```

```bash
gcloud compute firewall-rules create reddit-app \
    --allow tcp:9292 \
    --target-tags=docker-machine \
    --description="Allow PUMA connections" \
    --direction=INGRESS
```


## Домашняя работа 13

- Установлен докер.
- Были выполнены запуски различных команд докера ps, run, start , stop, attach, exec, commit, inspect.
- Выполнено сравнение вывода команды inspect для образа и контейнера.
- Выполнена команды по удалению образов и контейнеров. 
