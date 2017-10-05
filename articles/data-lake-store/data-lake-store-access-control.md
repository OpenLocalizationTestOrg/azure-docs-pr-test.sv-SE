---
title: "Översikt över åtkomstkontroll i Data Lake Store | Microsoft Docs"
description: "Förstå åtkomstkontroll i Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 99fbad770290d280bdec490d988391ad276ce1ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="access-control-in-azure-data-lake-store"></a><span data-ttu-id="dc9fd-103">Åtkomstkontroll i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dc9fd-103">Access control in Azure Data Lake Store</span></span>

<span data-ttu-id="dc9fd-104">Azure Data Lake Store implementerar en modell för åtkomstkontroll som härstammar från HDFS och i sin tur från POSIX-modellen för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-104">Azure Data Lake Store implements an access control model that derives from HDFS, which in turn derives from the POSIX access control model.</span></span> <span data-ttu-id="dc9fd-105">Den här artikeln sammanfattar grunderna i modellen för åtkomstkontroll för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-105">This article summarizes the basics of the access control model for Data Lake Store.</span></span> <span data-ttu-id="dc9fd-106">Läs mer om HDFS-modellen för åtkomstkontroll i [Guide för HDFS-behörigheter](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span><span class="sxs-lookup"><span data-stu-id="dc9fd-106">To learn more about the HDFS access control model, see [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span></span>

## <a name="access-control-lists-on-files-and-folders"></a><span data-ttu-id="dc9fd-107">Åtkomstkontrollistor för filer och mappar</span><span class="sxs-lookup"><span data-stu-id="dc9fd-107">Access control lists on files and folders</span></span>

<span data-ttu-id="dc9fd-108">Det finns två sorters åtkomstkontrollistor (ACL:er), **Åtkomst-ACL:er** och **Standard-ACL:er**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-108">There are two kinds of access control lists (ACLs), **Access ACLs** and **Default ACLs**.</span></span>

* <span data-ttu-id="dc9fd-109">**Åtkomst-ACL:er**: De här kontrollerar åtkomst till ett objekt.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-109">**Access ACLs**: These control access to an object.</span></span> <span data-ttu-id="dc9fd-110">Både filer och mappar har åtkomst-ACL:er.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-110">Files and folders both have Access ACLs.</span></span>

* <span data-ttu-id="dc9fd-111">**Standard-ACL**: En "mall" av ACL:er som är associerad med en mapp som bestämmer åtkomst-ACL:er för underordnade objekt som skapats under mappen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-111">**Default ACLs**: A "template" of ACLs associated with a folder that determine the Access ACLs for any child items that are created under that folder.</span></span> <span data-ttu-id="dc9fd-112">Filer har inte standard-ACL:er.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-112">Files do not have Default ACLs.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

<span data-ttu-id="dc9fd-114">Både åtkomst-ACL:er och standard-ACL:er har samma struktur.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-114">Both Access ACLs and Default ACLs have the same structure.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> <span data-ttu-id="dc9fd-116">Att ändra standard-ACL på ett överordnat objekt påverkar inte åtkomst-ACL eller standard-ACL för underordnade objekt som redan finns.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-116">Changing the Default ACL on a parent does not affect the Access ACL or Default ACL of child items that already exist.</span></span>
>
>

## <a name="users-and-identities"></a><span data-ttu-id="dc9fd-117">Användare och identiteter</span><span class="sxs-lookup"><span data-stu-id="dc9fd-117">Users and identities</span></span>

<span data-ttu-id="dc9fd-118">Alla filer och mappar har olika behörigheter för dessa identiteter:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-118">Every file and folder has distinct permissions for these identities:</span></span>

* <span data-ttu-id="dc9fd-119">Ägande användare av filen</span><span class="sxs-lookup"><span data-stu-id="dc9fd-119">The owning user of the file</span></span>
* <span data-ttu-id="dc9fd-120">Ägande grupp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-120">The owning group</span></span>
* <span data-ttu-id="dc9fd-121">Namngivna användare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-121">Named users</span></span>
* <span data-ttu-id="dc9fd-122">Namngivna grupper</span><span class="sxs-lookup"><span data-stu-id="dc9fd-122">Named groups</span></span>
* <span data-ttu-id="dc9fd-123">Alla andra användare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-123">All other users</span></span>

<span data-ttu-id="dc9fd-124">Identiteten för användare och grupper är Azure Active Directory (Azure AD)-identiteter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-124">The identities of users and groups are Azure Active Directory (Azure AD) identities.</span></span> <span data-ttu-id="dc9fd-125">Så om inget annat anges kan ha en "användare" i samband med Data Lake Store: antingen betyda en Azure AD-användare eller en Azure AD-säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-125">So unless otherwise noted, a "user," in the context of Data Lake Store, can either mean an Azure AD user or an Azure AD security group.</span></span>

## <a name="permissions"></a><span data-ttu-id="dc9fd-126">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="dc9fd-126">Permissions</span></span>

<span data-ttu-id="dc9fd-127">Behörigheter för ett objekt i filsystemet är **Läsa**, **Skriva** och **Köra** och de kan användas för filer och mappar som visas i tabellen nedan:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-127">The permissions on a filesystem object are **Read**, **Write**, and **Execute**, and they can be used on files and folders as shown in the following table:</span></span>

|            |    <span data-ttu-id="dc9fd-128">Fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-128">File</span></span>     |   <span data-ttu-id="dc9fd-129">Mapp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-129">Folder</span></span> |
|------------|-------------|----------|
| <span data-ttu-id="dc9fd-130">**Läsa (R)**</span><span class="sxs-lookup"><span data-stu-id="dc9fd-130">**Read (R)**</span></span> | <span data-ttu-id="dc9fd-131">Kan läsa innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-131">Can read the contents of a file</span></span> | <span data-ttu-id="dc9fd-132">Kräver **Läsa** och **Köra** för att visa innehållet i mappen</span><span class="sxs-lookup"><span data-stu-id="dc9fd-132">Requires **Read** and **Execute** to list the contents of the folder</span></span>|
| <span data-ttu-id="dc9fd-133">**Skriva (W)**</span><span class="sxs-lookup"><span data-stu-id="dc9fd-133">**Write (W)**</span></span> | <span data-ttu-id="dc9fd-134">Kan skriva eller lägg till i en fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-134">Can write or append to a file</span></span> | <span data-ttu-id="dc9fd-135">Kräver **Skriva** och **Köra** för att skapa underordnade objekt i en mapp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-135">Requires **Write** and **Execute** to create child items in a folder</span></span> |
| <span data-ttu-id="dc9fd-136">**Köra (X)**</span><span class="sxs-lookup"><span data-stu-id="dc9fd-136">**Execute (X)**</span></span> | <span data-ttu-id="dc9fd-137">Innebär inte något i samband med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dc9fd-137">Does not mean anything in the context of Data Lake Store</span></span> | <span data-ttu-id="dc9fd-138">Krävs för att bläddra bland de underordnade objekten i en mapp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-138">Required to traverse the child items of a folder</span></span> |

### <a name="short-forms-for-permissions"></a><span data-ttu-id="dc9fd-139">Kortformat för behörigheter</span><span class="sxs-lookup"><span data-stu-id="dc9fd-139">Short forms for permissions</span></span>

<span data-ttu-id="dc9fd-140">**RWX**används för att ange **läsa + skriva + köra**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-140">**RWX** is used to indicate **Read + Write + Execute**.</span></span> <span data-ttu-id="dc9fd-141">Ett numeriskt mer komprimerat format finns där **Läsa = 4**, **skriva = 2** och **Köra = 1** och deras summa representerar behörigheterna.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-141">A more condensed numeric form exists in which **Read=4**, **Write=2**, and **Execute=1**, the sum of which represents the permissions.</span></span> <span data-ttu-id="dc9fd-142">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-142">Following are some examples.</span></span>

| <span data-ttu-id="dc9fd-143">Numeriskt format</span><span class="sxs-lookup"><span data-stu-id="dc9fd-143">Numeric form</span></span> | <span data-ttu-id="dc9fd-144">Kortformat</span><span class="sxs-lookup"><span data-stu-id="dc9fd-144">Short form</span></span> |      <span data-ttu-id="dc9fd-145">Vad det innebär</span><span class="sxs-lookup"><span data-stu-id="dc9fd-145">What it means</span></span>     |
|--------------|------------|------------------------|
| <span data-ttu-id="dc9fd-146">7</span><span class="sxs-lookup"><span data-stu-id="dc9fd-146">7</span></span>            | <span data-ttu-id="dc9fd-147">RWX</span><span class="sxs-lookup"><span data-stu-id="dc9fd-147">RWX</span></span>        | <span data-ttu-id="dc9fd-148">Läsa + skriva + köra</span><span class="sxs-lookup"><span data-stu-id="dc9fd-148">Read + Write + Execute</span></span> |
| <span data-ttu-id="dc9fd-149">5</span><span class="sxs-lookup"><span data-stu-id="dc9fd-149">5</span></span>            | <span data-ttu-id="dc9fd-150">R-X</span><span class="sxs-lookup"><span data-stu-id="dc9fd-150">R-X</span></span>        | <span data-ttu-id="dc9fd-151">Läsa + köra</span><span class="sxs-lookup"><span data-stu-id="dc9fd-151">Read + Execute</span></span>         |
| <span data-ttu-id="dc9fd-152">4</span><span class="sxs-lookup"><span data-stu-id="dc9fd-152">4</span></span>            | <span data-ttu-id="dc9fd-153">R--</span><span class="sxs-lookup"><span data-stu-id="dc9fd-153">R--</span></span>        | <span data-ttu-id="dc9fd-154">Läsa</span><span class="sxs-lookup"><span data-stu-id="dc9fd-154">Read</span></span>                   |
| <span data-ttu-id="dc9fd-155">0</span><span class="sxs-lookup"><span data-stu-id="dc9fd-155">0</span></span>            | ---        | <span data-ttu-id="dc9fd-156">Inga behörigheter</span><span class="sxs-lookup"><span data-stu-id="dc9fd-156">No permissions</span></span>         |


### <a name="permissions-do-not-inherit"></a><span data-ttu-id="dc9fd-157">Behörigheter ärvs inte</span><span class="sxs-lookup"><span data-stu-id="dc9fd-157">Permissions do not inherit</span></span>

<span data-ttu-id="dc9fd-158">I POSIX-modellen som används av Data Lake Store, förvaras behörigheter för ett objekt i själva objektet.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-158">In the POSIX-style model that's used by Data Lake Store, permissions for an item are stored on the item itself.</span></span> <span data-ttu-id="dc9fd-159">Behörigheter för ett objekt kan med andra ord inte ärvas från de överordnade objekten.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-159">In other words, permissions for an item cannot be inherited from the parent items.</span></span>

## <a name="common-scenarios-related-to-permissions"></a><span data-ttu-id="dc9fd-160">Vanliga scenarier som rör behörigheter</span><span class="sxs-lookup"><span data-stu-id="dc9fd-160">Common scenarios related to permissions</span></span>

<span data-ttu-id="dc9fd-161">Här följer några vanliga scenarier för att förstå vilka behörigheter som krävs för att utföra vissa åtgärder på ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-161">Following are some common scenarios to help you understand which permissions are needed to perform certain operations on a Data Lake Store account.</span></span>

### <a name="permissions-needed-to-read-a-file"></a><span data-ttu-id="dc9fd-162">Behörigheter som krävs för att läsa en fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-162">Permissions needed to read a file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* <span data-ttu-id="dc9fd-164">För den fil som ska läsas behöver anroparen **Läs**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-164">For the file to be read, the caller needs **Read** permissions.</span></span>
* <span data-ttu-id="dc9fd-165">För alla mapparna i mappstrukturen som innehåller filen behöver anroparen **Kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-165">For all the folders in the folder structure that contain the file, the caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-to-append-to-a-file"></a><span data-ttu-id="dc9fd-166">Behörigheter som krävs för att lägga till i en fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-166">Permissions needed to append to a file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* <span data-ttu-id="dc9fd-168">För filen som ska läggas till i behöver anroparen **Skriv**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-168">For the file to be appended to, the caller needs **Write** permissions.</span></span>
* <span data-ttu-id="dc9fd-169">För alla mappar som innehåller filen behöver anroparen **Kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-169">For all the folders that contain the file, the caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-to-delete-a-file"></a><span data-ttu-id="dc9fd-170">Behörigheter för att ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-170">Permissions needed to delete a file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* <span data-ttu-id="dc9fd-172">För den överordnade mappen behöver anroparen **Skriv + kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-172">For the parent folder, the caller needs **Write + Execute** permissions.</span></span>
* <span data-ttu-id="dc9fd-173">För alla andra mappar i sökvägen behöver anroparen **Kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-173">For all the other folders in the file’s path, the caller needs **Execute** permissions.</span></span>



> [!NOTE]
> <span data-ttu-id="dc9fd-174">Skrivbehörigheterna för filen behövs inte att ta bort filen så länge de två villkoren ovan är sanna.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-174">Write permissions on the file are not required to delete it as long as the previous two conditions are true.</span></span>
>
>

### <a name="permissions-needed-to-enumerate-a-folder"></a><span data-ttu-id="dc9fd-175">Behörigheter som krävs för att iterera en mapp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-175">Permissions needed to enumerate a folder</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* <span data-ttu-id="dc9fd-177">För att mappen ska iterera behöver anroparen **Läs + kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-177">For the folder to enumerate, the caller needs **Read + Execute** permissions.</span></span>
* <span data-ttu-id="dc9fd-178">För alla överordnade mappar behöver anroparen **Kör**-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-178">For all the ancestor folders, the caller needs **Execute** permissions.</span></span>

## <a name="viewing-permissions-in-the-azure-portal"></a><span data-ttu-id="dc9fd-179">Visa behörigheter i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dc9fd-179">Viewing permissions in the Azure portal</span></span>

<span data-ttu-id="dc9fd-180">Från bladet **Datautforskaren** i Data Lake Store-kontot, klickar du på **Åtkomst** för att se ACL:er för en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-180">From the **Data Explorer** blade of the Data Lake Store account, click **Access** to see the ACLs for a file or a folder.</span></span> <span data-ttu-id="dc9fd-181">Klicka på **Åtkomst** för att se ACL:er för mappen **katalog** under kontot **mydatastore**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-181">Click **Access** to see the ACLs for the **catalog** folder under the **mydatastore** account.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

<span data-ttu-id="dc9fd-183">På det här bladet visar det översta avsnittet en översikt av de behörigheter du har.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-183">On this blade, the top section shows an overview of the permissions that you have.</span></span> <span data-ttu-id="dc9fd-184">(På skärmbilden är användaren Bob.) Därefter visas åtkomstbehörigheterna.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-184">(In the screenshot, the user is Bob.) Following that, the access permissions are shown.</span></span> <span data-ttu-id="dc9fd-185">Efter det klickar du från bladet **Åtkomst** på **Enkel vy** för att visa den enklare vyn.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-185">After that, from the **Access** blade, click **Simple View** to see the simpler view.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

<span data-ttu-id="dc9fd-187">Klicka på **Avancerad vy** om du vill se en mer avancerad vy där begreppen standard-ACL:er, mask och superanvändare visas.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-187">Click **Advanced View** to see the more advanced view, where the concepts of Default ACLs, mask, and super-user are shown.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a><span data-ttu-id="dc9fd-189">Superanvändaren</span><span class="sxs-lookup"><span data-stu-id="dc9fd-189">The super-user</span></span>

<span data-ttu-id="dc9fd-190">En superanvändare har mest behörighet av alla användare i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-190">A super-user has the most rights of all the users in the Data Lake Store.</span></span> <span data-ttu-id="dc9fd-191">En superanvändare:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-191">A super-user:</span></span>

* <span data-ttu-id="dc9fd-192">Har RWX-behörigheter till **alla** filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-192">Has RWX Permissions to **all** files and folders.</span></span>
* <span data-ttu-id="dc9fd-193">Kan ändra behörigheterna för alla filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-193">Can change the permissions on any file or folder.</span></span>
* <span data-ttu-id="dc9fd-194">Kan ändra ägande användare eller ägande grupp för alla filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-194">Can change the owning user or owning group of any file or folder.</span></span>

<span data-ttu-id="dc9fd-195">I Azure har ett Data Lake Store-konto flera Azure-roller, inklusive:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-195">In Azure, a Data Lake Store account has several Azure roles, including:</span></span>

* <span data-ttu-id="dc9fd-196">Ägare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-196">Owners</span></span>
* <span data-ttu-id="dc9fd-197">Deltagare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-197">Contributors</span></span>
* <span data-ttu-id="dc9fd-198">Läsare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-198">Readers</span></span>

<span data-ttu-id="dc9fd-199">Alla i rollen **Ägare** för ett Data Lake Store-konto är automatiskt en superanvändare för det kontot.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-199">Everyone in the **Owners** role for a Data Lake Store account is automatically a super-user for that account.</span></span> <span data-ttu-id="dc9fd-200">Läs mer i [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="dc9fd-200">To learn more, see [Role-based access control](../active-directory/role-based-access-control-configure.md).</span></span>
<span data-ttu-id="dc9fd-201">Om du vill skapa en anpassad rollbaserad åtkomstkontroll(RBAC)-roll som har superanvändarbehörigheter så måste den ha följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-201">If you want to create a custom role-based-access control (RBAC) role that has super-user permissions, it needs to have the following permissions:</span></span>
- <span data-ttu-id="dc9fd-202">Microsoft.DataLakeStore/accounts/Superuser/action</span><span class="sxs-lookup"><span data-stu-id="dc9fd-202">Microsoft.DataLakeStore/accounts/Superuser/action</span></span>
- <span data-ttu-id="dc9fd-203">Microsoft.Authorization/roleAssignments/write</span><span class="sxs-lookup"><span data-stu-id="dc9fd-203">Microsoft.Authorization/roleAssignments/write</span></span>


## <a name="the-owning-user"></a><span data-ttu-id="dc9fd-204">Ägande användare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-204">The owning user</span></span>

<span data-ttu-id="dc9fd-205">Användaren som skapade objektet är automatiskt ägande användare för objektet.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-205">The user who created the item is automatically the owning user of the item.</span></span> <span data-ttu-id="dc9fd-206">En ägande användare kan:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-206">An owning user can:</span></span>

* <span data-ttu-id="dc9fd-207">Ändra behörigheterna för en fil som ägs.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-207">Change the permissions of a file that is owned.</span></span>
* <span data-ttu-id="dc9fd-208">Ändra ägande grupp för en fil som ägs, så länge den ägande användaren också är medlem i målgruppen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-208">Change the owning group of a file that is owned, as long as the owning user is also a member of the target group.</span></span>

> [!NOTE]
> <span data-ttu-id="dc9fd-209">Den ägande användaren *kan inte* ändra ägande användare för en annan ägd fil.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-209">The owning user *cannot* change the owning user of another owned file.</span></span> <span data-ttu-id="dc9fd-210">Endast superanvändare kan ändra ägande användare av en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-210">Only super-users can change the owning user of a file or folder.</span></span>
>
>

## <a name="the-owning-group"></a><span data-ttu-id="dc9fd-211">Ägande grupp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-211">The owning group</span></span>

<span data-ttu-id="dc9fd-212">I POSIX-ACL:er är varje användare associerad med en "primär grupp".</span><span class="sxs-lookup"><span data-stu-id="dc9fd-212">In the POSIX ACLs, every user is associated with a "primary group."</span></span> <span data-ttu-id="dc9fd-213">Användaren "Alice" kan t.ex. tillhöra gruppen "Ekonomi".</span><span class="sxs-lookup"><span data-stu-id="dc9fd-213">For example, user "alice" might belong to the "finance" group.</span></span> <span data-ttu-id="dc9fd-214">Alice kan också tillhöra flera grupper, men en grupp anges alltid som hennes primära grupp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-214">Alice might also belong to multiple groups, but one group is always designated as her primary group.</span></span> <span data-ttu-id="dc9fd-215">När Alice skapar en fil i POSIX ställs den ägande gruppen för filen in som hennes primära grupp, som i det här fallet är "Ekonomi".</span><span class="sxs-lookup"><span data-stu-id="dc9fd-215">In POSIX, when Alice creates a file, the owning group of that file is set to her primary group, which in this case is "finance."</span></span>

<span data-ttu-id="dc9fd-216">När ett nytt filsystemsobjekt skapats, tilldelar Data Lake Store ett värde till den ägande gruppen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-216">When a new filesystem item is created, Data Lake Store assigns a value to the owning group.</span></span>

* <span data-ttu-id="dc9fd-217">**Fall 1**: Rotmappen "/".</span><span class="sxs-lookup"><span data-stu-id="dc9fd-217">**Case 1**: The root folder "/".</span></span> <span data-ttu-id="dc9fd-218">Den här mappen skapas när ett Data Lake Store-konto skapas.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-218">This folder is created when a Data Lake Store account is created.</span></span> <span data-ttu-id="dc9fd-219">I det här fallet har den ägande gruppen angetts till den användaren som skapade kontot.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-219">In this case, the owning group is set to the user who created the account.</span></span>
* <span data-ttu-id="dc9fd-220">**Fall 2** (alla andra fall): När ett nytt objekt skapas, kopieras den ägande gruppen från den överordnade mappen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-220">**Case 2** (Every other case): When a new item is created, the owning group is copied from the parent folder.</span></span>

<span data-ttu-id="dc9fd-221">Den ägande gruppen kan ändras av:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-221">The owning group can be changed by:</span></span>
* <span data-ttu-id="dc9fd-222">Alla superanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-222">Any super-users.</span></span>
* <span data-ttu-id="dc9fd-223">Den ägande användaren, om den ägande användaren också är medlem i målgruppen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-223">The owning user, if the owning user is also a member of the target group.</span></span>

## <a name="access-check-algorithm"></a><span data-ttu-id="dc9fd-224">Algoritm för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="dc9fd-224">Access check algorithm</span></span>

<span data-ttu-id="dc9fd-225">Följande bild visar algoritmen för åtkomstkontroll för Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-225">The following illustration represents the access check algorithm for Data Lake Store accounts.</span></span>

![ALC-algoritmer för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a><span data-ttu-id="dc9fd-227">Mask och "gällande behörigheter"</span><span class="sxs-lookup"><span data-stu-id="dc9fd-227">The mask and "effective permissions"</span></span>

<span data-ttu-id="dc9fd-228">**Mask** är ett RWX-värde som används för att begränsa åtkomsten för **namngivna användare**, **ägande grupp** och **namngivna grupper** när du utför algoritmen för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-228">The **mask** is an RWX value that is used to limit access for **named users**, the **owning group**, and **named groups** when you're performing the access check algorithm.</span></span> <span data-ttu-id="dc9fd-229">Här följer nyckelbegreppen för masken.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-229">Here are the key concepts for the mask.</span></span>

* <span data-ttu-id="dc9fd-230">Masken skapar "gällande behörigheter"</span><span class="sxs-lookup"><span data-stu-id="dc9fd-230">The mask creates "effective permissions."</span></span> <span data-ttu-id="dc9fd-231">D.v.s. den modifierar behörigheterna vid tidpunkten för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-231">That is, it modifies the permissions at the time of access check.</span></span>
* <span data-ttu-id="dc9fd-232">Masken kan redigeras direkt av filens ägare och alla superanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-232">The mask can be directly edited by the file owner and any super-users.</span></span>
* <span data-ttu-id="dc9fd-233">Masken kan ta bort behörigheter för att skapa den gällande behörigheten.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-233">The mask can remove permissions to create the effective permission.</span></span> <span data-ttu-id="dc9fd-234">Masken *kan inte* lägga till behörigheter till den gällande behörigheten.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-234">The mask *cannot* add permissions to the effective permission.</span></span>

<span data-ttu-id="dc9fd-235">Låt oss titta på några exempel.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-235">Let's look at some examples.</span></span> <span data-ttu-id="dc9fd-236">Nedan är masken inställd på **RWX**, vilket innebär att masken inte tar bort några behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-236">In the following example, the mask is set to **RWX**, which means that the mask does not remove any permissions.</span></span> <span data-ttu-id="dc9fd-237">De gällande behörigheterna för den namngivna användaren, ägande gruppen och namngivna gruppen ändras inte under åtkomstkontrollen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-237">The effective permissions for the named user, owning group, and named group are not altered during the access check.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

<span data-ttu-id="dc9fd-239">I exemplet nedan anges masken till **R-X**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-239">In the following example, the mask is set to **R-X**.</span></span> <span data-ttu-id="dc9fd-240">Så den **inaktiverar skrivbehörighet** för **namngiven användare**, **ägande grupp** och **namngiven grupp** vid tidpunkten för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-240">This means that it **turns off the Write permissions** for **named user**, **owning group**, and **named group** at the time of access check.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

<span data-ttu-id="dc9fd-242">Som referens är det här masken för en fil eller mapp visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-242">For reference, here is where the mask for a file or folder appears in the Azure portal.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> <span data-ttu-id="dc9fd-244">För ett nytt Data Lake Store-konto får masken för åtkomst-ACL och standard-ACL i rotmappen ("/") som standard RWX.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-244">For a new Data Lake Store account, the mask for the Access ACL and Default ACL of the root folder ("/") defaults to RWX.</span></span>
>
>

## <a name="permissions-on-new-files-and-folders"></a><span data-ttu-id="dc9fd-245">Behörigheter för nya filer och mappar</span><span class="sxs-lookup"><span data-stu-id="dc9fd-245">Permissions on new files and folders</span></span>

<span data-ttu-id="dc9fd-246">När en ny fil eller mapp skapas under en befintlig mapp, anger standard-ACL:en på den överordnade mappen:</span><span class="sxs-lookup"><span data-stu-id="dc9fd-246">When a new file or folder is created under an existing folder, the Default ACL on the parent folder determines:</span></span>

- <span data-ttu-id="dc9fd-247">En underordnad mapps standard-ACL och åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-247">A child folder’s Default ACL and Access ACL.</span></span>
- <span data-ttu-id="dc9fd-248">En underordnad fils åtkomst-ACL (filer har inte en standard-ACL).</span><span class="sxs-lookup"><span data-stu-id="dc9fd-248">A child file's Access ACL (files do not have a Default ACL).</span></span>

### <a name="the-access-acl-of-a-child-file-or-folder"></a><span data-ttu-id="dc9fd-249">En underordnad fils eller mapps åtkomst-ACL</span><span class="sxs-lookup"><span data-stu-id="dc9fd-249">The Access ACL of a child file or folder</span></span>

<span data-ttu-id="dc9fd-250">När en underordnad fil eller mapp skapas, kopieras den överordnade filens standard-ACL till den underordnade filens eller mappens åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-250">When a child file or folder is created, the parent's Default ACL is copied as the Access ACL of the child file or folder.</span></span> <span data-ttu-id="dc9fd-251">Även om **andra** användare har RWX-behörigheter i det överordnade objektets standard-ACL, tas de bort från det underordnade objektets åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-251">Also, if **other** user has RWX permissions in the parent's default ACL, it is removed from the child item's Access ACL.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

<span data-ttu-id="dc9fd-253">I de flesta fall är ovanstående information allt du behöver känna till om hur ett underordnat objekts åtkomst-ACL bestäms.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-253">In most scenarios, the previous information is all you need to know about how a child item’s Access ACL is determined.</span></span> <span data-ttu-id="dc9fd-254">Om du är bekant med POSIX-system och vill förstå mer djupgående hur transformationen uppnås finns information i avsnittet [Umasks roll vid skapandet av åtkomst-ACL för nya filer och mappar](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-254">However, if you are familiar with POSIX systems and want to understand in-depth how this transformation is achieved, see the section [Umask’s role in creating the Access ACL for new files and folders](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) later in this article.</span></span>


### <a name="a-child-folders-default-acl"></a><span data-ttu-id="dc9fd-255">En underordnad mapps standard-ACL</span><span class="sxs-lookup"><span data-stu-id="dc9fd-255">A child folder's Default ACL</span></span>

<span data-ttu-id="dc9fd-256">När en underordnad mapp skapas under en överordnad mapp, kopieras den överordnade mappens standard-ACL över, som den är, till den underordnade mappens standard-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-256">When a child folder is created under a parent folder, the parent folder's Default ACL is copied over as is to the child folder's Default ACL.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a><span data-ttu-id="dc9fd-258">Avancerad information för att förstå ACL:er i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dc9fd-258">Advanced topics for understanding ACLs in Data Lake Store</span></span>

<span data-ttu-id="dc9fd-259">Här följer lite avancerad information som hjälper dig att förstå hur ACL:er bestäms för Data Lake Store-filer eller -mappar.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-259">Following are some advanced topics to help you understand how ACLs are determined for Data Lake Store files or folders.</span></span>

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a><span data-ttu-id="dc9fd-260">Umasks roll är att skapa åtkomst-ACL för nya filer och mappar</span><span class="sxs-lookup"><span data-stu-id="dc9fd-260">Umask’s role in creating the Access ACL for new files and folders</span></span>

<span data-ttu-id="dc9fd-261">I ett POSIX-kompatibelt system är allmänna begrepp som umask ett 9-bitars värde för den överordnade mappen som används för att omvandla behörigheten för **ägande användare**, **ägande grupp** och **andra** på en ny underordnad fils eller mapps åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-261">In a POSIX-compliant system, the general concept is that umask is a 9-bit value on the parent folder that's used to transform the permission for **owning user**, **owning group**, and **other** on the Access ACL of a new child file or folder.</span></span> <span data-ttu-id="dc9fd-262">En umasks bitar identifierar vilka bitar som ska inaktiveras i det underordnade objektets åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-262">The bits of a umask identify which bits to turn off in the child item’s Access ACL.</span></span> <span data-ttu-id="dc9fd-263">Den används därför för att selektivt förhindra spridning av behörigheter för **ägande användare**, **ägande grupp** och **övriga**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-263">Thus it is used to selectively prevent the propagation of permissions for **owning user**, **owning group**, and **other**.</span></span>

<span data-ttu-id="dc9fd-264">I ett HDFS-system är umask vanligtvis ett konfigurationsalternativ för hela platsen som styrs av administratörer.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-264">In an HDFS system, the umask is typically a sitewide configuration option that is controlled by administrators.</span></span> <span data-ttu-id="dc9fd-265">Data Lake Store använder en **umask för konto** som inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-265">Data Lake Store uses an **account-wide umask** that cannot be changed.</span></span> <span data-ttu-id="dc9fd-266">I följande tabell visas Data Lake Stores umask.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-266">The following table shows the unmask for Data Lake Store.</span></span>

| <span data-ttu-id="dc9fd-267">Användargrupp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-267">User group</span></span>  | <span data-ttu-id="dc9fd-268">Inställning</span><span class="sxs-lookup"><span data-stu-id="dc9fd-268">Setting</span></span> | <span data-ttu-id="dc9fd-269">Effekt av nytt underordnade objekts åtkomst-ACL</span><span class="sxs-lookup"><span data-stu-id="dc9fd-269">Effect on new child item's Access ACL</span></span> |
|------------ |---------|---------------------------------------|
| <span data-ttu-id="dc9fd-270">Ägande användare</span><span class="sxs-lookup"><span data-stu-id="dc9fd-270">Owning user</span></span> | ---     | <span data-ttu-id="dc9fd-271">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="dc9fd-271">No effect</span></span>                             |
| <span data-ttu-id="dc9fd-272">Ägande grupp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-272">Owning group</span></span>| ---     | <span data-ttu-id="dc9fd-273">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="dc9fd-273">No effect</span></span>                             |
| <span data-ttu-id="dc9fd-274">Annat</span><span class="sxs-lookup"><span data-stu-id="dc9fd-274">Other</span></span>       | <span data-ttu-id="dc9fd-275">RWX</span><span class="sxs-lookup"><span data-stu-id="dc9fd-275">RWX</span></span>     | <span data-ttu-id="dc9fd-276">Ta bort läsa + skriva + köra</span><span class="sxs-lookup"><span data-stu-id="dc9fd-276">Remove Read + Write + Execute</span></span>         |

<span data-ttu-id="dc9fd-277">Följande bild visar denna umask när den är aktiv.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-277">The following illustration shows this umask in action.</span></span> <span data-ttu-id="dc9fd-278">Nettoeffekten är att ta bort **läsa + skriva + köra** för **andra** användare.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-278">The net effect is to remove **Read + Write + Execute** for **other** user.</span></span> <span data-ttu-id="dc9fd-279">Eftersom umask inte har angett bitar för **ägande användare** och **ägande grupp**, omvandlas inte dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-279">Because the umask did not specify bits for **owning user** and **owning group**, those permissions are not transformed.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="the-sticky-bit"></a><span data-ttu-id="dc9fd-281">Sticky bit</span><span class="sxs-lookup"><span data-stu-id="dc9fd-281">The sticky bit</span></span>

<span data-ttu-id="dc9fd-282">Sticky bit är en mer avancerad funktion i ett POSIX-filsystem.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-282">The sticky bit is a more advanced feature of a POSIX filesystem.</span></span> <span data-ttu-id="dc9fd-283">Det är inte troligt att sticky bit kommer att krävas i kontexten för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-283">In the context of Data Lake Store, it is unlikely that the sticky bit will be needed.</span></span>

<span data-ttu-id="dc9fd-284">Tabellen nedan visar hur sticky bit fungerar i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-284">The following table shows how the sticky bit works in Data Lake Store.</span></span>

| <span data-ttu-id="dc9fd-285">Användargrupp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-285">User group</span></span>         | <span data-ttu-id="dc9fd-286">Fil</span><span class="sxs-lookup"><span data-stu-id="dc9fd-286">File</span></span>    | <span data-ttu-id="dc9fd-287">Mapp</span><span class="sxs-lookup"><span data-stu-id="dc9fd-287">Folder</span></span> |
|--------------------|---------|-------------------------|
| <span data-ttu-id="dc9fd-288">Sticky bit **AV**</span><span class="sxs-lookup"><span data-stu-id="dc9fd-288">Sticky bit **OFF**</span></span> | <span data-ttu-id="dc9fd-289">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="dc9fd-289">No effect</span></span>   | <span data-ttu-id="dc9fd-290">Ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-290">No effect.</span></span>           |
| <span data-ttu-id="dc9fd-291">Sticky bit **PÅ**</span><span class="sxs-lookup"><span data-stu-id="dc9fd-291">Sticky bit **ON**</span></span>  | <span data-ttu-id="dc9fd-292">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="dc9fd-292">No effect</span></span>   | <span data-ttu-id="dc9fd-293">Förhindrar alla utom **superanvändare** och **ägande användare** av ett underordnat objekt från att ta bort eller byta namn på det underordnade objektet.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-293">Prevents anyone except **super-users** and the **owning user** of a child item from deleting or renaming that child item.</span></span>               |

<span data-ttu-id="dc9fd-294">Sticky bit visas inte i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-294">The sticky bit is not shown in the Azure portal.</span></span>

## <a name="common-questions-about-acls-in-data-lake-store"></a><span data-ttu-id="dc9fd-295">Vanliga frågor om ACL:er i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dc9fd-295">Common questions about ACLs in Data Lake Store</span></span>

<span data-ttu-id="dc9fd-296">Här är några frågor som ofta kommer upp rörande ACL:er i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-296">Here are some questions that come up often about ACLs in Data Lake Store.</span></span>

### <a name="do-i-have-to-enable-support-for-acls"></a><span data-ttu-id="dc9fd-297">Måste jag aktivera stöd för ACL:er?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-297">Do I have to enable support for ACLs?</span></span>

<span data-ttu-id="dc9fd-298">Nej.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-298">No.</span></span> <span data-ttu-id="dc9fd-299">Åtkomstkontroll via ACL:er är alltid aktiverat för ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-299">Access control via ACLs is always on for a Data Lake Store account.</span></span>

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a><span data-ttu-id="dc9fd-300">Vilka behörigheter krävs för att rekursivt ta bort en mapp och dess innehåll?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-300">Which permissions are required to recursively delete a folder and its contents?</span></span>

* <span data-ttu-id="dc9fd-301">Den överordnade mappen måste ha behörigheterna **skriva + köra**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-301">The parent folder must have **Write + Execute** permissions.</span></span>
* <span data-ttu-id="dc9fd-302">Mappen som ska tas bort och alla mappar inom den, behöver behörigheterna **läsa + skriva + köra**.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-302">The folder to be deleted, and every folder within it, requires **Read + Write + Execute** permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="dc9fd-303">Du behöver inte Skriv-behörigheter för att ta bort filer i mappar.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-303">You do not need Write permissions to delete files in folders.</span></span> <span data-ttu-id="dc9fd-304">Dessutom kan rotmappen "/" **aldrig** tas bort.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-304">Also, the root folder "/" can **never** be deleted.</span></span>
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a><span data-ttu-id="dc9fd-305">Vem äger en fil eller mapp?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-305">Who is the owner of a file or folder?</span></span>

<span data-ttu-id="dc9fd-306">Skaparen av en fil eller mapp blir ägaren.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-306">The creator of a file or folder becomes the owner.</span></span>

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a><span data-ttu-id="dc9fd-307">Vilken grupp anges som den ägande gruppen för en fil eller mapp vid skapandet?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-307">Which group is set as the owning group of a file or folder at creation?</span></span>

<span data-ttu-id="dc9fd-308">Det kopieras från den ägande gruppen för den överordnade mappen under vilken den nya filen eller mappen skapas.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-308">The owning group is copied from the owning group of the parent folder under which the new file or folder is created.</span></span>

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a><span data-ttu-id="dc9fd-309">Jag är ägande användare av en fil, men jag har inte den RWX-behörighet jag behöver.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-309">I am the owning user of a file but I don’t have the RWX permissions I need.</span></span> <span data-ttu-id="dc9fd-310">Vad gör jag nu?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-310">What do I do?</span></span>

<span data-ttu-id="dc9fd-311">Den ägande användaren kan lätt ändra behörigheterna för filen för att ge sig själva de RWX-behörigheter de behöver.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-311">The owning user can change the permissions of the file to give themselves any RWX permissions they need.</span></span>

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a><span data-ttu-id="dc9fd-312">När jag tittar på ACL:er i Azure-portalen, visas användarnamn men via API:er visas GUID:er. Varför?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-312">When I look at ACLs in the Azure portal I see user names but through APIs, I see GUIDs, why is that?</span></span>

<span data-ttu-id="dc9fd-313">Posterna i ACL lagras som GUID:er som motsvarar användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-313">Entries in the ACLs are stored as GUIDs that correspond to users in Azure AD.</span></span> <span data-ttu-id="dc9fd-314">API:erna returnerar GUID:erna i befintligt skick.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-314">The APIs return the GUIDs as is.</span></span> <span data-ttu-id="dc9fd-315">Azure-portalen försöker göra det enklare att använda ACL:er genom att översätta GUID:er till bekanta namn när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-315">The Azure portal tries to make ACLs easier to use by translating the GUIDs into friendly names when possible.</span></span>

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a><span data-ttu-id="dc9fd-316">Varför visas ibland GUID:er i ACL:er när jag använder Azure-portalen?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-316">Why do I sometimes see GUIDs in the ACLs when I'm using the Azure portal?</span></span>

<span data-ttu-id="dc9fd-317">En GUID visas när användaren inte finns i Azure AD längre.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-317">A GUID is shown when the user doesn't exist in Azure AD anymore.</span></span> <span data-ttu-id="dc9fd-318">Detta inträffar vanligtvis när användaren har lämnat företaget eller om kontot har tagits bort i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-318">Usually this happens when the user has left the company or if their account has been deleted in Azure AD.</span></span>

### <a name="does-data-lake-store-support-inheritance-of-acls"></a><span data-ttu-id="dc9fd-319">Stöds arv av ACL:er i Data Lake Store?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-319">Does Data Lake Store support inheritance of ACLs?</span></span>

<span data-ttu-id="dc9fd-320">Nej.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-320">No.</span></span>

### <a name="what-is-the-difference-between-mask-and-umask"></a><span data-ttu-id="dc9fd-321">Vad är skillnaden mellan mask och umask?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-321">What is the difference between mask and umask?</span></span>

| <span data-ttu-id="dc9fd-322">mask</span><span class="sxs-lookup"><span data-stu-id="dc9fd-322">mask</span></span> | <span data-ttu-id="dc9fd-323">umask</span><span class="sxs-lookup"><span data-stu-id="dc9fd-323">umask</span></span>|
|------|------|
| <span data-ttu-id="dc9fd-324">Egenskapen **mask** är tillgänglig på varje fil och mapp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-324">The **mask** property is available on every file and folder.</span></span> | <span data-ttu-id="dc9fd-325">**Umask** är en egenskap för Data Lake Store-kontot.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-325">The **umask** is a property of the Data Lake Store account.</span></span> <span data-ttu-id="dc9fd-326">Så det finns bara en enda umask i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-326">So there is only a single umask in the Data Lake Store.</span></span>    |
| <span data-ttu-id="dc9fd-327">Maskegenskapen på en fil eller mapp kan ändras av ägande användare eller ägande grupp för en fil eller en superanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-327">The mask property on a file or folder can be altered by the owning user or owning group of a file or a super-user.</span></span> | <span data-ttu-id="dc9fd-328">Umask-egenskapen kan inte ändras av någon användare, inte ens en superanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-328">The umask property cannot be modified by any user, even a super-user.</span></span> <span data-ttu-id="dc9fd-329">Det är ett konstant värde som inte ändras.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-329">It is an unchangeable, constant value.</span></span>|
| <span data-ttu-id="dc9fd-330">Maskegenskapen används under algoritmen för åtkomstkontroll vid körning för att avgöra om en användare har behörighet att utföra åtgärden på en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-330">The mask property is used during the access check algorithm at runtime to determine whether a user has the right to perform on operation on a file or folder.</span></span> <span data-ttu-id="dc9fd-331">Maskrollen är att skapa "gällande behörigheter" vid tidpunkten för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-331">The role of the mask is to create "effective permissions" at the time of access check.</span></span> | <span data-ttu-id="dc9fd-332">Umask används inte vid åtkomstkontroll alls.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-332">The umask is not used during access check at all.</span></span> <span data-ttu-id="dc9fd-333">Umask används för att bestämma åtkomst-ACL för de nya underordnade objekten i en mapp.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-333">The umask is used to determine the Access ACL of new child items of a folder.</span></span> |
| <span data-ttu-id="dc9fd-334">Masken är ett 3-bitars RWX-värde som gäller för namngiven användare, namngiven grupp och ägande användare vid tidpunkten för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-334">The mask is a 3-bit RWX value that applies to named user, named group, and owning user at the time of access check.</span></span>| <span data-ttu-id="dc9fd-335">Umask är ett 9-bitarsvärde som gäller för ägande användare, ägande grupper och **övriga** för ett nytt underordnat objekt.</span><span class="sxs-lookup"><span data-stu-id="dc9fd-335">The umask is a 9-bit value that applies to the owning user, owning group, and **other** of a new child.</span></span>|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a><span data-ttu-id="dc9fd-336">Var hittar jag mer information om POSIX-modellen för åtkomstkontroll?</span><span class="sxs-lookup"><span data-stu-id="dc9fd-336">Where can I learn more about POSIX access control model?</span></span>

* [<span data-ttu-id="dc9fd-337">POSIX-åtkomstkontrollistor på Linux</span><span class="sxs-lookup"><span data-stu-id="dc9fd-337">POSIX Access Control Lists on Linux</span></span>](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [<span data-ttu-id="dc9fd-338">Guide för HDFS-behörighet</span><span class="sxs-lookup"><span data-stu-id="dc9fd-338">HDFS permission guide</span></span>](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [<span data-ttu-id="dc9fd-339">Vanliga frågor och svar om POSIX</span><span class="sxs-lookup"><span data-stu-id="dc9fd-339">POSIX FAQ</span></span>](http://www.opengroup.org/austin/papers/posix_faq.html)

* [<span data-ttu-id="dc9fd-340">POSIX 1003.1 2008</span><span class="sxs-lookup"><span data-stu-id="dc9fd-340">POSIX 1003.1 2008</span></span>](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [<span data-ttu-id="dc9fd-341">POSIX 1003.1 2013</span><span class="sxs-lookup"><span data-stu-id="dc9fd-341">POSIX 1003.1 2013</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [<span data-ttu-id="dc9fd-342">POSIX 1003.1 2016</span><span class="sxs-lookup"><span data-stu-id="dc9fd-342">POSIX 1003.1 2016</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [<span data-ttu-id="dc9fd-343">POSIX ACL på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="dc9fd-343">POSIX ACL on Ubuntu</span></span>](https://help.ubuntu.com/community/FilePermissionsACLs)

* [<span data-ttu-id="dc9fd-344">ACL med åtkomstkontrollistor på Linux</span><span class="sxs-lookup"><span data-stu-id="dc9fd-344">ACL using access control lists on Linux</span></span>](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a><span data-ttu-id="dc9fd-345">Se även</span><span class="sxs-lookup"><span data-stu-id="dc9fd-345">See also</span></span>

* [<span data-ttu-id="dc9fd-346">Översikt över Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dc9fd-346">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
