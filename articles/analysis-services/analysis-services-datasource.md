---
title: "aaaData källor som stöds i Azure Analysis Services | Microsoft Docs"
description: "Beskriver de datakällor som stöds för datamodeller i Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2902d7d3c3bcf086419822fa826193bd247bde61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a><span data-ttu-id="9461e-103">Datakällor som stöds i Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="9461e-103">Data sources supported in Azure Analysis Services</span></span>
<span data-ttu-id="9461e-104">Azure Analysis Services-servrar som stöder anslutning toodata källor i hello molnet och lokalt i din organisation.</span><span class="sxs-lookup"><span data-stu-id="9461e-104">Azure Analysis Services servers support connecting toodata sources in hello cloud and on-premises in your organization.</span></span> <span data-ttu-id="9461e-105">Ytterligare datakällor läggs alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="9461e-105">Additional supported data sources are being added all hello time.</span></span> <span data-ttu-id="9461e-106">Kom tillbaka ofta.</span><span class="sxs-lookup"><span data-stu-id="9461e-106">Check back often.</span></span> 

<span data-ttu-id="9461e-107">hello följande datakällor stöds:</span><span class="sxs-lookup"><span data-stu-id="9461e-107">hello following data sources are currently supported:</span></span>

| <span data-ttu-id="9461e-108">Molnet</span><span class="sxs-lookup"><span data-stu-id="9461e-108">Cloud</span></span>  |
|---|
| <span data-ttu-id="9461e-109">Azure Blob Storage *</span><span class="sxs-lookup"><span data-stu-id="9461e-109">Azure Blob Storage*</span></span>  |
| <span data-ttu-id="9461e-110">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9461e-110">Azure SQL Database</span></span>  |
| <span data-ttu-id="9461e-111">Azure Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9461e-111">Azure Data Warehouse</span></span> |


| <span data-ttu-id="9461e-112">Lokal</span><span class="sxs-lookup"><span data-stu-id="9461e-112">On-premises</span></span>  |   |   |   |
|---|---|---|---|
| <span data-ttu-id="9461e-113">Access-databas</span><span class="sxs-lookup"><span data-stu-id="9461e-113">Access Database</span></span>  | <span data-ttu-id="9461e-114">Mappen *</span><span class="sxs-lookup"><span data-stu-id="9461e-114">Folder*</span></span> | <span data-ttu-id="9461e-115">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="9461e-115">Oracle Database</span></span>  | <span data-ttu-id="9461e-116">Teradata-databas</span><span class="sxs-lookup"><span data-stu-id="9461e-116">Teradata Database</span></span> |
| <span data-ttu-id="9461e-117">Active Directory *</span><span class="sxs-lookup"><span data-stu-id="9461e-117">Active Directory*</span></span>  | <span data-ttu-id="9461e-118">JSON-dokumentet *</span><span class="sxs-lookup"><span data-stu-id="9461e-118">JSON document*</span></span>  | <span data-ttu-id="9461e-119">Postgre SQL-databasen *</span><span class="sxs-lookup"><span data-stu-id="9461e-119">Postgre SQL Database*</span></span>  |<span data-ttu-id="9461e-120">XML-tabellen *</span><span class="sxs-lookup"><span data-stu-id="9461e-120">XML table*</span></span> |
| <span data-ttu-id="9461e-121">Analysis Services</span><span class="sxs-lookup"><span data-stu-id="9461e-121">Analysis Services</span></span>  | <span data-ttu-id="9461e-122">Rader från binary *</span><span class="sxs-lookup"><span data-stu-id="9461e-122">Lines from binary*</span></span>  | <span data-ttu-id="9461e-123">SAP HANA *</span><span class="sxs-lookup"><span data-stu-id="9461e-123">SAP HANA*</span></span>  |
| <span data-ttu-id="9461e-124">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="9461e-124">Analytics Platform System</span></span>  | <span data-ttu-id="9461e-125">MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="9461e-125">MySQL Database</span></span>  | <span data-ttu-id="9461e-126">SAP Business Warehouse *</span><span class="sxs-lookup"><span data-stu-id="9461e-126">SAP Business Warehouse*</span></span>  | |
| <span data-ttu-id="9461e-127">Dynamics CRM *</span><span class="sxs-lookup"><span data-stu-id="9461e-127">Dynamics CRM*</span></span>  | <span data-ttu-id="9461e-128">OData-Feed *</span><span class="sxs-lookup"><span data-stu-id="9461e-128">OData Feed*</span></span>  | <span data-ttu-id="9461e-129">SharePoint *</span><span class="sxs-lookup"><span data-stu-id="9461e-129">SharePoint*</span></span>  |
| <span data-ttu-id="9461e-130">Excel-arbetsbok</span><span class="sxs-lookup"><span data-stu-id="9461e-130">Excel workbook</span></span>  | <span data-ttu-id="9461e-131">ODBC-fråga</span><span class="sxs-lookup"><span data-stu-id="9461e-131">ODBC query</span></span>  | <span data-ttu-id="9461e-132">SQL Database</span><span class="sxs-lookup"><span data-stu-id="9461e-132">SQL Database</span></span>  |
| <span data-ttu-id="9461e-133">Exchange *</span><span class="sxs-lookup"><span data-stu-id="9461e-133">Exchange*</span></span>  | <span data-ttu-id="9461e-134">OLE DB</span><span class="sxs-lookup"><span data-stu-id="9461e-134">OLE DB</span></span>  | <span data-ttu-id="9461e-135">Sybase-databas</span><span class="sxs-lookup"><span data-stu-id="9461e-135">Sybase Database</span></span>  |

