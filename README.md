# Нагрузочное тестирование

## Вопросы

1. [Основы тестирования](#основы-тестирования) \
   1.1 [Требования](#требования) \
   1.2 [Дефекты](#дефекты) \
   1.3 [Виды тестирования](#виды-тестирования)
2. [Методика нагрузочного тестирования](#методика-нагрузочного-тестирования) \
  2.1 [Назначение, почему это важно](#назначение-почему-это-важно) \
  2.2 [Основные разделы](#основные-разделы)
3. [Введение в методологию нагрузочного тестирования](#введение-в-методологию-нагрузочного-тестирования) \
  3.1 [Типовой план работ по НТ](#) \
  3.2 [Цели НТ](#цели-нт) \
  3.3 [Виды НТ](#виды-нагрузочного-тестирования) \
  3.4 [Поиск максимальной производительности](#поиск-максимальной-производительности) \
   Проверка стабильности
   Стресс тесты
   Проверка стрессоустойчивости
   Проверка отказоустойчивости
   Проверка масштабируемости
   Объемное тестирование

4. [Service Level Agreement](#service-level-agreement)

5. [Расчет профиля нагрузочного тестирования](#расчет-профиля-нагрузочного-тестирования)
  5.1 На основе аналитики
  5.2 На основе статистики (по ЧПН)
  5.3 [Точность профиля](#точность-профиля)
  5.4 [Случай с несколькими профилями (например, дневной и ночной)](#случай-с-несколькими-профилями-например-дневной-и-ночной)

6. [Анализ архитектуры систем](#анализ-архитектуры-систем) \
  6.1 Промышленный и тестовый стенды \
  6.2 Важность их сравнения \
  6.3 [Источники нагрузки](#источники-нагрузки) \
  6.4 [Смежные системы](#смежные-системы) \
  6.4 [Заглушки](#заглушки) \
  6.5 [Протоколы сетевые](#протоколы-сетевые) \
    6.5.1 [Стек (TCP/IP)](#стек-tcpip) \
    6.5.2 [НТТР. Состав запроса и ответа, основные методы, коды ответа](#http-состав-запроса-и-ответа-основные-методы-коды-ответа) \
    6.5.3 [Стандарты обмена REST и SOAP](#) \
  6.6 [Двузхвенная и трехзвенная архитектура](#) \
    6.6.1 [Толстый и тонкий клиенты](#) \
  Основные компоненты тестируемых систем
   Front, Back, DB
   Микросервисная и монолитная архитектура
   Балансировщики
   Брокеры сообщений (MQ vs Kafka)

1. JMeter
6.1. Разработка скриптов
  Параметризация
  Корреляция
  Верификация
  Настройка pacing
6.2 Выполнение тестов
  Создание теста по профилю из МНТ
  Интеграция c InfluxDB+Grafana
  Распределенный запуск теста
6.3 Разбор результатов тестов
  Основные графики
  Порядок разбора результатов

1. Обзор средств мониторинга (Prometheus+Grafana, Telegraf+InfluxDB+Grafana)
2. Метрики, собираемые во время НТ (классификация и примеры наиболее важных)

3. [Базы данных](#базы-данных)
   - [Введение в БД (классификация и примеры)](#введение-в-бд-классификация-и-примеры)
   - [Терминология и нормальные формы](#терминология-и-нормальные-формы)
   - [SQL (DDL, DML, DCL, TCL)](#sql-ddl-dml-dcl-tcl)
   - [Select, Update, Insert, Trancate, Delete](#select-update-insert-trancate-delete)
   - [Индексы (плюсы и минусы)](#индексы-плюсы-и-минусы)
   - [View vs Table](#view-vs-table)
   - [Алиасы](#алиасы)
   - [Oracle, AWR](#oracle-awr)
   - [PostgreSQL, аналоги AWR](#postgresql-аналоги-awr)
   - [План запроса (как построить и зачем)](#план-запроса-как-построить-и-зачем)

4.  Введение в заглушки
  Определение и назначение
  Плюсы и минусы использования
  Обзор инструментов и фрейворков

1.  Создание отчетов НТ
  Основные разделы и связь с МНТ

1.  [Основы программирования](#основы-программирования)
    - [Парадигмы ООП](#парадигмы-ооп)
    - [Классы и объекты](#классы-и-объекты)

2.  Самопредставление
  О себе
  О выполненных в рамках школы работах

## Основы тестирования

### Требования

__Требования__ — это конкретные условия и параметры, которым должен соответствовать программный продукт или система. Они определяют, что именно нужно протестировать, чтобы убедиться в качестве продукта.

Требования могут быть функциональными (связанными с определёнными функциями системы) и нефункциональными (касающимися производительности, безопасности, удобства использования и т. д.).

Тестирование требований позволяет проверить соответствие программного продукта установленным критериям и стандартам, а также выявить возможные проблемы и недочёты на ранних этапах разработки. Это помогает обеспечить высокое качество продукта и его успешное внедрение на рынок.

### Дефекты

__Дефект, баг__ - несовершенство или недостаток в рабочем продукте, если он не соответствует требованиям или спецификациям.

__Баг-репорт__ - это документ, описывающий ситуацию или последовательность действий приведшую к некорректной работе объекта тестирования, с указанием причин и ожидаемого результата.

Существует несколько вариантов градации дефектов. Одним из ключевых - по степени влияния на функционал системы:

- S1 блокирующий (blocked)
- S2 критический (critical)
- S3 значительный (major)
- S4 незначительный (minor)
- S5 тривиальный (trivial)

По приоритеру:

- P1 высокий (high)
- P2 средний (medium)
- P3 низкий  (low)

### Виды тестирования

Тестирование программного обеспечения можно классифицировать по различным критериям.

![Классификация тестирования](/images/image_01.jpeg)

Основные виды тестирования:

__Функциональное__ — вид тестирования, при котором проверяются функциональные требования ПО, то есть способность ПО решать возложенные на него задачи. Направлено на проверку корректности работы функциональности приложения. Например, в приложении интернет‑магазина при нажатии на кнопку "Добавить в корзину", товар добавляется в корзину пользователя.

__Автоматизированое__ — тип тестирования, при котором проверки выполняются с использованием программных средств для выполнения тестовых сценариев.
Например, тестировщик пишет код на JavaScript для автоматизации процесса авторизации на сайте.

__Нагрузочное тестирование__ - это проверка устойчивости и производительности программного обеспечения под нагрузкой, сопоставимой с реальными условиями использования.

[Дополнительная информация](https://habr.com/ru/articles/770600/)

## Методика нагрузочного тестирования

Перед тем как приступить к проверке программного продукта, нужно разработать __методику нагрузочного тестирования (МНТ)__. Это документ, в котором описан весь план тестирования, включая сценарии возможного развития проекта. МНТ полезна как заказчикам, так и команде исполнителей, потому что позволяет формализовать требования, однозначно определить ожидания от проекта.

### Назначение, почему это важно

Для заказчика - фиксирование всех договоренностей с исполнителем, чтобы он понимал, что получит на выходе.
Для специалистов НТ - МНТ является некоторой "страховкой" при общении с заказчиком. Также МНТ - это инструкция, содержащая информацию по распределению работ. Помогает с передачей знаний по проекту.

### Основные разделы

Основные разделы методики нагрузочного тестирования:

Сокращения и терминология – общий список, встречающийся по тексту.

Введение – общие слова о проведении НТ.

__Цели тестирования__ – цели НТ, которые определяют виды тестов.

__Ограничения и риски__ –  обязательно указываем существующие ограничения и возможные риски.

__Объект тестирования__ – данные раздела берутся из ТЗ на тестируемую систему.

__Тестовый стенд__ – схема с описанием тестового стенда, смежных систем, элементов для записи скриптов, моделирования нагрузки и мониторинга. Так же приводится сравнение тестового и промышленного стендов.

__Планируемые тесты__ – список панируемых тестов с детализацией способов их проведения.

__Моделирование нагрузки__ – инструменты НТ, профиль нагрузки, сценарии и заглушки.

Наполнение БД – при необходимости указывается, до каких объемов необходимо наполнить БД.

Мониторинг – указывается перечень используемых средств мониторинга и список собираемых метрик.

## Введение в методологию нагрузочного тестирования

### Цели НТ

Основная цель __нагрузочного тестирования__ - проверка системы на соответствие нефункциональным требованиям.

Основными целямя являются:

- определение максимальной производительности;
- выявление "узких мест";
- проверка стабильности;
- проверка отказоустойчивости;
- проверка стрессоустойчивости;
- проверка системы при стрессовых ситуациях;
- оценка влияния мониторинга;
- проверка масштабирования;
- оценка влияния объемов;
- подборка оптимального комплекса технических средств (КТС);
- проверка исправления ошибок.

Каждую цель необходимо конкретизировать до технических требований. Критерии успеха.

### Виды нагрузочного тестирования

Существует довольно много видов НТ, при этом у каждого свои цели и специфика. Выбор конкретного вида зависит от требований к системе, а также планируемых условий ее использования:

__Нагрузочное тестирование (Load Testing).__ Проверяет работоспособность системы или сайта при заданной нагрузке. С его помощью можно понять, сможет ли система за определенное время обработать желаемое количество запросов.

__Тестирование производительности (Performance Testing).__ Позволяет понять, как поведет себя система в различных условиях. Например, при каком максимальном количестве запросов пользователей она в состоянии работать быстро, без ошибок, а при каком объеме данных начинаются проблемы.

__Тестирование стабильности (Stability Testing).__ Это проверка работоспособности и надежности системы на протяжении длительного времени.

__Стрессовое тестирование (Stress Testing).__ Проводят, чтобы проверить работоспособность системы при огромных нагрузках — больших, чем ожидается в обычных условиях. Это важно в том числе для определения максимальных нагрузок.

__Тестирование отказоустойчивости (Failover Testing).__ Проверка работоспособности системы при аварийных ситуациях. Например, чтобы узнать, переключилась ли система на резервный сервер, если сбой все-таки случился.

__Тестирование объемов (Volume Testing).__ Проверка стабильности системы при работе с большим объемом данных. Показывает, насколько меняется производительность при их обработке.

__Тестирование масштабируемости (Scalability Testing).__ Это тестирование стабильной работы системы при ожидаемом увеличении нагрузки, например, при большем количестве пользователей или объеме данных.

__Тестирование нагрузки на сервер (Server Load Testing).__ Позволяет оценить, сколько пользователей одновременно могут успешно отправить запрос на сервер и получить результат.

## Поиск максимальной производительности

По результатам тестирования определяется максимальный уровень производительности, при котором система соответствует требованиям, предъявленным к ней в части произоводительности. Например, время отклика, утилизация ресурсов, количество ошибок и т.д.

Тест состоит из двух этапов:

- ступенчатый тест;
- подтверждение максимума.

## Service Level Agreement

__SLA (Service Level Agreement)__ в тестировании — это соглашение об уровне предоставления услуг, которое определяет параметры качества и доступности сервиса.

В контексте тестирования SLA устанавливает стандарты и ожидания относительно качества продукта или услуги, а также определяет критерии успешности тестирования. Это помогает обеспечить соответствие продукта требованиям заказчика и стандартам качества.

Основные аспекты SLA в тестировании включают:

Цели и задачи тестирования: определение основных целей и задач тестирования, которые должны быть достигнуты в рамках SLA.
Критерии успешности: установление критериев, по которым будет оцениваться успешность тестирования, таких как количество найденных дефектов, время выполнения тестов и т. д.
Сроки и этапы тестирования: определение сроков и этапов проведения тестирования, чтобы обеспечить своевременное выполнение работ.
Ответственность сторон: распределение ответственности между заказчиком и исполнителем за выполнение условий SLA.

Таким образом, SLA является важным инструментом для обеспечения качества и надёжности продукта или услуги.

В нагрузочном тестировании SLA могут быть наложены следующием параметыр системы: время отклика, процент ошибок, утилизация аппаратных ресурсов, время восстаносления системы.

## Расчет профиля нагрузочного тестирования

__Профиль__ – набор операций, выбранных для нагрузочного тестирования, и их интенсивность.

В профиль входят:

- Наиболее интенсивные операции
- Бизнес критичные операции
- Тяжелые операции (ресурсоемкие)

Расчет профиля нагрузочного тестирования проводится:

- на основе аналитики
- на основе статистики

## Точность профиля

__Точность профиля__ нагрузочного тестирования — это показатель, который отражает, насколько точно тестовые сценарии и параметры нагрузки соответствуют реальным условиям использования системы или приложения.

## Случай с несколькими профилями (например, дневной и ночной)

Дневной профиль используется для имитации типичной дневной нагрузки на систему. Он включает в себя сценарии, которые отражают действия пользователей в течение рабочего дня.

Ночной профиль используется для проверки системы в условиях низкой нагрузки. Он позволяет выявить проблемы, связанные с ночным временем, когда активность пользователей минимальна. Например, можно протестировать систему резервного копирования данных или обновления программного обеспечения.

## Анализ архитектуры систем

### Источники нагрузки

Источники нагрузки делятся на два типа:

- видимые (пользователи, внешние системы);
- невидимые (фоновые процессы).

### Смежные системы

В нагрузочном тестировании __смежные системы__ — это системы, которые взаимодействуют с основной системой, подвергающейся нагрузке. Они играют важную роль в обеспечении точности и достоверности результатов тестирования.

Смежные системы включают:

- Базы данных. Системы, используемые для хранения и обработки данных. Нагрузочное тестирование должно учитывать нагрузку на базы данных, чтобы оценить их производительность и стабильность.
- API и веб-сервисы. Программные интерфейсы, через которые происходит обмен данными между системами. Тестирование API и веб-сервисов помогает выявить узкие места и проблемы с производительностью.
- Очереди сообщений. Системы обмена сообщениями, такие как RabbitMQ или Kafka. Нагрузочное тестирование очередей сообщений позволяет оценить их способность обрабатывать большое количество сообщений.
- Системы кэширования. Системы хранения данных в оперативной памяти для ускорения доступа к ним. Тестирование систем кэширования помогает определить их эффективность и влияние на производительность основной системы.
- Другие системы интеграции. Любые другие системы, с которыми основная система обменивается данными. Это могут быть системы электронной почты, платёжные системы и т. д.

### Заглушки

__Заглушки__ в нагрузочном тестировании — это специальные программные компоненты, которые используются для имитации ответов от внешних систем во время тестирования. Они позволяют разработчикам и тестировщикам создавать тестовые сценарии без необходимости подключения к реальным внешним сервисам.

Необходимость в заглушках может возникнуть в следующих случаях:

- смежные системы являются потенциально узким местом с точки зрения производительности;
- смежные системы не доступны на тестовом стенде;
- смежные системы еще не разработаны или находятся в постоянной доработке;
- есть необходимость проверить тестируемую системы при различных временах отклика смежных систем.

Заглушки помогают ускорить процесс тестирования, так как они предоставляют заранее определённые ответы на запросы, что позволяет сосредоточиться на функциональности тестируемого приложения. Это особенно полезно при проведении нагрузочного тестирования, когда необходимо проверить работу системы под высокой нагрузкой.

Основные преимущества использования заглушек в нагрузочном тестировании:

- Изоляция тестируемой системы. Заглушки позволяют изолировать тестируемую систему от внешних зависимостей, что упрощает процесс тестирования и позволяет сосредоточиться на конкретных функциях.
- Ускорение тестирования. Заглушки предоставляют быстрые и предсказуемые ответы, что ускоряет процесс тестирования.
- Контроль над тестовыми данными. Заглушки позволяют контролировать тестовые данные, что обеспечивает стабильность и повторяемость тестов.
- Упрощение отладки. Заглушки могут помочь упростить отладку, предоставляя контролируемые ответы на запросы.

Однако использование заглушек также имеет некоторые недостатки:

- Сложность создания и поддержки. Создание и поддержка заглушек может быть сложным и трудоёмким процессом.
- Риск ошибок. Неправильно настроенные заглушки могут привести к ошибкам в тестах.
- Ограниченная функциональность. Заглушки не всегда могут полностью заменить реальные внешние сервисы, что может ограничить тестирование некоторых функций.

### Протоколы сетевые

#### Стек (TCP/IP)

#### HTTP. Состав запроса и ответа, основные методы, коды ответа

__Hypertext Transfer Protocol, HTTP (протокол передачи гипертекста)__ — это протокол прикладного уровня передачи данных, изначально — в виде гипертекстовых документов в формате HTML, в настоящее время используется для передачи произвольных данных.

Каждое HTTP-сообщение состоит из следующих частей, которые передаются в указанном порядке:

- Стартовая строка (англ. Starting line) — определяет тип сообщения;
- Заголовки (англ. Headers) — характеризуют тело сообщения, параметры передачи и прочие сведения; после каждой строки следует символ CRLF
- Пустая строка (то есть строка, в которой ничего не предшествует CRLF), она обозначает конец полей заголовка
- Тело сообщения (англ. Message Body) — непосредственно данные сообщения. Обязательно должно отделяться от заголовков пустой строкой.

Стартовая строка сообщения

Стартовые строки различаются для запроса и ответа.

Стартовая строка ответа сервера имеет следующий формат:

HTTP/Версия КодСостояния Пояснение

где:

- __Версия__ — пара разделённых точкой цифр, как в запросе;
- __Код состояния (англ. Status Code)__ — три цифры. По коду состояния определяется дальнейшее содержимое сообщения и поведение клиента;
- __Пояснение (англ. Reason Phrase)__ — текстовое короткое пояснение к коду ответа для пользователя. Никак не влияет на сообщение и является необязательным.

 Поля заголовка

Поля HTTP заголовка предоставляют необходимую информацию о запросе или ответе или об объекте, отправленном в теле сообщения. Существует четыре типа заголовков HTTP-сообщений:

- General (Общие заголовки): эти поля заголовка имеют общее применение как для сообщений запроса, так и для сообщений ответа.
- Request (Заголовки запроса): эти поля заголовка применимы только для сообщений запроса.
- Response (Заголовки ответа): эти поля заголовка применимы только для ответных сообщений.
- Entity (Заголовки сущности): эти поля заголовка определяют метаинформацию о теле объекта

Все вышеупомянутые заголовки следуют одному и тому же общему формату, и каждое поле заголовка состоит из имени, за которым следует двоеточие (:) и значение поля, как показано ниже:

имя-поля: [значение-поля]

Ниже приведены примеры различных полей заголовка:

```text
Host: suip.biz
User-Agent: Chrome
Accept: */*

Date: Tue, 03 Nov 2020 06:15:50 GMT`
Server: Apache/2.4.46 (Unix) PHP/7.4.12
Vary: Accept-Encoding
X-Powered-By: PHP/7.4.12
X-Frame-Options: SAMEORIGIN
Content-Type: text/html; charset=UTF-8
X-Varnish: 328517
Age: 0
Via: 1.1 varnish (Varnish/6.5)
Accept-Ranges: bytes
Connection: keep-alive
```

Тело сообщения

Часть сообщения, которое называется телом, является необязательной для HTTP-сообщения, но если она доступна, она используется для переноса тела объекта, связанного с запросом или ответом. Если тело объекта присутствует, то обычно строки заголовков Content-Type и Content-Length указывают природу тела сообщения.

Тело сообщения — это та часть, которая содержит фактические данные HTTP-запроса (сюда же относятся данные, передаваемые через веб-формы и выгружаемые файлы) и данные HTTP-ответа от сервера (включая HTML код, файлы, изображения и т. д.). Ниже приводится простое содержание тела сообщения (ответ):

```text
<html>
<body>
<h1>Hello, World!</h1>
</body>
</html>
```

Метод запроса

Метод HTTP (англ. HTTP Method) — последовательность из любых символов, кроме управляющих и разделителей, указывающая на основную операцию над ресурсом. Обычно метод представляет собой короткое английское слово, записанное заглавными буквами. Обратите внимание, что название метода чувствительно к регистру.

| Метод   | Описание |
|---------|----------|
| GET     |  Используется для запроса содержимого указанного ресурса. С помощью метода GET можно также начать какой-либо процесс. В этом случае в тело ответного сообщения следует включить информацию о ходе выполнения процесса. Клиент может передавать параметры выполнения запроса в URI целевого ресурса после символа «?»: `GET /path/resource?param1=value1&param2=value2 HTTP/1.1` Согласно стандарту HTTP, запросы типа GET считаются идемпотентными |
| HEAD    | Аналогичен методу GET, за исключением того, что в ответе сервера отсутствует тело. Запрос HEAD обычно применяется для извлечения метаданных, проверки наличия ресурса (валидация URL) и чтобы узнать, не изменился ли он с момента последнего обращения. |
| POST    |  Применяется для передачи пользовательских данных заданному ресурсу, в том числе выгрузки файлов на веб-сервер. Например, в блогах посетители обычно могут вводить свои комментарии к записям в HTML-форму, после чего они передаются серверу методом POST и он помещает их на страницу. При этом передаваемые данные (в примере с блогами — текст комментария) включаются в тело запроса. Аналогично с помощью метода POST обычно загружаются файлы на сервер. В отличие от метода GET, метод POST не считается идемпотентным, то есть многократное повторение одних и тех же запросов POST может возвращать разные результаты (например, после каждой отправки комментария будет появляться очередная копия этого комментария). |
| PUT     | Применяется для загрузки содержимого запроса на указанный в запросе URI. Если по заданному URI не существует ресурс, то сервер создаёт его и возвращает статус 201 (Created). Если же был изменён ресурс, то сервер возвращает 200 (Ok) или 204 (No Content). Сервер не должен игнорировать некорректные заголовки Content-*, передаваемые клиентом вместе с сообщением. Если какой-то из этих заголовков не может быть распознан или не допустим при текущих условиях, то необходимо вернуть код ошибки 501 (Not Implemented). |
| DELETE  | Удаляет указанный ресурс. |
| CONNECT | Преобразует соединение запроса в прозрачный TCP/IP-туннель, обычно чтобы содействовать установлению защищённого SSL-соединения через нешифрованный прокси. |
| OPTIONS | Используется для определения возможностей веб-сервера или параметров соединения для конкретного ресурса. В ответ серверу следует включить заголовок Allow со списком поддерживаемых методов. Также в заголовке ответа может включаться информация о поддерживаемых расширениях. |
| TRACE   | Возвращает полученный запрос так, что клиент может увидеть, какую информацию промежуточные серверы добавляют или изменяют в запросе. |
| PATCH   | Аналогично PUT, но применяется только к фрагменту ресурса. |

Ответы

После получения и интерпретации сообщения запроса сервер отвечает сообщением ответа HTTP, его структура:

- Строка состояния
- Ноль или более полей заголовка (общие заголовки, заголовки ответа и заголовки сущности), за которыми следует символ CRLF
- Пустая строка (т.е. строка, в которой ничего не предшествует CRLF), указывающая на конец полей заголовка
- Необязательно тело сообщения

Код состояния

Клиент узнаёт по коду ответа о результатах его запроса и определяет, какие действия ему предпринимать дальше. Набор кодов состояния является стандартом, и они описаны в соответствующих документах RFC. Введение новых кодов должно производиться только после согласования с IETF. Клиент может не знать все коды состояния, но он обязан отреагировать в соответствии с классом кода.

В настоящее время выделено пять классов кодов состояния.

| Код | Класс           | Назначение |
|-----|-----------------|------------|
| 1xx | Информационный  | Информирование о процессе передачи. |
| 2xx | Успех           | Информирование о случаях успешного принятия и обработки запроса клиента. В зависимости от статуса, сервер может ещё передать заголовки и тело сообщения. |
| 3xx | Перенаправление | Сообщает клиенту, что для успешного выполнения операции необходимо сделать другой запрос (как правило по другому URI). Из данного класса пять кодов 301, 302, 303, 305 и 307 относятся непосредственно к перенаправлениям (редирект). Адрес, по которому клиенту следует произвести запрос, сервер указывает в заголовке Location. При этом допускается использование фрагментов в целевом URI. |
| 4xx | Ошибка клиента  | Указание ошибок со стороны клиента. При использовании всех методов, кроме HEAD, сервер должен вернуть в теле сообщения гипертекстовое пояснение для пользователя. |
| 5xx | Ошибка сервера  | Информирование о случаях неудачного выполнения операции по вине сервера. Для всех ситуаций, кроме использования метода HEAD, сервер должен включать в тело сообщения объяснение, которое клиент отобразит пользователю. |

### Базы данных

#### Введение в БД (классификация и примеры)

#### Терминология и нормальные формы

#### SQL (DDL, DML, DCL, TCL)

#### Select, Update, Insert, Trancate, Delete

#### Индексы (плюсы и минусы)

#### View vs Table

#### Алиасы

#### Oracle, AWR

#### PostgreSQL, аналоги AWR

#### План запроса (как построить и зачем)

### Основы программирования

#### Парадигмы ООП

__Объектно-ориентированное программирование (ООП)__ — методология программирования, основанная на представлении программы в виде совокупности объектов, каждый из которых является экземпляром определенного класса, а классы образуют иерархию наследования.

- объектно-ориентированное программирование использует в качестве основных логических конструктивных элементов объекты, а не алгоритмы;
- каждый объект является экземпляром определенного класса;
- классы образуют иерархии.

Программа считается объектно-ориентированной, только если выполнены все три указанных требования. В частности, программирование, не использующее наследование, называется не объектно-ориентированным, а программированием с помощью абстрактных типов данных.

Согласно парадигме ООП программа состоит из объектов, обменивающихся сообщениями. Объекты могут обладать состоянием, единственный способ изменить состояние объекта - послать ему сообщение, в ответ на которое, объект может изменить собственное состояние.

Оосновные принципы _ООП_:

- _Инкапсуляция_ - сокрытие реализации
- _Наследование_ - создание новой сущности на базе уже существующей
- _Полиморфизм_ - возможность иметь разные формы для одной и той же сущности
- _Абстракция_ - набор общих характеристик
  
__Инкапсуляция__ – это свойство системы, позволяющее объединить данные и методы, работающие с ними, в классе и скрыть детали реализации от пользователя, открыв только то, что необходимо при последующем использовании.

Цель инкапсуляции — уйти от зависимости внешнего интерфейса класса (то, что могут использовать другие классы) от реализации. Чтобы малейшее изменение в классе не влекло за собой изменение внешнего поведения класса.

__Наследование__ – это свойство системы, позволяющее описать новый класс на основе уже существующего с частично или полностью заимствующейся функциональностью.

Класс, от которого производится наследование, называется _предком_, _базовым_ или _родительским_. Новый класс – _потомком_, _наследником_ или _производным_ классом.

__Полиморфизм__ – это свойство системы использовать объекты с одинаковым интерфейсом без информации о типе и внутренней структуре объекта.

Преимуществом полиморфизма является то, что он помогает снижать сложность программ, разрешая использование одного и того же интерфейса для задания единого набора действий. Выбор же конкретного действия, в зависимости от ситуации, возлагается на компилятор языка программирования. Отсюда следует ключевая особенность полиморфизма - использование объекта производного класса, вместо объекта базового (потомки могут изменять родительское поведение, даже если обращение к ним будет производиться по ссылке родительского типа).

Полиморфизм бывает _динамическим_ (переопределение) и _статическим_ (перегрузка).

_Полиморфная переменная_, это переменная, которая может принимать значения разных типов, а _полиморфная функция_, это функция у которой хотя бы один аргумент является полиморфной переменной.
Выделяют два вида полиморфных функций:

- _ad hoc_, функция ведет себя по разному для разных типов аргументов (например, функция `draw()` — рисует по разному фигуры разных типов);
- _параметрический_, функция ведет себя одинаково для аргументов разных типов (например, функция `add()` — одинаково кладет в контейнер элементы разных типов).

_Абстрагирование_ – это способ выделить набор общих характеристик объекта, исключая из рассмотрения частные и незначимые. Соответственно, __абстракция__ – это набор всех таких характеристик.

#### Классы и объекты

Класс — это тип данных, созданный пользователем. Он содержит разные свойства и методы, как, например, тип String или Integer. Класс — это «шаблон» для объекта, который описывает его свойства. Несколько похожих между собой объектов, например профили разных пользователей, будут иметь одинаковую структуру, а значит, принадлежать к одному классу.

Объект — это основная единица в ООП, представляющая собой экземпляр класса, сочетающий в себе данные (состояние) и методы (поведение) для работы с этими данными. Например, когда вы создаёте переменную типа String и присваиваете ей значение «Строка», то в памяти создаётся экземпляр класса String.
