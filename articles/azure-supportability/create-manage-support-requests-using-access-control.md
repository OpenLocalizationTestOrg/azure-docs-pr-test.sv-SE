---
title: "aaaAzure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden | Microsoft Docs"
description: "Azure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a><span data-ttu-id="38567-103">Azure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden</span><span class="sxs-lookup"><span data-stu-id="38567-103">Azure Role-Based Access Control (RBAC) toocontrol access rights toocreate and manage support requests</span></span>

<span data-ttu-id="38567-104">[Rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) Aktivera detaljerad åtkomsthantering för Azure.</span><span class="sxs-lookup"><span data-stu-id="38567-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="38567-105">Stöd för begäran skapas i hello Azure-portalen [portal.azure.com](https://portal.azure.com), använder Azure RBAC-modellen toodefine som kan skapa och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="38567-105">Support request creation in hello Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model toodefine who can create and manage support requests.</span></span>
<span data-ttu-id="38567-106">Komma åt genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program för ett visst område som kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="38567-106">Access is granted by assigning hello appropriate RBAC role toousers, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="38567-107">Låt oss ta ett exempel: du kan hantera alla hello resurser under hello resursgrupp som webbplatser, virtuella datorer och undernät som en resurs gruppägare med läsbehörighet hello prenumeration definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="38567-107">Let’s take an example: As a resource group owner with read permissions at hello subscription scope, you can manage all hello resources under hello resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="38567-108">Men när du försöker toocreate en supportbegäran mot hello virtuell datorresurs, får du hello följande fel</span><span class="sxs-lookup"><span data-stu-id="38567-108">However, when you try toocreate a support request against hello virtual machine resource, you encounter hello following error</span></span>

