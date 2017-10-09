---
title: "aaaManage roller och användare i Azure Analysis Services-databas | Microsoft Docs"
description: "Lär dig hur toomanage databasen roller och användare på en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2ad069a6bcce11bc43347625cb32ec400d48af18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-database-roles-and-users"></a><span data-ttu-id="5a282-103">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="5a282-103">Manage database roles and users</span></span>

<span data-ttu-id="5a282-104">Alla användare måste tillhöra tooa roll på hello modellen databasnivå.</span><span class="sxs-lookup"><span data-stu-id="5a282-104">At hello model database level, all users must belong tooa role.</span></span> <span data-ttu-id="5a282-105">Rollerna för att definiera användare med särskilda behörigheter för hello modelldatabasen.</span><span class="sxs-lookup"><span data-stu-id="5a282-105">Roles define users with particular permissions for hello model database.</span></span> <span data-ttu-id="5a282-106">Alla användare eller säkerhetsgrupp grupp läggs tooa roll måste ha ett konto i Azure AD-klient i hello samma prenumeration som hello-server.</span><span class="sxs-lookup"><span data-stu-id="5a282-106">Any user or security group added tooa role must have an account in an Azure AD tenant in hello same subscription as hello server.</span></span>

<span data-ttu-id="5a282-107">Hur du definierar roller är olika beroende på hello-verktyg som du använder, men hello effekt är hello samma.</span><span class="sxs-lookup"><span data-stu-id="5a282-107">How you define roles is different depending on hello tool you use, but hello effect is hello same.</span></span>

<span data-ttu-id="5a282-108">Rollbehörigheter inkluderar:</span><span class="sxs-lookup"><span data-stu-id="5a282-108">Role permissions include:</span></span>
*  <span data-ttu-id="5a282-109">**Administratören** -användare har fullständig behörighet för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="5a282-109">**Administrator** - Users have full permissions for hello database.</span></span> <span data-ttu-id="5a282-110">Databasroller med administratörsbehörighet skiljer sig från server-administratörer.</span><span class="sxs-lookup"><span data-stu-id="5a282-110">Database roles with Administrator permissions are different from server administrators.</span></span>
*  <span data-ttu-id="5a282-111">**Processen** -användare kan ansluta tooand processen utförs på hello databas och analysera modellen databasdata.</span><span class="sxs-lookup"><span data-stu-id="5a282-111">**Process** - Users can connect tooand perform process operations on hello database, and analyze model database data.</span></span>
*  <span data-ttu-id="5a282-112">**Läs** -användare kan använda ett program tooconnect tooand analysera modellen databasdata.</span><span class="sxs-lookup"><span data-stu-id="5a282-112">**Read** -  Users can use a client application tooconnect tooand analyze model database data.</span></span>

