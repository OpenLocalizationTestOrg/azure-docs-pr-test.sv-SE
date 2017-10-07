---
title: "aaaPublish en start av vår app som en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur toopublish en web app tooMicrosoft Azure som en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>Publicera en start av vår app som en dockerbehållare med hello Azure Toolkit för Eclipse

Hej [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig genom hello steg toodeploy en källan startprogrammet som en Docker-behållare tooMicrosoft Azure med hjälp av hello Azure Toolkit för Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a>Klona hello standarddatabas källan Start Docker

### <a name="import-hello-public-repository"></a>Importera hello offentliga lagringsplats

hello vägleder följande steg dig genom kloning hello källan Start Docker databasen tooyour lokala datorn med hjälp av IntelliJ. Om du vill toouse kommandoraden finns [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öppna Eclipse.

1. Klicka på **filen** > **importera**.

   ![Importera Arkivmenyn][CL01]

1. När hello **importera** öppnas:

   a. Expandera **Git**.

   b. Välj **projekt från Git**.
   
   c. Klicka på **Nästa**.

   ![Dialogrutan Importera][CL02]

1. På hello **Välj Lagerkälla** sidan:

   a. Välj **klona URI**.
   
   b. Klicka på **Nästa**.

   ![Välj Lagerkälla sida][CL03]

1. På hello **Git-Lagerkälla** sidan:

   a. För **URI**, ange `https://github.com/spring-guides/gs-spring-boot-docker.git`. Det här steget ska automatiskt fylla i hello **värden** och **databassökvägen** fält med hello korrigera värden.
   
   b. hello källan Start databas är offentliga så du inte bör ha tooenter Git användarnamn och lösenord.
   
   c. Klicka på **Nästa**.

   ![Datakälla på sidan för Git-lagringsplats][CL04]

1. På hello **val av gren** klickar du på **nästa**.

   ![Sidan för val av gren][CL05]

1. På hello **lokal Destination** sidan:

   a. Ange hello lokala mappen där du vill att din lokala lagringsplatsen.
   
   b. Klicka på **Nästa**.

   ![Lokala målsida][CL06]

1. På hello **väljer en toouse guide för importprojekt** sidan:

   a. Välj **Import som ett allmänt projekt**.
   
   b. Klicka på **Nästa**.

   ![På sidan ”Välj ett toouse guide för importprojekt”][CL07]

1. På hello **Importera projekt** sidan:

   a. Ange ditt projektnamn.
   
   b. Klicka på **Slutför**.

   ![Importera projekt sida][CL08]

1. När hello databasen är klonad lyckas, ser du alla hello filer i Eclipse.

   ![Lokal databas][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>Skapa ett Maven-projekt från din lokala databas

hello källan Start Docker-databasen innehåller en slutförd Maven-projekt som du ska använda för den här självstudiekursen. 

1. Klicka på **filen** > **importera**.

   ![Importkommando hello Arkiv-menyn][CL01]

1. När hello **importera** öppnas:

   a. Expandera **Maven**.
   
   b. Välj **befintliga Maven-projekt**.
   
   c. Klicka på **Nästa**.

   ![Dialogrutan Importera][MV01]

1. På hello **Maven-projekt** sidan:

   a. För **rotkatalog**, ange hello **fullständig** mappen i lokala databasen.
   
   b. Expandera hello **Avancerat** , och ange ett eget namn för **mall**.
   
   c. Välj hello rutan för hello **pom.xml** fil i hello-projekt.
   
   d. Klicka på **Slutför**.

   ![Sidan för maven-projekt][MV02]

1. När hello Maven-projekt öppnas har, kan du se ett andra projekt i Eclipse.

   ![Lokala Maven-projekt][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>Skapa din app för vår start med hjälp av Maven

1. Välj hello Maven-projekt i hello Eclipse Projektutforskaren.

1. Klicka på **kör** > **kör som** > **Maven build**.

   ![Kommandon toorun som Maven build][BU01]

1. När programmet är skapat visar hello konsolfönstret hello status.

   ![Lyckad Maven-version][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publicera din web app tooAzure med hjälp av en dockerbehållare

1. Välj hello Maven-projekt i hello Eclipse Projektutforskaren.

1. Klicka på hello Azure **publicera** -menyn och klicka sedan på **Publicera som Dockerbehållare**.

   ![Publicera som Dockerbehållare kommando][PU01]

1. När hello **distribuera Dockerbehållare på Azure** dialogrutan visas:

   a. Ange ett eget namn för Docker-bild.
   
   b. För **artefakt toodeploy**, ange hello sökvägen toohello **gs-källan-Start-docker-0.1.0.jar** filen som du precis har skapat.

   ![Ange alternativ för Docker][PU02]

   Alla befintliga Docker-värdar visas. 

1. Om du väljer toodeploy tooan befintliga värden kan du hoppa över toostep 5. Annars Använd hello följande steg toocreate en värd:

   a. Klicka på **Lägg till**.

      ![Lägga till en ny Docker-värd][PU03]

   b. När hello **skapa Docker värden** dialogrutan visas, kan du välja tooaccept hello standardinställningar eller du kan ange anpassade inställningar för din nya Docker-värden. (Detaljerade beskrivningar av hello olika inställningar, finns i [publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar toouse.

      ![Ange alternativ för Docker-värden][PU04]

   c. Du kan välja toouse befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du tooenter nya Docker-inloggningsuppgifter. Klicka på **Slutför** när du har angett dina alternativ.

      ![Ange autentiseringsuppgifter för Docker-värden][PU05]

1. Välj Docker-värd och klicka sedan på **nästa**.

   ![Välj Docker värden toouse][PU06]

1. På hello sista sidan i hello **distribuera Dockerbehållare på Azure** dialogrutan Ange hello följande alternativ:

   a. Du kan välja toospecify ett eget namn för hello-behållare som är värd för din dockerbehållare eller du kan acceptera hello standard.

   b. Ange hello TCP-portar för docker-värden med hjälp av hello följande syntax: *[extern port]*:*[Intern port]*. Till exempel **80:8080** anger ett extern port 80 och hello interna källan Start standardporten 8080.
   
      Om du har anpassat en intern port (t.ex, genom att redigera hello application.yml filen), behöver du toospecify hello-portnumret för hello rätt routning toooccur i Azure.

   c. När du konfigurerar dessa alternativ klickar du på **Slutför**.

   ![Distribuera en dockerbehållare på Azure][PU07]

1. När hello Azure Toolkit har publicerats hello Azure-aktivitetsloggen visar **publicerade** hello status.

   ![Distribuera Docker-värden][PU08]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
