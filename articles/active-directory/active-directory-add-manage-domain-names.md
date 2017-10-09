---
title: "aaaManaging anpassade domännamn i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="63cd2-103">Hantera anpassade domännamn i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63cd2-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="63cd2-104">Ett domännamn kan vara en viktig identifierare för många directory-resurser som en del av:</span><span class="sxs-lookup"><span data-stu-id="63cd2-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="63cd2-105">Ett användarnamn eller e-postadress för en användare</span><span class="sxs-lookup"><span data-stu-id="63cd2-105">A user name or email address for a user</span></span>
* <span data-ttu-id="63cd2-106">hello-adressen för en grupp</span><span class="sxs-lookup"><span data-stu-id="63cd2-106">hello address for a group</span></span>
* <span data-ttu-id="63cd2-107">hello app-ID URI för ett program</span><span class="sxs-lookup"><span data-stu-id="63cd2-107">hello app ID URI for an application</span></span>

<span data-ttu-id="63cd2-108">En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats toobe ägs av hello katalog som innehåller hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="63cd2-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified toobe owned by hello directory that contains hello resource.</span></span> <span data-ttu-id="63cd2-109">Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63cd2-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="63cd2-110">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="63cd2-110">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="63cd2-111">Hur toomanage ditt domännamn i administrationscentret för hello Azure AD, finns i [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="63cd2-111">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="63cd2-112">Ange namnet på hello primär domän för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="63cd2-112">Set hello primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="63cd2-113">När din katalog har skapats är också hello inledande domännamn, till exempel 'contoso.onmicrosoft.com' hello primära domännamn för din katalog.</span><span class="sxs-lookup"><span data-stu-id="63cd2-113">When your directory is created, hello initial domain name, such as ‘contoso.onmicrosoft.com,’ is also hello primary domain name for your directory.</span></span> <span data-ttu-id="63cd2-114">hello primär domän är hello standarddomännamnet för en ny användare när du skapar en ny användare i hello [klassiska Azure-portalen](https://manage.windowsazure.com/), eller andra portaler, till exempel hello Office 365 administrationsportal.</span><span class="sxs-lookup"><span data-stu-id="63cd2-114">hello primary domain is hello default domain name for a new user when you create a new user in hello [Azure classic portal](https://manage.windowsazure.com/), or other portals such as hello Office 365 admin portal.</span></span> <span data-ttu-id="63cd2-115">Detta förenklar hello process för en administratör toocreate nya användare i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="63cd2-115">This streamlines hello process for an administrator toocreate new users in hello portal.</span></span>

<span data-ttu-id="63cd2-116">toochange hello primära domännamn för din katalog:</span><span class="sxs-lookup"><span data-stu-id="63cd2-116">toochange hello primary domain name for your directory:</span></span>

1. <span data-ttu-id="63cd2-117">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med ett användarkonto som är en global administratör i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="63cd2-117">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="63cd2-118">Välj **Active Directory** hello vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="63cd2-118">Select **Active Directory** on hello left navigation bar.</span></span>
3. <span data-ttu-id="63cd2-119">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="63cd2-119">Open your directory.</span></span>
4. <span data-ttu-id="63cd2-120">Välj hello **domäner** fliken.</span><span class="sxs-lookup"><span data-stu-id="63cd2-120">Select hello **Domains** tab.</span></span>
5. <span data-ttu-id="63cd2-121">Välj hello **ändra primära** hello kommandofältet-knappen.</span><span class="sxs-lookup"><span data-stu-id="63cd2-121">Select hello **Change primary** button on hello command bar.</span></span>
6. <span data-ttu-id="63cd2-122">Välj hello-domän som du vill toobe hello nya primär domän för din katalog.</span><span class="sxs-lookup"><span data-stu-id="63cd2-122">Select hello domain that you want toobe hello new primary domain for your directory.</span></span>

<span data-ttu-id="63cd2-123">Du kan ändra hello primära domännamn för din katalog toobe verifierade domänen som inte är federerat.</span><span class="sxs-lookup"><span data-stu-id="63cd2-123">You can change hello primary domain name for your directory toobe any verified custom domain that is not federated.</span></span> <span data-ttu-id="63cd2-124">Ändra hello primär domän för din katalog ändrar inte hello användarnamn för eventuella befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="63cd2-124">Changing hello primary domain for your directory will not change hello user names for any existing users.</span></span>

