---
title: aaaCreate en ASP.NET-webbapp i Azure | Microsoft Docs
description: "Lär dig hur toorun web apps i Azure App Service genom att distribuera hello standard ASP.NET web app."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a><span data-ttu-id="fe6ea-103">Skapa en ASP.NET-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="fe6ea-103">Create an ASP.NET web app in Azure</span></span>

<span data-ttu-id="fe6ea-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="fe6ea-105">Den här snabbstarten visar hur toodeploy din första ASP.NET web app tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-105">This quickstart shows how toodeploy your first ASP.NET web app tooAzure Web Apps.</span></span> <span data-ttu-id="fe6ea-106">När du är klar har du en resursgrupp som består av en App Service-plan och en Azure-webbapp med en distribuerad webbapp.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-106">When you're finished, you'll have a resource group that consists of an App Service plan and an Azure web app with a deployed web application.</span></span>

<span data-ttu-id="fe6ea-107">Titta hello video toosee denna Snabbstart i praktiken och följ sedan hello steg själv toopublish din första .NET-app på Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-107">Watch hello video toosee this quickstart in action and then follow hello steps yourself toopublish your first .NET app on Azure.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a><span data-ttu-id="fe6ea-108">Krav</span><span class="sxs-lookup"><span data-stu-id="fe6ea-108">Prerequisites</span></span>

<span data-ttu-id="fe6ea-109">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="fe6ea-109">toocomplete this tutorial:</span></span>

