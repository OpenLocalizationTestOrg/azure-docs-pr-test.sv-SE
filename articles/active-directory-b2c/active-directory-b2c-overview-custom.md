---
title: 'Azure Active Directory B2C: Anpassade principer | Microsoft Docs'
description: "Ett ämne på Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1ff398a4-2079-4615-94f1-57de22c0aad6
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: bada9cf5f1a86f3dd047536f6fd9359c0231a384
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Anpassade policys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Vad är anpassade principer?

Anpassade principer är konfigurationsfiler som definierar hello beteende för din Azure AD B2C-klient. Medan **inbyggda principer** är fördefinierade i hello Azure AD B2C portal hello vanligaste identitet uppgifter, anpassade principer kan vara helt redigeras av en identitet developer toocomplete en nära obegränsat antal uppgifter. Läsa på toodetermine om anpassade principer är rätt för dig och din identitet scenario.

**Redigera anpassad princip är inte för alla.** hello inlärningskurvan krävande hello starttiden är längre och framtida ändringar toocustom principer kräver liknande expertis toomaintain. Inbyggda principer bör planeras noggrant först för ditt scenario innan du använder anpassade principer.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Jämföra inbyggda och anpassade principer

| | Inbyggda principer | anpassade principer |
|-|-------------------|-----------------|
|Målgruppsanvändare | Alla utvecklare av program med eller utan kunskaper identitet | Identity-tekniker: systemintegrerare, konsulter och interna identitet team. De är nöjd med OpenIDConnect flöden och förstå identitetsleverantörer och Anspråksbaserad autentisering |
|Metod för principkonfiguration | Azure-portalen med ett användarvänligt gränssnitt | Direkt redigera XML-filer och sedan överföra toohello Azure-portalen |
|Anpassning av Användargränssnittet | Fullständig anpassning av Användargränssnittet, inklusive stöd för HTML, CSS och jscript (kräver anpassade domäner)<br><br>Stöd för flera språk med anpassade strängar | samma |
| Attributanpassning | Standard- och anpassade attribut | samma |
|Hantering av token och session | Anpassade token och flera olika sessionsalternativ för | samma |
|Identitetsleverantörer| **Idag**: fördefinierade lokal, sociala provider<br><br>**Framtida**: standardbaserad OIDC SAML OAuth | **Idag**: standardbaserad OIDC OAUTH SAML<br><br>**Framtida**: WsFed |
|Identitet uppgifter (exempel) | Registrering eller inloggning med lokala och många sociala konton<br><br>Återställning av lösenord för självbetjäning<br><br>Redigera profil<br><br>Multi-Factor Authentication-scenarier<br><br>Anpassa token och sessioner<br><br>Åtkomsttoken flödar | Slutföra hello samma uppgifter som inbyggda principer med hjälp av anpassade identitetsleverantörer eller använda anpassade omfattningar<br><br>Etablera användare i ett annat system hello för närvarande av registrering<br><br>Skicka ett välkomstmeddelande med din egen e-post-leverantör<br><br>Använd en användararkivet utanför B2C<br><br>Verifiera användarens angivna information med en betrodd dator via API |

## <a name="policy-files"></a>Principfiler

En anpassad princip representeras som en eller flera XML-formaterade filer som finns tooeach andra i en hierarkisk kedja. Definiera hello XML-element: anspråk schemat anspråk omvandlingar, definitioner av innehåll, anspråk providers-tekniska profiler och Userjourney orchestration-steg, bland annat.

Vi rekommenderar hello användning av tre typer av principfiler:

- **En grundläggande fil**, som innehåller de flesta av hello definitioner och för som Azure tillhandahåller ett fullständigt exempel.  Vi rekommenderar att du gör ett minsta antal ändringar toothis filen toohelp med felsökning och långsiktig underhåll av dina principer
- **en fil i tillägg** som innehåller hello unika konfigurationsändringar för din klient
- **en förlitande part (RP)-fil** hello enda fokuserad aktivitet filen som anropas direkt av hello programmet eller tjänsten (även kallat förlitande part).  Läs hello artikel om principdefinitioner för mer information.  Varje unik aktiviteten kräver sin egen RP och beroende på anpassningen krav hello antalet vara ”Totalt program x Totalt antal användningsområden”.


Inbyggda principer i Azure AD B2C följer hello 3-fil mönster som beskrivs ovan, men hello developer endast ser hello förlitande part (RP)-fil, medan hello portal gör ändringar i hello bakgrund toohello EXTenstions-filen.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Grundläggande begrepp som du bör känna till när du använder anpassade principer

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azures identitets- och hantering (CIAM) kundtjänst. hello tjänsten omfattar:

1. En användarkatalog i hello form av en särskild syfte Azure Active Directory tillgänglig via Microsoft Graph och som innehåller användardata för både lokala och konton för federerade 
2. Komma åt toohello **identitet upplevelse Framework** som samordnar förtroende mellan användare och enheter och skickar anspråk mellan dem toocomplete en identity-/ access management-aktivitet 
3. En säkerhetstokentjänst (STS) utfärda id token, uppdatera token och åtkomst-token (och motsvarande SAML intyg) och godkänna dem tooprotect resurser.

