---
title: "en grundläggande Azure web app med hjälp av aaaCreate solförmörkelse | Microsoft Docs"
description: "Den här kursen visar hur toouse hello Azure Toolkit för Eclipse toocreate Hello World Web App för Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>Skapa en grundläggande Azure-webbapp med Eclipse
Den här kursen visar hur toocreate och distribuera en grundläggande Hello World programmet tooAzure som ett webbprogram med hjälp av hello [Azure Toolkit för Eclipse]. En grundläggande JSP-exempel visas för enkelhetens skull, men liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.

När du har slutfört den här självstudiekursen ser liknande toohello följande bild visas i en webbläsare i ditt program:

![Förhandsgranskning av Hello World-app][01]

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), v 1,8 eller senare.
* Solförmörkelse IDE för Java EE-utvecklare, Luna eller senare. Detta kan hämtas från <http://www.eclipse.org/downloads/>.
* En distributionsplats i en Java-baserad webbserver eller programserver som [Apache Tomcat] eller [Jetty].
* En Azure-prenumeration som kan erhållas från <https://azure.microsoft.com/free/> eller <http://azure.microsoft.com/pricing/purchase-options/>.
* Hej [Azure Toolkit för Eclipse]. Information om hur du installerar hello Azure Toolkit finns [installerar hello Azure Toolkit för Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate ett Hello World-program
Först ska vi börja med att skapa ett Java-projekt.

1. Starta Eclipse och hello-menyn klickar du på **filen**, klickar du på **ny**, och klicka sedan på **dynamiskt webbprojekt**. (Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt när du klickar på **filen** och **ny**, sedan hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt...** , expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.)
2. Namn för den här kursen är hello projektet **MyWebApp**. Skärmen visas liknande toohello följande:
   
    ![Skapa en ny dynamiskt webbprojekt][02]
3. Klicka på **Slutför**.
4. I Eclipses Projektutforskaren vy Expandera **MyWebApp**. Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.
5. I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**, hålla hello överordnade mappen som **MyWebApp/webbinnehåll**, och klicka sedan på **nästa**.
6. I hello **Välj JSP-mall** dialogrutan för den här självstudiekursen väljer **ny JSP-fil (html)**, och klicka sedan på **Slutför**.
7. När index.jsp-filen öppnas i Eclipse lägga till texten toodynamically visas på **Hello World!** inom hello befintliga `<body>` element. Uppdaterade `<body>` innehåll bör likna följande exempel hello:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. Spara index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy dina program tooan Azure Web App-behållare
Det finns flera olika sätt som du kan distribuera en Java web application tooAzure. Den här självstudiekursen beskriver en hello enklaste: ditt program kommer att distribuerade tooan Azure Web App-behållare – krävs inget speciellt projekttyp eller andra verktyg. hello JDK hello web behållaren programvara och kommer att tillhandahållas för dig av Azure, så det finns inget behov av tooupload egna; allt du behöver är Java-Webbappen. Därför tar hello publiceringsprocessen för tillämpningsprogrammet sekunder, inte minuter.

1. Högerklicka i Eclipses Projektutforskaren **MyWebApp**.
2. Hello snabbmenyn Välj **Azure**, klicka på **Publicera som Azure Web App...**
   
    ![Publicera som Azure Webbapp][03]
   
    Medan webbprojektet för programmet är markerat i hello Projektutforskaren, du kan också klicka på hello **publicera** nedrullningsbara knappen hello verktygsfältet och välj **Publicera som Azure Web App** där:
   
    ![Publicera som Azure Webbapp][14]
3. Om du inte redan loggat in Azure från Eclipse kommer du att ange toosign i ditt Azure-konto:
   
    ![Azure logga In dialogrutan][04]
   
    Om du har flera Azure-konton, vissa hello frågor under hello logga in processen kanske visas mer än en gång, även om de visas toobe hello samma. Då kan fortsätta efter hello inloggning instruktioner.
4. När du har loggat in ditt Azure-konto, hello **hantera prenumerationer** dialogrutan visas en lista över prenumerationer som är associerade med dina autentiseringsuppgifter. Om det finns flera prenumerationerna i listan och du vill toowork med bara en delmängd av dem, kan du avmarkera hello som du vill toouse (valfritt). När du har valt dina prenumerationer, klickar du på **Stäng**.
   
    ![Hantera prenumerationer dialogrutan][05]
5. När hello **distribuera tooAzure Web App behållaren** i dialogrutan visas alla webbprogram-behållare som du tidigare har skapat; om du inte har skapat några behållare, hello listan är tom.
   
    ![Distribuera tooAzure App Webbehållaren dialogrutan][06]
6. Om du inte har skapat ett Azure Web App-behållare innan, eller om du vill att toopublish dina program tooa ny behållare, använda hello följande steg. Annars markerar du en befintlig Web Appbehållare och hoppa över toostep 7 nedan.
   
   1. Klicka på **nya...**
      
       ![Distribuera tooAzure App Webbehållaren dialogrutan][15]
   2. Hej **ny Web Appbehållare** dialogrutan visas:
      
       ![Dialogrutan Nytt Web Appbehållare][07a]
   3. Ange en **DNS-etikett** för App Webbehållaren; kommer att skapas hello löv DNS-etikett hello värd-URL för ditt webbprogram i Azure. (Observera att hello-namn måste vara tillgängliga och följa tooAzure krav för webbprogram.)
   4. I hello **Webbehållaren** nedrullningsbara menyn, Välj hello rätt programvara för ditt program.
      
       För närvarande kan du välja mellan Tomcat 8, 7 Tomcat eller Jetty 9. En senaste fördelning av hello valt programvara kommer att tillhandahållas av Azure och körs på en senaste fördelning av JDK 8 skapats av Oracle och från Azure.
   5. I hello **prenumeration** nedrullningsbara menyn, Välj hello-prenumeration som du vill toouse för den här distributionen.
   6. I hello **resursgruppen** nedrullningsbara menyn, Välj hello resursgrupp som du vill använda tooassociate ditt webbprogram. (Azure resursgrupper kan du toogroup relaterade resurser tillsammans så att till exempel kan du ta bort tillsammans.)
      
       Du kan välja en befintlig resursgrupp (om du har några) och hoppa över toostep g nedan eller Använd hello följa dessa steg toocreate en ny resursgrupp:
      
      * Klicka på **nya...**
      * Hej **ny resursgrupp** dialogrutan visas:
        
          ![Dialogrutan Ny resursgrupp][08]
      * I hello hello **namn** textruta, ange ett namn för din nya resursgrupp.
      * I hello hello **Region** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för resursgruppen.
      * Valfritt: Som standard en senaste fördelning av Java 8 kommer att distribueras av Azure automatiskt tooyour web appbehållare som din JVM. Du kan dock ange en annan version och distribution av hello JVM om ditt program kräver. toospecify hello JDK för ditt webbprogram, klicka på hello **JDK** och välj något av följande alternativ för hello:
        
        * **Distribuera hello standard JDK som erbjuds av Azure Web Apps service**: det här alternativet distribuerar en senaste fördelning av Java 8.
        * **Distribuera en 3 part JDK som är tillgängliga på Azure**: det här alternativet kan du toochoose hello listan över JDKs som tillhandahålls av Microsoft Azure.
        * **Distribuera min egen JDK från den här hämtningsplats**: det här alternativet kan du toospecify egna JDK-distribution som måste paketeras som en ZIP-filen och överföra tooeither offentligt tillgängliga hämtningsplats eller ett Azure storage-konto som du ha åtkomst.
          
          ![Dialogrutan Nytt Web Appbehållare][07b]
   7. Klicka på **OK**.
   8. Hej **Apptjänstplan** listrutan visar hello apptjänstplaner som är associerade med hello resursgrupp som du har valt. (Programtjänstplaner ange information, till exempel hello platsen för ditt webbprogram, hello prisnivå och hello instansstorleken för beräkning. En enda App Service-Plan kan användas för flera webbprogram, vilket är anledningen till att den hanteras separat från en viss distribution för webbprogrammet.)
      
       Du kan välja en befintlig App Service-Plan (om du har några) och hoppa över toostep h nedan eller använda hello följa dessa steg toocreate en ny App Service-Plan:
      
      * Klicka på **nya...**
      * Hej **nya App Service-Plan** dialogrutan visas:
        
          ![Dialogrutan för ny App Service-Plan][09]
      * I hello hello **namn** textruta, ange ett namn för din nya App Service-Plan.
      * I hello hello **plats** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för hello-plan.
      * I hello hello **prisnivån** nedrullningsbara menyn, Välj hello lämpliga priser för hello plan. I testsyfte kan du välja **lediga**.
      * I hello hello **Instansstorleken** nedrullningsbara menyn, Välj hello lämplig instans storlek för hello-plan. I testsyfte kan du välja **små**.
   9. När du har slutfört alla hello senare steg, bör hello ny Web Appbehållare dialogrutan likna följande illustration hello:
      
       ![Dialogrutan Nytt Web Appbehållare][10]
   10. Klicka på **OK** toocomplete hello skapandet av din nya Webbapp-behållaren.
       
        Vänta några sekunder hello lista över hello webbprogrammet behållare toobe uppdateras och nyskapade app webbehållaren ska nu vara markerade i hello-listan.
7. Du är nu redo toocomplete hello initiala distributionen av ditt webbprogram tooAzure:
   
    ![Distribuera tooAzure App Webbehållaren dialogrutan][11]
   
    Klicka på **OK** toodeploy toohello din Java-programmet valt Web App behållare.
   
    Programmet kommer att distribueras som en underkatalog till hello programservern som standard. Om du vill att den distribueras som hello rotprogram toobe, kontrollera hello **distribuera tooroot** kryssrutan innan du klickar på **OK**.
8. Därefter bör du se hello **Azure-aktivitetsloggen** vy, som visar hello Distributionsstatus för ditt webbprogram.
   
    ![Azure-aktivitetsloggen][12]
   
    hello-processen för att distribuera webbprogram-tooAzure tar endast några sekunder toocomplete. När din programmet kan påbörjas kan du visa en länk med namnet **publicerade** i hello **Status** kolumn. När du klickar på länken hello tar du tooyour distribueras Web App startsidan.

## <a name="updating-your-web-app"></a>Uppdatera din webbapp
Uppdatering av ett befintligt kör Azure Web App är en snabb och enkel process och har du två alternativ för att uppdatera:

* Du kan uppdatera hello distribution av en befintlig Java-Webbapp.
* Du kan publicera en ytterligare Java-program toohello Web Appbehållare.

I båda fallen hello processen är identiska och tar endast några sekunder:

1. Högerklicka på hello Java-program du vill tooupdate eller lägga till tooan befintliga Web Appbehållare i hello Eclipse Projektutforskaren.
2. Välj när hello snabbmenyn visas **Azure** och sedan **Publicera som Azure Web App...**
3. Eftersom du har redan loggat in tidigare, visas en lista över dina befintliga Web App-behållare. Välj hello en du vill toopublish eller publicera din Java-programmet tooand klickar **OK**.

Några sekunder senare hello **Azure-aktivitetsloggen** vyer visar uppdaterade distributionen som **publicerade** och du kommer att kunna tooverify uppdaterade programmet i en webbläsare.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Starta, stoppa eller starta om en befintlig webbapp
toostart eller stoppa en befintlig Azure Web App-behållare (inklusive alla hello distribueras Java-program i den), kan du använda hello **Azure Explorer** vyn.

Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **fönstret** Klicka på menyn i Eclipse **visa**, sedan **andra...** , sedan **Azure**, och klicka sedan på **Azure Explorer**. Om du inte tidigare har loggat in uppmanas du toodo så.

När hello **Azure Explorer** visas, Använd följer du dessa steg toostart eller stoppa ditt webbprogram: 

1. Expandera hello **Azure** nod.
2. Expandera hello **Web Apps** nod. 
3. Högerklicka på hello önskat webbprogram.
4. På snabbmenyn för hello visas **starta**, **stoppa**, eller **starta om**. Observera att hello menyalternativen kontextmedvetna, så att du kan bara stoppa en webbapp som körs eller starta ett webbprogram som inte körs.
   
    ![Stoppa en befintlig Webbapp][13]

## <a name="next-steps"></a>Nästa steg
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [installerar hello Azure Toolkit för Eclipse]
  * *Skapa en Hello World-Webbapp för Azure i Eclipse (den här artikeln)*
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

Mer information om hur du skapar Azure Web Apps finns hello [översikt över Web Apps].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[installerar hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[översikt över Web Apps]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
