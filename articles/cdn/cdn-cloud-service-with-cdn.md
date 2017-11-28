---
title: "Integrera en Azure-molntjänst med Azure CDN | Microsoft Docs"
description: "Lär dig hur du distribuerar en molnbaserad tjänst som hanterar innehåll från en integrerad Azure CDN-slutpunkt"
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
ms.openlocfilehash: f2849fe25fd0d5b3dc26598ffba7591cb7433161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="7b06f-103"><a name="intro"></a>Integrera en tjänst i molnet med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="7b06f-104">En tjänst i molnet kan integreras med Azure CDN betjänar allt innehåll från Molntjänsten plats.</span><span class="sxs-lookup"><span data-stu-id="7b06f-104">A cloud service can be integrated with Azure CDN, serving any content from the cloud service's location.</span></span> <span data-ttu-id="7b06f-105">Den här metoden ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7b06f-105">This approach gives you the following advantages:</span></span>

* <span data-ttu-id="7b06f-106">Enkelt distribuera och uppdatera avbildningar, skript och matmallar i din molntjänst projekt kataloger</span><span class="sxs-lookup"><span data-stu-id="7b06f-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="7b06f-107">Lätt att uppgradera NuGet-paket i din molntjänst, t.ex jQuery eller Bootstrap versioner</span><span class="sxs-lookup"><span data-stu-id="7b06f-107">Easily upgrade the NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="7b06f-108">Hantera ditt webbprogram och din CDN-hanterat innehåll alla från samma Visual Studio-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="7b06f-108">Manage your Web application and your CDN-served content all from the same Visual Studio interface</span></span>
* <span data-ttu-id="7b06f-109">Arbetsflöde för enhetlig distribution för webbprogrammet och din CDN-hanterat innehåll</span><span class="sxs-lookup"><span data-stu-id="7b06f-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="7b06f-110">Integrera ASP.NET paketering och minification med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7b06f-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="7b06f-111">What you will learn</span></span>
<span data-ttu-id="7b06f-112">I den här kursen får du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="7b06f-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="7b06f-113">Integrera Azure CDN-slutpunkten med Molntjänsten och hantera statiskt innehåll i webbsidor från Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="7b06f-114">Konfigurera inställningar för cachelagring för statiskt innehåll i Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="7b06f-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="7b06f-115">Hantera innehåll från domänkontrollanten åtgärder via Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="7b06f-116">Fungera tillsammans och minified innehåll via Azure CDN samtidigt som skriptet upplevelse i Visual Studio-felsökning</span><span class="sxs-lookup"><span data-stu-id="7b06f-116">Serve bundled and minified content through Azure CDN while preserving the script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="7b06f-117">Konfigurera reserv skripten och CSS när Azure CDN är offline</span><span class="sxs-lookup"><span data-stu-id="7b06f-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="7b06f-118">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="7b06f-118">What you will build</span></span>
<span data-ttu-id="7b06f-119">Du distribuerar en cloud service-webbroll med ASP.NET MVC-mallen, lägga till kod för att hantera innehåll från en integrerad Azure CDN som en bild, domänkontrollant åtgärd resultat och standard JavaScript och CSS-filer, och även skriva kod för att konfigurera reserven mekanism för paket hanteras i händelse av att CDN är offline.</span><span class="sxs-lookup"><span data-stu-id="7b06f-119">You will deploy a cloud service Web role using the default ASP.NET MVC template, add code to serve content from an integrated Azure CDN, such as an image, controller action results, and the default JavaScript and CSS files, and also write code to configure the fallback mechanism for bundles served in the event that the CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="7b06f-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="7b06f-120">What you will need</span></span>
<span data-ttu-id="7b06f-121">Den här självstudiekursen har följande krav:</span><span class="sxs-lookup"><span data-stu-id="7b06f-121">This tutorial has the following prerequisites:</span></span>

* <span data-ttu-id="7b06f-122">En aktiv [Microsoft Azure-konto](/account/)</span><span class="sxs-lookup"><span data-stu-id="7b06f-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="7b06f-123">Visual Studio 2015 med [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="7b06f-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="7b06f-124">Du behöver ett Azure-konto för att kunna slutföra den här guiden:</span><span class="sxs-lookup"><span data-stu-id="7b06f-124">You need an Azure account to complete this tutorial:</span></span>
> 
> * <span data-ttu-id="7b06f-125">Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda för att testa Azure-betaltjänster och även när de används upp kan du behålla kontot och använda kostnadsfria Azure-tjänster, till exempel Websites.</span><span class="sxs-lookup"><span data-stu-id="7b06f-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="7b06f-126">Du kan [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="7b06f-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="7b06f-127">Distribuera en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="7b06f-127">Deploy a cloud service</span></span>
<span data-ttu-id="7b06f-128">I det här avsnittet ska du distribuera standard mall för ASP.NET MVC-program i Visual Studio 2015 till en cloud service-webbroll och integrera den med en ny CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7b06f-128">In this section, you will deploy the default ASP.NET MVC application template in Visual Studio 2015 to a cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="7b06f-129">Följ anvisningarna nedan:</span><span class="sxs-lookup"><span data-stu-id="7b06f-129">Follow the instructions below:</span></span>

