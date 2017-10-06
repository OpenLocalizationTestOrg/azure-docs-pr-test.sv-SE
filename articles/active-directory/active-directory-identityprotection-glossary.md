---
title: "aaaAzure Active Directory Identity Protection – ordlista | Microsoft Docs"
description: "Azure Active Directory Identity Protection – ordlista"
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem, säkerhetsprinciper och ordlista"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ff2e96d20e2a3f1df24b78e66be5a0c6807e60a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Identity Protection – ordlista
### <a name="at-risk-user"></a>Risk (användare)
En användare med en eller flera aktiva riskhändelser. 

### <a name="atypical-sign-in-location"></a>Ovanlig inloggning plats
En inloggning från en geografisk plats som inte är typiska för hello specifika användare, liknande användare eller hello-klient.

### <a name="azure-ad-identity-protection"></a>Azure AD Identity Protection
En säkerhetsmodul för Azure Active Directory som tillhandahåller en samlad vy riskhändelser och potentiella säkerhetsproblem som påverkar en organisations identiteter.

### <a name="conditional-access"></a>Villkorlig åtkomst
En princip för att skydda åtkomst tooresources. Villkorliga tillgångsregler lagras i hello Azure Active Directory och utvärderas av Azure AD innan du beviljar åtkomst toohello resurs.  Exempel regler är att begränsa åtkomst baserat på plats, enhet hälsa eller user autentiseringsmetod.

### <a name="credentials"></a>Autentiseringsuppgifter
Information om identifiering och bekräftelse för identifiering som används toogain åtkomst toolocal och nätverksresurser. Exempel på autentiseringsuppgifter är användarnamn och lösenord, smartkort och certifikat.

### <a name="event"></a>Händelse
En post för en aktivitet i Azure Active Directory.

### <a name="false-positive-risk-event"></a>Falska positiva (risk händelse)
En risk händelsestatus ange manuellt av en Identity Protection-användare, som anger att hello risk händelsen undersöktes och felaktigt har flaggats som en händelse för risk.

### <a name="identity"></a>Identitet
En person eller enhet som måste verifieras genom autentisering, baserat på kriterier, till exempel lösenord eller ett certifikat.

### <a name="identity-risk-event"></a>Identitet risk händelse
AAD-händelse som har flaggats som avvikande av Identity Protection och kan tyda på att en identitet har komprometterats.

### <a name="ignored-risk-event"></a>Ignoreras (risk händelse)
En risk händelsestatus ange manuellt av en Identity Protection-användare som visar hello risk händelsen har stängts utan att ta en Reparationsåtgärd.

### <a name="impossible-travel-from-atypical-locations"></a>Omöjlig resa från ovanliga platser
En risk händelsen som utlöses när två inloggningar för hello samma användare identifieras, där minst en av dem är från en ovanlig inloggning plats, och där hello tiden mellan hello inloggningar är kortare än minsta hello tid det tar toophysically färdas mellan dessa platser.  

### <a name="investigation"></a>Undersökning
hello granska hello aktiviteter, loggar och annan relevant information relaterade tooa risk händelsen toodecide om reparation eller minimering steg är nödvändiga, förstå om och hur hello identitet har drabbats och förstå hur hello avslöjade identitet användes.

### <a name="leaked-credentials"></a>Läckta autentiseringsuppgifter
En risk händelsen som utlöses när den aktuella användarens autentiseringsuppgifter (användarnamn och lösenord) hittas upp offentligt i hello mörkt webb av våra forskare.

### <a name="mitigation"></a>Åtgärd
En åtgärd toolimit eller eliminera hello möjligheten för en angripare tooexploit en komprometterad identitet eller en enhet utan att återställa hello identitet eller enheten tooa felsäkert läge. En lösning kan inte matchas tidigare riskhändelser som är associerade med hello identitet eller enhet.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
En autentiseringsmetod som kräver två eller flera autentiseringsmetoder, som kan innehålla något hello användaren har certifikatet. något hello användaren känner till exempel användarnamn, lösenord eller pass fraser. fysiska attribut, till exempel ett tumavtryck; och personliga attribut, till exempel en personlig signatur.

### <a name="offline-detection"></a>Offline-identifiering
hello identifiera avvikelser och utvärdering av hello risken för en händelse som inloggning försök efter hello faktum för en händelse som redan har skett.

### <a name="policy-condition"></a>Villkor
En del av en säkerhetsprincip som definierar hello entiteter (grupper, användare, appar, enhetsplattformar, enhetstillstånden, IP-adressintervall, klienttyper) ingår i hello principen eller uteslutas från den.

### <a name="policy-rule"></a>Principregel
hello del av en säkerhetsprincip som beskriver hello omständigheter som ska utlösa hello principen och hello åtgärder när hello princip utlöses.

### <a name="prevention"></a>Prevention (Skydd)
En åtgärd tooprevent skada toohello organisation via missbruk av en identitet eller enhet misstänkt eller vet toobe komprometteras. En åtgärd för att förhindra skyddar inte hello enhet eller identitet och matchar inte tidigare riskhändelser.

### <a name="privileged-user"></a>Privilegierad (användare)
En användare som för närvarande hello av en risk-händelse har permanenta eller tillfälliga admin behörigheter tooone eller mer resurser i Azure Active Directory, till exempel en Global administratör faktureringsadministratör, administratör, Användaradministratör och lösenord Administratören. 

### <a name="real-time"></a>Realtidsskydd
Finns i realtid identifiering.

