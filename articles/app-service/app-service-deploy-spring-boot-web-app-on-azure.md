---
title: "aaaDeploy källan startprogrammet-toohello Azure App Service | Microsoft Docs"
description: "Den här kursen hjälper utvecklare via hello steg toodeploy hello källan Start komma igång web app tooAzure Apptjänst."
services: app-service\web
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
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a>Distribuera en källan startprogrammet toohello Azure App Service

Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper Java-utvecklare som skapar program på företagsnivå och en hello mer populära projekt som bygger på den plattformen är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

Den här kursen får du om att skapa hello källan Start komma igång exempelwebbapp och distribuera den för[Azure App Service].

### <a name="prerequisites"></a>Krav

I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande:

* En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].
* En uppdaterad [Java Developer Kit (JDK)].
* Apache's [Maven] skapa verktyget (Version 3).
* En [Git] klienten.

## <a name="create-hello-spring-boot-getting-started-web-app"></a>Skapa hello källan Start komma igång-webbprogram

hello vägleder följande steg dig igenom hello steg som är nödvändiga toocreate ett enkelt källan Start-webbprogram och testa den lokalt.

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

1. Klona hello [Vårversionen Start komma igång] exempelprojektet i hello-katalog som du just skapat, till exempel:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Ändra katalogen toohello slutförts projektet. Exempel:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Skapa hello JAR-filen med Maven; Exempel:
   ```
   mvn package
   ```

1. När hello webbprogrammet har skapats, ändra directory toohello JAR-filen och starta hello webbprogrammet; Exempel:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Testa hello webbprogram genom att bläddra toohttp://localhost:8080 i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:
   ```
   curl http://localhost:8080
   ```

1. Du bör se hello följande meddelande visas: **helg från källan Start!**

   ![Bläddra Sample-appen][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Skapa en Azure-webbapp för användning med Java

hello följande steg vägleder dig genom hello steg toocreate en Azure-Webbapp, konfigurera inställningar för hello som krävs för Java och konfigurera FTP-autentiseringsuppgifter.

1. Bläddra toohello [Azure-portalen] och logga in.

1. När du har loggat in på ditt konto på hello Azure-portalen klickar du på hello menyn ikonen för **Apptjänster**:
   
   ![Azure Portal][AZ01]

1. När hello **Apptjänster** visas klickar du på **+ Lägg till** toocreate en ny App-tjänst.

   ![Skapa Apptjänst][AZ02]

1. När hello lista över mallar för app visas, klickar du på hello länk för hello grundläggande Microsoft Web App.

   ![Mallar för App][AZ03]

1. När hello information för hello webbprogrammet mallen visas, klickar du på **skapa**.

   ![Skapa webbapp][AZ04]

1. Ange ett unikt namn för din webbapp och ange eventuella ytterligare inställningar och sedan **skapa**.

   ![Skapa Webbprograminställningar][AZ05]

1. När webbappen har skapats, klickar du på hello menyn ikonen för **Apptjänster**, och klicka sedan på ditt nyligen skapade webbprogram:

   ![Lista över Webbappar][AZ06]

1. När webbappen visas, ange hello Java version med hello följande steg:

   a. Klicka på hello **programinställningar** menyalternativet.

   b. Välj **Java 8** för hello Java-version.

   c. Välj **nyaste** för Java hello delversion.

   d. Välj **senaste Tomcat 8.5** för hello webbehållare. (Den här behållaren faktiskt används inte; Azure använder hello behållare från källan Start programmet.)

   e. Klicka på **Spara**.

   ![Programinställningar][AZ07]

1. Ange dina autentiseringsuppgifter för FTP-distribution med hjälp av hello följande steg:

   a. Klicka på hello **Distributionsbehörigheterna** menyalternativet.

   b. Ange ditt användarnamn och lösenord.

   c. Klicka på **Spara**.

   ![Ange autentiseringsuppgifter för distribution][AZ08]

1. Hämta din FTP-anslutningsinformation med hjälp av hello följande steg:

   a. Klicka på hello **Distributionsbehörigheterna** menyalternativet.

   b. Kopiera hela FTP-användarnamn och URL: en och spara dem för hello nästa avsnitt i den här kursen.

   ![FTP-URL och autentiseringsuppgifter][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a>Distribuera din källan Start web app tooAzure

hello följande tar dig igenom hello steg toodeploy tooAzure din källan Start web app.

1. Öppna en textredigerare, till exempel Anteckningar och klistra in följande text i ett nytt dokument hello och spara hello som *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. När du har sparat hello *web.config* tooyour filsystem, ansluta tooyour webbprogrammet via FTP-med hello URL, användarnamn och lösenord från hello föregående avsnitt i den här kursen. Exempel:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Ändra hello fjärrkatalog toohello rotmapp ditt webbprogram (som är */platsen/wwwroot*), kopiera hello JAR-filen från källan Start programmet och hello *web.config* från tidigare. Exempel:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. När du har distribuerat din JAR och *web.config* filer tooyour webbapp behöver du toorestart ditt webbprogram med hjälp av hello Azure-portalen:

   ![][AZ10]

1. Testa hello webbprogram genom att bläddra tooyour webbappens Webbadress i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. Du bör se hello följande meddelande visas: **helg från källan Start!**

   ![Bläddra Sample-appen][SB02]

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:

* [Distribuera en källan startprogrammet på Linux i hello Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

Mer information om depoying web apps tooAzure med FTP finns [distribuera din app tooAzure App Service med FTP/S].

Mer information om hello källan Start exempelprojektet finns [Vårversionen Start komma igång].

Hjälp med att komma igång med din egen källan startprogram, finns hello **Vårversionen Initializr** på https://start.spring.io/.

Mer information om hur du konfigurerar ytterligare inställningar för ditt webbprogram finns [konfigurera webbappar i Azure App Service].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[konfigurera webbappar i Azure App Service]: /azure/app-service-web/web-sites-configure
[distribuera din app tooAzure App Service med FTP/S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start komma igång]: https://github.com/spring-guides/gs-spring-boot
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
