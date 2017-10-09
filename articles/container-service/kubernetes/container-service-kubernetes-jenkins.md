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
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Jenkins integrering med Azure Container Service och Kubernetes 
I den här självstudiekursen kommer vi att gå igenom hello processen tooset in kontinuerlig integration av ett program för flera behållare i Azure Container Service Kubernetes med hello Jenkins platform. hello arbetsflödet hello behållaren bilden i Docker hubb uppdateras och uppgraderingar hello Kubernetes skida med hjälp av en distribution för distribution. 

## <a name="high-level-process"></a>Hög nivå process
hello grundläggande steg som beskrivs i den här artikeln är: 
- Installera ett Kubernetes kluster i Container Service
- Konfigurera Jenkins och konfigurera åtkomst tooContainer Service
- Skapa ett arbetsflöde för Jenkins
- Testa hello CI/CD-processen slutet tooend

## <a name="install-a-kubernetes-cluster"></a>Installera ett Kubernetes kluster
    
Distribuera hello Kubernetes kluster i Azure Container Service med hjälp av hello följande steg. Fullständig dokumentation finns [här](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Steg 1: Skapa en resursgrupp
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a>Steg 2: Distribuera hello-kluster
> [!NOTE]
> hello kräver följande steg en lokal offentlig SSH-nyckel som lagras i hello ~/.ssh mapp.
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

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a>Konfigurera Jenkins och konfigurera åtkomst tooContainer Service

### <a name="step-1-install-jenkins"></a>Steg 1: Installera Jenkins
1. Skapa en virtuell dator i Azure med Ubuntu 16.04 LTS.  Eftersom senare i hello steg du ska måste tooconnect toothis VM med bash på din lokala dator set hello Authentication type too'SSH offentlig nyckel- och klistra in hello offentliga nyckel som lagras lokalt i mappen ~/.ssh.  Dessutom anteckna hello ”användarnamn”, som du anger sedan det här användarnamnet kommer att nödvändiga tooview hello Jenkins instrumentpanelen och för att ansluta toohello Jenkins VM i senare steg.
2. Installera Jenkins via dessa [instruktioner](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). En mer detaljerad genomgång finns på [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. tooview Hej Jenkins instrumentpanelen på den lokala datorn, uppdatera hello Azure network security group tooallow port 8080 genom att lägga till en regel för inkommande trafik som tillåter åtkomst tooport 8080.  Du kan också installera vidarebefordrade portar genom att köra det här kommandot:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Anslut tooyour Jenkins server med hello webbläsare genom att gå toohello offentlig IP-adress (http:// < your_jenkins_public_ip >: 8080) och låsa upp hello Jenkins instrumentpanel för hello första gången med hello inledande administratörslösenord.  hello adminlösenord lagras på /var/lib/jenkins/secrets/initialAdminPassword på hello Jenkins VM.  Ett enkelt sätt tooget lösenordet är tooSSH till hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Kör därefter: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Installera Docker på hello Jenkins datorn via dessa [instruktioner](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). Detta ger Docker kommandon toobe kör Jenkins jobb.
6. Konfigurera Docker behörigheter tooallow Jenkins tooaccess hello Docker slutpunkten.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Installera `kubectl` CLI på Jenkins. Mer information finns på [installation och konfigurering av kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Jenkins jobb använder 'kubectl' toomanage och distribuera toohello Kubernetes kluster.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a>Steg 2: Konfigurera åtkomst toohello Kubernetes kluster

> [!NOTE]
> Det finns flera metoder tooaccomplishing hello följande steg. Använd hello-metod som är enklast för dig.
>

1. Kopiera hello `kubectl` config-fil toohello Jenkins datorn så att Jenkins jobb har åtkomst toohello Kubernetes kluster. Dessa instruktioner förutsätter att du använder bash från en annan dator än hello Jenkins VM och att en lokal offentlig SSH-nyckel lagras i hello datorns ~/.ssh mapp.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Verifiera att hello Kubernetes från Jenkins klustret är tillgänglig.  toodo detta, SSH till hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Kontrollera Jenkins kan ansluta tooyour kluster: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Skapa ett arbetsflöde för Jenkins

### <a name="prerequisites"></a>Krav

- GitHub-konto för koden lagringsplatsen.
- Docker-hubb konto toostore och uppdatera avbildningar.
- Av program som kan byggas och uppdateras. Du kan använda den här exempelappen behållare som skrivits i Golang: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> hello måste följande steg utföras i ditt GitHub-konto. Känna sig fria tooclone hello ovan lagringsplatsen, men du måste använda ditt eget konto tooconfigure hello webhooks och Jenkins komma åt.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Steg 1: Distribuera inledande v1 för program
1. Skapa hello appen från hello developer datorn med hello följande kommandon. Ersätt `myrepo` med dina egna.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Push-avbildningen tooDocker hubb.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Distribuera toohello Kubernetes kluster.
    
    > [!NOTE] 
    > Redigera hello `go-web.yaml` filen tooupdate din behållaren avbildningen och lagringsplatsen.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Steg 2: Konfigurera Jenkins system
1. Klicka på **hantera Jenkins** > **konfigurera systemet**.
2. Under **GitHub**väljer **Lägg till GitHub-Server**.
3. Lämna **API-URL** som standard.
4. Under **autentiseringsuppgifter**, lägga till en Jenkins autentiseringsuppgifter med hjälp av **hemliga text**. Vi rekommenderar att du använder GitHub personlig åtkomst-token har konfigurerats i din GitHub inställningar. Mer information om detta [här.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Klicka på **Testanslutningen** tooensure konfigureras korrekt.
6. Under **globala egenskaper**, lägger du till ett miljövariabel `DOCKER_HUB` och ange lösenordet Docker-hubb. (Detta är användbart i den här demon, men en form av produktionsscenario kräver ett säkrare sätt.)
7. Spara.

![Jenkins GitHub-åtkomst](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a>Steg 3: Skapa hello Jenkins arbetsflöde
1. Skapa ett Jenkins-objekt.
2. Ange ett namn (till exempel ”gå web”) och välj **Freestyle projekt**. 
3. Kontrollera **GitHub projekt** och ange hello URL tooyour GitHub-lagringsplatsen.
4. I **källa kod Management**, ange hello GitHub-repo-URL och autentiseringsuppgifter. 
5. Lägg till en **skapa steg** av typen **köra shell** och Använd hello följande text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Lägga till en annan **skapa steg** av typen **köra shell** och Använd hello följande text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins skapa steg](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Spara hello Jenkins objekt och testa med **skapa nu**.

### <a name="step-4-connect-github-webhook"></a>Steg 4: Anslut GitHub-webhook
1. Hej Jenkins objekt som du har skapat, klicka på **konfigurera**.
2. Under **Skapa utlösare**väljer **GitHub hook utlösare för GITScm avsökning** och **spara**. Hej GitHub-webhook konfigureras automatiskt.
3. Klicka på ditt GitHub-lagringsplatsen för gå webb **Inställningar > Webhooks**.
4. Kontrollera att hello Jenkins webhook URL har lagts till. hello URL måste sluta med ”github-webhook”.

![Jenkins webhook-konfiguration](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a>Testa hello CI/CD-processen slutet tooend

1. Uppdatera koden för hello lagringsplatsen och push-synk med hello GitHub-lagringsplatsen.
2. Kontrollera hello hello Jenkins konsolen **skapa historik** och verifiera att hello jobbet kördes. Visa konsolen utdata toosee information.
3. Visa information om hello uppgraderas från Kubernetes, distribution:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Nästa steg

- Distribuera Azure-behållare registret och lagra avbildningar i en säker databas. Se [Azure Container registret docs](https://docs.microsoft.com/azure/container-registry).
- Skapa en mer komplex arbetsflöde som innehåller sida-vid-sida-distribution och testerna i Jenkins.
- Mer information om CI/CD-skiva med Jenkins och Kubernetes finns hello [Jenkins blogg](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
