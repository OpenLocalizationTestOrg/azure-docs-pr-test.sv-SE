---
title: "aaaManage developer konton med hjälp av grupper i Azure API Management | Microsoft Docs"
description: "Lär dig hur toomanage developer användarkonton med grupper i Azure API Management"
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
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="d1099-103">Hur toocreate och Använd grupper toomanage developer konton i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d1099-103">How toocreate and use groups toomanage developer accounts in Azure API Management</span></span>
<span data-ttu-id="d1099-104">Grupper finns i API Management används toomanage hello synligheten för produkter toodevelopers.</span><span class="sxs-lookup"><span data-stu-id="d1099-104">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="d1099-105">Produkter är första gjorts synliga toogroups och utvecklare i dessa grupper kan sedan visa och prenumerera toohello produkter som är associerade med hello grupper.</span><span class="sxs-lookup"><span data-stu-id="d1099-105">Products are first made visible toogroups, and then developers in those groups can view and subscribe toohello products that are associated with hello groups.</span></span> 

<span data-ttu-id="d1099-106">API Management har hello följande ändras system grupper.</span><span class="sxs-lookup"><span data-stu-id="d1099-106">API Management has hello following immutable system groups.</span></span>

* <span data-ttu-id="d1099-107">**Administratörer** – Administratörer av Azure-prenumerationer är medlemmar i den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="d1099-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="d1099-108">Administratörer hantera API Management-tjänstinstanser skapar hello-API: er, åtgärder och produkter som används av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="d1099-108">Administrators manage API Management service instances, creating hello APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="d1099-109">**Utvecklare** – Autentiserade användare av utvecklarportalen hör till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="d1099-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="d1099-110">Utvecklare är hello-kunder som skapar program med hjälp av dina API: er.</span><span class="sxs-lookup"><span data-stu-id="d1099-110">Developers are hello customers that build applications using your APIs.</span></span> <span data-ttu-id="d1099-111">Utvecklare har beviljats åtkomst toohello developer-portalen och bygga program som anropar hello driften av ett API.</span><span class="sxs-lookup"><span data-stu-id="d1099-111">Developers are granted access toohello developer portal and build applications that call hello operations of an API.</span></span>
* <span data-ttu-id="d1099-112">**Gäster** -oautentiserad developer portal användare, till exempel potentiella kunder som besöker hello developer-portalen för en API-hantering instans återgång till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="d1099-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting hello developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="d1099-113">De kan beviljas vissa skrivskyddad åtkomst till exempel hello möjlighet tooview API: er men anropa inte dem.</span><span class="sxs-lookup"><span data-stu-id="d1099-113">They can be granted certain read-only access, such as hello ability tooview APIs but not call them.</span></span>

