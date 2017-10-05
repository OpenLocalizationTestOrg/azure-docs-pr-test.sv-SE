---
title: "Skapa din första Java-webbapp i Azure"
description: "Distribuera en grundläggande Java-app och lär dig hur du kör webbappar i App Service."
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
ms.openlocfilehash: b91b9bde5eb8ea0d7e2196056b635fe54095e748
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="49f8a-103">Skapa din första Java-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="49f8a-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="49f8a-104">[Webb Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)-funktionen i [Azure App Service](../app-service/app-service-value-prop-what-is.md) tillhandahåller en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="49f8a-104">The [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="49f8a-105">Den här snabbstarten visar hur du distribuerar en Java-webbapp till App Service med hjälp av [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="49f8a-105">This quickstart shows how to deploy a Java web app to App Service by using the [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

!["Hello Azure!"](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="49f8a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="49f8a-108">Prerequisites</span></span>

<span data-ttu-id="49f8a-109">Installera följande för att slutföra den här snabbstarten:</span><span class="sxs-lookup"><span data-stu-id="49f8a-109">To complete this quickstart, install:</span></span>

* <span data-ttu-id="49f8a-110">Kostnadsfria [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49f8a-110">The free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="49f8a-111">Den här snabbstarten använder Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="49f8a-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="49f8a-112">[Azure Toolkit för Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="49f8a-112">The [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="49f8a-113">Skapa ett dynamiskt webbprojekt i Eclipse</span><span class="sxs-lookup"><span data-stu-id="49f8a-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="49f8a-114">Välj **Arkiv** > **Nytt** > **Dynamic Web Project** (Dynamiskt webbprojekt) Eclipse.</span><span class="sxs-lookup"><span data-stu-id="49f8a-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="49f8a-115">I dialogrutan **New Dynamic Web Project** (Nytt dynamiskt webbprojekt) ger du projektet namnet **MyFirstJavaOnAzureWebApp** och väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-115">In the **New Dynamic Web Project** dialog box, name the project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![Dialogrutan för nytt dynamiskt webbprojekt](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="49f8a-117">Lägg till en JSP-sida</span><span class="sxs-lookup"><span data-stu-id="49f8a-117">Add a JSP page</span></span>

<span data-ttu-id="49f8a-118">Om projektutforskaren inte visas kan du återställa den.</span><span class="sxs-lookup"><span data-stu-id="49f8a-118">If Project Explorer is not displayed, restore it.</span></span>

![Java EE-arbetsytan för Eclipse](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="49f8a-120">Expandera projektet **MyFirstJavaOnAzureWebApp** i projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="49f8a-120">In Project Explorer, expand the **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="49f8a-121">Högerklicka på **Webbinnehåll** och välj sedan **Ny** > **JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Menyn för en ny JSP-fil i projektutforskaren](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="49f8a-123">I dialogrutan **Ny JSP-fil**:</span><span class="sxs-lookup"><span data-stu-id="49f8a-123">In the **New JSP File** dialog box:</span></span>

* <span data-ttu-id="49f8a-124">Namnge filen **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-124">Name the file **index.jsp**.</span></span>
* <span data-ttu-id="49f8a-125">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-125">Select **Finish**.</span></span>

  ![Dialogruta för en ny JSP-fil](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="49f8a-127">I filen index.jsp ersätter du elementet `<body></body>` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="49f8a-127">In the index.jsp file, replace the `<body></body>` element with the following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="49f8a-128">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="49f8a-128">Save the changes.</span></span>

## <a name="publish-the-web-app-to-azure"></a><span data-ttu-id="49f8a-129">Publicera webbappen i Azure</span><span class="sxs-lookup"><span data-stu-id="49f8a-129">Publish the web app to Azure</span></span>

<span data-ttu-id="49f8a-130">Högerklicka på projektet i projektutforskaren och välj sedan **Azure** > **Publicera som Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-130">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![Publicera som Azure Web App (snabbmeny)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="49f8a-132">I dialogrutan för **Azure-inloggning** behåller du alternativet **Interaktiv** och väljer sedan **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-132">In the **Azure Sign In** dialog box, keep the **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="49f8a-133">Följ anvisningarna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="49f8a-133">Follow the sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="49f8a-134">Dialogrutan Distribuera webbapp</span><span class="sxs-lookup"><span data-stu-id="49f8a-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="49f8a-135">När du har loggats in på Azure-kontot visas dialogrutan **Distribuera webbapp**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-135">After you have signed in to your Azure account, the **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="49f8a-136">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-136">Select **Create**.</span></span>

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="49f8a-138">Dialogrutan Skapa App Service</span><span class="sxs-lookup"><span data-stu-id="49f8a-138">Create App Service dialog box</span></span>

<span data-ttu-id="49f8a-139">Dialogrutan **Skapa App Service** visas med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="49f8a-139">The **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="49f8a-140">Numret **170602185241** som visas i följande bild skiljer sig från vad som visas i din dialogruta.</span><span class="sxs-lookup"><span data-stu-id="49f8a-140">The number **170602185241** shown in the following image is different in your dialog box.</span></span>

![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="49f8a-142">I dialogrutan **Skapa App Service**:</span><span class="sxs-lookup"><span data-stu-id="49f8a-142">In the **Create App Service** dialog box:</span></span>

* <span data-ttu-id="49f8a-143">Behåll det genererade namnet för webbappen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-143">Keep the generated name for the web app.</span></span> <span data-ttu-id="49f8a-144">Användarnamnet måste vara unikt inom Azure.</span><span class="sxs-lookup"><span data-stu-id="49f8a-144">This name must be unique across Azure.</span></span> <span data-ttu-id="49f8a-145">Namnet är en del av URL-adressen för webbappen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-145">The name is part of the URL address for the web app.</span></span> <span data-ttu-id="49f8a-146">Exempel: om webbappens namn är **MyJavaWebApp** så är URL-adressen *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="49f8a-146">For example: if the web app name is **MyJavaWebApp**, the URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="49f8a-147">Behåll webbehållaren som är standard.</span><span class="sxs-lookup"><span data-stu-id="49f8a-147">Keep the default web container.</span></span>
* <span data-ttu-id="49f8a-148">Välj en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="49f8a-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="49f8a-149">På fliken **App Service-plan**:</span><span class="sxs-lookup"><span data-stu-id="49f8a-149">On the **App service plan** tab:</span></span>

  * <span data-ttu-id="49f8a-150">**Skapa ny**: Behåll standardvärdet, som är namnet på App Service-planen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-150">**Create new**: Keep the default, which is the name of the App Service plan.</span></span>
  * <span data-ttu-id="49f8a-151">**Plats**: Välj **Europa, västra** eller en plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="49f8a-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="49f8a-152">**Prisnivå**: Markera det kostnadsfria alternativet.</span><span class="sxs-lookup"><span data-stu-id="49f8a-152">**Pricing tier**: Select the free option.</span></span> <span data-ttu-id="49f8a-153">Se [Priser för App Service](https://azure.microsoft.com/pricing/details/app-service/) för funktioner.</span><span class="sxs-lookup"><span data-stu-id="49f8a-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="49f8a-155">Flik för resursgrupp</span><span class="sxs-lookup"><span data-stu-id="49f8a-155">Resource group tab</span></span>

<span data-ttu-id="49f8a-156">Välj fliken **Resursgrupp**. Behåll det genererade standardvärdet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-156">Select the **Resource group** tab. Keep the default generated value for the resource group.</span></span>

![Flik för resursgrupp](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="49f8a-158">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-158">Select **Create**.</span></span>

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="49f8a-159">Webbappen skapas av Azure Toolkit och en dialogruta med en förloppsindikator visas.</span><span class="sxs-lookup"><span data-stu-id="49f8a-159">The Azure Toolkit creates the web app and displays a progress dialog box.</span></span>

![Dialogruta med förloppsindikator för hur apptjänsten skapas](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="49f8a-161">Dialogrutan Distribuera webbapp</span><span class="sxs-lookup"><span data-stu-id="49f8a-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="49f8a-162">I dialogrutan **Distribuera webbapp** väljer du **Distribuera till rot**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-162">In the **Deploy Web App** dialog box, select **Deploy to root**.</span></span> <span data-ttu-id="49f8a-163">Om du har en apptjänst på *wingtiptoys.azurewebsites.net* och inte distribuerar webbappen till roten så distribueras webbappen med namnet **MyFirstJavaOnAzureWebApp** till *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="49f8a-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy to the root, the web app named **MyFirstJavaOnAzureWebApp** is deployed to *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="49f8a-165">Dialogrutan visar valen för Azure, JDK och webbehållaren.</span><span class="sxs-lookup"><span data-stu-id="49f8a-165">The dialog box shows the Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="49f8a-166">Välj **Distribuera** för att publicera webbappen i Azure.</span><span class="sxs-lookup"><span data-stu-id="49f8a-166">Select **Deploy** to publish the web app to Azure.</span></span>

<span data-ttu-id="49f8a-167">När publiceringen är klar väljer du länken **Publicerade** i dialogrutan **Azure-aktivitetsloggen**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-167">When the publishing finishes, select the **Published** link in the **Azure Activity Log** dialog box.</span></span>

![Dialogrutan för Azure aktivitetsloggen](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="49f8a-169">Grattis!</span><span class="sxs-lookup"><span data-stu-id="49f8a-169">Congratulations!</span></span> <span data-ttu-id="49f8a-170">Din webbapp har distribuerats till Azure.</span><span class="sxs-lookup"><span data-stu-id="49f8a-170">You have successfully deployed your web app to Azure.</span></span> 

!["Hello Azure!"](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a><span data-ttu-id="49f8a-173">Uppdatera webbappen</span><span class="sxs-lookup"><span data-stu-id="49f8a-173">Update the web app</span></span>

<span data-ttu-id="49f8a-174">Ändra JSP-exempelkod till ett annat meddelande.</span><span class="sxs-lookup"><span data-stu-id="49f8a-174">Change the sample JSP code to a different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="49f8a-175">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="49f8a-175">Save the changes.</span></span>

<span data-ttu-id="49f8a-176">Högerklicka på projektet i projektutforskaren och välj sedan **Azure** > **Publicera som Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="49f8a-176">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="49f8a-177">Dialogrutan **Distribuera webbapp** visas med apptjänsten som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="49f8a-177">The **Deploy Web App** dialog box appears and shows the app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="49f8a-178">Välj **Distribuera till rot** varje gång du publicerar.</span><span class="sxs-lookup"><span data-stu-id="49f8a-178">Select **Deploy to root** each time you publish.</span></span>
>

<span data-ttu-id="49f8a-179">Välj webbappen och sedan **Distribuera**. Då publiceras ändringarna.</span><span class="sxs-lookup"><span data-stu-id="49f8a-179">Select the web app and select **Deploy**, which publishes the changes.</span></span>

<span data-ttu-id="49f8a-180">När länken **Publicering** visas väljer du den för att bläddra till webbappen och se ändringarna.</span><span class="sxs-lookup"><span data-stu-id="49f8a-180">When the **Publishing** link appears, select it to browse to the web app and see the changes.</span></span>

## <a name="manage-the-web-app"></a><span data-ttu-id="49f8a-181">Hantera webbappen</span><span class="sxs-lookup"><span data-stu-id="49f8a-181">Manage the web app</span></span>

<span data-ttu-id="49f8a-182">Gå till <a href="https://portal.azure.com" target="_blank">Azure Portal</a> för att se den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="49f8a-182">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to see the web app that you created.</span></span>

<span data-ttu-id="49f8a-183">Välj **Resursgrupper** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="49f8a-183">From the left menu, select **Resource Groups**.</span></span>

![Portalnavigering för resursgrupper](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="49f8a-185">Välj resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-185">Select the resource group.</span></span> <span data-ttu-id="49f8a-186">På sidan visas resurserna som du skapade i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="49f8a-186">The page shows the resources that you created in this quickstart.</span></span>

![Resursgruppen myResourceGroup](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="49f8a-188">Välj webbappen (**webbapp-170602193915** i den föregående bilden).</span><span class="sxs-lookup"><span data-stu-id="49f8a-188">Select the web app (**webapp-170602193915** in the preceding image).</span></span>

<span data-ttu-id="49f8a-189">Sidan **Översikt** visas.</span><span class="sxs-lookup"><span data-stu-id="49f8a-189">The **Overview** page appears.</span></span> <span data-ttu-id="49f8a-190">Den här sidan ger dig en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="49f8a-190">This page gives you a view of how the app is doing.</span></span> <span data-ttu-id="49f8a-191">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="49f8a-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="49f8a-192">På flikarna till vänster på sidan kan du se olika konfigurationer som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="49f8a-192">The tabs on the left side of the page show the different configurations that you can open.</span></span> 

![App Service-sidan på Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="49f8a-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49f8a-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="49f8a-195">Mappa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="49f8a-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