## <a name="add-custom-domain-names-tooyour-azure-ad"></a><span data-ttu-id="63cd2-125">Lägg till anpassad domän namn tooyour Azure AD</span><span class="sxs-lookup"><span data-stu-id="63cd2-125">Add custom domain names tooyour Azure AD</span></span>
<span data-ttu-id="63cd2-126">Du kan lägga upp too900 domänen namn tooeach Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="63cd2-126">You can add up too900 custom domain names tooeach Azure AD directory.</span></span> <span data-ttu-id="63cd2-127">Hej processen för[lägga till ytterligare ett eget domännamn](active-directory-add-domain.md) hello samma hello första anpassade domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-127">hello process too[add an additional custom domain name](active-directory-add-domain.md) is hello same for hello first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="63cd2-128">Lägger till underdomäner i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="63cd2-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="63cd2-129">Om du vill tooadd ett tredje nivån domännamn, till exempel 'europe.contoso.com' tooyour directory, bör du först lägga till och kontrollera hello andranivådomän, t.ex contoso.com. hello underdomän verifieras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63cd2-129">If you want tooadd a third-level domain name such as ‘europe.contoso.com’ tooyour directory, you should first add and verify hello second-level domain, such as contoso.com. hello subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="63cd2-130">toosee som hello underdomän som du just lagt har verifierats, uppdatera hello sida i hello webbläsare som visar hello domäner i katalogen.</span><span class="sxs-lookup"><span data-stu-id="63cd2-130">toosee that hello subdomain that you just added has been verified, refresh hello page in hello browser that lists hello domains in your directory.</span></span>

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="63cd2-131">Vilka toodo om du ändrar hello DNS-registratorns för ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="63cd2-131">What toodo if you change hello DNS registrar for your custom domain name</span></span>
<span data-ttu-id="63cd2-132">Om du ändrar hello DNS-registratorns för ditt domännamn kan du fortsätta toouse ditt domännamn med Azure AD själva utan avbrott och utan ytterligare konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="63cd2-132">If you change hello DNS registrar for your custom domain name, you can continue toouse your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="63cd2-133">Om du använder ditt domännamn med Office 365, Intune eller andra tjänster som är beroende av anpassade domännamn i Azure AD läser du toohello dokumentationen för dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="63cd2-133">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer toohello documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="63cd2-134">Ta bort ett anpassat domännamn</span><span class="sxs-lookup"><span data-stu-id="63cd2-134">Delete a custom domain name</span></span>
<span data-ttu-id="63cd2-135">Du kan ta bort ett anpassat domännamn från din Azure AD, om organisationen inte längre använder domännamnet eller om du behöver toouse domännamnet med en annan Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63cd2-135">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need toouse that domain name with another Azure AD.</span></span>

<span data-ttu-id="63cd2-136">toodelete ett anpassat domännamn, måste du först kontrollera att inga resurser i din katalog är beroende av hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-136">toodelete a custom domain name, you must first ensure that no resources in your directory rely on hello domain name.</span></span> <span data-ttu-id="63cd2-137">Du kan inte ta bort ett domännamn från katalogen om:</span><span class="sxs-lookup"><span data-stu-id="63cd2-137">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="63cd2-138">Alla användare har ett användarnamn, e-postadress eller proxyadress som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-138">Any user has a user name, email address, or proxy address that include hello domain name.</span></span>
* <span data-ttu-id="63cd2-139">Valfri grupp har en e-postadress eller proxyadress som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-139">Any group has an email address or proxy address that includes hello domain name.</span></span>
* <span data-ttu-id="63cd2-140">Alla program i din Azure AD har en app-ID-URI som innehåller hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-140">Any application in your Azure AD has an app ID URI that includes hello domain name.</span></span>

<span data-ttu-id="63cd2-141">Du måste ändra eller ta bort alla dessa resurser i Azure AD-katalogen innan du kan ta bort hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="63cd2-141">You must change or delete any such resource in your Azure AD directory before you can delete hello custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a><span data-ttu-id="63cd2-142">Använd PowerShell eller Graph API toomanage domännamn</span><span class="sxs-lookup"><span data-stu-id="63cd2-142">Use PowerShell or Graph API toomanage domain names</span></span>
<span data-ttu-id="63cd2-143">De flesta hanteringsuppgifter för domännamn i Azure Active Directory kan också utföras med Microsoft PowerShell eller via programmering med Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="63cd2-143">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="63cd2-144">Med hjälp av PowerShell toomanage domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="63cd2-144">Using PowerShell toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="63cd2-145">Med hjälp av Graph API toomanage domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="63cd2-145">Using Graph API toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="63cd2-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63cd2-146">Next steps</span></span>
* [<span data-ttu-id="63cd2-147">Lär dig mer om domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="63cd2-147">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="63cd2-148">Hantera egna domännamn</span><span class="sxs-lookup"><span data-stu-id="63cd2-148">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

