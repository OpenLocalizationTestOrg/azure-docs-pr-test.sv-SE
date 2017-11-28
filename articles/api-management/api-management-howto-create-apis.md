---
title: 'aaaHow toocreate API: er i Azure API Management'
description: "Lär dig hur toocreate och konfigurera API: er i Azure API Management."
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
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a><span data-ttu-id="bac2c-103">Hur toocreate API: er i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="bac2c-103">How toocreate APIs in Azure API Management</span></span>
<span data-ttu-id="bac2c-104">En API i API Management representerar en uppsättning åtgärder som kan anropas av klientprogram.</span><span class="sxs-lookup"><span data-stu-id="bac2c-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="bac2c-105">Nya API: er skapas i hello publisher portal och sedan hello önskad operations har lagts till.</span><span class="sxs-lookup"><span data-stu-id="bac2c-105">New APIs are created in hello publisher portal, and then hello desired operations are added.</span></span> <span data-ttu-id="bac2c-106">När hello operations läggs hello API läggs tooa produkten och publiceras.</span><span class="sxs-lookup"><span data-stu-id="bac2c-106">Once hello operations are added, hello API is added tooa product and can be published.</span></span> <span data-ttu-id="bac2c-107">När en API som har publicerats kan vara det prenumererar på tooand som används av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="bac2c-107">Once an API is published, it can be subscribed tooand used by developers.</span></span>

<span data-ttu-id="bac2c-108">Den här guiden visar hello första steget i processen för hello: hur toocreate och konfigurera ett nytt API i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="bac2c-108">This guide shows hello first step in hello process: how toocreate and configure a new API in API Management.</span></span> <span data-ttu-id="bac2c-109">Mer information om att lägga till åtgärder och publicering av en produkt finns [hur tooadd operations tooan API] [ How tooadd operations tooan API] och [hur toocreate och publicera en produkt] [ How toocreate and publish a product].</span><span class="sxs-lookup"><span data-stu-id="bac2c-109">For more information on adding operations and publishing a product, see [How tooadd operations tooan API][How tooadd operations tooan API] and [How toocreate and publish a product][How toocreate and publish a product].</span></span>

## <span data-ttu-id="bac2c-110"><a name="create-new-api"></a>Skapa ett nytt API</span><span class="sxs-lookup"><span data-stu-id="bac2c-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="bac2c-111">API: er skapas och konfigureras i hello publisher portal.</span><span class="sxs-lookup"><span data-stu-id="bac2c-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="bac2c-112">tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bac2c-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="bac2c-114">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="bac2c-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="bac2c-115">Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till API**.</span><span class="sxs-lookup"><span data-stu-id="bac2c-115">Click **APIs** from hello **API Management** menu on hello left, and then click **add API**.</span></span>

![Skapa API][api-management-create-api]

<span data-ttu-id="bac2c-117">Använd hello **lägga till nya API** fönstret tooconfigure hello nya API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-117">Use hello **Add new API** window tooconfigure hello new API.</span></span>

![Lägga till ett nytt API][api-management-add-new-api]

<span data-ttu-id="bac2c-119">hello följande fält är används tooconfigure hello nya API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-119">hello following fields are used tooconfigure hello new API.</span></span>

* <span data-ttu-id="bac2c-120">**Webb-API-namnet** ger ett unikt och beskrivande namn för hello API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-120">**Web API name** provides a unique and descriptive name for hello API.</span></span> <span data-ttu-id="bac2c-121">Den visas i hello utvecklare och utgivare portaler.</span><span class="sxs-lookup"><span data-stu-id="bac2c-121">It is displayed in hello developer and publisher portals.</span></span>
* <span data-ttu-id="bac2c-122">**Webbtjänstens URL** referenser hello HTTP-tjänsten implementerar hello API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-122">**Web service URL** references hello HTTP service implementing hello API.</span></span> <span data-ttu-id="bac2c-123">API-hantering vidarebefordrar begäranden toothis adress.</span><span class="sxs-lookup"><span data-stu-id="bac2c-123">API management forwards requests toothis address.</span></span>
* <span data-ttu-id="bac2c-124">**Web API-URL-suffix** är tillagda toohello bas-URL för hello API management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bac2c-124">**Web API URL suffix** is appended toohello base URL for hello API management service.</span></span> <span data-ttu-id="bac2c-125">hello bas-URL är gemensamma för alla API: er som en instans för API Management-tjänsten är värd för.</span><span class="sxs-lookup"><span data-stu-id="bac2c-125">hello base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="bac2c-126">API Management särskiljer API: er av deras suffix och därför hello suffix måste vara unikt för alla API: et för en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="bac2c-126">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="bac2c-127">**Web API-URL-schema** anger vilka protokoll som kan vara används tooaccess hello API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-127">**Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span> <span data-ttu-id="bac2c-128">HTTPs har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="bac2c-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="bac2c-129">toooptionally Lägg till den här nya API tooa produkten, markera hello **produkter (valfritt)** listrutan och väljer en produkt.</span><span class="sxs-lookup"><span data-stu-id="bac2c-129">toooptionally add this new API tooa product, click hello **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="bac2c-130">Det här steget kan vara upprepas flera gånger tooadd hello API toomultiple produkter.</span><span class="sxs-lookup"><span data-stu-id="bac2c-130">This step can be repeated multiple times tooadd hello API toomultiple products.</span></span>

