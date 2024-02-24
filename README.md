##### <a name="pageup"></a>

# 1CBPAutomatedEntry
Автоматическое заполнение документа "Операции, введенные вручную" конфигурации 1С Бухгалтерия 3.0

🗺️ Обработка позволяет заполнить в 1С Бухгалтерия 3.0, Платформа 8.3 документ "ОперацияБух" (синоним "Операции, введенные вручную")
данными из табличного документа, а также есть программный интерфейс который позволит заполнить из файла excel или результата запроса.

👀 ["Видео инструкиця"](https://youtu.be/JiO6coTDH0U)

# 📜 Разделы

+ [Пользовательский интерфейс](#step0); 👥
+ [Программный интерфейс](#step1); 👨‍💻
+ [На что обратить внимание](#step2); 🖖🏻

##### <a name="step0"></a> 👥 Пользовательский интерфейс [(начало)](#pageup);

1. Скачайте внешнюю обработку "ОперацияВведеннаяВручную.epf".
2. Добавьте обработку в интерактивном режиме (Администрирование/Печатные формы, отчеты и обработки).
3. Выполните обработку для открытия главной формы (или откройте через меню "Файл/Открыть").

На форме обработки есть табличный документ с заполненным универсальным заголовком, универсальным потому что подходит для заполнения разным набором счетов.
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/aa03c3d7-39d5-47ed-8842-5266c8d94417)

> [!IMPORTANT]
> Для составных видов субконто алгоритм автоматически определит вид субконто (тип данных).
> Нет необходимости указывать условия поиска справочников, так как
> алгоритм выполняет поиск как по коду, так и по наименованию. Это значит, что в каждой строке
> вам доступно указывать код или наименование, и алгоритм разберется с этим самостоятельно.

☝ Вкладка "Строки Различных Счетов" позволяет:
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/c098d782-55b2-40a7-98b0-411233c1099c)
1. Заполнить табличный документ универсальным заголовком (команда "Заголовок").
2. Выполнить перенос данных из табличного документа в документ "Операции, введенные вручную" (команда "Добавить в документ").

> [!TIP]
> Этот раздел предназначен для наполнения табличного документа строками, в которых счета могут различаться на разных строках.
> По этой причине заголовок универсальный и содержит все возможные колонки.
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/05fd8a0f-97b9-4b05-8b08-2e73d6dd1ed7)

> [!NOTE]
> Субконто расположены идентично расположению в таблице счета.
> Субконто в строке под номером 1 является "Субконто1" в заголовке и документе Операция и т.д.
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/ed70b8c3-d184-4f30-a1d9-df23d27b4d9e)

✌️ Вкладка "Строки Повторяющихся Счетов" позволяет:
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/5d170da6-e04f-46bd-8242-4b6815606172)
1. Заполнить табличный документ заголовком указанных счетов, при этом выводится наименование вида Субконто (команда "Заголовок").
2. Выполнить перенос данных из табличного документа в документ "Операции, введенные вручную" (команда "Добавить в документ").

> [!TIP]
> Этот раздел предназначен для наполнения табличного документа строками, в которых счета повторяются для каждой строки табличных данных. 
> По этой причине заголовок явно определен для каждого счета и не содержит лишних колонок, в отличие от универсального заголовка.
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/7856f509-b179-4e31-9385-848afb3b2015)

> [!IMPORTANT]
> Запуская загрузку строк различных счетов алгоритм группирует строки по повторяющимся счетам и выполняет 
> для каждой группы запуск загрузки строк с повторяющимися счетами. После выполнения сообщение будет содержать
> информацию загруженных групп, например:
> "Проводки сформированы! (19.03 | 60.01)"
> "Проводки сформированы! (41.01 | 60.01)"
> "Проводки сформированы! (90.03 | 68.02)"

##### <a name="step1"></a> 👨‍💻 Программный интерфейс [(начало)](#pageup);

