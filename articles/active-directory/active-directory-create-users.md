---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare eller ändrar användarinformation i Azure Active Directory."
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
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a><span data-ttu-id="f2b4f-103">Lägga till nya användare eller användare med Microsoft-konton tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2b4f-103">Add new users or users with Microsoft accounts tooAzure Active Directory</span></span>
<span data-ttu-id="f2b4f-104">Lägg till användare toopopulate din katalog.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-104">Add users toopopulate your directory.</span></span> <span data-ttu-id="f2b4f-105">Den här artikeln förklarar hur tooadd nya användare i din organisation och hur tooadd användare som har Microsoft-konton.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-105">This article explains how tooadd new users in your organization, and how tooadd users who have Microsoft accounts.</span></span> <span data-ttu-id="f2b4f-106">Mer information om hur du lägger till användare från andra kataloger i Azure Active Directory eller hur du lägger till användare från partnerföretag finns i [Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="f2b4f-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="f2b4f-107">Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller toothem när som helst.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-107">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2b4f-108">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="f2b4f-109">För hur tooadd en användare i administrationscentret för hello Azure AD, se [lägga till nya användare tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f2b4f-109">For how tooadd a user in hello Azure AD admin center, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="f2b4f-110">Lägga till en användare</span><span class="sxs-lookup"><span data-stu-id="f2b4f-110">Add a user</span></span>
1. <span data-ttu-id="f2b4f-111">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-111">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="f2b4f-112">Välj **Active Directory**, och välj sedan hello namnet på din organisationskatalog.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-112">Select **Active Directory**, and then select hello name of your organization directory.</span></span>
3. <span data-ttu-id="f2b4f-113">Välj hello **användare** fliken och markera i kommandofältet hello **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-113">Select hello **Users** tab, and then, in hello command bar, select **Add User**.</span></span>
4. <span data-ttu-id="f2b4f-114">På hello **berätta om den här användaren** sidan under **typ av användare**, väljer du antingen:</span><span class="sxs-lookup"><span data-stu-id="f2b4f-114">On hello **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="f2b4f-115">**Ny användare i din organisation** – Lägger till ett nytt konto till din katalog.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="f2b4f-116">**Användare med ett befintligt microsoftkonto** – lägger till en befintlig Microsoft konsumenten konto tooyour katalog (till exempel ett Outlook-konto)</span><span class="sxs-lookup"><span data-stu-id="f2b4f-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account tooyour directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="f2b4f-117">Beroende på **typen av användare** anger du ett användarnamn (för en ny användare) eller en e-postadress (för en användare med ett Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="f2b4f-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="f2b4f-118">Hello användaren **profil** anger du ett första och sista namn, ett användarvänligt namn och en användarroll från hello **roller** lista.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-118">On hello user **Profile** page, provide a first and last name, a user-friendly name, and a user role from hello **Roles** list.</span></span> <span data-ttu-id="f2b4f-119">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f2b4f-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="f2b4f-120">Ange om för**aktivera Multifaktorautentisering** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-120">Specify whether too**Enable Multi-Factor Authentication** for hello user.</span></span>
7. <span data-ttu-id="f2b4f-121">På hello **skaffa tillfälligt lösenord** väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-121">On hello **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2b4f-122">Om din organisation använder mer än en domän, bör du känna till om hello följande problem när du lägger till ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="f2b4f-122">If your organization uses more than one domain, you should know about hello following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="f2b4f-123">tooadd användarkonton med hello samma användarens huvudnamn (UPN) över domäner, **första** lägga till, till exempel geoffgrisso@contoso.onmicrosoft.com, **följt av** geoffgrisso@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-123">tooadd user accounts with hello same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="f2b4f-124">Lägg **inte** till geoffgrisso@contoso.com innan du lägger till geoffgrisso@contoso.onmicrosoft.com. Den här ordningen är viktig och kan vara komplicerade tooundo.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com. This order is important, and can be cumbersome tooundo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="f2b4f-125">Ändra användarinformation</span><span class="sxs-lookup"><span data-stu-id="f2b4f-125">Change user information</span></span>
<span data-ttu-id="f2b4f-126">Du kan ändra valfria användarattribut utom hello objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-126">You can change any user attribute except for hello object ID.</span></span>

1. <span data-ttu-id="f2b4f-127">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-127">Open your directory.</span></span>
2. <span data-ttu-id="f2b4f-128">Välj hello **användare** fliken och markera sedan hello visningsnamn hello användare som du vill toochange.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-128">Select hello **Users** tab, and then select hello display name of hello user you want toochange.</span></span>
3. <span data-ttu-id="f2b4f-129">Slutför ändringarna och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-129">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="f2b4f-130">Du kan inte ändra hello användarinformationen med den här proceduren om hello-användare som du ändrar synkroniseras med din lokala Active Directory-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-130">If hello user that you're changing is synchronized with your on-premises Active Directory service, you can't change hello user information using this procedure.</span></span> <span data-ttu-id="f2b4f-131">toochange hello användare kan använda din lokala Active Directory-hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-131">toochange hello user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="f2b4f-132">Hantering av och begränsningar för gästanvändare</span><span class="sxs-lookup"><span data-stu-id="f2b4f-132">Guest user management and limitations</span></span>
<span data-ttu-id="f2b4f-133">Gästkonton är användare från andra kataloger som var inbjudna tooyour directory tooaccess SharePoint-dokument, program eller andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-133">Guest accounts are users from other directories who were invited tooyour directory tooaccess SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="f2b4f-134">Ett gästkonto i din katalog har dess underliggande UserType-attributet för ”gäst”.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-134">A guest account in your directory has its underlying UserType attribute set too"Guest."</span></span> <span data-ttu-id="f2b4f-135">Vanliga användare (särskilt medlemmar i din katalog) har hello UserType-attributet ”medlem”.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-135">Regular users (specifically, members of your directory) have hello UserType attribute "Member."</span></span>

<span data-ttu-id="f2b4f-136">Gäster har en begränsad uppsättning rättigheter i hello directory.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-136">Guests have a limited set of rights in hello directory.</span></span> <span data-ttu-id="f2b4f-137">Dessa rättigheter begränsar hello möjligheten för gäster toodiscover information om andra användare i hello directory.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-137">These rights limit hello ability for Guests toodiscover information about other users in hello directory.</span></span> <span data-ttu-id="f2b4f-138">Gästanvändare kan dock fortfarande interagera med hello användare och grupper som är associerade med hello resurser som de arbetar med.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-138">However, guest users can still interact with hello users and groups associated with hello resources they're working on.</span></span> <span data-ttu-id="f2b4f-139">Gästanvändare kan:</span><span class="sxs-lookup"><span data-stu-id="f2b4f-139">Guest users can:</span></span>

* <span data-ttu-id="f2b4f-140">Visa andra användare och grupper som är associerade med en Azure-prenumeration toowhich som de tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-140">See other users and groups associated with an Azure subscription toowhich they're assigned</span></span>
* <span data-ttu-id="f2b4f-141">Se hello medlemmar i grupper toowhich som de tillhör</span><span class="sxs-lookup"><span data-stu-id="f2b4f-141">See hello members of groups toowhich they belong</span></span>
* <span data-ttu-id="f2b4f-142">Leta upp andra användare i hello directory, om de redan känner till hello hello användarens fullständiga e-postadress</span><span class="sxs-lookup"><span data-stu-id="f2b4f-142">Look up other users in hello directory, if they know hello full email address of hello user</span></span>
* <span data-ttu-id="f2b4f-143">Finns bara en begränsad uppsättning attribut för hello användare de letar upp, begränsat toodisplay namn, e-postadress, användarens huvudnamn (UPN) och miniatyrfoto.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-143">See only a limited set of attributes of hello users they look up--limited toodisplay name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="f2b4f-144">Hämta en lista över verifierade domäner i hello directory</span><span class="sxs-lookup"><span data-stu-id="f2b4f-144">Get a list of verified domains in hello directory</span></span>
* <span data-ttu-id="f2b4f-145">Medgivande tooapplications, ger dem hello samma åtkomst som medlemmar har i din katalog</span><span class="sxs-lookup"><span data-stu-id="f2b4f-145">Consent tooapplications, granting them hello same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="f2b4f-146">Ange åtkomstprinciper för gästanvändare</span><span class="sxs-lookup"><span data-stu-id="f2b4f-146">Set guest user access policies</span></span>
<span data-ttu-id="f2b4f-147">Hej **konfigurera** för en katalog innehåller alternativ toocontrol åtkomsten för gästanvändare.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-147">hello **Configure** tab of a directory includes options toocontrol access for guest users.</span></span> <span data-ttu-id="f2b4f-148">Dessa alternativ kan bara ändras på den klassiska Azure-portalen av en global katalogadministratör.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-148">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="f2b4f-149">För närvarande finns det ingen PowerShell- eller API-metod.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-149">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="f2b4f-150">tooopen hello **konfigurera** fliken i hello Azure klassiska portal, Välj **Active Directory**, och välj sedan hello namnet på hello katalog.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-150">tooopen hello **Configure** tab in hello Azure classic portal, select **Active Directory**, and then select hello name of hello directory.</span></span>

![Konfigurera fliken i Azure Active Directory][1]

<span data-ttu-id="f2b4f-152">Du kan redigera hello alternativ toocontrol åtkomsten för gästanvändare.</span><span class="sxs-lookup"><span data-stu-id="f2b4f-152">Then you can edit hello options toocontrol access for guest users.</span></span>

![Alternativ för åtkomstkontroll för gästanvändare][2]

## <a name="whats-next"></a><span data-ttu-id="f2b4f-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2b4f-154">What's next</span></span>
* [<span data-ttu-id="f2b4f-155">Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2b4f-155">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="f2b4f-156">Administrera Azure AD</span><span class="sxs-lookup"><span data-stu-id="f2b4f-156">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="f2b4f-157">Hantera lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f2b4f-157">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="f2b4f-158">Hantera grupper i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f2b4f-158">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
