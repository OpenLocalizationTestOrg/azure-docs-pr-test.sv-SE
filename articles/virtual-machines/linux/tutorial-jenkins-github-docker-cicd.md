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
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Hur toocreate en infrastruktur för utveckling på en Linux-VM i Azure med Jenkins, GitHub och Docker
tooautomate hello build och testa fas för programutveckling, kan du använda en kontinuerlig integrering och distribution (CI/CD) pipeline. I den här självstudiekursen skapar du en CI/CD-pipeline på en Azure VM att:

> [!div class="checklist"]
> * Skapa en virtuell dator Jenkins
> * Installera och konfigurera Jenkins
> * Skapa webhook integrering mellan GitHub och Jenkins
> * Skapa och genomför utlösaren Jenkins skapa jobb från GitHub
> * Skapa en Docker-avbildning för din app
> * Kontrollera GitHub incheckningar Skapa ny Docker-avbildning och uppdateringar som kör appen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Skapa en instans av Jenkins
I en tidigare självstudiekurs om [hur toocustomize en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md), du lärt dig hur tooautomate VM anpassning med molnet initiering. Den här kursen använder ett moln init filen tooinstall Jenkins och Docker på en virtuell dator. 

Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration. Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn. Ange `sensible-editor cloud-init-jenkins.txt` toocreate hello filen och visas i listan över tillgängliga redigerare. Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:

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

Innan du kan skapa en virtuell dator, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupJenkins* i hello *eastus* plats:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create). Använd hello `--custom-data` parametern toopass i konfigurationsfilen molnet initiering. Ange hello fullständig sökväg för*moln-init-jenkins.txt* om du har sparat filen hello utanför arbetskatalogen finns.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Det tar några minuter för hello VM toobe skapas och konfigureras.

