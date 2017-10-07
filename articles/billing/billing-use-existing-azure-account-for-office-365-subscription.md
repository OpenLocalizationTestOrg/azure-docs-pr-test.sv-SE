---
title: "aaaSign för Office 365 med Azure-konto | Microsoft Docs"
description: "Lär dig hur toocreate en Office 365-prenumeration med hjälp av ett Azure-konto"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: d19e1c1edff0b9658b639e796a72bbf4e87b9c3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Registrera dig för Office 365-prenumeration med ditt Azure-konto
Om du använder Azure-prenumerant kan använda du din Azure-konto toosign för Office 365-prenumeration. Om du är en del av en organisation som har en Azure-prenumeration kan skapa du Office 365-prenumerationer för användare i din befintliga Azure Active Directory (AD Azure). Registrera dig tooOffice 365 med ett konto som har behörighet som Global administratör eller fakturering administratör i din Azure Active Directory-klient. Mer information finns i [Kontrollera min behörighet i Azure AD](#RoleInAzureAD) och [Tilldela administratörsroller i Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

Om du redan har både ett Office 365-konto och en Azure-prenumeration kan du [associera en Azure-prenumeration med Office 365-klient tooan](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Hämta en Office 365-prenumeration med ditt Azure-konto

1. Gå toohello [produktsidan för Office 365](https://products.office.com/business), och välj en plan.
2. Klicka på **logga in** på hello övre högra hörnet av hello-sidan.

    ![Skärmbild av Office 365-utvärderingsversion sidan](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Logga in med dina Azure-konto. Om du skapar en prenumeration för din organisation kan du använda ett Azure-konto som är medlem i rollen för hello-Global administratör eller fakturering Admin katalog i Azure Active Directory-klient.

    ![Skärmbild av Office 365-inloggning](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Klicka på **försöker nu**.

    ![Skärmbild som bekräftar din order för Office 365.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Hello ordning mottagande sidan, klicka på **Fortsätt**.

    ![Skärmbild av inleverans hello Office 365](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Nu är installeras allt klart. Om du har skapat hello Office 365-prenumeration för din organisation Använd hello följande steg toocheck som Azure AD-användare är nu i Office 365.

1. Öppna hello Office 365 Administrationscenter.
2. Expandera **användare**, och klicka sedan på **aktiva användare**.

    ![Skärmbild av hello Office 365 admin center användare](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

När du registrerar dig hello Office 365-prenumeration läggs toohello samma Azure Active Directory-instans som tillhör din Azure-prenumeration. Mer information finns i [mer om Azure och Office 365-prenumerationer](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) och [hur Azure-prenumerationer är associerade med Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Kontrollera min behörighet i Azure AD
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **fler tjänster**, och sök sedan efter **Active Directory**.

    ![Skärmbild av Active Directory i hello Azure-portalen](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Klicka på **användare och grupper** > **alla användare**.
4. Välj hello användarnamn. 

    ![Skärmbild som visar hello Azure Active Directory-användare](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Klicka på **Directory rollen**.
  
    ![Skärmbild som visar hello Azure portal directory roll](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  hello rollen **Global administratör** eller **begränsad administratör** > **faktureringsadministratör** är obligatoriska toocreate Office 365-prenumeration för användare i din befintliga Azure Active Directory.

    ![Skärmbild som visar Azure portal directory rollen fakturering Admin](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt. 