1. <span data-ttu-id="7b06f-130">I Visual Studio 2015, skapar du en ny Azure-molntjänst på menyraden genom att gå till **Arkiv > Nytt > Projekt > molntjänster > Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-130">In Visual Studio 2015, create a new Azure cloud service from the menu bar by going to **File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="7b06f-131">Ge det ett namn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="7b06f-132">Välj **ASP.NET Web Role** och klicka på den  **>**  knappen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-132">Select **ASP.NET Web Role** and click the **>** button.</span></span> <span data-ttu-id="7b06f-133">Klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="7b06f-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="7b06f-134">Välj **MVC** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="7b06f-135">Nu publicera den här webbroll till en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="7b06f-135">Now, publish this Web role to an Azure cloud service.</span></span> <span data-ttu-id="7b06f-136">Högerklicka på molntjänstprojektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-136">Right-click the cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="7b06f-137">Om du ännu inte signerats i Microsoft Azure klickar du på den **Lägg till ett konto...**  listrutan och klicka på den **Lägg till ett konto** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="7b06f-137">If you have not yet signed into Microsoft Azure, click the **Add an account...** dropdown and click the **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="7b06f-138">Logga in med Microsoft-konto som du använde för att aktivera din Azure-konto på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="7b06f-138">In the sign-in page, sign in with the Microsoft account you used to activate your Azure account.</span></span>
7. <span data-ttu-id="7b06f-139">När du har loggat in, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="7b06f-140">Förutsatt att du inte har skapat ett cloud service eller storage-konto kan du skapa både i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b06f-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="7b06f-141">I den **skapa Molntjänsten och kontot** dialogrutan Skriv önskade tjänstens namn och välj sedan önskad region.</span><span class="sxs-lookup"><span data-stu-id="7b06f-141">In the **Create Cloud Service and Account** dialog, type the desired service name and select the desired region.</span></span> <span data-ttu-id="7b06f-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="7b06f-143">Kontrollera konfigurationen i inställningssidan publicera och klickar på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-143">In the publish settings page, verify the configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="7b06f-144">Publiceringsprocessen för molntjänster tar lång tid.</span><span class="sxs-lookup"><span data-stu-id="7b06f-144">The publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="7b06f-145">Den aktivera webbdistribution för alla roller alternativet kan göra felsökning Molntjänsten mycket snabbare genom att tillhandahålla snabb (men tillfälliga) uppdateringar till Web-roller.</span><span class="sxs-lookup"><span data-stu-id="7b06f-145">The Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates to your Web roles.</span></span> <span data-ttu-id="7b06f-146">Mer information om det här alternativet, se [publicering av en tjänst i molnet med hjälp av Azure-verktyg](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b06f-146">For more information on this option, see [Publishing a Cloud Service using the Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="7b06f-147">När den **Microsoft Azure-aktivitetsloggen** ser att Publiceringsstatus **slutförd**, skapar du en CDN-slutpunkt som är integrerad med den här Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-147">When the **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="7b06f-148">Om distribuerad Molntjänsten visas ett felmeddelande när du har publicerat, är förmodligen eftersom den molntjänst som du har distribuerat en [gästen OS som inte innehåller .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="7b06f-148">If, after publishing, the deployed cloud service displays an error screen, it's likely because the cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="7b06f-149">Du kan undvika det här problemet genom att [distribuerar .NET 4.5.2 som en startåtgärd](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="7b06f-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="7b06f-150">Skapa en ny CDN-profil</span><span class="sxs-lookup"><span data-stu-id="7b06f-150">Create a new CDN profile</span></span>
<span data-ttu-id="7b06f-151">En CDN-profil är en samling CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7b06f-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="7b06f-152">Varje profil innehåller en eller flera CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7b06f-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="7b06f-153">Du kanske vill använda flera profiler för att organisera dina CDN-slutpunkter efter Internetdomän, webbapp eller andra kriterier.</span><span class="sxs-lookup"><span data-stu-id="7b06f-153">You may wish to use multiple profiles to organize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="7b06f-154">Om du redan har en CDN-profil som du vill använda för den här självstudiekursen, fortsätter du till [skapa en ny CDN-slutpunkt](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="7b06f-154">If you already have a CDN profile that you want to use for this tutorial, proceed to [Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="7b06f-155">Skapa en ny CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7b06f-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="7b06f-156">**Skapa en ny CDN-slutpunkt för ditt lagringskonto**</span><span class="sxs-lookup"><span data-stu-id="7b06f-156">**To create a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="7b06f-157">I den [Azure-hanteringsportalen](https://portal.azure.com), navigera till CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-157">In the [Azure Management Portal](https://portal.azure.com), navigate to your CDN profile.</span></span>  <span data-ttu-id="7b06f-158">Du kanske fäste den på instrumentpanelen i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="7b06f-158">You may have pinned it to the dashboard in the previous step.</span></span>  <span data-ttu-id="7b06f-159">Om du inte hittar den klicka du på **Bläddra**, sedan på **CDN-profiler** och sedan på den profil som du vill lägga till slutpunkten till.</span><span class="sxs-lookup"><span data-stu-id="7b06f-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on the profile you plan to add your endpoint to.</span></span>
   
    <span data-ttu-id="7b06f-160">Bladet för CDN-profilen visas.</span><span class="sxs-lookup"><span data-stu-id="7b06f-160">The CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="7b06f-162">Klicka på knappen **Lägg till slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-162">Click the **Add Endpoint** button.</span></span>
   
    ![Knappen Lägg till slutpunkt][cdn-new-endpoint-button]
   
    <span data-ttu-id="7b06f-164">Bladet **Lägg till slutpunkt** visas.</span><span class="sxs-lookup"><span data-stu-id="7b06f-164">The **Add an endpoint** blade appears.</span></span>
   
    ![Bladet Lägg till slutpunkt][cdn-add-endpoint]
3. <span data-ttu-id="7b06f-166">Ange ett **namn** för den här CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="7b06f-167">Det här namnet används för att komma åt dina cachelagrade resurser i domänen `<EndpointName>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="7b06f-167">This name will be used to access your cached resources at the domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="7b06f-168">I den **typ av ursprung** listrutan *Molntjänsten*.</span><span class="sxs-lookup"><span data-stu-id="7b06f-168">In the **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="7b06f-169">I den **ursprungsvärdnamn** listrutan, Välj din molntjänst.</span><span class="sxs-lookup"><span data-stu-id="7b06f-169">In the **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="7b06f-170">Lämnar du standardinställningarna för **ursprungssökväg**, **ursprungsvärdadress**, och **protokollet ursprung port**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-170">Leave the defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="7b06f-171">Du måste ange minst ett protokoll (HTTP eller HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7b06f-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="7b06f-172">Klicka på knappen **Lägg till** för att skapa en ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7b06f-172">Click the **Add** button to create the new endpoint.</span></span>
8. <span data-ttu-id="7b06f-173">När slutpunkten har skapats visas den i en lista över slutpunkter för profilen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-173">Once the endpoint is created, it appears in a list of endpoints for the profile.</span></span> <span data-ttu-id="7b06f-174">I listvyn visas bara URL:en som ska användas för att komma åt cachelagrat innehåll, samt ursprungsdomänen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-174">The list view shows the URL to use to access cached content, as well as the origin domain.</span></span>
   
    ![CDN-slutpunkt][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="7b06f-176">Slutpunkten blir omedelbart inte tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="7b06f-176">The endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="7b06f-177">Det kan ta upp till 90 minuter för registreringen ska spridas via nätverket CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-177">It can take up to 90 minutes for the registration to propagate through the CDN network.</span></span> <span data-ttu-id="7b06f-178">Användare som försöker använda domännamnet CDN direkt får statuskod 404 tills innehållet är tillgängligt via CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-178">Users who try to use the CDN domain name immediately may receive status code 404 until the content is available via the CDN.</span></span>
   > 
   > 

## <a name="test-the-cdn-endpoint"></a><span data-ttu-id="7b06f-179">Testa CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="7b06f-179">Test the CDN endpoint</span></span>
<span data-ttu-id="7b06f-180">När Publiceringsstatus är **slutförd**, öppna ett webbläsarfönster och navigera till  **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-180">When the publishing status is **Completed**, open a browser window and navigate to **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="7b06f-181">URL: en är i min installationsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="7b06f-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="7b06f-182">Som motsvarar ursprung följande URL i CDN-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="7b06f-182">Which corresponds to the following origin URL at the CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="7b06f-183">När du navigerar till  **http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, beroende på din webbläsare, uppmanas du att hämta eller öppna bootstrap.css som följde från ditt publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7b06f-183">When you navigate to **http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted to download or open the bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="7b06f-184">På samma sätt kan du komma åt valfri offentligt tillgänglig URL på  **http://*&lt;serviceName >*.cloudapp.net/** direkt från din CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="7b06f-185">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7b06f-185">For example:</span></span>

* <span data-ttu-id="7b06f-186">En .js-fil i sökvägen/Script</span><span class="sxs-lookup"><span data-stu-id="7b06f-186">A .js file from the /Script path</span></span>
* <span data-ttu-id="7b06f-187">Varje innehållsfil från /Content sökväg</span><span class="sxs-lookup"><span data-stu-id="7b06f-187">Any content file from the /Content path</span></span>
* <span data-ttu-id="7b06f-188">En domänkontrollant/åtgärd</span><span class="sxs-lookup"><span data-stu-id="7b06f-188">Any controller/action</span></span>
* <span data-ttu-id="7b06f-189">Om frågesträngen är aktiverad på en URL med frågesträngar din CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="7b06f-189">If the query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="7b06f-190">Med konfigurationen ovan kan du i själva verket värd hela Molntjänsten från  **http://*&lt;cdnName >*.azureedge.net/**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-190">In fact, with the above configuration, you can host the entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**.</span></span> <span data-ttu-id="7b06f-191">Om jag navigerar till **http://camservice.azureedge.net/**, jag åtgärd resultatet från Home-Index.</span><span class="sxs-lookup"><span data-stu-id="7b06f-191">If I navigate to **http://camservice.azureedge.net/**, I get the action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="7b06f-192">Detta innebär inte, men att det alltid är en bra idé att hantera en hela molntjänst via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-192">This does not mean, however, that it's always a good idea to serve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="7b06f-193">En CDN med statiska leveransoptimering inte nödvändigtvis snabba upp överföringen av dynamisk tillgångar som inte är avsedda att cachelagras eller uppdateras väldigt ofta eftersom CDN måste hämta en ny version av tillgången från den ursprungliga servern ofta.</span><span class="sxs-lookup"><span data-stu-id="7b06f-193">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant to be cached, or are updated very frequently, since the CDN must pull a new version of the asset from the Origin server very often.</span></span> <span data-ttu-id="7b06f-194">I det här scenariot kan du aktivera [dynamiska plats Acceleration](cdn-dynamic-site-acceleration.md) optimering (DSA) på din CDN-slutpunkt som använder olika metoder för att påskynda överföringen av ej Cacheable ställs dynamiska tillgångar.</span><span class="sxs-lookup"><span data-stu-id="7b06f-194">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques to speed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="7b06f-195">Om du har en plats med en blandning av statiska och dynamiska innehåll kan välja du att hantera din statiskt innehåll från CDN med en statisk optimering typ (till exempel Internet leverans) och för att hantera dynamiskt innehåll direkt från den ursprungliga servern eller via ett CDN-slutpunkten w : te DSA optimering aktiverat-fall till fall.</span><span class="sxs-lookup"><span data-stu-id="7b06f-195">If you have a site with a mix of static and dynamic content, you can choose to serve your static content from CDN with a static optimization type (such as general web delivery), and to serve dynamic content either directly from the origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="7b06f-196">Du har redan sett hur du kommer åt enskilda innehållsfiler från CDN-slutpunkten för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="7b06f-196">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="7b06f-197">Jag visas hur du har en specifik domänkontrollant åtgärd via en viss CDN-slutpunkt i fungera innehåll från domänkontrollanten åtgärder via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-197">I will show you how to serve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="7b06f-198">Alternativt är att avgöra vilket innehåll som fungerar från Azure CDN för fall till fall i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-198">The alternative is to determine which content to serve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="7b06f-199">Du har redan sett hur du kommer åt enskilda innehållsfiler från CDN-slutpunkten för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="7b06f-199">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="7b06f-200">Jag visar hur att hantera en specifik domänkontrollant åtgärd via CDN-slutpunkten i [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller).</span><span class="sxs-lookup"><span data-stu-id="7b06f-200">I will show you how to serve a specific controller action through the CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="7b06f-201">Konfigurera cachelagringsalternativ för statiska filer i Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="7b06f-201">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="7b06f-202">Du kan ange hur du vill att statiskt innehåll cachelagras i CDN-slutpunkten med Azure CDN-integration i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-202">With Azure CDN integration in your cloud service, you can specify how you want static content to be cached in the CDN endpoint.</span></span> <span data-ttu-id="7b06f-203">Det gör du genom att öppna *Web.config* från din webbroll projekt (t.ex. WebRole1) och Lägg till en `<staticContent>` elementet så att `<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="7b06f-203">To do this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element to `<system.webServer>`.</span></span> <span data-ttu-id="7b06f-204">XML-koden nedan konfigurerar cache för att gå ut inom tre dagar.</span><span class="sxs-lookup"><span data-stu-id="7b06f-204">The XML below configures the cache to expire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="7b06f-205">När du gör detta kommer alla statiska filer i din molntjänst se samma regel i CDN-cachen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-205">Once you do this, all static files in your cloud service will observe the same rule in your CDN cache.</span></span> <span data-ttu-id="7b06f-206">För mer detaljerad kontroll över inställningar för cachelagring för att lägga till en *Web.config* till en mapp och Lägg till dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="7b06f-206">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="7b06f-207">Till exempel lägga till en *Web.config* filen till den *\Content* mapp och Ersätt innehållet med följande XML-filen:</span><span class="sxs-lookup"><span data-stu-id="7b06f-207">For example, add a *Web.config* file to the *\Content* folder and replace the content with the following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="7b06f-208">Den här inställningen gör att alla statiska filer från den *\Content* mappen cachelagras för 15 dagar.</span><span class="sxs-lookup"><span data-stu-id="7b06f-208">This setting causes all static files from the *\Content* folder to be cached for 15 days.</span></span>

<span data-ttu-id="7b06f-209">Mer information om hur du konfigurerar den `<clientCache>` element, se [klientcachen &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="7b06f-209">For more information on how to configure the `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="7b06f-210">I [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller), jag visas också hur du kan konfigurera inställningar för cachelagring för domänkontrollant åtgärden resulterar i CDN-cachen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-210">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in the CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="7b06f-211">Hantera innehåll från domänkontrollanten åtgärder via Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-211">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="7b06f-212">Det är relativt enkelt att hantera innehåll från domänkontrollanten åtgärder via Azure CDN när du integrerar en cloud service-webbroll med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-212">When you integrate a cloud service Web role with Azure CDN, it is relatively easy to serve content from controller actions through the Azure CDN.</span></span> <span data-ttu-id="7b06f-213">Tjänsten direkt via Azure CDN (som visas ovan) än betjänar ditt moln [Maarten Balliauw](https://twitter.com/maartenballiauw) visar hur du gör det med ett roligt MemeGenerator domänkontrollant i [minskar svarstiden på webben med Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="7b06f-213">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how to do it with a fun MemeGenerator controller in [Reducing latency on the web with the Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="7b06f-214">Jag kommer att återskapa den här.</span><span class="sxs-lookup"><span data-stu-id="7b06f-214">I will simply reproduce it here.</span></span>

<span data-ttu-id="7b06f-215">Anta att i ditt moln tjänsten som du vill generera memes baserat på ett barn Chuck Norris bild (foto av [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) så här:</span><span class="sxs-lookup"><span data-stu-id="7b06f-215">Suppose in your cloud service you want to generate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="7b06f-216">Du har en enkel `Index` åtgärd som gör att kunder kan ange superlativ i bilden, sedan genererar meme när de gör ett inlägg till åtgärden.</span><span class="sxs-lookup"><span data-stu-id="7b06f-216">You have a simple `Index` action that allows the customers to specify the superlatives in the image, then generates the meme once they post to the action.</span></span> <span data-ttu-id="7b06f-217">Eftersom det är Chuck Norris förväntar du dig den här sidan ska bli wildly populära globalt.</span><span class="sxs-lookup"><span data-stu-id="7b06f-217">Since it's Chuck Norris, you would expect this page to become wildly popular globally.</span></span> <span data-ttu-id="7b06f-218">Detta är ett bra exempel på betjänar delvis dynamiskt innehåll med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-218">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="7b06f-219">Följ stegen ovan för att konfigurera den här åtgärden för domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="7b06f-219">Follow the steps above to setup this controller action:</span></span>

1. <span data-ttu-id="7b06f-220">I den *\Controllers* mapp, skapa en ny .cs-fil som kallas *MemeGeneratorController.cs* och Ersätt innehållet med följande kod.</span><span class="sxs-lookup"><span data-stu-id="7b06f-220">In the *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace the content with the following code.</span></span> <span data-ttu-id="7b06f-221">Se till att ersätta den markerade delen med CDN-namn.</span><span class="sxs-lookup"><span data-stu-id="7b06f-221">Be sure to replace the highlighted portion with your CDN name.</span></span>  
   
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
   
                    if (Debugger.IsAttached) // Preserve the debug experience
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
2. <span data-ttu-id="7b06f-222">Högerklicka i standard `Index()` och väljer **Lägg till vy**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-222">Right-click in the default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="7b06f-223">Godkänner inställningarna nedan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7b06f-223">Accept the settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="7b06f-224">Öppna den nya *Views\MemeGenerator\Index.cshtml* och Ersätt innehållet med följande enkel HTML-koden för att skicka superlativ:</span><span class="sxs-lookup"><span data-stu-id="7b06f-224">Open the new *Views\MemeGenerator\Index.cshtml* and replace the content with the following simple HTML for submitting the superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="7b06f-225">Publicera Molntjänsten igen och gå till  **http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="7b06f-225">Publish the cloud service again and navigate to **http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="7b06f-226">När du skickar formulärvärden till `/MemeGenerator/Index`, `Index_Post` åtgärdsmetod returnerar en länk till den `Show` åtgärdsmetod med respektive inkommande identifierare.</span><span class="sxs-lookup"><span data-stu-id="7b06f-226">When you submit the form values to `/MemeGenerator/Index`, the `Index_Post` action method returns a link to the `Show` action method with the respective input identifier.</span></span> <span data-ttu-id="7b06f-227">När du klickar på länken kommer du till följande kod:</span><span class="sxs-lookup"><span data-stu-id="7b06f-227">When you click the link, you reach the following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="7b06f-228">Om din lokala felsökare, får du regelbundet debug-upplevelse med en lokal omdirigering.</span><span class="sxs-lookup"><span data-stu-id="7b06f-228">If your local debugger is attached, then you will get the regular debug experience with a local redirect.</span></span> <span data-ttu-id="7b06f-229">Om den körs i Molntjänsten ska den omdirigera till:</span><span class="sxs-lookup"><span data-stu-id="7b06f-229">If it's running in the cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="7b06f-230">Som motsvarar följande ursprung URL på CDN-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="7b06f-230">Which corresponds to the following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="7b06f-231">Du kan sedan använda den `OutputCacheAttribute` attributet för den `Generate` metod för att ange hur åtgärden resultatet ska cachelagras, som hanterar Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-231">You can then use the `OutputCacheAttribute` attribute on the `Generate` method to specify how the action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="7b06f-232">Koden nedan ange en giltighetstid för cache 1 timme (3 600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="7b06f-232">The code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="7b06f-233">På samma sätt kan du hantera innehåll från något domänkontrollant i Molntjänsten via din Azure CDN med alternativet för önskad cachelagring.</span><span class="sxs-lookup"><span data-stu-id="7b06f-233">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with the desired caching option.</span></span>

<span data-ttu-id="7b06f-234">I nästa avsnitt hur I du hantera paketerade och minified skript och CSS via Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-234">In the next section, I will show you how to serve the bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="7b06f-235">Integrera ASP.NET paketering och minification med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-235">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="7b06f-236">Skript och CSS matmallar ändras sällan och är särskilda kandidater för Azure CDN-cachen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-236">Scripts and CSS stylesheets change infrequently and are prime candidates for the Azure CDN cache.</span></span> <span data-ttu-id="7b06f-237">Betjänar hela webbrollen via Azure CDN är det enklaste sättet att integrera paketering och minification med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-237">Serving the entire Web role through your Azure CDN is the easiest way to integrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="7b06f-238">Dock som du inte kanske vill göra detta, visas I hur du göra det när bevarar önskade develper upplevelsen av ASP.NET paketering och minification, som:</span><span class="sxs-lookup"><span data-stu-id="7b06f-238">However, as you may not want to do this, I will show you how to do it while preserving the desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="7b06f-239">Bra debug problem</span><span class="sxs-lookup"><span data-stu-id="7b06f-239">Great debug mode experience</span></span>
* <span data-ttu-id="7b06f-240">Effektiv distribution</span><span class="sxs-lookup"><span data-stu-id="7b06f-240">Streamlined deployment</span></span>
* <span data-ttu-id="7b06f-241">Omedelbar uppdateringar till klienter för skriptet/CSS versionsuppgraderingar</span><span class="sxs-lookup"><span data-stu-id="7b06f-241">Immediate updates to clients for script/CSS version upgrades</span></span>
* <span data-ttu-id="7b06f-242">Fallback mekanism när CDN-slutpunkten misslyckas</span><span class="sxs-lookup"><span data-stu-id="7b06f-242">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="7b06f-243">Minimera kodändring</span><span class="sxs-lookup"><span data-stu-id="7b06f-243">Minimize code modification</span></span>

<span data-ttu-id="7b06f-244">I den **WebRole1** projekt som du skapade i [integrera Azure CDN-slutpunkten med din Azure-webbplatsen och hantera statiskt innehåll i webbsidor från Azure CDN](#deploy)öppnar *App_Start\ BundleConfig.cs* och ta en titt på den `bundles.Add()` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="7b06f-244">In the **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at the `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="7b06f-245">Först `bundles.Add()` instruktionen lägger till ett skript-paket på den virtuella katalogen `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="7b06f-245">The first `bundles.Add()` statement adds a script bundle at the virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="7b06f-246">Öppna *Views\Shared\_Layout.cshtml* att se hur paket skripttypen återges.</span><span class="sxs-lookup"><span data-stu-id="7b06f-246">Then, open *Views\Shared\_Layout.cshtml* to see how the script bundle tag is rendered.</span></span> <span data-ttu-id="7b06f-247">Du ska kunna hitta Razor följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="7b06f-247">You should be able to find the following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="7b06f-248">När den här Razor-koden körs i Azure-webbroll blir en `<script>` tagg för paket-skriptet liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7b06f-248">When this Razor code is run in the Azure Web role, it will render a `<script>` tag for the script bundle similar to the following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="7b06f-249">Men när det körs i Visual Studio genom att skriva `F5`, den kommer att visas varje skriptfilen i paketet individuellt (i fallet endast en skriptfil finns i paketet):</span><span class="sxs-lookup"><span data-stu-id="7b06f-249">However, when it is run in Visual Studio by typing `F5`, it will render each script file in the bundle individually (in the case above, only one script file is in the bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="7b06f-250">På så sätt kan du felsöka JavaScript-kod i din utvecklingsmiljö medan vilket minskar antalet samtidiga klientanslutningar (paketering) och förbättra filen hämta prestanda (minification) i produktion.</span><span class="sxs-lookup"><span data-stu-id="7b06f-250">This enables you to debug the JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="7b06f-251">Det är en bra funktion för att bevara med Azure CDN-integration.</span><span class="sxs-lookup"><span data-stu-id="7b06f-251">It's a great feature to preserve with Azure CDN integration.</span></span> <span data-ttu-id="7b06f-252">Dessutom eftersom renderade paketet innehåller redan en automatiskt genererad versionssträng måste du vill replikera funktionen därför när du uppdaterar din jQuery version via NuGet den kan uppdateras på klientsidan så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="7b06f-252">Furthermore, since the rendered bundle already contains an automatically generated version string, you want to replicate that functionality so the whenever you update your jQuery version through NuGet, it can be updated at the client side as soon as possible.</span></span>

<span data-ttu-id="7b06f-253">Följ stegen nedan för att integration ASP.NET paketering och minification med CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-253">Follow the steps below to integration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="7b06f-254">Tillbaka i *App_Start\BundleConfig.cs*, ändra den `bundles.Add()` metoder för att använda en annan [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), som anger en CDN-adress.</span><span class="sxs-lookup"><span data-stu-id="7b06f-254">Back in *App_Start\BundleConfig.cs*, modify the `bundles.Add()` methods to use a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="7b06f-255">Om du vill göra detta måste du ersätta den `RegisterBundles` metoddefinition med följande kod:</span><span class="sxs-lookup"><span data-stu-id="7b06f-255">To do this, replace the `RegisterBundles` method definition with the following code:</span></span>  
   
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
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="7b06f-256">Se till att ersätta `<yourCDNName>` med namnet på Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-256">Be sure to replace `<yourCDNName>` with the name of your Azure CDN.</span></span>
   
    <span data-ttu-id="7b06f-257">Inställningen i klartext, `bundles.UseCdn = true` och lagt till en noggrant utformad CDN-URL i varje paket.</span><span class="sxs-lookup"><span data-stu-id="7b06f-257">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL to each bundle.</span></span> <span data-ttu-id="7b06f-258">Till exempel första konstruktorn i koden:</span><span class="sxs-lookup"><span data-stu-id="7b06f-258">For example, the first constructor in the code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="7b06f-259">är samma som:</span><span class="sxs-lookup"><span data-stu-id="7b06f-259">is the same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="7b06f-260">Den här konstruktorn visar ASP.NET paketering och minification att återge enskilda filer när felsöks lokalt men använda den angivna CDN-adressen till skriptet i fråga.</span><span class="sxs-lookup"><span data-stu-id="7b06f-260">This constructor tells ASP.NET bundling and minification to render individual script files when debugged locally, but use the specified CDN address to access the script in question.</span></span> <span data-ttu-id="7b06f-261">Observera dock två viktiga egenskaper med den här noggrant utformad CDN-URL:</span><span class="sxs-lookup"><span data-stu-id="7b06f-261">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="7b06f-262">Ursprung för denna CDN-URL är `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vilket är att den virtuella katalogen för skript-paket i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-262">The origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually the virtual directory of the script bundle in your cloud service.</span></span>
   * <span data-ttu-id="7b06f-263">Eftersom du använder en CDN-konstruktorn innehåller skripttypen CDN för paket inte längre skapas automatiskt Versionsträngen i den återgivna URL: en.</span><span class="sxs-lookup"><span data-stu-id="7b06f-263">Since you are using CDN constructor, the CDN script tag for the bundle no longer contains the automatically generated version string in the rendered URL.</span></span> <span data-ttu-id="7b06f-264">Manuellt måste du generera en unik versionssträng varje gång skriptet paketet ändras för att tvinga en cache-miss på Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b06f-264">You must manually generate a unique version string every time the script bundle is modified to force a cache miss at your Azure CDN.</span></span> <span data-ttu-id="7b06f-265">Unik versionssträngen måste vara konstant via livslängd för distributionen för att maximera cacheträffar vid Azure CDN när paketet har distribuerats på samma gång.</span><span class="sxs-lookup"><span data-stu-id="7b06f-265">At the same time, this unique version string must remain constant through the life of the deployment to maximize cache hits at your Azure CDN after the bundle is deployed.</span></span>
   * <span data-ttu-id="7b06f-266">Frågesträngen v = < W.X.Y.Z > hämtar från *Properties\AssemblyInfo.cs* i din webbrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="7b06f-266">The query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="7b06f-267">Du kan ha ett arbetsflöde för distribution som innehåller ökar Sammansättningsversionen varje gång du publicerar till Azure.</span><span class="sxs-lookup"><span data-stu-id="7b06f-267">You can have a deployment workflow that includes incrementing the assembly version every time you publish to Azure.</span></span> <span data-ttu-id="7b06f-268">Eller, du kan bara ändra *Properties\AssemblyInfo.cs* i projektet att räkna upp Versionsträngen automatiskt varje gång du skapar kan använda jokertecknet ' *'.</span><span class="sxs-lookup"><span data-stu-id="7b06f-268">Or, you can just modify *Properties\AssemblyInfo.cs* in your project to automatically increment the version string every time you build, using the wildcard character '*'.</span></span> <span data-ttu-id="7b06f-269">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7b06f-269">For example:</span></span>
     
        <span data-ttu-id="7b06f-270">[sammansättningen: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="7b06f-270">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="7b06f-271">Andra strategi att förenkla Generera en unik sträng för livslängd för en distribution som fungerar här.</span><span class="sxs-lookup"><span data-stu-id="7b06f-271">Any other strategy to streamline generating a unique string for the life of a deployment will work here.</span></span>
2. <span data-ttu-id="7b06f-272">Publicera Molntjänsten och gå till hemsidan.</span><span class="sxs-lookup"><span data-stu-id="7b06f-272">Republish the cloud service and access the home page.</span></span>
3. <span data-ttu-id="7b06f-273">Visa HTML-koden för sidan.</span><span class="sxs-lookup"><span data-stu-id="7b06f-273">View the HTML code for the page.</span></span> <span data-ttu-id="7b06f-274">Du ska kunna se CDN-URL som renderas med en unik versionssträng varje gång du publicerar ändringarna på Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-274">You should be able to see the CDN URL rendered, with a unique version string every time you republish changes to your cloud service.</span></span> <span data-ttu-id="7b06f-275">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7b06f-275">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="7b06f-276">I Visual Studio debug Molntjänsten i Visual Studio genom att skriva `F5`.,</span><span class="sxs-lookup"><span data-stu-id="7b06f-276">In Visual Studio, debug the cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="7b06f-277">Visa HTML-koden för sidan.</span><span class="sxs-lookup"><span data-stu-id="7b06f-277">View the HTML code for the page.</span></span> <span data-ttu-id="7b06f-278">Du kommer fortfarande att se varje skriptfilen individuellt återges så att du får en konsekvent debug upplevelse i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b06f-278">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
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

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="7b06f-279">Fallback mekanism för CDN-URL: er</span><span class="sxs-lookup"><span data-stu-id="7b06f-279">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="7b06f-280">När Azure CDN-slutpunkten misslyckas av någon anledning, vill webbsidan vara smart att komma åt webbservern ursprung som återställningsalternativ för inläsning av JavaScript eller starttjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b06f-280">When your Azure CDN endpoint fails for any reason, you want your Web page to be smart enough to access your origin Web server as the fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="7b06f-281">Det är tillräckligt allvarligt för att förlora bilder på webbplatsen eftersom det inte CDN finns, men mycket allvarligare förlorar avgörande sidan funktionalitet som tillhandahålls av skripten och formatmallar.</span><span class="sxs-lookup"><span data-stu-id="7b06f-281">It's serious enough to lose images on your website due to CDN unavailability, but much more severe to lose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="7b06f-282">Den [paket](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) klassen innehåller en egenskap som kallas [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) som gör att du kan konfigurera återställningsplats mekanismen för CDN-fel.</span><span class="sxs-lookup"><span data-stu-id="7b06f-282">The [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you to configure the fallback mechanism for CDN failure.</span></span> <span data-ttu-id="7b06f-283">Följ stegen nedan om du vill använda den här egenskapen:</span><span class="sxs-lookup"><span data-stu-id="7b06f-283">To use this property, follow the steps below:</span></span>

1. <span data-ttu-id="7b06f-284">Öppna i din webbrollsprojektet *App_Start\BundleConfig.cs*, där du lade till en CDN-URL i varje [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), och gör följande markerade ändringar att lägga till återställningsplats mekanism till standardinställningarna paket:</span><span class="sxs-lookup"><span data-stu-id="7b06f-284">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make the following highlighted changes to add fallback mechanism to the default bundles:</span></span>  
   
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
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
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
   
    <span data-ttu-id="7b06f-285">När `CdnFallbackExpression` är null, skript är injekteras i HTML för att testa om paketet har lästs in och, om inte, komma åt paketet direkt från webbservern ursprung.</span><span class="sxs-lookup"><span data-stu-id="7b06f-285">When `CdnFallbackExpression` is not null, script is injected into the HTML to test whether the bundle is loaded successfully and, if not, access the bundle directly from the origin Web server.</span></span> <span data-ttu-id="7b06f-286">Den här egenskapen måste anges till ett JavaScript-uttryck testar om respektive CDN-paketet har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="7b06f-286">This property needs to be set to a JavaScript expression that tests whether the respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="7b06f-287">Det uttryck som behövs för att testa varje paket skiljer sig enligt innehållet.</span><span class="sxs-lookup"><span data-stu-id="7b06f-287">The expression needed to test each bundle differs according to the content.</span></span> <span data-ttu-id="7b06f-288">För standard paket ovan:</span><span class="sxs-lookup"><span data-stu-id="7b06f-288">For the default bundles above:</span></span>
   
   * <span data-ttu-id="7b06f-289">`window.jquery`har definierats i jquery-{version} .js</span><span class="sxs-lookup"><span data-stu-id="7b06f-289">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="7b06f-290">`$.validator`har definierats i jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="7b06f-290">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="7b06f-291">`window.Modernizr`har definierats i modernizer-{version} .js</span><span class="sxs-lookup"><span data-stu-id="7b06f-291">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="7b06f-292">`$.fn.modal`har definierats i bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b06f-292">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="7b06f-293">Du kanske har lagt märke till att jag inte har angett CdnFallbackExpression för den `~/Cointent/css` paket.</span><span class="sxs-lookup"><span data-stu-id="7b06f-293">You might have noticed that I did not set CdnFallbackExpression for the `~/Cointent/css` bundle.</span></span> <span data-ttu-id="7b06f-294">Detta beror på att det finns för närvarande en [programfel i System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) som lägger in en `<script>` taggen för återställningsplats CSS i stället för den förväntade `<link>` tagg.</span><span class="sxs-lookup"><span data-stu-id="7b06f-294">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for the fallback CSS instead of the expected `<link>` tag.</span></span>
     
     <span data-ttu-id="7b06f-295">Det är emellertid en bra [Style paket reserv](https://github.com/EmberConsultingGroup/StyleBundleFallback) erbjuds av [Ember samråd grupp](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="7b06f-295">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="7b06f-296">Om du vill använda lösningen för CSS, skapa en ny .cs-fil i din webbrollsprojektet *App_Start* mapp med namnet *StyleBundleExtensions.cs*, och Ersätt innehållet med den [kod från GitHub ](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="7b06f-296">To use the workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with the [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="7b06f-297">I *App_Start\StyleFundleExtensions.cs*, byta namn på namnområdet till din webbroll namn (t.ex. **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="7b06f-297">In *App_Start\StyleFundleExtensions.cs*, rename the namespace to your Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="7b06f-298">Gå tillbaka till `App_Start\BundleConfig.cs` och ändra senaste `bundles.Add` instruktion med följande markerade koden:</span><span class="sxs-lookup"><span data-stu-id="7b06f-298">Go back to `App_Start\BundleConfig.cs` and modify the last `bundles.Add` statement with the following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="7b06f-299">Den här nya tilläggsmetoden använder samma idé att mata in skriptet i HTML för att kontrollera DOM för den en matchande klassnamn och Regelnamn regeln värdet som definierats i CSS-paket och faller tillbaka till webbservern ursprung om det inte går att hitta matchningen.</span><span class="sxs-lookup"><span data-stu-id="7b06f-299">This new extension method uses the same idea to inject script in the HTML to check the DOM for the a matching class name, rule name, and rule value defined in the CSS bundle, and falls back to the origin Web server if it fails to find the match.</span></span>
5. <span data-ttu-id="7b06f-300">Publicera Molntjänsten igen och gå till hemsidan.</span><span class="sxs-lookup"><span data-stu-id="7b06f-300">Publish the cloud service again and access the home page.</span></span>
6. <span data-ttu-id="7b06f-301">Visa HTML-koden för sidan.</span><span class="sxs-lookup"><span data-stu-id="7b06f-301">View the HTML code for the page.</span></span> <span data-ttu-id="7b06f-302">Du ska hitta inmatat skript liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7b06f-302">You should find injected scripts similar to the following:</span></span>    
   
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

    <span data-ttu-id="7b06f-303">Observera att inmatat skript för paket-CSS fortfarande innehåller felaktiga kvarleva från den `CdnFallbackExpression` egenskapen i rad:</span><span class="sxs-lookup"><span data-stu-id="7b06f-303">Note that injected script for the CSS bundle still contains the errant remnant from the `CdnFallbackExpression` property in the line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="7b06f-304">Men eftersom den första delen av den. uttrycket returnerar alltid true (på rad ovanför som), funktionen document.write() aldrig ska köras.</span><span class="sxs-lookup"><span data-stu-id="7b06f-304">But since the first part of the || expression will always return true (in the line directly above that), the document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="7b06f-305">Mer information</span><span class="sxs-lookup"><span data-stu-id="7b06f-305">More Information</span></span>
* [<span data-ttu-id="7b06f-306">Översikt över Azure Content Delivery Network (CDN)</span><span class="sxs-lookup"><span data-stu-id="7b06f-306">Overview of the Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="7b06f-307">Med hjälp av Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b06f-307">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="7b06f-308">ASP.NET paketering och Minification</span><span class="sxs-lookup"><span data-stu-id="7b06f-308">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
