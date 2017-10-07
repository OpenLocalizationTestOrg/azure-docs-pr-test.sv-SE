---
title: "Azure AD Connect: Uppgradera från DirSync | Microsoft Docs"
description: "Lär dig hur tooupgrade från DirSync tooAzure AD Connect. Den här artikeln beskriver hello steg för att uppgradera från DirSync tooAzure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: baf52da7-76a8-44c9-8e72-33245790001c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 05572af410698deaa1392c8837bfcb749efc69e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-dirsync"></a>Azure AD Connect: Uppgradera från DirSync
Azure AD Connect är hello efterföljande tooDirSync. Du hittar hello sätt som du kan uppgradera från DirSync i det här avsnittet. Stegen fungerar inte om du ska uppgradera från en annan version av Azure AD Connect eller från Azure AD Sync.

Kontrollera innan du börjar installera Azure AD Connect, för[hämta Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) och krav för fullständig hello stegen i [Azure AD Connect: maskin- och](active-directory-aadconnect-prerequisites.md). I synnerhet vill tooread om hello följande, eftersom dessa områden är olika från DirSync:

* hello version som krävs för .net och PowerShell. Nyare versioner är obligatoriska toobe på hello server än DirSync behövs.
* hello proxyserverkonfiguration. Om du använder en proxy server tooreach hello internet, den här inställningen måste konfigureras innan du uppgraderar. DirSync används alltid hello konfigurerad proxyserver för hello användaren installerar den, men använder Azure AD Connect datorinställningar i stället.
* hello URL krävs toobe öppna i hello proxyserver. Grundläggande scenarier med de scenarier som stöds även av DirSync, är hello kraven hello samma. Om du vill toouse hello nya funktionerna som ingår i Azure AD Connect, måste vissa nya URL: er öppnas.

> [!NOTE]
> När du har aktiverat din nya Azure AD Connect-servern toostart synkroniserar ändringar tooAzure AD, måste du inte återställa toousing DirSync eller Azure AD Sync. Nedgradering från Azure AD Connect toolegacy klienter inklusive DirSync och Azure AD Sync stöds inte och kan leda tooissues, till exempel förlust av data i Azure AD.

Om du inte uppgraderar från DirSync läser du [relaterad dokumentation](#related-documentation) för andra scenarier.

## <a name="upgrade-from-dirsync"></a>Uppgradera från DirSync
Det finns olika alternativ för hello uppgradering beroende på din aktuella DirSync-distribution. Om hello förväntade tiden för uppgraderingen är mindre än tre timmar, är hello rekommendation toodo uppgradering på plats. Om hello förväntade tiden för uppgraderingen är mer än tre timmar, är hello rekommendation toodo en parallell distribution på en annan server. Uppskattningsvis att om du har fler än 50 000 objekt tar mer än tre timmar toodo hello uppgraderingen.

| Scenario |
| --- | --- |
| [Uppgradering på plats](#in-place-upgrade) |
| [Parallell distribution](#parallel-deployment) |

> [!NOTE]
> När du planerar tooupgrade från DirSync tooAzure AD Connect du inte avinstallera DirSync själv före uppgraderingen hello. Azure AD Connect läser och migrerar hello konfigurationen från DirSync och avinstallerar efter att ha kontrollerat hello-server.

**Uppgradering på plats**  
hello förväntad tid toocomplete hello uppgraderingen visas hello guiden. Den här uppskattningen baseras på hello antagandet att det tar tre timmar toocomplete en uppgradering för en databas med 50 000 objekt (användare, kontakter och grupper). Om hello antalet objekt i databasen är färre än 50 000, rekommenderar en uppgradering på plats med Azure AD Connect. Om du väljer toocontinue, tillämpas dina aktuella inställningar automatiskt under uppgraderingen och servern återupptar automatiskt aktiva synkroniseringen.

Om du vill toodo en konfigurationsmigrering och en parallell distribution kan du åsidosätta rekommendationen om hello på plats en uppgradering. Du kan till exempel ta hello möjlighet toorefresh hello maskinvara och operativsystem. Mer information finns i hello [parallell distribution](#parallel-deployment) avsnitt.

**Parallell distribution**  
En parallell distribution rekommenderas om du har fler än 50 000 objekt. Med den här distributionen undviker du risken för att användarna upplever fördröjningar i processerna. hello Azure AD Connect-installationen försöker tooestimate hello driftstopp för hello uppgradering, men om du har uppgraderat DirSync tidigare hello, din egen erfarenheter är sannolikt toobe hello bästa guide.

### <a name="supported-dirsync-configurations-toobe-upgraded"></a>Stöds DirSync konfigurationer toobe uppgraderas
hello följande konfigurationsändringar stöds med uppgraderade DirSync:

* Domän- och organisationsenhetsfiltrering
* Alternativt ID (UPN)
* Lösenordssynkronisering och Exchange-hybridinställningar
* Din skog/domän och dina Azure AD-inställningar
* Filtrering baserat på användarattribut

hello följande ändring kan inte uppgraderas. Om du har den här konfigurationen, blockeras hello uppgraderingen:

* DirSync-ändringar som inte stöds, t.ex. borttagna attribut och användningen av ett anpassat tilläggs-DLL

![Uppgraderingen har blockerats](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

I sådana fall hello rekommendation är tooinstall en ny Azure AD Connect-server i [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode) och kontrollera hello gamla DirSync och nya Azure AD Connect-konfigurationen. Tillämpa eventuella ändringar med en anpassad konfiguration. Mer information finns i [Anpassad Azure AD Connect Sync-konfiguration](active-directory-aadconnectsync-whatis.md).

hello-lösenord som används av DirSync för hello tjänstkonton kan inte hämtas och migreras inte. Dessa lösenord återställs under hello uppgraderingen.

### <a name="high-level-steps-for-upgrading-from-dirsync-tooazure-ad-connect"></a>Anvisningar för att uppgradera från DirSync tooAzure AD Connect
1. Välkommen tooAzure AD Connect
2. Analys av den aktuella DirSync-konfigurationen
3. Samla in lösenordet för globala Azure AD-administratörer
4. Samla in autentiseringsuppgifter för ett företagsadministratörskonto (används endast under hello installation av Azure AD Connect)
5. Installation av Azure AD Connect
   * Avinstallera DirSync (eller inaktivera det tillfälligt)
   * Installera Azure AD Connect
   * Starta en synkronisering om det behövs

Ytterligare steg krävs om:

* Du har en fullständig SQL-server – lokal eller fjärransluten
* Du har mer än 50 000 objekt som ska synkroniseras

## <a name="in-place-upgrade"></a>Uppgradering på plats
1. Starta hello Azure AD Connect-installationsprogrammet (MSI).
2. Läs och Godkänn toolicense villkoren och sekretesspolicyn.  
   ![Välkommen till tooAzure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Klicka på nästa toobegin analys av din befintliga DirSync-installation.  
   ![Analysera den befintliga Directory Sync-installationen](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. När hello analysen är klar visas hello rekommendationer om hur tooproceed.  
   * Om du använder SQL Server Express och har färre än 50 000 objekt visas följande skärm hello:  
     ![Analysen klar redo tooupgrade från DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
   * Om du använder en fullständig SQL Server för DirSync kan du få se den här sidan i stället:  
     ![Analysen klar redo tooupgrade från DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
     hello information om hello befintliga SQL Server-databasserver som används av DirSync visas. Gör relevanta justeringar om det behövs. Klicka på **nästa** toocontinue hello installation.
   * Om du har fler än 50 000 objekt kan se du få se den här skärmen i stället:  
     ![Analysen klar redo tooupgrade från DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
     tooproceed med uppgradering på plats klickar du på kryssrutan nästa toothis hälsningsmeddelande: **Fortsätt att uppgradera DirSync på den här datorn.**
     toodo en [parallell distribution](#parallel-deployment) i stället kan du exportera hello DirSync-konfigurationsinställningarna och flytta hello toohello nya konfigurationsservern.
5. Ange hello lösenord för hello-kontot som du använder för närvarande tooconnect tooAzure AD. Detta måste vara hello-konto som för närvarande används av DirSync.  
   ![Ange dina autentiseringsuppgifter för Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
   Om du får ett fel och har problem med anslutningen läser du [Felsöka anslutningsproblem](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Ange ett företagsadministratörskonto för Active Directory.  
   ![Ange dina ADDS-autentiseringsuppgifter](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Nu är du redo tooconfigure. När du klickar på **Uppgradera** avinstalleras DirSync och Azure AD Connect konfigureras och börjar synkronisera.  
   ![Redo tooconfigure](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. När hello-installationen har slutförts, logga ut och logga in igen tooWindows innan du använder Synchronization Service Manager eller Synchronization Rule Editor eller försök toomake andra konfigurationsändringar.

## <a name="parallel-deployment"></a>Parallell distribution
### <a name="export-hello-dirsync-configuration"></a>Exportera hello DirSync-konfiguration
**Parallell distribution med mer än 50 000 objekt**

Om du har mer än 50 000 objekt och sedan hello rekommenderar Azure AD Connect-installationen en parallell distribution.

En skärm liknande toohello följande visas:  
![Analysen är klar](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Om du vill tooproceed med den parallella distributionen måste tooperform hello följande steg:

* Klicka på hello **Exportera inställningar** knappen. När du installerar Azure AD Connect på en separat server migreras inställningarna från din aktuella DirSync tooyour nya Azure AD Connect-installationen.

När inställningarna har exporterats kan avsluta du hello Azure AD Connect-guiden på hello DirSync-servern. Fortsätt med nästa steg i hello för[installera Azure AD Connect på en separat server](#installation-of-azure-ad-connect-on-separate-server)

**Parallell distribution med mindre än 50 000 objekt**

Om du har färre än 50 000 objekt men ändå vill toodo en parallell distribution sedan hello följande:

1. Kör hello Azure AD Connect-installationsprogrammet (MSI).
2. När du ser hello **Välkommen tooAzure AD Connect** skärmen avsluta hello-installationsguiden genom att klicka på hello ”X” i hello övre högra hörnet av hello-fönstret.
3. Öppna en kommandotolk.
4. Hello för att installera platsen för Azure AD Connect (standard: C:\Program Files\Microsoft Azure Active Directory Connect) köra följande kommando hello: `AzureADConnect.exe /ForceExport`.
5. Klicka på hello **Exportera inställningar** knappen. När du installerar Azure AD Connect på en separat server migreras inställningarna från din aktuella DirSync tooyour nya Azure AD Connect-installationen.

![Analysen är klar](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

När inställningarna har exporterats kan avsluta du hello Azure AD Connect-guiden på hello DirSync-servern. Fortsätt med nästa steg i hello för[installera Azure AD Connect på en separat server](#installation-of-azure-ad-connect-on-separate-server).

### <a name="install-azure-ad-connect-on-separate-server"></a>Installera Azure AD Connect på en separat server
När du installerar Azure AD Connect på en ny server antas hello som du vill tooperform en ren installation av Azure AD Connect. Eftersom du vill toouse hello DirSync-konfigurationen finns några extra steg tootake:

1. Kör hello Azure AD Connect-installationsprogrammet (MSI).
2. När du ser hello **Välkommen tooAzure AD Connect** skärmen avsluta hello-installationsguiden genom att klicka på hello ”X” i hello övre högra hörnet av hello-fönstret.
3. Öppna en kommandotolk.
4. Hello för att installera platsen för Azure AD Connect (standard: C:\Program Files\Microsoft Azure Active Directory Connect) köra följande kommando hello: `AzureADConnect.exe /migrate`.
   Installationsguiden för hello Azure AD Connect startar och ger dig hello följande skärm:  
   ![Ange dina autentiseringsuppgifter för Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. Välj hello inställningsfilen som exporterades från DirSync-installation.
6. Konfigurera eventuella avancerade alternativ, inklusive:
   * En anpassad installationsplats för Azure AD Connect.
   * En befintlig instans av SQL Server (SQL Server 2012 Express installeras som standard av Azure AD Connect). Använd inte hello samma databasinstans som DirSync-servern.
   * Ett tjänstkonto som används tooconnect tooSQL Server (om SQL Server-databasen är en fjärrplats kontot måste vara ett Domäntjänstkonto).
     Följande alternativ visas på skärmen:  
     ![Ange dina autentiseringsuppgifter för Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. Klicka på **Nästa**.
8. På hello **klar tooconfigure** lämnar hello **startar hello synkroniseringsprocessen så snart hello konfigurationen är klar** markerad. hello-servern är nu i [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode) så att ändringar inte exporterade tooAzure AD.
9. Klicka på **Installera**.
10. När hello-installationen har slutförts, logga ut och logga in igen tooWindows innan du använder Synchronization Service Manager eller Synchronization Rule Editor eller försök toomake andra konfigurationsändringar.

> [!NOTE]
> Synkroniseringen mellan Windows Server Active Directory och Azure Active Directory börjar, men inga ändringar är exporterade tooAzure AD. Endast ett synkroniseringsverktyg i taget kan aktivt exportera ändringar. Det här tillståndet kallas för [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-that-azure-ad-connect-is-ready-toobegin-synchronization"></a>Kontrollera att Azure AD Connect är klar toobegin synkronisering
tooverify som Azure AD Connect är klar tootake över från DirSync måste tooopen **Synchronization Service Manager** i hello gruppen **Azure AD Connect** hello start-menyn.

Hello programmet gå toohello **Operations** fliken. På den här fliken kan du bekräfta att hello följande åtgärder har slutförts:

* Importera på hello AD-koppling
* Importera på hello Azure AD-koppling
* Fullständig synkronisering hello AD-koppling
* Fullständig synkronisering hello Azure AD-koppling

![Importen och synkroniseringen har slutförts](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Granska hello resultatet från dessa åtgärder och se till att det inte finns några fel.

Om du vill toosee och inspektera hello ändringar som är i färd toobe exporteras tooAzure AD läser hur tooverify hello konfigurationen under [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode). Gör nödvändiga konfigurationsändringar tills du inte ser något oväntat.

Är du redo tooswitch från DirSync tooAzure AD när du har utfört stegen och är nöjd med hello resultat.

### <a name="uninstall-dirsync-old-server"></a>Avinstallera DirSync (den gamla servern)
* Leta upp **Synkroniseringsverktyget för Microsoft Azure Active Directory**i **Program och funktioner**
* Avinstallera **Synkroniseringsverktyget för Microsoft Azure Active Directory**
* hello avinstallationen kan ta upp too15 minuter toocomplete.

Om du vill toouninstall DirSync senare kan du också tillfälligt stänga av servern hello eller inaktivera hello-tjänsten. Om något går fel kan den här metoden kan du aktivera toore den. Dock förväntas inte detta hello steg misslyckas, så detta inte bör behövs.

Med DirSync avinstalleras eller är inaktiverad, finns det ingen aktiv server som exporterar tooAzure AD. hello nästa steg tooenable Azure AD Connect måste slutföras innan ändringar i din lokala Active Directory fortsätter toobe synkroniseras tooAzure AD.

### <a name="enable-azure-ad-connect-new-server"></a>Aktivera Azure AD Connect (den nya servern)
Efter installationen, öppna Azure AD connect igen kan du toomake ytterligare konfigurationsändringar. Starta **Azure AD Connect** från hello start-menyn eller hello genvägen på skrivbordet hello. Kontrollera att du inte försöka toorun hello MSI-installationsfilen igen.

Du bör se hello följande:  
![Ytterligare uppgifter](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

* Välj **Konfigurera mellanlagringsläge**.
* Inaktivera mellanlagring genom att avmarkera hello **aktivera mellanlagringsläge** kryssrutan.

![Ange dina autentiseringsuppgifter för Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

* Klicka på hello **nästa** knappen
* Klicka på sidan Bekräfta hello hello **installera** knappen.

Azure AD Connect är nu din aktiva server och du måste inte växla tillbaka toousing din befintliga DirSync-server.

## <a name="next-steps"></a>Nästa steg
Nu när du har Azure AD Connect installeras kan du [verifiera hello installationen och tilldela licenser](active-directory-aadconnect-whats-next.md).

Mer information om dessa nya funktioner, som aktiverades med installationen hello: [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md), [förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), och [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Mer information om dessa vanliga avsnitt: [Schemaläggaren och hur tootrigger synkronisera](active-directory-aadconnectsync-feature-scheduler.md).

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
