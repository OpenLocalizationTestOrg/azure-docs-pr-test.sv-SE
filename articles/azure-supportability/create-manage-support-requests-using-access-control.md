---
title: "Azure rollbaserad åtkomstkontroll (RBAC) till åtkomstbehörighet att skapa och hantera supportärenden | Microsoft Docs"
description: "Azure rollbaserad åtkomstkontroll (RBAC) till åtkomstbehörighet att skapa och hantera supportärenden"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 20ebd324cbf379980b43d255d468673de2b6d950
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a><span data-ttu-id="fd9ee-103">Azure rollbaserad åtkomstkontroll (RBAC) till åtkomstbehörighet att skapa och hantera supportärenden</span><span class="sxs-lookup"><span data-stu-id="fd9ee-103">Azure Role-Based Access Control (RBAC) to control access rights to create and manage support requests</span></span>

<span data-ttu-id="fd9ee-104">[Rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) Aktivera detaljerad åtkomsthantering för Azure.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="fd9ee-105">Stöd för begäran skapas i Azure-portalen [portal.azure.com](https://portal.azure.com), använder Azure RBAC-modellen för att definiera vem som kan skapa och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-105">Support request creation in the Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model to define who can create and manage support requests.</span></span>
<span data-ttu-id="fd9ee-106">Komma åt genom att tilldela rollen RBAC till användare, grupper och program för ett visst område som kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-106">Access is granted by assigning the appropriate RBAC role to users, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="fd9ee-107">Låt oss ta ett exempel: du kan hantera alla resurser under resursgruppen som webbplatser, virtuella datorer och undernät som en resurs gruppägare med läsbehörighet i omfånget för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-107">Let’s take an example: As a resource group owner with read permissions at the subscription scope, you can manage all the resources under the resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="fd9ee-108">Men när du försöker skapa en supportbegäran mot den virtuella datorresursen får du följande fel</span><span class="sxs-lookup"><span data-stu-id="fd9ee-108">However, when you try to create a support request against the virtual machine resource, you encounter the following error</span></span>

![Prenumerationen fel](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="fd9ee-110">Stöd för hantering av begäran måste du skriva behörighet eller en roll som har stöd för åtgärden Microsoft.Support/* definitionsområdet prenumeration för att kunna skapa och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-110">In support request management, you need write permission or a role that has the Support action Microsoft.Support/* at the Subscription scope to be able to create and manage support requests.</span></span>

<span data-ttu-id="fd9ee-111">I följande artikel som förklarar hur du kan använda Azures anpassade rollbaserad åtkomstkontroll (RBAC) för att skapa och hantera supportärenden i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-111">The following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) to create and manage support requests in the Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="fd9ee-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="fd9ee-112">Getting Started</span></span>

<span data-ttu-id="fd9ee-113">Använda exemplet ovan kan skulle du kunna skapa en supportförfrågan för din resurs om du har tilldelat en anpassad roll RBAC på prenumerationen av prenumerationsägaren.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-113">Using the example above, you would be able to create a support request for your resource if you were assigned a custom RBAC role on the subscription by the subscription owner.</span></span>
<span data-ttu-id="fd9ee-114">[Anpassade RBAC-roller](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) kan skapas med hjälp av Azure PowerShell, Azure-kommandoradsgränssnittet (CLI) och REST-API.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and the REST API.</span></span>

<span data-ttu-id="fd9ee-115">Egenskapen åtgärder för en anpassad roll anger Azure operationer som rollen ger åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-115">The actions property of a custom role specifies the Azure operations to which the role grants access.</span></span>
<span data-ttu-id="fd9ee-116">Om du vill skapa en anpassad roll för stöd för hantering av begäran måste rollen ha åtgärden Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="fd9ee-116">To create a custom role for support request management, the role must have the action Microsoft.Support/*</span></span>

<span data-ttu-id="fd9ee-117">Här är ett exempel på en anpassad roll som du kan använda för att skapa och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-117">Here’s an example of a custom role you can use to create and manage support requests.</span></span>
<span data-ttu-id="fd9ee-118">Vi har med namnet ”Support begär bidragsgivare” för den här rollen och hur vi refererar till den anpassade rollen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-118">We’ve named this role “Support Request Contributor” and that’s how we refer to the custom role in this article.</span></span>

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

<span data-ttu-id="fd9ee-119">Följ stegen som beskrivs i [den här videon](https://www.youtube.com/watch?v=-PaBaDmfwKI) att lära dig hur du skapar en anpassad roll för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-119">Follow the steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) to learn how to create a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a><span data-ttu-id="fd9ee-120">Skapa och hantera supportärenden i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fd9ee-120">Create and manage support requests in the Azure portal</span></span>

<span data-ttu-id="fd9ee-121">Låt oss ta ett exempel – du är ägare till prenumerationen ”Visual Studio MSDN-prenumeration”.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-121">Let’s take an example – you are the owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="fd9ee-122">Joe är din peer som är en resursägare till vissa av resursgrupper i den här prenumerationen och har läsbehörighet för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-122">Joe is your peer who is a resource owner to some of the resource groups in this subscription and has read permission to the subscription.</span></span>
<span data-ttu-id="fd9ee-123">Du vill ge åtkomst till din peer, Johan, möjlighet att skapa och hantera supportärenden för resurser under den här prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-123">You wish to give access to your peer, Joe, the ability to create and manage support tickets for the resources under this subscription.</span></span>

1. <span data-ttu-id="fd9ee-124">Det första steget är att gå till prenumerationen och under ”inställningar” visas en lista med användare.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-124">The first step is to go to the subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="fd9ee-125">Klicka på användare Joe som har reader åtkomst på prenumerationen och vi tilldela en ny anpassad roll i hubben.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-125">Click the user Joe who has reader access on the Subscription and let’s assign a new custom role to him.</span></span>

    ![Lägg till roll](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="fd9ee-127">Klicka på ”Lägg till” under bladet ”användare”.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-127">Click "Add" under the "Users" blade.</span></span> <span data-ttu-id="fd9ee-128">Välj den anpassade rollen som ”Support begär bidragsgivare” i listan över roller</span><span class="sxs-lookup"><span data-stu-id="fd9ee-128">Select the custom role "Support Request Contributor" from the list of roles</span></span>

    ![Lägga till stöd för deltagarrollen](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="fd9ee-130">När du har valt rollnamnet, klicka på ”Lägg till användare” och ange den Joe e-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-130">After selecting the role name, click "Add users" and enter the Joe's email credentials.</span></span> <span data-ttu-id="fd9ee-131">Klicka på ”Välj”</span><span class="sxs-lookup"><span data-stu-id="fd9ee-131">Click "Select"</span></span>

    ![Lägga till användare](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="fd9ee-133">Klicka på ”Ok” om du vill fortsätta</span><span class="sxs-lookup"><span data-stu-id="fd9ee-133">Click "Ok" to proceed</span></span>

    ![Lägg till åtkomst](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="fd9ee-135">Nu ska du se användare med den nyligen tillagda anpassade rollen ”Support begär bidragsgivare” under den prenumeration som du är ägare</span><span class="sxs-lookup"><span data-stu-id="fd9ee-135">Now you see the user with the newly added custom role "Support Request Contributor" under the Subscription for which you are the owner</span></span>

    ![Användare som läggs till](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="fd9ee-137">När Joe loggar in på portalen, ser han prenumeration som han har lagts till.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-137">When Joe logs in the portal, he sees the subscription to which he was added.</span></span>

7. <span data-ttu-id="fd9ee-138">Joe klickar du på ”en ny supportbegäran” från ”Hjälp och Support”-bladet och kan skapa supportärenden för ”Visual Studio Ultimate med MSDN”</span><span class="sxs-lookup"><span data-stu-id="fd9ee-138">Joe clicks "New Support request" from the "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Ny supportbegäran](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="fd9ee-140">Klicka på ”alla stöder begäranden” Joe kan se listan över supportärenden som skapats för den här prenumerationen ![fallet detaljvy](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="fd9ee-140">Clicking "All support requests" Joe can see the list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-the-azure-portal"></a><span data-ttu-id="fd9ee-141">Ta bort stöd för begäran om åtkomst i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fd9ee-141">Remove support request access in the Azure portal</span></span>

<span data-ttu-id="fd9ee-142">Det är möjligt att ta bort åtkomsten för användaren samt precis som det är möjligt att bevilja åtkomst till en användare att skapa och hantera supportärenden.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-142">Just as it is possible to grant access to a user to create and manage support requests, it's possible to remove access for the user as well.</span></span>
<span data-ttu-id="fd9ee-143">Klicka på ”inställningar” och användare (i det här fallet Joe) för att ta bort möjligheten att skapa och hantera supportärenden, gå till prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fd9ee-143">To remove the ability to create and manage support requests, go to the Subscription, click "Settings" and click the user (in this case, Joe).</span></span>
<span data-ttu-id="fd9ee-144">Högerklicka på rollen, ”Support begär bidragsgivare” och klicka på ”Ta bort”</span><span class="sxs-lookup"><span data-stu-id="fd9ee-144">Right-click the role name, "Support Request Contributor" and click "Remove"</span></span>

![Ta bort stöd för begäran om åtkomst](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="fd9ee-146">När Joe loggar in på portalen och försök att skapa en supportbegäran, påträffar han felmeddelande</span><span class="sxs-lookup"><span data-stu-id="fd9ee-146">When Joe logs in to the portal and tries to create a support request, he encounters the following error</span></span>

![Fel-2-prenumeration](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="fd9ee-148">Johan kan inte se någon stöder begäranden när han klickar ”alla stöder begäranden”</span><span class="sxs-lookup"><span data-stu-id="fd9ee-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![Case information vy-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
