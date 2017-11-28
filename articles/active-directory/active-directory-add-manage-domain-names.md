---
title: "Hantera anpassade domännamn i Azure Active Directory | Microsoft Docs"
description: "Management-begrepp och instruktioner för att hantera anpassade domäner i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="934b2-103">Hantera anpassade domännamn i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="934b2-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="934b2-104">Ett domännamn kan vara en viktig identifierare för många directory-resurser som en del av:</span><span class="sxs-lookup"><span data-stu-id="934b2-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="934b2-105">Ett användarnamn eller e-postadress för en användare</span><span class="sxs-lookup"><span data-stu-id="934b2-105">A user name or email address for a user</span></span>
* <span data-ttu-id="934b2-106">Adressen för en grupp</span><span class="sxs-lookup"><span data-stu-id="934b2-106">The address for a group</span></span>
* <span data-ttu-id="934b2-107">App-ID URI för ett program</span><span class="sxs-lookup"><span data-stu-id="934b2-107">The app ID URI for an application</span></span>

<span data-ttu-id="934b2-108">En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats som ägs av den katalog som innehåller resursen.</span><span class="sxs-lookup"><span data-stu-id="934b2-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified to be owned by the directory that contains the resource.</span></span> <span data-ttu-id="934b2-109">Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="934b2-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="934b2-110">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="934b2-110">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="934b2-111">Att hantera dina domännamn i Azure AD-administrationscentret, se [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="934b2-111">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="934b2-112">Ange namnet på primär domän för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="934b2-112">Set the primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="934b2-113">När din katalog har skapats, är det ursprungliga domännamnet, till exempel contoso.onmicrosoft.com, också primära domännamnet för din katalog.</span><span class="sxs-lookup"><span data-stu-id="934b2-113">When your directory is created, the initial domain name, such as ‘contoso.onmicrosoft.com,’ is also the primary domain name for your directory.</span></span> <span data-ttu-id="934b2-114">Den primära domänen är standarddomännamnet för en ny användare när du skapar en ny användare i den [klassiska Azure-portalen](https://manage.windowsazure.com/), eller andra portaler, till exempel administrationsportalen för Office 365.</span><span class="sxs-lookup"><span data-stu-id="934b2-114">The primary domain is the default domain name for a new user when you create a new user in the [Azure classic portal](https://manage.windowsazure.com/), or other portals such as the Office 365 admin portal.</span></span> <span data-ttu-id="934b2-115">Detta förenklar processen för en administratör för att skapa nya användare i portalen.</span><span class="sxs-lookup"><span data-stu-id="934b2-115">This streamlines the process for an administrator to create new users in the portal.</span></span>

<span data-ttu-id="934b2-116">Ändra det primära domännamnet för din katalog:</span><span class="sxs-lookup"><span data-stu-id="934b2-116">To change the primary domain name for your directory:</span></span>

1. <span data-ttu-id="934b2-117">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com/) med ett användarkonto som är en global administratör i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="934b2-117">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="934b2-118">Välj **Active Directory** i det vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="934b2-118">Select **Active Directory** on the left navigation bar.</span></span>
3. <span data-ttu-id="934b2-119">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="934b2-119">Open your directory.</span></span>
4. <span data-ttu-id="934b2-120">Välj den **domäner** fliken.</span><span class="sxs-lookup"><span data-stu-id="934b2-120">Select the **Domains** tab.</span></span>
5. <span data-ttu-id="934b2-121">Välj den **ändra primära** knappen i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="934b2-121">Select the **Change primary** button on the command bar.</span></span>
6. <span data-ttu-id="934b2-122">Välj den domän som du vill att den nya primära domänen för din katalog.</span><span class="sxs-lookup"><span data-stu-id="934b2-122">Select the domain that you want to be the new primary domain for your directory.</span></span>

<span data-ttu-id="934b2-123">Du kan ändra det primära domännamnet för din katalog ska alla verifierade anpassade domäner som inte är federerat.</span><span class="sxs-lookup"><span data-stu-id="934b2-123">You can change the primary domain name for your directory to be any verified custom domain that is not federated.</span></span> <span data-ttu-id="934b2-124">Primär domän för din katalog kommer ändra användarnamn för eventuella befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="934b2-124">Changing the primary domain for your directory will not change the user names for any existing users.</span></span>

## <a name="add-custom-domain-names-to-your-azure-ad"></a><span data-ttu-id="934b2-125">Lägg till anpassade domännamn i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="934b2-125">Add custom domain names to your Azure AD</span></span>
<span data-ttu-id="934b2-126">Du kan lägga till upp till 900 anpassade domännamn varje Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="934b2-126">You can add up to 900 custom domain names to each Azure AD directory.</span></span> <span data-ttu-id="934b2-127">Processen för att [lägga till ytterligare ett eget domännamn](active-directory-add-domain.md) är densamma för det första anpassade domännamnet.</span><span class="sxs-lookup"><span data-stu-id="934b2-127">The process to [add an additional custom domain name](active-directory-add-domain.md) is the same for the first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="934b2-128">Lägger till underdomäner i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="934b2-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="934b2-129">Om du vill lägga till en tredje nivån domännamn, till exempel 'europe.contoso.com' i katalogen bör du först lägga till och kontrollera andranivådomän, t.ex contoso.com.</span><span class="sxs-lookup"><span data-stu-id="934b2-129">If you want to add a third-level domain name such as ‘europe.contoso.com’ to your directory, you should first add and verify the second-level domain, such as contoso.com.</span></span> <span data-ttu-id="934b2-130">Underdomänen verifieras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="934b2-130">The subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="934b2-131">Om du vill se underdomänen som du just lagt har verifierats, uppdatera sidan i webbläsaren visas domänerna i katalogen.</span><span class="sxs-lookup"><span data-stu-id="934b2-131">To see that the subdomain that you just added has been verified, refresh the page in the browser that lists the domains in your directory.</span></span>

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="934b2-132">Vad du gör om du ändrar DNS-registratorns för ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="934b2-132">What to do if you change the DNS registrar for your custom domain name</span></span>
<span data-ttu-id="934b2-133">Om du ändrar DNS-registratorns för ditt domännamn kan fortsätta du att använda ditt domännamn med Azure AD själva utan avbrott och utan ytterligare konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="934b2-133">If you change the DNS registrar for your custom domain name, you can continue to use your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="934b2-134">Om du använder ditt domännamn med Office 365, Intune eller andra tjänster som är beroende av anpassade domännamn i Azure AD finns i dokumentationen för dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="934b2-134">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer to the documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="934b2-135">Ta bort ett anpassat domännamn</span><span class="sxs-lookup"><span data-stu-id="934b2-135">Delete a custom domain name</span></span>
<span data-ttu-id="934b2-136">Du kan ta bort ett anpassat domännamn från din Azure AD, om organisationen inte längre använder domännamnet, eller om du behöver använda domännamnet med en annan Azure AD.</span><span class="sxs-lookup"><span data-stu-id="934b2-136">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need to use that domain name with another Azure AD.</span></span>

<span data-ttu-id="934b2-137">Om du vill ta bort ett anpassat domännamn måste kontrollera du först att inga resurser i din katalog är beroende av domännamnet.</span><span class="sxs-lookup"><span data-stu-id="934b2-137">To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name.</span></span> <span data-ttu-id="934b2-138">Du kan inte ta bort ett domännamn från katalogen om:</span><span class="sxs-lookup"><span data-stu-id="934b2-138">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="934b2-139">Alla användare har ett användarnamn, e-postadress eller proxyadress som inkluderar domännamnet.</span><span class="sxs-lookup"><span data-stu-id="934b2-139">Any user has a user name, email address, or proxy address that include the domain name.</span></span>
* <span data-ttu-id="934b2-140">Valfri grupp har en e-postadress eller proxyadress som innehåller namnet på en domän.</span><span class="sxs-lookup"><span data-stu-id="934b2-140">Any group has an email address or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="934b2-141">Alla program i din Azure AD har en app-ID-URI som innehåller namnet på en domän.</span><span class="sxs-lookup"><span data-stu-id="934b2-141">Any application in your Azure AD has an app ID URI that includes the domain name.</span></span>

<span data-ttu-id="934b2-142">Du måste ändra eller ta bort alla dessa resurser i Azure AD-katalogen innan du kan ta bort det anpassade domännamnet.</span><span class="sxs-lookup"><span data-stu-id="934b2-142">You must change or delete any such resource in your Azure AD directory before you can delete the custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a><span data-ttu-id="934b2-143">Använd PowerShell eller Graph API för att hantera domännamn</span><span class="sxs-lookup"><span data-stu-id="934b2-143">Use PowerShell or Graph API to manage domain names</span></span>
<span data-ttu-id="934b2-144">De flesta hanteringsuppgifter för domännamn i Azure Active Directory kan också utföras med Microsoft PowerShell eller via programmering med Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="934b2-144">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="934b2-145">Använda PowerShell för att hantera domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="934b2-145">Using PowerShell to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="934b2-146">Hantera domännamn i Azure AD med hjälp av Graph API</span><span class="sxs-lookup"><span data-stu-id="934b2-146">Using Graph API to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="934b2-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="934b2-147">Next steps</span></span>
* [<span data-ttu-id="934b2-148">Lär dig mer om domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="934b2-148">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="934b2-149">Hantera egna domännamn</span><span class="sxs-lookup"><span data-stu-id="934b2-149">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

