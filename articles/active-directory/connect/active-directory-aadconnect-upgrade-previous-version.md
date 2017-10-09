---
title: "Azure AD Connect: Uppgradera från en tidigare version | Microsoft Docs"
description: "Förklarar hello olika metoder tooupgrade toohello senaste versionen av Azure Active Directory Connect, inklusive en uppgradering på plats och en öppning migrering."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 57bd5b094654e4983cafa303b6f3daecadafb01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-toohello-latest"></a>Azure AD Connect: Uppgradera från en tidigare version toohello senaste
Det här avsnittet beskriver hello olika metoder som du kan använda tooupgrade din Azure Active Directory (AD Azure) Anslut installation toohello senaste versionen. Vi rekommenderar att du hålla dig uppdaterad med hello versioner av Azure AD Connect. Du också använda hello steg i hello [svänga migrering](#swing-migration) avsnittet när du gör en betydande konfigurationsändring.

Om du vill tooupgrade från DirSync, se [uppgradera från Azure AD-synkroniseringsverktyget (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) i stället.

Det finns några olika strategier som du kan använda tooupgrade Azure AD Connect.

| Metod | Beskrivning |
| --- | --- |
| [Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) |Detta är hello enklaste sättet för kunder med en snabb installation. |
| [Uppgradering på plats](#in-place-upgrade) |Om du har en enda server, kan du uppgradera hello installation på plats på hello samma server. |
| [Öppning migrering](#swing-migration) |Med två servrar du förbereder en hello servrar med hello ny utgåva eller konfiguration och ändra hello aktiva servern när du är klar. |

Behörighetsinformation finns i hello [behörigheter som krävs för en uppgradering](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> När du har aktiverat din nya Azure AD Connect-servern toostart synkroniserar ändringar tooAzure AD, måste du inte återställa toousing DirSync eller Azure AD Sync. Nedgradering från Azure AD Connect toolegacy klienter, inklusive DirSync och Azure AD Sync stöds inte och kan leda tooissues, till exempel förlust av data i Azure AD.

## <a name="in-place-upgrade"></a>Uppgradering på plats
En uppgradering på plats fungerar för att flytta från Azure AD Sync eller Azure AD Connect. Det fungerar inte för att flytta från DirSync eller för en lösning med Forefront Identity Manager (FIM) + Azure AD-koppling.

Den här metoden är att föredra när du har en enda server och på mindre än 100 000 objekt. Om det finns ändringar toohello out-of-box sync regler, sker en fullständig import och en fullständig synkronisering efter hello uppgraderingen. Den här metoden garanterar att nya hello-konfigurationen är tillämpade tooall befintliga objekt i hello system. Det här körs kan ta några timmar, beroende på hello antalet objekt i omfånget för hello Synkroniseringsmotorn. hello normal delta schemaläggare för synkronisering (som synkroniserar var 30: e minut som standard) har pausats, men Lösenordssynkronisering fortsätter. Du kan även göra hello platsuppgradering när en helg. Om det finns ingen ändringar toohello out-of-box-konfiguration med hello nya Azure AD Connect-versionen, sedan en normal import/Deltasynkronisering startar i stället.  
![Uppgradering på plats](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Om du har gjort ändringar toohello out-of-box Synkroniseringsregler sedan dessa regler är inställda tillbaka toohello standardkonfigurationen vid uppgradering. toomake till att konfigurationen förblir mellan uppgraderingar, se till att du gör ändringar som de är beskrivs i [bästa praxis för att ändra standardkonfigurationen för hello](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Under uppgradering på plats, kanske ändringar som kräver specifika synkronisering aktiviteter (inklusive fullständig Import steg och fullständig synkronisering) toobe utförs efter uppgraderingen har slutförts. toodefer sådana aktiviteter finns toosection [hur toodefer fullständig synkronisering efter uppgraderingen](#how-to-defer-full-synchronization-after-upgrade).

## <a name="swing-migration"></a>Swingmigrering
Om du har en komplex distribution eller många objekt, kan det vara opraktiska toodo uppgradering på plats i hello live-system. Den här processen kan ta flera dagar--för vissa kunder och under denna tid kan ingen deltaändringar bearbetas. Du kan också använda den här metoden när du planerar toomake betydande ändringar tooyour konfiguration och du vill tootry dem ut innan de är vidare toohello moln.

hello rekommenderad metod för dessa scenarier är toouse en öppning migrering. Du måste (minst) två servrar – en active server och en fristående server. hello aktiva servern (visas med fasta blå rader i följande bild hello) ansvarar för hello aktiv produktion. hello mellanlagring server (visas med streckad lila) har förberetts med hello ny utgåva eller konfiguration. Den här servern har aktiverats när den är helt klar. hello tidigare active server, som nu har hello gamla version eller konfiguration installerad, görs till hello fristående server och har uppgraderats.

hello två servrar kan använda olika versioner. Till exempel hello active server som du planerar toodecommission kan använda Azure AD Sync och hello nya testserver kan använda Azure AD Connect. Om du använder öppning migrering toodevelop hello en ny konfiguration, det är en bra idé toohave hello samma versioner på två servrar.  
![Server för Förproduktion](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Vissa kunder föredrar toohave tre eller fyra servrar för det här scenariot. När hello mellanlagring servern har uppgraderats kan du inte har en reservserver för [katastrofåterställning](active-directory-aadconnectsync-operations.md#disaster-recovery). Med tre eller fyra servrar, kan du förbereda en uppsättning primära/vänteläge-servrar med nya hello-versionen, vilket garanterar att det alltid är en testserver som är redo tootake över.

De här stegen fungerar också toomove från Azure AD Sync eller en lösning med FIM + Azure AD-koppling. De här stegen fungerar inte för DirSync, men hello samma öppning migreringsmetod (kallas även parallell distribution) med steg för DirSync finns i [uppgradera Azure Active Directory sync (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-tooupgrade"></a>Använd en öppning migrering tooupgrade
1. Om du använder Azure AD Connect på både servrar och planera tooonly gör en konfigurationsändring, se till att din aktiva server och fristående server är både med hello samma version. Det gör det enklare toocompare skillnader senare. Om du uppgraderar från Azure AD Sync har olika versioner av dessa servrar. Om du uppgraderar från en äldre version av Azure AD Connect är det en bra idé toostart med hello två servrar som är med hjälp av hello samma version, men det krävs inte.
2. Du har gjort en anpassad konfiguration testservern inte har den och gör om hello under [flytta en anpassad konfiguration från hello active server toohello mellanlagring server](#move-custom-configuration-from-active-to-staging-server).
3. Uppgradera hello mellanlagring server toohello senaste versionen om du uppgraderar från en tidigare version av Azure AD Connect. Om du flyttar från Azure AD Sync, installera Azure AD Connect på testservern.
4. För att hello sync motorn som kör fullständig import och fullständig synkronisering på testservern.
5. Verifiera den nya konfigurationen hello inte orsakar ändringar oväntat med hello åtgärder under ”verifiera” i [Kontrollera hello konfigurationen för en server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Om något inte som förväntat, korrekt, kör hello import och synkronisering och verifiera hello data förrän den ser bra ut, genom att följa stegen i hello.
6. Växla hello mellanlagring server toobe hello aktiva servern. Detta är hello sista steget ”växel active server” i [Kontrollera hello konfigurationen för en server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Uppgradera hello-server som är nu i Förproduktion läge toohello senaste versionen om du uppgraderar Azure AD Connect. Följ samma steg som före tooget hello data och konfigurationsinformation uppgraderas hello. Om du har uppgraderat från Azure AD Sync kan nu stänga av och inaktivera den gamla servern.

### <a name="move-a-custom-configuration-from-hello-active-server-toohello-staging-server"></a>Flytta en anpassad konfiguration från hello active toohello fristående server
Om du har gjort ändringar toohello active konfigurationsservern måste toomake Kontrollera som hello samma ändras tillämpade toohello mellanlagring av servern. toohelp med den här flyttar du kan använda hello [Azure AD Connect configuration Dokumenteraren](https://github.com/Microsoft/AADConnectConfigDocumenter).

Du kan flytta hello anpassade sync regler som du har skapat med hjälp av PowerShell. Du måste tillämpa andra ändringar hello likadant på både system och du kan inte migrera hello ändringar. Hej [configuration Dokumenteraren](https://github.com/Microsoft/AADConnectConfigDocumenter) kan hjälpa dig att jämföra hello två system toomake att de är identiska. hello-verktyget kan också om du vill automatisera hello stegen i det här avsnittet.

Du behöver tooconfigure hello följande saker hello samma sätt på båda servrarna:

* Anslutningen toohello samma skogar
* Alla domän- och organisationsenhetsfiltrering
* Hej samma valfria funktioner, till exempel Lösenordssynkronisering och tillbakaskrivning av lösenord

**Flytta anpassade Synkroniseringsregler**  
toomove anpassade Synkroniseringsregler hello följande:

1. Öppna **Synchronization regler Editor** på din aktiva server.
2. Välj en anpassad regel. Klicka på **exportera**. Detta öppnar ett fönster för anteckningar. Spara hello temporär fil med tillägget PS1. Detta gör att det PowerShell-skript. Kopiera hello PS1 filen toohello mellanlagring av servern.  
   ![Synkronisera regeln export](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. hello Connector GUID skiljer sig åt på hello mellanlagring server och du måste ändra den. tooget hello GUID, starta **Synchronization regler Editor**, väljer du något av hello out-of-box regler som representerar hello samma anslutna system och klicka på **exportera**. Ersätt hello GUID i PS1-fil med hello GUID från hello mellanlagring av servern.
4. I PowerShell-Kommandotolken kör du hello PS1-fil. Detta skapar hello anpassade synkroniseringsregeln på hello mellanlagring av servern.
5. Upprepa detta för alla anpassade regler.

## <a name="how-toodefer-full-synchronization-after-upgrade"></a>Hur toodefer fullständig synkronisering efter uppgradering
Under uppgradering på plats, kanske ändringar som kräver specifika synkronisering aktiviteter (inklusive fullständig Import steg och fullständig synkronisering) toobe körs. Exempelvis kräver connector schemaändringar **fullständig import** steg och out-of-box synkronisering regeln kräver **fullständig synkronisering** steg toobe utfördes berörda kopplingar. Under uppgraderingen, Azure AD Connect avgör vilka synkronisering aktiviteter som krävs och registrerar dem som *åsidosätter*. I följande synkroniseringscykel hello, hello synkronisering scheduler hämtar dessa åsidosättningar och kör dem.. När en åsidosättning har körts tas bort.

Det kan finnas situationer där du inte vill att dessa åsidosättningar tootake plats omedelbart efter uppgraderingen. Till exempel du har ett stort antal synkroniserade objekt och du vill att dessa synkronisering steg toooccur efter kontorstid. tooremove dessa åsidosättningar:

1. Under uppgraderingen **avmarkera** hello alternativet **starta synkroniseringsprocessen hello när konfigurationen är klar**. Detta inaktiverar hello synkronisering Schemaläggaren och förhindrar att synkroniseringscykel äger rum automatiskt innan hello åsidosättningar tas bort.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. När uppgraderingen är klar kör du hello följande cmdlet toofind reda på vilka åsidosättningar har lagts till:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > hello åsidosättningar är specifika för anslutningen. I hello följande exempel, fullständig Import steg och fullständig synkronisering lagts tooboth hello lokala AD-koppling och Azure AD-koppling.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Notera hello befintliga åsidosättningar som lagts till.
   
4. tooremove hello åsidosättningar för både fullständig import och fullständig synkronisering på en godtycklig koppling kör hello följande cmdlet:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   tooremove hello åsidosättningar för alla kopplingar kör hello följande PowerShell-skript:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. tooresume hello scheduler, kör följande cmdlet hello:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Kom ihåg synkroniseringssteg i tooexecute hello krävs när det tidigaste dig. Du kan manuellt utföra dessa steg med hello Synchronization Service Manager eller Lägg till hello åsidosättningar tillbaka cmdleten hello Set-ADSyncSchedulerConnectorOverride.

tooadd hello åsidosättningar för både fullständig import och fullständig synkronisering på en godtycklig koppling kör hello följande cmdlet:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
