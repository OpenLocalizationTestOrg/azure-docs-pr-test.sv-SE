---
title: "Skapa anpassade roller för rollbaserad åtkomstkontroll och tilldela interna och externa användare i Azure | Microsoft Docs"
description: "Tilldela anpassade RBAC-roller med hjälp av PowerShell och CLI för interna och externa användare"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: d687f94bebfd0b6c1ec0690da798be5409640954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="6c066-103">Introduktion på rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="6c066-103">Intro on role-based access control</span></span>

<span data-ttu-id="6c066-104">Rollbaserad åtkomstkontroll är en Azure portal endast funktionen som tillåter ägare av en prenumeration att tilldela detaljerade roller till andra användare som kan hantera en viss resurs-scope i sina miljöer.</span><span class="sxs-lookup"><span data-stu-id="6c066-104">Role-based access control is an Azure portal only feature allowing the owners of a subscription to assign granular roles to other users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="6c066-105">RBAC kan bättre säkerhetshantering för stora organisationer och för små och medelstora företag arbetar med externa samarbetspartners, leverantörer eller freelancers som behöver åtkomst till specifika resurser i din miljö, men inte nödvändigtvis att hela infrastrukturen eller någon fakturerings-relaterade scope.</span><span class="sxs-lookup"><span data-stu-id="6c066-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access to specific resources in your environment but not necessarily to the entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="6c066-106">RBAC kan flexibiliteten för en Azure-prenumeration som äger hanteras av administratörskontot (service administratörsrollen på en prenumerationsnivå) och har flera användare uppmanas för att arbeta under samma prenumeration, men utan några administrativa rättigheter för det. .</span><span class="sxs-lookup"><span data-stu-id="6c066-106">RBAC allows the flexibility of owning one Azure subscription managed by the administrator account (service administrator role at a subscription level) and have multiple users invited to work under the same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="6c066-107">Från en hanteringen och fakturering perspektiv bevisar RBAC-funktionen ska vara ett tids- och effektivt alternativ för att använda Azure i olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="6c066-107">From a management and billing perspective, the RBAC feature proves to be a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c066-108">Krav</span><span class="sxs-lookup"><span data-stu-id="6c066-108">Prerequisites</span></span>
<span data-ttu-id="6c066-109">Med RBAC i Azure-miljön kräver:</span><span class="sxs-lookup"><span data-stu-id="6c066-109">Using RBAC in the Azure environment requires:</span></span>

