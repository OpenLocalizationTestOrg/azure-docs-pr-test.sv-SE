---
title: "Hantera databasroller och användare i Azure Analysis Services | Microsoft Docs"
description: "Lär dig hur du hanterar databasroller och användare på en Analysis Services-server i Azure."
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
ms.openlocfilehash: d0bc7d7514f111b4bbde33bd60ae21264bd797fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-database-roles-and-users"></a><span data-ttu-id="50821-103">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="50821-103">Manage database roles and users</span></span>

<span data-ttu-id="50821-104">Alla användare måste höra till en roll på databasnivå modellen.</span><span class="sxs-lookup"><span data-stu-id="50821-104">At the model database level, all users must belong to a role.</span></span> <span data-ttu-id="50821-105">Rollerna för att definiera användare med särskilda behörigheter för modelldatabasen.</span><span class="sxs-lookup"><span data-stu-id="50821-105">Roles define users with particular permissions for the model database.</span></span> <span data-ttu-id="50821-106">Alla användare eller säkerhetsgrupp som lagts till i en roll måste ha ett konto i Azure AD-klient på samma prenumeration som servern.</span><span class="sxs-lookup"><span data-stu-id="50821-106">Any user or security group added to a role must have an account in an Azure AD tenant in the same subscription as the server.</span></span>

<span data-ttu-id="50821-107">Hur du definierar roller skiljer sig åt beroende på hur du använder verktyget men effekten är samma.</span><span class="sxs-lookup"><span data-stu-id="50821-107">How you define roles is different depending on the tool you use, but the effect is the same.</span></span>

<span data-ttu-id="50821-108">Rollbehörigheter inkluderar:</span><span class="sxs-lookup"><span data-stu-id="50821-108">Role permissions include:</span></span>
*  <span data-ttu-id="50821-109">**Administratören** -användare har fullständig behörighet till databasen.</span><span class="sxs-lookup"><span data-stu-id="50821-109">**Administrator** - Users have full permissions for the database.</span></span> <span data-ttu-id="50821-110">Databasroller med administratörsbehörighet skiljer sig från server-administratörer.</span><span class="sxs-lookup"><span data-stu-id="50821-110">Database roles with Administrator permissions are different from server administrators.</span></span>
*  <span data-ttu-id="50821-111">**Processen** -användare kan ansluta till och utföra åtgärder för processen på databasen och analysera modellen databasdata.</span><span class="sxs-lookup"><span data-stu-id="50821-111">**Process** - Users can connect to and perform process operations on the database, and analyze model database data.</span></span>
*  <span data-ttu-id="50821-112">**Läs** -användare kan använda ett klientprogram att ansluta till och analysera modellen databasdata.</span><span class="sxs-lookup"><span data-stu-id="50821-112">**Read** -  Users can use a client application to connect to and analyze model database data.</span></span>

<span data-ttu-id="50821-113">När du skapar en tabellmodell-projekt kan du skapa roller och Lägg till användare eller grupper rollerna genom att använda rollhanteraren i SSDT.</span><span class="sxs-lookup"><span data-stu-id="50821-113">When creating a tabular model project, you create roles and add users or groups to those roles by using Role Manager in SSDT.</span></span> <span data-ttu-id="50821-114">När distribuerats till en server kan du använda SSMS, [Analysis Services PowerShell-cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), eller [Tabular modellen Scripting språk](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) för att lägga till eller ta bort roller och användaren medlemmar.</span><span class="sxs-lookup"><span data-stu-id="50821-114">When deployed to a server, you use SSMS, [Analysis Services PowerShell cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), or [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) to add or remove roles and user members.</span></span>

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a><span data-ttu-id="50821-115">Att lägga till eller hantera roller och användare i SSDT</span><span class="sxs-lookup"><span data-stu-id="50821-115">To add or manage roles and users in SSDT</span></span>  
  
1.  <span data-ttu-id="50821-116">I SSDT > **Tabular modellen Explorer**, högerklicka på **roller**.</span><span class="sxs-lookup"><span data-stu-id="50821-116">In SSDT > **Tabular Model Explorer**, right-click **Roles**.</span></span>  
  
2.  <span data-ttu-id="50821-117">Klicka på **Ny** i **rollhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="50821-117">In **Role Manager**, click **New**.</span></span>  
  
