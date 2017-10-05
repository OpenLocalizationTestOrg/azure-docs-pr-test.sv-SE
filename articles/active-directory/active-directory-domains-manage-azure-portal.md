---
title: "Hantera anpassade domännamn i Azure Active Directory | Microsoft Docs"
description: "Management-begrepp och instruktioner för att hantera ett domännamn i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: curtand;jeffsta
ms.openlocfilehash: 402c1be07b8ee885ee5341128fb3f419611b924d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="15d17-103">Hantera anpassade domännamn i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="15d17-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="15d17-104">Ett domännamn är en viktig del av identifierare för många katalogresurserna: det är en del av ett användarnamn eller e-postadress för en användare, en del av adressen för en grupp, och kan vara en del av app-ID URI för ett program.</span><span class="sxs-lookup"><span data-stu-id="15d17-104">A domain name is an important part of the identifier for many directory resources: it is part of a user name or email address for a user, part of the address for a group, and can be part of the app ID URI for an application.</span></span> <span data-ttu-id="15d17-105">En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats som ägs av katalogen som innehåller resursen.</span><span class="sxs-lookup"><span data-stu-id="15d17-105">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified as owned by the directory that contains the resource.</span></span> <span data-ttu-id="15d17-106">Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15d17-106">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="15d17-107">Ange namnet på primär domän för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="15d17-107">Set the primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="15d17-108">När din katalog har skapats, är det ursprungliga domännamnet, till exempel contoso.onmicrosoft.com, också namnet på primära domänen.</span><span class="sxs-lookup"><span data-stu-id="15d17-108">When your directory is created, the initial domain name, such as ‘contoso.onmicrosoft.com,’ is also the primary domain name.</span></span> <span data-ttu-id="15d17-109">Den primära domänen är standarddomännamnet för en ny användare när du skapar en ny användare.</span><span class="sxs-lookup"><span data-stu-id="15d17-109">The primary domain is the default domain name for a new user when you create a new user.</span></span> <span data-ttu-id="15d17-110">Inställningsnamn primär domän förenklar processen för en administratör för att skapa nya användare i portalen.</span><span class="sxs-lookup"><span data-stu-id="15d17-110">Setting a primary domain name streamlines the process for an administrator to create new users in the portal.</span></span> <span data-ttu-id="15d17-111">Ändra namnet på primära domänen:</span><span class="sxs-lookup"><span data-stu-id="15d17-111">To change the primary domain name:</span></span>

1. <span data-ttu-id="15d17-112">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="15d17-112">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="15d17-113">Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="15d17-113">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. <span data-ttu-id="15d17-115">På den ***katalognamn*** bladet väljer **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="15d17-115">On the ***directory-name*** blade, select **Domain names**.</span></span>
4. <span data-ttu-id="15d17-116">På den  ***katalognamn* -domännamn** bladet välj domännamnet som du vill se namnet på primära domänen.</span><span class="sxs-lookup"><span data-stu-id="15d17-116">On the ***directory-name* - Domain names** blade, select the domain name you would like to make the primary domain name.</span></span>
5. <span data-ttu-id="15d17-117">På den ***domännamn*** bladet (det vill säga bladet som öppnas som har ett nytt domännamn i namnet), Välj den **gör primär** kommando.</span><span class="sxs-lookup"><span data-stu-id="15d17-117">On the ***domain name*** blade (that is, the blade that opens that has your new domain name in the title), select the **Make primary** command.</span></span> <span data-ttu-id="15d17-118">Bekräfta valet när du uppmanas göra det.</span><span class="sxs-lookup"><span data-stu-id="15d17-118">Confirm your choice when prompted.</span></span>
   
   ![Gör ett domännamn primär](./media/active-directory-domains-manage-azure-portal/make-primary.png)

<span data-ttu-id="15d17-120">Du kan ändra det primära domännamnet för din katalog ska alla verifierade anpassade domäner som inte är federerat.</span><span class="sxs-lookup"><span data-stu-id="15d17-120">You can change the primary domain name for your directory to be any verified custom domain that is not federated.</span></span> <span data-ttu-id="15d17-121">Primär domän för din katalog kommer ändra användarnamn för eventuella befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="15d17-121">Changing the primary domain for your directory will not change the user names for any existing users.</span></span>