* <span data-ttu-id="6c066-110">Med en fristående Azure-prenumeration tilldelas användaren som ägare (prenumeration roll)</span><span class="sxs-lookup"><span data-stu-id="6c066-110">Having a standalone Azure subscription assigned to the user as owner (subscription role)</span></span>
* <span data-ttu-id="6c066-111">Har rollen som ägare av Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c066-111">Have the Owner role of the Azure subscription</span></span>
* <span data-ttu-id="6c066-112">Ha åtkomst till den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="6c066-112">Have access to the [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="6c066-113">Se till att ha följande Resursleverantörer som registrerats för användaren prenumerationen: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="6c066-113">Make sure to have the following Resource Providers registered for the user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="6c066-114">Läs mer om hur du registrerar resursleverantörer [Resource Manager-providers, regioner, API-versioner och scheman](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="6c066-114">For more information on how to register the resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6c066-115">Office 365-prenumerationer eller Azure Active Directory-licenser (till exempel: åtkomst till Azure Active Directory) etablerats från O365 portal inte kvalitet med RBAC.</span><span class="sxs-lookup"><span data-stu-id="6c066-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access to Azure Active Directory) provisioned from the O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="6c066-116">Hur kan RBAC användas</span><span class="sxs-lookup"><span data-stu-id="6c066-116">How can RBAC be used</span></span>
<span data-ttu-id="6c066-117">RBAC kan tillämpas på tre olika scope i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c066-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="6c066-118">Från området högsta till lägsta som är de följande:</span><span class="sxs-lookup"><span data-stu-id="6c066-118">From the highest scope to the lowest one, they are as follows:</span></span>

* <span data-ttu-id="6c066-119">Prenumeration (högsta)</span><span class="sxs-lookup"><span data-stu-id="6c066-119">Subscription (highest)</span></span>
* <span data-ttu-id="6c066-120">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6c066-120">Resource group</span></span>
* <span data-ttu-id="6c066-121">Resursen omfång (lägsta åtkomstnivå erbjudande riktade behörigheter till en enskild resurs i Azure omfattning)</span><span class="sxs-lookup"><span data-stu-id="6c066-121">Resource scope (the lowest access level offering targeted permissions to an individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-the-subscription-scope"></a><span data-ttu-id="6c066-122">Tilldela roller RBAC definitionsområdet prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c066-122">Assign RBAC roles at the subscription scope</span></span>
<span data-ttu-id="6c066-123">Det finns två vanliga exempel när RBAC används (men inte begränsat till):</span><span class="sxs-lookup"><span data-stu-id="6c066-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="6c066-124">Med externa användare från organisationer (inte del av användaren administratör Azure Active Directory-klient) bjudits in till att hantera vissa resurser eller hela prenumerationen</span><span class="sxs-lookup"><span data-stu-id="6c066-124">Having external users from the organizations (not part of the admin user's Azure Active Directory tenant) invited to manage certain resources or the whole subscription</span></span>
* <span data-ttu-id="6c066-125">Arbeta med användare i organisationen (det är en del av användarens Azure Active Directory-klient) men en del av olika grupper eller grupper som behöver detaljerad åtkomst till hela prenumerationen eller till vissa resursgrupp eller resurs scope i den miljö</span><span class="sxs-lookup"><span data-stu-id="6c066-125">Working with users inside the organization (they are part of the user's Azure Active Directory tenant) but part of different teams or groups which need granular access either to the whole subscription or to certain resource groups or resource scopes in the environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="6c066-126">Bevilja åtkomst till en prenumerationsnivå för en användare utanför Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c066-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="6c066-127">RBAC-roller som kan beviljas endast av **ägare** prenumerationens därför administratörsanvändare måste vara inloggad med ett användarnamn som har rollen förinställda eller har skapats i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-127">RBAC roles can be granted only by **Owners** of the subscription therefore the admin user must be logged with a username which has this role pre-assigned or has created the Azure subscription.</span></span>

<span data-ttu-id="6c066-128">Azure-portalen när du loggar in som administratör, Välj ”prenumerationer” och välj en.</span><span class="sxs-lookup"><span data-stu-id="6c066-128">From the Azure portal, after you sign-in as admin, select “Subscriptions” and chose the desired one.</span></span>
<span data-ttu-id="6c066-129">![prenumerationsbladet i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) som standard om administratören har köpt Azure-prenumeration användaren visas som **kontoadministratören**, detta är rollen prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if the admin user has purchased the Azure subscription, the user will show up as **Account Admin**, this being the subscription role.</span></span> <span data-ttu-id="6c066-130">Mer information om Azure-prenumeration roller finns [lägga till eller ändra Azure-administratörsroller som hanterar prenumerationen eller tjänster](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="6c066-130">For more details on the Azure subscription roles, see [Add or change Azure administrator roles that manage the subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="6c066-131">I det här exemplet är användaren ”alflanigan@outlook.com” är den **ägare** prenumerationen i AAD för ”utvärderingsversion” klient ”standard klient Azure”.</span><span class="sxs-lookup"><span data-stu-id="6c066-131">In this example, the user "alflanigan@outlook.com" is the **Owner** of the "Free Trial" subscription in the AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="6c066-132">Eftersom den här användaren är skapare av Azure-prenumeration med inledande Account ”Outlook” (Account = Outlook Live etc.) standarddomännamnet för alla andra användare som lagts till i den här klienten kommer att **”@alflaniganuoutlook.onmicrosoft.com”**.</span><span class="sxs-lookup"><span data-stu-id="6c066-132">Since this user is the creator of the Azure subscription with the initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) the default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="6c066-133">Avsiktligt syntaxen för den nya domänen bildas genom att sätta ihop användarnamn och domän namnet på användaren som skapade klienten och lägger till tillägget **”. onmicrosoft.com”**.</span><span class="sxs-lookup"><span data-stu-id="6c066-133">By design, the syntax of the new domain is formed by putting together the username and domain name of the user who created the tenant and adding the extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="6c066-134">Dessutom kan användare kan logga in med ett anpassat domännamn i klienten efter att lägga till och verifierar för den nya innehavaren.</span><span class="sxs-lookup"><span data-stu-id="6c066-134">Furthermore, users can sign-in with a custom domain name in the tenant after adding and verifying it for the new tenant.</span></span> <span data-ttu-id="6c066-135">Mer information om hur du verifierar ett anpassat domännamn i Azure Active Directory-klient finns [lägga till ett anpassat domännamn i katalogen](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="6c066-135">For more details on how to verify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name to your directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="6c066-136">I det här exemplet innehåller klientkatalogen ”standard Azure” endast användare med domännamnet ”@alflanigan.onmicrosoft.com”.</span><span class="sxs-lookup"><span data-stu-id="6c066-136">In this example, the "Default tenant Azure" directory contains only users with the domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="6c066-137">När du har valt prenumerationen admin-användaren måste klicka på **Access Control (IAM)** och sedan **lägga till en ny roll**.</span><span class="sxs-lookup"><span data-stu-id="6c066-137">After selecting the subscription, the admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![funktionen IAM för åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![lägga till nya användare i IAM-funktionen åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="6c066-140">Nästa steg är att välja rollen tilldelas och användaren som ska tilldelas rollen RBAC.</span><span class="sxs-lookup"><span data-stu-id="6c066-140">The next step is to select the role to be assigned and the user whom the RBAC role will be assigned to.</span></span> <span data-ttu-id="6c066-141">I den **rollen** listrutan administratörsanvändare ser de inbyggda RBAC roller som är tillgängliga i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c066-141">In the **Role** dropdown menu the admin user sees only the built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="6c066-142">Mer detaljerade beskrivningar av varje roll och deras tilldelningsbara scope finns [inbyggda roller för rollbaserad åtkomstkontroll i](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="6c066-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="6c066-143">Administratören måste sedan lägga till den externa användaren e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6c066-143">The admin user then needs to add the email address of the external user.</span></span> <span data-ttu-id="6c066-144">Förväntat beteende är externa användare kan inte visas i den befintliga klienten.</span><span class="sxs-lookup"><span data-stu-id="6c066-144">The expected behavior is for the external user to not show up in the existing tenant.</span></span> <span data-ttu-id="6c066-145">När den externa användaren har bjudits han visas under **prenumerationer > Access Control (IAM)** med de aktuella användare som är tilldelade en RBAC-rollen på prenumerationsomfattningen.</span><span class="sxs-lookup"><span data-stu-id="6c066-145">After the external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all the current users which are currently assigned an RBAC role at the Subscription scope.</span></span>





![Lägg till behörigheter till den nya RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![lista över RBAC-roller på prenumerationsnivån](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="6c066-148">Användaren ”chessercarlton@gmail.com” har bjudits in till att vara en **ägare** för prenumerationen ”utvärderingsversion”.</span><span class="sxs-lookup"><span data-stu-id="6c066-148">The user "chessercarlton@gmail.com" has been invited to be an **Owner** for the “Free Trial” subscription.</span></span> <span data-ttu-id="6c066-149">När inbjudan, får den externa användaren ett e-postbekräftelse med en aktiveringslänk.</span><span class="sxs-lookup"><span data-stu-id="6c066-149">After sending the invitation, the external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="6c066-150">![e-postinbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="6c066-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="6c066-151">Som utanför organisationen, har den nya användaren inte några befintliga attribut i klientkatalogen ”standard Azure”.</span><span class="sxs-lookup"><span data-stu-id="6c066-151">Being external to the organization, the new user does not have any existing attributes in the "Default tenant Azure" directory.</span></span> <span data-ttu-id="6c066-152">De kommer att skapas när den externa användaren har gett samtycke till att läggas till i katalogen som är kopplat till prenumerationen som har tilldelats en roll till.</span><span class="sxs-lookup"><span data-stu-id="6c066-152">They will be created after the external user has given consent to be recorded in the directory which is associated with the subscription which he has been assigned a role to.</span></span>





![e-postmeddelande för inbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="6c066-154">Externa användare visas i Azure Active Directory-klient hädanefter som externa användare och det kan visas både i Azure-portalen och i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="6c066-154">The external user shows in the Azure Active Directory tenant from now on as external user and this can be viewed both in the Azure portal and in the classic portal.</span></span>





![användare bladet azure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![användare bladet azure active directory klassiska Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="6c066-157">I den **användare** vyn i båda portaler externa användare kan identifieras av:</span><span class="sxs-lookup"><span data-stu-id="6c066-157">In the **Users** view in both portals the external users can be recognized by:</span></span>

* <span data-ttu-id="6c066-158">Typen av olika ikoner i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6c066-158">The different icon type in the Azure portal</span></span>
* <span data-ttu-id="6c066-159">Olika sourcing punkten i den klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="6c066-159">The different sourcing point in the classic portal</span></span>

<span data-ttu-id="6c066-160">Dock bevilja **ägare** eller **deltagare** åtkomst till en extern användare i den **prenumeration** omfång, tillåter inte åtkomst till katalogen för admin-användare, såvida inte den **Global administratör** tillåter.</span><span class="sxs-lookup"><span data-stu-id="6c066-160">However, granting **Owner** or **Contributor** access to an external user at the **Subscription** scope, does not allow the access to the admin user's directory, unless the **Global Admin** allows it.</span></span> <span data-ttu-id="6c066-161">I användar-proprieties den **användartyp** som har två gemensamma parametrar, **medlem** och **gäst** kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="6c066-161">In the user proprieties,  the **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="6c066-162">En medlem är en användare som har registrerats i katalogen medan gäst är en användare som bjudits in till katalogen från en extern källa.</span><span class="sxs-lookup"><span data-stu-id="6c066-162">A member is a user which is registered in the directory while a guest is a user invited to the directory from an external source.</span></span> <span data-ttu-id="6c066-163">Mer information finns i [hur till B2B-samarbete användare av Azure Active Directory-administratörer](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="6c066-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="6c066-164">Kontrollera att den externa användaren väljer att logga in på rätt katalog när du har angett autentiseringsuppgifterna i portalen.</span><span class="sxs-lookup"><span data-stu-id="6c066-164">Make sure that after entering the credentials in the portal, the external user selects the correct directory to sign-in to.</span></span> <span data-ttu-id="6c066-165">Samma användare kan ha åtkomst till flera kataloger och kan välja någon av dem genom att klicka på användarnamnet i den översta högra sidan i Azure-portalen och välj sedan den aktuella katalogen i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6c066-165">The same user can have access to multiple directories and can select either one of  them by clicking the username in the top right-hand side in the Azure portal and then choose the appropriate directory from the dropdown list.</span></span>

<span data-ttu-id="6c066-166">Samtidigt som gäst i katalogen, den externa användaren kan hantera alla resurser för Azure-prenumeration, men har inte åtkomst till katalogen.</span><span class="sxs-lookup"><span data-stu-id="6c066-166">While being a guest in the directory, the external user can manage all resources for the Azure subscription, but can't access the directory.</span></span>





![åtkomst begränsas till azure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="6c066-168">Azure Active Directory och Azure-prenumeration har inte en underordnad-överordnad relation som andra Azure-resurser (till exempel: virtuella datorer, virtuella nätverk, webbprogram, lagring etc.) med en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="6c066-169">Alla dessa skapas, hanteras och debiteras enligt en Azure-prenumeration medan en Azure-prenumeration som används för att hantera åtkomst till en Azure-katalog.</span><span class="sxs-lookup"><span data-stu-id="6c066-169">All the latter is created, managed and billed under an Azure subscription while an Azure subscription is used to manage the access to an Azure directory.</span></span> <span data-ttu-id="6c066-170">Mer information finns i [hur Azure-prenumeration är kopplad till Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="6c066-170">For more details, see [How an Azure subscription is related to Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="6c066-171">Från alla de inbyggda RBAC rollerna, **ägare** och **deltagare** ger fullständig åtkomst till alla resurser i miljön, skillnaden är att en deltagare inte kan skapa och ta bort nya RBAC-roller .</span><span class="sxs-lookup"><span data-stu-id="6c066-171">From all the built-in RBAC roles, **Owner** and **Contributor** offer full management access to all resources in the environment, the difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="6c066-172">De inbyggda rollerna som **Virtual Machine-deltagare** erbjuder fullständiga endast åtkomst till resurser som anges av namnet, oberoende av den **resursgruppen** skapas i.</span><span class="sxs-lookup"><span data-stu-id="6c066-172">The other built-in roles like **Virtual Machine Contributor** offer full management access only to the resources indicated by the name, regardless of the **Resource Group** they are being created into.</span></span>

<span data-ttu-id="6c066-173">Tilldela rollen inbyggda RBAC för **Virtual Machine-deltagare** innebär att användaren har tilldelats rollen på en på prenumerationsnivå:</span><span class="sxs-lookup"><span data-stu-id="6c066-173">Assigning the built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that the user assigned the role:</span></span>

* <span data-ttu-id="6c066-174">Visa alla virtuella datorer oavsett deras distributionsdatum och resursgrupper som de tillhör</span><span class="sxs-lookup"><span data-stu-id="6c066-174">Can view all virtual machines regardless their deployment date and the resource groups they are part of</span></span>
* <span data-ttu-id="6c066-175">Har fullständig åtkomst till de virtuella datorerna i prenumerationen</span><span class="sxs-lookup"><span data-stu-id="6c066-175">Has full management access to the virtual machines in the subscription</span></span>
* <span data-ttu-id="6c066-176">Det går inte att visa alla andra typer av resurser i prenumerationen</span><span class="sxs-lookup"><span data-stu-id="6c066-176">Can't view any other resource types in the subscription</span></span>
* <span data-ttu-id="6c066-177">Fungerar inte ändringar ur fakturering</span><span class="sxs-lookup"><span data-stu-id="6c066-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="6c066-178">RBAC som en portal endast funktion i Azure, ger det inte tillgång till den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="6c066-178">RBAC being an Azure portal only feature, it doesn't grant access to the classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a><span data-ttu-id="6c066-179">Tilldela en inbyggd roll RBAC till en extern användare</span><span class="sxs-lookup"><span data-stu-id="6c066-179">Assign a built-in RBAC role to an external user</span></span>
<span data-ttu-id="6c066-180">För ett annat scenario i det här testet kan den externa användaren ”alflanigan@gmail.com” läggs till som en **Virtual Machine-deltagare**.</span><span class="sxs-lookup"><span data-stu-id="6c066-180">For a different scenario in this test, the external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![inbyggda deltagarrollen virtuell dator](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="6c066-182">Normalt beteende för den här externa användare med den här inbyggda rollen är att se och hantera endast virtuella datorer och deras intilliggande Resource Manager endast resurser som är nödvändiga vid distribution.</span><span class="sxs-lookup"><span data-stu-id="6c066-182">The normal behavior for this external user with this built-in role is to see and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="6c066-183">Avsiktligt rollerna begränsad erbjuder endast åtkomst till sina motsvarande resurser som skapats i Azure-portalen, oavsett vissa kan fortfarande vara distribuerad i den klassiska portalen (till exempel: virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="6c066-183">By design, these limited roles offer access only to their correspondent resources created in the Azure portal, regardless some can still be deployed in the classic portal as well (for example: virtual machines).</span></span>





![deltagare rollen översikt över virtuella datorer i azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a><span data-ttu-id="6c066-185">Bevilja åtkomst till en prenumerationsnivå för en användare i samma katalog</span><span class="sxs-lookup"><span data-stu-id="6c066-185">Grant access at a subscription level for a user in the same directory</span></span>
<span data-ttu-id="6c066-186">Processflödet är identisk med att lägga till en extern användare, både ur administratör ge RBAC-roll som användaren beviljas åtkomst till rollen.</span><span class="sxs-lookup"><span data-stu-id="6c066-186">The process flow is identical to adding an external user, both from the admin perspective granting the RBAC role as well as the user being granted access to the role.</span></span> <span data-ttu-id="6c066-187">Skillnaden här är att inbjudna användare inte får någon e-inbjudningar som resursen scope i prenumerationen kommer att finnas i instrumentpanelen efter inloggningen.</span><span class="sxs-lookup"><span data-stu-id="6c066-187">The difference here is that the invited user will not receive any email invitations as all the resource scopes within the subscription will be available in the dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a><span data-ttu-id="6c066-188">Tilldela RBAC-roller på Gruppomfång resurs</span><span class="sxs-lookup"><span data-stu-id="6c066-188">Assign RBAC roles at the resource group scope</span></span>
<span data-ttu-id="6c066-189">Tilldela en RBAC-rollen på en **resursgruppen** scope har en identisk process för att tilldela rollen på prenumerationsnivån för båda typer av användare – extern eller intern (ingår i samma katalog).</span><span class="sxs-lookup"><span data-stu-id="6c066-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning the role at the subscription level, for both types of users - either external or internal (part of the same directory).</span></span> <span data-ttu-id="6c066-190">De användare som har tilldelats rollen RBAC är att se i sina miljöer endast resursgruppen de har tilldelats åtkomst från den **resursgrupper** ikon i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c066-190">The users which are assigned the RBAC role is to see in their environment only the resource group they have been assigned access from the **Resource Groups** icon in the Azure portal.</span></span>

## <a name="assign-rbac-roles-at-the-resource-scope"></a><span data-ttu-id="6c066-191">Tilldela RBAC-roller i omfånget för resurs</span><span class="sxs-lookup"><span data-stu-id="6c066-191">Assign RBAC roles at the resource scope</span></span>
<span data-ttu-id="6c066-192">Tilldela en roll RBAC definitionsområdet resurs i Azure har en identisk process för att tilldela rollen på prenumerationsnivån eller på resursgruppsnivå, följa samma arbetsflöde för båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="6c066-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning the role at the subscription level or at the resource group level, following the same workflow for both scenarios.</span></span> <span data-ttu-id="6c066-193">De användare som har tilldelats rollen RBAC kan finns endast de objekt som de har tilldelats åtkomst till, antingen i den **alla resurser** fliken eller direkt i sina instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6c066-193">Again, the users which are assigned the RBAC role can see only the items that they have been assigned access to, either in the **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="6c066-194">En viktig aspekt för RBAC både på resursen Gruppomfång eller resurs scope är att användarna ska se till att logga in på rätt katalog.</span><span class="sxs-lookup"><span data-stu-id="6c066-194">An important aspect for RBAC both at resource group scope or resource scope is for the users to make sure to sign-in to the correct directory.</span></span>





![Directory inloggning i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="6c066-196">Tilldela RBAC-roller för en Azure Active Directory-grupp</span><span class="sxs-lookup"><span data-stu-id="6c066-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="6c066-197">Alla scenarier med RBAC på tre olika scope i Azure ger behörighet för att hantera, distribuera och administrera olika resurser som tilldelats användare utan att behöva hantera en personlig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-197">All the scenarios using RBAC at the three different scopes in Azure offer the privilege of managing, deploying and administering various resources as an assigned user without the need of managing a personal subscription.</span></span> <span data-ttu-id="6c066-198">Oavsett tilldelas rollen RBAC för en prenumeration, resursgrupp eller resurs omfång, alla resurser skapas ytterligare på av tilldelade användare debiteras under en Azure-prenumerationen där användarna har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="6c066-198">Regardless the RBAC role is assigned for a subscription, resource group or resource scope, all the resources created further on by the assigned users are billed under the one Azure subscription where the users have access to.</span></span> <span data-ttu-id="6c066-199">På så sätt kan användare som har administratörsbehörighet för att hela Azure-prenumeration fakturering har en fullständig översikt på förbrukning, oavsett vem som hanterar resurser.</span><span class="sxs-lookup"><span data-stu-id="6c066-199">This way, the users who have billing administrator permissions for that entire Azure subscription has a complete overview on the consumption, regardless who is managing the resources.</span></span>

<span data-ttu-id="6c066-200">För större organisationer kan RBAC-roller tillämpas på samma sätt för Azure Active Directory-grupper med tanke på att administratören vill ge detaljerade åtkomst för team eller hela avdelningar, inte individuellt för varje användare måste därför överväger perspektiv det som ett mycket tid och hantering av effektivt alternativ.</span><span class="sxs-lookup"><span data-stu-id="6c066-200">For larger organizations, RBAC roles can be applied in the same way for Azure Active Directory groups considering the perspective that the admin user wants to grant the granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="6c066-201">Som det här exemplet illustrerar den **deltagare** roll har lagts till någon av grupperna i klienten på prenumerationsnivån.</span><span class="sxs-lookup"><span data-stu-id="6c066-201">To illustrate this example, the **Contributor** role has been added to one of the groups in the tenant at the subscription level.</span></span>





![Lägg till RBAC-rollen för AAD-grupper](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="6c066-203">Dessa grupper är säkerhetsgrupper som etableras och hanteras i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6c066-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a><span data-ttu-id="6c066-204">Skapa en anpassad RBAC-roll för att öppna supportärenden med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c066-204">Create a custom RBAC role to open support requests using PowerShell</span></span>
<span data-ttu-id="6c066-205">De inbyggda RBAC-roller som är tillgängliga i Azure kontrollera vissa behörighetsnivåer baserat på tillgängliga resurser i miljön.</span><span class="sxs-lookup"><span data-stu-id="6c066-205">The built-in RBAC roles which are available in Azure ensure certain permission levels based on the available resources in the environment.</span></span> <span data-ttu-id="6c066-206">Men om ingen av dessa roller passar admin-användare, finns alternativet att begränsa åtkomsten ytterligare genom att skapa anpassade RBAC-roller.</span><span class="sxs-lookup"><span data-stu-id="6c066-206">However, if none of these roles suit the admin user's needs, there is the option to limit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="6c066-207">Skapa anpassade RBAC-roller kräver att ta en inbyggd roll, redigera och importera den sedan tillbaka i miljön.</span><span class="sxs-lookup"><span data-stu-id="6c066-207">Creating custom RBAC roles requires to take one built-in role, edit it and then import it back in the environment.</span></span> <span data-ttu-id="6c066-208">Ladda ned och överföra rollen som hanteras med PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="6c066-208">The download and upload of the role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="6c066-209">Det är viktigt att förstå kraven för att skapa en anpassad roll som kan ge detaljerade åtkomst på prenumerationsnivå och även tillåta inbjudna användaren möjlighet att öppna supportärenden.</span><span class="sxs-lookup"><span data-stu-id="6c066-209">It is important to understand the prerequisites of creating a custom role which can grant granular access at the subscription level and also allow the invited user the flexibility of opening support requests.</span></span>

<span data-ttu-id="6c066-210">Det här exemplet den inbyggda rollen **Reader** som ger användare åtkomst att visa alla resurs scope men inte att redigera dem eller skapa nya har anpassats för att tillåta användaren att öppna supportärenden.</span><span class="sxs-lookup"><span data-stu-id="6c066-210">For this example the built-in role **Reader** which allows users access to view all the resource scopes but not to edit them or create new ones has been customized to allow the user the option of opening support requests.</span></span>

<span data-ttu-id="6c066-211">Den första åtgärden för att exportera den **Reader** roll måste utföras i PowerShell kördes med förhöjda behörigheter som administratör.</span><span class="sxs-lookup"><span data-stu-id="6c066-211">The first action of exporting the **Reader** role needs to be completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![PowerShell skärmbild för läsare RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="6c066-213">Du måste sedan extrahera JSON-mall för rollen.</span><span class="sxs-lookup"><span data-stu-id="6c066-213">Then you need to extract the JSON template of the role.</span></span>





![JSON-mall för anpassad Reader RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="6c066-215">En typisk RBAC-rollen består av tre huvudavsnitt **åtgärder**, **NotActions** och **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="6c066-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="6c066-216">I den **åtgärd** finns i avsnittet visas alla tillåtna åtgärder för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="6c066-216">In the **Action** section are listed all the permitted operations for this role.</span></span> <span data-ttu-id="6c066-217">Det är viktigt att förstå att tilldelas varje åtgärd från en resursleverantör.</span><span class="sxs-lookup"><span data-stu-id="6c066-217">It's important to understand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="6c066-218">I detta fall för att skapa supportärenden den **Microsoft.Support** resursprovidern måste anges.</span><span class="sxs-lookup"><span data-stu-id="6c066-218">In this case, for creating support tickets the **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="6c066-219">Du kan använda PowerShell för att kunna se alla resursleverantörer tillgänglig och registrerade i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-219">To be able to see all the resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="6c066-220">Dessutom kan du söka efter de alla tillgängliga PowerShell cmdletarna för hantering av resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="6c066-220">Additionally, you can check for the all the available PowerShell cmdlets to manage the resource providers.</span></span>
    <span data-ttu-id="6c066-221">![PowerShell skärmbild för providern resurshantering](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="6c066-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="6c066-222">Om du vill begränsa alla åtgärder för en viss roll RBAC resursproviders anges under avsnittet **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="6c066-222">To restrict all the actions for a particular RBAC role, resource providers are listed under the section **NotActions**.</span></span>
<span data-ttu-id="6c066-223">Senast, är det obligatoriskt att rollen RBAC innehåller explicit prenumerationen ID: N där den används.</span><span class="sxs-lookup"><span data-stu-id="6c066-223">Last, it's mandatory that the RBAC role contains the explicit subscription IDs where it is used.</span></span> <span data-ttu-id="6c066-224">Prenumerations-ID: N visas under den **AssignableScopes**, annars du kommer inte att importera rollen i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-224">The subscription IDs are listed under the **AssignableScopes**, otherwise you will not be allowed to import the role in your subscription.</span></span>

<span data-ttu-id="6c066-225">När du skapar och anpassar RBAC-rollen, behöver importeras tillbaka i miljön.</span><span class="sxs-lookup"><span data-stu-id="6c066-225">After creating and customizing the RBAC role, it needs to be imported back the environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="6c066-226">I det här exemplet är eget namn för den här rollen RBAC ”Reader stöd biljetter åtkomstnivå” så att användaren kan visa allt i prenumerationen och också öppna supportärenden.</span><span class="sxs-lookup"><span data-stu-id="6c066-226">In this example, the custom name for this RBAC role is "Reader support tickets access level" allowing the user to view everything in the subscription and also to open support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="6c066-227">Bara två inbyggda RBAC rollerna tillåter åtgärden öppnandet av supportärenden är **ägare** och **deltagare**.</span><span class="sxs-lookup"><span data-stu-id="6c066-227">The only two built-in RBAC roles allowing the action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="6c066-228">För en användare för att kunna öppna supportärenden måste han tilldelas en RBAC roll bara definitionsområdet prenumeration, eftersom alla supportärenden skapas baserat på en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c066-228">For a user to be able to open support requests, he must be assigned an RBAC role only at the subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="6c066-229">Den här nya anpassade rollen har tilldelats till en användare från samma katalog.</span><span class="sxs-lookup"><span data-stu-id="6c066-229">This new custom role has been assigned to an user from the same directory.</span></span>





![Skärmbild av anpassade RBAC-roll som importeras i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Skärmbild av tilldela anpassade importerade RBAC roll till användare i samma katalog](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![Skärmbild av behörigheter för anpassad importerade RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="6c066-233">Exemplet har mer detaljerad att visa gränserna för den här anpassade RBAC-rollen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="6c066-233">The example has been further detailed to emphasize the limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="6c066-234">Kan skapa nya supportförfrågningar</span><span class="sxs-lookup"><span data-stu-id="6c066-234">Can create new support requests</span></span>
* <span data-ttu-id="6c066-235">Det går inte att skapa den nya resursen scope (till exempel: virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="6c066-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="6c066-236">Det går inte att skapa nya resursgrupper</span><span class="sxs-lookup"><span data-stu-id="6c066-236">Can't create new resource groups</span></span>





![Skärmbild av anpassade RBAC roll skapar supportärenden](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Skärmbild av anpassad RBAC roll går inte att skapa virtuella datorer](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Skärmbild av anpassad RBAC roll går inte att skapa nya RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a><span data-ttu-id="6c066-240">Skapa en anpassad RBAC-roll för att öppna supportärenden med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c066-240">Create a custom RBAC role to open support requests using Azure CLI</span></span>
<span data-ttu-id="6c066-241">Körs på en Mac och utan att ha åtkomst till PowerShell, är Azure CLI att.</span><span class="sxs-lookup"><span data-stu-id="6c066-241">Running on a Mac and without having access to PowerShell, Azure CLI is the way to go.</span></span>

<span data-ttu-id="6c066-242">Stegen för att skapa en anpassad roll är desamma, med det enda undantaget att med hjälp av CLI rollen inte kan hämtas i en JSON-mall, men den kan visas i CLI.</span><span class="sxs-lookup"><span data-stu-id="6c066-242">The steps to create a custom role are the same, with the sole exception that using CLI the role can't be downloaded in a JSON template, but it can be viewed in the CLI.</span></span>

<span data-ttu-id="6c066-243">Jag har valt den inbyggda rollen i det här exemplet **säkerhetskopiering Reader**.</span><span class="sxs-lookup"><span data-stu-id="6c066-243">For this example I have chosen the built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Visa CLI Skärmbild av rollen säkerhetskopiering läsare](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="6c066-245">Redigera rollen i Visual Studio när du har kopierat proprieties i en JSON-mall i **Microsoft.Support** resursprovidern har lagts till i den **åtgärder** avsnitt så att användaren kan öppna stöd begäranden när fortsätter att vara en läsare för säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="6c066-245">Editing the role in Visual Studio after copying the proprieties in a JSON template, the **Microsoft.Support** resource provider has been added in the **Actions** sections so that this user can open support requests while continuing to be a reader for the backup vaults.</span></span> <span data-ttu-id="6c066-246">Igen är det nödvändigt att lägga till prenumerations-ID där den här rollen ska användas i den **AssignableScopes** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6c066-246">Again it is necessary to add the subscription ID where this role will be used in the **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![CLI Skärmbild av Importera anpassad RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="6c066-248">Den nya rollen är nu tillgänglig i Azure-portalen och assignation-processen är densamma som i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="6c066-248">The new role is now available in the Azure portal and the assignation process is the same as in the previous examples.</span></span>





![Azure portal Skärmbild av anpassade RBAC-roll som skapats med hjälp av CLI 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="6c066-250">Azure Cloud-gränssnittet är allmänt tillgänglig från och med den senaste Build 2017.</span><span class="sxs-lookup"><span data-stu-id="6c066-250">As of the latest Build 2017, the Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="6c066-251">Azure Cloud-gränssnittet är ett komplement till IDE- och Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6c066-251">Azure Cloud Shell is a complement to IDE and the Azure Portal.</span></span> <span data-ttu-id="6c066-252">Med den här tjänsten får du ett webbaserat gränssnitt som är autentiserad och värdbaserad i Azure och du kan använda den i stället för CLI installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6c066-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Azure-molnet Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
