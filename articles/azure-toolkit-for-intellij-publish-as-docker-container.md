---
title: "aaaPublish en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>Publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ

Docker-behållare är en mycket vanlig metod för att distribuera webbprogram. Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution tooa server. hello Azure Toolkit för IntelliJ förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution tooMicrosoft Azure. Den här artikeln vägleder dig genom hello steg krävs toopublish program-tooAzure som Docker-behållare.

> [!NOTE]
>
> Mer information om Docker är tillgänglig på hello [Docker webbplats].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publicera din web app tooAzure med hjälp av en dockerbehållare

> [!NOTE]
> * toopublish ditt webbprogram måste du skapa en distribution redo artefakt. toolearn finns fler hello [ytterligare information om hur du skapar artefakter](#artifacts) avsnitt.
>
> * När du har slutfört guiden för distribution av hello minst en gång, används de flesta av inställningarna som standard när du kör hello guiden igen.
>

1. Öppna din webbappsprojektet i IntelliJ.

2. toostart hello **Publicera som Dockerbehållare** guiden gör du något av följande hello:

   * I hello **projekt** verktyg fönstret, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**:

      ![hello Publicera som Dockerbehållare kommando][PUB01]

   * Hello på hello IntelliJ verktygsfältet **publicera grupp** knappen och klicka sedan på **Publicera som Dockerbehållare**:

      ![hello Publicera som Dockerbehållare kommando][PUB02]  
    Hej **distribuera Dockerbehållare på Azure** öppnas guiden.

   ![hello distribuera Dockerbehållare på Azure-guiden][PUB03]

3. I hello **skriver du ett namn, Välj hello artefakt sökväg och kontrollera en Docker värden toobe används** fönstret hello följande: 

   a. I hello **Docker avbildningsnamn** ange ett unikt namn för Docker-värden. (hello guiden skapar automatiskt ett namn, men du kan ändra den.) 

   b. Hej **värdar** området visas alla Docker-värdar som du redan har skapat. Gör något av följande hello: 
      * Om du har en befintlig Docker-värd kan distribuera du din web app tooit.
      * toocreate en Docker-värd, klicka hello grönt plustecken (**+**).  
       Hej **skapa Docker värden** öppnas. 

      ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. I hello **konfigurera hello ny virtuell dator** fönstret Ange hello följande information om Docker-värden. (hello guiden genererar automatiskt de flesta av hello-information, men du kan ändra någon av dem.) 

   a. I hello **namn** ange ett unikt namn för hello Docker-värden. (Det är inte hello samma som hello Docker avbildningens namn som du angav tidigare.) 
    
   b. I hello **prenumeration** ange hello Azure-prenumeration som du använder för värden. 
      
   c. I hello **Region** ange hello geografisk region där värden finns.
      
   d. På hello **OS- och** fliken, hello följande:      
      * **Värd för OS**: Ange hello operativsystemet för hello virtuell dator som innehåller värden. 
      * **Storlek**: Ange hello storlek för virtuell dator för värden.   
       
   e. På hello **resursgruppen** väljer du något av följande hello:      
      * **Ny resursgrupp**: skapa en resursgrupp för värden.
      * **Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto. 
       
   f. På hello **nätverk** väljer du något av följande hello:      
      * **Nytt virtuellt nätverk**: skapa ett virtuellt nätverk för värden.
      * **Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto. 
       
   g. På hello **lagring** väljer du något av följande hello:      
      * **Nytt lagringskonto**: skapa ett lagringskonto för värden.
      * **Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.
       
5. Klicka på **Nästa**.  
     Hej **Konfigurera logg i autentiseringsuppgifter och portinställningarna** öppnas.

      ![hello Konfigurera logg i autentiseringsuppgifter och port inställningar][PUB05]

6. Välj något av följande alternativ för hello:

      * **Importera autentiseringsuppgifter från Azure Key Vault**: Ange en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.

          > [!NOTE]
          > Ett Azure key vault som har skapats med ett visst konto eller tjänstens huvudnamn är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar hello prenumeration. tooallow ett annat konto eller service principal toouse hello nyckeln valvet, måste du använda hello Azure portal tooadd hello konto eller tjänstens huvudnamn.

      * **Ny logg i autentiseringsuppgifter**: skapa en ny uppsättning autentiseringsuppgifter för inloggning. Om du väljer det här alternativet hello följande:

        a. På hello **VM autentiseringsuppgifter** och ange följande information för hello inloggningsuppgifterna för virtuella datorer i din Docker-värd hello: * **användarnamn**: Ange hello användarnamn för virtuell dator-inloggning autentiseringsuppgifter.
             * **Lösenordet** och **Bekräfta**: Ange hello lösenord för dina inloggningsuppgifter för virtuella datorer.
             * **SSH**: Ange inställningar för hello SSH (Secure Shell) för Docker-värden. Du kan välja något av följande alternativ för hello: * **ingen**: Anger att den virtuella datorn inte tillåter SSH-anslutningar.
                * **Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via SSH.
                * **Importera från directory**: tillåter toospecify en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar. hello directory måste innehålla hello följande två filer:
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        b. På hello **Docker Daemon åtkomst** fliken, ange hello följande information:

          ![Skapa Docker-värd][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. När du har angett hello krävs information klickar du på **Slutför**.  
    Hej **distribuera Dockerbehållare på Azure** guiden visas igen.

   ![Distribuera Dockerbehållare på Azure-guiden][PUB07]

8. Klicka på **Nästa**.  
    Hej **konfigurera hello Docker behållare toobe skapade** öppnas.

   ![hello konfigurera hello Docker behållare toobe skapade fönster][PUB08]

9. I hello **konfigurera hello Docker behållare toobe skapade** fönstret Ange hello följande information: 

   a. I hello **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.

   b. Välj något av hello följande Docker bilder: 

      * **Fördefinierade Docker bild**: Ange en befintlig Docker-avbildning från Azure. 

        > [!NOTE]
        > hello listan med Docker bilder i den här rutan består av flera avbildningar som hello Azure Toolkit har konfigurerats toopatch så att din artefakt distribueras automatiskt. 

      * **Anpassade Dockerfile**: Ange en tidigare sparad Dockerfile från den lokala datorn.

        > [!NOTE]
        > Detta är en avancerad funktion för utvecklare som vill toodeploy sina egna Dockerfile. Dock är det upp toodevelopers som använder det här alternativet tooensure som deras Dockerfile är uppbyggd. Eftersom hello Azure Toolkit inte kan valideras hello innehållet i en anpassad Dockerfile, misslyckas hello distributionen om hello Dockerfile har problem. Dessutom eftersom hello Azure Toolkit förväntar hello anpassade Dockerfile toocontain en web app artefakt, försöker den tooopen en HTTP-anslutning. Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.

   c. I hello **portinställningarna** ange hello unik TCP-port bindning för Docker-behållare. 

10. När du har slutfört föregående steg hello, klickar du på **Slutför**. 

hello Azure Toolkit börjar distribuera din web app tooAzure i en dockerbehållare. Om du har konfigurerat IntelliJ toobe distribuerats i hello bakgrund, en **distribuera tooAzure** förloppsindikatorn visas. 

![hello förloppsindikatorn för distribution][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Mer information om hur du skapar artefakter

toocreate distribution redo-artefakt hello följande:

1. Öppna din webbappsprojektet i IntelliJ.

2. Klicka på **filen**, och klicka sedan på **projektstruktur**.

   ![hello projektstruktur kommando][ART01]

3. tooadd en artefakt, klicka hello grönt plustecken (**+**), och klicka sedan på **webbprogrammet: Arkiv**.

   ![Hej ”program: webbarkiv” kommando][ART02]

4. I hello **namn** anger du ett namn för din artefakt (inkluderar inte hello *.war* tillägget), och klicka sedan på **OK**.

   ![hello artefakt namn.][ART03]

Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på hello JetBrains webbplats.

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

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

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
