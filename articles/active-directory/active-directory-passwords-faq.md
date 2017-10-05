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
ms.openlocfilehash: b3fab99ff9fab5bc67fa70113dc5b06fac775b09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="password-management-frequently-asked-questions"></a>Vanliga och frågor svar om lösenordshantering

Följande är några vanliga frågor och svar för alla objekt som är relaterade till återställning av lösenord.

Om du har en allmän fråga om Azure AD och självbetjäning lösenord återställning som inte besvaras här, du kan be gemenskapen för att få hjälp på den [Azure Ad-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Medlemmar i gruppen innehåller tekniker, projektledare, MVP och andra IT-proffs.

Dessa vanliga frågor är uppdelat i följande avsnitt:

* [**Frågor om registreringen för lösenordsåterställning**](#password-reset-registration)
* [**Frågor om återställning av lösenord**](#password-reset)
* [**Frågor om ändring av lösenord**](#password-change)
* [**Frågor om lösenordshantering rapporter**](#password-management-reports)
* [**Frågor om tillbakaskrivning av lösenord**](#password-writeback)

## <a name="password-reset-registration"></a>Registrering för lösenordsåterställning
* **F: kan användarna registrera sina egna data för återställning av lösenord?**

  > **S:** Ja, så länge återställning av lösenord är aktiverat och de licensierade kan de gå till portalen för registrering för lösenordsåterställning på http://aka.ms/ssprsetup att registrera sina autentiseringsinformationen. Användarna kan också registrera genom att gå till åtkomstpanelen på http://myapps.microsoft.com, på fliken profil och på Registrera dig för lösenordsåterställning alternativet.
  >
  >
* **F: kan jag definiera data om återställning av lösenord för åt mina användare?**

  > **S:** Ja, kan du göra det med Azure AD Connect PowerShell den [Azure-portalen](https://portal.azure.com), eller administrationsportalen för Office. Mer information finns i artikeln [Data som används av Azure AD Självbetjäning för återställning av lösenord](active-directory-passwords-data.md).
  >
  >
* **F: kan jag synkroniserar data om säkerhetsfrågor lokalt?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: kan användarna registrera data så att andra användare inte kan se dessa data?**

  > **S:** Ja, när användare registrerar data med hjälp av återställa Lösenordsregistreringsportal sparas i privata autentisering fält som endast visas av globala administratörer och användare.
    >
    > [!NOTE]
    > Om en **Azure administratörskontot** registrerar sina telefonnummer för autentisering så fylls också i fältet mobiltelefon och är synliga.
    >
  >
  >
* **F: Mina användare måste registreras innan de kan använda lösenordsåterställning?**

  > **S:** Nej, om du definierar tillräckligt med autentiseringsinformation för åt användarna behöver inte registrera. Lösenordsåterställning fungerar så länge som du har korrekt formaterat data som lagras i relevanta fält i katalogen.
  >
  >
* **F: kan jag synkronisera eller ange telefon för autentisering, e-autentisering eller autentisering telefon fält åt mina användare?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: hur vet vilket alternativ för att visa mina användare i portalen för registrering?**

  > **S:** på registreringsportalen för lösenordsåterställning bara visar alternativ som du har aktiverat för dina användare. Dessa alternativ finns under avsnittet princip för lösenordsåterställning för användare i din katalog konfigurera fliken. Detta innebär till exempel att om du inte aktiverar säkerhetsfrågor sedan användare inte kan registrera dig för det alternativet.
  >
  >
* **F: när en användare anses vara registrerad?**

  > **S:** en användare anses registrerade för SSPR när de har registrerat minst **antal metoder som krävs för att återställa** som du har angett i den [Azure-portalen](https://portal.azure.com).
  >
  >
## <a name="password-reset"></a>Lösenordsåterställning
* **F: hur lång tid ska gå att få ett e-post, SMS eller telefonsamtal från lösenordsåterställning?**

  > **S:** e-post, SMS-meddelanden och samtal ska komma in under en minut med normal fallet 5-20 sekunder.
    >Om du inte har fått meddelandet inom den här tiden:
        > * Kontrollera mappen för skräppost.
        > * Kontrollera numret eller e-post som kontaktas är den som du förväntar dig.
        > * Kontrollera autentiseringsdata i katalogen är korrekt formaterad.
                >     * Exempel: ”+ 1 4255551234” eller ”user@contoso.com”
  >
  >
* **F: på vilka språk som stöds av lösenordsåterställning?**

  > **S:** Användargränssnittet för lösenordsåterställning SMS-meddelanden och samtal är lokaliserade till samma språk som stöds i Office 365.
  >
  >
* **F: Vad delar av återställning av lösenord hämta märkta när jag har angett organisationens företagsanpassning i min katalog har konfigurera fliken?**

  > **S:** portalen för återställning av lösenord visar din organisations logotyp och du kan konfigurera kontakten länken administratören att peka till en anpassad e-postadress eller URL. E-postmeddelandet som skickas av lösenordsåterställning innehåller organisationens logotyp, färger, namn i brödtexten i e-postmeddelandet och anpassa namn.
  >
  >
* **F: hur kan jag för att informera användarna om hur du går att återställa sina lösenord?**

  > **S:** kan du skicka användarna till https://passwordreset.microsoftonline.com direkt eller kan du be dem att klicka på den **kan inte komma åt ditt kontolänken** finns på alla arbets- eller Skol-inloggningssida. Du kan också publicera dessa länkar på en plats som är lätt tillgängliga för användarna.
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

  > **S:** om du är en Azure AD Premium-kund kan du installera Microsoft Identity Manager utan extra kostnad och distribuera återställningslösning för lokala lösenord för att uppfylla detta krav.
  >
  >
* **F: kan jag ange olika säkerhetsfrågor för olika språk?**

  > **S:** detta inte är möjligt i dag.
  >
  >
* **F: hur många frågor kan vi konfigurera för alternativet säkerhetsfrågor autentisering?**

  > **S:** du kan konfigurera upp till 20 anpassade säkerhetsfrågor i den [Azure-portalen](https://portal.azure.com).
  >
  >
* **F: hur lång tid kanske säkerhetsfrågor?**

  > **S:** säkerhetsfrågor kan innehålla mellan 3 och 200 tecken.
  >
  >
* **F: hur lång tid kanske svar på säkerhetsfrågor?**

  > **S:** svar får inte vara 3 40 tecken.
  >
  >
* **F: är dubbla svar på säkerhetsfrågor avvisade?**

  > **S:** Ja, vi avvisa dubbla svar på säkerhetsfrågor.
  >
  >
* **F: kan en användare registrera samma säkerhetsfråga mer än en gång?**

  > **S:** Nej, när en användare som registrerar en fråga, de kan inte registrera dig för att fråga en andra gång.
  >
  >
* **F: är det möjligt att ange minimigräns av säkerhetsfrågor för registrering och återställa?**

  > **S:** Ja, en gräns kan anges för registrering och en annan för återställning. 3-5 säkerhetsfrågor kan krävas för registrering och 3-5 kan krävas för återställning.
  >
  >
* **F: om en användare har registrerats fler än det maximala antalet frågor som krävs för att återställa, hur säkerhetsfrågor väljs under återställning?**

  > **S:** N säkerhetsfrågor väljs slumpmässigt utanför det totala antalet frågor en användare har registrerats för, där N är den **antalet frågor som krävs för att återställa**. Till exempel om en användare har 5 säkerhetsfrågor registrerade, men endast 3 krävs för att återställa, är 3 av 5 valt slumpmässigt och uppvisas vid återställning. Om användaren får svar på frågorna fel återkommer val av processen för att förhindra att fråga hammering.
  >
  >
* **F: du hindrar användare från att försöka lösenordsåterställning många gånger under en kort tidsperiod?**

  > **S:** Ja, det finns säkerhetsfunktioner som är inbyggda i för att skydda från missbruk för återställning av lösenord. Användare kan bara försök 5 Återställ lösenordsförsök inom en timme innan är utelåst under 24 timmar. Användare kan endast försök att validera ett telefonnummer 5 gånger inom en timme innan är utelåst under 24 timmar. Användare kan bara försöker en enda autentiseringsmetod 5 gånger inom en timme innan är utelåst under 24 timmar.
  >
  >
* **F: för hur länge är e-post och SMS engångskod giltiga?**

  > **S:** sessioners livstid för återställning av lösenord är 105 minuter. Användaren har 105 minuter att återställa sina lösenord från början av återställning av lösenord. E-post och SMS engångskod är ogiltiga när den här tiden har löpt ut.
  >
  >

## <a name="password-change"></a>Ändra lösenordet
* **F: var ska gå Mina användare ändra sina lösenord?**

  > **S:** användare kan ändra sina lösenord överallt där de ser sina profilbild eller ikonen (precis som i det övre högra hörnet av deras [Office 365](https://portal.office.com) eller [åtkomstpanelen](https://myapps.microsoft.com) inträffar. Användare kan ändra sina lösenord från den [profil åtkomstpanelsidan](https://account.activedirectory.windowsazure.com/r#/profile). Användare kan också behöva ändra sina lösenord automatiskt på skärmen för Azure AD om sina lösenord har gått ut. Slutligen användare kan navigera till den [Azure AD-lösenord ändra Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) direkt om de vill ändra sina lösenord.
  >
  >
* **F: kan användarna meddelas i Office-portalen när sina lokala lösenord upphör att gälla?**

  > **S:** detta är idag möjligt om du använder AD FS genom att följa anvisningarna här: [skicka lösenord princip anspråk med AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Om du använder hash-synkronisering av lösenord, är det inte möjligt i dag. Det beror på att vi inte synkronisera lösenordsprinciper från lokalt, så det inte är möjligt för oss att publicera utgången meddelanden till molnet upplevelser. I båda fallen är det också möjligt att [meddela användare vars lösenord ska upphöra att gälla med hjälp av PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Rapporter för lösenord
* **F: hur lång tid tar det för att data ska visas i rapporter för lösenord?**

  > **S:** Data ska visas på rapporterna som lösenord management inom 5-10 minuter. Den vissa instanser som det kan ta upp till en timme ska visas.
  >
  >
* **F: hur filtrera rapporter för lösenord?**

  > **S:** du kan filtrera rapporter för lösenord genom att klicka på små förstoringsglaset till extrema höger om kolumnetiketterna längst upp i rapporten. Om du vill göra bättre filtrering kan du hämta rapport till excel och skapa en pivottabell.
  >
  >
* **F: Vad är det maximala antalet händelser som lagras i management-rapporter lösenord?**

  > **S:** upp till 75 000 återställning eller lösenord registreringen för lösenordsåterställning händelser lagras i rapporter för lösenord, utsträckning tillbaka upp till 30 dagar.  Vi arbetar för att expandera det här numret för att inkludera fler händelser.
  >
  >
* **F: hur långt tillbaka göra lösenordshantering rapporter gå?**

  > **S:** lösenordshanteringen rapporterar Visa åtgärder som sker inom de senaste 30 dagarna. Just nu om du vill arkivera informationen, kan du hämta rapporter med jämna mellanrum och spara dem på en annan plats.
  >
  >
* **F: finns det ett maximalt antal rader som kan visas på management-rapporter lösenord?**

  > **S:** Ja, högst 75 000 rader kan visas på något av lösenord för hantering av rapporter, om de som visas i Användargränssnittet eller håller på att hämtas.
  >
  >
* **F: finns det en API för åtkomst till lösenordsåterställning eller registrering rapporterar data?**

  > **S:** Ja, finns följande dokumentation för att lära dig hur du kan komma åt lösenordsåterställning reporting dataström.  [Lär dig att komma åt lösenordsåterställning rapporteringshändelser programmässigt](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Tillbakaskrivning av lösenord
* **F: hur fungerar tillbakaskrivning av lösenord i bakgrunden?**

  > **S:** finns [hur tillbakaskrivning av lösenord fungerar](active-directory-passwords-writeback.md) för en förklaring om vad som händer när du aktiverar tillbakaskrivning av lösenord och hur data som flödar genom systemet tillbaka till din lokala miljö.
  >
  >
* **F: hur lång tid tillbakaskrivning av lösenord tar ska fungera?  Finns det en synkronisering fördröjning som med synkronisering av lösenords-hash?**

  > **S:** tillbakaskrivning av lösenord är snabbmeddelanden. Det är en synkron pipeline som fungerar helt annorlunda än hash-synkronisering av lösenord. Tillbakaskrivning av lösenord kan användare få realtid feedback om genomförandet av deras återställning av lösenord eller ändra igen. Den genomsnittliga tiden för en lyckad tillbakaskrivning av lösenord är under 500 ms.
  >
  >
* **F: hur påverkas mitt konto/molnåtkomst om mitt lokalt konto har inaktiverats?**

  > **S:** om ditt lokala ID inaktiveras ditt moln-ID-/ access inaktiveras även vid nästa synkroniseringsintervall via AAD Connect byt standard är var 30: e minut.
  >
  >
* **F: om mitt konto lokalt är begränsad av en lokal Active Directory-lösenordsprincip, SSPR lyder under den här principen när jag ändrar lösenordet?**

  > **S:** Ja, SSPR förlitar sig på och som följer av lokalt AD-lösenordsprincip, inklusive vanliga lösenordsprincip för AD-domän, samt alla definierats detaljerade lösenordsprinciper för en viss användare.
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

  > **S:** Ja, tillbakaskrivning av lösenord är säker. Om du vill läsa mer om de fyra säkerhetslagren implementerats av tjänsten för tillbakaskrivning av lösenord, ta en titt på [lösenord tillbakaskrivning säkerhetsmodell](active-directory-passwords-writeback.md#password-writeback-security-model) avsnitt i hur tillbakaskrivning av lösenord fungerar.
  >
  >

## <a name="next-steps"></a>Nästa steg

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR
