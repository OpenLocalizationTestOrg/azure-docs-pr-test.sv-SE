---
title: aaaDeploy en Node.js-programmet tooLinux virtuella datorer i Azure
description: "Lär dig hur toodeploy en Node.js programmet tooLinux virtuella datorer i Azure."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a><span data-ttu-id="50ce1-103">Distribuera virtuella datorer i Azure för tooLinux en Node.js-program</span><span class="sxs-lookup"><span data-stu-id="50ce1-103">Deploy a Node.js application tooLinux Virtual Machines in Azure</span></span>
<span data-ttu-id="50ce1-104">Den här kursen visar hur tootake ett Node.js-program och distribuerar den tooLinux virtuella datorer som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="50ce1-104">This tutorial shows how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="50ce1-105">hello anvisningarna i den här kursen kan tillämpas på alla operativsystem som kan köra Node.js.</span><span class="sxs-lookup"><span data-stu-id="50ce1-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Node.js.</span></span>

<span data-ttu-id="50ce1-106">Du lär dig hur du:</span><span class="sxs-lookup"><span data-stu-id="50ce1-106">You'll learn how to:</span></span>

* <span data-ttu-id="50ce1-107">Förgrening och klona en GitHub-databas som innehåller en enkel TODO ansökan.</span><span class="sxs-lookup"><span data-stu-id="50ce1-107">Fork and clone a GitHub repository containing a simple TODO application;</span></span>
* <span data-ttu-id="50ce1-108">Skapa och konfigurera två virtuella Linux-datorer i Azure toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="50ce1-108">Create and configure two Linux virtual machines in Azure toorun hello application;</span></span>
* <span data-ttu-id="50ce1-109">Iterera hello program genom att trycka på uppdateringar toohello web klientdel virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-109">Iterate on hello application by pushing updates toohello web frontend virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="50ce1-110">toocomplete den här självstudiekursen kommer du behöver en GitHub-konto och ett Microsoft Azure-konto och hello möjlighet toouse Git från en utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-110">toocomplete this tutorial, you need a GitHub account and a Microsoft Azure account, and hello ability toouse Git from a development machine.</span></span>
> 
> <span data-ttu-id="50ce1-111">Om du inte har en GitHub-konto kan du registrera dig [här](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="50ce1-111">If you don't have a GitHub account, you can sign up [here](https://github.com/join).</span></span>
> 
> <span data-ttu-id="50ce1-112">Om du inte har en [Microsoft Azure](https://azure.microsoft.com/) konto, du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="50ce1-112">If you don't have a [Microsoft Azure](https://azure.microsoft.com/) account, you can sign up for a FREE trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="50ce1-113">Detta också leder dig igenom hello inloggningsprocessen för en [Account](http://account.microsoft.com) om du inte redan har en.</span><span class="sxs-lookup"><span data-stu-id="50ce1-113">This will also lead you through hello sign up process for a [Microsoft Account](http://account.microsoft.com) if you do not already have one.</span></span> <span data-ttu-id="50ce1-114">Om du prenumererar på Visual Studio, du kan också [aktivera MSDN-förmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="50ce1-114">Alternatively, if you are a Visual Studio subscriber, you can [activate your MSDN benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="50ce1-115">Om du inte har git på utvecklingsdatorn om du använder en Macintosh eller Windows-dator, installerar git från [här](http://www.git-scm.com).</span><span class="sxs-lookup"><span data-stu-id="50ce1-115">If you do not have git on your development machine, then if you are using a Macintosh or Windows machine, install git from [here](http://www.git-scm.com).</span></span> <span data-ttu-id="50ce1-116">Om du använder Linux, installera git med hello mekanism som är bäst för dig, till exempel `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="50ce1-116">If you are using Linux, install git using hello mechanism most appropriate for you, such as `sudo apt-get install git`.</span></span>
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a><span data-ttu-id="50ce1-117">Onödiga kostnader och klona hello TODO-program</span><span class="sxs-lookup"><span data-stu-id="50ce1-117">Forking and Cloning hello TODO Application</span></span>
<span data-ttu-id="50ce1-118">hello TODO-program som används av den här kursen implementerar en enkel web-klientdel över en MongoDB-instans som övervakas av en att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="50ce1-118">hello TODO application used by this tutorial implements a simple web frontend over a MongoDB instance that keeps track of a TODO list.</span></span> <span data-ttu-id="50ce1-119">Efter inloggningen tooGitHub [här](https://github.com/stepro/node-todo) toofind hello program och duplicera den med hello länken i hello uppe till höger.</span><span class="sxs-lookup"><span data-stu-id="50ce1-119">After signing in tooGitHub, go [here](https://github.com/stepro/node-todo) toofind hello application and fork it using hello link in hello top right.</span></span> <span data-ttu-id="50ce1-120">Detta bör du skapa en databas i ditt konto med namnet *accountname*/node-todo.</span><span class="sxs-lookup"><span data-stu-id="50ce1-120">This should create a repository in your account named *accountname*/node-todo.</span></span>

<span data-ttu-id="50ce1-121">Nu på utvecklingsdatorn, klona lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="50ce1-121">Now on your development machine, clone this repository:</span></span>

    git clone https://github.com/accountname/node-todo.git

<span data-ttu-id="50ce1-122">Vi använder den här lokala kloning av hello databasen lite senare när du ändrar toohello källkoden.</span><span class="sxs-lookup"><span data-stu-id="50ce1-122">We'll use this local clone of hello repository a little later when making changes toohello source code.</span></span>

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a><span data-ttu-id="50ce1-123">Skapa och konfigurera hello virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="50ce1-123">Creating and Configuring hello Linux Virtual Machines</span></span>
<span data-ttu-id="50ce1-124">Azure har bra stöd för raw beräkning med hjälp av virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="50ce1-124">Azure has great support for raw compute using Linux virtual machines.</span></span> <span data-ttu-id="50ce1-125">Den här delen av hello självstudiekurs visar hur du kan lätt få igång två virtuella Linux-datorer och distribuerar hello TODO programmet toothem kör hello web-klientdel på en och hello hello andra MongoDB-instansen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-125">This part of hello tutorial shows how you can easily spin up two Linux virtual machines and deploy hello TODO application toothem, running hello web frontend on one and hello MongoDB instance on hello other.</span></span>

### <a name="creating-virtual-machines"></a><span data-ttu-id="50ce1-126">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="50ce1-126">Creating Virtual Machines</span></span>
<span data-ttu-id="50ce1-127">hello enklaste sättet toocreate en ny virtuell dator i Azure är toouse hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-127">hello easiest way toocreate a new virtual machine in Azure is toouse hello Azure Portal.</span></span> <span data-ttu-id="50ce1-128">Klicka på [här](https://portal.azure.com) toosign i och starta hello Azure-portalen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="50ce1-128">Click [here](https://portal.azure.com) toosign in and launch hello Azure Portal in your web browser.</span></span> <span data-ttu-id="50ce1-129">När hello Azure-portalen har lästs in, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50ce1-129">Once hello Azure Portal has loaded, complete hello following steps:</span></span>

* <span data-ttu-id="50ce1-130">Klicka på hello ”+ nytt” länka;</span><span class="sxs-lookup"><span data-stu-id="50ce1-130">Click hello "+ New" link;</span></span>
* <span data-ttu-id="50ce1-131">Välj hello ”Compute” kategori och välj ”Ubuntu Server 14.04 LTS”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-131">Pick hello "Compute" category and choose "Ubuntu Server 14.04 LTS";</span></span>
* <span data-ttu-id="50ce1-132">Välj distributionsmodell för hello ”Resource Manager” och klicka på ”Skapa”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-132">Select hello "Resource Manager" deployment model and click "Create";</span></span>
* <span data-ttu-id="50ce1-133">Fyll i hello grunderna följa dessa riktlinjer:</span><span class="sxs-lookup"><span data-stu-id="50ce1-133">Fill in hello basics following these guidelines:</span></span>
  * <span data-ttu-id="50ce1-134">Ange ett namn som du lätt kan identifiera senare.</span><span class="sxs-lookup"><span data-stu-id="50ce1-134">Specify a name you can easily identify later;</span></span>
  * <span data-ttu-id="50ce1-135">För den här kursen Välj lösenordsautentisering;</span><span class="sxs-lookup"><span data-stu-id="50ce1-135">For this tutorial, choose Password authentication;</span></span>
  * <span data-ttu-id="50ce1-136">Skapa en ny resursgrupp med ett namn som kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="50ce1-136">Create a new resource group with an identifiable name.</span></span>
* <span data-ttu-id="50ce1-137">För hello storlek för virtuell dator är ”A1 Standard” ett rimligt val för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-137">For hello Virtual Machine size, "A1 Standard" is a reasonable choice for this tutorial.</span></span>
* <span data-ttu-id="50ce1-138">För ytterligare inställningar, kontrollera hello disktyp är ”Standard” och acceptera alla hello återstående standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="50ce1-138">For additional settings, ensure hello disk type is "Standard" and accept all hello remaining defaults.</span></span>
* <span data-ttu-id="50ce1-139">Startar hello skapas på hello sammanfattningssidan.</span><span class="sxs-lookup"><span data-stu-id="50ce1-139">Kick off hello creation on hello summary page.</span></span>

<span data-ttu-id="50ce1-140">Utför ovanstående hello bearbeta två gånger toocreate två virtuella Linux-datorer, en för hello web klientdel och en för hello MongoDB-instansen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-140">Perform hello above process twice toocreate two Linux virtual machines, one for hello web frontend and one for hello MongoDB instance.</span></span> <span data-ttu-id="50ce1-141">Skapa hello virtuella datorer tar cirka 5-10 minuter.</span><span class="sxs-lookup"><span data-stu-id="50ce1-141">Creation of hello virtual machines will take about 5-10 minutes.</span></span>

### <a name="assigning-a-dns-entry-for-virtual-machines"></a><span data-ttu-id="50ce1-142">Tilldela en DNS-post för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="50ce1-142">Assigning a DNS entry for Virtual Machines</span></span>
<span data-ttu-id="50ce1-143">Virtuella datorer som skapats i Azure är som standard endast nås via en offentlig IP-adress som 1.2.3.4.</span><span class="sxs-lookup"><span data-stu-id="50ce1-143">Virtual machines created in Azure are by default only accessible through a public IP address like 1.2.3.4.</span></span> <span data-ttu-id="50ce1-144">Vi behöver kontrollera hello datorer lättare kan identifieras genom att tilldela dem DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="50ce1-144">Let's make hello machines more easily identifiable by assigning them DNS entries.</span></span>

<span data-ttu-id="50ce1-145">När hello portal anger att hello virtuella datorer har skapats, klickar du på hello ”virtuella datorer” länk i hello vänstra navigeringsfält för och leta reda på dina datorer.</span><span class="sxs-lookup"><span data-stu-id="50ce1-145">Once hello portal indicates that hello virtual machines have been created, click on hello "Virtual machines" link in hello left navbar and locate your machines.</span></span> <span data-ttu-id="50ce1-146">För varje dator:</span><span class="sxs-lookup"><span data-stu-id="50ce1-146">For each machine:</span></span>

* <span data-ttu-id="50ce1-147">Leta upp hello Essentials-fliken och klicka på hello offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="50ce1-147">Locate hello Essentials tab and click on hello Public IP Address;</span></span>
* <span data-ttu-id="50ce1-148">Tilldela en DNS-namnetikett i hello offentliga IP-adresskonfiguration, och spara.</span><span class="sxs-lookup"><span data-stu-id="50ce1-148">In hello public IP address configuration, assign a DNS name label and save.</span></span>

<span data-ttu-id="50ce1-149">hello portal säkerställer att hello-namn som du anger är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="50ce1-149">hello portal will ensure that hello name you specify is available.</span></span> <span data-ttu-id="50ce1-150">När du har sparat hello konfiguration, virtuella datorer har liknande värdnamn för`machinename.region.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="50ce1-150">After saving hello configuration, your virtual machines will have host names similar too`machinename.region.cloudapp.azure.com`.</span></span>

### <a name="connecting-toohello-virtual-machines"></a><span data-ttu-id="50ce1-151">Ansluta toohello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="50ce1-151">Connecting toohello Virtual Machines</span></span>
<span data-ttu-id="50ce1-152">När dina virtuella datorer har etablerats var de förkonfigurerade tooallow fjärranslutningar via SSH.</span><span class="sxs-lookup"><span data-stu-id="50ce1-152">When your virtual machines were provisioned, they were pre-configured tooallow remote connections over SSH.</span></span> <span data-ttu-id="50ce1-153">Detta är hello mekanism som vi använder tooconfigure hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="50ce1-153">This is hello mechanism we will use tooconfigure hello virtual machines.</span></span> <span data-ttu-id="50ce1-154">Om du använder Windows för din utveckling behöver tooget en SSH-klienten om du inte redan har en.</span><span class="sxs-lookup"><span data-stu-id="50ce1-154">If you are using Windows for your development, you will need tooget an SSH client if you do not already have one.</span></span> <span data-ttu-id="50ce1-155">Ett vanligt val är PuTTY som kan hämtas från [här](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span><span class="sxs-lookup"><span data-stu-id="50ce1-155">A common choice here is PuTTY, which can be downloaded from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span></span> <span data-ttu-id="50ce1-156">Macintosh- och Linux OSes levereras med en version av SSH förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="50ce1-156">Macintosh and Linux OSes come with a version of SSH pre-installed.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="50ce1-157">Konfigurera hello Web klientdel virtuell dator</span><span class="sxs-lookup"><span data-stu-id="50ce1-157">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="50ce1-158">SSH toohello klientdel datorn på webbservern du skapat med PuTTY, ssh kommandorad eller din andra favorit SSH-verktyget.</span><span class="sxs-lookup"><span data-stu-id="50ce1-158">SSH toohello web frontend machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="50ce1-159">Du bör se ett välkomstmeddelande följt av en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="50ce1-159">You should see a welcome message followed by a command prompt.</span></span>

<span data-ttu-id="50ce1-160">Först se till att git och noden både är installerade:</span><span class="sxs-lookup"><span data-stu-id="50ce1-160">First, let's make sure that git and node are both installed:</span></span>

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

<span data-ttu-id="50ce1-161">Eftersom hello programmets web klientdel är beroende av vissa inbyggda Node.js-moduler, måste vi också tooinstall hello grundläggande uppsättning verktyg:</span><span class="sxs-lookup"><span data-stu-id="50ce1-161">Since hello application's web frontend relies on some native Node.js modules, we also need tooinstall hello essential set of build tools:</span></span>

    sudo apt-get install -y build-essential

<span data-ttu-id="50ce1-162">Slutligen ska vi installera ett Node.js-program som kallas *alltid*, som hjälper toorun Node.js server-program:</span><span class="sxs-lookup"><span data-stu-id="50ce1-162">Finally, let's install a Node.js application called *forever*, which helps toorun Node.js server applications:</span></span>

    sudo npm install -g forever

<span data-ttu-id="50ce1-163">Detta är alla hello beroenden som krävs på den här virtuella datorn toobe kan toorun hello programmets web klientdel, så det är dags att köras.</span><span class="sxs-lookup"><span data-stu-id="50ce1-163">These are all hello dependencies needed on this virtual machine toobe able toorun hello application's web frontend, so let's get that running.</span></span> <span data-ttu-id="50ce1-164">toodo kan vi först skapar en utan kloning av hello GitHub-lagringsplats som du tidigare forked så att du kan enkelt publicera uppdateringar toohello virtuell dator (vi tar upp det här scenariot uppdateringen senare) och klona hello utan kloning tooprovide en version av hello databasen som faktiskt kan utföras.</span><span class="sxs-lookup"><span data-stu-id="50ce1-164">toodo this, we will first create a bare clone of hello GitHub repository you previously forked so that you can easily publish updates toohello virtual machine (we'll cover this update scenario later), and then clone hello bare clone tooprovide a version of hello repository that can actually be executed.</span></span>

<span data-ttu-id="50ce1-165">Från hello arbetskatalog (~), kör följande kommandon hello (ersätter *accountname* med ditt användarkonto i GitHub):</span><span class="sxs-lookup"><span data-stu-id="50ce1-165">Starting from hello home (~) directory, run hello following commands (replacing *accountname* with your GitHub user account name):</span></span>

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

<span data-ttu-id="50ce1-166">Nu ange hello nod todo-katalogen och köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="50ce1-166">Now enter hello node-todo directory and run these commands:</span></span>

    npm install
    forever start server.js

<span data-ttu-id="50ce1-167">hello programmets web klientdel körs nu, men det finns ytterligare ett steg innan du kan komma åt programmet hello från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="50ce1-167">hello application's web frontend is now running, however there is one more step before you can access hello application from a web browser.</span></span> <span data-ttu-id="50ce1-168">hello virtuell dator som du skapade skyddas av en Azure-resurs kallas en *nätverkssäkerhetsgruppen*, som skapades för dig när du etablerat hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-168">hello virtual machine you created is protected by an Azure resource called a *network security group*, which was created for you when you provisioned hello virtual machine.</span></span> <span data-ttu-id="50ce1-169">Den här resursen kan för närvarande endast externa begäranden tooport 22 toobe dirigeras toohello virtuell dator, vilket gör att SSH-kommunikation med hello datorn men inget annat.</span><span class="sxs-lookup"><span data-stu-id="50ce1-169">Currently, this resource only allows external requests tooport 22 toobe routed toohello virtual machine, which enables SSH communication with hello machine but nothing else.</span></span> <span data-ttu-id="50ce1-170">Så i ordning tooview hello TODO-program som är konfigurerade toorun på port 8080, måste den här porten också toobe öppnat.</span><span class="sxs-lookup"><span data-stu-id="50ce1-170">So in order tooview hello TODO application, which is configured toorun on port 8080, this port also needs toobe opened up.</span></span>

<span data-ttu-id="50ce1-171">Returnera toohello Azure-portalen och slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50ce1-171">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="50ce1-172">Klicka på ”resursgrupper” i vänster navigeringsfält för hello;</span><span class="sxs-lookup"><span data-stu-id="50ce1-172">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="50ce1-173">Välj hello resursgruppen som innehåller den virtuella datorn;</span><span class="sxs-lookup"><span data-stu-id="50ce1-173">Select hello resource group that contains your virtual machine;</span></span>
* <span data-ttu-id="50ce1-174">Välj i hello resulterande lista över resurser, hello nätverkssäkerhetsgruppen (hello en med en sköldikon);</span><span class="sxs-lookup"><span data-stu-id="50ce1-174">In hello resulting list of resources, select hello network security group (hello one with a shield icon);</span></span>
* <span data-ttu-id="50ce1-175">I hello egenskaper, väljer du ”inkommande säkerhetsregler”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-175">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="50ce1-176">I hello i verktygsfältet klickar du på ”Lägg till”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-176">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="50ce1-177">Ange ett namn som ”standard-Tillåt-todo”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-177">Provide a name like "default-allow-todo";</span></span>
* <span data-ttu-id="50ce1-178">Ange hello-protokollet för ”TCP”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-178">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="50ce1-179">Ange hello Målportintervall för ”8080”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-179">Set hello destination port range too"8080";</span></span>
* <span data-ttu-id="50ce1-180">Klicka på OK och vänta hello säkerhet regeln toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="50ce1-180">Click OK and wait for hello security rule toobe created.</span></span>

<span data-ttu-id="50ce1-181">När du har skapat den här säkerhetsregeln hello TODO program visas offentligt på hello internet och du kan bläddra tooit, till exempel med en URL som:</span><span class="sxs-lookup"><span data-stu-id="50ce1-181">After creating this security rule, hello TODO application is publically visible on hello internet and you can browse tooit, for instance using a URL such as:</span></span>

    http://machinename.region.cloudapp.azure.com:8080

<span data-ttu-id="50ce1-182">Du ser att även om vi ännu inte har konfigurerat hello MongoDB virtuella hello TODO program visas toobe helt funktionell.</span><span class="sxs-lookup"><span data-stu-id="50ce1-182">You will notice that even though we have not yet configured hello MongoDB virtual machine, hello TODO application appears toobe quite functional.</span></span> <span data-ttu-id="50ce1-183">Detta beror på att hello källdatabasen är hårdkodad toouse före distribuerade MongoDB-instansen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-183">This is because hello source repository is hardcoded toouse a pre-deployed MongoDB instance.</span></span> <span data-ttu-id="50ce1-184">När vi har konfigurerat hello MongoDB virtuell dator, ska vi gå tillbaka och ändra hello källa kod tooutilize våra privata MongoDB-instansen i stället.</span><span class="sxs-lookup"><span data-stu-id="50ce1-184">Once we have configured hello MongoDB virtual machine, we will go back and change hello source code tooutilize our private MongoDB instance instead.</span></span>

### <a name="configuring-hello-mongodb-virtual-machine"></a><span data-ttu-id="50ce1-185">Konfigurera hello MongoDB virtuell dator</span><span class="sxs-lookup"><span data-stu-id="50ce1-185">Configuring hello MongoDB Virtual Machine</span></span>
<span data-ttu-id="50ce1-186">SSH toohello andra dator du skapat med PuTTY, ssh kommandorad eller din andra favorit SSH-verktyget.</span><span class="sxs-lookup"><span data-stu-id="50ce1-186">SSH toohello second machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="50ce1-187">Installera MongoDB efter hello välkomstmeddelande och Kommandotolken, (instruktionerna togs från [här](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span><span class="sxs-lookup"><span data-stu-id="50ce1-187">After seeing hello welcome message and command prompt, install MongoDB (these instructions were taken from [here](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span></span>

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

<span data-ttu-id="50ce1-188">Som standard konfigureras MongoDB så att det bara kan kommas åt lokalt.</span><span class="sxs-lookup"><span data-stu-id="50ce1-188">By default, MongoDB is configured so it can only be accessed locally.</span></span> <span data-ttu-id="50ce1-189">För den här självstudiekursen kommer vi konfigurera MongoDB så att den kan nås från hello programmets virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="50ce1-189">For this tutorial, we will configure MongoDB so it can be accessed from hello application's virtual machine.</span></span> <span data-ttu-id="50ce1-190">I en kontext som sudo, öppna hello /etc/mongod.conf filen och leta upp hello `# network interfaces` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="50ce1-190">In a sudo context, open hello /etc/mongod.conf file and locate hello `# network interfaces` section.</span></span> <span data-ttu-id="50ce1-191">Ändra hello `net.bindIp` configuration värdet för`0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="50ce1-191">Change hello `net.bindIp` configuration value too`0.0.0.0`.</span></span>

> [!NOTE]
> <span data-ttu-id="50ce1-192">Den här konfigurationen är hello avsedd endast den här kursen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-192">This configuration is for hello purposes of this tutorial only.</span></span> <span data-ttu-id="50ce1-193">Det är **inte** en rekommenderad säkerhetsrutin och ska inte användas i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="50ce1-193">It is **NOT** a recommended security practice and should not be used in production environments.</span></span>
> 
> 

<span data-ttu-id="50ce1-194">Nu ska du kontrollera hello MongoDB-tjänsten har startats:</span><span class="sxs-lookup"><span data-stu-id="50ce1-194">Now ensure hello MongoDB service has been started:</span></span>

    sudo service mongod restart

<span data-ttu-id="50ce1-195">MongoDB fungerar över port 27017 som standard.</span><span class="sxs-lookup"><span data-stu-id="50ce1-195">MongoDB operates over port 27017 by default.</span></span> <span data-ttu-id="50ce1-196">Så i hello samma sätt som vi behövde tooopen port 8080 på hello web klientdel virtuell dator, vi behöver tooopen port 27017 på hello MongoDB virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="50ce1-196">So, in hello same way that we needed tooopen port 8080 on hello web frontend virtual machine, we need tooopen port 27017 on hello MongoDB virtual machine.</span></span>

<span data-ttu-id="50ce1-197">Returnera toohello Azure-portalen och slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50ce1-197">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="50ce1-198">Klicka på ”resursgrupper” i vänster navigeringsfält för hello;</span><span class="sxs-lookup"><span data-stu-id="50ce1-198">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="50ce1-199">Välj hello resursgruppen som innehåller hello MongoDB virtuella maskinen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-199">Select hello resource group that contains hello MongoDB virtual machine;</span></span>
* <span data-ttu-id="50ce1-200">I hello resulterande lista över resurser, väljer du hello nätverkssäkerhetsgruppen (hello en med en sköldikon) med hello samma namn som du gav toohello MongoDB virtuella maskinen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-200">In hello resulting list of resources, select hello network security group (hello one with a shield icon) with hello same name that you gave toohello MongoDB virtual machine;</span></span>
* <span data-ttu-id="50ce1-201">I hello egenskaper, väljer du ”inkommande säkerhetsregler”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-201">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="50ce1-202">I hello i verktygsfältet klickar du på ”Lägg till”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-202">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="50ce1-203">Ange ett namn som ”standard-Tillåt-mongo”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-203">Provide a name like "default-allow-mongo";</span></span>
* <span data-ttu-id="50ce1-204">Ange hello-protokollet för ”TCP”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-204">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="50ce1-205">Ange hello Målportintervall för ”27017”;</span><span class="sxs-lookup"><span data-stu-id="50ce1-205">Set hello destination port range too"27017";</span></span>
* <span data-ttu-id="50ce1-206">Klicka på OK och vänta hello säkerhet regeln toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="50ce1-206">Click OK and wait for hello security rule toobe created.</span></span>

## <a name="iterating-on-hello-todo-application"></a><span data-ttu-id="50ce1-207">Iteration av hello TODO-program</span><span class="sxs-lookup"><span data-stu-id="50ce1-207">Iterating on hello TODO application</span></span>
<span data-ttu-id="50ce1-208">Hittills har vi har etablerat två virtuella Linux-datorer: en som kör hello programmets web klientdel och en som kör en MongoDB-instansen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-208">So far, we have provisioned two Linux virtual machines: one that is running hello application's web frontend and one that is running a MongoDB instance.</span></span> <span data-ttu-id="50ce1-209">Men det finns ett problem - hello web klientdel använder inte hello etableras MongoDB-instansen ännu.</span><span class="sxs-lookup"><span data-stu-id="50ce1-209">But there is a problem - hello web frontend isn't actually using hello provisioned MongoDB instance yet.</span></span> <span data-ttu-id="50ce1-210">Korrigeras genom att uppdatera hello web klientdel koden toouse en miljövariabel i stället för en hårdkodad instans.</span><span class="sxs-lookup"><span data-stu-id="50ce1-210">Let's fix that by updating hello web frontend code toouse an environment variable instead of a hard-coded instance.</span></span>

### <a name="changing-hello-todo-application"></a><span data-ttu-id="50ce1-211">Ändra hello TODO-program</span><span class="sxs-lookup"><span data-stu-id="50ce1-211">Changing hello TODO application</span></span>
<span data-ttu-id="50ce1-212">Öppna hello på utvecklingsdatorn där du först har klonat hello nod-göra databasen, `node-todo/config/database.js` filen i din favorit-redigeraren och ändra hello url-värdet från hello hårdkodat värde som `mongodb://...` för`process.env.MONGODB`.</span><span class="sxs-lookup"><span data-stu-id="50ce1-212">On your development machine where you first cloned hello node-todo repository, open hello `node-todo/config/database.js` file in your favorite editor and change hello url value from hello hard-coded value like `mongodb://...` too`process.env.MONGODB`.</span></span>

<span data-ttu-id="50ce1-213">Genomför ändringarna och skicka toohello GitHub master:</span><span class="sxs-lookup"><span data-stu-id="50ce1-213">Commit your changes and push toohello GitHub master:</span></span>

    git commit -am "Get MongoDB instance from env"
    git push origin master

<span data-ttu-id="50ce1-214">Tyvärr Publicera inte detta hello ändra toohello web klientdel virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-214">Unfortunately, this doesn't publish hello change toohello web frontend virtual machine.</span></span> <span data-ttu-id="50ce1-215">Låt oss se några fler ändringar toothat virtuella tooenable en tydlig och effektiv mekanism för att publicera uppdateringar så att du kan snabbt se hello effekten av hello ändringar i hello live-miljö.</span><span class="sxs-lookup"><span data-stu-id="50ce1-215">Let's make a few more changes toothat virtual machine tooenable a simple but effective mechanism for publishing updates so you can quickly observe hello effect of hello changes in hello live environment.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="50ce1-216">Konfigurera hello Web klientdel virtuell dator</span><span class="sxs-lookup"><span data-stu-id="50ce1-216">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="50ce1-217">Kom ihåg att vi tidigare har skapat en utan kloning av hello nod-göra databasen på hello web klientdel virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="50ce1-217">Recall that we previously created a bare clone of hello node-todo repository on hello web frontend virtual machine.</span></span> <span data-ttu-id="50ce1-218">Det visar sig att den här åtgärden skapas en ny Git remote toowhich ändringar kan skickas.</span><span class="sxs-lookup"><span data-stu-id="50ce1-218">It turns out that this action created a new Git remote toowhich changes can be pushed.</span></span> <span data-ttu-id="50ce1-219">Dock får bara trycka toothis remote ganska inte hello snabb iteration modell som utvecklare behöver när du arbetar med koden.</span><span class="sxs-lookup"><span data-stu-id="50ce1-219">However, simply pushing toothis remote doesn't quite give hello rapid iteration model that developers are looking for when working on their code.</span></span>

<span data-ttu-id="50ce1-220">Vad vi vill toobe kan toodo se till att när ett push toohello fjärrdatabasen på den virtuella datorn hello inträffar hello körs TODO programmet uppdateras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="50ce1-220">What we would like toobe able toodo is ensure that when a push toohello remote repository on hello virtual machine occurs, hello running TODO application is automatically updated.</span></span> <span data-ttu-id="50ce1-221">Detta är som tur är kan enkelt tooachieve med git.</span><span class="sxs-lookup"><span data-stu-id="50ce1-221">Fortunately, this is easy tooachieve with git.</span></span>

<span data-ttu-id="50ce1-222">Git exponerar ett antal kod som kan anropas på en viss tidpunkt tooreact tooactions vidtagits hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-222">Git exposes a number of hooks that are called at particular times tooreact tooactions taken on hello repository.</span></span> <span data-ttu-id="50ce1-223">Dessa anges med kommandoskript i hello databas `hooks` mapp.</span><span class="sxs-lookup"><span data-stu-id="50ce1-223">These are specified using shell scripts in hello repository's `hooks` folder.</span></span> <span data-ttu-id="50ce1-224">hello hook som är mest användbart för hello automatisk uppdatering scenariot är hello `post-update` händelse.</span><span class="sxs-lookup"><span data-stu-id="50ce1-224">hello hook that is most applicable for hello auto-update scenario is hello `post-update` event.</span></span>

<span data-ttu-id="50ce1-225">I en SSH-session toohello web klientdel virtuell dator, ändrar du toohello `~/node-todo.git/hooks` directory och lägga till hello efter innehåll tooa fil med namnet `post-update` (ersätter `machinename` och `region` med informationen om MongoDB virtuella):</span><span class="sxs-lookup"><span data-stu-id="50ce1-225">In a SSH session toohello web frontend virtual machine, change toohello `~/node-todo.git/hooks` directory and add hello following content tooa file named `post-update` (replacing `machinename` and `region` with your MongoDB virtual machine information):</span></span>

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

<span data-ttu-id="50ce1-226">Se till att den här filen är körbara genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="50ce1-226">Ensure this file is executable by running hello following command:</span></span>

    chmod 755 post-update

<span data-ttu-id="50ce1-227">Det här skriptet säkerställer att hello aktuella serverprogrammet har stoppats, hello koden i hello klonade databasen är uppdaterad toohello senaste, eventuella uppdaterade beroenden är uppfyllda och hello-servern har startats om.</span><span class="sxs-lookup"><span data-stu-id="50ce1-227">This script ensures that hello current server application is stopped, hello code in hello cloned repository is updated toohello latest, any updated dependencies are satisfied, and hello server is restarted.</span></span> <span data-ttu-id="50ce1-228">Det innebär också att hello-miljö har konfigurerats som förberedelse för att ta emot vår första programmet uppdatering tooget hello MongoDB-instansen från en miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="50ce1-228">It also ensures that hello environment has been configured in preparation for receiving our first application update tooget hello MongoDB instance from an environment variable.</span></span>

### <a name="configuring-your-development-machine"></a><span data-ttu-id="50ce1-229">Konfigurera utvecklingsdatorn</span><span class="sxs-lookup"><span data-stu-id="50ce1-229">Configuring your Development Machine</span></span>
<span data-ttu-id="50ce1-230">Nu ska vi gå utvecklingsdatorn ansluten toohello web klientdel virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-230">Now let's get your development machine hooked up toohello web frontend virtual machine.</span></span> <span data-ttu-id="50ce1-231">Detta är så enkelt som att lägga till hello utan databasen på hello virtuell dator som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="50ce1-231">This is as simple as adding hello bare repository on hello virtual machine as a remote.</span></span> <span data-ttu-id="50ce1-232">Kör följande kommando toodo hello detta (ersätter *användare* med ditt inloggningsnamn för web klientdel virtuell dator och *machinename* och *region* efter behov):</span><span class="sxs-lookup"><span data-stu-id="50ce1-232">Run hello following command toodo this (replacing *user* with your web frontend virtual machine login name and *machinename* and *region* as appropriate):</span></span>

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

<span data-ttu-id="50ce1-233">Det här är allt som behövs tooenable push-installation eller publicering i praktiken, ändras toohello web klientdel virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="50ce1-233">This is all that is needed tooenable pushing, or in effect publishing, changes toohello web frontend virtual machine.</span></span>

### <a name="publishing-updates"></a><span data-ttu-id="50ce1-234">Publicera uppdateringar</span><span class="sxs-lookup"><span data-stu-id="50ce1-234">Publishing Updates</span></span>
<span data-ttu-id="50ce1-235">Vi publicerar hello en ändring har gjorts hittills så att programmet hello kommer att använda våra egna MongoDB-instansen:</span><span class="sxs-lookup"><span data-stu-id="50ce1-235">Let's publish hello one change that has been made so far so that hello application will use our own MongoDB instance:</span></span>

    git push azure master

<span data-ttu-id="50ce1-236">Du bör se utdata liknande toothis:</span><span class="sxs-lookup"><span data-stu-id="50ce1-236">You should see output similar toothis:</span></span>

    Counting objects: 4, done.
    Delta compression using up too4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

<span data-ttu-id="50ce1-237">När det här kommandot har slutförts, försök att uppdatera programmet hello i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="50ce1-237">After this command completes, try refreshing hello application in a web browser.</span></span> <span data-ttu-id="50ce1-238">Du ska kunna toosee att hello TODO-listan som visas här är tom och inte längre bundet toohello delade distribueras MongoDB-instansen.</span><span class="sxs-lookup"><span data-stu-id="50ce1-238">You should be able toosee that hello TODO list presented here is empty and no longer tied toohello shared deployed MongoDB instance.</span></span>

<span data-ttu-id="50ce1-239">toocomplete hello kursen ska vi ändra en annan, tydligare.</span><span class="sxs-lookup"><span data-stu-id="50ce1-239">toocomplete hello tutorial, let's make another, more visible change.</span></span> <span data-ttu-id="50ce1-240">På utvecklingsdatorn, öppna hello node-todo/public/index.html filen med din favorit-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="50ce1-240">On your development machine, open hello node-todo/public/index.html file using your favorite editor.</span></span> <span data-ttu-id="50ce1-241">Leta upp hello jumbotron sidhuvud och ändra hello rubrik från ”jag Todo-aholic” för ”jag Todo-aholic i Azure”!.</span><span class="sxs-lookup"><span data-stu-id="50ce1-241">Locate hello jumbotron header and change  hello title from "I'm a Todo-aholic" too"I'm a Todo-aholic on Azure!".</span></span>

<span data-ttu-id="50ce1-242">Nu ska vi genomför:</span><span class="sxs-lookup"><span data-stu-id="50ce1-242">Now let's commit:</span></span>

    git commit -am "Azurify hello title"

<span data-ttu-id="50ce1-243">Den här gången, vi publicerar hello ändra tooAzure innan du skickar det till GitHub-repo-tooback toohello:</span><span class="sxs-lookup"><span data-stu-id="50ce1-243">This time, let's publish hello change tooAzure before pushing it tooback toohello GitHub repo:</span></span>

    git push azure master

<span data-ttu-id="50ce1-244">När det här kommandot har slutförts, ser uppdatera hello webbsida och du hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="50ce1-244">Once this command completes, refresh hello web page and you will see hello changes.</span></span> <span data-ttu-id="50ce1-245">Eftersom de passar push hello ändra tillbaka toohello ursprung fjärråtkomst:</span><span class="sxs-lookup"><span data-stu-id="50ce1-245">Since they look good, push hello change back toohello origin remote:</span></span> 

    git push origin master

## <a name="next-steps"></a><span data-ttu-id="50ce1-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50ce1-246">Next Steps</span></span>
<span data-ttu-id="50ce1-247">Den här artikeln visar hur tootake ett Node.js-program och distribuerar den tooLinux virtuella datorer som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="50ce1-247">This article showed how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="50ce1-248">toolearn mer information om Linux-datorer i Azure, se [introduktion tooLinux på Azure](/documentation/articles/virtual-machines-linux-introduction/).</span><span class="sxs-lookup"><span data-stu-id="50ce1-248">toolearn more about Linux virtual machines in Azure, see [Introduction tooLinux on Azure](/documentation/articles/virtual-machines-linux-introduction/).</span></span>

<span data-ttu-id="50ce1-249">Mer information om hur toodevelop Node.js-program i Azure, se hello [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="50ce1-249">For more information about how toodevelop Node.js applications on Azure, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

