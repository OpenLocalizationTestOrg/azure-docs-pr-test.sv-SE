---
title: aaaAzure distribution av virtuella datorer med Chef | Microsoft Docs
description: "Lär dig hur toouse Chef toodo automatisk distribution av virtuella datorer och konfiguration på Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: c5ea98c673b2ee75dd4cedf27e50330af05230d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a><span data-ttu-id="b67ca-103">Automatisera distribution av virtuella Azure-datorer med Chef</span><span class="sxs-lookup"><span data-stu-id="b67ca-103">Automating Azure virtual machine deployment with Chef</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="b67ca-104">Chef är ett bra verktyg för att leverera automation och önskade tillstånd konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="b67ca-104">Chef is a great tool for delivering automation and desired state configurations.</span></span>

<span data-ttu-id="b67ca-105">Med vår senaste moln-api-versionen Chef ger sömlös integrering med Azure, så att du hello möjlighet tooprovision och distribuera konfigureringstillstånd via ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="b67ca-105">With our latest cloud-api release, Chef provides seamless integration with Azure, giving you hello ability tooprovision and deploy configuration states through a single command.</span></span>

<span data-ttu-id="b67ca-106">I den här artikeln jag lära dig hur tooset upp din Chef miljö tooprovision Azure virtuella datorer och beskriver hur du skapar en princip eller ”CookBook” och sedan distribuera den här cookbook tooan virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="b67ca-106">In this article, I’ll show you how tooset up your Chef environment tooprovision Azure virtual machines and walk you through creating a policy or “CookBook” and then deploying this cookbook tooan Azure virtual machine.</span></span>

<span data-ttu-id="b67ca-107">Vi börjar!</span><span class="sxs-lookup"><span data-stu-id="b67ca-107">Let’s begin!</span></span>

## <a name="chef-basics"></a><span data-ttu-id="b67ca-108">Chef grunderna</span><span class="sxs-lookup"><span data-stu-id="b67ca-108">Chef basics</span></span>
<span data-ttu-id="b67ca-109">Innan du börjar rekommenderar jag du granska hello grundläggande begrepp för Chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-109">Before you begin, I suggest you review hello basic concepts of Chef.</span></span> <span data-ttu-id="b67ca-110">Det är bra material <a href="http://www.chef.io/chef" target="_blank">här</a> och jag rekommenderar att du har en snabb läsning innan du utför den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="b67ca-110">There is great material <a href="http://www.chef.io/chef" target="_blank">here</a> and I recommend you have a quick read before you attempt this walkthrough.</span></span> <span data-ttu-id="b67ca-111">Jag kommer dock Sammanfattningsvis hello grunderna innan vi börjar.</span><span class="sxs-lookup"><span data-stu-id="b67ca-111">I will however recap hello basics before we get started.</span></span>

<span data-ttu-id="b67ca-112">hello följande diagram visar hello övergripande Chef arkitektur.</span><span class="sxs-lookup"><span data-stu-id="b67ca-112">hello following diagram depicts hello high-level Chef architecture.</span></span>

![][2]

<span data-ttu-id="b67ca-113">Chef har tre huvudkomponenter arkitektur: Chef servern, Chef klienten (nod) och Chef-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b67ca-113">Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.</span></span>

<span data-ttu-id="b67ca-114">hello Chef Server är vår hanteringsplatsen och det finns två alternativ för hello Chef Server: en värdbaserad lösning eller en lokal lösning.</span><span class="sxs-lookup"><span data-stu-id="b67ca-114">hello Chef Server is our management point and there are two options for hello Chef Server: a hosted solution or an on-premises solution.</span></span> <span data-ttu-id="b67ca-115">Vi kommer att använda en värdbaserad lösning.</span><span class="sxs-lookup"><span data-stu-id="b67ca-115">We will be using a hosted solution.</span></span>

<span data-ttu-id="b67ca-116">hello Chef klienten är (nod) hello-agent som placeras på hello-servrar som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="b67ca-116">hello Chef Client (node) is hello agent that sits on hello servers you are managing.</span></span>

