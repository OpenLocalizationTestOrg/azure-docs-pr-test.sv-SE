---
title: aaaSign i aktivitetsrapporter hello Azure Active Directory-portalen | Microsoft Docs
description: Introduktion toosign i aktivitetsrapporter hello Azure Active Directory-portalen
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a>Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen

Med Azure Active Directory (AD Azure) rapportering i hello [Azure-portalen](https://portal.azure.com), kan du få hello information du behöver toodetermine hur din miljö gör.

hello-arkitekturen i Azure Active Directory reporting består av hello följande komponenter:

- **Aktivitet** 
    - **Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter
    - **Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter.
- **Säkerhet** 
    - **Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto. Mer information finns i avsnittet om riskfyllda inloggningar.
    - **Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats. Mer information finns i avsnittet om användare som har flaggats för risk.

Det här avsnittet ger en översikt över hello inloggning aktiviteter.

## <a name="pre-requisite"></a>Förhandskrav

### <a name="who-can-access-hello-data"></a>Vem som kan komma åt hello data?
* Användare med hello säkerhet Admin eller säkerhet Reader rollen
* Globala administratörer
* Alla användare (icke-administratörer) kan komma åt sina egna inloggningar 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a>Vilka Azure AD-licens behöver du tooaccess inloggningsaktivitet?
* Din klient måste ha en Azure AD Premium-licens som är associerade med den toosee hello alla upp inloggningsaktivitet rapport


## <a name="signs-in-activities"></a>Inloggningsaktiviteter

Hello information som tillhandahålls av hello användaren logga in rapporten, kan du hitta svar tooquestions som:

* Vad är hello inloggning mönstret för en användare?
* Hur många användare har en användare loggat in under en vecka?
* Vad är hello statusen för dessa inloggningar?

Dina första posten punkt tooall inloggning aktiviteter data är **inloggningar** under hello aktivitet i **Azure Active**.


![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/61.png "Inloggningsaktivitet")


En granskningslogg har en standardlistvy som visar:

- hello relaterade användare
- hello hello programanvändare har inloggad till
- Hej inloggningsstatusen
- hello inloggning tid

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/41.png "Inloggningsaktivitet")

Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/19.png "Inloggningsaktivitet")

Detta gör att du toodisplay ytterligare fält eller ta bort fält som redan visas.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/42.png "Inloggningsaktivitet")

Genom att klicka på ett objekt i listvyn hello, hämta alla information om den.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/43.png "Inloggningsaktivitet")


## <a name="filtering-sign-in-activities"></a>Filtrerar inloggningsaktiviteter

toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello inloggningar data med hjälp av hello följande fält:

- Tidsintervall
- Användare
- Program
- Client
- Inloggningsstatus

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/44.png "Inloggningsaktivitet")


Hej **tidsintervall** filter aktiverar tooyou toodefine en tidsram för hello returnerade data.  
Möjliga värden:

- 1 månad
- 7 dagar
- 24 timmar
- Anpassat

När du väljer en anpassad tidsram kan du konfigurera en starttid och en sluttid.

Hej **användaren** filter kan du toospecify hello namn eller hello användarens huvudnamn (UPN) för hello användare som intresserar dig.

Hej **programmet** filter kan du toospecify hello namnet på hello-program som intresserar dig.

Hej **klienten** filter kan du toospecify information om hello-enhet som intresserar dig.

Hej **inloggningsstatusen** filter kan du tooselect något av följande filter hello:

- Alla
- Lyckades
- Fel


## <a name="sign-in-activities-shortcuts"></a>Genvägar till inloggningsaktiviteter

Dessutom tooAzure Active Directory hello Azure-portalen ger dig två ytterligare posten pekar toosign i aktiviteter data:

- Användare och grupper
- Företagsprogram


### <a name="users-and-groups-sign-ins-activities"></a>Inloggningsaktivitet för användare och grupper

Hello information som tillhandahålls av hello användaren logga in rapporten, kan du hitta svar tooquestions som:

- Vad är hello inloggning mönstret för en användare?
- Hur många användare har en användare loggat in under en vecka?
- Vad är hello statusen för dessa inloggningar?



Punkt toothis transaktionsdata är hello användaren logga in diagrammet i hello **översikt** avsnittet **användare och grupper**.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/45.png "Inloggningsaktivitet")

hello användaren logga in diagrammet visar veckovisa aggregeringar för logga moduler för alla användare i en viss tidsperiod. hello standardvärdet för hello är tidsperiod 30 dagar.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/46.png "Inloggningsaktivitet")

När du klickar på en dag i hello inloggning graph få en detaljerad lista över hello inloggning aktiviteter för den aktuella dagen.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/41.png "Inloggningsaktivitet")

Varje rad i hello inloggning aktiviteter lista ger du hello detaljerad information om hello valt logga in som:

* Vem har loggat in?
* Vad hette hello relaterade UPN?
* Vilka program har hello målet för inloggning hello?
* Vad är hello IP-adressen för inloggning hello?
* Vad hette hello status för inloggning hello?

Hej **inloggningar** alternativet ger en fullständig överblick över alla användarinloggningar.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/51.png "Inloggningsaktivitet")



## <a name="usage-of-managed-applications"></a>Användning av hanterade program

Med en programcentrerad vy över dina inloggningsuppgifter kan du få svar på frågor som:

* Vem använder mina program?
* Vad är hello översta 3-program i din organisation?
* Jag har nyligen distribuerat ett program. Hur går det för det?

Punkt toothis transaktionsdata är hello översta 3-program i din organisation i hello senaste 30 dagarna rapporten i hello **översikt** avsnittet **företagsprogram**.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/64.png "Inloggningsaktivitet")

hello app användning diagrammet veckovisa aggregeringar för inloggningar för översta 3-program i en viss tidsperiod. hello standardvärdet för hello är tidsperiod 30 dagar.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/47.png "Inloggningsaktivitet")

Om du vill kan ange du hello fokus på ett visst program.


![Rapportering](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Rapportering")

När du klickar på en dag i hello app Användningsdiagram få en detaljerad lista över hello inloggning aktiviteter.


![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/48.png "Inloggningsaktivitet")


Hej **inloggningar** alternativet ger en fullständig överblick över alla inloggning händelser tooyour program.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/49.png "Inloggningsaktivitet")



## <a name="next-steps"></a>Nästa steg

Om du vill tooknow mer om inloggningsaktivitet felkoder finns hello [inloggning aktivitet rapporten felkoder i hello Azure Active Directory-portalen](active-directory-reporting-activity-sign-ins-errors.md).

