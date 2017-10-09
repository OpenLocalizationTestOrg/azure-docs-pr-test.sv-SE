---
title: "Azure AD Connect-synkronisering: Schemaläggaren | Microsoft Docs"
description: "Det här avsnittet beskriver hello inbyggda scheduler-funktionen i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: c587039cc68d305862a07beff364894b6f74cd2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect-synkronisering: Schemaläggaren
Det här avsnittet beskrivs hello inbyggda Schemaläggaren i Azure AD Connect-synkronisering (kallas även Synkroniseringsmotorn).

Den här funktionen introducerades med build 1.1.105.0 (utgiven februari 2016).

## <a name="overview"></a>Översikt
Azure AD Connect-synkronisering synkronisera ändringar äger rum i din lokala katalog med hjälp av en Schemaläggaren. Det finns två scheduler processer, ett för synkronisering av lösenord och ett annat objekt/attribut synkronisering och underhåll uppgifter. Det här avsnittet beskriver hello senare.

I tidigare versioner kunde hello schemaläggare för objekt och attribut externa toohello Synkroniseringsmotorn. Den använda Schemaläggaren i Windows eller en separat Windows service tootrigger hello processen för synkronisering. hello Schemaläggaren är med hello 1.1 versioner inbyggda toohello Synkroniseringsmotorn och låta vissa anpassning. Synkroniseringsfrekvens hello nya standard är 30 minuter.

hello Schemaläggaren är ansvarig för två aktiviteter:

* **Synkroniseringscykel**. hello processen tooimport synkronisera och exportera ändringar.
* **Underhållsaktiviteter**. Förnya nycklar och certifikat för återställning av lösenord och registreringstjänsten för enheter (DRS). Rensa gamla posterna i loggen för hello-åtgärder.

hello scheduler själva körs alltid, men det kan vara konfigurerade tooonly som kör en eller inga av dessa uppgifter. Till exempel om du behöver toohave egna cykel synkroniseringsprocessen kan kan du inaktivera denna uppgift i Schemaläggaren hello men fortfarande kör hello underhållsåtgärden.

## <a name="scheduler-configuration"></a>Konfiguration av Schemaläggaren
toosee den aktuella konfigurationen gå tooPowerShell och kör `Get-ADSyncScheduler`. Den visar ungefär som den här bilden:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Om du ser **hello sync kommando eller en cmdlet är inte tillgänglig** när du kör denna cmdlet sedan hello PowerShell-modulen har inte lästs in. Det här problemet kan inträffa om du kör Azure AD Connect på en domänkontrollant eller på en server med PowerShell begränsning högre än hello standardinställningarna. Om du ser felet kör `Import-Module ADSync` toomake hello-cmdlets som är tillgängliga.

* **AllowedSyncCycleInterval**. hello kortaste tidsintervallet mellan synkronisering cykler tillåts av Azure AD. Du kan inte synkronisera oftare än den här inställningen och fortfarande stöds.
* **CurrentlyEffectiveSyncCycleInterval**. hello schema för närvarande i kraft. Den har samma värde som CustomizedSyncInterval hello (om Ange) om den inte oftare än AllowedSyncInterval. Om du använder en version innan 1.1.281 och du ändrar CustomizedSyncCycleInterval, träder den här ändringen i kraft efter nästa synkroniseringscykel. Från build 1.1.281 träder hello ändringen i kraft omedelbart.
* **CustomizedSyncCycleInterval**. Om du vill hello scheduler toorun på andra frekvens än hello standard 30 minuter kan konfigurera du den här inställningen. Hello bilden ovan har hello Schemaläggaren ställts in toorun varje timme i stället. Om du använder inställningen tooa värdet vara lägre än AllowedSyncInterval används hello senare.
* **NextSyncCyclePolicyType**. Delta eller inledande. Definierar om hello kör nästa gång bör endast deltaändringar process, eller om hello nästa körning bör göra en fullständig import och synkronisering. hello senare skulle också bearbeta några nya eller ändrade regler.
* **NextSyncCycleStartTimeInUTC**. Nästa gång du startar hello scheduler hello nästa synkronisering cykel.
* **PurgeRunHistoryInterval**. Hej loggar bör hållas time-åtgärden. Dessa loggar kan ses i hello synkronisering service manager. Hej standard är tookeep loggarna för 7 dagar.
* **SyncCycleEnabled**. Anger om hello Schemaläggaren körs hello import, synkronisering och exportera processer som en del av åtgärden.
* **MaintenanceEnabled**. Visar om hello underhållsprocessen är aktiverat. Uppdaterar den hello certifikat/nycklar och återställningspunkter hello operations loggen.
* **StagingModeEnabled**. Visar om [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode) är aktiverad. Om den här inställningen är aktiverad, sedan den undertrycks hello exporter från att köras men fortfarande köra import och synkronisering.
* **SchedulerSuspended**. Anges av Connect under en uppgradering tootemporarily block hello Schemaläggaren körs.

