---
title: "Azure Analysis Services självstudiekurs 11: Skapa roller | Microsoft Docs"
description: "Beskriver hur du skapar roller i Azure Analysis Services-självstudieprojektet."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 085a36edd2a0e80123ac8754b438bceadfa6c0e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-11-create-roles"></a><span data-ttu-id="79038-103">Lektion 11: Skapa roller</span><span class="sxs-lookup"><span data-stu-id="79038-103">Lesson 11: Create roles</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="79038-104">Under den här lektionen skapar du roller.</span><span class="sxs-lookup"><span data-stu-id="79038-104">In this lesson, you create roles.</span></span> <span data-ttu-id="79038-105">Roller skyddar modelldatabasobjekt och data genom att begränsa åtkomsten till endast de användare som är rollmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="79038-105">Roles provide model database object and data security by limiting access to only those users that are role members.</span></span> <span data-ttu-id="79038-106">Varje roll definieras med en enskild behörighet: Ingen, Läsa, Läsa och bearbeta, Bearbeta eller Administratör.</span><span class="sxs-lookup"><span data-stu-id="79038-106">Each role is defined with a single permission: None, Read, Read and Process, Process, or Administrator.</span></span> <span data-ttu-id="79038-107">Roller kan definieras vid modellredigering med hjälp av rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="79038-107">Roles can be defined during model authoring by using Role Manager.</span></span> <span data-ttu-id="79038-108">När en modell har distribuerats kan du hantera roller med SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="79038-108">After a model has been deployed, you can manage roles by using SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="79038-109">Mer information finns i [Roller](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="79038-109">To learn more, see [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span></span>
  
> [!NOTE]  
> <span data-ttu-id="79038-110">Du behöver inte skapa roller för att slutföra den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="79038-110">Creating roles is not necessary to complete this tutorial.</span></span> <span data-ttu-id="79038-111">Som standard har det konto som du är inloggad med administratörsbehörighet på modellen.</span><span class="sxs-lookup"><span data-stu-id="79038-111">By default, the account you are currently logged in with has Administrator privileges on the model.</span></span> <span data-ttu-id="79038-112">Men för att andra användare i din organisation ska kunna se modellen med en rapporteringsklient måste du skapa minst en roll med läsbehörighet och lägga till dessa användare som medlemmar i rollen.</span><span class="sxs-lookup"><span data-stu-id="79038-112">However, for other users in your organization to browse by using a reporting client, you must create at least one role with Read permissions and add those users as members.</span></span>  
  
<span data-ttu-id="79038-113">Du kan skapa tre roller:</span><span class="sxs-lookup"><span data-stu-id="79038-113">You create three roles:</span></span>  
  
-   <span data-ttu-id="79038-114">**Säljchef** – Den här rollen kan innefatta användare i organisationen som du vill ska ha läsbehörighet till alla modellobjekt och data.</span><span class="sxs-lookup"><span data-stu-id="79038-114">**Sales Manager** – This role can include users in your organization for which you want to have Read permission to all model objects and data.</span></span>  
  
-   <span data-ttu-id="79038-115">**Försäljningsanalytiker, USA** – Den här rollen kan innefatta användare i organisationen som endast ska ha behörighet att visa data som rör försäljning i USA.</span><span class="sxs-lookup"><span data-stu-id="79038-115">**Sales Analyst US** – This role can include users in your organization for which you want only to be able to browse data related to sales in the United States.</span></span> <span data-ttu-id="79038-116">För den här rollen använder du en DAX-formel för att definiera ett *radfilter* som begränsar medlemmarna till att endast kunna visa data för USA.</span><span class="sxs-lookup"><span data-stu-id="79038-116">For this role, you use a DAX formula to define a *Row Filter*, which restricts members to browse data only for the United States.</span></span>  
  
-   <span data-ttu-id="79038-117">**Administratör** – Den här rollen kan innefatta användare som du vill ska ha administratörsbehörighet, vilket ger dem obegränsad åtkomst och behörighet att utföra administrativa uppgifter för modelldatabasen.</span><span class="sxs-lookup"><span data-stu-id="79038-117">**Administrator** – This role can include users for which you want to have Administrator permission, which allows unlimited access and permissions to perform administrative tasks on the model database.</span></span>  
  
<span data-ttu-id="79038-118">Eftersom Windows-användarkonton och -gruppkonton i din organisation är unika kan du lägga till konton från din organisation till medlemmar.</span><span class="sxs-lookup"><span data-stu-id="79038-118">Because Windows user and group accounts in your organization are unique, you can add accounts from your particular organization to members.</span></span> <span data-ttu-id="79038-119">Men i den här självstudiekursen kan du lämna medlemsfälten tomma om du vill.</span><span class="sxs-lookup"><span data-stu-id="79038-119">However, for this tutorial, you can also leave the members blank.</span></span> <span data-ttu-id="79038-120">Du får testa effekten av varje roll senare i lektion 12: Analysera i Excel.</span><span class="sxs-lookup"><span data-stu-id="79038-120">You test the effect of each role later in Lesson 12: Analyze in Excel.</span></span>  
  
