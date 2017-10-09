---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - översikt | Microsoft Docs"
description: "Översikt och innehåll karta över designguide Hybrid Identity"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 10aacb04c90abd100eb56d7c44d590946b052f18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory Hybrid Identity designöverväganden
Konsumenten-baserade enheter Prolifererande företagets hälsningsmeddelande och molnbaserade program för programvara som en tjänst (SaaS) är enkelt tooadopt. Därför kan behålla kontrollen över användarnas programåtkomst plattformar interna datacenter och moln en utmaning.  

Microsofts identitetslösningar omfattar lokala och molnbaserade funktioner, och skapa en enda användaridentitet för autentisering och auktorisering tooall resurser, oavsett plats. Vi kallar detta Hybrididentitet. Det finns olika design- och konfigurationsalternativ för hybrididentitet med hjälp av Microsoft solutions och i vissa fall kan det vara svårt toodetermine vilken kombination som passar organisationens behov hello. 

Den här Designguide för Hybrid Identity hjälper toounderstand hur toodesign en hybrididentitetslösning som bäst passar hello affärs- och teknikbehov behöver för din organisation.  Den här guiden innehåller en rad steg och aktiviteter som att du kan följa toohelp du utforma en hybrididentitetslösning som uppfyller organisationens unika krav. I hello steg och aktiviteter presenterar handboken hello hello relevanta tekniker och funktionen alternativ tillgängliga tooorganizations toomeet funktionella och kvalitetskrav (till exempel tillgänglighet, skalbarhet, prestanda, hantering och säkerhet) krav. 

Specifikt, finns hello hybrid identity överväganden guiden designmål tooanswer hello följande frågor: 

* Frågor gör jag behöver tooask och besvara toodrive en identity-specifika hybridutformning för en domän med teknik- eller problemdomän som bäst uppfyller Mina krav?
* Vilka sekvensen av aktiviteter bör jag slutföra toodesign en hybrididentitetslösning för hello teknik eller problem? 
* Vilka hybrid identity-teknik och vilka konfigurationsalternativ finns tillgängliga toohelp mig uppfylla kraven? Vad är hello avvägningarna mellan dessa alternativ så att jag kan välja hello bästa alternativet för mitt företag?

## <a name="who-is-this-guide-intended-for"></a>Vem riktar den här handboken till?
 CHEF, CITO, chefen identitet arkitekter, Enterprise-arkitekter och IT-arkitekter som ansvarar för att utforma en hybrididentitetslösning för medelstora eller stora organisationer.

## <a name="how-can-this-guide-help-you"></a>Hur kan den här handboken hjälpa dig?
Du kan använda den här guiden toounderstand hur toodesign en hybrididentitetslösning som kan toointegrate ett moln baserade identity management-systemet med din aktuella lokalt identitetslösning. 

hello följande bild visar ett exempel en hybrididentitetslösning som gör att IT-administratörer toomanage toointegrate deras aktuella Windows Server Active Directory lösningen som finns lokalt med Microsoft Azure Active Directory tooenable användare toouse enkel Inloggning (SSO) över program som finns i hello molnet och lokalt.

![](./media/hybrid-id-design-considerations/hybridID-example.png)

Hej ovanför bild är ett exempel på en hybrididentitetslösning som använder cloud services toointegrate med lokala funktioner i ordning tooprovide ett enda upplevelse toohello slutanvändaren autentiseringsprocessen och toofacilitate IT hanterar dessa resurser. Även om detta kan vara ett mycket vanligt scenario är varje organisations identitet hybridutformning sannolikt toobe skiljer sig hello visas i exemplet i bild 1 på grund av toodifferent krav. 

Den här guiden innehåller en serie steg och uppgifter som att du kan följa toodesign en hybrididentitetslösning som uppfyller organisationens unika krav. I hela hello alternativ följande steg och aktiviteter, hello guiden visar hello relevanta tekniker och funktionen tillgängliga tooyou toomeet funktions- och kvalitetsnivån som krävs för din organisation.

**Antaganden**: du har viss erfarenhet av Windows Server, Active Directory Domain Services och Azure Active Directory. I det här dokumentet förutsätter vi att du söker efter hur lösningarna kan uppfylla företagets behov på egen hand eller i en integrerad lösning.

## <a name="design-considerations-overview"></a>Översikt över design-överväganden
Det här dokumentet innehåller en uppsättning steg och aktiviteter som att du kan följa toodesign en hybrididentitetslösning som bäst uppfyller dina krav. hello stegen visas i tur och ordning. Överväganden vid utformning av du lär dig i senare steg kan kräva toochange beslut som du tog tidigare, men på grund av tooconflicting designalternativ. Varje försök görs tooalert du toopotential designkonflikter i hello dokumentet. 

Du anländer till hello design som bäst motsvarar dina krav förrän du har gått igenom hello steg så många gånger som behövs tooincorporate alla hello överväganden i hello dokumentet. 

| Hybrid Identity fas | Avsnittet innehåller en listan |
| --- | --- |
| Ange identitetskrav för |[Fastställa affärsbehov](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Ange krav för directory-synkronisering](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Ange krav för multifaktorautentisering](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definiera en strategi för införandet en hybrid identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Planera för att förbättra datasäkerhet via starkt identitetslösning |[Fastställa kraven på dataskydd](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Ange krav för innehållshantering](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Fastställa krav på åtkomstkontroll](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definiera en strategi för skydd av data](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Planera för hybrid identity livscykel |[Fastställa hanteringsuppgifter för hybrid identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Hantering av datasynkronisering](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Fastställa införandestrategin hybrid identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Hämta denna guide
Du kan hämta en pdf-version av hello överväganden vid utformning av Hybrid guide från hello [Technet-galleriet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

