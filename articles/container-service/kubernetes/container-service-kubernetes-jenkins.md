---
title: aaaJenkins CI/CD med Kubernetes i Azure Container Service | Microsoft Docs
description: "Hur tooautomate CI/CD bearbeta med Jenkins toodeploy och uppgradera en av app på Kubernetes i Azure Container Service"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Docker-behållare, Kubernetes, Azure, Jenkins"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: e00e13bf06619bed73e82878777e55458ea3dd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a><span data-ttu-id="fbb77-104">Jenkins integrering med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fbb77-104">Jenkins integration with Azure Container Service and Kubernetes</span></span> 
<span data-ttu-id="fbb77-105">I den här självstudiekursen kommer vi att gå igenom hello processen tooset in kontinuerlig integration av ett program för flera behållare i Azure Container Service Kubernetes med hello Jenkins platform.</span><span class="sxs-lookup"><span data-stu-id="fbb77-105">In this tutorial, we walk through hello process tooset up continuous integration of a multi-container application into Azure Container Service Kubernetes using hello Jenkins platform.</span></span> <span data-ttu-id="fbb77-106">hello arbetsflödet hello behållaren bilden i Docker hubb uppdateras och uppgraderingar hello Kubernetes skida med hjälp av en distribution för distribution.</span><span class="sxs-lookup"><span data-stu-id="fbb77-106">hello workflow updates hello container image in Docker Hub and upgrades hello Kubernetes pods using a deployment rollout.</span></span> 

## <a name="high-level-process"></a><span data-ttu-id="fbb77-107">Hög nivå process</span><span class="sxs-lookup"><span data-stu-id="fbb77-107">High level process</span></span>
<span data-ttu-id="fbb77-108">hello grundläggande steg som beskrivs i den här artikeln är:</span><span class="sxs-lookup"><span data-stu-id="fbb77-108">hello basic steps detailed in this article are:</span></span> 
- <span data-ttu-id="fbb77-109">Installera ett Kubernetes kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="fbb77-109">Install a Kubernetes cluster in Container Service</span></span>
- <span data-ttu-id="fbb77-110">Konfigurera Jenkins och konfigurera åtkomst tooContainer Service</span><span class="sxs-lookup"><span data-stu-id="fbb77-110">Set up Jenkins and configure access tooContainer Service</span></span>
- <span data-ttu-id="fbb77-111">Skapa ett arbetsflöde för Jenkins</span><span class="sxs-lookup"><span data-stu-id="fbb77-111">Create a Jenkins workflow</span></span>
- <span data-ttu-id="fbb77-112">Testa hello CI/CD-processen slutet tooend</span><span class="sxs-lookup"><span data-stu-id="fbb77-112">Test hello CI/CD process end tooend</span></span>

## <a name="install-a-kubernetes-cluster"></a><span data-ttu-id="fbb77-113">Installera ett Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="fbb77-113">Install a Kubernetes cluster</span></span>
    
