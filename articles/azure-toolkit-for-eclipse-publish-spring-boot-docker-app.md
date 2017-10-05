---
title: "Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktygen för Eclipse | Microsoft Docs"
description: "Lär dig hur du publicerar ett webbprogram till Microsoft Azure som en dockerbehållare med hjälp av Azure-verktygen för Eclipse."
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
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktygen för Eclipse

Den [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En av de mer populära projekt som bygger som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig genom stegen för att distribuera en källan startprogrammet som en dockerbehållare till Microsoft Azure med hjälp av Azure-verktygen för Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a>Klona lagringsplatsen standard källan Start Docker

### <a name="import-the-public-repository"></a>Importera den offentliga lagringsplatsen

Följande steg vägleder dig genom kloning källan Start Docker-databasen till den lokala datorn med hjälp av IntelliJ. Om du vill använda en kommandorad Se [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öppna Eclipse.

1. Klicka på **filen** > **importera**.

   ![Importera Arkivmenyn][CL01]

1. När den **importera** öppnas:

   a. Expandera **Git**.

   b. Välj **projekt från Git**.
   
   c. Klicka på **Nästa**.

   ![Dialogrutan Importera][CL02]

1. På den **Välj Lagerkälla** sidan:

   a. Välj **klona URI**.
   
   b. Klicka på **Nästa**.

   ![Välj Lagerkälla sida][CL03]

1. På den **Git-Lagerkälla** sidan:

   a. För **URI**, ange `https://github.com/spring-guides/gs-spring-boot-docker.git`. Det här steget ska automatiskt fylla i den **värden** och **databassökvägen** fält med rätt värden.
   
   b. Källan Start-databasen är offentliga så bör du inte ange Git-användarnamn och lösenord.
   
   c. Klicka på **Nästa**.

   ![Datakälla på sidan för Git-lagringsplats][CL04]

1. På den **val av gren** klickar du på **nästa**.

   ![Sidan för val av gren][CL05]

1. På den **lokal Destination** sidan:

   a. Ange den lokala mappen där du vill att din lokala lagringsplatsen.
   
   b. Klicka på **Nästa**.

   ![Lokala målsida][CL06]

1. På den **Välj en guide för importprojekt** sidan:

   a. Välj **Import som ett allmänt projekt**.
   
   b. Klicka på **Nästa**.

   ![På sidan ”Välj en guide för importprojekt”][CL07]

1. På den **Importera projekt** sidan:

   a. Ange ditt projektnamn.
   
   b. Klicka på **Slutför**.

   ![Importera projekt sida][CL08]

1. När databasen är klonad lyckas, ser du alla filer i Eclipse.

   ![Lokal databas][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>Skapa ett Maven-projekt från din lokala databas

Källan Start Docker-databasen innehåller en slutförd Maven-projekt som du ska använda för den här självstudiekursen. 

1. Klicka på **filen** > **importera**.

   ![Importkommandot på Arkiv-menyn][CL01]

1. När den **importera** öppnas:

   a. Expandera **Maven**.
   
   b. Välj **befintliga Maven-projekt**.
   
   c. Klicka på **Nästa**.

   ![Dialogrutan Importera][MV01]

1. På den **Maven-projekt** sidan:

   a. För **rotkatalog**, ange den **fullständig** mappen i lokala databasen.
   
   b. Expandera den **Avancerat** , och ange ett eget namn för **mall**.
   
   c. Markera kryssrutan för den **pom.xml** filen i projektet.
   
   d. Klicka på **Slutför**.

   ![Sidan för maven-projekt][MV02]

1. När Maven-projekt öppnas har, kan du se ett andra projekt i Eclipse.

   ![Lokala Maven-projekt][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>Skapa din app för vår start med hjälp av Maven

1. Välj Maven-projekt i Projektutforskaren Eclipse.

1. Klicka på **kör** > **kör som** > **Maven build**.

   ![Skapa kommandon som ska köras som Maven][BU01]

1. När programmet har har skapat, visas statusen i konsolfönstret.

   ![Lyckad Maven-version][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publicera webbappen till Azure med hjälp av en dockerbehållare

1. Välj Maven-projekt i Projektutforskaren Eclipse.

1. Klicka på Azure **publicera** -menyn och klicka sedan på **Publicera som Dockerbehållare**.

   ![Publicera som Dockerbehållare kommando][PU01]

1. När den **distribuera Dockerbehållare på Azure** dialogrutan visas:

   a. Ange ett eget namn för Docker-bild.
   
   b. För **artefakt att distribuera**, ange sökvägen till den **gs-källan-Start-docker-0.1.0.jar** filen som du precis har skapat.

   ![Ange alternativ för Docker][PU02]

   Alla befintliga Docker-värdar visas. 

1. Om du vill distribuera till en befintlig värd kan du gå vidare till steg 5. Annars använder du följande steg för att skapa en värd:

   a. Klicka på **Lägg till**.

      ![Lägga till en ny Docker-värd][PU03]

   b. När den **skapa Docker värden** dialogrutan visas, kan du acceptera standardinställningarna och du kan ange anpassade inställningar för din nya Docker-värden. (Detaljerade beskrivningar av olika inställningar finns i [publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar som ska användas.

      ![Ange alternativ för Docker-värden][PU04]

   c. Du kan välja att använda befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du kan välja att ange nya autentiseringsuppgifter för Docker. Klicka på **Slutför** när du har angett dina alternativ.

      ![Ange autentiseringsuppgifter för Docker-värden][PU05]

1. Välj Docker-värd och klicka sedan på **nästa**.

   ![Välj Docker-värden ska använda][PU06]

1. På den sista sidan i den **distribuera Dockerbehållare på Azure** dialogrutan anger du följande alternativ:

   a. Du kan välja att ange ett eget namn för den behållare som är värd för din dockerbehållare eller du kan acceptera standardinställningarna.

   b. Ange TCP-portar för docker-värden med följande syntax: *[extern port]*:*[Intern port]*. Till exempel **80:8080** anger en extern port 80 och interna källan Start standardporten 8080.
   
      Om du har anpassat en intern port (t.ex, genom att redigera filen application.yml), måste du ange portnumret för rätt routning i Azure.

   c. När du konfigurerar dessa alternativ klickar du på **Slutför**.

   ![Distribuera en dockerbehållare på Azure][PU07]

1. När Azure Toolkit har publicerats visas Azure-aktivitetsloggen **publicerade** status.

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
