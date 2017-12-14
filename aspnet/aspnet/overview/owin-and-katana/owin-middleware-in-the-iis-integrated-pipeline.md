---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: "По промежуточного слоя OWIN в IIS интегрированном конвейере | Документы Microsoft"
author: Praburaj
description: "В этой статье показано, как выполнять компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как задать события конвейера OMC на. Вы должны..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 42851cb9b8046ca4f70894b9ec5b671b269da04c
ms.sourcegitcommit: 97432cbf9b8673bc4ad7012d5b6f2ed273420295
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a><span data-ttu-id="38388-104">По промежуточного слоя OWIN в интегрированном конвейере служб IIS</span><span class="sxs-lookup"><span data-stu-id="38388-104">OWIN Middleware in the IIS integrated pipeline</span></span>
====================
<span data-ttu-id="38388-105">по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="38388-105">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="38388-106">В этой статье показано, как выполнять компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как задать события конвейера OMC на.</span><span class="sxs-lookup"><span data-stu-id="38388-106">This article shows how to run OWIN middleware Components (OMCs) in the IIS integrated pipeline, and how to set the pipeline event an OMC runs on.</span></span> <span data-ttu-id="38388-107">Изучите [Обзор проекта Katana](an-overview-of-project-katana.md) и [определение класса запуска OWIN](owin-startup-class-detection.md) перед чтением этого учебника.</span><span class="sxs-lookup"><span data-stu-id="38388-107">You should review [An Overview of Project Katana](an-overview-of-project-katana.md) and [OWIN Startup Class Detection](owin-startup-class-detection.md) before reading this tutorial.</span></span> <span data-ttu-id="38388-108">Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Крис Ross Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="38388-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>


<span data-ttu-id="38388-109">Несмотря на то что [OWIN](an-overview-of-project-katana.md) компонентов по промежуточного слоя (OMCs) в первую очередь предназначены для запуска в конвейере независимой от сервера, возможно, для работы в интегрированном конвейере служб IIS также OMC (**классический режим *не* поддерживается**).</span><span class="sxs-lookup"><span data-stu-id="38388-109">Although [OWIN](an-overview-of-project-katana.md) middleware components (OMCs) are primarily designed to run in a server-agnostic pipeline, it is possible to run an OMC in the IIS integrated pipeline as well (**classic mode is *not* supported**).</span></span> <span data-ttu-id="38388-110">OMC можно сделать для работы в интегрированном конвейере служб IIS, установив следующий пакет из консоли диспетчера пакетов (PMC).</span><span class="sxs-lookup"><span data-stu-id="38388-110">An OMC can be made to work in the IIS integrated pipeline by installing the following package from the Package Manager Console (PMC):</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

<span data-ttu-id="38388-111">Это означает, что все платформы приложений, даже те, которые еще не может запустить вне IIS и System.Web, можно воспользоваться преимуществами существующих компонентов по промежуточного слоя OWIN.</span><span class="sxs-lookup"><span data-stu-id="38388-111">This means that all application frameworks, even those that are not yet able to run outside of IIS and System.Web, can benefit from existing OWIN middleware components.</span></span> 

