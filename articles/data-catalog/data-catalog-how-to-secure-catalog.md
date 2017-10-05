---
title: "Så här säkrar du åtkomst till Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln beskrivs hur data catalog och dess datatillgångar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: 9664a7bc8493b08c8e0797ac6f1b212079829833
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a><span data-ttu-id="49abd-103">Så här säkrar du åtkomst till data catalog och datatillgångar</span><span class="sxs-lookup"><span data-stu-id="49abd-103">How to secure access to data catalog and data assets</span></span>
> [!IMPORTANT]
> <span data-ttu-id="49abd-104">Den här funktionen är endast tillgänglig i standard edition av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="49abd-104">This feature is available only in the standard edition of Azure Data Catalog.</span></span>

<span data-ttu-id="49abd-105">Azure Data Catalog kan du ange vem som kan komma åt data catalog och vilka åtgärder (registrera, lägga till anteckningar, bli ägare) som kan utföras för metadata i katalogen.</span><span class="sxs-lookup"><span data-stu-id="49abd-105">Azure Data Catalog allows you to specify who can access the data catalog and what operations (register, annotate, take ownership) they can perform on metadata in the catalog.</span></span> 

## <a name="catalog-users-and-permissions"></a><span data-ttu-id="49abd-106">Katalogens användare och behörigheter</span><span class="sxs-lookup"><span data-stu-id="49abd-106">Catalog users and permissions</span></span>
<span data-ttu-id="49abd-107">Ge en användare eller grupp åtkomst till en datakatalog och ange behörigheter:</span><span class="sxs-lookup"><span data-stu-id="49abd-107">To give a user or a group the access to a data catalog and set permissions:</span></span>

