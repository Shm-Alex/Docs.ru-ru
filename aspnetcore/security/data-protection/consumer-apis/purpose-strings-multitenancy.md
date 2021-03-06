---
title: Иерархия назначений и Мультитенантность в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о строка иерархия назначений и Мультитенантность по отношению к API защиты данных ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/10/2018
ms.locfileid: "41827518"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Иерархия назначений и Мультитенантность в ASP.NET Core

Так как `IDataProtector` неявно `IDataProtectionProvider`, целей можно связывать друг с другом. В этом смысле `provider.CreateProtector([ "purpose1", "purpose2" ])` эквивалентен `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Это позволяет выполнять некоторые интересные иерархические связи через систему защиты данных. В приведенном ранее примере [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), можно вызвать компонент SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` после первоначальных и кэш-памяти результат в закрытую `_myProvider` поля. Будущие предохранители можно создать путем вызова метода `_myProvider.CreateProtector("User: username")`, и эти предохранители ключа будет использоваться для защиты отдельных сообщений.

Это может также быть перевернуто. Рассмотрим один логический приложение узлы нескольких клиентов (это система управления Содержимым кажется вполне разумным) и каждого клиента можно настроить с помощью собственной проверки подлинности и состояния управления системой. Зонтик приложение имеет один главный поставщик, и он вызывает `provider.CreateProtector("Tenant 1")` и `provider.CreateProtector("Tenant 2")` предоставить каждому клиенту собственный изолированной срез система защиты данных. Клиенты затем создать свои собственные отдельных средств защиты, исходя из собственных потребностей, но независимо от того, насколько сильно они пытаются выполнить не могут создавать средства защиты, которые конфликтуют с любым другим клиентом в системе. Графически оно представлено ниже.

![С несколькими аренды целей](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> При этом предполагается считать элементы управления приложениями, какие интерфейсы API доступны для отдельных клиентов и что клиенты не могут выполнять произвольный код на сервере. Если клиент может выполняться произвольный код, они удалось выполнить отражение закрытых переключиться в гарантии изоляции или они может просто выполнить чтение материала основного напрямую и являются производными любые подразделы пожелает.

Система защиты данных использует своего рода Многопользовательские приложения в конфигурации по умолчанию out of box. По умолчанию главный материал ключа хранится в папке профиля пользователя учетной записи процесса рабочей роли (или реестр, для удостоверения пула приложений IIS). Но фактически довольно часто, чтобы использовать одну учетную запись для запуска нескольких приложений, и таким образом все эти приложения появятся общего доступа к хозяину материала. Чтобы решить эту проблему, система защиты данных автоматически вставляет идентификатор уникальный каждого приложения как первый элемент в цепочке общего назначения. Этой цели неявное служит для [изолировать отдельные приложения](xref:security/data-protection/configuration/overview#per-application-isolation) друг у друга рассматривая эффективно каждое приложение как уникальный клиента в системе и процесс создания предохранителя выглядит так же на рисунке выше.
