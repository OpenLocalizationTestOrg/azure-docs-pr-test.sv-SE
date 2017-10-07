---
title: "aaaCI/CD med Azure Container Service-motorn och Swarm läge | Microsoft Docs"
description: "Använda Azure Container Service-motorn med Docker Swarm-läge, ett Azure Container registret och Visual Studio Team Services toodeliver kontinuerligt ett flera behållare .NET Core-program"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Docker behållare Micro-tjänster, Swarm Azure, Visual Studio Team Services, DevOps, ACS-motorn"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>Fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med ACS-motorn och Docker Swarm-läge med hjälp av Visual Studio Team Services

*Den här artikeln är baserad på [fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med Docker Swarm med hjälp av Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) dokumentation*

Men i dag en av hello största utmaningarna när utveckla moderna applikationer för hello molnet kan toodeliver dessa program kontinuerligt. I den här artikeln får du lära dig hur pipeline med tooimplement en fullständig kontinuerlig integrering och distribution (CI/CD): 
* Azure Container Service-motorn med Docker Swarm-läge
* Azure Container Registry
* Visual Studio Team Services

Den här artikeln är baserad på ett enkelt program tillgängliga på [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), utvecklade med ASP.NET Core. hello program består av fyra olika tjänster: tre webb-API: er och en frontwebb:

