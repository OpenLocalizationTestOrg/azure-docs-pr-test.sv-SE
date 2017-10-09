---
title: "aaaDeploy Vårversionen Start Web App på Linux i Azure Container Service | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig även om hello steg toodeploy en källan startprogrammet som en Linux-webbapp i Microsoft Azure."
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
ms.openlocfilehash: 2c44be1c7f66a38f48239001f0be9e90c7e6edef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a>Distribuera ett program med vår Start på Linux i hello Azure Container Service

Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

**[Docker]**  är öppen källkod som hjälper utvecklare att automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig genom med Docker toodevelop och distribuera en källan Start programmet tooa Linux-värd i hello [Azure Container Service (ACS)].

## <a name="prerequisites"></a>Krav

I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:

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

hello följande steg vägleder dig igenom hello steg som är nödvändiga toocreate ett enkelt källan Start-webbprogram och testa den lokalt.

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

1. Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Ändra katalogen toohello slutförts projektet. Exempel:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Skapa hello JAR-filen med Maven; Exempel:
   ```
   mvn package
   ```

1. När hello webbprogrammet har skapats kan du ändra directory toohello `target` katalogen där hello JAR-filen finns och starta hello webbprogram, till exempel:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare. Till exempel om du har konfigurerade curl som är tillgängliga och du hello Tomcat server toorun på port 80:
   ```
   curl http://localhost
   ```

1. Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande!**

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a>Skapa ett Azure Container registret toouse som ett privat Docker-register

hello följande steg beskriver hur du använder hello Azure portal toocreate registret en Azure-behållare.

