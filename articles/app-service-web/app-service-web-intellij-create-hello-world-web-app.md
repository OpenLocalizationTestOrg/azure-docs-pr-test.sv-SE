---
title: "aaaCreate en grundläggande Azure-webbapp i IntelliJ | Microsoft Docs"
description: "Den här kursen visar hur toouse hello Azure Toolkit för IntelliJ toocreate Hello World Web App för Azure."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4667497213cac3ddf754d164e614c809f338cce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a>Skapa en grundläggande Azure webbapp i IntelliJ
Den här kursen visar hur toocreate och distribuera en grundläggande Hello World programmet tooAzure som ett webbprogram med hjälp av hello [Azure Toolkit för IntelliJ]. En grundläggande JSP-exempel visas för enkelhetens skull, men liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.

När du har slutfört den här självstudiekursen ser liknande toohello följande bild visas i en webbläsare i ditt program:

![Exempelsida][01]

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), v 1,8 eller senare.
* IntelliJ IDEA Ultimate Edition. Detta kan hämtas från <https://www.jetbrains.com/idea/download/index.html>.
* En distributionsplats i en Java-baserad webbserver eller programserver som [Apache Tomcat] eller [Jetty].
* En Azure-prenumeration som kan erhållas från <https://azure.microsoft.com/free/> eller <http://azure.microsoft.com/pricing/purchase-options/>.
* Hej [Azure Toolkit för IntelliJ]. Information om hur du installerar hello Azure Toolkit finns [installerar hello Azure Toolkit för IntelliJ].

## <a name="toocreate-a-hello-world-application"></a>toocreate ett Hello World-program
Först ska vi börja med att skapa ett Java-projekt.

1. Starta IntelliJ och klicka på hello **filen** -menyn och klicka sedan på **ny**, och klicka sedan på **projekt**.
   
    ![Filen nytt projekt][02]
2. Hello nytt projekt i dialogrutan Välj **Java**, sedan **webbprogram**, och klicka sedan på **ny** tooadd en projekt-SDK.
   
    ![Dialogrutan Nytt projekt][03a]
   
3. I hello Välj arbetskatalog för JDK dialogrutan Välj hello mappen där din JDK är installerat och klicka sedan på **OK**. Klicka på **nästa** i hello nytt projekt dialogrutan rutan toocontinue.
   
    ![Ange arbetskatalog för JDK][03b]
4. Namn för den här kursen är hello projektet **Java-webb-App-på-Azure**, och klicka sedan på **Slutför**.
   
    ![Dialogrutan Nytt projekt][04]
5. I vyn Projektutforskaren Intellijs Expandera **Java-webb-App-på-Azure**, expandera **web**, och dubbelklicka sedan på **index.jsp**.
   
    ![Öppna indexsidan][05c]
6. När index.jsp-filen öppnas i IntelliJ, lägga till texten toodynamically visas på **Hello World!** inom hello befintliga `<body>` element. Uppdaterade `<body>` innehåll bör likna följande exempel hello:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. Spara index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy dina program tooan Azure Web App-behållare
Det finns flera olika sätt som du kan distribuera en Java web application tooAzure. Den här självstudiekursen beskriver en hello enklaste: ditt program kommer att distribuerade tooan Azure Web App-behållare – krävs inget speciellt projekttyp eller andra verktyg. hello JDK hello web behållaren programvara och kommer att tillhandahållas för dig av Azure, så det finns inget behov av tooupload egna; allt du behöver är Java-Webbappen. Därför tar hello publiceringsprocessen för tillämpningsprogrammet sekunder, inte minuter.

Innan du publicerar ditt program måste du först tooconfigure modulinställningarna i. toodo så Använd hello följande steg:

1. Högerklicka i Intellijs Projektutforskaren hello **Java-webb-App-på-Azure** projekt. På snabbmenyn för hello visas **öppna Modulinställningar**.

    ![Öppna modulinställningarna för][05a]
2. När dialogrutan för hello projektstruktur visas:

   a. Klicka på **artefakter** i hello lista över **Projektinställningar**.
   b. Ändra hello artefakt namn i hello **namn** så att den inte innehåller blanksteg eller specialtecken; detta är nödvändigt eftersom hello namn kommer att användas i hello identifierare URI (Uniform Resource).
   c. Ändra hello **typen** för**webbprogrammet: Arkiv**.
   d. Klicka på **OK** tooclose hello projektstrukturen i dialogrutan.

    ![Öppna modulinställningarna för][05b]

När du har konfigurerat modulinställningarna för, kan du publicera program-tooAzure med hjälp av hello följande steg:

1. Högerklicka i Intellijs Projektutforskaren hello **Java-webb-App-på-Azure** projekt. Välj när hello snabbmenyn visas **Azure**, och klicka sedan på **Publicera som Azure Web App...**
   
    ![Azure publicera snabbmenyn][06]
