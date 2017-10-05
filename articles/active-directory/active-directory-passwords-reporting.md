---
title: 'Rapportering: Azure AD SSPR | Microsoft Docs'
description: "Rapportering av Azure AD-lösenordet för självbetjäning återställa händelser"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: ba7b36e654aa0bf3b74d42a2b0ae96ae2a9b6241
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Alternativ för Azure AD-lösenordshantering

Efter distributionen som många organisationer vill veta hur eller om SSPR verkligen används. Azure AD innehåller rapporteringsfunktioner som hjälper dig att besvara frågor med kapslade rapporter och om du har rätt licenser, kan du skapa egna frågor.

Följande frågor kan betjänas av rapporter som finns i [Azure-portalen] (https://portal.azure.com/).

> [!NOTE]
> Du måste vara [en global administratör](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) och måste välja i dessa data samlas in för din organisation genom att besöka reporting TABB eller granska loggarna minst en gång. Tills gör det, kommer inte samlas in för din organisation

* Hur många som har registrerats för återställning av lösenord?
* Vem som har registrerats för återställning av lösenord?
* Vilka data personer registrerar?
* Hur många personer återställa sina lösenord under de senaste sju dagarna?
* Vilka är de vanligaste metoder användarna eller administratörer använder för att återställa sina lösenord?
* Vad är vanliga problem med användare eller administratörer står inför när de försöker använda lösenordsåterställning?
* Vilka administratörer ofta återställa sina lösenord?
* Finns det några misstänkt aktivitet fortsätter med återställning av lösenord?

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Så här visar du lösenordshantering rapporter i Azure-portalen

Vi har ett bättre sätt att visa lösenordsåterställning och lösenordsåterställningsaktivitet för registrering i Azure portal upplevelsen.  Följ stegen nedan för att söka efter återställning av lösenord och lösenord för återställning av registrering händelser:

1. Gå till [ **portal.azure.com**](https://portal.azure.com)
2. Klicka på den **fler tjänster** menyn på det huvudsakliga Azure portal vänstra navigeringsfönstret
3. Sök efter **Azure Active Directory** i listan över tjänster och markera den
4. Klicka på **användare och grupper** i Azure Active Directory-navigationsmenyn
5. Klicka på den **granskningsloggar** navigeringsobjektet från navigeringsmenyn användare och grupper. Detta visar alla granskningshändelser inträffar mot alla användare i din katalog. Du kan filtrera den här vyn om du vill se alla lösenord-relaterade händelser, samt.
6. Om du vill filtrera vyn till endast relaterade händelser för återställning av lösenord, klickar du på den **Filter** längst upp på bladet.
7. Från den **Filter** väljer du den **kategori** listrutan och ändra den till den **Self-service lösenordshantering** kategorityp.
8. Du kan också ytterligare filter i listan genom att välja specifikt **aktiviteten** du är intresserad av

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Hur du hämtar lösenord management händelser från Azure AD-rapporter och händelser API

Azure AD-rapporter och händelser API stöder hämtar all information som inkluderas i lösenordet återställning och lösenord Återställ registrering rapporter. Du kan hämta enskilda lösenordsåterställning och registrering händelser för integrering med reporting tekniken önskat för återställning av lösenord med hjälp av den här API.

### <a name="how-to-get-started-with-the-reporting-api"></a>Hur du kommer igång med reporting API

Du måste skriva en liten app eller ett skript för att hämta från våra servrar för att komma åt dessa data. [Lär dig hur du kommer igång med Azure AD Reporting API](active-directory-reporting-api-getting-started.md).

När du har ett fungerande skript ska du därefter vill undersöka lösenord återställning och registrering händelser som du kan hämta för att uppfylla dina scenarier.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Visar en lista över tillgängliga kolumner för händelser för återställning av lösenord
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Visar en lista över tillgängliga kolumner för registrering händelser för återställning av lösenord

### <a name="reporting-api-data-retrieval-limitations"></a>Reporting API begränsningar för hämtning av data

För närvarande Azure AD-rapporter och händelser API hämtar upp till **75 000 enskilda händelser** av den [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) och [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typer som sträcker sig från den **senaste 30 dagarna**.

Om du behöver hämta eller lagra data utöver det här fönstret, föreslår vi beständighet i en extern databas och använder API: et för att fråga går som uppstår. Vår rekommendation är att börja hämta dessa data när du börjar använda SSPR i din organisation, spara den externt och sedan fortsätta att spåra går från den här tidpunkten och framåt.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Hämta lösenordet för återställning registrering händelser snabbt med PowerShell

Förutom att använda Azure AD-rapporter och händelser API direkt, kan du också använda den under PowerShell-skript till den senaste registreringen händelser i din katalog. Detta är användbart om du vill se vem som har registrerats nyligen eller skulle vilja se till att distributionen återställning av lösenord pågår som förväntat.

* [Azure AD SSPR-registrering aktivitet PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Beskrivning av rapportkolumner i Azure-portalen

I följande lista beskrivs var och en av rapportkolumner i detalj:

* **Användaren** – användaren som försökte lösenord återställa registrering igen.
* **Rollen** – rollen för användare i katalogen.
* **Datum och tid** – datum och tid för försöket.
* **Data registrerade** – vilka autentiseringsdata som användaren har angett under lösenord återställer registrering.

### <a name="description-of-report-values-in-azure-portal"></a>Beskrivning av rapporten värden i Azure-portalen

I följande tabell beskrivs de olika värden som tillåts för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Data som registrerats |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-postadress för att autentisera<p><p>**Arbetstelefon**– Arbetstelefon för användare som används för att autentisera<p>**Mobiltelefon** -användare används mobiltelefon eller telefon för autentisering för autentisering<p>**Säkerhetsfrågor** – användaren används säkerhetsfrågor för att autentisera<p>**Valfri kombination av ovanstående (till exempel alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder den användare som används för autentisering deras begäran om lösenordsåterställning. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Visa aktiviteten i den klassiska portalen för lösenordsåterställning

Den här rapporten visar alla försök som har inträffat i din organisation för lösenordsåterställning.

* **Maximalt tidsintervall**: 30 dagar
* **Max antal rader**: 75 000
* **Nedladdningsbar**: Ja, via CSV-fil

### <a name="description-of-report-columns-in-azure-classic-portal"></a>Beskrivning av rapportkolumner i den klassiska Azure-portalen

I följande lista beskrivs var och en av rapportkolumner i detalj:

1. **Användaren** – användaren som försökte lösenord återställa åtgärden (baserat på fältet Användar-ID anges när användaren kommer att återställa ett lösenord).
2. **Rollen** – rollen för användare i katalogen.
3. **Datum och tid** – datum och tid för försöket.
4. **Metoderna används** – vilka autentiseringsmetoder som användes för den här återställa igen.
5. **Resultatet** – resultatet av lösenordet återställa igen.
6. **Information om** – information om varför lösenordsåterställning resulterade i det gjorde-värdet.  Även om alla säkerhetsåtgärder som du kan vidta för att lösa ett oväntat fel.

### <a name="description-of-report-values-in-azure-classic-portal"></a>Beskrivning av rapporten värden i den klassiska Azure-portalen

I följande tabell beskrivs de olika värden som tillåts för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Metoder som används |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-postadress för att autentisera<p>**Arbetstelefon** – Arbetstelefon för användare som används för att autentisera<p>**Mobiltelefon** – mobiltelefon för användare som används eller telefon för autentisering för autentisering<p>**Säkerhetsfrågor** – användaren används säkerhetsfrågor för att autentisera<p>**Valfri kombination av ovanstående (till exempel alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder den användare som används för autentisering deras begäran om lösenordsåterställning. |
| Resultat |**Avbrutna** – användaren startade lösenordsåterställning men sedan stoppas halvvägs via utan att slutföra<p>**Blockerad** – användarens konto hindrades för lösenordsåterställning på grund av försök vill använda sidan för återställning av lösenord eller ett enda lösenord återställa gate för många gånger under en 24-timmarsperiod<p>**Avbruten** – användaren startade lösenordsåterställning men sedan klickat på knappen Avbryt om du vill avbryta en bra via sessionen <p>**Kontakta administratören** – användaren hade problem under sin session som han inte gick att matcha, så att användaren har klickat på länken ”kontaktar du administratören” i stället för att slutföra flödet för återställning av lösenord<p>**Det gick inte** – användaren kunde inte återställa ett lösenord, sannolikt eftersom användaren inte har konfigurerats för att använda funktionen (till exempel ingen licens, autentisering-information som saknas, lösenordet hanteras lokalt men tillbakaskrivning är inaktiverat).<p>**Lyckades** – lösenordsåterställning lyckades. |
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
| Användaren avbröt innan autentiseringsmetoderna som krävs |Avbrutna |
| Användaren avbröt innan du skickar ett nytt lösenord |Avbrutna |
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

## <a name="self-service-password-management-activity-types"></a>Självbetjäning lösenordshantering aktivitetstyper

Följande aktivitetstyper som visas i den **Self-Service lösenordshantering** audit händelsekategori.  En beskrivning för var och en av följande.

* [**Blockeras från lösenordsåterställning via självbetjäning** ](#activity-type-blocked-from-self-service-password-reset) -anger att en användare försökte återställa ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* [**Ändra lösenord (Self Service)** ](#activity-type-change-password-self-service) -anger användaren utföra en frivillig eller framtvingad (på grund av utgången) Ändra lösenordet.
* [**Återställ lösenord (som admin)** ](#activity-type-reset-password-by-admin) -anger att en administratör utföra en lösenordsåterställning för en användare från Azure-portalen.
* [**Återställ lösenord (Self Service)** ](#activity-type-reset-password-self-service) -anger att en användare har återställts sina lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* [**Självbetjäning för lösenordsåterställning flödet aktivitet pågår** ](#activity-type-self-serve-password-reset-flow-activity-progress) -indikerar varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av lösenordet att återställa lösenordet.
* [**Låsa upp användarkonto (Self Service)** ](#activity-type-unlock-user-account-self-service) -anger att en användare har låsts upp deras Active Directory-konto utan att återställa sina lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med hjälp av AD-kontot upplåsning utan funktionen för återställning.
* [**Användare som har registrerats för lösenordsåterställning via självbetjäning** ](#activity-type-user-registered-for-self-service-password-reset) -anger en användare har registrerat all nödvändig information för att kunna återställa sina lösenord i enlighet med den angivna innehavaren principen för lösenordsåterställning.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Aktivitetstypen: blockeras från lösenordsåterställning via självbetjäning

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare försökte återställa ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* **Aktiviteten aktören** -återställning av den användare som har begränsats från att utföra ytterligare åtgärder. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -återställning av den användare som har begränsats från att utföra ytterligare åtgärder. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har begränsats försöka eventuella ytterligare autentiseringsmetoder från att utföra några ytterligare återställer eller verifiera eventuella ytterligare telefonnummer för nästkommande 24 timmar.
* **Aktivitetens Status felorsak** – ej tillämpligt

### <a name="activity-type-change-password-self-service"></a>Aktivitetstypen: ändra lösenord (Self Service)

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare utföra en frivillig eller framtvingad (på grund av utgången) Ändra lösenordet.
* **Aktiviteten aktören** -användare ändra sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -användare ändra sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har ändrat sitt lösenord
  * _Fel_ -anger att en användare gick inte att ändra sina lösenord. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod.
* **Aktivitetens Status felorsak** - 
  * _FuzzyPolicyViolationInvalidPassword_ -användaren har valt ett lösenord, som var förbjuden automatiskt på grund av Microsofts förbjudna lösenord identifieringsfunktionerna hitta dem vara för vanligt eller särskilt svaga.

### <a name="activity-type-reset-password-by-admin"></a>Aktivitetstypen: Återställ lösenord (som admin)

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en administratör utföra en lösenordsåterställning för en användare från Azure-portalen.
* **Aktiviteten aktören** -administratören som utförs på uppdrag av en annan användare eller administratör för återställning av lösenord. Måste vara antingen en global administratör, lösenordsadministratör, Användaradministratör eller supportavdelning administratör.
* **Mål för aktiviteten** -användaren vars lösenord har återställts. Kan vara en slutanvändare eller en annan administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en administratör har återställts en användares lösenord
  * _Fel_ -anger att en administratör gick inte att ändra en användares lösenord. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod.

### <a name="activity-type-reset-password-self-service"></a>Aktivitetstypen: Återställ lösenord (Self Service)

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har återställts sina lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* **Aktiviteten aktören** -användaren som återställa sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -användaren som återställa sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har återställts sina egna lösenord
  * _Fel_ -anger att en användare gick inte att återställa sina egna lösenord. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod.
* **Aktivitetens Status felorsak** -
  * _FuzzyPolicyViolationInvalidPassword_ -administratören har valt ett lösenord, som var förbjuden automatiskt på grund av Microsofts förbjudna lösenord identifieringsfunktionerna hitta dem vara för vanligt eller särskilt svaga.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Aktivitetstypen: Self hantera lösenord återställa flödet aktivitet pågår

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av lösenordet återställa processen.
* **Aktiviteten aktören** -användaren som utförde en del av lösenordet återställa flödet. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -användaren som utförde en del av lösenordet återställa flödet. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ – visar en användare har slutförts specifikt steg för återställning av lösenord.
  * _Fel_ -anger ett specifikt steg av lösenordet återställa flödet misslyckades. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod.
* **Tillåtna aktivitetens statusorsaker**
  * Se tabellen nedan för [alla tillåtna återställningsaktivitet statusorsaker](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Aktivitetstypen: låsa upp användarkonto (Self Service)

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har låsts upp deras Active Directory-konto utan att återställa sina lösenord från den [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med hjälp av AD-kontot upplåsning utan funktionen för återställning.
* **Aktiviteten aktören** -användaren som låsas upp sitt konto utan att återställa sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -användaren som låsas upp sitt konto utan att återställa sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har låsts upp sitt eget konto
  * _Fel_ -anger att en användare kunde inte låsa upp sitt konto. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Aktivitetstypen: användare som har registrerats för lösenordsåterställning via självbetjäning

I följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har registrerat all nödvändig information för att kunna återställa sina lösenord i enlighet med den angivna innehavaren principen för lösenordsåterställning. 
* **Aktiviteten aktören** -användaren som registrerad för återställning av lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -användaren som registrerad för återställning av lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger en användare som har registrerats för lösenordsåterställning i enlighet med den aktuella principen. 
  * _Fel_ -anger att en användare kunde inte registreras för återställning av lösenord. Klicka på raden kan du se den **Statusanledning för aktiviteten** kategori som du vill veta mer om varför felet uppstod. Observera - detta innebär inte att inte det går för att återställa sina egna lösenord just de inte slutföra registreringen. Om det finns overifierade data på sitt konto som stämmer (till exempel ett telefonnummer som inte har verifierats), även om de inte har kontrollerat telefonnumret, de kan fortfarande använda den för att återställa sina lösenord. Mer information finns i [vad som händer när en användare som registrerar?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Nästa steg

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [Genväg till användaren management granskningsloggar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) – gå direkt till din klient Användarhantering granskningsloggar
* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? – Svar på allt du någonsin velat fråga
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
