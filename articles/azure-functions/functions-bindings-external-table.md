---
title: "aaaAzure funktioner extern tabell bindning (förhandsversion) | Microsoft Docs"
description: "Använda extern tabell bindningar i Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: bf19d7d377232edc91087d5f4110602bb82c67ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-table-binding-preview"></a><span data-ttu-id="0146b-103">Azure Functions extern tabell bindning (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0146b-103">Azure Functions External Table binding (Preview)</span></span>
<span data-ttu-id="0146b-104">Den här artikeln visar hur toomanipulate tabelldata på SaaS-providers (t.ex. Sharepoint, Dynamics) i din funktion med inbyggda bindningar.</span><span class="sxs-lookup"><span data-stu-id="0146b-104">This article shows how toomanipulate tabular data on SaaS providers (e.g. Sharepoint, Dynamics) within your function with built-in bindings.</span></span> <span data-ttu-id="0146b-105">Azure Functions stöder inkommande och utgående bindningar för externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="0146b-105">Azure Functions supports input, and output bindings for external tables.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a><span data-ttu-id="0146b-106">API-anslutningar</span><span class="sxs-lookup"><span data-stu-id="0146b-106">API Connections</span></span>

<span data-ttu-id="0146b-107">Tabell bindningar utnyttja externa API anslutningar tooauthenticate med 3 part SaaS-providers.</span><span class="sxs-lookup"><span data-stu-id="0146b-107">Table bindings leverage external API connections tooauthenticate with 3rd party SaaS providers.</span></span> 

<span data-ttu-id="0146b-108">När du tilldelar en bindning du kan antingen skapa en ny API-anslutning eller använda en befintlig anslutning API inom hello samma resursgrupp</span><span class="sxs-lookup"><span data-stu-id="0146b-108">When assigning a binding you can either create a new API connection or use an existing API connection within hello same resource group</span></span>

### <a name="supported-api-connections-tables"></a><span data-ttu-id="0146b-109">Stöds API-anslutningar (tabellen) s</span><span class="sxs-lookup"><span data-stu-id="0146b-109">Supported API Connections (Table)s</span></span>

|<span data-ttu-id="0146b-110">koppling</span><span class="sxs-lookup"><span data-stu-id="0146b-110">Connector</span></span>|<span data-ttu-id="0146b-111">Utlösare</span><span class="sxs-lookup"><span data-stu-id="0146b-111">Trigger</span></span>|<span data-ttu-id="0146b-112">Indata</span><span class="sxs-lookup"><span data-stu-id="0146b-112">Input</span></span>|<span data-ttu-id="0146b-113">Resultat</span><span class="sxs-lookup"><span data-stu-id="0146b-113">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="0146b-114">DB2</span><span class="sxs-lookup"><span data-stu-id="0146b-114">DB2</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||<span data-ttu-id="0146b-115">x</span><span class="sxs-lookup"><span data-stu-id="0146b-115">x</span></span>|<span data-ttu-id="0146b-116">x</span><span class="sxs-lookup"><span data-stu-id="0146b-116">x</span></span>
|[<span data-ttu-id="0146b-117">Dynamics 365 för åtgärder</span><span class="sxs-lookup"><span data-stu-id="0146b-117">Dynamics 365 for Operations</span></span>](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||<span data-ttu-id="0146b-118">x</span><span class="sxs-lookup"><span data-stu-id="0146b-118">x</span></span>|<span data-ttu-id="0146b-119">x</span><span class="sxs-lookup"><span data-stu-id="0146b-119">x</span></span>
|[<span data-ttu-id="0146b-120">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="0146b-120">Dynamics 365</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="0146b-121">x</span><span class="sxs-lookup"><span data-stu-id="0146b-121">x</span></span>|<span data-ttu-id="0146b-122">x</span><span class="sxs-lookup"><span data-stu-id="0146b-122">x</span></span>
|[<span data-ttu-id="0146b-123">Dynamics NAV</span><span class="sxs-lookup"><span data-stu-id="0146b-123">Dynamics NAV</span></span>](https://msdn.microsoft.com/library/gg481835.aspx)||<span data-ttu-id="0146b-124">x</span><span class="sxs-lookup"><span data-stu-id="0146b-124">x</span></span>|<span data-ttu-id="0146b-125">x</span><span class="sxs-lookup"><span data-stu-id="0146b-125">x</span></span>
|[<span data-ttu-id="0146b-126">Google-blad</span><span class="sxs-lookup"><span data-stu-id="0146b-126">Google Sheets</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||<span data-ttu-id="0146b-127">x</span><span class="sxs-lookup"><span data-stu-id="0146b-127">x</span></span>|<span data-ttu-id="0146b-128">x</span><span class="sxs-lookup"><span data-stu-id="0146b-128">x</span></span>
|[<span data-ttu-id="0146b-129">Informix</span><span class="sxs-lookup"><span data-stu-id="0146b-129">Informix</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||<span data-ttu-id="0146b-130">x</span><span class="sxs-lookup"><span data-stu-id="0146b-130">x</span></span>|<span data-ttu-id="0146b-131">x</span><span class="sxs-lookup"><span data-stu-id="0146b-131">x</span></span>
|[<span data-ttu-id="0146b-132">Dynamics 365 för ekonomi</span><span class="sxs-lookup"><span data-stu-id="0146b-132">Dynamics 365 for Financials</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="0146b-133">x</span><span class="sxs-lookup"><span data-stu-id="0146b-133">x</span></span>|<span data-ttu-id="0146b-134">x</span><span class="sxs-lookup"><span data-stu-id="0146b-134">x</span></span>
|[<span data-ttu-id="0146b-135">MySQL</span><span class="sxs-lookup"><span data-stu-id="0146b-135">MySQL</span></span>](https://docs.microsoft.com/azure/store-php-create-mysql-database)||<span data-ttu-id="0146b-136">x</span><span class="sxs-lookup"><span data-stu-id="0146b-136">x</span></span>|<span data-ttu-id="0146b-137">x</span><span class="sxs-lookup"><span data-stu-id="0146b-137">x</span></span>
|[<span data-ttu-id="0146b-138">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="0146b-138">Oracle Database</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||<span data-ttu-id="0146b-139">x</span><span class="sxs-lookup"><span data-stu-id="0146b-139">x</span></span>|<span data-ttu-id="0146b-140">x</span><span class="sxs-lookup"><span data-stu-id="0146b-140">x</span></span>
|[<span data-ttu-id="0146b-141">Vanliga datatjänst</span><span class="sxs-lookup"><span data-stu-id="0146b-141">Common Data Service</span></span>](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||<span data-ttu-id="0146b-142">x</span><span class="sxs-lookup"><span data-stu-id="0146b-142">x</span></span>|<span data-ttu-id="0146b-143">x</span><span class="sxs-lookup"><span data-stu-id="0146b-143">x</span></span>
|[<span data-ttu-id="0146b-144">Salesforce</span><span class="sxs-lookup"><span data-stu-id="0146b-144">Salesforce</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||<span data-ttu-id="0146b-145">x</span><span class="sxs-lookup"><span data-stu-id="0146b-145">x</span></span>|<span data-ttu-id="0146b-146">x</span><span class="sxs-lookup"><span data-stu-id="0146b-146">x</span></span>
|[<span data-ttu-id="0146b-147">SharePoint</span><span class="sxs-lookup"><span data-stu-id="0146b-147">SharePoint</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||<span data-ttu-id="0146b-148">x</span><span class="sxs-lookup"><span data-stu-id="0146b-148">x</span></span>|<span data-ttu-id="0146b-149">x</span><span class="sxs-lookup"><span data-stu-id="0146b-149">x</span></span>
|[<span data-ttu-id="0146b-150">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0146b-150">SQL Server</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||<span data-ttu-id="0146b-151">x</span><span class="sxs-lookup"><span data-stu-id="0146b-151">x</span></span>|<span data-ttu-id="0146b-152">x</span><span class="sxs-lookup"><span data-stu-id="0146b-152">x</span></span>
|[<span data-ttu-id="0146b-153">Teradata</span><span class="sxs-lookup"><span data-stu-id="0146b-153">Teradata</span></span>](http://www.teradata.com/products-and-services/azure/products/)||<span data-ttu-id="0146b-154">x</span><span class="sxs-lookup"><span data-stu-id="0146b-154">x</span></span>|<span data-ttu-id="0146b-155">x</span><span class="sxs-lookup"><span data-stu-id="0146b-155">x</span></span>
|<span data-ttu-id="0146b-156">UserVoice</span><span class="sxs-lookup"><span data-stu-id="0146b-156">UserVoice</span></span>||<span data-ttu-id="0146b-157">x</span><span class="sxs-lookup"><span data-stu-id="0146b-157">x</span></span>|<span data-ttu-id="0146b-158">x</span><span class="sxs-lookup"><span data-stu-id="0146b-158">x</span></span>
|<span data-ttu-id="0146b-159">Zendesk</span><span class="sxs-lookup"><span data-stu-id="0146b-159">Zendesk</span></span>||<span data-ttu-id="0146b-160">x</span><span class="sxs-lookup"><span data-stu-id="0146b-160">x</span></span>|<span data-ttu-id="0146b-161">x</span><span class="sxs-lookup"><span data-stu-id="0146b-161">x</span></span>


> [!NOTE]
> <span data-ttu-id="0146b-162">Den externa tabellen anslutningar kan också användas i [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="0146b-162">External Table connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

### <a name="creating-an-api-connection-step-by-step"></a><span data-ttu-id="0146b-163">Skapa en API-anslutning: steg-för-steg</span><span class="sxs-lookup"><span data-stu-id="0146b-163">Creating an API connection: step by step</span></span>

1. <span data-ttu-id="0146b-164">Skapa en funktion > egen funktion ![skapa en egen funktion](./media/functions-bindings-storage-table/create-custom-function.jpg)</span><span class="sxs-lookup"><span data-stu-id="0146b-164">Create a function > custom function ![Create a custom function](./media/functions-bindings-storage-table/create-custom-function.jpg)</span></span>
1. <span data-ttu-id="0146b-165">Scenariot `Experimental`  >  `ExternalTable-CSharp` mall > Skapa en ny `External Table connection` 
 ![Välj mall för inkommande](./media/functions-bindings-storage-table/create-template-table.jpg)</span><span class="sxs-lookup"><span data-stu-id="0146b-165">Scenario `Experimental` > `ExternalTable-CSharp` template > Create a new `External Table connection`
![Choose table input template](./media/functions-bindings-storage-table/create-template-table.jpg)</span></span>
1. <span data-ttu-id="0146b-166">Välj din SaaS-provider > Välj/Skapa en anslutning ![konfigurera SaaS-anslutning](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="0146b-166">Choose your SaaS provider > choose/create a connection ![Configure SaaS connection](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span></span>
1. <span data-ttu-id="0146b-167">Välj API-anslutning > Skapa hello funktion ![skapa tabellfunktion](./media/functions-bindings-storage-table/table-template-options.jpg)</span><span class="sxs-lookup"><span data-stu-id="0146b-167">Select your API connection > create hello function ![Create table function](./media/functions-bindings-storage-table/table-template-options.jpg)</span></span>
1. <span data-ttu-id="0146b-168">Välj`Integrate` > `External Table`</span><span class="sxs-lookup"><span data-stu-id="0146b-168">Select `Integrate` > `External Table`</span></span>
    1. <span data-ttu-id="0146b-169">Konfigurera hello anslutning toouse din måltabellen.</span><span class="sxs-lookup"><span data-stu-id="0146b-169">Configure hello connection toouse your target table.</span></span> <span data-ttu-id="0146b-170">De här inställningarna kommer mycket mellan SaaS-providers.</span><span class="sxs-lookup"><span data-stu-id="0146b-170">These settings will very between SaaS providers.</span></span> <span data-ttu-id="0146b-171">De är disposition nedan i [inställningar för datakälla](#datasourcesettings)
![konfigurera tabell](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="0146b-171">They are outline below in [data source settings](#datasourcesettings)
![Configure table](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span></span>

## <a name="usage"></a><span data-ttu-id="0146b-172">Användning</span><span class="sxs-lookup"><span data-stu-id="0146b-172">Usage</span></span>

<span data-ttu-id="0146b-173">Det här exemplet ansluter tooa tabell med namnet ”kontakta” med Id, efternamn och förnamn kolumner.</span><span class="sxs-lookup"><span data-stu-id="0146b-173">This example connects tooa table named "Contact" with Id, LastName, and FirstName columns.</span></span> <span data-ttu-id="0146b-174">hello kod visar hello kontakta entiteter i hello tabell och loggar hello-och efternamn.</span><span class="sxs-lookup"><span data-stu-id="0146b-174">hello code lists hello Contact entities in hello table and logs hello first and last names.</span></span>

### <a name="bindings"></a><span data-ttu-id="0146b-175">Bindningar</span><span class="sxs-lookup"><span data-stu-id="0146b-175">Bindings</span></span>
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
<span data-ttu-id="0146b-176">`entityId`måste vara tom för tabell-bindningar.</span><span class="sxs-lookup"><span data-stu-id="0146b-176">`entityId` must be empty for table bindings.</span></span>

<span data-ttu-id="0146b-177">`ConnectionAppSettingsKey`identifierar hello appinställningen som lagrar hello API-anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0146b-177">`ConnectionAppSettingsKey` identifies hello app setting that stores hello API connection string.</span></span> <span data-ttu-id="0146b-178">Hej appinställningen skapas automatiskt när du lägger till en API-anslutningen i hello integrera Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0146b-178">hello app setting is created automatically when you add an API connection in hello integrate UI.</span></span>

<span data-ttu-id="0146b-179">En tabell koppling innehåller datauppsättningar och varje datamängd innehåller tabeller.</span><span class="sxs-lookup"><span data-stu-id="0146b-179">A tabular connector provides data sets, and each data set contains tables.</span></span> <span data-ttu-id="0146b-180">hello heter hello standarddatauppsättningen ”standard”.</span><span class="sxs-lookup"><span data-stu-id="0146b-180">hello name of hello default data set is “default.”</span></span> <span data-ttu-id="0146b-181">hello titlar för en datauppsättning och en tabell i olika SaaS-providers visas nedan:</span><span class="sxs-lookup"><span data-stu-id="0146b-181">hello titles for a dataset and a table in various SaaS providers are listed below:</span></span>

|<span data-ttu-id="0146b-182">koppling</span><span class="sxs-lookup"><span data-stu-id="0146b-182">Connector</span></span>|<span data-ttu-id="0146b-183">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="0146b-183">Dataset</span></span>|<span data-ttu-id="0146b-184">Tabell</span><span class="sxs-lookup"><span data-stu-id="0146b-184">Table</span></span>|
|:-----|:---|:---| 
|<span data-ttu-id="0146b-185">**SharePoint**</span><span class="sxs-lookup"><span data-stu-id="0146b-185">**SharePoint**</span></span>|<span data-ttu-id="0146b-186">plats</span><span class="sxs-lookup"><span data-stu-id="0146b-186">Site</span></span>|<span data-ttu-id="0146b-187">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="0146b-187">SharePoint List</span></span>
|<span data-ttu-id="0146b-188">**SQL**</span><span class="sxs-lookup"><span data-stu-id="0146b-188">**SQL**</span></span>|<span data-ttu-id="0146b-189">Databas</span><span class="sxs-lookup"><span data-stu-id="0146b-189">Database</span></span>|<span data-ttu-id="0146b-190">Tabell</span><span class="sxs-lookup"><span data-stu-id="0146b-190">Table</span></span> 
|<span data-ttu-id="0146b-191">**Google-blad**</span><span class="sxs-lookup"><span data-stu-id="0146b-191">**Google Sheet**</span></span>|<span data-ttu-id="0146b-192">Kalkylblad</span><span class="sxs-lookup"><span data-stu-id="0146b-192">Spreadsheet</span></span>|<span data-ttu-id="0146b-193">Kalkylblad</span><span class="sxs-lookup"><span data-stu-id="0146b-193">Worksheet</span></span> 
|<span data-ttu-id="0146b-194">**Excel**</span><span class="sxs-lookup"><span data-stu-id="0146b-194">**Excel**</span></span>|<span data-ttu-id="0146b-195">Excel-fil</span><span class="sxs-lookup"><span data-stu-id="0146b-195">Excel file</span></span>|<span data-ttu-id="0146b-196">Blad</span><span class="sxs-lookup"><span data-stu-id="0146b-196">Sheet</span></span> 

<!--
See hello language-specific sample that copies hello input file toohello output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="0146b-197">Användning i C#</span><span class="sxs-lookup"><span data-stu-id="0146b-197">Usage in C#</span></span> #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound toohello incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in hello source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<span data-ttu-id="0146b-198"><!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
##Inställningar för datakälla</span><span class="sxs-lookup"><span data-stu-id="0146b-198"><!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
## Data Source Settings</span></span>

### <a name="sql-server"></a><span data-ttu-id="0146b-199">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0146b-199">SQL Server</span></span>

<span data-ttu-id="0146b-200">Hej skriptet toocreate och fylla hello kontakttabellen är nedan.</span><span class="sxs-lookup"><span data-stu-id="0146b-200">hello script toocreate and populate hello Contact table is below.</span></span> <span data-ttu-id="0146b-201">dataSetName är ”standard”.</span><span class="sxs-lookup"><span data-stu-id="0146b-201">dataSetName is “default.”</span></span>

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a><span data-ttu-id="0146b-202">Google-blad</span><span class="sxs-lookup"><span data-stu-id="0146b-202">Google Sheets</span></span>
<span data-ttu-id="0146b-203">Skapa ett kalkylblad i Google-dokument med ett kalkylblad med namnet `Contact`.</span><span class="sxs-lookup"><span data-stu-id="0146b-203">In Google Docs, create a spreadsheet with a worksheet named `Contact`.</span></span> <span data-ttu-id="0146b-204">hello-anslutningen kan inte använda hello kalkylblad visningsnamn.</span><span class="sxs-lookup"><span data-stu-id="0146b-204">hello connector cannot use hello spreadsheet display name.</span></span> <span data-ttu-id="0146b-205">hello interna namn (i fetstil) måste användas som dataSetName, till exempel toobe: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  lägga till hello kolumnnamn `Id`, `LastName`, `FirstName` toohello rad först och sedan fylla i data på efterföljande rader.</span><span class="sxs-lookup"><span data-stu-id="0146b-205">hello internal name (in bold) needs toobe used as dataSetName, for example: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Add hello column names `Id`, `LastName`, `FirstName` toohello first row, then populate data on subsequent rows.</span></span>

### <a name="salesforce"></a><span data-ttu-id="0146b-206">Salesforce</span><span class="sxs-lookup"><span data-stu-id="0146b-206">Salesforce</span></span>
<span data-ttu-id="0146b-207">dataSetName är ”standard”.</span><span class="sxs-lookup"><span data-stu-id="0146b-207">dataSetName is “default.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="0146b-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0146b-208">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
