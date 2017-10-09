---
title: aaaCreate en utveckling pipeline i Azure med Jenkins | Microsoft Docs
description: "Lär dig hur toocreate en Jenkins virtuell dator i Azure som hämtar från GitHub på varje kod genomför och skapar en ny Docker behållare toorun din app"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c079e3c9186c9da0a3e51e1823215779c565e0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a><span data-ttu-id="3a1c1-103">Hur toocreate en infrastruktur för utveckling på en Linux-VM i Azure med Jenkins, GitHub och Docker</span><span class="sxs-lookup"><span data-stu-id="3a1c1-103">How toocreate a development infrastructure on a Linux VM in Azure with Jenkins, GitHub, and Docker</span></span>
<span data-ttu-id="3a1c1-104">tooautomate hello build och testa fas för programutveckling, kan du använda en kontinuerlig integrering och distribution (CI/CD) pipeline.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-104">tooautomate hello build and test phase of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="3a1c1-105">I den här självstudiekursen skapar du en CI/CD-pipeline på en Azure VM att:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3a1c1-106">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="3a1c1-107">Installera och konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-107">Install and configure Jenkins</span></span>
> * <span data-ttu-id="3a1c1-108">Skapa webhook integrering mellan GitHub och Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-108">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="3a1c1-109">Skapa och genomför utlösaren Jenkins skapa jobb från GitHub</span><span class="sxs-lookup"><span data-stu-id="3a1c1-109">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="3a1c1-110">Skapa en Docker-avbildning för din app</span><span class="sxs-lookup"><span data-stu-id="3a1c1-110">Create a Docker image for your app</span></span>
> * <span data-ttu-id="3a1c1-111">Kontrollera GitHub incheckningar Skapa ny Docker-avbildning och uppdateringar som kör appen</span><span class="sxs-lookup"><span data-stu-id="3a1c1-111">Verify GitHub commits build new Docker image and updates running app</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3a1c1-112">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3a1c1-113">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3a1c1-114">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3a1c1-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-jenkins-instance"></a><span data-ttu-id="3a1c1-115">Skapa en instans av Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-115">Create Jenkins instance</span></span>
<span data-ttu-id="3a1c1-116">I en tidigare självstudiekurs om [hur toocustomize en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md), du lärt dig hur tooautomate VM anpassning med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-116">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="3a1c1-117">Den här kursen använder ett moln init filen tooinstall Jenkins och Docker på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-117">This tutorial uses a cloud-init file tooinstall Jenkins and Docker on a VM.</span></span> 

<span data-ttu-id="3a1c1-118">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-118">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="3a1c1-119">Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-119">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="3a1c1-120">Ange `sensible-editor cloud-init-jenkins.txt` toocreate hello filen och visas i listan över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-120">Enter `sensible-editor cloud-init-jenkins.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="3a1c1-121">Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-121">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

<span data-ttu-id="3a1c1-122">Innan du kan skapa en virtuell dator, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3a1c1-122">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3a1c1-123">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupJenkins* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-123">hello following example creates a resource group named *myResourceGroupJenkins* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

<span data-ttu-id="3a1c1-124">Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3a1c1-124">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="3a1c1-125">Använd hello `--custom-data` parametern toopass i konfigurationsfilen molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-125">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="3a1c1-126">Ange hello fullständig sökväg för*moln-init-jenkins.txt* om du har sparat filen hello utanför arbetskatalogen finns.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-126">Provide hello full path too*cloud-init-jenkins.txt* if you saved hello file outside of your present working directory.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

<span data-ttu-id="3a1c1-127">Det tar några minuter för hello VM toobe skapas och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-127">It takes a few minutes for hello VM toobe created and configured.</span></span>

