---
title: "aaaCreate anpassade rollbaserad åtkomstkontroll roller och tilldela toointernal och externa användare i Azure | Microsoft Docs"
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
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="83de1-103">Introduktion på rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="83de1-103">Intro on role-based access control</span></span>

<span data-ttu-id="83de1-104">Rollbaserad åtkomstkontroll är en Azure portal endast funktion som tillåter att hello ägare för en prenumeration tooassign detaljerade roller tooother användare som kan hantera en viss resurs-scope i sina miljöer.</span><span class="sxs-lookup"><span data-stu-id="83de1-104">Role-based access control is an Azure portal only feature allowing hello owners of a subscription tooassign granular roles tooother users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="83de1-105">RBAC kan bättre säkerhetshantering för stora organisationer och för små och medelstora företag arbetar med externa samarbetspartners, leverantörer eller freelancers som behöver åtkomst till toospecific resurser i din miljö, men inte nödvändigtvis toohello hela infrastruktur eller någon fakturerings-relaterade scope.</span><span class="sxs-lookup"><span data-stu-id="83de1-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access toospecific resources in your environment but not necessarily toohello entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="83de1-106">RBAC ger hello flexibiliteten för en Azure-prenumeration som hanteras av hello administratörskonto (service administratörsrollen på en prenumerationsnivå) som äger och har flera användare som bjudits in toowork under hello samma prenumeration men utan några administrativa behörighet för den.</span><span class="sxs-lookup"><span data-stu-id="83de1-106">RBAC allows hello flexibility of owning one Azure subscription managed by hello administrator account (service administrator role at a subscription level) and have multiple users invited toowork under hello same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="83de1-107">Från en hanteringen och fakturering perspektiv bevisar hello RBAC funktionen toobe tids- och effektiv alternativet för att använda Azure i olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="83de1-107">From a management and billing perspective, hello RBAC feature proves toobe a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83de1-108">Krav</span><span class="sxs-lookup"><span data-stu-id="83de1-108">Prerequisites</span></span>
<span data-ttu-id="83de1-109">Med RBAC i hello Azure-miljön kräver:</span><span class="sxs-lookup"><span data-stu-id="83de1-109">Using RBAC in hello Azure environment requires:</span></span>