> [!NOTE]
> <span data-ttu-id="38388-112">Все `Microsoft.Owin.Security.*` пакетов доставки в новой системе удостоверений в Visual Studio 2013 (например: файлы cookie, учетной записи Майкрософт, Google, Facebook, Twitter, [маркеров носителей](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, сервер авторизации, JWT, Azure Active каталог служб федерации Active directory) создан как OMCs и может использоваться в сценариях резидентной и размещенных в IIS.</span><span class="sxs-lookup"><span data-stu-id="38388-112">All of the `Microsoft.Owin.Security.*` packages shipping with the new Identity System in Visual Studio 2013 (for example: Cookies, Microsoft Account, Google, Facebook, Twitter, [Bearer Token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, Authorization server, JWT, Azure Active directory, and Active directory federation services) are authored as OMCs, and can be used in both self-hosted and IIS-hosted scenarios.</span></span>

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a><span data-ttu-id="38388-113">Как выполняется по промежуточного слоя OWIN в интегрированном конвейере служб IIS</span><span class="sxs-lookup"><span data-stu-id="38388-113">How OWIN Middleware Executes in the IIS Integrated Pipeline</span></span>

<span data-ttu-id="38388-114">Для консольных приложений OWIN, созданные в конвейер приложения [начальной конфигурации](owin-startup-class-detection.md) задается порядок добавления компонентов с помощью `IAppBuilder.Use` метод.</span><span class="sxs-lookup"><span data-stu-id="38388-114">For OWIN console applications, the application pipeline built using the [startup configuration](owin-startup-class-detection.md) is set by the order the components are added using the `IAppBuilder.Use` method.</span></span> <span data-ttu-id="38388-115">То есть в конвейер OWIN в [Katana](an-overview-of-project-katana.md) среда выполнения будет обрабатывать OMCs в порядке, они были зарегистрированы с помощью `IAppBuilder.Use`.</span><span class="sxs-lookup"><span data-stu-id="38388-115">That is, the OWIN pipeline in the [Katana](an-overview-of-project-katana.md) runtime will process OMCs in the order they were registered using `IAppBuilder.Use`.</span></span> <span data-ttu-id="38388-116">В интегрированном конвейере служб IIS конвейер запросов состоит из [HttpModules](https://msdn.microsoft.com/en-us/library/ms178468(v=vs.85).aspx) подписка на набор предварительно определенных событий конвейера, таких как [BeginRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx)и т. д.</span><span class="sxs-lookup"><span data-stu-id="38388-116">In the IIS integrated pipeline the request pipeline consists of [HttpModules](https://msdn.microsoft.com/en-us/library/ms178468(v=vs.85).aspx) subscribed to a pre-defined set of the pipeline events such as [BeginRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx), etc.</span></span>

<span data-ttu-id="38388-117">Сравнив OMC, [HttpModule](https://msdn.microsoft.com/en-us/library/zec9k340(v=vs.85).aspx) в мире ASP.NET OMC должны быть зарегистрированы правильно конвейера предварительно определенных событий.</span><span class="sxs-lookup"><span data-stu-id="38388-117">If we compare an OMC to that of an [HttpModule](https://msdn.microsoft.com/en-us/library/zec9k340(v=vs.85).aspx) in the ASP.NET world, an OMC must be registered to the correct pre-defined pipeline event.</span></span> <span data-ttu-id="38388-118">Например, HttpModule `MyModule` вызывается при поступлении запроса [AuthenticateRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx) этап конвейера:</span><span class="sxs-lookup"><span data-stu-id="38388-118">For example, the HttpModule `MyModule` will get invoked when a request comes to the [AuthenticateRequest](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx) stage in the pipeline:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

<span data-ttu-id="38388-119">Чтобы OMC участвовать в этот порядок выполнения одинаковую, основанной на событиях [Katana](an-overview-of-project-katana.md) кода среды выполнения просматривает [начальной конфигурации](owin-startup-class-detection.md) и подписывается каждого из компонентов по промежуточного слоя для событие интегрированного конвейера.</span><span class="sxs-lookup"><span data-stu-id="38388-119">In order for an OMC to participate in this same, event-based execution ordering, the [Katana](an-overview-of-project-katana.md) runtime code scans through the [startup configuration](owin-startup-class-detection.md) and subscribes each of the middleware components to an integrated pipeline event.</span></span> <span data-ttu-id="38388-120">Например следующий код OMC и регистрация дает возможность просмотра регистрации событий по умолчанию для компонентов по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="38388-120">For example, the following OMC and registration code enables you to see the default event registration of middleware components.</span></span> <span data-ttu-id="38388-121">(См. более подробные инструкции по созданию класс запуска OWIN, [определение класса запуска OWIN](owin-startup-class-detection.md).)</span><span class="sxs-lookup"><span data-stu-id="38388-121">(For more detailed instructions on creating an OWIN startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).)</span></span>

1. <span data-ttu-id="38388-122">Создайте пустой веб-узел проекта приложения и назовите его **owin2**.</span><span class="sxs-lookup"><span data-stu-id="38388-122">Create an empty web application project and name it **owin2**.</span></span>
2. <span data-ttu-id="38388-123">Из пакета Диспетчер консоли (PMC), выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="38388-123">From the Package Manager Console (PMC), run the following command:</span></span> 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. <span data-ttu-id="38388-124">Добавить `OWIN Startup Class` и назовите его `Startup`.</span><span class="sxs-lookup"><span data-stu-id="38388-124">Add an `OWIN Startup Class` and name it `Startup`.</span></span> <span data-ttu-id="38388-125">Замените созданный код (изменения выделены) следующие:</span><span class="sxs-lookup"><span data-stu-id="38388-125">Replace the generated code with the following (the changes are highlighted):</span></span>  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. <span data-ttu-id="38388-126">Нажмите клавишу F5 для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="38388-126">Hit F5 to run the app.</span></span>

<span data-ttu-id="38388-127">Конфигурация запуска настраивает конвейер с компонентов три по промежуточного слоя, первые два отображения диагностической информации и реагирование на события последнее из них (и также вывода диагностических сведений).</span><span class="sxs-lookup"><span data-stu-id="38388-127">The Startup configuration sets up a pipeline with three middleware components, the first two displaying diagnostic information and the last one responding to events (and also displaying diagnostic information).</span></span> <span data-ttu-id="38388-128">`PrintCurrentIntegratedPipelineStage` Метод выводит события интегрированного конвейера, вызванную этого по промежуточного слоя и сообщение.</span><span class="sxs-lookup"><span data-stu-id="38388-128">The `PrintCurrentIntegratedPipelineStage` method displays the integrated pipeline event this middleware is invoked on and a message.</span></span> <span data-ttu-id="38388-129">Вывод windows выводится следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="38388-129">The output windows displays the following:</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

<span data-ttu-id="38388-130">Среда выполнения Katana сопоставить каждый из компонентов по промежуточного слоя OWIN для [PreExecuteRequestHandler](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.prerequesthandlerexecute.aspx) по умолчанию, который соответствует событию конвейера IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.prerequesthandlerexecute.aspx).</span><span class="sxs-lookup"><span data-stu-id="38388-130">The Katana runtime mapped each of the OWIN middleware components to [PreExecuteRequestHandler](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.prerequesthandlerexecute.aspx) by default, which corresponds to the IIS pipeline event [PreRequestHandlerExecute](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.prerequesthandlerexecute.aspx).</span></span>

## <a name="stage-markers"></a><span data-ttu-id="38388-131">Маркеры рабочей области</span><span class="sxs-lookup"><span data-stu-id="38388-131">Stage Markers</span></span>

<span data-ttu-id="38388-132">Вы можете пометить OMCs выполнение на определенных стадиях конвейера при помощи `IAppBuilder UseStageMarker()` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="38388-132">You can mark OMCs to execute at specific stages of the pipeline by using the `IAppBuilder UseStageMarker()` extension method.</span></span> <span data-ttu-id="38388-133">Для определенного этапе необходимо запустить набор компонентов по промежуточного слоя, вставьте метку этапа сразу после последнего компонента — это набор во время регистрации.</span><span class="sxs-lookup"><span data-stu-id="38388-133">To run a set of middleware components during a particular stage, insert a stage marker right after the last component is the set during registration.</span></span> <span data-ttu-id="38388-134">Правила, на каком этапе конвейера можно выполнить по промежуточного слоя и порядка компонентов необходимо запустить (Далее в этом учебнике рассматриваются правила).</span><span class="sxs-lookup"><span data-stu-id="38388-134">There are rules on which stage of the pipeline you can execute middleware and the order components must run (The rules are explained later in the tutorial).</span></span> <span data-ttu-id="38388-135">Добавить `UseStageMarker` метод `Configuration` код, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="38388-135">Add the `UseStageMarker` method to the `Configuration` code as shown below:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

<span data-ttu-id="38388-136">`app.UseStageMarker(PipelineStage.Authenticate)` Вызов настраивает все компоненты (в данном случае наших двух компонентов диагностики) ранее зарегистрированного по промежуточного слоя для выполнения на стадии конвейера проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="38388-136">The `app.UseStageMarker(PipelineStage.Authenticate)` call configures all the previously registered middleware components (in this case, our two diagnostic components) to run on the authentication stage of the pipeline.</span></span> <span data-ttu-id="38388-137">Последний компонент по промежуточного слоя (который отображает диагностики и отвечает на запросы) будут выполняться на `ResolveCache` рабочей области ( [ResolveRequestCache](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.resolverequestcache.aspx) событий).</span><span class="sxs-lookup"><span data-stu-id="38388-137">The last middleware component (which displays diagnostics and responds to requests) will run on the `ResolveCache` stage (the [ResolveRequestCache](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.resolverequestcache.aspx) event).</span></span>

<span data-ttu-id="38388-138">Нажмите клавишу F5 для запуска приложения. В окне вывода содержит следующую информацию:</span><span class="sxs-lookup"><span data-stu-id="38388-138">Hit F5 to run the app.The output window shows the following:</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a><span data-ttu-id="38388-139">Правила этапа маркера</span><span class="sxs-lookup"><span data-stu-id="38388-139">Stage Marker Rules</span></span>

<span data-ttu-id="38388-140">Компоненты по промежуточного слоя Owin (OMC) можно настроить на запуск в следующие события этап конвейера OWIN:</span><span class="sxs-lookup"><span data-stu-id="38388-140">Owin middleware components (OMC) can be configured to run at the following OWIN pipeline stage events:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. <span data-ttu-id="38388-141">По умолчанию OMCs выполняются на последнего события (`PreHandlerExecute`).</span><span class="sxs-lookup"><span data-stu-id="38388-141">By default, OMCs run at the last event (`PreHandlerExecute`).</span></span> <span data-ttu-id="38388-142">Вот почему наши первый пример кода отображается «PreExecuteRequestHandler».</span><span class="sxs-lookup"><span data-stu-id="38388-142">That's why our first example code displayed "PreExecuteRequestHandler".</span></span>
2. <span data-ttu-id="38388-143">Можно использовать `pp.UseStageMarker` метода для регистрации OMC чтобы раньше, на любом этапе конвейера OWIN перечисленные в `PipelineStage` перечисления.</span><span class="sxs-lookup"><span data-stu-id="38388-143">You can use the a `pp.UseStageMarker` method to register a OMC to run earlier, at any stage of the OWIN pipeline listed in the `PipelineStage` enum.</span></span>
3. <span data-ttu-id="38388-144">Конвейер OWIN и конвейер IIS упорядочен, поэтому вызовы `app.UseStageMarker` должны располагаться в порядке.</span><span class="sxs-lookup"><span data-stu-id="38388-144">The OWIN pipeline and the IIS pipeline is ordered, therefore calls to `app.UseStageMarker` must be in order.</span></span> <span data-ttu-id="38388-145">Невозможно задать обработчик событий к событию, предшествует зарегистрирована для последнего события `app.UseStageMarker`.</span><span class="sxs-lookup"><span data-stu-id="38388-145">You cannot set the event handler to an event that precedes the last event registered with to `app.UseStageMarker`.</span></span> <span data-ttu-id="38388-146">Например *после* вызов:</span><span class="sxs-lookup"><span data-stu-id="38388-146">For example, *after* calling:</span></span>

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

 <span data-ttu-id="38388-147">вызовы `app.UseStageMarker` передачи `Authenticate` или `PostAuthenticate` не удается обработать, а не исключение.</span><span class="sxs-lookup"><span data-stu-id="38388-147">calls to `app.UseStageMarker` passing `Authenticate` or `PostAuthenticate` will not be honored, and no exception will be thrown.</span></span> <span data-ttu-id="38388-148">Выполняются на последнюю этапе, который по умолчанию является OMCs `PreHandlerExecute`.</span><span class="sxs-lookup"><span data-stu-id="38388-148">OMCs run at the latest stage, which by default is `PreHandlerExecute`.</span></span> <span data-ttu-id="38388-149">Чтобы сделать их для выполнения ранее используются маркеры рабочей области.</span><span class="sxs-lookup"><span data-stu-id="38388-149">The stage markers are used to make them to run earlier.</span></span> <span data-ttu-id="38388-150">При указании маркеры этапа по порядку, мы округление до более ранней маркера.</span><span class="sxs-lookup"><span data-stu-id="38388-150">If you specify stage markers out of order, we round to the earlier marker.</span></span> <span data-ttu-id="38388-151">Другими словами Добавление метку этапа говорится «Запуск не позднее чем этап X».</span><span class="sxs-lookup"><span data-stu-id="38388-151">In other words, adding a stage marker says "Run no later than stage X".</span></span> <span data-ttu-id="38388-152">OMC его выполнения в ранняя метка этапа добавлен после их в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="38388-152">OMC's run at the earliest stage marker added after them in the OWIN pipeline.</span></span>
4. <span data-ttu-id="38388-153">Самым ранним этапом вызовы `app.UseStageMarker` wins.</span><span class="sxs-lookup"><span data-stu-id="38388-153">The earliest stage of calls to `app.UseStageMarker` wins.</span></span> <span data-ttu-id="38388-154">Например, если поменять местами `app.UseStageMarker` вызовы из предыдущего примера:</span><span class="sxs-lookup"><span data-stu-id="38388-154">For example, if you switch the order of `app.UseStageMarker` calls from our previous example:</span></span>

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

 <span data-ttu-id="38388-155">Для отображения в окне вывода:</span><span class="sxs-lookup"><span data-stu-id="38388-155">The output window will display:</span></span> 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

 <span data-ttu-id="38388-156">Работать в OMCs `AuthenticateRequest` этап, так как последний OMC зарегистрирован с `Authenticate` события и `Authenticate` событие предшествует все события.</span><span class="sxs-lookup"><span data-stu-id="38388-156">The OMCs all run in the `AuthenticateRequest` stage, because the last OMC registered with the `Authenticate` event, and the `Authenticate` event precedes all other events.</span></span>