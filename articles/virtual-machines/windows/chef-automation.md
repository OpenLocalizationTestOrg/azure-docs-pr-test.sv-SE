---
title: Distribution av Azure virtuella datorer med Chef | Microsoft Docs
description: "Lär dig hur du använder Chef för automatiserade virtuella distribution och konfiguration i Microsoft Azure"
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
ms.openlocfilehash: b6db0fbb4e0de896994954974ddcc39daad9c125
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a><span data-ttu-id="15065-103">Automatisera distribution av virtuella Azure-datorer med Chef</span><span class="sxs-lookup"><span data-stu-id="15065-103">Automating Azure virtual machine deployment with Chef</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="15065-104">Chef är ett bra verktyg för att leverera automation och önskade tillstånd konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="15065-104">Chef is a great tool for delivering automation and desired state configurations.</span></span>

<span data-ttu-id="15065-105">Med vår senaste moln-api-versionen tillhandahåller Chef sömlös integrering med Azure, ger dig möjlighet att tillhandahålla och distribuera konfigureringstillstånd via ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="15065-105">With our latest cloud-api release, Chef provides seamless integration with Azure, giving you the ability to provision and deploy configuration states through a single command.</span></span>

<span data-ttu-id="15065-106">I den här artikeln kommer jag visar hur du konfigurerar miljön Chef att etablera virtuella Azure-datorer och beskriver hur du skapar en princip eller ”CookBook” och sedan distribuera den här cookbook till en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="15065-106">In this article, I’ll show you how to set up your Chef environment to provision Azure virtual machines and walk you through creating a policy or “CookBook” and then deploying this cookbook to an Azure virtual machine.</span></span>

<span data-ttu-id="15065-107">Vi börjar!</span><span class="sxs-lookup"><span data-stu-id="15065-107">Let’s begin!</span></span>

## <a name="chef-basics"></a><span data-ttu-id="15065-108">Chef grunderna</span><span class="sxs-lookup"><span data-stu-id="15065-108">Chef basics</span></span>
<span data-ttu-id="15065-109">Innan du börjar rekommenderar jag du granska de grundläggande principerna för Chef.</span><span class="sxs-lookup"><span data-stu-id="15065-109">Before you begin, I suggest you review the basic concepts of Chef.</span></span> <span data-ttu-id="15065-110">Det är bra material <a href="http://www.chef.io/chef" target="_blank">här</a> och jag rekommenderar att du har en snabb läsning innan du utför den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="15065-110">There is great material <a href="http://www.chef.io/chef" target="_blank">here</a> and I recommend you have a quick read before you attempt this walkthrough.</span></span> <span data-ttu-id="15065-111">Jag kommer dock Sammanfattningsvis grunderna innan vi börjar.</span><span class="sxs-lookup"><span data-stu-id="15065-111">I will however recap the basics before we get started.</span></span>

<span data-ttu-id="15065-112">Följande diagram visar den övergripande Chef-arkitekturen.</span><span class="sxs-lookup"><span data-stu-id="15065-112">The following diagram depicts the high-level Chef architecture.</span></span>

![][2]

<span data-ttu-id="15065-113">Chef har tre huvudkomponenter arkitektur: Chef servern, Chef klienten (nod) och Chef-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="15065-113">Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.</span></span>

<span data-ttu-id="15065-114">Chef-servern är vår hanteringsplatsen och det finns två alternativ för Chef-Server: en värdbaserad lösning eller en lokal lösning.</span><span class="sxs-lookup"><span data-stu-id="15065-114">The Chef Server is our management point and there are two options for the Chef Server: a hosted solution or an on-premises solution.</span></span> <span data-ttu-id="15065-115">Vi kommer att använda en värdbaserad lösning.</span><span class="sxs-lookup"><span data-stu-id="15065-115">We will be using a hosted solution.</span></span>

<span data-ttu-id="15065-116">Chef-klienten (nod) är den agent som placeras på de servrar som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="15065-116">The Chef Client (node) is the agent that sits on the servers you are managing.</span></span>

