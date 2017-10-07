---
title: 'Azure AD Connect-synkronisering: tekniska begrepp | Microsoft Docs'
description: "Förklarar hello tekniska begrepp i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: c6309bb9be462fb3d49c5b6ab302d4327ce4b7be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect-synkronisering: Tekniska begrepp
Den här artikeln är en sammanfattning av hello avsnittet [förstå arkitektur](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect-synkronisering bygger på en fast metadirectory synkronisering plattform.
följande avsnitt hello införa hello begrepp för metadirectory synkronisering.
Bygger på MIIS, ILM och FIM, ger hello Azure Active Directory Sync Services hello nästa plattform för anslutande toodata källor synkronisera data mellan datakällor samt hello etablering och borttagning av identiteter.

![Tekniska begrepp](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

hello följande avsnitt innehåller mer information om följande aspekter av hello FIM-synkroniseringstjänsten hello:

* koppling
* Attributflöde
* Anslutarplats
* Metaversum
* Etablering

## <a name="connector"></a>koppling
hello kodmoduler som används toocommunicate med en ansluten katalog kallas kopplingar (kallades tidigare hanteringsagenter (MAs)).

Dessa är installerade på hello-dator som kör Azure AD Connect-synkronisering. hello ger hello utan Agent möjlighet tooconverse med hjälp av fjärrsystemet protokoll i stället för att förlita dig på hello distribution av särskilda agenter. Detta innebär att minskar risken och distributionstiden, särskilt när du hanterar viktiga program och system.

I hello bilden ovan hello connector är synonym med hello anslutningsplatsen men omfattar all kommunikation med hello externt system.

hello anslutningen ansvarar för alla importera och exportera funktioner toohello system och frigör utvecklare inte behöver toounderstand hur tooconnect tooeach system internt när du använder deklarativ etablering toocustomize Datatransformationer.

Import och export inträffar bara när schemat, vilket ger ytterligare isolering från ändringar som inträffar inom hello systemet, eftersom ändringar inte automatiskt sprida toohello anslutna datakällan. Utvecklare kan dessutom också skapa sina egna anslutningsprogram för att ansluta toovirtually någon datakälla.

## <a name="attribute-flow"></a>Attributflöde
hello metaversum är hello konsoliderad vy över alla kopplade identiteter från intilliggande kopplingens utrymmen. I ovanstående hello bild visas attributflöde med linjer med pilar för både inkommande och utgående flöde. Attributflöde är hello processen att kopiera eller överföra data från ett system tooanother och alla attribut flöden (inkommande eller utgående).

Attributflöde sker mellan hello anslutningsutrymme och hello metaversum båda riktningarna när synkroniseringsåtgärder (fullständig eller delta) är schemalagda toorun.

Attributflöde inträffar bara när dessa synkroniseringar körs. Attributflöden definieras i Synkroniseringsregler. Dessa kan vara inkommande (ISR hello bilden ovan) eller utgående (OSR hello bilden ovan).

## <a name="connected-system"></a>Det anslutna systemet
Det anslutna systemet (aka ansluten katalog) hänvisar toohello fjärrsystemet Azure AD Connect-synkronisering har anslutit tooand läsning och skrivning identitet data tooand från.

## <a name="connector-space"></a>Anslutarplats
Varje anslutna datakällan representeras som en filtrerad delmängd av hello objekt och attribut i hello anslutningsplatsen.
Detta gör hello sync service toooperate lokalt utan hello måste toocontact hello fjärrsystemet när du synkroniserar hello objekt och begränsar interaktion tooimports och exporterar endast.

När hello datakälla och hello connector har hello möjlighet tooprovide en lista över ändringar (Deltaimport), sedan utbyts hello effektiviteten ökar kraftigt eftersom endast ändringar sedan den senaste avsökningen för hello växla. hello anslutningsplatsen gör hello anslutna datakällan från ändringar som sprider automatiskt genom att kräva hello connector schemat import och export. Den här extra insurance ger lugn vid testning, förhandsgranska eller bekräfta hello nästa uppdatering.

## <a name="metaverse"></a>Metaversum
hello metaversum är hello konsoliderad vy över alla kopplade identiteter från intilliggande kopplingens utrymmen.

Identiteter är länkade till varandra, och tilldelas behörighet för olika attribut via importera mappningar av attributflöde börjar hello centrala metaversumobjekt tooaggregate information från flera system. Från det här objektet attributflöde utföra mappningar toooutbound informationssystem.

Objekt som skapas när en auktoritativ system projekt dem till hello metaversum. När alla anslutningar tas bort raderas hello metaversumobjekt.

Objekt i hello metaversum kan inte ändras direkt. Alla data i hello-objektet måste vara bidragit via attributflöde. hello metaversum underhåller beständiga kopplingar med varje anslutningsplatsen. Dessa anslutningar kräver inte utvärdering för varje synkronisering körs. Det innebär att Azure AD Connect-synkronisering inte har toolocate hello matchande fjärrobjektet varje gång. Detta förhindrar hello behovet av kostsamma agenter tooprevent ändringar tooattributes som vanligtvis är ansvarig för korrelerar hello-objekt.

När identifierar nya datakällor som kan ha redan befintliga objekt som måste hanteras, toobe Azure AD Connect sync använder en process som kallas en koppling regeln tooevaluate potentiella kandidater med vilka tooestablish en länk.
När hello länken är etablerad kan utvärderingen försvinner och normal attributflöde kan inträffa mellan hello anslutna fjärrdatakällan och hello metaversum.

## <a name="provisioning"></a>Etablering
När ett projekt som auktoritativ datakälla skapas ett nytt objekt i hello metaversum ett nytt objekt i kopplingen kan i en annan koppling som representerar en underordnad ansluten datakälla.

Detta upprättar en länk natur och attributflöde kan fortsätta båda riktningarna.

När en regel avgör att ett nytt connector utrymme objekt måste toobe skapas, kallas etablering. Men eftersom åtgärden endast sker inom hello anslutningsplatsen, innehåller det inte till hello anslutna datakällan förrän en export utförs.

## <a name="additional-resources"></a>Ytterligare resurser
* [Azure AD Connect Sync: Anpassa synkroniseringsalternativ](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
