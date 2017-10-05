---
title: "Få insikter: Lösenordshantering i Azure AD-rapporter | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder rapporter för att få bättre överblick på hanteringsåtgärder för lösenord i din organisation."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 1472b51d-53f4-4b0f-b1be-57f6fa88fa65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: ae83df618e3c392fe89878bcd1be0d6c6cb1edb4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-get-operational-insights-with-password-management-reports"></a>Rapporter om hur du erhåller verksamhetsinsikter med lösenordshantering
> [!IMPORTANT]
> **Är du här eftersom du har problem med att logga in?** I så fall är det [här som du ser hur du kan ändra och återställa ditt eget lösenord](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
>
>

Det här avsnittet beskrivs hur du kan använda rapporter för Azure Active Directory-lösenord för att visa hur användare använder återställning av lösenord och ändra i din organisation.

* [**Översikt över lösenord management-rapporter**](#overview-of-password-management-reports)
* [**Så här visar du lösenordshantering rapporter i den nya Azure-portalen**](#how-to-view-password-management-reports)
 * [Du kan läsa rapporter i katalogen](#directory-roles-allowed-to-read-reports)
* [**Självbetjäning lösenordshantering för aktivitetstyper i den nya Azure-portalen**](#self-service-password-management-activity-types)
 * [Blockeras från lösenordsåterställning via självbetjäning](#activity-type-blocked-from-self-service-password-reset)
 * [Ändra lösenord (Self Service)](#activity-type-change-password-self-service)
 * [Återställ lösenord (som admin)](#activity-type-reset-password-by-admin)
 * [Återställa lösenord (Self Service)](#activity-type-reset-password-self-service)
 * [Self fungera lösenordsåterställning flödet aktivitet pågår](#activity-type-self-serve-password-reset-flow-activity-progress)
 * [Låsa upp användarkonto (Self Service)](#activity-type-unlock-user-account-self-service)
 * [Användaren har registrerats för lösenordsåterställning via självbetjäning](#activity-type-user-registered-for-self-service-password-reset)
* [**Hur du hämtar lösenord management händelser från Azure AD-rapporter och händelser API**](#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
 * [Reporting API begränsningar för hämtning av data](#reporting-api-data-retrieval-limitations)
* [**Hämta lösenordet för återställning registrering händelser snabbt med PowerShell**](#how-to-download-password-reset-registration-events-quickly-with-powershell)
* [**Så här visar du lösenordshantering rapporter i den klassiska portalen**](#how-to-view-password-management-reports-in-the-classic-portal)
* [**Visa registrering aktivitet i din organisation i den klassiska portalen för lösenordsåterställning**](#view-password-reset-registration-activity-in-the-classic-portal)
* [**Visa aktivitet i din organisation i den klassiska portalen för lösenordsåterställning**](#view-password-reset-activity-in-the-classic-portal)


## <a name="overview-of-password-management-reports"></a>Översikt över rapporter för lösenord
När du distribuerar lösenordsåterställning är något av de vanligaste nästa steg att se hur den används i din organisation.  Du kanske exempelvis vill få inblick i hur du registrerar användare för lösenordsåterställning eller hur många lösenord återställning har utförts i de senaste dagarna.  Här följer några vanliga frågor som du kommer att kunna svara med lösenord management rapporterna som finns i den [Azure-hanteringsportalen](https://manage.windowsazure.com) idag:

* Hur många som har registrerats för återställning av lösenord?
* Vem som har registrerats för återställning av lösenord?
* Vilka data personer registrerar?
* Hur många personer återställa sina lösenord under de senaste 7 dagarna?
* Vilka är de vanligaste metoder användarna eller administratörer använder för att återställa sina lösenord?
* Vad är vanliga problem med användare eller administratörer står inför när de försöker använda lösenordsåterställning?
* Vilka administratörer ofta återställa sina lösenord?
* Finns det några misstänkt aktivitet fortsätter med återställning av lösenord?

## <a name="how-to-view-password-management-reports"></a>Visa rapporter för lösenord
I den nya [Azure Portal](https://portal.azure.com) upplevelse, har vi ett bättre sätt att visa lösenordsåterställning och lösenordsåterställningsaktivitet för registrering.  Följ stegen nedan för att hitta lösenordsåterställningen och händelser för registrering för lösenordsåterställning i den nya [Azure Portal](https://portal.azure.com):

1. Gå till [ **portal.azure.com**](https://portal.azure.com)
2. Klicka på den **fler tjänster** menyn på det huvudsakliga Azure Portal vänstra navigeringsfönstret
3. Sök efter **Azure Active Directory** i listan över tjänster och markera den
4. Klicka på **användare och grupper** i Azure Active Directory-navigationsmenyn
5. Klicka på den **granskningsloggar** navigeringsobjektet från navigeringsmenyn användare och grupper. Detta visar alla audit händelser utförs mot alla användare i din katalog. Du kan filtrera den här vyn om du vill se alla lösenord-relaterade händelser, samt.
6. Om du vill filtrera vyn till endast lösenordshanteringen relaterade händelser, klickar du på den **Filter** längst upp på bladet.
7. Från den **Filter** väljer du den **kategori** listrutan och ändra den till den **Self-service lösenordshantering** kategorityp.
8. Du kan också ytterligare filter i listan genom att välja specifikt **aktiviteten** du är intresserad av
### <a name="direct-link-to-user-audit-blade"></a>Direktlänk till användaren Audit bladet
Om du har loggat in till din portal är här en direktlänk till bladet användare audit där du kan se dessa händelser:

* [Gå till användaren audit hanteringsvyn direkt](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit)

### <a name="directory-roles-allowed-to-read-reports"></a>Du kan läsa rapporter i katalogen
För närvarande kan följande directory roller läsa lösenordshantering i Azure AD-rapporter i den klassiska Azure-portalen:

* Global administratör

Innan du kan läsa dessa rapporter, en global administratör i företaget måste ha deltar i för dessa data hämtas åt organisationen genom att besöka reporting fliken eller granskningsloggar minst en gång. Tills gör det, kommer inte samlas in för din organisation.

Du kan läsa mer om directory roller och vad de kan göra, se [Tilldela administratörsroller i Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles).

## <a name="self-service-password-management-activity-types"></a>Självbetjäning lösenordshantering aktivitetstyper
Följande aktivitetstyper som visas i den **Self-Service lösenordshantering** audit händelsekategori.  En beskrivning för var och en av dessa sätt.

* [**Blockeras från lösenordsåterställning via självbetjäning** ](#activity-type-blocked-from-self-service-password-reset) -anger att en användare försökte återställa ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* [**Ändra lösenord (Self Service)** ](#activity-type-change-password-self-service) -anger användaren utföra en frivillig eller framtvingad (på grund av utgången) Ändra lösenordet.
* [**Återställ lösenord (som admin)** ](#activity-type-reset-password-by-admin) -anger att en administratör utföra en lösenordsåterställning för en användare från Azure Portal.
* [**Återställ lösenord (Self Service)** ](#activity-type-reset-password-self-service) -anger att en användare har återställts sitt lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* [**Self fungera lösenordsåterställning flödet aktivitet pågår** ](#activity-type-self-serve-password-reset-flow-activity-progress) -indikerar varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av lösenordet att återställa lösenordet.
* [**Låsa upp användarkonto (Self Service)** ](#activity-type-unlock-user-account-self-service) -anger att en användare har låsts upp sitt konto i Active Directory utan att återställa sitt lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med den [AD kontoupplåsning utan att återställa](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) funktion.
* [**Användare som har registrerats för lösenordsåterställning via självbetjäning** ](#activity-type-user-registered-for-self-service-password-reset) -anger en användare har registrerat all nödvändig information för att kunna återställa hans eller hennes i enlighet med lösenordet som för närvarande anges klient princip för lösenordsåterställning.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Aktivitetstypen: blockeras från lösenordsåterställning via självbetjäning
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare försökte återställa ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* **Aktiviteten aktören** -återställning av den användare som har begränsats från att utföra ytterligare åtgärder. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -återställning av den användare som har begränsats från att utföra ytterligare åtgärder. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger att en användare har begränsats försöka eventuella ytterligare autentiseringsmetoder från att utföra några ytterligare återställer eller verifiera eventuella ytterligare telefonnummer för nästkommande 24 timmar.
* **Aktivitetens Status felorsak** – ej tillämpligt

### <a name="activity-type-change-password-self-service"></a>Aktivitetstypen: ändra lösenord (Self Service)
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare utföra en frivillig eller framtvingad (på grund av utgången) Ändra lösenordet.
* **Aktiviteten aktören** -användaren som ändrade sitt lösenord. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -användaren som ändrade sitt lösenord. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger att en användare har ändrat sitt lösenord
 * _Fel_ -anger att en användare gick inte att ändra sitt lösenord. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade.
* **Aktivitetens Status felorsak** -
 * _FuzzyPolicyViolationInvalidPassword_ -användaren har valt ett lösenord som var förbjuden automatiskt på grund av Microsofts förbjudna lösenord identifieringsfunktionerna hitta dem vara för vanligt eller särskilt svaga.

### <a name="activity-type-reset-password-by-admin"></a>Aktivitetstypen: Återställ lösenord (som admin)
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en administratör utföra en lösenordsåterställning för en användare från Azure Portal.
* **Aktiviteten aktören** -administratören som utförs på uppdrag av en annan slutanvändare eller administratör för återställning av lösenord. Måste vara antingen en global administratör, lösenordsadministratör, Användaradministratör eller supportavdelning administratör.
* **Mål för aktiviteten** -användaren vars lösenord har återställts. Kan vara en slutanvändare eller en annan administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger att en administratör har återställts en användares lösenord
 * _Fel_ -anger att en administratör gick inte att ändra en användares lösenord. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade.

### <a name="activity-type-reset-password-self-service"></a>Aktivitetstypen: Återställ lösenord (Self Service)
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har återställts sitt lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* **Aktiviteten aktören** -användaren som återställa sitt lösenord. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -användaren som återställa sitt lösenord. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger att en användare har återställts sitt lösenord
 * _Fel_ -anger att en användare gick inte att återställa sitt lösenord. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade.
* **Aktivitetens Status felorsak** -
 * _FuzzyPolicyViolationInvalidPassword_ -administratören har valt ett lösenord som var förbjuden automatiskt på grund av Microsofts förbjudna lösenord identifieringsfunktionerna hitta dem vara för vanligt eller särskilt svaga.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Aktivitetstypen: Self hantera lösenord återställa flödet aktivitet pågår
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av lösenordet återställa processen.
* **Aktiviteten aktören** -användaren som utförde en del av lösenordet återställa flödet. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -användaren som utförde en del av lösenordet återställa flödet. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ – visar en användare har slutförts specifikt steg för återställning av lösenord.
 * _Fel_ -anger ett specifikt steg av lösenordet återställa flödet misslyckades. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade.
* **Tillåtna aktivitetens statusorsaker**
 * Se tabellen nedan för [alla tillåtna återställningsaktivitet statusorsaker](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Aktivitetstypen: låsa upp användarkonto (Self Service)
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har låsts upp sitt konto i Active Directory utan att återställa sitt lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med hjälp av den [AD-kontot upplåsning utan återställning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) funktion.
* **Aktiviteten aktören** -användaren som låsas upp sitt konto utan att återställa sina lösenord. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -användaren som låsas upp sitt konto utan att återställa sina lösenord. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger att en användare har låsts upp sitt eget konto
 * _Fel_ -anger att en användare kunde inte låsa upp sitt konto. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Aktivitetstypen: användare som har registrerats för lösenordsåterställning via självbetjäning
I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har registrerat all nödvändig information för att kunna återställa hans eller hennes i enlighet med lösenordet som för närvarande anges klient princip för lösenordsåterställning.
* **Aktiviteten aktören** -användaren som registrerad för återställning av lösenord. Kan vara en slutanvändare eller administratör.
* **Mål för aktiviteten** -användaren som registrerad för återställning av lösenord. Kan vara en slutanvändare eller administratör.
* **Tillåtna aktivitetens status**
 * _Lyckade_ -anger en användare som har registrerats för lösenordsåterställning i enlighet med den aktuella principen.
 * _Fel_ -anger att en användare kunde inte registreras för återställning av lösenord. Klicka på raden gör att du kan se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför de inträffade. Observera - detta inte innebär att en användare inte som kan återställa sina eller lösenord, precis som han eller hon gick inte att slutföra registreringen. Om det finns overifierade data på sitt konto som stämmer (till exempel ett telefonnummer som inte har verifierats), även om de inte har kontrollerat telefonnumret, de kan fortfarande använda den för att återställa sina lösenord. Mer information finns i [vad som händer när en användare som registrerar?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Hur du hämtar lösenord management händelser från Azure AD-rapporter och händelser API
Från och med augusti 2015 stöder Azure AD-rapporter och händelser API nu hämtar all information i lösenord återställning och lösenord Återställ registrering rapporterna. Du kan hämta enskilda lösenordsåterställning och registrering händelser för integrering med reporting-teknik för din choce för återställning av lösenord med hjälp av den här API.

### <a name="how-to-get-started-with-the-reporting-api"></a>Hur du kommer igång med reporting API
Om du vill komma åt dessa data behöver du skriva en liten app eller ett skript för att hämta från våra servrar. [Lär dig hur du kommer igång med Azure AD Reporting API](active-directory-reporting-api-getting-started.md).

När du har ett fungerande skript ska du därefter vill undersöka lösenord återställning och registrering händelser som du kan hämta för att uppfylla dina scenarier.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Visar en lista över tillgängliga kolumner för händelser för återställning av lösenord
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Visar en lista över tillgängliga kolumner för registrering händelser för återställning av lösenord

### <a name="reporting-api-data-retrieval-limitations"></a>Reporting API begränsningar för hämtning av data
För närvarande Azure AD-rapporter och händelser API hämtar upp till **75 000 enskilda händelser** av den [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) och [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typer som sträcker sig från den **senaste 30 dagarna**.

Om du behöver hämta eller lagra data utöver det här fönstret, föreslår vi beständighet i en extern databas och använder API: et för att fråga går som uppstår. Ett bra tips är att börja hämta dessa data när du startar ditt lösenord återställer registreringen i din organisation, spara den externt och fortsätt sedan till spåra går från den här tidpunkten och framåt.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Hämta lösenordet för återställning registrering händelser snabbt med PowerShell
Förutom att använda Azure AD-rapporter och händelser API direkt, kan du också använda den under PowerShell-skript till den senaste registreringen händelser i din katalog. Detta är användbart om du vill se vem som har registrerats nyligen eller skulle vilja se till att distributionen återställning av lösenord pågår som förväntat.

* [Azure AD SSPR-registrering aktivitet PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

## <a name="how-to-view-password-management-reports-in-the-classic-portal"></a>Så här visar du lösenordshantering rapporter i den klassiska portalen
Följ stegen nedan om du vill hitta lösenordet hantering av rapporter:

1. Klicka på den **Active Directory** tillägget på den [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj din katalog i listan som visas på portalen.
3. Klicka på den **rapporter** fliken.
4. Tittar du under den **aktivitetsloggar** avsnitt.
5. Välj antingen den **lösenordsåterställningsaktivitet** rapporten eller **lösenordsåterställningsaktivitet för registrering** rapporten.

## <a name="view-password-reset-registration-activity-in-the-classic-portal"></a>Visa återställningsaktivitet för lösenord registrering i den klassiska portalen
Lösenord Återställ registrering aktivitetsrapporten visar alla lösenordsåterställning registreringar som har inträffat i din organisation.  En registreringen för lösenordsåterställning visas i den här rapporten för alla användare som har registrerat autentiseringsinformationen på lösenordet återställa portalen för registrering ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)).

* **Maximalt tidsintervall**: 30 dagar
* **Max antal rader**: 75 000
* **Nedladdningsbar**: Ja, via CSV-fil

### <a name="description-of-report-columns"></a>Beskrivning av rapportkolumner
I följande lista beskrivs var och en av rapportkolumner i detalj:

* **Användaren** – användaren som försökte lösenord återställa registrering igen.
* **Rollen** – rollen för användare i katalogen.
* **Datum och tid** – datum och tid för försöket.
* **Data registrerade** – vilka autentiseringsdata som användaren har angett under lösenord återställer registrering.

### <a name="description-of-report-values"></a>Beskrivning av rapporten värden
I följande tabell beskrivs de olika värden som tillåts för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Data som registrerats |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-postadress för att autentisera<p><p>**Arbetstelefon**– Arbetstelefon för användare som används för att autentisera<p>**Mobiltelefon** -användare används mobiltelefon eller telefon för autentisering för autentisering<p>**Säkerhetsfrågor** – användaren används säkerhetsfrågor för att autentisera<p>**En kombination av ovanstående (t.ex. alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder den användare som används för autentisering sin begäran om lösenordsåterställning. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Visa aktiviteten i den klassiska portalen för lösenordsåterställning
Den här rapporten visar alla försök som har inträffat i din organisation för lösenordsåterställning.

* **Maximalt tidsintervall**: 30 dagar
* **Max antal rader**: 75 000
* **Nedladdningsbar**: Ja, via CSV-fil

### <a name="description-of-report-columns"></a>Beskrivning av rapportkolumner
I följande lista beskrivs var och en av rapportkolumner i detalj:

1. **Användaren** – användaren som försökte lösenord återställa åtgärden (baserat på fältet Användar-ID anges när användaren kommer att återställa ett lösenord).
2. **Rollen** – rollen för användare i katalogen.
3. **Datum och tid** – datum och tid för försöket.
4. **Metod används** – vilka autentiseringsmetoder som användes för den här återställa igen.
5. **Resultatet** – slutresultatet av lösenordet återställa igen.
6. **Information om** – information om varför lösenordsåterställning resulterade i det gjorde-värdet.  Även om alla säkerhetsåtgärder som du kan vidta för att lösa ett oväntat fel.

### <a name="description-of-report-values"></a>Beskrivning av rapporten värden
I följande tabell beskrivs de olika värden som tillåts för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Metoder som används |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-postadress för att autentisera<p>**Arbetstelefon** – Arbetstelefon för användare som används för att autentisera<p>**Mobiltelefon** – mobiltelefon för användare som används eller telefon för autentisering för autentisering<p>**Säkerhetsfrågor** – användaren används säkerhetsfrågor för att autentisera<p>**En kombination av ovanstående (t.ex. alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder den användare som används för autentisering sin begäran om lösenordsåterställning. |
| Resultat |**Avbrutna** – användaren startade lösenordsåterställning men sedan stoppas halvvägs via utan att slutföra<p>**Blockerad** – användarens konto hindrades för lösenordsåterställning på grund av försök vill använda sidan för återställning av lösenord eller ett enda lösenord återställa gate för många gånger under en 24-timmarsperiod<p>**Avbröt** – användaren startade lösenordsåterställning men sedan klickat på knappen Avbryt om du vill avbryta en bra via sessionen <p>**Kontakta administratören** – användaren hade problem under sin session som han inte gick att matcha, så att användaren har klickat på länken ”kontaktar du administratören” i stället för att slutföra flödet för återställning av lösenord<p>**Det gick inte** – användaren kunde inte återställa ett lösenord, sannolikt eftersom användaren inte har konfigurerats för att använda funktionen (t.ex. Ingen finns licens, saknas autentisering info, lösenord hanteras lokala men tillbakaskrivning av).<p>**Lyckades** – lösenordsåterställning lyckades. |
| Information |Se tabellen nedan |

### <a name="allowed-values-for-details-column"></a>Tillåtna värden för kolumnen information
Nedan visas i listan över resultattyper som du kan förvänta dig när du använder lösenord återställa aktivitetsrapport:

| Information | Typ av resultat |
| --- | --- |
| Användaren avbrutna när du har slutfört verifieringsalternativ e-post |Avbrutna |
| Användaren avbrutna när du har slutfört mobila SMS-verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har slutfört mobila röst-anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har slutfört office röst anrop-verifieringsalternativ |Avbrutna |
| Användaren avbröts efter Slutför säkerheten frågor alternativet |Avbrutna |
| Användaren avbröts efter att ange sina användar-ID |Avbrutna |
| Användaren avbrutna när du har startat verifieringsalternativ e-post |Avbrutna |
| Användaren avbrutna när du har startat mobila SMS-verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har startat mobila röst-anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har startat verifieringsalternativ för office röst anrop |Avbrutna |
| Användaren avbröts efter startar säkerheten frågor alternativet |Avbrutna |
| Användaren avbrutna innan du väljer ett nytt lösenord |Avbrutna |
| Användaren avbrutna när du väljer ett nytt lösenord |Avbrutna |
| Användaren har angett för många ogiltiga koder för SMS och blockeras för 24 timmar |Blockerad |
| Användaren försökte röst mobiltelefonsverifiering för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren försökte office telefonverifiering röst för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren försökte besvara säkerhetsfrågor för många gånger och har blockerats i 24 timmar |Blockerad |
| Användaren försökte verifiera ett telefonnummer för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren avbröt innan autentiseringsmetoderna som krävs |Avbröts |
| Användaren avbröt innan du skickar ett nytt lösenord |Avbröts |
| Användaren kontakta en administratör när du har försökt verifieringsalternativ e-post |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt mobila SMS-verifieringsalternativ |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt mobila röst-anropet verifieringsalternativ |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt verifieringsalternativ för office röst anrop |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt säkerhetsalternativ för verifiering av frågan |Kontaktade admin |
| Återställning av lösenord har inte aktiverats för den här användaren. Aktivera lösenordsåterställning under fliken Konfigurera för att lösa problemet |Det gick inte |
| Användaren har inte en licens. Du kan lägga till en licens till användaren för att lösa problemet |Det gick inte |
| Användaren försökte återställa från en enhet utan att cookies har aktiverats |Det gick inte |
| Användarkontot har inte tillräckligt autentiseringsmetoder som definierats. Lägg till autentisering-information för att lösa problemet |Det gick inte |
| Användarens lösenord är hanteras lokalt. Du kan aktivera tillbakaskrivning av lösenord här löser du problemet |Det gick inte |
| Vi kunde inte nå din lokala lösenordsåterställning av tjänsten. Händelseloggen för datorn synkronisering |Det gick inte |
| Det uppstod ett problem när du återställer användarens lokala lösenord. Händelseloggen för datorn synkronisering |Det gick inte |
| Den här användaren är inte medlem i användargruppen för återställning av lösenord. Lägg till användaren till gruppen att lösa problemet. |Det gick inte |
| Återställning av lösenord har inaktiverats helt och hållet för den här klienten. Se [här](http://aka.ms/ssprtroubleshoot) att lösa problemet. |Det gick inte |
| Användaren återställts har lösenord |Lyckades |

## <a name="next-steps"></a>Nästa steg
Nedan finns länkar till alla sidor med dokumentation om lösenordsåterställning i Azure AD:

* **Är du här eftersom du har problem med att logga in?** I så fall är det [här som du ser hur du kan ändra och återställa ditt eget lösenord](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
* [**Så här fungerar det**](active-directory-passwords-how-it-works.md) – lär dig om de sex olika komponenterna i tjänsten och vad de gör
* [**Komma igång** ](active-directory-passwords-getting-started.md) – Lär dig hur du tillåter användare att återställa och ändra sina lösenord i molnet eller lokalt
* [**Anpassa**](active-directory-passwords-customize.md) – lär dig hur du anpassar tjänstens utseende, känsla och beteende efter din organisations behov
* [**Metodtips**](active-directory-passwords-best-practices.md) – lär dig hur du snabbt distribuerar och effektivt hanterar lösenord i din organisation
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – få svar på vanliga frågor
* [**Felsökning**](active-directory-passwords-troubleshoot.md) – lär dig hur du snabbt felsöker problem med tjänsten
* [**Lär dig mer**](active-directory-passwords-learn-more.md) – gräv djupare i de tekniska detaljerna för tjänsten