<span data-ttu-id="79038-121">Uppskattad tidsåtgång för den här lektionen: **15 minuter**</span><span class="sxs-lookup"><span data-stu-id="79038-121">Estimated time to complete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="79038-122">Krav</span><span class="sxs-lookup"><span data-stu-id="79038-122">Prerequisites</span></span>  
<span data-ttu-id="79038-123">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="79038-123">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="79038-124">Innan du utför uppgifterna under den här lektionen bör du ha slutfört föregående lektion: [Lektion 10: Skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="79038-124">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span>  
  
## <a name="create-roles"></a><span data-ttu-id="79038-125">Skapa roller</span><span class="sxs-lookup"><span data-stu-id="79038-125">Create roles</span></span>  
  
#### <a name="to-create-a-sales-manager-user-role"></a><span data-ttu-id="79038-126">Så här skapar du användarrollen Säljchef</span><span class="sxs-lookup"><span data-stu-id="79038-126">To create a Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="79038-127">Högerklicka på **Roller** > **Roller** i tabellmodellutforskaren.</span><span class="sxs-lookup"><span data-stu-id="79038-127">In Tabular Model Explorer, right-click **Roles** > **Roles**.</span></span>  
  
2.  <span data-ttu-id="79038-128">Klicka på **Ny** i rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="79038-128">In Role Manager, click **New**.</span></span>  
  
3.  <span data-ttu-id="79038-129">Klicka på den nya rollen och ändra namnet på rollen till **Säljchef** i kolumnen **Namn**.</span><span class="sxs-lookup"><span data-stu-id="79038-129">Click the new role, and then in the **Name** column, rename the role to **Sales Manager**.</span></span>  
  
4.  <span data-ttu-id="79038-130">Klicka på listrutan i kolumnen **Behörigheter** och välj sedan behörigheten **Läsa**.</span><span class="sxs-lookup"><span data-stu-id="79038-130">In the **Permissions** column, click the dropdown list, and then select the **Read** permission.</span></span> 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  <span data-ttu-id="79038-132">Valfritt: Klicka på fliken **Medlemmar** och sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="79038-132">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="79038-133">I dialogrutan **Välj användare eller grupper** anger du de Windows-användare eller grupper från din organisation som du vill ska ingå i rollen.</span><span class="sxs-lookup"><span data-stu-id="79038-133">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span>  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a><span data-ttu-id="79038-134">Så här skapar du användarrollen Försäljningsanalytiker, USA</span><span class="sxs-lookup"><span data-stu-id="79038-134">To create a Sales Analyst US user role</span></span>  
  
1.  <span data-ttu-id="79038-135">Klicka på **Ny** i rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="79038-135">In Role Manager, click **New**.</span></span>    
  
2.  <span data-ttu-id="79038-136">Byt namn på rollen till **Försäljningsanalytiker, USA**.</span><span class="sxs-lookup"><span data-stu-id="79038-136">Rename the role to **Sales Analyst US**.</span></span>  
  
3.  <span data-ttu-id="79038-137">Ge rollen **läsbehörighet**.</span><span class="sxs-lookup"><span data-stu-id="79038-137">Give this role **Read** permission.</span></span>  
  
4.  <span data-ttu-id="79038-138">Klicka på fliken Radfilter och skriv sedan följande formel i kolumnen DAX-filter, endast för tabellen **DimGeography**:</span><span class="sxs-lookup"><span data-stu-id="79038-138">Click the Row Filters tab, and then for the **DimGeography** table only, in the DAX Filter column, type the following formula:</span></span>  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    <span data-ttu-id="79038-139">En radfilterformel måste matcha ett booleskt värde (SANT/FALSKT).</span><span class="sxs-lookup"><span data-stu-id="79038-139">A Row Filter formula must resolve to a Boolean (TRUE/FALSE) value.</span></span> <span data-ttu-id="79038-140">Med den här formeln anger du att endast rader med land-/regionskoden ”US” visas för användaren.</span><span class="sxs-lookup"><span data-stu-id="79038-140">With this formula, you are specifying that only rows with the Country Region Code value of “US” are visible to the user.</span></span>  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  <span data-ttu-id="79038-142">Valfritt: Klicka på fliken **Medlemmar** och sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="79038-142">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="79038-143">I dialogrutan **Välj användare eller grupper** anger du de Windows-användare eller grupper från din organisation som du vill ska ingå i rollen.</span><span class="sxs-lookup"><span data-stu-id="79038-143">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span>  
  
#### <a name="to-create-an-administrator-user-role"></a><span data-ttu-id="79038-144">Så här skapar du användarrollen Administratör</span><span class="sxs-lookup"><span data-stu-id="79038-144">To create an Administrator user role</span></span>  
  
1.  <span data-ttu-id="79038-145">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="79038-145">Click **New**.</span></span>  
  
2.  <span data-ttu-id="79038-146">Byt namn på rollen till **Administratör**.</span><span class="sxs-lookup"><span data-stu-id="79038-146">Rename the role to **Administrator**.</span></span>  
  
3.  <span data-ttu-id="79038-147">Ge den här rollen **administratörsbehörighet**.</span><span class="sxs-lookup"><span data-stu-id="79038-147">Give this role **Administrator** permission.</span></span>  
  
4.  <span data-ttu-id="79038-148">Valfritt: Klicka på fliken **Medlemmar** och sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="79038-148">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="79038-149">I dialogrutan **Välj användare eller grupper** anger du de Windows-användare eller grupper från din organisation som du vill ska ingå i rollen.</span><span class="sxs-lookup"><span data-stu-id="79038-149">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span> 
  
  
## <a name="whats-next"></a><span data-ttu-id="79038-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79038-150">What's next?</span></span>
<span data-ttu-id="79038-151">[Lektion 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="79038-151">[Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>

  
  
