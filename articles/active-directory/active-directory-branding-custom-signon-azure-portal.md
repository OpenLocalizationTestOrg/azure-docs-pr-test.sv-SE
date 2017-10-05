---
title: Anpassa inloggningssidan i Azure Active Directory | Microsoft Docs
description: "Lär dig hur du lägger till en företagsanpassning till Azure inloggningssidan"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Lägga till företagsanpassning till inloggningssidan i Azure Active Directory
För att undvika förvirring vill många företag använda ett enhetligt utseende på alla webbplatser och tjänster som de hanterar. Azure Active Directory erbjuder den här funktionen genom att låta dig anpassa utseendet på inloggningssidan med företagets logotyp och egna färgscheman. På inloggningssidan är den sida som visas när du loggar in på Office 365 eller andra webbaserade program som använder Azure AD som identitetsprovider. Du kan interagera med den här sidan om du vill ange dina autentiseringsuppgifter.

Om du vill att ditt företags varumärke, färger och andra anpassningsbara element ska återges på den här sidan tittar du på följande bilder som visar skillnaden mellan de båda upplevelserna.

Följande skärmbild visar ett exempel på Office 365-inloggningssidan på en stationär dator **före** en anpassning:

![Office 365-inloggningssida före anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Följande skärmbild visar ett exempel på Office 365-inloggningssidan på en stationär dator **efter** en anpassning:

![Office 365-inloggningssida efter anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a>Anpassa inloggningssidan
Du använder vanligtvis inloggningssidan om du behöver webbläsarbaserad åtkomst till molnappar och tjänster som din organisation prenumererar på.

Om du har gjort ändringar på inloggningssidan kan det ta upp till en timme innan ändringarna visas.

En företagsanpassad inloggningssida visas bara när du besöker en tjänst med en URL som är specifik för en klientorganisation, till exempel https://outlook.com/**contoso**.com eller https://mail.**contoso**.com.

När du besöker en tjänst med en URL som inte är specifik för en klientorganisation (t.ex. https://mail.office365.com) visas en icke företagsanpassad inloggningssida. I så fall visas din anpassning först när du har angett ditt användar-ID eller då du har valt en användarikon.

> [!NOTE]
> * Domännamnet måste visas som ”aktiv” i den **domäner** del av Azure-portalen där du har konfigurerat anpassningen. Mer information finns i [lägga till anpassade domännamn](active-directory-domains-add-azure-portal.md).
> * Anpassningen av inloggningssidan överförs inte till Microsofts konsumentinloggningssida. Om du loggar in med ett Microsoft-konto kan du se en företagsanpassad lista över användarikoner som återges av Azure AD, men anpassning av din organisation inte gäller för Microsoft-konto-inloggningssidan.
>
>

På inloggningssidan gör kryssrutan **Håll mig inloggad** att användaren kan förbli inloggad när de stänger och öppnar webblösaren.

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Den påverkar inte sessionens längd. Du kan dölja kryssrutan på inloggningssidan för Azure Active Directory.
Om kryssrutan visas beror på inställningen för **Håll mig inloggad inaktiverats**.

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/02.png)

Konfigurera den här inställningen om du vill dölja kryssrutan **Ja**.

> [!NOTE]
> För vissa funktioner i SharePoint Online och Office 2010 måste användarna kunna markera den här kryssrutan. Om du konfigurerar den här inställningen som dold kan eventuellt ytterligare och oväntade uppmaningar att logga in visas för dina användare.
>
>

**Lägga till företagsanpassning till din katalog:**

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. På den **användare och grupper** bladet väljer **företagets företagsanpassning**.
4. På den **användare och grupper – företagets anpassning** bladet väljer den **redigera** kommando.

    ![Redigera anpassning](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Ändra de element som du vill anpassa. Alla element är valfria.
6. Klicka på **Spara**.

Det kan ta upp till en timme innan ändringarna inloggningssidan företagsanpassning ska visas.

## <a name="next-steps"></a>Nästa steg
[Lägga till språkspecifik företagsanpassning](active-directory-branding-localize-azure-portal.md)
