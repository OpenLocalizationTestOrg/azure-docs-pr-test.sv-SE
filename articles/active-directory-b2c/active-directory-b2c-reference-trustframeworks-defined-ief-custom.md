---
title: "Azure Active Directory B2C: Referera - förtroende ramverk | Microsoft Docs"
description: Ett avsnitt om Azure Active Directory B2C anpassade principer och hello identitet upplevelse Framework
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
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: d9634da72cb136ac165dd32e735622b5d0e22ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Definiera förtroende ramverk med Azure AD B2C identitet upplevelse Framework

Azure Active Directory B2C (Azure AD B2C) anpassade principer som använder hello identitet upplevelse Framework ge din organisation en centraliserad tjänst. Den här tjänsten minskar hello identitetsfederation i en stor grupp av intresse. hello komplexitet är minskade tooa enkel förtroenderelation och en enda metadatautväxling.

Azure AD B2C anpassade principer som använder hello identitet upplevelse Framework tooenable du tooanswer hello följande frågor:

- Vad är hello juridiska, säkerhet, sekretess och dataskyddsprinciper som måste iakttas?
- Vilka är hello kontakter och vad är hello processer för att bli en auktoriserad deltagare?
- Vem som hello ges information identitetsleverantörer (även kallat ”Anspråksproviders”) och vad de erbjuder?
- Vem som hello ges förlitande parter (och eventuellt vad de behöver)?
- Vad är hello tekniska ”på hello överföring” samverkanskrav för deltagare?
- Vad är hello operativa ”körning” regler som måste tillämpas för att utbyta information digitala identitet?

tooanswer konstruera för alla dessa frågor, Azure AD B2C anpassade principer som använder hello identitet upplevelse Framework används hello förtroende Framework (TF). Nu ska vi titta här konstruktionen och den innehåller.

## <a name="understand-hello-trust-framework-and-federation-management-foundation"></a>Förstå hello förtroende Framework och federation management foundation

hello förtroende Framework är en skriftlig specifikation av hello identitet, säkerhet, sekretess och data protection principer toowhich deltagare i en grupp av intresse måste överensstämma.

Federerade identiteter ger en grund för att uppnå slutanvändarens identitet säkerhet i Internet-skala. Genom att delegera identity management toothird parter kan kan en enda digitala identitet för slutanvändaren återanvändas med flera förlitande parter.  

Garantier identiteten kräver att identitetsleverantörer (IdPs) och attributet providers (AtPs) följer toospecific säkerhet, sekretess och operativa principer och metoder.  Om de inte kan utföra direkt kontroller, måste förlitande parter (RPs) Utveckla förtroenderelationer med hello IdPs och AtPs som de själva väljer toowork med.  

När hello många konsumenter och leverantörer av digitala identitetsinformation växer, det är svårt toocontinue pairwise hantering av dessa förtroenderelationer, eller även hello pairwise exchange hello tekniska metadata som krävs för nätverksanslutning .  Federation NAV har uppnått endast begränsad lyckades lösa dessa problem.

### <a name="what-a-trust-framework-specification-defines"></a>Definierar vad en specifikation av ett förtroende Framework
TFs är hello linchpins av hello öppna identitet Exchange (OIX) förtroende Framework modellen, där varje grupp av intresse styrs av en viss TF-specifikation. En sådan specifikation av TF definierar:

- **Hej mätvärden för säkerhet och sekretess för hello community intressanta hello definition av:**
    - hello säkerhetsnivåer (Kompilera) som erbjuds/krävs av deltagare. till exempel en ordnad uppsättning förtroende klassificeringar för hello digitala identitetsinformation.
    - hello skyddsnivåer (LOP) som erbjuds/krävs av deltagare. till exempel en ordnad uppsättning förtroende klassificeringar för hello skydd av digitala identitets-information som hanteras av deltagare i hello community av intresse.

- **Hej beskrivning av hello digitala identitetsinformation som har erbjuds/krävs av deltagare**.

