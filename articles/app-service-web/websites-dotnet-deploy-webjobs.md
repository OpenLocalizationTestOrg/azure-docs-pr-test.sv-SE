---
title: aaaDeploy WebJobs med Visual Studio
description: "Lär dig hur toodeploy Azure WebJobs tooAzure App Service Web Apps med Visual Studio."
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
ms.openlocfilehash: 5fc5d9562e8836348f5ab6844fb6c23ff40a321c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="e62f1-103">Distribuera WebJobs med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e62f1-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="e62f1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e62f1-104">Overview</span></span>
<span data-ttu-id="e62f1-105">Det här avsnittet beskrivs hur toouse Visual Studio toodeploy ett konsolprogram projektet tooa webbapp i [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) som en [Azure Webjobs](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="e62f1-105">This topic explains how toouse Visual Studio toodeploy a Console Application project tooa web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="e62f1-106">Information om hur toodeploy WebJobs med hjälp av hello [Azure Portal](https://portal.azure.com), se [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="e62f1-106">For information about how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="e62f1-107">När Visual Studio distribuerar ett WebJobs-aktiverade konsolprogram projekt, utför två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="e62f1-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="e62f1-108">Kopior runtime filer toohello lämplig mapp i hello webbapp (*App_Data/jobb/kontinuerlig* för kontinuerliga Webbjobb *App_Data/jobb/utlöst* för WebJobs schemalagda och på begäran).</span><span class="sxs-lookup"><span data-stu-id="e62f1-108">Copies runtime files toohello appropriate folder in hello web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="e62f1-109">Ställer in [Azure schemaläggare](#scheduler) för WebJobs som är schemalagda toorun vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled toorun at particular times.</span></span> <span data-ttu-id="e62f1-110">(Detta behövs inte för kontinuerliga Webbjobb.)</span><span class="sxs-lookup"><span data-stu-id="e62f1-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="e62f1-111">Ett WebJobs-aktiverade projekt har hello efter objekt som lagts till tooit:</span><span class="sxs-lookup"><span data-stu-id="e62f1-111">A WebJobs-enabled project has hello following items added tooit:</span></span>

* <span data-ttu-id="e62f1-112">Hej [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="e62f1-112">hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="e62f1-113">En [webbjobb publicera settings.json](#publishsettings) -fil som innehåller inställningar för distribution och Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="e62f1-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Diagram över vad läggs tooa konsolen tooenable appdistribution som ett Webbjobb](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="e62f1-115">Du kan lägga till dessa objekt tooan befintliga konsolprogram projekt eller använda en mall toocreate ett nytt projekt WebJobs-aktiverade program.</span><span class="sxs-lookup"><span data-stu-id="e62f1-115">You can add these items tooan existing Console Application project or use a template toocreate a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="e62f1-116">Du kan distribuera ett projekt som ett Webbjobb ensamt eller länka det tooa webbprojekt så att den distribuerar automatiskt när du distribuerar hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-116">You can deploy a project as a WebJob by itself, or link it tooa web project so that it automatically deploys whenever you deploy hello web project.</span></span> <span data-ttu-id="e62f1-117">toolink projekt, Visual Studio innehåller hello namnet på hello WebJobs-aktiverade projekt i en [webjobs list.json](#webjobslist) filen i hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-117">toolink projects, Visual Studio includes hello name of hello WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in hello web project.</span></span>

![Diagram över Webbjobb projekt länka tooweb projekt](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="e62f1-119">Krav</span><span class="sxs-lookup"><span data-stu-id="e62f1-119">Prerequisites</span></span>
<span data-ttu-id="e62f1-120">Funktioner för distribution av WebJobs är tillgängliga i Visual Studio när du installerar hello Azure SDK för .NET:</span><span class="sxs-lookup"><span data-stu-id="e62f1-120">WebJobs deployment features are available in Visual Studio when you install hello Azure SDK for .NET:</span></span>

* <span data-ttu-id="e62f1-121">[Azure SDK för .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e62f1-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="e62f1-122"><a id="convert"></a>Aktivera WebJobs-distribution för ett befintligt projekt konsolprogram</span><span class="sxs-lookup"><span data-stu-id="e62f1-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="e62f1-123">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="e62f1-123">You have two options:</span></span>

* <span data-ttu-id="e62f1-124">[Aktivera automatisk distribution med ett webbprojekt](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="e62f1-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="e62f1-125">Konfigurera ett befintligt projekt konsolprogram så att den distribuerar automatiskt som ett Webbjobb när du distribuerar ett webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="e62f1-126">Använd det här alternativet när du vill toorun din Webbjobb i hello samma webbprogram som du kör hello relaterade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e62f1-126">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>
* <span data-ttu-id="e62f1-127">[Aktivera distributionen utan ett webbprojekt](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="e62f1-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="e62f1-128">Konfigurera en befintlig konsolprogram projektet toodeploy som ett Webbjobb, med ingen länk tooa webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-128">Configure an existing Console Application project toodeploy as a WebJob by itself, with no link tooa web project.</span></span> <span data-ttu-id="e62f1-129">Använd det här alternativet när du vill toorun ett Webbjobb i en webbapp, med Inga webbprogram som körs i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="e62f1-129">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="e62f1-130">Du kanske vill toodo detta beställa toobe kan tooscale resurserna Webbjobb oberoende av webbtillämpningsresurser.</span><span class="sxs-lookup"><span data-stu-id="e62f1-130">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="e62f1-131"><a id="convertlink"></a>Aktivera automatisk WebJobs-distribution med ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="e62f1-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="e62f1-132">Högerklicka på hello webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **befintligt projekt som Azure Webjobs**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-132">Right-click hello web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Befintligt projekt som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="e62f1-134">Hej [lägga till Azure Webjobs](#configure) dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e62f1-134">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="e62f1-135">I hello **projektnamn** listrutan, Välj hello konsolprogram projektet tooadd som ett Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="e62f1-135">In hello **Project name** drop-down list, select hello Console Application project tooadd as a WebJob.</span></span>
   
    ![Att markera projekt i dialogrutan Lägg till Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="e62f1-137">Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-137">Complete hello [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="e62f1-138"><a id="convertnolink"></a>Aktivera WebJobs distribution utan ett webbprojekt</span><span class="sxs-lookup"><span data-stu-id="e62f1-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="e62f1-139">Högerklicka på hello konsolprogram projekt i **Solution Explorer**, och klicka sedan på **Publicera som Azure Webjobs...** .</span><span class="sxs-lookup"><span data-stu-id="e62f1-139">Right-click hello Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="e62f1-141">Hej [lägga till Azure Webjobs](#configure) dialogruta visas med hello-projektet som väljs i hello **projektnamn** rutan.</span><span class="sxs-lookup"><span data-stu-id="e62f1-141">hello [Add Azure WebJob](#configure) dialog box appears, with hello project selected in hello **Project name** box.</span></span>
2. <span data-ttu-id="e62f1-142">Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-142">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="e62f1-143">Hej **Publicera webbplats** guiden visas.</span><span class="sxs-lookup"><span data-stu-id="e62f1-143">hello **Publish Web** wizard appears.</span></span>  <span data-ttu-id="e62f1-144">Stäng hello guiden om du inte vill toopublish omedelbart.</span><span class="sxs-lookup"><span data-stu-id="e62f1-144">If you do not want toopublish immediately, close hello wizard.</span></span> <span data-ttu-id="e62f1-145">hello-inställningar som du har angett sparas för när du vill för[distribuera hello projekt](#deploy).</span><span class="sxs-lookup"><span data-stu-id="e62f1-145">hello settings that you've entered are saved for when you do want too[deploy hello project](#deploy).</span></span>

## <span data-ttu-id="e62f1-146"><a id="create"></a>Skapa ett nytt projekt WebJobs-aktiverad</span><span class="sxs-lookup"><span data-stu-id="e62f1-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="e62f1-147">toocreate ett nytt WebJobs-aktiverade projekt, du kan använda hello konsolen projektet mall och aktivera WebJobs programdistribution enligt beskrivningen i [hello föregående avsnitt](#convert).</span><span class="sxs-lookup"><span data-stu-id="e62f1-147">toocreate a new WebJobs-enabled project, you can use hello Console Application project template and enable WebJobs deployment as explained in [hello previous section](#convert).</span></span> <span data-ttu-id="e62f1-148">Alternativt kan använda du hello WebJobs nytt projekt mallen:</span><span class="sxs-lookup"><span data-stu-id="e62f1-148">As an alternative, you can use hello WebJobs new-project template:</span></span>

* [<span data-ttu-id="e62f1-149">Använd hello WebJobs nytt projekt mall för en oberoende Webbjobbet</span><span class="sxs-lookup"><span data-stu-id="e62f1-149">Use hello WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="e62f1-150">Skapa ett projekt och konfigurera den toodeploy ensamt som ett Webbjobb med ingen länk tooa webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-150">Create a project and configure it toodeploy by itself as a WebJob, with no link tooa web project.</span></span> <span data-ttu-id="e62f1-151">Använd det här alternativet när du vill toorun ett Webbjobb i en webbapp, med Inga webbprogram som körs i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="e62f1-151">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="e62f1-152">Du kanske vill toodo detta beställa toobe kan tooscale resurserna Webbjobb oberoende av webbtillämpningsresurser.</span><span class="sxs-lookup"><span data-stu-id="e62f1-152">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="e62f1-153">Använd hello WebJobs nytt projekt mall för ett Webbjobb länkade tooa webbprojekt</span><span class="sxs-lookup"><span data-stu-id="e62f1-153">Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>](#createlink)
  
    <span data-ttu-id="e62f1-154">Skapa ett projekt som är konfigurerade toodeploy automatiskt som ett Webbjobb när ett webbprojekt i hello samma lösningen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="e62f1-154">Create a project that is configured toodeploy automatically as a WebJob when a web project in hello same solution is deployed.</span></span> <span data-ttu-id="e62f1-155">Använd det här alternativet när du vill toorun din Webbjobb i hello samma webbprogram som du kör hello relaterade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e62f1-155">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="e62f1-156">Hej WebJobs nytt projekt mallen som automatiskt installerar NuGet-paket och inkluderar den kod i *Program.cs* för hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="e62f1-156">hello WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="e62f1-157">Om du inte vill toouse hello WebJobs SDK, ta bort eller ändra hello `host.RunAndBlock` instruktionen i *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e62f1-157">If you don't want toouse hello WebJobs SDK, remove or change hello `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="e62f1-158"><a id="createnolink"></a>Använd hello WebJobs nytt projekt mall för en oberoende Webbjobbet</span><span class="sxs-lookup"><span data-stu-id="e62f1-158"><a id="createnolink"></a> Use hello WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="e62f1-159">Klicka på **filen** > **nytt projekt**, och klicka sedan på hello **nytt projekt** klickar du på **moln**  >   **Azure Webjobs (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-159">Click **File** > **New Project**, and then in hello **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Dialogrutan Nytt projekt som visar Webbjobb mall](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="e62f1-161">Följ hello anvisningar som visades tidigare för[Se hello konsolprogram projektet ett oberoende WebJobs-projekt](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="e62f1-161">Follow hello directions shown earlier too[make hello Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="e62f1-162"><a id="createlink"></a>Använd hello WebJobs nytt projekt mall för ett Webbjobb länkade tooa webbprojekt</span><span class="sxs-lookup"><span data-stu-id="e62f1-162"><a id="createlink"></a> Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>
1. <span data-ttu-id="e62f1-163">Högerklicka på hello webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **nytt projekt för Azure Webjobs**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-163">Right-click hello web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Nya menypost för Azure Webjobs-projekt](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="e62f1-165">Hej [lägga till Azure Webjobs](#configure) dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e62f1-165">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="e62f1-166">Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e62f1-166">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="e62f1-167"><a id="configure"></a>hello lägga till Azure Webjobs dialogrutan</span><span class="sxs-lookup"><span data-stu-id="e62f1-167"><a id="configure"></a>hello Add Azure WebJob dialog</span></span>
<span data-ttu-id="e62f1-168">Hej **lägga till Azure Webjobs** dialogrutan låter dig ange hello webbjobbsnamnet och kör inställning för din Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="e62f1-168">hello **Add Azure WebJob** dialog lets you enter hello WebJob name and run mode setting for your WebJob.</span></span> 

![Azure Webjobs dialogrutan Lägg till](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="e62f1-170">hello fält i den här dialogrutan motsvarar toofields på hello **nytt jobb** dialogrutan av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e62f1-170">hello fields in this dialog correspond toofields on hello **New Job** dialog of hello Azure Portal.</span></span> <span data-ttu-id="e62f1-171">Mer information finns i [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="e62f1-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="e62f1-172">Information om kommandoradsverktyget distribution finns [aktiverar kommandoraden eller kontinuerlig leverans Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="e62f1-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="e62f1-173">Om du distribuerar ett Webbjobb och sedan bestämmer du vill toochange hello typ av Webbjobb och distribuera om, behöver du toodelete hello webjobs publicera settings.json fil.</span><span class="sxs-lookup"><span data-stu-id="e62f1-173">If you deploy a WebJob and then decide you want toochange hello type of WebJob and redeploy, you'll need toodelete hello webjobs-publish-settings.json file.</span></span> <span data-ttu-id="e62f1-174">Detta gör Visual Studio som visar hello publiceringsalternativ igen, så att du kan ändra hello typ av Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="e62f1-174">This will make Visual Studio show hello publishing options again, so you can change hello type of WebJob.</span></span>
> * <span data-ttu-id="e62f1-175">Om du distribuerar ett Webbjobb och senare ändrar hello körningsläge från kontinuerlig toonon kontinuerlig eller tvärtom, Visual Studio skapar ett nytt Webbjobb i Azure när du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="e62f1-175">If you deploy a WebJob and later change hello run mode from continuous toonon-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="e62f1-176">Om du ändrar andra schemainställningarna men lämna kör läge hello samma eller växla mellan schemalagda och på begäran, Visual Studio uppdateringar hello befintligt jobb i stället skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="e62f1-176">If you change other scheduling settings but leave run mode hello same or switch between Scheduled and On Demand, Visual Studio updates hello existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="e62f1-177"><a id="publishsettings"></a>webbjobbet publicera settings.json</span><span class="sxs-lookup"><span data-stu-id="e62f1-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="e62f1-178">När du konfigurerar ett konsolprogram för distribution av WebJobs installerar Visual Studio hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paket- och lagrar schemainformation i en *webbjobb publicera settings.json*  fil i hello projekt *egenskaper* för hello WebJobs-projekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in hello project *Properties* folder of hello WebJobs project.</span></span> <span data-ttu-id="e62f1-179">Här är ett exempel på filen:</span><span class="sxs-lookup"><span data-stu-id="e62f1-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="e62f1-180">Du kan redigera den här filen direkt och Visual Studio har IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e62f1-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="e62f1-181">hello filen schemat lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) och kan visa.</span><span class="sxs-lookup"><span data-stu-id="e62f1-181">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="e62f1-182"><a id="webjobslist"></a>webjobs list.json</span><span class="sxs-lookup"><span data-stu-id="e62f1-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="e62f1-183">När du länkar ett WebJobs-aktiverade projektet tooa webbprojekt Visual Studio lagrar hello namnet på hello WebJobs-projekt i en *webjobs list.json* filen i hello-webbprojekt *egenskaper* mapp.</span><span class="sxs-lookup"><span data-stu-id="e62f1-183">When you link a WebJobs-enabled project tooa web project, Visual Studio stores hello name of hello WebJobs project in a *webjobs-list.json* file in hello web project's *Properties* folder.</span></span> <span data-ttu-id="e62f1-184">hello listan kan innehålla flera WebJobs projekt som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e62f1-184">hello list might contain multiple WebJobs projects, as shown in hello following example:</span></span>

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

<span data-ttu-id="e62f1-185">Du kan redigera den här filen direkt och Visual Studio har IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e62f1-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="e62f1-186">hello filen schemat lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) och kan visa.</span><span class="sxs-lookup"><span data-stu-id="e62f1-186">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="e62f1-187"><a id="deploy"></a>Distribuera ett WebJobs-projekt</span><span class="sxs-lookup"><span data-stu-id="e62f1-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="e62f1-188">Ett WebJobs-projekt som du har länkat tooa webbprojekt distribuerar automatiskt med hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e62f1-188">A WebJobs project that you have linked tooa web project deploys automatically with hello web project.</span></span> <span data-ttu-id="e62f1-189">Information om web project distribution finns [hur toodeploy tooWeb appar](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e62f1-189">For information about web project deployment, see [How toodeploy tooWeb Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="e62f1-190">toodeploy WebJobs-projekt, högerklicka på hello-projekt i **Solution Explorer** och på **Publicera som Azure Webjobs...** .</span><span class="sxs-lookup"><span data-stu-id="e62f1-190">toodeploy a WebJobs project by itself, right-click hello project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="e62f1-192">För en oberoende Webbjobb hello samma **Publicera webbplats** guiden som används för webbprojekt visas, men med färre inställningar tillgängliga toochange.</span><span class="sxs-lookup"><span data-stu-id="e62f1-192">For an independent WebJob, hello same **Publish Web** wizard that is used for web projects appears, but with fewer settings available toochange.</span></span>

## <span data-ttu-id="e62f1-193"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e62f1-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="e62f1-194">Den här artikeln har förklaras hur toodeploy WebJobs med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e62f1-194">This article has explained how toodeploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="e62f1-195">Mer information om hur toodeploy Azure WebJobs Se [Azure WebJobs - rekommenderade resurser - distribution](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="e62f1-195">For more information about how toodeploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