![MyShop exempelprogrammet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

hello mål är toodeliver programmet kontinuerligt i ett läge för Docker Swarm-kluster med hjälp av Visual Studio Team Services. hello följande figur information denna kontinuerlig leverans pipeline:

![MyShop exempelprogrammet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Här följer en kort beskrivning av hello steg:

1. Koden ändringarna har bekräftats toohello källdatabasen kod (här GitHub) 
2. GitHub utlöser en version i Visual Studio Team Services 
3. Visual Studio Team Services hämtar hello senaste versionen av hello källor och skapar alla hello-avbildningar som ska utgöra hello program 
4. Visual Studio Team Services skickar varje avbildning tooa Docker-registret som skapats med hjälp av hello Azure Container Registry-tjänsten 
5. Visual Studio Team Services utlöser en ny version 
6. hello körs vissa kommandon med hjälp av SSH på hello Azure container service master klusternod 
7. Docker Swarm-läge på kluster hello hämtar hello senaste versionen av hello bilder 
8. hello ny version av programmet hello distribueras med Docker-stacken 

## <a name="prerequisites"></a>Krav

Innan du påbörjar de här självstudierna måste toocomplete hello följande uppgifter:

- [Skapa ett läge för Swarm-kluster i Azure Container Service med ACS-motorn](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Ansluta med hello Swarm-kluster i Azure Container Service](../container-service-connect.md)
- [Skapa en Azure-behållaren registernyckel](../../container-registry/container-registry-get-started-portal.md)
- [Har en Visual Studio Team Services-konto och team projekt](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Duplicera hello GitHub-lagringsplatsen tooyour GitHub-konto](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> Hej Docker Swarm orchestrator i Azure Container Service använder äldre fristående Swarm. För närvarande hello integrerad [Swarm läge](https://docs.docker.com/engine/swarm/) (i Docker 1.12 och högre) är inte en stöds orchestrator i Azure Container Service. Därför kan vi använda [ACS-motorn](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), en community-bidragit [quickstart mallen](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), eller en Docker-lösning i hello [Azure Marketplace](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Steg 1: Konfigurera Visual Studio Team Services-konto 

I det här avsnittet kan du konfigurera Visual Studio Team Services-konto. tooconfigure VSTS Services-slutpunkter, i ditt projekt i Visual Studio Team Services, klicka på hello **inställningar** ikon i hello verktygsfältet och välj **Services**.

![Öppna Services slutpunkt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Ansluta till Visual Studio Team Services och Azure-konto

Upprätta en anslutning mellan projektet VSTS och ditt Azure-konto.

1. Klicka på vänster hello **nya tjänstslutpunkten** > **Azure Resource Manager**.
2. tooauthorize VSTS toowork med ditt Azure-konto, Välj din **prenumeration** och på **OK**.

    ![Visual Studio Team Services - auktorisera Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Ansluta Visual Studio Team Services och GitHub

Upprätta en anslutning mellan projektet VSTS och GitHub-konto.

1. Klicka på vänster hello **nya tjänstslutpunkten** > **GitHub**.
2. tooauthorize VSTS toowork med ditt GitHub-konto, klickar du på **auktorisera** och följa hello i hello-fönster som öppnas.

    ![Visual Studio Team Services - auktorisera GitHub](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a>Ansluta VSTS tooyour Azure Container Service-kluster

hello sista stegen innan du hämtar till hello CI/CD-pipeline är tooconfigure externa anslutningar tooyour Docker Swarm-kluster i Azure. 

1. Lägga till en slutpunkt av typen för Docker Swarm-kluster hello **SSH**. Ange hello SSH-anslutningsinformationen för Swarm-klustret (nod).

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Alla hello-konfigurationen är nu klar. Skapa hello CI/CD-pipeline som skapar och distribuerar hello programmet toohello Docker Swarm-kluster i hello nästa steg. 

## <a name="step-2-create-hello-build-definition"></a>Steg 2: Skapa hello build-definition

I det här steget du ställer in en build-definition för projektet VSTS och definiera hello build arbetsflöde för bilderna behållare

### <a name="initial-definition-setup"></a>Inledande definition installationen

1. toocreate en build-definition ansluta tooyour Visual Studio Team Services projektet och klicka på **Skapa & släpper**. I hello **skapa definitioner** klickar du på **+ ny**. 

    ![Visual Studio Team Services - nya skapa Definition](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Välj hello **tom process**.

    ![Visual Studio Team Services - ny tom skapa Definition](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Klicka på hello **variabler** fliken och skapar två nya variabler: **RegistryURL** och **AgentURL**. Klistra in hello värden för registernyckeln och klustret agenter DNS.

    ![Visual Studio Team Services - variabler Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. På hello **skapa definitioner** sida, öppna hello **utlösare** och konfigurera hello build toouse kontinuerlig integration med hello förgrening av hello MyShop projekt som du skapade i hello krav. Markera **Batch ändringar**. Kontrollera att du väljer *docker-linux* som hello **Förgrena specifikationen**.

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Klicka slutligen på hello **alternativ** och konfigurera hello standard agent kön för**finns Linux Preview**.

    ![Visual Studio Team Services - agenten Värdkonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a>Definiera hello build-arbetsflöde
Nästa steg i hello definiera hello build-arbetsflöde. Du måste först tooconfigure hello källan för hello koden. toodo det, väljer **GitHub** och **databasen** och **gren** (docker-linux).

![Konfigurera Visual Studio Team Services - Kodkälla](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Det finns fem behållaren bilder toobuild för hello *MyShop* program. Varje avbildning skapas med hello Dockerfile finns i hello projektet mappar:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Du behöver två Docker steg för varje avbildning, en toobuild hello bild och en toopush hello bild i registret för hello Azure-behållaren. 

1. tooadd ett steg i hello build-arbetsflöde, klickar du på **+ Lägg till build steg** och välj **Docker**.

    ![Visual Studio Team Services – Lägg till Build steg](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. För varje bild och konfigurera ett steg som använder hello `docker build` kommando.

    ![Visual Studio Team Services - Docker skapa](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Hello skapa i åtgärden, Välj Azure-behållare registret, hello **skapa en avbildning** åtgärd och hello Dockerfile som definierar varje avbildning. Ange hello **Working Directory** som hello Dockerfile rotkatalog, definiera hello **avbildningsnamn**, och välj **inkludera senaste taggen**.
    
    hello Avbildningsnamnet har toobe i det här formatet: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Ersätt **[NAME]** med hello avbildningens namn:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. För varje bild och konfigurera ett andra steg som använder hello `docker push` kommando.

    ![Visual Studio Team Services - Docker Push](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Hello push, välja för din Azure-behållaren i registret hello **Push en bild** åtgärd, ange hello **avbildningsnamn** som är inbyggd i hello föregående steg och välj **inkludera senaste tagg** .

4. När du har konfigurerat hello build och push steg för varje hello fem bilder, lägga till tre fler steg i hello skapa arbetsflödet.

   ![Visual Studio Team Services – Lägg till Kommandoradsaktivitet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *RegistryURL* förekomsten i filen för hello docker-compose.yml med hello RegistryURL variabel. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - uppdatering skriva filen med registret URL](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *AgentURL* förekomsten i filen för hello docker-compose.yml med hello AgentURL variabel.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. En uppgift som släpper hello uppdateras skriva filen eftersom en build-artefakt så den kan användas i hello-versionen. Se hello följande skärm för ytterligare information.

         ![Visual Studio Team Services - Publicera artefakt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - Publicera Skriv fil](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Klicka på **Spara & kö** tootest build-definition.

   ![Visual Studio Team-Services - spara och kön](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services - ny kö](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Om hello **skapa** är korrekt, du har toosee den här skärmen:

  ![Visual Studio Team Services - genereringen lyckades](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a>Steg 3: Skapa hello versionen definition

Visual Studio Team Services kan du för[hantera versioner i miljöer](https://www.visualstudio.com/team-services/release-management/). Du kan aktivera kontinuerlig distribution toomake till att programmet har distribuerats på dina olika miljöer (t.ex dev, testa, Förproduktion och produktion) i enkelt. Du kan skapa en miljö som representerar läge i Azure Container Service Docker Swarm-klustret.

![Visual Studio Team Services - versionen tooACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Första utgåvan installationen

1. toocreate en definition av versionen, klickar du på **versioner** > **+ släpper**

2. tooconfigure hello artefakt källa, klicka på **artefakter** > **länka en artefakt källa**. Här kan länka den här nya versionen definition toohello build-versionen som du definierade i hello föregående steg. Efter det är hello docker-compose.yml filen tillgänglig i hello release-processen.

    ![Visual Studio Team Services - versionen artefakter](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. tooconfigure hello versionen utlösaren klickar du på **utlösare** och välj **kontinuerlig distribution**. Ange hello utlösare på hello samma artefakt källa. Den här inställningen garanterar att en ny version startar när hello build har slutförts.

    ![Visual Studio Team Services - versionen utlösare](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. tooconfigure hello versionen variabler, klickar du på **variabler** och välj **+ variabeln** toocreate tre nya variabler med hello information av hello registerdata: **docker.username**, **docker.password**, och **docker.registry**. Klistra in hello värden för registernyckeln och klustret agenter DNS.

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Som visas i föregående hello-skärmen, klickar du på hello **Lås** kryssrutan i docker.password. Den här inställningen är viktiga toorestrict hello lösenord.
    >

### <a name="define-hello-release-workflow"></a>Definiera hello versionen arbetsflöde

hello versionen arbetsflöde består av två aktiviteter som du lägger till.

1. Konfigurera en aktivitet toosecurely kopiera hello utgöra filen tooa *distribuera* mapp på hello Docker Swarm huvudnoden, använder du tidigare konfigurerat hello SSH-anslutning. Se hello följande skärm för ytterligare information.
    
    Källmapp:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio Team Services - versionen SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Konfigurera en andra uppgiften tooexecute ett bash kommandot toorun `docker` och `docker stack deploy` kommandon på hello huvudnod. Se hello följande skärm för ytterligare information.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - versionen Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    hello-kommandot som kördes på hello master använder hello Docker CLI och hello Docker Compose CLI toodo hello följande uppgifter:

    - Logga in toohello Azure-behållaren registret (den använder tre build-variabler som definieras i hello **variabler** fliken)
    - Definiera hello **DOCKER_HOST** variabeln toowork med hello Swarm-slutpunkten (: 2375)
    - Navigera toohello *distribuera* mapp som har skapats av föregående aktivitet säker copy hello och som innehåller filen för hello docker-compose.yml 
    - Köra `docker stack deploy` kommandon som hämtar hello nya bilder och skapa hello behållare.

    >[!IMPORTANT]
    > Som det visas på föregående hello-skärmen, lämna hello **misslyckas på STDERR** kryssrutan avmarkerad. Den här inställningen gör att vi toocomplete hello release-processen på grund av för`docker-compose` flera diagnostiska meddelanden skrivs ut som behållare är stoppas eller tas bort på hello standardfel utdata. Om du markerar kryssrutan hello rapporterar Visual Studio Team Services att fel uppstod vid hello versionen, även om allt går bra.
    >
3. Spara den här nya versionen definitionen.

## <a name="step-4-test-hello-cicd-pipeline"></a>Steg 4: Testa hello CI/CD-pipeline

Nu när du är klar med hello konfiguration, det är tid tootest detta nya CI/CD-pipeline. Hej enklaste sättet tootest är det tooupdate hello källa kod och bekräfta hello ändras till GitHub-lagringsplatsen. Några sekunder efter att du överför hello kod, ser du en ny version körs i Visual Studio Team Services. När slutförts, utlöses en ny version och distribuera hello ny version av programmet hello på hello Azure Container Service-kluster.

## <a name="next-steps"></a>Nästa steg

* Mer information om CI/CD-skiva med Visual Studio Team Services finns hello [VSTS skapa översikt](https://www.visualstudio.com/docs/build/overview).
* Mer information om ACS-motorn finns hello [ACS-motorn GitHub-repo-](https://github.com/Azure/acs-engine).
* Mer information om Docker Swarm läge finns hello [Docker Swarm översikt över](https://docs.docker.com/engine/swarm/).
