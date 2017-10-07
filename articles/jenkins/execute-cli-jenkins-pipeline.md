---
title: aaaExecute hello Azure CLI med Jenkins | Microsoft Docs
description: "Lär dig hur toouse Azure CLI toodeploy en Java web app tooAzure i Jenkins Pipeline"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a>Distribuera tooAzure App Service med Jenkins och hello Azure CLI
toodeploy tooAzure en Java web app som du kan använda Azure CLI i [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/). I den här självstudiekursen skapar du en CI/CD-pipeline på en Azure VM att:

> [!div class="checklist"]
> * Skapa en virtuell dator Jenkins
> * Konfigurera Jenkins
> * Skapa en webbapp i Azure
> * Förbereda en GitHub-databas
> * Skapa Jenkins pipeline
> * Kör hello pipeline och verifiera hello-webbprogram

Den här kursen kräver hello Azure CLI version 2.0.4 eller senare. toofind hello version, kör `az --version`. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Skapa och konfigurera Jenkins instans
Om du inte redan har en Jenkins master börja med hello [Lösningsmall](install-jenkins-solution-template.md), som innehåller hello krävs [Azure-autentiseringsuppgifterna](https://plugins.jenkins.io/azure-credentials) plugin-programmet som standard. 

Autentiseringsuppgifter för Azure-plugin-programmet hello tillåter toostore Microsoft Azure service principal autentiseringsuppgifter i Jenkins. I version 1.2, vi har lagt till stöd för hello så att Jenkins-Pipeline kan få hello autentiseringsuppgifter för Azure. 

Se till att du har version 1.2 eller senare:
* Inom hello Jenkins instrumentpanelen, klickar du på **Plugin-hanteraren -> Hantera Jenkins ->** och Sök efter **Azure autentiseringsuppgifter**. 
* Uppdatera hello plugin om hello-versionen är tidigare än 1.2.

JDK Java och Maven också krävs i hello Jenkins master. tooinstall, logga in tooJenkins master med SSH och kör följande kommandon hello:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a>Lägg till Azure-tjänstens huvudnamn tooJenkins autentiseringsuppgift

En Azure-autentiseringsuppgift är nödvändiga tooexecute Azure CLI.

* Inom hello Jenkins instrumentpanelen, klickar du på **autentiseringsuppgifter -> System ->**. Klicka på **globala credentials(unrestricted)**.
* Klicka på **lägga till autentiseringsuppgifter** tooadd en [Microsoft Azure-tjänstens huvudnamn](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) genom att fylla i hello prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt. Ange ett ID för användning i senare steg.

![Lägga till autentiseringsuppgifter](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a>Skapa en Azure App Service för att distribuera hello Java-webbapp

Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando. Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar. Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

När hello plan är klar, utdata hello Azure CLI visar liknande toohello följande exempel:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Skapa en Azure-webbapp

 Använd hello [az webapp skapa](/cli/azure/appservice/web#create) CLI kommandot toocreate en web app definition i hello `myAppServicePlan` App Service-plan. hello web app definition innehåller en URL-tooaccess ditt program med och konfigurerar flera alternativ toodeploy tooAzure din kod. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Ersätt hello `<app_name>` med dina egna unika namn. Detta unika namn är en del av hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure. Du kan mappa ett webbprogram för domänen namnet post toohello innan exponera tooyour användare.

När hello web app definition är klar visar hello Azure CLI information liknande toohello följande exempel: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurera Java 

Konfigurera hello Java runtime konfiguration som din app behöver med hello [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.

hello följande kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Förbereda en GitHub-databas
Öppna hello [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen. toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.

* Öppna i GitHub-webbgränssnittet, **Jenkinsfile** fil. Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 20 och 21 respektive.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Ändra raden 23 tooupdate uppgifts-ID i Jenkins-instans

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Skapa Jenkins pipeline
Öppna Jenkins i en webbläsare, klicka på **nytt objekt**. 

* Ange ett namn för hello jobbet och välja **Pipeline**. Klicka på **OK**.
* Klicka på hello **Pipeline** fliken nästa. 
* För **Definition**väljer **Pipeline-skriptet från SCM**.
* För **SCM**väljer **Git**.
* Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:\<din andelen vridvuxna lagringsplatsen\>.git
* Klicka på **spara**

## <a name="test-your-pipeline"></a>Testa din pipeline
* Gå toohello pipeline som du skapade **skapa nu**
* En version ska lyckas i några sekunder och du kan gå toohello build och klicka på **konsolens utdata** toosee hello information

## <a name="verify-your-web-app"></a>Kontrollera ditt webbprogram
tooverify hello WAR-filen har distribueras tooyour webbprogram. Öppna en webbläsare:

* Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping  
Du ser:

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y

![Kalkylatorn: Lägg till](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a>Distribuera tooAzure webbprogram på Linux
Nu när du vet hur toouse Azure CLI i din Jenkins pipeline, kan du ändra hello skriptet toodeploy tooan Azure Web App på Linux.

Web App på Linux stöder en annorlunda sätt toodo hello distribution, vilket är toouse Docker. toodeploy, behöver du tooprovide en Dockerfile som ditt webbprogram med tjänsten runtime-paket till en Docker-bild. hello-plugin-programmet skapar hello-avbildning, push-installera den tooa Docker registret och därefter distribuera hello avbildningen tooyour webbapp.

* Gör hello [här](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate en Azure-Webbapp som körs på Linux.
* Installera Docker på Jenkins-instans genom att följa hello instruktionerna i det här [artikel](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Skapa en behållare registret hello Azure-portalen med hjälp av hello steg [här](/azure/container-registry/container-registry-get-started-azure-cli).
* Hej i samma [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen du forked redigera hello **Jenkinsfile2** fil:
    * Rad 18 21, uppdatera toohello namn på din resursgrupp, webbprogram och ACR respektive. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Raden 24, uppdatera \<azsrvprincipal\> tooyour uppgifts-ID
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Skapa en ny Jenkins pipeline som du gjorde när du distribuerar tooAzure webbapp i Windows, men den här gången Använd **Jenkinsfile2** i stället.
* Kör ditt nya jobb.
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
Du har konfigurerat en Jenkins pipeline som checkar ut hello källkoden i GitHub-repo i den här självstudiekursen. Kör Maven toobuild en war-fil och sedan använder Azure CLI toodeploy tooAzure Apptjänst. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell dator Jenkins
> * Konfigurera Jenkins
> * Skapa en webbapp i Azure
> * Förbereda en GitHub-databas
> * Skapa Jenkins pipeline
> * Kör hello pipeline och verifiera hello-webbprogram
