---
title: Distribution av Azure AD SSPR | Microsoft Docs
description: "Tips för en lyckad distribution av Azure AD:s självbetjäningsfunktion för återställning av lösenord"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 73d31679b38ff009a767335adaebc49fbc5a75b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="roll-out-password-reset-for-users"></a>Distribuera lösenordsåterställning till användare

De flesta kunder åtgärderna hello som följer tooensure en smidig distribution av SSPR-funktioner.

1. [Aktivera lösenordsåterställning i katalogen](active-directory-passwords-getting-started.md)
2. [Konfigurera lokala AD-behörigheter för tillbakaskrivning av lösenord](active-directory-passwords-how-it-works.md#active-directory-permissions)
3. [Konfigurera tillbakaskrivning av lösenord](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite lösenord från Azure AD tillbaka tooyour lokal katalog
4. [Tilldela och verifiera nödvändiga licenser](active-directory-passwords-licensing.md)
5. Om du vill tooroll gradvis, du kan alternativt gränsen lösenordsåterställning tooa grupp av användare tooroll ut hello funktionen långsamt över tid. toodo detta in hello **Self Service lösenord återställa aktiverat** växla från **alla** för**en grupp** och välj en grupp tooenable för säkerhet för återställning av lösenord. hello medlemmar i den här gruppen har licenser som tilldelats toothem och är ett bra sätt tooenable [grupp baserad licensiering](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing).
6. Fylla hello minsta uppsättning [autentiseringsdata](active-directory-passwords-data.md), baserat på din princip.
7. Utbilda användarna hur toouse SSPR, genom att skicka dem instruktioner tooshow dem hur tooregister och hur tooreset.
    > [!NOTE]
    > Testa SSPR med en användare och inte en administratör eftersom Microsoft tillämpar starka autentiseringskrav för Azure-konton av administratörstyp. Mer information om Hej administratör lösenordsprincip finns i vår [ingående artikel](active-directory-passwords-how-it-works.md).

8. Du kan välja tooenforce registrering när som helst och behöver användare tooreconfirm sina autentiseringsinformation efter en viss tidsperiod. Om du inte vill att dina användare toohave tooregister, kan du [distribuera lösenordsåterställning utan slutanvändarregistrering](active-directory-passwords-data.md).
9. Granska användare registrerar och använder genom att visa hello över tiden, [reporting tillhandahålls av Azure AD](active-directory-passwords-reporting.md).

## <a name="email-based-rollout"></a>E-postbaserad distribution

Många kunder att hitta en e-post-kampanj med enkla toouse instruktioner hello enklaste sättet tooget användare toouse SSPR. [Vi har skapat tre enkla e-postmeddelanden som du kan använda som mallar toohelp i distributionen.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* **Kommer snart** e-mallen toobe används i hello veckor eller dagar innan distributionen toolet användarna vet de behöver toodo något.
* **Tillgängliga nu** e-mallen toobe används hello veckodag starta toodrive användare tooregister och bekräfta sina autentiseringsdata så att de kan använda SSPR när de behöver.
* **Logga in påminnelse** e-postmallen för några dagar tooweeks efter distributionen tooremind användare tooregister och bekräfta sina autentiseringsdata.

## <a name="creating-your-own-password-portal"></a>Skapa din egen lösenordsportal

Många av våra större kunder väljer toohost webbsidan och skapa en rot-DNS-posten som https://passwords.contoso.com. De fylla sidan med länkar toohello Azure AD-lösenordsåterställning, lösenordsåterställning registrering, portaler för ändring av lösenord och annan organisation-specifik information. I alla e-postmeddelanden eller reklamblad kan du skicka ut, du sedan inkludera en företagsanpassad lätt att komma ihåg, URL: en att användarna kan gå toowhen som de behöver toouse hello tjänster.

* Portal för lösenordsåterställning – https://passwordreset.microsoftonline.com/
* Portal för registrering inför lösenordsåterställning – http://aka.ms/ssprsetup
* Portal för ändring av lösenord – https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Använda tvingade registrering

Om du vill att dina användare tooregister för återställning av lösenord, kan du tvinga dem tooregister när de loggar in med Azure AD. Du kan aktivera det här alternativet från din katalog **lösenordsåterställning** bladet genom att aktivera hello **kräver användare tooRegister när du loggar in** alternativet på hello **registrering** fliken.

Administratörer kan kräva användare toore-register efter en tid genom att ange hello **antal dagar innan användarna och tooreconfirm sina autentiseringsinformation** mellan 0-730 dagar.

När du aktiverar det här alternativet kan användare som loggar visas ett meddelande som informerar dem om administratören har begärt dem tooverify sina autentiseringsinformationen.

## <a name="populate-authentication-data"></a>Fylla i autentiseringsdata

Om du [fylla autentiseringsdata för dina användare](active-directory-passwords-data.md), och användarna inte behöver tooregister för lösenordsåterställning innan du kan toouse SSPR. Så länge som användarna har hello autentiseringsdata definieras som uppfyller hello principen för lösenordsåterställning du har definierat, användare är kan tooreset sina lösenord.

## <a name="disabling-self-service-password-reset"></a>Inaktivera självbetjäningsfunktionen för återställning av lösenord

Inaktivera lösenordsåterställning via självbetjäning är så enkelt som att öppna Azure AD-klienten och gå för**lösenordsåterställning**, **egenskaper**, och välja **ingen** under  **Aktiverad för lösenordsåterställning via självbetjäning**

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR
