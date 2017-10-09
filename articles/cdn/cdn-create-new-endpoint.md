---
title: "aaaGetting igång med Azure CDN | Microsoft Docs"
description: "Det här avsnittet visar hur tooenable hello Azure Content Delivery Network (CDN). hello kursen går igenom hello skapandet av en ny CDN-profil och en slutpunkt."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a><span data-ttu-id="23f6e-104">Komma igång med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="23f6e-104">Getting started with Azure CDN</span></span>
<span data-ttu-id="23f6e-105">Det här avsnittet beskriver steg för steg hur du aktiverar Azure CDN genom att skapa en ny profil och slutpunkt för CDN.</span><span class="sxs-lookup"><span data-stu-id="23f6e-105">This topic walks through enabling Azure CDN by creating a new CDN profile and endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23f6e-106">En introduktion toohow CDN fungerar, samt en lista över funktioner finns hello [CDN-översikt](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="23f6e-106">For an introduction toohow CDN works, as well as a list of features, see hello [CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="23f6e-107">Skapa en ny CDN-profil</span><span class="sxs-lookup"><span data-stu-id="23f6e-107">Create a new CDN profile</span></span>
<span data-ttu-id="23f6e-108">En CDN-profil är en samling CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="23f6e-108">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="23f6e-109">Varje profil innehåller en eller flera CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="23f6e-109">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="23f6e-110">Du kanske vill toouse flera profiler tooorganize CDN-slutpunkter med internet-domän, webbprogram eller andra kriterier.</span><span class="sxs-lookup"><span data-stu-id="23f6e-110">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="23f6e-111">En enda Azure-prenumeration är som standard begränsad tooeight CDN profiler.</span><span class="sxs-lookup"><span data-stu-id="23f6e-111">By default, a single Azure subscription is limited tooeight CDN profiles.</span></span> <span data-ttu-id="23f6e-112">Varje CDN-profilen är begränsad tooten CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="23f6e-112">Each CDN profile is limited tooten CDN endpoints.</span></span>
> 
> <span data-ttu-id="23f6e-113">CDN priser tillämpas på hello CDN-profilen nivå.</span><span class="sxs-lookup"><span data-stu-id="23f6e-113">CDN pricing is applied at hello CDN profile level.</span></span> <span data-ttu-id="23f6e-114">Om du vill toouse en blandning av Azure CDN prisnivåer behöver flera CDN-profiler.</span><span class="sxs-lookup"><span data-stu-id="23f6e-114">If you wish toouse a mix of Azure CDN pricing tiers, you will need multiple CDN profiles.</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="23f6e-115">Skapa en ny CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="23f6e-115">Create a new CDN endpoint</span></span>
<span data-ttu-id="23f6e-116">**toocreate en ny CDN-slutpunkt**</span><span class="sxs-lookup"><span data-stu-id="23f6e-116">**toocreate a new CDN endpoint**</span></span>

