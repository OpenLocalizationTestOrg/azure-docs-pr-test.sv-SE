---
title: "Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ | Microsoft Docs"
description: "Lär dig hur du publicerar ett webbprogram till Microsoft Azure som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ

Den [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En av de mer populära projekt som bygger som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig genom stegen för att distribuera en källan startprogrammet som en dockerbehållare till Microsoft Azure med hjälp av Azure-verktyget för IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a>Klona lagringsplatsen standard källan Start Docker

Följande steg vägleder dig genom kloning källan Start Docker-lagringsplatsen med hjälp av IntelliJ. Om du vill använda en kommandorad Se [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öppna IntelliJ.

1. På välkomstskärmen väljer du den **GitHub** alternativet i den **checka ut från versionskontroll** lista.

   ![GitHub-alternativet för versionskontroll][CL01]

1. Ange dina autentiseringsuppgifter om du uppmanas att logga in.

   * Om du använder ett användarnamn/lösenord för att logga in på GitHub:

      ![Dialogrutan för att ange GitHub-användarnamn och lösenord][CL02a]

   * Om du använder en token för att logga in på GitHub:

      ![Dialogrutan för att ange en GitHub-token][CL02b]

1. Ange **https://github.com/spring-guides/gs-spring-boot-docker.git** Ange lokala sökvägen och mappinformation för lagringsplatsen-URL och klicka sedan på **klona**.

   ![Klona lagringsplatsen dialogrutan][CL03]

1. När du uppmanas att skapa ett projekt för IntelliJ Välj **nr**.

   ![Acceptera att skapa ett projekt för IntelliJ][CL04]

1. På sidan Välkommen **Importera projekt**.

   ![Val av Importera projekt][CL05]

1. Leta upp den sökväg där du klona lagringsplatsen källan Start, Välj den **fullständig** mapp under roten och klicka sedan på **OK**.

   ![Välj en mapp för import][CL06]

1. När du uppmanas, välja **skapa projekt från befintliga datakällor**.

   ![Alternativet för att skapa ett projekt från befintliga datakällor][CL07]

1. Ange ditt projektnamn eller acceptera standardinställningarna, kontrollera att rätt sökväg till den **fullständig** mappen och klicka sedan på **nästa**.

   ![Ange projektets namn][CL08]

1. Anpassa alla kataloger för att importera och klicka sedan på **nästa**.

   ![Välj kataloger][CL09]

1. Granska bibliotek för att importera och klicka sedan på **nästa**.

   ![Granska projektet bibliotek][CL10]

1. Granska modulstrukturen och klicka sedan på **nästa**.

   ![Granska modulstrukturen][CL11]

1. Ange din JDK och klicka sedan på **nästa**.

   ![Ange en JDK][CL12]

1. Klicka på **Slutför**.

   ![Slutför][CL13]

IntelliJ importerar källan Start-appen som ett projekt och visar strukturen när importen är klar.

![Källan Start app i IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a>Skapa din källan Start app

### <a name="build-the-app-by-using-the-maven-pom"></a>Bygga appen med hjälp av Maven POM

1. Öppna fönstret Maven-verktyget om den inte redan är öppen. Klicka på **visa** > **verktyget Windows** > **Maven-projekt**.

   ![Kommandona verktyget Windows och Maven-projekt][BU01]

1. Högerklicka i fönstret Maven verktyget **paketet** och välj **kör Maven skapa**. (Om Maven-projekt inte visas automatiskt, klickar du på den **importera** ikonen i verktygsfältet Maven.)

   ![Kör skapa Maven-kommando][BU02]

1. IntelliJ ska visa en **skapa lyckade** meddelande när appen källan Start har skapats.

   ![Skapa meddelande][BU03]

### <a name="create-a-deployment-ready-artifact"></a>Skapa en distribution redo artefakt

Om du vill publicera appen källan Start måste du skapa en distribution redo artefakt. Använd följande steg:

1. Öppna din webbappsprojektet i IntelliJ.

1. Klicka på **filen**, och klicka sedan på **projektstruktur**.

   ![Projektet struktur kommando][ART01]

1. Klicka på den gröna plus (**+**) symbolen om du vill lägga till en artefakt klickar du på **JAR**, och klicka sedan på **tom**.

   ![Lägg till en artefakt][ART02]

1. Namnge din artefakt samtidigt till att du inte lägga till filnamnstillägget ”.jar” och ange målmappen för Maven-utdata.

   ![Ange egenskaper för artefakt][ART03]

1. Skapa ett manifest för din artefakt (valfritt):

   a. Klicka på **skapa manifestet**.

      ![Klicka på knappen Skapa Manifest][ART04a]

   b. Välj standardsökvägen för artefakten och klicka sedan på **OK**.

      ![Ange artefakt sökväg][ART04b]

   c. Klicka på ellipsknappen (**...** ) att hitta klassen huvudsakliga.

      ![Leta upp huvudsakliga klass][ART04c]

   d. Välj huvudklassen och klicka sedan på **OK**.

      ![Ange huvudsakliga klass][ART04d]

1. Klicka på **OK**.

   ![Stäng dialogrutan projektstruktur][ART05]

> [!NOTE]
> Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på webbplatsen JetBrains.
>

### <a name="build-the-artifact-for-deployment"></a>Skapa artefakt för distribution

1. Klicka på **skapa**, och klicka sedan på **artefakter**.

   ![Skapa artefakter kommando][BU04]

1. När den **skapa artefakt** snabbmenyn visas klickar du på **skapa**.

   ![Skapa artefakt snabbmenyn][BU05]

IntelliJ ska visa slutförda artefakten för vår Start-app i projektfönstret-verktyget.

   ![Skapade artefakt][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publicera webbappen till Azure med hjälp av en dockerbehållare

1. Om du inte har loggat in på Azure-konto, följer du stegen i [inloggning instruktioner för Azure-Toolkit för IntelliJ][Azure Sign In for IntelliJ].

1. I verktyget Projektutforskaren, högerklicka på projektet och välj sedan **Azure** > **Publicera som Dockerbehållare**.

   ![Publicera som Dockerbehållare kommando][PU01]

1. När den **distribuera Dockerbehållare på Azure** dialogruta visas alla befintliga Docker-värdar. Om du vill distribuera till en befintlig värd kan du gå vidare till steg 4. Annars använder du följande steg för att skapa en värd:

   a. Klicka på den gröna plus (**+**) symbolen.

      ![Lägga till en ny Docker-värd][PU02]

   b. När den **skapa Docker värden** dialogrutan visas, kan du acceptera standardinställningarna och du kan ange anpassade inställningar för din nya Docker-värden. (Detaljerade beskrivningar av olika inställningar finns i [publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar som ska användas.

      ![Ange alternativ för Docker-värden][PU03a]

   c. Du kan välja att använda befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du kan välja att ange nya autentiseringsuppgifter för Docker. Klicka på **Slutför** när du har angett dina alternativ.

      ![Ange autentiseringsuppgifter för Docker-värden][PU03b]

1. Välj Docker-värd och klicka sedan på **nästa**.

   ![Välj Docker-värden ska använda][PU04]

1. På den sista sidan i den **distribuera Dockerbehållare på Azure** dialogrutan anger du följande alternativ:

   a. Du kan välja att ange ett eget namn för den behållare som är värd för din dockerbehållare eller du kan acceptera standardinställningarna.

   b. Ange TCP-portar för docker-värden med följande syntax: *[extern port]*:*[Intern port]*. Till exempel **80:8080** anger en extern port 80 och interna källan Start standardporten 8080.
   
      Om du har anpassat en intern port (t.ex, genom att redigera filen application.yml), måste du ange portnumret för rätt routning i Azure.

   c. När du har konfigurerat dessa alternativ klickar du på **Slutför**.

   ![Distribuera en dockerbehållare på Azure][PU05]

1. När Azure Toolkit har publicerats visas Azure-aktivitetsloggen **publicerade** status.

   ![Distribuera Docker-värden][PU06]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Läs om ytterligare metoder för att skapa källan Start appar med hjälp av IntelliJ i [skapa källan Start projekt](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) på webbplatsen JetBrains.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
