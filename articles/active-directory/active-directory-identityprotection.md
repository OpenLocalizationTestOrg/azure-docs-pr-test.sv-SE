---
title: aaaAzure Active Directory-identitetsskydd | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection kan du toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller enheten och toosecure en identitet eller en enhet som tidigare var misstänkt eller kända toobe komprometteras."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a>Identitetsskydd för Azure Active Directory

Azure Active Directory Identity Protection är en funktion i hello Azure AD Premium P2-version som gör det möjligt att:

- Identifiera potentiella problem som påverkar din organisations identiteter

- Konfigurera automatiska svar toodetected misstänkta åtgärder som är relaterade tooyour organisations identiteter  

- Undersöka misstänkt incidenter och vidta lämplig åtgärd tooresolve dem.   


## <a name="getting-started"></a>Komma igång

Microsoft skyddar molnbaserade identiteter för mer än en tio åren. Med Azure Active Directory Identity Protection i din miljö kan du använda hello datorer med samma skydd Microsoft används toosecure identiteter.

hello majoriteten av security överträdelser ta placera när angripare får åtkomst tooan miljö genom att stjäla en användares identitet. Angripare har blivit allt mer effektivt utnyttja från tredje part överträdelser och använda avancerade nätfiskeattacker hello års. När en angripare får åtkomst tooeven låg Privilegierade konton, är det relativt enkelt för dem toogain åt tooimportant företagets resurser via lateral förflyttning.

Till följd av detta måste du:

- Skydda alla identiteter oavsett deras behörighetsnivå

- Proaktivt förhindra avslöjade identiteter som missbrukat

Identifiering av avslöjade identiteter finns ingen enkel aktivitet. Azure Active Directory använder anpassningsbar maskininlärningsalgoritmer och heuristik toodetect avvikelser och misstänkta incidenter som indikerar potentiellt komprometteras identiteter. Med dessa data kan identitetsskydd genererar rapporter och aviseringar som möjliggör tooevaluate hello identifierade problem och vidta lämplig lösning eller reparationsåtgärder.

Azure Active Directory Identity Protection är mer än en övervakning och rapportering av verktyget. tooprotect din organisations identiteter som du kan konfigurera risk-baserade principer som svarar toodetected problem automatiskt när en angiven risknivå har uppnåtts. Dessa principer dessutom tooother villkorlig åtkomst till kontroller som tillhandahålls av Azure Active Directory och EMS, kan blockera automatiskt eller initiera anpassningsbar reparationsåtgärder inklusive lösenordsåterställning och multifaktorautentisering tvingande.


#### <a name="identity-protection-capabilities"></a>Identitetsfunktionerna

**Identifiera säkerhetsrisker och riskfyllda konton:**  

* Att tillhandahålla anpassade rekommendationer tooimprove övergripande säkerhetstillståndet genom att markera säkerhetsrisker
* Beräkning av risknivåer för inloggning
* Beräkning av risknivåer för användaren


**Undersöka riskhändelser:**

* Skicka meddelanden för riskhändelser
* Undersöka riskhändelser med hjälp av relevanta och sammanhangsberoende information
* Tillhandahåller grundläggande arbetsflöden tootrack undersökningar
* Att ge dig tillgång till tooremediation åtgärder, till exempel återställning av lösenord

**Principer för risk-baserad villkorlig åtkomst:**

* Principen toomitigate riskfyllda inloggningar genom att blockera inloggningar eller att kräva multifaktorautentisering utmaningar.
* Principen tooblock eller säker riskfyllda användarkonton
* Principen toorequire användare tooregister för multifaktorautentisering



## <a name="identity-protection-roles"></a>Identity Protection roller

tooload saldo hello hanteringsaktiviteter runt implementeringen identitetsskydd, kan du tilldela flera roller. Azure AD Identity Protection stöder 3 directory roller:

