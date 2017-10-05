---
title: "Registrera dig för Office 365 med Azure-konto | Microsoft Docs"
description: "Lär dig hur du skapar en prenumeration på Office 365 med ett Azure-konto"
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
ms.openlocfilehash: 1c6e277e321980aaf30f821dbb41c7eaf296b4cf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a><span data-ttu-id="ff71b-103">Registrera dig för Office 365-prenumeration med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="ff71b-103">Sign up for an Office 365 subscription with your Azure account</span></span>
<span data-ttu-id="ff71b-104">Om du använder Azure-prenumerant kan använda du ditt Azure-konto du registrerar dig för en prenumeration på Office 365.</span><span class="sxs-lookup"><span data-stu-id="ff71b-104">If you're Azure subscriber, you can use your Azure account to sign up for an Office 365 subscription.</span></span> <span data-ttu-id="ff71b-105">Om du är en del av en organisation som har en Azure-prenumeration kan skapa du Office 365-prenumerationer för användare i din befintliga Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ff71b-105">If you're part of an organization that has an Azure subscription, you can create Office 365 subscriptions for users in your existing Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ff71b-106">Logga på Office 365 med ett konto som har behörighet som Global administratör eller fakturering administratör i din Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="ff71b-106">Sign up to Office 365 using an account that has Global Admin or Billing Admin permissions in your Azure Active Directory tenant.</span></span> <span data-ttu-id="ff71b-107">Mer information finns i [Kontrollera min behörighet i Azure AD](#RoleInAzureAD) och [Tilldela administratörsroller i Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ff71b-107">For more information, see [Check my account permissions in Azure AD](#RoleInAzureAD) and [Assigning administrator roles in Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="ff71b-108">Om du redan har både ett Office 365-konto och en Azure-prenumeration kan du [associera en Office 365-klient till en Azure-prenumeration](billing-add-office-365-tenant-to-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="ff71b-108">If you already have both an Office 365 account and an Azure subscription, you can [Associate an Office 365 tenant to an Azure subscription](billing-add-office-365-tenant-to-azure-subscription.md).</span></span>

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a><span data-ttu-id="ff71b-109">Hämta en Office 365-prenumeration med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="ff71b-109">Get an Office 365 subscription by using your Azure account</span></span>

1. <span data-ttu-id="ff71b-110">Gå till den [produktsidan för Office 365](https://products.office.com/business), och välj en plan.</span><span class="sxs-lookup"><span data-stu-id="ff71b-110">Go to the [Office 365 product page](https://products.office.com/business), and select a plan.</span></span>
2. <span data-ttu-id="ff71b-111">Klicka på **logga in** på det övre högra hörnet på sidan.</span><span class="sxs-lookup"><span data-stu-id="ff71b-111">Click **Sign in** on the upper-right corner of the page.</span></span>

    ![Skärmbild av Office 365-utvärderingsversion sidan](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. <span data-ttu-id="ff71b-113">Logga in med dina Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ff71b-113">Sign in with your Azure account credentials.</span></span> <span data-ttu-id="ff71b-114">Om du skapar en prenumeration för din organisation kan du använda ett Azure-konto som är medlem i rollen Global administratör eller fakturering Admin katalog i Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="ff71b-114">If you're creating a subscription for your organization, use an Azure account that's a member of the Global Admin or Billing Admin directory role in your Azure Active Directory tenant.</span></span>

    ![Skärmbild av Office 365-inloggning](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. <span data-ttu-id="ff71b-116">Klicka på **försöker nu**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-116">Click **Try now**.</span></span>

    ![Skärmbild som bekräftar din order för Office 365.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. <span data-ttu-id="ff71b-118">På sidan ordning inleverans **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-118">On the order receipt page, click **Continue**.</span></span>

    ![Skärmbild av Office 365 inleverans](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

<span data-ttu-id="ff71b-120">Nu är installeras allt klart.</span><span class="sxs-lookup"><span data-stu-id="ff71b-120">Now you're all set.</span></span> <span data-ttu-id="ff71b-121">Om du har skapat Office 365-prenumeration för din organisation kan du använda följande steg för att kontrollera att Azure AD-användare är nu i Office 365.</span><span class="sxs-lookup"><span data-stu-id="ff71b-121">If you created the Office 365 subscription for your organization, use the following steps to check that your Azure AD users are now in Office 365.</span></span>

1. <span data-ttu-id="ff71b-122">Öppna administrationscentret för Office 365.</span><span class="sxs-lookup"><span data-stu-id="ff71b-122">Open the Office 365 admin center.</span></span>
2. <span data-ttu-id="ff71b-123">Expandera **användare**, och klicka sedan på **aktiva användare**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-123">Expand **USERS**, and then click **Active Users**.</span></span>

    ![Skärmbild av Office 365 admin center-användare](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

<span data-ttu-id="ff71b-125">När du registrerar dig läggs Office 365-prenumeration i samma Azure Active Directory-instans som tillhör din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ff71b-125">After you sign up, the Office 365 subscription is added to the same Azure Active Directory instance that your Azure subscription belongs to.</span></span> <span data-ttu-id="ff71b-126">Mer information finns i [mer om Azure och Office 365-prenumerationer](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) och [hur Azure-prenumerationer är associerade med Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ff71b-126">For more information, see [More about Azure and Office 365 subscriptions](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) and [How Azure subscriptions are associated with Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <span data-ttu-id="ff71b-127"><a id="RoleInAzureAD"></a>Kontrollera min behörighet i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff71b-127"><a id="RoleInAzureAD"></a>Check my account permissions in Azure AD</span></span>
1. <span data-ttu-id="ff71b-128">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ff71b-128">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="ff71b-129">Klicka på **fler tjänster**, och sök sedan efter **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-129">Click **More services**, and then search for **Active Directory**.</span></span>

    ![Skärmbild av Active Directory i Azure-portalen](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. <span data-ttu-id="ff71b-131">Klicka på **användare och grupper** > **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-131">Click **Users and groups** > **All users**.</span></span>
4. <span data-ttu-id="ff71b-132">Välj användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="ff71b-132">Select the user name.</span></span> 

    ![Skärmbild som visar de Azure Active Directory-användarna](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. <span data-ttu-id="ff71b-134">Klicka på **Directory rollen**.</span><span class="sxs-lookup"><span data-stu-id="ff71b-134">Click **Directory role**.</span></span>
  
    ![Skärmbild som visar rollen Azure portal directory](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  <span data-ttu-id="ff71b-136">Rollen **Global administratör** eller **begränsad administratör** > **faktureringsadministratör** krävs för att skapa en prenumeration på Office 365 för användare i en befintlig Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ff71b-136">The role **Global administrator** or **Limited administrator** > **Billing administrator** is required to create an Office 365 subscription for users in your existing Azure Active Directory.</span></span>

    ![Skärmbild som visar Azure portal directory rollen fakturering Admin](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="ff71b-138">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="ff71b-138">Need help?</span></span> <span data-ttu-id="ff71b-139">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="ff71b-139">Contact support.</span></span>
<span data-ttu-id="ff71b-140">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="ff71b-140">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span> 