1. <span data-ttu-id="23f6e-117">I hello [Azure Portal](https://portal.azure.com), navigera tooyour CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="23f6e-117">In hello [Azure Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="23f6e-118">Du kanske har Fäst det toohello instrumentpanelen i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="23f6e-118">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="23f6e-119">Om du inte hittar du den genom att klicka på **Bläddra**, sedan **CDN profiler**, och klicka på hello profilen du planerar tooadd till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="23f6e-119">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="23f6e-120">hello CDN-profilbladet visas.</span><span class="sxs-lookup"><span data-stu-id="23f6e-120">hello CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="23f6e-122">Klicka på hello **Lägg till slutpunkt** knappen.</span><span class="sxs-lookup"><span data-stu-id="23f6e-122">Click hello **Add Endpoint** button.</span></span>
   
    ![Knappen Lägg till slutpunkt][cdn-new-endpoint-button]
   
    <span data-ttu-id="23f6e-124">Hej **lägga till en slutpunkt** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="23f6e-124">hello **Add an endpoint** blade appears.</span></span>
   
    ![Bladet Lägg till slutpunkt][cdn-add-endpoint]
3. <span data-ttu-id="23f6e-126">Ange ett **namn** för den här CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="23f6e-126">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="23f6e-127">Det här namnet kommer att använda tooaccess cachelagrade resurserna på hello domän `<endpointname>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="23f6e-127">This name will be used tooaccess your cached resources at hello domain `<endpointname>.azureedge.net`.</span></span>
4. <span data-ttu-id="23f6e-128">I hello **typ av ursprung** listrutan väljer du typen av ursprung.</span><span class="sxs-lookup"><span data-stu-id="23f6e-128">In hello **Origin type** dropdown, select your origin type.</span></span>  <span data-ttu-id="23f6e-129">Välj **Storage** för ett Azure Storage-konto, **Cloud Services** för ett Azure Cloud Services-konto, **Web Apps** för ett Azure Web Apps-konto eller **Anpassat ursprung** för ett annat ursprung i form av en offentligt tillgänglig webbserver (som finns i Azure eller någon annanstans).</span><span class="sxs-lookup"><span data-stu-id="23f6e-129">Select **Storage** for an Azure Storage account, **Cloud service** for an Azure Cloud Service, **Web App** for an Azure Web App, or **Custom origin** for any other publicly accessible web server origin (hosted in Azure or elsewhere).</span></span>
   
    ![Ursprungstyp för CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. <span data-ttu-id="23f6e-131">I hello **ursprungsvärdnamn** listrutan, Välj eller Skriv din ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="23f6e-131">In hello **Origin hostname** dropdown, select or type your origin domain.</span></span>  <span data-ttu-id="23f6e-132">hello listrutan visar en lista över alla tillgängliga ursprung av hello-typ som du angav i steg 4.</span><span class="sxs-lookup"><span data-stu-id="23f6e-132">hello dropdown will list all available origins of hello type you specified in step 4.</span></span>  <span data-ttu-id="23f6e-133">Om du har valt *anpassad ursprung* som din **typ av ursprung**, du kommer att ange hello ursprungsdomän din anpassade.</span><span class="sxs-lookup"><span data-stu-id="23f6e-133">If you selected *Custom origin* as your **Origin type**, you will type in hello domain of your custom origin.</span></span>
6. <span data-ttu-id="23f6e-134">I hello **ursprungssökväg** text Ange hello sökvägen toohello resurser du vill att toocache eller lämna tomt tooallow cache någon resurs på hello domän du angav i steg 5.</span><span class="sxs-lookup"><span data-stu-id="23f6e-134">In hello **Origin path** text box, enter hello path toohello resources you want toocache, or leave blank tooallow cache any resource at hello domain you specified in step 5.</span></span>
7. <span data-ttu-id="23f6e-135">I hello **ursprungsvärdadress**anger hello värdadressen önskade hello CDN toosend med varje begäran, eller lämna hello standard.</span><span class="sxs-lookup"><span data-stu-id="23f6e-135">In hello **Origin host header**, enter hello host header you want hello CDN toosend with each request, or leave hello default.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="23f6e-136">Vissa typer av ursprung, till exempel Azure Storage och Web Apps kräver hello värden huvud toomatch hello ursprungsdomän hello.</span><span class="sxs-lookup"><span data-stu-id="23f6e-136">Some types of origins, such as Azure Storage and Web Apps, require hello host header toomatch hello domain of hello origin.</span></span> <span data-ttu-id="23f6e-137">Om du inte har ett ursprung som kräver ett värdhuvud som skiljer sig från sin domän, bör du lämna hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="23f6e-137">Unless you have an origin that requires a host header different from its domain, you should leave hello default value.</span></span>
   > 
   > 
8. <span data-ttu-id="23f6e-138">För **protokollet** och **ursprung port**anger hello protokoll och portar används tooaccess dina resurser vid hello ursprung.</span><span class="sxs-lookup"><span data-stu-id="23f6e-138">For **Protocol** and **Origin port**, specify hello protocols and ports used tooaccess your resources at hello origin.</span></span>  <span data-ttu-id="23f6e-139">Du måste välja minst ett protokoll (HTTP eller HTTPS).</span><span class="sxs-lookup"><span data-stu-id="23f6e-139">At least one protocol (HTTP or HTTPS) must be selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23f6e-140">Hej **ursprung port** påverkar endast vilken port hello slutpunkten använder tooretrieve information från hello ursprung.</span><span class="sxs-lookup"><span data-stu-id="23f6e-140">hello **Origin port** only affects what port hello endpoint uses tooretrieve information from hello origin.</span></span>  <span data-ttu-id="23f6e-141">hello endpoint själva kommer bara att tillgängliga tooend klienter på hello standard HTTP och HTTPS-portar (80 och 443), oavsett hello **ursprung port**.</span><span class="sxs-lookup"><span data-stu-id="23f6e-141">hello endpoint itself will only be available tooend clients on hello default HTTP and HTTPS ports (80 and 443), regardless of hello **Origin port**.</span></span>  
   > 
   > <span data-ttu-id="23f6e-142">**Azure CDN från Akamai** slutpunkter inte tillåter hello fullständig TCP-portintervallet för ursprung.</span><span class="sxs-lookup"><span data-stu-id="23f6e-142">**Azure CDN from Akamai** endpoints do not allow hello full TCP port range for origins.</span></span>  <span data-ttu-id="23f6e-143">En lista över ursprungsportar som inte tillåts finns i [Azure CDN från Akamai-tillåtna ursprungsportar](https://msdn.microsoft.com/library/mt757337.aspx).</span><span class="sxs-lookup"><span data-stu-id="23f6e-143">For a list of origin ports that are not allowed, see [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx).</span></span>  
   > 
   > <span data-ttu-id="23f6e-144">Åtkomst till CDN har innehåll med hjälp av HTTPS hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="23f6e-144">Accessing CDN content using HTTPS has hello following constraints:</span></span>
   > 
   > * <span data-ttu-id="23f6e-145">Du måste använda hello SSL-certifikat som tillhandahålls av hello CDN.</span><span class="sxs-lookup"><span data-stu-id="23f6e-145">You must use hello SSL certificate provided by hello CDN.</span></span> <span data-ttu-id="23f6e-146">Certifikat från tredje part stöds inte.</span><span class="sxs-lookup"><span data-stu-id="23f6e-146">Third party certificates are not supported.</span></span>
   > * <span data-ttu-id="23f6e-147">Du måste använda hello angivna CDN domän (`<endpointname>.azureedge.net`) tooaccess HTTPS-innehåll.</span><span class="sxs-lookup"><span data-stu-id="23f6e-147">You must use hello CDN-provided domain (`<endpointname>.azureedge.net`) tooaccess HTTPS content.</span></span> <span data-ttu-id="23f6e-148">HTTPS-stöd är inte tillgängligt för anpassade domännamn (skapa CNAME-poster) eftersom hello CDN inte stöder anpassade certifikat just nu.</span><span class="sxs-lookup"><span data-stu-id="23f6e-148">HTTPS support is not available for custom domain names (CNAMEs) since hello CDN does not support custom certificates at this time.</span></span>
   > 
   > 
9. <span data-ttu-id="23f6e-149">Klicka på hello **Lägg till** knappen toocreate hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="23f6e-149">Click hello **Add** button toocreate hello new endpoint.</span></span>
10. <span data-ttu-id="23f6e-150">När hello slutpunkten har skapats visas den i en lista över slutpunkter för hello-profilen.</span><span class="sxs-lookup"><span data-stu-id="23f6e-150">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="23f6e-151">hello listvyn visar hello URL toouse tooaccess cachelagrat innehåll, samt hello ursprungsdomän.</span><span class="sxs-lookup"><span data-stu-id="23f6e-151">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
    
    ![CDN-slutpunkt][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > <span data-ttu-id="23f6e-153">hello endpoint blir omedelbart inte tillgängliga för användning, som det tar tid för hello registrering toopropagate via hello CDN.</span><span class="sxs-lookup"><span data-stu-id="23f6e-153">hello endpoint will not immediately be available for use, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="23f6e-154">För profiler av typen <b>Azure CDN från Akamai</b> slutförs spridningen vanligtvis inom en minut.</span><span class="sxs-lookup"><span data-stu-id="23f6e-154">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="23f6e-155">För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.</span><span class="sxs-lookup"><span data-stu-id="23f6e-155">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
    > 
    > <span data-ttu-id="23f6e-156">Användare som försöker toouse hello CDN domännamn innan hello slutpunktskonfiguration har spridits toohello POP får HTTP 404 svarskoder.</span><span class="sxs-lookup"><span data-stu-id="23f6e-156">Users who try toouse hello CDN domain name before hello endpoint configuration has propagated toohello POPs will receive HTTP 404 response codes.</span></span>  <span data-ttu-id="23f6e-157">Om det har gått flera timmar sedan du skapade slutpunkten och du fortfarande får 404-svar läser du [Felsöka CDN-slutpunkter som returnerar 404-status](cdn-troubleshoot-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="23f6e-157">If it's been several hours since you created your endpoint and you're still receiving 404 responses, please see [Troubleshooting CDN endpoints returning 404 statuses](cdn-troubleshoot-endpoint.md).</span></span>
    > 
    > 

## <a name="see-also"></a><span data-ttu-id="23f6e-158">Se även</span><span class="sxs-lookup"><span data-stu-id="23f6e-158">See Also</span></span>
* [<span data-ttu-id="23f6e-159">Kontrollera cachelagringsbeteendet för begäranden med frågesträngar</span><span class="sxs-lookup"><span data-stu-id="23f6e-159">Controlling caching behavior of requests with query strings</span></span>](cdn-query-string.md)
* [<span data-ttu-id="23f6e-160">Hur tooMap innehåll till CDN tooa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="23f6e-160">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="23f6e-161">Läsa in tillgångar för en Azure CDN-slutpunkt i förväg</span><span class="sxs-lookup"><span data-stu-id="23f6e-161">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="23f6e-162">Rensa en Azure CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="23f6e-162">Purge an Azure CDN Endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="23f6e-163">Felsöka CDN-slutpunkter som returnerar 404-status</span><span class="sxs-lookup"><span data-stu-id="23f6e-163">Troubleshooting CDN endpoints returning 404 statuses</span></span>](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