1. [Общий код получения внешней обработки перед использованием](#пункт-а)
2. [Пример наполнения данными указав путь к файлу](#пункт-б)
3. [Пример наполнения данными указав результат запроса](#пункт-в)

## Общий код получения внешней обработки перед использованием
<a name="пункт-а"></a>
```1C
	&НаСервереБезКонтекста
	Функция ОперацияВведеннаяВручнуюВнешняяОбработка()

	#Область ПолучениеВнешнейОбработки_ОперацияВведеннаяВручную
		внешняяОбработкаИмя    = "ОперацияВведеннаяВручную";
		внешняяОбработкаСсылка = Справочники.ДополнительныеОтчетыИОбработки.НайтиПоРеквизиту("ИмяОбъекта", внешняяОбработкаИмя);
		внешняяОбработкаОбъект = ДополнительныеОтчетыИОбработки.ОбъектВнешнейОбработки(внешняяОбработкаСсылка);
	#КонецОбласти

	#Область ПрерватьВыполненияЕслиОбработкаОтсутствует
		Если внешняяОбработкаОбъект = Неопределено Тогда
			текстСообщения = СтрШаблон(НСтр("ru = 'Ошибка инициализации внешней обработки ""%1""'"), внешняяОбработкаИмя);
			ОбщегоНазначения.СообщитьПользователю(текстСообщения);
			Возврат Неопределено;
		КонецЕсли;
	#КонецОбласти

		Возврат внешняяОбработкаОбъект;

	КонецФункции
```

## Пример наполнения данными указав путь к файлу
<a name="пункт-б"></a>
```1C
	&НаКлиенте
	Процедура ДобавитьИзФайла(Команда)
	
		ПутьКФайлу = "D:\ОбразецYouTubeКонтента\Video7\testdata\testTabDoc.xls";
		ОписаниеФайла = Новый Структура;
		ОписаниеФайла.Вставить("ПутьКФайлу", ПутьКФайлу);
		ОписаниеФайла.Вставить("ДвоичныеДанные", Новый ДвоичныеДанные(ПутьКФайлу));
		ОписаниеФайлаАдрес = ПоместитьВоВременноеХранилище(ОписаниеФайла);
	
		ДобавитьИзФайлаНаСервере(ОписаниеФайлаАдрес);
	
	КонецПроцедуры

	&НаСервереБезКонтекста
	Процедура ДобавитьИзФайлаНаСервере(ОписаниеФайлаАдрес)
			
		внешняяОбработкаОбъект = ОперацияВведеннаяВручнуюВнешняяОбработка();
		Если внешняяОбработкаОбъект = Неопределено Тогда
			Возврат;
		КонецЕсли;
	
		РезультатаЗапроса = Неопределено;
	
		ПараметрыДобавленя = внешняяОбработкаОбъект.ПараметрыДобавленя();		
		ПараметрыДобавленя.Приемник = Документы.ОперацияБух.НайтиПоНомеру("0000-000002", Дата("20240211")).Ссылка;
		ПараметрыДобавленя.Источник = ОписаниеФайлаАдрес;
	
		внешняяОбработкаОбъект.ДобавитьИзФайла(ПараметрыДобавленя);
	
	КонецПроцедуры
```

## Пример наполнения данными указав результат запроса
<a name="пункт-в"></a>
```1C
	&НаКлиенте
	Процедура ДобавитьИзРезультатаЗапроса(Команда)
		ДобавитьИзРезультатаЗапросаНаСервере();
	КонецПроцедуры

	&НаСервереБезКонтекста
	Процедура ДобавитьИзРезультатаЗапросаНаСервере()
					
		внешняяОбработкаОбъект = ОперацияВведеннаяВручнуюВнешняяОбработка();
		Если внешняяОбработкаОбъект = Неопределено Тогда
			Возврат;
		КонецЕсли;
	
		ДокСсылка = Документы.ОперацияБух.НайтиПоНомеру("0000-000002", Дата("20240211")).Ссылка;
		РезультатЗапроса = ОперацияВручнуюРезультатаЗапроса(ДокСсылка);
	
		ПараметрыДобавленя = внешняяОбработкаОбъект.ПараметрыДобавленя();	
		ПараметрыДобавленя.Приемник = ДокСсылка;
		ПараметрыДобавленя.Источник = РезультатЗапроса;
	
		// Уточняем какой Вид субконто (Контрагент или ФизическиеЛица) >
		ФизЛицоСтрокаПоиска = "Никитаева И.В.";
		ФизЛицоИндексВидаСубконто = 2;
		ФизЛицоСсылка = Справочники.ФизическиеЛица.НайтиПоКоду("00-0000013").Ссылка;
		ФизЛицоКакКлючКэша = внешняяОбработкаОбъект.СсылкаКакКлючКэша(
	  	ФизЛицоСсылка
		, ФизЛицоИндексВидаСубконто
		, ФизЛицоСтрокаПоиска
		);
		ПараметрыДобавленя.КэшДанных = Новый Соответствие;
		ПараметрыДобавленя.КэшДанных.Вставить(ФизЛицоКакКлючКэша, ФизЛицоСсылка);
		// Уточняем какой Вид субконто (Контрагент или ФизическиеЛица) <	
		
		внешняяОбработкаОбъект.ДобавитьИзРезультатаЗапроса(ПараметрыДобавленя);
	
	КонецПроцедуры

	&НаСервереБезКонтекста
	Функция ОперацияВручнуюРезультатаЗапроса(ДокСсылка)
		
	Запрос = Новый Запрос;
	Запрос.Текст = 
		"ВЫБРАТЬ
		|	Хозрасчетный.СчетДт КАК СчетДт,
		|	Хозрасчетный.ПодразделениеДт КАК ПодразделениеДт,
		|	Хозрасчетный.СубконтоДт1 КАК Субконто1Дт,
		|	Хозрасчетный.СубконтоДт2 КАК Субконто2Дт,
		|	Хозрасчетный.СубконтоДт3 КАК Субконто3Дт,
		|	Хозрасчетный.ВалютаДт КАК ВалютаДт,
		|	Хозрасчетный.ВалютнаяСуммаДт КАК ВалютнаяСуммаДт,
		|	Хозрасчетный.КоличествоДт КАК КоличествоДт,
		|	Хозрасчетный.СуммаНУДт КАК СуммаНУДт,
		|	Хозрасчетный.СуммаПРДт КАК СуммаПРДт,
		|	Хозрасчетный.СуммаВРДт КАК СуммаВРДт,
		|
		|	Хозрасчетный.СчетКт КАК СчетКт,
		|	Хозрасчетный.ПодразделениеКт КАК ПодразделениеКт,
		|	Хозрасчетный.СубконтоКт1 КАК Субконто1Кт,
		|	Хозрасчетный.СубконтоКт2 КАК Субконто2Кт,
		|	Хозрасчетный.СубконтоКт3 КАК Субконто3Кт,
		|	Хозрасчетный.ВалютаКт КАК ВалютаКт,
		|	Хозрасчетный.ВалютнаяСуммаКт КАК ВалютнаяСуммаКт,
		|	Хозрасчетный.КоличествоКт КАК КоличествоКт,
		|	Хозрасчетный.СуммаНУКт КАК СуммаНУКт,
		|	Хозрасчетный.СуммаПРКт КАК СуммаПРКт,
		|	Хозрасчетный.СуммаВРКт КАК СуммаВРКт,
		|
		|	Хозрасчетный.Сумма КАК Сумма,
		|	Хозрасчетный.Содержание КАК Содержание
		|ИЗ
		|	РегистрБухгалтерии.Хозрасчетный.ДвиженияССубконто(
		|		ДАТАВРЕМЯ(2024, 1, 1, 0, 0, 0)
		|		, ДАТАВРЕМЯ(2024, 3, 31, 23, 59, 59)
		|		, Регистратор = &Регистратор,,
		|
		|	) КАК Хозрасчетный";
	
	Запрос.УстановитьПараметр("Регистратор", ДокСсылка);
	РезультатЗапроса = Запрос.Выполнить();
	
	Возврат РезультатЗапроса;
	
	КонецФункции
```

##### <a name="step2"></a> 🖖🏻 На что обратить внимание [(начало)](#pageup);




> [!IMPORTANT]
> Если вы копируете строку в табличный документ не из 1С и строка начинается с кавычки " тогда
> строка должна начинатся с 4 кавычек для экранирования переноса в табличный документ.
> Если вы попытаетесь перенести "Венские" Вафли со сгущенным молоком, табличный документ удалит кавычки и вы получите Венские Вафли со сгущенным молоком.
> Правильно будет добавить так """"Венские" Вафли со сгущенным молоком (не важно сколько кавычек в строке, достаточно поставить 4 кавычки в начале)
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/86ab9c99-9032-473b-82b6-81f4c87e3d18)
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/147dc83e-da50-4daa-904d-ad714e9f0a23)



> [!IMPORTANT]
> Платформа позволяет добавить счету составное субконто с видами субконто, наименование которых может совпадать.
> Например, счет 80 "Уставный капитал" имеет составной вид субконто "Учредители", который может быть как "Контрагент", так и "Физическое лицо".
> А что, если контрагент и физическое лицо будут одинаковыми, например, "Иванов И.И."? Тогда как программа определит источник данных по строке "Никитаева И.В."?
> В этом случае программа подставит первого (порядок в соответствии с порядком перечисления в виде субконто) и в реквизит проводки "Содержание" добавит
> текст содержания, введенного пользователем + found=КоличествоНайденных.
![image](https://github.com/KistanovSerhii/1CBPAutomatedEntry/assets/28355711/fcea636c-9750-4180-848c-b6c167d2931d)



> [!NOTE]
> Я не стал перегружать пользовательский интерфейс реализацией сопоставления при нахождении более одной записи,
> так как на данный момент у меня нет данных о том, что вы столкнетесь с такой ситуацией. Однако код рассчитан на
> предоставление пользователю данного инструмента. Соответственно, через программный интерфейс этим можно воспользоваться
> добавив в параметр "Кэш" (это тип соответствие) ключ с указанием порядкового номера вида субконто. Тогда будет
> выбран именно он, а не просто первый по порядку.

# Уточняем, какой вид субконто (Контрагент или ФизическиеЛица)
```1C
	ФизЛицоСтрокаПоиска = "Никитаева И.В.";
	ФизЛицоИндексВидаСубконто = 2;
	ФизЛицоСсылка = Справочники.ФизическиеЛица.НайтиПоКоду("00-0000013").Ссылка;
	ФизЛицоКакКлючКэша = внешняяОбработкаОбъект.СсылкаКакКлючКэша(
	  ФизЛицоСсылка
	, ФизЛицоИндексВидаСубконто
	, ФизЛицоСтрокаПоиска
	);
	ПараметрыДобавленя.КэшДанных = Новый Соответствие;
	ПараметрыДобавленя.КэшДанных.Вставить(ФизЛицоКакКлючКэша, ФизЛицоСсылка);
```