<span data-ttu-id="9461e-136">\*1400 tabellmodeller endast.</span><span class="sxs-lookup"><span data-stu-id="9461e-136">\* Tabular 1400 models only.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9461e-137">Ansluta tooon lokala data källor kräver en [lokala datagateway](analysis-services-gateway.md) installeras på en dator i din miljö.</span><span class="sxs-lookup"><span data-stu-id="9461e-137">Connecting tooon-premises data sources require an [On-premises data gateway](analysis-services-gateway.md) installed on a computer in your environment.</span></span>

## <a name="data-providers"></a><span data-ttu-id="9461e-138">Dataleverantörer</span><span class="sxs-lookup"><span data-stu-id="9461e-138">Data providers</span></span>

<span data-ttu-id="9461e-139">Datamodeller i Azure Analysis Services kan kräva olika dataleverantörer när du ansluter toocertain datakällor.</span><span class="sxs-lookup"><span data-stu-id="9461e-139">Data models in Azure Analysis Services may require different data providers when connecting toocertain data sources.</span></span> <span data-ttu-id="9461e-140">I vissa fall kan tabellmodeller ansluta toodata källor med hjälp av inbyggda providrar, till exempel SQL Server Native Client (SQLNCLI11) returnerar ett fel.</span><span class="sxs-lookup"><span data-stu-id="9461e-140">In some cases, tabular models connecting toodata sources using native providers such as SQL Server Native Client (SQLNCLI11) may return an error.</span></span>

<span data-ttu-id="9461e-141">För datamodeller som ansluter tooa molndata källa, till exempel Azure SQL Database, om du använder inbyggda providers än SQLOLEDB kanske du ser felmeddelandet: **”hello-providern 'SQLNCLI11.1' inte är registrerad”.**</span><span class="sxs-lookup"><span data-stu-id="9461e-141">For data models that connect tooa cloud data source such as Azure SQL Database, if you use native providers other than SQLOLEDB, you may see error message: **“hello provider 'SQLNCLI11.1' is not registered.”**</span></span> <span data-ttu-id="9461e-142">Om du har en DirectQuery modell anslutande tooon lokala datakällor, om du använder inbyggda providrar kan du läsa felmeddelande: **”Error OLE DB raden skapades. Felaktig syntax nära 'LIMIT' ”**.</span><span class="sxs-lookup"><span data-stu-id="9461e-142">Or, if you have a DirectQuery model connecting tooon-premises data sources, if you use native providers you may see error message: **“Error creating OLE DB row set. Incorrect syntax near 'LIMIT'”**.</span></span>

<span data-ttu-id="9461e-143">hello följande datasource providers stöds för i minnet eller DirectQuery datamodeller när anslutande toodata datakällor i hello molnet eller lokalt:</span><span class="sxs-lookup"><span data-stu-id="9461e-143">hello following datasource providers are supported for in-memory or DirectQuery data models when connecting toodata sources in hello cloud or on-premises:</span></span>

