---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления базы данных | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 5c9b0c71e2e0d35645e975e9adb7086e65bcf4c3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839172"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления базы данных
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010. Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).


## <a name="overview"></a>Обзор

В этом руководстве внесенные изменения базы данных и изменения кода, тестирования изменений в Visual Studio, а затем развернуть обновление в средах тестирования, промежуточной и рабочей среде.

В этом руководстве сначала рассматривается для обновления базы данных, которая управляется Code First Migrations и позже он показывает, как обновить базу данных, используя поставщик dbDacFx.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Развертывание обновления базы данных с помощью Code First Migrations

В этом разделе, добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущностей. Затем вы обновите со страницей, отображающей данные instructor, чтобы он отображал нового столбца. Наконец необходимо выполнить развертывание изменений для тестирования, промежуточной и рабочей среде.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Добавить столбец в таблицу в базе данных приложения

1. В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класс (должно быть два закрывающей фигурных скобок после нее):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Затем обновите `Seed` метода, так что он содержит значение для нового столбца. Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, который содержит информацию о дате рождения:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна. Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проект по умолчанию**.
3. В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проект по умолчанию**, а затем введите следующую команду:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода вы увидите код, создающий новый столбец. `Up` Метод создает столбец при внесении изменений и `Down` метод удаляет столбец, когда выполняется откат изменений.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Выполните сборку решения, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что проект ContosoUniversity.DAL по-прежнему выбран):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Платформа Entity Framework выполняет `Up` метод, а затем выполняет `Seed` метод.

### <a name="display-the-new-column-in-the-instructors-page"></a>Отображение нового столбца на странице преподавателей

1. В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для отображения даты рождения. Между для них назначения даты и office приема на работу, добавьте ее.

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Если отступов кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL-D для автоматически переформатирует файл.)
2. Запустите приложение и нажмите кнопку **преподавателей** ссылку.

    При загрузке страницы, вы увидите, что у него есть новое поле даты рождения.

    ![Странице "Instructors" с даты рождения](deploying-a-database-update/_static/image2.png)
3. Закройте браузер.

### <a name="deploy-the-database-update"></a>Развертывание обновления базы данных

1. В **обозревателе решений** выберите проект ContosoUniversity.
2. В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **теста** профиль публикации, а затем нажмите кнопку **веб-публикации**. (Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)

    Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница.
3. Запустите **преподавателей** страницу, чтобы убедиться, что обновление было успешно развернуто.

    Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод. Когда отобразится страница, вы увидите ожидаемый **Дата рождения** столбец с датами в нем.
4. В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикации**.
5. Запустите **преподавателей** страницы в промежуточной среде, чтобы убедиться, что обновление было успешно развернуто.
6. В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **рабочей** профиль публикации, а затем нажмите кнопку **веб-публикации**.
7. Запустите **преподавателей** страницы в рабочей среде, чтобы убедиться, что обновление было успешно развернуто.

    Для обновления приложения реальной рабочей среде, который включает в себя изменение базы данных также обычно выполняемые приложения в автономный режим во время развертывания с помощью *приложения\_offline.htm*, как показано в предыдущем учебном курсе.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Развертывание обновления базы данных, используя поставщик dbDacFx

В этом разделе вы добавите *комментарии* столбец *пользователя* таблицы в базе данных членства и создать страницу, которая позволяет отображать и редактировать комментарии для каждого пользователя. Затем необходимо выполнить развертывание изменений для тестирования, промежуточной и рабочей среде.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Добавить столбец в таблицу в базе данных членства

1. В Visual Studio откройте **обозреватель объектов SQL Server**.
2. Разверните **(localdb) \v11.0**, разверните **баз данных**, разверните **aspnet ContosoUniversity** (не **aspnet-ContosoUniversity-Prod**) а затем разверните **таблиц**.

    Если вы не видите **(localdb) \v11.0** под **SQL Server** узел, щелкните правой кнопкой мыши **SQL Server** узел и нажмите кнопку **добавить SQL Server**. В **соединение с сервером** введите диалоговое окно *(localdb) \v11.0* как **имя сервера**, а затем нажмите кнопку **Connect**.

    Если вы не видите **aspnet ContosoUniversity**, запустите проект и выполните вход с помощью *администратора* учетные данные (пароль *devpwd*) и обновите  **Обозреватель объектов SQL Server** окна.
3. Щелкните правой кнопкой мыши **пользователей** таблицы, а затем нажмите кнопку **конструктор представлений**.

    ![Конструктор представлений SSOX](deploying-a-database-update/_static/image3.png)
