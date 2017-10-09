---
title: aaaUsing Azure AD Connect Health med synkronisering | Microsoft Docs
description: "Detta är hello Azure AD Connect Health sida som innehåller information om hur toomonitor Azure AD Connect synkroniserar."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56f0582be30e664026cedf15350bc23501998bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a>Övervaka Azure AD Connect-synkronisering med Azure AD Connect Health
hello följande dokumentation är specifik toomonitoring Azure AD Connect (synkronisering) med Azure AD Connect Health.  Information om övervakning av AD FS med Azure AD Connect Health finns i [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md). Mer information om övervakning av Active Directory Domain Services med Azure AD Connect Health finns i [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Aviseringar för Azure AD Connect Health för synkronisering
hello Azure AD Connect Health-aviseringar för synkronisering tillhandahåller du hello lista över aktiva aviseringar. Varje avisering innehåller relevant information, Lösningssteg och länkar toorelated dokumentation. Genom att välja en aktiv eller åtgärdad avisering visas ett nytt blad med ytterligare information, samt vad du kan göra tooresolve hello aviseringen och länkar tooadditional dokumentation. Du kan också visa historiska data om aviseringar som har lösts i hello tidigare.

Genom att markera en avisering som levereras med ytterligare information, samt steg kan du ta tooresolve hello aviseringen liksom länkar tooadditional dokumentation.

![Azure AD Connect-synkroniseringsfel](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Begränsad utvärdering av aviseringar
Om Azure AD Connect inte använder standardkonfigurationen för hello (till exempel om Attributfiltrering ändras från hello standard tooa anpassad konfiguration), sedan hello Azure AD Connect Health-agenten kommer inte att överföra hello fel händelser relaterade tooAzure AD Connect.

Detta begränsar hello utvärdering av aviseringar av hello-tjänsten. Visas en banderoll som anger det här villkoret i hello Azure Portal under din tjänst.

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/banner.png)

Du kan ändra detta genom att klicka på ”inställningar” och låta Azure AD Connect Health agent tooupload alla felloggarna.

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Synkronisera Insight
Administratörer ofta vill tooknow om hello tid det tar toosync ändringar tooAzure AD och hello mängd ändringar äger rum. Den här funktionen ger ett enkelt sätt toovisualize detta med hjälp av hello nedan diagram:   

* Svarstiden för synkroniseringsåtgärder
* Objektändringstrend

### <a name="sync-latency"></a>Synkronisera svarstider
Denna funktion tillhandahåller en grafisk trend över svarstiderna för hello synkroniseringsåtgärder (import, export osv.) för kopplingar.  Detta ger en snabb och enkelt sätt toounderstand hello inte bara svarstiderna för dina åtgärder (större om du har en stor mängd ändringar äger rum), utan också ett sätt toodetect avvikelser i hello latens som kan kräva ytterligare utredning.

![Synkronisera svarstider](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Som standard visas endast hello fördröjning av hello ”Export” för hello Azure AD-koppling.  toosee flera åtgärder på hello koppling eller tooview åtgärder från andra anslutningsappar högerklickar du på hello diagram, väljer Redigera diagram eller klicka på ”Redigera latens diagram” Hej och välj hello specifika åtgärden och kopplingar.

### <a name="sync-object-changes"></a>Synkronisera objektändringar
Denna funktion tillhandahåller en grafisk trend över hello antalet ändringar som utvärderas och exporteras tooAzure AD.  Idag, när toogather är den här informationen från synkroniseringsloggarna hello svårt.  hello diagrammet innehåller inte bara ett enklare sätt att övervaka hello antalet ändringar som äger rum i din miljö, utan också en visuell översikt över hello-fel som uppstår.

![Synkronisera svarstider](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Felrapport för synkronisering på objektnivå (förhandsgranskning)
Denna funktion tillhandahåller en rapport om synkroniseringsfel som kan uppstå när identitetsdata synkroniseras mellan Windows Server AD och Azure AD med Azure AD Connect.

* hello rapporten täcker fel som har registrerats av hello synkroniseringsklient (Azure AD Connect version 1.1.281.0 eller senare)
* Hello senaste synkroniseringen på hello Synkroniseringsmotorn innehåller hello-fel som uppstått. (”Export” på hello Azure AD-koppling.)
* Azure AD Connect Health agent för synkronisering måste ha utgående anslutning toohello krävs slutpunkter för hello tooinclude hello senaste rapportdata.
* hello rapporten är **uppdaterade efter var 30: e minut** med hello data har laddats upp av Azure AD Connect Health agent för synkronisering. Det ger hello följande viktiga funktioner

  * Kategorisering av fel
  * Lista över objekt med fel per kategori
  * Alla hello data om hello fel på samma ställe
  * Sida vid sida-jämförelse av objekt med fel på grund av tooa konflikt
  * Hämta hello felrapport som en CVS (kommer snart)

### <a name="categorization-of-errors"></a>Kategorisering av fel
hello rapporten kategoriserar hello befintliga synkroniseringsfel i hello följande kategorier:

| Kategori | Beskrivning |
| --- | --- |
| Duplicerat attribut |Fel när Azure AD Connect försöker skapa eller uppdatera objekt med dubblerade värden av ett eller flera attribut i Azure AD som måste vara unika i en klient, till exempel proxyAddresses, UserPrincipalName. |
| Felmatchning av data |Fel uppstod när hello soft-matchar inte toomatch objekt som resulterar i synkroniseringsfel. |
| Verifieringsfel för data |Fel på grund av tooinvalid data, till exempel tecken som inte stöds i viktiga attribut, till exempel UserPrincipalName, formatera fel som inte kan valideras innan de skrivs i Azure AD. |
| Stora attribut |Fel när en eller flera attribut är större än hello tillåten storlek, längd eller antal. |
| Annat |Alla andra fel som inte får plats i hello ovanför kategorier. Baserat på feedback, kommer den här kategorin att delas upp i underkategorier. |

![ Rapportsammanfattning för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Rapportkategorier för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Lista över objekt med fel per kategori
Vidaresökning i varje kategori får hello listan med objekt med hello fel i den kategorin.
![Rapportlista för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Information om fel
Följande data är tillgängliga i hello detaljerad vy för varje fel

* Identifierare för hello *AD-objekt* ingår
* Identifierare för hello *Azure AD-objekt* inblandade (som är tillämpligt)
* Felbeskrivning och hur toofix
* Relaterade artiklar

![Rapportdetaljer för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-hello-error-report-as-csv"></a>Hämta hello felrapport som CSV-fil
Genom att välja hello ”Export” knapp som du kan ladda ned en CSV-fil med alla hello detaljer om alla hello-fel.

## <a name="related-links"></a>Relaterade länkar
* [Felsöka fel under synkronisering](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Återhämtning av duplicerat attribut](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)