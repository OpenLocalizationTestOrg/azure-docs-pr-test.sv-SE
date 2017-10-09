---
title: "aaaAzure Active Directory-riskhändelser | Microsoft Docs"
description: "Det här avsnittet ger en detaljerad översikt över riskhändelser är."
services: active-directory
keywords: "Azure active directory identitetsskydd, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
author: MarkusVi
manager: femila
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d771c1f43707744aac728c4f72000d855cbd6e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory-riskhändelser

hello majoriteten av security överträdelser ta placera när angripare får åtkomst tooan miljö genom att stjäla en användares identitet. Identifiering av avslöjade identiteter finns ingen enkel aktivitet. Azure Active Directory använder anpassningsbar maskininlärning algoritmer och heuristik toodetect misstänkta åtgärder som är relaterade tooyour användarkonton. Alla upptäckta misstänkta åtgärd lagras i en post som kallas *risk händelsen*.

För närvarande identifierar Azure Active Directory sex typer av riskhändelser:

- [Användare med läckta autentiseringsuppgifter](#leaked-credentials) 
- [Inloggningar från anonyma IP-adresser](#sign-ins-from-anonymous-ip-addresses) 
- [Omöjlig resa tooatypical platser](#impossible-travel-to-atypical-locations) 
- [Inloggningar från okända platser](#sign-in-from-unfamiliar-locations)
- [Inloggningar från infekterade enheter](#sign-ins-from-infected-devices) 
- [Inloggningar från IP-adresser med misstänkt aktivitet](#sign-ins-from-ip-addresses-with-suspicious-activity) 


![Risk händelse](./media/active-directory-reporting-risk-events/91.png)

Det här avsnittet får du en detaljerad översikt över vilka riskhändelser är och hur du kan använda dem tooprotect identiteter Azure AD.


## <a name="risk-event-types"></a>Typer av riskhändelser

hello risk händelseegenskap typ är en identifierare för hello misstänkta åtgärden en risk händelseposten har skapats för.  
Microsofts kontinuerliga investeringar i hello identifieringsprocessen leda till:

- Förbättringar av toohello identifiering riktighet befintliga riskhändelser 
- Ny risk händelsetyper som kommer att läggas till i framtida hello

### <a name="leaked-credentials"></a>Läckta autentiseringsuppgifter

När andra nätkriminella utgör ett giltigt lösenord för behöriga användare, dela hello kriminella ofta dessa autentiseringsuppgifter. Detta görs normalt genom att publicera dem offentligt för hello mörkt webbplatser eller klistra in platser eller genom att kompromissa eller sälja hello autentiseringsuppgifter på hello svarta marknaden. hello Microsoft läcka ut autentiseringsuppgifter service hämtar användarnamn / lösenord par genom att övervaka offentliga och mörkt webbplatser och genom att arbeta med:

- Forskare
- Polis och rättsväsende
- Säkerhet team på Microsoft
- Andra tillförlitliga källor 

När tjänsten hello får användarnamn / lösenord par de kontrolleras mot AAD användarnas aktuella giltiga autentiseringsuppgifter. När en matchning hittas, innebär det att en användares lösenord har komprometterats, och en *läcka ut autentiseringsuppgifter risk händelse* skapas.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inloggningar från anonyma IP-adresser

Den här typen av risk händelse identifierar användare som har loggat in från en IP-adress har identifierats som en anonym proxyserver IP-adress. Dessa proxyservrar är som används av användare som vill toohide sina enhetens IP-adress och kan användas för skadliga åtgärder.


### <a name="impossible-travel-tooatypical-locations"></a>Omöjlig resa tooatypical platser

Den här typen av risk händelse identifierar två inloggningar som sker från geografiskt avlägsna platser, där minst en av hello platser kanske också ovanliga för hello användare, anges tidigare beteende. Dessutom hello tiden mellan hello två inloggningar är kortare än hello tiden det skulle ha tagit hello användaren tootravel från hello första plats toohello andra, som anger att en annan användare använder hello samma autentiseringsuppgifter. 

Den här maskininlärningsalgoritmen som ignorerar uppenbara ”*falska positiva identifieringar*” bidrar toohello omöjligt att resa villkor, till exempel VPN och platser som regelbundet används av andra användare i organisationen hello.  hello system har en inledande learning-period på 14 dagar då den lär sig en ny användare loggar in beteende.

### <a name="sign-in-from-unfamiliar-locations"></a>Logga in från okända platser

Den här typen av risk händelse anser tidigare inloggning platser (IP, latitud / longitud och ASN) toodetermine nya / okända platser. hello system lagrar information om tidigare platser som används av en användare så att dessa ”bekant” platser. hello risk händelsen utlöses när hello inloggningen sker från en plats som inte redan hello listan över välkända platser. hello system har en inledande learning-period på 30 dagar då inte flaggas några nya platser som okända platser. hello också ignoreras inloggningar från bekant enheter och platser som är geografiskt Stäng tooa bekant plats. 

### <a name="sign-ins-from-infected-devices"></a>Inloggningar från angripna enheter

Den här typen av risk händelse identifierar inloggningar från enheter som infekterats med skadlig kod, som är kända tooactively kommunicera med en botserver. Detta bestäms genom att sammanföra IP-adresser för hello användarens enhet mot IP-adresser som varit i kontakt med en botserver. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Inloggningar från IP-adresser med misstänkt aktivitet
Den här typen av risk händelse identifierar IP-adresser som ett stort antal misslyckade inloggningsförsök visades, för flera användarkonton under en kort tidsperiod. Detta matchar trafikmönster för IP-adresser som används av angripare och är en starkt indikator konton antingen redan eller om toobe komprometteras. Det här är en maskininlärningsalgoritmen som ignorerar uppenbara ”*FALSKT positiva*”, till exempel IP-adresser som används av andra användare i organisationen hello regelbundet.  hello system har en inledande learning-period på 14 dagar där den lär sig hello inloggning beteendet för en ny användare och nya innehavaren.


## <a name="detection-type"></a>Identifiering av typen

hello identifiering typegenskapen är en indikator (realtid eller Offline) för hello identifiering tidsram för en händelse för risk.  
För närvarande kan identifieras de flesta riskhändelser offline i en åtgärd för efterbearbetning när hello risk händelse har inträffat.

hello i den följande tabellen listas hello lång tid det tar för en identifiering av typen tooshow i en relaterad rapport:

| Identifiering av typen | Rapportering svarstid |
| --- | --- |
| Realtidsskydd | too10 5 minuter |
| Offline | 2 too4 timmar |


För hello risk händelsetyper Azure Active Directory identifierar, är hello identifiering typer:

| Risk händelsetyp | Identifiering av typen |
| :-- | --- | 
| [Användare med läckta autentiseringsuppgifter](#leaked-credentials) | Offline |
| [Inloggningar från anonyma IP-adresser](#sign-ins-from-anonymous-ip-addresses) | Realtidsskydd |
| [Omöjlig resa tooatypical platser](#impossible-travel-to-atypical-locations) | Offline |
| [Inloggningar från okända platser](#sign-in-from-unfamiliar-locations) | Realtidsskydd |
| [Inloggningar från infekterade enheter](#sign-ins-from-infected-devices) | Offline |
| [Inloggningar från IP-adresser med misstänkt aktivitet](#sign-ins-from-ip-addresses-with-suspicious-activity) | Offline|


## <a name="risk-level"></a>Risknivå

hello risk level-egenskap för en händelse för risk är en indikator (hög, medel eller låg) för hello allvarlighetsgrad och hello förtroende för en händelse för risk. Den här egenskapen kan du tooprioritize hello åtgärder du måste utföra. 

hello allvarlighetsgraden hello risk händelse representerar hello styrkan hos hello signal som en ge prognoser för identitet har komprometterats.  
hello förtroende är en indikator hello möjlighet till falska positiva identifieringar. 

Exempel: 

* **Hög**: hög exakthet och hög allvarlighetsgrad risk händelse. Dessa händelser är starkt indikatorer hello användarens identitet har komprometterats, och alla användarkonton som påverkas bör åtgärdas omedelbart.

* **Medel**: Hög allvarlighetsgrad, men lägre förtroende risk händelse, och vice versa. Dessa händelser är potentiellt riskfyllda och alla användarkonton som påverkas bör åtgärdas.

* **Låg**: låg förtroende och låg angelägenhetsgrad risk händelse. Den här händelsen kanske inte kräver en omedelbar åtgärd, men när den kombineras med andra riskhändelser ger en stark indikation på att hello identitet har komprometterats.

![Risknivå](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Läckta autentiseringsuppgifter

Läcka ut autentiseringsuppgifter riskhändelser klassificeras som en **hög**, eftersom de ger en tydlig indikation hello användarnamn och lösenord är tillgängliga tooan angripare.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inloggningar från anonyma IP-adresser

hello risknivå för den här typen av risk händelse är **medel** eftersom en anonym IP-adress inte är en stark indikation på ett konto har komprometterats.  
Vi rekommenderar att du omedelbart kontaktar hello användaren tooverify om de använder anonym IP-adresser.


### <a name="impossible-travel-tooatypical-locations"></a>Omöjlig resa tooatypical platser

Omöjlig resa är vanligtvis en god indikator för att en hackare har inloggning kan toosuccessfully. FALSKT positiva kan dock uppstå när en användare reser med hjälp av en ny enhet eller en VPN-anslutning som normalt inte används av andra användare i organisationen hello. En annan källa för falskt positiva är program som skickar felaktigt server IP-adresser som klient IP-adresser, som kan ge hello utseende av inloggningar äger rum från hello datacenter där programmet har backend-värd (det är ofta Microsoft datacenter vilket kan ge hello intryck av inloggningar äger rum från Microsoft ägs IP-adresser). På grund av dessa FALSKT positiva hello risknivå för den här risken händelsen är **medel**.

> [!TIP]
> Du kan minska hello rapporterade false-positves för den här typen av risk händelsen genom att konfigurera [med namnet platser](active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Logga in från okända platser

Okända platser kan ge en stark indikation på att en angripare är kan toouse en stulen identitet. FALSE-positiva identifieringar kan uppstå när en användare reser, testar en ny enhet eller använder en ny VPN-anslutning. På grund av dessa falska positiva identifieringar hello risknivå för den här händelsen är **medel**.

### <a name="sign-ins-from-infected-devices"></a>Inloggningar från angripna enheter

Den här risken händelsen identifierar IP-adresser, inte användarenheter. Om flera enheter som finns bakom en IP-adress och endast vissa är styrs av ett bot nätverk, inloggningar från andra enheter min utlösaren den här händelsen i onödan, vilket är hello orsak för klassificering av den här händelsen risk som **låg**.  

Vi rekommenderar att du kontaktar hello användare och söka igenom alla hello användarens enheter. Det är också möjligt att en användares personliga enhet har infekterats eller som tidigare nämnts att någon annan har använt en infekterad enhet från hello samma IP-adressen som hello användare. Infekterade enheter är ofta smittats av skadlig kod som ännu inte identifierats av antivirusprogram och kan också indikera som felaktiga användaren vanor som kan ha orsakat hello enheten toobecome infekterade.

Mer information om hur tooaddress skadlig kod finns hello [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Inloggningar från IP-adresser med misstänkt aktivitet

Vi rekommenderar att du kontaktar hello användaren tooverify om de faktiskt har loggat in från en IP-adress som har markerats som misstänkt. hello risknivå för den här händelsen är ”**medelstor**” eftersom flera enheter kanske bakom hello samma IP-adress, även om endast vissa är ansvarig för hello misstänkt aktivitet. 


 
## <a name="next-steps"></a>Nästa steg

Riskhändelser är hello foundation för att skydda din Azure AD identiteter. Azure AD kan för närvarande identifieras sex riskhändelser: 


| Risk händelsetyp | Risknivå | Identifiering av typen |
| :-- | --- | --- |
| [Användare med läckta autentiseringsuppgifter](#leaked-credentials) | Hög | Offline |
| [Inloggningar från anonyma IP-adresser](#sign-ins-from-anonymous-ip-addresses) | Medel | Realtidsskydd |
| [Omöjlig resa tooatypical platser](#impossible-travel-to-atypical-locations) | Medel | Offline |
| [Inloggningar från okända platser](#sign-in-from-unfamiliar-locations) | Medel | Realtidsskydd |
| [Inloggningar från infekterade enheter](#sign-ins-from-infected-devices) | Låg | Offline |
| [Inloggningar från IP-adresser med misstänkt aktivitet](#sign-ins-from-ip-addresses-with-suspicious-activity) | Medel | Offline|

Var du hittar hello riskhändelser som har identifierats i din miljö?
Det finns två platser där du kan granska rapporterade riskhändelser:

 - **Azure AD-rapportering** -riskhändelser är en del av Azure AD-säkerhetsgrupp rapporter. Mer information finns i hello [användare på risk säkerhetsrapporten](active-directory-reporting-security-user-at-risk.md) och hello [riskfyllda inloggningar säkerhetsrapporten](active-directory-reporting-security-risky-sign-ins.md).

 - **Azure AD Identity Protection** -riskhändelser är också en del av [Azure Active Directory Identity Protection](active-directory-identityprotection.md) rapporteringsfunktioner.
    

Medan hello identifiering av riskhändelser redan representerar en viktig del av skydda identiteter, har du också hello alternativet tooeither adress dem manuellt eller även implementera automatiska svar genom att konfigurera principer för villkorlig åtkomst. Mer information finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
 
