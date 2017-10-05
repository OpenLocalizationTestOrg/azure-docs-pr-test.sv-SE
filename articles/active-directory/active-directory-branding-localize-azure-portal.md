---
title: "Lägga till språkspecifik företagsanpassning till inloggningssidan i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du lägger till ett visst språk företag bilder och text till en Azure-inloggningssida"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Lägga till språkspecifik företagsanpassning till inloggningssidan i Azure Active Directory
För att undvika förvirring vill många företag använda ett enhetligt utseende på alla webbplatser och tjänster som de hanterar. Azure Active Directory erbjuder den här funktionen genom att låta dig anpassa utseendet på inloggningssidan med företagets logotyp och egna färgscheman. På inloggningssidan är den sida som visas när du loggar in på Office 365 eller andra webbaserade program som använder Azure AD som identitetsprovider. Du kan interagera med den här sidan om du vill ange dina autentiseringsuppgifter.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Anpassa inloggningssidan för ett annat språk
Du kan lägga till språkspecifik element din anpassade inloggningssidan endast om du redan har skapat en anpassad inloggningssida som beskrivs i [lägga till företagsprofilering för din inloggningssidan](active-directory-branding-custom-signon-azure-portal.md). Du kan konfigurera en inloggningssida per katalog med en standarduppsättning med anpassningsbara element. När du har konfigurerat en standarduppsättning av sidelement kan konfigurera du fler versioner för olika språk. Du kan också blanda och matcha olika element. Du kan till exempel:

* Skapa en standard **inloggning sidan avbildningen** som passar alla kulturer och sedan skapa specifika versioner för engelska och franska. När du ställer in din webbläsare till någon av dessa språk språkspecifika bilden visas, medan standardbilden visas för andra språk.
* Konfigurera olika logotyper för din organisation (till exempel japanska och hebreiska versioner).

Vi rekommenderar att du behåller antal språk variationer låg för underhåll och prestandaskäl.

**Lägga till företagsanpassning till din katalog:**

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. På den **användare och grupper** bladet väljer **företagets företagsanpassning**.
4. På den **användare och grupper – företagets anpassning** bladet väljer den **lägga till språk** kommando.

    ![Lägga till språkspecifik företagsanpassning element](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Ändra de element som du vill anpassa. Alla element är valfria.
6. Klicka på **Spara**.

Det kan ta upp till en timme innan ändringarna inloggningssidan företagsanpassning ska visas.

## <a name="next-steps"></a>Nästa steg
[Lägga till företagsanpassning till din inloggningssida](active-directory-branding-custom-signon-azure-portal.md)