2. Om du inte redan loggat in Azure från IntelliJ kommer du att tillfrågas toosign i ditt Azure-konto. (Om du har flera Azure-konton kan vissa hello frågor under hello inloggning i processen för kanske visas mer än en gång, även om de visas toobe hello samma. När detta inträffar kan fortsätta toofollow hello logga i anvisningarna.)
   
    ![Azure logganalys i dialogrutan][07]
3. När du har loggat in ditt Azure-konto, hello **hantera prenumerationer** dialogrutan visas en lista över prenumerationer som är associerade med dina autentiseringsuppgifter. (Om det finns flera prenumerationerna i listan och du vill toowork med bara en delmängd av dem, du kan du avmarkera hello prenumerationer som du inte vill toouse.) När du har valt dina prenumerationer, klickar du på **Stäng**.
   
    ![Hantera prenumerationer][08]
4. När hello **distribuera tooAzure Web App behållaren** i dialogrutan visas alla webbprogram-behållare som du tidigare har skapat; om du inte har skapat några behållare, hello listan är tom.
   
    ![App-behållare][09]
5. Om du inte har skapat ett Azure Web App-behållare innan, eller om du vill att toopublish dina program tooa ny behållare, använda hello följande steg. Annars markerar du en befintlig Web Appbehållare och hoppa över toostep 6 nedan.
   
   1. Klicka på**+**
      
       ![Lägg till Appbehållare][10]
   2. Hej **ny Web Appbehållare** dialogrutan kommer att visas som kommer att vara används för hello bredvid flera steg.
      
       ![Ny Appbehållare][11a]
   3. Ange en **DNS-etikett** för App Webbehållaren; kommer att skapas hello löv DNS-etikett hello värd-URL för ditt webbprogram i Azure. Observera att hello-namnet måste vara tillgängliga och uppfylla krav för tooAzure Web App.
   4. I hello **Webbehållaren** nedrullningsbara menyn, Välj hello rätt programvara för ditt program.
      
       För närvarande kan du välja mellan Tomcat 8, 7 Tomcat eller Jetty 9. En senaste fördelning av hello valt programvara kommer att tillhandahållas av Azure och körs på en senaste fördelning av JDK 8 skapats av Oracle och från Azure.
   5. I hello **prenumeration** nedrullningsbara menyn, Välj hello-prenumeration som du vill toouse för den här distributionen.
   6. I hello **resursgruppen** nedrullningsbara menyn, Välj hello resursgrupp som du vill använda tooassociate ditt webbprogram. (Azure resursgrupper kan du toogroup relaterade resurser tillsammans så att till exempel kan du ta bort tillsammans.)
      
       Du kan välja en befintlig resursgrupp (om du har några) och hoppa över toostep g nedan eller Använd hello följande steg toocreate en ny resursgrupp:
      
      * Välj  **&lt; &lt; Skapa ny resursgrupp &gt; &gt;**  i hello **resursgruppen** nedrullningsbara menyn.
      * Hej **ny resursgrupp** dialogrutan visas:
        
          ![Ny resursgrupp][12]
      * I hello hello **namn** textruta, ange ett namn för din nya resursgrupp.
      * I hello hello **Region** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för resursgruppen.
      * Klicka på **OK**.
   7. Hej **Apptjänstplan** listrutan visar hello apptjänstplaner som är associerade med hello resursgrupp som du har valt. (En Apptjänstplan anger information, till exempel hello platsen för ditt webbprogram, hello prisnivå och hello instansstorleken för beräkning. En enda App Service-Plan kan användas för flera webbprogram, vilket är anledningen till att den hanteras separat från en viss distribution för webbprogrammet.)
      
       Du kan välja en befintlig App Service-Plan (om du har några) och hoppa över toostep h nedan eller använda följande steg toocreate en ny App Service-Plan hello:
      
      * Välj  **&lt; &lt; skapa nya App Service-Plan &gt; &gt;**  i hello **Apptjänstplan** nedrullningsbara menyn.
      * Hej **nya App Service-Plan** dialogrutan visas:
        
          ![Ny Programtjänstplan][13]
      * I hello hello **namn** textruta, ange ett namn för din nya App Service-Plan.
      * I hello hello **plats** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för hello-plan.
      * I hello hello **prisnivån** nedrullningsbara menyn, Välj hello lämpliga priser för hello plan. I testsyfte kan du välja **lediga**.
      * I hello hello **Instansstorleken** nedrullningsbara menyn, Välj hello lämplig instans storlek för hello-plan. I testsyfte kan du välja **små**.
      * Klicka på **OK**.
   8. (Valfritt) Som standard ska en senaste fördelning av Java 8 automatiskt distribueras som din JVM enligt tooyour Azure web appbehållare. Du kan dock välja en annan version och distribution av hello JVM. toodo så Använd hello följande steg:
      
      * Klicka på hello **JDK** fliken i hello **ny Web Appbehållare** dialogrutan.
      * Du kan välja något av följande alternativ för hello:
        
        * Distribuera hello standard JDK som erbjuds av Azure
        * Distribuera en 3 part JDK från listrutan av ytterligare JDKs som är tillgängliga på Azure
        * Distribuera en anpassad JDK som måste paketeras som en ZIP-fil och antingen offentligt tillgängliga eller i Azure storage-konto
        
        ![Ny App behållaren JDK flik][11b]
   9. När du har slutfört alla hello senare steg, bör hello ny Web Appbehållare dialogrutan likna följande illustration hello:
      
       ![Ny Appbehållare][14]
   10. Klicka på **OK** toocomplete hello skapandet av din nya Webbapp-behållaren.
       
        Vänta några sekunder hello lista över hello webbprogrammet behållare toobe uppdateras och nyskapade app webbehållaren ska nu vara markerade i hello-listan.