<span data-ttu-id="3a1c1-128">tooallow web tooreach trafik den virtuella datorn, Använd [az vm öppna port](/cli/azure/vm#open-port) tooopen port *8080* för Jenkins trafik och port *1337* för hello Node.js-app som är används toorun en exempelapp:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-128">tooallow web traffic tooreach your VM, use [az vm open-port](/cli/azure/vm#open-port) tooopen port *8080* for Jenkins traffic and port *1337* for hello Node.js app that is used toorun a sample app:</span></span>

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a><span data-ttu-id="3a1c1-129">Konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-129">Configure Jenkins</span></span>
<span data-ttu-id="3a1c1-130">tooaccess din Jenkins instansen kan du få hello offentliga IP-adressen för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-130">tooaccess your Jenkins instance, obtain hello public IP address of your VM:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="3a1c1-131">Av säkerhetsskäl måste du tooenter hello inledande adminlösenord som lagras i en textfil på din Virtuella toostart hello Jenkins installera.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-131">For security purposes, you need tooenter hello initial admin password that is stored in a text file on your VM toostart hello Jenkins install.</span></span> <span data-ttu-id="3a1c1-132">Använd hello offentlig IP-adress som erhölls i hello föregående steg tooSSH tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-132">Use hello public IP address obtained in hello previous step tooSSH tooyour VM:</span></span>

```bash
ssh azureuser@<publicIps>
```

<span data-ttu-id="3a1c1-133">Visa hello `initialAdminPassword` för din Jenkins installera och kopiera den:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-133">View hello `initialAdminPassword` for your Jenkins install and copy it:</span></span>

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<span data-ttu-id="3a1c1-134">Om hello-filen är inte tillgängligt ännu, Vänta några minuter för molnet init toocomplete hello Jenkins och Docker-installation.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-134">If hello file isn't available yet, wait a couple more minutes for cloud-init toocomplete hello Jenkins and Docker install.</span></span>

<span data-ttu-id="3a1c1-135">Öppna en webbläsare och gå för`http://<publicIps>:8080`.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-135">Now open a web browser and go too`http://<publicIps>:8080`.</span></span> <span data-ttu-id="3a1c1-136">Slutför hello Jenkins installationen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-136">Complete hello initial Jenkins setup as follows:</span></span>

- <span data-ttu-id="3a1c1-137">Ange hello *initialAdminPassword* från hello VM i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-137">Enter hello *initialAdminPassword* obtained from hello VM in hello previous step.</span></span>
- <span data-ttu-id="3a1c1-138">Klicka på **Välj tooinstall plugin-program**</span><span class="sxs-lookup"><span data-stu-id="3a1c1-138">Click **Select plugins tooinstall**</span></span>
- <span data-ttu-id="3a1c1-139">Sök efter *GitHub* hello texten i rutan överst hello väljer hello *GitHub-plugin-programmet*, klicka på **installera**</span><span class="sxs-lookup"><span data-stu-id="3a1c1-139">Search for *GitHub* in hello text box across hello top, select hello *GitHub plugin*, then click **Install**</span></span>
- <span data-ttu-id="3a1c1-140">toocreate ett Jenkins användarkonto, Fyll i hello formuläret efter behov.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-140">toocreate a Jenkins user account, fill out hello form as desired.</span></span> <span data-ttu-id="3a1c1-141">Du bör skapa första Jenkins användaren snarare än fortsätter som hello admin standardkonto från ett säkerhetsperspektiv.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-141">From a security perspective, you should create this first Jenkins user rather than continuing as hello default admin account.</span></span>
- <span data-ttu-id="3a1c1-142">När du är klar klickar du på **börja använda Jenkins**</span><span class="sxs-lookup"><span data-stu-id="3a1c1-142">When finished, click **Start using Jenkins**</span></span>


## <a name="create-github-webhook"></a><span data-ttu-id="3a1c1-143">Skapa GitHub-webhook</span><span class="sxs-lookup"><span data-stu-id="3a1c1-143">Create GitHub webhook</span></span>
<span data-ttu-id="3a1c1-144">tooconfigure hello integrering med GitHub, öppna hello [Node.js Hello World exempelapp](https://github.com/Azure-Samples/nodejs-docs-hello-world) från hello Azure-exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-144">tooconfigure hello integration with GitHub, open hello [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) from hello Azure samples repo.</span></span> <span data-ttu-id="3a1c1-145">toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-145">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

<span data-ttu-id="3a1c1-146">Skapa en webhook inuti hello förgrening som du skapade:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-146">Create a webhook inside hello fork you created:</span></span>

- <span data-ttu-id="3a1c1-147">Klicka på **inställningar**och välj **integreringar & tjänster** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-147">Click **Settings**, then select **Integrations & services** on hello left-hand side.</span></span>
- <span data-ttu-id="3a1c1-148">Klicka på **lägga till tjänst**, ange *Jenkins* i filterfältet.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-148">Click **Add service**, then enter *Jenkins* in filter box.</span></span>
- <span data-ttu-id="3a1c1-149">Välj *Jenkins (GitHub-plugin-programmet)*</span><span class="sxs-lookup"><span data-stu-id="3a1c1-149">Select *Jenkins (GitHub plugin)*</span></span>
- <span data-ttu-id="3a1c1-150">För hello **Jenkins koppla URL**, ange `http://<publicIps>:8080/github-webhook/`.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-150">For hello **Jenkins hook URL**, enter `http://<publicIps>:8080/github-webhook/`.</span></span> <span data-ttu-id="3a1c1-151">Kontrollera att du inkluderar avslutande hello /</span><span class="sxs-lookup"><span data-stu-id="3a1c1-151">Make sure you include hello trailing /</span></span>
- <span data-ttu-id="3a1c1-152">Klicka på **lägga till tjänst**</span><span class="sxs-lookup"><span data-stu-id="3a1c1-152">Click **Add service**</span></span>

![Lägg till GitHub-repo-tooyour forked webhooken](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a><span data-ttu-id="3a1c1-154">Skapa Jenkins jobb</span><span class="sxs-lookup"><span data-stu-id="3a1c1-154">Create Jenkins job</span></span>
<span data-ttu-id="3a1c1-155">toohave Jenkins svara tooan händelsen i GitHub, till exempel när en kod, skapa ett Jenkins-jobb.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-155">toohave Jenkins respond tooan event in GitHub such as committing code, create a Jenkins job.</span></span> 

<span data-ttu-id="3a1c1-156">Klicka på webbplatsen Jenkins **skapa nya jobb** från hello startsida:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-156">In your Jenkins website, click **Create new jobs** from hello home page:</span></span>

- <span data-ttu-id="3a1c1-157">Ange *HelloWorld* som jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-157">Enter *HelloWorld* as job name.</span></span> <span data-ttu-id="3a1c1-158">Välj **Freestyle projektet**och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-158">Choose **Freestyle project**, then select **OK**.</span></span>
- <span data-ttu-id="3a1c1-159">Under hello **allmänna** väljer **GitHub** projektet och ange din andelen vridvuxna lagringsplatsen URL, exempelvis *https://github.com/iainfoulds/nodejs-docs-hello-world*</span><span class="sxs-lookup"><span data-stu-id="3a1c1-159">Under hello **General** section, select **GitHub** project and enter your forked repo URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world*</span></span>
- <span data-ttu-id="3a1c1-160">Under hello **datakällan kod management** väljer **Git**, ange din andelen vridvuxna lagringsplatsen *.git* URL, exempelvis *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span><span class="sxs-lookup"><span data-stu-id="3a1c1-160">Under hello **Source code management** section, select **Git**, enter your forked repo *.git* URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span></span>
- <span data-ttu-id="3a1c1-161">Under hello **Skapa utlösare** väljer **GitHub hook utlösare för GITscm avsökning**.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-161">Under hello **Build Triggers** section, select **GitHub hook trigger for GITscm polling**.</span></span>
- <span data-ttu-id="3a1c1-162">Under hello **skapa** väljer **Lägg till build steg**.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-162">Under hello **Build** section, choose **Add build step**.</span></span> <span data-ttu-id="3a1c1-163">Välj **köra shell**, ange `echo "Testing"` i toocommand fönster.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-163">Select **Execute shell**, then enter `echo "Testing"` in toocommand window.</span></span>
- <span data-ttu-id="3a1c1-164">Klicka på **spara** längst hello hello jobb-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-164">Click **Save** at hello bottom of hello jobs window.</span></span>


## <a name="test-github-integration"></a><span data-ttu-id="3a1c1-165">Testa GitHub-integrering</span><span class="sxs-lookup"><span data-stu-id="3a1c1-165">Test GitHub integration</span></span>
<span data-ttu-id="3a1c1-166">tootest hello GitHub integrering med Jenkins, bekräfta att en ändring i din förgrening.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-166">tootest hello GitHub integration with Jenkins, commit a change in your fork.</span></span> 

<span data-ttu-id="3a1c1-167">Tillbaka i GitHub-webbgränssnitt, Välj din andelen vridvuxna lagringsplatsen och klicka sedan på hello **index.js** fil.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-167">Back in GitHub web UI, select your forked repo, and then click hello **index.js** file.</span></span> <span data-ttu-id="3a1c1-168">Klicka på hello penna ikonen tooedit filen så läser rad 6:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-168">Click hello pencil icon tooedit this file so line 6 reads:</span></span>

```nodejs
response.end("Hello World!");
```

<span data-ttu-id="3a1c1-169">toocommit dina ändringar klickar du på hello **genomför ändringar** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-169">toocommit your changes, click hello **Commit changes** button at hello bottom.</span></span>

<span data-ttu-id="3a1c1-170">I Jenkins, startar en ny version under hello **skapa historik** avsnitt i hello längst ned till vänster på sidan jobb.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-170">In Jenkins, a new build starts under hello **Build history** section of hello bottom left-hand corner of your job page.</span></span> <span data-ttu-id="3a1c1-171">Klicka på länken för hello build antal och markera **konsolen utdata** på hello vänstra storlek.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-171">Click hello build number link and select **Console output** on hello left-hand size.</span></span> <span data-ttu-id="3a1c1-172">Du kan visa hello steg Jenkins utförs som koden hämtas från GitHub och hello build utdata hello åtgärdsmeddelande `Testing` toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-172">You can view hello steps Jenkins takes as your code is pulled from GitHub and hello build action outputs hello message `Testing` toohello console.</span></span> <span data-ttu-id="3a1c1-173">Varje gång ett genomförande görs i GitHub hello webhook når ut tooJenkins och utlösa en ny version på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-173">Each time a commit is made in GitHub, hello webhook reaches out tooJenkins and trigger a new build in this way.</span></span>


## <a name="define-docker-build-image"></a><span data-ttu-id="3a1c1-174">Definiera Docker build-bild</span><span class="sxs-lookup"><span data-stu-id="3a1c1-174">Define Docker build image</span></span>
<span data-ttu-id="3a1c1-175">toosee hello Node.js-app som körs baserat på ditt GitHub-incheckningar kan bygga en Docker bild toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-175">toosee hello Node.js app running based on your GitHub commits, lets build a Docker image toorun hello app.</span></span> <span data-ttu-id="3a1c1-176">hello-avbildningen skapas från en Dockerfile som definierar hur tooconfigure hello behållare som kör hello-app.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-176">hello image is built from a Dockerfile that defines how tooconfigure hello container that runs hello app.</span></span> 

<span data-ttu-id="3a1c1-177">Hello SSH-anslutning tooyour VM, ändra toohello Jenkins arbetsytan katalogen efter hello jobb som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-177">From hello SSH connection tooyour VM, change toohello Jenkins workspace directory named after hello job you created in a previous step.</span></span> <span data-ttu-id="3a1c1-178">I vårt exempel som kallades *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-178">In our example, that was named *HelloWorld*.</span></span>

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

<span data-ttu-id="3a1c1-179">Skapa en fil med i den här arbetsytan katalogen med `sudo sensible-editor Dockerfile` och klistra in hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-179">Create a file with in this workspace directory with `sudo sensible-editor Dockerfile` and paste hello following contents.</span></span> <span data-ttu-id="3a1c1-180">Kontrollera att hello hela Dockerfile har kopierats på rätt sätt, särskilt hello första rad:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-180">Make sure that hello whole Dockerfile is copied correctly, especially hello first line:</span></span>

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

<span data-ttu-id="3a1c1-181">Den här Dockerfile använder hello Node.js basavbildningen med Alpine Linux, visar port 1337 som hello World Hello app körs på, och sedan kopierar hello-programfilerna och initierar den.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-181">This Dockerfile uses hello base Node.js image using Alpine Linux, exposes port 1337 that hello Hello World app runs on, then copies hello app files and initializes it.</span></span>


## <a name="create-jenkins-build-rules"></a><span data-ttu-id="3a1c1-182">Skapa Jenkins build-regler</span><span class="sxs-lookup"><span data-stu-id="3a1c1-182">Create Jenkins build rules</span></span>
<span data-ttu-id="3a1c1-183">I föregående steg skapa Jenkins build regel som resulterar i ett meddelande toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-183">In a previous step, you created a basic Jenkins build rule that output a message toohello console.</span></span> <span data-ttu-id="3a1c1-184">Kan skapa hello build steg toouse våra Dockerfile och köra hello app.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-184">Lets create hello build step toouse our Dockerfile and run hello app.</span></span>

<span data-ttu-id="3a1c1-185">Välj hello jobb som du skapade i föregående steg i din Jenkins-instans.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-185">Back in your Jenkins instance, select hello job you created in a previous step.</span></span> <span data-ttu-id="3a1c1-186">Klicka på **konfigurera** på vänster sida av hello och rulla ned toohello **skapa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-186">Click **Configure** on hello left-hand side and scroll down toohello **Build** section:</span></span>

- <span data-ttu-id="3a1c1-187">Ta bort den befintliga `echo "Test"` skapa steg.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-187">Remove your existing `echo "Test"` build step.</span></span> <span data-ttu-id="3a1c1-188">Klicka på hello röd mellan hello övre högra hörnet på hello befintlig build steg ruta.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-188">Click hello red cross on hello top right-hand corner of hello existing build step box.</span></span>
- <span data-ttu-id="3a1c1-189">Klicka på **Lägg till build steg**och välj **köra shell**</span><span class="sxs-lookup"><span data-stu-id="3a1c1-189">Click **Add build step**, then select **Execute shell**</span></span>
- <span data-ttu-id="3a1c1-190">I hello **kommandot** , ange hello följande Docker-kommandon och sedan markera **spara**:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-190">In hello **Command** box, enter hello following Docker commands, then select **Save**:</span></span>

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

<span data-ttu-id="3a1c1-191">Hej Docker build steg skapa en avbildning och tagg med hello Jenkins build-nummer så att du kan hantera en historik över bilder.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-191">hello Docker build steps create an image and tag it with hello Jenkins build number so you can maintain a history of images.</span></span> <span data-ttu-id="3a1c1-192">Alla befintliga behållare kör hello app stoppas och sedan har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-192">Any existing containers running hello app are stopped and then removed.</span></span> <span data-ttu-id="3a1c1-193">En ny behållare är igång med hello avbildningen och kör Node.js-appen baserat på hello senaste incheckningar i GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-193">A new container is then started using hello image and runs your Node.js app based on hello latest commits in GitHub.</span></span>


## <a name="test-your-pipeline"></a><span data-ttu-id="3a1c1-194">Testa din pipeline</span><span class="sxs-lookup"><span data-stu-id="3a1c1-194">Test your pipeline</span></span>
<span data-ttu-id="3a1c1-195">toosee hello hela pipeline i åtgärden, redigera hello *index.js* filen i din GitHub-repo-andelen vridvuxna igen och klicka på **spara ändringen**.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-195">toosee hello whole pipeline in action, edit hello *index.js* file in your forked GitHub repo again and click **Commit change**.</span></span> <span data-ttu-id="3a1c1-196">Ett nytt jobb startar i Jenkins baserat på hello webhook för GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-196">A new job starts in Jenkins based on hello webhook for GitHub.</span></span> <span data-ttu-id="3a1c1-197">Det tar några sekunder toocreate hello Docker avbildningen och starta appen i en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-197">It takes a few seconds toocreate hello Docker image and start your app in a new container.</span></span>

<span data-ttu-id="3a1c1-198">Om det behövs kan du hämta hello offentliga IP-adressen för den virtuella datorn igen:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-198">If needed, obtain hello public IP address of your VM again:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="3a1c1-199">Öppna en webbläsare och ange `http://<publicIps>:1337`.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-199">Open a web browser and enter `http://<publicIps>:1337`.</span></span> <span data-ttu-id="3a1c1-200">Node.js-appen visas och visar hello senaste incheckningar i din GitHub-förgrening på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-200">Your Node.js app is displayed and reflects hello latest commits in your GitHub fork as follows:</span></span>

![Node.js-app som körs](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

<span data-ttu-id="3a1c1-202">Skapa en annan redigera toohello *index.js* filen i GitHub och spara hello ändra.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-202">Now make another edit toohello *index.js* file in GitHub and commit hello change.</span></span> <span data-ttu-id="3a1c1-203">Vänta några sekunder för hello jobbet toocomplete i Jenkins och uppdatera sedan din webbläsare toosee hello uppdateras webbversionen av din app som körs i en ny behållare på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-203">Wait a few seconds for hello job toocomplete in Jenkins, then refresh your web browser toosee hello updated version of your app running in a new container as follows:</span></span>

![Node.js-app som körs efter en annan GitHub genomförande](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a><span data-ttu-id="3a1c1-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a1c1-205">Next steps</span></span>
<span data-ttu-id="3a1c1-206">I den här självstudiekursen konfigurerats GitHub toorun ett Jenkins build-jobb på varje kod genomförande och distribuera en Docker-behållare tootest din app.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-206">In this tutorial, you configured GitHub toorun a Jenkins build job on each code commit and then deploy a Docker container tootest your app.</span></span> <span data-ttu-id="3a1c1-207">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="3a1c1-207">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3a1c1-208">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-208">Create a Jenkins VM</span></span>
> * <span data-ttu-id="3a1c1-209">Installera och konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-209">Install and configure Jenkins</span></span>
> * <span data-ttu-id="3a1c1-210">Skapa webhook integrering mellan GitHub och Jenkins</span><span class="sxs-lookup"><span data-stu-id="3a1c1-210">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="3a1c1-211">Skapa och genomför utlösaren Jenkins skapa jobb från GitHub</span><span class="sxs-lookup"><span data-stu-id="3a1c1-211">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="3a1c1-212">Skapa en Docker-avbildning för din app</span><span class="sxs-lookup"><span data-stu-id="3a1c1-212">Create a Docker image for your app</span></span>
> * <span data-ttu-id="3a1c1-213">Kontrollera GitHub incheckningar Skapa ny Docker-avbildning och uppdateringar som kör appen</span><span class="sxs-lookup"><span data-stu-id="3a1c1-213">Verify GitHub commits build new Docker image and updates running app</span></span>

<span data-ttu-id="3a1c1-214">I förväg toohello nästa självstudiekurs toolearn mer om hur toointegrate Jenkins med Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="3a1c1-214">Advance toohello next tutorial toolearn more about how toointegrate Jenkins with Visual Studio Team Services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3a1c1-215">Distribuera appar med Jenkins och Team Services</span><span class="sxs-lookup"><span data-stu-id="3a1c1-215">Deploy apps with Jenkins and Team Services</span></span>](tutorial-build-deploy-jenkins.md)