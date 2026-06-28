# JMeter Load Test for Shop API

Нагрузочный тест API интернет-магазина, выполненный в Apache JMeter в рамках тестового задания.

## Тестируемые эндпоинты

```text
GET  /api/products?page=1&size=20
GET  /api/products/{id}
POST /api/orders
GET  /api/orders/{id}
```

## Сценарий

Каждый виртуальный пользователь выполняет следующие шаги:

1. Получает список товаров: `GET /api/products?page=1&size=20`
2. Выбирает случайный товар из ответа.
3. Получает детали товара: `GET /api/products/{id}`
4. Создаёт заказ: `POST /api/orders`
5. Проверяет статус заказа: `GET /api/orders/{id}`

Для данных пользователя используется `CSV Data Set Config` и файл `users.csv`.

## Профиль нагрузки

```text
Количество пользователей: 100
Ramp-up period: 60 секунд
Количество циклов: 10
```

Итого:

```text
100 пользователей × 10 циклов = 1000 пользовательских сценариев
1000 сценариев × 4 запроса = около 4000 HTTP-запросов
```

## Файлы проекта

```text
load_test.jmx                  JMeter-сценарий
users.csv                      Тестовые пользовательские данные
internet-magazin_api.1.0.0.yml OpenAPI-спецификация
README.md                      Описание проекта
```

## Используемые элементы JMeter

```text
Thread Group
HTTP Request Defaults
HTTP Header Manager
CSV Data Set Config
HTTP Request
JSON Extractor
Response Assertion
View Results Tree
```

## Проверки

В сценарии проверяются HTTP-коды ответов:

```text
GET  /api/products       - 200
GET  /api/products/{id}  - 200
POST /api/orders         - 201
GET  /api/orders/{id}    - 200
```

Также используются JSON Extractor:

```text
productId — извлекается из ответа GET /api/products
orderId   — извлекается из ответа POST /api/orders
```

## Запуск

Запуск через JMeter GUI:

1. Открыть Apache JMeter.
2. Открыть `load_test.jmx`.
3. Убедиться, что `users.csv` находится рядом с `.jmx`.
4. Запустить тест.
5. Посмотреть результаты в `Aggregate Report`.

Запуск из командной строки:

```cmd
jmeter.bat -n -t load_test.jmx -l results/result.jtl -e -o results/html-report
```
