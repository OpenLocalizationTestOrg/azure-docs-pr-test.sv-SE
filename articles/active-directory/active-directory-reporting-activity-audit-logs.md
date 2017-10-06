---
title: aaaAudit aktivitetsrapporter hello Azure Active Directory-portalen | Microsoft Docs
description: Introduktion toohello granska aktivitetsrapporter hello Azure Active Directory-portalen
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a>Granska aktivitetsrapporter hello Azure Active Directory-portalen 

Du kan hämta hello information du behöver toodetermine hur din miljö fungerar med rapportering i Azure Active Directory (AD Azure).

Hej Rapporteringsarkitektur i Azure AD består av hello följande komponenter:

- **Aktivitet** 
    - **Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter
    - **Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter.
- **Säkerhet** 
    - **Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto. Mer information finns i avsnittet om riskfyllda inloggningar.
    - **Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats. Mer information finns i avsnittet om användare som har flaggats för risk.

Det här avsnittet ger en översikt över hello audit aktiviteter.
 
## <a name="who-can-access-hello-data"></a>Vem som kan komma åt hello data?
* Användare med hello säkerhet Admin eller säkerhet Reader rollen
* Globala administratörer
* Enskilda användare (icke-administratörer) kan se sina egna aktiviteter


## <a name="audit-logs"></a>Granskningsloggar

hello granskningsloggar i Azure Active Directory innehåller poster för systemaktiviteter för kompatibilitet.  
Din första posten punkt tooall granska data är **granskningsloggar** i hello **aktiviteten** avsnitt i **Azure Active Directory**.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/61.png "Granskningsloggar")

En granskningslogg har en standardlistvy som visar:

- hello datum och tid för hello förekomst
- Hej initieraren / aktören (*som*) för en aktivitet 
- Hej aktivitet (*vad*) 
- hello mål

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/18.png "Granskningsloggar")

Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/19.png "Granskningsloggar")

Detta gör att du toodisplay ytterligare fält eller ta bort fält som redan visas.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/21.png "Granskningsloggar")


Genom att klicka på ett objekt i listvyn hello, hämta alla information om den.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/22.png "Granskningsloggar")


## <a name="filtering-audit-logs"></a>Filtrera granskningsloggar

toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello granskningsdata med hello följande fält:

- Datumintervall
- Initierad av (aktör)
- Kategori
- Resurstyp för aktivitet
- Aktivitet

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/23.png "Granskningsloggar")


Hej **intervallet** filter aktiverar tooyou toodefine en tidsram för hello returnerade data.  
Möjliga värden:

- 1 månad
- 7 dagar
- 24 timmar
- Anpassat

När du väljer en anpassad tidsram kan du konfigurera en starttid och en sluttid.

Hej **initieras av** filter kan du toodefine skådespelare, namn eller dess universal huvudnamn (UPN).

Hej **kategori** filter kan du tooselect något av följande filter hello:

- Alla
- Grundläggande kategori
- Grundläggande katalog
- Lösenordshantering via självbetjäning
- Självbetjäning, grupphantering
- Kontoetablering – Automatiserad förnyelse av lösenord
- Inbjudna användare
- MIM-tjänst
- Identity Protection
- B2C

Hej **aktivitet resurstypen** filter kan du tooselect en av följande hello filtrerar:

- Alla 
- Grupp
- Katalog
- Användare
- Program
- Princip
- Enhet
- Annat

När du väljer **grupp** som **aktivitet resurstypen**, får du en ytterligare filterkategori som du kan använda tooalso ger en **källa**:

- Azure AD
- O365


Hej **aktiviteten** filter baseras på hello kategori och aktivitet resurs typ du väljer. Du kan välja en specifik aktivitet som du vill toosee eller Välj alla. 

Du får hello lista över alla gransknings-och aktiviteter med hjälp av hello Graph API https://graph.windows.net/$ tenantdomain/aktiviteter/auditActivityTypes? api-version = beta, där $tenantdomain = ditt domännamn eller referera toohello artikel [granska rapporten händelser](active-directory-reporting-audit-events.md).


## <a name="audit-logs-shortcuts"></a>Genvägar till granskningsloggar

Dessutom för**Azure Active Directory**, hello Azure-portalen ger dig två ytterligare startpunkter tooaudit data:

- Användare och grupper
- Företagsprogram

### <a name="users-and-groups-audit-logs"></a>Granskningsloggar för användare och grupper

Du kan få svar tooquestions som med användar- och gruppbaserade granskningsrapporter:

- Vilka typer av uppdateringar har tillämpade hello användare?

- Hur många användare ändrades?

- Hur många lösenord ändrades?

- Vad har en administratör gjort i en katalog?

- Vad är hello-grupper som har lagts till?

- Finns det grupper med ändringar i medlemskap?

- Har ändrats hello ägare av gruppen?

- Vilka licenser har tilldelats tooa grupp eller en användare?

Om du bara vill tooreview granska data som är relaterade toousers och grupper du kan hitta en filtrerad vy under **granskningsloggar** i hello **aktiviteten** avsnitt i hello **användare och grupper**. I det här området är **Användare och grupper** den förvalda **aktivitetsresurstypen**.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/93.png "Granskningsloggar")

### <a name="enterprise-applications-audit-logs"></a>Granskningsloggar för företagsprogram

Granska rapporter med programbaserade, du får svar tooquestions som:

* Vad är hello-program som har lagts till eller uppdaterats?
* Vad är hello-program som har tagits bort?
* Har tjänstprincipen för ett program ändrats?
* Har ändrats hello namnen på program?
* Vem som gav medgivande tooan program?

Om du bara vill tooreview granska data som är relaterade tooyour program kan du hitta en filtrerad vy under **granskningsloggar** i hello **aktiviteten** avsnitt i hello **företagsprogram**  bladet. I det här området är **Företagsprogram** den förvalda **aktivitetsresurstypen**.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/134.png "Granskningsloggar")

Du kan filtrera den här vyn ytterligare ned toojust **grupper** eller bara **användare**.

![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/25.png "Granskningsloggar")


## <a name="next-steps"></a>Nästa steg

En översikt över rapportering finns i hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).