### <a name="cloud"></a><span data-ttu-id="9461e-144">Molnet</span><span class="sxs-lookup"><span data-stu-id="9461e-144">Cloud</span></span>
| <span data-ttu-id="9461e-145">**DataSource**</span><span class="sxs-lookup"><span data-stu-id="9461e-145">**Datasource**</span></span> | <span data-ttu-id="9461e-146">**Minnesintern**</span><span class="sxs-lookup"><span data-stu-id="9461e-146">**In-memory**</span></span> | <span data-ttu-id="9461e-147">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="9461e-147">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="9461e-148">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9461e-148">Azure SQL Data Warehouse</span></span> |<span data-ttu-id="9461e-149">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-149">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="9461e-150">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-150">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="9461e-151">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9461e-151">Azure SQL Database</span></span> |<span data-ttu-id="9461e-152">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-152">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="9461e-153">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-153">.NET Framework Data Provider for SQL Server</span></span> | |

### <a name="on-premises-via-gateway"></a><span data-ttu-id="9461e-154">Lokalt (via Gateway)</span><span class="sxs-lookup"><span data-stu-id="9461e-154">On-premises (via Gateway)</span></span>
|<span data-ttu-id="9461e-155">**DataSource**</span><span class="sxs-lookup"><span data-stu-id="9461e-155">**Datasource**</span></span> | <span data-ttu-id="9461e-156">**Minnesintern**</span><span class="sxs-lookup"><span data-stu-id="9461e-156">**In-memory**</span></span> | <span data-ttu-id="9461e-157">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="9461e-157">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="9461e-158">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9461e-158">SQL Server</span></span> |<span data-ttu-id="9461e-159">SQL Server Native Client 11.0</span><span class="sxs-lookup"><span data-stu-id="9461e-159">SQL Server Native Client 11.0</span></span> |<span data-ttu-id="9461e-160">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-160">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="9461e-161">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9461e-161">SQL Server</span></span> |<span data-ttu-id="9461e-162">Microsoft OLE DB Provider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-162">Microsoft OLE DB Provider for SQL Server</span></span> |<span data-ttu-id="9461e-163">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-163">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="9461e-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9461e-164">SQL Server</span></span> |<span data-ttu-id="9461e-165">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-165">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="9461e-166">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-166">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="9461e-167">Oracle</span><span class="sxs-lookup"><span data-stu-id="9461e-167">Oracle</span></span> |<span data-ttu-id="9461e-168">Microsoft OLE DB Provider för Oracle</span><span class="sxs-lookup"><span data-stu-id="9461e-168">Microsoft OLE DB Provider for Oracle</span></span> |<span data-ttu-id="9461e-169">Oracle dataprovider för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-169">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="9461e-170">Oracle</span><span class="sxs-lookup"><span data-stu-id="9461e-170">Oracle</span></span> |<span data-ttu-id="9461e-171">Oracle dataprovider för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-171">Oracle Data Provider for .NET</span></span> |<span data-ttu-id="9461e-172">Oracle dataprovider för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-172">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="9461e-173">Teradata</span><span class="sxs-lookup"><span data-stu-id="9461e-173">Teradata</span></span> |<span data-ttu-id="9461e-174">OLE DB Provider för Teradata</span><span class="sxs-lookup"><span data-stu-id="9461e-174">OLE DB Provider for Teradata</span></span> |<span data-ttu-id="9461e-175">Teradata-dataprovidern för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-175">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="9461e-176">Teradata</span><span class="sxs-lookup"><span data-stu-id="9461e-176">Teradata</span></span> |<span data-ttu-id="9461e-177">Teradata-dataprovidern för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-177">Teradata Data Provider for .NET</span></span> |<span data-ttu-id="9461e-178">Teradata-dataprovidern för .NET</span><span class="sxs-lookup"><span data-stu-id="9461e-178">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="9461e-179">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="9461e-179">Analytics Platform System</span></span> |<span data-ttu-id="9461e-180">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-180">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="9461e-181">.NET framework dataprovider för SQLServer</span><span class="sxs-lookup"><span data-stu-id="9461e-181">.NET Framework Data Provider for SQL Server</span></span> | |

