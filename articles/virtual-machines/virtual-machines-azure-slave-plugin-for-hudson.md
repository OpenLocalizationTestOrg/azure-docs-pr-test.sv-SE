---
title: aaaHow toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration | Microsoft Docs
description: Beskriver hur toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration.
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a>Hur toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration
hello Azure slavserver plugin-program för Hudson kan tooprovision underordnade noder i Azure när distribuerade versioner.

## <a name="install-hello-azure-slave-plug-in"></a>Installera hello Azure slavserver plugin-program
1. I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.
2. I hello **hantera Hudson** klickar du på **hantera plugin-program**.
3. Klicka på hello **tillgänglig** fliken.
4. Klicka på **Sök** och skriv **Azure** toolimit hello listan toorelevant plugin-program.
   
    Om du väljer tooscroll hello listan med tillgängliga plugin-program, hittar du hello Azure slavserver plugin-programmet under hello **klusterhantering och distribuerade bygga** avsnitt i hello **andra** fliken.
5. Markera kryssrutan för hello för **Azure slavserver Plugin**.
6. Klicka på **Installera**.
7. Starta om Hudson.

Nu att plugin-programmet hello installeras vara hello nästa steg tooconfigure hello plugin-program med Azure-prenumeration profil och toocreate en mall som ska användas för att skapa hello VM för hello underordnade nod.

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurera hello Azure slavserver plugin-program med din prenumeration profil
En prenumeration profil också hänvisade tooas Publiceringsinställningar, är en XML-fil som innehåller säkra referenser och ytterligare information som du behöver toowork med Azure i din utvecklingsmiljö. tooconfigure hello Azure slavserver plugin-program, behöver du:

* Prenumerations-id
* Ett certifikat för din prenumeration

Dessa återfinns i din [prenumeration profil]. Nedan visas ett exempel på en profil för prenumerationen.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Följ dessa steg tooconfigure hello Azure slavserver plugin-program när du har din prenumeration profil.

1. I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.
2. Klicka på **konfigurera systemet**.
3. Bläddra nedåt hello sidan toofind hello **moln** avsnitt.
4. Klicka på **lägga till nya molntjänster > Microsoft Azure**.
   
    ![lägga till nya molntjänster][add new cloud]
   
    Detta visar hello fält där du behöver tooenter din prenumerationsinformation.
   
    ![Konfigurera profilen][configure profile]
5. Kopiera hello prenumerations-id och hantering av certifikat från profilen prenumeration och klistra in dem i hello relevanta fält.
   
    När du kopierar hello prenumerations-id och hantering av certifikat, **inte** citattecken hello som omsluter hello värden.
6. Klicka på **verifiera konfigurationen**.
7. När du har verifierats hello konfiguration, klickar du på **spara**.

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a>Ställ in en mall för virtuell dator för hello Azure slavserver plugin-program
En mall för virtuell dator definierar hello parametrar hello plugin-programmet använder toocreate en slav nod på Azure. I följande steg hello ska vi skapa mall för en Ubuntu VM.

1. I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.
2. Klicka på **konfigurera systemet**.
3. Bläddra nedåt hello sidan toofind hello **moln** avsnitt.
4. Inom hello **moln** avsnittet, hitta **Lägg till mall för virtuell dator i Azure** och klicka på hello **Lägg till** knappen.
   
    ![Lägg till mall för virtuell dator][add vm template]
5. Ange ett tjänstnamn för molnet i hello **namn** fältet. Om hello namn du refererar tooan befintlig molntjänst, kommer att tillhandahållas hello VM i tjänsten. Annars skapar Azure en ny.
6. I hello **beskrivning** , ange text som beskriver hello-mall som du skapar. Den här informationen är endast för dokumentation och används inte för att etablera en virtuell dator.
7. I hello **etiketter** anger **linux**. Den här etiketten är används tooidentify hello mall som du skapar och därefter används tooreference hello mallen när du skapar ett jobb för Hudson.
8. Välj en region där hello VM kommer att skapas.
9. Välj hello lämpliga VM-storlek.
10. Ange ett lagringskonto där hello VM kommer att skapas. Se till att den är i hello samma region som hello molntjänst som du ska använda. Om du vill att den nya lagring toobe skapas kan du lämna fältet tomt.
11. Tiden för datakvarhållning anger hello antalet minuter innan Hudson tar bort en inaktiv slavserver. Lämna detta på hello standardvärdet 60.
12. I **användning**, Välj hello lämpliga villkor när den här sekundära noden kommer att användas. Nu väljer **använda den här noden så mycket som möjligt**.
    
     Formuläret skulle nu se något liknande toothis:
    
     ![Mallkonfigurationen][template config]
13. I **Id eller bild-familjen** har toospecify vilka systemavbildning kommer att installeras på den virtuella datorn. Du kan välja från en lista över avbildningen familjer, eller så kan du ange en anpassad avbildning.
    
     Om du vill tooselect från en lista över avbildningen familjer, ange hello första tecknet (skiftlägeskänsligt) hello avbildningsnamn. Till exempel skriva **U** visas en lista över Ubuntu Server familjer. När du har valt hello listan använder Jenkins hello senaste versionen av systemavbildningen från den familjen vid etablering av den virtuella datorn.
    
     ![OS-familjen lista][OS family list]
    
     Om du har en anpassad avbildning som du vill toouse i stället ange hello namnet på den egna avbildningen. Anpassad avbildningsnamn visas inte i en lista så att du har tooensure som hello namn har angetts korrekt.    
    
     Den här kursen skriver **U** toobring visas en lista över Ubuntu avbildningar och väljer **Ubuntu Server 14.04 LTS**.
14. För **starta metoden**väljer **SSH**.
15. Kopiera hello skriptet nedan och klistra in i hello **Init skriptet** fältet.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     Hej **Init skriptet** ska köras efter hello virtuell dator skapas. I det här exemplet installerar Java, git och ant hello skript.
16. I hello **användarnamn** och **lösenord** fält, ange din önskade värden för hello administratörskontot som kommer att skapas på den virtuella datorn.
17. Klicka på **Kontrollera mallen** toocheck om hello-parametrar som du angav är giltiga.
18. Klicka på **Spara**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Skapa ett Hudson jobb som körs på en slav nod på Azure
I det här avsnittet ska du skapa en Hudson aktivitet som körs på en slav nod på Azure.

1. I hello Hudson instrumentpanelen, klickar du på **nytt jobb**.
2. Ange ett namn för hello jobb som du skapar.
3. Hello jobbtypen Välj **bygga ett ledigt format programvara jobb**.
4. Klicka på **OK**.
5. Hello jobbet konfiguration på sidan Välj **begränsa där du kan köra det här projektet**.
6. Välj **nod och etikett menyn** och välj **linux** (vi angett den här etiketten när du skapar hello mall för virtuell dator i hello föregående avsnitt).
7. I hello **skapa** klickar du på **Lägg till build steg** och välj **köra shell**.
8. Redigera hello följande skript, ersätter **{github kontonamn}**, **{ditt projektnamn}**, och **{projektkatalogen}** med lämpliga värden och klistra in hello Redigera skriptet i område för hello text som visas.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. Klicka på **Spara**.
10. I hello Hudson instrumentpanelen, hitta hello jobb som du just har skapat och klicka på hello **schemalägga en build** ikon.

Hudson sedan skapa en slav nod med hjälp av hello mall hello föregående avsnitt och köra hello-skript som du angav i hello build steg för den här uppgiften.

## <a name="next-steps"></a>Nästa steg
Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[prenumeration profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