<span data-ttu-id="bac2c-131">När hello önskade värden har konfigurerats, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bac2c-131">Once hello desired values are configured, click **Save**.</span></span> <span data-ttu-id="bac2c-132">När du har skapat hello nya API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="bac2c-132">Once hello new API is created, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![API-sammanfattning][api-management-api-summary]

## <span data-ttu-id="bac2c-134"><a name="configure-api-settings"></a>Konfigurera API-inställningar</span><span class="sxs-lookup"><span data-stu-id="bac2c-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="bac2c-135">Du kan använda hello **inställningar** fliken tooverify och redigera hello konfigurationen för en API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-135">You can use hello **Settings** tab tooverify and edit hello configuration for an API.</span></span> <span data-ttu-id="bac2c-136">**Webb-API-namnet**, **-webbtjänstens URL**, och **URL för Web API-suffix** från början anges när hello API skapas och kan ändras här.</span><span class="sxs-lookup"><span data-stu-id="bac2c-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when hello API is created and can be modified here.</span></span> <span data-ttu-id="bac2c-137">**Beskrivning** ger en valfri beskrivning och **Web API-URL-schema** anger vilka protokoll som kan vara används tooaccess hello API.</span><span class="sxs-lookup"><span data-stu-id="bac2c-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span>

![API-inställningar][api-management-api-settings]

<span data-ttu-id="bac2c-139">tooconfigure gateway-autentisering för hello backend tjänsten implementerar hello API, Välj hello **säkerhet** fliken hello **med autentiseringsuppgifter** listrutan kan vara används tooconfigure **HTTP grundläggande** eller **klientcertifikat** autentisering.</span><span class="sxs-lookup"><span data-stu-id="bac2c-139">tooconfigure gateway authentication for hello backend service implementing hello API, select hello **Security** tab. hello **With credentials** drop-down can be used tooconfigure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="bac2c-140">toouse HTTP grundläggande autentisering, anger du bara hello önskad autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bac2c-140">toouse HTTP basic authentication, simply enter hello desired credentials.</span></span> <span data-ttu-id="bac2c-141">Information om hur du använder certifikat för klientautentisering finns [hur toosecure backend-tjänster som använder klienten certifikatautentisering i Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="bac2c-141">For information on using client certificate authentication, see [How toosecure back-end services using client certificate authentication in Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="bac2c-142">Hej **säkerhet** flik kan också vara används tooconfigure **användarautentiseringen** med hjälp av OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="bac2c-142">hello **Security** tab can also be used tooconfigure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="bac2c-143">Mer information finns i [hur tooauthorize developer användarkonton med OAuth 2.0 i Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="bac2c-143">For more information, see [How tooauthorize developer accounts using OAuth 2.0 in Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Inställningar för grundläggande autentisering][api-management-api-settings-credentials]

<span data-ttu-id="bac2c-145">Klicka på **spara** toosave alla ändringar du gör toohello API-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="bac2c-145">Click **Save** toosave any changes you make toohello API settings.</span></span>

## <span data-ttu-id="bac2c-146"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bac2c-146"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="bac2c-147">När en API som har skapats och hello-inställningarna, hello nästa steg är tooadd hello operations toohello API, lägga till hello API tooa produkt och publicera den så att den är tillgänglig för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="bac2c-147">Once an API is created and hello settings configured, hello next steps are tooadd hello operations toohello API, add hello API tooa product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="bac2c-148">Mer information finns i följande artiklar hello.</span><span class="sxs-lookup"><span data-stu-id="bac2c-148">For more information, see hello following articles.</span></span>

* <span data-ttu-id="bac2c-149">[Hur tooadd operations tooan API][How tooadd operations tooan API]</span><span class="sxs-lookup"><span data-stu-id="bac2c-149">[How tooadd operations tooan API][How tooadd operations tooan API]</span></span>
* <span data-ttu-id="bac2c-150">[Hur toocreate och publicera en produkt][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="bac2c-150">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