4. В конструкторе добавьте *комментарии* столбца и сделать его *nvarchar(128)* и допускает значения NULL и нажмите кнопку **обновления**.

    ![Добавление столбца комментарии](deploying-a-database-update/_static/image4.png)
5. В **Просмотр обновлений базы данных** выберите **обновления базы данных**.

    ![Просмотр обновлений базы данных](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Создание страницы для отображения и редактирования нового столбца

1. В **обозревателе решений**, щелкните правой кнопкой мыши **учетной записи** папка в проекте "ContosoUniversity", щелкните **добавить**, а затем нажмите кнопку **новый элемент** .
2. Создайте новый **страницу веб-формы с помощью главного** и назовите его *UserInfo.aspx*. Примите имя по умолчанию *Site.Master* файл в качестве главной страницы.
3. Скопируйте следующую разметку в `MainContent` `Content` элемент (последние 3 `Content` элементы):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Щелкните правой кнопкой мыши *UserInfo.aspx* и нажмите кнопку **просмотреть в браузере**.
5. Войти с помощью вашей *администратора* учетные данные пользователя (пароль *devpwd*) и добавить примечания для пользователя, чтобы убедиться, что страница работает правильно.

    ![Страница сведений о пользователе](deploying-a-database-update/_static/image6.png)
6. Закройте браузер.

## <a name="deploy-the-database-update"></a>Развертывание обновления базы данных

Чтобы развернуть, используя поставщик dbDacFx, необходимо просто выбрать **обновления базы данных** параметр в профиле публикации. Тем не менее, для первоначального развертывания при использовании этого параметра можно также настроить некоторые дополнительные скрипты SQL для выполнения: они по-прежнему в профиле, и необходимо предотвратить их повторный запуск.

1. Откройте **веб-публикации** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.
2. Выберите **теста** профиля.
3. Нажмите кнопку **параметры** вкладки.
4. В разделе **DefaultConnection**выберите **обновления базы данных**.
5. Отключите эти дополнительные сценарии, которые настроены на выполнение для первоначального развертывания:

    1. Нажмите кнопку **настроить обновление базы данных**.
    2. В **Настройка обновления базы данных** диалоговом окне снимите флажки рядом с полем *Grant.sql* и *aspnet-data-dev.sql*.
    3. Нажмите кнопку **Закрыть**.
6. Нажмите кнопку **предварительной версии** вкладки.
7. В разделе **баз данных** и справа от **DefaultConnection**, нажмите кнопку **предварительной версии базы данных** ссылку.

    ![Предварительная версия базы данных](deploying-a-database-update/_static/image7.png)

    Окно предварительного просмотра приведен скрипт, который будет выполняться в целевой базе данных, чтобы сделать схемы базы данных соответствует схеме базы данных-источника. Скрипт включает команду ALTER TABLE, который добавляет новый столбец.
8. Закрыть **предварительной версии базы данных** диалоговое окно, а затем нажмите кнопку **публикации**.

    Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница.
9. Откройте страницу сведений о пользователе (Добавить *Account/UserInfo.aspx* URL-адрес домашней страницы) чтобы проверить, что обновление было успешно развернуто. Необходимо выполнить вход, введя *администратора* и *devpwd*.

    Данные в таблицах не развертывается по умолчанию, и вы не настроили скрипта развертывания данных для запуска, поэтому вы не найдете комментарий, добавленный в разработке. Теперь новый комментарий можно добавить в промежуточной среде, чтобы убедиться, что изменение было развернуто в базу данных, и страница работает правильно.
10. Выполните ту же процедуру для развертывания в промежуточной и рабочей.

    Не забудьте отключить дополнительные сценарии. По сравнению с профиль тестирования отличается только что отключит только один скрипт в промежуточной и рабочей среде профили, так как они были настроены для запуска только *aspnet-prod-data.sql*.

    Учетные данные для тестовых и производственных: администратора и prodpwd.

    Для обновления приложения реальной рабочей среде, который включает в себя изменение базы данных также обычно выполняемые приложения в автономный режим во время развертывания, передав *приложения\_offline.htm* перед публикацией и его удаление После этого, как видно из [предыдущем учебном курсе](deploying-a-code-update.md).

## <a name="summary"></a>Сводка

Теперь вы развернули, включены изменения базы данных с помощью Code First Migrations и поставщик dbDacFx обновления приложения.

![Странице "Instructors" с даты рождения](deploying-a-database-update/_static/image8.png)

![Страница сведений о пользователе](deploying-a-database-update/_static/image9.png)

Следующее руководство показано, как выполнить развертывание с помощью командной строки.

> [!div class="step-by-step"]
> [Назад](deploying-a-code-update.md)
> [Вперед](command-line-deployment.md)
