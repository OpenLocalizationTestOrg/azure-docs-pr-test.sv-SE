---
title: "Vanliga frågor och svar: Azure AD SSPR | Microsoft Docs"
description: "Vanliga frågor och svar om Azure AD-lösenordet för självbetjäning återställa"
services: active-directory
keywords: "Hantering av Active directory-lösenord, lösenordshantering, Azure AD self service för lösenordsåterställning"
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
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a>Vanliga och frågor svar om lösenordshantering

hello följande är några vanliga frågor för allt som rör relaterade toopassword återställa.

Om du har en allmän fråga om Azure AD och självbetjäning lösenord återställning som inte besvaras här, du kan be hello community för att få hjälp på hello [Azure Ad-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Medlemmar i communityn hello innehåller tekniker, projektledare, MVP och andra IT-proffs.

Dessa vanliga frågor är uppdelat i hello följande avsnitt:

* [**Frågor om registreringen för lösenordsåterställning**](#password-reset-registration)
* [**Frågor om återställning av lösenord**](#password-reset)
* [**Frågor om ändring av lösenord**](#password-change)
* [**Frågor om lösenordshantering rapporter**](#password-management-reports)
* [**Frågor om tillbakaskrivning av lösenord**](#password-writeback)

## <a name="password-reset-registration"></a>Registrering för lösenordsåterställning
* **F: kan användarna registrera sina egna data för återställning av lösenord?**

  > **S:** Ja, så länge återställning av lösenord är aktiverat och de licensierade kan de gå toohello registrering för lösenordsåterställning portalen på http://aka.ms/ssprsetup tooregister sina autentiseringsinformationen. Användare kan också registrera genom att gå åtkomstpanelen för toohello på http://myapps.microsoft.com, fliken för hello-profil och klicka på hello registrering för lösenordsåterställning alternativet.
  >
  >
* **F: kan jag definiera data om återställning av lösenord för åt mina användare?**

  > **S:** Ja, kan du göra det med Azure AD Connect PowerShell hello [Azure-portalen](https://portal.azure.com), eller hello administrationsportalen för Office. Mer information finns i artikeln hello [Data som används av Azure AD Självbetjäning för återställning av lösenord](active-directory-passwords-data.md).
  >
  >
* **F: kan jag synkroniserar data om säkerhetsfrågor lokalt?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: kan användarna registrera data så att andra användare inte kan se dessa data?**

  > **S:** Ja, när användare registrerar data med hjälp av hello återställa portalen för Lösenordsregistrering sparas i privata autentisering fält som endast visas av globala administratörer och hello användare.
    >
    > [!NOTE]
    > Om en **Azure administratörskontot** registrerar sina telefonnummer för autentisering som den också fylls i hello mobiltelefon fältet och är synliga.
    >
  >
  >
* **F: Mina användare har toobe registrerad innan de kan använda lösenordsåterställning?**

  > **S:** Nej, om du definierar tillräckligt med autentiseringsinformation för åt användare har inte tooregister. Lösenordsåterställning fungerar så länge data som lagras i hello relevanta fält i hello directory är korrekt formaterat.
  >
  >
* **F: kan jag synkronisera eller ange hello telefon för autentisering, e-autentisering eller autentisering telefon fält åt mina användare?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: hur portalen för registrering av hello känner vilka alternativ tooshow Mina användare?**

  > **S:** hello lösenordsåterställning portalen för registrering av endast visar hello alternativ som du har aktiverat för dina användare. Dessa alternativ finns under hello avsnitt princip för lösenordsåterställning för användare i din katalog konfigurera fliken. Detta innebär till exempel att om du inte aktiverar säkerhetsfrågor sedan användare inte som kan tooregister för det alternativet.
  >
  >
* **F: när en användare anses vara registrerad?**

  > **S:** en användare anses registrerade för SSPR när de har registrerat minst hello **antal metoder krävs tooreset** som du har angett i hello [Azure-portalen](https://portal.azure.com).
  >
  >
## <a name="password-reset"></a>Lösenordsåterställning
* **F: hur länge vänta bör tooreceive en e-post, SMS eller telefonsamtal från lösenordsåterställning?**

  > **S:** e-post, SMS-meddelanden och samtal ska komma in under en minut med hello normal fallet 5-20 sekunder.
    >Om du inte har fått hello-meddelande i den här tidsperiod:
        > * Kontrollera mappen för skräppost.
        > * Kontrollera hello eller en e-kontaktas är hello något du förväntar dig.
        > * Kontrollera hello autentiseringsdata i hello directory är korrekt formaterad.
                >     * Exempel: ”+ 1 4255551234” eller ”user@contoso.com”
  >
  >
* **F: på vilka språk som stöds av lösenordsåterställning?**

  > **S:** hello lösenordsåterställning UI, SMS-meddelanden och röst anrop är lokaliserade till hello samma språk som stöds i Office 365.
  >
  >
* **F: vilka delar av hello lösenord Återställ hämta märkta när jag har angett organisationens företagsanpassning i min katalog har konfigurera fliken?**

  > **S:** hello-lösenordsåterställning, portal visar din organisations logotyp och ger dig tooconfigure hello Kontakta din administratör länken toopoint tooa anpassade e-postadress eller URL. E-postmeddelandet som skickas av lösenordsåterställning innehåller organisationens logotyp, färger, namn i hello brödtext hello e-post och anpassa namn.
  >
  >
* **F: hur kan jag för att informera användarna om var toogo tooreset sina lösenord?**

  > **S:** du kan skicka dina användare toohttps://passwordreset.microsoftonline.com direkt eller instruera tooclick hello **kan inte komma åt ditt kontolänken** finns på alla arbets- eller Skol-inloggningssida. Du kan också publicera dessa länkar i en lättillgänglig tooyour plats-användare.
  >
  >
* **F: kan jag använda den här sidan från en mobil enhet?**

  > **S:** Ja, den här sidan fungerar på mobila enheter.
  >
  >
* **F: du stöder upplåsning lokala active directory-konton när användarna återställa sina lösenord?**

  > **S:** Ja, när en användare återställer sitt lösenord och tillbakaskrivning av lösenord har distribuerats med Azure AD Connect, användarens konto låses automatiskt när de återställa sina lösenord.
  >
  >
* **F: hur kan jag integrera lösenordsåterställning direkt till min användarens inloggning Skrivbordsmiljö?**

  > **S:** om du är en Azure AD Premium-kund kan du kan installera Microsoft Identity Manager utan extra kostnad och distribuera hello lokala lösenord Återställ lösning toomeet det här kravet.
  >
  >
* **F: kan jag ange olika säkerhetsfrågor för olika språk?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: hur många frågor kan vi konfigurera för hello säkerhetsfrågor autentiseringsalternativet?**

  > **S:** kan du konfigurera too20 anpassade säkerhetsfrågor i hello [Azure-portalen](https://portal.azure.com).
  >
  >
* **F: hur lång tid kanske säkerhetsfrågor?**

  > **S:** säkerhetsfrågor kan innehålla mellan 3 och 200 tecken.
  >
  >
* **F: hur lång tid kanske svar toosecurity frågor?**

  > **S:** svar får inte vara 3 too40 tecken.
  >
  >
* **F: dubblettsvar toosecurity frågor avvisas?**

  > **S:** Ja, vi avvisa dubblettsvar toosecurity frågor.
  >
  >
* **F: kan en användare registrera hello samma säkerhetsfråga mer än en gång?**

  > **S:** Nej, när en användare som registrerar en fråga, de kan inte registrera dig för att fråga en andra gång.
  >
  >
* **F: är det möjligt tooset minimigräns av säkerhetsfrågor för registrering och återställning av?**

  > **S:** Ja, en gräns kan anges för registrering och en annan för återställning. 3-5 säkerhetsfrågor kan krävas för registrering och 3-5 kan krävas för återställning.
  >
  >
* **F: om en användare har registrerat mer än hello maximalt antal frågor krävs tooreset, hur säkerhetsfrågor väljs under återställning?**

  > **S:** N säkerhet frågorna väljs slumpmässigt hello Totalt antal frågor en användare har registrerats för, där N är hello **antalet frågor nödvändiga tooreset**. Till exempel om en användare har 5 säkerhetsfrågor registrerade, men endast 3 är obligatoriska tooreset, är 3 av hello 5 väljas slumpmässigt och uppvisas vid återställning. Om hello användare hämtar hello svar toohello frågor fel, återkommer hello markeringen processen tooprevent fråga hammering.
  >
  >
* **F: du hindrar användare från att försöka lösenordsåterställning många gånger under en kort tidsperiod?**

  > **S:** Ja, det finns inbyggda i lösenord Återställ tooprotect från missbruk säkerhetsfunktioner. Användare kan bara försök 5 Återställ lösenordsförsök inom en timme innan är utelåst under 24 timmar. Användare kan bara försöker toovalidate ett telefonnummer 5 gånger inom en timme innan är utelåst under 24 timmar. Användare kan bara försöker en enda autentiseringsmetod 5 gånger inom en timme innan är utelåst under 24 timmar.
  >
  >
* **F: för hur länge är hello e-post och SMS engångskod giltiga?**

  > **S:** hello sessioners livstid för återställning av lösenord är 105 minuter. Från början hello hello lösenordsåterställning åtgärden, hello användaren har 105 minuter tooreset sitt lösenord. hello är e-post och SMS engångskod ogiltiga när den här tiden har löpt ut.
  >
  >

## <a name="password-change"></a>Ändra lösenordet
* **F: bör användarna var toochange sina lösenord?**

  > **S:** användare kan ändra sina lösenord överallt där de ser sina profilbild eller ikonen (precis som i hello övre högra hörnet på sina [Office 365](https://portal.office.com) eller [åtkomstpanelen](https://myapps.microsoft.com) inträffar. Användare kan ändra sina lösenord från hello [profil åtkomstpanelsidan](https://account.activedirectory.windowsazure.com/r#/profile). Användare kan också vara och toochange sina lösenord automatiskt vid hello Azure AD inloggningssida om sina lösenord har gått ut. Slutligen användare kan navigera toohello [Azure AD-lösenord ändra Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) direkt om de vill toochange sina lösenord.
  >
  >
* **F: kan användarna meddelas i hello Office-portalen när sina lokala lösenord upphör att gälla?**

  > **S:** detta är idag möjligt om du använder AD FS genom att följa hello anvisningarna här: [skicka lösenord princip anspråk med AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Om du använder hash-synkronisering av lösenord, är det inte möjligt i dag. Detta beror på att vi inte synkronisera lösenordsprinciper lokalt, så det inte är möjligt för oss toopost upphör att gälla meddelanden toocloud inträffar. I båda fallen kan också för[meddela användare vars lösenord om tooexpire med hjälp av PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Rapporter för lösenord
* **F: hur lång tid tar det för data tooshow in på rapporter hello lösenord?**

  > **S:** Data ska visas på hello lösenord rapporter inom 5-10 minuter. Den vissa instanser som det kan ta upp tooan timme tooappear.
  >
  >
* **F: hur filtrera rapporter hello lösenord?**

  > **S:** du kan filtrera rapporter hello lösenord genom att klicka på hello små förstoringsglas toohello extremt höger av hello kolumnetiketterna hello övre delen av hello rapporten. Om du vill toodo bättre filtrering kan du hämta hello rapporten tooexcel och skapa en pivottabell.
  >
  >
* **F: Vad är hello högsta antalet händelser lagras i rapporter hello lösenord?**

  > **S:** in too75, 000 lösenord återställning eller lösenord Återställ registrering händelser lagras i hello lösenord rapporter, utsträckning säkerhetskopiera too30 dagar.  Vi arbetar tooexpand detta number tooinclude fler händelser.
  >
  >
* **F: hur långt tillbaka går hello lösenord rapporter?**

  > **S:** hello lösenordshantering rapporterar Visa åtgärder som sker inom hello senaste 30 dagarna. För tillfället, om du behöver tooarchive data, kan du hämta hello rapporter med jämna mellanrum och spara dem på en annan plats.
  >
  >
* **F: finns det ett maximalt antal rader som kan visas på rapporter hello lösenord?**

  > **S:** Ja, högst 75 000 rader kan visas på något av hello lösenord rapporter, om de som visas i hello användargränssnitt eller håller på att hämtas.
  >
  >
* **F: finns det en API-tooaccess hello lösenord återställning eller registrering rapporteringsdata?**

  > **S:** Ja, hello finns följande dokumentation toolearn hur du kan komma åt hello lösenord återställa reporting dataström.  [Lär dig hur tooaccess lösenordsåterställning rapporteringshändelser programmässigt](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Tillbakaskrivning av lösenord
* **F: hur fungerar tillbakaskrivning av lösenord hello bakgrunden?**

  > **S:** finns [hur tillbakaskrivning av lösenord fungerar](active-directory-passwords-writeback.md) för en förklaring om vad som händer när du aktiverar tillbakaskrivning av lösenord och hur data som flödar genom hello system tillbaka till din lokala miljö.
  >
  >
* **F: hur lång tid tar tillbakaskrivning av lösenord toowork?  Finns det en synkronisering fördröjning som med synkronisering av lösenords-hash?**

  > **S:** tillbakaskrivning av lösenord är snabbmeddelanden. Det är en synkron pipeline som fungerar helt annorlunda än hash-synkronisering av lösenord. Tillbakaskrivning av lösenord kan användare tooget realtid feedback om hello genomförandet av sitt lösenord återställa eller ändra igen. hello Genomsnittlig tid för en lyckad tillbakaskrivning av lösenord är under 500 ms.
  >
  >
* **F: hur påverkas mitt konto/molnåtkomst om mitt lokalt konto har inaktiverats?**

  > **S:** om ditt lokala ID inaktiveras ditt moln-ID-/ access inaktiveras även vid hello nästa synkroniseringsintervall via AAD Connect byt standard är var 30: e minut.
  >
  >
* **F: om mitt konto lokalt är begränsad av en lokal Active Directory-lösenordsprincip, SSPR lyder under den här principen när jag ändrar hello lösenord?**

  > **S:** Ja, SSPR förlitar sig på och som följer av hello lokala AD-lösenordsprincip, inklusive vanliga lösenordsprinciper för AD i domänen, samt några definierade detaljerade lösenordsprinciper riktade tooa som anges av användaren.
  >
  >
* **F: vilka typer av konton fungerar tillbakaskrivning av lösenord för?**

  > **S:** tillbakaskrivning av lösenord fungerar för federerat och lösenord Hash synkroniserade användare.
  >
  >
* **F: tillbakaskrivning av lösenord kan genomdriva lösenordsprinciper för min domän?**

  > **S:** Ja, tillbakaskrivning av lösenord tillämpar ålder för lösenord, historik, komplexitet, filter och andra begränsningar som du kan infördes för lösenord i den lokala domänen.
  >
  >
* **F: är tillbakaskrivning av lösenord säker?  Hur vet jag att jag inte hämta över?**

  > **S:** Ja, tillbakaskrivning av lösenord är säker. tooread mer om hello fyra säkerhetslagren implementeras av hello lösenord tillbakaskrivning av tjänsten, kolla hello [lösenord tillbakaskrivning säkerhetsmodell](active-directory-passwords-writeback.md#password-writeback-security-model) avsnitt i hur tillbakaskrivning av lösenord fungerar.
  >
  >

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR
