---
title: aaaCI/CD med Azure Container Service och Swarm | Microsoft Docs
description: "Använda Azure Container Service med Docker Swarm, ett Azure Container registret och Visual Studio Team Services toodeliver kontinuerligt ett flera behållare .NET Core-program"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: "Docker behållare Micro-tjänster, Swarm, Azure, Visual Studio Team Services DevOps"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med Docker Swarm med hjälp av Visual Studio Team Services

En av hello största utmaningarna när utveckla moderna applikationer för hello molnet kan toodeliver dessa program kontinuerligt. I den här artikeln lär du dig hur du skapar tooimplement en fullständig kontinuerlig integrering och distribution (CI/CD) pipeline med hjälp av Azure Container Service med Docker Swarm Azure Container registret och Visual Studio Team Services och versionshantering.

Den här artikeln är baserad på ett enkelt program tillgängliga på [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), utvecklade med ASP.NET Core. hello program består av fyra olika tjänster: tre webb-API: er och en frontwebb:

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

hello mål är toodeliver programmet kontinuerligt i ett Docker Swarm-kluster med hjälp av Visual Studio Team Services. hello följande figur information denna kontinuerlig leverans pipeline:

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Här följer en kort beskrivning av hello steg:

1. Koden ändringarna har bekräftats toohello källdatabasen kod (här GitHub) 
2. GitHub utlöser en version i Visual Studio Team Services 
3. Visual Studio Team Services hämtar hello senaste versionen av hello källor och skapar alla hello-avbildningar som ska utgöra hello program 
4. Visual Studio Team Services skickar varje avbildning tooa Docker-registret som skapats med hjälp av hello Azure Container Registry-tjänsten 
5. Visual Studio Team Services utlöser en ny version 
6. hello körs vissa kommandon med hjälp av SSH på hello Azure container service master klusternod 
7. Docker Swarm på hello klustret hämtar hello senaste versionen av hello bilder 
8. hello ny version av programmet hello distribueras med Docker Compose 

## <a name="prerequisites"></a>Krav

Innan du påbörjar de här självstudierna måste toocomplete hello följande uppgifter:

