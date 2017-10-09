---
title: aaaCreate en CI/CD-pipeline i Azure med Team Services | Microsoft Docs
description: "Lär dig hur toocreate en Visual Studio Team Services pipeline för kontinuerlig integration och som distribuerar ett web app tooIIS på en Windows VM"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a><span data-ttu-id="272ed-103">Skapa en pipeline för kontinuerlig integrering med Visual Studio Team Services och IIS</span><span class="sxs-lookup"><span data-stu-id="272ed-103">Create a continuous integration pipeline with Visual Studio Team Services and IIS</span></span>
<span data-ttu-id="272ed-104">Du kan använda en kontinuerlig integrering och distribution (CI/CD) pipeline tooautomate hello build-, test- och faser för distribution av programutveckling.</span><span class="sxs-lookup"><span data-stu-id="272ed-104">tooautomate hello build, test, and deployment phases of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="272ed-105">I den här självstudiekursen skapar du en CI/CD-pipeline med hjälp av Visual Studio Team Services och en Windows-dator (VM) i Azure som kör IIS.</span><span class="sxs-lookup"><span data-stu-id="272ed-105">In this tutorial, you create a CI/CD pipeline using Visual Studio Team Services and a Windows virtual machine (VM) in Azure that runs IIS.</span></span> <span data-ttu-id="272ed-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="272ed-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="272ed-107">Publicera ett ASP.NET-program tooa Team Services webbprojekt</span><span class="sxs-lookup"><span data-stu-id="272ed-107">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="272ed-108">Skapa en definition av build som utlöses av koden incheckningar</span><span class="sxs-lookup"><span data-stu-id="272ed-108">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="272ed-109">Installera och konfigurera IIS på en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="272ed-109">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="272ed-110">Lägg till hello IIS instans tooa distributionsgruppen i Team Services</span><span class="sxs-lookup"><span data-stu-id="272ed-110">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="272ed-111">Skapa en ny webbplats för versionen definition toopublish distribuera paket tooIIS</span><span class="sxs-lookup"><span data-stu-id="272ed-111">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="272ed-112">Testa hello CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="272ed-112">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="272ed-113">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="272ed-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="272ed-114">Kör `Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="272ed-114">Run `Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="272ed-115">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="272ed-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="create-project-in-team-services"></a><span data-ttu-id="272ed-116">Skapa projekt i Team Services</span><span class="sxs-lookup"><span data-stu-id="272ed-116">Create project in Team Services</span></span>
<span data-ttu-id="272ed-117">Visual Studio Team Services möjliggör enkel samarbete och utveckling utan att behålla en hanteringslösning för lokal kod.</span><span class="sxs-lookup"><span data-stu-id="272ed-117">Visual Studio Team Services allows for easy collaboration and development without maintaining an on-premises code management solution.</span></span> <span data-ttu-id="272ed-118">Team Services innehåller molnet kod testning, bygga och application insights.</span><span class="sxs-lookup"><span data-stu-id="272ed-118">Team Services provides cloud code testing, build, and application insights.</span></span> <span data-ttu-id="272ed-119">Du kan välja en version kontrollen lagringsplatsen och IDE som bäst passar din kod utveckling.</span><span class="sxs-lookup"><span data-stu-id="272ed-119">You can choose a version control repo and IDE that best fits your code development.</span></span> <span data-ttu-id="272ed-120">Du kan använda ett kostnadsfritt konto toocreate en grundläggande ASP.NET webbapp och CI/CD-pipeline för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="272ed-120">For this tutorial, you can use a free account toocreate a basic ASP.NET web app and CI/CD pipeline.</span></span> <span data-ttu-id="272ed-121">Om du inte redan har ett Team Services-konto [skapar du en](http://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="272ed-121">If you do not already have a Team Services account, [create one](http://go.microsoft.com/fwlink/?LinkId=307137).</span></span>

<span data-ttu-id="272ed-122">toomanage hello kod commit processen skapa definitioner, och viktiga definitioner, skapa ett projekt i Team Services på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="272ed-122">toomanage hello code commit process, build definitions, and release definitions, create a project in Team Services as follows:</span></span>

1. <span data-ttu-id="272ed-123">Öppna instrumentpanelen Team Services i en webbläsare och välj **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="272ed-123">Open your Team Services dashboard in a web browser and choose **New project**.</span></span>
2. <span data-ttu-id="272ed-124">Ange *myWebApp* för hello **projektnamn**.</span><span class="sxs-lookup"><span data-stu-id="272ed-124">Enter *myWebApp* for hello **Project name**.</span></span> <span data-ttu-id="272ed-125">Lämna alla andra standard värden toouse *Git* versionskontroll och *Agile* process för arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="272ed-125">Leave all other default values toouse *Git* version control and *Agile* work item process.</span></span>
3. <span data-ttu-id="272ed-126">Välj hello alternativet för**dela med** *gruppmedlemmar*och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-126">Choose hello option too**Share with** *Team Members*, then select **Create**.</span></span>
5. <span data-ttu-id="272ed-127">När projektet har skapats, Välj hello alternativet för**initieras med en viktigt- eller gitignore**, sedan **initiera**.</span><span class="sxs-lookup"><span data-stu-id="272ed-127">Once your project has been created, choose hello option too**Initialize with a README or gitignore**, then **Initialize**.</span></span>
6. <span data-ttu-id="272ed-128">I det nya projektet, Välj **instrumentpaneler** hello överst, Välj **öppna i Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="272ed-128">Inside your new project, choose **Dashboards** across hello top, then select **Open in Visual Studio**.</span></span>


## <a name="create-aspnet-web-application"></a><span data-ttu-id="272ed-129">Skapa ASP.NET-webbprogram</span><span class="sxs-lookup"><span data-stu-id="272ed-129">Create ASP.NET web application</span></span>
<span data-ttu-id="272ed-130">I föregående steg hello skapat du ett projekt i Team Services.</span><span class="sxs-lookup"><span data-stu-id="272ed-130">In hello previous step, you created a project in Team Services.</span></span> <span data-ttu-id="272ed-131">hello sista steget öppnar det nya projektet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="272ed-131">hello final step opens your new project in Visual Studio.</span></span> <span data-ttu-id="272ed-132">Du hanterar din kod incheckningar i hello **Team Explorer** fönster.</span><span class="sxs-lookup"><span data-stu-id="272ed-132">You manage your code commits in hello **Team Explorer** window.</span></span> <span data-ttu-id="272ed-133">Skapa en lokal kopia av det nya projektet och sedan skapa en ASP.NET-webbprogram från en mall på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="272ed-133">Create a local copy of your new project, then create an ASP.NET web application from a template as follows:</span></span>

1. <span data-ttu-id="272ed-134">Välj **klona** toocreate en lokal git-lagringsplatsen i ditt Team Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="272ed-134">Select **Clone** toocreate a local git repo of your Team Services project.</span></span>
    
    ![Klona lagringsplatsen från Team Services-projekt](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. <span data-ttu-id="272ed-136">Under **lösningar**väljer **ny**.</span><span class="sxs-lookup"><span data-stu-id="272ed-136">Under **Solutions**, select **New**.</span></span>

    ![Skapa-webbprogramslösning](media/tutorial-vsts-iis-cicd/new_solution.png)

3. <span data-ttu-id="272ed-138">Välj **Web** mallar, och välj sedan hello **ASP.NET-webbprogram** mall.</span><span class="sxs-lookup"><span data-stu-id="272ed-138">Select **Web** templates, and then choose hello **ASP.NET Web Application** template.</span></span>
    1. <span data-ttu-id="272ed-139">Ange ett namn för programmet, till exempel *myWebApp*, och avmarkerar kryssrutan hello för **Skapa katalog för lösning**.</span><span class="sxs-lookup"><span data-stu-id="272ed-139">Enter a name for your application, such as *myWebApp*, and uncheck hello box for **Create directory for solution**.</span></span>
    2. <span data-ttu-id="272ed-140">Om hello alternativet är tillgängligt, avmarkerar du kryssrutan för hello för**Lägg till Application Insights tooproject**.</span><span class="sxs-lookup"><span data-stu-id="272ed-140">If hello option is available, uncheck hello box too**Add Application Insights tooproject**.</span></span> <span data-ttu-id="272ed-141">Application Insights kräver du tooauthorize ditt webbprogram med Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="272ed-141">Application Insights requires you tooauthorize your web application with Azure Application Insights.</span></span> <span data-ttu-id="272ed-142">tookeep den enkla i den här självstudiekursen, hoppa över den här processen.</span><span class="sxs-lookup"><span data-stu-id="272ed-142">tookeep it simple in this tutorial, skip this process.</span></span>
    3. <span data-ttu-id="272ed-143">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="272ed-143">Select **OK**.</span></span>
4. <span data-ttu-id="272ed-144">Välj **MVC** hello mall-listan.</span><span class="sxs-lookup"><span data-stu-id="272ed-144">Choose **MVC** from hello template list.</span></span>
    1. <span data-ttu-id="272ed-145">Välj **ändra autentisering**, Välj **ingen autentisering**och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="272ed-145">Select **Change Authentication**, choose **No Authentication**, then select **OK**.</span></span>
    2. <span data-ttu-id="272ed-146">Välj **OK** toocreate din lösning.</span><span class="sxs-lookup"><span data-stu-id="272ed-146">Select **OK** toocreate your solution.</span></span>
5. <span data-ttu-id="272ed-147">I hello **Team Explorer** fönstret Välj **ändringar**.</span><span class="sxs-lookup"><span data-stu-id="272ed-147">In hello **Team Explorer** window, choose **Changes**.</span></span>

    ![Genomför lokala ändringar tooTeam Services git repo](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. <span data-ttu-id="272ed-149">Hello genomförande i textrutan anger du ett meddelande som *inledande commit*.</span><span class="sxs-lookup"><span data-stu-id="272ed-149">In hello commit text box, enter a message such as *Initial commit*.</span></span> <span data-ttu-id="272ed-150">Välj **genomför alla och Sync** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="272ed-150">Choose **Commit All and Sync** from hello drop-down menu.</span></span>


## <a name="create-build-definition"></a><span data-ttu-id="272ed-151">Skapa build-definition</span><span class="sxs-lookup"><span data-stu-id="272ed-151">Create build definition</span></span>
<span data-ttu-id="272ed-152">I Team Services använder du en build definition toooutline hur programmet ska byggas.</span><span class="sxs-lookup"><span data-stu-id="272ed-152">In Team Services, you use a build definition toooutline how your application should be built.</span></span> <span data-ttu-id="272ed-153">I den här självstudiekursen skapar vi en grundläggande definition att tar våra källkoden bygger hello lösning och sedan skapar web distribuera paket som vi kan använda toorun hello webbprogram på en IIS-server.</span><span class="sxs-lookup"><span data-stu-id="272ed-153">In this tutorial, we create a basic definition that takes our source code, builds hello solution, then creates web deploy package we can use toorun hello web app on an IIS server.</span></span>

1. <span data-ttu-id="272ed-154">I projektet Team Services väljer **Skapa & släpper** hello överst, Välj **bygger**.</span><span class="sxs-lookup"><span data-stu-id="272ed-154">In your Team Services project, choose **Build & Release** across hello top, then select **Builds**.</span></span>
3. <span data-ttu-id="272ed-155">Välj **+ ny definition**.</span><span class="sxs-lookup"><span data-stu-id="272ed-155">Select **+ New definition**.</span></span>
4. <span data-ttu-id="272ed-156">Välj hello **ASP.NET (FÖRHANDSGRANSKNING)** mall och välj **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-156">Choose hello **ASP.NET (PREVIEW)** template and select **Apply**.</span></span>
5. <span data-ttu-id="272ed-157">Lämna alla hello standard uppgiften värden.</span><span class="sxs-lookup"><span data-stu-id="272ed-157">Leave all hello default task values.</span></span> <span data-ttu-id="272ed-158">Under **hämta källor**, se till att hello *myWebApp* databasen och *master* grenen väljs.</span><span class="sxs-lookup"><span data-stu-id="272ed-158">Under **Get sources**, ensure that hello *myWebApp* repository and *master* branch are selected.</span></span>

    ![Skapa build-definition i Team Services-projekt](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. <span data-ttu-id="272ed-160">På hello **utlösare** fliken, flytta Hej reglage för **aktivera den här utlösaren** för*aktiverad*.</span><span class="sxs-lookup"><span data-stu-id="272ed-160">On hello **Triggers** tab, move hello slider for **Enable this trigger** too*Enabled*.</span></span>
7. <span data-ttu-id="272ed-161">Spara hello build definitions- och kön en ny version genom att välja **Spara & kö** , sedan **Spara & kö** igen.</span><span class="sxs-lookup"><span data-stu-id="272ed-161">Save hello build definition and queue a new build by selecting **Save & queue** , then **Save & queue** again.</span></span> <span data-ttu-id="272ed-162">Lämna hello standardvärden och välj **kön**.</span><span class="sxs-lookup"><span data-stu-id="272ed-162">Leave hello defaults and select **Queue**.</span></span>

<span data-ttu-id="272ed-163">Titta på startar som hello build schemaläggs på en värdbaserad agent sedan toobuild.</span><span class="sxs-lookup"><span data-stu-id="272ed-163">Watch as hello build is scheduled on a hosted agent, then begins toobuild.</span></span> <span data-ttu-id="272ed-164">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="272ed-164">hello output is similar toohello following example:</span></span>

![Lyckad version av Team Services-projekt](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a><span data-ttu-id="272ed-166">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="272ed-166">Create virtual machine</span></span>
<span data-ttu-id="272ed-167">tooprovide en plattform toorun ditt ASP.NET-webbprogram måste en virtuell Windows-dator som kör IIS.</span><span class="sxs-lookup"><span data-stu-id="272ed-167">tooprovide a platform toorun your ASP.NET web app, you need a Windows virtual machine that runs IIS.</span></span> <span data-ttu-id="272ed-168">Team Services använder en agent toointeract med hello IIS-instans som du kopplar kod och versioner har utlösts.</span><span class="sxs-lookup"><span data-stu-id="272ed-168">Team Services uses an agent toointeract with hello IIS instance as you commit code and builds are triggered.</span></span>

<span data-ttu-id="272ed-169">Skapa en virtuell Windows Server 2016-dator med hjälp av [detta skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="272ed-169">Create a Windows Server 2016 VM using [this script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span></span> <span data-ttu-id="272ed-170">Det tar några minuter för hello skriptet toorun och skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="272ed-170">It takes a few minutes for hello script toorun and create hello VM.</span></span> <span data-ttu-id="272ed-171">När hello VM har skapats, öppna port 80 för webbtrafik med [Lägg till AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="272ed-171">Once hello VM has been created, open port 80 for web traffic with [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) as follows:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

<span data-ttu-id="272ed-172">tooconnect tooyour VM, hämta hello offentliga IP-adress med [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="272ed-172">tooconnect tooyour VM, obtain hello public IP address with [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) as follows:</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

<span data-ttu-id="272ed-173">Skapa en fjärrskrivbordssession tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="272ed-173">Create a remote desktop session tooyour VM:</span></span>

```cmd
mstsc /v:<publicIpAddress>
```

<span data-ttu-id="272ed-174">Öppna på hello VM, en **administratör PowerShell** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="272ed-174">On hello VM, open an **Administrator PowerShell** command prompt.</span></span> <span data-ttu-id="272ed-175">Installera IIS och nödvändiga .NET-funktioner på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="272ed-175">Install IIS and required .NET features as follows:</span></span>

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a><span data-ttu-id="272ed-176">Skapa distributionsgrupp</span><span class="sxs-lookup"><span data-stu-id="272ed-176">Create deployment group</span></span>
<span data-ttu-id="272ed-177">toopush ut hello web driftsätta paketet toohello IIS-server kan du definiera en distributionsgrupp i Team Services.</span><span class="sxs-lookup"><span data-stu-id="272ed-177">toopush out hello web deploy package toohello IIS server, you define a deployment group in Team Services.</span></span> <span data-ttu-id="272ed-178">Den här gruppen kan du toospecify vilka servrar som hello mål för nya versioner som du har checkat kod tooTeam tjänster och versioner är slutförda.</span><span class="sxs-lookup"><span data-stu-id="272ed-178">This group allows you toospecify which servers are hello target of new builds as you commit code tooTeam Services and builds are completed.</span></span>

1. <span data-ttu-id="272ed-179">I Team Services väljer **Skapa & släpper** och välj sedan **distributionsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="272ed-179">In Team Services, choose **Build & Release** and then select **Deployment groups**.</span></span>
2. <span data-ttu-id="272ed-180">Välj **lägga till distributionsgruppen**.</span><span class="sxs-lookup"><span data-stu-id="272ed-180">Choose **Add Deployment group**.</span></span>
3. <span data-ttu-id="272ed-181">Ange ett namn för hello gruppen som *myIIS*och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-181">Enter a name for hello group, such as *myIIS*, then select **Create**.</span></span>
4. <span data-ttu-id="272ed-182">I hello **registrera datorer** Kontrollera *Windows* är markerat och klicka sedan på kryssrutan hello för**använder en personlig åtkomsttoken i hello skript för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="272ed-182">In hello **Register machines** section, ensure *Windows* is selected, then check hello box too**Use a personal access token in hello script for authentication**.</span></span>
5. <span data-ttu-id="272ed-183">Välj **kopiera skriptet tooclipboard**.</span><span class="sxs-lookup"><span data-stu-id="272ed-183">Select **Copy script tooclipboard**.</span></span>


### <a name="add-iis-vm-toohello-deployment-group"></a><span data-ttu-id="272ed-184">Lägg till IIS VM toohello distributionsgruppen</span><span class="sxs-lookup"><span data-stu-id="272ed-184">Add IIS VM toohello deployment group</span></span>
<span data-ttu-id="272ed-185">Lägg till varje IIS-instans toohello grupp med hello distributionsgruppen har skapats.</span><span class="sxs-lookup"><span data-stu-id="272ed-185">With hello deployment group created, add each IIS instance toohello group.</span></span> <span data-ttu-id="272ed-186">Team Services genererar ett skript som hämtar och konfigurerar en agent på hello VM som tar emot nya distribuera paket och använder sedan den tooIIS.</span><span class="sxs-lookup"><span data-stu-id="272ed-186">Team Services generates a script that downloads and configures an agent on hello VM that receives new web deploy packages then applies it tooIIS.</span></span>

1. <span data-ttu-id="272ed-187">Tillbaka i hello **administratör PowerShell** session på den virtuella datorn, klistra in och kör hello-skript som kopieras från Team Services.</span><span class="sxs-lookup"><span data-stu-id="272ed-187">Back in hello **Administrator PowerShell** session on your VM, paste and run hello script copied from Team Services.</span></span>
2. <span data-ttu-id="272ed-188">När begärd tooconfigure taggar för hello agent väljer *Y* och ange *web*.</span><span class="sxs-lookup"><span data-stu-id="272ed-188">When prompted tooconfigure tags for hello agent, choose *Y* and enter *web*.</span></span>
3. <span data-ttu-id="272ed-189">När du uppmanas att ange hello användarkonto trycker du på *returnera* tooaccept hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="272ed-189">When prompted for hello user account, press *Return* tooaccept hello defaults.</span></span>
4. <span data-ttu-id="272ed-190">Vänta tills hello skriptet toofinish med ett meddelande *vstsagent.account.computername för tjänsten har startats*.</span><span class="sxs-lookup"><span data-stu-id="272ed-190">Wait for hello script toofinish with a message *Service vstsagent.account.computername started successfully*.</span></span>
5. <span data-ttu-id="272ed-191">I hello **distributionsgrupper** sidan hello **Skapa & släpper** -menyn, öppna hello *myIIS* distributionsgruppen.</span><span class="sxs-lookup"><span data-stu-id="272ed-191">In hello **Deployment groups** page of hello **Build & Release** menu, open hello *myIIS* deployment group.</span></span> <span data-ttu-id="272ed-192">På hello **datorer** kontrollerar du att den virtuella datorn visas.</span><span class="sxs-lookup"><span data-stu-id="272ed-192">On hello **Machines** tab, verify that your VM is listed.</span></span>

    ![TooTeam Services distributionsgruppen har lagts till VM](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a><span data-ttu-id="272ed-194">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="272ed-194">Create release definition</span></span>
<span data-ttu-id="272ed-195">toopublish din versioner du skapa en definition av versionen i Team Services.</span><span class="sxs-lookup"><span data-stu-id="272ed-195">toopublish your builds, you create a release definition in Team Services.</span></span> <span data-ttu-id="272ed-196">Den här definitionen utlöses automatiskt av en lyckad version av programmet.</span><span class="sxs-lookup"><span data-stu-id="272ed-196">This definition is triggered automatically by a successful build of your application.</span></span> <span data-ttu-id="272ed-197">Du väljer hello distribution grupp toopush webbplatsen distribuera paketet till och definiera hello IIS-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="272ed-197">You choose hello deployment group toopush your web deploy package to, and define hello appropriate IIS settings.</span></span>

1. <span data-ttu-id="272ed-198">Välj **Skapa & släpper**och välj **bygger**.</span><span class="sxs-lookup"><span data-stu-id="272ed-198">Choose **Build & Release**, then select **Builds**.</span></span> <span data-ttu-id="272ed-199">Välj hello build definition skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="272ed-199">Choose hello build definition created in a previous step.</span></span>
2. <span data-ttu-id="272ed-200">Under **nyligen slutförts**, Välj hello senaste build och markerar sedan **versionen**.</span><span class="sxs-lookup"><span data-stu-id="272ed-200">Under **Recently completed**, choose hello most recent build, then select **Release**.</span></span>
3. <span data-ttu-id="272ed-201">Välj **Ja** toocreate en definition av versionen.</span><span class="sxs-lookup"><span data-stu-id="272ed-201">Choose **Yes** toocreate a release definition.</span></span>
4. <span data-ttu-id="272ed-202">Välj hello **tom** mall och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-202">Choose hello **Empty** template, then select **Next**.</span></span>
5. <span data-ttu-id="272ed-203">Kontrollera hello projektet och källa build-definition fylls med projektet.</span><span class="sxs-lookup"><span data-stu-id="272ed-203">Verify hello project and source build definition are populated with your project.</span></span>
6. <span data-ttu-id="272ed-204">Välj hello **kontinuerlig distribution** kryssrutan och välj sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-204">Select hello **Continuous deployment** check box, then select **Create**.</span></span>
7. <span data-ttu-id="272ed-205">Välj hello listrutan bredvid för**+ Lägg till aktiviteter** och välj *lägga till en distribution grupp fas*.</span><span class="sxs-lookup"><span data-stu-id="272ed-205">Select hello drop-down box next too**+ Add tasks** and choose *Add a deployment group phase*.</span></span>
    
    ![Lägg till toorelease aktivitetsdefinitionen i Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. <span data-ttu-id="272ed-207">Välj **Lägg till** nästa för**IIS Web App Deploy(Preview)**och välj **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="272ed-207">Choose **Add** next too**IIS Web App Deploy(Preview)**, then select **Close**.</span></span>
9. <span data-ttu-id="272ed-208">Välj hello **körs på distributionsgruppen** överordnad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="272ed-208">Select hello **Run on deployment group** parent task.</span></span>
    1. <span data-ttu-id="272ed-209">För **distributionsgruppen**väljer hello distribution gruppen du skapade tidigare, såsom *myIIS*.</span><span class="sxs-lookup"><span data-stu-id="272ed-209">For **Deployment Group**, select hello deployment group you created earlier, such as *myIIS*.</span></span>
    2. <span data-ttu-id="272ed-210">I hello **datorn taggar** väljer **Lägg till** och välj hello *web* tagg.</span><span class="sxs-lookup"><span data-stu-id="272ed-210">In hello **Machine tags** box, select **Add** and choose hello *web* tag.</span></span>
    
    ![Släpp definition distribution grupp uppgiften för IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. <span data-ttu-id="272ed-212">Välj hello **distribuera: IIS Web App Deploy** aktivitet tooconfigure din IIS-instansen följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="272ed-212">Select hello **Deploy: IIS Web App Deploy** task tooconfigure your IIS instance settings as follows:</span></span>
    1. <span data-ttu-id="272ed-213">För **webbplatsnamn**, ange *standardwebbplats*.</span><span class="sxs-lookup"><span data-stu-id="272ed-213">For **Website Name**, enter *Default Web Site*.</span></span>
    2. <span data-ttu-id="272ed-214">Lämna alla hello andra standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="272ed-214">Leave all hello other default settings.</span></span>
12. <span data-ttu-id="272ed-215">Välj **spara**och välj **OK** två gånger.</span><span class="sxs-lookup"><span data-stu-id="272ed-215">Choose **Save**, then select **OK** twice.</span></span>


## <a name="create-a-release-and-publish"></a><span data-ttu-id="272ed-216">Skapa en version och publicera</span><span class="sxs-lookup"><span data-stu-id="272ed-216">Create a release and publish</span></span>
<span data-ttu-id="272ed-217">Du kan nu push webbplatsen distribuera paket som en ny version.</span><span class="sxs-lookup"><span data-stu-id="272ed-217">You can now push your web deploy package as a new release.</span></span> <span data-ttu-id="272ed-218">Det här steget kommunicerar med hello-agenten på varje-instans som är en del av hello distributionsgruppen, skickar hello web distribuera paket och konfigurerar IIS toorun hello uppdateras webbprogram.</span><span class="sxs-lookup"><span data-stu-id="272ed-218">This step communicates with hello agent on each instance that is part of hello deployment group, pushes hello web deploy package, then configures IIS toorun hello updated web application.</span></span>

1. <span data-ttu-id="272ed-219">Markera i definitionen för versionen **+ släpper**, Välj **skapa släpper**.</span><span class="sxs-lookup"><span data-stu-id="272ed-219">In your release definition, select **+ Release**, then choose **Create Release**.</span></span>
2. <span data-ttu-id="272ed-220">Kontrollera att hello senaste versionen är markerad i hello listrutan, tillsammans med **automatisk distribution: efter att versionen har skapats**.</span><span class="sxs-lookup"><span data-stu-id="272ed-220">Verify that hello latest build is selected in hello drop-down list, along with **Automated deployment: After release creation**.</span></span> <span data-ttu-id="272ed-221">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="272ed-221">Select **Create**.</span></span>
3. <span data-ttu-id="272ed-222">En liten banderoll visas överst hello av definitionen för versionen som *version 'Version 1' har skapats*.</span><span class="sxs-lookup"><span data-stu-id="272ed-222">A small banner appears across hello top of your release definition, such as *Release 'Release-1' has been created*.</span></span> <span data-ttu-id="272ed-223">Välj hello versionen länk.</span><span class="sxs-lookup"><span data-stu-id="272ed-223">Select hello release link.</span></span>
4. <span data-ttu-id="272ed-224">Öppna hello **loggar** fliken toowatch hello versionen pågår.</span><span class="sxs-lookup"><span data-stu-id="272ed-224">Open hello **Logs** tab toowatch hello release progress.</span></span>
    
    ![Lyckad Team Services versionen och web distribuera paketet push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. <span data-ttu-id="272ed-226">När hello-versionen har slutförts, öppna en webbläsare och ange hello offentliga PIA-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="272ed-226">After hello release is complete, open a web browser and enter hello public IIP address of your VM.</span></span> <span data-ttu-id="272ed-227">ASP.NET-webbprogram körs.</span><span class="sxs-lookup"><span data-stu-id="272ed-227">Your ASP.NET web application is running.</span></span>

    ![ASP.NET-webbprogram som körs på IIS VM](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a><span data-ttu-id="272ed-229">Testa hello hela CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="272ed-229">Test hello whole CI/CD pipeline</span></span>
<span data-ttu-id="272ed-230">Med ditt webbprogram som körs på IIS, försök hello hela CI/CD-pipeline.</span><span class="sxs-lookup"><span data-stu-id="272ed-230">With your web application running on IIS, now try hello whole CI/CD pipeline.</span></span> <span data-ttu-id="272ed-231">När du gör en ändring i Visual Studio och spara utlöses koden, en version som utlöser en version av webbplatsen uppdaterade distribuera paketet tooIIS:</span><span class="sxs-lookup"><span data-stu-id="272ed-231">After you make a change in Visual Studio and commit your code, a build is triggered which then triggers a release of your updated web deploy package tooIIS:</span></span>

1. <span data-ttu-id="272ed-232">Öppna i Visual Studio hello **Solution Explorer** fönster.</span><span class="sxs-lookup"><span data-stu-id="272ed-232">In Visual Studio, open hello **Solution Explorer** window.</span></span>
2. <span data-ttu-id="272ed-233">Navigera tooand öppna *myWebApp | Vyer | Start | Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="272ed-233">Navigate tooand open *myWebApp | Views | Home | Index.cshtml*</span></span>
3. <span data-ttu-id="272ed-234">Redigera rad 6 tooread:</span><span class="sxs-lookup"><span data-stu-id="272ed-234">Edit line 6 tooread:</span></span>

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. <span data-ttu-id="272ed-235">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="272ed-235">Save hello file.</span></span>
5. <span data-ttu-id="272ed-236">Öppna hello **Team Explorer** fönster, Välj hello *myWebApp* projektet och välj sedan **ändringar**.</span><span class="sxs-lookup"><span data-stu-id="272ed-236">Open hello **Team Explorer** window, select hello *myWebApp* project, then choose **Changes**.</span></span>
6. <span data-ttu-id="272ed-237">Ange ett commit-meddelande som *testning CI/CD-pipeline*, Välj **alla genomförande och Sync** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="272ed-237">Enter a commit message, such as *Testing CI/CD pipeline*, then choose **Commit All and Sync** from hello drop-down menu.</span></span>
7. <span data-ttu-id="272ed-238">I arbetsytan Team Services utlöses en ny version från hello kod genomförande.</span><span class="sxs-lookup"><span data-stu-id="272ed-238">In Team Services workspace, a new build is triggered from hello code commit.</span></span> 
    - <span data-ttu-id="272ed-239">Välj **Skapa & släpper**och välj **bygger**.</span><span class="sxs-lookup"><span data-stu-id="272ed-239">Choose **Build & Release**, then select **Builds**.</span></span> 
    - <span data-ttu-id="272ed-240">Välj build-definition, och välj sedan hello **i kö & körs** build toowatch som hello skapa fortskrider.</span><span class="sxs-lookup"><span data-stu-id="272ed-240">Choose your build definition, then select hello **Queued & running** build toowatch as hello build progresses.</span></span>
9. <span data-ttu-id="272ed-241">När hello build lyckas, utlöses en ny version.</span><span class="sxs-lookup"><span data-stu-id="272ed-241">Once hello build is successful, a new release is triggered.</span></span>
    - <span data-ttu-id="272ed-242">Välj **Skapa & släpper**, sedan **versioner** toosee hello webbdistributionspaket pushas tooyour IIS VM.</span><span class="sxs-lookup"><span data-stu-id="272ed-242">Choose **Build & Release**, then **Releases** toosee hello web deploy package pushed tooyour IIS VM.</span></span> 
    - <span data-ttu-id="272ed-243">Välj hello **uppdatera** ikonen tooupdate hello status.</span><span class="sxs-lookup"><span data-stu-id="272ed-243">Select hello **Refresh** icon tooupdate hello status.</span></span> <span data-ttu-id="272ed-244">När hello *miljöer* kolumnen visas en grön bock, hello-versionen har distribuerats tooIIS.</span><span class="sxs-lookup"><span data-stu-id="272ed-244">When hello *Environments* column shows a green check mark, hello release has successfully deployed tooIIS.</span></span>
11. <span data-ttu-id="272ed-245">toosee ändringarna tillämpas, uppdatera IIS-webbplatsen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="272ed-245">toosee your changes applied, refresh your IIS website in a browser.</span></span>

    ![ASP.NET-webbprogram som körs på IIS VM från CI/CD-pipeline](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a><span data-ttu-id="272ed-247">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="272ed-247">Next steps</span></span>

<span data-ttu-id="272ed-248">I kursen får du skapade ASP.NET-webbprogram i Team Services och konfigurerade version och utgåva definitioner toodeploy ny webbplats distribuera paket tooIIS på varje kod genomförande.</span><span class="sxs-lookup"><span data-stu-id="272ed-248">In this tutorial, you created an ASP.NET web application in Team Services and configured build and release definitions toodeploy new web deploy packages tooIIS on each code commit.</span></span> <span data-ttu-id="272ed-249">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="272ed-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="272ed-250">Publicera ett ASP.NET-program tooa Team Services webbprojekt</span><span class="sxs-lookup"><span data-stu-id="272ed-250">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="272ed-251">Skapa en definition av build som utlöses av koden incheckningar</span><span class="sxs-lookup"><span data-stu-id="272ed-251">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="272ed-252">Installera och konfigurera IIS på en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="272ed-252">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="272ed-253">Lägg till hello IIS instans tooa distributionsgruppen i Team Services</span><span class="sxs-lookup"><span data-stu-id="272ed-253">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="272ed-254">Skapa en ny webbplats för versionen definition toopublish distribuera paket tooIIS</span><span class="sxs-lookup"><span data-stu-id="272ed-254">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="272ed-255">Testa hello CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="272ed-255">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="272ed-256">I förväg toohello nästa självstudiekurs toolearn hur toosecure en webbserver med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="272ed-256">Advance toohello next tutorial toolearn how toosecure a web server with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="272ed-257">Säker webbserver med SSL</span><span class="sxs-lookup"><span data-stu-id="272ed-257">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)