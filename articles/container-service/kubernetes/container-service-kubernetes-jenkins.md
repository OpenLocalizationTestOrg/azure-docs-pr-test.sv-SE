---
title: Jenkins CI/CD med Kubernetes i Azure Container Service | Microsoft Docs
description: "Automatisera en CI/CD-process med Jenkins att distribuera och uppgradera en av app på Kubernetes i Azure Container Service"
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
ms.openlocfilehash: 2078d0694fc4dd6e83ecd2792588b4254980cd78
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a><span data-ttu-id="3bf03-104">Jenkins integrering med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="3bf03-104">Jenkins integration with Azure Container Service and Kubernetes</span></span> 
<span data-ttu-id="3bf03-105">I den här självstudiekursen kommer vi att gå igenom processen för att ställa in kontinuerlig integration av ett program för flera behållare i Azure Container Service Kubernetes med Jenkins platform.</span><span class="sxs-lookup"><span data-stu-id="3bf03-105">In this tutorial, we walk through the process to set up continuous integration of a multi-container application into Azure Container Service Kubernetes using the Jenkins platform.</span></span> <span data-ttu-id="3bf03-106">Arbetsflödet uppdateringar bilden behållare i Docker-hubb och uppgraderingar Kubernetes-skida med hjälp av en distribution för distribution.</span><span class="sxs-lookup"><span data-stu-id="3bf03-106">The workflow updates the container image in Docker Hub and upgrades the Kubernetes pods using a deployment rollout.</span></span> 

## <a name="high-level-process"></a><span data-ttu-id="3bf03-107">Hög nivå process</span><span class="sxs-lookup"><span data-stu-id="3bf03-107">High level process</span></span>
<span data-ttu-id="3bf03-108">De grundläggande stegen som beskrivs i den här artikeln är:</span><span class="sxs-lookup"><span data-stu-id="3bf03-108">The basic steps detailed in this article are:</span></span> 
- <span data-ttu-id="3bf03-109">Installera ett Kubernetes kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="3bf03-109">Install a Kubernetes cluster in Container Service</span></span>
- <span data-ttu-id="3bf03-110">Konfigurera Jenkins och konfigurera åtkomst till Container Service</span><span class="sxs-lookup"><span data-stu-id="3bf03-110">Set up Jenkins and configure access to Container Service</span></span>
- <span data-ttu-id="3bf03-111">Skapa ett arbetsflöde för Jenkins</span><span class="sxs-lookup"><span data-stu-id="3bf03-111">Create a Jenkins workflow</span></span>
- <span data-ttu-id="3bf03-112">Testa processen CI/CD-slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="3bf03-112">Test the CI/CD process end to end</span></span>

## <a name="install-a-kubernetes-cluster"></a><span data-ttu-id="3bf03-113">Installera ett Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="3bf03-113">Install a Kubernetes cluster</span></span>
    
