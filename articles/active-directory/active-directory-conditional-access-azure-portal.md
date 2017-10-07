---
title: "aaaAzure Active Directory för villkorlig åtkomst | Microsoft Docs"
description: "Använda villkorlig åtkomstkontroll i Azure Active Directory toocheck för specifika förhållanden vid autentisering för åtkomst tooapplications."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 9fa8a5c3e514c032fbe3aa56f33d759485a018c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Villkorlig åtkomst i Azure Active Directory

I en mobile första, molnet först värld kan Azure Active Directory enkel inloggning toodevices, appar och tjänster från var som helst. Med hello ökande mängden av enheter (inklusive BYOD), fungerar av företagets nätverk och 3 part SaaS-appar, IT-proffs står inför två andra mål:

- Ge hello slutanvändare toobe produktiva var och när
- Skydda hello företagets tillgångar när som helst

tooimprove produktivitet, Azure Active Directory ger användarna en mängd olika alternativ tooaccess din företagets tillgångar. Med programhantering åtkomst, Azure Active Directory kan du tooensure som bara *hello rätt personer* kan komma åt dina program. Vad händer om du vill toohave mer kontroll över hur hello rätt personer använder dina resurser under vissa förhållanden? Vad händer om du även har villkor som du vill använda tooblock åtkomst toocertain appar även för hello *Högerklicka personer*? Exempelvis kanske OK du om hello rätt personer kommer åt vissa appar från ett betrott nätverk. Däremot kanske du inte vill dem tooaccess dessa appar från ett nätverk som du inte litar på. Du kan åtgärda dessa frågor med villkorlig åtkomst.

Villkorlig åtkomst är en funktion av Azure Active Directory som gör att du tooenforce kontroller i hello åtkomst tooapps i din miljö baserat på specifika villkor. Du kan antingen koppla ytterligare krav toohello åtkomst med kontroller, eller du kan blockera den. hello implementering av villkorlig åtkomst baseras på principer. Principbaserad inställning förenklar konfigurationen upplevelsen eftersom hello sätt du tycker om åtkomstkraven följs.  

Normalt kan du definiera åtkomstkraven med hjälp av rapporter som baseras på hello följande mönster:

![Kontrollen](./media/active-directory-conditional-access-azure-portal/10.png)

När du ersätter hello två förekomster av ”*detta*” verkliga information du har ett exempel på en kommentar som förmodligen bekant tooyou:

*När leverantörer försöker tooaccess våra molnappar från nätverk som inte är betrodda, blockera åtkomst.*

hello Principframställning ovan visar hello power av villkorlig åtkomst. Medan du kan aktivera leverantörer toobasically åtkomst till dina molnappar (**som**), med villkorlig åtkomst kan du också definiera villkor under vilka hello åtkomst är möjligt (**hur**).

Hello tillsammans med Azure Active Directory villkorlig åtkomst

- ”**När detta sker**” kallas **condition-instruktion**
- ”**Gör du så här**” kallas **kontroller**

![Kontrollen](./media/active-directory-conditional-access-azure-portal/11.png)

hello kombination av en condition-instruktion med kontrollerna representerar en princip för villkorlig åtkomst.

![Kontrollen](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Kontroller

I en princip för villkorlig åtkomst definierar kontroller vad det är som ska hända när en villkorssatsen har uppfyllts.  
Med kontroller kan du blockera åtkomst eller tillåter åtkomst med ytterligare krav.
När du konfigurerar en princip som tillåter åtkomst, måste tooselect minst ett krav.   

### <a name="grant-controls"></a>Bevilja kontroller
hello aktuella implementationen av Azure Active Directory kan du tooconfigure hello följande bevilja kontrollen krav:

![Kontrollen](./media/active-directory-conditional-access-azure-portal/05.png)

- **Multifaktorautentisering** -du kan kräva stark autentisering via multifaktorautentisering. Du kan använda Azure Multi-Factor eller en lokal flerfunktionsautentiseringsleverantör, tillsammans med Active Directory Federation Services (AD FS) som leverantör. Använda Multi-Factor authentication skyddar resurser från används av en obehörig användare som kan ha fick tillgång toohello autentiseringsuppgifterna för en giltig användare.

- **Kompatibel enhet** -du kan ange principer för villkorlig åtkomst på enhetsnivå hello. Du kan ställa in en princip tooonly aktivera datorer som är kompatibla eller mobila enheter som är registrerade i en tooaccess för hantering av mobila enheter av din organisations resurser. Du kan till exempel använda Intune toocheck enheternas kompatibilitet och rapportera det tooAzure AD för tvingande när hello användare försöker tooaccess ett program. Detaljerad vägledning om hur toouse Intune tooprotect appar och data, se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune). Du kan också använda Intune tooenforce dataskydd för borttappade eller stulna enheter. Mer information finns i [skydda dina data med fullständig eller selektiv rensning med Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

- **Domänansluten enhet** – du kan kräva hello-enhet som du har använt tooconnect tooAzure Active Directory toobe domänanslutna tooyour lokala Active Directory (AD). Den här principen gäller tooWindows arbetsstationer, bärbara datorer och surfplattor enterprise. 

Om du har markerat flera kontroller kan konfigurera du också om alla krävs när principen har bearbetats.

![Kontrollen](./media/active-directory-conditional-access-azure-portal/06.png)

### <a name="session-controls"></a>Sessionen kontroller
Sessionen kontroller aktivera begränsande användarupplevelse inom en molnappen. hello session kontroller tillämpas av molnappar och förlitar sig på ytterligare information som tillhandahålls av Azure AD toohello app om hello session.

![Kontrollen](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### <a name="use-app-enforced-restrictions"></a>Använd appbegränsningar tillämpas
Du kan använda den här kontrollen toorequire Azure AD toopass hello enheten information toohello molnappen. Detta hjälper hello molnappen vet om hello användare kommer från en kompatibel enhet eller en domänansluten enhet. Den här kontrollen är för närvarande stöds endast med SharePoint som hello molnappen. SharePoint använder hello enhetsanvändare information tooprovide en begränsad eller fullständig upplevelse beroende på hello enhetens tillstånd.
toolearn mer information om hur toorequire begränsad åtkomst med SharePoint, gå [här](https://aka.ms/spolimitedaccessdocs).

## <a name="condition-statement"></a>Villkorssatsen

hello föregående avsnitt har införts toosupported alternativ tooblock eller begränsa åtkomst tooyour resurser i form av kontroller. I en princip för villkorlig åtkomst definierar du hello kriterier som måste uppfyllas toobe för din toobe för kontroller som används i form av en condition-instruktion.  

Du kan inkludera följande tilldelningar i din villkorssatsen hello:

![Kontrollen](./media/active-directory-conditional-access-azure-portal/07.png)


- **Som** -i många fall du vill att din kontroller toobe tillämpas tooa specifik uppsättning användare. I en villkorssatsen kan du definiera detta genom att välja hello användare och grupper som principen gäller för. Om det behövs kan du även uttryckligen undanta en uppsättning användare från din princip genom att undanta dem.  
Genom att välja användare och grupper kan definiera du hello omfånget för användare som principen gäller för.    

    ![Kontrollen](./media/active-directory-conditional-access-azure-portal/08.png)



- **Vad** -det finns normalt sett vissa appar som körs i din miljö som kräver mer uppmärksamhet än andra ur skydd. Detta påverkar t.ex, appar som har åtkomst till toosensitive data.
Genom att välja molnappar kan definiera du hello omfattning molnappar som principen gäller för. Om det behövs kan du också explicit utesluta en uppsättning appar från principen.

    ![Kontrollen](./media/active-directory-conditional-access-azure-portal/09.png)


- **Hur** – så länge åtkomst tooyour appar utförs enligt villkor som du kan kontrollera det kan inte finnas något behov för att införa ytterligare kontroller på hur dina molnappar används av användarna. Saker kan dock se annorlunda ut om åtkomst tooyour molnappar utförs, till exempel från nätverk som inte är betrodda eller enheter som inte är kompatibla. Du kan definiera vissa villkor som har ytterligare krav för hur åtkomst tooyour appar utförs i en condition-instruktion.

    ![Villkor](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Villkor

I hello aktuella implementationen av Azure Active Directory, kan du definiera villkor för hello följande områden:

- Logga in risk
- Enhetsplattformar
- Platser
- Klientprogram

![Villkor](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Logga in risk

En risk för inloggning är ett objekt som används av Azure Active Directory tootrack hello sannolikheten att ett försök att logga in inte har utförts av hello legitima ägaren till ett användarkonto. I det här objektet hello sannolikheten (hög, medel eller låg) lagras i form av ett attribut som kallas [inloggning risknivå](active-directory-reporting-risk-events.md#risk-level). Det här objektet skapas under en inloggning för en användare logga in risker har upptäckts av Azure Active Directory. Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins).  
Du kan använda hello beräknade inloggning risknivå som villkor i en princip för villkorlig åtkomst. 

![Villkor](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Enhetsplattformar

Hej enhetsplattform kännetecknas av hello-operativsystemet som körs på din enhet:

- Android
- iOS
- Windows Phone
- Windows
- macOS (förhandsversion). 

![Villkor](./media/active-directory-conditional-access-azure-portal/02.png)

Du kan definiera hello enhetsplattformar som ingår samt plattformar för enheter som är undantagna från en princip.  
toouse enhetsplattformar i hello princip, första ändra hello konfigurera växlar för**Ja**, och markera alla eller enskild enhet plattformar hello principen gäller för. Om du väljer enskilda enhetsplattformar påverkar hello princip endast på dessa plattformar. I det här fallet påverkas inloggningar tooother stöds plattformar inte av hello principen.


### <a name="locations"></a>Platser

hello plats identifieras av hello hello klientens IP-adress som du har använt tooconnect tooAzure Active Directory. Det här villkoret kräver toobe bekant med **med namnet platser** och **MFA tillförlitliga IP-adresser**.  

**Med namnet platser** är en funktion i Azure Active Directory som gör att du toolabel tillförlitliga IP-adressintervall i ditt företag. I din miljö kan du använda sökvägarna i hello kontexten hello upptäcka [riskerar händelser](active-directory-reporting-risk-events.md) samt villkorlig åtkomst. Mer information om hur du konfigurerar med namnet platser i Azure Active Directory finns [med namnet platser i Azure Active Directory](active-directory-named-locations.md).

hello antalet platser som du kan konfigurera är begränsad av hello storleken på hello relaterade objekt i Azure AD. Du kan konfigurera:
 
 - En namngiven plats med av too500 IP-intervall
 - Högst 60 sökvägarna (förhandsversion) med en IP-adressintervall tilldelade tooeach av dem. 


**MFA tillförlitliga IP-adresser** är en funktion av Multi-Factor authentication som gör att du toodefine tillförlitliga IP-adressintervall som motsvarar din organisations lokalt intranät. När du konfigurerar en plats-villkor, tillförlitliga IP-adresser som gör att du toodistinguish mellan anslutningar till nätverket och alla andra platser. Mer information finns i [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Du kan antingen ta med alla platser eller alla betrodda IP-adresser och du kan utesluta alla betrodda IP-adresser.

![Villkor](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-app"></a>-Klientappen

hello-klientappen kan vara antingen i en generisk nivån hello-app (webbläsare, mobila appar, klientversionen) du har använt tooconnect tooAzure Active Directory eller mer specifikt kan du välja Exchange Active Sync.  
Äldre autentisering refererar tooclients som använder grundläggande autentisering, till exempel äldre Office-klienter som inte använder modern autentisering. Villkorlig åtkomst stöds för närvarande inte med äldre autentisering.

![Villkor](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="common-scenarios"></a>Vanliga scenarier

### <a name="requiring-multi-factor-authentication-for-apps"></a>Att kräva multifaktorautentisering för appar

Många miljöer har appar som kräver en högre nivå av skydd än hello andra.
Detta är till exempel hello fallet för appar som har åtkomst till toosensitive data.
Om du vill tooadd ett extra lager av skydd toothese appar kan konfigurera du en princip för villkorlig åtkomst som kräver multifaktorautentisering när användarna kommer åt dessa appar.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Kräver multifaktorautentisering för åtkomst från nätverk som inte är betrodda

Det här scenariot är liknande toohello föregående scenario eftersom den lägger till ett krav för multifaktorautentisering.
Hello största skillnaden är dock hello villkor för det här kravet.  
Medan hello fokus på hello föregående scenario var i appar med åtkomst till toosensitve data, fokuserar hello i det här scenariot på betrodda platser.  
Du kan med andra ord ha ett krav för multifaktorautentisering om en app öppnas av en användare från ett nätverk som du inte litar på.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Endast betrodda enheter kan komma åt Office 365-tjänster

Om du använder Intune i din miljö kan du direkt börja använda hello villkorlig åtkomst för gränssnittet i hello Azure-konsolen.

Många Intune-kunder använder villkorlig åtkomst tooensure att endast betrodda enheter kan komma åt Office 365-tjänster. Detta innebär att mobila enheter har registrerats med Intune och uppfyller krav på efterlevnad och att Windows-datorer är anslutna tooan lokal domän. En viktiga förbättringar är att du inte har tooset hello samma princip för varje hello Office 365-tjänster.  När du skapar en ny princip, konfigurera hello molnet appar tooinclude varje hello O365-appar som du vill tooprotect med med villkorlig åtkomst.

## <a name="next-steps"></a>Nästa steg

Om du vill tooknow hur tooconfigure en princip för villkorlig åtkomst, se [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Om du är klar tooconfigure principer för villkorlig åtkomst för din miljö finns hello [bästa praxis för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-best-practices.md). 
