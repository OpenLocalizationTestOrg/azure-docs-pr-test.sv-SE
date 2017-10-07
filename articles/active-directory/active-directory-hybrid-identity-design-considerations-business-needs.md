---
title: "aaaAzure designöverväganden Active Directory hybrid identity - fastställa identitetskrav | Microsoft Docs"
description: "Identifiera hello företagets affärsbehov som leda toodefine hello kraven för utformningen av hello hybrid identity."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: b2f1cad923b0f08ededa0d8f9a4ea8e799956e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Ange identitetskrav för för din hybrididentitetslösning
hello första steget i utforma en hybrididentitetslösning är toodetermine hello krav för hello organisationen som kommer att använda den här lösningen.  Hybrididentitet startas som en stödjande roll (stöds alla andra molnlösningar genom att tillhandahålla autentisering) och tooprovide nya och intressanta funktioner som att låsa upp nya arbetsbelastningar för användare.  Dessa arbetsbelastningar eller tjänster som du vill tooadopt för dina användare styr hello kraven för utformningen av hello hybrid identity.  Dessa tjänster och arbetsbelastningar måste tooleverage hybrididentitet både lokalt och i hello molnet.  

Du behöver toogo igenom dessa viktiga aspekter av hello business toounderstand vad det är ett krav nu och vilka hello företaget planerar för framtida hello. Om du inte har hello synligheten för hello långsiktig strategi för identitet hybridutformning, är risken att lösningen inte är skalbar när hello verksamheten behöver växa och ändra.   T han diagrammet nedan visar ett exempel på en hybrid identity-arkitektur och hello arbetsbelastningar som är att låsas upp för användare. Detta är bara ett exempel på alla hello nya möjligheter som kan låsas upp och levereras med en fast hybrid identity-strategi. 

