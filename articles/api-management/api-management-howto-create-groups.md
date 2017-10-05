---
title: "Hantera konton för utvecklare med hjälp av grupper i Azure API Management | Microsoft Docs"
description: "Lär dig att hantera developer konton med hjälp av grupper i Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b4d71cdfbab535b02542fbb26c7555265e5f9c37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="c83d8-103">Hur du skapar och använda grupper för att hantera developer konton i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="c83d8-103">How to create and use groups to manage developer accounts in Azure API Management</span></span>
<span data-ttu-id="c83d8-104">I API Management används grupper för att hantera hur produkter visas för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="c83d8-104">In API Management, groups are used to manage the visibility of products to developers.</span></span> <span data-ttu-id="c83d8-105">Produkter görs först synliga för grupper och sedan utvecklare i dessa grupper kan visa och prenumerera på de produkter som är kopplade till grupperna.</span><span class="sxs-lookup"><span data-stu-id="c83d8-105">Products are first made visible to groups, and then developers in those groups can view and subscribe to the products that are associated with the groups.</span></span> 

<span data-ttu-id="c83d8-106">API Management har följande systemgrupper som inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="c83d8-106">API Management has the following immutable system groups.</span></span>

* <span data-ttu-id="c83d8-107">**Administratörer** – Administratörer av Azure-prenumerationer är medlemmar i den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="c83d8-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="c83d8-108">Administratörer hanterar API Management-tjänstinstanser genom att skapa API:er, åtgärder och produkter som används av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="c83d8-108">Administrators manage API Management service instances, creating the APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="c83d8-109">**Utvecklare** – Autentiserade användare av utvecklarportalen hör till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="c83d8-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="c83d8-110">Utvecklare är de kunder som utvecklar program med hjälp av dina API:er.</span><span class="sxs-lookup"><span data-stu-id="c83d8-110">Developers are the customers that build applications using your APIs.</span></span> <span data-ttu-id="c83d8-111">Utvecklare beviljas åtkomst till utvecklarportalen och bygger program som anropar åtgärderna i ett API.</span><span class="sxs-lookup"><span data-stu-id="c83d8-111">Developers are granted access to the developer portal and build applications that call the operations of an API.</span></span>
* <span data-ttu-id="c83d8-112">**Gäster** – Oautentiserade användare av utvecklarportalen. Potentiella kunder som besöker utvecklarportalen för en API Management-instans hör t.ex. till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="c83d8-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting the developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="c83d8-113">De kan beviljas viss skrivskyddad åtkomst, t.ex. möjligheten att visa API:er men inte anropa dem.</span><span class="sxs-lookup"><span data-stu-id="c83d8-113">They can be granted certain read-only access, such as the ability to view APIs but not call them.</span></span>