![Prenumerationen fel](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="38567-110">Stöd för hantering av begäran, behöver du skrivbehörighet eller i en roll som har hello stöd för åtgärden Microsoft.Support/* på hello prenumeration scope toobe kan toocreate och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="38567-110">In support request management, you need write permission or a role that has hello Support action Microsoft.Support/* at hello Subscription scope toobe able toocreate and manage support requests.</span></span>

<span data-ttu-id="38567-111">hello följande artikel förklarar hur du kan använda Azures anpassade rollbaserad åtkomstkontroll (RBAC) toocreate och hantera supportärenden i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="38567-111">hello following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) toocreate and manage support requests in hello Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="38567-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="38567-112">Getting Started</span></span>

<span data-ttu-id="38567-113">Med hello-exemplet ovan kan kommer du att kunna toocreate en supportbegäran för din resurs om du har tilldelat en anpassad roll RBAC på hello prenumeration av hello prenumerationsägaren.</span><span class="sxs-lookup"><span data-stu-id="38567-113">Using hello example above, you would be able toocreate a support request for your resource if you were assigned a custom RBAC role on hello subscription by hello subscription owner.</span></span>
<span data-ttu-id="38567-114">[Anpassade RBAC-roller](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) kan skapas med hjälp av Azure PowerShell, Azure-kommandoradsgränssnittet (CLI) och hello REST API.</span><span class="sxs-lookup"><span data-stu-id="38567-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and hello REST API.</span></span>

<span data-ttu-id="38567-115">hello åtgärder egenskapen för en anpassad roll anger hello Azure operations toowhich hello rollen ger åtkomst.</span><span class="sxs-lookup"><span data-stu-id="38567-115">hello actions property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span>
<span data-ttu-id="38567-116">toocreate en anpassad roll för stöd för hantering av begäran hello roll måste ha hello åtgärden Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="38567-116">toocreate a custom role for support request management, hello role must have hello action Microsoft.Support/*</span></span>

<span data-ttu-id="38567-117">Här är ett exempel på en anpassad roll kan du använda toocreate och hantera stöder begäranden.</span><span class="sxs-lookup"><span data-stu-id="38567-117">Here’s an example of a custom role you can use toocreate and manage support requests.</span></span>
<span data-ttu-id="38567-118">Vi har med namnet ”Support begär bidragsgivare” för den här rollen och hur vi refererar toohello anpassad roll i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="38567-118">We’ve named this role “Support Request Contributor” and that’s how we refer toohello custom role in this article.</span></span>

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

<span data-ttu-id="38567-119">Följ stegen i hello [den här videon](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn hur toocreate en anpassad roll för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38567-119">Follow hello steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn how toocreate a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a><span data-ttu-id="38567-120">Skapa och hantera supportärenden i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="38567-120">Create and manage support requests in hello Azure portal</span></span>

<span data-ttu-id="38567-121">Låt oss ta ett exempel – du är hello ägaren av prenumerationen ”Visual Studio MSDN-prenumeration”.</span><span class="sxs-lookup"><span data-stu-id="38567-121">Let’s take an example – you are hello owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="38567-122">Joe är din peer som är en resurs ägare toosome hello resursgrupper i den här prenumerationen och har skrivskyddad behörighet toohello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38567-122">Joe is your peer who is a resource owner toosome of hello resource groups in this subscription and has read permission toohello subscription.</span></span>
<span data-ttu-id="38567-123">Du vill toogive åtkomst tooyour peer, Johan, hello möjlighet toocreate och hantera supportärenden för hello resurser under den här prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="38567-123">You wish toogive access tooyour peer, Joe, hello ability toocreate and manage support tickets for hello resources under this subscription.</span></span>

1. <span data-ttu-id="38567-124">hello första steget är toogo toohello prenumeration under ”inställningar” visas en lista med användare.</span><span class="sxs-lookup"><span data-stu-id="38567-124">hello first step is toogo toohello subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="38567-125">Klicka på hello användare Joe som har reader åtkomst på hello prenumeration och vi tilldela en ny anpassad roll toohim.</span><span class="sxs-lookup"><span data-stu-id="38567-125">Click hello user Joe who has reader access on hello Subscription and let’s assign a new custom role toohim.</span></span>

    ![Lägg till roll](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="38567-127">Klicka på ”Lägg till” under hello ”användare” bladet.</span><span class="sxs-lookup"><span data-stu-id="38567-127">Click "Add" under hello "Users" blade.</span></span> <span data-ttu-id="38567-128">Välj hello anpassad roll ”Support begär bidragsgivare” hello listan över roller</span><span class="sxs-lookup"><span data-stu-id="38567-128">Select hello custom role "Support Request Contributor" from hello list of roles</span></span>

    ![Lägga till stöd för deltagarrollen](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="38567-130">När du har valt hello rollnamn, klicka på ”Lägg till användare” och ange hello Joe e-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="38567-130">After selecting hello role name, click "Add users" and enter hello Joe's email credentials.</span></span> <span data-ttu-id="38567-131">Klicka på ”Välj”</span><span class="sxs-lookup"><span data-stu-id="38567-131">Click "Select"</span></span>

    ![Lägga till användare](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="38567-133">Klicka på ”Ok” tooproceed</span><span class="sxs-lookup"><span data-stu-id="38567-133">Click "Ok" tooproceed</span></span>

    ![Lägg till åtkomst](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="38567-135">Nu ska du se hello användare med hello nytillagda anpassad roll ”Support begär bidragsgivare” under hello prenumeration som du är hello ägare</span><span class="sxs-lookup"><span data-stu-id="38567-135">Now you see hello user with hello newly added custom role "Support Request Contributor" under hello Subscription for which you are hello owner</span></span>

    ![Användare som läggs till](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="38567-137">När Joe loggar in hello portal, ser han hello prenumeration toowhich han har lagts till.</span><span class="sxs-lookup"><span data-stu-id="38567-137">When Joe logs in hello portal, he sees hello subscription toowhich he was added.</span></span>

7. <span data-ttu-id="38567-138">Joe klickar du på ”ny supportbegäran” hello ”hjälp och Support” bladet och kan skapa supportärenden för ”Visual Studio Ultimate med MSDN”</span><span class="sxs-lookup"><span data-stu-id="38567-138">Joe clicks "New Support request" from hello "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Ny supportbegäran](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="38567-140">Klicka på ”alla stöder begäranden” Joe visas hello lista över supportärenden som skapats för den här prenumerationen ![fallet detaljvy](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="38567-140">Clicking "All support requests" Joe can see hello list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-hello-azure-portal"></a><span data-ttu-id="38567-141">Ta bort support begär åtkomst i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="38567-141">Remove support request access in hello Azure portal</span></span>

<span data-ttu-id="38567-142">Precis som det är möjligt toogrant åtkomst tooa användaren toocreate och hantera supportärenden är möjliga tooremove åtkomst för samt hello-användare.</span><span class="sxs-lookup"><span data-stu-id="38567-142">Just as it is possible toogrant access tooa user toocreate and manage support requests, it's possible tooremove access for hello user as well.</span></span>
<span data-ttu-id="38567-143">tooremove hello möjlighet toocreate och hantera supportärenden, gå toohello prenumeration, klicka på ”inställningar” och klicka hello användare (i det här fallet Joe).</span><span class="sxs-lookup"><span data-stu-id="38567-143">tooremove hello ability toocreate and manage support requests, go toohello Subscription, click "Settings" and click hello user (in this case, Joe).</span></span>
<span data-ttu-id="38567-144">Högerklicka på hello rollnamn, ”Support begär bidragsgivare” och klicka på ”Ta bort”</span><span class="sxs-lookup"><span data-stu-id="38567-144">Right-click hello role name, "Support Request Contributor" and click "Remove"</span></span>

![Ta bort stöd för begäran om åtkomst](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="38567-146">När Joe loggar i toohello portal och försöker toocreate en supportbegäran, påträffar han hello följande fel</span><span class="sxs-lookup"><span data-stu-id="38567-146">When Joe logs in toohello portal and tries toocreate a support request, he encounters hello following error</span></span>

![Fel-2-prenumeration](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="38567-148">Johan kan inte se någon stöder begäranden när han klickar ”alla stöder begäranden”</span><span class="sxs-lookup"><span data-stu-id="38567-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![Case information vy-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