| Roll                         | Kan göra                          | Det går inte att göra
| :--                          | ---                                |  ---   |
| Global administratör         | Fullständig åtkomst tooIdentity skydd, publicera identitetsskydd| |
| Säkerhetsadministratör       | Fullständig åtkomst tooIdentity skydd | Publicera identitetsskydd, återställa lösenord för en användare |
| Säkerhetsläsare              | Endast klar tooIdentity skydd | Publicera identitetsskydd, remidiate användare konfigurera principer, återställa lösenord |




Mer information finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>Detection (Identifiering)

### <a name="vulnerabilities"></a>Säkerhetsrisker

Azure Active Directory-identitetsskydd analyser konfigurationen och identifierar säkerhetsrisker som kan påverka din användaridentitet. Mer information finns i [sårbarheter som identifieras av Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Riskhändelser

Azure Active Directory använder anpassningsbar maskininlärning algoritmer och heuristik toodetect misstänkta åtgärder som är relaterade tooyour användaridentiteter. hello skapas en post för varje upptäckt misstänkt åtgärd. Dessa poster kallas även riskhändelser.  
Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Undersökning
Din resa via Identity Protection startas normalt med hello identitetsskydd instrumentpanelen.

![Reparation](./media/active-directory-identityprotection/1000.png "reparation")

hello instrumentpanelen ger dig tillgång till:

* Rapporter som **användare som har flaggats för risk**, **riskerar händelser** och **säkerhetsrisker**
* Inställningar, till exempel hello konfigurationen av din **säkerhetsprinciper**, **meddelanden** och **multifaktorautentisering registrering**

Det är vanligtvis utgångspunkten för undersökningar, som är hello processen för att granska hello aktiviteter, loggar och annan relevant information relaterad tooa riskerar händelsen toodecide om reparation eller minimering steg är nödvändiga, och hur hello identity var komprometteras och förstå hur hello komprometteras identitet användes.

Du kan koppla din undersökning aktiviteter toohello [meddelanden](active-directory-identityprotection-notifications.md) Azure Active Directory Protection skickar per e-post.

hello följande avsnitt kan du med mer information och hello steg som är relaterade tooan undersökning.  


## <a name="risky-sign-ins"></a>Riskfyllda inloggningar

Azure Active Directory identifierar [riskerar händelsetyper](active-directory-reporting-risk-events.md#risk-event-types) i realtid och offline. Varje händelse för risker som har identifierats för en inloggning för en användare bidrar tooa logiskt koncept kallas riskfyllda inloggning. Är en indikator för en inloggning försök som inte kanske har utförts av hello legitima ägare för ett användarkonto riskfyllda loggar in.


### <a name="sign-in-risk-level"></a>Risknivå för inloggning

Logga in risknivå är en indikation (hög, medel eller låg) på hello sannolikheten att ett försök att logga in inte har utförts av hello legitima ägaren till ett användarkonto.

### <a name="mitigating-sign-in-risk-events"></a>Minimera riskhändelser för inloggning

En lösning är en åtgärd toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller en enhet utan att återställa hello identitet eller enheten tooa felsäkert läge. En lösning kan inte matchas tidigare inloggning riskhändelser som är associerade med hello identitet eller enhet.

toomitigate riskfyllda inloggningar automatiskt, kan du konfigurera inloggning risk säkerhet policicies. Med dessa principer kan du överväga hello risknivå för hello användare eller hello inloggning tooblock riskfyllda inloggningar eller kräver hello användaren tooperform Multi-Factor authentication. Dessa åtgärder kan hindra en angripare från att stulna identitet toocause skador och ge några toosecure hello identiteten.

### <a name="sign-in-risk-security-policy"></a>Logga in risk säkerhetsprincip
Logga in riskprincipen är en princip för villkorlig åtkomst som utvärderar hello risk tooa specifika inloggning och tillämpar åtgärder baserat på fördefinierade villkor och regler.

![Logga in riskprincipen](./media/active-directory-identityprotection/1014.png "inloggning riskprincipen")

Azure AD Identity Protection hjälper dig att hantera hello minskning av riskfyllda inloggningar genom att du kan:

* Ange hello användare och grupper hello-principen gäller för:

    ![Logga in riskprincipen](./media/active-directory-identityprotection/1015.png "inloggning riskprincipen")
* Ange hello inloggning risk nivån tröskelvärde (låg, medel eller hög) som utlöser hello princip:

    ![Logga in riskprincipen](./media/active-directory-identityprotection/1016.png "inloggning riskprincipen")
* Ange hello kontroller toobe tillämpas när hello princip utlöser:  

    ![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")
* Växeln hello tillstånd för principen:

    ![MFA-registrering](./media/active-directory-identityprotection/403.png "MFA-registrering")
* Granska och utvärdera hello resultatet av ändringen innan du aktiverar det:

    ![Logga in riskprincipen](./media/active-directory-identityprotection/1018.png "inloggning riskprincipen")

#### <a name="what-you-need-tooknow"></a>Vad du behöver tooknow
Du kan konfigurera en inloggning risk säkerhet princip toorequire multifaktorautentisering:

![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")

Men av säkerhetsskäl har fungerar den här inställningen bara för användare som redan har registrerats för multifaktorautentisering. Om hello villkoret toorequire multifaktorautentisering uppfylls för en användare som ännu inte har registrerats för multifaktorautentisering är hello användaren blockerad.

Som bästa praxis, om du vill toorequire Multi-Factor authentication för riskfyllda inloggningar, bör du:

1. Aktivera hello [multifaktorautentisering registrering princip](#multi-factor-authentication-registration-policy) för hello påverkade användare.
2. Kräv hello påverkade användare toologin i en icke-riskfyllda session tooperform en MFA-registrering

Gör så här ser du till att multifaktorautentisering krävs för riskfyllda loggar in.

#### <a name="best-practices"></a>Bästa praxis
Om du väljer en **hög** tröskelvärdet minskar hello antalet gånger en princip utlöses och minimerar hello påverkan toousers.  

Men det omfattar inte **låg** och **medel** inloggningar som flaggats för risk från hello-principen, som inte kan medföra att en angripare från att en komprometterad identitet.

När inställningen hello principen

* Undanta användare som inte / kan inte ha multifaktorautentisering
* Undanta användare i där aktiverar hello principen inte är en praktisk språk (till exempel ingen åtkomst toohelpdesk)
* Undanta användare som är sannolikt toogenerate mycket FALSKT positiva (utvecklare, säkerhetsanalytiker)
* Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.
* Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet. Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.

hello rekommenderas standard för de flesta organisationer är tooconfigure en regel för en **medel** tröskelvärdet toostrike balans mellan användbarhet och säkerhet.

hello inloggning riskprincipen är:

* Tillämpade tooall webbläsartrafik och inloggningar som använder modern autentisering.
* Ej tillämpad tooapplications med äldre säkerhetsprotokoll genom att inaktivera hello WS-Trust-slutpunkten på hello federerad IDP som AD FS.

Hej **riskhändelser** i hello identitetsskydd konsolen visar alla händelser:

* Den här principen har tillämpats på
* Du kan granska hello aktiviteten och avgöra om hello åtgärden har lämplig

En översikt över hello relaterade användarupplevelsen, se:

* [Återställning för riskfyllda inloggning](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Riskfyllda inloggning blockeras](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Logga in som inträffar med Azure AD Identity Protection](active-directory-identityprotection-flows.md)  

**tooopen hello relaterade konfigurationsdialogruta**:

- På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **inloggning riskprincipen**.

    ![Användarprincip ridk](./media/active-directory-identityprotection/1014.png "ridk användarprincip")



## <a name="users-flagged-for-risk"></a>Användare som har flaggats för risk

Alla aktiva [riskerar händelser](active-directory-identity-protection-risk-events.md) som identifierades av Azure Active Directory för en användare bidra tooa logiskt koncept kallas användaren risk. En användare som har flaggats för risk är en indikator för ett konto som kan ha drabbats.

![Användare som har flaggats för risk](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Risknivå för användaren

Risknivå för en användare är en indikation (hög, medel eller låg) på hello sannolikheten att hello användarens identitet har komprometterats. Den beräknas baserat på hello användaren riskhändelser som är kopplade till en användarens identitet.

hello status för en risk händelse är antingen **Active** eller **stängd**. Endast riskerar händelser som är **Active** bidra toohello användaren nivå vid beräkningen.

hello användaren risknivå beräknas med hjälp av följande indata hello:

* Aktiva riskhändelser påverka hello användare
* Risknivå för dessa händelser
* Om eventuella åtgärder har utförts

![Användaren risker](./media/active-directory-identityprotection/1031.png "användaren risker")

Du kan använda hello användaren risk nivåer toocreate principer för villkorlig åtkomst som blockera riskfyllda användare från att logga in eller tvinga dem toosecurely ändra sina lösenord.

### <a name="closing-risk-events-manually"></a>Stänger riskhändelser manuellt

I de flesta fall ska vi ta reparationsåtgärder som en säker lösenordsåterställning tooautomatically Stäng riskhändelser. Men kanske det inte alltid är möjligt.  
Detta gäller, till exempel hello när:

* En användare med aktiva riskhändelser har tagits bort
* En undersökningen visar att en händelse rapporteras risk utför av hello legitim användare

Eftersom riskhändelser som är **Active** bidra toohello användaren vid beräkningen kan du ha toomanually lägre en risknivå genom att stänga riskhändelser manuellt.  
Under hello undersökningar, kan du välja tootake någon av dessa åtgärder toochange hello status för en risk händelse:

![Åtgärder](./media/active-directory-identityprotection/34.png "åtgärder")

* **Lös** - om när du undersöker en risk händelse du använde en lämplig Reparationsåtgärd utanför identitetsskydd och du tror att hello risk händelsen ska betraktas stängs, markera hello händelse som löst. Löst händelser anger hello risk händelse status tooClosed och hello risk händelsen kommer inte längre att bidra toouser risk.
* **Markera som falska positiva** – i vissa fall kan du kan undersöka en händelse för risk och identifiera att det var felaktigt flaggas som en riskfyllda. Du kan minska hello antalet sådana händelser genom att markera hello risk händelse som falska positiva. Detta hjälper hello algoritmer tooimprove hello klassificering av liknande händelser i hello framtida för maskininlärning. hello status för falska positiva händelser är för**stängd** och de kommer inte längre att bidra toouser risk.
* **Ignorera** - om du inte har vidtagit några Reparationsåtgärd, men vill hello risk händelse toobe bort från listan över aktiva hello, kan du markera en händelse för risk Ignorera och hello händelsestatus kommer att stängas. Ignorerade händelser bidrar inte toouser risk. Det här alternativet bör endast användas under ovanliga omständigheter.
* **Återaktivera** -riskerar händelser som har stängts manuellt (genom att välja **lösa**, **falsklarm**, eller **Ignorera**) kan återaktiveras, ange hello händelsestatus tillbaka för**Active**. Återaktiverade riskhändelser bidrar toohello användaren nivå vid beräkningen. Riskhändelser stängd genom reparation (t.ex. en säker lösenordsåterställning) kan inte aktiveras igen.

**tooopen hello relaterade konfigurationsdialogruta**:

1. På hello **Azure AD Identity Protection** bladet under **Undersök**, klickar du på **riskerar händelser**.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1002.png "manuell lösenordsåterställning")
2. I hello **riskerar händelser** klickar du på en risk.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1003.png "manuell lösenordsåterställning")
3. Högerklicka på en användare på hello risk bladet.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1004.png "manuell lösenordsåterställning")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Stänger alla riskhändelser för en användare manuellt
I stället för att stänga manuellt riskhändelser för en användare individuellt, ger Azure Active Directory-identitetsskydd dig en metod tooclose alla händelser för en användare med ett enda klick.

![Åtgärder](./media/active-directory-identityprotection/2222.png "åtgärder")

När du klickar på **stänga alla händelser**, alla händelser har stängts och hello påverkade användare inte längre är i fara.

### <a name="remediating-user-risk-events"></a>Hälsostatus riskhändelser för användaren

En är en åtgärd toosecure en identitet eller en enhet som har tidigare eller misstänks toobe komprometteras. En Reparationsåtgärd återställer hello identitet eller enheten tooa säkert tillstånd och löser tidigare riskhändelser som är associerade med hello identitet eller enhet.

tooremediate användaren riskhändelser, kan du:

* Utför en riskhändelser för säkert lösenord Återställ tooremediate användare manuellt
* Konfigurera en användare risk säkerhet princip toomitigate eller reparera riskhändelser användare automatiskt
* Ny avbildning hello infekterade enheter  

#### <a name="manual-secure-password-reset"></a>Manuell säker lösenordsåterställning
En säker lösenordsåterställning är en effektiv åtgärder för många riskhändelser och när utföra automatiskt stängs händelserna risk och beräknar hello risknivå för användaren. Du kan använda hello identitetsskydd instrumentpanelen tooinitiate en återställning av lösenord för en riskfyllda användare.

hello innehåller relaterade två olika metoder tooreset lösenord:

**Återställ lösenord** – Välj **kräver hello användaren tooreset sitt lösenord** tooallow hello användaren tooself återskapa om hello användaren har registrerats för multifaktorautentisering. Under hello användarens nästa inloggning, kommer hello användaren att nödvändiga toosolve multifaktorautentisering utmaning har och sedan, framtvingad toochange hello lösenord. Det här alternativet är inte tillgängligt om hello användarkonto inte är redan registrerad multifaktorautentisering.

**Tillfälligt lösenord** – Välj **generera ett tillfälligt lösenord** tooimmediately ogiltigförklara hello befintliga lösenord och skapa ett nytt tillfälligt lösenord för hello användare. Skicka hello nytt tillfälligt lösenord tooan alternativa e-postadress för hello användare eller toohello användarens chef. Eftersom hello lösenord är tillfällig kommer hello användaren att tillfrågas toochange hello lösenord vid inloggning.

![Principen](./media/active-directory-identityprotection/1005.png "princip")

**tooopen hello relaterade konfigurationsdialogruta**:

1. På hello **Azure AD Identity Protection** bladet, klickar du på **användare som har flaggats för risk**.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1006.png "manuell lösenordsåterställning")
2. Välj en användare med minst en riskhändelser hello listan användare.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1007.png "manuell lösenordsåterställning")
3. På bladet användare hello klickar du på **Återställ lösenord**.

    ![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1008.png "manuell lösenordsåterställning")

### <a name="user-risk-security-policy"></a>Användaren risk säkerhetsprincip
En säkerhetsprincip för användaren risk är en princip för villkorlig åtkomst som utvärderar hello risk nivå tooa specifik användare och tillämpar reparation och minskning åtgärder baserat på fördefinierade villkor och regler.

![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")

Azure AD Identity Protection hjälper dig att hantera hello minskning och reparationen av användare som har flaggats för risken genom att du kan:

* Ange hello användare och grupper hello-principen gäller för:

    ![Användarprincip ridk](./media/active-directory-identityprotection/1010.png "ridk användarprincip")
* Ange hello användaren risk nivån tröskelvärde (låg, medel eller hög) som utlöser hello princip:

    ![Användarprincip ridk](./media/active-directory-identityprotection/1011.png "ridk användarprincip")
* Ange hello kontroller toobe tillämpas när hello princip utlöser:

    ![Användarprincip ridk](./media/active-directory-identityprotection/1012.png "ridk användarprincip")
* Växeln hello tillstånd för principen:

    ![Användarprincip ridk](./media/active-directory-identityprotection/403.png "MFA-registrering")
* Granska och utvärdera hello resultatet av ändringen innan du aktiverar det:

    ![Användarprincip ridk](./media/active-directory-identityprotection/1013.png "ridk användarprincip")

Om du väljer en **hög** tröskelvärdet minskar hello antalet gånger en princip utlöses och minimerar hello påverkan toousers.
Men det omfattar inte **låg** och **medel** användare som har flaggats för risk från hello-principen, som inte kan skydda identiteter eller enheter som har tidigare eller misstänks toobe komprometteras.

När inställningen hello principen

* Undanta användare som är sannolikt toogenerate mycket FALSKT positiva (utvecklare, säkerhetsanalytiker)
* Undanta användare i där aktiverar hello principen inte är en praktisk språk (till exempel ingen åtkomst toohelpdesk)
* Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.
* Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet. Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.

hello rekommenderas standard för de flesta organisationer är tooconfigure en regel för en **medel** tröskelvärdet toostrike balans mellan användbarhet och säkerhet.

En översikt över hello relaterade användarupplevelsen, se:

* [Komprometterat konto recovery flödet](active-directory-identityprotection-flows.md#compromised-account-recovery).  
* [Komprometteras kontot blockerades flödet](active-directory-identityprotection-flows.md#compromised-account-blocked).  

**tooopen hello relaterade konfigurationsdialogruta**:

- På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **risk användarprincip**.

    ![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")

### <a name="mitigating-user-risk-events"></a>Minimera riskhändelser för användaren
Administratörer kan ange en användare risk säkerhet princip tooblock användaren vid inloggning beroende på hello risknivå.

Blockera en inloggning:

* Förhindrar hello generering av nya användare riskhändelser för hello påverkade användare
* Låter administratörer toomanually reparera hello riskhändelser påverkar hello användarens identitet och återställa den tooa säkert tillstånd



## <a name="multi-factor-authentication-registration-policy"></a>Princip för registrering av multifaktorautentisering
Azure Multi-Factor authentication är en metod för att verifiera vem du är som kräver hello att mer än bara ett användarnamn och lösenord. Det ger ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner.  
Vi rekommenderar att du behöver Azure Multi-Factor authentication för användarinloggningar eftersom den:

* Ger stark autentisering med ett antal alternativ för enkel verifiering
* Spelar en viktig roll i att förbereda din organisation tooprotect och återställa från kontot kompromisser

![Användarprincip ridk](./media/active-directory-identityprotection/1019.png "ridk användarprincip")

Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

Azure AD Identity Protection hjälper dig att hantera hello lansering av multifaktorautentisering registrering genom att konfigurera en princip som gör att du kan:

* Ange hello användare och grupper hello-principen gäller för:

    ![Principen för MFA](./media/active-directory-identityprotection/1020.png "MFA-principen")
* Ange hello kontroller toobe tillämpas när hello princip utlöser::  

    ![Principen för MFA](./media/active-directory-identityprotection/1021.png "MFA-principen")
* Växeln hello tillstånd för principen:

    ![Principen för MFA](./media/active-directory-identityprotection/403.png "MFA-principen")
* Visa status för aktuella hello-registrering:

    ![Principen för MFA](./media/active-directory-identityprotection/1022.png "MFA-principen")

En översikt över hello relaterade användarupplevelsen, se:

* [Multifaktorautentisering registrering flödet](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  
* [Logga in som inträffar med Azure AD Identity Protection](active-directory-identityprotection-flows.md).  

**tooopen hello relaterade konfigurationsdialogruta**:

- På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **multifaktorautentisering registrering**.

    ![Principen för MFA](./media/active-directory-identityprotection/1019.png "MFA-principen")

## <a name="next-steps"></a>Nästa steg
* [Channel 9: Azure AD och Identity visa: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Aktivera Identitetsskydd för Azure Active Directory](active-directory-identityprotection-enable.md)

* [Sårbarheter som identifieras av Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md)

* [Azure Active Directory Identity Protection-aviseringar](active-directory-identityprotection-notifications.md)

* [Azure Active Directory-identitetsskydd playbook](active-directory-identityprotection-playbook.md)

* [Azure Active Directory Identity Protection – ordlista](active-directory-identityprotection-glossary.md)

* [Logga in som inträffar med Azure AD Identity Protection](active-directory-identityprotection-flows.md)

* [Azure Active Directory Identity Protection - hur toounblock användare](active-directory-identityprotection-unblock-howto.md)

* [Kom igång med Azure Active Directory Identity Protection och Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
