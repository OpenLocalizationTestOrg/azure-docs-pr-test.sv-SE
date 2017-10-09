---
title: "aaaManaging anpassade domännamn i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 4719524c4e972f6c981db39f016729da13b37670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="57db7-103">Hantera anpassade domännamn i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="57db7-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="57db7-104">Ett domännamn är en viktig del av hello identifierare för många katalogresurserna: det är en del av ett användarnamn eller e-postadress för en användare, en del av hello-adressen för en grupp, och kan vara en del av hello app-ID URI för ett program.</span><span class="sxs-lookup"><span data-stu-id="57db7-104">A domain name is an important part of hello identifier for many directory resources: it is part of a user name or email address for a user, part of hello address for a group, and can be part of hello app ID URI for an application.</span></span> <span data-ttu-id="57db7-105">En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats som ägs av hello katalog som innehåller hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="57db7-105">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified as owned by hello directory that contains hello resource.</span></span> <span data-ttu-id="57db7-106">Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57db7-106">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="57db7-107">Ange namnet på hello primär domän för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="57db7-107">Set hello primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="57db7-108">När din katalog har skapats är hello inledande domännamn, till exempel contoso.onmicrosoft.com, också hello primära domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-108">When your directory is created, hello initial domain name, such as ‘contoso.onmicrosoft.com,’ is also hello primary domain name.</span></span> <span data-ttu-id="57db7-109">hello primär domän är hello standarddomännamnet för en ny användare när du skapar en ny användare.</span><span class="sxs-lookup"><span data-stu-id="57db7-109">hello primary domain is hello default domain name for a new user when you create a new user.</span></span> <span data-ttu-id="57db7-110">Ange namn på en primär domän förenklar hello process för en administratör toocreate nya användare i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="57db7-110">Setting a primary domain name streamlines hello process for an administrator toocreate new users in hello portal.</span></span> <span data-ttu-id="57db7-111">toochange hello primära domännamn:</span><span class="sxs-lookup"><span data-stu-id="57db7-111">toochange hello primary domain name:</span></span>

1. <span data-ttu-id="57db7-112">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="57db7-112">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="57db7-113">Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="57db7-113">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. <span data-ttu-id="57db7-115">På hello ***katalognamn*** bladet väljer **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="57db7-115">On hello ***directory-name*** blade, select **Domain names**.</span></span>
4. <span data-ttu-id="57db7-116">På hello  ***katalognamn* -domännamn** bladet, väljer hello domännamn som du vill att toomake hello primära domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-116">On hello ***directory-name* - Domain names** blade, select hello domain name you would like toomake hello primary domain name.</span></span>
5. <span data-ttu-id="57db7-117">På hello ***domännamn*** bladet (det vill säga hello bladet som öppnas med ett nytt domännamn i hello rubrik) Välj hello **gör primär** kommando.</span><span class="sxs-lookup"><span data-stu-id="57db7-117">On hello ***domain name*** blade (that is, hello blade that opens that has your new domain name in hello title), select hello **Make primary** command.</span></span> <span data-ttu-id="57db7-118">Bekräfta valet när du uppmanas göra det.</span><span class="sxs-lookup"><span data-stu-id="57db7-118">Confirm your choice when prompted.</span></span>
   
   ![Gör ett domännamn primär](./media/active-directory-domains-manage-azure-portal/make-primary.png)

<span data-ttu-id="57db7-120">Du kan ändra hello primära domännamn för din katalog toobe verifierade domänen som inte är federerat.</span><span class="sxs-lookup"><span data-stu-id="57db7-120">You can change hello primary domain name for your directory toobe any verified custom domain that is not federated.</span></span> <span data-ttu-id="57db7-121">Ändra hello primär domän för din katalog ändrar inte hello användarnamn för eventuella befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="57db7-121">Changing hello primary domain for your directory will not change hello user names for any existing users.</span></span>