<span data-ttu-id="fbb77-114">Distribuera hello Kubernetes kluster i Azure Container Service med hjälp av hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="fbb77-114">Deploy hello Kubernetes cluster in Azure Container Service using hello following steps.</span></span> <span data-ttu-id="fbb77-115">Fullständig dokumentation finns [här](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="fbb77-115">Full documentation is located [here](container-service-kubernetes-walkthrough.md).</span></span>

### <a name="step-1-create-a-resource-group"></a><span data-ttu-id="fbb77-116">Steg 1: Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fbb77-116">Step 1: Create a resource group</span></span>
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a><span data-ttu-id="fbb77-117">Steg 2: Distribuera hello-kluster</span><span class="sxs-lookup"><span data-stu-id="fbb77-117">Step 2: Deploy hello cluster</span></span>
> [!NOTE]
> <span data-ttu-id="fbb77-118">hello kräver följande steg en lokal offentlig SSH-nyckel som lagras i hello ~/.ssh mapp.</span><span class="sxs-lookup"><span data-stu-id="fbb77-118">hello following steps require a local SSH public key stored in hello ~/.ssh folder.</span></span>
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a><span data-ttu-id="fbb77-119">Konfigurera Jenkins och konfigurera åtkomst tooContainer Service</span><span class="sxs-lookup"><span data-stu-id="fbb77-119">Set up Jenkins and configure access tooContainer Service</span></span>

### <a name="step-1-install-jenkins"></a><span data-ttu-id="fbb77-120">Steg 1: Installera Jenkins</span><span class="sxs-lookup"><span data-stu-id="fbb77-120">Step 1: Install Jenkins</span></span>
1. <span data-ttu-id="fbb77-121">Skapa en virtuell dator i Azure med Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="fbb77-121">Create an Azure VM with Ubuntu 16.04 LTS.</span></span>  <span data-ttu-id="fbb77-122">Eftersom senare i hello steg du ska måste tooconnect toothis VM med bash på din lokala dator set hello Authentication type too'SSH offentlig nyckel- och klistra in hello offentliga nyckel som lagras lokalt i mappen ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="fbb77-122">Since later in hello steps you will need tooconnect toothis VM using bash on your local machine, set hello 'Authentication type' too'SSH public key' and paste hello SSH public key that is stored locally in your ~/.ssh folder.</span></span>  <span data-ttu-id="fbb77-123">Dessutom anteckna hello ”användarnamn”, som du anger sedan det här användarnamnet kommer att nödvändiga tooview hello Jenkins instrumentpanelen och för att ansluta toohello Jenkins VM i senare steg.</span><span class="sxs-lookup"><span data-stu-id="fbb77-123">Also, take note of hello 'User name' that you specify since this user name will be needed tooview hello Jenkins dashboard and for connecting toohello Jenkins VM in later steps.</span></span>
2. <span data-ttu-id="fbb77-124">Installera Jenkins via dessa [instruktioner](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="fbb77-124">Install Jenkins via these [instructions](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span></span> <span data-ttu-id="fbb77-125">En mer detaljerad genomgång finns på [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span><span class="sxs-lookup"><span data-stu-id="fbb77-125">A more detailed tutorial is at [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span></span>
3. <span data-ttu-id="fbb77-126">tooview Hej Jenkins instrumentpanelen på den lokala datorn, uppdatera hello Azure network security group tooallow port 8080 genom att lägga till en regel för inkommande trafik som tillåter åtkomst tooport 8080.</span><span class="sxs-lookup"><span data-stu-id="fbb77-126">tooview hello Jenkins dashboard on your local machine, update hello Azure network security group tooallow port 8080 by adding an inbound rule that allows access tooport 8080.</span></span>  <span data-ttu-id="fbb77-127">Du kan också installera vidarebefordrade portar genom att köra det här kommandot:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span><span class="sxs-lookup"><span data-stu-id="fbb77-127">Alternatively, you may setup port forwarding by running this command: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span></span>
4. <span data-ttu-id="fbb77-128">Anslut tooyour Jenkins server med hello webbläsare genom att gå toohello offentlig IP-adress (http:// < your_jenkins_public_ip >: 8080) och låsa upp hello Jenkins instrumentpanel för hello första gången med hello inledande administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="fbb77-128">Connect tooyour Jenkins server using hello browser by navigating toohello public IP (http://<your_jenkins_public_ip>:8080) and unlock hello Jenkins dashboard for hello first time with hello initial admin password.</span></span>  <span data-ttu-id="fbb77-129">hello adminlösenord lagras på /var/lib/jenkins/secrets/initialAdminPassword på hello Jenkins VM.</span><span class="sxs-lookup"><span data-stu-id="fbb77-129">hello admin password is stored at /var/lib/jenkins/secrets/initialAdminPassword on hello Jenkins VM.</span></span>  <span data-ttu-id="fbb77-130">Ett enkelt sätt tooget lösenordet är tooSSH till hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="fbb77-130">An easy way tooget this password is tooSSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="fbb77-131">Kör därefter: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span><span class="sxs-lookup"><span data-stu-id="fbb77-131">Next, run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span></span>
5. <span data-ttu-id="fbb77-132">Installera Docker på hello Jenkins datorn via dessa [instruktioner](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span><span class="sxs-lookup"><span data-stu-id="fbb77-132">Install Docker on hello Jenkins machine via these [instructions](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span></span> <span data-ttu-id="fbb77-133">Detta ger Docker kommandon toobe kör Jenkins jobb.</span><span class="sxs-lookup"><span data-stu-id="fbb77-133">This allows for Docker commands toobe run in Jenkins jobs.</span></span>
6. <span data-ttu-id="fbb77-134">Konfigurera Docker behörigheter tooallow Jenkins tooaccess hello Docker slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="fbb77-134">Configure Docker permissions tooallow Jenkins tooaccess hello Docker endpoint.</span></span>

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. <span data-ttu-id="fbb77-135">Installera `kubectl` CLI på Jenkins.</span><span class="sxs-lookup"><span data-stu-id="fbb77-135">Install `kubectl` CLI on Jenkins.</span></span> <span data-ttu-id="fbb77-136">Mer information finns på [installation och konfigurering av kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="fbb77-136">More details are at [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>  <span data-ttu-id="fbb77-137">Jenkins jobb använder 'kubectl' toomanage och distribuera toohello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="fbb77-137">Jenkins jobs will use 'kubectl' toomanage and deploy toohello Kubernetes cluster.</span></span>

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a><span data-ttu-id="fbb77-138">Steg 2: Konfigurera åtkomst toohello Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="fbb77-138">Step 2: Set up access toohello Kubernetes cluster</span></span>

> [!NOTE]
> <span data-ttu-id="fbb77-139">Det finns flera metoder tooaccomplishing hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="fbb77-139">There are multiple approaches tooaccomplishing hello following steps.</span></span> <span data-ttu-id="fbb77-140">Använd hello-metod som är enklast för dig.</span><span class="sxs-lookup"><span data-stu-id="fbb77-140">Use hello approach that is easiest for you.</span></span>
>

1. <span data-ttu-id="fbb77-141">Kopiera hello `kubectl` config-fil toohello Jenkins datorn så att Jenkins jobb har åtkomst toohello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="fbb77-141">Copy hello `kubectl` config file toohello Jenkins machine so that Jenkins jobs have access toohello Kubernetes cluster.</span></span> <span data-ttu-id="fbb77-142">Dessa instruktioner förutsätter att du använder bash från en annan dator än hello Jenkins VM och att en lokal offentlig SSH-nyckel lagras i hello datorns ~/.ssh mapp.</span><span class="sxs-lookup"><span data-stu-id="fbb77-142">These instructions assume that you are using bash from a different machine than hello Jenkins VM and that a local SSH public key is stored in hello machine's ~/.ssh folder.</span></span>

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. <span data-ttu-id="fbb77-143">Verifiera att hello Kubernetes från Jenkins klustret är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="fbb77-143">Validate from Jenkins that hello Kubernetes cluster is accessible.</span></span>  <span data-ttu-id="fbb77-144">toodo detta, SSH till hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="fbb77-144">toodo this, SSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="fbb77-145">Kontrollera Jenkins kan ansluta tooyour kluster: `kubectl cluster-info`.</span><span class="sxs-lookup"><span data-stu-id="fbb77-145">Next, verify Jenkins can successfully connect tooyour cluster: `kubectl cluster-info`.</span></span>
    

## <a name="create-a-jenkins-workflow"></a><span data-ttu-id="fbb77-146">Skapa ett arbetsflöde för Jenkins</span><span class="sxs-lookup"><span data-stu-id="fbb77-146">Create a Jenkins workflow</span></span>

### <a name="prerequisites"></a><span data-ttu-id="fbb77-147">Krav</span><span class="sxs-lookup"><span data-stu-id="fbb77-147">Prerequisites</span></span>

- <span data-ttu-id="fbb77-148">GitHub-konto för koden lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fbb77-148">GitHub account for code repo.</span></span>
- <span data-ttu-id="fbb77-149">Docker-hubb konto toostore och uppdatera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="fbb77-149">Docker Hub account toostore and update images.</span></span>
- <span data-ttu-id="fbb77-150">Av program som kan byggas och uppdateras.</span><span class="sxs-lookup"><span data-stu-id="fbb77-150">Containerized application that can be rebuilt and updated.</span></span> <span data-ttu-id="fbb77-151">Du kan använda den här exempelappen behållare som skrivits i Golang: https://github.com/chzbrgr71/go-web</span><span class="sxs-lookup"><span data-stu-id="fbb77-151">You can use this sample container app written in Golang: https://github.com/chzbrgr71/go-web</span></span> 

> [!NOTE]
> <span data-ttu-id="fbb77-152">hello måste följande steg utföras i ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="fbb77-152">hello following steps must be performed in your own GitHub account.</span></span> <span data-ttu-id="fbb77-153">Känna sig fria tooclone hello ovan lagringsplatsen, men du måste använda ditt eget konto tooconfigure hello webhooks och Jenkins komma åt.</span><span class="sxs-lookup"><span data-stu-id="fbb77-153">Feel free tooclone hello above repo, but you must use your own account tooconfigure hello webhooks and Jenkins access.</span></span>
>

### <a name="step-1-deploy-initial-v1-of-application"></a><span data-ttu-id="fbb77-154">Steg 1: Distribuera inledande v1 för program</span><span class="sxs-lookup"><span data-stu-id="fbb77-154">Step 1: Deploy initial v1 of application</span></span>
1. <span data-ttu-id="fbb77-155">Skapa hello appen från hello developer datorn med hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="fbb77-155">Build hello app from hello developer machine with hello following commands.</span></span> <span data-ttu-id="fbb77-156">Ersätt `myrepo` med dina egna.</span><span class="sxs-lookup"><span data-stu-id="fbb77-156">Replace `myrepo` with your own.</span></span>
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. <span data-ttu-id="fbb77-157">Push-avbildningen tooDocker hubb.</span><span class="sxs-lookup"><span data-stu-id="fbb77-157">Push image tooDocker Hub.</span></span>

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. <span data-ttu-id="fbb77-158">Distribuera toohello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="fbb77-158">Deploy toohello Kubernetes cluster.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="fbb77-159">Redigera hello `go-web.yaml` filen tooupdate din behållaren avbildningen och lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fbb77-159">Edit hello `go-web.yaml` file tooupdate your container image and repo.</span></span>
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a><span data-ttu-id="fbb77-160">Steg 2: Konfigurera Jenkins system</span><span class="sxs-lookup"><span data-stu-id="fbb77-160">Step 2: Configure Jenkins system</span></span>
1. <span data-ttu-id="fbb77-161">Klicka på **hantera Jenkins** > **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-161">Click **Manage Jenkins** > **Configure System**.</span></span>
2. <span data-ttu-id="fbb77-162">Under **GitHub**väljer **Lägg till GitHub-Server**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-162">Under **GitHub**, select **Add GitHub Server**.</span></span>
3. <span data-ttu-id="fbb77-163">Lämna **API-URL** som standard.</span><span class="sxs-lookup"><span data-stu-id="fbb77-163">Leave **API URL** as default.</span></span>
4. <span data-ttu-id="fbb77-164">Under **autentiseringsuppgifter**, lägga till en Jenkins autentiseringsuppgifter med hjälp av **hemliga text**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-164">Under **Credentials**, add a Jenkins credential using **Secret text**.</span></span> <span data-ttu-id="fbb77-165">Vi rekommenderar att du använder GitHub personlig åtkomst-token har konfigurerats i din GitHub inställningar.</span><span class="sxs-lookup"><span data-stu-id="fbb77-165">We recommend using GitHub personal access tokens, which are configured in your GitHub user account settings.</span></span> <span data-ttu-id="fbb77-166">Mer information om detta [här.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span><span class="sxs-lookup"><span data-stu-id="fbb77-166">More details on this [here.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span></span>
5. <span data-ttu-id="fbb77-167">Klicka på **Testanslutningen** tooensure konfigureras korrekt.</span><span class="sxs-lookup"><span data-stu-id="fbb77-167">Click **Test connection** tooensure this is configured correctly.</span></span>
6. <span data-ttu-id="fbb77-168">Under **globala egenskaper**, lägger du till ett miljövariabel `DOCKER_HUB` och ange lösenordet Docker-hubb.</span><span class="sxs-lookup"><span data-stu-id="fbb77-168">Under **Global Properties**, add an environment variable `DOCKER_HUB` and provide your Docker Hub password.</span></span> <span data-ttu-id="fbb77-169">(Detta är användbart i den här demon, men en form av produktionsscenario kräver ett säkrare sätt.)</span><span class="sxs-lookup"><span data-stu-id="fbb77-169">(This is useful in this demo, but a production scenario would require a more secure approach.)</span></span>
7. <span data-ttu-id="fbb77-170">Spara.</span><span class="sxs-lookup"><span data-stu-id="fbb77-170">Save.</span></span>

![Jenkins GitHub-åtkomst](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a><span data-ttu-id="fbb77-172">Steg 3: Skapa hello Jenkins arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="fbb77-172">Step 3: Create hello Jenkins workflow</span></span>
1. <span data-ttu-id="fbb77-173">Skapa ett Jenkins-objekt.</span><span class="sxs-lookup"><span data-stu-id="fbb77-173">Create a Jenkins item.</span></span>
2. <span data-ttu-id="fbb77-174">Ange ett namn (till exempel ”gå web”) och välj **Freestyle projekt**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-174">Provide a name (for example, "go-web") and select **Freestyle Project**.</span></span> 
3. <span data-ttu-id="fbb77-175">Kontrollera **GitHub projekt** och ange hello URL tooyour GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fbb77-175">Check **GitHub project** and provide hello URL tooyour GitHub repo.</span></span>
4. <span data-ttu-id="fbb77-176">I **källa kod Management**, ange hello GitHub-repo-URL och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fbb77-176">In **Source Code Management**, provide hello GitHub repo URL and credentials.</span></span> 
5. <span data-ttu-id="fbb77-177">Lägg till en **skapa steg** av typen **köra shell** och Använd hello följande text:</span><span class="sxs-lookup"><span data-stu-id="fbb77-177">Add a **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. <span data-ttu-id="fbb77-178">Lägga till en annan **skapa steg** av typen **köra shell** och Använd hello följande text:</span><span class="sxs-lookup"><span data-stu-id="fbb77-178">Add another **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins skapa steg](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. <span data-ttu-id="fbb77-180">Spara hello Jenkins objekt och testa med **skapa nu**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-180">Save hello Jenkins item and test with **Build Now**.</span></span>

### <a name="step-4-connect-github-webhook"></a><span data-ttu-id="fbb77-181">Steg 4: Anslut GitHub-webhook</span><span class="sxs-lookup"><span data-stu-id="fbb77-181">Step 4: Connect GitHub webhook</span></span>
1. <span data-ttu-id="fbb77-182">Hej Jenkins objekt som du har skapat, klicka på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-182">In hello Jenkins item you created, click **Configure**.</span></span>
2. <span data-ttu-id="fbb77-183">Under **Skapa utlösare**väljer **GitHub hook utlösare för GITScm avsökning** och **spara**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-183">Under **Build Triggers**, select **GitHub hook trigger for GITScm polling** and **Save**.</span></span> <span data-ttu-id="fbb77-184">Hej GitHub-webhook konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fbb77-184">This automatically configures hello GitHub webhook.</span></span>
3. <span data-ttu-id="fbb77-185">Klicka på ditt GitHub-lagringsplatsen för gå webb **Inställningar > Webhooks**.</span><span class="sxs-lookup"><span data-stu-id="fbb77-185">In your GitHub repo for go-web, click **Settings > Webhooks**.</span></span>
4. <span data-ttu-id="fbb77-186">Kontrollera att hello Jenkins webhook URL har lagts till.</span><span class="sxs-lookup"><span data-stu-id="fbb77-186">Verify that hello Jenkins webhook URL was added successfully.</span></span> <span data-ttu-id="fbb77-187">hello URL måste sluta med ”github-webhook”.</span><span class="sxs-lookup"><span data-stu-id="fbb77-187">hello URL should end in "github-webhook".</span></span>

![Jenkins webhook-konfiguration](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a><span data-ttu-id="fbb77-189">Testa hello CI/CD-processen slutet tooend</span><span class="sxs-lookup"><span data-stu-id="fbb77-189">Test hello CI/CD process end tooend</span></span>

1. <span data-ttu-id="fbb77-190">Uppdatera koden för hello lagringsplatsen och push-synk med hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fbb77-190">Update code for hello repo and push/synch with hello GitHub repository.</span></span>
2. <span data-ttu-id="fbb77-191">Kontrollera hello hello Jenkins konsolen **skapa historik** och verifiera att hello jobbet kördes.</span><span class="sxs-lookup"><span data-stu-id="fbb77-191">From hello Jenkins console, check hello **Build History** and validate that hello job has run.</span></span> <span data-ttu-id="fbb77-192">Visa konsolen utdata toosee information.</span><span class="sxs-lookup"><span data-stu-id="fbb77-192">View console output toosee details.</span></span>
3. <span data-ttu-id="fbb77-193">Visa information om hello uppgraderas från Kubernetes, distribution:</span><span class="sxs-lookup"><span data-stu-id="fbb77-193">From Kubernetes, view details of hello upgraded deployment:</span></span>

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a><span data-ttu-id="fbb77-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fbb77-194">Next steps</span></span>

- <span data-ttu-id="fbb77-195">Distribuera Azure-behållare registret och lagra avbildningar i en säker databas.</span><span class="sxs-lookup"><span data-stu-id="fbb77-195">Deploy Azure Container Registry and store images in a secure repository.</span></span> <span data-ttu-id="fbb77-196">Se [Azure Container registret docs](https://docs.microsoft.com/azure/container-registry).</span><span class="sxs-lookup"><span data-stu-id="fbb77-196">See [Azure Container Registry docs](https://docs.microsoft.com/azure/container-registry).</span></span>
- <span data-ttu-id="fbb77-197">Skapa en mer komplex arbetsflöde som innehåller sida-vid-sida-distribution och testerna i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="fbb77-197">Build a more complex workflow that includes side-by-side deployment and automated tests in Jenkins.</span></span>
- <span data-ttu-id="fbb77-198">Mer information om CI/CD-skiva med Jenkins och Kubernetes finns hello [Jenkins blogg](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span><span class="sxs-lookup"><span data-stu-id="fbb77-198">For more information about CI/CD with Jenkins and Kubernetes, see hello [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span></span>