1. <span data-ttu-id="49abd-108">På den [startsidan i data catalog](http://www.azuredatacatalog.com), klickar du på **inställningar** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="49abd-108">On the [home page of your data catalog](http://www.azuredatacatalog.com),  click **Settings** on the toolbar.</span></span>

    ![data catalog – inställningar](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. <span data-ttu-id="49abd-110">På inställningssidan expanderar den **katalogens användare** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="49abd-110">In the settings page, expand the **Catalog Users** section.</span></span>
    <span data-ttu-id="49abd-111">![Katalogen användare – Lägg till](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="49abd-111">![Catalog Users - Add](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span></span>
3. <span data-ttu-id="49abd-112">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="49abd-112">Click **Add**.</span></span>
4. <span data-ttu-id="49abd-113">Ange den fullständiga **användarnamn** eller namnet på den **säkerhetsgrupp** i i Azure Active Directory (AAD) som är kopplade till katalogen.</span><span class="sxs-lookup"><span data-stu-id="49abd-113">Enter the fully qualified **user name** or name of the **security group** in the Azure Active Directory (AAD) associated with the catalog.</span></span> <span data-ttu-id="49abd-114">Kommatecken (', ') som avgränsare om du lägger till fler än en användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="49abd-114">Use comma (\`,’) as a separator if you are adding more than one user or group.</span></span>
    <span data-ttu-id="49abd-115">![Katalogens användare - användare eller grupper](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span><span class="sxs-lookup"><span data-stu-id="49abd-115">![Catalog Users - users or groups](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span></span>
5. <span data-ttu-id="49abd-116">Tryck på **RETUR** eller **FLIKEN** utanför textrutan.</span><span class="sxs-lookup"><span data-stu-id="49abd-116">Press **ENTER** or **TAB** out of the text box.</span></span> 
6.  <span data-ttu-id="49abd-117">Bekräfta att alla behörigheter (**anteckna**, **registrera**, och **bli ägare**) är tilldelade till dessa användare eller grupper som standard.</span><span class="sxs-lookup"><span data-stu-id="49abd-117">Confirm that all permissions (**Annotate**, **Register**, and **Take Ownership**) are assigned to these users or groups by default.</span></span> <span data-ttu-id="49abd-118">Det vill säga den användare eller grupp kan [registrera datatillgångar]( data-catalog-how-to-register.md), [kommenterar du datatillgångar]( data-catalog-how-to-annotate.md), och [överta ägarskapet för datatillgångar]( data-catalog-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="49abd-118">That is, the user or group can [register data assets]( data-catalog-how-to-register.md), [annotate data assets]( data-catalog-how-to-annotate.md), and [take ownership of data assets]( data-catalog-how-to-manage.md).</span></span> 
    <span data-ttu-id="49abd-119">![Katalogens användare - standardbehörigheter](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span><span class="sxs-lookup"><span data-stu-id="49abd-119">![Catalog Users - default permissions](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span></span>
7.  <span data-ttu-id="49abd-120">Om du vill ge en användare eller grupp bara läsbehörighet till katalogen, avmarkera den **kommentera** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="49abd-120">To give a user or group only the read access to the catalog, clear the **annotate** option for that user or group.</span></span> <span data-ttu-id="49abd-121">När du gör det, användare eller grupp kan inte kommenterar du datatillgångar i katalogen, men de kan visa dem.</span><span class="sxs-lookup"><span data-stu-id="49abd-121">When you do so, the user or group cannot annotate data assets in the catalog but they can view them.</span></span> 
8.  <span data-ttu-id="49abd-122">Om du vill neka en användare eller grupp registrerar datatillgångar, avmarkera den **registrera** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="49abd-122">To deny a user or group from registering data assets, clear the **register** option for that user or group.</span></span>
9.  <span data-ttu-id="49abd-123">Om du vill neka en användare från att bli ägare till en datatillgång, avmarkera den **bli ägare** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="49abd-123">To deny a user from taking ownership of a data asset, clear the **take ownership** option for that user or group.</span></span> 
10. <span data-ttu-id="49abd-124">Om du vill ta bort en användargrupp från katalogens användare klickar du på **x** för användargruppen längst ned i listan.</span><span class="sxs-lookup"><span data-stu-id="49abd-124">To delete a user/group from catalog users, click **x** for the user/group at the bottom of the list.</span></span> 
    <span data-ttu-id="49abd-125">![Katalogen användare – ta bort användaren](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="49abd-125">![Catalog Users - delete user](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="49abd-126">Vi rekommenderar att du lägger till säkerhetsgrupper för att direkt-katalogen användare i stället för att lägga till användare och tilldela behörigheter.</span><span class="sxs-lookup"><span data-stu-id="49abd-126">We recommend that you add security groups to catalog users rather than adding users directly and assign permissions.</span></span> <span data-ttu-id="49abd-127">Sedan kan lägga till användare på de säkerhetsgrupper som matchar deras roller och deras nödvändiga åtkomst till katalogen.</span><span class="sxs-lookup"><span data-stu-id="49abd-127">Then, add users to the security groups that match their roles and their required access to the catalog.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="49abd-128">Speciella överväganden</span><span class="sxs-lookup"><span data-stu-id="49abd-128">Special considerations</span></span>

- <span data-ttu-id="49abd-129">Behörigheter som tilldelats säkerhetsgrupper är additiva.</span><span class="sxs-lookup"><span data-stu-id="49abd-129">The permissions assigned to security groups are additive.</span></span> <span data-ttu-id="49abd-130">En användare finns, i två grupper.</span><span class="sxs-lookup"><span data-stu-id="49abd-130">Say, a user is in two groups.</span></span> <span data-ttu-id="49abd-131">En grupp har kommentera behörigheter och andra grupper inte har kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="49abd-131">One group has annotate permissions and other group does not have annotate permissions.</span></span> <span data-ttu-id="49abd-132">Användaren har sedan kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="49abd-132">Then, user has annotate permissions.</span></span> 
- <span data-ttu-id="49abd-133">Behörigheter som uttryckligen tilldelas en användare åsidosätta behörigheter till grupper som användaren tillhör.</span><span class="sxs-lookup"><span data-stu-id="49abd-133">The permissions assigned explicitly to a user override the permissions assigned to groups to which the user belongs.</span></span> <span data-ttu-id="49abd-134">I föregående exempel, exempelvis du uttryckligen har lagt till katalogen användare och gör att användaren inte tilldela kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="49abd-134">In the previous example, say, you explicitly added the user to catalog users and do not assign annotate permissions.</span></span> <span data-ttu-id="49abd-135">Användaren kan inte lägga till anteckningar datatillgångar trots att användaren är medlem i en grupp som har kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="49abd-135">The user cannot annotate data assets even though the user is a member of a group that does have annotate permissions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49abd-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49abd-136">Next steps</span></span>
- [<span data-ttu-id="49abd-137">Kom igång med Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="49abd-137">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)