* <span data-ttu-id="fe6ea-110">Installera [Visual Studio 2017](https://www.visualstudio.com/downloads/) med hello följande arbetsbelastningar:</span><span class="sxs-lookup"><span data-stu-id="fe6ea-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
    - <span data-ttu-id="fe6ea-111">**ASP.NET och webbutveckling**</span><span class="sxs-lookup"><span data-stu-id="fe6ea-111">**ASP.NET and web development**</span></span>
    - <span data-ttu-id="fe6ea-112">**Azure Development**</span><span class="sxs-lookup"><span data-stu-id="fe6ea-112">**Azure development**</span></span>

    ![ASP.NET och webbutveckling och Azure Development (under webb och moln)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="fe6ea-114">Skapa en ASP.NET-webbapp</span><span class="sxs-lookup"><span data-stu-id="fe6ea-114">Create an ASP.NET web app</span></span>

<span data-ttu-id="fe6ea-115">Skapa ett nytt projekt i Visual Studio genom att välja **Arkiv > Nytt > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-115">In Visual Studio, create a project by selecting **File > New > Project**.</span></span> 

<span data-ttu-id="fe6ea-116">I hello **nytt projekt** markerar **Visual C# > webb > ASP.NET-webbprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-116">In hello **New Project** dialog, select **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.</span></span>

<span data-ttu-id="fe6ea-117">Namnge programmet hello _myFirstAzureWebApp_, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-117">Name hello application _myFirstAzureWebApp_, and then select **OK**.</span></span>
   
![Dialogrutan Nytt projekt](./media/app-service-web-get-started-dotnet/new-project.png)

<span data-ttu-id="fe6ea-119">Du kan distribuera någon typ av ASP.NET web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-119">You can deploy any type of ASP.NET web app tooAzure.</span></span> <span data-ttu-id="fe6ea-120">För Snabbstart, väljer du hello **MVC** mall, och kontrollera att autentisering har angetts för**ingen autentisering**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-120">For this quickstart, select hello **MVC** template, and make sure authentication is set too**No Authentication**.</span></span>
      
<span data-ttu-id="fe6ea-121">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-121">Select **OK**.</span></span>

![Dialogrutan Nytt ASP.NET-projekt](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

<span data-ttu-id="fe6ea-123">Hello-menyn, Välj **Felsök > Starta utan Debugging** toorun hello webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-123">From hello menu, select **Debug > Start without Debugging** toorun hello web app locally.</span></span>

![Kör appen lokalt](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a><span data-ttu-id="fe6ea-125">Publicera tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe6ea-125">Publish tooAzure</span></span>

<span data-ttu-id="fe6ea-126">I hello **Solution Explorer**, högerklicka på hello **myFirstAzureWebApp** projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-126">In hello **Solution Explorer**, right-click hello **myFirstAzureWebApp** project and select **Publish**.</span></span>

![Publicera från Solution Explorer](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

<span data-ttu-id="fe6ea-128">Se till att **Microsoft Azure App Service** är markerat och välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-128">Make sure that **Microsoft Azure App Service** is selected and select **Publish**.</span></span>

![Publicera från projektöversiktssidan](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

<span data-ttu-id="fe6ea-130">Då öppnas hello **skapa App Service** dialog, som hjälper dig att skapa alla hello Azure resurser toorun hello ASP.NET-webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-130">This opens hello **Create App Service** dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="fe6ea-131">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe6ea-131">Sign in tooAzure</span></span>

<span data-ttu-id="fe6ea-132">I hello **skapa Apptjänst** markerar **Lägg till ett konto**, och logga in tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-132">In hello **Create App Service** dialog, select **Add an account**, and sign in tooyour Azure subscription.</span></span> <span data-ttu-id="fe6ea-133">Om du redan är inloggad önskade väljer hello-konto som innehåller hello prenumeration hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-133">If you're already signed in, select hello account containing hello desired subscription from hello dropdown.</span></span>

> [!NOTE]
> <span data-ttu-id="fe6ea-134">Välj inte **Skapa** ännu om du redan är inloggad.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-134">If you're already signed in, don't select **Create** yet.</span></span>
>
>
   
![Logga in tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a><span data-ttu-id="fe6ea-136">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fe6ea-136">Create a resource group</span></span>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

<span data-ttu-id="fe6ea-137">Nästa för**resursgruppen**väljer **ny**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-137">Next too**Resource Group**, select **New**.</span></span>

<span data-ttu-id="fe6ea-138">Namnet hello resursgruppen **myResourceGroup** och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-138">Name hello resource group **myResourceGroup** and select **OK**.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="fe6ea-139">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="fe6ea-139">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="fe6ea-140">Nästa för**Apptjänstplan**väljer **ny**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-140">Next too**App Service Plan**, select **New**.</span></span> 

<span data-ttu-id="fe6ea-141">I hello **konfigurera Apptjänstplan** dialogrutan Inställningar för hello i hello tabell efter hello skärmbild.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-141">In hello **Configure App Service Plan** dialog, use hello settings in hello table following hello screenshot.</span></span>

![Skapa apptjänstplan](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| <span data-ttu-id="fe6ea-143">Inställning</span><span class="sxs-lookup"><span data-stu-id="fe6ea-143">Setting</span></span> | <span data-ttu-id="fe6ea-144">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="fe6ea-144">Suggested Value</span></span> | <span data-ttu-id="fe6ea-145">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe6ea-145">Description</span></span> |
|-|-|-|
|<span data-ttu-id="fe6ea-146">App Service-plan</span><span class="sxs-lookup"><span data-stu-id="fe6ea-146">App Service Plan</span></span>| <span data-ttu-id="fe6ea-147">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="fe6ea-147">myAppServicePlan</span></span> | <span data-ttu-id="fe6ea-148">Namnet på hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-148">Name of hello App Service plan.</span></span> |
| <span data-ttu-id="fe6ea-149">Plats</span><span class="sxs-lookup"><span data-stu-id="fe6ea-149">Location</span></span> | <span data-ttu-id="fe6ea-150">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="fe6ea-150">West Europe</span></span> | <span data-ttu-id="fe6ea-151">hello datacenter där hello webbprogram finns.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-151">hello datacenter where hello web app is hosted.</span></span> |
| <span data-ttu-id="fe6ea-152">Storlek</span><span class="sxs-lookup"><span data-stu-id="fe6ea-152">Size</span></span> | <span data-ttu-id="fe6ea-153">Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="fe6ea-153">Free</span></span> | <span data-ttu-id="fe6ea-154">[Prisnivån](https://azure.microsoft.com/pricing/details/app-service/) avgör tillgängliga värdfunktioner.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-154">[Pricing tier](https://azure.microsoft.com/pricing/details/app-service/) determines hosting features.</span></span> |

<span data-ttu-id="fe6ea-155">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-155">Select **OK**.</span></span>

## <a name="create-and-publish-hello-web-app"></a><span data-ttu-id="fe6ea-156">Skapa och publicera hello webbapp</span><span class="sxs-lookup"><span data-stu-id="fe6ea-156">Create and publish hello web app</span></span>

<span data-ttu-id="fe6ea-157">I **Webbprogramnamnet**, Skriv ett unikt appnamn (giltiga tecken är `a-z`, `0-9`, och `-`), eller Godkänn hello genereras automatiskt unikt namn.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-157">In **Web App Name**, type a unique app name (valid characters are `a-z`, `0-9`, and `-`), or accept hello automatically generated unique name.</span></span> <span data-ttu-id="fe6ea-158">hello webbadressen hello webbprogrammet `http://<app_name>.azurewebsites.net`, där `<app_name>` är din webbprogrammets namn.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-158">hello URL of hello web app is `http://<app_name>.azurewebsites.net`, where `<app_name>` is your web app name.</span></span>

<span data-ttu-id="fe6ea-159">Välj **skapa** toostart skapar hello Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-159">Select **Create** toostart creating hello Azure resources.</span></span>

![Ange webbappnamn](./media/app-service-web-get-started-dotnet/web-app-name.png)

<span data-ttu-id="fe6ea-161">När hello guiden slutförs kommer den publicerar hello ASP.NET web app tooAzure och sedan startar hello app i hello standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-161">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>

![Publicerad ASP.NET-webbapp i Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

<span data-ttu-id="fe6ea-163">hello webbprogrammets namn anges i hello [skapa och publicera steg](#create-and-publish-the-web-app) används som hello URL-prefixet i hello format `http://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-163">hello web app name specified in hello [create and publish step](#create-and-publish-the-web-app) is used as hello URL prefix in hello format `http://<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="fe6ea-164">Grattis, din ASP.NET-webbapp körs live i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-164">Congratulations, your ASP.NET web app is running live in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="fe6ea-165">Uppdatera hello app och distribuera om</span><span class="sxs-lookup"><span data-stu-id="fe6ea-165">Update hello app and redeploy</span></span>

<span data-ttu-id="fe6ea-166">Från hello **Solution Explorer**öppnar _Views\Home\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-166">From hello **Solution Explorer**, open _Views\Home\Index.cshtml_.</span></span>

<span data-ttu-id="fe6ea-167">Hitta hello `<div class="jumbotron">` HTML-tagg hello övre delen och Ersätt hello hela elementet med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="fe6ea-167">Find hello `<div class="jumbotron">` HTML tag near hello top, and replace hello entire element with hello following code:</span></span>

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

<span data-ttu-id="fe6ea-168">tooredeploy tooAzure, högerklicka på hello **myFirstAzureWebApp** projektet i **Solution Explorer** och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-168">tooredeploy tooAzure, right-click hello **myFirstAzureWebApp** project in **Solution Explorer** and select **Publish**.</span></span>

<span data-ttu-id="fe6ea-169">I hello publicera sidan, Välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-169">In hello publish page, select **Publish**.</span></span>

<span data-ttu-id="fe6ea-170">När publiceringen är klar startar webbläsaren toohello URL: en hello webbprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-170">When publishing completes, Visual Studio launches a browser toohello URL of hello web app.</span></span>

![Uppdaterad ASP.NET-webbapp i Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="fe6ea-172">Hantera hello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="fe6ea-172">Manage hello Azure web app</span></span>

<span data-ttu-id="fe6ea-173">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-173">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app.</span></span>

<span data-ttu-id="fe6ea-174">Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-174">From hello left menu, select **App Services**, and then select hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-dotnet/access-portal.png)

<span data-ttu-id="fe6ea-176">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-176">You see your web app's Overview page.</span></span> <span data-ttu-id="fe6ea-177">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-177">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

<span data-ttu-id="fe6ea-179">hello vänstra menyn innehåller olika sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="fe6ea-179">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="fe6ea-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe6ea-180">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe6ea-181">ASP.NET med SQL Database</span><span class="sxs-lookup"><span data-stu-id="fe6ea-181">ASP.NET with SQL Database</span></span>](app-service-web-tutorial-dotnet-sqldatabase.md)