Du kan ändra vissa av inställningarna med `Set-ADSyncScheduler`. hello följande parametrar kan ändras:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

I tidigare versioner av Azure AD Connect **isStagingModeEnabled** exponerats i Set-ADSyncScheduler. Det är **stöds inte** tooset den här egenskapen. Hej egenskapen **SchedulerSuspended** bör endast ändras av Connect. Det är **stöds inte** tooset detta med PowerShell direkt.

konfiguration av Schemaläggaren hello lagras i Azure AD. Om du har en fristående server påverkar ändringar på hello primära servern också hello mellanlagring server (utom IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntax:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dagar, HH - timmar, mm - minuter och ss - sekunder

Exempel:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Ändringar hello scheduler toorun var 3: e timme.

Exempel:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Du ändrar hello scheduler toorun dagligen.

### <a name="disable-hello-scheduler"></a>Inaktivera hello Schemaläggaren  
Om du behöver toomake konfigurationsändringar vill toodisable hello Schemaläggaren. Till exempel när du [filtreringen](active-directory-aadconnectsync-configure-filtering.md) eller [ändrar toosynchronization regler](active-directory-aadconnectsync-change-the-configuration.md).

toodisable hello scheduler, kör `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Inaktivera hello Schemaläggaren](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

När du har gjort ändringarna Glöm inte tooenable hello scheduler igen med `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-hello-scheduler"></a>Starta hello Schemaläggaren
hello Schemaläggaren är som standard körs var 30: e minut. I vissa fall kanske du vill toorun en synkronisering växla mellan hello schemalagda cykler eller toorun en annan typ måste.

**Delta sync cykel**  
En cykel för synkronisering av delta innehåller hello följande steg:

* Deltaimport på alla kopplingar
* Deltasynkronisering för alla kopplingar
* Exportera alla kopplingar

Det kan vara att du har en brådskande ändring som måste synkroniseras omedelbart, vilket är anledningen till toomanually måste köra en cykel. Om du behöver toomanually kör sedan en cykel från PowerShell kör `Start-ADSyncSyncCycle -PolicyType Delta`.

**Fullständig synkronisering**  
Om du har gjort något av följande konfigurationsändringar hello måste toorun en fullständig synkronisering cykel (kallas även Inledande):

* Lägga till flera objekt eller attribut toobe som importerats från en katalog
* Ändrat toohello Synkroniseringsregler
* Ändra [filtrering](active-directory-aadconnectsync-configure-filtering.md) så att ett annat antal objekt som ska tas med

Om du har gjort något av dessa ändringar, måste en fullständig synkronisering växla så hello Synkroniseringsmotorn har hello möjlighet tooreconsolidate hello kopplingens utrymmen toorun. En fullständig synkronisering cykel innehåller hello följande steg:

* Fullständig Import på alla kopplingar
* Fullständig synkronisering på alla kopplingar
* Exportera alla kopplingar

tooinitiate en fullständig synkronisering cykel kör `Start-ADSyncSyncCycle -PolicyType Initial` från en PowerShell-kommandotolk. Detta kommando startar en fullständig synkronisering cykel.

## <a name="stop-hello-scheduler"></a>Stoppa hello Schemaläggaren
Om hello Schemaläggaren körs för närvarande en synkroniseringscykel, måste du kanske toostop den. Till exempel om du startar installationsguiden för hello och du får detta fel:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

När en cykel synkronisering körs, kan du inte göra ändringar i konfigurationen. Du kan vänta tills hello Schemaläggaren har slutförts hello process, men du kan också avbryta den så att du kan göra dina ändringar omedelbart. Stoppa hello aktuella cykel är inte skadliga och väntande ändringar bearbetas med nästa körning.

1. Starta genom att ange hello scheduler toostop nuvarande cykel med hello PowerShell-cmdlet `Stop-ADSyncSyncCycle`.
2. Om du använder en version innan 1.1.281 och sedan stoppa hello scheduler inte stoppa hello aktuella Connector från den aktuella aktiviteten. tooforce Hej Connector toostop, vidta följande åtgärder hello: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
   * Starta **synkroniseringstjänsten** hello start-menyn. Gå för**kopplingar**, markera hello Connector med hello tillstånd **kör**, och välj **stoppa** från hello åtgärder.

hello scheduler fortfarande är aktivt och påbörjas igen nästa tillfälle.

## <a name="custom-scheduler"></a>Anpassade scheduler
hello-cmdletarna som beskrivs i det här avsnittet är bara tillgängliga i build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) och senare.

