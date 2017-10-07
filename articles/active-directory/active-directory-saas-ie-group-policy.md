---
title: "aaaDeploy Azure Access panelen-tillägg för Internet Explorer med hjälp av ett grupprincipobjekt | Microsoft Docs"
description: "Hur toouse gruppera princip toodeploy hello Internet Explorer-tillägget för hello Mina appar portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a>Hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip
Den här kursen visar hur toouse grupp princip tooremotely installera hello åtkomstpanelen tillägg för Internet Explorer på användarnas datorer. Det här tillägget krävs för Internet Explorer-användare som behöver toosign i appar som har konfigurerats med hjälp av [lösenordsbaserade enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Vi rekommenderar att administratörer automatisera hello distribution av det här tillägget. Annars användare har toodownload och installera tillägg för hello själva, som är lätt toouser fel och måste ha administratörsbehörighet. Den här självstudiekursen beskriver en metod för att automatisera programvarudistributioner med hjälp av Grupprincip. [Läs mer om grupprinciper.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Hej åtkomstpanelen tillägg är också tillgängligt för [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) och [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), som inte kräver att administratören behörigheter tooinstall.

## <a name="prerequisites"></a>Krav
* Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer tooyour domän.
* Du måste ha hello ”redigera inställningar för” behörighet tooedit hello grupprincipobjektet (GPO). Som standard medlemmar i hello följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip. [Läs mer.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a>Steg 1: Skapa hello distributionsplats
Först måste du placera hello installer-paketet på en nätverksplats som kan nås av hello datorer som du vill tooremotely installera hello tillägg på. toodo, gör du följande:

1. Logga in på toohello server som administratör
2. I hello **Serverhanteraren** fönstret gå för**filer och lagringstjänster**.
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Gå toohello **resurser** fliken. Klicka på **uppgifter** > **ny resurs...**
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/shares.png)
4. Fullständig hello **ny resurs Guide** och ange behörigheter tooensure kan nås från användarnas datorer. [Mer information om resurser.](https://technet.microsoft.com/library/cc753175.aspx)
5. Hämta hello följande Microsoft Windows Installer-paketet (MSI-fil): [åtkomst panelen Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)
6. Kopiera hello installer tooa önskad plats för hello-resursen.
   
    ![Kopiera hello .msi toohello filresurs.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. Kontrollera att dina klientdatorer kan tooaccess hello installationspaketet från hello resurs. 

## <a name="step-2-create-hello-group-policy-object"></a>Steg 2: Skapa hello grupprincipobjekt
1. Logga in toohello-server som är värd för Active Directory Domain Services (AD DS)-installationen.
2. Hello Serverhanteraren, gå för**verktyg** > **Grupprinciphantering**.
   
    ![Gå tooTools > Group Policy Management](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. Hello vänster i hello **Grupprinciphantering** fönstret Visa organisationsenheten (OU) hierarkin och avgöra vilka definitionsområdet som tooapply hello Grupprincip. Exempelvis kan du välja toopick en liten OU toodeploy tooa några användare för att testa eller du kan välja en översta OU toodeploy tooyour hela organisationen.
   
   > [!NOTE]
   > Om du skulle t.ex. toocreate eller redigera dina organisationsenheter (OU), växlar tillbaka toohello Serverhanteraren och gå för**verktyg** > **Active Directory-användare och datorer**.
   > 
   > 
4. När du har valt en Organisationsenhet, högerklicka och välj **skapa ett grupprincipobjekt i den här domänen och länka det här...**
   
    ![Skapa ett nytt GPO](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. I hello **nytt GPO** snabb, Skriv ett namn för hello nytt grupprincipobjekt.
   
    ![Namnet hello nytt GPO](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Högerklicka på hello grupprincipobjekt som du skapade och välj **redigera**.
   
    ![Redigera hello nytt GPO](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a>Steg 3: Tilldela hello installationspaketet
1. Avgöra om du vill att toodeploy hello tillägget baserat på **Datorkonfiguration** eller **Användarkonfiguration**. När du använder [Datorkonfiguration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), hello tillägg har installerats på hello datorn oavsett vilka användare som loggar in tooit. Med [Användarkonfiguration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), användare ha hello-tillägg som installerats för dem oavsett vilka datorer de loggar in på.
2. Hello vänster i hello **Redigeraren för Grupprinciphantering** fönstret gå tooeither av hello mappsökvägar, beroende på vilken typ av du har valt följande:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Högerklicka på **Programvaruinstallation**och välj **ny** > **paket...**
   
    ![Skapa en ny installationspaket för programvara](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Gå toohello delad mapp som innehåller hello installationspaketet från [steg 1: skapa hello distributionsplats](#step-1-create-the-distribution-point), Välj hello MSI-filen och klicka på **öppna**.
   
   > [!IMPORTANT]
   > Om hello resursen finns på den här samma server, kontrollerar du att du använder hello .msi via hello nätverkssökvägen fil i stället för hello lokal filsökväg.
   > 
   > 
   
    ![Välj hello installationspaketet från hello delad mapp.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. I hello **distribuera programvara** fråga, Välj **tilldelad** för din distributionsmetod. Klicka sedan på **OK**.
   
    ![Välj tilldelad och klicka sedan på OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

hello tillägget är nu distribuerade toohello Organisationsenhet som du har valt. [Läs mer om Programvaruinstallation för Grupprincip.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a>Steg 4: Aktivera automatiskt hello tillägg för Internet Explorer
Dessutom toorunning hello installationsprogrammet för alla tillägg för Internet Explorer måste uttryckligen aktiveras innan den kan användas. Följ hello stegen nedan tooenable hello åtkomst panelen tillägget med hjälp av Grupprincip:

1. I hello **Redigeraren för Grupprinciphantering** gå tooeither av hello följande sökvägar, beroende på vilken typ av du valde i fönstret [steg3: tilldela hello installationspaketet](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Högerklicka på **lista över tillägg**, och välj **redigera**.
    ![Redigera lista över tillägg.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. I hello **lista över tillägg** väljer **aktiverad**. Sedan, under hello **alternativ** klickar du på **visa...** .
   
    ![Klicka på Aktivera och klicka sedan på Visa...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. I hello **Visa innehåll** fönstret utföra hello följande steg:
   
   1. För hello första kolumnen (hello **värdenamn** fältet), kopiera och klistra in hello efter klass-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. För andra kolumnen i hello (hello **värdet** fältet), ange hello följande värde:`1`
   3. Klicka på **OK** tooclose hello **Visa innehåll** fönster.
      
      ![Fyll i hello värden som anges ovan.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Klicka på **OK** tooapply dina ändringar och Stäng hello **lista över tillägg** fönster.

hello tillägget bör nu vara aktiverat för hello datorer i hello valda Organisationsenheten. [Lär dig mer om hur du använder gruppen princip tooenable eller inaktivera tillägg för Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Steg 5 (valfritt): inaktivera ”Kom ihåg lösenord” fråga
När användare loggar in toowebsites med hello tillägget för åtkomst-panelen, Internet Explorer kan fråga visa hello följande frågar ”vill du som toostore ditt lösenord”?

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Om du vill tooprevent användarna ser den här uppmaningen sedan gör hello nedan tooprevent automatisk komplettering från komma ihåg lösenord:

1. I hello **Redigeraren för Grupprinciphantering** fönster, gå toohello sökväg som anges nedan. Den här inställningen är endast tillgängliga under **Användarkonfiguration**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. Hitta hello inställning med namnet **aktivera hello automatisk komplettering funktionen för användarnamn och lösenord för formulär för**.
   
   > [!NOTE]
   > Tidigare versioner av Active Directory kan visa en lista med den här inställningen med hello namnet **tillåter inte automatisk komplettering toosave lösenord**. hello-konfigurationen för den här inställningen skiljer sig från hello inställningen beskrivs i den här kursen.
   > 
   > 
   
    ![Kom ihåg toolook för detta under inställningar.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Högerklicka på hello ovan inställningen och välj **redigera**.
4. I fönstret hello **aktivera hello automatisk komplettering funktionen för användarnamn och lösenord för formulär för**väljer **inaktiverade**.
   
    ![Välj inaktivera](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Klicka på **OK** tooapply dessa ändringar och Stäng hello-fönstret.

Användare kommer inte längre kan toostore sina autentiseringsuppgifter eller använda automatisk komplettering tooaccess tidigare lagrade autentiseringsuppgifter. Den här principen tillåter dock att användare toocontinue toouse automatisk komplettering för andra typer av fält, till exempel sökfält.

> [!WARNING]
> Om den här principen är aktiverad när användare har valt toostore vissa autentiseringsuppgifter kan den här principen ska *inte* ta bort hello-autentiseringsuppgifter som redan har lagrats.
> 
> 

## <a name="step-6-testing-hello-deployment"></a>Steg 6: Testa hello distribution
Gör hello nedan tooverify om hello tillägget distributionen lyckades:

1. Om du har distribuerat med **Datorkonfiguration**, logga in på en klientdator som tillhör toohello Organisationsenhet som du valde i [steg 2: skapa hello grupprincipobjekt](#step-2-create-the-group-policy-object). Om du har distribuerat med **Användarkonfiguration**, se till att toosign i som en användare som tillhör toothat Organisationsenhet.
2. Det kan ta ett par logga moduler för gruppolicy hello ändras toofully uppdateringen till den här datorn. tooforce hello uppdatering, öppna en **kommandotolk** fönster och kör hello följande kommando:`gpupdate /force`
3. Du måste starta om datorn hello hello installation tootake plats. Autostart kan ta betydligt längre tid än vanligt när hello tillägget installeras.
4. Efter omstart, öppna **Internet Explorer**. Klicka på hello övre högra hörnet av hello **verktyg** (hello växeln ikonen) och välj sedan **Hantera tillägg**.
   
    ![Gå tooTools > Hantera tillägg](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. I hello **Hantera tillägg** fönster, kontrollera att hello **åtkomst panelen tillägget** har installerats och att dess **Status** har ställts in för**aktiverad**.
   
    ![Kontrollera att hello åtkomst panelen tillägget är installerat och aktiverat.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Felsöka hello Access panelen-tillägg för Internet Explorer](active-directory-saas-ie-troubleshooting.md)