### <a name="real-time-detection"></a>Realtid identifiering
hello identifiera avvikelser och utvärdering av hello risken för en händelse som inloggning försök innan hello händelse tillåts tooproceed.

### <a name="remediated-risk-event"></a>Åtgärdad (risk händelse)
En risk händelsestatus anges automatiskt av identitetsskydd, som visar hello risk händelsen har reparerats med hello standard Reparationsåtgärd för den här typen av risk händelse. Till exempel åtgärdas automatiskt många riskhändelser som visar hello tidigare lösenordet har drabbats när hello användarens lösenord återställs.

### <a name="remediation"></a>Reparation
En åtgärd toosecure en identitet eller en enhet som har tidigare eller misstänks toobe komprometteras. En Reparationsåtgärd återställer hello identitet eller enheten tooa säkert tillstånd och löser tidigare riskhändelser som är associerade med hello identitet eller enhet.

### <a name="resolved-risk-event"></a>Löst (risk händelse)
En risk händelsestatus ange manuellt av en Identity Protection-användare som visar att användaren hello tog en lämplig Reparationsåtgärd utanför identitetsskydd och hello risk händelsen ska betraktas som stängd.

### <a name="risk-event-status"></a>Risk händelsestatus
En egenskap för en risk händelse som anger om hello händelse är aktiv och om stängda hello orsak för att stänga den.

### <a name="risk-event-type"></a>Risk händelsetyp
En kategori för hello riskerar händelsen, som visar hello typ av avvikelseidentifiering som orsakade hello händelse toobe anses vara riskfyllda.

### <a name="risk-level-risk-event"></a>Risknivå (risk händelse)
Uppgift (hög, medel eller låg) om hello allvarlighetsgraden hello risk händelse toohelp identitetsskydd användare prioritera hello åtgärder de utför tooreduce hello risk tootheir organisation. 

### <a name="risk-level-sign-in"></a>Risknivå (inloggning)
Uppgift (hög, medel eller låg) om hello sannolikheten att för en specifik inloggning, någon annan försöker toouse hello användarens identitet.

### <a name="risk-level-user-compromise"></a>Risknivå (användare kompromettering)
Uppgift (hög, medel eller låg) om hello sannolikheten att en identitet har komprometterats.

### <a name="risk-level-vulnerability"></a>Risknivå (säkerhetsproblem)
Uppgift (hög, medel eller låg) om hello allvarlighetsgraden hello säkerhetsproblem toohelp identitetsskydd användare prioritera hello åtgärder de utför tooreduce hello risk tootheir organisation.

### <a name="secure-identity"></a>Skydda (identitet)
Ta Reparationsåtgärd t.ex ändra lösenordet eller datorn återställning av avbildning toorestore ett potentiellt komprometterade identitet tooan kompromisslös tillstånd.

### <a name="security-policy"></a>Säkerhetsprincip
En uppsättning regler och villkor. En princip kan vara tillämpade tooentities, till exempel användare, grupper, appar, enheter, enhetsplattformar, enhetstillstånden, IP-adressintervall och Auth2.0 klienttyper. När en princip är aktiverad utvärderas när en enhet som ingår i hello principen utfärdas en token för en resurs.

### <a name="sign-in-v"></a>Logga in (v)
tooauthenticate tooan identitet i Azure Active Directory.

### <a name="sign-in-n"></a>Logga in (n)
hello process eller åtgärd för att autentisera en identitet i Azure Active Directory och hello händelse som samlar in den här åtgärden.

### <a name="sign-in-from-anonymous-ip-address"></a>Logga in från anonyma IP-adress
En risk händelse utlöses efter en lyckad inloggning från IP-adress som har identifierats som en anonym proxyserver IP-adress.

### <a name="sign-in-from-infected-device"></a>Logga in från infekterade enheter
En risk händelsen som utlöses när en inloggning som kommer från en IP-adress som är känd toobe som används av en eller flera avslöjade enheter som försöker aktivt toocommunicate med en botserver.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Logga in från IP-adresser med misstänkt aktivitet
En risk händelsen utlöses när en lyckad inloggning från en IP-adress med ett stort antal misslyckade inloggningsförsök för flera användarkonton under en kort tidsperiod.

### <a name="sign-in-from-unfamiliar-location"></a>Logga in från okänd plats
En risk händelsen som utlöses när en användare loggar in från en ny plats (IP, latitud/longitud och ASN) har.

### <a name="sign-in-risk"></a>Logga in risk
Finns Risk nivå (inloggning)

### <a name="sign-in-risk-policy"></a>Logga in riskprincipen
En villkorlig åtkomstprincip som utvärderar hello risk tooa specifika inloggning och tillämpar åtgärder baserat på fördefinierade villkor och regler.

### <a name="user-compromise-risk"></a>Användaren har komprometterats risk
Finns Risk nivå (användare kompromettering)

### <a name="user-risk"></a>Användaren risk
Finns Risk nivå (användare kompromettering).

### <a name="user-risk-policy"></a>Risk användarprincip
En villkorlig åtkomstprincip som överväger hello-inloggning och tillämpar åtgärder baserat på fördefinierade villkor och regler.

### <a name="users-flagged-for-risk"></a>Användare som har flaggats för risk
Användare som har riskhändelser som är aktiva eller reparerade

### <a name="vulnerability"></a>Säkerhetsproblem
En konfiguration eller villkor i Azure Active Directory som gör hello directory mottagliga tooexploits eller hot.

## <a name="see-also"></a>Se även
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

