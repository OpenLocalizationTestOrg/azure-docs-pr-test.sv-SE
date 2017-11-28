---
title: "aaaHow toosecure åtkomst tooAzure Data Catalog | Microsoft Docs"
description: "Den här artikeln förklarar hur toosecure datakatalogen och dess datatillgångar."
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
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a><span data-ttu-id="5d40d-103">Hur toosecure åt toodata katalog- och tillgångar</span><span class="sxs-lookup"><span data-stu-id="5d40d-103">How toosecure access toodata catalog and data assets</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5d40d-104">Den här funktionen är endast tillgänglig i hello standardversionen av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="5d40d-104">This feature is available only in hello standard edition of Azure Data Catalog.</span></span>

<span data-ttu-id="5d40d-105">Azure Data Catalog kan du toospecify som kan komma åt hello data catalog och vad operations (registrera, lägga till anteckningar, bli ägare) som kan utföras för metadata i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5d40d-105">Azure Data Catalog allows you toospecify who can access hello data catalog and what operations (register, annotate, take ownership) they can perform on metadata in hello catalog.</span></span> 

## <a name="catalog-users-and-permissions"></a><span data-ttu-id="5d40d-106">Katalogens användare och behörigheter</span><span class="sxs-lookup"><span data-stu-id="5d40d-106">Catalog users and permissions</span></span>
<span data-ttu-id="5d40d-107">toogive en användare eller en grupp hello öppnar tooa data catalog och ange behörigheter:</span><span class="sxs-lookup"><span data-stu-id="5d40d-107">toogive a user or a group hello access tooa data catalog and set permissions:</span></span>

