---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First с ASP.NET MVC: Создание веб-приложения и модели данных | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827595"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Database First с ASP.NET MVC: Создание веб-приложения и модели данных
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных. Созданный код соответствует столбцам в таблице базы данных.
> 
> Части этой серии рассматривается создание веб-приложения и создание моделей данных, основанных на таблицах базы данных.


## <a name="create-a-new-aspnet-web-application"></a>Создание нового веб-приложения ASP.NET

В новое решение или в том же решении, что проект базы данных, создайте новый проект в Visual Studio и выберите **веб-приложение ASP.NET** шаблона. Назовите проект **ContosoSite**.

![Создание проекта](creating-the-web-application/_static/image1.png)

Нажмите кнопку **ОК**.

В окне "новый проект ASP.NET", выберите **MVC** шаблона. Можно снять **разместить в облаке** параметр сейчас, так как вы развернете приложение в облако позднее. Нажмите кнопку **ОК** для создания приложения.

![Выберите шаблон mvc](creating-the-web-application/_static/image2.png)

Создается проект с по умолчанию файлы и папки.

В этом руководстве используется Entity Framework 6. Можно еще раз проверьте версии Entity Framework в проекте в окне «Управление пакетами NuGet». При необходимости обновите версию Entity Framework.

![Показать версию](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Создание моделей

Теперь вы создадите модели Entity Framework из таблиц базы данных. Эти модели представляют собой классы, которые будут использоваться для работы с данными. Каждая модель отражает таблицы в базе данных и содержит свойства, которые соответствуют столбцам в таблице.

Щелкните правой кнопкой мыши **моделей** и выберите **добавить** и **новый элемент**.

![Добавление нового элемента](creating-the-web-application/_static/image4.png)

В окне «Добавление нового элемента» выберите **данных** в левой области и **ADO.NET Entity Data Model** из параметров в центральной области. Назовите новый файл модели **ContosoModel**.

![Создание модели](creating-the-web-application/_static/image5.png)

Нажмите кнопку **Добавить**.

В мастере модели EDM выберите **конструктор EF из базы данных**.

![Создать из базы данных](creating-the-web-application/_static/image6.png)

Нажмите кнопку **Далее**.

При наличии подключения базы данных, определенные в вашей среде разработки, может появиться одно из этих подключений предварительно выбраны. Тем не менее вы хотите создать новое подключение к базе данных, созданной в первой части этого руководства. Нажмите кнопку **новое подключение** кнопки.

![подключение к базе данных](creating-the-web-application/_static/image7.png)

В окне «Свойства подключения» введите имя локального сервера, где она была создана (в данном случае **\ProjectsV12 (localdb)**). После ввода имени сервера, выберите ContosoUniversityData из доступных баз данных.

![Задание свойств соединения](creating-the-web-application/_static/image8.png)

Нажмите кнопку **ОК**.

Теперь будут показаны свойства подключения. Можно использовать имя по умолчанию для подключения в файле Web.Config

![параметры подключения](creating-the-web-application/_static/image9.png)

Нажмите кнопку **Далее**.

Выберите **таблиц** для создания моделей для всех трех таблиц.

![Выбор таблиц](creating-the-web-application/_static/image10.png)

Нажмите кнопку **Готово**.

Если появляется предупреждение системы безопасности, выберите **ОК** для продолжения выполнения шаблона.

Модели создаются на основе таблиц базы данных и отображения диаграммы, показывающий свойства и связи между таблицами.

![Схема модели](creating-the-web-application/_static/image11.png)

Папку Models теперь включает много новых файлов, связанные с моделями, которые были созданы из базы данных.

![Показать новые файлы модели](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** файл содержит класс, производный от **DbContext** класса, а также предоставляет свойство для каждого класса модели, который соответствует таблице базы данных. **Course.cs**, **Enrollment.cs**, и **Student.cs** файлы содержат классы моделей, которые представляют таблицы базы данных. Класс контекста и классы моделей будет использовать при работе с помощью формирования шаблонов.

Прежде чем продолжить с этим руководством, выполните сборку проекта. В следующем разделе вы создадите код на основе моделей данных, но этот раздел не будет работать, если проект не построен.

> [!div class="step-by-step"]
> [Назад](setting-up-database.md)
> [Вперед](generating-views.md)
