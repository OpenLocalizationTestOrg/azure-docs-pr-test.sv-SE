---
title: "aaaCreate din första Java-webbapp i Azure"
description: "Lär dig hur toorun webbappar i App Service genom att distribuera en grundläggande Java-app."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="8db27-103">Skapa din första Java-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="8db27-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="8db27-104">Hej [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) funktion i [Azure App Service](../app-service/app-service-value-prop-what-is.md) ger en mycket skalbar, automatisk uppdatering värdtjänst.</span><span class="sxs-lookup"><span data-stu-id="8db27-104">hello [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="8db27-105">Den här snabbstarten visar hur toodeploy en Java web app tooApp tjänsten med hjälp av hello [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="8db27-105">This quickstart shows how toodeploy a Java web app tooApp Service by using hello [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

!["Hello Azure!"](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="8db27-108">Krav</span><span class="sxs-lookup"><span data-stu-id="8db27-108">Prerequisites</span></span>

<span data-ttu-id="8db27-109">toocomplete Snabbstart, installera:</span><span class="sxs-lookup"><span data-stu-id="8db27-109">toocomplete this quickstart, install:</span></span>

* <span data-ttu-id="8db27-110">hello ledigt [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8db27-110">hello free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="8db27-111">Den här snabbstarten använder Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="8db27-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="8db27-112">Hej [Azure Toolkit för Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="8db27-112">hello [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="8db27-113">Skapa ett dynamiskt webbprojekt i Eclipse</span><span class="sxs-lookup"><span data-stu-id="8db27-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="8db27-114">Välj **Arkiv** > **Nytt** > **Dynamic Web Project** (Dynamiskt webbprojekt) Eclipse.</span><span class="sxs-lookup"><span data-stu-id="8db27-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="8db27-115">I hello **nya dynamiskt webbprojekt** dialogrutan, namnet hello projekt **MyFirstJavaOnAzureWebApp**, och välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8db27-115">In hello **New Dynamic Web Project** dialog box, name hello project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![Dialogrutan för nytt dynamiskt webbprojekt](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="8db27-117">Lägg till en JSP-sida</span><span class="sxs-lookup"><span data-stu-id="8db27-117">Add a JSP page</span></span>

<span data-ttu-id="8db27-118">Om projektutforskaren inte visas kan du återställa den.</span><span class="sxs-lookup"><span data-stu-id="8db27-118">If Project Explorer is not displayed, restore it.</span></span>

![Java EE-arbetsytan för Eclipse](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="8db27-120">Expandera hello i Projektutforskaren **MyFirstJavaOnAzureWebApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="8db27-120">In Project Explorer, expand hello **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="8db27-121">Högerklicka på **Webbinnehåll** och välj sedan **Ny** > **JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="8db27-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Menyn för en ny JSP-fil i projektutforskaren](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="8db27-123">I hello **ny JSP-fil** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="8db27-123">In hello **New JSP File** dialog box:</span></span>

* <span data-ttu-id="8db27-124">Namnet hello filen **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="8db27-124">Name hello file **index.jsp**.</span></span>
* <span data-ttu-id="8db27-125">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8db27-125">Select **Finish**.</span></span>

  ![Dialogruta för en ny JSP-fil](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="8db27-127">Ersätt hello i hello index.jsp-filen, `<body></body>` element med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="8db27-127">In hello index.jsp file, replace hello `<body></body>` element with hello following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="8db27-128">Spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="8db27-128">Save hello changes.</span></span>

## <a name="publish-hello-web-app-tooazure"></a><span data-ttu-id="8db27-129">Publicera hello web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="8db27-129">Publish hello web app tooAzure</span></span>

<span data-ttu-id="8db27-130">Högerklicka på hello-projekt i Projektutforskaren, och välj sedan **Azure** > **Publicera som Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="8db27-130">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![Publicera som Azure Web App (snabbmeny)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="8db27-132">I hello **Azure logga In** dialogrutan, Behåll hello **interaktiv** alternativet och välj sedan **logga in**.</span><span class="sxs-lookup"><span data-stu-id="8db27-132">In hello **Azure Sign In** dialog box, keep hello **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="8db27-133">Följ instruktionerna för hello-inloggning.</span><span class="sxs-lookup"><span data-stu-id="8db27-133">Follow hello sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="8db27-134">Dialogrutan Distribuera webbapp</span><span class="sxs-lookup"><span data-stu-id="8db27-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="8db27-135">När du har loggat in tooyour Azure-konto hello **distribuera webbprogram** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="8db27-135">After you have signed in tooyour Azure account, hello **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="8db27-136">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8db27-136">Select **Create**.</span></span>

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="8db27-138">Dialogrutan Skapa App Service</span><span class="sxs-lookup"><span data-stu-id="8db27-138">Create App Service dialog box</span></span>

<span data-ttu-id="8db27-139">Hej **skapa App Service** dialogruta med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="8db27-139">hello **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="8db27-140">Hej nummer **170602185241** visas i följande hello bilden är olika i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8db27-140">hello number **170602185241** shown in hello following image is different in your dialog box.</span></span>

![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="8db27-142">I hello **skapa App Service** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="8db27-142">In hello **Create App Service** dialog box:</span></span>

* <span data-ttu-id="8db27-143">Behåll hello genererade namnet för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8db27-143">Keep hello generated name for hello web app.</span></span> <span data-ttu-id="8db27-144">Användarnamnet måste vara unikt inom Azure.</span><span class="sxs-lookup"><span data-stu-id="8db27-144">This name must be unique across Azure.</span></span> <span data-ttu-id="8db27-145">hello namn är en del av hello URL-adressen för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8db27-145">hello name is part of hello URL address for hello web app.</span></span> <span data-ttu-id="8db27-146">Exempel: om hello webbprogrammets namn är **MyJavaWebApp**, hello URL: en är *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="8db27-146">For example: if hello web app name is **MyJavaWebApp**, hello URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="8db27-147">Behåll hello standardbehållaren för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8db27-147">Keep hello default web container.</span></span>
* <span data-ttu-id="8db27-148">Välj en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8db27-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="8db27-149">På hello **apptjänstplan** fliken:</span><span class="sxs-lookup"><span data-stu-id="8db27-149">On hello **App service plan** tab:</span></span>

  * <span data-ttu-id="8db27-150">**Skapa en ny**: Behåll hello standard, vilket är hello namnet på hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="8db27-150">**Create new**: Keep hello default, which is hello name of hello App Service plan.</span></span>
  * <span data-ttu-id="8db27-151">**Plats**: Välj **Europa, västra** eller en plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="8db27-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="8db27-152">**Prisnivån**: Välj hello kostnadsfritt alternativ.</span><span class="sxs-lookup"><span data-stu-id="8db27-152">**Pricing tier**: Select hello free option.</span></span> <span data-ttu-id="8db27-153">Se [Priser för App Service](https://azure.microsoft.com/pricing/details/app-service/) för funktioner.</span><span class="sxs-lookup"><span data-stu-id="8db27-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="8db27-155">Flik för resursgrupp</span><span class="sxs-lookup"><span data-stu-id="8db27-155">Resource group tab</span></span>

<span data-ttu-id="8db27-156">Välj hello **resursgruppen** fliken. Behåll hello genereras standardvärdet för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8db27-156">Select hello **Resource group** tab. Keep hello default generated value for hello resource group.</span></span>

![Flik för resursgrupp](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="8db27-158">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8db27-158">Select **Create**.</span></span>

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="8db27-159">hello Azure Toolkit skapar hello webbprogram och visar en förloppsindikator för.</span><span class="sxs-lookup"><span data-stu-id="8db27-159">hello Azure Toolkit creates hello web app and displays a progress dialog box.</span></span>

![Dialogruta med förloppsindikator för hur apptjänsten skapas](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="8db27-161">Dialogrutan Distribuera webbapp</span><span class="sxs-lookup"><span data-stu-id="8db27-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="8db27-162">I hello **distribuera webbprogram** dialogrutan **distribuera tooroot**.</span><span class="sxs-lookup"><span data-stu-id="8db27-162">In hello **Deploy Web App** dialog box, select **Deploy tooroot**.</span></span> <span data-ttu-id="8db27-163">Om du har en apptjänst vid *wingtiptoys.azurewebsites.net* och du inte distribuera toohello rot, hello webbprogram med namnet **MyFirstJavaOnAzureWebApp** distribueras för *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="8db27-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy toohello root, hello web app named **MyFirstJavaOnAzureWebApp** is deployed too*wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="8db27-165">hello dialogrutan visar hello Azure och JDK web behållaren val.</span><span class="sxs-lookup"><span data-stu-id="8db27-165">hello dialog box shows hello Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="8db27-166">Välj **distribuera** toopublish hello web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8db27-166">Select **Deploy** toopublish hello web app tooAzure.</span></span>

<span data-ttu-id="8db27-167">När hello publiceringen är klar väljer du hello **publicerade** länken i hello **Azure-aktivitetsloggen** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8db27-167">When hello publishing finishes, select hello **Published** link in hello **Azure Activity Log** dialog box.</span></span>

![Dialogrutan för Azure aktivitetsloggen](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="8db27-169">Grattis!</span><span class="sxs-lookup"><span data-stu-id="8db27-169">Congratulations!</span></span> <span data-ttu-id="8db27-170">Din web app tooAzure har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="8db27-170">You have successfully deployed your web app tooAzure.</span></span> 

!["Hello Azure!"](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a><span data-ttu-id="8db27-173">Uppdatera hello webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="8db27-173">Update hello web app</span></span>

<span data-ttu-id="8db27-174">Ändra hello JSP kod tooa olika exempelmeddelande.</span><span class="sxs-lookup"><span data-stu-id="8db27-174">Change hello sample JSP code tooa different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="8db27-175">Spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="8db27-175">Save hello changes.</span></span>

<span data-ttu-id="8db27-176">Högerklicka på hello-projekt i Projektutforskaren, och välj sedan **Azure** > **Publicera som Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="8db27-176">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="8db27-177">Hej **distribuera webbprogram** dialogrutan visas och visar hello app service som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8db27-177">hello **Deploy Web App** dialog box appears and shows hello app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="8db27-178">Välj **distribuera tooroot** varje gång du publicerar.</span><span class="sxs-lookup"><span data-stu-id="8db27-178">Select **Deploy tooroot** each time you publish.</span></span>
>

<span data-ttu-id="8db27-179">Välj hello webbapp och markera **distribuera**, som publicerar hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="8db27-179">Select hello web app and select **Deploy**, which publishes hello changes.</span></span>

<span data-ttu-id="8db27-180">När hello **Publishing** länk visas markerar du den toobrowse toohello webbapp och se hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="8db27-180">When hello **Publishing** link appears, select it toobrowse toohello web app and see hello changes.</span></span>

## <a name="manage-hello-web-app"></a><span data-ttu-id="8db27-181">Hantera hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="8db27-181">Manage hello web app</span></span>

<span data-ttu-id="8db27-182">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toosee hello webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8db27-182">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toosee hello web app that you created.</span></span>

<span data-ttu-id="8db27-183">Välj hello vänstra menyn **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="8db27-183">From hello left menu, select **Resource Groups**.</span></span>

![Portalen navigering tooresource grupper](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="8db27-185">Välj hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8db27-185">Select hello resource group.</span></span> <span data-ttu-id="8db27-186">hello sidan visar hello-resurser som du skapade i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="8db27-186">hello page shows hello resources that you created in this quickstart.</span></span>

![Resursgruppen myResourceGroup](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="8db27-188">Välj hello webbprogram (**webapp 170602193915** i föregående bild hello).</span><span class="sxs-lookup"><span data-stu-id="8db27-188">Select hello web app (**webapp-170602193915** in hello preceding image).</span></span>

<span data-ttu-id="8db27-189">Hej **översikt** visas.</span><span class="sxs-lookup"><span data-stu-id="8db27-189">hello **Overview** page appears.</span></span> <span data-ttu-id="8db27-190">Den här sidan ger dig en överblick över hur hello appen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8db27-190">This page gives you a view of how hello app is doing.</span></span> <span data-ttu-id="8db27-191">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="8db27-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="8db27-192">hello flikar på hello vänster hello sidan visar hello olika konfigurationer som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="8db27-192">hello tabs on hello left side of hello page show hello different configurations that you can open.</span></span> 

![App Service-sidan på Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="8db27-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8db27-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8db27-195">Mappa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="8db27-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
