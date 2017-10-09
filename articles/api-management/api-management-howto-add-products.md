---
title: aaaHow toocreate och publicera en produkt i Azure API Management
description: "Lär dig hur toocreate och publicera produkter i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a><span data-ttu-id="bf609-103">Hur toocreate och publicera en produkt i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="bf609-103">How toocreate and publish a product in Azure API Management</span></span>
<span data-ttu-id="bf609-104">En produkt innehåller en eller flera API: er samt användning kvoten och hello användningsvillkor i Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="bf609-104">In Azure API Management, a product contains one or more APIs as well as a usage quota and hello terms of use.</span></span> <span data-ttu-id="bf609-105">När en produkt har publicerats kan utvecklare prenumerera toohello produkt och börja toouse hello produkten API: er.</span><span class="sxs-lookup"><span data-stu-id="bf609-105">Once a product is published, developers can subscribe toohello product and begin toouse hello product's APIs.</span></span> <span data-ttu-id="bf609-106">hello avsnittet innehåller en guide toocreating en produkt, lägga till en API och publicera för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="bf609-106">hello topic provides a guide toocreating a product, adding an API, and publishing it for developers.</span></span>

## <span data-ttu-id="bf609-107"><a name="create-product"></a>Skapar en produkt</span><span class="sxs-lookup"><span data-stu-id="bf609-107"><a name="create-product"> </a>Create a product</span></span>
<span data-ttu-id="bf609-108">Åtgärder har lagts till och konfigurerats tooan API i hello publisher portal.</span><span class="sxs-lookup"><span data-stu-id="bf609-108">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="bf609-109">tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bf609-109">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="bf609-111">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="bf609-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="bf609-112">Klicka på **produkter** hello menyn hello vänstra toodisplay hello **produkter** och klicka på **lägga till produkten**.</span><span class="sxs-lookup"><span data-stu-id="bf609-112">Click on **Products** in hello menu on hello left toodisplay hello **Products** page, and click **Add Product**.</span></span>

![Produkter][api-management-products]

![Ny produkt][api-management-add-new-product]

<span data-ttu-id="bf609-115">Ange ett beskrivande namn för hello produkt i hello **namn** fältet och en beskrivning av hello produkt i hello **beskrivning** fältet.</span><span class="sxs-lookup"><span data-stu-id="bf609-115">Enter a descriptive name for hello product in hello **Name** field and a description of hello product in hello **Description** field.</span></span>

<span data-ttu-id="bf609-116">Produkter i API-hantering kan vara **öppna** eller **skyddade**.</span><span class="sxs-lookup"><span data-stu-id="bf609-116">Products in API Management can be **Open** or **Protected**.</span></span> <span data-ttu-id="bf609-117">Skyddade produkter måste vara prenumererade toobefore som de kan användas, öppen produkter kan användas utan en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bf609-117">Protected products must be subscribed toobefore they can be used, while open products can be used without a subscription.</span></span> <span data-ttu-id="bf609-118">Kontrollera **kräver prenumeration** toocreate en skyddad produkt som kräver en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bf609-118">Check **Require subscription** toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="bf609-119">Det här är standardinställningen för hello.</span><span class="sxs-lookup"><span data-stu-id="bf609-119">This is hello default setting.</span></span>

<span data-ttu-id="bf609-120">Kontrollera **kräver godkännande för prenumerationen** om du vill att en administratör tooreview och godkänna eller avvisa prenumeration försöker toothis produkten.</span><span class="sxs-lookup"><span data-stu-id="bf609-120">Check **Require subscription approval** if you want an administrator tooreview and accept or reject subscription attempts toothis product.</span></span> <span data-ttu-id="bf609-121">Om hello rutan är avmarkerad att prenumeration försök automatiskt godkänd.</span><span class="sxs-lookup"><span data-stu-id="bf609-121">If hello box is unchecked, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="bf609-122">Mer information om prenumerationer finns [visa prenumeranter tooa produkt][View subscribers tooa product].</span><span class="sxs-lookup"><span data-stu-id="bf609-122">For more information on subscriptions, see [View subscribers tooa product][View subscribers tooa product].</span></span>

<span data-ttu-id="bf609-123">tooallow developer konton toosubscribe flera gånger toohello produkt, kontrollera hello **tillåter flera prenumerationer** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="bf609-123">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box.</span></span> <span data-ttu-id="bf609-124">Om inte den här kryssrutan är markerad, kan varje utvecklarkonto prenumerera på bara en gång toohello produkt.</span><span class="sxs-lookup"><span data-stu-id="bf609-124">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

![Obegränsade flera prenumerationer][api-management-unlimited-multiple-subscriptions]

<span data-ttu-id="bf609-126">toolimit hello antal flera samtidiga prenumerationer, kontrollera hello **begränsa antalet samtidiga prenumerationer** kryssrutan och ange hello gränsen.</span><span class="sxs-lookup"><span data-stu-id="bf609-126">toolimit hello count of multiple simultaneous subscriptions, check hello **Limit number of simultaneous subscriptions to** check box and enter hello subscription limit.</span></span> <span data-ttu-id="bf609-127">I följande exempel hello, är samtidiga prenumerationer begränsad toofour per utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="bf609-127">In hello following example, simultaneous subscriptions are limited toofour per developer account.</span></span>