<span data-ttu-id="5a282-113">När du skapar en tabellmodell-projekt kan du skapa roller och lägga till användare eller grupper toothose roller med hjälp av rollhanteraren i SSDT.</span><span class="sxs-lookup"><span data-stu-id="5a282-113">When creating a tabular model project, you create roles and add users or groups toothose roles by using Role Manager in SSDT.</span></span> <span data-ttu-id="5a282-114">När distribuerade tooa server kan du använda SSMS, [Analysis Services PowerShell-cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), eller [Tabular modellen Scripting språk](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd eller ta bort roller och användaren medlemmar.</span><span class="sxs-lookup"><span data-stu-id="5a282-114">When deployed tooa server, you use SSMS, [Analysis Services PowerShell cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), or [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd or remove roles and user members.</span></span>

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a><span data-ttu-id="5a282-115">tooadd eller hantera roller och användare i SSDT</span><span class="sxs-lookup"><span data-stu-id="5a282-115">tooadd or manage roles and users in SSDT</span></span>  
  
1.  <span data-ttu-id="5a282-116">I SSDT > **Tabular modellen Explorer**, högerklicka på **roller**.</span><span class="sxs-lookup"><span data-stu-id="5a282-116">In SSDT > **Tabular Model Explorer**, right-click **Roles**.</span></span>  
  
2.  <span data-ttu-id="5a282-117">Klicka på **Ny** i **rollhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="5a282-117">In **Role Manager**, click **New**.</span></span>  
  
3.  <span data-ttu-id="5a282-118">Ange ett namn för hello roll.</span><span class="sxs-lookup"><span data-stu-id="5a282-118">Type a name for hello role.</span></span>  
  
     <span data-ttu-id="5a282-119">Som standard numreras inkrementellt hello namnet på hello standardroll för varje ny roll.</span><span class="sxs-lookup"><span data-stu-id="5a282-119">By default, hello name of hello default role is incrementally numbered for each new role.</span></span> <span data-ttu-id="5a282-120">Det rekommenderas att du skriver ett namn som tydligt identifierar hello Medlemstyp, till exempel finansiella chefer eller personal specialister.</span><span class="sxs-lookup"><span data-stu-id="5a282-120">It's recommended you type a name that clearly identifies hello member type, for example, Finance Managers or Human Resources Specialists.</span></span>  
  
4.  <span data-ttu-id="5a282-121">Välj något av hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="5a282-121">Select one of hello following permissions:</span></span>  
  
    |<span data-ttu-id="5a282-122">Behörighet</span><span class="sxs-lookup"><span data-stu-id="5a282-122">Permission</span></span>|<span data-ttu-id="5a282-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a282-123">Description</span></span>|  
    |----------------|-----------------|  
    |<span data-ttu-id="5a282-124">**Ingen**</span><span class="sxs-lookup"><span data-stu-id="5a282-124">**None**</span></span>|<span data-ttu-id="5a282-125">Medlemmar kan inte ändra hello modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="5a282-125">Members cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="5a282-126">**Läsa**</span><span class="sxs-lookup"><span data-stu-id="5a282-126">**Read**</span></span>|<span data-ttu-id="5a282-127">Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra hello modellschemat.</span><span class="sxs-lookup"><span data-stu-id="5a282-127">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="5a282-128">**Läs- och processen**</span><span class="sxs-lookup"><span data-stu-id="5a282-128">**Read and Process**</span></span>|<span data-ttu-id="5a282-129">Medlemmar kan fråga data (baserat på radnivå filter) och kör processen och bearbeta alla åtgärder, men det går inte att ändra hello modellschemat.</span><span class="sxs-lookup"><span data-stu-id="5a282-129">Members can query data (based on row-level filters) and run Process and Process All operations, but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="5a282-130">**Processen**</span><span class="sxs-lookup"><span data-stu-id="5a282-130">**Process**</span></span>|<span data-ttu-id="5a282-131">Medlemmar kan köra processen och bearbeta alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5a282-131">Members can run Process and Process All operations.</span></span> <span data-ttu-id="5a282-132">Det går inte att ändra hello modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="5a282-132">Cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="5a282-133">**Administratören**</span><span class="sxs-lookup"><span data-stu-id="5a282-133">**Administrator**</span></span>|<span data-ttu-id="5a282-134">Medlemmar kan ändra hello modellschemat och läsa alla data.</span><span class="sxs-lookup"><span data-stu-id="5a282-134">Members can modify hello model schema and query all data.</span></span>|   
  
5.  <span data-ttu-id="5a282-135">Om hello roll du skapar har läs- eller Läs- och processen behörighet, du kan lägga till radfilter med hjälp av en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="5a282-135">If hello role you are creating has Read or Read and Process permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="5a282-136">Klicka på hello **radfilter** , och sedan markera en tabell och klicka sedan på hello **DAX-Filter** fältet och skriv sedan en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="5a282-136">Click hello **Row Filters** tab, then select a table, then click hello **DAX Filter** field, and then type a DAX formula.</span></span>
  
6.  <span data-ttu-id="5a282-137">Klicka på **medlemmar** > **lägga till externa**.</span><span class="sxs-lookup"><span data-stu-id="5a282-137">Click **Members** > **Add External**.</span></span>  
  
8.  <span data-ttu-id="5a282-138">I **lägga till extern medlem**, ange användare eller grupper i din Azure AD-klient med e-postadress.</span><span class="sxs-lookup"><span data-stu-id="5a282-138">In **Add External Member**, enter users or groups in your tenant Azure AD by email address.</span></span> <span data-ttu-id="5a282-139">När du klickar på OK och stänga rollhanteraren visas roller och medlemmar i rollen i tabellformat modellen Explorer.</span><span class="sxs-lookup"><span data-stu-id="5a282-139">After you click OK and close Role Manager, roles and role members appear in Tabular Model Explorer.</span></span> 
 
     ![Roller och användare i tabellform modellen Explorer](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. <span data-ttu-id="5a282-141">Distribuera tooyour Azure Analysis Services-servern.</span><span class="sxs-lookup"><span data-stu-id="5a282-141">Deploy tooyour Azure Analysis Services server.</span></span>


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a><span data-ttu-id="5a282-142">tooadd eller hantera roller och användare i SSMS</span><span class="sxs-lookup"><span data-stu-id="5a282-142">tooadd or manage roles and users in SSMS</span></span>
<span data-ttu-id="5a282-143">tooadd roller och användare tooa distribueras modelldatabasen måste du vara ansluten toohello server som en serveradministratör eller redan i en databasroll med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="5a282-143">tooadd roles and users tooa deployed model database, you must be connected toohello server as a Server administrator or already in a database role with administrator permissions.</span></span>

1. <span data-ttu-id="5a282-144">Högerklicka i objektet Exporer **roller** > **ny roll**.</span><span class="sxs-lookup"><span data-stu-id="5a282-144">In Object Exporer, right-click **Roles** > **New Role**.</span></span>

2. <span data-ttu-id="5a282-145">I **skapa roll**, ange role-namn och beskrivning.</span><span class="sxs-lookup"><span data-stu-id="5a282-145">In **Create Role**, enter a role name and description.</span></span>

3. <span data-ttu-id="5a282-146">Markera en behörighet.</span><span class="sxs-lookup"><span data-stu-id="5a282-146">Select a permission.</span></span>
   |<span data-ttu-id="5a282-147">Behörighet</span><span class="sxs-lookup"><span data-stu-id="5a282-147">Permission</span></span>|<span data-ttu-id="5a282-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a282-148">Description</span></span>|  
   |----------------|-----------------|  
   |<span data-ttu-id="5a282-149">**Fullständig behörighet (administratör)**</span><span class="sxs-lookup"><span data-stu-id="5a282-149">**Full control (Administrator)**</span></span>|<span data-ttu-id="5a282-150">Medlemmar kan ändra hello modellschemat bearbeta och kan fråga efter alla data.</span><span class="sxs-lookup"><span data-stu-id="5a282-150">Members can modify hello model schema, process, and can query all data.</span></span>| 
   |<span data-ttu-id="5a282-151">**Process-databas**</span><span class="sxs-lookup"><span data-stu-id="5a282-151">**Process database**</span></span>|<span data-ttu-id="5a282-152">Medlemmar kan köra processen och bearbeta alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5a282-152">Members can run Process and Process All operations.</span></span> <span data-ttu-id="5a282-153">Det går inte att ändra hello modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="5a282-153">Cannot modify hello model schema and cannot query data.</span></span>|  
   |<span data-ttu-id="5a282-154">**Läsa**</span><span class="sxs-lookup"><span data-stu-id="5a282-154">**Read**</span></span>|<span data-ttu-id="5a282-155">Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra hello modellschemat.</span><span class="sxs-lookup"><span data-stu-id="5a282-155">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
  
4. <span data-ttu-id="5a282-156">Klicka på **medlemskap**, ange en användare eller grupp i din Azure AD-klient av e-postadress.</span><span class="sxs-lookup"><span data-stu-id="5a282-156">Click **Membership**, then enter a user or group in your tenant Azure AD by email address.</span></span>

     ![Lägga till användare](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. <span data-ttu-id="5a282-158">Hello-roll som du skapar har behörighet att läsa, kan du lägga till radfilter med hjälp av en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="5a282-158">If hello role you are creating has Read permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="5a282-159">Klicka på **radfilter**, markera en tabell och skriv sedan en DAX-formel i hello **DAX-Filter** fältet.</span><span class="sxs-lookup"><span data-stu-id="5a282-159">Click **Row Filters**, select a table, and then type a DAX formula in hello **DAX Filter** field.</span></span> 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a><span data-ttu-id="5a282-160">tooadd roller och användare med ett TMSL skript</span><span class="sxs-lookup"><span data-stu-id="5a282-160">tooadd roles and users by using a TMSL script</span></span>
<span data-ttu-id="5a282-161">Du kan köra ett skript för TMSL i hello XMLA-fönstret i SSMS eller med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a282-161">You can run a TMSL script in hello XMLA window in SSMS or by using PowerShell.</span></span> <span data-ttu-id="5a282-162">Använd hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) kommandot och hello [roller](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objekt.</span><span class="sxs-lookup"><span data-stu-id="5a282-162">Use hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) command and hello [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.</span></span>

<span data-ttu-id="5a282-163">**Exempelskript för TMSL**</span><span class="sxs-lookup"><span data-stu-id="5a282-163">**Sample TMSL script**</span></span>

<span data-ttu-id="5a282-164">I det här exemplet läggs en B2B externa användare och en grupp toohello analytiker rollen med läsbehörighet för hello SalesBI databasen.</span><span class="sxs-lookup"><span data-stu-id="5a282-164">In this sample, a B2B external user and a group are added toohello Analyst role with Read permissions for hello SalesBI database.</span></span> <span data-ttu-id="5a282-165">Både hello extern användare och grupp måste vara i samma Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="5a282-165">Both hello external user and group must be in same tenant Azure AD.</span></span>

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users tooquery hello model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="tooadd-roles-and-users-by-using-powershell"></a><span data-ttu-id="5a282-166">tooadd roller och användare med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5a282-166">tooadd roles and users by using PowerShell</span></span>
<span data-ttu-id="5a282-167">Hej [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modulen innehåller uppgiftsspecifika databasen management-cmdlets och hello allmänna Invoke-ASCmd cmdlet som accepterar en tabellvy modellen Scripting språk (TMSL) fråga eller skript.</span><span class="sxs-lookup"><span data-stu-id="5a282-167">hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module provides task-specific database management cmdlets and hello general-purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="5a282-168">hello följande cmdlets används för att hantera databasroller och användare.</span><span class="sxs-lookup"><span data-stu-id="5a282-168">hello following cmdlets are used for managing database roles and users.</span></span>
  
|<span data-ttu-id="5a282-169">Cmdlet</span><span class="sxs-lookup"><span data-stu-id="5a282-169">Cmdlet</span></span>|<span data-ttu-id="5a282-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a282-170">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="5a282-171">Lägg till RoleMember</span><span class="sxs-lookup"><span data-stu-id="5a282-171">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="5a282-172">Lägg till en medlem tooa databasroll.</span><span class="sxs-lookup"><span data-stu-id="5a282-172">Add a member tooa database role.</span></span>| 
|[<span data-ttu-id="5a282-173">Ta bort RoleMember</span><span class="sxs-lookup"><span data-stu-id="5a282-173">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="5a282-174">Ta bort en medlem från en databasroll.</span><span class="sxs-lookup"><span data-stu-id="5a282-174">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="5a282-175">Anropa ASCmd</span><span class="sxs-lookup"><span data-stu-id="5a282-175">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="5a282-176">Köra ett skript för TMSL.</span><span class="sxs-lookup"><span data-stu-id="5a282-176">Execute a TMSL script.</span></span>|

## <a name="row-filters"></a><span data-ttu-id="5a282-177">Radfilter</span><span class="sxs-lookup"><span data-stu-id="5a282-177">Row filters</span></span>  
<span data-ttu-id="5a282-178">Radfilter definiera vilka rader i en tabell kan efterfrågas av medlemmar i en viss roll.</span><span class="sxs-lookup"><span data-stu-id="5a282-178">Row filters define which rows in a table can be queried by members of a particular role.</span></span> <span data-ttu-id="5a282-179">Radfilter definieras för varje tabell i en modell med hjälp av DAX-formler.</span><span class="sxs-lookup"><span data-stu-id="5a282-179">Row filters are defined for each table in a model by using DAX formulas.</span></span>  
  
<span data-ttu-id="5a282-180">Radfilter kan definieras endast för roller med läs- och läs och processen behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5a282-180">Row filters can be defined only for roles with Read and Read and Process permissions.</span></span> <span data-ttu-id="5a282-181">Som standard om ett radfilter inte har definierats för en viss tabell kan medlemmar fråga efter alla rader i tabellen hello såvida inte mellan filtrering gäller från en annan tabell.</span><span class="sxs-lookup"><span data-stu-id="5a282-181">By default, if a row filter is not defined for a particular table, members  can query all rows in hello table unless cross-filtering applies from another table.</span></span>
  
 <span data-ttu-id="5a282-182">Radfilter kräver en DAX-formel, som måste utvärderas tooa TRUE/FALSE-värde, toodefine hello rader som kan efterfrågas av medlemmar i specifika rollen.</span><span class="sxs-lookup"><span data-stu-id="5a282-182">Row filters require a DAX formula, which must evaluate tooa TRUE/FALSE value, toodefine hello rows that can be queried by members of that particular role.</span></span> <span data-ttu-id="5a282-183">Det går inte att uppdatera rader som inte ingår i hello DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="5a282-183">Rows not included in hello DAX formula cannot be queried.</span></span> <span data-ttu-id="5a282-184">Till exempel hello tabellen kunder med hello följande rad filter uttryck *= kunder [Land] = ”USA”*, medlemmar i hello försäljningsrollen kan bara se kunder i hello USA.</span><span class="sxs-lookup"><span data-stu-id="5a282-184">For example, hello Customers table with hello following row filters expression, *=Customers [Country] = “USA”*, members of hello Sales role can only see customers in hello USA.</span></span>  
  
<span data-ttu-id="5a282-185">Radfilter gäller toohello angivna rader och relaterade rader.</span><span class="sxs-lookup"><span data-stu-id="5a282-185">Row filters apply toohello specified rows and related rows.</span></span> <span data-ttu-id="5a282-186">När en tabell har flera relationer, tillämpa filter säkerhet för hello relation som är aktiv.</span><span class="sxs-lookup"><span data-stu-id="5a282-186">When a table has multiple relationships, filters apply security for hello relationship that is active.</span></span> <span data-ttu-id="5a282-187">Radfilter överlappar med andra raden filter som definierats för relaterade tabeller, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5a282-187">Row filters are intersected with other row filers defined for related tables, for example:</span></span>  
  
|<span data-ttu-id="5a282-188">Tabell</span><span class="sxs-lookup"><span data-stu-id="5a282-188">Table</span></span>|<span data-ttu-id="5a282-189">DAX-uttryck</span><span class="sxs-lookup"><span data-stu-id="5a282-189">DAX expression</span></span>|  
|-----------|--------------------|  
|<span data-ttu-id="5a282-190">Region</span><span class="sxs-lookup"><span data-stu-id="5a282-190">Region</span></span>|<span data-ttu-id="5a282-191">= Region [Land] = ”USA”</span><span class="sxs-lookup"><span data-stu-id="5a282-191">=Region[Country]=”USA”</span></span>|  
|<span data-ttu-id="5a282-192">Produktkategori</span><span class="sxs-lookup"><span data-stu-id="5a282-192">ProductCategory</span></span>|<span data-ttu-id="5a282-193">= Produktkategori [Name] = ”cyklar”</span><span class="sxs-lookup"><span data-stu-id="5a282-193">=ProductCategory[Name]=”Bicycles”</span></span>|  
|<span data-ttu-id="5a282-194">Transaktioner</span><span class="sxs-lookup"><span data-stu-id="5a282-194">Transactions</span></span>|<span data-ttu-id="5a282-195">= Transaktioner [år] = 2016</span><span class="sxs-lookup"><span data-stu-id="5a282-195">=Transactions[Year]=2016</span></span>|  
  
 <span data-ttu-id="5a282-196">hello net effekten är att medlemmar kan fråga datarader där hello kunden är i USA hello, hello produktkategori cyklar och hello år 2016.</span><span class="sxs-lookup"><span data-stu-id="5a282-196">hello net effect is members can query rows of data where hello customer is in hello USA, hello product category is bicycles, and hello year is 2016.</span></span> <span data-ttu-id="5a282-197">Användare kan inte fråga transaktioner utanför USA hello, transaktioner som inte cyklar eller transaktioner inte 2016 såvida de inte är medlem i en annan roll som ger dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5a282-197">Users cannot query transactions outside of hello USA, transactions that are not bicycles, or transactions not in 2016 unless they are a member of another role that grants these permissions.</span></span>
  
 <span data-ttu-id="5a282-198">Du kan använda hello filtrera *=FALSE()*, toodeny åtkomst tooall rader för en hel tabell.</span><span class="sxs-lookup"><span data-stu-id="5a282-198">You can use hello filter, *=FALSE()*, toodeny access tooall rows for an entire table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a282-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a282-199">Next steps</span></span>
  <span data-ttu-id="5a282-200">[Hantera server-administratörer](analysis-services-server-admins.md) </span><span class="sxs-lookup"><span data-stu-id="5a282-200">[Manage server administrators](analysis-services-server-admins.md) </span></span>  
  [<span data-ttu-id="5a282-201">Hantera Azure Analysis Services med PowerShell</span><span class="sxs-lookup"><span data-stu-id="5a282-201">Manage Azure Analysis Services with PowerShell</span></span>](analysis-services-powershell.md)  
  [<span data-ttu-id="5a282-202">Tabellmodell Scripting Språkreferens (TMSL)</span><span class="sxs-lookup"><span data-stu-id="5a282-202">Tabular Model Scripting Language (TMSL) Reference</span></span>](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

