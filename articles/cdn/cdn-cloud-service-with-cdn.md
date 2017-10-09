---
title: "aaaIntegrate en Azure-molntjänst med Azure CDN | Microsoft Docs"
description: "Lär dig hur toodeploy en molnbaserad tjänst som hanterar innehåll från en integrerad Azure CDN-slutpunkt"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="ba41f-103"><a name="intro"></a>Integrera en tjänst i molnet med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="ba41f-104">En tjänst i molnet kan integreras med Azure CDN betjänar allt innehåll från plats hello cloud service.</span><span class="sxs-lookup"><span data-stu-id="ba41f-104">A cloud service can be integrated with Azure CDN, serving any content from hello cloud service's location.</span></span> <span data-ttu-id="ba41f-105">Den här metoden ger du hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ba41f-105">This approach gives you hello following advantages:</span></span>

* <span data-ttu-id="ba41f-106">Enkelt distribuera och uppdatera avbildningar, skript och matmallar i din molntjänst projekt kataloger</span><span class="sxs-lookup"><span data-stu-id="ba41f-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="ba41f-107">Lätt att uppgradera hello NuGet-paket i din molntjänst, t.ex jQuery eller Bootstrap versioner</span><span class="sxs-lookup"><span data-stu-id="ba41f-107">Easily upgrade hello NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="ba41f-108">Hantera ditt webbprogram och din CDN-hanterat innehåll från hello samma Visual Studio-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="ba41f-108">Manage your Web application and your CDN-served content all from hello same Visual Studio interface</span></span>
* <span data-ttu-id="ba41f-109">Arbetsflöde för enhetlig distribution för webbprogrammet och din CDN-hanterat innehåll</span><span class="sxs-lookup"><span data-stu-id="ba41f-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="ba41f-110">Integrera ASP.NET paketering och minification med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ba41f-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="ba41f-111">What you will learn</span></span>
<span data-ttu-id="ba41f-112">I den här kursen får du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="ba41f-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="ba41f-113">Integrera Azure CDN-slutpunkten med Molntjänsten och hantera statiskt innehåll i webbsidor från Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="ba41f-114">Konfigurera inställningar för cachelagring för statiskt innehåll i Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="ba41f-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="ba41f-115">Hantera innehåll från domänkontrollanten åtgärder via Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="ba41f-116">Fungera tillsammans och minified innehåll via Azure CDN samtidigt som hello skript upplevelse i Visual Studio-felsökning</span><span class="sxs-lookup"><span data-stu-id="ba41f-116">Serve bundled and minified content through Azure CDN while preserving hello script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="ba41f-117">Konfigurera reserv skripten och CSS när Azure CDN är offline</span><span class="sxs-lookup"><span data-stu-id="ba41f-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="ba41f-118">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="ba41f-118">What you will build</span></span>
<span data-ttu-id="ba41f-119">Du distribuerar en cloud service-webbroll med hello standard ASP.NET MVC-mallen, lägga till kod tooserve innehåll från en integrerad Azure CDN, till exempel en bild, domänkontrollant åtgärd resultat och hello standard JavaScript och CSS-filer, och även skriva koden tooconfigure hello fallback mekanism för paket som hanteras i hello händelse som hello CDN är offline.</span><span class="sxs-lookup"><span data-stu-id="ba41f-119">You will deploy a cloud service Web role using hello default ASP.NET MVC template, add code tooserve content from an integrated Azure CDN, such as an image, controller action results, and hello default JavaScript and CSS files, and also write code tooconfigure hello fallback mechanism for bundles served in hello event that hello CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="ba41f-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="ba41f-120">What you will need</span></span>
<span data-ttu-id="ba41f-121">Den här självstudiekursen har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="ba41f-121">This tutorial has hello following prerequisites:</span></span>

* <span data-ttu-id="ba41f-122">En aktiv [Microsoft Azure-konto](/account/)</span><span class="sxs-lookup"><span data-stu-id="ba41f-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="ba41f-123">Visual Studio 2015 med [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="ba41f-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="ba41f-124">Du behöver ett Azure-konto toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="ba41f-124">You need an Azure account toocomplete this tutorial:</span></span>
> 
> * <span data-ttu-id="ba41f-125">Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används upp du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Websites.</span><span class="sxs-lookup"><span data-stu-id="ba41f-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="ba41f-126">Du kan [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="ba41f-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="ba41f-127">Distribuera en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="ba41f-127">Deploy a cloud service</span></span>
<span data-ttu-id="ba41f-128">I det här avsnittet ska du distribuera hello standard mall för ASP.NET MVC-program i Visual Studio 2015 tooa cloud service-webbroll och integrera den med en ny CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ba41f-128">In this section, you will deploy hello default ASP.NET MVC application template in Visual Studio 2015 tooa cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="ba41f-129">Följ anvisningarna nedan för hello:</span><span class="sxs-lookup"><span data-stu-id="ba41f-129">Follow hello instructions below:</span></span>