3.  <span data-ttu-id="50821-118">Ange ett namn för rollen.</span><span class="sxs-lookup"><span data-stu-id="50821-118">Type a name for the role.</span></span>  
  
     <span data-ttu-id="50821-119">Som standard numreras inkrementellt namnet på standardrollen som för varje ny roll.</span><span class="sxs-lookup"><span data-stu-id="50821-119">By default, the name of the default role is incrementally numbered for each new role.</span></span> <span data-ttu-id="50821-120">Det rekommenderas att du skriver ett namn som tydligt identifierar medlemstypen, till exempel finansiella chefer eller personal specialister.</span><span class="sxs-lookup"><span data-stu-id="50821-120">It's recommended you type a name that clearly identifies the member type, for example, Finance Managers or Human Resources Specialists.</span></span>  
  
4.  <span data-ttu-id="50821-121">Välj något av följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="50821-121">Select one of the following permissions:</span></span>  
  
    |<span data-ttu-id="50821-122">Behörighet</span><span class="sxs-lookup"><span data-stu-id="50821-122">Permission</span></span>|<span data-ttu-id="50821-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50821-123">Description</span></span>|  
    |----------------|-----------------|  
    |<span data-ttu-id="50821-124">**Ingen**</span><span class="sxs-lookup"><span data-stu-id="50821-124">**None**</span></span>|<span data-ttu-id="50821-125">Medlemmar kan inte ändra modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="50821-125">Members cannot modify the model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="50821-126">**Läsa**</span><span class="sxs-lookup"><span data-stu-id="50821-126">**Read**</span></span>|<span data-ttu-id="50821-127">Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra modellschemat.</span><span class="sxs-lookup"><span data-stu-id="50821-127">Members can query data (based on row filters) but cannot modify the model schema.</span></span>|  
    |<span data-ttu-id="50821-128">**Läs- och processen**</span><span class="sxs-lookup"><span data-stu-id="50821-128">**Read and Process**</span></span>|<span data-ttu-id="50821-129">Medlemmar kan fråga data (baserat på radnivå filter) och kör processen och bearbeta alla åtgärder, men det går inte att ändra modellschemat.</span><span class="sxs-lookup"><span data-stu-id="50821-129">Members can query data (based on row-level filters) and run Process and Process All operations, but cannot modify the model schema.</span></span>|  
    |<span data-ttu-id="50821-130">**Processen**</span><span class="sxs-lookup"><span data-stu-id="50821-130">**Process**</span></span>|<span data-ttu-id="50821-131">Medlemmar kan köra processen och bearbeta alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="50821-131">Members can run Process and Process All operations.</span></span> <span data-ttu-id="50821-132">Det går inte att ändra modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="50821-132">Cannot modify the model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="50821-133">**Administratören**</span><span class="sxs-lookup"><span data-stu-id="50821-133">**Administrator**</span></span>|<span data-ttu-id="50821-134">Medlemmar kan ändra modellschemat och läsa alla data.</span><span class="sxs-lookup"><span data-stu-id="50821-134">Members can modify the model schema and query all data.</span></span>|   
  
5.  <span data-ttu-id="50821-135">Om rollen som du skapar har läs- eller Läs- och processen behörighet, du kan lägga till radfilter med hjälp av en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="50821-135">If the role you are creating has Read or Read and Process permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="50821-136">Klicka på den **radfilter** , och sedan markera en tabell och klicka sedan på den **DAX-Filter** fältet och skriv sedan en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="50821-136">Click the **Row Filters** tab, then select a table, then click the **DAX Filter** field, and then type a DAX formula.</span></span>
  
6.  <span data-ttu-id="50821-137">Klicka på **medlemmar** > **lägga till externa**.</span><span class="sxs-lookup"><span data-stu-id="50821-137">Click **Members** > **Add External**.</span></span>  
  
