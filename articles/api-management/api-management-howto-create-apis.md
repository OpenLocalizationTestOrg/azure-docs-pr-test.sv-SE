---
title: 'Hur du skapar API: er i Azure API Management'
description: "Lär dig hur du skapar och konfigurerar API: er i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a><span data-ttu-id="d8fec-103">Hur du skapar API: er i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d8fec-103">How to create APIs in Azure API Management</span></span>
<span data-ttu-id="d8fec-104">En API i API Management representerar en uppsättning åtgärder som kan anropas av klientprogram.</span><span class="sxs-lookup"><span data-stu-id="d8fec-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="d8fec-105">Nya API: er skapas i portalen publisher och sedan önskad åtgärderna har lagts till.</span><span class="sxs-lookup"><span data-stu-id="d8fec-105">New APIs are created in the publisher portal, and then the desired operations are added.</span></span> <span data-ttu-id="d8fec-106">När åtgärderna har lagts till API: et har lagts till i en produkt och kan publiceras.</span><span class="sxs-lookup"><span data-stu-id="d8fec-106">Once the operations are added, the API is added to a product and can be published.</span></span> <span data-ttu-id="d8fec-107">När en API som har publicerats kan den prenumererar på och användas av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="d8fec-107">Once an API is published, it can be subscribed to and used by developers.</span></span>

<span data-ttu-id="d8fec-108">Den här guiden visar det första steget i processen: hur du skapar och konfigurerar ett nytt API i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="d8fec-108">This guide shows the first step in the process: how to create and configure a new API in API Management.</span></span> <span data-ttu-id="d8fec-109">Mer information om att lägga till åtgärder och publicering av en produkt finns [lägga till åtgärder i en API] [ How to add operations to an API] och [hur du skapar och publicerar en produkt][How to create and publish a product].</span><span class="sxs-lookup"><span data-stu-id="d8fec-109">For more information on adding operations and publishing a product, see [How to add operations to an API][How to add operations to an API] and [How to create and publish a product][How to create and publish a product].</span></span>

## <span data-ttu-id="d8fec-110"><a name="create-new-api"></a>Skapa ett nytt API</span><span class="sxs-lookup"><span data-stu-id="d8fec-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="d8fec-111">API: er skapas och konfigureras i publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="d8fec-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="d8fec-112">För att komma åt publisher-portalen klickar du på **Publisher portal** i Azure Portal för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d8fec-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="d8fec-114">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d8fec-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d8fec-115">Klicka på **API: er** från den **API Management** menyn till vänster och klicka sedan på **lägga till API**.</span><span class="sxs-lookup"><span data-stu-id="d8fec-115">Click **APIs** from the **API Management** menu on the left, and then click **add API**.</span></span>

![Skapa API][api-management-create-api]

<span data-ttu-id="d8fec-117">Använd den **lägga till nya API** fönster för att konfigurera nya API: et.</span><span class="sxs-lookup"><span data-stu-id="d8fec-117">Use the **Add new API** window to configure the new API.</span></span>

![Lägga till ett nytt API][api-management-add-new-api]

<span data-ttu-id="d8fec-119">Följande fält används för att konfigurera nya API: et.</span><span class="sxs-lookup"><span data-stu-id="d8fec-119">The following fields are used to configure the new API.</span></span>

* <span data-ttu-id="d8fec-120">**Webb-API-namnet** ger ett unikt och beskrivande namn för API: et.</span><span class="sxs-lookup"><span data-stu-id="d8fec-120">**Web API name** provides a unique and descriptive name for the API.</span></span> <span data-ttu-id="d8fec-121">Den visas i portaler utvecklare och utgivare.</span><span class="sxs-lookup"><span data-stu-id="d8fec-121">It is displayed in the developer and publisher portals.</span></span>
* <span data-ttu-id="d8fec-122">**Webbtjänstens URL** refererar till HTTP-tjänsten implementerar API: et.</span><span class="sxs-lookup"><span data-stu-id="d8fec-122">**Web service URL** references the HTTP service implementing the API.</span></span> <span data-ttu-id="d8fec-123">API-hantering vidarebefordrar begäranden till den här adressen.</span><span class="sxs-lookup"><span data-stu-id="d8fec-123">API management forwards requests to this address.</span></span>
* <span data-ttu-id="d8fec-124">**Web API-URL-suffix** läggs till en bas-URL för API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d8fec-124">**Web API URL suffix** is appended to the base URL for the API management service.</span></span> <span data-ttu-id="d8fec-125">Bas som URL: en är gemensamma för alla API: er som en instans för API Management-tjänsten är värd för.</span><span class="sxs-lookup"><span data-stu-id="d8fec-125">The base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="d8fec-126">API Management särskiljer API: er av deras suffix och därmed suffixet måste vara unikt för alla API: et för en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="d8fec-126">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="d8fec-127">**Web API-URL-schema** avgör vilka protokoll som kan användas för åtkomst till API.</span><span class="sxs-lookup"><span data-stu-id="d8fec-127">**Web API URL scheme** determines which protocols can be used to access the API.</span></span> <span data-ttu-id="d8fec-128">HTTPs har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="d8fec-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="d8fec-129">Du kan också lägga till detta nya Programmeringsgränssnitt för en produkt, klickar du på den **produkter (valfritt)** listrutan och väljer en produkt.</span><span class="sxs-lookup"><span data-stu-id="d8fec-129">To optionally add this new API to a product, click the **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="d8fec-130">Det här steget kan upprepas flera gånger för att lägga till API: et på flera produkter.</span><span class="sxs-lookup"><span data-stu-id="d8fec-130">This step can be repeated multiple times to add the API to multiple products.</span></span>