Om hello inbyggda scheduler inte uppfyller dina krav kan du schemalägga hello kopplingar med hjälp av PowerShell.

### <a name="invoke-adsyncrunprofile"></a>Anropa ADSyncRunProfile
Du kan starta en profil för en koppling i det här sättet:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

hello namn toouse för [Kopplingsnamn](active-directory-aadconnectsync-service-manager-ui-connectors.md) och [kör profilnamn](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) hittar du i hello [Synchronization Service Manager UI](active-directory-aadconnectsync-service-manager-ui.md).

![Starta Körningsprofilen](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Hej `Invoke-ADSyncRunProfile` cmdlet sker synkront, det vill säga återgår den inte kontroll tills hello koppling har slutförts hello igen utan eller med ett fel.

När du schemalägger kopplingar hello rekommendation är tooschedule dem i hello följande ordning:

1. (Fullständig/Delta) Importera från lokala kataloger, till exempel Active Directory
2. (Fullständig/Delta) Import från Azure AD
3. (Fullständig/Delta) Synkronisering från lokala kataloger, till exempel Active Directory
4. (Fullständig/Delta) Synkronisering från Azure AD
5. Exportera tooAzure AD
6. Exportera tooon lokala kataloger, till exempel Active Directory

Den här ordningen är hur hello inbyggda Schemaläggaren körs hello kopplingar.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Du kan också övervaka hello sync motorn toosee om den är upptagen eller inaktiv. Denna cmdlet returnerar ett tomt resultat om hello Synkroniseringsmotorn är inaktiv och inte kör en koppling. Om en koppling körs returnerar hello namnet på hello Connector.

```
Get-ADSyncConnectorRunStatus
```

![Kopplingen Körstatus](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
I hello bilden ovan är hello första raden från ett tillstånd där hello Synkroniseringsmotorn är inaktiv. hello andra raden från när hello Azure AD Connector körs.

## <a name="scheduler-and-installation-wizard"></a>Guiden Schemaläggaren och installation
Om du startar installationsguiden för hello inaktiverats hello scheduler tillfälligt. Det här beteendet är eftersom det förutsätts att du gör konfigurationsändringar och de här inställningarna kan inte användas om hello Synkroniseringsmotorn körs aktivt. Därför lämna hello installationsguiden öppna eftersom stoppas hello Synkroniseringsmotorn från att utföra några åtgärder för synkronisering.

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