> [!NOTE]
> <span data-ttu-id="9461e-182">Kontrollera 64-bitars providrar är installerade när du använder lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="9461e-182">Ensure 64-bit providers are installed when using On-premises gateway.</span></span>
> 
> 

<span data-ttu-id="9461e-183">När du migrerar en lokal SQL Server Analysis Services tabellmodell tooAzure Analysis Services, kan det vara nödvändigt toochange hello-providern.</span><span class="sxs-lookup"><span data-stu-id="9461e-183">When migrating an on-premises SQL Server Analysis Services tabular model tooAzure Analysis Services, it may be necessary toochange hello provider.</span></span>

<span data-ttu-id="9461e-184">**toospecify en datasource-provider**</span><span class="sxs-lookup"><span data-stu-id="9461e-184">**toospecify a datasource provider**</span></span>

1. <span data-ttu-id="9461e-185">I SSDT > **Tabular modellen Explorer** > **datakällor**, högerklicka på anslutning till en datakälla och klicka sedan på **redigera datakällan**.</span><span class="sxs-lookup"><span data-stu-id="9461e-185">In SSDT > **Tabular Model Explorer** > **Data Sources**, right-click a data source connection, and then click **Edit Data Source**.</span></span>
2. <span data-ttu-id="9461e-186">I **Redigera anslutning**, klickar du på **Avancerat** tooopen hello avancerade egenskapsfönster.</span><span class="sxs-lookup"><span data-stu-id="9461e-186">In **Edit Connection**, click **Advanced** tooopen hello Advance properties window.</span></span>
3. <span data-ttu-id="9461e-187">I **avancerade egenskaper för** > **Providers**, och sedan väljer hello lämplig provider.</span><span class="sxs-lookup"><span data-stu-id="9461e-187">In **Set Advanced Properties** > **Providers**, then select hello appropriate provider.</span></span>

## <a name="impersonation"></a><span data-ttu-id="9461e-188">Personifiering</span><span class="sxs-lookup"><span data-stu-id="9461e-188">Impersonation</span></span>
<span data-ttu-id="9461e-189">I vissa fall kanske den nödvändiga toospecify en annan personifieringskontot.</span><span class="sxs-lookup"><span data-stu-id="9461e-189">In some cases, it may be necessary toospecify a different impersonation account.</span></span> <span data-ttu-id="9461e-190">Personifieringskontot kan anges i SSDT eller SSMS.</span><span class="sxs-lookup"><span data-stu-id="9461e-190">Impersonation account can be specified in SSDT or SSMS.</span></span>

<span data-ttu-id="9461e-191">För lokala datakällor:</span><span class="sxs-lookup"><span data-stu-id="9461e-191">For on-premises data sources:</span></span>

* <span data-ttu-id="9461e-192">Om du använder SQL-autentisering vara personifiering tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="9461e-192">If using SQL authentication, impersonation should be Service Account.</span></span>
* <span data-ttu-id="9461e-193">Ange Windows användarlösenord om du använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9461e-193">If using Windows authentication, set Windows user/password.</span></span> <span data-ttu-id="9461e-194">Windows-autentisering med en specifik personifieringskontot stöds endast för datamodeller i minnet för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9461e-194">For SQL Server, Windows authentication with a specific impersonation account is supported only for in-memory data models.</span></span>

<span data-ttu-id="9461e-195">För molnet datakällor:</span><span class="sxs-lookup"><span data-stu-id="9461e-195">For cloud data sources:</span></span>

* <span data-ttu-id="9461e-196">Om du använder SQL-autentisering vara personifiering tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="9461e-196">If using SQL authentication, impersonation should be Service Account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9461e-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9461e-197">Next steps</span></span>
<span data-ttu-id="9461e-198">Om du har lokala datakällor kan vara säker på att tooinstall hello [lokala gateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="9461e-198">If you have on-premises data sources, be sure tooinstall hello [On-premises gateway](analysis-services-gateway.md).</span></span>   
<span data-ttu-id="9461e-199">toolearn mer information om hur du hanterar servern i SSDT eller SSMS, se [hantera servern](analysis-services-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9461e-199">toolearn more about managing your server in SSDT or SSMS, see [Manage your server](analysis-services-manage.md).</span></span>

