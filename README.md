# Описание работы

Комментарии:
1.1 Добавлен контекст, показаны домены.
1.2 Исправлена диаграма контейнеров.
1.3 Для пункта 1.3 добавлено описание таблиц.

1. Первая часть работы находится в ./diagrams/
  1.1 Контекст системы ./diagrams/context/SmartHome_Context.puml
  1.2 Описание контэйнеров системы ./diagrams/container/SmartHome_Container.puml
  1.3 Описание компонентов системы ./diagrams/components/SmartHome_Components.puml
      У пользователя может быть несколько домов (User -> House).
      В доме может быть множенство устройств, множество модулей.
        Модуль - набор устройств, например несколько ламп объединенных в группу. (Module)
      Одному устройству соответсвует один тип. (Device -> Device type)
      У устройства множество записей показаний. (Device - Telemetry)
      Так же в доме может быть множество сценариев.
      Одному сценарию одно устройство или один модуль.
  1.4 Основные сущности ./diagrams/erd/SmartHome_ERD.puml

2. Вторая часть выполнена приблизительно на треть. (Основная цель была попробовать k8s и helm и большую часть времени потратил на них).
  2.1 В сервисе DeviceManagement не реализованы методы в полном объеме, только подписка и отправка сообщений в Kafka.
  Споткнулся на подключении сервисом к БД(postgres) в k8s (видимо нужно добавить секреты).
  2.2 Добавлен docker-compose.
  
  [Ссылка на сервис] (https://github.com/VisTzz/device-management)



# Базовая настройка

## Запуск minikube

[Инструкция по установке](https://minikube.sigs.k8s.io/docs/start/)

```bash
minikube start
```


## Добавление токена авторизации GitHub

[Получение токена](https://github.com/settings/tokens/new)

```bash
kubectl create secret docker-registry ghcr --docker-server=https://ghcr.io --docker-username=<github_username> --docker-password=<github_token> -n default
```


## Установка API GW kusk

[Install Kusk CLI](https://docs.kusk.io/getting-started/install-kusk-cli)

```bash
kusk cluster install
```


## Настройка terraform

[Установите Terraform](https://yandex.cloud/ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform)


Создайте файл ~/.terraformrc

```hcl
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

## Применяем terraform конфигурацию 

```bash
cd terraform
terraform apply
```

## Настройка API GW

```bash
kusk deploy -i api.yaml
```

## Проверяем работоспособность

```bash
kubectl port-forward svc/kusk-gateway-envoy-fleet -n kusk-system 8080:80
curl localhost:8080/hello
```


## Delete minikube

```bash
minikube delete
```