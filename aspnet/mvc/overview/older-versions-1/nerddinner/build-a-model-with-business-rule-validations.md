---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Создание модели с проверками бизнес-правил | Документация Майкрософт
author: microsoft
description: Шаг 3 показано, как создать модель, что мы можно использовать для обоих запросов и обновления базы данных для нашего приложения NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828868"
---
<a name="build-a-model-with-business-rule-validations"></a>Создание модели с проверками бизнес-правил
====================
по [Microsoft](https://github.com/microsoft)

[Загрузить PDF-файл](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Это 3 из бесплатной [руководство по использованию приложения «NerdDinner»](introducing-the-nerddinner-tutorial.md) , пошаговое рассмотрение как создать небольшой, но завершить, веб-приложения с помощью ASP.NET MVC 1.
> 
> Шаг 3 показано, как создать модель, что мы можно использовать для обоих запросов и обновления базы данных для нашего приложения NerdDinner.
> 
> Если вы используете ASP.NET MVC 3, рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner шаг 3: Построение модели

В платформу model-view-controller термин «модель» ссылается на объекты, представляющие данные приложения, а также логику домена, включающей проверки и бизнес-правила с ним. Модель во многих отношениях «основу» приложения на базе MVC и как мы увидим позже фундаментально диски его поведение.

Платформа ASP.NET MVC поддерживает использование любой технологии доступа к данным, и разработчики могут выбрать из множества вариантов широкие возможности .NET данных для реализации своих моделей, в том числе: LINQ для сущности, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM или просто необработанные ADO. NET DataReaders или наборов данных.

Для нашего приложения NerdDinner мы собираемся использовать LINQ to SQL, чтобы создать простую модель, которая соответствует крайне последовательное наши структуры базы данных, а также добавляет некоторые пользовательские логику и бизнес-правила проверки. Мы затем реализуем класс репозитория, которая помогает от абстрактного реализации сохраняемости данных от остальной части приложения и позволяет нам легко модульного тестирования.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL — это ORM (реляционный модуль сопоставления объектов), который поставляется как часть .NET 3.5.

LINQ to SQL предоставляет простой способ классы .NET, которые мы можете писать код применительно сопоставляются с таблицами базы данных. Наше приложение NerdDinner мы будем использовать для сопоставления таблицы ужинов и RSVP в базе данных для классов Dinner и RSVP. Столбцы таблицы ужинов и RSVP будет соответствовать свойствам классы Dinner и RSVP. Каждый объект Dinner и RSVP будет представлять отдельную строку в пределах ужинов или RSVP таблиц в базе данных.

LINQ to SQL позволяет избежать необходимости вручную создавать инструкции SQL для извлечения и обновления Dinner и RSVP объекты с данными базы данных. Вместо этого мы определим классы Dinner и RSVP, как они сопоставляются и из базы данных и связи между ними. LINQ to SQL будет затем принимает осторожностью создания соответствующую логику выполнения SQL для использования во время выполнения, когда мы взаимодействовать и использовать их.

Поддержка языка LINQ в Visual Basic и C# можно использовать для написания выражений запросов, которые получают Dinner и RSVP объекты из базы данных. Это сводит к минимуму объем кода данных, нам нужно создать, и можно строить это отличный приложений.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Добавление классов LINQ to SQL в свой проект

Будет начать, щелкнув правой кнопкой папку «Моделей» в нашем проекте и выберите **Add -&gt;новый элемент** команды меню:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Это приведет к появлению диалогового окна «Добавление нового элемента». Мы фильтрация по категории «Данные» и выберите шаблон «LINQ to SQL Classes» внутри него:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Мы имя элемента «NerdDinner» и нажмите кнопку «Добавить». Visual Studio добавьте файл NerdDinner.dbml наших \Models в каталоге и откройте LINQ to SQL реляционный конструктор объектов:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Создание классов моделей данных с помощью LINQ to SQL

LINQ to SQL позволяет нам быстро создать классы модели данных из существующей схемы базы данных. Задача этого мы будем откройте NerdDinner базы данных в обозревателе серверов и выберите таблицы, мы хотим модели в нем:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Затем можно перетащить таблицы на LINQ SQL конструктора. При выполнении этого LINQ to SQL автоматически создаст Dinner и RSVP классы, с помощью схемы таблиц (с помощью свойств класса, которые сопоставляются со столбцами таблицы базы данных):

![](build-a-model-with-business-rule-validations/_static/image5.png)

По умолчанию LINQ to SQL конструктор автоматически «форму множественного «имена таблиц и столбцов при создании классов на основе схемы базы данных. Например: в таблице «Ужинов» в приведенном выше примере привело к класс «Обед». Этот класс именования помогает сделать наши модели, соответствуют соглашения об именовании .NET и обычно найти, наличие конструктора исправления этого вверх удобным (особенно при добавлении множество таблиц). Если вам не нравится имя класса или свойство, конструктор создает, хотя всегда можно переопределить и изменить его на любое нужное имя. Это можно сделать, изменив свойство сущности или имя в строке конструктора или путем изменения его с помощью сетки свойств.

По умолчанию конструктор LINQ to SQL также проверяет первичного/внешнего ключа связи между таблицами и на их основе автоматически создает по умолчанию «отношение ассоциации» между классами другой модели, которые оно создает. Например, когда мы перетащили ужины и RSVP таблицы на конструктор LINQ to SQL связь "один к многим" между двумя было выведено основана на том, что в таблице RSVP было внешнего ключа к таблице ужинов (это состояние обозначается стрелкой в конструктор):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Ассоциации выше приведет к LINQ to SQL, добавляемый класс RSVP, который разработчики могут использовать для доступа к компании Dinner, связанный с данной RSVP свойство со строгим типом «Обед». Это также вызовет класс Dinner имеет свойство коллекции «RSVPs», позволяющая разработчикам для извлечения и обновления RSVP объекты, связанные с определенной Dinner.

Ниже вы увидите пример intellisense в Visual Studio, создать объект RSVP элемент и его добавления в коллекцию Кроме Dinner. Обратите внимание на то, как LINQ to SQL автоматически добавляется коллекция «Кроме» в объекте Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Путем добавления объекта RSVP коллекцию Кроме Dinner мы указываем LINQ to SQL, чтобы связать отношение внешнего ключа между ужин и строкой RSVP в нашей базе данных:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Если вас не устраивает, как конструктор моделировать или с именем привязку в таблице, его можно переопределить. Просто щелкните стрелку связи в конструкторе и получить доступ к его свойства с помощью сетки свойств переименовать, удалить или изменить его. Для нашего приложения NerdDinner правила сопоставления по умолчанию подходят для классов модели данных, создаваемых и можно просто использовать поведение по умолчанию.

### <a name="nerddinnerdatacontext-class"></a>Класс NerdDinnerDataContext

Visual Studio автоматически создает классы .NET, представляющие моделей и связей базы данных, определенных с помощью LINQ to SQL конструктор. Для каждого файла LINQ to SQL конструктора добавлен в решение также будет создаваться класс LINQ to SQL DataContext. Так как мы назвали наш LINQ to SQL классов: элемент «NerdDinner», класса DataContext, создан будет вызываться «NerdDinnerDataContext». Этот класс NerdDinnerDataContext является основным способом, который мы будет взаимодействовать с базой данных.

Наш класс NerdDinnerDataContext предоставляет два свойства — «Ужинов» и «RSVPs -», представляющие двух таблиц, которые мы моделировать внутри базы данных. C# можно использовать для записи запросы LINQ к этим свойствам объектов Dinner и RSVP запроса и их извлечение из базы данных.

Следующий код демонстрирует создание экземпляра объекта NerdDinnerDataContext и выполняют запросы LINQ к его, чтобы получить последовательность ужинов, возникающие в будущем. Visual Studio предоставляет полную поддержку технологии intellisense при написании запроса LINQ и объектов, возвращаемых из него являются строго типизированными и также поддерживает intellisense.

![](build-a-model-with-business-rule-validations/_static/image9.png)

В дополнение к позволяет запрашивать объекты Dinner и RSVP, NerdDinnerDataContext также автоматически отслеживает все изменения, вносимые для компании Dinner и RSVP объекты, которые извлекаются через него впоследствии. Эту функцию можно использовать для легко сохранить изменения в базе данных — без необходимости написания явного обновления кода SQL.

Например в приведенном ниже коде показано, как использовать LINQ-запрос для получения объекта Dinner из базы данных, двумя свойствами, компании Dinner обновления и затем сохраните изменения в базе данных:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

В коде выше объекта NerdDinnerDataContext автоматически отслеживаться изменения свойств, внесенные в объект обед, полученного от него. Когда мы вызываем метод «SubmitChanges()», он выполнит соответствующие «UPDATE» инструкции SQL в базу данных, чтобы сохранить обновленные значения обратно.

### <a name="creating-a-dinnerrepository-class"></a>Создание класса DinnerRepository

Для небольших приложений иногда бывает нормальной контроллеров работают непосредственно с LINQ to SQL DataContext класса и внедрять запросы LINQ в контроллерах. Как приложения увеличиваются, однако такой подход становится громоздким сопровождение и тестирование. Он также может привести к нам дублирования те же запросы LINQ в нескольких местах.

Один из подходов, можно сделать проще в обслуживании и тестировании приложений является использование шаблона «repository». Класс репозитория позволяет инкапсулировать запроса данных и логику сохраняемости и абстрагируется детали реализации сохраняемости данных из приложения. Кроме того, что код приложения чище, используя шаблон репозитория можно упростить его для реализации хранилища данных в будущем она и помогает упростить модульное тестирование приложения без необходимости реальную базу данных.

Для нашего приложения NerdDinner мы определим класс DinnerRepository с ниже подписи:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Примечание: Далее в этой главе мы извлечение интерфейса IDinnerRepository от этого класса и включить внедрения зависимостей с ней на нашем контроллерах. Начнем с того Однако мы собираемся начните с простого и просто работать непосредственно с классом DinnerRepository.*

Для реализации этого класса, мы наших папку «Моделей» правой кнопкой мыши и выберите **Add -&gt;новый элемент** команды меню. В диалоговом окне «Добавление нового элемента» мы выберите шаблон «Класс» и назовите файл «DinnerRepository.cs»:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Затем мы можем реализовать наш класс DinnerRespository, с помощью следующего кода:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Извлечение, обновление, вставка и удаление с помощью класса DinnerRepository

Теперь, когда мы создали наш класс DinnerRepository, давайте рассмотрим несколько примеров кода, демонстрирующих часто встречающиеся задачи, что можно сделать с ней:

#### <a name="querying-examples"></a>Примеры запросов

Приведенный ниже код получает один обед, с использованием значения DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Приведенный ниже код извлекает все предстоящие ужинов и циклы над ними:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT и Update примеры

В приведенном ниже коде показано добавление двух новых ужинов. Дополнения и изменения в репозиторий не зафиксированы в базе данных, до вызова метода «Метод Save()» на нем. LINQ to SQL автоматически переносит все изменения в транзакцию базы данных —, поэтому все изменения вступают в силу, либо ни одна из них в случае сохраняет нашего репозитория на:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Приведенный ниже код получает существующий объект Dinner и изменяет ее два свойства. Изменения фиксируются в базе данных при вызове метода «Метод Save()» в репозитории:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Приведенный ниже код извлекает dinner и добавляет в него Подтверждать. Это делается с помощью коллекции кроме компании Dinner объекта, который LINQ to SQL создан для нас (так как между ними в базе данных имеется связь первичный ключ/внешний ключ). Это изменение сохраняется в базе данных как новой строки RSVP, при вызове метода «Метод Save()» в репозитории:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Пример удаления

Приведенный ниже код получает существующий объект ужин, а затем помечает оно будет удалено. При вызове метода «Метод Save()» в репозитории фиксируется удаления в базе данных:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Интеграция проверки и логику бизнес-правила с помощью классов модели

Интеграция проверки и бизнес-правила, что логика является ключевой частью любого приложения, которое работает с данными.

#### <a name="schema-validation"></a>Проверка схемы

При определении классов модели с помощью конструктор LINQ to SQL, типы данных свойств в классах модели данных соответствуют типы данных таблицы базы данных. Например: Если столбец «EventDate» в таблице ужинов «datetime», класс модели данных, созданные с LINQ to SQL будет иметь тип «DateTime» (это встроенный тип данных .NET). Это означает, что вы получите ошибки компиляции, если вы попытаетесь назначить целое число или логическое значение к нему из кода, и он автоматически возвращает ошибку при попытке выполнить неявное преобразование недопустимого строкового типа к нему во время выполнения.

LINQ to SQL будет автоматически обрабатывает escape-преобразование значений SQL при использовании строки — который помогает защитить вас от атак путем внедрения кода SQL, при его использовании.

#### <a name="validation-and-business-rule-logic"></a>Проверка и логику бизнес-правила

Проверка схемы полезны в качестве первого шага, но достаточно редко. Большинство реальных сценариях требуется возможность указать более широкие логику проверки, которая может охватывать несколько свойств, выполните код и часто имеют осведомленность о состоянии модели (например: она создается/обновления/удалена, или в состоянии доменный Например «архивирован»). Существует множество различных шаблонов и платформ, которые можно использовать для определения и применения правил проверки к классам модели, и существует несколько на основе .NET платформ, которые можно использовать для этого. Можно использовать практически любой из них в приложениях ASP.NET MVC.

В рамках наше приложение NerdDinner мы будем использовать шаблон относительно простой и эффективную где мы предоставляем IsValid свойство и метод GetRuleViolations() на объект модели нашей компании Dinner. Свойство IsValid возвращает значение true или false в зависимости от того, являются ли правила проверки и бизнес-все допустимые. Метод GetRuleViolations() возвращает список ошибок правило.

Мы реализуем IsValid и GetRuleViolations() для нашей компании Dinner модели путем добавления «разделяемый класс» наш проект. Разделяемые классы можно использовать дополнительные методы, свойства и события для классов, обслуживается конструктор VS (например, компания Dinner класс, созданный средством конструктор LINQ to SQL) и помогает избежать средство из фактической нашим кодом. Мы можно добавить наш проект новый разделяемый класс, щелкнув правой кнопкой папку \Models и затем выберите команду меню «Добавление нового элемента». Мы затем можно выбрать в диалоговом окне «Добавление нового элемента» шаблон «Класс» и назовите его Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Нажатие кнопки «Добавить» добавьте файл Dinner.cs наш проект и откройте его в интегрированной среде разработки. Затем можно реализовать в framework принудительного применения базовые правила и проверки с помощью кода:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Несколько примечаний к коду выше:

- Класс Dinner предшествовало ключевое слово «частичных» — это означает, объединяются с помощью класса создается и сохраняется по конструктор LINQ to SQL и компилируются в единый класс кода, содержащегося в ней.
- Класс RuleViolation является вспомогательный класс, который мы добавим к проекту, который позволяет нам предоставить дополнительные сведения о нарушение правила.
- Метод Dinner.GetRuleViolations() приводит к нашей проверки и бизнес-правила для вычисления (мы реализуем их чуть позже). Затем возвращается обратно последовательность RuleViolation объектов, которые содержат дополнительные сведения о любых ошибках правило.
- Свойство Dinner.IsValid предоставляет удобные вспомогательные свойство, указывающее, имеет ли объект Dinner любой active RuleViolations. Его можно заранее проверить, разработчик, использующий объект ужин в любое время (и не вызывает исключение).
- Разделяемый метод Dinner.OnValidate() — это обработчик, который предоставляет LINQ to SQL, позволяющий получать уведомления в любое время, когда объект Dinner будет сохраняться в базе данных. Наша выше реализация OnValidate() обеспечивает компании Dinner не RuleViolations до его сохранения. Если он находится в недопустимом состоянии, он выдает исключение, которое приведет к LINQ to SQL, чтобы прервать транзакцию.

Такой подход обеспечивает простой платформы, мы можете интегрировать проверки и бизнес-правила в. Теперь давайте добавим правила к нашему методу GetRuleViolations() ниже:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Мы используем функцию C# «yield return» для возвращения последовательности любой RuleViolations. Сначала проверяет шесть правил, которые выше просто устанавливают, что свойства строки на нашей компании Dinner не может быть пустым или равным NULL. Последнее правило более интересным и вызовы PhoneValidator.IsValidNumber() вспомогательный метод, можно добавить в свой проект, чтобы убедиться, что ContactPhone номер формат соответствует ужин в стране.

Мы можем использовать. Поддержка NET регулярного выражения для реализации этой поддержки проверки телефона. Ниже приведен простой реализации PhoneValidator, мы можем добавить наш проект, который позволит нам добавить проверки шаблон регулярного выражения конкретной страны:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Обработка проверки и нарушения бизнес логики

Теперь, когда мы добавили выше проверки и бизнес-правило кода, каждый раз, когда мы пытаемся создать или обновить обед, наши правила логики проверки оценивается и применяется.

Разработчики могут писать код как ниже, чтобы заблаговременно определить допустимость объекта компании Dinner и получить список всех нарушений в ней без создания исключений:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

При попытке сохранить ужин в недопустимом состоянии, когда мы вызываем метод Save() на DinnerRepository, будет вызвано исключение. Это происходит, так как LINQ to SQL автоматически вызывает разделяемый метод наших Dinner.OnValidate() до того, как он сохраняет изменения Dinner мы добавили код Dinner.OnValidate() вызывает исключение, если существует нарушения правил в компании Dinner. Можно перехватить это исключение и оперативно получать список нарушения для исправления:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Поскольку наши проверки и бизнес-правил реализованы в наш слой модели, а не в слое пользовательского интерфейса, они будут применены и используется во всех сценариях, в наше приложение. Мы можно позже изменить или добавить бизнес-правил и весь код, что работает с нашей компании Dinner объектов принимают его.

Наличие легко изменить бизнес-правила в одном месте, без необходимости изменения ripple на протяжении всего приложения и логика пользовательского интерфейса, является признаком качественно написанное приложение и это преимущество, платформу MVC помогает соблюдать.

### <a name="next-step"></a>Следующий шаг

У нас теперь есть модель, которая позволяет запрашивать и обновлять базу данных.

Теперь добавим некоторые контроллеры и представления в проект, который можно использовать для построения интерфейс пользовательского интерфейса HTML вокруг него.

> [!div class="step-by-step"]
> [Назад](create-a-database.md)
> [Вперед](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
