---
title: "Distribuera WebJobs med hjälp av Visual Studio"
description: "Lär dig hur du distribuerar Azure WebJobs till Azure App Service Web Apps med Visual Studio."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2016
ms.author: glenga
ms.openlocfilehash: 5b0808afdadcf4d86a9a2d07ee6fc63b80b22993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="40ff6-103">Distribuera WebJobs med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40ff6-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="40ff6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="40ff6-104">Overview</span></span>
<span data-ttu-id="40ff6-105">Det här avsnittet beskriver hur du använder Visual Studio för att distribuera ett konsolprogram projekt till en webbapp i [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) som en [Azure Webjobs](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="40ff6-105">This topic explains how to use Visual Studio to deploy a Console Application project to a web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="40ff6-106">Information om hur du distribuerar WebJobs med hjälp av den [Azure Portal](https://portal.azure.com), se [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="40ff6-106">For information about how to deploy WebJobs by using the [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="40ff6-107">När Visual Studio distribuerar ett WebJobs-aktiverade konsolprogram projekt, utför två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="40ff6-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="40ff6-108">Runtime filer kopieras till rätt mapp i webbapp (*App_Data/jobb/kontinuerlig* för kontinuerliga Webbjobb *App_Data/jobb/utlöst* för WebJobs schemalagda och på begäran).</span><span class="sxs-lookup"><span data-stu-id="40ff6-108">Copies runtime files to the appropriate folder in the web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="40ff6-109">Ställer in [Azure schemaläggare](#scheduler) för WebJobs som är schemalagda att köras vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="40ff6-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled to run at particular times.</span></span> <span data-ttu-id="40ff6-110">(Detta behövs inte för kontinuerliga Webbjobb.)</span><span class="sxs-lookup"><span data-stu-id="40ff6-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="40ff6-111">Ett WebJobs-aktiverade projekt har lagts till följande objekt:</span><span class="sxs-lookup"><span data-stu-id="40ff6-111">A WebJobs-enabled project has the following items added to it:</span></span>

* <span data-ttu-id="40ff6-112">Den [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-112">The [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="40ff6-113">En [webbjobb publicera settings.json](#publishsettings) -fil som innehåller inställningar för distribution och Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="40ff6-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Diagram över vad som har lagts till i en Konsolapp för att aktivera distribution som ett Webbjobb](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="40ff6-115">Du kan lägga till dessa objekt till ett befintligt projekt konsolprogram eller Använd en mall för att skapa ett nytt projekt WebJobs-aktiverade program.</span><span class="sxs-lookup"><span data-stu-id="40ff6-115">You can add these items to an existing Console Application project or use a template to create a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="40ff6-116">Du kan distribuera ett projekt som ett Webbjobb ensamt eller länka det till ett webbprojekt så att den distribuerar automatiskt när du distribuerar webbprojektet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-116">You can deploy a project as a WebJob by itself, or link it to a web project so that it automatically deploys whenever you deploy the web project.</span></span> <span data-ttu-id="40ff6-117">Om du vill länka projekt, Visual Studio innehåller namnet på WebJobs-aktiverade projektet i en [webjobs list.json](#webjobslist) filen i webbprojektet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-117">To link projects, Visual Studio includes the name of the WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in the web project.</span></span>

![Diagram över Webbjobb projekt som länkar till webbprojekt](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="40ff6-119">Krav</span><span class="sxs-lookup"><span data-stu-id="40ff6-119">Prerequisites</span></span>
<span data-ttu-id="40ff6-120">Funktioner för distribution av WebJobs är tillgängliga i Visual Studio när du installerar Azure SDK för .NET:</span><span class="sxs-lookup"><span data-stu-id="40ff6-120">WebJobs deployment features are available in Visual Studio when you install the Azure SDK for .NET:</span></span>

* <span data-ttu-id="40ff6-121">[Azure SDK för .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="40ff6-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="40ff6-122"><a id="convert"></a>Aktivera WebJobs-distribution för ett befintligt projekt konsolprogram</span><span class="sxs-lookup"><span data-stu-id="40ff6-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="40ff6-123">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="40ff6-123">You have two options:</span></span>

* <span data-ttu-id="40ff6-124">[Aktivera automatisk distribution med ett webbprojekt](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="40ff6-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="40ff6-125">Konfigurera ett befintligt projekt konsolprogram så att den distribuerar automatiskt som ett Webbjobb när du distribuerar ett webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="40ff6-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="40ff6-126">Använd det här alternativet om du vill köra din Webbjobbet i samma webbapp som du kör relaterade webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-126">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>
* <span data-ttu-id="40ff6-127">[Aktivera distributionen utan ett webbprojekt](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="40ff6-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="40ff6-128">Konfigurera ett befintligt projekt konsolprogram att distribuera som ett Webbjobb, med någon länk till ett webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="40ff6-128">Configure an existing Console Application project to deploy as a WebJob by itself, with no link to a web project.</span></span> <span data-ttu-id="40ff6-129">Använd det här alternativet när du vill köra ett Webbjobb i en webbapp, med Inga webbprogram som körs i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-129">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="40ff6-130">Du kanske vill göra detta för att kunna skala resurserna Webbjobb oberoende av webbtillämpningsresurser.</span><span class="sxs-lookup"><span data-stu-id="40ff6-130">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="40ff6-131"><a id="convertlink"></a>Aktivera automatisk WebJobs-distribution med ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="40ff6-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="40ff6-132">Högerklicka på webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **befintligt projekt som Azure Webjobs**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-132">Right-click the web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Befintligt projekt som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="40ff6-134">Den [lägga till Azure Webjobs](#configure) dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="40ff6-134">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="40ff6-135">I den **projektnamn** listrutan, Välj konsolprogrammet projektet ska läggas till som ett Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="40ff6-135">In the **Project name** drop-down list, select the Console Application project to add as a WebJob.</span></span>
   
    ![Att markera projekt i dialogrutan Lägg till Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="40ff6-137">Slutför den [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-137">Complete the [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="40ff6-138"><a id="convertnolink"></a>Aktivera WebJobs distribution utan ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="40ff6-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="40ff6-139">Högerklicka på projektet konsolprogram i **Solution Explorer**, och klicka sedan på **Publicera som Azure Webjobs...** .</span><span class="sxs-lookup"><span data-stu-id="40ff6-139">Right-click the Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="40ff6-141">Den [lägga till Azure Webjobs](#configure) dialogruta visas med projektet som väljs i den **projektnamn** rutan.</span><span class="sxs-lookup"><span data-stu-id="40ff6-141">The [Add Azure WebJob](#configure) dialog box appears, with the project selected in the **Project name** box.</span></span>
2. <span data-ttu-id="40ff6-142">Slutför den [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-142">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="40ff6-143">Den **Publicera webbplats** guiden visas.</span><span class="sxs-lookup"><span data-stu-id="40ff6-143">The **Publish Web** wizard appears.</span></span>  <span data-ttu-id="40ff6-144">Om du inte vill publicera omedelbart stänga guiden.</span><span class="sxs-lookup"><span data-stu-id="40ff6-144">If you do not want to publish immediately, close the wizard.</span></span> <span data-ttu-id="40ff6-145">De inställningar som du har angett sparas för när du vill [distribuera projektet](#deploy).</span><span class="sxs-lookup"><span data-stu-id="40ff6-145">The settings that you've entered are saved for when you do want to [deploy the project](#deploy).</span></span>

## <span data-ttu-id="40ff6-146"><a id="create"></a>Skapa ett nytt projekt WebJobs-aktiverad</span><span class="sxs-lookup"><span data-stu-id="40ff6-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="40ff6-147">Om du vill skapa ett nytt projekt WebJobs-aktiverade, kan du använda projektmall konsolprogram och aktivera WebJobs distribution enligt beskrivningen i [i föregående avsnitt](#convert).</span><span class="sxs-lookup"><span data-stu-id="40ff6-147">To create a new WebJobs-enabled project, you can use the Console Application project template and enable WebJobs deployment as explained in [the previous section](#convert).</span></span> <span data-ttu-id="40ff6-148">Alternativt kan använda du WebJobs nytt projekt mallen:</span><span class="sxs-lookup"><span data-stu-id="40ff6-148">As an alternative, you can use the WebJobs new-project template:</span></span>

* [<span data-ttu-id="40ff6-149">Använd WebJobs nytt projekt mall för en oberoende Webbjobbet</span><span class="sxs-lookup"><span data-stu-id="40ff6-149">Use the WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="40ff6-150">Skapa ett projekt och konfigurera den för att distribuera fristående som ett Webbjobb med ingen länk till ett webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="40ff6-150">Create a project and configure it to deploy by itself as a WebJob, with no link to a web project.</span></span> <span data-ttu-id="40ff6-151">Använd det här alternativet när du vill köra ett Webbjobb i en webbapp, med Inga webbprogram som körs i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-151">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="40ff6-152">Du kanske vill göra detta för att kunna skala resurserna Webbjobb oberoende av webbtillämpningsresurser.</span><span class="sxs-lookup"><span data-stu-id="40ff6-152">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="40ff6-153">Använd WebJobs nytt projekt mall för ett Webbjobb som är kopplad till ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="40ff6-153">Use the WebJobs new-project template for a WebJob linked to a web project</span></span>](#createlink)
  
    <span data-ttu-id="40ff6-154">Skapa ett projekt som är konfigurerad för att distribuera automatiskt som ett Webbjobb när ett webbprojekt i samma lösning som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="40ff6-154">Create a project that is configured to deploy automatically as a WebJob when a web project in the same solution is deployed.</span></span> <span data-ttu-id="40ff6-155">Använd det här alternativet om du vill köra din Webbjobbet i samma webbapp som du kör relaterade webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-155">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="40ff6-156">Mallen WebJobs nytt projekt som automatiskt installerar NuGet-paket och innehåller koden i *Program.cs* för den [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="40ff6-156">The WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for the [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="40ff6-157">Om du inte vill använda WebJobs-SDK, ta bort eller ändra den `host.RunAndBlock` instruktionen i *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="40ff6-157">If you don't want to use the WebJobs SDK, remove or change the `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="40ff6-158"><a id="createnolink"></a>Använd WebJobs nytt projekt mall för en oberoende Webbjobbet</span><span class="sxs-lookup"><span data-stu-id="40ff6-158"><a id="createnolink"></a> Use the WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="40ff6-159">Klicka på **filen** > **nytt projekt**, och klicka sedan på den **nytt projekt** klickar du på **moln**  >   **Azure Webjobs (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-159">Click **File** > **New Project**, and then in the **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Dialogrutan Nytt projekt som visar Webbjobb mall](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="40ff6-161">Följ anvisningarna som visas tidigare till [Se konsolprogrammet projicera ett oberoende WebJobs-projekt](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="40ff6-161">Follow the directions shown earlier to [make the Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="40ff6-162"><a id="createlink"></a>Använd WebJobs nytt projekt mall för ett Webbjobb som är kopplad till ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="40ff6-162"><a id="createlink"></a> Use the WebJobs new-project template for a WebJob linked to a web project</span></span>
1. <span data-ttu-id="40ff6-163">Högerklicka på webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **nytt projekt för Azure Webjobs**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-163">Right-click the web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Nya menypost för Azure Webjobs-projekt](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="40ff6-165">Den [lägga till Azure Webjobs](#configure) dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="40ff6-165">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="40ff6-166">Slutför den [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="40ff6-166">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="40ff6-167"><a id="configure"></a>Dialogrutan Lägg till Azure Webbjobbet</span><span class="sxs-lookup"><span data-stu-id="40ff6-167"><a id="configure"></a>The Add Azure WebJob dialog</span></span>
<span data-ttu-id="40ff6-168">Den **lägga till Azure Webjobs** dialogrutan låter dig ange webbjobbsnamnet och kör inställning för din Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="40ff6-168">The **Add Azure WebJob** dialog lets you enter the WebJob name and run mode setting for your WebJob.</span></span> 

![Azure Webjobs dialogrutan Lägg till](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="40ff6-170">Fälten i den här dialogrutan motsvarar fält på den **nytt jobb** dialogrutan i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="40ff6-170">The fields in this dialog correspond to fields on the **New Job** dialog of the Azure Portal.</span></span> <span data-ttu-id="40ff6-171">Mer information finns i [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="40ff6-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="40ff6-172">Information om kommandoradsverktyget distribution finns [aktiverar kommandoraden eller kontinuerlig leverans Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="40ff6-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="40ff6-173">Om du distribuerar ett Webbjobb och sedan bestämmer du vill ändra typen av Webbjobb och distribuera om behöver du ta bort filen webjobs publicera settings.json.</span><span class="sxs-lookup"><span data-stu-id="40ff6-173">If you deploy a WebJob and then decide you want to change the type of WebJob and redeploy, you'll need to delete the webjobs-publish-settings.json file.</span></span> <span data-ttu-id="40ff6-174">Detta gör Visual Studio visa publiceringsalternativ igen, så att du kan ändra typ av Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="40ff6-174">This will make Visual Studio show the publishing options again, so you can change the type of WebJob.</span></span>
> * <span data-ttu-id="40ff6-175">Om du distribuerar ett Webbjobb och senare ändrar läget Kör från kontinuerlig till icke-kontinuerliga och vice versa skapar Visual Studio ett nytt Webbjobb i Azure när du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="40ff6-175">If you deploy a WebJob and later change the run mode from continuous to non-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="40ff6-176">Om du ändrar andra schemainställningarna men lämna körningsläge samma eller växla mellan schemalagda och på begäran, Visual Studio uppdaterar befintliga jobbet i stället skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="40ff6-176">If you change other scheduling settings but leave run mode the same or switch between Scheduled and On Demand, Visual Studio updates the existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="40ff6-177"><a id="publishsettings"></a>webbjobbet publicera settings.json</span><span class="sxs-lookup"><span data-stu-id="40ff6-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="40ff6-178">När du konfigurerar ett konsolprogram för distribution av WebJobs Visual Studio installerar den [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paket- och lagrar schemainformation i en *webbjobb publicera settings.json*  filen i projektet *egenskaper* mappen WebJobs-projektet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs the [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in the project *Properties* folder of the WebJobs project.</span></span> <span data-ttu-id="40ff6-179">Här är ett exempel på filen:</span><span class="sxs-lookup"><span data-stu-id="40ff6-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="40ff6-180">Du kan redigera den här filen direkt och Visual Studio har IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="40ff6-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="40ff6-181">Schemat filen lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) och kan visa.</span><span class="sxs-lookup"><span data-stu-id="40ff6-181">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="40ff6-182"><a id="webjobslist"></a>webjobs list.json</span><span class="sxs-lookup"><span data-stu-id="40ff6-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="40ff6-183">När du länkar ett WebJobs-aktiverade projekt till ett webbprojekt Visual Studio lagrar namnet på WebJobs-projekt i en *webjobs list.json* filen i webbprojektet *egenskaper* mapp.</span><span class="sxs-lookup"><span data-stu-id="40ff6-183">When you link a WebJobs-enabled project to a web project, Visual Studio stores the name of the WebJobs project in a *webjobs-list.json* file in the web project's *Properties* folder.</span></span> <span data-ttu-id="40ff6-184">Listan kan innehålla flera WebJobs projekt som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="40ff6-184">The list might contain multiple WebJobs projects, as shown in the following example:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

<span data-ttu-id="40ff6-185">Du kan redigera den här filen direkt och Visual Studio har IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="40ff6-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="40ff6-186">Schemat filen lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) och kan visa.</span><span class="sxs-lookup"><span data-stu-id="40ff6-186">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="40ff6-187"><a id="deploy"></a>Distribuera ett WebJobs-projekt</span><span class="sxs-lookup"><span data-stu-id="40ff6-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="40ff6-188">Ett WebJobs-projekt som du har länkats till ett webbprojekt distribuerar automatiskt med webbprojektet.</span><span class="sxs-lookup"><span data-stu-id="40ff6-188">A WebJobs project that you have linked to a web project deploys automatically with the web project.</span></span> <span data-ttu-id="40ff6-189">Information om web project distribution finns [hur du distribuerar till Web Apps](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="40ff6-189">For information about web project deployment, see [How to deploy to Web Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="40ff6-190">Högerklicka på projektet i för att distribuera ett projekt WebJobs ensamt **Solution Explorer** och på **Publicera som Azure Webjobs...** .</span><span class="sxs-lookup"><span data-stu-id="40ff6-190">To deploy a WebJobs project by itself, right-click the project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="40ff6-192">För en oberoende Webbjobb samma **Publicera webbplats** guiden som används för webbprojekt visas, men med färre inställningar som är tillgängliga för att ändra.</span><span class="sxs-lookup"><span data-stu-id="40ff6-192">For an independent WebJob, the same **Publish Web** wizard that is used for web projects appears, but with fewer settings available to change.</span></span>

## <span data-ttu-id="40ff6-193"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="40ff6-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="40ff6-194">Den här artikeln har förklaras hur du distribuerar WebJobs med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40ff6-194">This article has explained how to deploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="40ff6-195">Läs mer om hur du distribuerar Azure WebJobs [Azure WebJobs - rekommenderade resurser - distribution](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="40ff6-195">For more information about how to deploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

