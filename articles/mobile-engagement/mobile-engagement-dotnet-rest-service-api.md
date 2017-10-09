---
title: 'aaaUsing hello REST API tooaccess Azure Mobile Engagement Service API: er'
description: 'Beskriver hur toouse hello Mobile Engagement REST API: er tooaccess Azure Mobile Engagement Service API: er'
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 315761299a42df6f65e81df20e632f9713844b0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-rest-tooaccess-azure-mobile-engagement-service-apis"></a><span data-ttu-id="31d4e-103">Med hjälp av REST tooaccess Azure Mobile Engagement Service API: er</span><span class="sxs-lookup"><span data-stu-id="31d4e-103">Using REST tooaccess Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="31d4e-104">Azure Mobile Engagement tillhandahåller hello [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) du toomanage enheter Reach/Push-kampanjer osv.</span><span class="sxs-lookup"><span data-stu-id="31d4e-104">Azure Mobile Engagement provides hello [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) for you toomanage Devices, Reach/Push campaigns etc.</span></span>

> [!NOTE]
> <span data-ttu-id="31d4e-105">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="31d4e-105">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="31d4e-106">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="31d4e-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="31d4e-107">Om du inte vill toouse hello REST API: er direkt vi tillhandahåller även en [Swagger-filen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) som du kan använda med verktygen toogenerate SDK: er för ditt språk.</span><span class="sxs-lookup"><span data-stu-id="31d4e-107">If you do not want toouse hello REST APIs directly, we also provide a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools toogenerate SDKs for your preferred language.</span></span> <span data-ttu-id="31d4e-108">Vi rekommenderar att du använder hello [AutoRest](https://github.com/Azure/AutoRest) verktyget toogenerate din SDK från våra Swagger-fil.</span><span class="sxs-lookup"><span data-stu-id="31d4e-108">We recommend using hello [AutoRest](https://github.com/Azure/AutoRest) tool toogenerate your SDK from our Swagger file.</span></span> <span data-ttu-id="31d4e-109">Vi har skapat en .NET-SDK på liknande sätt som gör att du toointeract med dessa API: er med hjälp av en C#-omslutning och du har inte toodo hello autentisering token förhandling och uppdatera själv.</span><span class="sxs-lookup"><span data-stu-id="31d4e-109">We have created a .NET SDK in a similar manner which allows you toointeract with these APIs using a C# wrapper and you don't have toodo hello authentication token negotiation and refresh yourself.</span></span> <span data-ttu-id="31d4e-110">Se [Service API .NET SDK-exempel](mobile-engagement-dotnet-sdk-service-api.md) toolearn hur toouse hello .net SDK-API: t</span><span class="sxs-lookup"><span data-stu-id="31d4e-110">See [Service API .NET SDK Sample](mobile-engagement-dotnet-sdk-service-api.md) toolearn how toouse hello .net SDK for API</span></span>
