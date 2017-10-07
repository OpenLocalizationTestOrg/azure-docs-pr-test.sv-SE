---
title: "aaaPublish en start av vår app som en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig hur toopublish en web app tooMicrosoft Azure som en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ."
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
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>Publicera en start av vår app som en dockerbehållare med hello Azure Toolkit för IntelliJ

Hej [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.

[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.

Den här självstudiekursen vägleder dig genom hello steg toodeploy en källan startprogrammet som en Docker-behållare tooMicrosoft Azure med hjälp av hello Azure Toolkit för IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a>Klona hello standard källan Start Docker lagringsplatsen

hello vägleder följande steg dig genom kloning hello källan Start Docker lagringsplatsen med hjälp av IntelliJ. Om du vill toouse kommandoraden finns [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öppna IntelliJ.

1. På välkomstskärmen hello väljer hello **GitHub** alternativ i hello **checka ut från versionskontroll** lista.

   ![GitHub-alternativet för versionskontroll][CL01]

1. Ange dina autentiseringsuppgifter om du tillfrågas toolog i.

   * Om du använder en användarnamn/lösenord toolog i tooGitHub:

      ![Dialogrutan för att ange GitHub-användarnamn och lösenord][CL02a]

   * Om du använder en token toolog i tooGitHub:

      ![Dialogrutan för att ange en GitHub-token][CL02b]

1. Ange **https://github.com/spring-guides/gs-spring-boot-docker.git** Ange lokala sökvägen och mappinformation för hello lagringsplatsen URL och klicka sedan på **klona**.

   ![Klona lagringsplatsen dialogrutan][CL03]

1. När du uppmanas toocreate en IntelliJ projektet, Välj **nr**.

   ![Avböja toocreate IntelliJ-projekt][CL04]

1. Klicka på välkomstsidan hello **Importera projekt**.

   ![Val av Importera projekt][CL05]

1. Hitta hello sökväg där du har klonat hello källan Start lagringsplatsen, Välj hello **fullständig** mapp under hello rot och klicka sedan på **OK**.

   ![Välj en mapp för import][CL06]

1. När du uppmanas, välja **skapa projekt från befintliga datakällor**.

   ![Alternativet toocreate ett projekt från befintliga datakällor][CL07]

1. Ange ditt projektnamn eller accepterar standardinställningen hello, kontrollera hello rätt sökväg toohello **fullständig** mappen och klicka sedan på **nästa**.

   ![Ange hello projektnamn][CL08]

1. Anpassa alla kataloger för att importera och klicka sedan på **nästa**.

   ![Välj kataloger][CL09]

1. Granska hello bibliotek tooimport och klicka sedan på **nästa**.

   ![Granska projektet bibliotek][CL10]

1. Granska hello modulstrukturen och klicka sedan på **nästa**.

   ![Granska modulstrukturen][CL11]

1. Ange din JDK och klicka sedan på **nästa**.

   ![Ange en JDK][CL12]

1. Klicka på **Slutför**.

   ![Slutför][CL13]

IntelliJ importerar hello källan Start appen som ett projekt och visar hello struktur när hello importen är klar.

![Källan Start app i IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a>Skapa din källan Start app

### <a name="build-hello-app-by-using-hello-maven-pom"></a>Skapa hello program med hjälp av hello Maven POM

1. Öppna hello Maven verktygsfönster om den inte redan är öppen. Klicka på **visa** > **verktyget Windows** > **Maven-projekt**.

   ![Kommandona verktyget Windows och Maven-projekt][BU01]

1. I hello Maven verktyget fönstret högerklickar du på **paketet** och välj **kör Maven skapa**. (Om Maven-projekt inte visas automatiskt, klickar du på hello **importera** ikonen i verktygsfältet för hello Maven.)

   ![Kör skapa Maven-kommando][BU02]

1. IntelliJ ska visa en **skapa lyckade** meddelande när appen källan Start har skapats.

   ![Skapa meddelande][BU03]

### <a name="create-a-deployment-ready-artifact"></a>Skapa en distribution redo artefakt

toopublish appen källan Start måste toocreate en artefakt distribution är redo. Använd hello följande steg:

1. Öppna din webbappsprojektet i IntelliJ.

1. Klicka på **filen**, och klicka sedan på **projektstruktur**.

   ![Projektet struktur kommando][ART01]

1. Klicka på hello grön plus (**+**) symbol tooadd en artefakt klickar du på **JAR**, och klicka sedan på **tom**.

   ![Lägg till en artefakt][ART02]

1. Namnge din artefakt samtidigt inte tooadd hello ”.jar” tillägg och sedan ange hello målmappen för hello Maven utdata.

   ![Ange egenskaper för artefakt][ART03]

1. Skapa ett manifest för din artefakt (valfritt):

   a. Klicka på **skapa manifestet**.

      ![Klicka på hello skapa Manifest][ART04a]

   b. Välj hello standardsökvägen för hello artefakt och klicka sedan på **OK**.

      ![Ange artefakt sökväg][ART04b]

   c. Klicka på ellipsknappen hello (**... **) toolocate hello huvudsakliga klass.

      ![Leta upp huvudsakliga klass][ART04c]

   d. Välj huvudklassen och klicka sedan på **OK**.

      ![Ange huvudsakliga klass][ART04d]

1. Klicka på **OK**.

   ![Stäng dialogrutan för hello projektstruktur][ART05]

> [!NOTE]
> Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på hello JetBrains webbplats.
>

### <a name="build-hello-artifact-for-deployment"></a>Skapa hello artefakt för distribution

1. Klicka på **skapa**, och klicka sedan på **artefakter**.

   ![Skapa artefakter kommando][BU04]

1. När hello **skapa artefakt** snabbmenyn visas klickar du på **skapa**.

   ![Skapa artefakt snabbmenyn][BU05]

IntelliJ ska visa hello slutförts artefakt för vår Start-app i hello projekt verktyget-fönstret.

   ![Skapade artefakt][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publicera din web app tooAzure med hjälp av en dockerbehållare

1. Om du inte har loggat in tooyour Azure-konto, gör hello i [inloggning instruktioner för hello Azure Toolkit för IntelliJ][Azure Sign In for IntelliJ].

1. Högerklicka på hello-projekt i hello verktyget Projektutforskaren, och välj sedan **Azure** > **Publicera som Dockerbehållare**.

   ![Publicera som Dockerbehållare kommando][PU01]

1. När hello **distribuera Dockerbehållare på Azure** dialogruta visas alla befintliga Docker-värdar. Om du väljer toodeploy tooan befintliga värden kan du hoppa över toostep 4. Annars Använd hello följande steg toocreate en värd:

   a. Klicka på hello grön plus (**+**) symbolen.

      ![Lägga till en ny Docker-värd][PU02]

   b. När hello **skapa Docker värden** dialogrutan visas, kan du välja tooaccept hello standardinställningar eller du kan ange anpassade inställningar för din nya Docker-värden. (Detaljerade beskrivningar av hello olika inställningar, finns i [publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar toouse.

      ![Ange alternativ för Docker-värden][PU03a]

   c. Du kan välja toouse befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du tooenter nya Docker-inloggningsuppgifter. Klicka på **Slutför** när du har angett dina alternativ.

      ![Ange autentiseringsuppgifter för Docker-värden][PU03b]

1. Välj Docker-värd och klicka sedan på **nästa**.

   ![Välj hello Docker värden toouse][PU04]

1. På hello sista sidan i hello **distribuera Dockerbehållare på Azure** dialogrutan Ange hello följande alternativ:

   a. Du kan välja toospecify ett eget namn för hello-behållare som är värd för din dockerbehållare eller du kan acceptera hello standard.

   b. Ange hello TCP-portar för docker-värden med hjälp av hello följande syntax: *[extern port]*:*[Intern port]*. Till exempel **80:8080** anger ett extern port 80 och hello interna källan Start standardporten 8080.
   
      Om du har anpassat en intern port (t.ex, genom att redigera hello application.yml filen), behöver du toospecify hello-portnumret för hello rätt routning toooccur i Azure.

   c. När du har konfigurerat dessa alternativ klickar du på **Slutför**.

   ![Distribuera en dockerbehållare på Azure][PU05]

1. När hello Azure Toolkit har publicerats hello Azure-aktivitetsloggen visar **publicerade** hello status.

   ![Distribuera Docker-värden][PU06]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

toolearn om ytterligare metoder för att skapa källan Start appar med hjälp av IntelliJ, se [skapa källan Start projekt](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) på hello JetBrains webbplats.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Källan Start]: http://projects.spring.io/spring-boot/
[Källan Framework]: https://spring.io/

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
