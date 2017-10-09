---
title: aaaAdd CDN-tooan Azure App Service | Microsoft Docs
description: "Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service toocache och leverera de statiska filerna från servrar Stäng tooyour kunder runt hälsningsmeddelande."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a><span data-ttu-id="532c1-103">Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service</span><span class="sxs-lookup"><span data-stu-id="532c1-103">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>

<span data-ttu-id="532c1-104">[Azure innehåll innehållsleveransnätverk (CDN)](../cdn/cdn-overview.md) cachelagrar statiska webbinnehåll på strategiskt monterade platser tooprovide maximalt dataflöde för att leverera innehåll toousers.</span><span class="sxs-lookup"><span data-stu-id="532c1-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) caches static web content at strategically placed locations tooprovide maximum throughput for delivering content toousers.</span></span> <span data-ttu-id="532c1-105">hello CDN minskar också belastningen på servern på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="532c1-105">hello CDN also decreases server load on your web app.</span></span> <span data-ttu-id="532c1-106">Den här kursen visar hur tooadd Azure CDN tooa [webbapp i Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="532c1-106">This tutorial shows how tooadd Azure CDN tooa [web app in Azure App Service](app-service-web-overview.md).</span></span> 

<span data-ttu-id="532c1-107">Här är hello startsidan för hello exempel statiska HTML-webbplats som du ska arbeta med:</span><span class="sxs-lookup"><span data-stu-id="532c1-107">Here's hello home page of hello sample static HTML site that you'll work with:</span></span>

