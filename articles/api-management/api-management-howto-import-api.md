---
title: aaaImport en API i Azure API Management | Microsoft Docs
description: "Lär dig hur tooimport API och dess åtgärder i Azure API Management."
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
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="067fd-103">Hur tooimport hello definitionen av ett API med åtgärder i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="067fd-103">How tooimport hello definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="067fd-104">Nya API: er kan skapas i API Management och hello åtgärder läggas till manuellt eller hello API kan importeras tillsammans med hello operations i ett enda steg.</span><span class="sxs-lookup"><span data-stu-id="067fd-104">In API Management, new APIs can be created and hello operations added manually, or hello API can be imported along with hello operations in one step.</span></span>

<span data-ttu-id="067fd-105">API: er och deras verksamhet kan importeras med hello följande format.</span><span class="sxs-lookup"><span data-stu-id="067fd-105">APIs and their operations can be imported using hello following formats.</span></span>

* <span data-ttu-id="067fd-106">WADL</span><span class="sxs-lookup"><span data-stu-id="067fd-106">WADL</span></span>
* <span data-ttu-id="067fd-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="067fd-107">Swagger</span></span>

<span data-ttu-id="067fd-108">Den här guiden visar hur skapa ett nytt API och importera sina åtgärder i ett steg.</span><span class="sxs-lookup"><span data-stu-id="067fd-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="067fd-109">Mer information om att skapa en API manuellt och lägga till operations finns [hur toocreate API: er] [ How toocreate APIs] och [hur tooadd operations tooan API] [ How tooadd operations tooan API].</span><span class="sxs-lookup"><span data-stu-id="067fd-109">For information on manually creating an API and adding operations, see [How toocreate APIs][How toocreate APIs] and [How tooadd operations tooan API][How tooadd operations tooan API].</span></span>

## <span data-ttu-id="067fd-110"><a name="import-api"> </a>Importera ett API</span><span class="sxs-lookup"><span data-stu-id="067fd-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="067fd-111">API: er skapas och konfigureras i hello publisher portal.</span><span class="sxs-lookup"><span data-stu-id="067fd-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="067fd-112">tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="067fd-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="067fd-113">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="067fd-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="067fd-115">Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **importera API**.</span><span class="sxs-lookup"><span data-stu-id="067fd-115">Click **APIs** from hello **API Management** menu on hello left, and then click **import API**.</span></span>

![Importera API][api-management-import-apis]

<span data-ttu-id="067fd-117">Hej **Import API** fönstret har tre flikar som motsvarar toohello tre sätt tooprovide hello API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="067fd-117">hello **Import API** window has three tabs that correspond toohello three ways tooprovide hello API specification.</span></span>

* <span data-ttu-id="067fd-118">**Från Urklipp** tillåter toopaste hello API-specifikationen i hello avsedda textruta.</span><span class="sxs-lookup"><span data-stu-id="067fd-118">**From clipboard** allows you toopaste hello API specification into hello designated text box.</span></span>
* <span data-ttu-id="067fd-119">**Från filen** kan du toobrowse tooand väljer hello-fil som innehåller hello API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="067fd-119">**From file** allows you toobrowse tooand select hello file that contains hello API specification.</span></span>
* <span data-ttu-id="067fd-120">**Från URL** kan du toosupply hello URL toohello specifikationen för hello API.</span><span class="sxs-lookup"><span data-stu-id="067fd-120">**From URL** allows you toosupply hello URL toohello specification for hello API.</span></span>

![Importera API-format][api-management-import-api-clipboard]

<span data-ttu-id="067fd-122">Använd hello alternativknappar på hello rätt tooindicate hello-specifikationen format när du har angett hello API-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="067fd-122">After providing hello API specification, use hello radio buttons on hello right tooindicate hello specification format.</span></span> <span data-ttu-id="067fd-123">följande format hello stöds.</span><span class="sxs-lookup"><span data-stu-id="067fd-123">hello following formats are supported.</span></span>

* <span data-ttu-id="067fd-124">WADL</span><span class="sxs-lookup"><span data-stu-id="067fd-124">WADL</span></span>
* <span data-ttu-id="067fd-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="067fd-125">Swagger</span></span>

<span data-ttu-id="067fd-126">Ange en **URL för Web API-suffix**.</span><span class="sxs-lookup"><span data-stu-id="067fd-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="067fd-127">Detta är tillagda toohello bas-URL för API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="067fd-127">This is appended toohello base URL for your API management service.</span></span> <span data-ttu-id="067fd-128">hello bas-URL är gemensamma för alla API: er som finns på varje instans av en API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="067fd-128">hello base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="067fd-129">API Management särskiljer API: er av deras suffix och därför hello suffix måste vara unikt för varje API i en specifik instans för API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="067fd-129">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="067fd-130">När alla värden anges, klickar du på **spara** toocreate hello API och hello tillhörande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="067fd-130">Once all values are entered, click **Save** toocreate hello API and hello associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="067fd-131">En genomgång av import av en enkel kalkylator API i Swagger-format finns [hantera din första API i Azure API Management](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="067fd-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="067fd-132"><a name="export-api"></a> Exportera en API</span><span class="sxs-lookup"><span data-stu-id="067fd-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="067fd-133">I tillägg tooimporting nya API: er, kan du exportera hello definitioner av dina API: er från hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="067fd-133">In addition tooimporting new APIs, you can export hello definitions of your APIs from hello publisher portal.</span></span> <span data-ttu-id="067fd-134">toodo Klicka på **exportera API** från hello **fliken Sammanfattning** av din **API**.</span><span class="sxs-lookup"><span data-stu-id="067fd-134">toodo so, click **Export API** from hello **Summary tab** of your **API**.</span></span>

![Exportera API][api-management-export-api]

<span data-ttu-id="067fd-136">API: er kan exporteras med WADL eller Swagger.</span><span class="sxs-lookup"><span data-stu-id="067fd-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="067fd-137">Välj önskat hello-format, klicka på **spara**, och välj hello plats i vilken toosave hello-fil.</span><span class="sxs-lookup"><span data-stu-id="067fd-137">Select hello desired format, click **Save**, and choose hello location in which toosave hello file.</span></span>

![Exportera API-format][api-management-export-api-format]

## <span data-ttu-id="067fd-139"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="067fd-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="067fd-140">När en API som har skapats och hello åtgärder importeras, som du kan granska och konfigurera ytterligare inställningar, Lägg till hello API tooa produkt och publicera den så att den är tillgänglig för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="067fd-140">Once an API is created and hello operations imported, you can review and configure any additional settings, add hello API tooa Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="067fd-141">Mer information finns i följande guider hello.</span><span class="sxs-lookup"><span data-stu-id="067fd-141">For more information, see hello following guides.</span></span>

* <span data-ttu-id="067fd-142">[Hur tooconfigure API-inställningar][How tooconfigure API settings]</span><span class="sxs-lookup"><span data-stu-id="067fd-142">[How tooconfigure API settings][How tooconfigure API settings]</span></span>
* <span data-ttu-id="067fd-143">[Hur toocreate och publicera en produkt][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="067fd-143">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
