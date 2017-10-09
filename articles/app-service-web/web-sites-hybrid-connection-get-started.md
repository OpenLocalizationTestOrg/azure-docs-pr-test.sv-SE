---
title: "aaaAccess lokala resurser genom att använda hybridanslutningar i Azure App Service"
description: "Skapa en anslutning mellan en webbapp i Azure App Service och en lokal resurs som använder en statisk TCP-port"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: a46ed26b-df8e-4fc3-8e05-2d002a6ee508
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: cephalin
ms.openlocfilehash: de7c57b94f4bd6250a93757817178e8455daae4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a><span data-ttu-id="d2a86-103">Få tillgång till lokala resurser med hybridanslutningar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d2a86-103">Access on-premises resources using hybrid connections in Azure App Service</span></span>
<span data-ttu-id="d2a86-104">Du kan ansluta en Azure App Service app tooany lokal resurs som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och de flesta anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="d2a86-104">You can connect an Azure App Service app tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="d2a86-105">Den här artikeln beskrivs hur du toocreate en hybridanslutning mellan App Service och en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-105">This article shows you how toocreate a hybrid connection between App Service and an on-premises SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="d2a86-106">hello Web Apps del av hello Hybridanslutningar funktionen är endast tillgänglig i hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2a86-106">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="d2a86-107">toocreate en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="d2a86-107">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span> 
> 
> <span data-ttu-id="d2a86-108">Det här innehållet gäller även tooMobile appar i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="d2a86-108">This content also applies tooMobile Apps in Azure App Service.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="d2a86-109">Krav</span><span class="sxs-lookup"><span data-stu-id="d2a86-109">Prerequisites</span></span>
* <span data-ttu-id="d2a86-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d2a86-110">An Azure subscription.</span></span> <span data-ttu-id="d2a86-111">För en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d2a86-111">For a free subscription, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
  
    <span data-ttu-id="d2a86-112">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d2a86-112">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d2a86-113">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d2a86-113">No credit cards required; no commitments.</span></span>