Azure AD B2C samverkar med identitetsleverantörer, användare, andra system och hello lokala användarkatalog i sekvens tooachieve en identity-aktivitet (t.ex. en användare logga in registrera en ny användare, återställa ett lösenord). hello underliggande plattformen som upprättar flerparti förtroende och utför dessa steg kallas hello identitet upplevelse Framework och en princip (kallas även för en användare resa eller en förtroendeprincipen framework) definierar hello aktörer, hello åtgärder hello protokoll och hello serie steg toocomplete.

### <a name="identity-experience-framework"></a>Identity Experience Framework

Ett fullständigt konfigurerbara, principer, molnbaserad Azure-plattformen som samordnar förtroende mellan enheter (brett Anspråksproviders) i standardprotokoll format, till exempel OpenIDConnect, OAuth, SAML, WSFed och några standardmässiga format (t.ex. REST API-baserade system till anspråk utbyte). Hej I2E skapar användarvänliga, whitelabelled inträffar som stöd för HTML, CSS och jscript.  Idag hello identitet upplevelse Framework finns bara i hello hello Azure AD B2C-tjänsten och prioriteras för uppgifter relaterade tooCIAM.

### <a name="built-in-policies"></a>Inbyggda principer

Fördefinierade konfigurationsfiler att direkt hello beteendet för Azure AD B2C tooperform hello vanligaste identitet uppgifter (d.v.s. användarregistrering, inloggning, återställning av lösenord) och interagera med betrodda parter vars relation också är fördefinierade i Azure AD B2C ( till exempel Facebook identitetsleverantör, LinkedIn, Account, Google-konton).  I framtida hello tillhandahåller också inbyggda principer för anpassning av identitetsleverantörer som vanligtvis är i hello enterprise sfär som Azure Active Directory Premium, Active Directory/ADFS o.s.v. Salesforce-ID-providern.


### <a name="custom-policies"></a>anpassade principer

Konfigurationsfiler som definierar hello beteendet för identitet upplevelse Framework i din Azure AD B2C-klient. En anpassad princip är tillgänglig som en eller flera XML-filer (se principfiler definitioner) som körs av hello identitet upplevelse Framework när den anropades av en förlitande part (t.ex. ett program). Anpassade principer kan vara direkt redigeras av en identitet developer toocomplete en nära obegränsat antal uppgifter. Utvecklare konfigurera anpassade principer måste definiera Hej betrodda relationer i noggrann detalj tooinclude Metadataslutpunkter, exakt anspråk exchange definitioner och konfigurerar hemligheter, nycklar och certifikat som krävs av varje identitetsleverantör.

## <a name="policy-file-definitions-for-identity-experience-framework-trustframeworks"></a>Princip för definitioner för identitet upplevelse Framework Trustframeworks

### <a name="policy-files"></a>Principfiler

En anpassad princip representeras som en eller flera XML-formaterade filer som finns tooeach andra i en hierarkisk kedja. Definiera hello XML-element: anspråk schemat anspråk omvandlingar, definitioner av innehåll, anspråk providers-tekniska profiler och Userjourney orchestration-steg, bland annat.  Vi rekommenderar hello användning av tre typer av principfiler:

- **En grundläggande fil**, som innehåller de flesta av hello definitioner och för som Azure tillhandahåller ett fullständigt exempel.  Vi rekommenderar att du gör ett minsta antal ändringar toothis filen toohelp med felsökning och långsiktig underhåll av dina principer
- **en fil i tillägg** som innehåller hello unika konfigurationsändringar för din klient
- **en förlitande part (RP)-fil** hello enda fokuserad aktivitet filen som anropas direkt av hello programmet eller tjänsten (även kallat förlitande part).  Läs hello artikel om principdefinitioner för mer information.  Varje unik aktiviteten kräver sin egen RP och beroende på anpassningen krav hello antalet vara ”Totalt program x Totalt antal användningsområden”.

![Princip för filtyper](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Typ av princip-fil | Exempel filnamn | Rekommenderad användning | Ärver från |
|---------------------|--------------------|-----------------|---------------|
| BAS |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_BASE1.xml | Innehåller hello grundläggande anspråk schemat, anspråksomvandlingar, Anspråksproviders och användaren resor konfigureras av Microsoft<br><br>Kontrollera minimala ändringar toothis filen | Ingen |
| Tillägget (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | Konsolidera dina ändringar toohello bas-fil här<br><br>Ändrade Anspråksproviders<br><br>Ändrade användaren resor<br><br>Anpassat schemadefinitionerna | BAS-fil |
| Förlitande part (RP) | B2C_1A_sign_up_sign_in.XML| Ändra token form och session här| Extensions(ext) fil |

### <a name="inheritance-model"></a>Arv modellen

När ett program anropar hello RP principfil, hello identitet upplevelse ramverk i B2C Lägg till alla hello element från basen och tillägg, och till sist i praktiken hello aktuella princip från hello RP princip filen tooassemble.  Element i hello samma Skriv och namn i hello RP-filen åsidosätter de i hello tillägg och tillägg åsidosättningar BASE.

**Principer för inbyggda** i Azure AD B2C följer hello 3-fil mönster som beskrivs ovan, men hello developer endast ser hello förlitande part (RP)-fil, medan hello portal gör ändringar i hello bakgrund toohello EXTenstions-filen.  Alla Azure AD B2C delar en grundläggande principfil som är under hello kontroll av hello Azure B2C-teamet och uppdateras ofta.
