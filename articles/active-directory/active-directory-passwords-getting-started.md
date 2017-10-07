---
title: 'Snabbstart: Azure AD SSPR | Microsoft Docs'
description: "Distribuera återställning av lösenord för självbetjäning i Azure AD snabbt"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4fed3a1c690fd6423ee5d3e5baef690d8896fbe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Snabbstart: återställning av lösenord för Azure AD-självbetjäning

> [!IMPORTANT]
> **Är du här eftersom du har problem med att logga in?** I så fall är det [här som du ser hur du kan ändra och återställa ditt eget lösenord](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Distribuera återställning av lösenord för självbetjäning snabbt

Självbetjäning för lösenordsåterställning (SSPR) erbjuder ett enkelt sätt för IT-administratörer tooempower användare tooreset eller låsa upp sitt lösenord eller konton. hello system innehåller detaljerade reporting tootrack när användare använder hello system tillsammans med meddelanden tooalert du toomisuse eller missbruk.

I den här handboken förutsätts att du redan har en aktiv utvärderingsversion eller licensierad Azure AD-klient. Om du behöver hjälp med att konfigurera Azure AD finns i artikeln hello [komma igång med Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Från din befintliga Azure AD-klient väljer du **"Återställning av lösenord"**

2. Från hello **”egenskaper”** skärmen under hello alternativet ”Self Service lösenord återställa aktiverat” Välj något av följande hello
    * Ingen - Inga en är kan toouse SSPR-funktioner
    * En grupp - gruppen endast medlemmar i en specifik Azure AD att du väljer är kan toouse SSPR-funktioner
    * Alla - alla användare med konton i Azure AD-klienten är kan toouse SSPR-funktioner

3. Från hello **”autentiseringsmetoder”** skärmen väljer
    * Antal metoder obligatoriskt tooreset – vi stöder minst en eller två
    * Metoder tillgängliga toousers - vi behöver minst en men den hurts aldrig toohave en extra alternativ som är tillgängligt
        * **E-post** skickar ett e-postmeddelande med en kod toohello användare konfigurerade e-postadress för autentisering
        * **Mobiltelefon** ger hello användaren hello val tooreceive ett samtal eller text med en kod tootheir konfigurerad mobila telefonnummer
        * **Arbetstelefon** anrop hello användare med en kod tootheir konfigurerat telefonnummer till arbetet
        * **Säkerhetsfrågor** kräver toochoose
            * Antal frågor som krävs för tooregister - hello minimum för lyckad registrering, vilket innebär att en användare kan välja tooanswer mer toocreate en pool av frågor toopull från. Det här alternativet måste kan anges från 3-5 och vara större än eller lika med toohello antalet nödvändiga tooreset för frågor.
                * Anpassade frågor kan läggas till genom att klicka på knappen för hello ”anpassad” när du väljer säkerhetsfrågor
            * Antal frågor som krävs tooreset - kan anges från 3-5 frågor toobe besvaras korrekt innan du tillåter en användare lösenord toobe återställa eller olåst.

4. REKOMMENDERAS: **”anpassning”** kan du toochange hello ”kontaktar du administratören” länken toopoint tooa sidan eller e-postadress du definiera

5. Valfritt: hello **”registrering”** skärmen ger administratörer hello alternativ för:
    * Kräv användare tooregister när du loggar in
    * Antal dagar innan användarna uppmanas ange tooreconfirm sina autentiseringsinformation

6. Valfritt: hello **”meddelanden”** skärmen ger administratörer hello alternativ för att:
    * Meddela användare om lösenordsåterställning
    * Meddela alla administratörer när andra administratörer återställer sina lösenord

**Nu har du konfigurerat SSPR för din Azure AD-klient**. Du kan stoppa här eller fortsätta med tooconfigure synkronisering av lösenord tooan lokala AD-domän.

> [!NOTE]
> Testa SSPR med en användare och inte en administratör eftersom Microsoft tillämpar starka autentiseringskrav för Azure-konton av administratörstyp. Mer information om Hej administratör lösenordsprincip finns i vår [lösenord princip artikel](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-tooexisting-identity-source"></a>Konfigurera synkroniseringskällan tooexisting identitet

tooenable lokalt identitet synkronisering tooAzure AD, du behöver tooinstall och konfigurera [Azure AD Connect](./connect/active-directory-aadconnect.md) på en server i din organisation. Det här programmet hanterar synkronisera användare och grupper från din befintliga identitet källa tooyour Azure AD-klient.

* [Uppgradera från DirSync eller Azure AD Sync tooAzure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Komma igång med Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurera tillbakaskrivning av lösenord](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite lösenord från Azure AD tillbaka tooyour lokala katalog.

## <a name="disabling-self-service-password-reset"></a>Inaktivera självbetjäningsfunktionen för återställning av lösenord

Inaktivera lösenordsåterställning via självbetjäning är så enkelt som att öppna Azure AD-klienten och gå för**för återställning av lösenord > Egenskaper** > Välj **ingen** under **Self för återställning av lösenord Aktiverad**

### <a name="learn-more"></a>Läs mer
hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten du har lärt dig hur tooconfigure Självbetjäning för återställning av lösenord för dina användare. toocontinue toohello Azure portal toocomplete följer du dessa steg hello länken nedan toohello portal.

> [!div class="nextstepaction"]
> [Aktivera lösenordsåterställning via självbetjäning](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