- **hello tekniska principer för produktion och användning av information om digitala identitet och därmed för att mäta Kompilera och LOP. Dessa skrivs principer normalt omfattar hello följande typer av principer:**
    - Identitet språkverktyg principer, till exempel: *hur starkt är en persons identitetsinformation granskade?*
    - Säkerhetsprinciper, till exempel: *är hur starkt informationsintegritet och sekretess skyddas?*
    - Sekretesspolicy, till exempel: *vilka en användare är över personligt identifierbar information (PII)*?
    - Överlevnads principer, till exempel: *om en provider upphör operations, hur fungerar verksamhetskontinuitet och skydd av funktionen personligt identifierbar information?*

- **hello tekniska profiler för produktion och användning av digital identitetsinformation. Dessa profiler innehåller:**
    - Scope-gränssnitt som digitala identitets-information är tillgänglig vid en angiven Kompilera.
    - Tekniska krav för samverkan under överföring.

- **Hej beskrivningar av hello olika roller som deltagare i hello gemenskapen kan utföras och hello kvalifikationer som är nödvändiga toofulfill rollerna.**

Därför en TF specifikation styr hur identitetsinformation utbyts mellan hello deltagare i hello community intressanta: förlitande parter, identitet och attributet providers och verifiering av attribut.

En TF-specifikation som ett eller flera dokument som fungerar som en referens för hello styrning av hello gemenskapen intressanta som reglerar hello assertion och förbrukningen av digitala identitetsinformation i hello community. Det är en dokumenterade uppsättning principer och procedurerna avsedda tooestablish förtroende i hello digitala identiteter som används för online transaktioner mellan medlemmarna i en grupp av intresse.  

Med andra ord definierar en TF specifikation hello regler för att skapa ett genomförbart federerad identitet ekosystem för en grupp.

Det finns för närvarande omfattande avtalet om hello nytta av ett sådant tillvägagångssätt. Det är säkert att förtroende framework specifikationer underlätta hello utvecklingen av digitala identitets ekosystem med verifierbara egenskaper för säkerhet, säkerhet och sekretess, vilket innebär att de kan återanvändas i flera grupper av intresse.

Därför använder Azure AD B2C anpassade principer som använder hello identitet upplevelse Framework hello-specifikationen som hello dess data representation för en TF toofacilitate samverkan.  

Azure AD B2C anpassade principer som utnyttjar hello identitet upplevelse Framework representerar en TF-specifikation som en blandning av data och människor. Vissa avsnitt i den här modellen (vanligtvis avsnitt som är mer riktade mot styrning) representeras som refererar till dokumentationen för toopublished-säkerhet och sekretess princip tillsammans med hello relaterade procedurer (om sådan finns). Andra avsnitt beskrivs i detalj hello metadata och runtime konfigurationsregler som underlättar operativa automation.

## <a name="understand-trust-framework-policies"></a>Förstå förtroende Framework principer

Vad gäller implementering består hello TF specifikation av en uppsättning principer som tillåter fullständig kontroll över identitet beteende och upplevelser.  Azure AD B2C anpassade principer som utnyttjar hello identitet upplevelse Framework aktiverar du tooauthor och skapa egna TF via sådana deklarativ principer som kan definiera och konfigurera:

- hello dokumentreferens eller referenser som definierar hello federerad identitet ekosystem med hello-community som relaterar toohello TF. De är länkar toohello TF dokumentation. hello (fördefinierade) operativa ”körning” regler eller hello användaren resor att automatisera och/eller styra hello exchange och användning av hello anspråk. Dessa användare resor är associerade med en Kompilera (och en LOP). En princip kan därför ha användaren resor med olika LOAs (och LOPs).

- hello identitets- och leverantörer eller hello anspråk leverantörer, i hello gemenskapen av intresse och hello tekniska profiler som de stöder tillsammans med hello (out-of-band) Kompilera/LOP godkännande som rör toothem.

- hello integrering med attributet verifiering eller Anspråksproviders.

- Hej förlitande parter i hello community (genom härledning).

- hello metadata för att fastställa nätverkskommunikation mellan deltagare. Dessa metadata tillsammans med hello tekniska profiler, används under en transaktion tooplumb ”på hello överföring” samverkan mellan hello förlitande part och andra community-deltagare.

- Hej protokollet konvertering eventuella (till exempel SAML, OAuth2, WS-Federation och OpenID Connect).

- hello autentiseringskrav.

