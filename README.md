# Тестовое задание для кандидатов на позицию junior backend-developer
<a href="https://ibb.co/WndtCqG"><img src="https://i.ibb.co/H7MdZ1B/image.jpg" alt="image" border="0"></a><br /><a target='_blank' href='https://ru.imgbb.com/'></a>
Данное тестовое задание предполагает создание нескольких таблиц в БД Sqlite3, моделей для работы с таблицам и апи для получения и создания записей в таблице, а именно:

1) Настройка подключения к базе `db.sqlite` в `src\db\connection.php`
2) Создание таблиц `users` и `post` в БД в `scr\db\initial.php`
3) Создание класса модели User в `src\Models\User.php` с указанными в задании методами и свойствами
4) Создание класса модели Post в `src\Models\Post.php` с указанными в задании методами и свойствами
5) Настройка endpoint для апи по урлам `PUT /api/users` `GET /api/users/:id` выполняющими указанные в задании процессы

## Шаги для прохождения теста

### Подготовка

1) Создайте свой пустой репозиторий на github (без форка этого репозитория)
2) Скачайте этот репозиторий как zip-архив и распакуйте
3) Закоммитьте распакованные файлы в свой пустой репозиторий как `test begin`

### Зависимости

Нужно убедиться что в php.ini включены нужные расширения: mbstring, curl и sqlite3

Установить composer локально или глобально

Установить зависимости: `php composer.phar install` (`composer install`)

Проверка будет производиться на версии PHP 8.0.15

### Настройте код для работы с БД

В файле `src\db\connection.php` должно быть подключение к БД sqlite

В файле `scr\db\initial.php` должно быть создание начальных таблиц для работы кода (ссылка на подключение с предыдущего шага будет в аргументе функции):

`users`

1) id - число, уникальный, авто-инкремент
2) email - строка
3) first_name - строка
4) last_name - строка
5) password - строка
6) created_at - число

`post`

1) id - число, уникальный, авто-инкремент
2) title - строка
3) body - строка
4) creator_id - связь с user.id
5) created_at - число

Выполните команду `php composer.phar bootstrap` (`composer bootstrap`) для выполнения скрипта поднятия таблиц из `scr\db\initial.php`

#### Проверка БД

Нужно убедиться что база данных создана верно, через программу для просмотра sqlite баз данных

### Создайте модели

В папке `src\Models` должны быть созданы классы User и Post отвечающие за соответствующие таблицы в БД

Экземпляр класса каждой модели должен хранить соответствующие поля как свойства класса, и метод `save` для сохранения в таблицу

Класс каждой модели должен иметь статический метод `findOne(int | null $id = null)` с необязательным первым аргументом $id, который по переданному $id возвращает запись в соответствующей таблице как экземпляр класса, если $id не передан возвращает последнюю (по ID) запись

При создании записи в таблицах, должен указываться `created_at` = текущий unix-timestamp

После записи в таблицу, в экземпляре класса должен установиться ID из БД (полученный авто-инкрементом)

Получить текущее подключение к БД из `src\db\connection.php` в коде можно через `\App\db\DB::getInstance()->getConnection()`;

#### Проверка моделей

Проверьте ваш код командой `php composer.phar test:db` (`composer test:db`)

### Создание API

В файле `src\api` есть класс `Api` и метод `connection`, нужно дописать класс и метод чтобы принимать следующие запросы:

#### Получение user по ID

`GET /api/users/:id` - где `:id` это число, указывающие ID из таблицы users, на этот запрос должен вернуться ответ в формате JSON с полями из таблицы users по полученному ID, пример:

```text
//Request:
GET /api/users/1

//Response:
{
    "id": 1,
    "email": "some-email@mail.com",
    "first_name": "SomeName",
    "last_name": "SomeLastName",
    "password": "SomePassword",
    "created_at": 1659633384,
}
```

#### Создание user

```text
PUT /api/users

{
    "email": :email,
    "first_name": :first_name,
    "last_name": :last_name,
    "password": :password,
}
```

- где `:email`, `:first_name`, `:last_name` и `:password` это поля в таблице users для создания в ней записи, на этот запрос должен вернуться ответ в формате JSON с полями из таблицы users по полученной после создания записи, пример:

```text
//Request:
PUT /api/users

{
    "email": "api-email@mail.com",
    "first_name": "ApiName",
    "last_name": "ApiLastName",
    "password": "ApiPassword",
}

//Response:
{
    "id": 23,
    "email": "api-email@mail.com",
    "first_name": "ApiName",
    "last_name": "ApiLastName",
    "password": "ApiPassword",
    "created_at": 1659633584,
]
```

#### Проверка api

Перед проверкой кода, запустите локальный сервер командой: `php composer.phar serve` (`composer serve`)

Проверьте ваш код командой `php composer.phar test:api` (`composer test:api`)

### Запустите тест

Перед проверкой кода, запустите локальный сервер командой: `php composer.phar serve` (`composer serve`)

Выполните команду `php composer.phar test` (`composer test`)

Если тест прошел успешно, закоммитьте изменения как `test passed`

Отправьте своё резюме и ссылку на ваш репозиторий с пройденым тестом на `undermuz@gmail.com`

## Вопросы и ответы

Для вопросов по существу тестового задания, или проблем с запуском автоматических тестов, воспользуйтесь [Формой создания issue](https://github.com/undermuz/junior-php-contest/issues/new)

В заголовке вопроса крато опишите проблему, в описании вопроса содержательно опишите его суть, и приложите информацию о вашем окружении, а именно:

1) Версия ОС
2) Версия PHP
3) Версия Composer
4) Способ установки composer: локально или глобально
