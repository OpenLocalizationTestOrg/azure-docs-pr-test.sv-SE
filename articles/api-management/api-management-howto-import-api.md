---
title: Importera en API i Azure API Management | Microsoft Docs
description: "Lär dig hur du importerar en API och dess åtgärder till Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="51840-103">Så här importerar du definitionen av ett API med åtgärder i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="51840-103">How to import the definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="51840-104">Nya API: er kan skapas i API-hantering och åtgärder läggas till manuellt eller API: et kan importeras tillsammans med operations i ett steg.</span><span class="sxs-lookup"><span data-stu-id="51840-104">In API Management, new APIs can be created and the operations added manually, or the API can be imported along with the operations in one step.</span></span>

<span data-ttu-id="51840-105">API: er och deras verksamhet kan importeras med följande format.</span><span class="sxs-lookup"><span data-stu-id="51840-105">APIs and their operations can be imported using the following formats.</span></span>

* <span data-ttu-id="51840-106">WADL</span><span class="sxs-lookup"><span data-stu-id="51840-106">WADL</span></span>
* <span data-ttu-id="51840-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="51840-107">Swagger</span></span>

<span data-ttu-id="51840-108">Den här guiden visar hur skapa ett nytt API och importera sina åtgärder i ett steg.</span><span class="sxs-lookup"><span data-stu-id="51840-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="51840-109">Mer information om att skapa en API manuellt och lägga till operations finns [hur du skapar API: er] [ How to create APIs] och [lägga till åtgärder i en API][How to add operations to an API].</span><span class="sxs-lookup"><span data-stu-id="51840-109">For information on manually creating an API and adding operations, see [How to create APIs][How to create APIs] and [How to add operations to an API][How to add operations to an API].</span></span>

## <span data-ttu-id="51840-110"><a name="import-api"> </a>Importera ett API</span><span class="sxs-lookup"><span data-stu-id="51840-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="51840-111">API: er skapas och konfigureras i publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="51840-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="51840-112">För att komma åt publisher-portalen klickar du på **Publisher portal** i Azure Portal för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51840-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="51840-113">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="51840-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="51840-115">Klicka på **API: er** från den **API Management** menyn till vänster och klicka sedan på **importera API**.</span><span class="sxs-lookup"><span data-stu-id="51840-115">Click **APIs** from the **API Management** menu on the left, and then click **import API**.</span></span>

![Importera API][api-management-import-apis]

<span data-ttu-id="51840-117">Den **Import API** fönstret har tre flikar som motsvarar de tre sätt att tillhandahålla API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="51840-117">The **Import API** window has three tabs that correspond to the three ways to provide the API specification.</span></span>

* <span data-ttu-id="51840-118">**Från Urklipp** kan du klistra in API-specifikationen i textrutan avsedda.</span><span class="sxs-lookup"><span data-stu-id="51840-118">**From clipboard** allows you to paste the API specification into the designated text box.</span></span>
* <span data-ttu-id="51840-119">**Från filen** kan du bläddra till och välj den fil som innehåller API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="51840-119">**From file** allows you to browse to and select the file that contains the API specification.</span></span>
* <span data-ttu-id="51840-120">**Från URL** kan du ange URL: en för API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="51840-120">**From URL** allows you to supply the URL to the specification for the API.</span></span>

![Importera API-format][api-management-import-api-clipboard]

<span data-ttu-id="51840-122">När du har angett i API-specifikationen Använd alternativknapparna till höger för att ange formatet specifikation.</span><span class="sxs-lookup"><span data-stu-id="51840-122">After providing the API specification, use the radio buttons on the right to indicate the specification format.</span></span> <span data-ttu-id="51840-123">Följande format stöds.</span><span class="sxs-lookup"><span data-stu-id="51840-123">The following formats are supported.</span></span>

* <span data-ttu-id="51840-124">WADL</span><span class="sxs-lookup"><span data-stu-id="51840-124">WADL</span></span>
* <span data-ttu-id="51840-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="51840-125">Swagger</span></span>

<span data-ttu-id="51840-126">Ange en **URL för Web API-suffix**.</span><span class="sxs-lookup"><span data-stu-id="51840-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="51840-127">Detta läggs till en bas-URL för API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51840-127">This is appended to the base URL for your API management service.</span></span> <span data-ttu-id="51840-128">Bas som URL: en är gemensamma för alla API: er som finns på varje instans av en API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51840-128">The base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="51840-129">API Management särskiljer API: er av deras suffix och därmed suffixet måste vara unikt för varje API i en specifik instans för API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51840-129">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="51840-130">När alla värden anges, klickar du på **spara** att skapa API: N och associerade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="51840-130">Once all values are entered, click **Save** to create the API and the associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="51840-131">En genomgång av import av en enkel kalkylator API i Swagger-format finns [hantera din första API i Azure API Management](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="51840-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="51840-132"><a name="export-api"></a> Exportera en API</span><span class="sxs-lookup"><span data-stu-id="51840-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="51840-133">Du kan exportera definitionerna av dina API: er från portalen publisher förutom Importera nya API: er.</span><span class="sxs-lookup"><span data-stu-id="51840-133">In addition to importing new APIs, you can export the definitions of your APIs from the publisher portal.</span></span> <span data-ttu-id="51840-134">Om du vill göra det klickar du på **exportera API** från den **fliken Sammanfattning** av din **API**.</span><span class="sxs-lookup"><span data-stu-id="51840-134">To do so, click **Export API** from the **Summary tab** of your **API**.</span></span>

![Exportera API][api-management-export-api]

<span data-ttu-id="51840-136">API: er kan exporteras med WADL eller Swagger.</span><span class="sxs-lookup"><span data-stu-id="51840-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="51840-137">Välj önskat format, klicka på **spara**, och välj den plats där du vill spara filen.</span><span class="sxs-lookup"><span data-stu-id="51840-137">Select the desired format, click **Save**, and choose the location in which to save the file.</span></span>

![Exportera API-format][api-management-export-api-format]

## <span data-ttu-id="51840-139"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51840-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="51840-140">När en API som har skapats och åtgärder som importeras, kan du granska och konfigurera ytterligare inställningar, Lägg till API: et för en produkt och publicera den så att den är tillgänglig för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="51840-140">Once an API is created and the operations imported, you can review and configure any additional settings, add the API to a Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="51840-141">Mer information finns i följande guider.</span><span class="sxs-lookup"><span data-stu-id="51840-141">For more information, see the following guides.</span></span>

* <span data-ttu-id="51840-142">[Hur du konfigurerar API-inställningar][How to configure API settings]</span><span class="sxs-lookup"><span data-stu-id="51840-142">[How to configure API settings][How to configure API settings]</span></span>
* <span data-ttu-id="51840-143">[Hur du skapar och publicerar en produkt][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="51840-143">[How to create and publish a product][How to create and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
