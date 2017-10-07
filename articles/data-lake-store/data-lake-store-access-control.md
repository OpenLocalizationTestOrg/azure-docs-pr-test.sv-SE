---
title: "aaaOverview för åtkomstkontroll i Data Lake Store | Microsoft Docs"
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
ms.openlocfilehash: 1cc5d578f22ef0a123a1547abebfb4795ea09139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-control-in-azure-data-lake-store"></a><span data-ttu-id="4c464-103">Åtkomstkontroll i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4c464-103">Access control in Azure Data Lake Store</span></span>

<span data-ttu-id="4c464-104">Azure Data Lake Store implementerar en åtkomstkontrollmodellen som härleds från HDFS, som i sin tur härleds från hello POSIX modellen för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="4c464-104">Azure Data Lake Store implements an access control model that derives from HDFS, which in turn derives from hello POSIX access control model.</span></span> <span data-ttu-id="4c464-105">Den här artikeln sammanfattar hello grunderna i hello modellen för åtkomstkontroll för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-105">This article summarizes hello basics of hello access control model for Data Lake Store.</span></span> <span data-ttu-id="4c464-106">toolearn mer om hello HDFS åtkomstkontrollmodellen finns [HDFS behörigheter guiden](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span><span class="sxs-lookup"><span data-stu-id="4c464-106">toolearn more about hello HDFS access control model, see [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span></span>

## <a name="access-control-lists-on-files-and-folders"></a><span data-ttu-id="4c464-107">Åtkomstkontrollistor för filer och mappar</span><span class="sxs-lookup"><span data-stu-id="4c464-107">Access control lists on files and folders</span></span>

<span data-ttu-id="4c464-108">Det finns två sorters åtkomstkontrollistor (ACL:er), **Åtkomst-ACL:er** och **Standard-ACL:er**.</span><span class="sxs-lookup"><span data-stu-id="4c464-108">There are two kinds of access control lists (ACLs), **Access ACLs** and **Default ACLs**.</span></span>

* <span data-ttu-id="4c464-109">**Åtkomst till ACL: er**: dessa kontrollobjekt åtkomst tooan.</span><span class="sxs-lookup"><span data-stu-id="4c464-109">**Access ACLs**: These control access tooan object.</span></span> <span data-ttu-id="4c464-110">Både filer och mappar har åtkomst-ACL:er.</span><span class="sxs-lookup"><span data-stu-id="4c464-110">Files and folders both have Access ACLs.</span></span>

* <span data-ttu-id="4c464-111">**ACL: er som standard**: en ”mall” i ACL: er som är kopplade till en mapp som bestämmer hello åtkomst ACL för alla underordnade objekt som skapas i mappen.</span><span class="sxs-lookup"><span data-stu-id="4c464-111">**Default ACLs**: A "template" of ACLs associated with a folder that determine hello Access ACLs for any child items that are created under that folder.</span></span> <span data-ttu-id="4c464-112">Filer har inte standard-ACL:er.</span><span class="sxs-lookup"><span data-stu-id="4c464-112">Files do not have Default ACLs.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

<span data-ttu-id="4c464-114">Både åtkomst ACL: er och standard-ACL: er har hello samma struktur.</span><span class="sxs-lookup"><span data-stu-id="4c464-114">Both Access ACLs and Default ACLs have hello same structure.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> <span data-ttu-id="4c464-116">Ändra hello standard ACL på en överordnad påverkar inte hello ACL Access eller standard ACL för underordnade objekt som redan finns.</span><span class="sxs-lookup"><span data-stu-id="4c464-116">Changing hello Default ACL on a parent does not affect hello Access ACL or Default ACL of child items that already exist.</span></span>
>
>

## <a name="users-and-identities"></a><span data-ttu-id="4c464-117">Användare och identiteter</span><span class="sxs-lookup"><span data-stu-id="4c464-117">Users and identities</span></span>

<span data-ttu-id="4c464-118">Alla filer och mappar har olika behörigheter för dessa identiteter:</span><span class="sxs-lookup"><span data-stu-id="4c464-118">Every file and folder has distinct permissions for these identities:</span></span>

* <span data-ttu-id="4c464-119">hello ägande användare av hello-fil</span><span class="sxs-lookup"><span data-stu-id="4c464-119">hello owning user of hello file</span></span>
* <span data-ttu-id="4c464-120">hello ägande grupp</span><span class="sxs-lookup"><span data-stu-id="4c464-120">hello owning group</span></span>
* <span data-ttu-id="4c464-121">Namngivna användare</span><span class="sxs-lookup"><span data-stu-id="4c464-121">Named users</span></span>
* <span data-ttu-id="4c464-122">Namngivna grupper</span><span class="sxs-lookup"><span data-stu-id="4c464-122">Named groups</span></span>
* <span data-ttu-id="4c464-123">Alla andra användare</span><span class="sxs-lookup"><span data-stu-id="4c464-123">All other users</span></span>

<span data-ttu-id="4c464-124">hello identiteter för användare och grupper är identiteter med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4c464-124">hello identities of users and groups are Azure Active Directory (Azure AD) identities.</span></span> <span data-ttu-id="4c464-125">Så om inget annat anges, ””, Hej kontexten för Data Lake Store kan antingen innebära en Azure AD-användare eller en Azure AD-säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4c464-125">So unless otherwise noted, a "user," in hello context of Data Lake Store, can either mean an Azure AD user or an Azure AD security group.</span></span>

## <a name="permissions"></a><span data-ttu-id="4c464-126">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="4c464-126">Permissions</span></span>

<span data-ttu-id="4c464-127">hello behörigheter för ett filsystem är **Läs**, **skriva**, och **kör**, och de kan användas för filer och mappar som visas i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="4c464-127">hello permissions on a filesystem object are **Read**, **Write**, and **Execute**, and they can be used on files and folders as shown in hello following table:</span></span>