## <a name="add-custom-domain-names-to-your-azure-ad"></a><span data-ttu-id="15d17-122">Lägg till anpassade domännamn i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="15d17-122">Add custom domain names to your Azure AD</span></span>
<span data-ttu-id="15d17-123">Du kan lägga till upp till 900 anpassade domännamn varje Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="15d17-123">You can add up to 900 custom domain names to each Azure AD directory.</span></span> <span data-ttu-id="15d17-124">Processen för att [lägga till ytterligare ett eget domännamn](add-custom-domain.md) är densamma för det första anpassade domännamnet.</span><span class="sxs-lookup"><span data-stu-id="15d17-124">The process to [add an additional custom domain name](add-custom-domain.md) is the same for the first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="15d17-125">Lägger till underdomäner i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="15d17-125">Add subdomains of a custom domain</span></span>
<span data-ttu-id="15d17-126">Om du vill lägga till en tredje nivån domännamn, till exempel 'europe.contoso.com' i katalogen bör du först lägga till och kontrollera andranivådomän, t.ex contoso.com. Underdomänen verifieras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15d17-126">If you want to add a third-level domain name such as ‘europe.contoso.com’ to your directory, you should first add and verify the second-level domain, such as contoso.com. The subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="15d17-127">Uppdatera sidan i webbläsaren visas domänerna om du vill se underdomänen som du just lagt har verifierats.</span><span class="sxs-lookup"><span data-stu-id="15d17-127">To see that the subdomain that you just added has been verified, refresh the page in the browser that lists the domains.</span></span>

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="15d17-128">Vad du gör om du ändrar DNS-registratorns för ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="15d17-128">What to do if you change the DNS registrar for your custom domain name</span></span>
<span data-ttu-id="15d17-129">Om du ändrar DNS-registratorns för ditt domännamn kan fortsätta du att använda ditt domännamn med Azure AD själva utan avbrott och utan ytterligare konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="15d17-129">If you change the DNS registrar for your custom domain name, you can continue to use your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="15d17-130">Om du använder ditt domännamn med Office 365, Intune eller andra tjänster som är beroende av anpassade domännamn i Azure AD finns i dokumentationen för dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="15d17-130">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer to the documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="15d17-131">Ta bort ett anpassat domännamn</span><span class="sxs-lookup"><span data-stu-id="15d17-131">Delete a custom domain name</span></span>
<span data-ttu-id="15d17-132">Du kan ta bort ett anpassat domännamn från din Azure AD, om organisationen inte längre använder domännamnet, eller om du behöver använda domännamnet med en annan Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15d17-132">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need to use that domain name with another Azure AD.</span></span>

<span data-ttu-id="15d17-133">Om du vill ta bort ett anpassat domännamn måste kontrollera du först att inga resurser i din katalog är beroende av domännamnet.</span><span class="sxs-lookup"><span data-stu-id="15d17-133">To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name.</span></span> <span data-ttu-id="15d17-134">Du kan inte ta bort ett domännamn från katalogen om:</span><span class="sxs-lookup"><span data-stu-id="15d17-134">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="15d17-135">Alla användare har ett användarnamn, e-postadress eller proxyadress som innehåller namnet på en domän.</span><span class="sxs-lookup"><span data-stu-id="15d17-135">Any user has a user name, email address, or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="15d17-136">Valfri grupp har en e-postadress eller proxyadress som innehåller namnet på en domän.</span><span class="sxs-lookup"><span data-stu-id="15d17-136">Any group has an email address or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="15d17-137">Alla program i din Azure AD har en app-ID-URI som innehåller namnet på en domän.</span><span class="sxs-lookup"><span data-stu-id="15d17-137">Any application in your Azure AD has an app ID URI that includes the domain name.</span></span>

<span data-ttu-id="15d17-138">Du måste ändra eller ta bort alla dessa resurser i Azure AD-katalogen innan du kan ta bort det anpassade domännamnet.</span><span class="sxs-lookup"><span data-stu-id="15d17-138">You must change or delete any such resource in your Azure AD directory before you can delete the custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a><span data-ttu-id="15d17-139">Använd PowerShell eller Graph API för att hantera domännamn</span><span class="sxs-lookup"><span data-stu-id="15d17-139">Use PowerShell or Graph API to manage domain names</span></span>
<span data-ttu-id="15d17-140">De flesta hanteringsuppgifter för domännamn i Azure Active Directory kan också utföras med Microsoft PowerShell eller via programmering med Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="15d17-140">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="15d17-141">Använda PowerShell för att hantera domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="15d17-141">Using PowerShell to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="15d17-142">Hantera domännamn i Azure AD med hjälp av Graph API</span><span class="sxs-lookup"><span data-stu-id="15d17-142">Using Graph API to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="15d17-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15d17-143">Next steps</span></span>
* [<span data-ttu-id="15d17-144">Lägg till anpassade domännamn</span><span class="sxs-lookup"><span data-stu-id="15d17-144">Add custom domain names</span></span>](add-custom-domain.md)