> [!NOTE]
>
> Om du vill toouse hello Azure CLI i stället för hello Azure-portalen gör hello i [skapa en privat Docker behållare registret med hjälp av hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Bläddra toohello [Azure-portalen] och logga in.

   När du har loggat in tooyour konto på hello Azure-portalen kan du följa hello stegen i hello [skapa en privat Docker behållare registret med hjälp av hello Azure-portalen] artikel som omskrivning i följande steg för hello hello saké är lämpligt.

1. Hello menyn ikonen för **+ ny**, klicka på **behållare**, och klicka sedan på **Azure Container registret**.
   
   ![Skapa en ny Azure-behållare registernyckel][AR01]

1. När hello information för hello Azure Container registret mallen visas, klickar du på **skapa**. 

   ![Skapa en ny Azure-behållare registernyckel][AR02]

1. När hello **skapa behållaren registret** visas, ange din **registrets** och **resursgruppen**, Välj **aktivera** för Hej **administratörsanvändare**, och klicka sedan på **skapa**.

   ![Konfigurera registerinställningar för Azure-behållare][AR03]

1. När registret behållaren har skapats, navigera tooyour behållare registret i hello Azure-portalen och klicka sedan på **åtkomstnycklar**. Anteckna hello användarnamn och lösenord för hello nästa steg.

   ![Azure Container åtkomst registernycklar][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a>Konfigurera Maven toouse din Azure-behållare åtkomst registernycklar

1. Navigera toohello konfigurationskatalogen för Maven-installation och öppna hello *settings.xml* fil med en textredigerare.

1. Lägga till din Azure-behållare åtkomst registerinställningar från hello föregående avsnitt i den här självstudiekursen toohello `<servers>` samling i hello *settings.xml* filen, till exempel:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (till exempel ”:*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker / fullständig*”), och öppna hello *pom.xml* fil med en textredigerare.

1. Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret från hello föregående avsnitt i den här självstudiekursen, till exempel:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Uppdatera hello `<plugins>` samling i hello *pom.xml* filen så som hello `<plugin>` innehåller hello server adress och registret inloggningsnamn för ditt Azure-behållare registernyckeln från hello föregående avsnitt i den här kursen. Exempel:

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

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando toorebuild hello programmet hello och push hello behållaren tooyour registret för Azure-behållare:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> När du trycker tooAzure din Docker-behållaren får ett felmeddelande liknande tooone av hello följande även om din dockerbehållare har skapats:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Om det händer kan behöva du toosign i tooyour Azure-konto från hello Docker-kommandoraden. Exempel:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Du kan sedan överför din behållare från hello-kommandoraden. Exempel:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Skapa en webbapp i Linux på Azure App Service med behållaren avbildningen

1. Bläddra toohello [Azure-portalen] och logga in.

1. Hello menyn ikonen för **+ ny**, klicka på **webb + mobilt**, och klicka sedan på **webbprogrammet på Linux**.
   
   ![Skapa en ny webbapp i hello Azure-portalen][LX01]

1. När hello **webbprogrammet på Linux** visas, ange hello följande information:

   a. Ange ett unikt namn för hello **appnamn**, till exempel ”:*wingtiptoyslinux*”.

   b. Välj din **prenumeration** hello nedrullningsbara listan.

   c. Välj en befintlig **resursgruppen**, eller ange name-toocreate en ny resursgrupp.

   d. Klicka på **konfigurera behållaren** och ange hello följande information:

      * Välj **privata registret**.

      * **Avbildningen och valfria taggen**: Ange din behållarnamn från tidigare, till exempel ”:*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*”

      * **Serveradress**: Ange registret URL: en från tidigare, till exempel ”:*https://wingtiptoysregistry.azurecr.io*”

      * **Användarnamn för inloggning** och **lösenord**: Ange dina inloggningsuppgifter från din **åtkomstnycklar** som du använde i föregående steg.
   
   e. När du har angett alla hello ovan information klickar du på **OK**.

   ![Konfigurera inställningarna för webbappen][LX02]

1. Klicka på **Skapa**.

> [!NOTE]
>
> Azure kommer automatiskt att mappa begäranden tooembedded Tomcat Internetserver som körs på hello standardporten 80 eller 8080. Om du har konfigurerat dina inbäddade Tomcat server toorun på en anpassad port, måste du tooadd en miljö variabeln tooyour webbprogram som definierar hello port för inbäddade Tomcat-servern. toodo så Använd hello följande steg:
>
> 1. Bläddra toohello [Azure-portalen] och logga in.
> 
> 2. Klicka på ikonen hello för **Apptjänster**. (Se artikeln #1 i hello bilden nedan.)
>
> 3. Välj ditt webbprogram hello-listan. (Artikeln #2 i hello bilden nedan.)
>
> 4. Klicka på **programinställningar**. (Artikeln #3 i hello bilden nedan.)
>
> 5. I hello **appinställningar** lägger du till en ny miljövariabel med namnet **PORT** och ange din anpassade portnummer för hello värde. (Artikeln #4 i hello bilden nedan.)
>
> 6. Klicka på **Spara**. (Artikeln #5 i hello bilden nedan.)
>
> ![Om du sparar ett anpassat portnummer i hello Azure-portalen][LX03]
>

<!--
##  OPTIONAL: Configure hello embedded Tomcat server toorun on a different port

hello embedded Tomcat server in hello sample Spring Boot application is configured toorun on port 8080 by default. However, if you want toorun hello embedded Tomcat server toorun on a different port, such as port 80 for local testing, you can configure hello port by using hello following steps.

1. Go toohello *resources* directory (or create hello directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open hello *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify hello **server** setting so that hello server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close hello *application.yml* file.
-->

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:

* [Distribuera en källan startprogrammet toohello Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service](container-service-deploy-spring-boot-app-on-kubernetes.md)

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

Mer information om hello källan Start på Docker exempelprojektet finns [Vårversionen Start på Docker komma igång].

Hjälp med att komma igång med din egen källan startprogram, finns hello **Vårversionen Initializr** på https://start.spring.io/.

Mer information om att komma igång med att skapa ett enkelt källan Start-program finns i hello källan Initializr på https://start.spring.io/.

Ytterligare exempel för hur toouse anpassade Docker bilder med Azure finns [med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux].

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[skapa en privat Docker behållare registret med hjälp av hello Azure-portalen]: /azure/container-registry/container-registry-get-started-portal
[med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