- [Skapa ett Swarm-kluster i Azure Container Service](container-service-deployment.md)
- [Ansluta med hello Swarm-kluster i Azure Container Service](../container-service-connect.md)
- [Skapa en Azure-behållaren registernyckel](../../container-registry/container-registry-get-started-portal.md)
- [Har en Visual Studio Team Services-konto och team projekt](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Duplicera hello GitHub-lagringsplatsen tooyour GitHub-konto](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Du måste också en Ubuntu (14.04 eller 16.04)-dator med Docker installerad. Den här datorn används av Visual Studio Team Services under hello bygga och släpper processer. Enkelriktade toocreate den här datorn är toouse hello avbildning tillgänglig i hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Steg 1: Konfigurera Visual Studio Team Services-konto 

I det här avsnittet kan du konfigurera Visual Studio Team Services-konto.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Konfigurera en Visual Studio Team Services Linux build-agent

toocreate Docker-bilder och push dessa avbildningar till en Azure-behållaren registret från en Visual Studio Team Services skapa måste tooregister en Linux-agenten. Du har dessa installationsalternativ:

* [Distribuera en agent på Linux](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Använda Docker toorun hello VSTS agent](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a>Installera hello Docker integrering VSTS tillägg

Microsoft tillhandahåller en VSTS tillägget toowork med Docker i version och utgåva processer. Det här tillägget är tillgängligt i hello [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Klicka på **installera** tooadd tillägget tooyour VSTS kontot:

![Installera hello Docker-integrering](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Du uppmanas tooconnect tooyour VSTS konto med hjälp av dina autentiseringsuppgifter. 

### <a name="connect-visual-studio-team-services-and-github"></a>Ansluta Visual Studio Team Services och GitHub

Upprätta en anslutning mellan projektet VSTS och GitHub-konto.

1. I ditt projekt i Visual Studio Team Services, klickar du på hello **inställningar** ikon i hello verktygsfältet och välj **Services**.

    ![Visual Studio Team Services - extern anslutning](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Klicka på vänster hello **nya tjänstslutpunkten** > **GitHub**.

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. tooauthorize VSTS toowork med ditt GitHub-konto, klickar du på **auktorisera** och följa hello i hello-fönster som öppnas.

    ![Visual Studio Team Services - auktorisera GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a>Ansluta VSTS tooyour Azure-behållaren registret och Azure Container Service-kluster

hello sista stegen innan du hämtar till hello CI/CD-pipeline är tooconfigure externa anslutningar tooyour behållare registret och din Docker Swarm-kluster i Azure. 

1. I hello **Services** inställningarna för ditt projekt i Visual Studio Team Services, lägga till en tjänstslutpunkt av typen **Docker registret**. 

2. Ange hello URL och hello autentiseringsuppgifter registret Azure-behållaren i hello popup-fönstret som öppnas.

    ![Visual Studio Team Services - Docker-registret](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Lägga till en slutpunkt av typen för Docker Swarm-kluster hello **SSH**. Ange hello SSH-anslutningsinformationen för Swarm-klustret.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Alla hello-konfigurationen är nu klar. Skapa hello CI/CD-pipeline som skapar och distribuerar hello programmet toohello Docker Swarm-kluster i hello nästa steg. 

## <a name="step-2-create-hello-build-definition"></a>Steg 2: Skapa hello build-definition

I det här steget du ställer in en build definitionfor VSTS-projektet och definiera hello build arbetsflöde för bilderna behållare

### <a name="initial-definition-setup"></a>Inledande definition installationen

1. toocreate en build-definition ansluta tooyour Visual Studio Team Services projektet och klicka på **Skapa & släpper**. 

2. I hello **skapa definitioner** klickar du på **+ ny**. Välj hello **tom** mall.

    ![Visual Studio Team Services - nya skapa Definition](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. Konfigurera hello ny version med en källa för GitHub-lagringsplatsen, kontrollera **kontinuerlig integration**, och välj hello agent kö där du har registrerat din Linux-agenten. Klicka på **skapa** toocreate hello skapa definition.

    ![Visual Studio Team Services - skapa Build-Definition](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. På hello **skapa definitioner** sidan, först öppna hello **databasen** och konfigurera hello build toouse hello förgrening av hello MyShop projekt som du skapade i hello krav. Kontrollera att du väljer *acs-dokumenten* som hello **standardförgrening**.

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. På hello **utlösare** konfigurerar hello build toobe Utlöses efter varje genomförande. Välj **kontinuerlig integration** och **Batch ändringar**.

    ![Visual Studio Team Services - konfiguration av Build-utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a>Definiera hello build-arbetsflöde
Nästa steg i hello definiera hello build-arbetsflöde. Det finns fem behållaren bilder toobuild för hello *MyShop* program. Varje avbildning skapas med hello Dockerfile finns i hello projektet mappar:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Du behöver tooadd två Docker steg för varje avbildning, en toobuild hello bild och en toopush hello bild i registret för hello Azure-behållaren. 

1. tooadd ett steg i hello build-arbetsflöde, klickar du på **+ Lägg till build steg** och välj **Docker**.

    ![Visual Studio Team Services – Lägg till Build steg](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. För varje bild och konfigurera ett steg som använder hello `docker build` kommando.

    ![Visual Studio Team Services - Docker skapa](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Skapa åtgärden för hello, Välj Azure-behållaren registret, hello **skapa en avbildning** åtgärd och hello Dockerfile som definierar varje avbildning. Ange hello **Build-kontexten** hello Dockerfile rotkatalogen och definiera hello **avbildningsnamn**. 
    
    Börja hello avbildningsnamn med hello URI för Azure-behållaren registret på hello föregående skärm visas. (Du kan också använda en build variabeln tooparameterize hello taggen för hello-avbildning, till exempel hello build-ID i det här exemplet.)

3. För varje bild och konfigurera ett andra steg som använder hello `docker push` kommando.

    ![Visual Studio Team Services - Docker Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Hello push, välja för din Azure-behållaren i registret hello **Push en bild** åtgärd, och ange hello **avbildningsnamn** som är inbyggd i hello föregående steg.

4. När du har konfigurerat hello build och push steg för varje hello fem bilder, lägga till två fler steg i hello skapa arbetsflödet.

    a. En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *BuildNumber* förekomsten i hello docker-compose.yml fil med hello aktuella skapa Id. Se hello följande skärm för ytterligare information.

    ![Visual Studio Team Services - uppdatering Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. En uppgift som släpper hello uppdateras skriva filen eftersom en build-artefakt så den kan användas i hello-versionen. Se hello följande skärm för ytterligare information.

    ![Visual Studio Team Services - Publicera Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Klicka på **spara** och namnet build-definition.

## <a name="step-3-create-hello-release-definition"></a>Steg 3: Skapa hello versionen definition

Visual Studio Team Services kan du för[hantera versioner i miljöer](https://www.visualstudio.com/team-services/release-management/). Du kan aktivera kontinuerlig distribution toomake till att programmet har distribuerats på dina olika miljöer (t.ex dev, testa, Förproduktion och produktion) i enkelt. Du kan skapa en ny miljö som representerar dina Azure Container Service Docker Swarm-kluster.

![Visual Studio Team Services - versionen tooACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Första utgåvan installationen

1. toocreate en definition av versionen, klickar du på **versioner** > **+ släpper**

2. tooconfigure hello artefakt källa, klicka på **artefakter** > **länka en artefakt källa**. Här kan länka den här nya versionen definition toohello build-versionen som du definierade i hello föregående steg. Hello docker-compose.yml filen är tillgänglig i hello versionen processen genom att göra.

    ![Visual Studio Team Services - versionen artefakter](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. tooconfigure hello versionen utlösaren klickar du på **utlösare** och välj **kontinuerlig distribution**. Ange hello utlösare på hello samma artefakt källa. Den här inställningen garanterar att en ny version startar så fort hello build har slutförts.

    ![Visual Studio Team Services - versionen utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a>Definiera hello versionen arbetsflöde

hello versionen arbetsflöde består av två aktiviteter som du lägger till.

1. Konfigurera en aktivitet toosecurely kopiera hello utgöra filen tooa *distribuera* mapp på hello Docker Swarm huvudnoden, använder du tidigare konfigurerat hello SSH-anslutning. Se hello följande skärm för ytterligare information.

    ![Visual Studio Team Services - versionen SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Konfigurera en andra uppgiften tooexecute ett bash kommandot toorun `docker` och `docker-compose` kommandon på hello huvudnod. Se hello följande skärm för ytterligare information.

    ![Visual Studio Team Services - versionen Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    hello-kommandot utförs på hello master Använd hello Docker CLI och hello Docker Compose CLI toodo hello följande uppgifter:

    - Inloggningen toohello Azure-behållaren registret (den använder tre build variab'les som definieras i hello **variabler** fliken)
    - Definiera hello **DOCKER_HOST** variabeln toowork med hello Swarm-slutpunkten (: 2375)
    - Navigera toohello *distribuera* mapp som har skapats av föregående aktivitet säker copy hello och som innehåller filen för hello docker-compose.yml 
    - Köra `docker-compose` kommandon som hämtar hello nya bilder, stoppa hello tjänster, ta bort hello services och skapa hello behållare.

    >[!IMPORTANT]
    > Som det visas på hello föregående skärm, lämna hello **misslyckas på STDERR** kryssrutan avmarkerad. Detta är ett viktigt, eftersom `docker-compose` flera diagnostiska meddelanden skrivs ut som behållare är stoppas eller tas bort på hello standardfel utdata. Om du markerar kryssrutan hello rapporterar Visual Studio Team Services att fel uppstod vid hello versionen, även om allt går bra.
    >
3. Spara den här nya versionen definitionen.


>[!NOTE]
>Den här distributionen innehåller vissa driftstopp eftersom vi stoppa hello gamla tjänster och kör hello ny. Det är möjligt tooavoid detta genom att göra en blå-grön-distribution.
>

## <a name="step-4-test-hello-cicd-pipeline"></a>Steg 4. Testa hello CI/CD-pipeline

Nu när du är klar med hello konfiguration, det är tid tootest detta nya CI/CD-pipeline. Hej enklaste sättet tootest är det tooupdate hello källa kod och bekräfta hello ändras till GitHub-lagringsplatsen. Några sekunder efter att du överför hello kod, ser du en ny version körs i Visual Studio Team Services. När har slutförts startas en ny version och distribuerar hello ny version av programmet hello på hello Azure Container Service-kluster.

## <a name="next-steps"></a>Nästa steg

* Mer information om CI/CD-skiva med Visual Studio Team Services finns hello [VSTS skapa översikt](https://www.visualstudio.com/docs/build/overview).
