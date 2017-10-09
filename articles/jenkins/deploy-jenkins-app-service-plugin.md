---
title: aaaDeploy tooAzure App Service med Jenkins Plugin | Microsoft Docs
description: "Lär dig hur toouse Azure App Service Jenkins plugin-programmet toodeploy en Java web app tooAzure i Jenkins"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a>Distribuera tooAzure Apptjänst med Jenkins plugin-program 
toodeploy tooAzure en Java web app som du kan använda Azure CLI i [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) eller så kan du använda hello [plugin-program för Azure App Service Jenkins](https://plugins.jenkins.io/azure-app-service). I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Konfigurera Jenkins toodeploy tooAzure Apptjänst via FTP 
> * Konfigurera Jenkins toodeploy tooAzure Apptjänst på Linux via Docker 

## <a name="create-and-configure-jenkins-instance"></a>Skapa och konfigurera Jenkins instans
Om du inte redan har en Jenkins master börja med hello [Lösningsmall](install-jenkins-solution-template.md), bland annat JDK8 och hello följande obligatoriska plugin-program:

* [Jenkins Git-plugin för klienten](https://plugins.jenkins.io/git-client) v.2.4.6 
* [Plugin-programmet för docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0
* [Autentiseringsuppgifter för Azure](https://plugins.jenkins.io/azure-credentials) v.1.2
* [Azure Apptjänst](https://plugins.jenkins.io/azure-app-server) v.0.1

Du kan använda hello Apptjänst plugin-programmet toodeploy Web App på alla språk (till exempel C#, PHP, Java och node.js etc.) som stöds av Azure App Service. I den här kursen använder hello för Java exempelapp [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample). toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.  

JDK Java och Maven krävs för att skapa hello Java-projekt. Se till att installera hello komponenter i hello Jenkins master eller hello VM-agenten om du använder en löpande integreringen. 

tooinstall, logga in toohello Jenkins-instans med SSH och kör följande kommandon hello:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

För att distribuera tooApp-tjänsten på Linux, behöver du också tooinstall Docker på hello Jenkins master eller hello VM-agenten används för version. Se toothis artikel tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.

## <a name="add-azure-service-principal-toojenkins-credential"></a>Lägg till Azure-tjänstens huvudnamn tooJenkins autentiseringsuppgift

En Azure-tjänstens huvudnamn är nödvändiga toodeploy tooAzure. 

<ol>
<li>Använd [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) eller [Azure-portalen](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate en Azure-tjänstens huvudnamn</li>
<li>Inom hello Jenkins instrumentpanelen, klickar du på **autentiseringsuppgifter -> System ->**. Klicka på **globala credentials(unrestricted)**.</li>
<li>Klicka på **lägga till autentiseringsuppgifter** tooadd ett huvudnamn för tjänsten Microsoft Azure för genom att fylla i hello prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt. Ange ett ID **mySp**, för användning i efterföljande steg.</li>
</ol>

## <a name="azure-app-service-plugin"></a>Azure Apptjänst-plugin-programmet

Azure Apptjänst-plugin-programmet v1.0 stöder kontinuerlig distribution tooAzure webbprogram via:

* Git och FTP
* Docker för webbprogrammet på Linux

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a>Konfigurera Jenkins toodeploy webbprogram via FTP-hjälp hello Jenkins instrumentpanelen

toodeploy ditt projekt tooAzure Web App kan du överföra build-artefakter (till exempel .war fil i Java) med hjälp av Git eller FTP.

Innan du konfigurerar hello jobb i Jenkins behöver du en Azure App Service-plan och en Webbapp för körs hello Java-app.


1. Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando. Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar. Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar.
2. Skapa en webbapp. Du kan antingen använda hello [Azure-portalen](/azure/app-service-web/web-sites-configure) eller Använd hello följande Az CLI-kommando:
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. Kontrollera att du ställer in hello Java runtime konfiguration som din app behöver. hello följande Azure CLI kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a>Ange hello Jenkins jobb


1. Skapa en ny **freestyle** projekt i Jenkins instrumentpanelen
2. Konfigurera **källa kod Management** toouse din lokala förgrening av [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) genom att tillhandahålla hello **databasen URL**. Till exempel: http://github.com/&lt;yourID > / javawebappsample.
3. Lägga till ett Build steg toobuild hello-projekt med Maven. Göra det genom att lägga till en **köra shell**. I det här exemplet behöver vi en ytterligare steg toorename hello *.war fil i målet mappen tooROOT.war.   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. Lägg till en efter build-åtgärd genom att välja **publicera ett Azure Web App**.
5. Ange ”mySp”, hello Azure tjänstens huvudnamn lagras i föregående steg.
6. I **Appkonfiguration** väljer hello resurs grupp och webb-app i din prenumeration. hello-plugin-programmet identifierar automatiskt om hello Web App är Windows eller Linux-baserade. För en Windows-baserade Webbapp, hello alternativet ”Publicera filer” visas.
7. Fyll i hello filer du toodeploy (till exempel en war-paket om du använder Java.) Källa och målkatalog är valfria. hello-parametrar kan du toospecify käll- och mappar när överföringen av filer. Java-webbapp på Azure körs i en Tomcat-server. Så att du överför krig paketet till webbappens mapp. Det här exemplet anger **källkatalog** för ”mål” och ange ”webbappar” för **målkatalogen**.
8. Om du vill toodeploy tooa fack än produktion, kan du också ange **fack** namn.
9. Spara hello projekt och skapa den. Webbappen är distribuerade tooAzure när version har slutförts.

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a>Distribuera webbprogram via FTP-använda Jenkins pipeline

hello-plugin-programmet är redo pipeline. Du kan se tooa provet i GitHub-repo-hello.

1. Öppna i GitHub-webbgränssnittet, **Jenkinsfile_ftp_plugin** fil. Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 11 och 12 respektive.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Ändra raden 14 tooupdate uppgifts-ID i din instans av Jenkins.    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a>Skapa en Jenkins pipeline

1. Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.
2. Ange ett namn för hello jobbet och välja **Pipeline**. Klicka på **OK**.
3. Klicka på hello **Pipeline** fliken nästa.
4. För **Definition**väljer **Pipeline-skriptet från SCM**.
5. För **SCM**väljer **Git**. Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:&lt;din andelen vridvuxna lagringsplatsen > .git
6. Uppdatera **skriptsökvägen** för ”Jenkinsfile_ftp_plugin”
7. Klicka på **spara** och kör hello-jobbet.

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a>Konfigurera Jenkins toodeploy webbprogrammet på Linux via Docker

Förutom Git/FTP stöder webbprogram på Linux-distributionen med Docker. toodeploy med Docker, behöver du tooprovide en Dockerfile som ditt webbprogram med tjänsten runtime-paket till en docker-bild. Sedan hello plugin bygger hello bild, skickar den tooa docker registret och distribuerar hello avbildningen tooyour webbprogram.

Web App på Linux stöder också traditionella sätt som Git och FTP, men endast för inbyggda språk (.NET Core, Node.js, PHP och Ruby). För andra språk du behöver toopackage programmets kod och tjänsten körning tillsammans i en docker-avbildningen och använda docker toodeploy.

Innan du konfigurerar hello jobb i Jenkins måste en Azure apptjänst i Linux. En behållare registret är också nödvändig toostore och hantera dina privata Docker behållare avbildningar. Du kan använda DockerHub; Vi använder Azure Container registret för det här exemplet.

* Du kan följa stegen hello [här](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate ett webbprogram på Linux 
* Azure-behållare är en hanterad [Docker registret] (https://docs.docker.com/registry/) tjänsten baserat på hello öppen källkod Docker registret 2.0. Gör hello [här] (/ azure/container-registry/container-registry-get-started-azure-cli) för mer information om hur toodo så. Du kan också använda DockerHub.

### <a name="toodeploy-using-docker"></a>toodeploy med docker:

1. Skapa ett nytt freestyle projekt i Jenkins instrumentpanelen.
2. Konfigurera **källa kod Management** toouse din lokala förgrening av [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) genom att tillhandahålla hello **databasen URL**. Till exempel: http://github.com/&lt;yourid > / javawebappsample.
Lägga till ett Build steg toobuild hello-projekt med Maven. Göra det genom att lägga till en **köra shell** och Lägg till följande rad i hello **kommandot**:    
```bash
mvn clean package
```

3. Lägg till en efter build-åtgärd genom att välja **publicera ett Azure Web App**.
4. Ange, **mySp**, hello Azure tjänstens huvudnamn lagras i föregående steg som Azure-autentiseringsuppgifter.
5. I **Appkonfiguration** väljer hello resursgrupp och en Linux-webbapp i din prenumeration.
6. Välj publicera via Docker.
7. Fyll i **Dockerfile** sökväg. Du kan behålla standard hello ”/ Dockerfile” för **Docker registret URL**, varor i hello-format för https://&lt;myRegistry >. azurecr.io om du använder Azure-behållare registret. Lämna det tomt om du använder DockerHub.
8. För **registret autentiseringsuppgifter**, Lägg till hello autentiseringsuppgift för hello Azure Container registret. Du kan hämta hello användar-ID och lösenord genom att köra följande kommandon i Azure CLI hello. hello första kommando aktiverar hello-administratörskonto.    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. Hej docker namn och en tagg i **Avancerat** fliken är valfria. Som standard avbildningsnamn erhålls från hello avbildning namn som du konfigurerade i Azure portal (i Dockerbehållare inställningen.) hello taggen genereras från $BUILD_NUMBER. Kontrollera att du anger hello avbildningsnamn i antingen Azure-portalen eller ange ett värde för **Docker bild** i **Avancerat** fliken. Det här exemplet anger ”&lt;yourRegistry >.azurecr.io/calculator” för **Docker bild** och lämna **Docker bildtagg** tomt.
10. Obs distributionen misslyckas om du använder inställningen av inbyggda Docker-bild. Kontrollera att du ändrar docker config toouse anpassad avbildning i Dockerbehållare inställningen i Azure-portalen. Använd filen överför metoden toodeploy för inbyggda avbildningen.
11. Du kan välja en annan plats än produktion liknande toofile överför sätt.
12. Spara och skapa hello-projekt. Du ser behållaren avbildningen skickas tooyour registret och webbapp distribueras.

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a>Distribuera tooWeb App på Linux via Docker använda Jenkins pipeline

1. Öppna i GitHub-webbgränssnittet, **Jenkinsfile_container_plugin** fil. Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 11 och 12 respektive.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Ändra raden 13 tooyour behållare registret servern    
```java
def registryServer = '<registryURL>'
```    

3. Ändra raden 16 tooupdate uppgifts-ID i Jenkins-instans    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a>Skapa Jenkins pipeline    

1. Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.
2. Ange ett namn för hello jobbet och välja **Pipeline**. Klicka på **OK**.
3. Klicka på hello **Pipeline** fliken nästa.
4. För **Definition**väljer **Pipeline-skriptet från SCM**.
5. För **SCM**väljer **Git**.
6. Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:&lt;din andelen vridvuxna lagringsplatsen > .git</li>
7, uppdatera **skriptsökvägen** för ”Jenkinsfile_container_plugin”
8. Klicka på **spara** och kör hello-jobbet.

## <a name="verify-your-web-app"></a>Kontrollera ditt webbprogram

1. tooverify hello WAR-filen har distribueras tooyour webbprogram. Öppna en webbläsare.
2. Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping visas:    
     Välkommen till tooJava webbprogrammet! Detta har uppdaterats!
   Sun Jun 17 16:39:10 UTC 2017
3. Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y        
    ![Kalkylatorn: Lägg till](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a>För apptjänst på Linux

* tooverify i Azure CLI kör:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Du får hello följande resultat:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping. Hello-meddelande visas: 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y
    
## <a name="next-steps"></a>Nästa steg

I den här kursen använder du hello Azure App Service-plugin-programmet toodeploy tooAzure.

Du har lärt dig att:

> [!div class="checklist"]
> * Konfigurera Jenkins toodeploy Azure App Service via FTP 
> * Konfigurera Jenkins toodeploy tooAzure Apptjänst på Linux via Docker 