* <span data-ttu-id="d2a86-114">toouse en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning, måste TCP/IP toobe aktiverad på en statisk port.</span><span class="sxs-lookup"><span data-stu-id="d2a86-114">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="d2a86-115">En standardinstans rekommenderas på SQL Server eftersom den använder statisk port 1433.</span><span class="sxs-lookup"><span data-stu-id="d2a86-115">Using a default instance on SQL Server is recommended because it uses static port 1433.</span></span> <span data-ttu-id="d2a86-116">Information om hur du installerar och konfigurerar SQL Server Express för användning med hybridanslutningar finns [Anslut tooan lokala SQL Server från en Azure-webbplats med Hybridanslutningar](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="d2a86-116">For information on installing and configuring SQL Server Express for use with hybrid connections, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span>
* <span data-ttu-id="d2a86-117">hello-dator som du installerar hello lokal Hybridanslutningshanterare agent beskrivs senare i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="d2a86-117">hello computer on which you install hello on-premises Hybrid Connection Manager agent described later in this article:</span></span>
  
  * <span data-ttu-id="d2a86-118">Måste vara kan tooconnect tooAzure via port 5671</span><span class="sxs-lookup"><span data-stu-id="d2a86-118">Must be able tooconnect tooAzure over port 5671</span></span>
  * <span data-ttu-id="d2a86-119">Måste vara kan tooreach hello *värdnamn*:*portnumber* på din lokala resursen.</span><span class="sxs-lookup"><span data-stu-id="d2a86-119">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span> 

> [!NOTE]
> <span data-ttu-id="d2a86-120">hello stegen i den här artikeln förutsätter att du använder hello webbläsaren från hello-dator som är värd för hello lokalt hybrid anslutning agent.</span><span class="sxs-lookup"><span data-stu-id="d2a86-120">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="d2a86-121">Skapa en webbapp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2a86-121">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="d2a86-122">Om du redan har skapat ett webbprogram eller en mobilappsserverdel i hello Azure-portalen som du vill toouse för den här självstudiekursen, du kan gå vidare för[skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och starta därifrån.</span><span class="sxs-lookup"><span data-stu-id="d2a86-122">If you have already created a web app or Mobile App backend in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and start from there.</span></span>
> 
> 

1. <span data-ttu-id="d2a86-123">I hello övre vänstra hörnet hello [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **Web App**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-123">In hello upper left corner of hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web App**.</span></span>
   
    ![Ny webbapp][NewWebsite]
2. <span data-ttu-id="d2a86-125">På hello **webbapp** bladet anger en URL och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-125">On hello **Web app** blade, provide a URL and click **Create**.</span></span> 
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. <span data-ttu-id="d2a86-127">Hello webbprogram skapas och dess web app bladet visas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="d2a86-127">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="d2a86-128">hello bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="d2a86-128">hello blade is a vertically scrollable dashboard that lets you manage your site.</span></span>
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
4. <span data-ttu-id="d2a86-130">tooverify hello webbplatsen är aktiv, kan du klicka på hello **Bläddra** ikonen toodisplay hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="d2a86-130">tooverify hello site is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>
   
    ![Klicka på Bläddra toosee ditt webbprogram][Browse]
   
    ![Standardsida web app][DefaultWebSitePage]

<span data-ttu-id="d2a86-133">Därefter skapar du en hybridanslutning och BizTalk-tjänst för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d2a86-133">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="d2a86-134">Skapa en Hybridanslutning och BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="d2a86-134">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="d2a86-135">I ditt webb-app-blad klickar du på **alla inställningar** > **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-135">In your web app blade click on **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. <span data-ttu-id="d2a86-137">Klicka på hello Hybrid anslutningar bladet **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-137">On hello Hybrid connections blade, click **Add**.</span></span>
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. <span data-ttu-id="d2a86-138">Hej **lägga till en hybridanslutning** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-138">hello **Add a hybrid connection** blade opens.</span></span>  <span data-ttu-id="d2a86-139">Eftersom det är ditt första hybridanslutning hello **ny hybridanslutning** är förvalt och hello **skapa hybridanslutning** blad öppnas för dig.</span><span class="sxs-lookup"><span data-stu-id="d2a86-139">Since this is your first hybrid connection, hello **New hybrid connection** option is preselected, and hello **Create hybrid connection** blade opens for you.</span></span>
   
    ![Skapa en hybridanslutning][TwinCreateHCBlades]
   
    <span data-ttu-id="d2a86-141">På hello **skapa hybrid anslutning bladet**:</span><span class="sxs-lookup"><span data-stu-id="d2a86-141">On hello **Create hybrid connection blade**:</span></span>
   
   * <span data-ttu-id="d2a86-142">För **namn**, ange ett namn för hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d2a86-142">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="d2a86-143">För **värdnamn**, ange hello namnet på hello lokala dator som är värd för din resurs.</span><span class="sxs-lookup"><span data-stu-id="d2a86-143">For **Hostname**, enter hello name of hello on-premises computer that hosts your resource.</span></span>
   * <span data-ttu-id="d2a86-144">För **Port**, ange hello portnummer som använder lokal resurs (1433 för en SQL Server-standardinstans).</span><span class="sxs-lookup"><span data-stu-id="d2a86-144">For **Port**, enter hello port number that your on-premises resource uses (1433 for a SQL Server default instance).</span></span>
   * <span data-ttu-id="d2a86-145">Klicka på **Biz kontakta Service**</span><span class="sxs-lookup"><span data-stu-id="d2a86-145">Click **Biz Talk Service**</span></span>
4. <span data-ttu-id="d2a86-146">Hej **skapa BizTalk-tjänst** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-146">hello **Create BizTalk Service** blade opens.</span></span> <span data-ttu-id="d2a86-147">Ange ett namn för hello BizTalk-tjänst och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-147">Enter a name for hello BizTalk service, and then click **OK**.</span></span>
   
    ![Skapa BizTalk-tjänst][CreateHCCreateBTS]
   
    <span data-ttu-id="d2a86-149">Hej **skapa BizTalk-tjänst** blad stängs och du kommer tillbaka toohello **skapa hybridanslutning** bladet.</span><span class="sxs-lookup"><span data-stu-id="d2a86-149">hello **Create BizTalk Service** blade closes and you are returned toohello **Create hybrid connection** blade.</span></span>
5. <span data-ttu-id="d2a86-150">Klicka på hello skapa hybrid anslutning bladet **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-150">On hello Create hybrid connection blade, click **OK**.</span></span> 
   
    ![Klicka på OK][CreateBTScomplete]
6. <span data-ttu-id="d2a86-152">När hello är klart informerar hello meddelandefältet i hello Portal dig om att hello anslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2a86-152">When hello process completes, hello notifications area in hello Portal informs you that hello connection has been successfully created.</span></span>
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in hello dogfood portal. I switch toohello classic portal
    (full portal) and created hello BizTalk service but it doesn't seem toolet you connnect them - When you finish the
    Create hybrid conn step, you get hello following error
    Failed toocreate hybrid connection RelecIoudHC. hello 
    resource type could not be found in hello namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    hello error indicates it couldn't find hello type, not hello instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. <span data-ttu-id="d2a86-153">På hello webbapps blad hello **hybridanslutningar** ikon visas nu att 1 hybridanslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2a86-153">On hello web app's blade, hello **Hybrid connections** icon now shows that 1 hybrid connection has been created.</span></span>
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

<span data-ttu-id="d2a86-155">Du har nu slutfört en viktig del av hello molninfrastruktur hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d2a86-155">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="d2a86-156">Därefter skapar du en motsvarande typ lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2a86-156">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="d2a86-157">Installera hello lokal Hybridanslutningshanterare toocomplete hello anslutning</span><span class="sxs-lookup"><span data-stu-id="d2a86-157">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
1. <span data-ttu-id="d2a86-158">Klicka på hello webbapps blad **alla inställningar** > **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-158">On hello web app's blade, click **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span> 
   
    ![Ikon för hybrid-anslutningar][HCIcon]
2. <span data-ttu-id="d2a86-160">På hello **hybridanslutningar** bladet, hello **Status** för hello nyligen har lagt till slutpunkten visar **inte ansluten**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-160">On hello **Hybrid connections** blade, hello **Status** column for hello recently added endpoint shows **Not connected**.</span></span> <span data-ttu-id="d2a86-161">Klicka på hello anslutning tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="d2a86-161">Click hello connection tooconfigure it.</span></span>
   
    ![Inte ansluten][NotConnected]
   
    <span data-ttu-id="d2a86-163">hello Hybrid anslutning blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-163">hello Hybrid connection blade opens.</span></span>
   
    ![NotConnectedBlade][NotConnectedBlade]
3. <span data-ttu-id="d2a86-165">På hello-bladet klickar du på **lyssnare installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-165">On hello blade, click **Listener Setup**.</span></span>
   
    ![Klicka på lyssnaren installationsprogrammet][ClickListenerSetup]
4. <span data-ttu-id="d2a86-167">Hej **Hybrid anslutningsegenskaper** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-167">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="d2a86-168">Under **på lokal Hybridanslutningshanterare**, Välj **Klicka här tooinstall**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-168">Under **On-premises Hybrid Connection Manager**, choose **Click here tooinstall**.</span></span>
   
    ![Klicka på tooinstall][ClickToInstallHCM]
5. <span data-ttu-id="d2a86-170">Hello programmet köras varning dialogrutan säkerhet, Välj **kör** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="d2a86-170">In hello Application Run security warning dialog, choose **Run** toocontinue.</span></span>
   
    ![Välj Kör toocontinue][ApplicationRunWarning]
6. <span data-ttu-id="d2a86-172">I hello **User Account Control** dialogrutan Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-172">In hello **User Account Control** dialog, choose **Yes**.</span></span>
   
   ![Välj Ja][UAC]
7. <span data-ttu-id="d2a86-174">Hej Hybridanslutningshanteraren hämtas och installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d2a86-174">hello Hybrid Connection Manager is downloaded and installed for you.</span></span> 
   
    ![Installera][HCMInstalling]
8. <span data-ttu-id="d2a86-176">När hello-installationen har slutförts klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-176">When hello install completes, click **Close**.</span></span>
   
    ![Klicka på Stäng][HCMInstallComplete]
   
    <span data-ttu-id="d2a86-178">På hello **hybridanslutningar** bladet, hello **Status** kolumnen visar nu **ansluten**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-178">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Anslutna Status][HCStatusConnected]

<span data-ttu-id="d2a86-180">Nu hello hybrid anslutning infrastrukturen är klar kan skapa du en hybridprogram som använder den.</span><span class="sxs-lookup"><span data-stu-id="d2a86-180">Now that hello hybrid connection infrastructure is complete, you can create a hybrid application that uses it.</span></span> 

> [!NOTE]
> <span data-ttu-id="d2a86-181">hello följande avsnitt visar hur toouse en hybridanslutning med ett projekt för Mobile Apps .NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="d2a86-181">hello following sections show you how toouse a hybrid connection with a Mobile Apps .NET backend project.</span></span>
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a><span data-ttu-id="d2a86-182">Konfigurera hello Mobile App .NET-serverdel projektet tooconnect toohello SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="d2a86-182">Configure hello Mobile App .NET backend project tooconnect toohello SQL Server database</span></span>
<span data-ttu-id="d2a86-183">I App Service är en Mobile Apps .NET serverdelsprojektet bara en ASP.NET webbapp med en ytterligare Mobile Apps-SDK har installerats och initierats.</span><span class="sxs-lookup"><span data-stu-id="d2a86-183">In App Service, a Mobile Apps .NET backend project is just an ASP.NET web app with an additional Mobile Apps SDK installed and initialized.</span></span> <span data-ttu-id="d2a86-184">toouse ditt webbprogram som en Mobile Apps-serverdel måste du [ladda ned och initiera hello Mobile Apps .NET-serverdel SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span><span class="sxs-lookup"><span data-stu-id="d2a86-184">toouse your web app as a Mobile Apps backend, you must [download and initialize hello Mobile Apps .NET backend SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span></span>  

<span data-ttu-id="d2a86-185">För Mobilappar du även behöver toodefine en anslutningssträng för hello lokala-databasen och ändra hello backend toouse den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d2a86-185">For Mobile Apps, you also need toodefine a connection string for hello on-premises database and modify hello backend toouse this connection.</span></span> 

1. <span data-ttu-id="d2a86-186">Leta upp hello i Solution Explorer i Visual Studio, öppna hello Web.config-filen för din mobila App .NET-serverdel **connectionStrings** lägger du till en ny post i SqlClient som hello nedan, vilken punkter toohello lokalt SQL Server-databas:</span><span class="sxs-lookup"><span data-stu-id="d2a86-186">In Solution Explorer in Visual Studio, open hello Web.config file for your Mobile App .NET backend, locate hello **connectionStrings** section, add a new SqlClient entry like hello following, which points toohello on-premises SQL Server database:</span></span>
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    <span data-ttu-id="d2a86-187">Kom ihåg tooreplace `<**secure_password**>` i den här strängen med hello lösenordet du skapade för *HybridConnectionLogin*.</span><span class="sxs-lookup"><span data-stu-id="d2a86-187">Remember tooreplace `<**secure_password**>` in this string with hello password you created for  *HybridConnectionLogin*.</span></span>
2. <span data-ttu-id="d2a86-188">Klicka på **spara** i Visual Studio toosave hello Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="d2a86-188">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d2a86-189">Den här anslutningsinställningen används när körs på hello lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d2a86-189">This connection setting is used when running on hello local computer.</span></span> <span data-ttu-id="d2a86-190">När den körs i Azure är den här inställningen åsidosatts av hello anslutningsinställningen som definierats i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2a86-190">When running in Azure, this setting is overriden by hello connection setting defined in hello portal.</span></span>
   > 
   > 
3. <span data-ttu-id="d2a86-191">Expandera hello **modeller** mapp och öppna hello modellen datafil som slutar i *Context.cs*.</span><span class="sxs-lookup"><span data-stu-id="d2a86-191">Expand hello **Models** folder and open hello data model file, which ends in *Context.cs*.</span></span>
4. <span data-ttu-id="d2a86-192">Ändra hello **DbContext** instans-konstruktorn toopass hello värdet `OnPremisesDBConnection` toohello grundläggande **DbContext** konstruktor, liknande toohello följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="d2a86-192">Modify hello **DbContext** instance constructor toopass hello value `OnPremisesDBConnection` toohello base **DbContext** constructor, similar toohello following snippet:</span></span>
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    <span data-ttu-id="d2a86-193">hello-tjänsten använder nu hello ny anslutning toohello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="d2a86-193">hello service will now use hello new connection toohello SQL Server database.</span></span>

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a><span data-ttu-id="d2a86-194">Uppdatera hello Mobilapp backend toouse hello lokala anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="d2a86-194">Update hello Mobile App backend toouse hello on-premises connection string</span></span>
<span data-ttu-id="d2a86-195">Därefter behöver du tooadd en appinställning för den här nya anslutningssträngen så att den kan användas från Azure.</span><span class="sxs-lookup"><span data-stu-id="d2a86-195">Next, you need tooadd an app setting for this new connection string so that it can be used from Azure.</span></span>  

1. <span data-ttu-id="d2a86-196">Tillbaka i hello [Azure-portalen](https://portal.azure.com) i hello web app backend-kod för Mobilappen, klickar du på **alla inställningar**, sedan **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d2a86-196">Back in hello [Azure portal](https://portal.azure.com) in hello web app backend code for your Mobile App, click **All settings**, then **Application settings**.</span></span>
2. <span data-ttu-id="d2a86-197">I hello **Web app-inställningar** bladet rulla nedåt för**anslutningssträngar** och lägga till en ny **SQL Server** anslutningssträngen med namnet `OnPremisesDBConnection` med ett värde som `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span><span class="sxs-lookup"><span data-stu-id="d2a86-197">In hello **Web app settings** blade, scroll down too**Connection strings** and add an new **SQL Server** connection string named `OnPremisesDBConnection` with a value like `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span></span>
   
    <span data-ttu-id="d2a86-198">Ersätt `<**secure_password**>` med hello säkert lösenord för lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="d2a86-198">Replace `<**secure_password**>` with hello secure password for your on-premises database.</span></span>
   
    ![Anslutningssträngen för lokala-databasen](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. <span data-ttu-id="d2a86-200">Tryck på **spara** toosave hello hybridanslutning och anslutningssträngen du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="d2a86-200">Press **Save** toosave hello hybrid connection and connection string you just created.</span></span>

<span data-ttu-id="d2a86-201">Nu kan du publicera hello serverprojekt och testa hello ny anslutning med dina befintliga Mobile Apps-klienter.</span><span class="sxs-lookup"><span data-stu-id="d2a86-201">At this point you can republish hello server project and test hello new connection with your existing Mobile Apps clients.</span></span> <span data-ttu-id="d2a86-202">Data läses och skrivs toohello lokal databas med hjälp av hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="d2a86-202">Data will be read from and written toohello on-premises database using hello hybrid connection.</span></span>

<a name="NextSteps"></a>

## <a name="next-steps"></a><span data-ttu-id="d2a86-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2a86-203">Next Steps</span></span>
* <span data-ttu-id="d2a86-204">Information om hur du skapar ett ASP.NET-webbprogram som använder en hybridanslutning finns [Anslut tooan lokala SQL Server från en Azure-webbplats med Hybridanslutningar](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="d2a86-204">For information on creating an ASP.NET web application that uses a hybrid connection, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span> 

### <a name="additional-resources"></a><span data-ttu-id="d2a86-205">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2a86-205">Additional Resources</span></span>
[<span data-ttu-id="d2a86-206">Översikt över hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="d2a86-206">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="d2a86-207">Josh Twist introducerar hybridanslutningar (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="d2a86-207">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="d2a86-208">Webbplats för hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="d2a86-208">Hybrid Connections web site</span></span>](https://azure.microsoft.com/services/biztalk-services/)

[<span data-ttu-id="d2a86-209">BizTalk-tjänst: Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar</span><span class="sxs-lookup"><span data-stu-id="d2a86-209">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="d2a86-210">Skapa en verklig Hybridmoln med sömlös programmet Portability (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="d2a86-210">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="d2a86-211">Ansluta tooan lokala SQL Server från Azure Mobile Services med hjälp av Hybridanslutningar (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="d2a86-211">Connect tooan on-premises SQL Server from Azure Mobile Services using Hybrid Connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a><span data-ttu-id="d2a86-212">Nyheter</span><span class="sxs-lookup"><span data-stu-id="d2a86-212">What's changed</span></span>
* <span data-ttu-id="d2a86-213">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="d2a86-213">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
