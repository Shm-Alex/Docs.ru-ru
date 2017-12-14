---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: "Разработка приложений ASP.NET с Azure Active Directory | Документы Microsoft"
author: Rick-Anderson
description: "Инструменты Microsoft ASP.NET для Azure Active Directory упрощает Включение проверки подлинности для веб-приложений, размещенных в Azure. Можно использовать предварительная пр Azure..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 425f8edff41588db363055d166995d5f563c5a23
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a><span data-ttu-id="8d561-104">Разработка приложений ASP.NET с Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d561-104">Developing ASP.NET Apps with Azure Active Directory</span></span>
====================
<span data-ttu-id="8d561-105">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8d561-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="8d561-106">Средства Microsoft ASP.NET для Azure Active Directory позволяет легко включить проверку подлинности для веб-приложений, размещенных на [Azure](https://www.windowsazure.com/en-us/home/features/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="8d561-106">Microsoft ASP.NET tools for Azure Active Directory makes it simple to enable authentication for web applications hosted on [Azure](https://www.windowsazure.com/en-us/home/features/web-sites/).</span></span> <span data-ttu-id="8d561-107">Проверка подлинности Azure можно использовать для проверки подлинности пользователей Office 365 в организации, корпоративных учетных записей, синхронизированные из локальной службы Active Directory или пользователей, созданных в собственный домен Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d561-107">You can use Azure Authentication to authenticate Office 365 users from your organization, corporate accounts synced from your on-premise Active Directory or users created in your own custom Azure Active Directory domain.</span></span> <span data-ttu-id="8d561-108">Включение проверки подлинности Windows Azure настраивает приложение для проверки подлинности пользователей с помощью одного [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) клиента.</span><span class="sxs-lookup"><span data-stu-id="8d561-108">Enabling Windows Azure Authentication configures your application to authenticate users using a single [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.</span></span>
> 
>  <span data-ttu-id="8d561-109">Это руководство было написано с Рик Андерсон[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8d561-109">This tutorial was written by Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)</span></span>


<span data-ttu-id="8d561-110">Этот учебник будет показано, как создать приложение ASP.NET, настроенная для единого входа с [Azure Active Directory](https://msdn.microsoft.com/en-us/library/azure/mt168838.aspx) (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8d561-110">This tutorial will show you how to create an ASP.NET application that is configured for sign-on with [Azure Active Directory](https://msdn.microsoft.com/en-us/library/azure/mt168838.aspx) (Azure AD).</span></span> <span data-ttu-id="8d561-111">Вы также узнаете, как вызвать API Graph для получения сведений о пользователе, выполнившего вход и развертывание приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="8d561-111">You will also learn how to call the Graph API to get information about the currently signed-in user and how to deploy the application to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d561-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8d561-112">Prerequisites</span></span>

1. <span data-ttu-id="8d561-113">[Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) или [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="8d561-113">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) or [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span>
2. <span data-ttu-id="8d561-114">[Visual Studio 2013 с обновлением 4](https://www.microsoft.com/en-us/download/details.aspx?id=44921) -требуется обновление 3 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8d561-114">[Visual Studio 2013 Update 4](https://www.microsoft.com/en-us/download/details.aspx?id=44921) - Update 3 or higher is required.</span></span>
3. <span data-ttu-id="8d561-115">Учетная запись Azure.</span><span class="sxs-lookup"><span data-stu-id="8d561-115">An Azure account.</span></span> <span data-ttu-id="8d561-116">[Щелкните здесь,](https://azure.microsoft.com/en-us/pricing/free-trial/) для получения бесплатной пробной версии, если у вас еще нет учетной записи.</span><span class="sxs-lookup"><span data-stu-id="8d561-116">[Click here](https://azure.microsoft.com/en-us/pricing/free-trial/) for a free trial if you do not already have an account.</span></span>

## <a name="add-a-global-administrator-to-your-active-directory"></a><span data-ttu-id="8d561-117">Добавление глобального администратора в Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d561-117">Add a Global Administrator to your Active Directory</span></span>

1. <span data-ttu-id="8d561-118">Войдите в [портала управления Azure](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="8d561-118">Sign in to the [Azure Management Portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="8d561-119">Все учетные записи Azure содержат **каталог по умолчанию** --щелкните его, а затем нажмите кнопку **пользователей** вкладки в верхней части страницы (см. рисунок ниже).</span><span class="sxs-lookup"><span data-stu-id="8d561-119">All Azure accounts contain a **Default Directory** -- click on it, then click the **Users** tab at the top of the page (see image below).</span></span>
3. <span data-ttu-id="8d561-120">Нажмите кнопку Добавить пользователя.</span><span class="sxs-lookup"><span data-stu-id="8d561-120">Click Add User.</span></span>  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. <span data-ttu-id="8d561-121">Создайте нового пользователя с **глобального администратора** роли.</span><span class="sxs-lookup"><span data-stu-id="8d561-121">Create a new user with the **Global Administrator** role.</span></span> <span data-ttu-id="8d561-122">Нажмите кнопку **пользователей** в верхнем меню и нажмите кнопку **добавить пользователя** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="8d561-122">Click **Users** from the top menu, and then click the **Add User** button on the command bar.</span></span>
5. <span data-ttu-id="8d561-123">В **добавить пользователя** диалоговое окно, введите имя для нового пользователя и нажмите кнопку со стрелкой вправо.</span><span class="sxs-lookup"><span data-stu-id="8d561-123">In the **Add User** dialog, enter a name for the new user and then click the right arrow.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. <span data-ttu-id="8d561-124">Введите имя пользователя и задайте для роли **глобального администратора**.</span><span class="sxs-lookup"><span data-stu-id="8d561-124">Enter the user name and set the role to **Global Administrator**.</span></span> <span data-ttu-id="8d561-125">Глобальные администраторы запасной адрес электронной почты требуется для восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="8d561-125">Global administrators require an alternate email address for password recovery purposes.</span></span> <span data-ttu-id="8d561-126">После завершения работы нажмите кнопку со стрелкой вправо.</span><span class="sxs-lookup"><span data-stu-id="8d561-126">After you're finished, click the right arrow.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. <span data-ttu-id="8d561-127">На следующей странице диалогового окна щелкните **создать**.</span><span class="sxs-lookup"><span data-stu-id="8d561-127">On the next page of the dialog, click **Create**.</span></span> <span data-ttu-id="8d561-128">Временный пароль будет создан для нового пользователя и отображаются в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="8d561-128">A temporary password will be created for the new user and displayed in the dialog.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 <span data-ttu-id="8d561-129">Этот пароль будет необходимо сменить пароль после первого входа в.</span><span class="sxs-lookup"><span data-stu-id="8d561-129">Save the password, you will be required to change the password after the first log in.</span></span> <span data-ttu-id="8d561-130">На следующем рисунке учетной записи администратора.</span><span class="sxs-lookup"><span data-stu-id="8d561-130">The following image shows the new admin account.</span></span> <span data-ttu-id="8d561-131">Необходимо использовать Azure Active Directory могли входить в приложение, а не учетной записи Майкрософт, также отображаются на этой странице.</span><span class="sxs-lookup"><span data-stu-id="8d561-131">You must use the Azure Active Directory to log into your app, not the Microsoft account also shown on this page.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a><span data-ttu-id="8d561-132">Создание приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d561-132">Create an ASP.NET Application</span></span>

<span data-ttu-id="8d561-133">В следующих шагах используется [Visual Studio Express 2013 для Web](https://www.microsoft.com/en-us/download/details.aspx?id=40747)и требует [Visual Studio 2013 Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=43721).</span><span class="sxs-lookup"><span data-stu-id="8d561-133">The following steps use [Visual Studio Express 2013 for Web](https://www.microsoft.com/en-us/download/details.aspx?id=40747), and requires [Visual Studio 2013 Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=43721).</span></span>

1. <span data-ttu-id="8d561-134">В Visual Studio щелкните **файл** и затем **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="8d561-134">In Visual Studio, click **File** and then **New Project**.</span></span> <span data-ttu-id="8d561-135">На **новый проект** диалоговое окно, выберите Visual C# веб-проекта в левом меню и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8d561-135">On the **New Project** dialog, select the Visual C# Web project from the left menu and click **OK**.</span></span> <span data-ttu-id="8d561-136">Можно также снять **добавить Application Insights в проект** Если вы не хотите функциональные возможности для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="8d561-136">You may also want to uncheck the **Add Application Insights to Project** if you don't want the functionality for your application.</span></span>
2. <span data-ttu-id="8d561-137">В **новый проект ASP.NET** диалогового окна выберите **MVC**, а затем нажмите кнопку **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="8d561-137">In the **New ASP.NET Project** dialog, select **MVC**, and then click **Change Authentication**.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. <span data-ttu-id="8d561-138">На **изменить аутентификацию** диалогового окна выберите **учетные записи организации**.</span><span class="sxs-lookup"><span data-stu-id="8d561-138">On the **Change Authentication** dialog, select **Organizational Accounts**.</span></span> <span data-ttu-id="8d561-139">Эти параметры можно использовать для автоматической регистрации приложения в Azure AD, а также автоматически настроить приложение для интеграции с Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d561-139">These options can be used to automatically register your application with Azure AD as well as automatically configure your application to integrate with Azure AD.</span></span> <span data-ttu-id="8d561-140">Нет необходимости в использовании **изменить аутентификацию** диалоговое окно для регистрации и настройки приложения, но он облегчает процесс.</span><span class="sxs-lookup"><span data-stu-id="8d561-140">You don't have to use the **Change Authentication** dialog to register and configure your application, but it makes it much easier.</span></span> <span data-ttu-id="8d561-141">Если вы используете Visual Studio 2012. Например, можно вручную зарегистрировать приложение в портале управления Azure и обновите конфигурацию для интеграции с Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d561-141">If you are using Visual Studio 2012 for example, you can still manually register the application in the Azure Management Portal and update its configuration to integrate with Azure AD.</span></span>  
 <span data-ttu-id="8d561-142">В раскрывающемся меню, выберите **облако — одной организации** и **единый вход, чтение данных каталога**.</span><span class="sxs-lookup"><span data-stu-id="8d561-142">In the drop-down menus, select **Cloud - Single Organization** and **Single Sign On, Read directory data**.</span></span> <span data-ttu-id="8d561-143">Введите имя домена для вашего каталога Azure AD (в примере ниже изображения) *aricka0yahoo.onmicrosoft.com*, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8d561-143">Enter the domain for your Azure AD directory, for example (in the images below) *aricka0yahoo.onmicrosoft.com*, and then click **OK**.</span></span> <span data-ttu-id="8d561-144">Имя домена можно получить на вкладке «домены» для каталога по умолчанию на портале azure (см. Далее рисунок вниз).</span><span class="sxs-lookup"><span data-stu-id="8d561-144">You can get the domain name from the Domains tab for the Default Directory on the azure portal (see the next image down).</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 <span data-ttu-id="8d561-145">Ниже приведен имя домена на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="8d561-145">The following image shows the domain name from the Azure portal.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > <span data-ttu-id="8d561-146">При необходимости можно настроить URI идентификатора приложения, который будет зарегистрирован в Azure AD, щелкнув **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="8d561-146">You can optionally configure the Application ID URI that will be registered in Azure AD by clicking **More Options**.</span></span> <span data-ttu-id="8d561-147">URI идентификатора приложения — уникальный идентификатор для приложения, который зарегистрирован в Azure AD, который используется приложением для идентификации при взаимодействии с Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d561-147">The App ID URI is the unique identifier for an application, which is registered in Azure AD and used by the application to identify itself when communicating with Azure AD.</span></span> <span data-ttu-id="8d561-148">Дополнительные сведения об URI идентификатора приложения и другие свойства зарегистрированных приложений см. в разделе [в этом разделе](https://msdn.microsoft.com/en-us/library/azure/dn499820.aspx#BKMK_Registering).</span><span class="sxs-lookup"><span data-stu-id="8d561-148">For more information about the App ID URI and other properties of registered applications, see [this topic](https://msdn.microsoft.com/en-us/library/azure/dn499820.aspx#BKMK_Registering).</span></span> <span data-ttu-id="8d561-149">Установив флажок под полем URI идентификатора приложения, можно перезаписать существующую регистрацию в Azure AD, который использует тот URI ИД приложения.</span><span class="sxs-lookup"><span data-stu-id="8d561-149">By clicking the checkbox below the App ID URI field, you can also choose to overwrite an existing registration in Azure AD that uses the same App ID URI.</span></span>
4. <span data-ttu-id="8d561-150">После нажатия кнопки **ОК**, появится диалоговое окно входа в систему, и вам потребуется выполнить вход с использованием учетной записи глобального администратора (не Майкрософт учетной записи, связанной с вашей подпиской).</span><span class="sxs-lookup"><span data-stu-id="8d561-150">After clicking **OK**, a sign-in dialog will appear, and you'll need to sign in using a Global Administrator account (not the Microsoft account associated with your subscription).</span></span> <span data-ttu-id="8d561-151">Если ранее вы создали новую учетную запись администратора, будет необходимо изменить пароль, а затем войдите в систему, используя новый пароль.</span><span class="sxs-lookup"><span data-stu-id="8d561-151">If you created a new Administrator account earlier, you'll be required to change the password and then sign in again using the new password.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. <span data-ttu-id="8d561-152">После вы успешно прошли проверку подлинности, **новый проект ASP.NET** диалоговом окне будут отображаться ваш выбор проверки подлинности (**организации** ) и каталог, где новое приложение будет зарегистрировано (*aricka0yahoo.onmicrosoft.com* на рисунке ниже).</span><span class="sxs-lookup"><span data-stu-id="8d561-152">After you've successfully authenticated, the **New ASP.NET Project** dialog will show your authentication choice (**Organizational** ) and the directory where the new application will be registered (*aricka0yahoo.onmicrosoft.com* in the image below).</span></span> <span data-ttu-id="8d561-153">Ниже эти сведения, установленном флажке **узел в облаке**.</span><span class="sxs-lookup"><span data-stu-id="8d561-153">Below this information, select the checkbox labeled **Host in the cloud**.</span></span> <span data-ttu-id="8d561-154">Если этот флажок установлен, проект будет использован как веб-приложение Azure и будут включены для простой публикации позже.</span><span class="sxs-lookup"><span data-stu-id="8d561-154">If this checkbox is selected, the project will be provisioned as an Azure web app and will be enabled for easy publishing later.</span></span> <span data-ttu-id="8d561-155">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8d561-155">Click **OK**.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. <span data-ttu-id="8d561-156">**Настройка веб-сайта Azure** появится диалоговое окно, с помощью имени сайта, автоматически сгенерированный и региона.</span><span class="sxs-lookup"><span data-stu-id="8d561-156">The **Configure Azure Website** dialog will appear, using an auto-generated site name and region.</span></span> <span data-ttu-id="8d561-157">Также Обратите внимание, учетной записи, которую в данный момент вы вошли в в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="8d561-157">Also note the account you're currently signed into in the dialog.</span></span> <span data-ttu-id="8d561-158">Будет необходимо убедиться, что эта учетная запись является тот, который присоединяется подписки Azure, обычно является учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8d561-158">You want to make sure that this account is the one that your Azure subscription is attached to, typically a Microsoft account.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8d561-159">Этот проект требует базы данных.</span><span class="sxs-lookup"><span data-stu-id="8d561-159">This project requires a database.</span></span> <span data-ttu-id="8d561-160">Необходимо выбрать один из существующих баз данных или создайте новую.</span><span class="sxs-lookup"><span data-stu-id="8d561-160">You need to select one of your existing databases, or create a new one.</span></span> <span data-ttu-id="8d561-161">Базы данных является обязательным, поскольку проект уже использует файл локальной базы данных для хранения небольшой объем данных конфигурации проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8d561-161">A database is required because the project already uses a local database file to store a small amount of authentication configuration data.</span></span> <span data-ttu-id="8d561-162">При развертывании приложения на веб-сайт Azure, эта база данных не упаковываются в развертывание, поэтому вам нужно выбрать один, который доступен в облаке.</span><span class="sxs-lookup"><span data-stu-id="8d561-162">When you deploy the application to an Azure Website, this database isn't packaged with the deployment, so you need to choose one that's accessible in the cloud.</span></span> <span data-ttu-id="8d561-163">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8d561-163">Click **OK**.</span></span>

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. <span data-ttu-id="8d561-164">Проект будет создан и вашей веб-приложения параметры и параметры проверки подлинности будут автоматически настроены с проектом.</span><span class="sxs-lookup"><span data-stu-id="8d561-164">The project will be created, and your authentication options and web app options will be automatically configured with the project.</span></span> <span data-ttu-id="8d561-165">После завершения этого процесса запустите проект локально, нажав клавишу **^ F5**.</span><span class="sxs-lookup"><span data-stu-id="8d561-165">Once this process has completed, run the project locally by pressing **^F5**.</span></span> <span data-ttu-id="8d561-166">Необходимо будет выполнить вход с использованием учетной записи организации.</span><span class="sxs-lookup"><span data-stu-id="8d561-166">You will be required to sign in using your organizational account.</span></span> <span data-ttu-id="8d561-167">Укажите имя пользователя и пароль для учетной записи, созданной ранее и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="8d561-167">Provide the username and password for the account you created earlier and click **Sign in**.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. <span data-ttu-id="8d561-168">После успешного входа в веб-сайте ASP.NET показывает, что вы прошли проверку подлинности, отображая имя пользователя в правом верхнем углу страницы.</span><span class="sxs-lookup"><span data-stu-id="8d561-168">After successful sign in, the ASP.NET site will show that you've authenticated by displaying the username in the top right corner of the page.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 <span data-ttu-id="8d561-169">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="8d561-169">If you get the error:</span></span>  
 <span data-ttu-id="8d561-170">Значение не может быть неопределенным или пустым.</span><span class="sxs-lookup"><span data-stu-id="8d561-170">Value cannot be null or empty.</span></span> <span data-ttu-id="8d561-171">Имя параметра: linkText</span><span class="sxs-lookup"><span data-stu-id="8d561-171">Parameter name: linkText</span></span>   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 <span data-ttu-id="8d561-172">в разделе [отладки](#dbg) в конце учебника.</span><span class="sxs-lookup"><span data-stu-id="8d561-172">see the [debug](#dbg) section at the end of the tutorial.</span></span>

## <a name="basics-of-the-graph-api"></a><span data-ttu-id="8d561-173">Основные сведения о API Graph</span><span class="sxs-lookup"><span data-stu-id="8d561-173">Basics of the Graph API</span></span>

<span data-ttu-id="8d561-174">[Graph API](https://msdn.microsoft.com/en-us/library/azure/hh974476.aspx) — это программный интерфейс, используемый для выполнения операций CRUD и других операций с объектами в каталоге Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d561-174">[The Graph API](https://msdn.microsoft.com/en-us/library/azure/hh974476.aspx) is the programmatic interface used to perform CRUD and other operations on objects in your Azure AD directory.</span></span> <span data-ttu-id="8d561-175">Если выбран параметр учетной записи организации для проверки подлинности при создании нового проекта в Visual Studio 2013, приложение будет уже настроен для вызова Graph API.</span><span class="sxs-lookup"><span data-stu-id="8d561-175">If you select an Organizational Account option for authentication when creating a new project in Visual Studio 2013, your application will already be configured to call the Graph API.</span></span> <span data-ttu-id="8d561-176">В этом разделе кратко показано, как работает Graph API.</span><span class="sxs-lookup"><span data-stu-id="8d561-176">This section briefly shows you how the Graph API works.</span></span>

1. <span data-ttu-id="8d561-177">В запущенном приложении, щелкните имя пользователя, выполнившего вход в верхней правой части страницы.</span><span class="sxs-lookup"><span data-stu-id="8d561-177">In your running application, click on the name of the signed-in user at the top right of the page.</span></span> <span data-ttu-id="8d561-178">Переходить на страницу профиля пользователя, что действие контроллера Home.</span><span class="sxs-lookup"><span data-stu-id="8d561-178">This will take you to the User Profile page, which is an action on the Home Controller.</span></span> <span data-ttu-id="8d561-179">Вы заметите, что таблица содержит сведения об учетной записи администратора созданной ранее.</span><span class="sxs-lookup"><span data-stu-id="8d561-179">You'll notice that the table contains user information about the administrator account you created earlier.</span></span> <span data-ttu-id="8d561-180">Эти сведения хранятся в каталоге и вызывает Graph API для получения этих сведений при загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="8d561-180">This information is stored in your directory, and the Graph API is called to retrieve this information when the page loads.</span></span>   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. <span data-ttu-id="8d561-181">Вернитесь в Visual Studio и разверните **контроллеров** папки, а затем откройте **HomeController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="8d561-181">Go back to Visual Studio and expand the **Controllers** folder and then open the **HomeController.cs** file.</span></span> <span data-ttu-id="8d561-182">Вы увидите **UserProfile()** действие, которое содержит код для получения токена и затем вызывать Graph API.</span><span class="sxs-lookup"><span data-stu-id="8d561-182">You'll see a **UserProfile()** action that contains code to retrieve a token and then call the Graph API.</span></span> <span data-ttu-id="8d561-183">Этот код повторяется ниже:</span><span class="sxs-lookup"><span data-stu-id="8d561-183">This code is duplicated below:</span></span> 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    <span data-ttu-id="8d561-184">Для вызова Graph API, сначала необходимо получить токен.</span><span class="sxs-lookup"><span data-stu-id="8d561-184">To call the Graph API, you first need to retrieve a token.</span></span> <span data-ttu-id="8d561-185">При получении токена, его строковым значением должен быть добавлен в заголовок авторизации для всех последующих запросов к Graph API.</span><span class="sxs-lookup"><span data-stu-id="8d561-185">When the token is retrieved, its string value must be appended in the Authorization header for all subsequent requests to the Graph API.</span></span> <span data-ttu-id="8d561-186">Большая часть кода выше обрабатывает сведения проверки подлинности в Azure AD, чтобы получить маркер, использующее токен для вызова Graph API и затем преобразование ответ, чтобы оно может быть представлено в представлении.</span><span class="sxs-lookup"><span data-stu-id="8d561-186">Most of the code above handles the details of authenticating to Azure AD to get a token, using the token to make a call to the Graph API, and then transforming the response so that it can be presented in the View.</span></span>

    <span data-ttu-id="8d561-187">Наиболее участку обсуждение имеет следующую выделенную строку: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`.</span><span class="sxs-lookup"><span data-stu-id="8d561-187">The most relevant portion for discussion is the following highlighted line: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`.</span></span> <span data-ttu-id="8d561-188">Эта строка представляет имя пользователя, который был десериализован из ответа JSON и выводятся в представлении.</span><span class="sxs-lookup"><span data-stu-id="8d561-188">This line represents the name of the user, which has been deserialized from the JSON response and is presented in the View.</span></span>

    <span data-ttu-id="8d561-189">Можно вызвать Graph API, с помощью HttpClient и самостоятельно обрабатывать необработанные данные, но более простым способом является использование [Graph клиентской библиотеки, который доступен через NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/).</span><span class="sxs-lookup"><span data-stu-id="8d561-189">You can call the Graph API using HttpClient and handle the raw data yourself, but an easier way is to use the [Graph Client Library which is available via NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/).</span></span> <span data-ttu-id="8d561-190">Клиентская библиотека обрабатывает необработанные HTTP-запросы и преобразования возвращаемых данных для вас и делает его намного легче работать с API Graph в среде .NET.</span><span class="sxs-lookup"><span data-stu-id="8d561-190">The Client Library handles the raw HTTP requests and the transformation of the returned data for you, and makes it much easier to work with the Graph API in a .NET environment.</span></span> <span data-ttu-id="8d561-191">Просмотрите образцы кода связанные Graph API на [GitHub.](https://github.com/AzureADSamples)</span><span class="sxs-lookup"><span data-stu-id="8d561-191">See the related Graph API code samples on [GitHub.](https://github.com/AzureADSamples)</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="8d561-192">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="8d561-192">Deploy the Application to Azure</span></span>

<span data-ttu-id="8d561-193">Ниже показано, как развернуть приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="8d561-193">The following steps will show you how to deploy the application to Azure.</span></span> <span data-ttu-id="8d561-194">В предыдущих шагах подключен нового проекта с веб-приложения в Azure, поэтому он готов к опубликованию за несколько шагов.</span><span class="sxs-lookup"><span data-stu-id="8d561-194">In the earlier steps, you connected your new project with a web app on Azure, so it's ready to be published in just a few steps.</span></span>

1. <span data-ttu-id="8d561-195">В Visual Studio щелкните правой кнопкой мыши проект и выберите пункт **публикации**.</span><span class="sxs-lookup"><span data-stu-id="8d561-195">In Visual Studio, right-click on the project and select **Publish**.</span></span> <span data-ttu-id="8d561-196">**Веб-публикация** с каждым параметром, который уже настроен, появится диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="8d561-196">The **Publish Web** dialog will appear with each setting already configured.</span></span> <span data-ttu-id="8d561-197">Щелкните **Далее** кнопку, чтобы перейти к **параметры** страницы.</span><span class="sxs-lookup"><span data-stu-id="8d561-197">Click on the **Next** button to go to the **Settings** page.</span></span> <span data-ttu-id="8d561-198">Может быть предложено проверку подлинности; Убедитесь, что проверка подлинности с помощью учетной записи подписки Azure (обычно учетная запись Майкрософт) и не учетная запись организации, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="8d561-198">You may be prompted to authenticate; make sure you authenticate using your Azure subscription account (typically a Microsoft account) and not the organizational account you created earlier.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. <span data-ttu-id="8d561-199">Проверьте **включить проверку подлинности по организации** параметр.</span><span class="sxs-lookup"><span data-stu-id="8d561-199">Check the **Enable Organizational Authentication** option.</span></span> <span data-ttu-id="8d561-200">В **домена** введите домен для вашего каталога.</span><span class="sxs-lookup"><span data-stu-id="8d561-200">In the **Domain** field, enter the domain for your directory.</span></span> <span data-ttu-id="8d561-201">Из **уровень доступа** раскрывающийся список, выберите **единый вход, чтение данных каталога**.</span><span class="sxs-lookup"><span data-stu-id="8d561-201">From the **Access Level** drop-down, select **Single Sign On, Read directory data**.</span></span> <span data-ttu-id="8d561-202">Обратите внимание, что используется Прежняя база данных уже заполняется **баз данных** раздела.</span><span class="sxs-lookup"><span data-stu-id="8d561-202">You'll notice that the previous database you used is already populated in the **Databases** section.</span></span> <span data-ttu-id="8d561-203">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="8d561-203">Click **Publish**.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. <span data-ttu-id="8d561-204">Visual Studio будет приступать к развертыванию веб-сайта, а затем появится новое окно браузера.</span><span class="sxs-lookup"><span data-stu-id="8d561-204">Visual Studio will begin deploying your website, and then a new browser window will appear.</span></span> <span data-ttu-id="8d561-205">Может быть предложено пройти проверку подлинности вашего каталога еще раз.</span><span class="sxs-lookup"><span data-stu-id="8d561-205">You may be prompted to authenticate to your directory once again.</span></span> <span data-ttu-id="8d561-206">Как только вы прошли проверку подлинности, вы будете перенаправлены на только что опубликованный веб-сайта в Azure.</span><span class="sxs-lookup"><span data-stu-id="8d561-206">Once you've authenticated, you'll be redirected to your newly published website on Azure.</span></span>  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="8d561-207">Отладка приложения</span><span class="sxs-lookup"><span data-stu-id="8d561-207">Debugging the app</span></span>

<span data-ttu-id="8d561-208">Если возникли следующие ошибки:</span><span class="sxs-lookup"><span data-stu-id="8d561-208">If you get the following error:</span></span>   
 <span data-ttu-id="8d561-209">Значение не может быть неопределенным или пустым.</span><span class="sxs-lookup"><span data-stu-id="8d561-209">Value cannot be null or empty.</span></span> <span data-ttu-id="8d561-210">Имя параметра: linkText</span><span class="sxs-lookup"><span data-stu-id="8d561-210">Parameter name: linkText</span></span>   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

<span data-ttu-id="8d561-211">Замените код в *представления\общие\\_LoginPartial.cshtml* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="8d561-211">Replace the code in the *Views\Shared\\_LoginPartial.cshtml* file with the following:</span></span>

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

<span data-ttu-id="8d561-212">После запуска приложения, если пользователь «User Null», выйдите из системы и снова войти в учетную запись Active Directory, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="8d561-212">After running the app, if the logged in user shows "Null User", sign out, and sign back in with the Active Directory account you created earlier.</span></span>

<span data-ttu-id="8d561-213">Рик Rainey является отличным учебника, выполните [глубокое погружение: веб-сайтов Azure и организации проверку подлинности с помощью Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).</span><span class="sxs-lookup"><span data-stu-id="8d561-213">An excellent tutorial to follow is Rick Rainey's [Deep Dive: Azure Websites and Organizational Authentication using Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).</span></span>

## <a name="more-information"></a><span data-ttu-id="8d561-214">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="8d561-214">More Information</span></span>

- [<span data-ttu-id="8d561-215">Подробное рассмотрение: Веб-сайтов Azure и организации проверку подлинности с помощью Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d561-215">Deep Dive: Azure Websites and Organizational Authentication using Azure AD</span></span>](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [<span data-ttu-id="8d561-216">Общие сведения о Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="8d561-216">Azure AD Graph API Overview</span></span>](https://msdn.microsoft.com/en-us/library/azure/hh974476.aspx)
- [<span data-ttu-id="8d561-217">Сценарии проверки подлинности в Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d561-217">Authentication Scenarios in Azure AD</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn499820.aspx)
- [<span data-ttu-id="8d561-218">Примеры кода Azure AD на GitHub</span><span class="sxs-lookup"><span data-stu-id="8d561-218">Azure AD Code Samples on GitHub</span></span>](https://github.com/AzureADSamples)