---
title: "Azure Active Directory B2C: Kommentarer för utvecklare om hur du använder anpassade principer | Microsoft Docs"
description: "Information för utvecklare på Konfigurera och underhålla Azure AD B2C med anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Viktig information för en förhandsversion av Azure Active Directory B2C anpassad princip
hello anpassad princip funktioner är nu tillgänglig för utvärdering under förhandsversion för alla Azure Active Directory B2C (Azure AD B2C) kunder. Den här funktionsuppsättningen är inriktad på avancerade identitet utvecklare skapar hello mest komplexa identitetslösningar.  

Idag kräver den här funktionsuppsättningen utvecklare tooconfigure hello identitet upplevelse Framework direkt via redigering av XML-filen. Den här metoden i konfigurationen är kraftfulla och komplexa. Avancerade identitet utvecklare som använder hello bör identitet upplevelse Framework planera tooinvest lite tid att slutföra genomgångar och läsa referensdokument. 

## <a name="features-included-in-this-public-preview"></a>Funktioner som ingår i denna förhandsversion
Med hello nya funktioner som introduceras i hello förhandsversion, kan utvecklare utföra hello följande uppgifter:<br>

* Författare och överföra anpassad autentisering användaren transporter med hjälp av anpassade principer. 
   * Beskriv användaren resor stegvisa som utbyten mellan Anspråksproviders. 
   * Definiera villkorliga grenar i resor för användaren. 
* Integrera REST API-aktiverade tjänster i din användare resor för anpassad autentisering.  
* Lägg till federation med identitetsleverantörer som är kompatibla med hello OpenIDConnect som standard. <br>
* Lägg till federation med identitetsleverantörer som följer toohello SAML 2.0-protokollet. 

## <a name="terms-of-hello-public-preview"></a>Villkor för hello förhandsversion

* Vi rekommenderar att du toouse hello nya funktioner i utvärderingssyfte.<br>
* Nya funktioner är inte avsedd att användas i en produktionsmiljö.<br>
* Servicenivåavtal (SLA) gäller inte toohello nya funktioner. <br>
* Supportärenden kan registreras via vanlig supportkanaler. <br>
* Det finns inga framtida datum för allmän tillgänglighet.<br>
* Vår gottfinnande och av någon anledning, kan Microsoft flagga och avvisa eller begränsa scenarier och användaren resor som överskrider hello omfattning hello Azure AD B2C produkten auktoriserad tooserve som plattform för hantering (CIAM) av identitets- och en kund.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Ansvaret för anpassad princip funktionsuppsättningen utvecklare
Manuell konfiguration ger lägre behörighet toohello underliggande plattform på Azure AD B2C och resulterar i en unik helt anpassningsbar förtroende ram hello skapas. Möjliga varianter av anpassade identitetsleverantörer, Betrodda relationer integration med externa tjänster och stegvisa arbetsflöden Placera större krav på hello avancerade utvecklare som använder dem.

toofully förmånen från hello förhandsversion, vi rekommenderar att utvecklare förbrukar hello anpassad princip funktionsuppsättningen följer toohello följande riktlinjer:
* Bekanta dig med nyckel/hemligheter hantering och hello configuration språk hello identitet upplevelse motorn.
* Bli ägare till scenarier och anpassad integrering.
* Utföra metodisk scenariotestning.
* Följ programvaruutveckling och mellanlagring metodtips med minst en utvecklings- och testmiljö och en produktionsmiljö.
* Håll dig informerad om förändringar från hello identitetsleverantörer och tjänster som du integrerar med. Till exempel hålla koll på ändringar i hemligheter och planerade och oplanerade ändringar toohello service.
* Konfigurera active övervakning och övervaka hello svarstider för produktionsmiljöer.
* Behåll det aktuella kontakta e-postadresser och håll responsiv toohello Microsoft live-plats-teamet e-postmeddelanden.
* Vidta lämpliga åtgärder när uppmanade toodo hello genom Microsoft live-plats-teamet. 


>[!NOTE]
>Dessa funktioner kan slutligen ingå i Azure AD inbyggda principer, vilket gör dem lättare att använda tooall-utvecklare.

## <a name="next-steps"></a>Nästa steg
[Kom igång med anpassade principer](active-directory-b2c-get-started-custom.md).
