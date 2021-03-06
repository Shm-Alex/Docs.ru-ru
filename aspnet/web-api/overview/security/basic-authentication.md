---
uid: web-api/overview/security/basic-authentication
title: Обычная проверка подлинности в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: Описывает использование обычной проверки подлинности в веб-API ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838607"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Обычная проверка подлинности в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Обычная проверка подлинности определена в [RFC 2617, проверка подлинности HTTP: Basic and Digest Authentication доступа](http://www.ietf.org/rfc/rfc2617.txt).

Недостатки

- Учетные данные отправляются в запросе.
- Учетные данные отправляются в виде открытого текста.
- Учетные данные отправляются с каждым запросом.
- Невозможно выйти, за исключением путем завершения сеанса браузера.
- Подвержена межсайтовых подделки запросов (CSRF); требуется anti-CSRF меры.

Преимущества

- Стандарты Интернета.
- Поддерживается всеми основными браузерами.
- Довольно простой протокол.

Обычная проверка подлинности работает следующим образом:

1. Если запрос требует проверки подлинности, сервер возвращает 401 (не санкционировано). Ответ включает заголовок WWW-Authenticate, указывающий, что сервер поддерживает обычную проверку подлинности.
2. Клиент отправляет другой запрос, с помощью учетных данных клиента в заголовке авторизации. Учетные данные форматируются в виде строки «имя: password», с кодировкой base64. Учетные данные не шифруются.

Обычная проверка подлинности выполняется в контексте «область». Сервер включает имя области в заголовке WWW-Authenticate. Учетные данные пользователя допустимы в пределах этой области. Точный объем область определяется сервером. Например можно определить несколько областей в порядке к ресурсам секции.

![](basic-authentication/_static/image1.png)

Так как учетные данные отсылаются нешифрованными, обычная проверка подлинности защищен только по протоколу HTTPS. См. в разделе [работа с SSL в веб-API](working-with-ssl-in-web-api.md).

Обычная проверка подлинности также является уязвимым к CSRF-атакам. После того как пользователь введет учетные данные, браузер автоматически отправляет их на последующие запросы к тому же домену, в течение сеанса. Сюда входят запросы AJAX. См. в разделе [атак с подделкой межсайтовых запросов](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Проверка подлинности IIS

IIS поддерживает обычную проверку подлинности, но Будьте осторожны: пользователь прошел проверку учетных данных Windows. Это означает, что пользователь должен иметь учетную запись в домене сервера. Общедоступный веб-сайта обычно требуется для проверки подлинности поставщик членства ASP.NET.

Чтобы включить обычную проверку подлинности с помощью служб IIS, задайте режим проверки подлинности «Windows» в файле Web.config проекта ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

В этом режиме IIS использует учетные данные Windows для проверки подлинности. Кроме того необходимо включить обычную проверку подлинности в службах IIS. В диспетчере служб IIS, перейдите к представлению функций, выберите проверку подлинности и включить обычную проверку подлинности.

![](basic-authentication/_static/image2.png)

В проекте веб-API добавьте `[Authorize]` атрибут для любого действия контроллера, требующих проверки подлинности.

Клиент проходит проверку подлинности, задав для заголовка авторизации в запросе. Клиенты браузера выполнить этот шаг автоматически. Клиенты nonbrowser потребуется задать заголовок.

## <a name="basic-authentication-with-custom-membership"></a>Обычная проверка подлинности с помощью настраиваемого членства

Как уже упоминалось, обычная проверка подлинности, встроенные в IIS использует учетные данные Windows. Это означает, что необходимо для создания пользовательских учетных записей на сервере размещения. Но для Интернет-приложения, учетные записи пользователей обычно хранятся во внешней базе данных.

В следующем примере кода, как модуль HTTP, который выполняет обычную проверку подлинности. Вы можете легко подключить поставщик членства ASP.NET, заменив `CheckPassword` метод, который является пустой метод в этом примере.

В веб-API 2, следует рассмотреть записи [фильтр проверки подлинности](authentication-filters.md) или [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/index.md), а не HTTP-модуль.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Чтобы включить HTTP-модуль, добавьте следующее в файл web.config в **system.webServer** раздел:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Замените на имя сборки (не включая расширение «dll») «Имя_сборки».

Следует отключить другие схемы проверки подлинности, например проверки подлинности Forms или Windows
