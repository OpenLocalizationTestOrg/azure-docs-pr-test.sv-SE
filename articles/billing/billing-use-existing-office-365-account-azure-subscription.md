---
title: "aaaSign för Azure med Office 365-konto | Microsoft Docs"
description: "Lär dig hur toocreate en Azure-prenumeration med hjälp av Office 365-konto"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 129cdf7a-2165-483d-83e4-8f11f0fa7f8b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: cc331bf7222b5b03e740cb40a214bc13ef585f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-up-for-an-azure-subscription-with-your-office-365-account"></a>Registrera dig för en Azure-prenumeration med ditt Office 365-konto
Om du har en Office 365-prenumeration kan använda du en Azure-prenumeration för toocreate din Office 365-konto. Logga in toohello [Azure-portalen](https://portal.azure.com/) med ditt Office 365-användarnamn och lösenord. Om du vill tooset av virtuella datorer eller använda andra Azure-tjänster måste du registrera dig för en Azure-prenumeration. Du kan dela din Azure-prenumeration med andra och [använda rollbaserad åtkomstkontroll toomanage åtkomst tooyour Azure-prenumeration och resurser](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)

Om du redan har både ett Office 365-konto och en Azure-prenumeration finns [associera en Azure-prenumeration med Office 365-klient tooan](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-azure-subscription-using-your-office-365-account"></a>Hämta en Azure-prenumeration med ditt Office 365-konto

Spara tid och undvika konto spridning genom att registrera dig för Azure med ditt Office 365-användarnamn och lösenord. 

1. Registrera dig på [Azure.com](https://account.azure.com/signup?offer=MS-AZR-0044p&appId=docs). 
2. Logga in med ditt Office 365-användarnamn och lösenord. hello-konto som du använder behöver inte toohave administratörsbehörigheter. Om du har mer än en Office 365-konto kan du kontrollera att du använder hello autentiseringsuppgifter för hello Office 365-konto som du vill tooassociate med din Azure-prenumeration. 

   ![Skärmbild som visar hello-inloggningssida.](./media/billing-use-existing-office-365-account-azure-subscription/billing-sign-in-with-office-365-account.png)

3. Ange hello krävs information och fullständiga hello registreringsprocessen. Viss information kan inte krävas om du redan har en Office 365-konto.

    ![Skärmbild som visar hello registreringsformuläret.](./media/billing-use-existing-office-365-account-azure-subscription/billing-azure-sign-up-fill-information.png)

- Om du behöver tooadd andra personer i din organisation toohello Azure-prenumeration går [Kom igång med åtkomsthantering i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md). 

## <a id="more-about-subs">Mer information om Azure och Office 365-prenumerationer</a>
Office 365 och Azure använder hello Azure AD-tjänsten toomanage-användare och -prenumerationer. hello Azure directory fungerar som en behållare där du kan gruppera användare och -prenumerationer. toouse hello samma användarkonton för dina Azure och Office 365-prenumerationer måste du toomake till att hello Azure prenumerationer skapas i hello samma katalog som hello Office 365-prenumerationer. Kom ihåg hello följande punkter:

* En prenumeration skapas under en katalog
* Användarna tillhör toodirectories
* En prenumeration hamnar i hello directory för hello-användare som skapar hello prenumeration. Så att din prenumeration på Office 365 är bundet toohello konto samma som din Azure-prenumeration.
* Azure-prenumerationer som ägs av enskilda användare i hello directory
* Office 365-prenumerationer ägs av själva hello-katalogen. Användare med rätt behörighet för hello i hello katalog kan hantera dessa prenumerationer.

![Skärmbild som visar hello relationen mellan hello directory, användare och prenumerationer.](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Mer information finns i [hur Azure-prenumerationer är associerade med Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt. 
