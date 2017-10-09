---
title: din inloggning sidan i hello Azure Active Directory aaaCustomize | Microsoft Docs
description: "Lär dig hur tooadd en företagets företagsanpassning toohello Azure inloggning sida"
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
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a>Lägga till företagsanpassning tooyour inloggningssidan i hello Azure Active Directory
tooavoid förvirring vill många företag tooapply ett konsekvent utseende på alla hello webbplatser och tjänster som de hanterar. Azure Active Directory erbjuder den här funktionen genom att låta dig toocustomize hello utseendet på hello-inloggningssida med företagets logotyp och egna färgscheman. hello-inloggningssida är hello sida som visas när du loggar in tooOffice 365 eller andra webbaserade program som använder Azure AD som identitetsprovider. Du interagera med den här sidan tooenter dina autentiseringsuppgifter.

Om du vill tooshow ditt företags varumärke, färger och andra anpassningsbara element på den här sidan, finns i följande bilder toounderstand hello skillnaden mellan två hello-upplevelser hello.

Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **innan** en anpassning:

![Office 365-inloggningssida före anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **när** en anpassning:

![Office 365-inloggningssida efter anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a>Anpassa hello-inloggningssida
Om du behöver webbläsarbaserad åtkomst tooyour molnappar och tjänster som din organisation prenumererar på, använder du normalt hello-inloggningssida.

Om du har tillämpat ändringar tooyour inloggningssidan kan ta det upp tooan timme för hello ändringar tooappear.

En företagsanpassad inloggningssida visas bara när du besöker en tjänst med en URL som är specifik för en klientorganisation, till exempel https://outlook.com/**contoso**.com eller https://mail.**contoso**.com.

När du besöker en tjänst med en URL som inte är specifik för en klientorganisation (t.ex. https://mail.office365.com) visas en icke företagsanpassad inloggningssida. I så fall visas din anpassning först när du har angett ditt användar-ID eller då du har valt en användarikon.

> [!NOTE]
> * Domännamnet måste visas som ”aktiv” i hello **domäner** del av hello Azure-portalen där du har konfigurerat anpassningen. Mer information finns i [lägga till anpassade domännamn](active-directory-domains-add-azure-portal.md).
> * Inloggningssidan företagsanpassning överföra inte toohello konsumenten inloggningssidan för Microsoft. Om du loggar in med ett Microsoft-konto kan du se en företagsanpassad lista över användarikoner som återges av Azure AD, men hello anpassning av din organisation gäller inte toohello Microsoft inloggningssidan.
>
>

På sidan logga in hello **Håll mig inloggad** checkbox tillåter en användare tooremain inloggad när de stänga och öppna webbläsaren igen.

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Den påverkar inte sessionens längd. Du kan dölja hello kryssruta på hello Azure Active Directory-inloggningssida.
Om kryssrutan hello visas beror på hello-inställningen **Håll mig inloggad inaktiverats**.

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/02.png)

toohide Hej kryssrutan, konfigurera den här inställningen för**Ja**.

> [!NOTE]
> Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan toocheck den här rutan. Om du konfigurerar den här inställningen toohidden kan användarna se ytterligare och oväntat prompter toosign i.
>
>

**tooadd företagets företagsanpassning tooyour katalog:**

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. På hello **användare och grupper** bladet väljer **företagets företagsanpassning**.
4. På hello **användare och grupper – företagets anpassning** bladet, Välj hello **redigera** kommando.

    ![Redigera anpassning](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Ändra hello-element som du vill toocustomize. Alla element är valfria.
6. Klicka på **Spara**.

Det kan ta upp tooan timme för alla ändringar du gjort toohello inloggning sidan företagsanpassning tooappear.

## <a name="next-steps"></a>Nästa steg
[Lägga till språkspecifik företagsanpassning](active-directory-branding-localize-azure-portal.md)
