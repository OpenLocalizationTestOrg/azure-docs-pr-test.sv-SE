---
title: "aaaDeploy en App för start av värdet på Kubernetes i Azure Container Service | Microsoft Docs"
description: "Den här kursen får du om hello steg toodeploy en källan startprogrammet i ett kluster med Kubernetes i Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a>Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service

Hej  **[Vårversionen Framework]**  ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program. Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att använda vår tooget igång snabbt.

**[Kubernetes]**  och  **[Docker]**  är öppen källkod som hjälper utvecklare att automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig om att kombinera dessa två populära, öppen källkod tekniker toodevelop och distribuera en källan Start programmet tooMicrosoft Azure. Mer specifikt du använder  *[Vårversionen Start]*  för programutveckling,  *[Kubernetes]*  för behållaren distribution och hello [Azure Container Service (ACS)] toohost ditt program.

### <a name="prerequisites"></a>Krav

* En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].
* Hej [Azure-kommandoradsgränssnittet (CLI)].
* En uppdaterad [Java Developer Kit (JDK)].
* Apache's [Maven] skapa verktyget (Version 3).
* En [Git] klienten.
* En [Docker] klienten.

> [!NOTE]
>
> På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a>Skapa hello källan Start på Docker komma igång webbprogram

hello följande steg beskriver hur du skapar ett webbprogram i vår start och testa dem lokalt.

1. Öppna en kommandotolk och skapa en lokal katalog toohold programmet och ändra toothat katalog. Exempel:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- eller--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello katalog.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Ändra katalogen toohello slutförts projektet.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Använd Maven toobuild och kör hello sample-appen.
   ```
   mvn package spring-boot:run
   ```

1. Testa hello-webbprogram genom att bläddra toohttp://localhost:8080 eller med hello följande `curl` kommando:
   ```
   curl http://localhost:8080
   ```

1. Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande**

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Skapa ett Azure Container registret med hjälp av hello Azure CLI

1. Öppna en kommandotolk.

1. Logga in tooyour Azure-konto:
   ```azurecli
   az login
   ```

1. Skapa en resursgrupp för hello Azure-resurser används i den här kursen.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Skapa ett register för privat Azure-behållaren i hello resursgruppen. hello självstudiekursen skickar hello sample-appen som Docker bild toothis register i senare steg. Ersätt `wingtiptoysregistry` med ett unikt namn för registret.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a>Push-app toohello behållare registret

1. Navigera toohello konfigurationskatalogen för Maven-installationen (standard ~/.m2/ eller C:\Users\username\.m2) och öppna hello *settings.xml* fil med en textredigerare.

1. Hämta hello lösenordet för behållaren registret från hello Azure CLI.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Lägg till din Azure-behållare register-id och lösenord tooa nya `<server>` samling i hello *settings.xml* fil.
Hej `id` och `username` är hello namnet på hello registret. Använd hello `password` värde från hello föregående kommando (utan citattecken).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (till exempel ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker / fullständig*”), och öppna hello *pom.xml* fil med en textredigerare.

1. Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Uppdatera hello `<plugins>` samling i hello *pom.xml* filen så som hello `<plugin>` innehåller hello server adress och registret inloggningsnamn för ditt Azure-behållare registret.

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör hello efter kommandot toobuild hello Docker-behållaren och push hello avbildningen toohello registret:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Du får ett felmeddelande liknande tooone av hello följande när Maven skickar hello avbildningen tooAzure:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Om du får det här felet kan logga in tooAzure från hello Docker-kommandoraden.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Sedan push din behållare:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a>Skapa ett Kubernetes kluster på ACS med hello Azure CLI

1. Skapa ett Kubernetes-kluster i Azure Container Service. hello följande kommando skapar en *kubernetes* klustret i hello *VingspetsLeksaker kubernetes* resursen med *VingspetsLeksaker containerservice* som hello kluster namn och *VingspetsLeksaker kubernetes* som hello DNS-prefix:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Det här kommandot kan ta en stund toocomplete.

1. Installera `kubectl` med hello Azure CLI. Linux-användare kan ha tooprefix det här kommandot med `sudo` eftersom den distribuerar hello Kubernetes CLI för`/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Hämta konfigurationsinformation för hello klustret så att du kan hantera klustret från hello Kubernetes webbgränssnitt och `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a>Distribuera hello avbildningen tooyour Kubernetes kluster