|            |    <span data-ttu-id="4c464-128">Fil</span><span class="sxs-lookup"><span data-stu-id="4c464-128">File</span></span>     |   <span data-ttu-id="4c464-129">Mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-129">Folder</span></span> |
|------------|-------------|----------|
| <span data-ttu-id="4c464-130">**Läsa (R)**</span><span class="sxs-lookup"><span data-stu-id="4c464-130">**Read (R)**</span></span> | <span data-ttu-id="4c464-131">Kan läsa hello innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="4c464-131">Can read hello contents of a file</span></span> | <span data-ttu-id="4c464-132">Kräver **Läs** och **kör** toolist hello innehållet i hello-mappen</span><span class="sxs-lookup"><span data-stu-id="4c464-132">Requires **Read** and **Execute** toolist hello contents of hello folder</span></span>|
| <span data-ttu-id="4c464-133">**Skriva (W)**</span><span class="sxs-lookup"><span data-stu-id="4c464-133">**Write (W)**</span></span> | <span data-ttu-id="4c464-134">Kan skriva eller Lägg till tooa fil</span><span class="sxs-lookup"><span data-stu-id="4c464-134">Can write or append tooa file</span></span> | <span data-ttu-id="4c464-135">Kräver **skriva** och **kör** toocreate underordnade objekt i en mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-135">Requires **Write** and **Execute** toocreate child items in a folder</span></span> |
| <span data-ttu-id="4c464-136">**Köra (X)**</span><span class="sxs-lookup"><span data-stu-id="4c464-136">**Execute (X)**</span></span> | <span data-ttu-id="4c464-137">Innebär inte någonting i hello kontexten för Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4c464-137">Does not mean anything in hello context of Data Lake Store</span></span> | <span data-ttu-id="4c464-138">Nödvändiga tootraverse hello underordnade objekt i en mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-138">Required tootraverse hello child items of a folder</span></span> |

### <a name="short-forms-for-permissions"></a><span data-ttu-id="4c464-139">Kortformat för behörigheter</span><span class="sxs-lookup"><span data-stu-id="4c464-139">Short forms for permissions</span></span>

<span data-ttu-id="4c464-140">**RWX** är används tooindicate **läsa + skriva + köra**.</span><span class="sxs-lookup"><span data-stu-id="4c464-140">**RWX** is used tooindicate **Read + Write + Execute**.</span></span> <span data-ttu-id="4c464-141">En mer komprimerad numeriska formatet finns där **Läs = 4**, **skriva = 2**, och **Execute = 1**, hello summan av som representerar hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-141">A more condensed numeric form exists in which **Read=4**, **Write=2**, and **Execute=1**, hello sum of which represents hello permissions.</span></span> <span data-ttu-id="4c464-142">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="4c464-142">Following are some examples.</span></span>

| <span data-ttu-id="4c464-143">Numeriskt format</span><span class="sxs-lookup"><span data-stu-id="4c464-143">Numeric form</span></span> | <span data-ttu-id="4c464-144">Kortformat</span><span class="sxs-lookup"><span data-stu-id="4c464-144">Short form</span></span> |      <span data-ttu-id="4c464-145">Vad det innebär</span><span class="sxs-lookup"><span data-stu-id="4c464-145">What it means</span></span>     |
|--------------|------------|------------------------|
| <span data-ttu-id="4c464-146">7</span><span class="sxs-lookup"><span data-stu-id="4c464-146">7</span></span>            | <span data-ttu-id="4c464-147">RWX</span><span class="sxs-lookup"><span data-stu-id="4c464-147">RWX</span></span>        | <span data-ttu-id="4c464-148">Läsa + skriva + köra</span><span class="sxs-lookup"><span data-stu-id="4c464-148">Read + Write + Execute</span></span> |
| <span data-ttu-id="4c464-149">5</span><span class="sxs-lookup"><span data-stu-id="4c464-149">5</span></span>            | <span data-ttu-id="4c464-150">R-X</span><span class="sxs-lookup"><span data-stu-id="4c464-150">R-X</span></span>        | <span data-ttu-id="4c464-151">Läsa + köra</span><span class="sxs-lookup"><span data-stu-id="4c464-151">Read + Execute</span></span>         |
| <span data-ttu-id="4c464-152">4</span><span class="sxs-lookup"><span data-stu-id="4c464-152">4</span></span>            | <span data-ttu-id="4c464-153">R--</span><span class="sxs-lookup"><span data-stu-id="4c464-153">R--</span></span>        | <span data-ttu-id="4c464-154">Läsa</span><span class="sxs-lookup"><span data-stu-id="4c464-154">Read</span></span>                   |
| <span data-ttu-id="4c464-155">0</span><span class="sxs-lookup"><span data-stu-id="4c464-155">0</span></span>            | ---        | <span data-ttu-id="4c464-156">Inga behörigheter</span><span class="sxs-lookup"><span data-stu-id="4c464-156">No permissions</span></span>         |


### <a name="permissions-do-not-inherit"></a><span data-ttu-id="4c464-157">Behörigheter ärvs inte</span><span class="sxs-lookup"><span data-stu-id="4c464-157">Permissions do not inherit</span></span>

<span data-ttu-id="4c464-158">Behörigheter för ett objekt som lagras på själva hello objektnamnet i hello POSIX-typ modellen som används av Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-158">In hello POSIX-style model that's used by Data Lake Store, permissions for an item are stored on hello item itself.</span></span> <span data-ttu-id="4c464-159">Med andra ord kan behörighet för ett objekt inte ärvas från hello överordnade objekt.</span><span class="sxs-lookup"><span data-stu-id="4c464-159">In other words, permissions for an item cannot be inherited from hello parent items.</span></span>

## <a name="common-scenarios-related-toopermissions"></a><span data-ttu-id="4c464-160">Vanliga scenarier relaterade toopermissions</span><span class="sxs-lookup"><span data-stu-id="4c464-160">Common scenarios related toopermissions</span></span>

<span data-ttu-id="4c464-161">Följande är några vanliga scenarier toohelp du förstår vilka behörigheter som krävs för tooperform vissa åtgärder på ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="4c464-161">Following are some common scenarios toohelp you understand which permissions are needed tooperform certain operations on a Data Lake Store account.</span></span>

### <a name="permissions-needed-tooread-a-file"></a><span data-ttu-id="4c464-162">Behörigheter som krävs för tooread en fil</span><span class="sxs-lookup"><span data-stu-id="4c464-162">Permissions needed tooread a file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* <span data-ttu-id="4c464-164">Hello filen toobe läsa, hello i anroparen måste **Läs** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-164">For hello file toobe read, hello caller needs **Read** permissions.</span></span>
* <span data-ttu-id="4c464-165">För alla hello mapparna i mappstrukturen hello som innehåller hello filen hello anroparen måste **Execute** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-165">For all hello folders in hello folder structure that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-tooappend-tooa-file"></a><span data-ttu-id="4c464-166">Behörigheter som krävs för tooappend tooa fil</span><span class="sxs-lookup"><span data-stu-id="4c464-166">Permissions needed tooappend tooa file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* <span data-ttu-id="4c464-168">För hello filen toobe läggs till, hello anroparen måste **skriva** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-168">For hello file toobe appended to, hello caller needs **Write** permissions.</span></span>
* <span data-ttu-id="4c464-169">För alla hello mapparna som innehåller filen hello hello anroparen måste **Execute** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-169">For all hello folders that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-toodelete-a-file"></a><span data-ttu-id="4c464-170">Behörigheter som krävs för toodelete en fil</span><span class="sxs-lookup"><span data-stu-id="4c464-170">Permissions needed toodelete a file</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* <span data-ttu-id="4c464-172">För hello överordnade mappen hello anroparen måste **skriva + köra** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-172">For hello parent folder, hello caller needs **Write + Execute** permissions.</span></span>
* <span data-ttu-id="4c464-173">För alla hello andra mappar i hello filsökväg hello anroparen måste **Execute** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-173">For all hello other folders in hello file’s path, hello caller needs **Execute** permissions.</span></span>



> [!NOTE]
> <span data-ttu-id="4c464-174">Skriva behörigheterna för hello-fil inte är obligatoriska toodelete det så länge hello föregående två villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="4c464-174">Write permissions on hello file are not required toodelete it as long as hello previous two conditions are true.</span></span>
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a><span data-ttu-id="4c464-175">Behörigheter som krävs för tooenumerate en mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-175">Permissions needed tooenumerate a folder</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* <span data-ttu-id="4c464-177">Hello mappen tooenumerate hello i anroparen måste **Läs + Execute** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-177">For hello folder tooenumerate, hello caller needs **Read + Execute** permissions.</span></span>
* <span data-ttu-id="4c464-178">För alla hello överordnade mappar hello anroparen måste **Execute** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-178">For all hello ancestor folders, hello caller needs **Execute** permissions.</span></span>

## <a name="viewing-permissions-in-hello-azure-portal"></a><span data-ttu-id="4c464-179">Visa behörigheter i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4c464-179">Viewing permissions in hello Azure portal</span></span>

<span data-ttu-id="4c464-180">Från hello **Data Explorer** bladet för hello Data Lake Store-konto, klickar du på **åtkomst** toosee hello ACL: er för en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-180">From hello **Data Explorer** blade of hello Data Lake Store account, click **Access** toosee hello ACLs for a file or a folder.</span></span> <span data-ttu-id="4c464-181">Klicka på **åtkomst** toosee hello ACL: er för hello **katalog** mapp under hello **mydatastore** konto.</span><span class="sxs-lookup"><span data-stu-id="4c464-181">Click **Access** toosee hello ACLs for hello **catalog** folder under hello **mydatastore** account.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

<span data-ttu-id="4c464-183">På det här bladet visas hello överst en översikt över hello behörigheter som du har.</span><span class="sxs-lookup"><span data-stu-id="4c464-183">On this blade, hello top section shows an overview of hello permissions that you have.</span></span> <span data-ttu-id="4c464-184">(Hello skärmbild hello användare är Bob.) Efter att hello åtkomstbehörigheter visas.</span><span class="sxs-lookup"><span data-stu-id="4c464-184">(In hello screenshot, hello user is Bob.) Following that, hello access permissions are shown.</span></span> <span data-ttu-id="4c464-185">Efter att från och med hello **åtkomst** bladet, klickar du på **enkel vy** toosee hello enklare vy.</span><span class="sxs-lookup"><span data-stu-id="4c464-185">After that, from hello **Access** blade, click **Simple View** toosee hello simpler view.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

<span data-ttu-id="4c464-187">Klicka på **Avancerat läge** toosee hello mer avancerat läge, där hello begreppet standard ACL: er, nätmask och en användare visas.</span><span class="sxs-lookup"><span data-stu-id="4c464-187">Click **Advanced View** toosee hello more advanced view, where hello concepts of Default ACLs, mask, and super-user are shown.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a><span data-ttu-id="4c464-189">hello superanvändare</span><span class="sxs-lookup"><span data-stu-id="4c464-189">hello super-user</span></span>

<span data-ttu-id="4c464-190">Superanvändare har hello de flesta rättigheter för alla hello användare i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-190">A super-user has hello most rights of all hello users in hello Data Lake Store.</span></span> <span data-ttu-id="4c464-191">En superanvändare:</span><span class="sxs-lookup"><span data-stu-id="4c464-191">A super-user:</span></span>

* <span data-ttu-id="4c464-192">Har RWX behörigheter för**alla** filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="4c464-192">Has RWX Permissions too**all** files and folders.</span></span>
* <span data-ttu-id="4c464-193">Kan ändra hello behörigheter på en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-193">Can change hello permissions on any file or folder.</span></span>
* <span data-ttu-id="4c464-194">Ändra hello ägande användare eller äger grupp med en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-194">Can change hello owning user or owning group of any file or folder.</span></span>

<span data-ttu-id="4c464-195">I Azure har ett Data Lake Store-konto flera Azure-roller, inklusive:</span><span class="sxs-lookup"><span data-stu-id="4c464-195">In Azure, a Data Lake Store account has several Azure roles, including:</span></span>

* <span data-ttu-id="4c464-196">Ägare</span><span class="sxs-lookup"><span data-stu-id="4c464-196">Owners</span></span>
* <span data-ttu-id="4c464-197">Deltagare</span><span class="sxs-lookup"><span data-stu-id="4c464-197">Contributors</span></span>
* <span data-ttu-id="4c464-198">Läsare</span><span class="sxs-lookup"><span data-stu-id="4c464-198">Readers</span></span>