![Fyra flera prenumerationer][api-management-four-multiple-subscriptions]

<span data-ttu-id="bf609-129">När alla nya produktalternativ för har konfigurerats, klickar du på **spara** toocreate hello ny produkt.</span><span class="sxs-lookup"><span data-stu-id="bf609-129">Once all new product options are configured, click **Save** toocreate hello new product.</span></span>

![Produkter][api-management-products-page]

> <span data-ttu-id="bf609-131">Som standard nya produkter är opublicerad och är synliga endast toohello **administratörer** grupp.</span><span class="sxs-lookup"><span data-stu-id="bf609-131">By default new products are unpublished, and are visible only toohello  **Administrators** group.</span></span>
> 
> 

<span data-ttu-id="bf609-132">tooconfigure en produkt, klicka på hello Produktnamn i hello **produkter** fliken.</span><span class="sxs-lookup"><span data-stu-id="bf609-132">tooconfigure a product, click on hello product name in hello **Products** tab.</span></span>

## <span data-ttu-id="bf609-133"><a name="add-apis"></a>Lägg till API: er tooa produkten</span><span class="sxs-lookup"><span data-stu-id="bf609-133"><a name="add-apis"> </a>Add APIs tooa product</span></span>
<span data-ttu-id="bf609-134">Hej **produkter** sidan innehåller fyra länkar för konfiguration: **sammanfattning**, **inställningar**, **synlighet**, och  **Prenumeranter**.</span><span class="sxs-lookup"><span data-stu-id="bf609-134">hello **Products** page contains four links for configuration: **Summary**, **Settings**, **Visibility**, and **Subscribers**.</span></span> <span data-ttu-id="bf609-135">Hej **sammanfattning** är där du kan lägga till API: er och publicera eller avpublicera en produkt.</span><span class="sxs-lookup"><span data-stu-id="bf609-135">hello **Summary** tab is where you can add APIs and publish or unpublish a product.</span></span>

![Sammanfattning][api-management-new-product-summary]

<span data-ttu-id="bf609-137">Innan du publicerar produkten måste tooadd en eller flera API: er.</span><span class="sxs-lookup"><span data-stu-id="bf609-137">Before publishing your product you need tooadd one or more APIs.</span></span> <span data-ttu-id="bf609-138">toodo, genom att välja **lägga till API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="bf609-138">toodo this, click **Add API tooproduct**.</span></span>

![Lägg till API: er][api-management-add-apis-to-product]

<span data-ttu-id="bf609-140">Välj hello önskad API: er och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bf609-140">Select hello desired APIs and click **Save**.</span></span>

## <span data-ttu-id="bf609-141"><a name="add-description"></a>Lägg till beskrivande information tooa produkten</span><span class="sxs-lookup"><span data-stu-id="bf609-141"><a name="add-description"> </a>Add descriptive information tooa product</span></span>
<span data-ttu-id="bf609-142">Hej **inställningar** fliken kan du tooprovide detaljerad information om hello produkten som sitt syfte, hello API: er som den ger åtkomst till och annan användbar information.</span><span class="sxs-lookup"><span data-stu-id="bf609-142">hello **Settings** tab allows you tooprovide detailed information about hello product such as its purpose, hello APIs it provides access to, and other useful information.</span></span> <span data-ttu-id="bf609-143">hello innehåll är riktad mot hello utvecklare som kommer att ringa upp hello API och kan skrivas i oformaterad text eller HTML-kod.</span><span class="sxs-lookup"><span data-stu-id="bf609-143">hello content is targeted at hello developers that will be calling hello API and can be written in plain text or HTML markup.</span></span>

![Produktinställningar för][api-management-product-settings]

<span data-ttu-id="bf609-145">Kontrollera **kräver prenumeration** toocreate en skyddad produkt som kräver en prenumeration toobe används, eller avmarkera hello kryssrutan toocreate en öppen produkt som kan anropas utan en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bf609-145">Check **Require subscription** toocreate a protected product that requires a subscription toobe used, or clear hello checkbox toocreate an open product that can be called without a subscription.</span></span>

<span data-ttu-id="bf609-146">Välj **kräver godkännande för prenumerationen** om du vill toomanually godkänna alla produkten prenumerationsbegäranden.</span><span class="sxs-lookup"><span data-stu-id="bf609-146">Select **Require subscription approval** if you want toomanually approve all product subscription requests.</span></span> <span data-ttu-id="bf609-147">Som standard beviljas alla produktprenumeration automatiskt.</span><span class="sxs-lookup"><span data-stu-id="bf609-147">By default all product subscriptions are granted automatically.</span></span>