<span data-ttu-id="3bf03-114">Distribuera Kubernetes kluster i Azure Container Service med följande steg.</span><span class="sxs-lookup"><span data-stu-id="3bf03-114">Deploy the Kubernetes cluster in Azure Container Service using the following steps.</span></span> <span data-ttu-id="3bf03-115">Fullständig dokumentation finns [här](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3bf03-115">Full documentation is located [here](container-service-kubernetes-walkthrough.md).</span></span>

### <a name="step-1-create-a-resource-group"></a><span data-ttu-id="3bf03-116">Steg 1: Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3bf03-116">Step 1: Create a resource group</span></span>
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a><span data-ttu-id="3bf03-117">Steg 2: Distribuera klustret</span><span class="sxs-lookup"><span data-stu-id="3bf03-117">Step 2: Deploy the cluster</span></span>
> [!NOTE]
> <span data-ttu-id="3bf03-118">Följande steg krävs en lokal offentlig SSH-nyckel som lagras i mappen ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="3bf03-118">The following steps require a local SSH public key stored in the ~/.ssh folder.</span></span>
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

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a><span data-ttu-id="3bf03-119">Konfigurera Jenkins och konfigurera åtkomst till Container Service</span><span class="sxs-lookup"><span data-stu-id="3bf03-119">Set up Jenkins and configure access to Container Service</span></span>

### <a name="step-1-install-jenkins"></a><span data-ttu-id="3bf03-120">Steg 1: Installera Jenkins</span><span class="sxs-lookup"><span data-stu-id="3bf03-120">Step 1: Install Jenkins</span></span>
1. <span data-ttu-id="3bf03-121">Skapa en virtuell dator i Azure med Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="3bf03-121">Create an Azure VM with Ubuntu 16.04 LTS.</span></span>  <span data-ttu-id="3bf03-122">Eftersom senare i steg behöver du ansluta till den här virtuella datorn med hjälp av bash på den lokala datorn, ange ”autentiseringstyp” till SSH offentlig nyckel och klistra in den offentliga SSH-nyckeln som lagras lokalt i mappen ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="3bf03-122">Since later in the steps you will need to connect to this VM using bash on your local machine, set the 'Authentication type' to 'SSH public key' and paste the SSH public key that is stored locally in your ~/.ssh folder.</span></span>  <span data-ttu-id="3bf03-123">Dessutom anteckna 'användarnamn' som du anger sedan användarnamnet som behövs för att visa instrumentpanelen för Jenkins och för att ansluta till Jenkins VM i senare steg.</span><span class="sxs-lookup"><span data-stu-id="3bf03-123">Also, take note of the 'User name' that you specify since this user name will be needed to view the Jenkins dashboard and for connecting to the Jenkins VM in later steps.</span></span>
2. <span data-ttu-id="3bf03-124">Installera Jenkins via dessa [instruktioner](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="3bf03-124">Install Jenkins via these [instructions](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span></span> <span data-ttu-id="3bf03-125">En mer detaljerad genomgång finns på [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span><span class="sxs-lookup"><span data-stu-id="3bf03-125">A more detailed tutorial is at [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span></span>
3. <span data-ttu-id="3bf03-126">Om du vill visa infopanelen Jenkins på den lokala datorn, uppdatera säkerhetsgruppen Azure-nätverk för att tillåta port 8080 genom att lägga till en regel för inkommande trafik som tillåter åtkomst till port 8080.</span><span class="sxs-lookup"><span data-stu-id="3bf03-126">To view the Jenkins dashboard on your local machine, update the Azure network security group to allow port 8080 by adding an inbound rule that allows access to port 8080.</span></span>  <span data-ttu-id="3bf03-127">Du kan också installera vidarebefordrade portar genom att köra det här kommandot:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span><span class="sxs-lookup"><span data-stu-id="3bf03-127">Alternatively, you may setup port forwarding by running this command: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span></span>
4. <span data-ttu-id="3bf03-128">Ansluta till servern Jenkins i webbläsaren genom att navigera till den offentliga IP-Adressen (http:// < your_jenkins_public_ip >: 8080) och låsa upp Jenkins instrumentpanelen för första gången med inledande administratörslösenordet.</span><span class="sxs-lookup"><span data-stu-id="3bf03-128">Connect to your Jenkins server using the browser by navigating to the public IP (http://<your_jenkins_public_ip>:8080) and unlock the Jenkins dashboard for the first time with the initial admin password.</span></span>  <span data-ttu-id="3bf03-129">Administratörslösenordet lagras på /var/lib/jenkins/secrets/initialAdminPassword på Jenkins VM.</span><span class="sxs-lookup"><span data-stu-id="3bf03-129">The admin password is stored at /var/lib/jenkins/secrets/initialAdminPassword on the Jenkins VM.</span></span>  <span data-ttu-id="3bf03-130">Ett enkelt sätt att hämta lösenordet är att SSH till Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="3bf03-130">An easy way to get this password is to SSH into the Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="3bf03-131">Kör därefter: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span><span class="sxs-lookup"><span data-stu-id="3bf03-131">Next, run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span></span>
5. <span data-ttu-id="3bf03-132">Installera Docker på Jenkins dator via dessa [instruktioner](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span><span class="sxs-lookup"><span data-stu-id="3bf03-132">Install Docker on the Jenkins machine via these [instructions](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span></span> <span data-ttu-id="3bf03-133">Detta ger Docker-kommandon för att köras i Jenkins jobb.</span><span class="sxs-lookup"><span data-stu-id="3bf03-133">This allows for Docker commands to be run in Jenkins jobs.</span></span>
6. <span data-ttu-id="3bf03-134">Konfigurera Docker-behörigheter för att tillåta Jenkins till Docker-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3bf03-134">Configure Docker permissions to allow Jenkins to access the Docker endpoint.</span></span>

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. <span data-ttu-id="3bf03-135">Installera `kubectl` CLI på Jenkins.</span><span class="sxs-lookup"><span data-stu-id="3bf03-135">Install `kubectl` CLI on Jenkins.</span></span> <span data-ttu-id="3bf03-136">Mer information finns på [installation och konfigurering av kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="3bf03-136">More details are at [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>  <span data-ttu-id="3bf03-137">Jenkins jobb använder 'kubectl' för att hantera och distribuera till Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="3bf03-137">Jenkins jobs will use 'kubectl' to manage and deploy to the Kubernetes cluster.</span></span>

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a><span data-ttu-id="3bf03-138">Steg 2: Konfigurera åtkomst till klustret Kubernetes</span><span class="sxs-lookup"><span data-stu-id="3bf03-138">Step 2: Set up access to the Kubernetes cluster</span></span>

> [!NOTE]
> <span data-ttu-id="3bf03-139">Det finns flera sätt att utföra följande steg.</span><span class="sxs-lookup"><span data-stu-id="3bf03-139">There are multiple approaches to accomplishing the following steps.</span></span> <span data-ttu-id="3bf03-140">Använd den metod som är enklast för dig.</span><span class="sxs-lookup"><span data-stu-id="3bf03-140">Use the approach that is easiest for you.</span></span>
>

1. <span data-ttu-id="3bf03-141">Kopiera den `kubectl` config-filen till den Jenkins datorn så att Jenkins jobb har åtkomst till klustret Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="3bf03-141">Copy the `kubectl` config file to the Jenkins machine so that Jenkins jobs have access to the Kubernetes cluster.</span></span> <span data-ttu-id="3bf03-142">Dessa instruktioner förutsätter att du använder bash från en annan dator än Jenkins VM och en lokal offentlig SSH-nyckel som lagras i datorns ~/.ssh mapp.</span><span class="sxs-lookup"><span data-stu-id="3bf03-142">These instructions assume that you are using bash from a different machine than the Jenkins VM and that a local SSH public key is stored in the machine's ~/.ssh folder.</span></span>

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. <span data-ttu-id="3bf03-143">Validera att klustret Kubernetes är tillgänglig från Jenkins.</span><span class="sxs-lookup"><span data-stu-id="3bf03-143">Validate from Jenkins that the Kubernetes cluster is accessible.</span></span>  <span data-ttu-id="3bf03-144">Gör SSH till Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="3bf03-144">To do this, SSH into the Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="3bf03-145">Kontrollera Jenkins kan ansluta till klustret: `kubectl cluster-info`.</span><span class="sxs-lookup"><span data-stu-id="3bf03-145">Next, verify Jenkins can successfully connect to your cluster: `kubectl cluster-info`.</span></span>
    

## <a name="create-a-jenkins-workflow"></a><span data-ttu-id="3bf03-146">Skapa ett arbetsflöde för Jenkins</span><span class="sxs-lookup"><span data-stu-id="3bf03-146">Create a Jenkins workflow</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3bf03-147">Krav</span><span class="sxs-lookup"><span data-stu-id="3bf03-147">Prerequisites</span></span>

- <span data-ttu-id="3bf03-148">GitHub-konto för koden lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3bf03-148">GitHub account for code repo.</span></span>
- <span data-ttu-id="3bf03-149">Docker-hubb-konto för att lagra och uppdatera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="3bf03-149">Docker Hub account to store and update images.</span></span>
- <span data-ttu-id="3bf03-150">Av program som kan byggas och uppdateras.</span><span class="sxs-lookup"><span data-stu-id="3bf03-150">Containerized application that can be rebuilt and updated.</span></span> <span data-ttu-id="3bf03-151">Du kan använda den här exempelappen behållare som skrivits i Golang: https://github.com/chzbrgr71/go-web</span><span class="sxs-lookup"><span data-stu-id="3bf03-151">You can use this sample container app written in Golang: https://github.com/chzbrgr71/go-web</span></span> 

> [!NOTE]
> <span data-ttu-id="3bf03-152">Följande steg måste utföras i ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="3bf03-152">The following steps must be performed in your own GitHub account.</span></span> <span data-ttu-id="3bf03-153">Passa på att klona lagringsplatsen ovan men du måste använda ditt eget konto för att konfigurera webhooks och Jenkins komma åt.</span><span class="sxs-lookup"><span data-stu-id="3bf03-153">Feel free to clone the above repo, but you must use your own account to configure the webhooks and Jenkins access.</span></span>
>

### <a name="step-1-deploy-initial-v1-of-application"></a><span data-ttu-id="3bf03-154">Steg 1: Distribuera inledande v1 för program</span><span class="sxs-lookup"><span data-stu-id="3bf03-154">Step 1: Deploy initial v1 of application</span></span>
1. <span data-ttu-id="3bf03-155">Bygga appen från developer-dator med följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="3bf03-155">Build the app from the developer machine with the following commands.</span></span> <span data-ttu-id="3bf03-156">Ersätt `myrepo` med dina egna.</span><span class="sxs-lookup"><span data-stu-id="3bf03-156">Replace `myrepo` with your own.</span></span>
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. <span data-ttu-id="3bf03-157">Push-avbildning till Docker-hubben.</span><span class="sxs-lookup"><span data-stu-id="3bf03-157">Push image to Docker Hub.</span></span>

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. <span data-ttu-id="3bf03-158">Distribuera till Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="3bf03-158">Deploy to the Kubernetes cluster.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="3bf03-159">Redigera den `go-web.yaml` fil att uppdatera behållaren avbildningen och lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3bf03-159">Edit the `go-web.yaml` file to update your container image and repo.</span></span>
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a><span data-ttu-id="3bf03-160">Steg 2: Konfigurera Jenkins system</span><span class="sxs-lookup"><span data-stu-id="3bf03-160">Step 2: Configure Jenkins system</span></span>
1. <span data-ttu-id="3bf03-161">Klicka på **hantera Jenkins** > **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-161">Click **Manage Jenkins** > **Configure System**.</span></span>
2. <span data-ttu-id="3bf03-162">Under **GitHub**väljer **Lägg till GitHub-Server**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-162">Under **GitHub**, select **Add GitHub Server**.</span></span>
3. <span data-ttu-id="3bf03-163">Lämna **API-URL** som standard.</span><span class="sxs-lookup"><span data-stu-id="3bf03-163">Leave **API URL** as default.</span></span>
4. <span data-ttu-id="3bf03-164">Under **autentiseringsuppgifter**, lägga till en Jenkins autentiseringsuppgifter med hjälp av **hemliga text**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-164">Under **Credentials**, add a Jenkins credential using **Secret text**.</span></span> <span data-ttu-id="3bf03-165">Vi rekommenderar att du använder GitHub personlig åtkomst-token har konfigurerats i din GitHub inställningar.</span><span class="sxs-lookup"><span data-stu-id="3bf03-165">We recommend using GitHub personal access tokens, which are configured in your GitHub user account settings.</span></span> <span data-ttu-id="3bf03-166">Mer information om detta [här.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span><span class="sxs-lookup"><span data-stu-id="3bf03-166">More details on this [here.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span></span>
5. <span data-ttu-id="3bf03-167">Klicka på **Anslutningstestet** så konfigureras korrekt.</span><span class="sxs-lookup"><span data-stu-id="3bf03-167">Click **Test connection** to ensure this is configured correctly.</span></span>
6. <span data-ttu-id="3bf03-168">Under **globala egenskaper**, lägger du till ett miljövariabel `DOCKER_HUB` och ange lösenordet Docker-hubb.</span><span class="sxs-lookup"><span data-stu-id="3bf03-168">Under **Global Properties**, add an environment variable `DOCKER_HUB` and provide your Docker Hub password.</span></span> <span data-ttu-id="3bf03-169">(Detta är användbart i den här demon, men en form av produktionsscenario kräver ett säkrare sätt.)</span><span class="sxs-lookup"><span data-stu-id="3bf03-169">(This is useful in this demo, but a production scenario would require a more secure approach.)</span></span>
7. <span data-ttu-id="3bf03-170">Spara.</span><span class="sxs-lookup"><span data-stu-id="3bf03-170">Save.</span></span>

![Jenkins GitHub-åtkomst](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a><span data-ttu-id="3bf03-172">Steg 3: Skapa Jenkins-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="3bf03-172">Step 3: Create the Jenkins workflow</span></span>
1. <span data-ttu-id="3bf03-173">Skapa ett Jenkins-objekt.</span><span class="sxs-lookup"><span data-stu-id="3bf03-173">Create a Jenkins item.</span></span>
2. <span data-ttu-id="3bf03-174">Ange ett namn (till exempel ”gå web”) och välj **Freestyle projekt**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-174">Provide a name (for example, "go-web") and select **Freestyle Project**.</span></span> 
3. <span data-ttu-id="3bf03-175">Kontrollera **GitHub projekt** och ange en URL till dina GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3bf03-175">Check **GitHub project** and provide the URL to your GitHub repo.</span></span>
4. <span data-ttu-id="3bf03-176">I **källa kod Management**, ange GitHub-repo-URL och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3bf03-176">In **Source Code Management**, provide the GitHub repo URL and credentials.</span></span> 
5. <span data-ttu-id="3bf03-177">Lägg till en **skapa steg** av typen **köra shell** och Använd följande text:</span><span class="sxs-lookup"><span data-stu-id="3bf03-177">Add a **Build Step** of type **Execute shell** and use the following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. <span data-ttu-id="3bf03-178">Lägga till en annan **skapa steg** av typen **köra shell** och Använd följande text:</span><span class="sxs-lookup"><span data-stu-id="3bf03-178">Add another **Build Step** of type **Execute shell** and use the following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins skapa steg](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. <span data-ttu-id="3bf03-180">Spara objektet Jenkins och testa med **skapa nu**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-180">Save the Jenkins item and test with **Build Now**.</span></span>

### <a name="step-4-connect-github-webhook"></a><span data-ttu-id="3bf03-181">Steg 4: Anslut GitHub-webhook</span><span class="sxs-lookup"><span data-stu-id="3bf03-181">Step 4: Connect GitHub webhook</span></span>
1. <span data-ttu-id="3bf03-182">Klicka på det Jenkins-objekt som du skapade **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-182">In the Jenkins item you created, click **Configure**.</span></span>
2. <span data-ttu-id="3bf03-183">Under **Skapa utlösare**väljer **GitHub hook utlösare för GITScm avsökning** och **spara**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-183">Under **Build Triggers**, select **GitHub hook trigger for GITScm polling** and **Save**.</span></span> <span data-ttu-id="3bf03-184">GitHub-webhook konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3bf03-184">This automatically configures the GitHub webhook.</span></span>
3. <span data-ttu-id="3bf03-185">Klicka på ditt GitHub-lagringsplatsen för gå webb **Inställningar > Webhooks**.</span><span class="sxs-lookup"><span data-stu-id="3bf03-185">In your GitHub repo for go-web, click **Settings > Webhooks**.</span></span>
4. <span data-ttu-id="3bf03-186">Kontrollera att Webhooksadressen Jenkins lades till.</span><span class="sxs-lookup"><span data-stu-id="3bf03-186">Verify that the Jenkins webhook URL was added successfully.</span></span> <span data-ttu-id="3bf03-187">URL: en måste sluta med ”github-webhook”.</span><span class="sxs-lookup"><span data-stu-id="3bf03-187">The URL should end in "github-webhook".</span></span>

![Jenkins webhook-konfiguration](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a><span data-ttu-id="3bf03-189">Testa processen CI/CD-slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="3bf03-189">Test the CI/CD process end to end</span></span>

1. <span data-ttu-id="3bf03-190">Uppdatera koden för lagringsplatsen och push-synk med GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3bf03-190">Update code for the repo and push/synch with the GitHub repository.</span></span>
2. <span data-ttu-id="3bf03-191">Från konsolen Jenkins Kontrollera den **skapa historik** och verifiera att jobbet har körts.</span><span class="sxs-lookup"><span data-stu-id="3bf03-191">From the Jenkins console, check the **Build History** and validate that the job has run.</span></span> <span data-ttu-id="3bf03-192">Visa konsolen utdata visas detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="3bf03-192">View console output to see details.</span></span>
3. <span data-ttu-id="3bf03-193">Kubernetes, se information om den uppgraderade distributionen:</span><span class="sxs-lookup"><span data-stu-id="3bf03-193">From Kubernetes, view details of the upgraded deployment:</span></span>

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a><span data-ttu-id="3bf03-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3bf03-194">Next steps</span></span>

- <span data-ttu-id="3bf03-195">Distribuera Azure-behållare registret och lagra avbildningar i en säker databas.</span><span class="sxs-lookup"><span data-stu-id="3bf03-195">Deploy Azure Container Registry and store images in a secure repository.</span></span> <span data-ttu-id="3bf03-196">Se [Azure Container registret docs](https://docs.microsoft.com/azure/container-registry).</span><span class="sxs-lookup"><span data-stu-id="3bf03-196">See [Azure Container Registry docs](https://docs.microsoft.com/azure/container-registry).</span></span>
- <span data-ttu-id="3bf03-197">Skapa en mer komplex arbetsflöde som innehåller sida-vid-sida-distribution och testerna i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="3bf03-197">Build a more complex workflow that includes side-by-side deployment and automated tests in Jenkins.</span></span>
- <span data-ttu-id="3bf03-198">Mer information om CI/CD-skiva med Jenkins och Kubernetes finns i [Jenkins blogg](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span><span class="sxs-lookup"><span data-stu-id="3bf03-198">For more information about CI/CD with Jenkins and Kubernetes, see the [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span></span>