Vissa komponenter som ingår i hello hybrid identity-arkitektur![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Fastställa affärsbehov
Alla företag har olika krav, även om dessa företag är en del av hello samma bransch hello verkliga affärskraven variera. Du kan fortfarande utnyttja bästa praxis från branschen hello men slutänden är hello företagets affärsbehov som leda toodefine hello kraven för utformningen av hello hybrid identity. 

Kontrollera att tooanswer hello följande frågor tooidentify ditt företag måste:

* Ditt företag söker efter toocut IT operativa kostnad?
* Ditt företag söker efter toosecure moln tillgångar (infrastruktur, SaaS-appar)?
* Är företaget söker toomodernize IT?
  * Är användarna mer mobila och krävande IT toocreate undantag i din DMZ tooallow annan typ av trafik tooaccess olika resurser?
  * Har företaget äldre appar som behövs toobe publicerade toothese moderna användare men inte är enkelt toorewrite?
  * Har ditt företag behöver tooaccomplish dessa uppgifter och placera den under kontroll på hello samtidigt?
* Är företaget söker toosecure användarnas identitet och minska riskerna genom att nya verktyg som utnyttjar hello expertis för Microsoft Azure-säkerhet expertis lokalt?
* Är företaget försök tooget bort hello dreaded ”externa” konton lokalt och flytta dem toohello moln där de inte längre ett vilande hot i din lokala miljö?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analysera lokala identitetsinfrastruktur
Nu när du har en uppfattning om företagets företagets krav måste tooevaluate din lokala identitetsinfrastruktur. Denna utvärdering är viktigt för att definiera hello tekniska krav toointegrate det aktuella identity lösning toohello moln identitet hanteringssystemet. Se till att tooanswer hello följande frågor:

* Vilka autentisering och auktorisering lösning har ditt företag använder lokalt? 
* Har ditt företag för närvarande några lokala synkroniseringstjänster?
* Använder företaget en tredje parts identitetsleverantörer (IdP)?

Du måste också toobe medveten om hello molntjänster som ditt företag kan ha. Utför en bedömning toounderstand hello aktuella integrering med SaaS IaaS och PaaS-modeller i din miljö är mycket viktigt. Se till att tooanswer hello följande frågor under denna bedömning:

* Har företaget någon integrering med en molntjänstleverantör?
* Om Ja, vilka tjänster som används?
* För närvarande är den här integreringen i produktion eller är en pilot?

> [!NOTE]
> Om du inte har en korrekt mappning för alla dina appar och molntjänster, kan du använda hello Cloud App Discovery-verktyget. Det här verktyget kan ge IT-avdelningen insyn i alla organisationens affärsbehov och konsumenten molnappar. Det gör det enklare än någonsin toodiscover shadow IT i din organisation, inklusive information om användningsmönster och alla användare att komma åt dina molnprogram. tooget igång finns [Cloud app discovery](active-directory-cloudappdiscovery-whatis.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Utvärdera identity integration krav
Sedan måste tooevaluate hello identity integration krav. Denna utvärdering är viktiga toodefine hello tekniska krav för hur användare autentiseras, hur hello organisation förekomst ser ut i hello molnet, hur hello organisation tillåter auktorisering och vilka hello användarupplevelsen är pågående toobe. Se till att tooanswer hello följande frågor:

* Kommer din organisation använder federation, standardautentisering eller båda?
* Är ett krav för federationen?  På grund av hello följande:
  * Kerberos-baserad SSO
  * Företaget har ett lokalt program (antingen inbyggd interna eller 3 part) som använder SAML eller liknande funktioner för federation.
  * MFA via smartkort. RSA SecurID osv.
  * Klienten åtkomstregler som löser hello frågorna nedan:
    1. Kan jag blockera alla extern åtkomst tooOffice 365 baserat på hello hello klientens IP-adress?
    2. Kan jag blockera alla extern åtkomst tooOffice 365, utom Exchange ActiveSync?
    3. Jag kan blockera alla extern åtkomst tooOffice 365, utom webbläsarbaserade appar (OWA, SPO)
    4. Jag kan blockera alla extern åtkomst tooOffice 365 för medlemmar i avsedda AD-grupper
* Säkerhetsgranskning/frågor
* Befintlig investering i federerad autentisering
* Vilket namn vår organisation använder våra domän i hello molnet?
* Har en anpassad domän i hello organisation?
  1. Är domänen offentliga och enkelt kan verifieras via DNS?
  2. Om det inte sedan har du en offentlig domän som kan vara tooregister används ett alternativt UPN i AD?
* Stämmer hello-ID: n för molnet representation? 
* Har organisationen hello appar som kräver integrering med molntjänster?
* Har organisationen hello flera domäner och alla använder standard- eller federerad autentisering?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Utvärdera program som körs i din miljö
Nu när du har en uppfattning om dina lokala och molnet infrastruktur, måste tooevaluate hello program som körs i dessa miljöer. Denna utvärdering är viktigt toodefine hello tekniska krav toointegrate dessa program toohello moln identity management-systemet. Se till att tooanswer hello följande frågor:

* Var kommer våra program bor?
* Kommer användarna åt lokala program?  I hello molnet? Eller båda?
* Finns det planer tootake hello befintliga programbelastningar och flytta dem toohello molnet?
* Finns det planer toodevelop nya program som ska finnas antingen lokalt eller i hello molnet som använder autentisering i molnet?

## <a name="evaluate-user-requirements"></a>Utvärdera användarkrav
Du kan också ha tooevaluate hello användarkrav. Denna utvärdering är viktiga toodefine hello steg som behövs för-boarding och hjälper användare när de flyttar toohello moln. Se till att tooanswer hello följande frågor:

* Kommer användarna åt program lokala?
* Kommer användarna åt program i hello molnet?
* Hur kan användare normalt inloggningen tootheir lokala miljö?
* Hur kommer användare logga in toohello cloud?

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) kommer att överskrida hello alternativen och -tekniker/nackdelar med varje alternativ.  Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Ange krav för directory-synkronisering](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