- Hej multifaktor orchestration eventuella.

- Ett delat schema för alla hello anspråk som är tillgängliga och mappningar tooparticipants för en grupp av intresse.

- Alla hello anspråk transformationer tillsammans med hello möjliga data minimering i den här kontexten, toosustain hello exchange och användning av hello anspråk.

- hello-bindning och kryptering.

- hello anspråk lagring.

### <a name="understand-claims"></a>Förstå anspråk

> [!NOTE]
> Vi gemensamt refererar tooall hello möjliga typer av identitetsinformation som kan utväxlas som ”anspråk”: anspråk om Autentiseringsbehörigheten för en slutanvändare, de prövas identitet, kommunikationsenhet, fysisk placering, identifiera personligt attribut och så vidare.  
>
> Vi använder hello termen ”anspråk”--snarare än ”attribut”--eftersom online transaktioner dessa data artefakter inte fakta som direkt kan verifieras av hello förlitande part. De är i stället intyg eller anspråk om fakta vilka hello förlitande part måste se till att utveckla tillräcklig förtroende toogrant hello slutanvändarens begärda transaktionen.  
>
> Vi använder också hello termen ”anspråk” eftersom Azure AD B2C anpassade principer som använder hello identitet upplevelse Framework är utformat toosimplify hello utbyte av alla typer av digitala identitets-information på ett konsekvent sätt oberoende av om hello underliggande protokollet har definierats för hämtning av användaren autentisering eller attribut.  På samma sätt kan använda vi hello termen ”anspråk providers” toocollectively finns tooidentity providers, attributet providers och verifiering av attributet när vi inte vill toodistinguish mellan funktionerna.   

Därför styr de hur identitetsinformation utbyts mellan en förlitande part, identitet och attributet providers och verifiering av attribut. De styr vilka identitet och attributet providers krävs för autentisering för en förlitande part. De bör betraktas som ett domänspecifika språk (DSL), som är ett datorspråk som har specialiserat för en viss programdomänen med arv, *om* instruktioner, polymorfism.

Dessa principer utgöra hello maskinläsbar delen av hello TF konstruera i Azure AD B2C anpassade principer som utnyttjar hello identitet upplevelse Framework. De omfattar alla hello information, inklusive Anspråksproviders metadata och tekniska profiler, schemadefinitioner för anspråk, funktioner för omvandling av anspråk och användaren resor som fylls i toofacilitate operativa orchestration och automatisering.  

De antas toobe *levande dokument* eftersom det finns en bra chans att innehållet kommer att ändras med tiden om hello active deltagare som deklarerats i hello principer. Det finns också hello risk att hello villkor för att vara en deltagare kan ändras.  

Federation-konfiguration och underhåll frågeprestanda avsevärt förenklas genom avskärmning förlitande parter från pågående förtroende och anslutningen reconfigurations som providers/verifiering av olika anspråk gå med i eller lämna (hello community representeras av) hello uppsättning principer.

Driftskompatibilitet är ett annat betydande utmaning. Ytterligare anspråk providers/verifiering måste vara integrerade, eftersom förlitande parter är inte troligt toosupport alla hello nödvändiga protokoll. Azure AD B2C anpassade principer lösa problemet genom att stödja branschstandardprotokoll och genom att använda specifika användare resor hello tootranspose begäranden när attributet providers och förlitande parter inte stöder samma protokoll.  

Användaren resor inkluderar protokollet profiler och metadata som används tooplumb ”på hello överföring” samverkan mellan hello förlitande part och andra deltagare. Det finns också operativa runtime-regler som är tillämpade tooidentity information exchange begäran och svar-meddelanden för att tillämpa kompatibilitet med publicerade principer som en del av hello TF specifikation. hello uppfattning om användaren resor är viktiga toohello anpassning av hello kundupplevelsen. Det kan också sheds ljus på hur hello system fungerar på protokollnivå hello.

På grundval kan, förlitande parters program och portaler beroende på deras kontexten anropa Azure AD B2C anpassade principer som utnyttjar hello identitet upplevelse Framework skicka hello namnet på en specifik princip och exakt hello beteende och information Exchange som de vill utan någon muss ansträngning och risker.
