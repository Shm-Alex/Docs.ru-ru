---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Определение того, что файлы должны быть развертыванию (C#) | Документация Майкрософт
author: rick-anderson
description: Какие файлы должны быть развернуты из среды разработки в рабочую среду отчасти зависит ли нам было создано приложение ASP.NET...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ad759cefc372f6276ce1b16beea7282d9685ef82
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828802"
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Определение того, что файлы должны быть развертыванию (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) или [скачать PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Какие файлы должны быть развернуты из среды разработки в рабочую среду частично зависит от приложения ASP.NET была ли создана с помощью модели веб-узла или модели веб-приложения. Дополнительные сведения о этими двумя моделями проектов и влияние развертывания в модель проекта.


## <a name="introduction"></a>Вступление

Развертывание веб-приложение ASP.NET включает в себя копирование файлов, связанных с ASP.NET в среде разработки в рабочую среду. Файлы, относящиеся к ASP.NET включают разметка веб-страницы ASP.NET и файлы поддержки кода и на стороне клиента и сервера. Файлы поддержки на стороне клиента находятся эти файлы ссылается на веб-страницы и отправляются непосредственно в браузере — изображения, файлы CSS и JavaScript-файлы, например. Файлы поддержки на стороне сервера относятся устройства, которые используются для обработки запроса на стороне сервера. Сюда входят файлы конфигурации, веб-службы, файлы классов, типизированных наборов DataSet и LINQ к файлам SQL, среди прочего.

Вообще говоря все файлы поддержки на стороне клиента должны копироваться из среды разработки в рабочую среду, но копируются файлы поддержки на стороне сервера зависит от того, ли явно компиляции кода на стороне сервера в сборку ( `.dll` файл) или если возникают эти сборки автоматически созданный. В этом руководстве показан, что файлы должны быть развернуты при явно компиляции кода в сборку, вместо того, этот этап компиляции происходит автоматически.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Явную компиляцию и автоматическую компиляцию

Веб-страниц ASP.NET можно разделить декларативной разметки и исходного кода. Включает в себя часть декларативная разметка HTML, веб-элементы управления и синтаксис привязки данных; часть кода содержит обработчики событий, записанных в коде Visual Basic или C#. Части разметки и кода обычно разделяются на различные файлы: `WebPage.aspx` содержит декларативную разметку при `WebPage.aspx.cs` содержит код.

Рассмотрим страницу ASP.NET с именем Clock.aspx, содержащий элемент управления Label, свойство которого текст имеет значение текущей датой и временем, при загрузке страницы. Часть декларативная разметка (в `Clock.aspx`) будет содержать разметку для элемента управления Label Web -`<asp:Label runat="server" id="TimeLabel" />` — при ту часть кода (в `Clock.aspx.cs`) будет иметь `Page_Load` обработчика событий следующим кодом:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

В порядке для ядра для обслуживания запроса для этой страницы, в коде страницы ASP.NET ( `WebPage.aspx.cs` файл) сначала должен быть скомпилирован. Этой компиляции может произойти явным образом или автоматически.

Если компиляция происходит явно, то все приложение исходный код компилируется в одну или несколько сборок (`.dll` файлы) в приложения `Bin` каталога. Если компиляция происходит автоматически, а затем полученный в результате создан сборка является, по умолчанию расположены в `Temporary ASP.NET` папке, которую можно найти в `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;версии&gt;*, Несмотря на то, что это расположение настраивается с помощью [ `<compilation>` элемент](https://msdn.microsoft.com/library/s10awwz0.aspx) в `Web.config`. С явную компиляцию необходимо выполнить некоторые действия для компиляции кода приложения ASP.NET в сборку, и этот шаг выполняется перед развертыванием. С автоматической компиляции процесс компиляции происходит на веб-сервере, при первом обращении к ресурсу.

Независимо от того, какая модель компиляции используется, части, содержащей разметку для всех страниц ASP.NET ( `WebPage.aspx` файлы) должны быть скопированы в рабочую среду. С помощью явную компиляцию необходимо скопировать сборок в `Bin` , но не обязательно должны скопировать вверх фрагменты кода на страницы ASP.NET ( `WebPage.aspx.cs` файлов). С автоматической компиляции необходимо скопировать файлы часть кода, чтобы код присутствует и может быть скомпилирован с автоматически, при посещении страницы. Части, содержащей разметку каждой веб-страницы ASP.NET включает в себя `@Page` директив с атрибутами, которые указывают, скомпилирован ли соответствующий код страницы уже явно или должен ли он автоматически компилируется. Таким образом в рабочей среде можно без проблем работать с любой модели компиляции и не нужно применять параметры специальной настройки для указания, что используется явная или автоматической компиляции.

В таблице 1 перечислены различные файлы для развертывания при использовании явную компиляцию и автоматическую компиляцию. Обратите внимание, что, независимо от компиляции модели используется вы всегда следует развертывать сборки в `Bin` папку, если эта папка существует. `Bin` Папка содержит сборки, относящиеся к веб-приложения, включая скомпилированный исходный код при использовании модели явную компиляцию. `Bin` Каталог также содержит сборки из других проектов и открытым исходным кодом или сторонние сборки, возможно, вы используете, и они должны быть на рабочем сервере. Таким образом, как правило, скопируйте `Bin` папка вплоть до рабочей при развертывании. (Если вы используете модель автоматической компиляции и не используете все внешние сборки, то у вас не будет `Bin` directory — это нормально!)

| **Модели компиляции** | **Развертывание файла часть разметки?** | **Развернуть файл исходного кода?** | **Развертывание сборок в `Bin` каталог?** |
| --- | --- | --- | --- |
| Явную компиляцию | Да | Нет | Да |
| Автоматическую компиляцию | Да | Да | Да (если он существует) |

**Таблица 1.** какие файлы развертывания зависит от компиляции модели используется.

## <a name="taking-a-trip-down-memory-lane"></a>Используя поездку вниз Lane памяти

Какой подход компиляции используется, отчасти зависит, управление приложения ASP.NET в Visual Studio. С момента. NET появления в 2000 году, были внесены четыре разных версий Visual Studio — Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 и Visual Studio 2008. Visual Studio .NET 2002 и 2003 управляемых приложений ASP.NET с помощью *модели проекта веб-приложения*. Перечислены ключевые возможности модели проекта веб-приложения.

- Файлы, что состав проекта определяются в одном файле проекта. Все файлы, не определенные в файле проекта не считаются частью веб-приложения в Visual Studio.
- Использует явную компиляцию. Построение проекта компилирует файлы кода в проекте в одну сборку, которая помещается в `Bin` папку.

Когда корпорация Майкрософт выпустила Visual Studio 2005 они прекращена поддержка модели проекта веб-приложения и заменил его в модели проекта веб-сайта. Модель проекта веб-сайта выделиться благодаря предоставлению из модели проекта веб-приложения одним из следующих способов:

- Вместо того, только одним файлом проекта, указывающий, как файлы проекта, вместо этого используется файловая система. Иными словами все файлы в папке веб-приложения (или подпапках) являются частью проекта.
- Построение проекта в Visual Studio не создает сборку в `Bin` каталога. Вместо этого сборка проекта веб-сайта выводятся ошибки времени компиляции.
- Поддержка автоматическую компиляцию. Проекты веб-сайта обычно развертываются путем копирования разметку и исходный код в рабочую среду, несмотря на то, что код может быть предварительно скомпилированных (явную компиляцию).

Microsoft восстановлен модели проекта веб-приложения после его выпуска пакета обновления 1 для Visual Studio 2005. Тем не менее Visual Web Developer продолжает поддерживают только модель проекта веб-сайта. Хорошей новостью является то, что это ограничение было удалено со Visual Web Developer 2008 Service Pack 1. Сегодня можно создавать приложения ASP.NET в Visual Studio (и Visual Web Developer), с помощью модели проекта веб-приложения или модели проекта веб-сайта. У каждой модели есть свои преимущества и недостатки. Ссылаться на [введение в проекты веб-приложений: сравнение проектов веб-сайтов и проектов веб-приложений](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) сравнение обеих моделей и чтобы помочь вам решить, какие модели проекта лучше всего подходит для вашей ситуации.

## <a name="exploring-the-sample-web-application"></a>Изучение примера веб-приложения

В этом руководстве загружаемый файл содержит приложение ASP.NET под именем рецензий на книги. Веб-сайт имитирует веб-сайт хобби, кто-то могут быть созданы для совместного использования своей книге проверки с Интернет-сообщество. Это веб-приложение ASP.NET очень прост и состоит из следующих ресурсов:

- `Web.config`, файл конфигурации приложения.
- Главная страница (`Site.master`).
- Семь другие страницы ASP.NET: 

    - ~`/Default.aspx`— домашнюю страницу веб-узла.
    - ~`/About.aspx` -на страницу «О сайт».
    - ~`/Fiction/Default.aspx` -страницы со списком книг фантастикой, которые были проанализированы. 

        - ~`/Fiction/Blaze.aspx` -Обзор novel Ричард Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -страницы со списком книг по технологиям, которые были проанализированы. 

        - ~/`Tech/CYOW.aspx`-Обзор *Создание веб-узла*.
        - ~/`Tech/TYASP35.aspx` -Обзор *обучение самостоятельно ASP.NET 3.5 за 24 часа*.
- Три разных файла CSS в папке Styles.
- Четыре файлов изображений - Powered by ASP.NET логотип и изображения из крышку три проверенные книги — все в `Images` папку.
- Объект `Web.sitemap` файла, который определяет карту узла и используется для отображения меню в `Default.aspx` страниц в корневом каталоге и `Fiction` и `Tech` папки.
- Файл класса с именем `BasePage.cs` , определяющий базовый `Page` класса. Этот класс расширяет функциональность `Page` класс автоматически присваивая `Title` значение в зависимости от позиции страницы в карте веб-узла. По сути, любой класс фонового кода ASP.NET, расширяющий `BasePage` (а не `System.Web.UI.Page`) будет иметь ее название, присвоено значение в зависимости от его положения в карту узла. Например при просмотре ~ /`Tech/CYOW.aspx` странице заголовок имеет значение «Домашняя страница: технология: Создание веб-узла».

Рис. 1 показан снимок экрана рецензий веб-сайта, при просмотре через браузер. Здесь вы увидите страницу ~ /`Tech/TYASP35.aspx`, который просматривает книги *обучение самостоятельно ASP.NET 3.5 за 24 часа*. Навигации, которая охватывает верхней части страницы и в меню в левом столбце основаны на структуре карты узла, определенный в `Web.sitemap`. Изображение в правом верхнем углу является одним из обложки, образы находятся в `Images` папку. Внешний вид веб-узла определяются вместе с отдельно от файлов CSS в папке Styles пока всеобъемлющая макета страницы определен на главной странице правила стилей каскадных `Site.master`.


[![Книга рассматривает веб-сайт предлагает проверки на ассортимент заголовков](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Рис. 1.** книги просматривает веб-сайт предлагает проверки на ассортимент заголовков ([Просмотр полноразмерного изображения](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Это приложение не использует базы данных. Каждая проверка реализуется как отдельный веб-страницы в приложении. В этом руководстве (и нескольких следующих учебных курсах) показано, как развертывание веб-приложения, не поддерживает базу данных. Тем не менее в следующем учебном курсе мы поможет повысить эффективность этого приложения для хранения проверок, комментарии и другие сведения в базе данных и изучим новые действия должны быть выполнены правильно развертывание управляемых данными веб-приложения.

> [!NOTE]
> Эти учебники сосредоточиться на размещении приложений ASP.NET с помощью веб-поставщик узла и не исследовать вспомогательные таким темам, как ASP. NET системы карты сайта или с помощью базового `Page` класса. Дополнительные сведения об этих технологиях и Дополнительные сведения о других разделы, которые описаны в этом руководстве обратитесь к разделу Дополнительные материалы, в конце каждого учебника.


Этот учебник загрузки имеет две копии веб-приложения, реализованы как разный тип проекта Visual Studio: BookReviewsWAP, проект веб-приложения и BookReviewsWSP, проект веб-сайта. Оба проекта были созданы с помощью Visual Web Developer 2008 SP1 и используйте ASP.NET 3.5 с пакетом обновления 1. Для работы с эти проекты сначала распаковки содержимого на рабочий стол. Для открытия проекта веб-приложения (BookReviewsWAP), перейдите к папке BookReviewsWAP и дважды щелкните файл решения, `BookReviewsWAP.sln`. Для открытия проекта веб-сайта (BookReviewsWSP), запустите Visual Studio и затем в меню "файл" выберите параметр Открытие веб-сайта, перейдите к `BookReviewsWSP` папки на рабочем столе и нажмите кнопку ОК.


В двух других частях этого учебника рассмотрения какие файлы необходимо скопировать в рабочую среду, при развертывании приложения. Следующие два руководства - *[развертывание Your сайта с помощью FTP](deploying-your-site-using-an-ftp-client-cs.md)* и *[развертывание Your сайта с помощью Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -демонстрируют различные способы Скопируйте эти файлы в поставщике веб-размещения.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Определение файлов для развертывания для проекта веб-приложения

Модель проекта веб-приложения использует явную компиляцию - проекта исходный код компилируется в одну сборку, каждый раз, когда вы создадите приложение. Этой компиляции включает в себя файлы с выделенным кодом на страницы ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, и так далее), а также `BasePage.cs` класса. Результирующую сборку с именем BookReviewsWAP.dll и находится в приложения `Bin` каталога.

Рис. 2 показаны файлы, составляющие проекта книги проверки веб-приложения.


[![В обозревателе решений перечислены файлы, которые составляют проект веб-приложения](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Рис. 2**: обозревателе решений отображается список файлов, входящих в проект веб-приложения


Для развертывания приложения ASP.NET, разработанные с помощью start модели проекта веб-приложения, создав приложение таким образом, чтобы явным образом скомпилировать самый последний исходный код в сборку. Затем скопируйте следующие файлы в рабочей среде:

- Файлы, содержащие декларативная разметка для каждого ASP.NET страницы, например ~ /`Default.aspx`, ~ /`About.aspx`, и т. д. Кроме того копирование копии декларативная разметка для любой главных страниц и пользовательских элементов управления.
- Сборки (`.dll` файлы) в `Bin` папку. Необходимо скопировать файлы базы данных программы (`.pdb`) или файлы XML можно найти в `Bin` каталога.

Не нужно копировать файлы исходного кода на страницы ASP.NET в рабочей среде, а также вам нужно скопировать `BasePage.cs` файл класса.

> [!NOTE]
> Как показано на рис. 2, `BasePage` класс реализуется как файл класса в проекте, помещаются в папку с именем `HelperClasses`. При компиляции проекта кода в `BasePage.cs` файл компилируется вместе с классы фонового кода страницы ASP.NET в рамках единой сборки, `BookReviewsWAP.dll.` ASP.NET имеет специальную папку с именем `App_Code` , предназначен для хранения файлов классов для веб-узла Проекты сайта. Код в `App_Code` папки автоматически компилируется и поэтому не следует использовать с проектами веб-приложений. Вместо этого следует помещать файлы класса приложения в обычном папку с именем `HelperClasses`, или `Classes`, или другим подобным. Кроме того можно поместить файлы классов в отдельном проекте библиотеки классов.


В дополнение к копированию файлов разметки, связанных с ASP.NET и сборки в `Bin` папку, необходимо также скопировать файлы поддержки на стороне клиента - изображений и файлов CSS - а также другие файлы поддержки на стороне сервера, `Web.config` и `Web.sitemap`. Эти клиента и стороне сервера поддерживать файлы нужно скопировать в рабочей среде, независимо от того, используете ли вы явной или автоматической компиляции.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Определение файлов для развертывания файлов проекта веб-сайта

Модель проекта веб-сайта поддерживает автоматическую компиляцию, функция, отсутствующая при использовании модели проекта веб-приложения. WITH явную компиляцию необходимо скомпилировать исходный код проекта в сборку и скопируйте эту сборку в рабочей среде. С другой стороны с помощью автоматической компиляции вы просто скопировать исходный код в рабочей среде, и она компилируется средой выполнения по требованию, при необходимости.

Параметр меню сборки в Visual Studio присутствует в проекты веб-приложений и проектами веб-сайтов. Создание Web Application Projects компилирует исходный код проекта в одну сборку, расположенный в `Bin` каталог; Создание проекта веб-сайта проверяет наличие ошибок во время компиляции, но не создает все сборки. Для развертывания приложения ASP.NET, разработанных с помощью всех веб-сайт проекта модели, необходимо выполнить является Скопируйте соответствующие файлы в рабочую среду, но я предлагаю вам для первой сборки проекта, чтобы убедиться, что отсутствуют ошибки времени компиляции.

Рис. 3 показаны файлы, составляющие проекта веб-сайта проверки книги.


 [![В обозревателе решений перечислены файлы, которые составляют проект веб-сайта](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Рис. 3**: обозревателе решений отображается список файлов, входящих в проект веб-сайта


Развертывание проекта веб-сайта включает копирование всех файлов, связанных с ASP.NET в рабочей среде —, содержит разметку страницы для страниц ASP.NET, главные страницы и пользовательские элементы управления, вместе с соответствующими файлами кода. Также необходимо скопировать файлы любой класс, например BasePage.cs. Обратите внимание, что `BasePage.cs` файл находится в `App_Code` папку, которая является специальной папке ASP.NET, используемым в проектах веб-сайта для файлов классов. Эта особая папка должна быть создана в рабочей среде, а также как файлы классов в `App_Code` необходимо скопировать папку в среде разработки `App_Code` папку в рабочей среде.

В дополнение к копированию файлов ASP.NET разметки и исходного кода, необходимо также скопировать файлы поддержки на стороне клиента - изображений и файлов CSS - а также другие файлы поддержки на стороне сервера, `Web.config` и `Web.sitemap`.

> [!NOTE]
> Проекты веб-сайта можно также использовать явную компиляцию. Следующем учебном курсе будет рассмотрено явно компиляции проекта веб-сайта.


## <a name="summary"></a>Сводка

Развертывание приложения ASP.NET влечет за собой, скопировать необходимые файлы из среды разработки в рабочую среду. Точный набор файлов, которые должны быть синхронизированы зависит от того, код приложения ASP.NET явным образом или автоматически скомпилирован ли. Стратегия компиляции влияют настроено ли в Visual Studio для управления приложения ASP.NET с помощью модели проекта веб-приложения или модели проекта веб-сайта.

Модель проекта веб-приложения использует явную компиляцию и компилирует код проекта в одной сборке в `Bin` папку. При развертывании приложения, часть разметки страницы ASP.NET и содержимое `Bin` папку необходимо достаточно передать в рабочую среду; исходный код в приложении - файлы кода и кода классов, например - не обязательно для копирования в рабочую среду.

Модель проекта веб-узел использует автоматическую компиляцию по умолчанию, несмотря на то, что имеется возможность явно компиляции проекта веб-сайта, как мы увидим в последующих руководствах. Развертывание приложения ASP.NET, использующего автоматическую компиляцию требует части, содержащей разметку *и* исходного кода должны копироваться в рабочую среду. Код автоматически компилируется в рабочей среде, когда он запрашивается в первый раз.

Теперь, когда мы рассмотрели, что файлы должны синхронизироваться между средами разработки и эксплуатации мы готовы развернуть приложение рецензий на поставщике веб-размещения.

Счастливого вам программирования!

### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Общие сведения о компиляции в ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Пользовательские элементы управления ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Проверка ASP. Переходы по узлу NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Введение в проекты веб-приложений](https://msdn.microsoft.com/library/aa730880.aspx)
- [Учебники по странице master](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Совместное использование кода на нескольких страницах](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [С помощью пользовательский базовый класс для классов фонового кода страницы ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Система проектов Visual Studio 2005 веб-сайта: что это такое и зачем мы сделали его?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Пошаговое руководство: Преобразование проекта веб-сайта в проект веб-приложения в Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Назад](asp-net-hosting-options-cs.md)
> [Вперед](deploying-your-site-using-an-ftp-client-cs.md)