<span data-ttu-id="d1099-114">Administratörer kan skapa anpassade grupper i tillägg toothese system grupper eller [utnyttja externa grupper i associerade Azure Active Directory-klienter][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="d1099-114">In addition toothese system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="d1099-115">Anpassad och externa grupper kan användas tillsammans med system-grupper i vilket ger utvecklare synlighet och komma åt tooAPI produkter.</span><span class="sxs-lookup"><span data-stu-id="d1099-115">Custom and external groups can be used alongside system groups in giving developers visibility and access tooAPI products.</span></span> <span data-ttu-id="d1099-116">Du kan till exempel skapa en anpassad grupp för utvecklare som är kopplad till en specifik kontopartner-organisation och ge dem åtkomst toohello API: er från en produkt som innehåller relevant API.</span><span class="sxs-lookup"><span data-stu-id="d1099-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access toohello APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="d1099-117">En användare kan tillhöra mer än en grupp.</span><span class="sxs-lookup"><span data-stu-id="d1099-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="d1099-118">Den här guiden visar hur administratörer av en instans för API-hantering kan lägga till nya grupper och koppla dem till produkter och utvecklare.</span><span class="sxs-lookup"><span data-stu-id="d1099-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="d1099-119">Dessutom toocreating och hantera grupper i hello publisher portal, du kan skapa och hantera grupper med hello API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.</span><span class="sxs-lookup"><span data-stu-id="d1099-119">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="d1099-120"><a name="create-group"></a>Skapar du en grupp</span><span class="sxs-lookup"><span data-stu-id="d1099-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="d1099-121">toocreate en ny grupp klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d1099-121">toocreate a new group, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="d1099-122">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="d1099-122">This takes you toohello API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="d1099-124">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="d1099-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d1099-125">Klicka på **grupper** från hello **API Management** menyn på hello vänster och klicka sedan på **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="d1099-125">Click **Groups** from hello **API Management** menu on hello left, and then click **Add Group**.</span></span>

![Lägg till ny grupp][api-management-add-group]

<span data-ttu-id="d1099-127">Ange ett unikt namn för hello grupp och en valfri beskrivning och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d1099-127">Enter a unique name for hello group and an optional description, and click **Save**.</span></span>

![Lägg till ny grupp][api-management-add-group-window]

<span data-ttu-id="d1099-129">hello nya gruppen visas i hello grupper fliken tooedit hello **namn** eller **beskrivning** hello-gruppen, klicka hello namn hello i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d1099-129">hello new group is displayed in hello groups tab. tooedit hello **Name** or **Description** of hello group, click hello name of hello group in hello list.</span></span> <span data-ttu-id="d1099-130">toodelete hello-gruppen, klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="d1099-130">toodelete hello group, click **Delete**.</span></span>

![Grupp som har lagts till][api-management-new-group]

<span data-ttu-id="d1099-132">Nu när hello grupp har skapats kan vara den associerad med produkter och utvecklare.</span><span class="sxs-lookup"><span data-stu-id="d1099-132">Now that hello group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="d1099-133"><a name="associate-group-product"></a>Associera en grupp med en produkt</span><span class="sxs-lookup"><span data-stu-id="d1099-133"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="d1099-134">tooassociate en grupp med en produkt, klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på hello namnet på hello önskade produkt.</span><span class="sxs-lookup"><span data-stu-id="d1099-134">tooassociate a group with a product, click **Products** from hello **API Management** menu on hello left, and then click hello name of hello desired product.</span></span>

![Ange synlighet][api-management-add-group-to-product]

<span data-ttu-id="d1099-136">Välj hello **synlighet** fliken tooadd och ta bort grupper och tooview hello aktuella grupper för hello produkten.</span><span class="sxs-lookup"><span data-stu-id="d1099-136">Select hello **Visibility** tab tooadd and remove groups, and tooview hello current groups for hello product.</span></span> <span data-ttu-id="d1099-137">tooadd eller ta bort grupper, markera eller avmarkera kryssrutorna för hello för hello önskad grupper och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d1099-137">tooadd or remove groups, check or uncheck hello checkboxes for hello desired groups and click **Save**.</span></span>

![Ange synlighet][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="d1099-139">tooadd Azure Active Directory-grupper, se [hur tooauthorize developer användarkonton med Azure Active Directory på Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="d1099-139">tooadd Azure Active Directory groups, see [How tooauthorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="d1099-140">tooconfigure grupper från hello **synlighet** för en produkt klickar du på **hantera grupper**.</span><span class="sxs-lookup"><span data-stu-id="d1099-140">tooconfigure groups from hello **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="d1099-141">När en produkt är associerad med en grupp kan kan utvecklare i gruppen visa och prenumerera toohello produkten.</span><span class="sxs-lookup"><span data-stu-id="d1099-141">Once a product is associated with a group, developers in that group can view and subscribe toohello product.</span></span>

## <span data-ttu-id="d1099-142"><a name="associate-group-developer"></a>Associera grupper med utvecklare</span><span class="sxs-lookup"><span data-stu-id="d1099-142"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="d1099-143">tooassociate grupper med utvecklare, klickar på **användare** från hello **API Management** menyn på hello vänster och sedan hello kryssrutan bredvid hello utvecklare gärna tooassociate med en grupp.</span><span class="sxs-lookup"><span data-stu-id="d1099-143">tooassociate groups with developers, click **Users** from hello **API Management** menu on hello left, and then check hello box beside hello developers you wish tooassociate with a group.</span></span>

![Lägg till toogroup för utvecklare][api-management-add-group-to-developer]

<span data-ttu-id="d1099-145">När hello önskad utvecklare kontrolleras, klickar du på önskad hello-grupp i hello **lägga till tooGroup** listrutan.</span><span class="sxs-lookup"><span data-stu-id="d1099-145">Once hello desired developers are checked, click hello desired group in hello **Add tooGroup** drop-down.</span></span> <span data-ttu-id="d1099-146">Utvecklare kan tas bort från grupper med hjälp av hello **ta bort från gruppen** listrutan.</span><span class="sxs-lookup"><span data-stu-id="d1099-146">Developers can be removed from groups by using hello **Remove from Group** drop-down.</span></span> 

![Utvecklare][api-management-add-group-to-developer-saved]

<span data-ttu-id="d1099-148">När hello association läggs mellan hello utvecklare och hello grupp, du kan visa i hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="d1099-148">Once hello association is added between hello developer and hello group, you can view it in hello **Users** tab.</span></span>

## <span data-ttu-id="d1099-149"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1099-149"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="d1099-150">När en utvecklare har lagts till tooa grupp, kan de visa och prenumerera toohello produkter som är associerade med gruppen.</span><span class="sxs-lookup"><span data-stu-id="d1099-150">Once a developer is added tooa group, they can view and subscribe toohello products associated with that group.</span></span> <span data-ttu-id="d1099-151">Mer information finns i [hur skapa och publicera en produkt i Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="d1099-151">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="d1099-152">Dessutom toocreating och hantera grupper i hello publisher portal, du kan skapa och hantera grupper med hello API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.</span><span class="sxs-lookup"><span data-stu-id="d1099-152">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

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
