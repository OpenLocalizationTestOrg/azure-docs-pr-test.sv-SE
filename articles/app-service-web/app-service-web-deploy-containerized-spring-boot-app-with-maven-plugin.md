---
title: "aaaHow toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en av vår Start app tooAzure"
description: "Lär dig hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: e7e760d4ef5bd4c92a4126a50a2b12e5c8f2b4a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a>Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en av vår Start app tooAzure

Hej [Maven plugin-program för Azure-Webbappar](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) för [Apache Maven](http://maven.apache.org/) ger sömlös integration av Azure App Service till Maven-projekt och förenklar hello process för utvecklare toodeploy web apps tooAzure Apptjänst.

Den här artikeln visas hur du använder hello Maven-plugin-programmet för Azure Web Apps toodeploy ett exempelprogram källan Start i en Docker-behållare tooAzure Apptjänster.

> [!NOTE]
>
> Hej Maven plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning. För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som planeras att hello framtida.
>

## <a name="prerequisites"></a>Krav

I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:

* En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].
* Hej [Azure-kommandoradsgränssnittet (CLI)].
* En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.
* Apache's [Maven] skapa verktyget (Version 3).
* En [Git] klienten.
* En [Docker] klienten.

> [!NOTE]
>
> På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a>Klona hello exempel källan Start på Docker-webbprogram

I det här avsnittet klona en container startprogrammet för källan och testa den lokalt.

1. Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog toohold källan Start programmet och ändra toothat katalog. Exempel:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- eller--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. Ändra katalogen toohello slutförts projektet. Exempel:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Skapa hello JAR-filen med Maven; Exempel:
   ```shell
   mvn clean package
   ```

1. När hello webbprogrammet har skapats, starta hello webbprogrammet med Maven; Exempel:
   ```shell
   mvn spring-boot:run
   ```

1. Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare. Du kan till exempel använda följande kommando om du har curl tillgängliga hello:
   ```shell
   curl http://localhost:8080
   ```

1. Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande**

## <a name="create-an-azure-service-principal"></a>Skapa en Azure huvudnamn för tjänsten

I det här avsnittet skapar du en Azure tjänstens huvudnamn som hello Maven plugin-programmet använder när du distribuerar behållare-tooAzure.

1. Öppna en kommandotolk.

1. Logga in på ditt Azure-konto med hjälp av hello Azure CLI:
   ```shell
   az login
   ```
   Följ hello instruktioner toocomplete hello inloggningsprocessen.

1. Skapa en Azure-tjänstens huvudnamn:
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Där `uuuuuuuu` är hello användarnamn och `pppppppp` är hello lösenord för hello tjänstens huvudnamn.