* <span data-ttu-id="83de1-110">Med en fristående Azure-prenumeration har gett toohello användaren som ägare (prenumeration roll)</span><span class="sxs-lookup"><span data-stu-id="83de1-110">Having a standalone Azure subscription assigned toohello user as owner (subscription role)</span></span>
* <span data-ttu-id="83de1-111">Har hello ägarrollen av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="83de1-111">Have hello Owner role of hello Azure subscription</span></span>
* <span data-ttu-id="83de1-112">Ha åtkomst toohello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="83de1-112">Have access toohello [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="83de1-113">Kontrollera att toohave hello följande Resursproviders registrerad för hello användaren prenumeration: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="83de1-113">Make sure toohave hello following Resource Providers registered for hello user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="83de1-114">Mer information om hur tooregister hello resursprovidrar finns [Resource Manager-providers, regioner, API-versioner och scheman](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="83de1-114">For more information on how tooregister hello resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="83de1-115">Office 365-prenumerationer eller Azure Active Directory-licenser (till exempel: komma åt tooAzure Active Directory) etablerats från hello O365 portal inte kvalitet med RBAC.</span><span class="sxs-lookup"><span data-stu-id="83de1-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access tooAzure Active Directory) provisioned from hello O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="83de1-116">Hur kan RBAC användas</span><span class="sxs-lookup"><span data-stu-id="83de1-116">How can RBAC be used</span></span>
<span data-ttu-id="83de1-117">RBAC kan tillämpas på tre olika scope i Azure.</span><span class="sxs-lookup"><span data-stu-id="83de1-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="83de1-118">Från hello högsta omfånget toohello lägsta en är de följande:</span><span class="sxs-lookup"><span data-stu-id="83de1-118">From hello highest scope toohello lowest one, they are as follows:</span></span>

* <span data-ttu-id="83de1-119">Prenumeration (högsta)</span><span class="sxs-lookup"><span data-stu-id="83de1-119">Subscription (highest)</span></span>
* <span data-ttu-id="83de1-120">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="83de1-120">Resource group</span></span>
* <span data-ttu-id="83de1-121">Resursen omfång (hello lägsta åtkomstnivå erbjudande riktade behörigheter tooan enskilda Azure-resurs scope)</span><span class="sxs-lookup"><span data-stu-id="83de1-121">Resource scope (hello lowest access level offering targeted permissions tooan individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a><span data-ttu-id="83de1-122">Tilldela roller RBAC hello prenumeration definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="83de1-122">Assign RBAC roles at hello subscription scope</span></span>
<span data-ttu-id="83de1-123">Det finns två vanliga exempel när RBAC används (men inte begränsat till):</span><span class="sxs-lookup"><span data-stu-id="83de1-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="83de1-124">Med externa användare från organisationer hello (inte ingår i hello admin användarens Azure Active Directory-klient) inbjuden toomanage vissa resurser eller hello hela prenumerationen</span><span class="sxs-lookup"><span data-stu-id="83de1-124">Having external users from hello organizations (not part of hello admin user's Azure Active Directory tenant) invited toomanage certain resources or hello whole subscription</span></span>
* <span data-ttu-id="83de1-125">Arbeta med användare i hello organisation (det är en del av hello användarens Azure Active Directory-klient), men en del av olika grupper eller grupper som behöver åtkomst noggrann antingen toohello scope hela prenumerationen eller toocertain resursgrupper eller resursen i hello miljö</span><span class="sxs-lookup"><span data-stu-id="83de1-125">Working with users inside hello organization (they are part of hello user's Azure Active Directory tenant) but part of different teams or groups which need granular access either toohello whole subscription or toocertain resource groups or resource scopes in hello environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="83de1-126">Bevilja åtkomst till en prenumerationsnivå för en användare utanför Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83de1-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="83de1-127">RBAC-roller som kan beviljas endast av **ägare** hello prenumeration därför hello admin-användare måste vara inloggad med ett användarnamn som har rollen förinställda eller har skapat hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-127">RBAC roles can be granted only by **Owners** of hello subscription therefore hello admin user must be logged with a username which has this role pre-assigned or has created hello Azure subscription.</span></span>

<span data-ttu-id="83de1-128">Från hello Azure-portalen, när du logga in som administratör, Välj ”prenumerationer” och valt hello önskade en.</span><span class="sxs-lookup"><span data-stu-id="83de1-128">From hello Azure portal, after you sign-in as admin, select “Subscriptions” and chose hello desired one.</span></span>
<span data-ttu-id="83de1-129">![prenumerationsbladet i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) som standard om hello administratörsanvändare har köpt hello Azure-prenumeration, hello användare visas som **kontoadministratören**använder den här som hello prenumeration roll.</span><span class="sxs-lookup"><span data-stu-id="83de1-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if hello admin user has purchased hello Azure subscription, hello user will show up as **Account Admin**, this being hello subscription role.</span></span> <span data-ttu-id="83de1-130">Mer information om hello Azure-prenumeration roller finns [lägga till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="83de1-130">For more details on hello Azure subscription roles, see [Add or change Azure administrator roles that manage hello subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="83de1-131">I det här exemplet hello användare ”alflanigan@outlook.com” är hello **ägare** prenumerationen i hello AAD av hello ”utvärderingsversion” klient ”standard klient Azure”.</span><span class="sxs-lookup"><span data-stu-id="83de1-131">In this example, hello user "alflanigan@outlook.com" is hello **Owner** of hello "Free Trial" subscription in hello AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="83de1-132">Eftersom den här användaren är hello skapare av hello Azure-prenumeration med hello inledande Account ”Outlook” (Account = Outlook Live etc.) hello standarddomännamnet för alla andra användare som lagts till i den här klienten kommer att **”@alflaniganuoutlook.onmicrosoft.com”**.</span><span class="sxs-lookup"><span data-stu-id="83de1-132">Since this user is hello creator of hello Azure subscription with hello initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) hello default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="83de1-133">Avsiktligt hello syntaxen för hello ny domän bildas genom att sätta ihop hello användarnamn och domän namnet på hello-användaren som skapade hello klient och lägga till hello tillägget **”. onmicrosoft.com”**.</span><span class="sxs-lookup"><span data-stu-id="83de1-133">By design, hello syntax of hello new domain is formed by putting together hello username and domain name of hello user who created hello tenant and adding hello extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="83de1-134">Dessutom användare kan logga in med ett anpassat domännamn i hello-klienten efter att lägga till och verifierar för hello nya innehavaren.</span><span class="sxs-lookup"><span data-stu-id="83de1-134">Furthermore, users can sign-in with a custom domain name in hello tenant after adding and verifying it for hello new tenant.</span></span> <span data-ttu-id="83de1-135">Mer information om hur tooverify ett anpassat domännamn i Azure Active Directory-klient Se [lägga till en anpassad domän namn tooyour katalog](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="83de1-135">For more details on how tooverify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name tooyour directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="83de1-136">I det här exemplet hello ”standard Azure” klientkatalogen innehåller endast användare med hello domännamnet ”@alflanigan.onmicrosoft.com”.</span><span class="sxs-lookup"><span data-stu-id="83de1-136">In this example, hello "Default tenant Azure" directory contains only users with hello domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="83de1-137">När du har valt hello prenumeration hello administratörsanvändaren måste klicka på **Access Control (IAM)** och sedan **lägga till en ny roll**.</span><span class="sxs-lookup"><span data-stu-id="83de1-137">After selecting hello subscription, hello admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![funktionen IAM för åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![lägga till nya användare i IAM-funktionen åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="83de1-140">hello nästa steg är tilldelade tooselect hello rollen toobe och hello hello RBAC roll ska tilldelas användare.</span><span class="sxs-lookup"><span data-stu-id="83de1-140">hello next step is tooselect hello role toobe assigned and hello user whom hello RBAC role will be assigned to.</span></span> <span data-ttu-id="83de1-141">I hello **rollen** nedrullningsbara menyn hello administratörsanvändaren ser endast hello inbyggda RBAC roller som är tillgängliga i Azure.</span><span class="sxs-lookup"><span data-stu-id="83de1-141">In hello **Role** dropdown menu hello admin user sees only hello built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="83de1-142">Mer detaljerade beskrivningar av varje roll och deras tilldelningsbara scope finns [inbyggda roller för rollbaserad åtkomstkontroll i](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="83de1-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="83de1-143">Hej administratör måste tooadd hello användarens e-postadress hello externa.</span><span class="sxs-lookup"><span data-stu-id="83de1-143">hello admin user then needs tooadd hello email address of hello external user.</span></span> <span data-ttu-id="83de1-144">hello förväntat beteende är för hello extern användare toonot visas i hello befintlig klient.</span><span class="sxs-lookup"><span data-stu-id="83de1-144">hello expected behavior is for hello external user toonot show up in hello existing tenant.</span></span> <span data-ttu-id="83de1-145">När du har bjudits in hello externa användare, han visas under **prenumerationer > Access Control (IAM)** med alla aktuella hello-användare som är tilldelade en RBAC-rollen på hello prenumerationsomfattningen.</span><span class="sxs-lookup"><span data-stu-id="83de1-145">After hello external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all hello current users which are currently assigned an RBAC role at hello Subscription scope.</span></span>





![lägga till behörigheter toonew RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![lista över RBAC-roller på prenumerationsnivån](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="83de1-148">hello användare ”chessercarlton@gmail.com” inbjudna toobe har en **ägare** för hello ”utvärderingsversion” prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-148">hello user "chessercarlton@gmail.com" has been invited toobe an **Owner** for hello “Free Trial” subscription.</span></span> <span data-ttu-id="83de1-149">När du har skickat hello inbjudan får hello externa användaren ett e-postbekräftelse med en aktiveringslänk.</span><span class="sxs-lookup"><span data-stu-id="83de1-149">After sending hello invitation, hello external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="83de1-150">![e-postinbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="83de1-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="83de1-151">Som externa toohello organisation har hello ny användare inte några befintliga attribut i hello ”klient Azure” standardkatalog.</span><span class="sxs-lookup"><span data-stu-id="83de1-151">Being external toohello organization, hello new user does not have any existing attributes in hello "Default tenant Azure" directory.</span></span> <span data-ttu-id="83de1-152">De kommer att skapas när hello extern användare har gett samtycke toobe registreras i hello-katalog som är kopplat till hello-prenumeration som har tilldelats en roll.</span><span class="sxs-lookup"><span data-stu-id="83de1-152">They will be created after hello external user has given consent toobe recorded in hello directory which is associated with hello subscription which he has been assigned a role to.</span></span>





![e-postmeddelande för inbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="83de1-154">hello extern användare visas i hello Azure Active Directory-klient hädanefter som externa användare och det kan visas både i hello Azure-portalen och hello klassiska portal.</span><span class="sxs-lookup"><span data-stu-id="83de1-154">hello external user shows in hello Azure Active Directory tenant from now on as external user and this can be viewed both in hello Azure portal and in hello classic portal.</span></span>





![användare bladet azure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![användare bladet azure active directory klassiska Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="83de1-157">I hello **användare** vyn i både portaler hello externa användare kan identifieras av:</span><span class="sxs-lookup"><span data-stu-id="83de1-157">In hello **Users** view in both portals hello external users can be recognized by:</span></span>

* <span data-ttu-id="83de1-158">hello olika ikoner typen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="83de1-158">hello different icon type in hello Azure portal</span></span>
* <span data-ttu-id="83de1-159">hello olika källor punkten i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="83de1-159">hello different sourcing point in hello classic portal</span></span>

<span data-ttu-id="83de1-160">Dock bevilja **ägare** eller **deltagare** åtkomst tooan externa användare på hello **prenumeration** omfång, tillåter inte hello åtkomst toohello admin användarens directory Om inte hello **Global administratör** tillåter.</span><span class="sxs-lookup"><span data-stu-id="83de1-160">However, granting **Owner** or **Contributor** access tooan external user at hello **Subscription** scope, does not allow hello access toohello admin user's directory, unless hello **Global Admin** allows it.</span></span> <span data-ttu-id="83de1-161">I hello användaren proprieties hello **användartyp** som har två gemensamma parametrar, **medlem** och **gäst** kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="83de1-161">In hello user proprieties,  hello **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="83de1-162">En medlem är en användare som är registrerad i hello directory medan gäst är en inbjuden toohello användarkatalog från en extern källa.</span><span class="sxs-lookup"><span data-stu-id="83de1-162">A member is a user which is registered in hello directory while a guest is a user invited toohello directory from an external source.</span></span> <span data-ttu-id="83de1-163">Mer information finns i [hur till B2B-samarbete användare av Azure Active Directory-administratörer](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="83de1-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="83de1-164">Kontrollera att hello extern användare väljer hello rätt katalog toosign i när du har angett hello autentiseringsuppgifter i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="83de1-164">Make sure that after entering hello credentials in hello portal, hello external user selects hello correct directory toosign-in to.</span></span> <span data-ttu-id="83de1-165">hello kan samma användare ha åtkomst toomultiple kataloger och kan välja någon av dem genom att klicka hello användarnamnet i hello översta högra sidan i hello Azure-portalen och välj sedan hello lämplig katalog hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="83de1-165">hello same user can have access toomultiple directories and can select either one of  them by clicking hello username in hello top right-hand side in hello Azure portal and then choose hello appropriate directory from hello dropdown list.</span></span>

<span data-ttu-id="83de1-166">Samtidigt som gäst i hello directory hello externa användare kan hantera alla resurser för hello Azure-prenumeration, men kan inte komma åt hello directory.</span><span class="sxs-lookup"><span data-stu-id="83de1-166">While being a guest in hello directory, hello external user can manage all resources for hello Azure subscription, but can't access hello directory.</span></span>





![åtkomst till begränsade tooazure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="83de1-168">Azure Active Directory och Azure-prenumeration har inte en underordnad-överordnad relation som andra Azure-resurser (till exempel: virtuella datorer, virtuella nätverk, webbprogram, lagring etc.) med en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="83de1-169">Alla hello senare skapas, hanteras och debiteras enligt en Azure-prenumeration medan en Azure-prenumeration används toomanage hello åtkomst tooan Azure-katalogen.</span><span class="sxs-lookup"><span data-stu-id="83de1-169">All hello latter is created, managed and billed under an Azure subscription while an Azure subscription is used toomanage hello access tooan Azure directory.</span></span> <span data-ttu-id="83de1-170">Mer information finns i [hur Azure-prenumerationen är relaterade tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="83de1-170">For more details, see [How an Azure subscription is related tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="83de1-171">Från alla hello inbyggda RBAC roller, **ägare** och **deltagare** erbjuda full tillgång till tooall resurser i hello miljö, hello skillnaden som som en deltagare som inte kan skapa och ta bort nya RBAC-roller.</span><span class="sxs-lookup"><span data-stu-id="83de1-171">From all hello built-in RBAC roles, **Owner** and **Contributor** offer full management access tooall resources in hello environment, hello difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="83de1-172">hello andra inbyggda roller som **Virtual Machine-deltagare** och ger fullständig åtkomst bara toohello resurser som anges av hello-namnet, oavsett hello **resursgruppen** de skapas i.</span><span class="sxs-lookup"><span data-stu-id="83de1-172">hello other built-in roles like **Virtual Machine Contributor** offer full management access only toohello resources indicated by hello name, regardless of hello **Resource Group** they are being created into.</span></span>

<span data-ttu-id="83de1-173">Tilldela hello inbyggda RBAC roll **Virtual Machine-deltagare** innebär hello användartilldelade hello rollen på en på prenumerationsnivå:</span><span class="sxs-lookup"><span data-stu-id="83de1-173">Assigning hello built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that hello user assigned hello role:</span></span>

* <span data-ttu-id="83de1-174">Kan visa alla virtuella datorer oavsett deras distribution datum och hello resursgrupper de ingår i</span><span class="sxs-lookup"><span data-stu-id="83de1-174">Can view all virtual machines regardless their deployment date and hello resource groups they are part of</span></span>
* <span data-ttu-id="83de1-175">Har fullständig åtkomst toohello virtuella hanteringsenheter hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="83de1-175">Has full management access toohello virtual machines in hello subscription</span></span>
* <span data-ttu-id="83de1-176">Kan inte visa några andra typer av resurser i hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="83de1-176">Can't view any other resource types in hello subscription</span></span>
* <span data-ttu-id="83de1-177">Fungerar inte ändringar ur fakturering</span><span class="sxs-lookup"><span data-stu-id="83de1-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="83de1-178">RBAC som en portal endast funktion i Azure, ger inte den åtkomst toohello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="83de1-178">RBAC being an Azure portal only feature, it doesn't grant access toohello classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a><span data-ttu-id="83de1-179">Tilldela en inbyggda RBAC rollen tooan externa användare</span><span class="sxs-lookup"><span data-stu-id="83de1-179">Assign a built-in RBAC role tooan external user</span></span>
<span data-ttu-id="83de1-180">Ett annat scenario i det här testet hello i externa användare ”alflanigan@gmail.com” läggs till som en **Virtual Machine-deltagare**.</span><span class="sxs-lookup"><span data-stu-id="83de1-180">For a different scenario in this test, hello external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![inbyggda deltagarrollen virtuell dator](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="83de1-182">hello normalt beteende för den här externa användare med den här inbyggda rollen är toosee och hantera endast virtuella datorer och deras intilliggande Resource Manager endast resurser som är nödvändiga vid distribution.</span><span class="sxs-lookup"><span data-stu-id="83de1-182">hello normal behavior for this external user with this built-in role is toosee and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="83de1-183">Avsiktligt rollerna begränsad erbjuda åtkomst endast motsvarande tootheir resurser som du skapade i hello Azure-portalen, oavsett vissa fortfarande distribueras i hello samt den klassiska portalen (till exempel: virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="83de1-183">By design, these limited roles offer access only tootheir correspondent resources created in hello Azure portal, regardless some can still be deployed in hello classic portal as well (for example: virtual machines).</span></span>





![deltagare rollen översikt över virtuella datorer i azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a><span data-ttu-id="83de1-185">Bevilja åtkomst till på en prenumerationsnivå för en användare i hello samma katalog</span><span class="sxs-lookup"><span data-stu-id="83de1-185">Grant access at a subscription level for a user in hello same directory</span></span>
<span data-ttu-id="83de1-186">processflöde för hello är identiska tooadding en extern användare både från hello perspektiv beviljande hello RBAC administratörsroll samt hello användaren beviljas åtkomst toohello roll.</span><span class="sxs-lookup"><span data-stu-id="83de1-186">hello process flow is identical tooadding an external user, both from hello admin perspective granting hello RBAC role as well as hello user being granted access toohello role.</span></span> <span data-ttu-id="83de1-187">hello skillnaden här är hello uppmanas användaren inte får någon e-inbjudningar som alla hello resurs scope inom hello prenumeration blir tillgängliga i hello instrumentpanelen efter inloggningen.</span><span class="sxs-lookup"><span data-stu-id="83de1-187">hello difference here is that hello invited user will not receive any email invitations as all hello resource scopes within hello subscription will be available in hello dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a><span data-ttu-id="83de1-188">Tilldela RBAC-roller på hello resurs Gruppomfång</span><span class="sxs-lookup"><span data-stu-id="83de1-188">Assign RBAC roles at hello resource group scope</span></span>
<span data-ttu-id="83de1-189">Tilldela en RBAC-rollen på en **resursgruppen** scope har en identisk process för att tilldela hello-rollen på hello prenumerationsnivån för båda typer av användare – extern eller intern (en del av hello samma katalog).</span><span class="sxs-lookup"><span data-stu-id="83de1-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning hello role at hello subscription level, for both types of users - either external or internal (part of hello same directory).</span></span> <span data-ttu-id="83de1-190">hello användare som är tilldelade hello RBAC roll är toosee i sina miljöer bara hello resursgruppen de har tilldelats åtkomst från hello **resursgrupper** ikon i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="83de1-190">hello users which are assigned hello RBAC role is toosee in their environment only hello resource group they have been assigned access from hello **Resource Groups** icon in hello Azure portal.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-scope"></a><span data-ttu-id="83de1-191">Tilldela roller RBAC hello resurs definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="83de1-191">Assign RBAC roles at hello resource scope</span></span>
<span data-ttu-id="83de1-192">Tilldela en roll RBAC definitionsområdet resurs i Azure har en identisk process för att tilldela hello-rollen på hello prenumerationsnivån eller på hello resursgruppsnivå, följande hello samma arbetsflöde för båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="83de1-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning hello role at hello subscription level or at hello resource group level, following hello same workflow for both scenarios.</span></span> <span data-ttu-id="83de1-193">Hello-användare som är tilldelade hello RBAC roll kan finns endast hello objekt som de har tilldelats åtkomst, antingen i hello **alla resurser** fliken eller direkt i sina instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="83de1-193">Again, hello users which are assigned hello RBAC role can see only hello items that they have been assigned access to, either in hello **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="83de1-194">En viktig aspekt för RBAC både på resursen Gruppomfång eller resurs scope är för hello användare toomake toohello toosign Kontrollera i rätt katalog.</span><span class="sxs-lookup"><span data-stu-id="83de1-194">An important aspect for RBAC both at resource group scope or resource scope is for hello users toomake sure toosign-in toohello correct directory.</span></span>





![Directory inloggning i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="83de1-196">Tilldela RBAC-roller för en Azure Active Directory-grupp</span><span class="sxs-lookup"><span data-stu-id="83de1-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="83de1-197">Alla hello scenarier med RBAC på hello tre olika scope i Azure-erbjudande hello behörighet för att hantera, distribuera och administrera olika resurser som tilldelats användare utan hello behöver för att hantera en personlig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-197">All hello scenarios using RBAC at hello three different scopes in Azure offer hello privilege of managing, deploying and administering various resources as an assigned user without hello need of managing a personal subscription.</span></span> <span data-ttu-id="83de1-198">Oavsett hello RBAC roll har tilldelats för en prenumeration, resursgrupp eller resurs scope, alla hello resurser skapas ytterligare på av hello tilldelade användare debiteras under hello en Azure-prenumeration där hello användare har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="83de1-198">Regardless hello RBAC role is assigned for a subscription, resource group or resource scope, all hello resources created further on by hello assigned users are billed under hello one Azure subscription where hello users have access to.</span></span> <span data-ttu-id="83de1-199">Det här sättet hello användare som har administratörsbehörighet för att hela Azure-prenumeration fakturering har en fullständig översikt på hello-förbrukning, oavsett vem som hanterar hello resurser.</span><span class="sxs-lookup"><span data-stu-id="83de1-199">This way, hello users who have billing administrator permissions for that entire Azure subscription has a complete overview on hello consumption, regardless who is managing hello resources.</span></span>

<span data-ttu-id="83de1-200">För större organisationer RBAC-roller kan användas i hello samma sätt för Azure Active Directory-grupper som överväger hello perspektiv hello administratörsanvändaren vill toogrant hello detaljerade åtkomst för team eller hela avdelningar inte individuellt för varje användare, vilket med tanke på det som ett mycket tid och hantering av effektivt alternativ.</span><span class="sxs-lookup"><span data-stu-id="83de1-200">For larger organizations, RBAC roles can be applied in hello same way for Azure Active Directory groups considering hello perspective that hello admin user wants toogrant hello granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="83de1-201">tooillustrate det här exemplet, hello **deltagare** roll har lagts till tooone hello grupper i hello-klient på hello prenumerationsnivå.</span><span class="sxs-lookup"><span data-stu-id="83de1-201">tooillustrate this example, hello **Contributor** role has been added tooone of hello groups in hello tenant at hello subscription level.</span></span>





![Lägg till RBAC-rollen för AAD-grupper](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="83de1-203">Dessa grupper är säkerhetsgrupper som etableras och hanteras i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="83de1-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a><span data-ttu-id="83de1-204">Skapa en anpassad RBAC rollen tooopen support-begäranden med PowerShell</span><span class="sxs-lookup"><span data-stu-id="83de1-204">Create a custom RBAC role tooopen support requests using PowerShell</span></span>
<span data-ttu-id="83de1-205">hello inbyggda RBAC roller som är tillgängliga i Azure kontrollera vissa behörighetsnivåer baserat på hello tillgängliga resurser i hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="83de1-205">hello built-in RBAC roles which are available in Azure ensure certain permission levels based on hello available resources in hello environment.</span></span> <span data-ttu-id="83de1-206">Om ingen av dessa roller passar hello administratörsanvändare, är det dock hello alternativet toolimit åtkomst ännu mer genom att skapa anpassade RBAC-roller.</span><span class="sxs-lookup"><span data-stu-id="83de1-206">However, if none of these roles suit hello admin user's needs, there is hello option toolimit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="83de1-207">När du skapar anpassade RBAC-roller måste tootake en inbyggd roll, redigera och importera den tillbaka i hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="83de1-207">Creating custom RBAC roles requires tootake one built-in role, edit it and then import it back in hello environment.</span></span> <span data-ttu-id="83de1-208">hello hämtningen och överföring av hello rollen hanteras med PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="83de1-208">hello download and upload of hello role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="83de1-209">Det är viktigt toounderstand hello förutsättningar för att skapa en anpassad roll som kan ge detaljerade åtkomst på hello prenumerationsnivå och även att hello uppmanas användaren hello flexibilitet att öppna supportärenden.</span><span class="sxs-lookup"><span data-stu-id="83de1-209">It is important toounderstand hello prerequisites of creating a custom role which can grant granular access at hello subscription level and also allow hello invited user hello flexibility of opening support requests.</span></span>

<span data-ttu-id="83de1-210">För exempel hello inbyggda rollen **Reader** vilket gör att användare åtkomst tooview alla hello resurs scope men inte tooedit dem eller skapa nya har anpassat tooallow hello användaren hello möjligheten att öppna supportärenden.</span><span class="sxs-lookup"><span data-stu-id="83de1-210">For this example hello built-in role **Reader** which allows users access tooview all hello resource scopes but not tooedit them or create new ones has been customized tooallow hello user hello option of opening support requests.</span></span>

<span data-ttu-id="83de1-211">Hej första åtgärd för att exportera hello **Reader** roll måste toobe slutförts i PowerShell kördes med förhöjda behörigheter som administratör.</span><span class="sxs-lookup"><span data-stu-id="83de1-211">hello first action of exporting hello **Reader** role needs toobe completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![PowerShell skärmbild för läsare RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="83de1-213">Du måste sedan tooextract hello JSON-mall för hello roll.</span><span class="sxs-lookup"><span data-stu-id="83de1-213">Then you need tooextract hello JSON template of hello role.</span></span>





![JSON-mall för anpassad Reader RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="83de1-215">En typisk RBAC-rollen består av tre huvudavsnitt **åtgärder**, **NotActions** och **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="83de1-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="83de1-216">I hello **åtgärd** avsnitt visas alla hello tillåtna åtgärder för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="83de1-216">In hello **Action** section are listed all hello permitted operations for this role.</span></span> <span data-ttu-id="83de1-217">Det är viktigt toounderstand som tilldelas varje åtgärd från en resursleverantör.</span><span class="sxs-lookup"><span data-stu-id="83de1-217">It's important toounderstand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="83de1-218">I detta fall för att skapa stöd biljetter hello **Microsoft.Support** resursprovidern måste anges.</span><span class="sxs-lookup"><span data-stu-id="83de1-218">In this case, for creating support tickets hello **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="83de1-219">toobe kan toosee alla hello resursproviders tillgänglig och registrerade i din prenumeration kan du använda PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83de1-219">toobe able toosee all hello resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="83de1-220">Dessutom kan du hello alla hello tillgängliga PowerShell-cmdlets toomanage hello providrar.</span><span class="sxs-lookup"><span data-stu-id="83de1-220">Additionally, you can check for hello all hello available PowerShell cmdlets toomanage hello resource providers.</span></span>
    <span data-ttu-id="83de1-221">![PowerShell skärmbild för providern resurshantering](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="83de1-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="83de1-222">alla hello åtgärder för en viss RBAC roll resurs providers visas under hello avsnittet toorestrict **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="83de1-222">toorestrict all hello actions for a particular RBAC role, resource providers are listed under hello section **NotActions**.</span></span>
<span data-ttu-id="83de1-223">Senast, är det obligatoriska hello RBAC rollen innehåller hello explicit prenumerations-ID: N där den används.</span><span class="sxs-lookup"><span data-stu-id="83de1-223">Last, it's mandatory that hello RBAC role contains hello explicit subscription IDs where it is used.</span></span> <span data-ttu-id="83de1-224">hello prenumerations-ID: N visas under hello **AssignableScopes**, annars du tillåts inte tooimport hello roll i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-224">hello subscription IDs are listed under hello **AssignableScopes**, otherwise you will not be allowed tooimport hello role in your subscription.</span></span>

<span data-ttu-id="83de1-225">När du skapar och anpassar hello RBAC roll, måste den ha toobe importerade tillbaka hello miljö.</span><span class="sxs-lookup"><span data-stu-id="83de1-225">After creating and customizing hello RBAC role, it needs toobe imported back hello environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="83de1-226">I det här exemplet är hello eget namn för den här rollen RBAC ”Reader stöd biljetter åtkomstnivå” tillåta hello användaren tooview allt i hello prenumeration och tooopen supportärenden.</span><span class="sxs-lookup"><span data-stu-id="83de1-226">In this example, hello custom name for this RBAC role is "Reader support tickets access level" allowing hello user tooview everything in hello subscription and also tooopen support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="83de1-227">hello bara två inbyggda RBAC-roller tillåter hello åtgärd att supportärenden är **ägare** och **deltagare**.</span><span class="sxs-lookup"><span data-stu-id="83de1-227">hello only two built-in RBAC roles allowing hello action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="83de1-228">För en användare toobe kan tooopen supportärenden, måste han tilldelas en RBAC roll bara definitionsområdet hello prenumeration, eftersom alla supportärenden skapas baserat på en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83de1-228">For a user toobe able tooopen support requests, he must be assigned an RBAC role only at hello subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="83de1-229">Den här nya anpassade rollen har tilldelats tooan användaren från hello samma katalog.</span><span class="sxs-lookup"><span data-stu-id="83de1-229">This new custom role has been assigned tooan user from hello same directory.</span></span>





![Skärmbild av anpassade RBAC-roll som importeras i hello Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Skärmbild av tilldela anpassade importerade RBAC rollen toouser i hello samma katalog](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![Skärmbild av behörigheter för anpassad importerade RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="83de1-233">hello exempel har mer detaljerad tooemphasize hello gränserna för den här anpassade RBAC-rollen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="83de1-233">hello example has been further detailed tooemphasize hello limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="83de1-234">Kan skapa nya supportförfrågningar</span><span class="sxs-lookup"><span data-stu-id="83de1-234">Can create new support requests</span></span>
* <span data-ttu-id="83de1-235">Det går inte att skapa den nya resursen scope (till exempel: virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="83de1-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="83de1-236">Det går inte att skapa nya resursgrupper</span><span class="sxs-lookup"><span data-stu-id="83de1-236">Can't create new resource groups</span></span>





![Skärmbild av anpassade RBAC roll skapar supportärenden](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Skärmbild av anpassad RBAC roll inte kan toocreate virtuella datorer](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Skärmbild av anpassad RBAC roll inte kan toocreate nya RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a><span data-ttu-id="83de1-240">Skapa en anpassad RBAC rollen tooopen support-begäranden som använder Azure CLI</span><span class="sxs-lookup"><span data-stu-id="83de1-240">Create a custom RBAC role tooopen support requests using Azure CLI</span></span>
<span data-ttu-id="83de1-241">Kör på en Mac och utan att behöva åtkomst tooPowerShell, är Azure CLI hello sätt toogo.</span><span class="sxs-lookup"><span data-stu-id="83de1-241">Running on a Mac and without having access tooPowerShell, Azure CLI is hello way toogo.</span></span>

<span data-ttu-id="83de1-242">hello steg toocreate en anpassad roll är hello detsamma, med hello enda undantaget att med hjälp av CLI hello roll inte kan hämtas i en JSON-mall, men den kan visas i hello CLI.</span><span class="sxs-lookup"><span data-stu-id="83de1-242">hello steps toocreate a custom role are hello same, with hello sole exception that using CLI hello role can't be downloaded in a JSON template, but it can be viewed in hello CLI.</span></span>

<span data-ttu-id="83de1-243">Det här exemplet har jag valt hello inbyggda rollen för **säkerhetskopiering Reader**.</span><span class="sxs-lookup"><span data-stu-id="83de1-243">For this example I have chosen hello built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Visa CLI Skärmbild av rollen säkerhetskopiering läsare](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="83de1-245">Redigera hello roll i Visual Studio när du har kopierat hello proprieties i en JSON-mall hello **Microsoft.Support** resursprovidern har lagts till i hello **åtgärder** avsnitt så att användaren kan öppna supportärenden medan toobe en läsare för hello säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="83de1-245">Editing hello role in Visual Studio after copying hello proprieties in a JSON template, hello **Microsoft.Support** resource provider has been added in hello **Actions** sections so that this user can open support requests while continuing toobe a reader for hello backup vaults.</span></span> <span data-ttu-id="83de1-246">Igen det är att nödvändiga tooadd hello prenumerations-ID där den här rollen ska användas i hello **AssignableScopes** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="83de1-246">Again it is necessary tooadd hello subscription ID where this role will be used in hello **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![CLI Skärmbild av Importera anpassad RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="83de1-248">hello nya rollen är nu tillgängligt i hello Azure-portalen och hello assignation processen är hello samma som hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="83de1-248">hello new role is now available in hello Azure portal and hello assignation process is hello same as in hello previous examples.</span></span>





![Azure portal Skärmbild av anpassade RBAC-roll som skapats med hjälp av CLI 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="83de1-250">Från och med hello är senaste Build 2017 hello Azure Cloud-gränssnittet allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="83de1-250">As of hello latest Build 2017, hello Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="83de1-251">Azure Cloud-gränssnittet är ett komplement tooIDE och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="83de1-251">Azure Cloud Shell is a complement tooIDE and hello Azure Portal.</span></span> <span data-ttu-id="83de1-252">Med den här tjänsten får du ett webbaserat gränssnitt som är autentiserad och värdbaserad i Azure och du kan använda den i stället för CLI installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="83de1-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Azure-molnet Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