Den här kursen distribuerar hello app med hjälp av `kubectl`, sedan kan du tooexplore hello distribution via hello Kubernetes webbgränssnitt.

### <a name="deploy-with-hello-kubernetes-web-interface"></a>Distribuera med hello Kubernetes webbgränssnitt

1. Öppna en kommandotolk.

1. Öppna hello configuration webbplats för klustret Kubernetes i standardwebbläsaren:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. När hello Kubernetes configuration webbplats öppnas i webbläsaren, klickar du på länken hello för**distribuera en behållare app**:

   ![Kubernetes Configuration webbplats][KB01]

1. När hello **distribuera en behållare app** visas, ange hello följande alternativ:

   a. Välj **ange information om appar nedan**.

   b. Ange programnamnet källan Start för hello **appnamn**, till exempel ”:*gs-källan-Start-docker*”.

   c. Ange inloggningen server och en behållare avbildningen från tidigare för hello **behållaren image**, till exempel ”:*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*”.

   d. Välj **externa** för hello **tjänsten**.

   e. Ange din externa och interna portar i hello **Port** och **mål port** textrutor.

   ![Kubernetes Configuration webbplats][KB02]


1. Klicka på **distribuera** toodeploy hello behållare.

   ![Distribuera behållare][KB05]

1. När programmet har distribuerats visas källan Start programmet visas under **Services**.

   ![Kubernetes tjänster][KB06]

1. Om du klickar på länken hello **externa slutpunkter**, du kan se din källan Start-program som körs på Azure.

   ![Kubernetes tjänster][KB07]

   ![Bläddra Sample-appen på Azure][SB02]


### <a name="deploy-with-kubectl"></a>Distribuera med kubectl

1. Öppna en kommandotolk.

1. Kör ditt behållare i hello Kubernetes kluster genom att använda hello `kubectl run` kommando. Ge en tjänst för din app i Kubernetes och hello fullständiga namn. Exempel:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   I det här kommandot:

   * hello behållarnamn `gs-spring-boot-docker` anges omedelbart efter hello `run` kommando

   * Hej `--image` parametern anger hello kombineras inloggningsserver och Avbildningsnamnet som`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Exponera Kubernetes klustret externt med hjälp av hello `kubectl expose` kommando. Ange tjänstnamnet, hello offentliga TCP-port som används tooaccess hello appen och hello interna målporten appen lyssnar på. Exempel:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   I det här kommandot:

   * hello behållarnamn `gs-spring-boot-docker` anges omedelbart efter hello `expose deployment` kommando

   * Hej `--type` parametern anger hello klustret använder belastningsutjämnare

   * Hej `--port` parametern anger hello offentliga TCP-port 80. Du har åtkomst till hello app på denna port.

   * Hej `--target-port` parametern anger hello interna TCP-port för 8080. hello belastningsutjämning vidarebefordrar begäranden tooyour app på denna port.

1. När hello appen har distribuerats toohello klustret fråga hello extern IP-adress och öppna den i webbläsaren:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Bläddra Sample-appen på Azure][SB02]


## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder värdet Start på Azure finns i hello följande artiklar:

* [Distribuera en källan startprogrammet toohello Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Distribuera ett program med vår Start på Linux i hello Azure Container Service](container-service-deploy-spring-boot-app-on-linux.md)

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

Mer information om hello källan Start på Docker exempelprojektet finns [Vårversionen Start på Docker komma igång].

hello följande länkar innehåller ytterligare information om hur du skapar källan startprogram:

* Mer information om hur du skapar ett enkelt källan Start-program finns i hello källan Initializr på https://start.spring.io/.

hello innehåller följande länkar ytterligare information om hur du använder Kubernetes med Azure:

* [Kom igång med ett Kubernetes kluster i Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Med hjälp av hello webbgränssnitt Kubernetes med Azure Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Mer information om hur du använder Kubernetes kommandoradsgränssnittet är tillgänglig i hello **kubectl** användarhandboken på <https://kubernetes.io/docs/user-guide/kubectl/>.

Hej Kubernetes webbplats har flera artiklar som handlar om med bilder i privata register:

* [Konfigurera Service-konton för skida]
* [Namnområden]
* [Dra en bild från ett privat register]

Ytterligare exempel för hur toouse anpassade Docker bilder med Azure finns [med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux].

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Vårversionen Framework]: https://spring.io/
[Konfigurera Service-konton för skida]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Namnområden]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Dra en bild från ett privat register]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