<span data-ttu-id="15065-117">Chef arbetsstationen är vår arbetsstation där vi skapar våra principer och köra vår management-kommandon.</span><span class="sxs-lookup"><span data-stu-id="15065-117">The Chef Workstation is our admin workstation where we create our policies and execute our management commands.</span></span> <span data-ttu-id="15065-118">Vi kör den **kniv** från Chef arbetsstationen för att hantera vår infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="15065-118">We run the **knife** command from the Chef Workstation to manage our infrastructure.</span></span>

<span data-ttu-id="15065-119">Det finns också begreppet ”Cookbooks” och ”recept”.</span><span class="sxs-lookup"><span data-stu-id="15065-119">There is also the concept of “Cookbooks” and “Recipes”.</span></span> <span data-ttu-id="15065-120">Det här är ett effektivt sätt principer vi definiera och gäller för våra servrar.</span><span class="sxs-lookup"><span data-stu-id="15065-120">These are effectively the policies we define and apply to our servers.</span></span>

## <a name="preparing-the-workstation"></a><span data-ttu-id="15065-121">Förbereder arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="15065-121">Preparing the workstation</span></span>
<span data-ttu-id="15065-122">Först gör Förbered dig arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="15065-122">First, lets prep the workstation.</span></span> <span data-ttu-id="15065-123">Jag använder en standard Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="15065-123">I’m using a standard Windows workstation.</span></span> <span data-ttu-id="15065-124">Vi behöver skapa en katalog för lagring av våra konfigurationsfiler och cookbooks.</span><span class="sxs-lookup"><span data-stu-id="15065-124">We need to create a directory to store our config files and cookbooks.</span></span>

<span data-ttu-id="15065-125">Först skapa en katalog med namnet C:\chef.</span><span class="sxs-lookup"><span data-stu-id="15065-125">First create a directory called C:\chef.</span></span>

<span data-ttu-id="15065-126">Skapa sedan den andra katalogen c:\chef\cookbooks.</span><span class="sxs-lookup"><span data-stu-id="15065-126">Then create a second directory called c:\chef\cookbooks.</span></span>

<span data-ttu-id="15065-127">Vi behöver nu hämta vårt Azure inställningsfilen så Chef kan kommunicera med våra Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15065-127">We now need to download our Azure settings file so Chef can communicate with our Azure subscription.</span></span>

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
<span data-ttu-id="15065-128">Hämtar dina publiceringsinställningar med hjälp av PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) kommando.</span><span class="sxs-lookup"><span data-stu-id="15065-128">Download your publish settings using the PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) command.</span></span> 

<span data-ttu-id="15065-129">Spara filen med publicera i C:\chef.</span><span class="sxs-lookup"><span data-stu-id="15065-129">Save the publish settings file in C:\chef.</span></span>