tooallow web tooreach trafik den virtuella datorn, Använd [az vm öppna port](/cli/azure/vm#open-port) tooopen port *8080* för Jenkins trafik och port *1337* för hello Node.js-app som är används toorun en exempelapp:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Konfigurera Jenkins
tooaccess din Jenkins instansen kan du få hello offentliga IP-adressen för den virtuella datorn:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Av säkerhetsskäl måste du tooenter hello inledande adminlösenord som lagras i en textfil på din Virtuella toostart hello Jenkins installera. Använd hello offentlig IP-adress som erhölls i hello föregående steg tooSSH tooyour VM:

```bash
ssh azureuser@<publicIps>
```

Visa hello `initialAdminPassword` för din Jenkins installera och kopiera den:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Om hello-filen är inte tillgängligt ännu, Vänta några minuter för molnet init toocomplete hello Jenkins och Docker-installation.

Öppna en webbläsare och gå för`http://<publicIps>:8080`. Slutför hello Jenkins installationen på följande sätt:

- Ange hello *initialAdminPassword* från hello VM i hello föregående steg.
- Klicka på **Välj tooinstall plugin-program**
- Sök efter *GitHub* hello texten i rutan överst hello väljer hello *GitHub-plugin-programmet*, klicka på **installera**
- toocreate ett Jenkins användarkonto, Fyll i hello formuläret efter behov. Du bör skapa första Jenkins användaren snarare än fortsätter som hello admin standardkonto från ett säkerhetsperspektiv.
- När du är klar klickar du på **börja använda Jenkins**


## <a name="create-github-webhook"></a>Skapa GitHub-webhook
tooconfigure hello integrering med GitHub, öppna hello [Node.js Hello World exempelapp](https://github.com/Azure-Samples/nodejs-docs-hello-world) från hello Azure-exempel lagringsplatsen. toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.

Skapa en webhook inuti hello förgrening som du skapade:

- Klicka på **inställningar**och välj **integreringar & tjänster** hello vänster.
- Klicka på **lägga till tjänst**, ange *Jenkins* i filterfältet.
- Välj *Jenkins (GitHub-plugin-programmet)*
- För hello **Jenkins koppla URL**, ange `http://<publicIps>:8080/github-webhook/`. Kontrollera att du inkluderar avslutande hello /
- Klicka på **lägga till tjänst**

![Lägg till GitHub-repo-tooyour forked webhooken](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Skapa Jenkins jobb
toohave Jenkins svara tooan händelsen i GitHub, till exempel när en kod, skapa ett Jenkins-jobb. 

Klicka på webbplatsen Jenkins **skapa nya jobb** från hello startsida:

- Ange *HelloWorld* som jobbnamn. Välj **Freestyle projektet**och välj **OK**.
- Under hello **allmänna** väljer **GitHub** projektet och ange din andelen vridvuxna lagringsplatsen URL, exempelvis *https://github.com/iainfoulds/nodejs-docs-hello-world*
- Under hello **datakällan kod management** väljer **Git**, ange din andelen vridvuxna lagringsplatsen *.git* URL, exempelvis *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- Under hello **Skapa utlösare** väljer **GitHub hook utlösare för GITscm avsökning**.
- Under hello **skapa** väljer **Lägg till build steg**. Välj **köra shell**, ange `echo "Testing"` i toocommand fönster.
- Klicka på **spara** längst hello hello jobb-fönstret.


## <a name="test-github-integration"></a>Testa GitHub-integrering
tootest hello GitHub integrering med Jenkins, bekräfta att en ändring i din förgrening. 

Tillbaka i GitHub-webbgränssnitt, Välj din andelen vridvuxna lagringsplatsen och klicka sedan på hello **index.js** fil. Klicka på hello penna ikonen tooedit filen så läser rad 6:

```nodejs
response.end("Hello World!");
```

toocommit dina ändringar klickar du på hello **genomför ändringar** knappen längst ned hello.

I Jenkins, startar en ny version under hello **skapa historik** avsnitt i hello längst ned till vänster på sidan jobb. Klicka på länken för hello build antal och markera **konsolen utdata** på hello vänstra storlek. Du kan visa hello steg Jenkins utförs som koden hämtas från GitHub och hello build utdata hello åtgärdsmeddelande `Testing` toohello-konsolen. Varje gång ett genomförande görs i GitHub hello webhook når ut tooJenkins och utlösa en ny version på detta sätt.


## <a name="define-docker-build-image"></a>Definiera Docker build-bild
toosee hello Node.js-app som körs baserat på ditt GitHub-incheckningar kan bygga en Docker bild toorun hello app. hello-avbildningen skapas från en Dockerfile som definierar hur tooconfigure hello behållare som kör hello-app. 

Hello SSH-anslutning tooyour VM, ändra toohello Jenkins arbetsytan katalogen efter hello jobb som du skapade i föregående steg. I vårt exempel som kallades *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Skapa en fil med i den här arbetsytan katalogen med `sudo sensible-editor Dockerfile` och klistra in hello efter innehållet. Kontrollera att hello hela Dockerfile har kopierats på rätt sätt, särskilt hello första rad:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Den här Dockerfile använder hello Node.js basavbildningen med Alpine Linux, visar port 1337 som hello World Hello app körs på, och sedan kopierar hello-programfilerna och initierar den.


## <a name="create-jenkins-build-rules"></a>Skapa Jenkins build-regler
I föregående steg skapa Jenkins build regel som resulterar i ett meddelande toohello-konsolen. Kan skapa hello build steg toouse våra Dockerfile och köra hello app.

Välj hello jobb som du skapade i föregående steg i din Jenkins-instans. Klicka på **konfigurera** på vänster sida av hello och rulla ned toohello **skapa** avsnitt:

- Ta bort den befintliga `echo "Test"` skapa steg. Klicka på hello röd mellan hello övre högra hörnet på hello befintlig build steg ruta.
- Klicka på **Lägg till build steg**och välj **köra shell**
- I hello **kommandot** , ange hello följande Docker-kommandon och sedan markera **spara**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

Hej Docker build steg skapa en avbildning och tagg med hello Jenkins build-nummer så att du kan hantera en historik över bilder. Alla befintliga behållare kör hello app stoppas och sedan har tagits bort. En ny behållare är igång med hello avbildningen och kör Node.js-appen baserat på hello senaste incheckningar i GitHub.


## <a name="test-your-pipeline"></a>Testa din pipeline
toosee hello hela pipeline i åtgärden, redigera hello *index.js* filen i din GitHub-repo-andelen vridvuxna igen och klicka på **spara ändringen**. Ett nytt jobb startar i Jenkins baserat på hello webhook för GitHub. Det tar några sekunder toocreate hello Docker avbildningen och starta appen i en ny behållare.

Om det behövs kan du hämta hello offentliga IP-adressen för den virtuella datorn igen:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Öppna en webbläsare och ange `http://<publicIps>:1337`. Node.js-appen visas och visar hello senaste incheckningar i din GitHub-förgrening på följande sätt:

![Node.js-app som körs](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Skapa en annan redigera toohello *index.js* filen i GitHub och spara hello ändra. Vänta några sekunder för hello jobbet toocomplete i Jenkins och uppdatera sedan din webbläsare toosee hello uppdateras webbversionen av din app som körs i en ny behållare på följande sätt:

![Node.js-app som körs efter en annan GitHub genomförande](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerats GitHub toorun ett Jenkins build-jobb på varje kod genomförande och distribuera en Docker-behållare tootest din app. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell dator Jenkins
> * Installera och konfigurera Jenkins
> * Skapa webhook integrering mellan GitHub och Jenkins
> * Skapa och genomför utlösaren Jenkins skapa jobb från GitHub
> * Skapa en Docker-avbildning för din app
> * Kontrollera GitHub incheckningar Skapa ny Docker-avbildning och uppdateringar som kör appen

I förväg toohello nästa självstudiekurs toolearn mer om hur toointegrate Jenkins med Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Distribuera appar med Jenkins och Team Services](tutorial-build-deploy-jenkins.md)