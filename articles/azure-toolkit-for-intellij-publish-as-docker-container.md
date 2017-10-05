---
title: "Publicera en dockerbehållare med hjälp av Azure-verktyget för IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>Publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ

Docker-behållare är en mycket vanlig metod för att distribuera webbprogram. Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution till en server. Azure-verktygen för IntelliJ förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution till Microsoft Azure. Den här artikeln vägleder dig igenom de steg som krävs för att publicera dina program till Azure som Docker-behållare.

> [!NOTE]
>
> Mer information om Docker är tillgängligt på den [Docker webbplats].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publicera webbappen till Azure med hjälp av en dockerbehållare

> [!NOTE]
> * För att publicera ditt webbprogram, måste du skapa en distribution redo artefakt. Mer information finns i [ytterligare information om hur du skapar artefakter](#artifacts) avsnitt.
>
> * När du har slutfört guiden minst en gång, är de flesta inställningarna används som standard när du kör guiden igen.
>

1. Öppna din webbappsprojektet i IntelliJ.

2. Starta den **Publicera som Dockerbehållare** guiden gör du något av följande:

   * I den **projekt** verktyg fönstret, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**:

      ![Publicera som Dockerbehållare kommando][PUB01]

   * Klicka på verktygsfältet IntelliJ den **publicera grupp** knappen och klicka sedan på **Publicera som Dockerbehållare**:

      ![Publicera som Dockerbehållare kommando][PUB02]  
    Den **distribuera Dockerbehållare på Azure** öppnas guiden.

   ![Dockerbehållare distribuera på Azure-guiden][PUB03]

3. I den **skriver du ett namn, Välj den artefakt sökväg och kontrollera en Docker-värden som ska användas** fönster, gör du följande: 

   a. I den **Docker avbildningsnamn** ange ett unikt namn för Docker-värden. (Skapar guiden automatiskt ett namn, men du kan ändra den.) 

   b. Den **värdar** området visas alla Docker-värdar som du redan har skapat. Gör något av följande: 
      * Om du har en befintlig Docker-värd kan distribuera du ditt webbprogram till den.
      * Klicka för att skapa en Docker-värd grönt plustecken (**+**).  
       Den **skapa Docker värden** öppnas. 

      ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. I den **konfigurera den nya virtuella datorn** och ange följande information om Docker-värd. (Guiden genererar automatiskt de flesta av information för att du, men du kan ändra någon av dem.) 

   a. I den **namn** ange ett unikt namn för Docker-värden. (Det är inte samma som Docker avbildningens namn som du angav tidigare.) 
    
   b. I den **prenumeration** ange Azure-prenumeration som du använder för värden. 
      
   c. I den **Region** ange den geografiska region där värden finns.
      
   d. På den **OS- och** gör följande:      
      * **Värd för OS**: Ange operativsystemet för den virtuella datorn som innehåller värden. 
      * **Storlek**: Ange storleken på virtuella datorn för värden.   
       
   e. På den **resursgruppen** väljer du något av följande:      
      * **Ny resursgrupp**: skapa en resursgrupp för värden.
      * **Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto. 
       
   f. På den **nätverk** väljer du något av följande:      
      * **Nytt virtuellt nätverk**: skapa ett virtuellt nätverk för värden.
      * **Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto. 
       
   g. På den **lagring** väljer du något av följande:      
      * **Nytt lagringskonto**: skapa ett lagringskonto för värden.
      * **Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.
       
5. Klicka på **Nästa**.  
     Den **Konfigurera logg i autentiseringsuppgifter och portinställningarna** öppnas.

      ![Konfigurera loggen i autentiseringsuppgifter och port inställningar][PUB05]

6. Välj något av följande alternativ:

      * **Importera autentiseringsuppgifter från Azure Key Vault**: Ange en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.

          > [!NOTE]
          > Ett Azure key vault som har skapats med ett visst konto eller tjänstens huvudnamn är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar prenumerationen. För att ett annat konto eller service principal att använda nyckelvalvet, måste du använda Azure-portalen för att lägga till kontot eller tjänstens huvudnamn.

      * **Ny logg i autentiseringsuppgifter**: skapa en ny uppsättning autentiseringsuppgifter för inloggning. Om du väljer det här alternativet måste du göra följande:

        a. På den **VM autentiseringsuppgifter** och ange följande information för virtuell dator inloggningsuppgifterna för Docker-värd: * **användarnamn**: Ange användarnamnet för dina inloggningsuppgifter för virtuella datorer.
             * **Lösenordet** och **Bekräfta**: Ange lösenordet för dina inloggningsuppgifter för virtuella datorer.
             * **SSH**: Ange inställningar för SSH (Secure Shell) för Docker-värden. Du kan välja något av följande alternativ: * **ingen**: Anger att den virtuella datorn inte tillåter SSH-anslutningar.
                * **Autogenerera**: skapar automatiskt de nödvändiga inställningarna för att ansluta via SSH.
                * **Importera från directory**: kan du ange en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar. Katalogen måste innehålla följande två filer:
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        b. På den **Docker Daemon åtkomst** och ange följande information:

          ![Skapa Docker-värd][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. När du har angett informationen som krävs, klickar du på **Slutför**.  
    Den **distribuera Dockerbehållare på Azure** guiden visas igen.

   ![Distribuera Dockerbehållare på Azure-guiden][PUB07]

8. Klicka på **Nästa**.  
    Den **konfigurera dockerbehållare skapas** öppnas.

   ![Konfigurera Docker-behållare skapas fönster][PUB08]

9. I den **konfigurera dockerbehållare skapas** och ange följande information: 

   a. I den **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.

   b. Välj något av följande Docker-bilder: 

      * **Fördefinierade Docker bild**: Ange en befintlig Docker-avbildning från Azure. 

        > [!NOTE]
        > Listan med Docker bilder i den här rutan består av flera avbildningar som Azure-verktygen har konfigurerats för att korrigera så att din artefakt distribueras automatiskt. 

      * **Anpassade Dockerfile**: Ange en tidigare sparad Dockerfile från den lokala datorn.

        > [!NOTE]
        > Detta är en avancerad funktion för utvecklare som vill distribuera sina egna Dockerfile. Men är det utvecklare som använder det här alternativet för att säkerställa att deras Dockerfile är uppbyggd. Eftersom Azure-verktyget inte validerar innehållet i en anpassad Dockerfile, kan distributionen misslyckas om Dockerfile har problem. Dessutom eftersom Azure Toolkit förväntar anpassade Dockerfile ska innehålla en web app artefakt, försöker öppna en HTTP-anslutning. Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.

   c. I den **portinställningarna** ange unika TCP-portbindningen för Docker-behållare. 

10. När du har slutfört föregående steg klickar du på **Slutför**. 

Azure-Toolkit börjar distribuera webbappen till Azure i en dockerbehållare. Om du har konfigurerat IntelliJ som ska distribueras i bakgrunden, en **distribution till Azure** förloppsindikatorn visas. 

![Förloppsindikatorn för distribution][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Mer information om hur du skapar artefakter

Om du vill skapa en artefakt distribution är redo att göra följande:

1. Öppna din webbappsprojektet i IntelliJ.

2. Klicka på **filen**, och klicka sedan på **projektstruktur**.

   ![Kommandot projektstruktur][ART01]

3. Lägg till en artefakt, klicka på grönt plustecken (**+**), och klicka sedan på **webbprogrammet: Arkiv**.

   ![Kommandot ”program: Webbarkiv”][ART02]

4. I den **namn** anger du ett namn för din artefakt (inkluderar inte den *.war* tillägget), och klicka sedan på **OK**.

   ![Rutan artefakt][ART03]

Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på webbplatsen JetBrains.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure-verktyg för Java IDEs finns i följande resurser:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i Azure-verktygen för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * [Logga in-instruktioner för Azure-verktygen för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i Azure-verktygen för IntelliJ]
  * [Installera Azure Toolkit för IntelliJ]
  * [Logga in instruktioner för Azure-Toolkit för IntelliJ]
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].

Ytterligare resurser för Docker finns i officiellt [Docker webbplats].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in-instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för Azure-Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webbplats]: https://www.docker.com/
[konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
