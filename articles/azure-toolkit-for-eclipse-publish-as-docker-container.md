---
title: "aaaPublish en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>Publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för Eclipse

Docker-behållare är en mycket vanlig metod för att distribuera webbprogram. Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution tooa server. hello Azure Toolkit för Eclipse förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution tooMicrosoft Azure. Den här artikeln vägleder dig genom hello steg krävs toopublish program-tooAzure som Docker-behållare.

> [!NOTE]
> Mer information om Docker är tillgänglig på hello [Docker webbplats].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publicera din web app tooAzure med hjälp av en dockerbehållare

1. Öppna projektet web app i Eclipse.

2. toostart hello **Publicera som Dockerbehållare** guiden gör du något av följande hello:

   * I hello **Navigator** visa, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**.

      ![Visa Navigator Publicera som Dockerbehållare kommando][PUB01]

   * Verktygsfältet hello Eclipse och klicka på hello **publicera** knappen och klicka sedan på **Publicera som Dockerbehållare**.

      ![Eclipse verktygsfältet Publicera som Dockerbehållare kommando][PUB02]
      
    Hej **distribuera Dockerbehållare på Azure** öppnas guiden.

    ![hello distribuera Dockerbehållare på Azure-guiden][PUB03]

3. I hello **skriver du ett namn, Välj hello artefakt sökväg och kontrollera en Docker värden toobe används** fönstret hello följande:

    a. I hello **Docker avbildningsnamn** ange ett unikt namn för Docker-värden. (hello guiden skapar automatiskt ett namn, men du kan ändra den.)

    b. Hej **värdar** området visas alla Docker-värdar som du redan har skapat. Gör något av följande hello:

    * Om du har en befintlig Docker-värd kan distribuera du din web app tooit.
    * toocreate nytt Docker-värd, klickar du på **Lägg till**.  
      
    Hej **skapa Docker värden** öppnas.

    ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. I hello **konfigurera hello ny virtuell dator** fönstret Ange hello följande alternativ för Docker-värden. (hello guiden genererar automatiskt de flesta av hello alternativ för dig, men du kan ändra någon av dem.)

   a. **Namnet**: Ange ett unikt namn för hello Docker-värden. (Det är inte hello samma som hello Docker avbildningens namn som du angav tidigare.)

   b. **Prenumerationen**: Ange hello Azure-prenumeration som du använder för värden.

   c. **Region**: Ange hello geografiska region där värden finns.

   d. På hello **Värdoperativsystem och storlek** fliken:
     * **Värd för OS**: Ange hello operativsystemet för hello virtuell dator som innehåller värden.
     * **Storlek**: Ange hello storlek för virtuell dator för värden.

   e. På hello **resursgruppen** fliken:
     * **Ny resursgrupp**: skapa en ny resursgrupp för värden.
     * **Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto.

   f. På hello **nätverk** fliken:
     * **Nytt virtuellt nätverk**: skapa ett nytt virtuellt nätverk för värden.
     * **Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto.

   g. På hello **lagring** fliken:
     * **Nytt lagringskonto**: skapa ett nytt lagringskonto för värden.
     * **Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.

5. Klicka på **Nästa**.

