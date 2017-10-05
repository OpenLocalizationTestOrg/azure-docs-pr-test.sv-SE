---
title: "Tilldela användare till en anpassad domän i Azure Active Directory | Microsoft Docs"
description: "Hur du fyller i en anpassad domän i Azure Active Directory med användarkonton."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a><span data-ttu-id="6373d-103">Tilldela användare till en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="6373d-103">Assign users to a custom domain</span></span>
<span data-ttu-id="6373d-104">När du har lagt till en anpassad domän till Azure Active Directory, måste du lägga till användarkonton för den här domänen så att du kan börja autentiseras.</span><span class="sxs-lookup"><span data-stu-id="6373d-104">After you have added your custom domain to Azure Active Directory, you must add the user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6373d-105">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6373d-105">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="6373d-106">Att hantera dina domännamn i Azure AD-administrationscentret, se [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6373d-106">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="6373d-107">Användare som synkroniseras från en lokal katalog</span><span class="sxs-lookup"><span data-stu-id="6373d-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="6373d-108">Om du redan har installerat en anslutning mellan din lokala Active Directory och Azure Active Directory, synkronisering kan fylla i kontona.</span><span class="sxs-lookup"><span data-stu-id="6373d-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate the accounts.</span></span> <span data-ttu-id="6373d-109">Mer information om hur du synkroniserar Azure Active Directory med din lokala Active Directory finns [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6373d-109">For more information on how to synchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-the-cloud"></a><span data-ttu-id="6373d-110">Användare läggs till och hanteras i molnet</span><span class="sxs-lookup"><span data-stu-id="6373d-110">Users added and managed in the cloud</span></span>
<span data-ttu-id="6373d-111">Ändra domänen för ett befintligt användarkonto:</span><span class="sxs-lookup"><span data-stu-id="6373d-111">To change the domain for an existing user account:</span></span>

1. <span data-ttu-id="6373d-112">Öppna den klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.</span><span class="sxs-lookup"><span data-stu-id="6373d-112">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="6373d-113">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="6373d-113">Open your directory.</span></span>
3. <span data-ttu-id="6373d-114">Välj fliken **Användare**.</span><span class="sxs-lookup"><span data-stu-id="6373d-114">Select the **Users** tab.</span></span>
4. <span data-ttu-id="6373d-115">Välj användaren i listan.</span><span class="sxs-lookup"><span data-stu-id="6373d-115">Select the user from the list.</span></span>
5. <span data-ttu-id="6373d-116">Ändra domänen för användaren och väljer sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="6373d-116">Change the domain for the user, and then select **Save**.</span></span>

<span data-ttu-id="6373d-117">Detta kan även göras med hjälp av [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) eller [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="6373d-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or the [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="6373d-118">Välj en anpassad domän när du skapar en ny användare</span><span class="sxs-lookup"><span data-stu-id="6373d-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="6373d-119">Öppna den klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.</span><span class="sxs-lookup"><span data-stu-id="6373d-119">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="6373d-120">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="6373d-120">Open your directory.</span></span>
3. <span data-ttu-id="6373d-121">Välj fliken **Användare**.</span><span class="sxs-lookup"><span data-stu-id="6373d-121">Select the **Users** tab.</span></span>
4. <span data-ttu-id="6373d-122">Välj i kommandofältet **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6373d-122">In the command bar, select **Add**.</span></span>
5. <span data-ttu-id="6373d-123">När du lägger till användarnamnet, väljer du den anpassade domänen från listan över domäner.</span><span class="sxs-lookup"><span data-stu-id="6373d-123">When you add the user name, choose the custom domain from the domain list.</span></span>
6. <span data-ttu-id="6373d-124">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6373d-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6373d-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6373d-125">Next steps</span></span>
* [<span data-ttu-id="6373d-126">Använda anpassade domännamn för att förenkla inloggning för dina användare</span><span class="sxs-lookup"><span data-stu-id="6373d-126">Using custom domain names to simplify the sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="6373d-127">Hantera egna domännamn</span><span class="sxs-lookup"><span data-stu-id="6373d-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="6373d-128">Lär dig mer om domänhanteringsbegrepp i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6373d-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

