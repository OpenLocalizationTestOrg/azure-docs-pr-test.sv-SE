---
title: 'Azure AD Connect: Automatisk uppgradering | Microsoft Docs'
description: "Det här avsnittet beskriver hello inbyggd automatisk uppgradering funktion i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 70d15eb3adf7758d8a43d278157daa504e059a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automatisk uppgradering
Den här funktionen introducerades med build 1.1.105.0 (utgiven februari 2016).

## <a name="overview"></a>Översikt
Kontrollera att installationen av Azure AD Connect är alltid in toodate har aldrig varit enklare med hello **automatisk uppgradering** funktion. Den här funktionen är aktiverad som standard för snabb installationer och uppgraderingar av DirSync. När en ny version släpps, uppgraderas automatiskt din installation.

Automatisk uppgradering är aktiverad som standard för hello följande:

* Express installation inställningar och DirSync uppgraderingar.
* Använda SQL Express LocalDB är vad standardinställningar alltid använda. Också använda DirSync med SQL Express LocalDB.
* hello AD-kontot är hello standard MSOL_ konto med standardinställningar och DirSync.
* Har mindre än 100 000 objekt i hello metaversum.

hello aktuell status för automatisk uppgradering kan visas med hello PowerShell-cmdlet `Get-ADSyncAutoUpgrade`. Det har hello följande tillstånd:

| Status | Kommentar |
| --- | --- |
| Enabled |Automatisk uppgradering är aktiverat. |
| avbruten |Ange av hello-systemet. hello systemet inte längre är berättigad tooreceive automatiska uppgraderingar. |
| Disabled |Automatisk uppgradering är inaktiverad. |

Du kan ändra mellan **aktiverad** och **inaktiverad** med `Set-ADSyncAutoUpgrade`. Endast hello system kan ange hello tillstånd **pausad**.

Automatisk uppgradering använder Azure AD Connect Health för hello uppgradera infrastrukturen. Kontrollera att du har öppnat hello URL: er i din proxyserver för för automatisk uppgradering toowork **Azure AD Connect Health** enligt beskrivningen i [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Om hello **Synchronization Service Manager** UI körs på hello-servern och sedan hello uppgraderingen pausas tills hello Användargränssnittet stängs.

## <a name="troubleshooting"></a>Felsökning
Om Connect-installationen inte uppgraderar sig själv som förväntat, följer du dessa steg toofind ut vad kan vara fel.

Först förvänta dig inte hello automatisk uppgradering toobe försökte hello första dagen som en ny version släpps. Det finns en avsiktlig slumpmässighet innan en uppgradering görs så inte vara tom om installationen inte uppgraderat omedelbart.

Om du tycker att något inte är rätt först kör `Get-ADSyncAutoUpgrade` tooensure automatisk uppgradering är aktiverad.

Kontrollera sedan att du har öppnat hello krävs URL: er i proxyservern eller brandväggen. Automatisk uppdatering använder Azure AD Connect Health enligt beskrivningen i hello [översikt](#overview). Om du använder en proxyserver, kontrollera att hälsotillståndet har konfigurerade toouse en [proxyserver](../connect-health/active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Också testa hello [hälsa anslutningen](../connect-health/active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) tooAzure AD.

Med hello anslutningen tooAzure AD verifieras, är det tid toolook till hello händelseloggar. Starta hello Loggboken och titta i hello **programmet** eventlog. Lägga till ett eventlog filter för hello källan **Azure AD Connect uppgradera** och hello händelse-id-intervallet **300 399**.  
![Eventlog filter för automatisk uppgradering](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Du kan nu se hello eventlogs som är associerade med hello status för automatisk uppgradering.  
![Eventlog filter för automatisk uppgradering](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

hello Resultatkod har ett prefix med en översikt över hello tillstånd.

| Prefixet resultat | Beskrivning |
| --- | --- |
| Lyckades |hello-installationen har uppgraderats. |
| UpgradeAborted |Ett tillfälligt tillstånd har stoppats hello uppgraderingen. Den provas igen och hello förväntningen är det lyckas senare. |
| UpgradeNotSupported |hello systemet har en konfiguration som blockerar hello system från uppgraderas automatiskt. Den kommer att göras toosee om hello tillstånd ändras, men hello förväntan är hello system måste uppgraderas manuellt. |

Här är en lista över de vanligaste hälsningsmeddelande du hittar. Visas inte alla men hälsningsmeddelande resultatet bör vara Rensa med vilket hello-problem.

| Resultatmeddelande | Beskrivning |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Det gick inte att skriva toohello registret. |
| UpgradeAbortedInsufficientDatabasePermissions |hello inbyggda administratörsgruppen har inte behörighet toohello databas. Uppgradera manuellt toohello senaste versionen av Azure AD Connect tooaddress problemet. |
| UpgradeAbortedInsufficientDiskSpace |Det finns inte tillräckligt med utrymme toosupport för skivan en uppgradering. |
| UpgradeAbortedSecurityGroupsNotPresent |Det gick inte att hitta och lösa alla säkerhetsgrupper som används av hello Synkroniseringsmotorn. |
| UpgradeAbortedServiceCanNotBeStarted |hello NT-tjänst **Microsoft Azure AD Sync** toostart misslyckades. |
| UpgradeAbortedServiceCanNotBeStopped |hello NT-tjänst **Microsoft Azure AD Sync** toostop misslyckades. |
| UpgradeAbortedServiceIsNotRunning |hello NT-tjänst **Microsoft Azure AD Sync** körs inte. |
| UpgradeAbortedSyncCycleDisabled |Hej SyncCycle alternativ i hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md) har inaktiverats. |
| UpgradeAbortedSyncExeInUse |Hej [Synkroniseringshanteraren service UI](active-directory-aadconnectsync-service-manager-ui.md) är öppen på hello. |
| UpgradeAbortedSyncOrConfigurationInProgress |hello installationsguiden körs eller en synkronisering har schemalagts utanför hello Schemaläggaren. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedCustomizedSyncRules |Du har lagt till egna anpassade regler toohello konfiguration. |
| UpgradeNotSupportedDeviceWritebackEnabled |Du har aktiverat hello [tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md) funktion. |
| UpgradeNotSupportedGroupWritebackEnabled |Du har aktiverat hello [tillbakaskrivning av grupp](active-directory-aadconnect-feature-preview.md#group-writeback) funktion. |
| UpgradeNotSupportedInvalidPersistedState |hello-installationen är inte en standardinställningar eller en uppgradering av DirSync. |
| UpgradeNotSupportedMetaverseSizeExceeeded |Du har mer än 100 000 objekt i hello metaversum. |
| UpgradeNotSupportedMultiForestSetup |Du ansluter toomore än en skog. Expressinstallationen ansluter bara tooone skog. |
| UpgradeNotSupportedNonLocalDbInstall |Du använder inte en SQL Server Express LocalDB-databas. |
| UpgradeNotSupportedNonMsolAccount |Hej [AD-koppling konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) inte är hello standardkontot MSOL_ längre. |
| UpgradeNotSupportedStagingModeEnabled |hello server anges toobe i [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |Du har aktiverat hello [tillbakaskrivning av användare](active-directory-aadconnect-feature-preview.md#user-writeback) funktion. |

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
