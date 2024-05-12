# Модель прецедентів

## Загальна схема

<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>

@startuml

actor "Адміністратор" as Admin
actor "Відвідувач" as Guest
actor "Користувач" as User

usecase "Управління користувачами" as UserManager
usecase "Підтримка системи" as SysAdministration
usecase "Управління контентом" as ContentManager
usecase "log in \n Увійти" as Login
usecase "Sign up \n Зареєструватися" as Register
usecase "Управління аккаунтом" as AccountManager
usecase "Аналіз медіаконтенту" as AnalysContent
usecase "Завантаження контенту" as ContentUpload
usecase " Оновлення контенту" as ContentUpdate
usecase "Видалення контенту" as ContentDelete

Admin -u-> UserManager
Admin -u-> SysAdministration
Admin -u-> ContentManager
Admin --> ContentUpload
Admin --> ContentUpdate
Admin --> ContentDelete

User --> AccountManager
User --> AnalysContent

Guest -u->Login
Guest -u->Register

Admin -l-|> User
User -l-|> Guest

@enduml

</center>

## Схема для Відвідувача

<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>

@startuml

actor "Відвідувач" as Guest

usecase "log in \n Увійти" as Login
usecase "Sign in \n Зареєструватися" as Register

Guest -d->Login
Guest -d->Register

@enduml

</center>

## Схема для Користувача

<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>

@startuml

actor "Користувач" as User

usecase "Управління обліковим записом" as AccountManager

usecase "Управління аккаунтом" as AccountEditor
usecase "Звернення до службу підтримки" as Support
usecase "Перегляд історії своїх запитів" as History

usecase "Аналіз медіа-контенту" as AnalysContent

usecase "Настройка фільтрів" as FilterManager
usecase "Порівняння результатів аналізу" as AnalisComparison
usecase "Створення та надсилання запитів" as SendingRequests
usecase "Експорт звітів" as ExportReports
usecase "Генерація звітів" as ReportGeneration

User -r-> AccountManager
User -l-> AnalysContent

AccountManager <-r. Support :<<extends>>
AccountManager <-u. AccountEditor :<<extends>>
AccountManager <-d. History :<<extends>>

AnalysContent <-l. FilterManager :<<extends>>
AnalysContent <-u. AnalisComparison :<<extends>>
AnalysContent <-d. SendingRequests :<<extends>>
AnalysContent <-u. ExportReports :<<extends>>
AnalysContent <-d. ReportGeneration :<<extends>>

@enduml

</center>

## Схема для Адміністратора

<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>

@startuml

actor "Адміністратор" as Admin

usecase "Управління користувачами" as UserManager
usecase "Налаштуваннями доступу" as AccessSettings
usecase "Підтримка Користувача" as UserSupport


usecase "Підтримка системи" as SystemSupport
usecase "Управління базами даних" as DatabaseManagement
usecase "Моніторинг продуктивності" as PerformanceMonitoring
usecase "Aдміністрування ресурсів" as ResourceAdministration


usecase "Управління контентом" as ContentManager
usecase "Захист конфіденційності" as PrivacyProtection
usecase "Конфігурація параметрів" as ConfigurationofParameters


Admin -d-> UserManager
Admin -d-> SystemSupport
Admin -d-> ContentManager


UserManager <.. UserSupport :<<extends>>
UserManager <.. AccessSettings :<<extends>>

SystemSupport <.. DatabaseManagement :<<extends>>
SystemSupport <.. PerformanceMonitoring :<<extends>>
SystemSupport <.. ResourceAdministration :<<extends>>

ContentManager <.. PrivacyProtection :<<extends>>
ContentManager <.. ConfigurationofParameters :<<extends>>

@enduml

</center>

## Сценарії використання

### Відвідувач

#### Реєстрація

| **_ID:_**                | user_register                        |
| ------------------------ | -----------------------------------------------------------------------------------------------------------|
| **_НАЗВА:_**             | Реєстрація користувача               |
| **_УЧАСНИКИ:_**          | Користувач, система                  |
| **_ПЕРЕДУМОВИ:_**        | Користувач не має облікового запису в системі                                                           |
| **_РЕЗУЛЬТАТ:_**         | Користувач має акаунт                |
| **_ВИКЛЮЧНІ СИТУАЦІЇ:_** | user_exists - Обліковий запис з такими даними вже існує<br /> wrong_credentials - Неправильно введені дані                                                              |

