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
   Поиск максимальной производительности
   Проверка стабильности
   Стресс тесты
   Проверка стрессоустойчивости
   Проверка отказоустойчивости
   Проверка масштабируемости
   Объемное тестирование

4. [Service Level Agreement](#service-level-agreement)

5. Расчет профиля НТ
   - На основе аналитики
   - На основе статистики (по ЧПН)
   - Точность профиля
   - Случай с несколькими профилями (например, дневной и ночной)

6. Анализ архитектуры систем
  Промышленный и тестовый стенды
  Важность их сравнения
  Источники нагрузки
  Смежные системы
  Заглушки
  Протоколы сетевые
   Стек (TCP/IP)
   НТТР (состав запроса и ответа, основные методы, коды ответа)
   Стандарты обмена REST и SOAP
  Двузхвенная и трехзвенная архитектура
   Толстый и тонкий клиенты
  Основные компоненты тестируемых систем
   Front, Back, DB
   Микросервисная и монолитная архитектура
   Балансировщики
   Брокеры сообщений (MQ vs Kafka)

7. JMeter
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

### Основы тестирования

#### Требования

#### Дефекты

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

#### Виды тестирования

Тестирование программного обеспечения можно классифицировать по различным критериям.

![Классификация тестирования](/images/image_01.jpeg)

Нагрузочное тестирование - это проверка устойчивости и производительности программного обеспечения под нагрузкой, сопоставимой с реальными условиями использования.

[Дополнительная информация](https://habr.com/ru/articles/770600/)

### Методика нагрузочного тестирования

Перед тем как приступить к проверке программного продукта, нужно разработать методику нагрузочного тестирования (МНТ). Это документ, в котором описан весь план тестирования, включая сценарии возможного развития проекта. МНТ полезна как заказчикам, так и команде исполнителей, потому что позволяет формализовать требования, однозначно определить ожидания от проекта.

#### Назначение, почему это важно

Для заказчика - фиксирование всех договоренностей с исполнителем, чтобы он понимал, что получит на выходе.
Для специалистов НТ - МНТ является некоторой "страховкой" при общении с заказчиком. Также МНТ - это инструкция, содержащая информацию по распределению работ. Помогает с передачей знаний по проекту.

#### Основные разделы

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

### Введение в методологию нагрузочного тестирования

#### Цели НТ

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
- подборка оптимального КТС;
- проверка исправления ошибок.

Каждую цель необходимо конкретизировать до технических требований. Критерии успеха.

#### Виды нагрузочного тестирования

Существует довольно много видов НТ, при этом у каждого свои цели и специфика. Выбор конкретного вида зависит от требований к системе, а также планируемых условий ее использования:

__Нагрузочное тестирование (Load Testing).__ Проверяет работоспособность системы или сайта при заданной нагрузке. С его помощью можно понять, сможет ли система за определенное время обработать желаемое количество запросов.

__Тестирование производительности (Performance Testing).__ Позволяет понять, как поведет себя система в различных условиях. Например, при каком максимальном количестве запросов пользователей она в состоянии работать быстро, без ошибок, а при каком объеме данных начинаются проблемы.

__Тестирование стабильности (Stability Testing).__ Это проверка работоспособности и надежности системы на протяжении длительного времени.

__Стрессовое тестирование (Stress Testing).__ Проводят, чтобы проверить работоспособность системы при огромных нагрузках — больших, чем ожидается в обычных условиях. Это важно в том числе для определения максимальных нагрузок.

__Тестирование отказоустойчивости (Failover Testing).__ Проверка работоспособности системы при аварийных ситуациях. Например, чтобы узнать, переключилась ли система на резервный сервер, если сбой все-таки случился.

__Тестирование объемов (Volume Testing).__ Проверка стабильности системы при работе с большим объемом данных. Показывает, насколько меняется производительность при их обработке.

__Тестирование масштабируемости (Scalability Testing).__ Это тестирование стабильной работы системы при ожидаемом увеличении нагрузки, например, при большем количестве пользователей или объеме данных.

__Тестирование нагрузки на сервер (Server Load Testing).__ Позволяет оценить, сколько пользователей одновременно могут успешно отправить запрос на сервер и получить результат.

### Service Level Agreement

__SLA (Service Level Agreement)__ в тестировании — это соглашение об уровне предоставления услуг, которое определяет параметры качества и доступности сервиса.

В контексте тестирования SLA устанавливает стандарты и ожидания относительно качества продукта или услуги, а также определяет критерии успешности тестирования. Это помогает обеспечить соответствие продукта требованиям заказчика и стандартам качества.

Основные аспекты SLA в тестировании включают:

Цели и задачи тестирования: определение основных целей и задач тестирования, которые должны быть достигнуты в рамках SLA.
Критерии успешности: установление критериев, по которым будет оцениваться успешность тестирования, таких как количество найденных дефектов, время выполнения тестов и т. д.
Сроки и этапы тестирования: определение сроков и этапов проведения тестирования, чтобы обеспечить своевременное выполнение работ.
Ответственность сторон: распределение ответственности между заказчиком и исполнителем за выполнение условий SLA.

Таким образом, SLA является важным инструментом для обеспечения качества и надёжности продукта или услуги.

В нагрузочном тестировании SLA могут быть наложены следующием параметыр системы: время отклика, процент ошибок, утилизация аппаратных ресурсов, время восстаносления системы.

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