1. Azure svarar med JSON som liknar följande exempel hello:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > Du använder hello värden från den här JSON-svar när du konfigurerar hello Maven plugin-programmet toodeploy behållaren-tooAzure. hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` platshållare för värden som används i det här exemplet toomake den enklare toomap värden tootheir respektive elementen när du konfigurerar din Maven `settings.xml` filen i hello nästa avsnittet.
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a>Konfigurera Maven toouse din Azure-tjänstens huvudnamn

I det här avsnittet använder du hello värden från Azure-tjänstens huvudnamn tooconfigure hello autentiseringen som Maven ska använda när du distribuerar behållare-tooAzure.

1. Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som hello följande exempel:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Lägg till Azure-tjänstens huvudnamn inställningarna från hello föregående avsnitt i den här självstudiekursen toohello `<servers>` samling i hello *settings.xml* filen, till exempel:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Där:
   Element | Beskrivning
   ---|---|---
   `<id>` | Anger ett unikt namn som Maven använder toolook in dina säkerhetsinställningar när du distribuerar din web app tooAzure.
   `<client>` | Innehåller hello `appId` värde från tjänstens huvudnamn.
   `<tenant>` | Innehåller hello `tenant` värde från tjänstens huvudnamn.
   `<key>` | Innehåller hello `password` värde från tjänstens huvudnamn.
   `<environment>` | Definierar hello Azure-molnet målmiljön, vilket är `AZURE` i det här exemplet. (En fullständig lista över miljöer finns i hello [Maven plugin-program för Azure Web Apps] dokumentation)

1. Spara och Stäng hello *settings.xml* fil.

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a>Valfritt: Distribuera din lokala Docker filen tooDocker Hub

Om du har en Docker-konto du push-installera den tooDocker hubb och skapa din Docker behållaren image lokalt. toodo så Använd hello följande steg.

1. Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare.

1. Leta upp hello `<imageName>` underordnat element för hello `<containerSettings>` element.

1. Uppdatera hello `${docker.image.prefix}` värdet med namnet på ditt Docker:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Välj något av följande distributionsmetoder hello:

   * Skapa behållaren avbildningen lokalt med Maven, och sedan använda Docker toopush din behållaren tooDocker hubb:
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * Om du har hello [Docker-plugin för Maven] installerat, kan du automatiskt bygga och din behållaren image tooDocker hubb med hjälp av hello `-DpushImage` parameter:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a>Valfritt: Anpassa din pom.xml innan du distribuerar behållare-tooAzure

Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta sedan upp hello `<plugin>` element för `azure-webapp-maven-plugin`. Det här elementet bör likna följande exempel hello:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Det finns flera värden som du kan ändra för hello Maven plugin-program och en utförlig beskrivning för var och en av dessa element är tillgänglig i hello [Maven plugin-program för Azure Web Apps] dokumentation. Som som säger att det finns flera värden som är värda markering i den här artikeln:

Element | Beskrivning
---|---|---
`<version>` | Anger hello version av hello [Maven plugin-program för Azure Web Apps]. Du bör kontrollera hello-version som anges i hello [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure som du använder hello senaste versionen.
`<authentication>` | Anger hello autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven använder den värdet toolook hello Azure-tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.
`<resourceGroup>` | Anger hello målresursgruppen, vilket är `maven-plugin` i det här exemplet. hello resursgruppen kommer att skapas under distributionen om det inte redan finns.
`<appName>` | Anger hello målets namn för ditt webbprogram. I det här exemplet är hello målnamnet `maven-linux-app-${maven.build.timestamp}`, där hello `${maven.build.timestamp}` suffix läggs till i det här exemplet tooavoid konflikt. (hello tidsstämpeln är valfria och du kan ange en unik sträng för hello programnamn.)
`<region>` | Anger hello målregionen, som i det här exemplet är `westus`. (En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)
`<appSettings>` | Anger alla unika inställningar för Maven toouse när du distribuerar din web app tooAzure. I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger hello port för din app.

> [!NOTE]
>
> portnummer för hello inställningar toochange hello i det här exemplet krävs bara när du ändrar hello port hello standardprincipen.
>

## <a name="build-and-deploy-your-container-tooazure"></a>Skapa och distribuera behållaren-tooAzure

När du har konfigurerat alla hello inställningar i föregående avsnitt i den här artikeln hello, är du redo toodeploy behållaren-tooAzure. toodo så Använd hello följande steg:

1. Från Kommandotolken hello eller terminalfönster som du använde tidigare återskapa hello JAR-filen med Maven om du har gjort några ändringar toohello *pom.xml* filen, till exempel:
   ```shell
   mvn clean package
   ```

1. Distribuera din web app tooAzure med Maven; Exempel:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven distribuerar din web app tooAzure; Om hello webbapp inte redan finns, skapas.

> [!NOTE]
>
> Om hello region som du anger i hello `<region>` element i din *pom.xml* filen har inte tillräckligt många servrar som är tillgängliga när du börjar din distribution, visas ett felmeddelande liknande toohello följande exempel:
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> Om det händer kan ange du en annan region och kör igen hello Maven-kommando toodeploy ditt program.
>
>

När webbplatsen har distribuerats, kommer du att kunna toomanage den med hjälp av hello [Azure-portalen].

* Ditt webbprogram visas i **Apptjänster**:

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* Och hello URL för ditt webbprogram visas i hello **översikt** för ditt webbprogram:

   ![När du fastställer hello URL: en för webbappen][AP02]

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

Mer information om hello finns olika tekniker som beskrivs i den här artikeln hello följande artiklar:

* [Maven plugin-program för Azure Web Apps]

* [Logga in tooAzure från hello Azure CLI](/azure/xplat-cli-connect)

* [Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure Apptjänst](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referens för maven](https://maven.apache.org/settings.html)

* [Docker-plugin för Maven]

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
