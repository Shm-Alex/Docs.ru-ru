---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Создание классов модели в LINQ to SQL (C#) | Документация Майкрософт
author: microsoft
description: Этот учебник призван объяснить одним из способов создание классов модели для приложения ASP.NET MVC. В этом руководстве вы узнаете, как для построения модели c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827817"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Создание классов модели в LINQ to SQL (C#)
====================
по [Microsoft](https://github.com/microsoft)

[Загрузить PDF-файл](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Этот учебник призван объяснить одним из способов создание классов модели для приложения ASP.NET MVC. В этом руководстве вы узнаете, как создать классы модели и выполнять доступ к базе данных, используя преимущества Microsoft LINQ to SQL.


Этот учебник призван объяснить одним из способов создание классов модели для приложения ASP.NET MVC. В этом руководстве вы узнаете, как создать классы модели и выполнять доступ к базе данных, используя преимущества Microsoft LINQ to SQL

В этом руководстве мы создать базовое приложение базы данных фильмов. Мы начнем с создания приложения базы данных Movie в быстрым и простым способом невозможно. Все наши доступ к данным выполнять непосредственно из наших действий контроллера.

Далее вы узнаете, как использовать шаблон репозитория. Чтобы использовать шаблон репозитория требует немного больше работы. Тем не менее, перехода на этот шаблон удобен тем, что она позволяет создавать приложения, для адаптации к изменить и можно легко проверить.

## <a name="what-is-a-model-class"></a>Что такое класс модели?

Модель MVC содержит всю логику приложения, не содержащийся в представлении MVC или контроллер MVC. В частности модель MVC содержит все логики доступа к данным и бизнес-приложения.

Можно использовать множество различных технологий для реализации логики доступа к данным. Например можно создать классы данных доступа с помощью классов Microsoft Entity Framework, NHibernate, Subsonic или ADO.NET.

В данном случае я использую LINQ to SQL для запроса и обновления базы данных. LINQ to SQL предоставляет очень простой метод взаимодействия с базой данных Microsoft SQL Server. Тем не менее важно понимать, что платформа ASP.NET MVC не привязан к LINQ to SQL любым способом. ASP.NET MVC совместим с любой технологии доступа к данным.

## <a name="create-a-movie-database"></a>Создание базы данных Movie

В этом руководстве--Чтобы проиллюстрировать, как создать классы модели — мы создать простое приложение базы данных фильмов. Первым шагом является создание новой базы данных. Щелкните правой кнопкой мыши приложение\_папка данных в окно обозревателя решений и выберите пункт меню **добавить, новый элемент**. Выберите **базы данных SQL Server** шаблон, присвойте ему имя MoviesDB.mdf и нажмите кнопку **добавить** кнопку (см. рис. 1).


[![Добавление новой базы данных SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Рис 01**: Добавление новой базы данных SQL Server ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


После создания новой базы данных, можно открыть базу данных, дважды щелкнув файл MoviesDB.mdf в приложении\_папку данных. Дважды щелкните файл MoviesDB.mdf открыть окно обозревателя сервера (см. рис. 2).

Окно обозревателя сервера называется окна обозревателя базы данных, при использовании Visual Web Developer.


[![В окне обозревателя серверов](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Рис. 02**: в окне обозревателя серверов ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Нам нужно добавить одной таблицы к базе данных, представляющий наших фильмов. Щелкните правой кнопкой мыши папку «таблицы» и выберите пункт меню **добавить новую таблицу**. Если выбрать этот пункт меню открывается в конструкторе таблиц (см. рис. 3).


[![В окне обозревателя серверов](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Рис 03**: конструктора таблиц ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Нам нужно добавить следующие столбцы в нашей таблице базы данных:

| **Имя столбца** | **Тип данных** | **Разрешить значения NULL** |
| --- | --- | --- |
| Идентификатор | Int | False |
| Заголовок | Nvarchar(200) | False |
| Директор | nvarchar(50) | False |

Необходимо выполнить два специальных действия со столбцом идентификаторов. Во-первых необходимо пометить столбец Id как столбец первичного ключа, выбрав столбец в конструкторе таблиц и щелкнув значок ключа. LINQ to SQL требует указания первичного ключа, при выполнении вставляет или обновляет в базе данных.

Далее необходимо обозначить столбец идентификатора как столбец идентификаторов, следует назначить Да для **Is Identity** свойство (см. рис. 3). Столбец Identity является столбец, который будет назначен новый номер автоматически каждый раз при добавлении новой строки данных в таблицу.

## <a name="create-linq-to-sql-classes"></a>Создание классов LINQ to SQL

Наша модель MVC будет содержать LINQ к SQL классам, представляющим tblMovie таблицы базы данных. Самый простой способ создать эти классы LINQ to SQL — щелкните папку Models правой кнопкой мыши **добавить, новый элемент**выберите LINQ to SQL Classes шаблон, присвойте имя Movie.dbml классы и нажмите кнопку **добавить**кнопку (см. рис. 4).


[![Создание LINQ для классов SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Рис. 04**: создание классов LINQ to SQL ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Сразу после создания фильма классам LINQ to SQL, откроется реляционный конструктор объектов. Таблицы базы данных можно перетащить из окна обозревателя серверов в реляционный конструктор объектов для создания классов LINQ to SQL, представляющих таблицы конкретной базы данных. Нам нужно добавить таблицу базы данных tblMovie на реляционный конструктор объектов (см. рис. 5).


[![С помощью реляционного конструктора объектов](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**05 рис**: с помощью реляционного конструктора объектов ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


По умолчанию реляционный конструктор объектов создает класс с именем то же самое, что в таблице базы данных, перетащите в конструктор. Тем не менее, мы не будем вызывать наш класс `tblMovie`. Таким образом щелкните имя класса в конструкторе и измените имя класса для фильмов.

Наконец, не забудьте нажать **Сохранить** кнопку для сохранения LINQ to SQL Classes (изображение дискеты). В противном случае LINQ to SQL Classes не будет создаваться реляционный конструктор объектов.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Использование LINQ to SQL в действие контроллера

Теперь, когда у нас есть наших классов LINQ to SQL, эти классы можно использовать для получения данных из базы данных. В этом разделе вы узнаете, как использовать LINQ для классов SQL непосредственно в действие контроллера. Мы будем отображать список фильмов tblMovies таблицы базы данных в представлении MVC.

Во-первых нам нужно изменить в класс HomeController. Этот класс можно найти в папке Controllers приложения. Измените класс, чтобы он выглядел классу в листинге 1.

**Листинг 1. `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Действие в листинге 1 использует LINQ to SQL DataContext класс ( `MovieDataContext`) для представления `MoviesDB` базы данных. `MoveDataContext` Был создан класс с Visual Studio реляционный конструктор объектов.

Выполнение запросов LINQ применительно к DataContext, чтобы получить все фильмы из `tblMovies` таблицы базы данных. Список фильмов назначается локальной переменной с именем `movies`. Наконец список фильмов передается в представление данных представления.

Чтобы можно было отобразить в кино, мы Далее необходимо изменение представления индекса. Можно найти в представление Index в `Views\Home\` папку. Обновление представления индекса, чтобы он выглядел подобно представлению в листинге 2.

**Листинг 2. `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Обратите внимание, что измененный представление индекса содержит `<%@ import namespace %>` директив в верхней части представления. Эта директива импортирует `MvcApplication1.Models namespace`. Мы должны это пространство имен для работы с `model` классы — в частности, `Movie` класс — в представлении.

Представление в листинге 2 содержит `foreach` цикла итерации всех элементов, представленных `ViewData.Model` свойство. Значение `Title` свойство отображается для каждого `movie`.

Обратите внимание, что значение `ViewData.Model` свойство приводится к `IEnumerable`. Это необходимо для перебора содержимого `ViewData.Model`. Другой вариант — это создание строго типизированного `view`. При создании строго типизированного `view`, вы приводите `ViewData.Model` свойства для конкретного типа в классе фонового кода представления.

При запуске приложения после изменения `HomeController` класс и индекс представления, то вы получите пустую страницу. Вы получите пустую страницу, так как нет записей фильмов в `tblMovies` таблицы базы данных.

Чтобы добавить записи в `tblMovies` таблица базы данных, щелкните правой кнопкой мыши `tblMovies` таблица в окне обозревателя серверов (окно обозревателя базы данных в Visual Web Developer) базы данных и выберите пункт меню Показать таблицу данных. Вы можете вставить `movie` записей с помощью сетки, который отображается (см. рис. 6).


[![Вставка видео](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Рис 06**: вставка фильмов ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


После добавления некоторых записей базы данных, чтобы `tblMovies` таблицы и запустить приложение, вы увидите страницу, на рис. 7. Все записи базы данных фильмов, отображаются в виде маркированного списка.


[![Отображение видео в представлении индекса](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Рис 07**: отображение фильмов в представлении индекса ([Просмотр полноразмерного изображения](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Чтобы использовать шаблон репозитория

В предыдущем разделе мы использовали LINQ для классов SQL непосредственно в действие контроллера. Мы использовали `MovieDataContext` класса непосредственно из `Index()` действия контроллера. Нет ничего плохого таким образом в случае простого приложения. Тем не менее работа напрямую с помощью LINQ to SQL в класс контроллера создает проблемы, когда вам нужно создать более сложное приложение.

С помощью LINQ to SQL в класс контроллера затрудняет для переключения технологии доступа к данным в будущем. Например можно перейти от использования Microsoft LINQ to SQL с помощью Microsoft Entity Framework в качестве вашей технологии доступа к данным. В этом случае необходимо переписать каждый контроллер, который обращается к базе данных в приложении.

С помощью LINQ to SQL в класс контроллера также усложняет задачу создания модульных тестов для приложения. Как правило вы не хотите взаимодействовать с базой данных при выполнении модульных тестов. Вы хотите использовать модульные тесты для тестирования логики приложения и не серверу базы данных.

Для создания приложения MVC более адаптируемым для будущих изменений и который может быть проще тестировать, можно использовать шаблон репозитория. При использовании шаблона репозитория, создается класс отдельный репозиторий, который содержит всю логику доступа к вашей базы данных.

При создании класс репозитория, создается интерфейс, представляющий все методы, используемые классом репозитория. В контроллерах при написании кода для интерфейса, а не репозитории. Таким образом, вы можете реализовать репозиторий, используя технологии доступа к данным различных в будущем.

Интерфейс в листинге 3 называется `IMovieRepository` и представляет один метод с именем `ListAll()`.

**Листинг 3. `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Реализует класс репозитория в листинге 4 `IMovieRepository` интерфейс. Обратите внимание на то, что он содержит метод с именем `ListAll()` , соответствующий методу, требующегося `IMovieRepository` интерфейс.

**Листинг 4. `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Наконец `MoviesController` класс в листинге 5 использует шаблон репозитория. Она больше не использует LINQ для классов SQL напрямую.

**Листинг 5. `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Обратите внимание, что `MoviesController` класс в листинге 5 имеет два конструктора. Первый конструктор конструктор без параметров, вызывается в том случае, когда приложение запущено. Этот конструктор создает экземпляр класса `MovieRepository` класса и передает его второй конструктор.

Второй конструктор имеет один параметр: `IMovieRepository` параметра. Этот конструктор просто присваивает значение параметра поле уровня класса с именем `_repository`.

`MoviesController` Класс использует преимущества программного обеспечения шаблон, который называется шаблон внедрения зависимостей. В частности она использует так называемую внедрение зависимостей конструктора. Дополнительные сведения об этой модели, ознакомьтесь со следующей статьей по (Martin Fowler):

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Обратите внимание, что весь код в `MoviesController` класса (за исключением первый конструктор) взаимодействует с `IMovieRepository` интерфейс вместо фактического `MovieRepository` класса. Код взаимодействует с абстрактный интерфейс вместо конкретную реализацию интерфейса.

Если вы хотите изменить технологии доступа к данным, используемые приложением, то можно просто реализовать `IMovieRepository` интерфейс с классом, который использует технологию доступа к альтернативной базы данных. Например, можно создать `EntityFrameworkMovieRepository` класса или `SubSonicMovieRepository` класса. Так как класс контроллера запрограммированы в интерфейсе, вы можете передать новую реализацию `IMovieRepository` к контроллеру и класс будет продолжать работать.

Кроме того Если вы хотите протестировать `MoviesController` класса, то можно передать класс репозитория с фиктивными фильма к `HomeController`. Вы можете реализовать `IMovieRepository` класс с классом, который не фактически доступа к базе данных, но содержит все необходимые методы `IMovieRepository` интерфейс. Таким образом, вы можете модульный тест `MoviesController` класс без фактически обращения к реальной базой данных.

## <a name="summary"></a>Сводка

Цель этого учебника было продемонстрировать, как можно создавать классы модели MVC, используя преимущества Microsoft LINQ to SQL. Мы рассмотрели две стратегии для отображения данных базы данных в приложении ASP.NET MVC. Во-первых мы создали LINQ для классов SQL и используемые классы непосредственно в действие контроллера. Использование LINQ для классов SQL в контроллере позволяет быстро и легко отображать данные базы данных в приложении MVC.

Далее мы изучили немного сложнее, но определенно более эффективный путь для отображения данных базы данных. Мы воспользовались преимуществами шаблона репозитория и поместить все наши логики доступа к базе данных в отдельном репозитории класс. В нашем контроллере мы написали все часть нашего кода на основе интерфейса вместо конкретного класса. Шаблона репозитория удобен тем, что позволяет легко изменять технологий доступа к базе данных в будущем, и это позволяет нам легко тестировать наших классов контроллеров.

> [!div class="step-by-step"]
> [Назад](creating-model-classes-with-the-entity-framework-cs.md)
> [Вперед](displaying-a-table-of-database-data-cs.md)
