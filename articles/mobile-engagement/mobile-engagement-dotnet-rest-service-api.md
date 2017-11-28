---
title: "Använda REST API för att komma åt Azure Mobile Engagement Service API: er"
description: "Beskriver hur du använder Mobile Engagement REST-API: er för att komma åt Azure Mobile Engagement Service API: er"
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
ms.openlocfilehash: 4b21bca6fee7012ce1dba96950ae075eded414f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a><span data-ttu-id="236a1-103">Med hjälp av REST API: er Azure Mobile Engagement Service</span><span class="sxs-lookup"><span data-stu-id="236a1-103">Using REST to access Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="236a1-104">Azure Mobile Engagement tillhandahåller den [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) att hantera enheter, Reach/Push-kampanjer osv.</span><span class="sxs-lookup"><span data-stu-id="236a1-104">Azure Mobile Engagement provides the [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) for you to manage Devices, Reach/Push campaigns etc.</span></span>

> [!NOTE]
> <span data-ttu-id="236a1-105">Tjänsten Azure Mobile Engagement kommer att dras tillbaka i mars 2018 och är för närvarande endast tillgänglig för befintliga kunder.</span><span class="sxs-lookup"><span data-stu-id="236a1-105">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="236a1-106">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="236a1-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="236a1-107">Om du inte vill använda REST-API: er direkt vi tillhandahåller även en [Swagger-filen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) att du kan använda med verktyg för att generera SDK: er för ditt språk.</span><span class="sxs-lookup"><span data-stu-id="236a1-107">If you do not want to use the REST APIs directly, we also provide a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools to generate SDKs for your preferred language.</span></span> <span data-ttu-id="236a1-108">Vi rekommenderar att du använder den [AutoRest](https://github.com/Azure/AutoRest) verktyget att generera din SDK från våra Swagger-fil.</span><span class="sxs-lookup"><span data-stu-id="236a1-108">We recommend using the [AutoRest](https://github.com/Azure/AutoRest) tool to generate your SDK from our Swagger file.</span></span> <span data-ttu-id="236a1-109">Vi har skapat en .NET-SDK på ett liknande sätt som gör att du kan interagera med dessa API: er med hjälp av en C#-omslutning och du behöver inte göra autentisering token förhandlingen och uppdatera själv.</span><span class="sxs-lookup"><span data-stu-id="236a1-109">We have created a .NET SDK in a similar manner which allows you to interact with these APIs using a C# wrapper and you don't have to do the authentication token negotiation and refresh yourself.</span></span> <span data-ttu-id="236a1-110">Se [Service API .NET SDK-exempel](mobile-engagement-dotnet-sdk-service-api.md) att lära dig hur du använder .net SDK-API: t</span><span class="sxs-lookup"><span data-stu-id="236a1-110">See [Service API .NET SDK Sample](mobile-engagement-dotnet-sdk-service-api.md) to learn how to use the .net SDK for API</span></span>
