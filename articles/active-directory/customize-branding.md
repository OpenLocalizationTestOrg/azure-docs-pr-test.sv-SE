---
title: din inloggning sidan i hello Azure Active Directory aaaCustomize | Microsoft Docs
description: "Lär dig hur tooadd en företagets företagsanpassning toohello Azure inloggning sida"
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: 7a7ccdeef0764f6cf9e9e224acd4350983031fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-company-branding-tooyour-sign-in-page-in-azure-ad"></a>Snabbstart: Lägga till företagsanpassning tooyour inloggningssidan i Azure AD
tooavoid förvirring vill många företag tooapply ett konsekvent utseende på alla hello webbplatser och tjänster som de hanterar. Azure Active Directory (AD Azure) tillhandahåller den här funktionen genom att låta dig toocustomize hello utseendet på hello-inloggningssida med företagets logotyp och egna färgscheman. hello-inloggningssida är hello sida som visas när du loggar in tooOffice 365 eller andra webbaserade program som använder Azure AD som identitetsprovider. Du interagera med den här sidan tooenter dina autentiseringsuppgifter.

> [!NOTE]
> * Företagsanpassning är bara tillgängligt om du har uppgraderat toohello Premium eller Basic-versionen av Azure AD eller ha en licens för Office 365. toolearn om en funktion som stöds av din licenstypen Kontrollera hello [Azure Active Directory information prissättningssidan](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Azure Active Directory Premium och Basic är tillgängliga för kunder i Kina hello globala instansen av Azure Active Directory. Azure Active Directory Premium och Basic stöds inte för närvarande i hello Microsoft Azure-tjänsten som drivs av 21Vianet i Kina. Mer information kontaktar du oss på hello [Azure Active Directory-forumet](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-hello-sign-in-page"></a>Anpassa hello-inloggningssida

<!--You can customize hello following elements on hello sign-in page: <attach image>-->

Företagets företagsanpassning anpassningar som visas på inloggningssidan för hello Azure AD när användare har åtkomst till en klient-specifika URL som [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

När användare besöker en allmän URL, till exempel www.office.com, innehåller företagsanpassning anpassningar eftersom hello system inte vet vem hello användaren är inte några hello-inloggningssida. Företagsanpassning visas när användare ange sitt användar-ID eller välj en Användarikon.

> [!NOTE]
> * Domännamnet måste visas som ”aktiv” i hello **domäner** del av hello Azure-portalen där du har konfigurerat anpassningen. Mer information finns i [lägga till ett anpassat domännamn](add-custom-domain.md).
> * Inloggningssidan företagsanpassning överföra inte toohello inloggningssidan för personliga Microsoft-konton. Om dina anställda eller business gäster loggar du in med ett personligt microsoftkonto, avspeglar inte deras inloggningssidan hello anpassning av din organisation.


### <a name="banner-logo"></a>Banderollslogotyp 

Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Hej banderollslogotypen visas på hello inloggnings- och hello panelen sidor.<br>Detta visar på hello inloggningssidan, när hello användarens organisation bestäms, vanligtvis efter hello användarnamn har angetts.  | Transparent JPG eller PNG<br>Maximal höjd: 36 px<br>Maximal bredd: 245 px | Använda organisationens logotyp här.<br>Använda en transparent bild. Inte förutsätt att hello bakgrund vit.<br>Lägg inte till utfyllnad kring din logotyp i avbildningen hello eller din logotyp ser oproportionerligt små.

### <a name="username-hint"></a>Ledtråd för användarnamn   
Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Detta anpassar hello instruktionstexten hello användarnamn fältet. | Unicode-text in too64 tecken<br>Endast oformaterad text | Vi rekommenderar att du inte anger detta om du räknar gästanvändare utanför din organisation toosign i tooyour app.
            
### <a name="sign-in-page-text"></a>Text på inloggningssidan   
Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Detta visas längst ned hello hello inloggning form och kan vara används toocommunicate ytterligare information, till exempel hello phone nummer tooyour supportavdelningen eller ett juridiskt meddelande. | Unicode-text in too256 tecken<br>Endast oformaterad text (inga länkar eller HTML-taggar) 

### <a name="sign-in-page-image"></a>Bild av inloggningssidan  
Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Detta visas i hello bakgrund hello-inloggningssida, är förankrad toohello center hello visas utrymme och skala och Beskär toofill hello webbläsarfönster.  <br>Den här bilden visas inte på smala skärmar, till exempel mobiltelefoner.<br>En svart mask med 0,55 opacitet tillämpas via den här avbildningen av vår kod när hello sidan har lästs in. | JPG eller PNG<br>Bild dimensioner: 1 920 x 1 080 bildpunkter<br>Filstorlek: &gt; 300 KB | <br>Använda avbildningar där det inte finns ett starkt ämne fokus. hello täckande inloggning formuläret visas över hello mitten av den här avbildningen och kan omfatta någon del av hello avbildningen beroende på hello storleken på hello webbläsarfönster.<br>Hålla hello filstorlek så mycket som möjligt tooensure snabb inläsningstiden. 

### <a name="background-color"></a>Bakgrundsfärg
Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Den här färgen används i stället för hello bakgrundsbild på anslutningar med låg bandbredd. |   RGB-färg i hexadecimalt (exempel: #FFFFFF | Vi rekommenderar att du använder hello primära färgen i banderollslogotyp hello eller din organisation färg.

### <a name="show-option-tooremain-signed-in"></a>Visa alternativet tooremain inloggad
Beskrivning | Villkor | Rekommendationer
------- | ------- | ----------
Azure AD-logga in ger hello användaren hello alternativet tooremain inloggad när de stänga och öppna webbläsaren igen. Använd den här toohide det alternativet.<br>Ställ in för ”Nej” toohide det här alternativet från användarna. | &nbsp; | Detta påverkar inte sessioners livstid.<br>Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare som kan toochoose tooremain loggat in. Om du ställer in den här toobe dolda kan användarna se ytterligare och oväntat prompter toosign i.

> [!NOTE]
> Alla element är valfria. Om du anger en banderollslogotyp med ingen bakgrundsbild kommer hello-inloggningssida visas din logotyp och hello bakgrundsbild för hello målplatsen (till exempel Office 365).

## <a name="add-company-branding-tooyour-directory"></a>Lägga till företagsanpassning tooyour directory

1. Logga in för[hello Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. På hello **användare och grupper** bladet väljer **företagets företagsanpassning**.
4. På hello **användare och grupper – företagets anpassning** bladet, Välj hello **redigera** kommando.

    ![Redigera anpassning](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Ändra hello-element som du vill toocustomize. Alla element är valfria.
6. Klicka på **Spara**.

Det kan ta upp tooan timme för alla ändringar du gjort toohello inloggning sidan företagsanpassning tooappear.

## <a name="add-language-specific-company-branding-tooyour-directory"></a>Lägga till språkspecifik företagsanpassning tooyour directory

1. Logga in toohello [administrationscentret för Azure AD](https://aad.portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. På hello **användare och grupper** bladet väljer **företagets företagsanpassning**.
4. På hello **användare och grupper – företagets anpassning** bladet, Välj hello **lägga till språk** kommando.

    ![Lägga till språkspecifik företagsanpassning element](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Ändra hello-element som du vill toocustomize. Alla element är valfria.
6. Klicka på **Spara**.

Det kan ta upp tooan timme för alla ändringar du gjort toohello inloggning sidan företagsanpassning tooappear.

## <a name="next-steps"></a>Nästa steg
I den här snabbstarten du har lärt dig hur tooadd företagets företagsanpassning tooyour Azure AD-katalog. 

Du kan använda hello följa länken tooconfigure din företagsanpassning i Azure AD från hello Azure-portalen.

> [!div class="nextstepaction"]
> [Konfigurera varumärkesexponering](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 