<span data-ttu-id="4c464-199">Alla i hello **ägare** rollen för ett Data Lake Store-konto är automatiskt Superanvändare för det kontot.</span><span class="sxs-lookup"><span data-stu-id="4c464-199">Everyone in hello **Owners** role for a Data Lake Store account is automatically a super-user for that account.</span></span> <span data-ttu-id="4c464-200">Det finns fler toolearn [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4c464-200">toolearn more, see [Role-based access control](../active-directory/role-based-access-control-configure.md).</span></span>
<span data-ttu-id="4c464-201">Om du vill toocreate en anpassad roll-baserad-åtkomstkontroll (RBAC)-roll som har behörighet för en användare, måste toohave hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="4c464-201">If you want toocreate a custom role-based-access control (RBAC) role that has super-user permissions, it needs toohave hello following permissions:</span></span>
- <span data-ttu-id="4c464-202">Microsoft.DataLakeStore/accounts/Superuser/action</span><span class="sxs-lookup"><span data-stu-id="4c464-202">Microsoft.DataLakeStore/accounts/Superuser/action</span></span>
- <span data-ttu-id="4c464-203">Microsoft.Authorization/roleAssignments/write</span><span class="sxs-lookup"><span data-stu-id="4c464-203">Microsoft.Authorization/roleAssignments/write</span></span>


## <a name="hello-owning-user"></a><span data-ttu-id="4c464-204">hello ägande användare</span><span class="sxs-lookup"><span data-stu-id="4c464-204">hello owning user</span></span>

<span data-ttu-id="4c464-205">hello-användaren som skapade hello-objektet är automatiskt hello ägande användare hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="4c464-205">hello user who created hello item is automatically hello owning user of hello item.</span></span> <span data-ttu-id="4c464-206">En ägande användare kan:</span><span class="sxs-lookup"><span data-stu-id="4c464-206">An owning user can:</span></span>

* <span data-ttu-id="4c464-207">Ändra hello behörigheter för en fil som ägs.</span><span class="sxs-lookup"><span data-stu-id="4c464-207">Change hello permissions of a file that is owned.</span></span>
* <span data-ttu-id="4c464-208">Ändra hello äger grupp för en fil som ägs, så länge hello ägande användaren också är medlem i hello målgruppen.</span><span class="sxs-lookup"><span data-stu-id="4c464-208">Change hello owning group of a file that is owned, as long as hello owning user is also a member of hello target group.</span></span>

> [!NOTE]
> <span data-ttu-id="4c464-209">hello ägande användare *kan* ändra hello ägande användare av en annan fil som företagsägda.</span><span class="sxs-lookup"><span data-stu-id="4c464-209">hello owning user *cannot* change hello owning user of another owned file.</span></span> <span data-ttu-id="4c464-210">Endast en användare kan ändra hello ägande användare av en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-210">Only super-users can change hello owning user of a file or folder.</span></span>
>
>

## <a name="hello-owning-group"></a><span data-ttu-id="4c464-211">hello ägande grupp</span><span class="sxs-lookup"><span data-stu-id="4c464-211">hello owning group</span></span>

<span data-ttu-id="4c464-212">I hello POSIX ACL: er, alla användare som är associerad med en ”primär grupp”.</span><span class="sxs-lookup"><span data-stu-id="4c464-212">In hello POSIX ACLs, every user is associated with a "primary group."</span></span> <span data-ttu-id="4c464-213">Användaren ”alice” kan till exempel hör toohello ”ekonomi” grupp.</span><span class="sxs-lookup"><span data-stu-id="4c464-213">For example, user "alice" might belong toohello "finance" group.</span></span> <span data-ttu-id="4c464-214">Alice kan också tillhör toomultiple grupper, men en grupp alltid är utsedd till sin primära grupp.</span><span class="sxs-lookup"><span data-stu-id="4c464-214">Alice might also belong toomultiple groups, but one group is always designated as her primary group.</span></span> <span data-ttu-id="4c464-215">I POSIX, när Anna skapar en fil anges hello äger gruppen av filen tooher primär grupp, som i det här fallet är ”ekonomi”.</span><span class="sxs-lookup"><span data-stu-id="4c464-215">In POSIX, when Alice creates a file, hello owning group of that file is set tooher primary group, which in this case is "finance."</span></span>

<span data-ttu-id="4c464-216">När ett nytt filsystem-objekt skapas, tilldelar en värdet toohello äger gruppen Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-216">When a new filesystem item is created, Data Lake Store assigns a value toohello owning group.</span></span>

* <span data-ttu-id="4c464-217">**Fall 1**: hello rotmappen ”/”.</span><span class="sxs-lookup"><span data-stu-id="4c464-217">**Case 1**: hello root folder "/".</span></span> <span data-ttu-id="4c464-218">Den här mappen skapas när ett Data Lake Store-konto skapas.</span><span class="sxs-lookup"><span data-stu-id="4c464-218">This folder is created when a Data Lake Store account is created.</span></span> <span data-ttu-id="4c464-219">I det här fallet in hello ägande gruppen toohello användaren som skapade hello-konto.</span><span class="sxs-lookup"><span data-stu-id="4c464-219">In this case, hello owning group is set toohello user who created hello account.</span></span>
* <span data-ttu-id="4c464-220">**Fall 2** (alla andra fall): när ett nytt objekt skapas hello ägande grupp kopieras från hello överordnade mappen.</span><span class="sxs-lookup"><span data-stu-id="4c464-220">**Case 2** (Every other case): When a new item is created, hello owning group is copied from hello parent folder.</span></span>

<span data-ttu-id="4c464-221">hello ägande grupp kan ändras av:</span><span class="sxs-lookup"><span data-stu-id="4c464-221">hello owning group can be changed by:</span></span>
* <span data-ttu-id="4c464-222">Alla superanvändare.</span><span class="sxs-lookup"><span data-stu-id="4c464-222">Any super-users.</span></span>
* <span data-ttu-id="4c464-223">hello äger användaren, om hello ägande användaren också är medlem i hello målgruppen.</span><span class="sxs-lookup"><span data-stu-id="4c464-223">hello owning user, if hello owning user is also a member of hello target group.</span></span>

## <a name="access-check-algorithm"></a><span data-ttu-id="4c464-224">Algoritm för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="4c464-224">Access check algorithm</span></span>

<span data-ttu-id="4c464-225">Följande bild hello representerar hello åtkomst Kontrollera algoritmen för Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="4c464-225">hello following illustration represents hello access check algorithm for Data Lake Store accounts.</span></span>

![ALC-algoritmer för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a><span data-ttu-id="4c464-227">hello mask och ”gällande behörigheter”</span><span class="sxs-lookup"><span data-stu-id="4c464-227">hello mask and "effective permissions"</span></span>

<span data-ttu-id="4c464-228">Hej **mask** är en RWX värde som är används toolimit åtkomst för **namngivna användare**, hello **äger gruppen**, och **namngiven grupp** när du är Utför hello åtkomst Kontrollera algoritm.</span><span class="sxs-lookup"><span data-stu-id="4c464-228">hello **mask** is an RWX value that is used toolimit access for **named users**, hello **owning group**, and **named groups** when you're performing hello access check algorithm.</span></span> <span data-ttu-id="4c464-229">Här följer hello viktiga begrepp för hello mask.</span><span class="sxs-lookup"><span data-stu-id="4c464-229">Here are hello key concepts for hello mask.</span></span>

* <span data-ttu-id="4c464-230">hello mask skapar ”gällande behörigheter”.</span><span class="sxs-lookup"><span data-stu-id="4c464-230">hello mask creates "effective permissions."</span></span> <span data-ttu-id="4c464-231">Det vill säga ändrar hello behörigheten när hello åtkomstkontrollen.</span><span class="sxs-lookup"><span data-stu-id="4c464-231">That is, it modifies hello permissions at hello time of access check.</span></span>
* <span data-ttu-id="4c464-232">hello mask kan redigeras direkt av hello filens ägare och en användare.</span><span class="sxs-lookup"><span data-stu-id="4c464-232">hello mask can be directly edited by hello file owner and any super-users.</span></span>
* <span data-ttu-id="4c464-233">hello mask kan ta bort behörigheter toocreate hello gällande behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-233">hello mask can remove permissions toocreate hello effective permission.</span></span> <span data-ttu-id="4c464-234">hello mask *kan* lägga till behörigheter toohello gällande behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-234">hello mask *cannot* add permissions toohello effective permission.</span></span>

<span data-ttu-id="4c464-235">Låt oss titta på några exempel.</span><span class="sxs-lookup"><span data-stu-id="4c464-235">Let's look at some examples.</span></span> <span data-ttu-id="4c464-236">I följande exempel hello, hello inställd för**RWX**, vilket innebär att hello mask tar inte bort alla behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-236">In hello following example, hello mask is set too**RWX**, which means that hello mask does not remove any permissions.</span></span> <span data-ttu-id="4c464-237">hello gällande behörigheter för hello namngiven användare äger grupp och namngivna gruppen ändras inte under hello åtkomstkontrollen.</span><span class="sxs-lookup"><span data-stu-id="4c464-237">hello effective permissions for hello named user, owning group, and named group are not altered during hello access check.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

<span data-ttu-id="4c464-239">I följande exempel hello, hello inställd för**R-X**.</span><span class="sxs-lookup"><span data-stu-id="4c464-239">In hello following example, hello mask is set too**R-X**.</span></span> <span data-ttu-id="4c464-240">Det innebär att den **inaktiverar hello skrivbehörighet** för **namngiven användare**, **äger gruppen**, och **med namnet grupp** när hello åtkomst Kontrollera.</span><span class="sxs-lookup"><span data-stu-id="4c464-240">This means that it **turns off hello Write permissions** for **named user**, **owning group**, and **named group** at hello time of access check.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

<span data-ttu-id="4c464-242">För referens anger du det här är där hello mask för en fil eller mapp visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c464-242">For reference, here is where hello mask for a file or folder appears in hello Azure portal.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> <span data-ttu-id="4c464-244">För en ny Data Lake Store-konto, hello mask för hello ACL för åtkomst och standard ACL för hello rot (”/”) som standard tooRWX.</span><span class="sxs-lookup"><span data-stu-id="4c464-244">For a new Data Lake Store account, hello mask for hello Access ACL and Default ACL of hello root folder ("/") defaults tooRWX.</span></span>
>
>

## <a name="permissions-on-new-files-and-folders"></a><span data-ttu-id="4c464-245">Behörigheter för nya filer och mappar</span><span class="sxs-lookup"><span data-stu-id="4c464-245">Permissions on new files and folders</span></span>

<span data-ttu-id="4c464-246">När en ny fil eller mapp skapas under en befintlig mapp, anger hello standard ACL på hello överordnad mapp:</span><span class="sxs-lookup"><span data-stu-id="4c464-246">When a new file or folder is created under an existing folder, hello Default ACL on hello parent folder determines:</span></span>

- <span data-ttu-id="4c464-247">En underordnad mapps standard-ACL och åtkomst-ACL.</span><span class="sxs-lookup"><span data-stu-id="4c464-247">A child folder’s Default ACL and Access ACL.</span></span>
- <span data-ttu-id="4c464-248">En underordnad fils åtkomst-ACL (filer har inte en standard-ACL).</span><span class="sxs-lookup"><span data-stu-id="4c464-248">A child file's Access ACL (files do not have a Default ACL).</span></span>

### <a name="hello-access-acl-of-a-child-file-or-folder"></a><span data-ttu-id="4c464-249">hello ACL för åtkomst av en underordnad fil eller mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-249">hello Access ACL of a child file or folder</span></span>

<span data-ttu-id="4c464-250">När en underordnad fil eller mapp skapas, kopieras hello överordnade standard ACL som hello ACL för åtkomst av hello underordnade filen eller mappen.</span><span class="sxs-lookup"><span data-stu-id="4c464-250">When a child file or folder is created, hello parent's Default ACL is copied as hello Access ACL of hello child file or folder.</span></span> <span data-ttu-id="4c464-251">Även om **andra** användaren har behörighet för RWX i hello överordnade standard ACL, det tas bort från ACL för åtkomst av hello underordnade objekt.</span><span class="sxs-lookup"><span data-stu-id="4c464-251">Also, if **other** user has RWX permissions in hello parent's default ACL, it is removed from hello child item's Access ACL.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

<span data-ttu-id="4c464-253">I de flesta fall är hello tidigare information du behöver tooknow om hur ACL för åtkomst av ett underordnat objekt fastställs.</span><span class="sxs-lookup"><span data-stu-id="4c464-253">In most scenarios, hello previous information is all you need tooknow about how a child item’s Access ACL is determined.</span></span> <span data-ttu-id="4c464-254">Men om du är bekant med POSIX-system och vill toounderstand djupgående hur transformationen uppnås avsnittet hello [Umasks roll i att skapa hello ACL för åtkomst för nya filer och mappar](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4c464-254">However, if you are familiar with POSIX systems and want toounderstand in-depth how this transformation is achieved, see hello section [Umask’s role in creating hello Access ACL for new files and folders](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) later in this article.</span></span>


### <a name="a-child-folders-default-acl"></a><span data-ttu-id="4c464-255">En underordnad mapps standard-ACL</span><span class="sxs-lookup"><span data-stu-id="4c464-255">A child folder's Default ACL</span></span>

<span data-ttu-id="4c464-256">När en underordnad mapp skapas under en överordnad mapp, kopieras hello överordnade mappen standard ACL över som standard ACL i toohello underordnad mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-256">When a child folder is created under a parent folder, hello parent folder's Default ACL is copied over as is toohello child folder's Default ACL.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a><span data-ttu-id="4c464-258">Avancerad information för att förstå ACL:er i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4c464-258">Advanced topics for understanding ACLs in Data Lake Store</span></span>

<span data-ttu-id="4c464-259">Följande är några avancerade ämnen toohelp du förstår hur ACL-listor fastställs för Data Lake Store-filer eller mappar.</span><span class="sxs-lookup"><span data-stu-id="4c464-259">Following are some advanced topics toohelp you understand how ACLs are determined for Data Lake Store files or folders.</span></span>

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a><span data-ttu-id="4c464-260">Umasks roll i att skapa hello ACL för åtkomst för nya filer och mappar</span><span class="sxs-lookup"><span data-stu-id="4c464-260">Umask’s role in creating hello Access ACL for new files and folders</span></span>

<span data-ttu-id="4c464-261">I ett system med POSIX-kompatibla är hello allmänna begrepp som umask är ett 9-bitars värde på hello överordnad mapp som har använt tootransform hello behörighet för **ägande användare**, **äger gruppen**, och ** andra** på hello ACL för åtkomst av en ny underordnad fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-261">In a POSIX-compliant system, hello general concept is that umask is a 9-bit value on hello parent folder that's used tootransform hello permission for **owning user**, **owning group**, and **other** on hello Access ACL of a new child file or folder.</span></span> <span data-ttu-id="4c464-262">hello bitarna i en umask identifiera vilka bits tooturn ut i ACL för åtkomst av hello underordnade objekt.</span><span class="sxs-lookup"><span data-stu-id="4c464-262">hello bits of a umask identify which bits tooturn off in hello child item’s Access ACL.</span></span> <span data-ttu-id="4c464-263">Därför används tooselectively förhindra hello spridning av behörigheter för **ägande användare**, **äger gruppen**, och **andra**.</span><span class="sxs-lookup"><span data-stu-id="4c464-263">Thus it is used tooselectively prevent hello propagation of permissions for **owning user**, **owning group**, and **other**.</span></span>

<span data-ttu-id="4c464-264">Hej umask är vanligtvis ett konfigurationsalternativ för hela platsen som styrs av administratörer i ett HDFS-system.</span><span class="sxs-lookup"><span data-stu-id="4c464-264">In an HDFS system, hello umask is typically a sitewide configuration option that is controlled by administrators.</span></span> <span data-ttu-id="4c464-265">Data Lake Store använder en **umask för konto** som inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="4c464-265">Data Lake Store uses an **account-wide umask** that cannot be changed.</span></span> <span data-ttu-id="4c464-266">följande tabell visar hello hello ta bort masken för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-266">hello following table shows hello unmask for Data Lake Store.</span></span>

| <span data-ttu-id="4c464-267">Användargrupp</span><span class="sxs-lookup"><span data-stu-id="4c464-267">User group</span></span>  | <span data-ttu-id="4c464-268">Inställning</span><span class="sxs-lookup"><span data-stu-id="4c464-268">Setting</span></span> | <span data-ttu-id="4c464-269">Effekt av nytt underordnade objekts åtkomst-ACL</span><span class="sxs-lookup"><span data-stu-id="4c464-269">Effect on new child item's Access ACL</span></span> |
|------------ |---------|---------------------------------------|
| <span data-ttu-id="4c464-270">Ägande användare</span><span class="sxs-lookup"><span data-stu-id="4c464-270">Owning user</span></span> | ---     | <span data-ttu-id="4c464-271">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="4c464-271">No effect</span></span>                             |
| <span data-ttu-id="4c464-272">Ägande grupp</span><span class="sxs-lookup"><span data-stu-id="4c464-272">Owning group</span></span>| ---     | <span data-ttu-id="4c464-273">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="4c464-273">No effect</span></span>                             |
| <span data-ttu-id="4c464-274">Annat</span><span class="sxs-lookup"><span data-stu-id="4c464-274">Other</span></span>       | <span data-ttu-id="4c464-275">RWX</span><span class="sxs-lookup"><span data-stu-id="4c464-275">RWX</span></span>     | <span data-ttu-id="4c464-276">Ta bort läsa + skriva + köra</span><span class="sxs-lookup"><span data-stu-id="4c464-276">Remove Read + Write + Execute</span></span>         |

<span data-ttu-id="4c464-277">hello följande bild visar den här umask i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="4c464-277">hello following illustration shows this umask in action.</span></span> <span data-ttu-id="4c464-278">hello net effekten är tooremove **läsa + skriva + köra** för **andra** användare.</span><span class="sxs-lookup"><span data-stu-id="4c464-278">hello net effect is tooremove **Read + Write + Execute** for **other** user.</span></span> <span data-ttu-id="4c464-279">Eftersom hello umask inte angav bits för **ägande användare** och **äger gruppen**, de behörigheterna inte omvandlas.</span><span class="sxs-lookup"><span data-stu-id="4c464-279">Because hello umask did not specify bits for **owning user** and **owning group**, those permissions are not transformed.</span></span>

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a><span data-ttu-id="4c464-281">Fäst hello-bitars</span><span class="sxs-lookup"><span data-stu-id="4c464-281">hello sticky bit</span></span>

<span data-ttu-id="4c464-282">Fäst hello-biten är en avancerad funktion för ett POSIX-filsystem.</span><span class="sxs-lookup"><span data-stu-id="4c464-282">hello sticky bit is a more advanced feature of a POSIX filesystem.</span></span> <span data-ttu-id="4c464-283">Hello kontexten för Data Lake Store är det osannolikt hello Fäst biten krävs.</span><span class="sxs-lookup"><span data-stu-id="4c464-283">In hello context of Data Lake Store, it is unlikely that hello sticky bit will be needed.</span></span>

<span data-ttu-id="4c464-284">hello visar följande tabell hur hello Fäst bitars fungerar i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-284">hello following table shows how hello sticky bit works in Data Lake Store.</span></span>

| <span data-ttu-id="4c464-285">Användargrupp</span><span class="sxs-lookup"><span data-stu-id="4c464-285">User group</span></span>         | <span data-ttu-id="4c464-286">Fil</span><span class="sxs-lookup"><span data-stu-id="4c464-286">File</span></span>    | <span data-ttu-id="4c464-287">Mapp</span><span class="sxs-lookup"><span data-stu-id="4c464-287">Folder</span></span> |
|--------------------|---------|-------------------------|
| <span data-ttu-id="4c464-288">Sticky bit **AV**</span><span class="sxs-lookup"><span data-stu-id="4c464-288">Sticky bit **OFF**</span></span> | <span data-ttu-id="4c464-289">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="4c464-289">No effect</span></span>   | <span data-ttu-id="4c464-290">Ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="4c464-290">No effect.</span></span>           |
| <span data-ttu-id="4c464-291">Sticky bit **PÅ**</span><span class="sxs-lookup"><span data-stu-id="4c464-291">Sticky bit **ON**</span></span>  | <span data-ttu-id="4c464-292">Ingen effekt</span><span class="sxs-lookup"><span data-stu-id="4c464-292">No effect</span></span>   | <span data-ttu-id="4c464-293">Hindrar alla utom **överordnad användare** och hello **ägande användare** för ett underordnat objekt tar bort eller byta namn på det underordnade objektet.</span><span class="sxs-lookup"><span data-stu-id="4c464-293">Prevents anyone except **super-users** and hello **owning user** of a child item from deleting or renaming that child item.</span></span>               |

<span data-ttu-id="4c464-294">Fäst hello-bitars visas inte i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c464-294">hello sticky bit is not shown in hello Azure portal.</span></span>

## <a name="common-questions-about-acls-in-data-lake-store"></a><span data-ttu-id="4c464-295">Vanliga frågor om ACL:er i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4c464-295">Common questions about ACLs in Data Lake Store</span></span>

<span data-ttu-id="4c464-296">Här är några frågor som ofta kommer upp rörande ACL:er i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-296">Here are some questions that come up often about ACLs in Data Lake Store.</span></span>

### <a name="do-i-have-tooenable-support-for-acls"></a><span data-ttu-id="4c464-297">Måste jag tooenable stöd för ACL: er?</span><span class="sxs-lookup"><span data-stu-id="4c464-297">Do I have tooenable support for ACLs?</span></span>

<span data-ttu-id="4c464-298">Nej.</span><span class="sxs-lookup"><span data-stu-id="4c464-298">No.</span></span> <span data-ttu-id="4c464-299">Åtkomstkontroll via ACL:er är alltid aktiverat för ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="4c464-299">Access control via ACLs is always on for a Data Lake Store account.</span></span>

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a><span data-ttu-id="4c464-300">Vilka behörigheter som krävs toorecursively ta bort en mapp och dess innehåll?</span><span class="sxs-lookup"><span data-stu-id="4c464-300">Which permissions are required toorecursively delete a folder and its contents?</span></span>

* <span data-ttu-id="4c464-301">hello överordnad mapp måste ha **skriva + köra** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-301">hello parent folder must have **Write + Execute** permissions.</span></span>
* <span data-ttu-id="4c464-302">Hej mappen toobe tas bort och alla mappar i denna kräver **läsa + skriva + köra** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4c464-302">hello folder toobe deleted, and every folder within it, requires **Read + Write + Execute** permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="4c464-303">Du behöver inte skrivbehörighet toodelete filer i mappar.</span><span class="sxs-lookup"><span data-stu-id="4c464-303">You do not need Write permissions toodelete files in folders.</span></span> <span data-ttu-id="4c464-304">Dessutom hello rotmappen ”/” kan **aldrig** tas bort.</span><span class="sxs-lookup"><span data-stu-id="4c464-304">Also, hello root folder "/" can **never** be deleted.</span></span>
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a><span data-ttu-id="4c464-305">Vem är hello ägare till en fil eller mapp?</span><span class="sxs-lookup"><span data-stu-id="4c464-305">Who is hello owner of a file or folder?</span></span>

<span data-ttu-id="4c464-306">hello skapare av en fil eller mapp blir hello ägare.</span><span class="sxs-lookup"><span data-stu-id="4c464-306">hello creator of a file or folder becomes hello owner.</span></span>

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a><span data-ttu-id="4c464-307">Vilken grupp har angetts som hello äger en fil eller mapp på Skapa grupp?</span><span class="sxs-lookup"><span data-stu-id="4c464-307">Which group is set as hello owning group of a file or folder at creation?</span></span>

<span data-ttu-id="4c464-308">hello ägande grupp kopieras från hello äger grupp hello överordnade mappen under vilka hello nya filen eller mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="4c464-308">hello owning group is copied from hello owning group of hello parent folder under which hello new file or folder is created.</span></span>

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a><span data-ttu-id="4c464-309">Jag är hello ägande användare av en fil men du har inte hello RWX behörigheter som jag behöver.</span><span class="sxs-lookup"><span data-stu-id="4c464-309">I am hello owning user of a file but I don’t have hello RWX permissions I need.</span></span> <span data-ttu-id="4c464-310">Vad gör jag nu?</span><span class="sxs-lookup"><span data-stu-id="4c464-310">What do I do?</span></span>

<span data-ttu-id="4c464-311">hello ägande användaren kan ändra hello behörigheter för hello filen toogive själva RWX behörighet de behöver.</span><span class="sxs-lookup"><span data-stu-id="4c464-311">hello owning user can change hello permissions of hello file toogive themselves any RWX permissions they need.</span></span>

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a><span data-ttu-id="4c464-312">När jag tittar på ACL: er i hello Azure-portalen visas användarnamn men via API: er, visas GUID, varför?</span><span class="sxs-lookup"><span data-stu-id="4c464-312">When I look at ACLs in hello Azure portal I see user names but through APIs, I see GUIDs, why is that?</span></span>

<span data-ttu-id="4c464-313">Poster i hello ACL: er lagras som GUID som motsvarar toousers i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c464-313">Entries in hello ACLs are stored as GUIDs that correspond toousers in Azure AD.</span></span> <span data-ttu-id="4c464-314">hello API: er returnera hello GUID som är.</span><span class="sxs-lookup"><span data-stu-id="4c464-314">hello APIs return hello GUIDs as is.</span></span> <span data-ttu-id="4c464-315">hello Azure-portalen försöker toomake ACL: er enklare toouse genom att översätta hello GUID till egna namn när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="4c464-315">hello Azure portal tries toomake ACLs easier toouse by translating hello GUIDs into friendly names when possible.</span></span>

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a><span data-ttu-id="4c464-316">Varför ibland visas GUID i hello ACL: er när jag använder hello Azure-portalen?</span><span class="sxs-lookup"><span data-stu-id="4c464-316">Why do I sometimes see GUIDs in hello ACLs when I'm using hello Azure portal?</span></span>

<span data-ttu-id="4c464-317">Ett GUID som visas när hello användaren inte finns i Azure AD längre.</span><span class="sxs-lookup"><span data-stu-id="4c464-317">A GUID is shown when hello user doesn't exist in Azure AD anymore.</span></span> <span data-ttu-id="4c464-318">Detta inträffar vanligtvis när hello användaren har lämnat företaget hello eller om deras konto har tagits bort i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c464-318">Usually this happens when hello user has left hello company or if their account has been deleted in Azure AD.</span></span>

### <a name="does-data-lake-store-support-inheritance-of-acls"></a><span data-ttu-id="4c464-319">Stöds arv av ACL:er i Data Lake Store?</span><span class="sxs-lookup"><span data-stu-id="4c464-319">Does Data Lake Store support inheritance of ACLs?</span></span>

<span data-ttu-id="4c464-320">Nej.</span><span class="sxs-lookup"><span data-stu-id="4c464-320">No.</span></span>

### <a name="what-is-hello-difference-between-mask-and-umask"></a><span data-ttu-id="4c464-321">Vad är hello skillnaden mellan mask och umask?</span><span class="sxs-lookup"><span data-stu-id="4c464-321">What is hello difference between mask and umask?</span></span>

| <span data-ttu-id="4c464-322">mask</span><span class="sxs-lookup"><span data-stu-id="4c464-322">mask</span></span> | <span data-ttu-id="4c464-323">umask</span><span class="sxs-lookup"><span data-stu-id="4c464-323">umask</span></span>|
|------|------|
| <span data-ttu-id="4c464-324">Hej **mask** egenskapen är tillgänglig på varje fil och mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-324">hello **mask** property is available on every file and folder.</span></span> | <span data-ttu-id="4c464-325">Hej **umask** är en egenskap hos hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="4c464-325">hello **umask** is a property of hello Data Lake Store account.</span></span> <span data-ttu-id="4c464-326">Det är därför bara en enda umask i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4c464-326">So there is only a single umask in hello Data Lake Store.</span></span>    |
| <span data-ttu-id="4c464-327">hello ägande användare eller äger grupp för en fil eller en användare kan ändras hello egenskap i en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-327">hello mask property on a file or folder can be altered by hello owning user or owning group of a file or a super-user.</span></span> | <span data-ttu-id="4c464-328">Hej umask egenskapen kan inte ändras av någon användare även superanvändare.</span><span class="sxs-lookup"><span data-stu-id="4c464-328">hello umask property cannot be modified by any user, even a super-user.</span></span> <span data-ttu-id="4c464-329">Det är ett konstant värde som inte ändras.</span><span class="sxs-lookup"><span data-stu-id="4c464-329">It is an unchangeable, constant value.</span></span>|
| <span data-ttu-id="4c464-330">hello egenskap används under hello åtkomst Kontrollera algoritmen vid körning toodetermine om en användare har rätt hello-tooperform på åtgärden på en fil eller mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-330">hello mask property is used during hello access check algorithm at runtime toodetermine whether a user has hello right tooperform on operation on a file or folder.</span></span> <span data-ttu-id="4c464-331">hello roll hello mask är toocreate ”gällande behörigheter” när hello åtkomstkontrollen.</span><span class="sxs-lookup"><span data-stu-id="4c464-331">hello role of hello mask is toocreate "effective permissions" at hello time of access check.</span></span> | <span data-ttu-id="4c464-332">Hej umask används inte vid åtkomstkontrollen alls.</span><span class="sxs-lookup"><span data-stu-id="4c464-332">hello umask is not used during access check at all.</span></span> <span data-ttu-id="4c464-333">Hej umask är används toodetermine hello ACL för åtkomst av den nya underordnade objekt i en mapp.</span><span class="sxs-lookup"><span data-stu-id="4c464-333">hello umask is used toodetermine hello Access ACL of new child items of a folder.</span></span> |
| <span data-ttu-id="4c464-334">hello masken är ett 3-bitars RWX värde som gäller toonamed användare, namngiven grupp och ägande användare när hello åtkomstkontrollen.</span><span class="sxs-lookup"><span data-stu-id="4c464-334">hello mask is a 3-bit RWX value that applies toonamed user, named group, and owning user at hello time of access check.</span></span>| <span data-ttu-id="4c464-335">Hej umask är ett 9-bitars värde som gäller toohello ägande användare, grupp, det ägande och **andra** av en ny underdomän.</span><span class="sxs-lookup"><span data-stu-id="4c464-335">hello umask is a 9-bit value that applies toohello owning user, owning group, and **other** of a new child.</span></span>|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a><span data-ttu-id="4c464-336">Var hittar jag mer information om POSIX-modellen för åtkomstkontroll?</span><span class="sxs-lookup"><span data-stu-id="4c464-336">Where can I learn more about POSIX access control model?</span></span>

* [<span data-ttu-id="4c464-337">POSIX-åtkomstkontrollistor på Linux</span><span class="sxs-lookup"><span data-stu-id="4c464-337">POSIX Access Control Lists on Linux</span></span>](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [<span data-ttu-id="4c464-338">Guide för HDFS-behörighet</span><span class="sxs-lookup"><span data-stu-id="4c464-338">HDFS permission guide</span></span>](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [<span data-ttu-id="4c464-339">Vanliga frågor och svar om POSIX</span><span class="sxs-lookup"><span data-stu-id="4c464-339">POSIX FAQ</span></span>](http://www.opengroup.org/austin/papers/posix_faq.html)

* [<span data-ttu-id="4c464-340">POSIX 1003.1 2008</span><span class="sxs-lookup"><span data-stu-id="4c464-340">POSIX 1003.1 2008</span></span>](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [<span data-ttu-id="4c464-341">POSIX 1003.1 2013</span><span class="sxs-lookup"><span data-stu-id="4c464-341">POSIX 1003.1 2013</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [<span data-ttu-id="4c464-342">POSIX 1003.1 2016</span><span class="sxs-lookup"><span data-stu-id="4c464-342">POSIX 1003.1 2016</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [<span data-ttu-id="4c464-343">POSIX ACL på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4c464-343">POSIX ACL on Ubuntu</span></span>](https://help.ubuntu.com/community/FilePermissionsACLs)

* [<span data-ttu-id="4c464-344">ACL med åtkomstkontrollistor på Linux</span><span class="sxs-lookup"><span data-stu-id="4c464-344">ACL using access control lists on Linux</span></span>](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a><span data-ttu-id="4c464-345">Se även</span><span class="sxs-lookup"><span data-stu-id="4c464-345">See also</span></span>

* [<span data-ttu-id="4c464-346">Översikt över Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4c464-346">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