8.  <span data-ttu-id="50821-138">I **lägga till extern medlem**, ange användare eller grupper i din Azure AD-klient med e-postadress.</span><span class="sxs-lookup"><span data-stu-id="50821-138">In **Add External Member**, enter users or groups in your tenant Azure AD by email address.</span></span> <span data-ttu-id="50821-139">När du klickar på OK och stänga rollhanteraren visas roller och medlemmar i rollen i tabellformat modellen Explorer.</span><span class="sxs-lookup"><span data-stu-id="50821-139">After you click OK and close Role Manager, roles and role members appear in Tabular Model Explorer.</span></span> 
 
     ![Roller och användare i tabellform modellen Explorer](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. <span data-ttu-id="50821-141">Distribuera till Azure Analysis Services-servern.</span><span class="sxs-lookup"><span data-stu-id="50821-141">Deploy to your Azure Analysis Services server.</span></span>


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a><span data-ttu-id="50821-142">Att lägga till eller hantera roller och användare i SSMS</span><span class="sxs-lookup"><span data-stu-id="50821-142">To add or manage roles and users in SSMS</span></span>
<span data-ttu-id="50821-143">Lägg till roller och användare i en distribuerad modelldatabasen, måste du vara ansluten till servern som en serveradministratör eller redan i en databasroll med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="50821-143">To add roles and users to a deployed model database, you must be connected to the server as a Server administrator or already in a database role with administrator permissions.</span></span>

1. <span data-ttu-id="50821-144">Högerklicka i objektet Exporer **roller** > **ny roll**.</span><span class="sxs-lookup"><span data-stu-id="50821-144">In Object Exporer, right-click **Roles** > **New Role**.</span></span>

2. <span data-ttu-id="50821-145">I **skapa roll**, ange role-namn och beskrivning.</span><span class="sxs-lookup"><span data-stu-id="50821-145">In **Create Role**, enter a role name and description.</span></span>

3. <span data-ttu-id="50821-146">Markera en behörighet.</span><span class="sxs-lookup"><span data-stu-id="50821-146">Select a permission.</span></span>
   |<span data-ttu-id="50821-147">Behörighet</span><span class="sxs-lookup"><span data-stu-id="50821-147">Permission</span></span>|<span data-ttu-id="50821-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50821-148">Description</span></span>|  
   |----------------|-----------------|  
   |<span data-ttu-id="50821-149">**Fullständig behörighet (administratör)**</span><span class="sxs-lookup"><span data-stu-id="50821-149">**Full control (Administrator)**</span></span>|<span data-ttu-id="50821-150">Medlemmar kan ändra modellschemat, bearbeta och kan fråga efter alla data.</span><span class="sxs-lookup"><span data-stu-id="50821-150">Members can modify the model schema, process, and can query all data.</span></span>| 
   |<span data-ttu-id="50821-151">**Process-databas**</span><span class="sxs-lookup"><span data-stu-id="50821-151">**Process database**</span></span>|<span data-ttu-id="50821-152">Medlemmar kan köra processen och bearbeta alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="50821-152">Members can run Process and Process All operations.</span></span> <span data-ttu-id="50821-153">Det går inte att ändra modellschemat och det går inte att fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="50821-153">Cannot modify the model schema and cannot query data.</span></span>|  
   |<span data-ttu-id="50821-154">**Läsa**</span><span class="sxs-lookup"><span data-stu-id="50821-154">**Read**</span></span>|<span data-ttu-id="50821-155">Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra modellschemat.</span><span class="sxs-lookup"><span data-stu-id="50821-155">Members can query data (based on row filters) but cannot modify the model schema.</span></span>|  
  
4. <span data-ttu-id="50821-156">Klicka på **medlemskap**, ange en användare eller grupp i din Azure AD-klient av e-postadress.</span><span class="sxs-lookup"><span data-stu-id="50821-156">Click **Membership**, then enter a user or group in your tenant Azure AD by email address.</span></span>

     ![Lägga till användare](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. <span data-ttu-id="50821-158">Om du skapar rollen har behörighet att läsa, kan du lägga till radfilter med hjälp av en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="50821-158">If the role you are creating has Read permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="50821-159">Klicka på **radfilter**, markera en tabell och skriv sedan en DAX-formel i den **DAX-Filter** fältet.</span><span class="sxs-lookup"><span data-stu-id="50821-159">Click **Row Filters**, select a table, and then type a DAX formula in the **DAX Filter** field.</span></span> 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a><span data-ttu-id="50821-160">Lägg till roller och användare med ett TMSL skript</span><span class="sxs-lookup"><span data-stu-id="50821-160">To add roles and users by using a TMSL script</span></span>
<span data-ttu-id="50821-161">Du kan köra ett skript för TMSL i fönstret XMLA i SSMS eller med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50821-161">You can run a TMSL script in the XMLA window in SSMS or by using PowerShell.</span></span> <span data-ttu-id="50821-162">Använd den [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) kommando och [roller](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objekt.</span><span class="sxs-lookup"><span data-stu-id="50821-162">Use the [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) command and the [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.</span></span>

<span data-ttu-id="50821-163">**Exempelskript för TMSL**</span><span class="sxs-lookup"><span data-stu-id="50821-163">**Sample TMSL script**</span></span>

<span data-ttu-id="50821-164">I det här exemplet läggs en B2B externa användare och en grupp till rollen analytiker med läsbehörighet för databasen SalesBI.</span><span class="sxs-lookup"><span data-stu-id="50821-164">In this sample, a B2B external user and a group are added to the Analyst role with Read permissions for the SalesBI database.</span></span> <span data-ttu-id="50821-165">Både externa användare och grupper måste vara i samma Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="50821-165">Both the external user and group must be in same tenant Azure AD.</span></span>

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
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

## <a name="to-add-roles-and-users-by-using-powershell"></a><span data-ttu-id="50821-166">Lägg till roller och användare med PowerShell</span><span class="sxs-lookup"><span data-stu-id="50821-166">To add roles and users by using PowerShell</span></span>
<span data-ttu-id="50821-167">Den [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modulen innehåller cmdlet: ar uppgiftsspecifika databasen och den allmänna Invoke-ASCmd cmdlet som accepterar en tabellvy modellen Scripting språk (TMSL) fråga eller skript.</span><span class="sxs-lookup"><span data-stu-id="50821-167">The [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module provides task-specific database management cmdlets and the general-purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="50821-168">Följande cmdlets används för att hantera databasroller och användare.</span><span class="sxs-lookup"><span data-stu-id="50821-168">The following cmdlets are used for managing database roles and users.</span></span>
  
|<span data-ttu-id="50821-169">Cmdlet</span><span class="sxs-lookup"><span data-stu-id="50821-169">Cmdlet</span></span>|<span data-ttu-id="50821-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50821-170">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="50821-171">Lägg till RoleMember</span><span class="sxs-lookup"><span data-stu-id="50821-171">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="50821-172">Lägga till en medlem i en databasroll.</span><span class="sxs-lookup"><span data-stu-id="50821-172">Add a member to a database role.</span></span>| 
|[<span data-ttu-id="50821-173">Ta bort RoleMember</span><span class="sxs-lookup"><span data-stu-id="50821-173">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="50821-174">Ta bort en medlem från en databasroll.</span><span class="sxs-lookup"><span data-stu-id="50821-174">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="50821-175">Anropa ASCmd</span><span class="sxs-lookup"><span data-stu-id="50821-175">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="50821-176">Köra ett skript för TMSL.</span><span class="sxs-lookup"><span data-stu-id="50821-176">Execute a TMSL script.</span></span>|

## <a name="row-filters"></a><span data-ttu-id="50821-177">Radfilter</span><span class="sxs-lookup"><span data-stu-id="50821-177">Row filters</span></span>  
<span data-ttu-id="50821-178">Radfilter definiera vilka rader i en tabell kan efterfrågas av medlemmar i en viss roll.</span><span class="sxs-lookup"><span data-stu-id="50821-178">Row filters define which rows in a table can be queried by members of a particular role.</span></span> <span data-ttu-id="50821-179">Radfilter definieras för varje tabell i en modell med hjälp av DAX-formler.</span><span class="sxs-lookup"><span data-stu-id="50821-179">Row filters are defined for each table in a model by using DAX formulas.</span></span>  
  
<span data-ttu-id="50821-180">Radfilter kan definieras endast för roller med läs- och läs och processen behörigheter.</span><span class="sxs-lookup"><span data-stu-id="50821-180">Row filters can be defined only for roles with Read and Read and Process permissions.</span></span> <span data-ttu-id="50821-181">Som standard om ett radfilter inte har definierats för en viss tabell kan medlemmar fråga efter alla rader i tabellen om filtrering av mellan gäller från en annan tabell.</span><span class="sxs-lookup"><span data-stu-id="50821-181">By default, if a row filter is not defined for a particular table, members  can query all rows in the table unless cross-filtering applies from another table.</span></span>
  
 <span data-ttu-id="50821-182">Radfilter kräver en DAX-formel måste utvärderas till ett TRUE/FALSE-värde, att definiera de rader som kan efterfrågas av medlemmar i specifika rollen.</span><span class="sxs-lookup"><span data-stu-id="50821-182">Row filters require a DAX formula, which must evaluate to a TRUE/FALSE value, to define the rows that can be queried by members of that particular role.</span></span> <span data-ttu-id="50821-183">Det går inte att uppdatera rader som inte ingår i DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="50821-183">Rows not included in the DAX formula cannot be queried.</span></span> <span data-ttu-id="50821-184">Till exempel tabellen kunder med följande rad filtrerar uttryck, *= kunder [Land] = ”USA”*, medlemmar i rollen kan bara se kunder i USA.</span><span class="sxs-lookup"><span data-stu-id="50821-184">For example, the Customers table with the following row filters expression, *=Customers [Country] = “USA”*, members of the Sales role can only see customers in the USA.</span></span>  
  
<span data-ttu-id="50821-185">Radfilter gäller för de angivna raderna och relaterade rader.</span><span class="sxs-lookup"><span data-stu-id="50821-185">Row filters apply to the specified rows and related rows.</span></span> <span data-ttu-id="50821-186">När en tabell har flera relationer, tillämpa filter säkerhet för relationen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="50821-186">When a table has multiple relationships, filters apply security for the relationship that is active.</span></span> <span data-ttu-id="50821-187">Radfilter överlappar med andra raden filter som definierats för relaterade tabeller, till exempel:</span><span class="sxs-lookup"><span data-stu-id="50821-187">Row filters are intersected with other row filers defined for related tables, for example:</span></span>  
  
|<span data-ttu-id="50821-188">Tabell</span><span class="sxs-lookup"><span data-stu-id="50821-188">Table</span></span>|<span data-ttu-id="50821-189">DAX-uttryck</span><span class="sxs-lookup"><span data-stu-id="50821-189">DAX expression</span></span>|  
|-----------|--------------------|  
|<span data-ttu-id="50821-190">Region</span><span class="sxs-lookup"><span data-stu-id="50821-190">Region</span></span>|<span data-ttu-id="50821-191">= Region [Land] = ”USA”</span><span class="sxs-lookup"><span data-stu-id="50821-191">=Region[Country]=”USA”</span></span>|  
|<span data-ttu-id="50821-192">Produktkategori</span><span class="sxs-lookup"><span data-stu-id="50821-192">ProductCategory</span></span>|<span data-ttu-id="50821-193">= Produktkategori [Name] = ”cyklar”</span><span class="sxs-lookup"><span data-stu-id="50821-193">=ProductCategory[Name]=”Bicycles”</span></span>|  
|<span data-ttu-id="50821-194">Transaktioner</span><span class="sxs-lookup"><span data-stu-id="50821-194">Transactions</span></span>|<span data-ttu-id="50821-195">= Transaktioner [år] = 2016</span><span class="sxs-lookup"><span data-stu-id="50821-195">=Transactions[Year]=2016</span></span>|  
  
 <span data-ttu-id="50821-196">Net effekten är att medlemmar kan fråga datarader där kunden finns i USA, produktkategorin cyklar och året 2016.</span><span class="sxs-lookup"><span data-stu-id="50821-196">The net effect is members can query rows of data where the customer is in the USA, the product category is bicycles, and the year is 2016.</span></span> <span data-ttu-id="50821-197">Användare kan inte fråga transaktioner utanför USA, transaktioner som inte cyklar eller transaktioner inte 2016 såvida de inte är medlem i en annan roll som ger dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="50821-197">Users cannot query transactions outside of the USA, transactions that are not bicycles, or transactions not in 2016 unless they are a member of another role that grants these permissions.</span></span>
  
 <span data-ttu-id="50821-198">Du kan använda filter, *=FALSE()*, för att neka åtkomst till alla rader för en hel tabell.</span><span class="sxs-lookup"><span data-stu-id="50821-198">You can use the filter, *=FALSE()*, to deny access to all rows for an entire table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50821-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50821-199">Next steps</span></span>
  <span data-ttu-id="50821-200">[Hantera server-administratörer](analysis-services-server-admins.md) </span><span class="sxs-lookup"><span data-stu-id="50821-200">[Manage server administrators](analysis-services-server-admins.md) </span></span>  
  [<span data-ttu-id="50821-201">Hantera Azure Analysis Services med PowerShell</span><span class="sxs-lookup"><span data-stu-id="50821-201">Manage Azure Analysis Services with PowerShell</span></span>](analysis-services-powershell.md)  
  [<span data-ttu-id="50821-202">Tabellmodell Scripting Språkreferens (TMSL)</span><span class="sxs-lookup"><span data-stu-id="50821-202">Tabular Model Scripting Language (TMSL) Reference</span></span>](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