<span data-ttu-id="bf609-148">tooallow developer konton toosubscribe flera gånger toohello produkt, kontrollera hello **tillåter flera prenumerationer** kryssrutan och om du vill ange en gräns.</span><span class="sxs-lookup"><span data-stu-id="bf609-148">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box and optionally specify a limit.</span></span> <span data-ttu-id="bf609-149">Om inte den här kryssrutan är markerad, kan varje utvecklarkonto prenumerera på bara en gång toohello produkt.</span><span class="sxs-lookup"><span data-stu-id="bf609-149">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

<span data-ttu-id="bf609-150">Om du vill fylla i hello **användningsvillkoren** fält som beskriver hello villkor för användning av hello produkt som prenumeranter måste acceptera i ordning toouse hello produkten.</span><span class="sxs-lookup"><span data-stu-id="bf609-150">Optionally fill in hello **Terms of use** field describing hello terms of use for hello product which subscribers must accept in order toouse hello product.</span></span>

## <span data-ttu-id="bf609-151"><a name="publish-product"></a>Publicera en produkt</span><span class="sxs-lookup"><span data-stu-id="bf609-151"><a name="publish-product"> </a>Publish a product</span></span>
<span data-ttu-id="bf609-152">Hello produkten måste publiceras innan hello API: er i en produkt kan anropas.</span><span class="sxs-lookup"><span data-stu-id="bf609-152">Before hello APIs in a product can be called, hello product must be published.</span></span> <span data-ttu-id="bf609-153">På hello **sammanfattning** för hello produkten klickar du på **publicera**, och klicka sedan på **Ja, publicera den** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="bf609-153">On hello **Summary** tab for hello product, click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span> <span data-ttu-id="bf609-154">toomake en tidigare publicerade produkten privat klickar du på **Upphäv publicering**.</span><span class="sxs-lookup"><span data-stu-id="bf609-154">toomake a previously published product private, click **Unpublish**.</span></span>

![Publicera produkten][api-management-publish-product]

## <span data-ttu-id="bf609-156"><a name="make-visible"></a>Gör en synlig toodevelopers produkten</span><span class="sxs-lookup"><span data-stu-id="bf609-156"><a name="make-visible"> </a>Make a product visible toodevelopers</span></span>
<span data-ttu-id="bf609-157">Hej **synlighet** fliken kan du toochoose vilka roller är kan toosee hello produkten på hello developer-portalen och prenumerera toohello produkten.</span><span class="sxs-lookup"><span data-stu-id="bf609-157">hello **Visibility** tab allows you toochoose which roles are able toosee hello product on hello developer portal and subscribe toohello product.</span></span>

![Produkten synlighet][api-management-product-visiblity]

<span data-ttu-id="bf609-159">tooenable eller inaktivera synligheten för en produkt för hello utvecklare i en grupp Markera eller avmarkera hello kryssrutan bredvid hello gruppen och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bf609-159">tooenable or disable visibility of a product for hello developers in a group, check or uncheck hello check box beside hello group and then click **Save**.</span></span>

> <span data-ttu-id="bf609-160">Mer information finns i [hur toocreate och Använd grupper toomanage developer konton i Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="bf609-160">For more information, see [How toocreate and use groups toomanage developer accounts in Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="bf609-161"><a name="view-subscribers"></a>Visa prenumeranter tooa produkt</span><span class="sxs-lookup"><span data-stu-id="bf609-161"><a name="view-subscribers"> </a>View subscribers tooa product</span></span>
<span data-ttu-id="bf609-162">Hej **prenumeranter** fliken listar hello utvecklare som prenumererar toohello produkten.</span><span class="sxs-lookup"><span data-stu-id="bf609-162">hello **Subscribers** tab lists hello developers who have subscribed toohello product.</span></span> <span data-ttu-id="bf609-163">hello kan information och inställningar för varje utvecklare visas genom att klicka på hello developer namn.</span><span class="sxs-lookup"><span data-stu-id="bf609-163">hello details and settings for each developer can be viewed by clicking on hello developer's name.</span></span> <span data-ttu-id="bf609-164">I det här exemplet har inga utvecklare ännu prenumererat toohello produkten.</span><span class="sxs-lookup"><span data-stu-id="bf609-164">In this example no developers have yet subscribed toohello product.</span></span>

![Utvecklare][api-management-developer-list]

## <span data-ttu-id="bf609-166"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf609-166"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="bf609-167">När önskade hello API: er och hello produkten publicerade utvecklare kan prenumerera toohello produkt och börja toocall hello API: er.</span><span class="sxs-lookup"><span data-stu-id="bf609-167">Once hello desired APIs are added and hello product published, developers can subscribe toohello product and begin toocall hello APIs.</span></span> <span data-ttu-id="bf609-168">En genomgång som visar dessa objekt samt avancerade produktkonfigurationen finns [hur skapar och konfigurerar produktinställningar för avancerad i Azure API Management][How create and configure advanced product settings in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="bf609-168">For a tutorial that demonstrates these items as well as advanced product configuration see [How create and configure advanced product settings in Azure API Management][How create and configure advanced product settings in Azure API Management].</span></span>

<span data-ttu-id="bf609-169">Mer information om hur du arbetar med produkter finns i följande video hello.</span><span class="sxs-lookup"><span data-stu-id="bf609-169">For more information about working with products, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