<span data-ttu-id="d8fec-131">När önskade värden har konfigurerats, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d8fec-131">Once the desired values are configured, click **Save**.</span></span> <span data-ttu-id="d8fec-132">När du har skapat den nya API visas sidan Sammanfattning för API i publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="d8fec-132">Once the new API is created, the summary page for the API is displayed in the publisher portal.</span></span>

![API-sammanfattning][api-management-api-summary]

## <span data-ttu-id="d8fec-134"><a name="configure-api-settings"></a>Konfigurera API-inställningar</span><span class="sxs-lookup"><span data-stu-id="d8fec-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="d8fec-135">Du kan använda den **inställningar** att kontrollera och redigera konfigurationen för en API.</span><span class="sxs-lookup"><span data-stu-id="d8fec-135">You can use the **Settings** tab to verify and edit the configuration for an API.</span></span> <span data-ttu-id="d8fec-136">**Webb-API-namnet**, **-webbtjänstens URL**, och **URL för Web API-suffix** från början anges när API: N skapas och kan ändras här.</span><span class="sxs-lookup"><span data-stu-id="d8fec-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when the API is created and can be modified here.</span></span> <span data-ttu-id="d8fec-137">**Beskrivning** ger en valfri beskrivning och **Web API-URL-schema** avgör vilka protokoll som kan användas för åtkomst till API.</span><span class="sxs-lookup"><span data-stu-id="d8fec-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used to access the API.</span></span>

![API-inställningar][api-management-api-settings]

<span data-ttu-id="d8fec-139">För att konfigurera gateway-autentisering för backend-tjänsten implementerar API: et, Välj den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="d8fec-139">To configure gateway authentication for the backend service implementing the API, select the **Security** tab.</span></span> <span data-ttu-id="d8fec-140">Den **med autentiseringsuppgifter** listrutan kan användas för att konfigurera **HTTP basic** eller **klientcertifikat** autentisering.</span><span class="sxs-lookup"><span data-stu-id="d8fec-140">The **With credentials** drop-down can be used to configure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="d8fec-141">Om du vill använda grundläggande HTTP-autentisering anger du bara de önskade autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="d8fec-141">To use HTTP basic authentication, simply enter the desired credentials.</span></span> <span data-ttu-id="d8fec-142">Information om hur du använder certifikat för klientautentisering finns [säkra backend-tjänster som använder klienten certifikatautentisering i Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d8fec-142">For information on using client certificate authentication, see [How to secure back-end services using client certificate authentication in Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="d8fec-143">Den **säkerhet** fliken kan också användas för att konfigurera **användarautentiseringen** med hjälp av OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="d8fec-143">The **Security** tab can also be used to configure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="d8fec-144">Mer information finns i [så att auktorisera developer konton med hjälp av OAuth 2.0 i Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d8fec-144">For more information, see [How to authorize developer accounts using OAuth 2.0 in Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Inställningar för grundläggande autentisering][api-management-api-settings-credentials]

<span data-ttu-id="d8fec-146">Klicka på **spara** att spara ändringar som görs till API-inställningar.</span><span class="sxs-lookup"><span data-stu-id="d8fec-146">Click **Save** to save any changes you make to the API settings.</span></span>

## <span data-ttu-id="d8fec-147"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8fec-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="d8fec-148">När en API som har skapats och de konfigurerade inställningarna nästa steg är att lägga till åtgärder i API: et, Lägg till API: et för en produkt och publicera den så att den är tillgänglig för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="d8fec-148">Once an API is created and the settings configured, the next steps are to add the operations to the API, add the API to a product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="d8fec-149">Mer information finns i följande artiklar.</span><span class="sxs-lookup"><span data-stu-id="d8fec-149">For more information, see the following articles.</span></span>

* <span data-ttu-id="d8fec-150">[Lägga till åtgärder i en API][How to add operations to an API]</span><span class="sxs-lookup"><span data-stu-id="d8fec-150">[How to add operations to an API][How to add operations to an API]</span></span>
* <span data-ttu-id="d8fec-151">[Hur du skapar och publicerar en produkt][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="d8fec-151">[How to create and publish a product][How to create and publish a product]</span></span>

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
