---
title: "aaaHow toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en källan Start-app i Azure Container registret tooAzure Apptjänst"
description: "Den här kursen får du om hello steg toodeploy en källan startprogrammet i Azure Container registret tooAzure tooAzure Apptjänst med hjälp av ett Maven plugin-program."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a>Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en källan Start-app i Azure Container registret tooAzure Apptjänst

Hej ** [Vårversionen Framework] ** ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program. Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att använda vår tooget igång snabbt.

Den här artikeln visar hur toodeploy en exempel källan Start programmet tooAzure behållare registret, och sedan använda hello Maven-plugin-programmet för Azure Web Apps toodeploy ditt program tooAzure Apptjänst.

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
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
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

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-service-principal"></a>Skapa en Azure huvudnamn för tjänsten

I det här avsnittet skapar du en Azure tjänstens huvudnamn som hello Maven plugin-programmet använder när du distribuerar behållare-tooAzure.

1. Öppna en kommandotolk.

1. Logga in på ditt Azure-konto med hjälp av hello Azure CLI:
   ```azurecli
   az login
   ```
   Följ hello instruktioner toocomplete hello inloggningsprocessen.

1. Skapa en Azure-tjänstens huvudnamn:
   ```azurecli
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

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Skapa ett Azure Container registret med hjälp av hello Azure CLI

1. Öppna en kommandotolk.

1. Logga in tooyour Azure-konto:
   ```azurecli
   az login
   ```

1. Skapa en resursgrupp för hello Azure-resurser du vill använda i den här artikeln:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Ersätt `wingtiptoysresources` i det här exemplet med ett unikt namn för resursgruppen.

1. Skapa ett register för privat Azure-behållaren i hello resursgruppen för vår Start appen: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Ersätt `wingtiptoysregistry` i det här exemplet med ett unikt namn för behållaren registret.

1. Hämta hello lösenord för behållaren registret:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure kommer att svara med ditt lösenord. Exempel:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a>Lägg till din Azure-behållaren registret och Azure service principal tooyour Maven-inställningar

1. Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som hello följande exempel:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Lägga till din Azure-behållare åtkomst registerinställningar från hello föregående avsnitt i den här artikeln toohello `<servers>` samling i hello *settings.xml* filen, till exempel:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Där:
   Element | Beskrivning
   ---|---|---
   `<id>` | Innehåller hello namnet på behållaren för privat Azure-registret.
   `<username>` | Innehåller hello namnet på behållaren för privat Azure-registret.
   `<password>` | Innehåller hello-lösenord som du hämtade i hello föregående avsnitt i den här artikeln.

1. Lägg till Azure-tjänstens huvudnamn inställningarna från ett tidigare avsnitt i den här artikeln toohello `<servers>` samling i hello *settings.xml* filen, till exempel:

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

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a>Skapa din Docker behållaren image och skicka tooyour Azure-behållaren registret

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (t.ex.) ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker/complete*”), och öppna hello *pom.xml* filen med en textredigerare.

1. Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret från hello föregående avsnitt i den här självstudiekursen, till exempel:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Där:
   Element | Beskrivning
   ---|---|---
   `<azure.containerRegistry>` | Anger hello för privat Azure-behållaren registret.
   `<docker.image.prefix>` | Anger hello URL för privat Azure-behållaren registret, som hämtas genom att lägga till ”. azurecr.io” toohello namnet på din privata behållare registret.

1. Kontrollera att `<plugin>` för hello Docker-plugin-programmet i din *pom.xml* filen innehåller hello rätt egenskaper för hello server adress och registret inloggningsnamn hello föregående steg i den här självstudiekursen. Exempel:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Där:
   Element | Beskrivning
   ---|---|---
   `<serverId>` | Anger hello-egenskap som innehåller namnet på behållaren för privat Azure-registret.
   `<registryUrl>` | Anger hello-egenskap som innehåller hello URL för privat Azure-behållaren registret.

1. Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando toorebuild hello programmet hello och push hello behållaren tooyour Azure-behållaren registret:

   ```
   mvn package docker:build -DpushImage 
   ```

1. Valfritt: Bläddra toohello [Azure-portalen] och kontrollera att det finns Docker behållare bild med namnet **gs-källan-Start-docker** i behållaren-registret.

   ![Kontrollera behållare i Azure-portalen][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a>Anpassa din pom.xml och sedan skapa och distribuera behållaren-tooAzure

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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
`<resourceGroup>` | Anger hello målresursgruppen, vilket är `wingtiptoysresources` i det här exemplet. hello resursgruppen kommer att skapas under distributionen om det inte redan finns.
`<appName>` | Anger hello målets namn för ditt webbprogram. I det här exemplet är hello målnamnet `maven-linux-app-${maven.build.timestamp}`, där hello `${maven.build.timestamp}` suffix läggs till i det här exemplet tooavoid konflikt. (hello tidsstämpeln är valfria och du kan ange en unik sträng för hello programnamn.)
`<region>` | Anger hello målregionen, som i det här exemplet är `westus`. (En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)
`<containerSettings>` | Anger egenskaperna för hello som innehåller hello namn och URL för din behållare.
`<appSettings>` | Anger alla unika inställningar för Maven toouse när du distribuerar din web app tooAzure. I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger hello port för din app.

> [!NOTE]
>
> portnummer för hello inställningar toochange hello i det här exemplet krävs bara när du ändrar hello port hello standardprincipen.
>

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

## <a name="next-steps"></a>Nästa steg

Mer information om hello finns olika tekniker som beskrivs i den här artikeln hello följande artiklar:

* [Maven-plugin-program för Azure-Webbappar]

* [Logga in tooAzure från hello Azure CLI](/azure/xplat-cli-connect)

* [Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referens för maven](https://maven.apache.org/settings.html)

* [Docker-plugin för Maven]

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure Portal]: https://portal.azure.com/
[Maven-plugin-program för Azure-Webbappar]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven 3.]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Källan Start]: http://projects.spring.io/spring-boot/
[Källan Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Källan Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
