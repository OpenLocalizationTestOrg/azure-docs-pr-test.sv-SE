---
title: "Lägga till nya användare i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur du lägger till nya användare eller ändrar användarinformation i Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: ff4b742e772a6062885313e9bb49e55907fe125a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-to-azure-active-directory"></a><span data-ttu-id="a25cd-103">Lägga till nya användare eller användare med Microsoft-konton i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a25cd-103">Add new users or users with Microsoft accounts to Azure Active Directory</span></span>
<span data-ttu-id="a25cd-104">Lägg till användare i din katalog.</span><span class="sxs-lookup"><span data-stu-id="a25cd-104">Add users to populate your directory.</span></span> <span data-ttu-id="a25cd-105">Den här artikeln förklarar hur du lägger till nya användare i din organisation och hur du lägger till användare som har Microsoft-konton.</span><span class="sxs-lookup"><span data-stu-id="a25cd-105">This article explains how to add new users in your organization, and how to add users who have Microsoft accounts.</span></span> <span data-ttu-id="a25cd-106">Mer information om hur du lägger till användare från andra kataloger i Azure Active Directory eller hur du lägger till användare från partnerföretag finns i [Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="a25cd-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="a25cd-107">Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller till dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="a25cd-107">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a25cd-108">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a25cd-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="a25cd-109">Hur du lägger till en användare i Azure AD-administrationscentret finns [lägga till nya användare i Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a25cd-109">For how to add a user in the Azure AD admin center, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="a25cd-110">Lägga till en användare</span><span class="sxs-lookup"><span data-stu-id="a25cd-110">Add a user</span></span>
1. <span data-ttu-id="a25cd-111">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="a25cd-111">Sign in to the [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="a25cd-112">Välj **Active Directory** och välj sedan namnet på din organisationskatalog.</span><span class="sxs-lookup"><span data-stu-id="a25cd-112">Select **Active Directory**, and then select the name of your organization directory.</span></span>
3. <span data-ttu-id="a25cd-113">Välj fliken **Användare** och sedan **Lägg till användare** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="a25cd-113">Select the **Users** tab, and then, in the command bar, select **Add User**.</span></span>
4. <span data-ttu-id="a25cd-114">På sidan **Berätta om den här användaren** under **Typ av användare** väljer du antingen:</span><span class="sxs-lookup"><span data-stu-id="a25cd-114">On the **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="a25cd-115">**Ny användare i din organisation** – Lägger till ett nytt konto till din katalog.</span><span class="sxs-lookup"><span data-stu-id="a25cd-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="a25cd-116">**Användare med ett befintligt Microsoft-konto** – Lägger till ett befintligt Microsoft-konsumentkonto till din katalog (till exempel ett Outlook-konto)</span><span class="sxs-lookup"><span data-stu-id="a25cd-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account to your directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="a25cd-117">Beroende på **typen av användare** anger du ett användarnamn (för en ny användare) eller en e-postadress (för en användare med ett Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="a25cd-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="a25cd-118">På användarens **profilsida** anger du ett för- och efternamn, ett användarvänligt namn och en användarroll från listan **Roller**.</span><span class="sxs-lookup"><span data-stu-id="a25cd-118">On the user **Profile** page, provide a first and last name, a user-friendly name, and a user role from the **Roles** list.</span></span> <span data-ttu-id="a25cd-119">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a25cd-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="a25cd-120">Ange om du vill **aktivera Multi-Factor Authentication** för användaren.</span><span class="sxs-lookup"><span data-stu-id="a25cd-120">Specify whether to **Enable Multi-Factor Authentication** for the user.</span></span>
7. <span data-ttu-id="a25cd-121">På sidan **Skaffa tillfälligt lösenord** väljer du **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a25cd-121">On the **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a25cd-122">Om din organisation använder mer än en domän bör du vara medveten om följande problem när du lägger till ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="a25cd-122">If your organization uses more than one domain, you should know about the following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="a25cd-123">Om du vill lägga till användarkonton med samma UPN (användarens huvudnamn) i olika domäner lägger du **först** till exempelvis geoffgrisso@contoso.onmicrosoft.com, **följt av** geoffgrisso@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a25cd-123">TO add user accounts with the same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="a25cd-124">Lägg **inte** till geoffgrisso@contoso.com innan du lägger till geoffgrisso@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="a25cd-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com.</span></span> <span data-ttu-id="a25cd-125">Den här ordningen är viktig och kan vara krånglig att ångra.</span><span class="sxs-lookup"><span data-stu-id="a25cd-125">This order is important, and can be cumbersome to undo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="a25cd-126">Ändra användarinformation</span><span class="sxs-lookup"><span data-stu-id="a25cd-126">Change user information</span></span>
<span data-ttu-id="a25cd-127">Du kan ändra valfria användarattribut utom objekt-ID:t.</span><span class="sxs-lookup"><span data-stu-id="a25cd-127">You can change any user attribute except for the object ID.</span></span>

1. <span data-ttu-id="a25cd-128">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="a25cd-128">Open your directory.</span></span>
2. <span data-ttu-id="a25cd-129">Välj fliken **Användare** och välj sedan visningsnamnet för den användare som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="a25cd-129">Select the **Users** tab, and then select the display name of the user you want to change.</span></span>
3. <span data-ttu-id="a25cd-130">Slutför ändringarna och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a25cd-130">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="a25cd-131">Om användaren som du ändrar synkroniseras med din lokala Active Directory-tjänst kan du inte ändra användarinformationen med den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="a25cd-131">If the user that you're changing is synchronized with your on-premises Active Directory service, you can't change the user information using this procedure.</span></span> <span data-ttu-id="a25cd-132">Om du vill ändra användaren använder du dina lokala Active Directory-hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="a25cd-132">To change the user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="a25cd-133">Hantering av och begränsningar för gästanvändare</span><span class="sxs-lookup"><span data-stu-id="a25cd-133">Guest user management and limitations</span></span>
<span data-ttu-id="a25cd-134">Gästkonton är användare från andra kataloger som bjudits in till din katalog för att få åtkomst till SharePoint-dokument, program eller andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a25cd-134">Guest accounts are users from other directories who were invited to your directory to access SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="a25cd-135">Det underliggande UserType-attributet för ett gästkonto i din katalog är ”Gäst”.</span><span class="sxs-lookup"><span data-stu-id="a25cd-135">A guest account in your directory has its underlying UserType attribute set to "Guest."</span></span> <span data-ttu-id="a25cd-136">Vanliga användare (särskilt medlemmar i din katalog) har UserType-attributet ”Medlem”.</span><span class="sxs-lookup"><span data-stu-id="a25cd-136">Regular users (specifically, members of your directory) have the UserType attribute "Member."</span></span>

<span data-ttu-id="a25cd-137">Gäster har en begränsad uppsättning rättigheter i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a25cd-137">Guests have a limited set of rights in the directory.</span></span> <span data-ttu-id="a25cd-138">Dessa rättigheter begränsar möjligheten för gäster att identifiera information om andra användare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a25cd-138">These rights limit the ability for Guests to discover information about other users in the directory.</span></span> <span data-ttu-id="a25cd-139">Gästanvändare kan dock fortfarande interagera med användare och grupper som är associerade med de resurser som de arbetar med.</span><span class="sxs-lookup"><span data-stu-id="a25cd-139">However, guest users can still interact with the users and groups associated with the resources they're working on.</span></span> <span data-ttu-id="a25cd-140">Gästanvändare kan:</span><span class="sxs-lookup"><span data-stu-id="a25cd-140">Guest users can:</span></span>

* <span data-ttu-id="a25cd-141">Visa andra användare och grupper som är associerade med en Azure-prenumeration som de tilldelats.</span><span class="sxs-lookup"><span data-stu-id="a25cd-141">See other users and groups associated with an Azure subscription to which they're assigned</span></span>
* <span data-ttu-id="a25cd-142">Visa medlemmarna i grupper som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="a25cd-142">See the members of groups to which they belong</span></span>
* <span data-ttu-id="a25cd-143">Leta upp andra användare i katalogen om de känner till användarens fullständiga e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a25cd-143">Look up other users in the directory, if they know the full email address of the user</span></span>
* <span data-ttu-id="a25cd-144">Visa en begränsad uppsättning attribut för de användare som de letar upp, begränsat till visningsnamn, e-postadress, användarens huvudnamn (UPN) och miniatyrfoto.</span><span class="sxs-lookup"><span data-stu-id="a25cd-144">See only a limited set of attributes of the users they look up--limited to display name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="a25cd-145">Hämta en lista över verifierade domäner i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a25cd-145">Get a list of verified domains in the directory</span></span>
* <span data-ttu-id="a25cd-146">Samtycka till program som ger dem samma åtkomst som medlemmar har i din katalog.</span><span class="sxs-lookup"><span data-stu-id="a25cd-146">Consent to applications, granting them the same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="a25cd-147">Ange åtkomstprinciper för gästanvändare</span><span class="sxs-lookup"><span data-stu-id="a25cd-147">Set guest user access policies</span></span>
<span data-ttu-id="a25cd-148">Fliken **Konfigurera** för en katalog innehåller alternativ som du kan använda för att kontrollera åtkomsten för gästanvändare.</span><span class="sxs-lookup"><span data-stu-id="a25cd-148">The **Configure** tab of a directory includes options to control access for guest users.</span></span> <span data-ttu-id="a25cd-149">Dessa alternativ kan bara ändras på den klassiska Azure-portalen av en global katalogadministratör.</span><span class="sxs-lookup"><span data-stu-id="a25cd-149">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="a25cd-150">För närvarande finns det ingen PowerShell- eller API-metod.</span><span class="sxs-lookup"><span data-stu-id="a25cd-150">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="a25cd-151">Öppna fliken **Konfigurera** på den klassiska Azure-portalen genom att välja **Active Directory** och sedan namnet på katalogen.</span><span class="sxs-lookup"><span data-stu-id="a25cd-151">To open the **Configure** tab in the Azure classic portal, select **Active Directory**, and then select the name of the directory.</span></span>

![Konfigurera fliken i Azure Active Directory][1]

<span data-ttu-id="a25cd-153">Sedan kan du redigera alternativen för att kontrollera gästanvändarnas åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a25cd-153">Then you can edit the options to control access for guest users.</span></span>

![Alternativ för åtkomstkontroll för gästanvändare][2]

## <a name="whats-next"></a><span data-ttu-id="a25cd-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a25cd-155">What's next</span></span>
* [<span data-ttu-id="a25cd-156">Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a25cd-156">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="a25cd-157">Administrera Azure AD</span><span class="sxs-lookup"><span data-stu-id="a25cd-157">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="a25cd-158">Hantera lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a25cd-158">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="a25cd-159">Hantera grupper i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a25cd-159">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