<span data-ttu-id="b67ca-117">hello Chef arbetsstation är vår arbetsstation där vi skapar våra principer och köra vår management-kommandon.</span><span class="sxs-lookup"><span data-stu-id="b67ca-117">hello Chef Workstation is our admin workstation where we create our policies and execute our management commands.</span></span> <span data-ttu-id="b67ca-118">Vi kör hello **kniv** kommandot från hello Chef arbetsstation toomanage vår infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="b67ca-118">We run hello **knife** command from hello Chef Workstation toomanage our infrastructure.</span></span>

<span data-ttu-id="b67ca-119">Det finns också hello begreppet ”Cookbooks” och ”recept”.</span><span class="sxs-lookup"><span data-stu-id="b67ca-119">There is also hello concept of “Cookbooks” and “Recipes”.</span></span> <span data-ttu-id="b67ca-120">Dessa är effektivt hello principer som vi definiera och tillämpa tooour servrar.</span><span class="sxs-lookup"><span data-stu-id="b67ca-120">These are effectively hello policies we define and apply tooour servers.</span></span>

## <a name="preparing-hello-workstation"></a><span data-ttu-id="b67ca-121">Förbereda hello arbetsstation</span><span class="sxs-lookup"><span data-stu-id="b67ca-121">Preparing hello workstation</span></span>
<span data-ttu-id="b67ca-122">Först gör prep hello arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b67ca-122">First, lets prep hello workstation.</span></span> <span data-ttu-id="b67ca-123">Jag använder en standard Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b67ca-123">I’m using a standard Windows workstation.</span></span> <span data-ttu-id="b67ca-124">Vi behöver toocreate en directory toostore våra konfigurationsfiler och cookbooks.</span><span class="sxs-lookup"><span data-stu-id="b67ca-124">We need toocreate a directory toostore our config files and cookbooks.</span></span>

<span data-ttu-id="b67ca-125">Först skapa en katalog med namnet C:\chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-125">First create a directory called C:\chef.</span></span>

<span data-ttu-id="b67ca-126">Skapa sedan den andra katalogen c:\chef\cookbooks.</span><span class="sxs-lookup"><span data-stu-id="b67ca-126">Then create a second directory called c:\chef\cookbooks.</span></span>

<span data-ttu-id="b67ca-127">Vi behöver nu toodownload vårt Azure inställningsfilen så Chef kan kommunicera med våra Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b67ca-127">We now need toodownload our Azure settings file so Chef can communicate with our Azure subscription.</span></span>

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
<span data-ttu-id="b67ca-128">Hämtar dina publiceringsinställningar med hello PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) kommando.</span><span class="sxs-lookup"><span data-stu-id="b67ca-128">Download your publish settings using hello PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) command.</span></span> 

<span data-ttu-id="b67ca-129">Spara hello inställningsfilen för publicering i C:\chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-129">Save hello publish settings file in C:\chef.</span></span>

