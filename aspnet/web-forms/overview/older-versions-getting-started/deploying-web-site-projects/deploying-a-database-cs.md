---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Развертывание базы данных (C#) | Документация Майкрософт
author: rick-anderson
description: Развертывание веб-приложения ASP.NET влечет за собой, получение необходимые файлы и ресурсы из среды разработки в рабочую среду. Для da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: ca9ce2b41cfd10504304c30bc965e446a7188120
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836616"
---
<a name="deploying-a-database-c"></a>Развертывание базы данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) или [скачать PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Развертывание веб-приложения ASP.NET влечет за собой, получение необходимые файлы и ресурсы из среды разработки в рабочую среду. Для управляемых данными веб-приложений это включает в себя схему и базы данных. Это руководство представляет собой первый в серии, рассматриваются шаги, необходимые для успешного развертывания базы данных из среды разработки в рабочую среду.


## <a name="introduction"></a>Вступление

Развертывание веб-приложения ASP.NET влечет за собой, получение необходимые файлы и ресурсы из среды разработки в рабочую среду. В ходе предыдущих шесть учебных курсах мы рассмотрели развертывание простого веб-приложения рецензий на книги. Этот демонстрационный сайт был включает ряд ресурсов на сервере - страниц ASP.NET, файлы конфигурации, `Web.sitemap` файл и т. д. — наряду с ресурсами на стороне клиента, такие как изображения и файлы CSS. Но как насчет управляемых данными веб-приложений? Какие дополнительные шаги необходимо выполнить для развертывания веб-приложения, использующего базу данных?

На нескольких следующих учебных курсах мы поговорим об шаги, необходимые для развертывания управляемых данными веб-приложения. Этот учебник начинает проверять как получить схему базы данных s и содержимое из среды разработки в рабочую среду в при следующем руководстве просматривает все изменения необходимые настройки. После того, что мы рассмотрим проблемы развертывания базы данных, использующего службы приложений (членства, ролей, профиля и т. д.).

## <a name="examining-the-updated-book-reviews-web-application"></a>Проверка обновленных книги проверки веб-приложения

Чтобы продемонстрировать развертывание управляемых данными веб-приложения, я ve обновлены рецензий веб-приложения с простой статический веб-сайта в один, управляемых данными. Как и ранее, существуют две версии приложения в этом учебнике s: одна с помощью модели проекта веб-приложения, а второй, использующего модель проекта веб-сайта.

Обновленные обзоры книг веб-приложение использует [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) базы данных, которая хранится на сайте s `App_Data` папку (`~/App_Data/Reviews.mdf`). Если у вас есть SQL Server 2008 на компьютере установлена демонстрацию должно работать без ошибок. При наличии более старой версии SQL Server, вы можете либо установить бесплатного SQL Server 2008 Express Edition, или можно использовать базу данных загрузки сценариев, доступных в этом руководстве s самостоятельно создать базы данных.

`Reviews.mdf` База данных содержит четыре таблицы:

- `Genres` — включает в себя записи для каждого жанра, таких как технология, вымыслы и бизнеса.
- `Books` — включает в себя записи для каждой новой рецензии, столбцы, такие как `Title`, `GenreId`, `ReviewDate`, и `Review`, среди прочего.
- `Authors` — включает в себя сведения о каждого автора, который добавил проверенные книгу.
- `BooksAuthors` -Таблица соединения многие ко многим, которая определяет, какие характеризуются какие книги.
  

Рис. 1 показана схема аварийного восстановления из этих четырех таблиц.


[![S книги проверки веб-приложение базы данных всегда состоит из четырех таблиц](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Рис. 1**: s книги проверки веб-приложение базы данных является состоит из четырех таблиц ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image3.jpg))


Предыдущую версию веб-сайт рецензий имели отдельную страницу ASP.NET для каждой книги. Например, существовала страница с именем `~/Tech/TYASP35.aspx` , содержащей проверки *обучение самостоятельно ASP.NET 3.5 за 24 часа*. В новой версии управляемых данными веб-сайта будет обзоры, хранящихся в базе данных и одной страницы ASP.NET, Review.aspx?ID=*bookId*, которая отображает проверки для указанной книги. Кроме того, имеется Genre.aspx?ID=*genreId* страницу, которая перечисляет проверенные книг заданного жанра.

Рис. 2 и 3 демонстрируют `Genre.aspx` и `Review.aspx` страницы в действии. Обратите внимание, URL-адрес в адресной строке для каждой страницы. На рисунке 2 ИТ s Genre.aspx? ID = 85d164ba-1123-4 c 47-82a0-c8ec75de7e0e. Так как 85d164ba-1123-4c47-82a0-c8ec75de7e0e `GenreId` значение жанр технологии, s заголовок операций чтения страниц «Рассматривает технологию» и маркированный список перечисляет отзывы пользователей на сайте, на которые распространяется действие этого жанра.


[![Страница технологии жанра](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Рис. 2**: на странице жанр технологии ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image6.jpg))


[![Рассмотрение приступите к изучению ASP.NET 3.5 на 24 часа](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Рис. 3**: проверка для *обучение самостоятельно ASP.NET 3.5 за 24 часа* ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image9.jpg))


Веб-приложение просматривает книги также включает раздел "Администрирование", где администраторы могут добавлять, изменять и удалить жанров, обзоры и создавать сведения. В настоящее время любой посетитель можно получить доступ к области администрирования. В следующем учебном курсе мы добавлена поддержка учетных записей пользователей и разрешен только авторизованным пользователям в страницы администрирования.

Если вы загрузите приложение рецензий Имейте в виду, что его цель — продемонстрировать развертывание приложения, управляемые данными. Оно не обеспечивает рекомендации, что касается разработки приложения. Например имеется не отдельный доступ уровня данным (DAL); на страницах ASP.NET напрямую взаимодействовать с базой данных через элемент управления SqlDataSource или кода ADO.NET в своих классов кода. Более подробно рассмотрим создание управляемых данными приложений с помощью многоуровневой архитектуры, см. в разделе my [ *работа с данными* учебники](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Базы данных для разработки и производства

При начале разработки управляемых данными веб-приложения необходимо указать строку подключения базы данных, которая предоставляет сведения о приложении о том, как соединиться с базой данных. Эта строка подключения указывает, среди прочего, сервер базы данных, имя базы данных и сведения о безопасности. Чаще всего используется приложением во время разработки базы данных отличается от базы данных используется, когда он s в рабочей среде. Далее перечислены преимущества использования разных баз данных для разработки и производства. Наличие разных базы данных в средство разработки вы Дон t нужно беспокоиться о случайное изменение или удаление динамических данных. Это также позволяет поместить в фиктивный тестовые данные или вносить критические изменения в модель данных, не заботясь о воздействии на приложения в рабочей среде. Недостатком при наличии в другую базу данных в средах разработки и эксплуатации является то, что когда приложение находится в базе данных и разворачивать любые необходимые изменения схемы базы данных s или данные также должны быть развернуты.

До первого развертывания имеется только один экземпляр базы данных, и этот экземпляр находится в среде разработки. При развертывании приложения в рабочей среде в первый раз мы не только скопировать необходимые файлы на сервере и на стороне клиента, но также скопировать базу данных из среды разработки в рабочую среду. Это где что мы готовы справа, теперь находится база данных с веб-приложением рецензий - в `App_Data` папку в нашей среде разработки, но имеет не еще была отправлена в рабочей среде.

Когда приложение будет развернуто существуют две копии базы данных. Мере разработки приложения, могут добавляться новые функции, создавая необходимость изменений в модель данных (такие как добавление новых столбцов к существующим таблицам, вносить изменения в существующие столбцы, добавление новых таблиц и т. д). Когда затем развертывается веб-приложения, изменения будут применены к базе данных в среде разработки с момента последнего развертывания, которые должны применяться в производственной базе данных. В следующем учебном курсе рассматриваются некоторые стратегии управления этот процесс. Это руководство посвящено развертыванию всей базы данных из среды разработки в рабочую среду.

## <a name="deploying-the-database-to-the-production-environment"></a>Развертывание базы данных в рабочей среде

Оставшейся части этого руководства рассматривается развертывание базы данных из среды разработки в рабочую среду. Если вы следуете вдоль вы должны чтобы убедиться в том, что ваша учетная запись, с помощью поставщика узла web включает Microsoft SQL Server база данных поддерживает. Также необходимо иметь некоторые сведения в одном месте, а именно: имя сервера базы данных, имя базы данных и имя пользователя и пароль, используемый для подключения к базе данных.

Как отмечалось ранее в этом учебнике, база данных веб-сайт s рецензий находится база данных SQL Server 2008 Express Edition, хранящиеся в `App_Data` папку. Он были получены в причине, что развертывание такой базы данных должно быть простым копированием `App_Data` папку из среды разработки в рабочую среду. Тем не менее большинство поставщиков веб-узла не поддерживают размещения баз данных в `App_Data` папку из-за соображений безопасности. Вместо этого веб-узлы указать учетную запись на сервере базы данных SQL Server в своей среде. Развертывание базы данных из среды разработки в рабочей среде требуется получить зарегистрирован s базы данных веб-узла сервера базы данных.

Так как вы получаете базы данных из среды разработки в рабочей среде? Существует несколько способов выполнения этой задачи в зависимости от того, какие службы вашего веб-узла предложения. С некоторых узлов, например компании DiscountASP.NET, можно FTP резервной копии базы данных или фактический `.mdf` файл на веб-сайт и восстановите файл резервной копии с помощью панели управления или прикрепить `.mdf` файл на сервер базы данных SQL Server. С помощью таких средств развертывания базы данных осуществлялось простым копированием `App_Data` папку для рабочей среды, а затем подключив его с помощью панели управления. Возможно это самый простой и быстрый способ публикации базы данных в первый раз.

Другой подход — использовать мастер публикации базы данных. Мастер публикации базы данных — это классическое приложение Windows, которое будет создавать команды SQL, чтобы создать схему базы данных s - таблицы, хранимые процедуры, представления, определяемые пользователем функции и т. д — и, возможно, данные в его таблиц. Можно затем подключитесь к серверу веб узла поставщика s базы данных через SQL Server Management Studio и затем выполните этот скрипт для копирования базы данных в рабочей среде. Еще лучше, если в поставщике веб-размещения поддерживает Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) имеется скрипт, созданный с помощью мастера публикации базы данных, автоматически выполняется на сервере базы данных от вашего имени. Поскольку мастер публикации базы данных создает скрипт, который создает схему базы данных s и данные, он будет работать независимо от того, ли на поставщике веб-размещения предлагает функции, например о прикреплении отправленного `.mdf` файл.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Создание команд SQL для создания схемы базы данных и данных с помощью мастера публикации базы данных

Позвольте s содержит информацию об использовании мастера публикации баз данных для развертывания базы данных рецензий на книги в рабочей среде. Если вы используете Visual Studio 2008 или более поздней, мастер публикации базы данных уже установлено. Если вы используете Visual Studio 2005, то сначала будет необходимо [загрузить и установить](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) мастера.

Откройте Visual Studio и перейдите к `Reviews.mdf` базы данных. Если вы используете Visual Web Developer, перейдите в обозреватель базы данных; Если вы используете Visual Studio, с помощью обозревателя серверов. Рис. 4 показан `Reviews.mdf` базы данных в обозревателе баз данных в Visual Web Developer. Как показано на рис. 4, `Reviews.mdf` база данных состоит из четырех таблиц, три хранимые процедуры и определяемые пользователем функции.


[![Найдите базу данных в обозревателе базы данных или обозревателя серверов](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Рис. 4**: найдите базу данных в обозреватель серверов или обозревателе баз данных ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image12.jpg))


Щелкните правой кнопкой мыши имя базы данных и выберите параметр «Опубликовать в поставщик» в контекстном меню. Откроется мастер публикации базы данных (см. рис. 5). Щелкните рядом с advance за пределами экрана-заставки.


[![Экран-заставка мастер публикации базы данных](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Рис. 5**: базы данных публикации мастера экран-заставка ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image15.jpg))


Второй экран в мастере перечислены доступные базы данных для мастера публикации баз данных и позволяет выбрать, следует ли в скрипт все объекты выбранной базы данных или выбрать, какие объекты в скрипт. Выберите соответствующую базу данных и оставьте включен параметр «Сценарий все объекты выбранной базы данных».

> [!NOTE]
> Если появляется ошибка «нет объектов в базе данных *databaseName* типов допускающий применение сценариев с помощью этого мастера» при щелчке следующий экран, показанный на рис. 6, убедитесь, что путь к файлу базы данных не слишком длинный. Было обнаружено, что эта ошибка может возникнуть, если указан слишком длинный путь к файлу базы данных.


[![Экран-заставка мастер публикации базы данных](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Рис. 6**: базы данных публикации мастера экран-заставка ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image18.jpg))


На следующем экране можно создать файл скрипта, или, если ваш веб-узел поддерживает его, опубликовать базу данных непосредственно на веб-узле поставщика s базы данных сервера. Как показано на рис. 7, у меня возникли скрипт, написанный в файл `C:\REVIEWS.MDF.sql`.


[![Скриптов для базы данных в файл или опубликовать на поставщике ваш веб-размещения](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Рис. 7**: скриптов для базы данных в файл или опубликовать его непосредственно в ваш поставщик веб-узла ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image21.jpg))


Последующие экрана запрашивает широкий набор параметров создания скриптов. Можно указать, должен ли скрипт включать инструкции drop для удаления этих существующих объектов. По умолчанию значение равно True, что подходит при развертывании базы данных в первый раз. Можно также указать, является ли целевой базы данных SQL Server 2000, SQL Server 2005 или SQL Server 2008. Наконец, можно указать, включать ли в скрипт схему и данные, только данные, или только схемы. Схема является коллекцией объектов базы данных, таблиц, хранимых процедур, представлений и т. д. Данные являются данные, хранимые в таблицах.

Как показано на рис. 8, я возникла мастера, настроен для удаления существующих объектов базы данных, чтобы создать скрипт для базы данных SQL Server 2008 и опубликовать схему и данные.


[![Укажите публикации параметров](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Рис. 8**: задать параметры публикации ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image24.jpg))


Окончательный двумя экранами приведены действия, которые будут выполнены и затем отобразить состояние создания скриптов. В результате выполнения мастера является то, что у нас есть файл скрипта, содержащего команды SQL, необходимые для создания базы данных в рабочей среде и заполнить его с теми же данными как на разработку.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Выполнение команд SQL в базе данных рабочей среды

Теперь, когда у нас есть скрипт, который содержит команды SQL для создания базы данных и его данных, остается является выполните скрипт в рабочей базе данных. Некоторые поставщики веб узла предлагают текстового поля в своей панели управления, где можно ввести команды SQL, выполняемые в базе данных. Если у вас есть очень большого файла скрипта, то этот параметр может не работать ( `REVIEWS.MDF.sql` файла скрипта является более 425 КБ, например).

Лучшим подходом является подключение непосредственно к рабочий сервер базы данных, с помощью SQL Server Management Studio (SSMS). Если у вас есть отличные от Express Edition из SQL Server на компьютере установлена затем уже есть установки SSMS. В противном случае вы можете [загрузить и установить](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) бесплатный экземпляр SQL Server Management Studio Express Edition.

Запустите среду SSMS и подключитесь к серверу базы данных web узла s с помощью сведений, предоставленных на поставщике веб-размещения.


[![Подключение к веб-узла поставщика s базы данных сервера](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Рис. 9**: подключения к данным поставщике веб-размещения s сервер базы данных ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image27.jpg))


Разверните вкладку баз данных и найдите свою базу данных. В левом верхнем углу панели инструментов нажмите кнопку Создать запрос, вставьте в команды SQL из файла скрипта, созданного с помощью мастера публикации базы данных и нажмите кнопку выполнения для выполнения этих команд на рабочем сервере базы данных. Если особенно большой файл сценария может занять несколько минут для выполнения команд.


[![Подключение к веб-узла поставщика s базы данных сервера](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Рис. 10**: подключения к данным поставщике веб-размещения s сервер базы данных ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image30.jpg))


Все, что s — его! На этом этапе разработки базы данных дублируется в рабочей среде. При обновлении базы данных в среде SSMS вы увидите, что новые объекты базы данных. Рис. 11 показана рабочие таблицы базы данных s, хранимые процедуры и определяемые пользователем функции, которые аналогичны операторам на базе данных разработки. И так как мы указали мастер публикации баз данных для публикации данных, базы данных s рабочие таблицы имеют те же данные, как таблицы s разработки базы данных во время выполнения мастера. Рис. 12 показан данные в `Books` таблицы в рабочей базе данных.


[![В рабочей базе данных были одинаковыми объекты базы данных](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Рис. 11**: база данных объекты имеют повторяется в рабочей базе данных ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image33.jpg))


[![Рабочая база данных содержит те же данные, как и в базе данных разработки](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Рис. 12**: рабочая база данных содержит те же данные в базе данных разработки ([Просмотр полноразмерного изображения](deploying-a-database-cs/_static/image36.jpg))


На этом этапе мы развернули только в базу данных разработки в рабочую среду. Мы еще не рассматривали развертывание самого веб-приложения или проверить необходимые изменения конфигурации для использовании в приложении на рабочих рабочей базы данных. В следующем учебном курсе мы рассмотрим эти проблемы!

## <a name="summary"></a>Сводка

Развертывание управляемых данными веб-приложения требуется копирование базы данных, используемой во время разработки в рабочую среду. Многие веб-узла поставщиками предоставляют средства для упрощения процесса развертывания базы данных. Например, с помощью компании DiscountASP.NET FTP базе `.mdf` файл (или резервной копии) и затем присоедините базу данных на сервер базы данных с помощью панели управления. Другой вариант, который работает независимо от того, какие функции вашего веб-узла поставщика предложения является средством мастер публикации базы данных s Майкрософт, которой для создания скрипта для команды SQL, чтобы создать схему s разработки базы данных и данные. После создания этот скрипт можно выполнить его на рабочую базу данных.

Теперь, когда s рецензий веб-приложения база данных находится в рабочей среде можно развернуть приложение. Тем не менее сведения о конфигурации для приложения s web указывает строку подключения к базе данных, а в базу данных разработки, ссылается на эту строку подключения. Нам нужно обновлять данные этой строки соединения во время развертывания узла в рабочей среде. Следующее руководство рассматривает эти различия в конфигурации и поможет выполнить действия, необходимые для публикации сайта рецензий на данных в рабочей среде.

Счастливого вам программирования!

#### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Скачайте Microsoft SQL Server Database, мастер 1.1 публикации](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Скачайте Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Назад](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Вперед](configuring-the-production-web-application-to-use-the-production-database-cs.md)