@startuml
|Відвідувач|
    start
    :Натискає кнопку реєстрації нового облікового запису;
|Система|
    :Надає форму для заповнення;
|Відвідувач|
    :Заповнює форму для реєстрації;
    :Заповнює поля: логін, адреса електронної пошти, пароль;
    :Натискає кнопку підтвердження реєстрації;
|Система|
    :Перевіряє дані з форми;
        note right #FF736E
            user_exists
            wrong_credentials
        end note

    :Створює новий обліковий запис;

|Відвідувач|
    stop;
@enduml

### Користувач

#### Авторизація

| **_ID:_**                | user_login                           |
| ------------------------ | -----------------------------------------------------------------------------------------------------------|
| **_НАЗВА:_**             | Авторизація користувача              |
| **_УЧАСНИКИ:_**          | Користувач, система                  |
| **_ПЕРЕДУМОВИ:_**        | Користувач вже має обліковий запис в системі                                                           |
| **_РЕЗУЛЬТАТ:_**         | Користувач авторизований у системі   |
| **_ВИКЛЮЧНІ СИТУАЦІЇ:_** | wrong_password - Введений хибний пароль<br /> no_user - Користувач не зареєстрований у системі                                                           |

@startuml
|Користувач|
    start
    :Натискає кнопку входу до облікового запису;
|Система|
    :Надає форму для заповнення;
|Користувач|
    :Отримує форму для авторизації;
    :Заповнює поля:\nлогін або адреса електронної пошти, пароль;
    :Натискає кнопку підтвердження авторизації;
|Система|
    :Перевіряє дані з форми;
        note right #FF736E
            wrong_password
            no_user
        end note
    :Підтверджує виконання;
|Користувач|
    stop;
@enduml

#### Аналіз медіа-контенту
| **ID:**                | media_analysis                           |
|------------------------|----------------------------------------------------------------------------------------------------------------|
| **НАЗВА:**             | Аналіз медіа-контенту                  |   
| **УЧАСНИКИ:**          | Користувач, Система                    |
| **ПЕРЕДУМОВИ:**        | Користувач має доступ до медіа-контенту в системі                                                           |
| **РЕЗУЛЬТАТ:**         | Користувач отримує аналізовану інформацію про медіа-контенту                                                |
| **ВИКЛЮЧНІ СИТУАЦІЇ:** | no_content - За запитом нічого не знайдено|

@startuml
|Користувач|
    start
    :Користувач надсилає запит на аналіз медіа-контенту до системи;
|Система|
    :Система перевіряє наявність медіа-контенту для аналізу;
        note right #FF736E
            no_content
        end note
    :Система аналізує медіа-контент, використовуючи відповідні алгоритми та методи;
    :Генерує звіт аналізу і надсилає користувачеві;
|Користувач|
    :Користувач отримує звіт аналізу медіа-контенту від системи;
    stop;
@enduml

#### Моніторинг трендів
| **ID:**                | trend_monitoring                         |
|------------------------|----------------------------------------------------------------------------------------------------------------|
| **НАЗВА:**             | Моніторинг трендів                       |
| **УЧАСНИКИ:**          | Користувач, Система                      |
| **ПЕРЕДУМОВИ:**        | Користувач має інтерес до відстеження трендів у медіа-просторі                                            |
| **РЕЗУЛЬТАТ:**         | Користувач отримує інформацію про актуальні медіа-тренди                                              |
| **ВИКЛЮЧНІ СИТУАЦІЇ:** | no_trends Немає актуальних трендів       |

@startuml
|Користувач|
    start
    :Користувач надсилає запит на моніторинг трендів до системи;
|Система|
    :Система аналізує дані з різних джерел для виявлення поточних трендів;
            note right #FF736E
            no_trends
        end note
    :Генерує звіт аналізу і надсилає користувачеві;
|Користувач|
    :Користувач отримує звіт з виявленими трендами від системи;
    stop;
@enduml