![Exempelstartsida för app](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

<span data-ttu-id="532c1-109">Vad du lära dig:</span><span class="sxs-lookup"><span data-stu-id="532c1-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="532c1-110">Skapa en CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="532c1-110">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="532c1-111">Uppdatera cachelagrade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="532c1-111">Refresh cached assets.</span></span>
> * <span data-ttu-id="532c1-112">Använd fråga strängar toocontrol cachelagras versioner.</span><span class="sxs-lookup"><span data-stu-id="532c1-112">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="532c1-113">Använda en anpassad domän för hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="532c1-113">Use a custom domain for hello CDN endpoint.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="532c1-114">Krav</span><span class="sxs-lookup"><span data-stu-id="532c1-114">Prerequisites</span></span>

<span data-ttu-id="532c1-115">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="532c1-115">toocomplete this tutorial:</span></span>

- [<span data-ttu-id="532c1-116">Installera Git</span><span class="sxs-lookup"><span data-stu-id="532c1-116">Install Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="532c1-117">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="532c1-117">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a><span data-ttu-id="532c1-118">Skapa hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="532c1-118">Create hello web app</span></span>

<span data-ttu-id="532c1-119">toocreate hello webbprogram som du ska arbeta med, följ hello [statiska HTML-Snabbstart](app-service-web-get-started-html.md) via hello **Bläddra toohello app** steg.</span><span class="sxs-lookup"><span data-stu-id="532c1-119">toocreate hello web app that you'll work with, follow hello [static HTML quickstart](app-service-web-get-started-html.md) through hello **Browse toohello app** step.</span></span>

### <a name="have-a-custom-domain-ready"></a><span data-ttu-id="532c1-120">Ha ett anpassat domän redo</span><span class="sxs-lookup"><span data-stu-id="532c1-120">Have a custom domain ready</span></span>

<span data-ttu-id="532c1-121">toocomplete hello domänen steg i den här kursen behöver tooown en anpassad domän och har åtkomst tooyour DNS-registret för domänleverantören (till exempel GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="532c1-121">toocomplete hello custom domain step of this tutorial, you need tooown a custom domain and have access tooyour DNS registry for your domain provider (such as GoDaddy).</span></span> <span data-ttu-id="532c1-122">Till exempel tooadd DNS-poster för `contoso.com` och `www.contoso.com`, måste du ha åtkomst tooconfigure hello DNS-inställningarna för hello `contoso.com` rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="532c1-122">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must have access tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

<span data-ttu-id="532c1-123">Om du inte redan har ett domännamn kan du överväga följande hello [Apptjänst domän kursen](custom-dns-web-site-buydomains-web-app.md) toopurchase en domän med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="532c1-123">If you don't already have a domain name, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="532c1-124">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="532c1-124">Log in toohello Azure portal</span></span>

<span data-ttu-id="532c1-125">Öppna en webbläsare och gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="532c1-125">Open a browser and navigate toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-a-cdn-profile-and-endpoint"></a><span data-ttu-id="532c1-126">Skapa en CDN-profil och en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="532c1-126">Create a CDN profile and endpoint</span></span>

<span data-ttu-id="532c1-127">I vänster navigeringsfält hello, väljer **Apptjänster**, och välj sedan hello-app som du skapade i hello [statiska HTML-Snabbstart](app-service-web-get-started-html.md).</span><span class="sxs-lookup"><span data-stu-id="532c1-127">In hello left navigation, select **App Services**, and then select hello app that you created in hello [static HTML quickstart](app-service-web-get-started-html.md).</span></span>

![Välj App Service-appen i hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

<span data-ttu-id="532c1-129">I hello **Apptjänst** i hello sidan **inställningar** väljer **nätverk > Konfigurera Azure CDN för din app**.</span><span class="sxs-lookup"><span data-stu-id="532c1-129">In hello **App Service** page, in hello **Settings** section, select **Networking > Configure Azure CDN for your app**.</span></span>

![Välj CDN hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

<span data-ttu-id="532c1-131">I hello **Azure Content Delivery Network** anger hello **ny slutpunkt** inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="532c1-131">In hello **Azure Content Delivery Network** page, provide hello **New endpoint** settings as specified in hello table.</span></span>

![Skapa profil och slutpunkt i hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| <span data-ttu-id="532c1-133">Inställning</span><span class="sxs-lookup"><span data-stu-id="532c1-133">Setting</span></span> | <span data-ttu-id="532c1-134">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="532c1-134">Suggested value</span></span> | <span data-ttu-id="532c1-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="532c1-135">Description</span></span> |
| ------- | --------------- | ----------- |
| <span data-ttu-id="532c1-136">**CDN-profil**</span><span class="sxs-lookup"><span data-stu-id="532c1-136">**CDN profile**</span></span> | <span data-ttu-id="532c1-137">myCDNProfile</span><span class="sxs-lookup"><span data-stu-id="532c1-137">myCDNProfile</span></span> | <span data-ttu-id="532c1-138">Välj **Skapa nytt** toocreate CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="532c1-138">Select **Create new** toocreate a CDN profile.</span></span> <span data-ttu-id="532c1-139">En CDN-profil är en samling av CDN-slutpunkter med hello samma prisnivån.</span><span class="sxs-lookup"><span data-stu-id="532c1-139">A CDN profile is a collection of CDN endpoints with hello same pricing tier.</span></span> |
| <span data-ttu-id="532c1-140">**prisnivå**</span><span class="sxs-lookup"><span data-stu-id="532c1-140">**Pricing tier**</span></span> | <span data-ttu-id="532c1-141">Standard Akamai</span><span class="sxs-lookup"><span data-stu-id="532c1-141">Standard Akamai</span></span> | <span data-ttu-id="532c1-142">Hej [prisnivån](../cdn/cdn-overview.md#azure-cdn-features) anger hello-providern och funktioner som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="532c1-142">hello [pricing tier](../cdn/cdn-overview.md#azure-cdn-features) specifies hello provider and available features.</span></span> <span data-ttu-id="532c1-143">I den här kursen använder du Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="532c1-143">In this tutorial, we are using Standard Akamai.</span></span> |
| <span data-ttu-id="532c1-144">**CDN-slutpunktsnamn**</span><span class="sxs-lookup"><span data-stu-id="532c1-144">**CDN endpoint name**</span></span> | <span data-ttu-id="532c1-145">Alla namn som är unikt i hello azureedge.net domän</span><span class="sxs-lookup"><span data-stu-id="532c1-145">Any name that is unique in hello azureedge.net domain</span></span> | <span data-ttu-id="532c1-146">Du har åtkomst till dina cachelagrade resurser på hello domän  *\<endpointname >. azureedge.net*.</span><span class="sxs-lookup"><span data-stu-id="532c1-146">You access your cached resources at hello domain *\<endpointname>.azureedge.net*.</span></span>

<span data-ttu-id="532c1-147">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="532c1-147">Select **Create**.</span></span>

<span data-ttu-id="532c1-148">Azure skapar hello profil och slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="532c1-148">Azure creates hello profile and endpoint.</span></span> <span data-ttu-id="532c1-149">hello ny slutpunkt visas i hello **slutpunkter** listan hello samma sida och när den har etablerats hello status är **kör**.</span><span class="sxs-lookup"><span data-stu-id="532c1-149">hello new endpoint appears in hello **Endpoints** list on hello same page, and when it's provisioned hello status is **Running**.</span></span>

![Ny slutpunkt i listan](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="532c1-151">Testa hello CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="532c1-151">Test hello CDN endpoint</span></span>

<span data-ttu-id="532c1-152">Om du har valt Verizon-prisnivån tar det vanligtvis cirka 90 minuter för slutpunktsspridning.</span><span class="sxs-lookup"><span data-stu-id="532c1-152">If you selected Verizon pricing tier, it typically takes about 90 minutes for endpoint propagation.</span></span> <span data-ttu-id="532c1-153">Det tar några minuter för spridning för Akamai</span><span class="sxs-lookup"><span data-stu-id="532c1-153">For Akamai, it takes a couple minutes for propagation</span></span>

<span data-ttu-id="532c1-154">hello sample-appen har en `index.html` fil och *css*, *img*, och *js* mappar som innehåller andra statiska tillgångar.</span><span class="sxs-lookup"><span data-stu-id="532c1-154">hello sample app has an `index.html` file and *css*, *img*, and *js* folders that contain other static assets.</span></span> <span data-ttu-id="532c1-155">hello innehåll sökvägar för alla dessa filer är hello samma på hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="532c1-155">hello content paths for all of these files are hello same at hello CDN endpoint.</span></span> <span data-ttu-id="532c1-156">Till exempel åtkomst både hello följande URL: er hello *bootstrap.css* filen i hello *css* mapp:</span><span class="sxs-lookup"><span data-stu-id="532c1-156">For example, both of hello following URLs access hello *bootstrap.css* file in hello *css* folder:</span></span>

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

<span data-ttu-id="532c1-157">Gå en webbläsare toohello följande URL:</span><span class="sxs-lookup"><span data-stu-id="532c1-157">Navigate a browser toohello following URL:</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![Exempelstartsida för app som hämtats från CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 <span data-ttu-id="532c1-159">Du kan se hello samma sidan du körde tidigare i en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="532c1-159">You see hello same page that you ran earlier in an Azure web app.</span></span> <span data-ttu-id="532c1-160">Azure CDN har hämtats hello ursprung webbprogram tillgångar och används av dem från hello CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="532c1-160">Azure CDN has retrieved hello origin web app's assets and is serving them from hello CDN endpoint</span></span>

<span data-ttu-id="532c1-161">tooensure som den här sidan sparas i cacheminnet i hello CDN uppdatera hello sidan.</span><span class="sxs-lookup"><span data-stu-id="532c1-161">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> <span data-ttu-id="532c1-162">Två begäranden för hello samma tillgång krävs ibland för hello CDN toocache hello begärda innehållet.</span><span class="sxs-lookup"><span data-stu-id="532c1-162">Two requests for hello same asset are sometimes required for hello CDN toocache hello requested content.</span></span>

<span data-ttu-id="532c1-163">Mer information om hur du skapar Azure CDN-profiler och slutpunkter finns i [Komma igång med Azure CDN](../cdn/cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="532c1-163">For more information about creating Azure CDN profiles and endpoints, see [Getting started with Azure CDN](../cdn/cdn-create-new-endpoint.md).</span></span>

## <a name="purge-hello-cdn"></a><span data-ttu-id="532c1-164">Rensa hello CDN</span><span class="sxs-lookup"><span data-stu-id="532c1-164">Purge hello CDN</span></span>

<span data-ttu-id="532c1-165">hello CDN uppdaterar regelbundet hur dess resurser från hello ursprung webbapp baserat på hello time to live (TTL) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="532c1-165">hello CDN periodically refreshes its resources from hello origin web app based on hello time-to-live (TTL) configuration.</span></span> <span data-ttu-id="532c1-166">hello standard TTL-värdet är sju dagar.</span><span class="sxs-lookup"><span data-stu-id="532c1-166">hello default TTL is seven days.</span></span>

<span data-ttu-id="532c1-167">Ibland kanske du måste toorefresh hello CDN innan hello TTL förfallodatum – exempelvis när du distribuerar uppdaterade innehållet toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="532c1-167">At times you might need toorefresh hello CDN before hello TTL expiration -- for example, when you deploy updated content toohello web app.</span></span> <span data-ttu-id="532c1-168">tootrigger en uppdatering kan du manuellt Rensa hello CDN resurser.</span><span class="sxs-lookup"><span data-stu-id="532c1-168">tootrigger a refresh, you can manually purge hello CDN resources.</span></span> 

<span data-ttu-id="532c1-169">I det här avsnittet av kursen hello distribuera en ändring toohello webbapp och rensa hello CDN tootrigger hello CDN toorefresh sin cache.</span><span class="sxs-lookup"><span data-stu-id="532c1-169">In this section of hello tutorial, you deploy a change toohello web app and purge hello CDN tootrigger hello CDN toorefresh its cache.</span></span>

### <a name="deploy-a-change-toohello-web-app"></a><span data-ttu-id="532c1-170">Distribuera en ändring toohello webbapp</span><span class="sxs-lookup"><span data-stu-id="532c1-170">Deploy a change toohello web app</span></span>

<span data-ttu-id="532c1-171">Öppna hello `index.html` och Lägg till ”-V2” toohello H1 rubriken som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="532c1-171">Open hello `index.html` file and add "- V2" toohello H1 heading, as shown in hello following example:</span></span> 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

<span data-ttu-id="532c1-172">Genomför ändringarna och distribuerar den toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="532c1-172">Commit your change and deploy it toohello web app.</span></span>

```bash
git commit -am "version 2"
git push azure master
```

<span data-ttu-id="532c1-173">När distributionen är klar finns i Bläddra toohello web app-URL och du hello ändra.</span><span class="sxs-lookup"><span data-stu-id="532c1-173">Once deployment has completed, browse toohello web app URL and you see hello change.</span></span>

```
http://<appname>.azurewebsites.net/index.html
```

![V2 i rubriken i webbappen](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

<span data-ttu-id="532c1-175">Bläddra toohello CDN-slutpunkten URL: en för hello startsidan och du inte ser hello ändra eftersom hello cachelagrade versionen i hello CDN inte har upphört att gälla ännu.</span><span class="sxs-lookup"><span data-stu-id="532c1-175">Browse toohello CDN endpoint URL for hello home page and you don't see hello change because hello cached version in hello CDN hasn't expired yet.</span></span> 

```
http://<endpointname>.azureedge.net/index.html
```

![Ingen V2 i rubriken i CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a><span data-ttu-id="532c1-177">Rensa hello CDN hello-portalen</span><span class="sxs-lookup"><span data-stu-id="532c1-177">Purge hello CDN in hello portal</span></span>

<span data-ttu-id="532c1-178">tootrigger Hej CDN tooupdate den cachelagra versionen, rensa hello CDN.</span><span class="sxs-lookup"><span data-stu-id="532c1-178">tootrigger hello CDN tooupdate its cached version, purge hello CDN.</span></span>

<span data-ttu-id="532c1-179">I hello portalens vänstra navigeringsfönstret, Välj **resursgrupper**, och välj sedan hello resursgrupp som du har skapat för ditt webbprogram (myResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="532c1-179">In hello portal left navigation, select **Resource groups**, and then select hello resource group that you created for your web app (myResourceGroup).</span></span>

![Välj resursgrupp](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

<span data-ttu-id="532c1-181">Välj CDN-slutpunkten i hello lista av resurser.</span><span class="sxs-lookup"><span data-stu-id="532c1-181">In hello list of resources, select your CDN endpoint.</span></span>

![Välj slutpunkt](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

<span data-ttu-id="532c1-183">Hello överst i hello **Endpoint** klickar du på **Rensa**.</span><span class="sxs-lookup"><span data-stu-id="532c1-183">At hello top of hello **Endpoint** page, click **Purge**.</span></span>

![Välj Rensa](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

<span data-ttu-id="532c1-185">Ange hello sökvägar till innehåll du vill toopurge.</span><span class="sxs-lookup"><span data-stu-id="532c1-185">Enter hello content paths you wish toopurge.</span></span> <span data-ttu-id="532c1-186">Du kan skicka en fullständig sökväg toopurge en enskild fil eller en sökväg segment toopurge och uppdatera allt innehåll i en mapp.</span><span class="sxs-lookup"><span data-stu-id="532c1-186">You can pass a complete file path toopurge an individual file, or a path segment toopurge and refresh all content in a folder.</span></span> <span data-ttu-id="532c1-187">Eftersom du har ändrat `index.html`, se till att den en hello sökvägar.</span><span class="sxs-lookup"><span data-stu-id="532c1-187">Since you changed `index.html`, make sure that is one of hello paths.</span></span>

<span data-ttu-id="532c1-188">Längst ned hello hello-sidan, Välj **Rensa**.</span><span class="sxs-lookup"><span data-stu-id="532c1-188">At hello bottom of hello page, select **Purge**.</span></span>

![Rensa sida](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a><span data-ttu-id="532c1-190">Kontrollera att hello CDN uppdateras</span><span class="sxs-lookup"><span data-stu-id="532c1-190">Verify that hello CDN is updated</span></span>

<span data-ttu-id="532c1-191">Vänta tills hello begäran om rensning har bearbetat vanligtvis några minuter.</span><span class="sxs-lookup"><span data-stu-id="532c1-191">Wait until hello purge request finishes processing, typically a couple of minutes.</span></span> <span data-ttu-id="532c1-192">toosee hello aktuell status, Välj hello klockikonen överst hello på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="532c1-192">toosee hello current status, select hello bell icon at hello top of hello page.</span></span> 

![Rensningsavisering](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

<span data-ttu-id="532c1-194">Bläddra toohello CDN slutpunkts-URL för `index.html`, och nu visas hello V2 som du lagt till toohello rubrik på hello startsida.</span><span class="sxs-lookup"><span data-stu-id="532c1-194">Browse toohello CDN endpoint URL for `index.html`, and now you see hello V2 that you added toohello title on hello home page.</span></span> <span data-ttu-id="532c1-195">Detta visar hello CDN cachen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="532c1-195">This shows that hello CDN cache has been refreshed.</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![V2 i rubriken i CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

<span data-ttu-id="532c1-197">Mer information finns i [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md) (Rensa en Azure CDN-slutpunkt).</span><span class="sxs-lookup"><span data-stu-id="532c1-197">For more information, see [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md).</span></span> 

## <a name="use-query-strings-tooversion-content"></a><span data-ttu-id="532c1-198">Använda frågan strängar tooversion innehåll</span><span class="sxs-lookup"><span data-stu-id="532c1-198">Use query strings tooversion content</span></span>

<span data-ttu-id="532c1-199">hello Azure CDN finns följande Beteendealternativ för cachelagring hello:</span><span class="sxs-lookup"><span data-stu-id="532c1-199">hello Azure CDN offers hello following caching behavior options:</span></span>

* <span data-ttu-id="532c1-200">Ignorera frågesträngar</span><span class="sxs-lookup"><span data-stu-id="532c1-200">Ignore query strings</span></span>
* <span data-ttu-id="532c1-201">Kringgå cachelagring för frågesträngar</span><span class="sxs-lookup"><span data-stu-id="532c1-201">Bypass caching for query strings</span></span>
* <span data-ttu-id="532c1-202">Cachelagra varje unik URL</span><span class="sxs-lookup"><span data-stu-id="532c1-202">Cache every unique URL</span></span> 

<span data-ttu-id="532c1-203">Hej först är hello standard, vilket innebär att endast en cachelagrad version av en tillgång oavsett hello frågesträng i hello URL: en av dessa.</span><span class="sxs-lookup"><span data-stu-id="532c1-203">hello first of these is hello default, which means there is only one cached version of an asset regardless of hello query string in hello URL.</span></span> 

<span data-ttu-id="532c1-204">I det här avsnittet av kursen hello ändra hello cachelagring beteende toocache varje unik URL.</span><span class="sxs-lookup"><span data-stu-id="532c1-204">In this section of hello tutorial, you change hello caching behavior toocache every unique URL.</span></span>

### <a name="change-hello-cache-behavior"></a><span data-ttu-id="532c1-205">Ändra hello cachebeteende</span><span class="sxs-lookup"><span data-stu-id="532c1-205">Change hello cache behavior</span></span>

<span data-ttu-id="532c1-206">I hello Azure-portalen **CDN-slutpunkten** väljer **Cache**.</span><span class="sxs-lookup"><span data-stu-id="532c1-206">In hello Azure portal **CDN Endpoint** page, select **Cache**.</span></span>

<span data-ttu-id="532c1-207">Välj **cachelagra varje unik URL** från hello **cachelagring av frågesträngar** listrutan.</span><span class="sxs-lookup"><span data-stu-id="532c1-207">Select **Cache every unique URL** from hello **Query string caching behavior** drop-down list.</span></span>

<span data-ttu-id="532c1-208">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="532c1-208">Select **Save**.</span></span>

![Välj beteende för cachelagring av frågesträngar](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a><span data-ttu-id="532c1-210">Kontrollera att unika URL:er cachelagras separat</span><span class="sxs-lookup"><span data-stu-id="532c1-210">Verify that unique URLs are cached separately</span></span>

<span data-ttu-id="532c1-211">Navigera toohello startsidan på hello CDN-slutpunkten i en webbläsare, men innehåller en frågesträng:</span><span class="sxs-lookup"><span data-stu-id="532c1-211">In a browser, navigate toohello home page at hello CDN endpoint, but include a query string:</span></span> 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

<span data-ttu-id="532c1-212">hello CDN returnerar hello aktuella app webbinnehåll, som innehåller ”V2” i hello rubrik.</span><span class="sxs-lookup"><span data-stu-id="532c1-212">hello CDN returns hello current web app content, which includes "V2" in hello heading.</span></span> 

<span data-ttu-id="532c1-213">tooensure som den här sidan sparas i cacheminnet i hello CDN uppdatera hello sidan.</span><span class="sxs-lookup"><span data-stu-id="532c1-213">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> 

<span data-ttu-id="532c1-214">Öppna `index.html` och ändra ”V2” för ”V3”, och distribuera hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="532c1-214">Open `index.html` and change "V2" too"V3", and deploy hello change.</span></span> 

```bash
git commit -am "version 3"
git push azure master
```

<span data-ttu-id="532c1-215">I en webbläsare går toohello CDN slutpunkts-URL med en ny frågesträng som `q=2`.</span><span class="sxs-lookup"><span data-stu-id="532c1-215">In a browser, go toohello CDN endpoint URL with a new query string such as `q=2`.</span></span> <span data-ttu-id="532c1-216">hello CDN hämtar hello aktuella `index.html` filen och visar ”V3”.</span><span class="sxs-lookup"><span data-stu-id="532c1-216">hello CDN gets hello current `index.html` file and displays "V3".</span></span>  <span data-ttu-id="532c1-217">Men om du navigerar toohello CDN-slutpunkten med hello `q=1` frågesträng, visas ”V2”.</span><span class="sxs-lookup"><span data-stu-id="532c1-217">But if you navigate toohello CDN endpoint with hello `q=1` query string, you see "V2".</span></span>

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 i rubriken i CDN, frågesträng 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 i rubriken i CDN, frågesträng 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

<span data-ttu-id="532c1-220">Den här visas varje frågesträngen behandlas på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="532c1-220">This output shows that each query string is treated differently:</span></span>

* <span data-ttu-id="532c1-221">q = 1 användes innan, så cachelagrat innehåll returneras (V2).</span><span class="sxs-lookup"><span data-stu-id="532c1-221">q=1 was used before, so cached contents are returned (V2).</span></span>
* <span data-ttu-id="532c1-222">q = 2 är nytt, så hello senaste web app innehållet hämtas och returnerade (V3).</span><span class="sxs-lookup"><span data-stu-id="532c1-222">q=2 is new, so hello latest web app contents are retrieved and returned (V3).</span></span>

<span data-ttu-id="532c1-223">Mer information finns i [Kontrollera cachelagringsbeteendet med frågesträngar](../cdn/cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="532c1-223">For more information, see [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md).</span></span>

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a><span data-ttu-id="532c1-224">Mappa en anpassad domän tooa CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="532c1-224">Map a custom domain tooa CDN endpoint</span></span>

<span data-ttu-id="532c1-225">Du måste mappa dina anpassade domäner tooyour CDN-slutpunkten genom att skapa en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="532c1-225">You'll map your custom domain tooyour CDN Endpoint by creating a CNAME record.</span></span> <span data-ttu-id="532c1-226">En CNAME-post är en DNS-funktion som mappar en domän tooa mål källdomänen.</span><span class="sxs-lookup"><span data-stu-id="532c1-226">A CNAME record is a DNS feature that maps a source domain tooa destination domain.</span></span> <span data-ttu-id="532c1-227">Du kan till exempel mappa `cdn.contoso.com` eller `static.contoso.com` för`contoso.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="532c1-227">For example, you might map `cdn.contoso.com` or `static.contoso.com` too`contoso.azureedge.net`.</span></span>

<span data-ttu-id="532c1-228">Om du inte har en anpassad domän, bör följande hello [Apptjänst domän kursen](custom-dns-web-site-buydomains-web-app.md) toopurchase en domän med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="532c1-228">If you don't have a custom domain, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a><span data-ttu-id="532c1-229">Hitta hello hostname toouse med hello CNAME</span><span class="sxs-lookup"><span data-stu-id="532c1-229">Find hello hostname toouse with hello CNAME</span></span>

<span data-ttu-id="532c1-230">I hello Azure-portalen **Endpoint** Kontrollera **översikt** har valts i vänster navigering och välj sedan hello hello **+ anpassad domän** knappen hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="532c1-230">In hello Azure portal **Endpoint** page, make sure **Overview** is selected in hello left navigation, and then select hello **+ Custom Domain** button at hello top of hello page.</span></span>

![Välj Lägg till en anpassad domän](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

<span data-ttu-id="532c1-232">I hello **lägga till en anpassad domän** kan du se hello endpoint värden namnet toouse för att skapa en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="532c1-232">In hello **Add a custom domain** page, you see hello endpoint host name toouse in creating a CNAME record.</span></span> <span data-ttu-id="532c1-233">hello värdnamn är härledd från din CDN-slutpunktens URL:  **&lt;EndpointName >. azureedge.net**.</span><span class="sxs-lookup"><span data-stu-id="532c1-233">hello host name is derived from your CDN endpoint URL: **&lt;EndpointName>.azureedge.net**.</span></span> 

![Lägg till domänsida](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a><span data-ttu-id="532c1-235">Konfigurera hello CNAME-post hos din domänregistrator</span><span class="sxs-lookup"><span data-stu-id="532c1-235">Configure hello CNAME with your domain registrar</span></span>

<span data-ttu-id="532c1-236">Navigera tooyour domän register-webbplats och leta upp hello-avsnittet för att skapa DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="532c1-236">Navigate tooyour domain registrar's web site, and locate hello section for creating DNS records.</span></span> <span data-ttu-id="532c1-237">Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.</span><span class="sxs-lookup"><span data-stu-id="532c1-237">You might find this in a section such as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>

<span data-ttu-id="532c1-238">Hitta hello avsnitt för att hantera skapa CNAME-poster.</span><span class="sxs-lookup"><span data-stu-id="532c1-238">Find hello section for managing CNAMEs.</span></span> <span data-ttu-id="532c1-239">Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord CNAME, Alias eller underdomäner.</span><span class="sxs-lookup"><span data-stu-id="532c1-239">You may have toogo tooan advanced settings page and look for hello words CNAME, Alias, or Subdomains.</span></span>

<span data-ttu-id="532c1-240">Skapa en CNAME-post som mappar dina valda underdomän (till exempel **Statiska** eller **cdn**) toohello **Endpoint värdnamn** visas tidigare i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="532c1-240">Create a CNAME record that maps your chosen subdomain (for example, **static** or **cdn**) toohello **Endpoint host name** shown earlier in hello portal.</span></span> 

### <a name="enter-hello-custom-domain-in-azure"></a><span data-ttu-id="532c1-241">Ange hello anpassade domäner i Azure</span><span class="sxs-lookup"><span data-stu-id="532c1-241">Enter hello custom domain in Azure</span></span>

<span data-ttu-id="532c1-242">Returnera toohello **lägga till en anpassad domän** sidan och ange din domän, inklusive hello underdomän hello i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="532c1-242">Return toohello **Add a custom domain** page, and enter your custom domain, including hello subdomain, in hello dialog box.</span></span> <span data-ttu-id="532c1-243">Ange till exempel `cdn.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="532c1-243">For example, enter `cdn.contoso.com`.</span></span>   
   
<span data-ttu-id="532c1-244">Azure verifierar att det finns hello CNAME-post för hello domännamn som du har angett.</span><span class="sxs-lookup"><span data-stu-id="532c1-244">Azure verifies that hello CNAME record exists for hello domain name you have entered.</span></span> <span data-ttu-id="532c1-245">Om hello CNAME stämmer har din anpassade domän verifierats.</span><span class="sxs-lookup"><span data-stu-id="532c1-245">If hello CNAME is correct, your custom domain is validated.</span></span>

<span data-ttu-id="532c1-246">Det kan ta tid för hello CNAME post toopropagate tooname servrar på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="532c1-246">It can take time for hello CNAME record toopropagate tooname servers on hello Internet.</span></span> <span data-ttu-id="532c1-247">Om din domän inte har verifierats omedelbart, vänta en stund och försök igen.</span><span class="sxs-lookup"><span data-stu-id="532c1-247">If your domain is not validated immediately, wait a few minutes and try again.</span></span>

### <a name="test-hello-custom-domain"></a><span data-ttu-id="532c1-248">Testa hello anpassad domän</span><span class="sxs-lookup"><span data-stu-id="532c1-248">Test hello custom domain</span></span>

<span data-ttu-id="532c1-249">I en webbläsare, navigera toohello `index.html` filen med hjälp av en anpassad domän (till exempel `cdn.contoso.com/index.html`) hello tooverify hello resultatet är samma som går direkt för`<endpointname>azureedge.net/index.html`.</span><span class="sxs-lookup"><span data-stu-id="532c1-249">In a browser, navigate toohello `index.html` file using your custom domain (for example, `cdn.contoso.com/index.html`) tooverify that hello result is hello same as when you go directly too`<endpointname>azureedge.net/index.html`.</span></span>

![Exempelstartsidan för app med URL för en anpassad domän](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

<span data-ttu-id="532c1-251">Mer information finns i [anpassad domän för kartan Azure CDN innehåll tooa](../cdn/cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="532c1-251">For more information, see [Map Azure CDN content tooa custom domain](../cdn/cdn-map-content-to-custom-domain.md).</span></span>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="532c1-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="532c1-252">Next steps</span></span>

<span data-ttu-id="532c1-253">Vad du lärt dig:</span><span class="sxs-lookup"><span data-stu-id="532c1-253">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="532c1-254">Skapa en CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="532c1-254">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="532c1-255">Uppdatera cachelagrade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="532c1-255">Refresh cached assets.</span></span>
> * <span data-ttu-id="532c1-256">Använd fråga strängar toocontrol cachelagras versioner.</span><span class="sxs-lookup"><span data-stu-id="532c1-256">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="532c1-257">Använda en anpassad domän för hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="532c1-257">Use a custom domain for hello CDN endpoint.</span></span>

<span data-ttu-id="532c1-258">Lär dig hur toooptimize CDN prestanda i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="532c1-258">Learn how toooptimize CDN performance in hello following articles:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="532c1-259">Förbättra prestandan genom att komprimera filer i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="532c1-259">Improve performance by compressing files in Azure CDN</span></span>](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [<span data-ttu-id="532c1-260">Läsa in tillgångar för en Azure CDN-slutpunkt i förväg</span><span class="sxs-lookup"><span data-stu-id="532c1-260">Pre-load assets on an Azure CDN endpoint</span></span>](../cdn/cdn-preload-endpoint.md)
