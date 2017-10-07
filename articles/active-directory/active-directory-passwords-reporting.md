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
ms.openlocfilehash: af65b9be1e00c2819431694a5b0064b97b9e1867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Alternativ för Azure AD-lösenordshantering

Efter distributionen många organisationer vill tooknow hur eller om SSPR verkligen används. Azure AD tillhandahåller rapporteringsfunktioner som hjälper dig tooanswer frågor använder burk rapporter och om du är korrekt licensierade du toocreate egna frågor.

hello följande frågor kan besvaras av rapporter som finns i hello [Azure portal] (https://portal.azure.com/).

> [!NOTE]
> Du måste vara [en global administratör](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) och måste Anmäl för toobe för data som samlats in för din organisation, av besökande hello reporting fliken eller audit loggas minst en gång. Tills gör det, kommer inte samlas in för din organisation

* Hur många som har registrerats för återställning av lösenord?
* Vem som har registrerats för återställning av lösenord?
* Vilka data personer registrerar?
* Hur många personer återställa sina lösenord i hello senaste sju dagarna?
* Vad är hello vanligaste metoder användare eller administratörer använder tooreset sina lösenord?
* Vad är vanliga problem användare eller administratörer står inför när toouse lösenordsåterställning?
* Vilka administratörer ofta återställa sina lösenord?
* Finns det några misstänkt aktivitet fortsätter med återställning av lösenord?

## <a name="how-tooview-password-management-reports-in-hello-azure-portal"></a>Hur lösenordshantering tooview rapporter i hello Azure-portalen

I hello Azure-portaler har vi ett bättre sätt tooview återställning och lösenord registrering återställningsaktiviteten för lösenord.  Så hello nedan toofind hello lösenord återställning och lösenord Återställ registrering händelser:

1. Navigera för[**portal.azure.com**](https://portal.azure.com)
2. Klicka på hello **fler tjänster** menyn på hello huvudsakliga Azure portal vänstra navigeringsfönstret
3. Sök efter **Azure Active Directory** hello lista över tjänster och markera den i
4. Klicka på **användare och grupper** från hello Azure Active Directory-navigeringsmenyn
5. Klicka på hello **granskningsloggar** navigeringsobjektet från hello navigeringsmenyn för användare och grupper. Detta visar alla hello granskningshändelser inträffar mot alla hello användare i katalogen. Du kan filtrera den här vyn toosee alla hello lösenord-relaterade händelser, samt.
6. toofilter den här vyn tooonly hello lösenordsåterställning relaterade händelser, klicka på hello **Filter** knappen hello överst i hello-bladet.
7. Från hello **Filter** -menyn, Välj hello **kategori** listrutan och ändra den toohello **Self-service lösenordshantering** kategorityp.
8. Eventuellt ytterligare hello-filterlista genom att välja hello specifika **aktiviteten** du är intresserad av

## <a name="how-tooretrieve-password-management-events-from-hello-azure-ad-reports-and-events-api"></a>Hur tooretrieve lösenord management händelser från hello händelser API: er och Azure AD-rapporter

hello Azure AD-rapporter och händelser API stöder hämtar alla hello information som ska ingå lösenord återställning och lösenord Återställ registrering. Du kan hämta enskilda lösenordsåterställning med hjälp av den här API och lösenordsåterställning registrering händelser för integrering med hello reporting teknik du väljer.

### <a name="how-tooget-started-with-hello-reporting-api"></a>Hur tooget igång med hello reporting API

tooaccess dessa data du behöver toowrite en liten app eller skriptet tooretrieve från våra servrar. [Lär dig hur tooget igång med hello Azure AD Reporting API](active-directory-reporting-api-getting-started.md).

När du har ett fungerande skript ska du nästa tooexamine hello lösenord återställning och registrering händelser, som du kan hämta toomeet dina scenarier.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Visar en lista över tillgängliga hello kolumner för händelser för återställning av lösenord
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Visar en lista över tillgängliga hello kolumner för registrering händelser för återställning av lösenord

### <a name="reporting-api-data-retrieval-limitations"></a>Reporting API begränsningar för hämtning av data

För närvarande hello Azure AD-rapporter och händelser API hämtar in för**75 000 enskilda händelser** av hello [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) och [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typer , utsträckning hello **senaste 30 dagarna**.

Om du behöver tooretrieve eller lagra data utöver det här fönstret föreslår vi beständighet i en extern databas och använder hello API tooquery hello går som uppstår. Vår rekommendation är toobegin hämta dessa data när du börjar använda SSPR i din organisation, spara den externt och fortsätta tootrack hello går från den här tidpunkten och framåt.

## <a name="how-toodownload-password-reset-registration-events-quickly-with-powershell"></a>Hur toodownload lösenordsåterställning registrering händelser snabbt med PowerShell

Dessutom toousing Hej händelser API: er och Azure AD-rapporter direkt, kan du också använda hello nedan PowerShell-skriptet toorecent registrering händelser i din katalog. Detta är användbart om du vill ha toosee som har nyligen registrerad eller skulle vilja tooensure att distributionen för återställning av lösenord pågår som förväntat.

* [Azure AD SSPR-registrering aktivitet PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Beskrivning av rapportkolumner i Azure-portalen

hello beskriver följande lista varje hello rapportkolumner i detalj:

* **Användaren** – hello-användaren som försökte lösenord återställa registrering igen.
* **Rollen** – hello roll hello användare i hello directory.
* **Datum och tid** – hello datum och tid för hello försök.
* **Data registrerade** – vilka authentication data hello-användare som angavs under lösenord återställer registrering.

### <a name="description-of-report-values-in-azure-portal"></a>Beskrivning av rapporten värden i Azure-portalen

hello beskrivs följande tabell hello olika värden för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Data som registrerats |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-tooauthenticate<p><p>**Arbetstelefon**– användaren använda office phone tooauthenticate<p>**Mobiltelefon** -användare används mobiltelefon eller autentisering phone tooauthenticate<p>**Säkerhetsfrågor** – används användarsäkerhet frågor tooauthenticate<p>**Valfri kombination av hello ovan (t.ex, alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder hello användas tooauthentication sin begäran om lösenordsåterställning. |

## <a name="view-password-reset-activity-in-hello-classic-portal"></a>Visa aktiviteten i hello klassiska portalen för lösenordsåterställning

Den här rapporten visar alla försök som har inträffat i din organisation för lösenordsåterställning.

* **Maximalt tidsintervall**: 30 dagar
* **Max antal rader**: 75 000
* **Nedladdningsbar**: Ja, via CSV-fil

### <a name="description-of-report-columns-in-azure-classic-portal"></a>Beskrivning av rapportkolumner i den klassiska Azure-portalen

hello beskriver följande lista varje hello rapportkolumner i detalj:

1. **Användaren** – hello-användaren som försökte lösenord återställa åtgärden (baserat på fältet för användar-ID hello när hello användaren kommer tooreset ett lösenord).
2. **Rollen** – hello roll hello användare i hello directory.
3. **Datum och tid** – hello datum och tid för hello försök.
4. **Metoderna används** – vilka autentiseringsmetoder hello som ska användas för detta återställa igen.
5. **Resultatet** – hello resultatet av hello lösenord återställa igen.
6. **Information om** – hello information om varför hello lösenordsåterställning resulterade i hello-värde som den visades.  Även om alla säkerhetsåtgärder som du kan ta tooresolve ett oväntat fel.

### <a name="description-of-report-values-in-azure-classic-portal"></a>Beskrivning av rapporten värden i den klassiska Azure-portalen

hello beskrivs följande tabell hello olika värden för varje kolumn:

| Kolumn | Tillåtna värden och deras innebörd |
| --- | --- |
| Metoder som används |**Alternativ e-postadress** – användaren används alternativa e-post eller autentisering e-tooauthenticate<p>**Arbetstelefon** – användaren använda office phone tooauthenticate<p>**Mobiltelefon** – använda mobiltelefon eller autentisering phone tooauthenticate för användaren<p>**Säkerhetsfrågor** – används användarsäkerhet frågor tooauthenticate<p>**Valfri kombination av hello ovan (t.ex, alternativa e-post + mobiltelefon)** – inträffar när en 2-gate-princip har angetts och visar vilka två metoder hello användas tooauthentication sin begäran om lösenordsåterställning. |
| Resultat |**Avbrutna** – användaren startade lösenordsåterställning men sedan stoppas halvvägs via utan att slutföra<p>**Blockerad** – användarkontot har förhindrat toouse lösenordsåterställning på grund av tooattempting toouse hello lösenord återställa sidan eller gate för återställning av en enda lösenord för många gånger under en 24-timmarsperiod<p>**Avbruten** – användaren startade lösenordsåterställning men klickat på hello Avbryt knappen toocancel hello session halvvägs via <p>**Kontakta administratören** – användaren hade problem under sin session som han inte gick att matcha, så hello användaren klickar på hello ”kontaktar du administratören” länk i stället för att slutföra hello lösenordsåterställning flöde<p>**Det gick inte** – användaren inte kan tooreset ett lösenord, sannolikt eftersom hello användaren inte konfigurerade toouse hello-funktionen (till exempel ingen licens, autentisering-information som saknas, lösenordet hanteras lokalt men tillbakaskrivning är inaktiverat).<p>**Lyckades** – lösenordsåterställning lyckades. |
| Information |Se tabellen nedan |

### <a name="allowed-values-for-details-column"></a>Tillåtna värden för kolumnen information

Nedan finns hello lista över resultattyper som du kan förvänta dig när du använder hello lösenord återställa aktivitetsrapport:

| Information | Typ av resultat |
| --- | --- |
| Användaren avbrutna när du har slutfört hello alternativ för verifiering av e-post |Avbrutna |
| Användaren avbrutna när du har slutfört hello mobila SMS verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har slutfört hello mobila röst anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har slutfört hello office röst anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har slutfört hello säkerhetsalternativ för frågor |Avbrutna |
| Användaren avbröts efter att ange sina användar-ID |Avbrutna |
| Användaren avbrutna när du har startat hello alternativ för verifiering av e-post |Avbrutna |
| Användaren avbrutna när du har startat hello mobila SMS verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har startat hello mobila röst anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har startat hello office röst anropet verifieringsalternativ |Avbrutna |
| Användaren avbrutna när du har startat hello säkerhetsalternativ för frågor |Avbrutna |
| Användaren avbrutna innan du väljer ett nytt lösenord |Avbrutna |
| Användaren avbrutna när du väljer ett nytt lösenord |Avbrutna |
| Användaren har angett för många ogiltiga koder för SMS och blockeras för 24 timmar |Blockerad |
| Användaren försökte röst mobiltelefonsverifiering för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren försökte office telefonverifiering röst för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren försökte tooanswer säkerhetsfrågor för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren försökte tooverify ett telefonnummer för många gånger och blockeras för 24 timmar |Blockerad |
| Användaren avbröt innan informationen skickas hello krävs autentiseringsmetoder |Avbrutna |
| Användaren avbröt innan du skickar ett nytt lösenord |Avbrutna |
| Användaren kontakta en administratör när du har försökt hello alternativ för verifiering av e-post |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt hello mobila SMS verifieringsalternativ |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt hello mobila röst anropet verifieringsalternativ |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt hello office röst anropet verifieringsalternativ |Kontaktade admin |
| Användaren kontakta en administratör när du har försökt hello säkerhet fråga verifieringsalternativ |Kontaktade admin |
| Återställning av lösenord har inte aktiverats för den här användaren. Aktivera lösenordsåterställning under hello konfigurera fliken tooresolve |Det gick inte |
| Användaren har inte en licens. Du kan lägga till en licens toohello användaren tooresolve detta |Det gick inte |
| Användaren försökte tooreset från en enhet utan att cookies har aktiverats |Det gick inte |
| Användarkontot har inte tillräckligt autentiseringsmetoder som definierats. Lägg till autentisering info tooresolve denna |Det gick inte |
| Användarens lösenord är hanteras lokalt. Du kan aktivera tillbakaskrivning av lösenord tooresolve det |Det gick inte |
| Vi kunde inte nå din lokala lösenordsåterställning av tjänsten. Händelseloggen för datorn synkronisering |Det gick inte |
| Det uppstod ett problem när du återställer hello användarens lokala lösenord. Händelseloggen för datorn synkronisering |Det gick inte |
| Den här användaren är inte medlem i användargruppen för hello lösenordsåterställning. Lägg till den här användaren toothat grupp tooresolve denna. |Det gick inte |
| Återställning av lösenord har inaktiverats helt och hållet för den här klienten. Se [här](http://aka.ms/ssprtroubleshoot) tooresolve detta. |Det gick inte |
| Användaren återställts har lösenord |Lyckades |

## <a name="self-service-password-management-activity-types"></a>Självbetjäning lösenordshantering aktivitetstyper

hello följande aktivitetstyper visas i hello **Self-Service lösenordshantering** audit händelsekategori.  En beskrivning för var och en av följande.

* [**Blockeras från lösenordsåterställning via självbetjäning** ](#activity-type-blocked-from-self-service-password-reset) -anger att en användare försökte tooreset ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* [**Ändra lösenord (Self Service)** ](#activity-type-change-password-self-service) -anger användaren utföra en frivillig eller framtvingad (på grund av tooexpiry) Ändra lösenordet.
* [**Återställ lösenord (som admin)** ](#activity-type-reset-password-by-admin) -anger att en administratör utföra en lösenordsåterställning för en användare från hello Azure-portalen.
* [**Återställ lösenord (Self Service)** ](#activity-type-reset-password-self-service) -anger att en användare har återställts sina lösenord från hello [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* [**Självbetjäning för lösenordsåterställning flödet aktivitet pågår** ](#activity-type-self-serve-password-reset-flow-activity-progress) -indikerar varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av hello lösenord att återställa lösenordet.
* [**Låsa upp användarkonto (Self Service)** ](#activity-type-unlock-user-account-self-service) -anger att en användare har låsts upp deras Active Directory-konto utan att återställa sina lösenord från hello [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med hello AD-kontot upplåsning utan funktionen för återställning.
* [**Användare som har registrerats för lösenordsåterställning via självbetjäning** ](#activity-type-user-registered-for-self-service-password-reset) -anger en användare har registrerat alla hello krävs information toobe kan tooreset sina lösenord i enlighet med hello specificerat klient princip för lösenordsåterställning.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Aktivitetstypen: blockeras från lösenordsåterställning via självbetjäning

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare försökte tooreset ett lösenord, använda en specifik gate eller verifiera ett telefonnummer mer än 5 Totalt antal gånger under 24 timmar.
* **Aktiviteten aktören** -hello-användare som har begränsats från att utföra ytterligare återställa åtgärder. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello-användare som har begränsats från att utföra ytterligare återställa åtgärder. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har begränsats försöka eventuella ytterligare autentiseringsmetoder från att utföra några ytterligare återställer eller verifiera eventuella ytterligare telefonnummer för hello nästkommande 24 timmar.
* **Aktivitetens Status felorsak** – ej tillämpligt

### <a name="activity-type-change-password-self-service"></a>Aktivitetstypen: ändra lösenord (Self Service)

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare utföra en frivillig eller framtvingad (på grund av tooexpiry) Ändra lösenordet.
* **Aktiviteten aktören** -hello användare ändra sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello användare ändra sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har ändrat sitt lösenord
  * _Fel_ -anger att en användare misslyckades toochange sitt lösenord. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade.
* **Aktivitetens Status felorsak** - 
  * _FuzzyPolicyViolationInvalidPassword_ -hello användaren har valt ett lösenord, som var förbjuden automatiskt på grund av Toomicrosoft's förbjudna lösenord identifieringsfunktionerna hitta dem toobe för vanligt eller särskilt svaga.

### <a name="activity-type-reset-password-by-admin"></a>Aktivitetstypen: Återställ lösenord (som admin)

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en administratör utföra en lösenordsåterställning för en användare från hello Azure-portalen.
* **Aktiviteten aktören** -Hej administratör som utförde hello lösenordsåterställning uppdrag av en annan användare eller administratör. Måste vara antingen en global administratör, lösenordsadministratör, Användaradministratör eller supportavdelning administratör.
* **Mål för aktiviteten** -hello användaren vars lösenord har återställts. Kan vara en slutanvändare eller en annan administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en administratör har återställts en användares lösenord
  * _Fel_ -visar administratör misslyckades toochange en användares lösenord. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade.

### <a name="activity-type-reset-password-self-service"></a>Aktivitetstypen: Återställ lösenord (Self Service)

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har återställts sina lösenord från hello [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com).
* **Aktiviteten aktören** -hello användare återställa sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello användare återställa sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har återställts sina egna lösenord
  * _Fel_ -anger att en användare misslyckades tooreset sina egna lösenord. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade.
* **Aktivitetens Status felorsak** -
  * _FuzzyPolicyViolationInvalidPassword_ -Hej administratör valt ett lösenord, som var förbjuden automatiskt på grund av Toomicrosoft's förbjudna lösenord identifieringsfunktionerna hitta dem toobe för vanligt eller särskilt svaga.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Aktivitetstypen: Self hantera lösenord återställa flödet aktivitet pågår

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger varje specifikt steg som en användare fortsätter via (t.ex. skicka ett särskilt lösenord återställa autentiseringsgaten) som en del av hello lösenord att återställa lösenordet.
* **Aktiviteten aktören** -hello-användaren som utförde en del av hello lösenord återställa flödet. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello-användaren som utförde en del av hello lösenord återställa flödet. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ – visar en användare har slutförts specifikt steg i hello lösenord Återställ flöde.
  * _Fel_ -anger ett specifikt steg hello lösenord återställa flödet misslyckades. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade.
* **Tillåtna aktivitetens statusorsaker**
  * Se tabellen nedan för [alla tillåtna återställningsaktivitet statusorsaker](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Aktivitetstypen: låsa upp användarkonto (Self Service)

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har låsts upp deras Active Directory-konto utan att återställa sina lösenord från hello [Azure AD-lösenordsåterställning, Portal](https://passwordreset.microsoftonline.com) med hello AD konto upplåsning utan återställning funktionen.
* **Aktiviteten aktören** -hello användare låsas upp sitt konto utan att återställa sina lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello användare låsas upp sitt konto utan att återställa sina lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger att en användare har låsts upp sitt eget konto
  * _Fel_ -anger att en användare misslyckades toounlock sitt konto. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Aktivitetstypen: användare som har registrerats för lösenordsåterställning via självbetjäning

hello följande lista beskrivs den här aktiviteten i detalj:

* **Aktivitetsbeskrivning av** – anger en användare har registrerat alla hello krävs information toobe kan tooreset sina lösenord i enlighet med hello specificerat klient princip för lösenordsåterställning. 
* **Aktiviteten aktören** -hello användare som registrerats för återställning av lösenord. Kan vara en användare eller administratör.
* **Mål för aktiviteten** -hello användare som registrerats för återställning av lösenord. Kan vara en användare eller administratör.
* **Tillåtna aktivitetens status**
  * _Lyckade_ -anger en användare som har registrerats för lösenordsåterställning i enlighet med hello aktuella principen. 
  * _Fel_ -anger en användare misslyckades tooregister för återställning av lösenord. Klicka på hello raden kan du toosee hello **Statusanledning för aktiviteten** kategori toolearn mer om varför hello-fel inträffade. Observera - detta inte innebär en användare är tooreset sina egna lösenord, bara de inte kunde slutföras hello registreringsprocessen. Om det finns overifierade data på sitt konto som stämmer (till exempel ett telefonnummer som inte har verifierats), även om de inte har kontrollerat telefonnumret, de kan fortfarande använda den tooreset sitt lösenord. Mer information finns i [vad som händer när en användare som registrerar?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [Genväg toouser management granskningsloggar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) – gå direkt tooyour klient Användarhantering granskningsloggar
* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