## <a name="creating-a-managed-chef-account"></a><span data-ttu-id="b67ca-130">Skapa ett hanterat konto Chef</span><span class="sxs-lookup"><span data-stu-id="b67ca-130">Creating a managed Chef account</span></span>
<span data-ttu-id="b67ca-131">Registrera dig för en värdbaserad Chef konto [här](https://manage.chef.io/signup).</span><span class="sxs-lookup"><span data-stu-id="b67ca-131">Sign up for a hosted Chef account [here](https://manage.chef.io/signup).</span></span>

<span data-ttu-id="b67ca-132">Under hello registreringsprocessen du kommer att tillfrågas toocreate en ny organisation.</span><span class="sxs-lookup"><span data-stu-id="b67ca-132">During hello signup process, you will be asked toocreate a new organization.</span></span>

![][3]

<span data-ttu-id="b67ca-133">När din organisation har skapats kan du hämta hello startpaket.</span><span class="sxs-lookup"><span data-stu-id="b67ca-133">Once your organization is created, download hello starter kit.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="b67ca-134">Om du får ett meddelande som varnar dig om att dina nycklar ska återställas är ok tooproceed som vi har inga befintliga infrastruktur konfigurerad ännu.</span><span class="sxs-lookup"><span data-stu-id="b67ca-134">If you receive a prompt warning you that your keys will be reset, it’s ok tooproceed as we have no existing infrastructure configured as yet.</span></span>
> 
> 

<span data-ttu-id="b67ca-135">Den här starter kit zip-filen innehåller din organisation konfigurationsfiler och nycklar.</span><span class="sxs-lookup"><span data-stu-id="b67ca-135">This starter kit zip file contains your organization config files and keys.</span></span>

## <a name="configuring-hello-chef-workstation"></a><span data-ttu-id="b67ca-136">Konfigurera hello Chef arbetsstation</span><span class="sxs-lookup"><span data-stu-id="b67ca-136">Configuring hello Chef workstation</span></span>
<span data-ttu-id="b67ca-137">Extrahera hello innehållet i hello chef starter.zip tooC:\chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-137">Extract hello content of hello chef-starter.zip tooC:\chef.</span></span>

<span data-ttu-id="b67ca-138">Kopiera alla filer under chef-starter\chef-lagringsplatsen\.chef tooyour c:\chef directory.</span><span class="sxs-lookup"><span data-stu-id="b67ca-138">Copy all files under chef-starter\chef-repo\.chef tooyour c:\chef directory.</span></span>

<span data-ttu-id="b67ca-139">Din katalog bör nu se ut ungefär som följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="b67ca-139">Your directory should now look something like hello following example.</span></span>

![][5]

<span data-ttu-id="b67ca-140">Du bör nu ha fyra filer, inklusive hello Azure publishing filen i c:\chef hello rot.</span><span class="sxs-lookup"><span data-stu-id="b67ca-140">You should now have four files including hello Azure publishing file in hello root of c:\chef.</span></span>

<span data-ttu-id="b67ca-141">hello PEM-filer innehåller din organisation och admin privata nycklar för kommunikation medan hello knife.rb filen innehåller kniv konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b67ca-141">hello PEM files contain your organization and admin private keys for communication while hello knife.rb file contains your knife configuration.</span></span> <span data-ttu-id="b67ca-142">Vi behöver tooedit hello knife.rb fil.</span><span class="sxs-lookup"><span data-stu-id="b67ca-142">We will need tooedit hello knife.rb file.</span></span>

<span data-ttu-id="b67ca-143">Öppna hello-filen i redigeringsprogrammet väljer och ändra hello ”cookbook_path” genom att ta bort hello /... / från hello sökvägen så visas det på Nästa.</span><span class="sxs-lookup"><span data-stu-id="b67ca-143">Open hello file in your editor of choice and modify hello “cookbook_path” by removing hello /../ from hello path so it appears as shown next.</span></span>

    cookbook_path  ["#{current_dir}/cookbooks"]

<span data-ttu-id="b67ca-144">Också lägga till hello följande rad reflekterande hello namnet på ditt Azure inställningsfilen för publicering.</span><span class="sxs-lookup"><span data-stu-id="b67ca-144">Also add hello following line reflecting hello name of your Azure publish settings file.</span></span>

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

<span data-ttu-id="b67ca-145">Filen knife.rb bör nu se ut ungefär toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="b67ca-145">Your knife.rb file should now look similar toohello following example.</span></span>

![][6]

<span data-ttu-id="b67ca-146">Dessa rader säkerställer att kniv refererar till hello cookbooks katalog under c:\chef\cookbooks och använder också vår Azure-publiceringsinställningsfil under åtgärder i Azure.</span><span class="sxs-lookup"><span data-stu-id="b67ca-146">These lines will ensure that Knife references hello cookbooks directory under c:\chef\cookbooks, and also uses our Azure Publish Settings file during Azure operations.</span></span>

## <a name="installing-hello-chef-development-kit"></a><span data-ttu-id="b67ca-147">Installera hello Chef Development Kit</span><span class="sxs-lookup"><span data-stu-id="b67ca-147">Installing hello Chef Development Kit</span></span>
<span data-ttu-id="b67ca-148">Nästa [ladda ned och installera](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef Development Kit) tooset upp arbetsstationen Chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-148">Next [download and install](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef Development Kit) tooset up your Chef Workstation.</span></span>

![][7]

<span data-ttu-id="b67ca-149">Installera i hello standardplatsen för c:\opscode.</span><span class="sxs-lookup"><span data-stu-id="b67ca-149">Install in hello default location of c:\opscode.</span></span> <span data-ttu-id="b67ca-150">Den här installationen tar cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="b67ca-150">This install will take around 10 minutes.</span></span>

<span data-ttu-id="b67ca-151">Bekräfta din sökvägsvariabeln innehåller poster för C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span><span class="sxs-lookup"><span data-stu-id="b67ca-151">Confirm your PATH variable contains entries for C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span></span>

<span data-ttu-id="b67ca-152">Om de inte är det, se till att du lägger till dessa sökvägar!</span><span class="sxs-lookup"><span data-stu-id="b67ca-152">If they are not there, make sure you add these paths!</span></span>

<span data-ttu-id="b67ca-153">*Obs hello ordning av hello sökväg är viktigt!*</span><span class="sxs-lookup"><span data-stu-id="b67ca-153">*NOTE hello ORDER OF hello PATH IS IMPORTANT!*</span></span> <span data-ttu-id="b67ca-154">Om din opscode sökvägar inte kan i rätt ordning för hello har du problem.</span><span class="sxs-lookup"><span data-stu-id="b67ca-154">If your opscode paths are not in hello correct order you will have issues.</span></span>

<span data-ttu-id="b67ca-155">Starta om arbetsstationen innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="b67ca-155">Reboot your workstation before you continue.</span></span>

<span data-ttu-id="b67ca-156">Därefter installerar vi hello kniv Azure-tillägget.</span><span class="sxs-lookup"><span data-stu-id="b67ca-156">Next, we will install hello Knife Azure extension.</span></span> <span data-ttu-id="b67ca-157">Detta ger kniv hello ”Azure plugin-program”.</span><span class="sxs-lookup"><span data-stu-id="b67ca-157">This provides Knife with hello “Azure Plugin”.</span></span>

<span data-ttu-id="b67ca-158">Kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="b67ca-158">Run hello following command.</span></span>

    chef gem install knife-azure ––pre

> [!NOTE]
> <span data-ttu-id="b67ca-159">hello – pre argumentet säkerställer du tar emot hello senaste RC-versionen av hello kniv Azure plugin-program som ger åtkomst toohello senaste uppsättning API: er.</span><span class="sxs-lookup"><span data-stu-id="b67ca-159">hello –pre argument ensures you are receiving hello latest RC version of hello Knife Azure Plugin which provides access toohello latest set of APIs.</span></span>
> 
> 

<span data-ttu-id="b67ca-160">Det är troligt att installeras även ett antal beroenden på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b67ca-160">It’s likely that a number of dependencies will also be installed at hello same time.</span></span>

![][8]

<span data-ttu-id="b67ca-161">tooensure allt är konfigurerade korrekt, kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="b67ca-161">tooensure everything is configured correctly, run hello following command.</span></span>

    knife azure image list

<span data-ttu-id="b67ca-162">Om allt är korrekt konfigurerad, visas en lista över tillgängliga Azure avbildningar rulla.</span><span class="sxs-lookup"><span data-stu-id="b67ca-162">If everything is configured correctly, you will see a list of available Azure images scroll through.</span></span>

<span data-ttu-id="b67ca-163">Grattis!</span><span class="sxs-lookup"><span data-stu-id="b67ca-163">Congratulations.</span></span> <span data-ttu-id="b67ca-164">hello arbetsstation ställs in!</span><span class="sxs-lookup"><span data-stu-id="b67ca-164">hello workstation is set up!</span></span>

## <a name="creating-a-cookbook"></a><span data-ttu-id="b67ca-165">Skapa en Cookbook</span><span class="sxs-lookup"><span data-stu-id="b67ca-165">Creating a Cookbook</span></span>
<span data-ttu-id="b67ca-166">En Cookbook används av Chef toodefine en uppsättning kommandon som du vill tooexecute på en hanterad klient.</span><span class="sxs-lookup"><span data-stu-id="b67ca-166">A Cookbook is used by Chef toodefine a set of commands that you wish tooexecute on your managed client.</span></span> <span data-ttu-id="b67ca-167">Det är enkelt att skapa en Cookbook och vi använder hello **chef generera cookbook** kommandot toogenerate våra Cookbook mallen.</span><span class="sxs-lookup"><span data-stu-id="b67ca-167">Creating a Cookbook is straightforward and we use hello **chef generate cookbook** command toogenerate our Cookbook template.</span></span> <span data-ttu-id="b67ca-168">Jag kommer att ringa upp webbservern Cookbook som jag vill att en princip som automatiskt distribuerar IIS.</span><span class="sxs-lookup"><span data-stu-id="b67ca-168">I will be calling my Cookbook web server as I would like a policy that automatically deploys IIS.</span></span>

<span data-ttu-id="b67ca-169">Kör följande kommando hello under katalogen C:\Chef.</span><span class="sxs-lookup"><span data-stu-id="b67ca-169">Under your C:\Chef directory run hello following command.</span></span>

    chef generate cookbook webserver

<span data-ttu-id="b67ca-170">Detta genererar en uppsättning filer under hello katalog C:\Chef\cookbooks\webserver.</span><span class="sxs-lookup"><span data-stu-id="b67ca-170">This will generate a set of files under hello directory C:\Chef\cookbooks\webserver.</span></span> <span data-ttu-id="b67ca-171">Vi behöver nu toodefine hello uppsättning kommandon som vi vill gärna vår Chef klienten tooexecute på vår hanterade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b67ca-171">We now need toodefine hello set of commands we would like our Chef client tooexecute on our managed virtual machine.</span></span>

<span data-ttu-id="b67ca-172">hello kommandon lagras i hello filen default.rb.</span><span class="sxs-lookup"><span data-stu-id="b67ca-172">hello commands are stored in hello file default.rb.</span></span> <span data-ttu-id="b67ca-173">I den här filen ska jag definierar en uppsättning kommandon som installerar IIS, startar IIS och kopierar en mall toohello wwwroot mapp.</span><span class="sxs-lookup"><span data-stu-id="b67ca-173">In this file, I’ll be defining a set of commands that installs IIS, starts IIS and copies a template file toohello wwwroot folder.</span></span>

<span data-ttu-id="b67ca-174">Ändra hello C:\chef\cookbooks\webserver\recipes\default.rb filen och Lägg till följande rader hello.</span><span class="sxs-lookup"><span data-stu-id="b67ca-174">Modify hello C:\chef\cookbooks\webserver\recipes\default.rb file and add hello following lines.</span></span>

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

<span data-ttu-id="b67ca-175">Spara hello-filen när du är klar.</span><span class="sxs-lookup"><span data-stu-id="b67ca-175">Save hello file once you are done.</span></span>

## <a name="creating-a-template"></a><span data-ttu-id="b67ca-176">Skapa en mall</span><span class="sxs-lookup"><span data-stu-id="b67ca-176">Creating a template</span></span>
<span data-ttu-id="b67ca-177">Som vi nämnt tidigare behöver vi toogenerate en mallfil som ska användas som sidan med våra default.html.</span><span class="sxs-lookup"><span data-stu-id="b67ca-177">As we mentioned previously, we need toogenerate a template file which will be used as our default.html page.</span></span>

<span data-ttu-id="b67ca-178">Hello kör följande kommando toogenerate hello mallen.</span><span class="sxs-lookup"><span data-stu-id="b67ca-178">Run hello following command toogenerate hello template.</span></span>

    chef generate template webserver Default.htm

<span data-ttu-id="b67ca-179">Gå nu toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fil.</span><span class="sxs-lookup"><span data-stu-id="b67ca-179">Now navigate toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb file.</span></span> <span data-ttu-id="b67ca-180">Redigera hello-filen genom att lägga till vissa enkel ”Hello World” HTML-kod och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b67ca-180">Edit hello file by adding some simple “Hello World” HTML code, and then save hello file.</span></span>

## <a name="upload-hello-cookbook-toohello-chef-server"></a><span data-ttu-id="b67ca-181">Överför hello Cookbook toohello Chef Server</span><span class="sxs-lookup"><span data-stu-id="b67ca-181">Upload hello Cookbook toohello Chef Server</span></span>
<span data-ttu-id="b67ca-182">I det här steget är vi tar en kopia av hello Cookbook som vi har skapat på våra lokala dator och överföra den toohello Chef Hosted Server.</span><span class="sxs-lookup"><span data-stu-id="b67ca-182">In this step, we are taking a copy of hello Cookbook that we have created on our local machine and uploading it toohello Chef Hosted Server.</span></span> <span data-ttu-id="b67ca-183">När du har överfört hello Cookbook visas under hello **princip** fliken.</span><span class="sxs-lookup"><span data-stu-id="b67ca-183">Once uploaded, hello Cookbook will appear under hello **Policy** tab.</span></span>

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a><span data-ttu-id="b67ca-184">Distribuera en virtuell dator med kniv Azure</span><span class="sxs-lookup"><span data-stu-id="b67ca-184">Deploy a virtual machine with Knife Azure</span></span>
<span data-ttu-id="b67ca-185">Vi kommer nu distribuera en virtuell Azure-dator och tillämpa hello ”webbserver” Cookbook som installerar våra IIS web service och standard webbsida.</span><span class="sxs-lookup"><span data-stu-id="b67ca-185">We will now deploy an Azure virtual machine and apply hello “Webserver” Cookbook which will install our IIS web service and default web page.</span></span>

<span data-ttu-id="b67ca-186">I ordning toodo detta använder hello **kniv azure-servern skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="b67ca-186">In order toodo this, use hello **knife azure server create** command.</span></span>

<span data-ttu-id="b67ca-187">Är exempel på hello kommandot visas.</span><span class="sxs-lookup"><span data-stu-id="b67ca-187">Am example of hello command appears next.</span></span>

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

<span data-ttu-id="b67ca-188">hello parametrar är självförklarande.</span><span class="sxs-lookup"><span data-stu-id="b67ca-188">hello parameters are self-explanatory.</span></span> <span data-ttu-id="b67ca-189">Ersätta viss variabler och kör.</span><span class="sxs-lookup"><span data-stu-id="b67ca-189">Substitute your particular variables and run.</span></span>

> [!NOTE]
> <span data-ttu-id="b67ca-190">Via hello hello kommandoraden jag också automatisera min filterregler för slutpunkt-nätverk med hjälp av hello – tcp-slutpunkter parametern.</span><span class="sxs-lookup"><span data-stu-id="b67ca-190">Through hello hello command line, I’m also automating my endpoint network filter rules by using hello –tcp-endpoints parameter.</span></span> <span data-ttu-id="b67ca-191">Jag har öppnat portarna 80 och 3389 tooprovide åtkomst toomy webbsida och RDP-session.</span><span class="sxs-lookup"><span data-stu-id="b67ca-191">I’ve opened up ports 80 and 3389 tooprovide access toomy web page and RDP session.</span></span>
> 
> 

<span data-ttu-id="b67ca-192">När du kör kommandot hello gå toohello Azure portal och du kommer att se din dator börjar tooprovision.</span><span class="sxs-lookup"><span data-stu-id="b67ca-192">Once you run hello command, go toohello Azure portal and you will see your machine begin tooprovision.</span></span>

![][13]

<span data-ttu-id="b67ca-193">hello kommandotolken visas nästa.</span><span class="sxs-lookup"><span data-stu-id="b67ca-193">hello command prompt appears next.</span></span>

![][10]

<span data-ttu-id="b67ca-194">När hello distributionen är klar kan vara vi kan tooconnect toohello webbtjänsten via port 80 som vi har öppnat hello port när vi etableras hello virtuell dator med hello kniv Azure-kommando.</span><span class="sxs-lookup"><span data-stu-id="b67ca-194">Once hello deployment is complete, we should be able tooconnect toohello web service over port 80 as we had opened hello port when we provisioned hello virtual machine with hello Knife Azure command.</span></span> <span data-ttu-id="b67ca-195">Eftersom den här virtuella datorn är hello virtuell dator i min Molntjänsten, kommer den ansluts med hello molnet tjänst-url.</span><span class="sxs-lookup"><span data-stu-id="b67ca-195">As this virtual machine is hello only virtual machine in my cloud service, I’ll connect it with hello cloud service url.</span></span>

![][11]

<span data-ttu-id="b67ca-196">Som du ser fick jag kreativa med HTML-kod.</span><span class="sxs-lookup"><span data-stu-id="b67ca-196">As you can see, I got creative with my HTML code.</span></span>

<span data-ttu-id="b67ca-197">Vi kan också ansluta via en RDP-session från hello Azure-portalen via port 3389 Glöm inte.</span><span class="sxs-lookup"><span data-stu-id="b67ca-197">Don’t forget we can also connect through an RDP session from hello Azure portal via port 3389.</span></span>

<span data-ttu-id="b67ca-198">Hoppas det här har bra!</span><span class="sxs-lookup"><span data-stu-id="b67ca-198">I hope this has been helpful!</span></span> <span data-ttu-id="b67ca-199">Gå och starta din infrastruktur som koden resa med Azure idag!</span><span class="sxs-lookup"><span data-stu-id="b67ca-199">Go  and start your infrastructure as code journey with Azure today!</span></span>

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