<span data-ttu-id="c83d8-114">Förutom grupperna system kan administratörer skapa anpassade grupper eller [utnyttja externa grupper i associerade Azure Active Directory-klienter][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="c83d8-114">In addition to these system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="c83d8-115">Anpassade och externa grupper kan användas tillsammans med systemgrupper för att välja vilka utvecklare som kan se och komma åt API-produkter.</span><span class="sxs-lookup"><span data-stu-id="c83d8-115">Custom and external groups can be used alongside system groups in giving developers visibility and access to API products.</span></span> <span data-ttu-id="c83d8-116">Du kan till exempel skapa en anpassad grupp för utvecklare som hör till en specifik partnerorganisation och ge dem åtkomst till API:erna från en produkt som endast innehåller relevanta API:er.</span><span class="sxs-lookup"><span data-stu-id="c83d8-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access to the APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="c83d8-117">En användare kan tillhöra mer än en grupp.</span><span class="sxs-lookup"><span data-stu-id="c83d8-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="c83d8-118">Den här guiden visar hur administratörer av en instans för API-hantering kan lägga till nya grupper och koppla dem till produkter och utvecklare.</span><span class="sxs-lookup"><span data-stu-id="c83d8-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="c83d8-119">Förutom att skapa och hantera grupper i publisher portal, du kan skapa och hantera grupper med hjälp av API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.</span><span class="sxs-lookup"><span data-stu-id="c83d8-119">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="c83d8-120"><a name="create-group"></a>Skapar du en grupp</span><span class="sxs-lookup"><span data-stu-id="c83d8-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="c83d8-121">Klicka för att skapa en ny grupp **Publisher portal** i Azure Portal för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c83d8-121">To create a new group, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="c83d8-122">När du gör det kommer du till utgivarportalen för API Management.</span><span class="sxs-lookup"><span data-stu-id="c83d8-122">This takes you to the API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="c83d8-124">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="c83d8-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="c83d8-125">Klicka på **grupper** från den **API Management** menyn till vänster och klicka sedan på **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="c83d8-125">Click **Groups** from the **API Management** menu on the left, and then click **Add Group**.</span></span>

![Lägg till ny grupp][api-management-add-group]

<span data-ttu-id="c83d8-127">Ange ett unikt namn för gruppen och en valfri beskrivning och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c83d8-127">Enter a unique name for the group and an optional description, and click **Save**.</span></span>

![Lägg till ny grupp][api-management-add-group-window]

<span data-ttu-id="c83d8-129">Den nya gruppen visas i fliken grupper.</span><span class="sxs-lookup"><span data-stu-id="c83d8-129">The new group is displayed in the groups tab.</span></span> <span data-ttu-id="c83d8-130">Så här redigerar du den **namn** eller **beskrivning** i gruppen, klicka på namnet på gruppen i listan.</span><span class="sxs-lookup"><span data-stu-id="c83d8-130">To edit the **Name** or **Description** of the group, click the name of the group in the list.</span></span> <span data-ttu-id="c83d8-131">Ta bort gruppen, klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c83d8-131">To delete the group, click **Delete**.</span></span>

![Grupp som har lagts till][api-management-new-group]

<span data-ttu-id="c83d8-133">Nu när gruppen skapas kan det vara associerat med produkter och utvecklare.</span><span class="sxs-lookup"><span data-stu-id="c83d8-133">Now that the group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="c83d8-134"><a name="associate-group-product"></a>Associera en grupp med en produkt</span><span class="sxs-lookup"><span data-stu-id="c83d8-134"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="c83d8-135">Koppla en grupp med en produkt genom att klicka på **produkter** från den **API Management** menyn till vänster och klicka sedan på namnet på den önskade produkten.</span><span class="sxs-lookup"><span data-stu-id="c83d8-135">To associate a group with a product, click **Products** from the **API Management** menu on the left, and then click the name of the desired product.</span></span>

![Ange synlighet][api-management-add-group-to-product]

<span data-ttu-id="c83d8-137">Välj den **synlighet** fliken att lägga till och ta bort grupper och för att visa de aktuella grupperna för produkten.</span><span class="sxs-lookup"><span data-stu-id="c83d8-137">Select the **Visibility** tab to add and remove groups, and to view the current groups for the product.</span></span> <span data-ttu-id="c83d8-138">Om du vill lägga till eller ta bort grupper, markera eller avmarkera kryssrutorna för de önskade grupperna och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c83d8-138">To add or remove groups, check or uncheck the checkboxes for the desired groups and click **Save**.</span></span>

![Ange synlighet][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="c83d8-140">Om du vill lägga till Azure Active Directory-grupper, se [så att auktorisera developer konton med hjälp av Azure Active Directory i Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="c83d8-140">To add Azure Active Directory groups, see [How to authorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="c83d8-141">Så här konfigurerar du grupper från den **synlighet** för en produkt klickar du på **hantera grupper**.</span><span class="sxs-lookup"><span data-stu-id="c83d8-141">To configure groups from the **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="c83d8-142">När en produkt är associerad med en grupp kan kan utvecklare i gruppen visa och prenumerera på produkten.</span><span class="sxs-lookup"><span data-stu-id="c83d8-142">Once a product is associated with a group, developers in that group can view and subscribe to the product.</span></span>

## <span data-ttu-id="c83d8-143"><a name="associate-group-developer"></a>Associera grupper med utvecklare</span><span class="sxs-lookup"><span data-stu-id="c83d8-143"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="c83d8-144">Om du vill associera grupper med utvecklare, klickar du på **användare** från den **API Management** menyn till vänster och sedan markera kryssrutan bredvid de utvecklare som du vill associera med en grupp.</span><span class="sxs-lookup"><span data-stu-id="c83d8-144">To associate groups with developers, click **Users** from the **API Management** menu on the left, and then check the box beside the developers you wish to associate with a group.</span></span>

![Lägga till utvecklaren i grupp][api-management-add-group-to-developer]

<span data-ttu-id="c83d8-146">När önskade utvecklare kontrolleras klickar du på önskad grupp i den **lägga till i gruppen** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c83d8-146">Once the desired developers are checked, click the desired group in the **Add to Group** drop-down.</span></span> <span data-ttu-id="c83d8-147">Utvecklare kan tas bort från grupper med hjälp av den **ta bort från gruppen** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c83d8-147">Developers can be removed from groups by using the **Remove from Group** drop-down.</span></span> 

![Utvecklare][api-management-add-group-to-developer-saved]

<span data-ttu-id="c83d8-149">När kopplingen har lagts till mellan utvecklare och gruppen, kan du visa den i den **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="c83d8-149">Once the association is added between the developer and the group, you can view it in the **Users** tab.</span></span>

## <span data-ttu-id="c83d8-150"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c83d8-150"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="c83d8-151">När en utvecklare har lagts till i en grupp, kan de visa och prenumerera på de produkter som är kopplade till den gruppen.</span><span class="sxs-lookup"><span data-stu-id="c83d8-151">Once a developer is added to a group, they can view and subscribe to the products associated with that group.</span></span> <span data-ttu-id="c83d8-152">Mer information finns i [hur skapa och publicera en produkt i Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="c83d8-152">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="c83d8-153">Förutom att skapa och hantera grupper i publisher portal, du kan skapa och hantera grupper med hjälp av API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.</span><span class="sxs-lookup"><span data-stu-id="c83d8-153">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
