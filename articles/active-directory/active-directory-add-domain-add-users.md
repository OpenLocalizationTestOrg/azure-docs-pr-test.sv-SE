---
title: "aaaAssign användare tooa anpassade domäner i Azure Active Directory | Microsoft Docs"
description: "Hur toopopulate en anpassad domän i Azure Active Directory med användarkonton."
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
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a><span data-ttu-id="e059f-103">Tilldela användare tooa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="e059f-103">Assign users tooa custom domain</span></span>
<span data-ttu-id="e059f-104">När du har lagt till din anpassade domän tooAzure Active Directory, måste du lägga till hello användarkonton för den här domänen så att du kan börja autentiseras.</span><span class="sxs-lookup"><span data-stu-id="e059f-104">After you have added your custom domain tooAzure Active Directory, you must add hello user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e059f-105">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e059f-105">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="e059f-106">Hur toomanage ditt domännamn i administrationscentret för hello Azure AD, finns i [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e059f-106">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="e059f-107">Användare som synkroniseras från en lokal katalog</span><span class="sxs-lookup"><span data-stu-id="e059f-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="e059f-108">Om du redan har installerat en anslutning mellan din lokala Active Directory och Azure Active Directory, fylla synkronisering hello konton.</span><span class="sxs-lookup"><span data-stu-id="e059f-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate hello accounts.</span></span> <span data-ttu-id="e059f-109">Mer information om hur toosynchronize Azure Active Directory med din lokala Active Directory, se [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="e059f-109">For more information on how toosynchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-hello-cloud"></a><span data-ttu-id="e059f-110">Användare läggs till och hanteras i hello moln</span><span class="sxs-lookup"><span data-stu-id="e059f-110">Users added and managed in hello cloud</span></span>
<span data-ttu-id="e059f-111">toochange hello domän för ett befintligt användarkonto:</span><span class="sxs-lookup"><span data-stu-id="e059f-111">toochange hello domain for an existing user account:</span></span>

1. <span data-ttu-id="e059f-112">Öppna hello klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.</span><span class="sxs-lookup"><span data-stu-id="e059f-112">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="e059f-113">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="e059f-113">Open your directory.</span></span>
3. <span data-ttu-id="e059f-114">Välj hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="e059f-114">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="e059f-115">Välj hello användare hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e059f-115">Select hello user from hello list.</span></span>
5. <span data-ttu-id="e059f-116">Ändra hello domän för hello användaren och väljer sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="e059f-116">Change hello domain for hello user, and then select **Save**.</span></span>

<span data-ttu-id="e059f-117">Detta kan även göras med hjälp av [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) eller hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="e059f-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="e059f-118">Välj en anpassad domän när du skapar en ny användare</span><span class="sxs-lookup"><span data-stu-id="e059f-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="e059f-119">Öppna hello klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.</span><span class="sxs-lookup"><span data-stu-id="e059f-119">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="e059f-120">Öppna din katalog.</span><span class="sxs-lookup"><span data-stu-id="e059f-120">Open your directory.</span></span>
3. <span data-ttu-id="e059f-121">Välj hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="e059f-121">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="e059f-122">Välj i kommandofältet hello **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e059f-122">In hello command bar, select **Add**.</span></span>
5. <span data-ttu-id="e059f-123">När du lägger till hello användarnamn Välj hello domänen hello domänlistan.</span><span class="sxs-lookup"><span data-stu-id="e059f-123">When you add hello user name, choose hello custom domain from hello domain list.</span></span>
6. <span data-ttu-id="e059f-124">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e059f-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e059f-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e059f-125">Next steps</span></span>
* [<span data-ttu-id="e059f-126">Med hjälp av anpassad domän namn toosimplify hello inloggningen för dina användare</span><span class="sxs-lookup"><span data-stu-id="e059f-126">Using custom domain names toosimplify hello sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="e059f-127">Hantera egna domännamn</span><span class="sxs-lookup"><span data-stu-id="e059f-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="e059f-128">Lär dig mer om domänhanteringsbegrepp i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e059f-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