## <a name="add-custom-domain-names-tooyour-azure-ad"></a><span data-ttu-id="57db7-122">Lägg till anpassad domän namn tooyour Azure AD</span><span class="sxs-lookup"><span data-stu-id="57db7-122">Add custom domain names tooyour Azure AD</span></span>
<span data-ttu-id="57db7-123">Du kan lägga upp too900 domänen namn tooeach Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="57db7-123">You can add up too900 custom domain names tooeach Azure AD directory.</span></span> <span data-ttu-id="57db7-124">Hej processen för[lägga till ytterligare ett eget domännamn](add-custom-domain.md) hello samma hello första anpassade domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-124">hello process too[add an additional custom domain name](add-custom-domain.md) is hello same for hello first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="57db7-125">Lägger till underdomäner i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="57db7-125">Add subdomains of a custom domain</span></span>
<span data-ttu-id="57db7-126">Om du vill tooadd ett tredje nivån domännamn, till exempel 'europe.contoso.com' tooyour directory, bör du först lägga till och kontrollera hello andranivådomän, t.ex contoso.com. hello underdomän verifieras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57db7-126">If you want tooadd a third-level domain name such as ‘europe.contoso.com’ tooyour directory, you should first add and verify hello second-level domain, such as contoso.com. hello subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="57db7-127">toosee som hello underdomän som du just lagt har verifierats, uppdatera hello sida i hello webbläsare som visar hello domäner.</span><span class="sxs-lookup"><span data-stu-id="57db7-127">toosee that hello subdomain that you just added has been verified, refresh hello page in hello browser that lists hello domains.</span></span>

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="57db7-128">Vilka toodo om du ändrar hello DNS-registratorns för ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="57db7-128">What toodo if you change hello DNS registrar for your custom domain name</span></span>
<span data-ttu-id="57db7-129">Om du ändrar hello DNS-registratorns för ditt domännamn kan du fortsätta toouse ditt domännamn med Azure AD själva utan avbrott och utan ytterligare konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="57db7-129">If you change hello DNS registrar for your custom domain name, you can continue toouse your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="57db7-130">Om du använder ditt domännamn med Office 365, Intune eller andra tjänster som är beroende av anpassade domännamn i Azure AD läser du toohello dokumentationen för dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="57db7-130">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer toohello documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="57db7-131">Ta bort ett anpassat domännamn</span><span class="sxs-lookup"><span data-stu-id="57db7-131">Delete a custom domain name</span></span>
<span data-ttu-id="57db7-132">Du kan ta bort ett anpassat domännamn från din Azure AD, om organisationen inte längre använder domännamnet eller om du behöver toouse domännamnet med en annan Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57db7-132">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need toouse that domain name with another Azure AD.</span></span>

<span data-ttu-id="57db7-133">toodelete ett anpassat domännamn, måste du först kontrollera att inga resurser i din katalog är beroende av hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-133">toodelete a custom domain name, you must first ensure that no resources in your directory rely on hello domain name.</span></span> <span data-ttu-id="57db7-134">Du kan inte ta bort ett domännamn från katalogen om:</span><span class="sxs-lookup"><span data-stu-id="57db7-134">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="57db7-135">Alla användare har ett användarnamn, e-postadress eller proxyadress som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-135">Any user has a user name, email address, or proxy address that includes hello domain name.</span></span>
* <span data-ttu-id="57db7-136">Valfri grupp har en e-postadress eller proxyadress som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-136">Any group has an email address or proxy address that includes hello domain name.</span></span>
* <span data-ttu-id="57db7-137">Alla program i din Azure AD har en app-ID-URI som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-137">Any application in your Azure AD has an app ID URI that includes hello domain name.</span></span>

<span data-ttu-id="57db7-138">Du måste ändra eller ta bort alla dessa resurser i Azure AD-katalogen innan du kan ta bort hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="57db7-138">You must change or delete any such resource in your Azure AD directory before you can delete hello custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a><span data-ttu-id="57db7-139">Använd PowerShell eller Graph API toomanage domännamn</span><span class="sxs-lookup"><span data-stu-id="57db7-139">Use PowerShell or Graph API toomanage domain names</span></span>
<span data-ttu-id="57db7-140">De flesta hanteringsuppgifter för domännamn i Azure Active Directory kan också utföras med Microsoft PowerShell eller via programmering med Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="57db7-140">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="57db7-141">Med hjälp av PowerShell toomanage domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="57db7-141">Using PowerShell toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="57db7-142">Med hjälp av Graph API toomanage domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="57db7-142">Using Graph API toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="57db7-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57db7-143">Next steps</span></span>
* [<span data-ttu-id="57db7-144">Lägg till anpassade domännamn</span><span class="sxs-lookup"><span data-stu-id="57db7-144">Add custom domain names</span></span>](add-custom-domain.md)

