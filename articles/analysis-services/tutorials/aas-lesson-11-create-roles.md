---
<span data-ttu-id="e8e1d-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 11: skapa roller | Microsoft Docs ”beskrivning: Beskriver hur toocreate roller i hello självstudiekursen Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-101">title: aaa"Azure Analysis Services tutorial lesson 11: Create roles | Microsoft Docs" description: Describes how toocreate roles in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="e8e1d-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="e8e1d-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="e8e1d-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="e8e1d-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-11-create-roles"></a><span data-ttu-id="e8e1d-104">Lektion 11: Skapa roller</span><span class="sxs-lookup"><span data-stu-id="e8e1d-104">Lesson 11: Create roles</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="e8e1d-105">Under den här lektionen skapar du roller.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-105">In this lesson, you create roles.</span></span> <span data-ttu-id="e8e1d-106">Roller skyddar modellen databasen objekt och data genom att begränsa åtkomst tooonly användare som är medlemmar i rollen.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-106">Roles provide model database object and data security by limiting access tooonly those users that are role members.</span></span> <span data-ttu-id="e8e1d-107">Varje roll definieras med en enskild behörighet: Ingen, Läsa, Läsa och bearbeta, Bearbeta eller Administratör.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-107">Each role is defined with a single permission: None, Read, Read and Process, Process, or Administrator.</span></span> <span data-ttu-id="e8e1d-108">Roller kan definieras vid modellredigering med hjälp av rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-108">Roles can be defined during model authoring by using Role Manager.</span></span> <span data-ttu-id="e8e1d-109">När en modell har distribuerats kan du hantera roller med SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="e8e1d-109">After a model has been deployed, you can manage roles by using SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="e8e1d-110">Det finns fler toolearn [roller](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="e8e1d-110">toolearn more, see [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span></span>
  
> [!NOTE]  
> <span data-ttu-id="e8e1d-111">Skapa roller är inte nödvändigt toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-111">Creating roles is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="e8e1d-112">Som standard har hello konto du är inloggad med administratörsbehörighet på hello modellen.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-112">By default, hello account you are currently logged in with has Administrator privileges on hello model.</span></span> <span data-ttu-id="e8e1d-113">För andra användare i din organisation toobrowse med hjälp av en reporting klient måste du dock skapa minst en roll med Läs behörigheter och lägga till de användarna som medlemmar.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-113">However, for other users in your organization toobrowse by using a reporting client, you must create at least one role with Read permissions and add those users as members.</span></span>  
  
<span data-ttu-id="e8e1d-114">Du kan skapa tre roller:</span><span class="sxs-lookup"><span data-stu-id="e8e1d-114">You create three roles:</span></span>  
  
-   <span data-ttu-id="e8e1d-115">**Försäljningschef** – den här rollen kan inkludera användare i organisationen som du vill toohave Läs behörighet tooall modellobjekt och data.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-115">**Sales Manager** – This role can include users in your organization for which you want toohave Read permission tooall model objects and data.</span></span>  
  
-   <span data-ttu-id="e8e1d-116">**Försäljning analytiker USA** – den här rollen kan inkludera användare i organisationen som du vill endast toobe kan toobrowse data relaterade toosales i hello USA.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-116">**Sales Analyst US** – This role can include users in your organization for which you want only toobe able toobrowse data related toosales in hello United States.</span></span> <span data-ttu-id="e8e1d-117">För den här rollen kan du använda en DAX-formeln toodefine en *radfilter*, vilket begränsar medlemmar toobrowse endast data för hello USA.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-117">For this role, you use a DAX formula toodefine a *Row Filter*, which restricts members toobrowse data only for hello United States.</span></span>  
  
-   <span data-ttu-id="e8e1d-118">**Administratören** – den här rollen kan innehålla användare som du vill toohave administratörsbehörigheter, vilket gör obegränsad åtkomst och behörighet tooperform administrativa uppgifter på hello modelldatabasen.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-118">**Administrator** – This role can include users for which you want toohave Administrator permission, which allows unlimited access and permissions tooperform administrative tasks on hello model database.</span></span>  
  
<span data-ttu-id="e8e1d-119">Du kan lägga till konton från din organisation särskilda toomembers eftersom Windows-användarkonton och gruppkonton i din organisation är unika.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-119">Because Windows user and group accounts in your organization are unique, you can add accounts from your particular organization toomembers.</span></span> <span data-ttu-id="e8e1d-120">Men för den här självstudiekursen kommer kan du även lämna hello medlemmar tomt.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-120">However, for this tutorial, you can also leave hello members blank.</span></span> <span data-ttu-id="e8e1d-121">Du testar hello effekten av varje roll senare i lektionen 12: Analysera i Excel.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-121">You test hello effect of each role later in Lesson 12: Analyze in Excel.</span></span>  
  