6. I hello **Konfigurera logg i autentiseringsuppgifter och portinställningarna** fönster, Välj något av följande alternativ för hello:

    * **Importera autentiseringsuppgifter från Azure Key Vault**: Anger en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.

      >[!NOTE]
      >Ett Azure Key Vault som har skapats med ett visst konto eller huvudnamn för tjänsten är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar hello prenumeration. tooallow ett annat konto eller service principal toouse Hej Key Vault, måste du använda hello Azure portal tooadd hello konto eller tjänstens huvudnamn.

    * **Ny logg i autentiseringsuppgifter**: skapar en ny uppsättning autentiseringsuppgifter för inloggning. Om du väljer det här alternativet hello följande:
    
      * På hello **VM autentiseringsuppgifter** väljer du något av följande hello alternativ för hello virtuella datorer inloggningsuppgifterna för Docker-värd:

          * **Användarnamnet**: Ange hello användarnamn för dina inloggningsuppgifter för virtuell dator.
          * **Lösenordet** och **Bekräfta**: Ange hello lösenord för dina inloggningsuppgifter för virtuell dator.
          * **SSH**: Ange inställningar för hello SSH (Secure Shell) för Docker-värden. Du kan välja mellan följande alternativ för hello:
            * **Ingen**: Anger att den virtuella datorn inte tillåter att SSH-anslutningar.
            * **Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via SSH.
            * **Importera från directory**: Anger en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar. hello directory måste innehålla hello följande två filer:
                * *id_rsa*: innehåller hello RSA-identifiering för en användare.
                * *id_rsa.pub*: innehåller hello offentliga RSA-nyckel som används för autentisering.
        
        ![Skapa Docker-värd][PUB05]

      * På hello **Docker Daemon autentiseringsuppgifter** anger hello följande alternativ:

          * **Docker Daemon port**: Ange hello unika TCP-port för Docker-värden.
          * **TLS-säkerhet**: Ange hello Transport Layer Security-inställningar för Docker-värden. Du kan välja mellan följande alternativ för hello:
            * **Ingen**: Anger att den virtuella datorn inte tillåter att TLS-anslutningar.
            * **Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via TLS.
            * **Importera från directory**: Anger en katalog som innehåller en uppsättning tidigare sparade TLS-inställningar. Mer specifikt måste hello directory innehålla hello följande sex filer:
                * *CA.PEM* och *ca-key.pem*: innehålla hello certifikat och offentlig nyckel för hello TLS-certifikatets utfärdare.
                * *CERT.PEM* och *key.pem*: innehålla hello klientcertifikatet och en offentlig nyckel som används för TLS-autentisering.
                * *Server.PEM* och *server key.pem*: innehålla hello servercertifikat och en offentlig nyckel för hello värden.

        ![Skapa Docker-värd][PUB06]

7. När du har angett alla hello föregående information klickar du på **Slutför**.

8. I hello **distribuera Dockerbehållare på Azure** guiden, klickar du på **nästa**.

   ![hello distribuera Dockerbehållare på Azure-guiden][PUB07]

9. I hello **konfigurera hello Docker behållare toobe skapade** fönstret hello följande:

   a. I hello **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.

   b. Välj något av hello följande Docker bilder:
     * **Fördefinierade Docker bild**: Anger en befintlig Docker-avbildning från Azure.

       >[!NOTE]
       >hello listan med Docker bilder i den här rutan består av flera avbildningar som hello Azure Toolkit har konfigurerats toopatch så att din artefakt distribueras automatiskt.

     * **Anpassade Dockerfile**: Anger en tidigare sparad Dockerfile från den lokala datorn.

       >[!NOTE]
       >Detta är en avancerad funktion för utvecklare som vill toodeploy sina egna Dockerfile. Dock är det upp toodevelopers som använder det här alternativet tooensure som deras Dockerfile är uppbyggd. hello Azure Toolkit kan inte valideras hello innehållet i en anpassad Dockerfile så hello distributionen misslyckas om hello Dockerfile har problem. Dessutom hello Azure Toolkit förväntar hello anpassade Dockerfile toocontain en web app artefakt och försök tooopen en HTTP-anslutning. Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.

   c. **Portinställningarna**: Ange hello unik TCP-port bindning för Docker-behållare.

     ![hello konfigurera hello Docker behållare toobe skapade fönster][PUB08]

10. När du har slutfört alla hello föregående steg klickar du på **Slutför**.

hello Azure Toolkit börjar distribuera din web app tooAzure i en dockerbehållare. 

## <a name="next-steps"></a>Nästa steg
Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * [Logga in anvisningar hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga in instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].

För ytterligare resurser för Docker Se hello officiella [Docker webbplats].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webbplats]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png