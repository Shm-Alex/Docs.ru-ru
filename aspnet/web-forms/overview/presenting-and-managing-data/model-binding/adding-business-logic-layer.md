---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Добавление уровня бизнес-логики в проект, использующий привязки модели и веб-форм | Документация Майкрософт
author: tfitzmac
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 2c54c0a635c662c8740aa7f9b6bd02cd71913e24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838917"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Добавление уровня бизнес-логики в проект, использующий привязки модели и веб-форм
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource). Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.
> 
> Этом руководстве показано, как использовать привязки модели с слой бизнес-логики. Необходимо задать элемент OnCallingDataMethods, чтобы указать, что объект, отличный от текущей страницы используется для вызова методов данных.
> 
> Этот учебник основан на проект, созданный в [более ранних](retrieving-data.md) части серии.
> 
> Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект полностью на C# или Visual Basic. Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013. Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, в этом учебнике.


## <a name="what-youll-build"></a>Вы создадите

Привязка модели позволяет размещать код взаимодействия данных, либо в файле кода для веб-страницы или в класс отдельные бизнес-логики. Предыдущих руководствах показано, как использовать файлы с выделенным кодом для взаимодействия кода данных. Этот метод подходит для небольших узлов, но это может привести к кода повторения, а также более сложные при поддержке большой узел. Он также может быть очень сложно программным образом проверить код, который находится в коде программной части файлов, так как ни один уровень абстракции.

Для централизации кода взаимодействия данных, можно создать слой бизнес-логики, который содержит всю логику для взаимодействия с данными. Затем вызовите уровня бизнес-логики от веб-страниц. Этом руководстве показано, как переместить весь код, написанный в предыдущих руководствах в слой бизнес-логики, а затем использовать этот код на страницах.

В этом руководстве вам потребуется:

1. Переместить код из файлов кода в слой бизнес-логики
2. Изменение элементов управления с привязкой к данным для вызова методов на уровне бизнес-логики

## <a name="create-business-logic-layer"></a>Создание уровня бизнес-логики

Теперь вы создадите класс, который вызывается из веб-страниц. Методы в этом классе выглядеть методы, которые вы использовали в предыдущих учебных курсах и включают в себя значение поставщика атрибуты.

Во-первых, добавьте новую папку с именем **BLL**.

![Добавить папку](adding-business-logic-layer/_static/image1.png)

В папку BLL, создайте новый класс с именем **SchoolBL.cs**. Он будет содержать все операции с данными, которые первоначально находились в файлы с выделенным кодом. Методы являются почти одинаковыми, как методы в файле кода, но будет включать некоторые изменения.

Самое важное изменение, следует отметить, — это больше не выполняются в экземпляре код из **страницы** класса. Класс Page содержит **TryUpdateModel** метод и **ModelState** свойство. Этот код при перемещении в слой бизнес-логики, вы больше не иметь экземпляр класса страницы для вызова этих членов. Чтобы обойти эту проблему, необходимо добавить **ModelMethodContext** параметра к любому методу, который обращается к TryUpdateModel или ModelState. Используйте этот параметр ModelMethodContext для вызова TryUpdateModel или получить ModelState. Не требуется вносить изменения в веб-страницы, чтобы учесть этот новый параметр.

Замените код в SchoolBL.cs следующим кодом.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Изменить существующие страницы для получения данных из уровня бизнес-логики

Наконец страницы Students.aspx AddStudent.aspx и Courses.aspx преобразование из с помощью запросов в файл с выделенным кодом с помощью уровня бизнес-логики.

В файлы с выделенным кодом для учащихся, AddStudent и курсы удалите или закомментируйте следующие методы запроса:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Теперь требуется без кода в файл с выделенным кодом, относящиеся к операции с данными.

**OnCallingDataMethods** обработчик событий позволяет указать объект, используемый для методов данных. В Students.aspx добавьте значение для этого обработчика событий и измените имена методов данных на имена методов в класс бизнес-логики.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

В файле с выделенным кодом для Students.aspx Определите обработчик событий для события CallingDataMethods. В этом обработчике событий укажите класс бизнес-логики для данных операций.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

В AddStudent.aspx внести аналогичные изменения.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

В Courses.aspx внести аналогичные изменения.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Запустите приложение и обратите внимание на то, что все страницы работать так, как у него есть ранее. Логика проверки также работает правильно.

## <a name="conclusion"></a>Заключение

В этом руководстве повторно структуру приложения для использования уровня доступа к данным и уровня бизнес-логики. Вы указали, что элементы управления данными использовать объект, который не является текущей страницы для операции с данными.

> [!div class="step-by-step"]
> [Назад](using-query-string-values-to-retrieve-data.md)