6. Du är nu redo toocomplete hello initiala distributionen av ditt webbprogram tooAzure; Klicka på **OK** toodeploy toohello din Java-programmet valt Web App behållare. Programmet kommer att distribueras som en underkatalog till hello programservern som standard. Om du vill att den distribueras som hello rotprogram toobe, kontrollera hello **distribuera tooroot** kryssrutan innan du klickar på **OK**.
   
    ![Distribuera tooAzure][15]
7. Därefter bör du se hello **Azure-aktivitetsloggen** vy, som visar hello Distributionsstatus för ditt webbprogram.
   
    ![Förloppsindikator][16]
   
    hello-processen för att distribuera webbprogram-tooAzure tar endast några sekunder toocomplete. När din programmet kan påbörjas kan du visa en länk med namnet **publicerade** i hello **Status** kolumn. När du klickar på länken hello tar du tooyour distribueras Web App startsidan eller du kan använda hello steg i hello följande avsnitt toobrowse tooyour webbprogram.

## <a name="browsing-tooyour-web-app-on-azure"></a>Bläddra tooyour Web App på Azure
toobrowse tooyour Web App på Azure, kan du använda hello **Azure Explorer** vyn.

Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **visa** -menyn i IntelliJ, klicka på **verktyget Windows**, och klicka sedan på  **Tjänsten Explorer**. Om du inte tidigare har loggat in uppmanas du toodo så.

När hello **Azure Explorer** visas, Använd följer du dessa steg toobrowse tooyour Web App: 

1. Expandera hello **Azure** nod.
2. Expandera hello **Web Apps** nod. 
3. Högerklicka på hello önskat webbprogram.
4. På snabbmenyn för hello visas **öppna i webbläsare**.
   
    ![Bläddra Webbapp][17]

## <a name="updating-your-web-app"></a>Uppdatera din webbapp
Uppdatering av ett befintligt kör Azure Web App är en snabb och enkel process och har du två alternativ för att uppdatera:

* Du kan uppdatera hello distribution av en befintlig Java-Webbapp.
* Du kan publicera en ytterligare Java-program toohello Web Appbehållare.

I båda fallen hello processen är identiska och tar endast några sekunder:

1. Högerklicka på hello Java-program du vill tooupdate eller lägga till tooan befintliga Web Appbehållare i hello IntelliJ Projektutforskaren.
2. Välj när hello snabbmenyn visas **Azure** och sedan **Publicera som Azure Web App...**
3. Eftersom du har redan loggat in tidigare, visas en lista över dina befintliga Web App-behållare. Välj hello en du vill toopublish eller publicera din Java-programmet tooand klickar **OK**.

Några sekunder senare hello **Azure-aktivitetsloggen** vyer visar uppdaterade distributionen som **publicerade** och du kommer att kunna tooverify uppdaterade programmet i en webbläsare.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Starta, stoppa eller starta om en befintlig webbapp
toostart eller stoppa en befintlig Azure Web App-behållare (inklusive alla hello distribueras Java-program i den), kan du använda hello **Azure Explorer** vyn.

Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **visa** -menyn i IntelliJ, klicka på **verktyget Windows**, och klicka sedan på  **Tjänsten Explorer**. Om du inte tidigare har loggat in uppmanas du toodo så.

När hello **Azure Explorer** visas, Använd följer du dessa steg toostart eller stoppa ditt webbprogram: 

1. Expandera hello **Azure** nod.
2. Expandera hello **Web Apps** nod. 
3. Högerklicka på hello önskat webbprogram.
4. På snabbmenyn för hello visas **starta**, **stoppa**, eller **starta om**. Observera att hello menyalternativen kontextmedvetna, så att du kan bara stoppa en webbapp som körs eller starta ett webbprogram som inte körs.
   
    ![Stoppa webbprogram][18]

## <a name="next-steps"></a>Nästa steg
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
* [Azure Toolkit för IntelliJ]
  * [installerar hello Azure Toolkit för IntelliJ]
  * *Skapa en Hello World-Webbapp för Azure i IntelliJ (den här artikeln)*
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]

<a name="see-also"></a>

## <a name="see-also"></a>Se även
Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

Mer information om hur du skapar Azure Web Apps finns hello [översikt över Web Apps].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[installerar hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[översikt över Web Apps]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
