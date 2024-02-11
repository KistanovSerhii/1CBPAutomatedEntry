ФАЙЛ обработки будет после публикации видео!!!

##### <a name="pageup"></a>

# 1CBPAutomatedEntry
Автоматическое заполнение документа "Операции, введенные вручную" конфигурации 1С Бухгалтерия 3.0


🗺️ Обработка позволяет заполнить в 1С Бухгалтерия 3.0, Платформа 8.3 документ "ОперацияБух" (синоним "Операции, введенные вручную")
данными из табличного документа, а также есть программный интерфейс который позволит заполнить из файла excel или результата запроса.

!!!Будет после готовности видео!!! 👀 ["Видео инструкиця"](https://youtu.be/)

# 📜 Разделы

+ [Пользовательский интерфейс](#step0); 👥
+ [Программный интерфейс](#step1); 👨‍💻
+ [На что обратить внимание](#step2); 🖖🏻

##### <a name="step0"></a> 👥 Пользовательский интерфейс [(начало)](#pageup);

1. Скачайте внешнюю обработку "ОперацияВведеннаяВручную.epf".
2. Добавьте обработку в интерактивном режиме (Администрирование/Печатные формы, отчеты и обработки).
3. Выполните обработку для открытия главной формы (или откройте через меню "Файл/Открыть").

На форме обработки есть табличный документ с заполненным универсальным заголовком, универсальным потому что подходит для заполнения разным набором счетов.

> [!IMPORTANT]
> Для составных видов субконто алгоритм автоматически определит вид субконто (тип данных).
> Нет необходимости указывать условия поиска справочников, так как
> алгоритм выполняет поиск как по коду, так и по наименованию. Это значит, что в каждой строке
> вам доступно указывать код или наименование, и алгоритм разберется с этим самостоятельно.

1️⃣  Вкладка "Строки Различных Счетов" позволяет:
1. Заполнить табличный документ универсальным заголовком (команда "Заголовок").
2. Выполнить перенос данных из табличного документа в документ "Операции, введенные вручную" (команда "Добавить в документ").

> [!TIP]
> Этот раздел предназначен для наполнения табличного документа строками, в которых счета могут различаться на разных строках.
> По этой причине заголовок универсальный и содержит все возможные колонки.

> [!NOTE]
> Субконто расположены идентично расположению в таблице счета.
> Субконто в строке под номером 1 является "Субконто1" в заголовке и документе Операция и т.д.

2️⃣  Вкладка "Строки Повторяющихся Счетов" позволяет:
1. Заполнить табличный документ заголовком указанных счетов, при этом выводится наименование вида Субконто (команда "Заголовок").
2. Выполнить перенос данных из табличного документа в документ "Операции, введенные вручную" (команда "Добавить в документ").

> [!TIP]
> Этот раздел предназначен для наполнения табличного документа строками, в которых счета повторяются для каждой строки табличных данных. 
> По этой причине заголовок явно определен для каждого счета и не содержит лишних колонок, в отличие от универсального заголовка.

> [!IMPORTANT]
> Запуская загрузку строк различных счетов алгоритм группирует строки по повторяющимся счетам и выполняет 
> для каждой группы запуск загрузки строк с повторяющимися счетами. После выполнения сообщение будет содержать
> информацию загруженных групп, например:
> "Проводки сформированы! (19.03 | 60.01)"
> "Проводки сформированы! (41.01 | 60.01)"
> "Проводки сформированы! (90.03 | 68.02)"


##### <a name="step1"></a> 👨‍💻 Программный интерфейс [(начало)](#pageup);


```
// Код
```

##### <a name="step2"></a> 🖖🏻 На что обратить внимание [(начало)](#pageup);