<span data-ttu-id="e8e1d-122">Uppskattad tid toocomplete lektionen: **15 minuter**</span><span class="sxs-lookup"><span data-stu-id="e8e1d-122">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="e8e1d-123">Krav</span><span class="sxs-lookup"><span data-stu-id="e8e1d-123">Prerequisites</span></span>  
<span data-ttu-id="e8e1d-124">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-124">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="e8e1d-125">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 10: skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="e8e1d-125">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span>  
  
## <a name="create-roles"></a><span data-ttu-id="e8e1d-126">Skapa roller</span><span class="sxs-lookup"><span data-stu-id="e8e1d-126">Create roles</span></span>  
  
#### <a name="toocreate-a-sales-manager-user-role"></a><span data-ttu-id="e8e1d-127">toocreate en försäljningschef användarroll</span><span class="sxs-lookup"><span data-stu-id="e8e1d-127">toocreate a Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="e8e1d-128">Högerklicka på **Roller** > **Roller** i tabellmodellutforskaren.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-128">In Tabular Model Explorer, right-click **Roles** > **Roles**.</span></span>  
  
2.  <span data-ttu-id="e8e1d-129">Klicka på **Ny** i rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-129">In Role Manager, click **New**.</span></span>  
  
3.  <span data-ttu-id="e8e1d-130">Klicka på ny hello-roll, och klicka sedan på hello **namn** kolumn, byta namn på hello rollen för**försäljningschef**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-130">Click hello new role, and then in hello **Name** column, rename hello role too**Sales Manager**.</span></span>  
  
4.  <span data-ttu-id="e8e1d-131">I hello **behörigheter** kolumnen, klickar du på hello listrutan och välj sedan hello **Läs** behörighet.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-131">In hello **Permissions** column, click hello dropdown list, and then select hello **Read** permission.</span></span> 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  <span data-ttu-id="e8e1d-133">Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-133">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="e8e1d-134">I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-134">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a><span data-ttu-id="e8e1d-135">toocreate en användarroll för försäljning analytiker USA</span><span class="sxs-lookup"><span data-stu-id="e8e1d-135">toocreate a Sales Analyst US user role</span></span>  
  
1.  <span data-ttu-id="e8e1d-136">Klicka på **Ny** i rollhanteraren.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-136">In Role Manager, click **New**.</span></span>    
  
2.  <span data-ttu-id="e8e1d-137">Byta namn på hello rollen för**försäljning analytiker USA**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-137">Rename hello role too**Sales Analyst US**.</span></span>  
  
3.  <span data-ttu-id="e8e1d-138">Ge rollen **läsbehörighet**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-138">Give this role **Read** permission.</span></span>  
  
4.  <span data-ttu-id="e8e1d-139">Fliken hello radfilter, och sedan efter Hej **DimGeography** i hello DAX-Filter kolumn, tabell typen hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="e8e1d-139">Click hello Row Filters tab, and then for hello **DimGeography** table only, in hello DAX Filter column, type hello following formula:</span></span>  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    <span data-ttu-id="e8e1d-140">Ett radfilter formeln måste matcha värdet för tooa boolesk (SANT/FALSKT).</span><span class="sxs-lookup"><span data-stu-id="e8e1d-140">A Row Filter formula must resolve tooa Boolean (TRUE/FALSE) value.</span></span> <span data-ttu-id="e8e1d-141">Med den här formeln anger du att endast rader med hello landskoden värdet ”USA” är synlig toohello användare.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-141">With this formula, you are specifying that only rows with hello Country Region Code value of “US” are visible toohello user.</span></span>  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  <span data-ttu-id="e8e1d-143">Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-143">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="e8e1d-144">I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-144">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-an-administrator-user-role"></a><span data-ttu-id="e8e1d-145">toocreate administratörsanvändarrollen</span><span class="sxs-lookup"><span data-stu-id="e8e1d-145">toocreate an Administrator user role</span></span>  
  
1.  <span data-ttu-id="e8e1d-146">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-146">Click **New**.</span></span>  
  
2.  <span data-ttu-id="e8e1d-147">Byta namn på hello rollen för**administratör**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-147">Rename hello role too**Administrator**.</span></span>  
  
3.  <span data-ttu-id="e8e1d-148">Ge den här rollen **administratörsbehörighet**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-148">Give this role **Administrator** permission.</span></span>  
  
4.  <span data-ttu-id="e8e1d-149">Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-149">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="e8e1d-150">I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation.</span><span class="sxs-lookup"><span data-stu-id="e8e1d-150">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span> 
  
  
## <a name="whats-next"></a><span data-ttu-id="e8e1d-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8e1d-151">What's next?</span></span>
<span data-ttu-id="e8e1d-152">[Lektion 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="e8e1d-152">[Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>

  
  
