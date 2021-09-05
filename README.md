# Паттерны декомпозиции микросервисов

- [Паттерны декомпозиции микросервисов](#паттерны-декомпозиции-микросервисов)
  - [Пользовательские сценарии](#пользовательские-сценарии)
    - [Регистрация](#регистрация)
    - [Аутентификация](#аутентификация)
    - [Создание партнерских ссылок](#создание-партнерских-ссылок)
    - [Просмотр партнерской статистики](#просмотр-партнерской-статистики)
    - [Реализация revenue share](#реализация-revenue-share)
    - [Создание postback'ов](#создание-postbackов)
  - [Общая схема взаимодействия микросервисов](#общая-схема-взаимодействия-микросервисов)
  - [Зоны ответственности и назначение микросервисов](#зоны-ответственности-и-назначение-микросервисов)
  - [Контракты](#контракты)
    - [REST API](#rest-api)
    - [Async API](#async-api)

## Пользовательские сценарии

### Регистрация

```gherkin
Scenario: Потенциальный партнер может зарегистрироваться на платформе Affo
    Given Я потенциальный партнер на странице регистрации
    When Я ввожу необходимую информацию в форму регистрации
		|email|b.adams@example.com|
		|phone|+1234567890|
		|first_name|Adams|
		|last_name|Brain|
		And Нажимаю кнопку Register
    Then Я успешно заргистрировался
	And Я перехожу в ЛК партнера
```

### Аутентификация

```gherkin
Scenario: Партнер может аутентифицироваться и перейти в ЛК партнера
    Given Я партнер на странице аутентификации
    When Я ввожу необходимую информацию в форму аутентификации
		|email|b.adams@example.com|
		|password|1234|
		And Нажимаю кнопку Login
    Then Я успешно аутентифицировался
	And Я перехожу в ЛК партнера
```

### Создание партнерских ссылок

```gherkin
Scenario: Партнер может создать партнерскую ссылку и "зашить" в нее параметры для трекинга клиента
    Given Я партнер на странице генерации партнерской ссылки
    When Я ввожу ссылку в поле Create your partner URL
		|url|https://www.lilachbullock.com|
		And Нажимаю кнопку Create
    Then Я вижу на экране короткую партнерскую ссылку, которую могу использовать для привлечения трафика
```

### Просмотр партнерской статистики

```gherkin
Scenario Outline: Партнер может посмотреть статистику по привлеченному трафику
    Given Я партнер на странице статистики
    When <Происходит связанное с моими клиентами событие>
    Then Я вижу изменения в статистике по привлеченному трафику

Examples:
    | Происходит связанное с привлеченными мной клиентами событие |
    | Клиент перешел по партнерской ссылке                        |
    | Бренд прислал событие, связанное с моим клиентом            |
```

### Реализация revenue share

```gherkin
Scenario: Интернет-магазин может сообщить о вознаграждении за совершенный привлеченным клиентом заказ
    Given Я партнер
		And я привлек клиента в интернет-магазин
    When Клиент оплачивает заказ
    Then Интернет-магазин отправляет событие с суммой вознаграждения платформе Affo
```

### Создание postback'ов

```gherkin
Scenario: Партнер может создать партнерскую ссылку и "зашить" в нее параметры для трекинга клиента
    Given Я партнер на странице генерации партнерской ссылки
    When Я ввожу ссылку в поле Create your partner URL
		|url|https://www.lilachbullock.com|
		And Нажимаю кнопку Create
    Then Я вижу на экране короткую партнерскую ссылку, которую могу использовать для привлечения трафика
```

## Общая схема взаимодействия микросервисов

<image src="./images/ms_communication_diagram.svg" width="100%">

## Зоны ответственности и назначение микросервисов

| Микросервис    | Зона ответственности | Назначение                                                                   |
| :------------- | :------------------- | :--------------------------------------------------------------------------- |
| **auth**       | Аутентификация       | Генерация и проверка токенов аутентификации                                  |
| **users**      | Пользователи         | Регистрация пользователя, получение профиля пользователя                     |
| **billing**    | Счета пользователей  | Создание счета пользователя, проведение транзакций по счету                  |
| **deeplinks**  | Партнерские ссылки   | Создание партнерких ссылок и переходы по ним                                 |
| **events**     | События              | Хранение и обработка событий                                                 |
| **postbacks**  | Сторонние системы    | Уведомление сторонних систем о событиях, связанных с привлеченными клиентами |
| **statistics** | Статистика           | Получение сводной статистики по партнерскому трафику                         |

## Контракты

### REST API

* [auth](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/auth.yaml)
* [users](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/users.yaml)
* [billing](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/billing.yaml)
* [deeplinks](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/deeplinks.yaml)
* [events](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/events.yaml)
* [postbacks](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/postbacks.yaml)
* [statistics](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/adalekin/otusdecomposition/master/specs/openapi/statistics.yaml)

### Async API

* [events](./specs/asyncapi/events.yaml)