1. <span data-ttu-id="ba41f-130">I Visual Studio 2015, skapa en ny Azure-molnet hello menyraden genom att gå för**Arkiv > Nytt > Projekt > molntjänster > Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-130">In Visual Studio 2015, create a new Azure cloud service from hello menu bar by going too**File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="ba41f-131">Ge det ett namn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="ba41f-132">Välj **ASP.NET Web Role** och klicka på hello  **>**  knappen.</span><span class="sxs-lookup"><span data-stu-id="ba41f-132">Select **ASP.NET Web Role** and click hello **>** button.</span></span> <span data-ttu-id="ba41f-133">Klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="ba41f-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="ba41f-134">Välj **MVC** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="ba41f-135">Nu kan publicera den här Web rollen tooan Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ba41f-135">Now, publish this Web role tooan Azure cloud service.</span></span> <span data-ttu-id="ba41f-136">Högerklicka på hello molntjänstprojektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-136">Right-click hello cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="ba41f-137">Om du ännu inte signerats i Microsoft Azure klickar du på hello **Lägg till ett konto...**  listrutan och klicka på hello **Lägg till ett konto** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="ba41f-137">If you have not yet signed into Microsoft Azure, click hello **Add an account...** dropdown and click hello **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="ba41f-138">Logga in med hello Microsoft-konto som du använde tooactivate ditt Azure-konto i hello inloggningssida visas.</span><span class="sxs-lookup"><span data-stu-id="ba41f-138">In hello sign-in page, sign in with hello Microsoft account you used tooactivate your Azure account.</span></span>
7. <span data-ttu-id="ba41f-139">När du har loggat in, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="ba41f-140">Förutsatt att du inte har skapat ett cloud service eller storage-konto kan du skapa både i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba41f-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="ba41f-141">I hello **skapa Molntjänsten och kontot** dialogrutan, hello önskade tjänstnamn och välj hello önskad region.</span><span class="sxs-lookup"><span data-stu-id="ba41f-141">In hello **Create Cloud Service and Account** dialog, type hello desired service name and select hello desired region.</span></span> <span data-ttu-id="ba41f-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="ba41f-143">I hello publicera inställningssidan, verifiera hello konfigurationen och klickar på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-143">In hello publish settings page, verify hello configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="ba41f-144">hello publiceringsprocessen för molntjänster tar lång tid.</span><span class="sxs-lookup"><span data-stu-id="ba41f-144">hello publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="ba41f-145">hello aktivera webbdistribution för alla roller alternativet kan göra felsökning Molntjänsten mycket snabbare genom att tillhandahålla uppdateringar för snabb (men tillfälliga) tooyour Web-roller.</span><span class="sxs-lookup"><span data-stu-id="ba41f-145">hello Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates tooyour Web roles.</span></span> <span data-ttu-id="ba41f-146">Mer information om det här alternativet, se [publicering av en tjänst i molnet med hjälp av hello Azure verktyg](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba41f-146">For more information on this option, see [Publishing a Cloud Service using hello Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="ba41f-147">När hello **Microsoft Azure-aktivitetsloggen** ser att Publiceringsstatus **slutförd**, skapar du en CDN-slutpunkt som är integrerad med den här Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-147">When hello **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="ba41f-148">Om hello distribuerad tjänst i molnet visas ett felmeddelande när du har publicerat, är förmodligen eftersom hello molntjänst som du har distribuerat en [gästen OS som inte innehåller .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="ba41f-148">If, after publishing, hello deployed cloud service displays an error screen, it's likely because hello cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="ba41f-149">Du kan undvika det här problemet genom att [distribuerar .NET 4.5.2 som en startåtgärd](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ba41f-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="ba41f-150">Skapa en ny CDN-profil</span><span class="sxs-lookup"><span data-stu-id="ba41f-150">Create a new CDN profile</span></span>
<span data-ttu-id="ba41f-151">En CDN-profil är en samling CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="ba41f-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="ba41f-152">Varje profil innehåller en eller flera CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="ba41f-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="ba41f-153">Du kanske vill toouse flera profiler tooorganize CDN-slutpunkter med internet-domän, webbprogram eller andra kriterier.</span><span class="sxs-lookup"><span data-stu-id="ba41f-153">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="ba41f-154">Om du redan har en CDN-profil som du vill toouse för den här kursen kan fortsätta för[skapa en ny CDN-slutpunkt](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="ba41f-154">If you already have a CDN profile that you want toouse for this tutorial, proceed too[Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="ba41f-155">Skapa en ny CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ba41f-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="ba41f-156">**toocreate en ny CDN-slutpunkt för ditt lagringskonto**</span><span class="sxs-lookup"><span data-stu-id="ba41f-156">**toocreate a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="ba41f-157">I hello [Azure-hanteringsportalen](https://portal.azure.com), navigera tooyour CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="ba41f-157">In hello [Azure Management Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="ba41f-158">Du kanske har Fäst det toohello instrumentpanelen i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ba41f-158">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="ba41f-159">Om du inte hittar du den genom att klicka på **Bläddra**, sedan **CDN profiler**, och klicka på hello profilen du planerar tooadd till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="ba41f-160">hello CDN-profilbladet visas.</span><span class="sxs-lookup"><span data-stu-id="ba41f-160">hello CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="ba41f-162">Klicka på hello **Lägg till slutpunkt** knappen.</span><span class="sxs-lookup"><span data-stu-id="ba41f-162">Click hello **Add Endpoint** button.</span></span>
   
    ![Knappen Lägg till slutpunkt][cdn-new-endpoint-button]
   
    <span data-ttu-id="ba41f-164">Hej **lägga till en slutpunkt** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="ba41f-164">hello **Add an endpoint** blade appears.</span></span>
   
    ![Bladet Lägg till slutpunkt][cdn-add-endpoint]
3. <span data-ttu-id="ba41f-166">Ange ett **namn** för den här CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="ba41f-167">Det här namnet kommer att använda tooaccess cachelagrade resurserna på hello domän `<EndpointName>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="ba41f-167">This name will be used tooaccess your cached resources at hello domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="ba41f-168">I hello **typ av ursprung** listrutan *Molntjänsten*.</span><span class="sxs-lookup"><span data-stu-id="ba41f-168">In hello **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="ba41f-169">I hello **ursprungsvärdnamn** listrutan, Välj din molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ba41f-169">In hello **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="ba41f-170">Lämna hello standardvärden för **ursprungssökväg**, **ursprungsvärdadress**, och **protokollet ursprung port**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-170">Leave hello defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="ba41f-171">Du måste ange minst ett protokoll (HTTP eller HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ba41f-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="ba41f-172">Klicka på hello **Lägg till** knappen toocreate hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ba41f-172">Click hello **Add** button toocreate hello new endpoint.</span></span>
8. <span data-ttu-id="ba41f-173">När hello slutpunkten har skapats visas den i en lista över slutpunkter för hello-profilen.</span><span class="sxs-lookup"><span data-stu-id="ba41f-173">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="ba41f-174">hello listvyn visar hello URL toouse tooaccess cachelagrat innehåll, samt hello ursprungsdomän.</span><span class="sxs-lookup"><span data-stu-id="ba41f-174">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
   
    ![CDN-slutpunkt][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="ba41f-176">hello endpoint blir omedelbart inte tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="ba41f-176">hello endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="ba41f-177">Det kan ta upp too90 minuter för hello registrering toopropagate via hello CDN nätverk.</span><span class="sxs-lookup"><span data-stu-id="ba41f-177">It can take up too90 minutes for hello registration toopropagate through hello CDN network.</span></span> <span data-ttu-id="ba41f-178">Användare som försöker toouse hello CDN domännamn direkt få statuskod 404 tills hello innehållet är tillgängligt via hello CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-178">Users who try toouse hello CDN domain name immediately may receive status code 404 until hello content is available via hello CDN.</span></span>
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="ba41f-179">Testa hello CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="ba41f-179">Test hello CDN endpoint</span></span>
<span data-ttu-id="ba41f-180">När hello Publiceringsstatus är **slutförd**, öppna ett webbläsarfönster och navigera för**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-180">When hello publishing status is **Completed**, open a browser window and navigate too**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="ba41f-181">URL: en är i min installationsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="ba41f-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="ba41f-182">Som motsvarar toohello följande ursprung URL vid hello CDN-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="ba41f-182">Which corresponds toohello following origin URL at hello CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="ba41f-183">När du navigerar för**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, beroende på din webbläsare kommer du att ange toodownload eller öppna hello bootstrap.css som Kom från ditt publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ba41f-183">When you navigate too**http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted toodownload or open hello bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="ba41f-184">På samma sätt kan du komma åt valfri offentligt tillgänglig URL på  **http://*&lt;serviceName >*.cloudapp.net/** direkt från din CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="ba41f-185">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ba41f-185">For example:</span></span>

* <span data-ttu-id="ba41f-186">En JS-fil från hello/Script sökväg</span><span class="sxs-lookup"><span data-stu-id="ba41f-186">A .js file from hello /Script path</span></span>
* <span data-ttu-id="ba41f-187">Varje innehållsfil från hello /Content sökväg</span><span class="sxs-lookup"><span data-stu-id="ba41f-187">Any content file from hello /Content path</span></span>
* <span data-ttu-id="ba41f-188">En domänkontrollant/åtgärd</span><span class="sxs-lookup"><span data-stu-id="ba41f-188">Any controller/action</span></span>
* <span data-ttu-id="ba41f-189">Om hello frågesträngen är aktiverad på en URL med frågesträngar din CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="ba41f-189">If hello query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="ba41f-190">Med hello ovan konfiguration du i själva verket värd hello hela molnbaserad tjänst från  **http://*&lt;cdnName >*.azureedge.net/**. Om jag navigerar för**http://camservice.azureedge.net/ ** jag hello åtgärd resultatet från Home-Index.</span><span class="sxs-lookup"><span data-stu-id="ba41f-190">In fact, with hello above configuration, you can host hello entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**. If I navigate too**http://camservice.azureedge.net/**, I get hello action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="ba41f-191">Detta innebär inte, men att det alltid är en bra idé tooserve en hel molntjänst via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-191">This does not mean, however, that it's always a good idea tooserve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="ba41f-192">En CDN med statiska leveransoptimering inte nödvändigtvis snabba upp överföringen av dynamisk tillgångar som inte är avsedda toobe cachelagras eller uppdateras väldigt ofta eftersom hello CDN måste hämta en ny version av hello tillgång från hello ursprungsservern så ofta.</span><span class="sxs-lookup"><span data-stu-id="ba41f-192">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant toobe cached, or are updated very frequently, since hello CDN must pull a new version of hello asset from hello Origin server very often.</span></span> <span data-ttu-id="ba41f-193">I det här scenariot kan du aktivera [dynamiska plats Acceleration](cdn-dynamic-site-acceleration.md) optimering (DSA) på din CDN-slutpunkt som använder olika tekniker toospeed upp överföringen av ej Cacheable ställs dynamiska tillgångar.</span><span class="sxs-lookup"><span data-stu-id="ba41f-193">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques toospeed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="ba41f-194">Om du har en plats med en blandning av statiska och dynamiska innehåll du tooserve dina statiskt innehåll från CDN med en statisk optimering typ (till exempel Internet leverans) och tooserve dynamiskt innehåll direkt från hello ursprungsservern eller via en CDN slutpunkten med DSA optimering aktiverat-fall till fall.</span><span class="sxs-lookup"><span data-stu-id="ba41f-194">If you have a site with a mix of static and dynamic content, you can choose tooserve your static content from CDN with a static optimization type (such as general web delivery), and tooserve dynamic content either directly from hello origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="ba41f-195">toothat end du redan har sett hur tooaccess enskilda innehåll filer från hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-195">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="ba41f-196">Jag visar hur tooserve en specifik domänkontrollant åtgärd via en specifik CDN-slutpunkt i Hantera innehåll från domänkontrollanten åtgärder via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-196">I will show you how tooserve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="ba41f-197">hello alternativ är toodetermine som innehåll tooserve från Azure CDN för fall till fall i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-197">hello alternative is toodetermine which content tooserve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="ba41f-198">toothat end du redan har sett hur tooaccess enskilda innehåll filer från hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-198">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="ba41f-199">Jag får du lära dig hur tooserve en specifik domänkontrollant åtgärd via hello CDN-slutpunkten i [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller).</span><span class="sxs-lookup"><span data-stu-id="ba41f-199">I will show you how tooserve a specific controller action through hello CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="ba41f-200">Konfigurera cachelagringsalternativ för statiska filer i Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="ba41f-200">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="ba41f-201">Du kan ange hur du vill att statiskt innehåll toobe som cachelagrats i hello CDN-slutpunkten med Azure CDN-integration i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-201">With Azure CDN integration in your cloud service, you can specify how you want static content toobe cached in hello CDN endpoint.</span></span> <span data-ttu-id="ba41f-202">toodo detta, öppna *Web.config* från din webbroll projekt (t.ex. WebRole1) och Lägg till en `<staticContent>` element för`<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="ba41f-202">toodo this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element too`<system.webServer>`.</span></span> <span data-ttu-id="ba41f-203">hello XML nedan konfigurerar hello cache tooexpire i tre dagar.</span><span class="sxs-lookup"><span data-stu-id="ba41f-203">hello XML below configures hello cache tooexpire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="ba41f-204">När du gör detta kommer alla statiska filer i din molntjänst Se hello samma regel i CDN-cachen.</span><span class="sxs-lookup"><span data-stu-id="ba41f-204">Once you do this, all static files in your cloud service will observe hello same rule in your CDN cache.</span></span> <span data-ttu-id="ba41f-205">För mer detaljerad kontroll över inställningar för cachelagring för att lägga till en *Web.config* till en mapp och Lägg till dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="ba41f-205">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="ba41f-206">Till exempel lägga till en *Web.config* filen toohello *\Content* mapp och Ersätt hello innehållet med följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="ba41f-206">For example, add a *Web.config* file toohello *\Content* folder and replace hello content with hello following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="ba41f-207">Den här inställningen gör att alla statiska filer från hello *\Content* mappen toobe som cachelagrats för 15 dagar.</span><span class="sxs-lookup"><span data-stu-id="ba41f-207">This setting causes all static files from hello *\Content* folder toobe cached for 15 days.</span></span>

<span data-ttu-id="ba41f-208">Mer information om hur tooconfigure hello `<clientCache>` element, se [klientcachen &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="ba41f-208">For more information on how tooconfigure hello `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="ba41f-209">I [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller), jag visas också hur du kan konfigurera inställningar för cachelagring för domänkontrollant åtgärden resulterar i hello CDN cache.</span><span class="sxs-lookup"><span data-stu-id="ba41f-209">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in hello CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="ba41f-210">Hantera innehåll från domänkontrollanten åtgärder via Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-210">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="ba41f-211">När du integrerar en cloud service-webbroll med Azure CDN är relativt enkelt tooserve innehåll från domänkontrollanten åtgärder via hello Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-211">When you integrate a cloud service Web role with Azure CDN, it is relatively easy tooserve content from controller actions through hello Azure CDN.</span></span> <span data-ttu-id="ba41f-212">Tjänsten direkt via Azure CDN (som visas ovan) än betjänar ditt moln [Maarten Balliauw](https://twitter.com/maartenballiauw) visas hur toodo den med ett roligt MemeGenerator domänkontrollant i [minskar svarstiden på hello web med hello Azure CDN ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="ba41f-212">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how toodo it with a fun MemeGenerator controller in [Reducing latency on hello web with hello Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="ba41f-213">Jag kommer att återskapa den här.</span><span class="sxs-lookup"><span data-stu-id="ba41f-213">I will simply reproduce it here.</span></span>

<span data-ttu-id="ba41f-214">Anta att i Molntjänsten du toogenerate memes baserat på ett barn Chuck Norris bild (foto av [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) så här:</span><span class="sxs-lookup"><span data-stu-id="ba41f-214">Suppose in your cloud service you want toogenerate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="ba41f-215">Du har en enkel `Index` åtgärd som kunder hello toospecify hello superlativ hello bild sedan genererar hello meme när de efter toohello åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba41f-215">You have a simple `Index` action that allows hello customers toospecify hello superlatives in hello image, then generates hello meme once they post toohello action.</span></span> <span data-ttu-id="ba41f-216">Eftersom det är Chuck Norris förväntar du dig den här sidan toobecome wildly populära globalt.</span><span class="sxs-lookup"><span data-stu-id="ba41f-216">Since it's Chuck Norris, you would expect this page toobecome wildly popular globally.</span></span> <span data-ttu-id="ba41f-217">Detta är ett bra exempel på betjänar delvis dynamiskt innehåll med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-217">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="ba41f-218">Följ hello stegen ovan toosetup åtgärden domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="ba41f-218">Follow hello steps above toosetup this controller action:</span></span>

1. <span data-ttu-id="ba41f-219">I hello *\Controllers* mapp, skapa en ny .cs-fil som kallas *MemeGeneratorController.cs* och Ersätt hello innehåll med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="ba41f-219">In hello *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace hello content with hello following code.</span></span> <span data-ttu-id="ba41f-220">Vara säker på att tooreplace hello markerade delen med CDN-namn.</span><span class="sxs-lookup"><span data-stu-id="ba41f-220">Be sure tooreplace hello highlighted portion with your CDN name.</span></span>  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. <span data-ttu-id="ba41f-221">Högerklicka i hello standard `Index()` och väljer **Lägg till vy**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-221">Right-click in hello default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="ba41f-222">Godkänn hello inställningarna nedan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ba41f-222">Accept hello settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="ba41f-223">Öppna ny hello *Views\MemeGenerator\Index.cshtml* och Ersätt hello innehållet med följande enkelt HTML-format för att skicka hello superlativ hello:</span><span class="sxs-lookup"><span data-stu-id="ba41f-223">Open hello new *Views\MemeGenerator\Index.cshtml* and replace hello content with hello following simple HTML for submitting hello superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="ba41f-224">Publicera hello tjänst i molnet igen och gå för**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ba41f-224">Publish hello cloud service again and navigate too**http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="ba41f-225">När du skickar hello formulärvärden för`/MemeGenerator/Index`, hello `Index_Post` åtgärdsmetod returnerar en länk toohello `Show` åtgärdsmetod med hello respektive inkommande identifierare.</span><span class="sxs-lookup"><span data-stu-id="ba41f-225">When you submit hello form values too`/MemeGenerator/Index`, hello `Index_Post` action method returns a link toohello `Show` action method with hello respective input identifier.</span></span> <span data-ttu-id="ba41f-226">När du klickar på länken hello nå hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="ba41f-226">When you click hello link, you reach hello following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="ba41f-227">Om din lokala felsökare, får du hello reguljära debug-upplevelse med en lokal omdirigering.</span><span class="sxs-lookup"><span data-stu-id="ba41f-227">If your local debugger is attached, then you will get hello regular debug experience with a local redirect.</span></span> <span data-ttu-id="ba41f-228">Om den körs i hello Molntjänsten ska den omdirigera till:</span><span class="sxs-lookup"><span data-stu-id="ba41f-228">If it's running in hello cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="ba41f-229">Som motsvarar toohello följande ursprung URL vid CDN-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="ba41f-229">Which corresponds toohello following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="ba41f-230">Du kan sedan använda hello `OutputCacheAttribute` -attributet på hello `Generate` metoden toospecify hur hello åtgärd resultatet ska cachelagras, som hanterar Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-230">You can then use hello `OutputCacheAttribute` attribute on hello `Generate` method toospecify how hello action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="ba41f-231">hello koden nedan ange en giltighetstid för cache 1 timme (3 600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="ba41f-231">hello code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="ba41f-232">På samma sätt kan du hantera innehåll från något domänkontrollant i Molntjänsten via din Azure CDN med alternativet för cachelagring av hello önskad.</span><span class="sxs-lookup"><span data-stu-id="ba41f-232">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with hello desired caching option.</span></span>

<span data-ttu-id="ba41f-233">I nästa avsnitt om hello visar jag hur tooserve hello paketerade och minified skript och CSS via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-233">In hello next section, I will show you how tooserve hello bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="ba41f-234">Integrera ASP.NET paketering och minification med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-234">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="ba41f-235">Skript och CSS matmallar ändras sällan och är särskilda kandidater för hello Azure CDN cache.</span><span class="sxs-lookup"><span data-stu-id="ba41f-235">Scripts and CSS stylesheets change infrequently and are prime candidates for hello Azure CDN cache.</span></span> <span data-ttu-id="ba41f-236">Betjänar hello hela webbroll via Azure CDN är hello enklaste sättet toointegrate paketering och minification med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-236">Serving hello entire Web role through your Azure CDN is hello easiest way toointegrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="ba41f-237">Men du kanske inte vill toodo detta, visas I hur toodo den samtidigt som hello önskad develper upplevelse av ASP.NET paketering och minification, som:</span><span class="sxs-lookup"><span data-stu-id="ba41f-237">However, as you may not want toodo this, I will show you how toodo it while preserving hello desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="ba41f-238">Bra debug problem</span><span class="sxs-lookup"><span data-stu-id="ba41f-238">Great debug mode experience</span></span>
* <span data-ttu-id="ba41f-239">Effektiv distribution</span><span class="sxs-lookup"><span data-stu-id="ba41f-239">Streamlined deployment</span></span>
* <span data-ttu-id="ba41f-240">Omedelbar uppdateringar tooclients för skriptet/CSS versionsuppgraderingar</span><span class="sxs-lookup"><span data-stu-id="ba41f-240">Immediate updates tooclients for script/CSS version upgrades</span></span>
* <span data-ttu-id="ba41f-241">Fallback mekanism när CDN-slutpunkten misslyckas</span><span class="sxs-lookup"><span data-stu-id="ba41f-241">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="ba41f-242">Minimera kodändring</span><span class="sxs-lookup"><span data-stu-id="ba41f-242">Minimize code modification</span></span>

<span data-ttu-id="ba41f-243">I hello **WebRole1** projekt som du skapade i [integrera Azure CDN-slutpunkten med din Azure-webbplatsen och hantera statiskt innehåll i webbsidor från Azure CDN](#deploy)öppnar *App_Start\ BundleConfig.cs* och ta en titt på hello `bundles.Add()` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="ba41f-243">In hello **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at hello `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="ba41f-244">Hej först `bundles.Add()` instruktionen lägger till ett skript-paket på hello virtuella katalogen `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="ba41f-244">hello first `bundles.Add()` statement adds a script bundle at hello virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="ba41f-245">Öppna *Views\Shared\_Layout.cshtml* toosee hur hello paket skripttypen återges.</span><span class="sxs-lookup"><span data-stu-id="ba41f-245">Then, open *Views\Shared\_Layout.cshtml* toosee how hello script bundle tag is rendered.</span></span> <span data-ttu-id="ba41f-246">Du ska kunna toofind hello följande Razor kodrad:</span><span class="sxs-lookup"><span data-stu-id="ba41f-246">You should be able toofind hello following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="ba41f-247">När den här Razor koden körs i hello Azure-webbroll blir en `<script>` taggen för hello skript paket liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ba41f-247">When this Razor code is run in hello Azure Web role, it will render a `<script>` tag for hello script bundle similar toohello following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="ba41f-248">Men när det körs i Visual Studio genom att skriva `F5`, den kommer att visas varje skriptfilen i hello paket individuellt (i hello fallet ovan endast en skriptfil finns i hello-paket):</span><span class="sxs-lookup"><span data-stu-id="ba41f-248">However, when it is run in Visual Studio by typing `F5`, it will render each script file in hello bundle individually (in hello case above, only one script file is in hello bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="ba41f-249">Detta gör att du toodebug hello JavaScript-kod i din utvecklingsmiljö medan vilket minskar antalet samtidiga klientanslutningar (paketering) och förbättra filen hämta prestanda (minification) i produktion.</span><span class="sxs-lookup"><span data-stu-id="ba41f-249">This enables you toodebug hello JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="ba41f-250">Det är en bra funktionen toopreserve med Azure CDN-integration.</span><span class="sxs-lookup"><span data-stu-id="ba41f-250">It's a great feature toopreserve with Azure CDN integration.</span></span> <span data-ttu-id="ba41f-251">Dessutom eftersom hello renderas paketet innehåller redan en automatiskt genererad versionssträng måste du vill tooreplicate funktioner hello så när du uppdaterar din jQuery version via NuGet, den kan uppdateras på klientsidan för hello så snart möjligt.</span><span class="sxs-lookup"><span data-stu-id="ba41f-251">Furthermore, since hello rendered bundle already contains an automatically generated version string, you want tooreplicate that functionality so hello whenever you update your jQuery version through NuGet, it can be updated at hello client side as soon as possible.</span></span>

<span data-ttu-id="ba41f-252">Följ hello steg nedan toointegration ASP.NET paketering och minification med CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-252">Follow hello steps below toointegration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="ba41f-253">Tillbaka i *App_Start\BundleConfig.cs*, ändra hello `bundles.Add()` metoder toouse en annan [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), som anger en CDN-adress.</span><span class="sxs-lookup"><span data-stu-id="ba41f-253">Back in *App_Start\BundleConfig.cs*, modify hello `bundles.Add()` methods toouse a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="ba41f-254">toodo detta, Ersätt hello `RegisterBundles` metoddefinition med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="ba41f-254">toodo this, replace hello `RegisterBundles` method definition with hello following code:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="ba41f-255">Vara säker på att tooreplace `<yourCDNName>` med namnet hello Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-255">Be sure tooreplace `<yourCDNName>` with hello name of your Azure CDN.</span></span>
   
    <span data-ttu-id="ba41f-256">Inställningen i klartext, `bundles.UseCdn = true` och lägga till ett noggrant utformad CDN URL tooeach paket.</span><span class="sxs-lookup"><span data-stu-id="ba41f-256">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL tooeach bundle.</span></span> <span data-ttu-id="ba41f-257">Till exempel hello första konstruktorn i hello kod:</span><span class="sxs-lookup"><span data-stu-id="ba41f-257">For example, hello first constructor in hello code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="ba41f-258">är hello samma som:</span><span class="sxs-lookup"><span data-stu-id="ba41f-258">is hello same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="ba41f-259">Den här konstruktorn instruerar ASP.NET paketering och minification toorender enskilda filer när felsöks lokalt, men Använd hello angetts CDN adress tooaccess hello skript i fråga.</span><span class="sxs-lookup"><span data-stu-id="ba41f-259">This constructor tells ASP.NET bundling and minification toorender individual script files when debugged locally, but use hello specified CDN address tooaccess hello script in question.</span></span> <span data-ttu-id="ba41f-260">Observera dock två viktiga egenskaper med den här noggrant utformad CDN-URL:</span><span class="sxs-lookup"><span data-stu-id="ba41f-260">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="ba41f-261">hello ursprung för denna CDN-URL är `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vilket är faktiskt hello virtuell katalog för hello skript paket i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-261">hello origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually hello virtual directory of hello script bundle in your cloud service.</span></span>
   * <span data-ttu-id="ba41f-262">Eftersom du använder CDN konstruktorn innehåller hello CDN skripttypen för hello paket inte längre hello genereras automatiskt Versionsträngen i hello renderas URL.</span><span class="sxs-lookup"><span data-stu-id="ba41f-262">Since you are using CDN constructor, hello CDN script tag for hello bundle no longer contains hello automatically generated version string in hello rendered URL.</span></span> <span data-ttu-id="ba41f-263">Manuellt måste du generera en unik versionssträng varje gång hello skript paket är ändrade tooforce en cache missar på Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ba41f-263">You must manually generate a unique version string every time hello script bundle is modified tooforce a cache miss at your Azure CDN.</span></span> <span data-ttu-id="ba41f-264">AT hello samma tid unik versionssträngen måste vara konstant via hello livslängd hello distribution toomaximize cacheträffar vid Azure CDN när hello-paket har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="ba41f-264">At hello same time, this unique version string must remain constant through hello life of hello deployment toomaximize cache hits at your Azure CDN after hello bundle is deployed.</span></span>
   * <span data-ttu-id="ba41f-265">Hej frågesträngen v = < W.X.Y.Z > hämtar från *Properties\AssemblyInfo.cs* i din webbrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="ba41f-265">hello query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="ba41f-266">Du kan ha ett arbetsflöde för distribution som innehåller ökar hello Sammansättningsversionen varje gång du publicerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ba41f-266">You can have a deployment workflow that includes incrementing hello assembly version every time you publish tooAzure.</span></span> <span data-ttu-id="ba41f-267">Eller, du kan bara ändra *Properties\AssemblyInfo.cs* i ditt projekt tooautomatically ökning hello versionssträng varje gång du skapar med hjälp av hello jokertecknet ' *'.</span><span class="sxs-lookup"><span data-stu-id="ba41f-267">Or, you can just modify *Properties\AssemblyInfo.cs* in your project tooautomatically increment hello version string every time you build, using hello wildcard character '*'.</span></span> <span data-ttu-id="ba41f-268">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ba41f-268">For example:</span></span>
     
        <span data-ttu-id="ba41f-269">[sammansättningen: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="ba41f-269">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="ba41f-270">Alla andra strategi toostreamline Generera en unik sträng för hello livslängden för en distribution fungerar här.</span><span class="sxs-lookup"><span data-stu-id="ba41f-270">Any other strategy toostreamline generating a unique string for hello life of a deployment will work here.</span></span>
2. <span data-ttu-id="ba41f-271">Publicera hello cloud service och åtkomst hello startsidan.</span><span class="sxs-lookup"><span data-stu-id="ba41f-271">Republish hello cloud service and access hello home page.</span></span>
3. <span data-ttu-id="ba41f-272">Visa hello HTML-koden för hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="ba41f-272">View hello HTML code for hello page.</span></span> <span data-ttu-id="ba41f-273">Du ska kunna toosee hello CDN-URL som renderas med en unik versionssträng varje gång du publicera ändringar tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-273">You should be able toosee hello CDN URL rendered, with a unique version string every time you republish changes tooyour cloud service.</span></span> <span data-ttu-id="ba41f-274">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ba41f-274">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="ba41f-275">I Visual Studio debug hello Molntjänsten i Visual Studio genom att skriva `F5`.,</span><span class="sxs-lookup"><span data-stu-id="ba41f-275">In Visual Studio, debug hello cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="ba41f-276">Visa hello HTML-koden för hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="ba41f-276">View hello HTML code for hello page.</span></span> <span data-ttu-id="ba41f-277">Du kommer fortfarande att se varje skriptfilen individuellt återges så att du får en konsekvent debug upplevelse i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba41f-277">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="ba41f-278">Fallback mekanism för CDN-URL: er</span><span class="sxs-lookup"><span data-stu-id="ba41f-278">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="ba41f-279">När Azure CDN-slutpunkten misslyckas av någon anledning, vill du din webbsida toobe smarta tillräckligt med tooaccess webbservern ursprung som hello återställningsalternativ för inläsning av JavaScript eller starttjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba41f-279">When your Azure CDN endpoint fails for any reason, you want your Web page toobe smart enough tooaccess your origin Web server as hello fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="ba41f-280">Det är tillräckligt allvarligt toolose bilder på din webbplats på grund av tooCDN inte finns, men mycket allvarligare toolose avgörande funktionalitet som tillhandahålls av skripten och formatmallar.</span><span class="sxs-lookup"><span data-stu-id="ba41f-280">It's serious enough toolose images on your website due tooCDN unavailability, but much more severe toolose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="ba41f-281">Hej [paket](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) klassen innehåller en egenskap som kallas [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) som du kan använda tooconfigure hello återställningsplats mekanism för CDN-fel.</span><span class="sxs-lookup"><span data-stu-id="ba41f-281">hello [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you tooconfigure hello fallback mechanism for CDN failure.</span></span> <span data-ttu-id="ba41f-282">toouse den här egenskapen följer hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="ba41f-282">toouse this property, follow hello steps below:</span></span>

1. <span data-ttu-id="ba41f-283">Öppna i din webbrollsprojektet *App_Start\BundleConfig.cs*, där du lade till en CDN-URL i varje [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), och se hello följande markerade ändras tooadd återställningsplats mekanism toohello standard-paket:</span><span class="sxs-lookup"><span data-stu-id="ba41f-283">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make hello following highlighted changes tooadd fallback mechanism toohello default bundles:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="ba41f-284">När `CdnFallbackExpression` är null, skript är injekteras i hello HTML tootest om hello-paket har lästs in och, om inte, kommer du åt hello paket direkt från hello ursprung webbservern.</span><span class="sxs-lookup"><span data-stu-id="ba41f-284">When `CdnFallbackExpression` is not null, script is injected into hello HTML tootest whether hello bundle is loaded successfully and, if not, access hello bundle directly from hello origin Web server.</span></span> <span data-ttu-id="ba41f-285">Den här egenskapen måste toobe tooa JavaScript mängduttryck som testar om hello respektive CDN-paket har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="ba41f-285">This property needs toobe set tooa JavaScript expression that tests whether hello respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="ba41f-286">hello uttryck behövs tootest varje paket skiljer sig bl.a toohello innehåll.</span><span class="sxs-lookup"><span data-stu-id="ba41f-286">hello expression needed tootest each bundle differs according toohello content.</span></span> <span data-ttu-id="ba41f-287">För hello standard paket ovan:</span><span class="sxs-lookup"><span data-stu-id="ba41f-287">For hello default bundles above:</span></span>
   
   * <span data-ttu-id="ba41f-288">`window.jquery`har definierats i jquery-{version} .js</span><span class="sxs-lookup"><span data-stu-id="ba41f-288">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="ba41f-289">`$.validator`har definierats i jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="ba41f-289">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="ba41f-290">`window.Modernizr`har definierats i modernizer-{version} .js</span><span class="sxs-lookup"><span data-stu-id="ba41f-290">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="ba41f-291">`$.fn.modal`har definierats i bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ba41f-291">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="ba41f-292">Du kanske har lagt märke till att jag inte har angett CdnFallbackExpression för hello `~/Cointent/css` paket.</span><span class="sxs-lookup"><span data-stu-id="ba41f-292">You might have noticed that I did not set CdnFallbackExpression for hello `~/Cointent/css` bundle.</span></span> <span data-ttu-id="ba41f-293">Detta beror på att det finns för närvarande en [programfel i System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) som lägger in en `<script>` taggen för hello återställningsplats CSS i stället för hello förväntat `<link>` tagg.</span><span class="sxs-lookup"><span data-stu-id="ba41f-293">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for hello fallback CSS instead of hello expected `<link>` tag.</span></span>
     
     <span data-ttu-id="ba41f-294">Det är emellertid en bra [Style paket reserv](https://github.com/EmberConsultingGroup/StyleBundleFallback) erbjuds av [Ember samråd grupp](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="ba41f-294">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="ba41f-295">toouse hello lösning för CSS, skapa en ny .cs-fil i din webbrollsprojektet *App_Start* mapp med namnet *StyleBundleExtensions.cs*, och Ersätt innehållet med hello [kod från GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="ba41f-295">toouse hello workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with hello [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="ba41f-296">I *App_Start\StyleFundleExtensions.cs*, byta namn på hello namnområde tooyour Web rollnamn (t.ex. **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="ba41f-296">In *App_Start\StyleFundleExtensions.cs*, rename hello namespace tooyour Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="ba41f-297">Gå tillbaka för`App_Start\BundleConfig.cs` och ändra hello senast `bundles.Add` instruktion med hello följande markerade koden:</span><span class="sxs-lookup"><span data-stu-id="ba41f-297">Go back too`App_Start\BundleConfig.cs` and modify hello last `bundles.Add` statement with hello following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="ba41f-298">Den här nya tilläggsmetoden använder hello samma uppfattning tooinject skript i hello HTML toocheck hello DOM för hello en matchande klassnamn och Regelnamn regeln värdet som definierats i hello CSS paket och faller tillbaka toohello ursprung webbserver om det misslyckas toofind hello matchning.</span><span class="sxs-lookup"><span data-stu-id="ba41f-298">This new extension method uses hello same idea tooinject script in hello HTML toocheck hello DOM for hello a matching class name, rule name, and rule value defined in hello CSS bundle, and falls back toohello origin Web server if it fails toofind hello match.</span></span>
5. <span data-ttu-id="ba41f-299">Publicera hello Molntjänsten igen och åtkomst hello-startsidan.</span><span class="sxs-lookup"><span data-stu-id="ba41f-299">Publish hello cloud service again and access hello home page.</span></span>
6. <span data-ttu-id="ba41f-300">Visa hello HTML-koden för hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="ba41f-300">View hello HTML code for hello page.</span></span> <span data-ttu-id="ba41f-301">Du ska hitta inmatat skript liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ba41f-301">You should find injected scripts similar toohello following:</span></span>    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    <span data-ttu-id="ba41f-302">Observera att inmatat skript för hello CSS paket fortfarande innehåller hello hänga kvarleva från hello `CdnFallbackExpression` egenskap i hello rad:</span><span class="sxs-lookup"><span data-stu-id="ba41f-302">Note that injected script for hello CSS bundle still contains hello errant remnant from hello `CdnFallbackExpression` property in hello line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="ba41f-303">Men eftersom hello första delen av hello. uttrycket returnerar alltid true (i hello rad ovanför som), hello document.write() funktionen aldrig ska köras.</span><span class="sxs-lookup"><span data-stu-id="ba41f-303">But since hello first part of hello || expression will always return true (in hello line directly above that), hello document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="ba41f-304">Mer information</span><span class="sxs-lookup"><span data-stu-id="ba41f-304">More Information</span></span>
* [<span data-ttu-id="ba41f-305">Översikt över hello Azure Content Delivery Network (CDN)</span><span class="sxs-lookup"><span data-stu-id="ba41f-305">Overview of hello Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="ba41f-306">Med hjälp av Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ba41f-306">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="ba41f-307">ASP.NET paketering och Minification</span><span class="sxs-lookup"><span data-stu-id="ba41f-307">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