## <a name="creating-a-managed-chef-account"></a><span data-ttu-id="15065-130">Skapa ett hanterat konto Chef</span><span class="sxs-lookup"><span data-stu-id="15065-130">Creating a managed Chef account</span></span>
<span data-ttu-id="15065-131">Registrera dig för en värdbaserad Chef konto [här](https://manage.chef.io/signup).</span><span class="sxs-lookup"><span data-stu-id="15065-131">Sign up for a hosted Chef account [here](https://manage.chef.io/signup).</span></span>

<span data-ttu-id="15065-132">Under registreringsprocessen, blir du ombedd att skapa en ny organisation.</span><span class="sxs-lookup"><span data-stu-id="15065-132">During the signup process, you will be asked to create a new organization.</span></span>

![][3]

<span data-ttu-id="15065-133">När din organisation har skapats kan du hämta starter kit.</span><span class="sxs-lookup"><span data-stu-id="15065-133">Once your organization is created, download the starter kit.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="15065-134">Om du får ett meddelande som varnar dig om att dina nycklar återställs, är det ok om du vill fortsätta eftersom vi har inga befintliga infrastruktur konfigurerad ännu.</span><span class="sxs-lookup"><span data-stu-id="15065-134">If you receive a prompt warning you that your keys will be reset, it’s ok to proceed as we have no existing infrastructure configured as yet.</span></span>
> 
> 

<span data-ttu-id="15065-135">Den här starter kit zip-filen innehåller din organisation konfigurationsfiler och nycklar.</span><span class="sxs-lookup"><span data-stu-id="15065-135">This starter kit zip file contains your organization config files and keys.</span></span>

## <a name="configuring-the-chef-workstation"></a><span data-ttu-id="15065-136">Konfigurera Chef arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="15065-136">Configuring the Chef workstation</span></span>
<span data-ttu-id="15065-137">Extrahera innehållet i chef-starter.zip till C:\chef.</span><span class="sxs-lookup"><span data-stu-id="15065-137">Extract the content of the chef-starter.zip to C:\chef.</span></span>

<span data-ttu-id="15065-138">Kopiera alla filer under chef-starter\chef-lagringsplatsen\.chef till c:\chef-katalogen.</span><span class="sxs-lookup"><span data-stu-id="15065-138">Copy all files under chef-starter\chef-repo\.chef to your c:\chef directory.</span></span>

<span data-ttu-id="15065-139">Katalogen bör nu se ut ungefär som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="15065-139">Your directory should now look something like the following example.</span></span>

![][5]

<span data-ttu-id="15065-140">Du bör nu ha fyra filer, inklusive Azure publishing filen i roten av c:\chef.</span><span class="sxs-lookup"><span data-stu-id="15065-140">You should now have four files including the Azure publishing file in the root of c:\chef.</span></span>

<span data-ttu-id="15065-141">PEM-filer innehåller din organisation och admin privata nycklar för kommunikation medan knife.rb-filen innehåller kniv konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="15065-141">The PEM files contain your organization and admin private keys for communication while the knife.rb file contains your knife configuration.</span></span> <span data-ttu-id="15065-142">Vi behöver du redigera filen knife.rb.</span><span class="sxs-lookup"><span data-stu-id="15065-142">We will need to edit the knife.rb file.</span></span>

<span data-ttu-id="15065-143">Öppna filen i redigeringsprogrammet val och ändra ”cookbook_path” genom att ta bort den /... / från sökvägen så visas det på Nästa.</span><span class="sxs-lookup"><span data-stu-id="15065-143">Open the file in your editor of choice and modify the “cookbook_path” by removing the /../ from the path so it appears as shown next.</span></span>

    cookbook_path  ["#{current_dir}/cookbooks"]

<span data-ttu-id="15065-144">Lägg även till följande rad reflektion namnet på ditt Azure inställningsfilen för publicering.</span><span class="sxs-lookup"><span data-stu-id="15065-144">Also add the following line reflecting the name of your Azure publish settings file.</span></span>

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

<span data-ttu-id="15065-145">Filen knife.rb bör nu se ut ungefär som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="15065-145">Your knife.rb file should now look similar to the following example.</span></span>

![][6]

<span data-ttu-id="15065-146">Dessa rader säkerställer att kniv refererar till katalogen cookbooks under c:\chef\cookbooks och använder också vår Azure-publiceringsinställningsfil under åtgärder i Azure.</span><span class="sxs-lookup"><span data-stu-id="15065-146">These lines will ensure that Knife references the cookbooks directory under c:\chef\cookbooks, and also uses our Azure Publish Settings file during Azure operations.</span></span>

## <a name="installing-the-chef-development-kit"></a><span data-ttu-id="15065-147">Installera Chef Development Kit</span><span class="sxs-lookup"><span data-stu-id="15065-147">Installing the Chef Development Kit</span></span>
<span data-ttu-id="15065-148">Nästa [ladda ned och installera](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef Development Kit) att ställa in din Chef arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="15065-148">Next [download and install](http://downloads.getchef.com/chef-dk/windows) the ChefDK (Chef Development Kit) to set up your Chef Workstation.</span></span>

![][7]

<span data-ttu-id="15065-149">Installera på standardplatsen för c:\opscode.</span><span class="sxs-lookup"><span data-stu-id="15065-149">Install in the default location of c:\opscode.</span></span> <span data-ttu-id="15065-150">Den här installationen tar cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="15065-150">This install will take around 10 minutes.</span></span>

<span data-ttu-id="15065-151">Bekräfta din sökvägsvariabeln innehåller poster för C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span><span class="sxs-lookup"><span data-stu-id="15065-151">Confirm your PATH variable contains entries for C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span></span>

<span data-ttu-id="15065-152">Om de inte är det, se till att du lägger till dessa sökvägar!</span><span class="sxs-lookup"><span data-stu-id="15065-152">If they are not there, make sure you add these paths!</span></span>

<span data-ttu-id="15065-153">*OBSERVERA ATT SÖKVÄGEN ÄR VIKTIGT!*</span><span class="sxs-lookup"><span data-stu-id="15065-153">*NOTE THE ORDER OF THE PATH IS IMPORTANT!*</span></span> <span data-ttu-id="15065-154">Om din opscode sökvägar inte kan i rätt ordning har du problem.</span><span class="sxs-lookup"><span data-stu-id="15065-154">If your opscode paths are not in the correct order you will have issues.</span></span>

<span data-ttu-id="15065-155">Starta om arbetsstationen innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="15065-155">Reboot your workstation before you continue.</span></span>

<span data-ttu-id="15065-156">Därefter installerar vi kniv Azure-tillägget.</span><span class="sxs-lookup"><span data-stu-id="15065-156">Next, we will install the Knife Azure extension.</span></span> <span data-ttu-id="15065-157">Detta ger kniv med pluginprogrammet ”Azure”.</span><span class="sxs-lookup"><span data-stu-id="15065-157">This provides Knife with the “Azure Plugin”.</span></span>

<span data-ttu-id="15065-158">Kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="15065-158">Run the following command.</span></span>

    chef gem install knife-azure ––pre

> [!NOTE]
> <span data-ttu-id="15065-159">Argumentet – pre säkerställer du får den senaste RC-versionen av kniv Azure plugin-programmet som ger åtkomst till den senaste uppsättningen API: er.</span><span class="sxs-lookup"><span data-stu-id="15065-159">The –pre argument ensures you are receiving the latest RC version of the Knife Azure Plugin which provides access to the latest set of APIs.</span></span>
> 
> 

<span data-ttu-id="15065-160">Det är troligt att installeras även ett antal beroenden på samma gång.</span><span class="sxs-lookup"><span data-stu-id="15065-160">It’s likely that a number of dependencies will also be installed at the same time.</span></span>

![][8]

<span data-ttu-id="15065-161">Kör följande kommando för att se till att allt är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="15065-161">To ensure everything is configured correctly, run the following command.</span></span>

    knife azure image list

<span data-ttu-id="15065-162">Om allt är korrekt konfigurerad, visas en lista över tillgängliga Azure avbildningar rulla.</span><span class="sxs-lookup"><span data-stu-id="15065-162">If everything is configured correctly, you will see a list of available Azure images scroll through.</span></span>

<span data-ttu-id="15065-163">Grattis.</span><span class="sxs-lookup"><span data-stu-id="15065-163">Congratulations.</span></span> <span data-ttu-id="15065-164">Arbetsstationen ställs in!</span><span class="sxs-lookup"><span data-stu-id="15065-164">The workstation is set up!</span></span>

## <a name="creating-a-cookbook"></a><span data-ttu-id="15065-165">Skapa en Cookbook</span><span class="sxs-lookup"><span data-stu-id="15065-165">Creating a Cookbook</span></span>
<span data-ttu-id="15065-166">En Cookbook används av Chef för att definiera en uppsättning kommandon som du vill köra på en hanterad klient.</span><span class="sxs-lookup"><span data-stu-id="15065-166">A Cookbook is used by Chef to define a set of commands that you wish to execute on your managed client.</span></span> <span data-ttu-id="15065-167">Det är enkelt att skapa en Cookbook och vi använder den **chef generera cookbook** kommando för att generera våra Cookbook mallen.</span><span class="sxs-lookup"><span data-stu-id="15065-167">Creating a Cookbook is straightforward and we use the **chef generate cookbook** command to generate our Cookbook template.</span></span> <span data-ttu-id="15065-168">Jag kommer att ringa upp webbservern Cookbook som jag vill att en princip som automatiskt distribuerar IIS.</span><span class="sxs-lookup"><span data-stu-id="15065-168">I will be calling my Cookbook web server as I would like a policy that automatically deploys IIS.</span></span>

<span data-ttu-id="15065-169">Kör följande kommando under katalogen C:\Chef.</span><span class="sxs-lookup"><span data-stu-id="15065-169">Under your C:\Chef directory run the following command.</span></span>

    chef generate cookbook webserver

<span data-ttu-id="15065-170">Detta genererar en uppsättning filer i katalogen C:\Chef\cookbooks\webserver.</span><span class="sxs-lookup"><span data-stu-id="15065-170">This will generate a set of files under the directory C:\Chef\cookbooks\webserver.</span></span> <span data-ttu-id="15065-171">Vi nu måste du definiera en uppsättning kommandon som vi vill gärna Chef klient ska köras på vår hanterade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="15065-171">We now need to define the set of commands we would like our Chef client to execute on our managed virtual machine.</span></span>

<span data-ttu-id="15065-172">Kommandon som lagras i filen default.rb.</span><span class="sxs-lookup"><span data-stu-id="15065-172">The commands are stored in the file default.rb.</span></span> <span data-ttu-id="15065-173">I den här filen ska jag definierar en uppsättning kommandon som installerar IIS, startar IIS och kopierar en mallfil till Wwwroot-mappen.</span><span class="sxs-lookup"><span data-stu-id="15065-173">In this file, I’ll be defining a set of commands that installs IIS, starts IIS and copies a template file to the wwwroot folder.</span></span>

<span data-ttu-id="15065-174">Ändra filen C:\chef\cookbooks\webserver\recipes\default.rb och Lägg till följande rader.</span><span class="sxs-lookup"><span data-stu-id="15065-174">Modify the C:\chef\cookbooks\webserver\recipes\default.rb file and add the following lines.</span></span>

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

<span data-ttu-id="15065-175">Spara filen när du är klar.</span><span class="sxs-lookup"><span data-stu-id="15065-175">Save the file once you are done.</span></span>

## <a name="creating-a-template"></a><span data-ttu-id="15065-176">Skapa en mall</span><span class="sxs-lookup"><span data-stu-id="15065-176">Creating a template</span></span>
<span data-ttu-id="15065-177">Som vi nämnt tidigare behöver vi skapa en mallfil som ska användas som sidan med våra default.html.</span><span class="sxs-lookup"><span data-stu-id="15065-177">As we mentioned previously, we need to generate a template file which will be used as our default.html page.</span></span>

<span data-ttu-id="15065-178">Kör följande kommando för att skapa mallen.</span><span class="sxs-lookup"><span data-stu-id="15065-178">Run the following command to generate the template.</span></span>

    chef generate template webserver Default.htm

<span data-ttu-id="15065-179">Navigera till filen C:\chef\cookbooks\webserver\templates\default\Default.htm.erb.</span><span class="sxs-lookup"><span data-stu-id="15065-179">Now navigate to the C:\chef\cookbooks\webserver\templates\default\Default.htm.erb file.</span></span> <span data-ttu-id="15065-180">Redigera filen genom att lägga till vissa enkel ”Hello World” HTML-kod och spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="15065-180">Edit the file by adding some simple “Hello World” HTML code, and then save the file.</span></span>

## <a name="upload-the-cookbook-to-the-chef-server"></a><span data-ttu-id="15065-181">Överför Cookbook till servern Chef</span><span class="sxs-lookup"><span data-stu-id="15065-181">Upload the Cookbook to the Chef Server</span></span>
<span data-ttu-id="15065-182">I det här steget är vi tar en kopia av Cookbook som vi har skapat på våra lokala dator och överföra den till Chef Hosted Server.</span><span class="sxs-lookup"><span data-stu-id="15065-182">In this step, we are taking a copy of the Cookbook that we have created on our local machine and uploading it to the Chef Hosted Server.</span></span> <span data-ttu-id="15065-183">När du har överfört Cookbook visas under den **princip** fliken.</span><span class="sxs-lookup"><span data-stu-id="15065-183">Once uploaded, the Cookbook will appear under the **Policy** tab.</span></span>

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a><span data-ttu-id="15065-184">Distribuera en virtuell dator med kniv Azure</span><span class="sxs-lookup"><span data-stu-id="15065-184">Deploy a virtual machine with Knife Azure</span></span>
<span data-ttu-id="15065-185">Vi kommer nu distribuera en virtuell Azure-dator och tillämpa ”webbserver”-Cookbook som installerar våra IIS web service och standard webbsida.</span><span class="sxs-lookup"><span data-stu-id="15065-185">We will now deploy an Azure virtual machine and apply the “Webserver” Cookbook which will install our IIS web service and default web page.</span></span>

<span data-ttu-id="15065-186">För att kunna göra detta måste använda den **kniv azure-servern skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="15065-186">In order to do this, use the **knife azure server create** command.</span></span>

<span data-ttu-id="15065-187">Är exempel på kommandot visas.</span><span class="sxs-lookup"><span data-stu-id="15065-187">Am example of the command appears next.</span></span>

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

<span data-ttu-id="15065-188">Parametrarna är självförklarande.</span><span class="sxs-lookup"><span data-stu-id="15065-188">The parameters are self-explanatory.</span></span> <span data-ttu-id="15065-189">Ersätta viss variabler och kör.</span><span class="sxs-lookup"><span data-stu-id="15065-189">Substitute your particular variables and run.</span></span>

> [!NOTE]
> <span data-ttu-id="15065-190">Via den kommandoraden jag också automatisera filterregler min endpoint nätverk med hjälp av parametern – tcp-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="15065-190">Through the the command line, I’m also automating my endpoint network filter rules by using the –tcp-endpoints parameter.</span></span> <span data-ttu-id="15065-191">Jag har öppnat portarna 80 och 3389 att ge åtkomst till min sida och RDP-session.</span><span class="sxs-lookup"><span data-stu-id="15065-191">I’ve opened up ports 80 and 3389 to provide access to my web page and RDP session.</span></span>
> 
> 

<span data-ttu-id="15065-192">När du kör kommandot Gå till Azure-portalen och börja etablera datorn visas.</span><span class="sxs-lookup"><span data-stu-id="15065-192">Once you run the command, go to the Azure portal and you will see your machine begin to provision.</span></span>

![][13]

<span data-ttu-id="15065-193">Kommandotolken visas.</span><span class="sxs-lookup"><span data-stu-id="15065-193">The command prompt appears next.</span></span>

![][10]

<span data-ttu-id="15065-194">När installationen är klar, ska vi kunna ansluta till webbtjänsten via port 80 som vi har öppnat porten när vi etableras den virtuella datorn med kommandot kniv Azure.</span><span class="sxs-lookup"><span data-stu-id="15065-194">Once the deployment is complete, we should be able to connect to the web service over port 80 as we had opened the port when we provisioned the virtual machine with the Knife Azure command.</span></span> <span data-ttu-id="15065-195">Eftersom den här virtuella datorn är den enda virtuella i min Molntjänsten, kommer den ansluts med molnet tjänst-url.</span><span class="sxs-lookup"><span data-stu-id="15065-195">As this virtual machine is the only virtual machine in my cloud service, I’ll connect it with the cloud service url.</span></span>

![][11]

<span data-ttu-id="15065-196">Som du ser fick jag kreativa med HTML-kod.</span><span class="sxs-lookup"><span data-stu-id="15065-196">As you can see, I got creative with my HTML code.</span></span>

<span data-ttu-id="15065-197">Vi kan också ansluta via en RDP-session från Azure-portalen via port 3389 Glöm inte.</span><span class="sxs-lookup"><span data-stu-id="15065-197">Don’t forget we can also connect through an RDP session from the Azure portal via port 3389.</span></span>

<span data-ttu-id="15065-198">Hoppas det här har bra!</span><span class="sxs-lookup"><span data-stu-id="15065-198">I hope this has been helpful!</span></span> <span data-ttu-id="15065-199">Gå och starta din infrastruktur som koden resa med Azure idag!</span><span class="sxs-lookup"><span data-stu-id="15065-199">Go  and start your infrastructure as code journey with Azure today!</span></span>

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