1. <span data-ttu-id="5d40d-108">På hello [startsidan i data catalog](http://www.azuredatacatalog.com), klickar du på **inställningar** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="5d40d-108">On hello [home page of your data catalog](http://www.azuredatacatalog.com),  click **Settings** on hello toolbar.</span></span>

    ![data catalog – inställningar](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. <span data-ttu-id="5d40d-110">Expandera hello i hello inställningssidan **katalogens användare** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5d40d-110">In hello settings page, expand hello **Catalog Users** section.</span></span>
    <span data-ttu-id="5d40d-111">![Katalogen användare – Lägg till](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="5d40d-111">![Catalog Users - Add](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span></span>
3. <span data-ttu-id="5d40d-112">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5d40d-112">Click **Add**.</span></span>
4. <span data-ttu-id="5d40d-113">Ange hello fullständigt kvalificerade **användarnamn** eller namnet på hello **säkerhetsgrupp** i hello Azure Active Directory (AAD) som är associerade med hello katalog.</span><span class="sxs-lookup"><span data-stu-id="5d40d-113">Enter hello fully qualified **user name** or name of hello **security group** in hello Azure Active Directory (AAD) associated with hello catalog.</span></span> <span data-ttu-id="5d40d-114">Kommatecken (', ') som avgränsare om du lägger till fler än en användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="5d40d-114">Use comma (\`,’) as a separator if you are adding more than one user or group.</span></span>
    <span data-ttu-id="5d40d-115">![Katalogens användare - användare eller grupper](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span><span class="sxs-lookup"><span data-stu-id="5d40d-115">![Catalog Users - users or groups](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span></span>
5. <span data-ttu-id="5d40d-116">Tryck på **RETUR** eller **FLIKEN** utanför hello textruta.</span><span class="sxs-lookup"><span data-stu-id="5d40d-116">Press **ENTER** or **TAB** out of hello text box.</span></span> 
6.  <span data-ttu-id="5d40d-117">Bekräfta att alla behörigheter (**anteckna**, **registrera**, och **bli ägare**) tilldelas toothese användare eller grupper som standard.</span><span class="sxs-lookup"><span data-stu-id="5d40d-117">Confirm that all permissions (**Annotate**, **Register**, and **Take Ownership**) are assigned toothese users or groups by default.</span></span> <span data-ttu-id="5d40d-118">Det vill säga hello användare eller grupp kan [registrera datatillgångar]( data-catalog-how-to-register.md), [kommenterar du datatillgångar]( data-catalog-how-to-annotate.md), och [överta ägarskapet för datatillgångar]( data-catalog-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="5d40d-118">That is, hello user or group can [register data assets]( data-catalog-how-to-register.md), [annotate data assets]( data-catalog-how-to-annotate.md), and [take ownership of data assets]( data-catalog-how-to-manage.md).</span></span> 
    <span data-ttu-id="5d40d-119">![Katalogens användare - standardbehörigheter](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span><span class="sxs-lookup"><span data-stu-id="5d40d-119">![Catalog Users - default permissions](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span></span>
7.  <span data-ttu-id="5d40d-120">toogive en användare eller grupp endast hello läsa åtkomst toohello katalog Rensa hello **kommentera** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="5d40d-120">toogive a user or group only hello read access toohello catalog, clear hello **annotate** option for that user or group.</span></span> <span data-ttu-id="5d40d-121">När du gör det hello användare eller grupp kan inte kommenterar du datatillgångar i hello-katalogen, men de kan visa dem.</span><span class="sxs-lookup"><span data-stu-id="5d40d-121">When you do so, hello user or group cannot annotate data assets in hello catalog but they can view them.</span></span> 
8.  <span data-ttu-id="5d40d-122">toodeny en användare eller grupp registrerar datatillgångar, avmarkera hello **registrera** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="5d40d-122">toodeny a user or group from registering data assets, clear hello **register** option for that user or group.</span></span>
9.  <span data-ttu-id="5d40d-123">toodeny en användare från att bli ägare till en datatillgång, rensa hello **bli ägare** alternativ för användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="5d40d-123">toodeny a user from taking ownership of a data asset, clear hello **take ownership** option for that user or group.</span></span> 
10. <span data-ttu-id="5d40d-124">toodelete användare/grupp från katalogens användare klickar du på **x** för hello användare/grupp hello botten hello-listan.</span><span class="sxs-lookup"><span data-stu-id="5d40d-124">toodelete a user/group from catalog users, click **x** for hello user/group at hello bottom of hello list.</span></span> 
    <span data-ttu-id="5d40d-125">![Katalogen användare – ta bort användaren](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="5d40d-125">![Catalog Users - delete user](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="5d40d-126">Vi rekommenderar att du lägger till säkerhet grupper toocatalog användare i stället för att lägga till användare direkt och tilldela behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5d40d-126">We recommend that you add security groups toocatalog users rather than adding users directly and assign permissions.</span></span> <span data-ttu-id="5d40d-127">Lägg sedan till säkerhetsgrupper för användare toohello som matchar deras roller och deras nödvändiga åtkomsten toohello katalogen.</span><span class="sxs-lookup"><span data-stu-id="5d40d-127">Then, add users toohello security groups that match their roles and their required access toohello catalog.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="5d40d-128">Speciella överväganden</span><span class="sxs-lookup"><span data-stu-id="5d40d-128">Special considerations</span></span>

- <span data-ttu-id="5d40d-129">hello behörigheter toosecurity grupper är additiva.</span><span class="sxs-lookup"><span data-stu-id="5d40d-129">hello permissions assigned toosecurity groups are additive.</span></span> <span data-ttu-id="5d40d-130">En användare finns, i två grupper.</span><span class="sxs-lookup"><span data-stu-id="5d40d-130">Say, a user is in two groups.</span></span> <span data-ttu-id="5d40d-131">En grupp har kommentera behörigheter och andra grupper inte har kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5d40d-131">One group has annotate permissions and other group does not have annotate permissions.</span></span> <span data-ttu-id="5d40d-132">Användaren har sedan kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5d40d-132">Then, user has annotate permissions.</span></span> 
- <span data-ttu-id="5d40d-133">hello behörigheter explicit tooa användaren åsidosättning hello behörigheter toogroups toowhich hello användaren tillhör.</span><span class="sxs-lookup"><span data-stu-id="5d40d-133">hello permissions assigned explicitly tooa user override hello permissions assigned toogroups toowhich hello user belongs.</span></span> <span data-ttu-id="5d40d-134">I föregående exempel hello säger du uttryckligen har lagt till hello toocatalog användare och tilldela inte kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5d40d-134">In hello previous example, say, you explicitly added hello user toocatalog users and do not assign annotate permissions.</span></span> <span data-ttu-id="5d40d-135">hello-användare kan inte lägga till anteckningar datatillgångar även om hello användaren är medlem i en grupp som har kommentera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5d40d-135">hello user cannot annotate data assets even though hello user is a member of a group that does have annotate permissions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d40d-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d40d-136">Next steps</span></span>
- [<span data-ttu-id="5d40d-137">Kom igång med Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="5d40d-137">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)

