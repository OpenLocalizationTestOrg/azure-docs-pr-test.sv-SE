---
title: "aaaSupported datakällor i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln visas hello stöds för närvarande datakällor."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 4bfcabf31bf9fd781c939a5026abc42a5407df32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a><span data-ttu-id="3757b-103">Datakällor som stöds i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="3757b-103">Supported data sources in Azure Data Catalog</span></span>

<span data-ttu-id="3757b-104">Du kan publicera metadata med hjälp av en offentlig API eller ett klick-en gång registreringsverktyget eller genom att manuellt ange information direkt toohello Azure Data Catalog web portal.</span><span class="sxs-lookup"><span data-stu-id="3757b-104">You can publish metadata by using a public API or a click-once registration tool, or by manually entering information directly toohello Azure Data Catalog web portal.</span></span> <span data-ttu-id="3757b-105">hello sammanfattas följande tabell alla datakällor som stöds av hello catalog idag och hello publishing funktioner för varje.</span><span class="sxs-lookup"><span data-stu-id="3757b-105">hello following table summarizes all data sources that are supported by hello catalog today, and hello publishing capabilities for each.</span></span> <span data-ttu-id="3757b-106">Listan finns även hello externa verktyg som varje datakälla kan starta från vår portal ”öppna i” erfarenhet.</span><span class="sxs-lookup"><span data-stu-id="3757b-106">Also listed are hello external data tools that each data source can launch from our portal "open-in" experience.</span></span> <span data-ttu-id="3757b-107">hello andra tabellen innehåller en mer teknisk specifikation för varje datakälla Anslutningsegenskapen.</span><span class="sxs-lookup"><span data-stu-id="3757b-107">hello second table contains a more technical specification of each data-source connection property.</span></span>


## <a name="list-of-supported-data-sources"></a><span data-ttu-id="3757b-108">Lista över datakällor som stöds</span><span class="sxs-lookup"><span data-stu-id="3757b-108">List of supported data sources</span></span>

<table>
    <tr>
       <td><span data-ttu-id="3757b-109"><b>Datakällobjektet</b></span><span class="sxs-lookup"><span data-stu-id="3757b-109"><b>Data source object</b></span></span></td>
       <td><span data-ttu-id="3757b-110"><b>API</b></span><span class="sxs-lookup"><span data-stu-id="3757b-110"><b>API</b></span></span></td>
       <td><span data-ttu-id="3757b-111"><b>Manuell inmatning</b></span><span class="sxs-lookup"><span data-stu-id="3757b-111"><b>Manual entry</b></span></span></td>
       <td><span data-ttu-id="3757b-112"><b>Registreringsverktyget</b></span><span class="sxs-lookup"><span data-stu-id="3757b-112"><b>Registration tool</b></span></span></td>
       <td><span data-ttu-id="3757b-113"><b>Öppna i Verktyg</b></span><span class="sxs-lookup"><span data-stu-id="3757b-113"><b>Open-in tools</b></span></span></td>
       <td><span data-ttu-id="3757b-114"><b>Anteckningar</b></span><span class="sxs-lookup"><span data-stu-id="3757b-114"><b>Notes</b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-115">Azure Data Lake Store-katalogen</span><span class="sxs-lookup"><span data-stu-id="3757b-115">Azure Data Lake Store directory</span></span></td>
      <td><span data-ttu-id="3757b-116">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-116">✓</span></span></td>
      <td><span data-ttu-id="3757b-117">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-117">✓</span></span></td>
      <td><span data-ttu-id="3757b-118">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-118">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-119">Azure Data Lake Store-fil</span><span class="sxs-lookup"><span data-stu-id="3757b-119">Azure Data Lake Store file</span></span></td>
      <td><span data-ttu-id="3757b-120">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-120">✓</span></span></td>
      <td><span data-ttu-id="3757b-121">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-121">✓</span></span></td>
      <td><span data-ttu-id="3757b-122">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-122">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-123">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3757b-123">Azure Blob storage</span></span></td>
      <td><span data-ttu-id="3757b-124">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-124">✓</span></span></td>
      <td><span data-ttu-id="3757b-125">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-125">✓</span></span></td>
      <td><span data-ttu-id="3757b-126">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-126">✓</span></span></td>
      <td><span data-ttu-id="3757b-127"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="3757b-127"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-128">Azure Storage-katalogen</span><span class="sxs-lookup"><span data-stu-id="3757b-128">Azure Storage directory</span></span></td>
      <td><span data-ttu-id="3757b-129">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-129">✓</span></span></td>
      <td><span data-ttu-id="3757b-130">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-130">✓</span></span></td>
      <td><span data-ttu-id="3757b-131">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-131">✓</span></span></td>
      <td><span data-ttu-id="3757b-132"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="3757b-132"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-133">Azure Storage-tabellen</span><span class="sxs-lookup"><span data-stu-id="3757b-133">Azure Storage table</span></span></td>
      <td><span data-ttu-id="3757b-134">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-134">✓</span></span></td>
      <td><span data-ttu-id="3757b-135">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-135">✓</span></span></td>
      <td><span data-ttu-id="3757b-136">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-136">✓</span></span></td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-137">HDFS-katalog</span><span class="sxs-lookup"><span data-stu-id="3757b-137">HDFS directory</span></span></td>
      <td><span data-ttu-id="3757b-138">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-138">✓</span></span></td>
      <td><span data-ttu-id="3757b-139">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-139">✓</span></span></td>
      <td><span data-ttu-id="3757b-140">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-140">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-141">HDFS-fil</span><span class="sxs-lookup"><span data-stu-id="3757b-141">HDFS file</span></span></td>
      <td><span data-ttu-id="3757b-142">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-142">✓</span></span></td>
      <td><span data-ttu-id="3757b-143">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-143">✓</span></span></td>
      <td><span data-ttu-id="3757b-144">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-144">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-145">Hive-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-145">Hive table</span></span></td>
      <td><span data-ttu-id="3757b-146">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-146">✓</span></span></td>
      <td><span data-ttu-id="3757b-147">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-147">✓</span></span></td>
      <td><span data-ttu-id="3757b-148">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-148">✓</span></span></td>
      <td><span data-ttu-id="3757b-149"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="3757b-149"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-150">Hive-vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-150">Hive view</span></span></td>
      <td><span data-ttu-id="3757b-151">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-151">✓</span></span></td>
      <td><span data-ttu-id="3757b-152">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-152">✓</span></span></td>
      <td><span data-ttu-id="3757b-153">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-153">✓</span></span></td>
      <td><span data-ttu-id="3757b-154"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="3757b-154"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-155">MySQL-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-155">MySQL table</span></span></td>
      <td><span data-ttu-id="3757b-156">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-156">✓</span></span></td>
      <td><span data-ttu-id="3757b-157">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-157">✓</span></span></td>
      <td><span data-ttu-id="3757b-158">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-158">✓</span></span></td>
      <td><span data-ttu-id="3757b-159"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-159"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-160">MySQL-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-160">MySQL view</span></span></td>
      <td><span data-ttu-id="3757b-161">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-161">✓</span></span></td>
      <td><span data-ttu-id="3757b-162">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-162">✓</span></span></td>
      <td><span data-ttu-id="3757b-163">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-163">✓</span></span></td>
      <td><span data-ttu-id="3757b-164"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-164"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-165">Oracle Database-tabellen</span><span class="sxs-lookup"><span data-stu-id="3757b-165">Oracle Database table</span></span></td>
      <td><span data-ttu-id="3757b-166">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-166">✓</span></span></td>
      <td><span data-ttu-id="3757b-167">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-167">✓</span></span></td>
      <td><span data-ttu-id="3757b-168">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-168">✓</span></span></td>
      <td><span data-ttu-id="3757b-169"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-169"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-170">Oracle databasvy</span><span class="sxs-lookup"><span data-stu-id="3757b-170">Oracle Database view</span></span></td>
      <td><span data-ttu-id="3757b-171">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-171">✓</span></span></td>
      <td><span data-ttu-id="3757b-172">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-172">✓</span></span></td>
      <td><span data-ttu-id="3757b-173">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-173">✓</span></span></td>
      <td><span data-ttu-id="3757b-174"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-174"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-175">Andra (allmän tillgång)</span><span class="sxs-lookup"><span data-stu-id="3757b-175">Other (generic asset)</span></span></td>
      <td><span data-ttu-id="3757b-176">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-176">✓</span></span></td>
      <td><span data-ttu-id="3757b-177">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-177">✓</span></span></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-178">Azure SQL Data Warehouse-tabellen</span><span class="sxs-lookup"><span data-stu-id="3757b-178">Azure SQL Data Warehouse table</span></span></td>
      <td><span data-ttu-id="3757b-179">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-179">✓</span></span></td>
      <td><span data-ttu-id="3757b-180">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-180">✓</span></span></td>
      <td><span data-ttu-id="3757b-181">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-181">✓</span></span></td>
      <td><span data-ttu-id="3757b-182"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="3757b-182"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-183">SQL Data Warehouse-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-183">SQL Data Warehouse view</span></span></td>
      <td><span data-ttu-id="3757b-184">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-184">✓</span></span></td>
      <td><span data-ttu-id="3757b-185">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-185">✓</span></span></td>
      <td><span data-ttu-id="3757b-186">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-186">✓</span></span></td>
      <td><span data-ttu-id="3757b-187"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="3757b-187"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-188">SQL Server Analysis Services-dimension</span><span class="sxs-lookup"><span data-stu-id="3757b-188">SQL Server Analysis Services dimension</span></span></td>
      <td><span data-ttu-id="3757b-189">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-189">✓</span></span></td>
      <td><span data-ttu-id="3757b-190">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-190">✓</span></span></td>
      <td><span data-ttu-id="3757b-191">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-191">✓</span></span></td>
      <td><span data-ttu-id="3757b-192"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-192"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-193">SQL Server Analysis Services KPI</span><span class="sxs-lookup"><span data-stu-id="3757b-193">SQL Server Analysis Services KPI</span></span></td>
      <td><span data-ttu-id="3757b-194">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-194">✓</span></span></td>
      <td><span data-ttu-id="3757b-195">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-195">✓</span></span></td>
      <td><span data-ttu-id="3757b-196">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-196">✓</span></span></td>
      <td><span data-ttu-id="3757b-197"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-197"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-198">SQL Server Analysis Services-mått</span><span class="sxs-lookup"><span data-stu-id="3757b-198">SQL Server Analysis Services measure</span></span></td>
      <td><span data-ttu-id="3757b-199">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-199">✓</span></span></td>
      <td><span data-ttu-id="3757b-200">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-200">✓</span></span></td>
      <td><span data-ttu-id="3757b-201">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-201">✓</span></span></td>
      <td><span data-ttu-id="3757b-202"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-202"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-203">SQL Server Analysis Services-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-203">SQL Server Analysis Services table</span></span></td>
      <td><span data-ttu-id="3757b-204">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-204">✓</span></span></td>
      <td><span data-ttu-id="3757b-205">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-205">✓</span></span></td>
      <td><span data-ttu-id="3757b-206">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-206">✓</span></span></td>
      <td><span data-ttu-id="3757b-207"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="3757b-207"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-208">SQL Server Reporting Services-rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-208">SQL Server Reporting Services report</span></span></td>
      <td><span data-ttu-id="3757b-209">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-209">✓</span></span></td>
      <td><span data-ttu-id="3757b-210">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-210">✓</span></span></td>
      <td><span data-ttu-id="3757b-211">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-211">✓</span></span></td>
      <td><span data-ttu-id="3757b-212"><font size=2>Webbläsare</font></span><span class="sxs-lookup"><span data-stu-id="3757b-212"><font size=2>Browser</font></span></span></td>
      <td><span data-ttu-id="3757b-213"><font size=2>Enhetligt läge-servrar. SharePoint-läge stöds inte.</font></span><span class="sxs-lookup"><span data-stu-id="3757b-213"><font size=2>Native mode servers only. SharePoint mode is not supported.</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-214">SQL Server-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-214">SQL Server table</span></span></td>
      <td><span data-ttu-id="3757b-215">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-215">✓</span></span></td>
      <td><span data-ttu-id="3757b-216">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-216">✓</span></span></td>
      <td><span data-ttu-id="3757b-217">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-217">✓</span></span></td>
      <td><span data-ttu-id="3757b-218"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="3757b-218"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-219">SQL Server-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-219">SQL Server view</span></span></td>
      <td><span data-ttu-id="3757b-220">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-220">✓</span></span></td>
      <td><span data-ttu-id="3757b-221">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-221">✓</span></span></td>
      <td><span data-ttu-id="3757b-222">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-222">✓</span></span></td>
      <td><span data-ttu-id="3757b-223"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="3757b-223"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-224">Tabell för Teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-224">Teradata table</span></span></td>
      <td><span data-ttu-id="3757b-225">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-225">✓</span></span></td>
      <td><span data-ttu-id="3757b-226">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-226">✓</span></span></td>
      <td><span data-ttu-id="3757b-227">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-227">✓</span></span></td>
      <td><span data-ttu-id="3757b-228"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="3757b-228"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-229">Visa för Teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-229">Teradata view</span></span></td>
      <td><span data-ttu-id="3757b-230">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-230">✓</span></span></td>
      <td><span data-ttu-id="3757b-231">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-231">✓</span></span></td>
      <td><span data-ttu-id="3757b-232">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-232">✓</span></span></td>
      <td><span data-ttu-id="3757b-233"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="3757b-233"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-234">SAP HANA-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-234">SAP HANA view</span></span></td>
      <td><span data-ttu-id="3757b-235">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-235">✓</span></span></td>
      <td><span data-ttu-id="3757b-236">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-236">✓</span></span></td>
      <td><span data-ttu-id="3757b-237">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-237">✓</span></span></td>
      <td><span data-ttu-id="3757b-238"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="3757b-238"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-239">DB2-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-239">DB2 table</span></span></td>
      <td><span data-ttu-id="3757b-240">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-240">✓</span></span></td>
      <td><span data-ttu-id="3757b-241">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-241">✓</span></span></td>
      <td><span data-ttu-id="3757b-242">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-242">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-243">DB2-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-243">DB2 view</span></span></td>
      <td><span data-ttu-id="3757b-244">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-244">✓</span></span></td>
      <td><span data-ttu-id="3757b-245">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-245">✓</span></span></td>
      <td><span data-ttu-id="3757b-246">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-246">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-247">Fil för system</span><span class="sxs-lookup"><span data-stu-id="3757b-247">File system file</span></span></td>
      <td><span data-ttu-id="3757b-248">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-248">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-249">FTP-katalog</span><span class="sxs-lookup"><span data-stu-id="3757b-249">FTP directory</span></span></td>
      <td><span data-ttu-id="3757b-250">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-250">✓</span></span></td>
      <td><span data-ttu-id="3757b-251">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-251">✓</span></span></td>
      <td><span data-ttu-id="3757b-252">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-252">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-253">FTP-fil</span><span class="sxs-lookup"><span data-stu-id="3757b-253">FTP file</span></span></td>
      <td><span data-ttu-id="3757b-254">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-254">✓</span></span></td>
      <td><span data-ttu-id="3757b-255">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-255">✓</span></span></td>
      <td><span data-ttu-id="3757b-256">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-256">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-257">HTTP-rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-257">HTTP report</span></span></td>
      <td><span data-ttu-id="3757b-258">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-258">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-259">HTTP-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="3757b-259">HTTP endpoint</span></span></td>
      <td><span data-ttu-id="3757b-260">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-260">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-261">HTTP-fil</span><span class="sxs-lookup"><span data-stu-id="3757b-261">HTTP file</span></span></td>
      <td><span data-ttu-id="3757b-262">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-262">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-263">OData-entitetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="3757b-263">OData entity set</span></span></td>
      <td><span data-ttu-id="3757b-264">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-264">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-265">OData-funktion</span><span class="sxs-lookup"><span data-stu-id="3757b-265">OData function</span></span></td>
      <td><span data-ttu-id="3757b-266">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-266">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-267">PostgreSQL tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-267">PostgreSQL table</span></span></td>
      <td><span data-ttu-id="3757b-268">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-268">✓</span></span></td>
      <td><span data-ttu-id="3757b-269">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-269">✓</span></span></td>
      <td><span data-ttu-id="3757b-270">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-270">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-271">PostgreSQL-vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-271">PostgreSQL view</span></span></td>
      <td><span data-ttu-id="3757b-272">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-272">✓</span></span></td>
      <td><span data-ttu-id="3757b-273">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-273">✓</span></span></td>
      <td><span data-ttu-id="3757b-274">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-274">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-275">SAP HANA-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-275">SAP HANA view</span></span></td>
      <td><span data-ttu-id="3757b-276">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-276">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> <span data-ttu-id="3757b-277">Salesforce-objekt</span><span class="sxs-lookup"><span data-stu-id="3757b-277">Salesforce object</span></span></td>
      <td><span data-ttu-id="3757b-278">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-278">✓</span></span></td>
      <td><span data-ttu-id="3757b-279">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-279">✓</span></span></td>
      <td><span data-ttu-id="3757b-280">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-280">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-281">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="3757b-281">SharePoint list</span></span> </td>
      <td><span data-ttu-id="3757b-282">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-282">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-283">Azure DB Cosmos-samling</span><span class="sxs-lookup"><span data-stu-id="3757b-283">Azure Cosmos DB collection</span></span></td>
      <td><span data-ttu-id="3757b-284">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-284">✓</span></span></td>
      <td><span data-ttu-id="3757b-285">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-285">✓</span></span></td>
      <td><span data-ttu-id="3757b-286">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-286">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-287">Allmän ODBC-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-287">Generic ODBC table</span></span></td>
      <td><span data-ttu-id="3757b-288">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-288">✓</span></span></td>
      <td><span data-ttu-id="3757b-289">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-289">✓</span></span></td>
      <td><span data-ttu-id="3757b-290">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-290">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-291">Allmän ODBC-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-291">Generic ODBC view</span></span></td>
      <td><span data-ttu-id="3757b-292">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-292">✓</span></span></td>
      <td><span data-ttu-id="3757b-293">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-293">✓</span></span></td>
      <td><span data-ttu-id="3757b-294">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-294">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-295">Cassandra tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-295">Cassandra table</span></span></td>
      <td><span data-ttu-id="3757b-296">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-296">✓</span></span></td>
      <td><span data-ttu-id="3757b-297">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-297">✓</span></span></td>
      <td><span data-ttu-id="3757b-298">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-298">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="3757b-299"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="3757b-299"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-300">Cassandra vy</span><span class="sxs-lookup"><span data-stu-id="3757b-300">Cassandra view</span></span></td>
      <td><span data-ttu-id="3757b-301">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-301">✓</span></span></td>
      <td><span data-ttu-id="3757b-302">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-302">✓</span></span></td>
      <td><span data-ttu-id="3757b-303">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-303">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="3757b-304"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="3757b-304"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-305">Sybase-tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-305">Sybase table</span></span></td>
      <td><span data-ttu-id="3757b-306">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-306">✓</span></span></td>
      <td><span data-ttu-id="3757b-307">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-307">✓</span></span></td>
      <td><span data-ttu-id="3757b-308">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-308">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-309">Sybase-vy</span><span class="sxs-lookup"><span data-stu-id="3757b-309">Sybase view</span></span></td>
      <td><span data-ttu-id="3757b-310">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-310">✓</span></span></td>
      <td><span data-ttu-id="3757b-311">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-311">✓</span></span></td>
      <td><span data-ttu-id="3757b-312">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-312">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-313">Tabell för MongoDB</span><span class="sxs-lookup"><span data-stu-id="3757b-313">MongoDB table</span></span></td>
      <td><span data-ttu-id="3757b-314">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-314">✓</span></span></td>
      <td><span data-ttu-id="3757b-315">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-315">✓</span></span></td>
      <td><span data-ttu-id="3757b-316">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-316">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="3757b-317"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="3757b-317"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-318">Visa MongoDB</span><span class="sxs-lookup"><span data-stu-id="3757b-318">MongoDB view</span></span></td>
      <td><span data-ttu-id="3757b-319">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-319">✓</span></span></td>
      <td><span data-ttu-id="3757b-320">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-320">✓</span></span></td>
      <td><span data-ttu-id="3757b-321">✓</span><span class="sxs-lookup"><span data-stu-id="3757b-321">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="3757b-322"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="3757b-322"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
</table>

<span data-ttu-id="3757b-323">Om du behöver stöd för ytterligare källor kan skicka en funktionen begäran toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="3757b-323">If you need support for additional sources, submit a feature request toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span></span>


## <a name="data-source-reference-specification"></a><span data-ttu-id="3757b-324">Referens-specifikationen för datakälla</span><span class="sxs-lookup"><span data-stu-id="3757b-324">Data-source reference specification</span></span>
> [!NOTE]
> <span data-ttu-id="3757b-325">Hej **DSL-strukturen** kolumnen i den följande tabellen hello visas endast hello anslutningsegenskaper ”adress” egenskapsuppsättningen som används av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="3757b-325">hello **DSL structure** column in hello following table lists only hello connection properties for "address" property bag that are used by Azure Data Catalog.</span></span> <span data-ttu-id="3757b-326">Det vill säga kan ”adress” egenskapsuppsättning innehålla andra anslutningsegenskaperna för hello datakällan som Azure Data Catalog kvarstår, men använder inte.</span><span class="sxs-lookup"><span data-stu-id="3757b-326">That is, "address" property bag can contain other connection properties of hello data source which Azure Data Catalog persists, but does not use.</span></span>

<table>
    <tr>
       <td><span data-ttu-id="3757b-327"><b>Typ av datakälla</b></span><span class="sxs-lookup"><span data-stu-id="3757b-327"><b>Source type</b></span></span></td>
       <td><span data-ttu-id="3757b-328"><b>Tillgångstypen</b></span><span class="sxs-lookup"><span data-stu-id="3757b-328"><b>Asset type</b></span></span></td>
       <td><span data-ttu-id="3757b-329"><b>Objekttyper</b></span><span class="sxs-lookup"><span data-stu-id="3757b-329"><b>Object types</b></span></span></td>
       <td><span data-ttu-id="3757b-330"><b>DSL-struktur<b></span><span class="sxs-lookup"><span data-stu-id="3757b-330"><b>DSL structure<b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-331">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3757b-331">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="3757b-332">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-332">Container</span></span></td>
      <td><span data-ttu-id="3757b-333">Data Lake</span><span class="sxs-lookup"><span data-stu-id="3757b-333">Data Lake</span></span></td>
      <td><span data-ttu-id="3757b-334">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="3757b-334">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="3757b-335">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-335">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="3757b-336">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-336">Address:</span></span> <br><span data-ttu-id="3757b-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-338">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3757b-338">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="3757b-339">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-339">Table</span></span></td>
      <td><span data-ttu-id="3757b-340">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="3757b-340">Directory, file</span></span></td>
      <td><span data-ttu-id="3757b-341">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="3757b-341">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="3757b-342">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-342">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="3757b-343">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-343">Address:</span></span> <br><span data-ttu-id="3757b-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-345">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3757b-345">Azure Storage</span></span></td>
      <td><span data-ttu-id="3757b-346">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-346">Container</span></span></td>
      <td><span data-ttu-id="3757b-347">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-347">Container</span></span></td>
      <td><span data-ttu-id="3757b-348">
        <font size=2>Protokoll: azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="3757b-348">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="3757b-349">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-349">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-350">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-350">Address:</span></span> <br><span data-ttu-id="3757b-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="3757b-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="3757b-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="3757b-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="3757b-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;behållaren</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-354">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3757b-354">Azure Storage</span></span></td>
      <td><span data-ttu-id="3757b-355">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-355">Table</span></span></td>
      <td><span data-ttu-id="3757b-356">BLOB, directory</span><span class="sxs-lookup"><span data-stu-id="3757b-356">Blob, directory</span></span></td>
      <td><span data-ttu-id="3757b-357">
        <font size=2>Protokoll: azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="3757b-357">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="3757b-358">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-358">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-359">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-359">Address:</span></span> <br><span data-ttu-id="3757b-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="3757b-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="3757b-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="3757b-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="3757b-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;behållaren</span><span class="sxs-lookup"><span data-stu-id="3757b-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container</span></span> <br><span data-ttu-id="3757b-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namn</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-364">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3757b-364">Azure Storage</span></span></td>
      <td><span data-ttu-id="3757b-365">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-365">Container</span></span></td>
      <td><span data-ttu-id="3757b-366">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-366">Container</span></span></td>
      <td><span data-ttu-id="3757b-367">
        <font size=2>Protokoll: azure-tabeller</span><span class="sxs-lookup"><span data-stu-id="3757b-367">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="3757b-368">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-368">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-369">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-369">Address:</span></span> <br><span data-ttu-id="3757b-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="3757b-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="3757b-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-372">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3757b-372">Azure Storage</span></span></td>
      <td><span data-ttu-id="3757b-373">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-373">Table</span></span></td>
      <td><span data-ttu-id="3757b-374">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-374">Table</span></span></td>
      <td><span data-ttu-id="3757b-375">
        <font size=2>Protokoll: azure-tabeller</span><span class="sxs-lookup"><span data-stu-id="3757b-375">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="3757b-376">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-376">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-377">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-377">Address:</span></span> <br><span data-ttu-id="3757b-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="3757b-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="3757b-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="3757b-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="3757b-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namn</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-381">Cosmos</span><span class="sxs-lookup"><span data-stu-id="3757b-381">Cosmos</span></span></td>
      <td><span data-ttu-id="3757b-382">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-382">Container</span></span></td>
      <td><span data-ttu-id="3757b-383">Virtuellt kluster</span><span class="sxs-lookup"><span data-stu-id="3757b-383">Virtual cluster</span></span></td>
      <td><span data-ttu-id="3757b-384">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="3757b-384">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="3757b-385">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-385">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-386">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-386">Address:</span></span> <br><span data-ttu-id="3757b-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-388">Cosmos</span><span class="sxs-lookup"><span data-stu-id="3757b-388">Cosmos</span></span></td>
      <td><span data-ttu-id="3757b-389">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-389">Table</span></span></td>
      <td><span data-ttu-id="3757b-390">Stream, stream set vy</span><span class="sxs-lookup"><span data-stu-id="3757b-390">Stream, stream set, view</span></span></td>
      <td><span data-ttu-id="3757b-391">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="3757b-391">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="3757b-392">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-392">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-393">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-393">Address:</span></span> <br><span data-ttu-id="3757b-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-395">Datazen</span><span class="sxs-lookup"><span data-stu-id="3757b-395">Datazen</span></span></td>
      <td><span data-ttu-id="3757b-396">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-396">Container</span></span></td>
      <td><span data-ttu-id="3757b-397">plats</span><span class="sxs-lookup"><span data-stu-id="3757b-397">Site</span></span></td>
      <td><span data-ttu-id="3757b-398">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-398">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-399">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-399">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-400">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-400">Address:</span></span> <br><span data-ttu-id="3757b-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-402">Datazen</span><span class="sxs-lookup"><span data-stu-id="3757b-402">Datazen</span></span></td>
      <td><span data-ttu-id="3757b-403">Rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-403">Report</span></span></td>
      <td><span data-ttu-id="3757b-404">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="3757b-404">Report, dashboard</span></span></td>
      <td><span data-ttu-id="3757b-405">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-405">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-406">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-406">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-407">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-407">Address:</span></span> <br><span data-ttu-id="3757b-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-409">DB2</span><span class="sxs-lookup"><span data-stu-id="3757b-409">DB2</span></span></td>
      <td><span data-ttu-id="3757b-410">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-410">Container</span></span></td>
      <td><span data-ttu-id="3757b-411">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-411">Database</span></span></td>
      <td><span data-ttu-id="3757b-412">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="3757b-412">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="3757b-413">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-413">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-414">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-414">Address:</span></span> <br><span data-ttu-id="3757b-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-417">DB2</span><span class="sxs-lookup"><span data-stu-id="3757b-417">DB2</span></span></td>
      <td><span data-ttu-id="3757b-418">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-418">Table</span></span></td>
      <td><span data-ttu-id="3757b-419">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-419">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-420">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="3757b-420">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="3757b-421">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-421">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-422">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-422">Address:</span></span> <br><span data-ttu-id="3757b-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-427">Filsystem</span><span class="sxs-lookup"><span data-stu-id="3757b-427">File system</span></span></td>
      <td><span data-ttu-id="3757b-428">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-428">Table</span></span></td>
      <td><span data-ttu-id="3757b-429">Fil</span><span class="sxs-lookup"><span data-stu-id="3757b-429">File</span></span></td>
      <td><span data-ttu-id="3757b-430">
        <font size=2>Protokoll: filen</span><span class="sxs-lookup"><span data-stu-id="3757b-430">
        <font size=2> Protocol: file</span></span> <br><span data-ttu-id="3757b-431">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-431">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="3757b-432">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-432">Address:</span></span> <br><span data-ttu-id="3757b-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sökväg</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-434">FTP</span><span class="sxs-lookup"><span data-stu-id="3757b-434">FTP</span></span></td>
      <td><span data-ttu-id="3757b-435">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-435">Table</span></span></td>
      <td><span data-ttu-id="3757b-436">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="3757b-436">Directory, file</span></span></td>
      <td><span data-ttu-id="3757b-437">
        <font size=2>Protokoll: ftp</span><span class="sxs-lookup"><span data-stu-id="3757b-437">
        <font size=2> Protocol: ftp</span></span> <br><span data-ttu-id="3757b-438">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-438">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="3757b-439">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-439">Address:</span></span> <br><span data-ttu-id="3757b-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-441">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="3757b-441">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="3757b-442">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-442">Container</span></span></td>
      <td><span data-ttu-id="3757b-443">Kluster</span><span class="sxs-lookup"><span data-stu-id="3757b-443">Cluster</span></span></td>
      <td><span data-ttu-id="3757b-444">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="3757b-444">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="3757b-445">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-445">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="3757b-446">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-446">Address:</span></span> <br><span data-ttu-id="3757b-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-448">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="3757b-448">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="3757b-449">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-449">Table</span></span></td>
      <td><span data-ttu-id="3757b-450">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="3757b-450">Directory, file</span></span></td>
      <td><span data-ttu-id="3757b-451">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="3757b-451">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="3757b-452">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-452">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="3757b-453">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-453">Address:</span></span> <br><span data-ttu-id="3757b-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-455">Hive</span><span class="sxs-lookup"><span data-stu-id="3757b-455">Hive</span></span></td>
      <td><span data-ttu-id="3757b-456">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-456">Container</span></span></td>
      <td><span data-ttu-id="3757b-457">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-457">Database</span></span></td>
      <td><span data-ttu-id="3757b-458">
        <font size=2>Protokoll: hive</span><span class="sxs-lookup"><span data-stu-id="3757b-458">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="3757b-459">Autentisering: {HDInsight, basic, username, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-459">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="3757b-460">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-460">Address:</span></span> <br><span data-ttu-id="3757b-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-463">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="3757b-463">connectionProperties:</span></span> <br><span data-ttu-id="3757b-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-465">Hive</span><span class="sxs-lookup"><span data-stu-id="3757b-465">Hive</span></span></td>
      <td><span data-ttu-id="3757b-466">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-466">Table</span></span></td>
      <td><span data-ttu-id="3757b-467">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-467">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-468">
        <font size=2>Protokoll: hive</span><span class="sxs-lookup"><span data-stu-id="3757b-468">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="3757b-469">Autentisering: {HDInsight, basic, username, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-469">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="3757b-470">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-470">Address:</span></span> <br><span data-ttu-id="3757b-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-474">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="3757b-474">connectionProperties:</span></span> <br><span data-ttu-id="3757b-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-476">HTTP</span><span class="sxs-lookup"><span data-stu-id="3757b-476">HTTP</span></span></td>
      <td><span data-ttu-id="3757b-477">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-477">Container</span></span></td>
      <td><span data-ttu-id="3757b-478">plats</span><span class="sxs-lookup"><span data-stu-id="3757b-478">Site</span></span></td>
      <td><span data-ttu-id="3757b-479">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-479">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-480">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-480">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-481">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-481">Address:</span></span> <br><span data-ttu-id="3757b-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-483">HTTP</span><span class="sxs-lookup"><span data-stu-id="3757b-483">HTTP</span></span></td>
      <td><span data-ttu-id="3757b-484">Rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-484">Report</span></span></td>
      <td><span data-ttu-id="3757b-485">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="3757b-485">Report, dashboard</span></span></td>
      <td><span data-ttu-id="3757b-486">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-486">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-487">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-487">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-488">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-488">Address:</span></span> <br><span data-ttu-id="3757b-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-490">HTTP</span><span class="sxs-lookup"><span data-stu-id="3757b-490">HTTP</span></span></td>
      <td><span data-ttu-id="3757b-491">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-491">Table</span></span></td>
      <td><span data-ttu-id="3757b-492">Slutpunkt, fil</span><span class="sxs-lookup"><span data-stu-id="3757b-492">Endpoint, file</span></span></td>
      <td><span data-ttu-id="3757b-493">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-493">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-494">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-494">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-495">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-495">Address:</span></span> <br><span data-ttu-id="3757b-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-497">MySQL</span><span class="sxs-lookup"><span data-stu-id="3757b-497">MySQL</span></span></td>
      <td><span data-ttu-id="3757b-498">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-498">Container</span></span></td>
      <td><span data-ttu-id="3757b-499">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-499">Database</span></span></td>
      <td><span data-ttu-id="3757b-500">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="3757b-500">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="3757b-501">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-501">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-502">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-502">Address:</span></span> <br><span data-ttu-id="3757b-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-505">MySQL</span><span class="sxs-lookup"><span data-stu-id="3757b-505">MySQL</span></span></td>
      <td><span data-ttu-id="3757b-506">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-506">Table</span></span></td>
      <td><span data-ttu-id="3757b-507">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-507">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-508">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="3757b-508">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="3757b-509">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-509">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-510">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-510">Address:</span></span> <br><span data-ttu-id="3757b-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-514">OData</span><span class="sxs-lookup"><span data-stu-id="3757b-514">OData</span></span></td>
      <td><span data-ttu-id="3757b-515">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-515">Container</span></span></td>
      <td><span data-ttu-id="3757b-516">Entitetsbehållaren</span><span class="sxs-lookup"><span data-stu-id="3757b-516">Entity container</span></span></td>
      <td><span data-ttu-id="3757b-517">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="3757b-517">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="3757b-518">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-518">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="3757b-519">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-519">Address:</span></span> <br><span data-ttu-id="3757b-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-521">OData</span><span class="sxs-lookup"><span data-stu-id="3757b-521">OData</span></span></td>
      <td><span data-ttu-id="3757b-522">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-522">Table</span></span></td>
      <td><span data-ttu-id="3757b-523">Entitetsuppsättningen funktion</span><span class="sxs-lookup"><span data-stu-id="3757b-523">Entity set, function</span></span></td>
      <td><span data-ttu-id="3757b-524">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="3757b-524">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="3757b-525">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-525">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="3757b-526">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-526">Address:</span></span> <br><span data-ttu-id="3757b-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="3757b-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="3757b-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resursen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-529">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="3757b-529">Oracle Database</span></span></td>
      <td><span data-ttu-id="3757b-530">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-530">Container</span></span></td>
      <td><span data-ttu-id="3757b-531">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-531">Database</span></span></td>
      <td><span data-ttu-id="3757b-532">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="3757b-532">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="3757b-533">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-533">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-534">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-534">Address:</span></span> <br><span data-ttu-id="3757b-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-537">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="3757b-537">Oracle Database</span></span></td>
      <td><span data-ttu-id="3757b-538">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-538">Table</span></span></td>
      <td><span data-ttu-id="3757b-539">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-539">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-540">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="3757b-540">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="3757b-541">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-541">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-542">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-542">Address:</span></span> <br><span data-ttu-id="3757b-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-547">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3757b-547">PostgreSQL</span></span></td>
      <td><span data-ttu-id="3757b-548">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-548">Container</span></span></td>
      <td><span data-ttu-id="3757b-549">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-549">Database</span></span></td>
      <td><span data-ttu-id="3757b-550">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="3757b-550">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="3757b-551">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-551">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-552">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-552">Address:</span></span> <br><span data-ttu-id="3757b-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-555">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3757b-555">PostgreSQL</span></span></td>
      <td><span data-ttu-id="3757b-556">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-556">Table</span></span></td>
      <td><span data-ttu-id="3757b-557">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-557">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-558">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="3757b-558">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="3757b-559">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-559">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-560">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-560">Address:</span></span> <br><span data-ttu-id="3757b-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-565">Power BI</span><span class="sxs-lookup"><span data-stu-id="3757b-565">Power BI</span></span></td>
      <td><span data-ttu-id="3757b-566">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-566">Container</span></span></td>
      <td><span data-ttu-id="3757b-567">plats</span><span class="sxs-lookup"><span data-stu-id="3757b-567">Site</span></span></td>
      <td><span data-ttu-id="3757b-568">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-568">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-569">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-569">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-570">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-570">Address:</span></span> <br><span data-ttu-id="3757b-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-572">Power BI</span><span class="sxs-lookup"><span data-stu-id="3757b-572">Power BI</span></span></td>
      <td><span data-ttu-id="3757b-573">Rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-573">Report</span></span></td>
      <td><span data-ttu-id="3757b-574">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="3757b-574">Report, dashboard</span></span></td>
      <td><span data-ttu-id="3757b-575">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="3757b-575">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="3757b-576">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-576">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="3757b-577">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-577">Address:</span></span> <br><span data-ttu-id="3757b-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-579">Power Query</span><span class="sxs-lookup"><span data-stu-id="3757b-579">Power Query</span></span></td>
      <td><span data-ttu-id="3757b-580">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-580">Table</span></span></td>
      <td><span data-ttu-id="3757b-581">Data mashup</span><span class="sxs-lookup"><span data-stu-id="3757b-581">Data mashup</span></span></td>
      <td><span data-ttu-id="3757b-582">
        <font size=2>Protokoll: power-fråga</span><span class="sxs-lookup"><span data-stu-id="3757b-582">
        <font size=2> Protocol: power-query</span></span> <br><span data-ttu-id="3757b-583">Autentisering: {oauth}</span><span class="sxs-lookup"><span data-stu-id="3757b-583">Authentication: {oauth}</span></span> <br><span data-ttu-id="3757b-584">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-584">Address:</span></span> <br><span data-ttu-id="3757b-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-586">Salesforce</span><span class="sxs-lookup"><span data-stu-id="3757b-586">Salesforce</span></span></td>
      <td><span data-ttu-id="3757b-587">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-587">Table</span></span></td>
      <td><span data-ttu-id="3757b-588">Objekt</span><span class="sxs-lookup"><span data-stu-id="3757b-588">Object</span></span></td>
      <td><span data-ttu-id="3757b-589">
        <font size=2>Protokoll: salesforce-com</span><span class="sxs-lookup"><span data-stu-id="3757b-589">
        <font size=2> Protocol: salesforce-com</span></span> <br><span data-ttu-id="3757b-590">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-590">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-591">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-591">Address:</span></span> <br><span data-ttu-id="3757b-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer</span><span class="sxs-lookup"><span data-stu-id="3757b-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer</span></span> <br><span data-ttu-id="3757b-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klass</span><span class="sxs-lookup"><span data-stu-id="3757b-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class</span></span> <br><span data-ttu-id="3757b-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-595">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3757b-595">SAP HANA</span></span></td>
      <td><span data-ttu-id="3757b-596">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-596">Container</span></span></td>
      <td><span data-ttu-id="3757b-597">Server</span><span class="sxs-lookup"><span data-stu-id="3757b-597">Server</span></span></td>
      <td><span data-ttu-id="3757b-598">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="3757b-598">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="3757b-599">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-599">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-600">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-600">Address:</span></span> <br><span data-ttu-id="3757b-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-602">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3757b-602">SAP HANA</span></span></td>
      <td><span data-ttu-id="3757b-603">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-603">Table</span></span></td>
      <td><span data-ttu-id="3757b-604">Visa</span><span class="sxs-lookup"><span data-stu-id="3757b-604">View</span></span></td>
      <td><span data-ttu-id="3757b-605">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="3757b-605">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="3757b-606">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-606">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-607">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-607">Address:</span></span> <br><span data-ttu-id="3757b-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-611">SharePoint</span><span class="sxs-lookup"><span data-stu-id="3757b-611">SharePoint</span></span></td>
      <td><span data-ttu-id="3757b-612">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-612">Table</span></span></td>
      <td><span data-ttu-id="3757b-613">Visa lista</span><span class="sxs-lookup"><span data-stu-id="3757b-613">List</span></span></td>
      <td><span data-ttu-id="3757b-614">
        <font size=2>Protokoll: sharepoint-lista över</span><span class="sxs-lookup"><span data-stu-id="3757b-614">
        <font size=2> Protocol: sharepoint-list</span></span> <br><span data-ttu-id="3757b-615">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-615">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-616">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-616">Address:</span></span> <br><span data-ttu-id="3757b-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-618">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3757b-618">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="3757b-619">Kommando</span><span class="sxs-lookup"><span data-stu-id="3757b-619">Command</span></span></td>
      <td><span data-ttu-id="3757b-620">Lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="3757b-620">Stored procedure</span></span></td>
      <td><span data-ttu-id="3757b-621">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-621">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-622">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-622">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-623">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-623">Address:</span></span> <br><span data-ttu-id="3757b-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-628">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3757b-628">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="3757b-629">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="3757b-629">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="3757b-630">Tabellvärdesfunktion</span><span class="sxs-lookup"><span data-stu-id="3757b-630">Table-valued function</span></span></td>
      <td><span data-ttu-id="3757b-631">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-631">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-632">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-632">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-633">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-633">Address:</span></span> <br><span data-ttu-id="3757b-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-638">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3757b-638">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="3757b-639">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-639">Container</span></span></td>
      <td><span data-ttu-id="3757b-640">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-640">Database</span></span></td>
      <td><span data-ttu-id="3757b-641">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-641">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-642">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-642">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-643">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-643">Address:</span></span> <br><span data-ttu-id="3757b-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-646">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3757b-646">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="3757b-647">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-647">Table</span></span></td>
      <td><span data-ttu-id="3757b-648">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-648">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-649">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-649">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-650">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-650">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-651">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-651">Address:</span></span> <br><span data-ttu-id="3757b-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-656">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3757b-656">SQL Server</span></span></td>
      <td><span data-ttu-id="3757b-657">Kommando</span><span class="sxs-lookup"><span data-stu-id="3757b-657">Command</span></span></td>
      <td><span data-ttu-id="3757b-658">Lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="3757b-658">Stored procedure</span></span></td>
      <td><span data-ttu-id="3757b-659">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-659">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-660">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-660">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-661">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-661">Address:</span></span> <br><span data-ttu-id="3757b-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-666">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3757b-666">SQL Server</span></span></td>
      <td><span data-ttu-id="3757b-667">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="3757b-667">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="3757b-668">Tabellvärdesfunktion</span><span class="sxs-lookup"><span data-stu-id="3757b-668">Table-valued function</span></span></td>
      <td><span data-ttu-id="3757b-669">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-669">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-670">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-670">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-671">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-671">Address:</span></span> <br><span data-ttu-id="3757b-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-676">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3757b-676">SQL Server</span></span></td>
      <td><span data-ttu-id="3757b-677">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-677">Container</span></span></td>
      <td><span data-ttu-id="3757b-678">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-678">Database</span></span></td>
      <td><span data-ttu-id="3757b-679">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-679">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-680">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-680">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-681">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-681">Address:</span></span> <br><span data-ttu-id="3757b-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-684">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3757b-684">SQL Server</span></span></td>
      <td><span data-ttu-id="3757b-685">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-685">Table</span></span></td>
      <td><span data-ttu-id="3757b-686">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-686">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-687">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="3757b-687">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="3757b-688">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-688">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-689">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-689">Address:</span></span> <br><span data-ttu-id="3757b-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-694">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="3757b-694">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="3757b-695">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-695">Container</span></span></td>
      <td><span data-ttu-id="3757b-696">Modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-696">Model</span></span></td>
      <td><span data-ttu-id="3757b-697">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-697">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-698">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-698">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-699">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-699">Address:</span></span> <br><span data-ttu-id="3757b-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-703">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="3757b-703">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="3757b-704">KPI: N</span><span class="sxs-lookup"><span data-stu-id="3757b-704">KPI</span></span></td>
      <td><span data-ttu-id="3757b-705">KPI: N</span><span class="sxs-lookup"><span data-stu-id="3757b-705">KPI</span></span></td>
      <td><span data-ttu-id="3757b-706">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-706">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-707">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-707">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-708">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-708">Address:</span></span> <br><span data-ttu-id="3757b-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-714">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="3757b-714">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="3757b-715">Mått</span><span class="sxs-lookup"><span data-stu-id="3757b-715">Measure</span></span></td>
      <td><span data-ttu-id="3757b-716">Mått</span><span class="sxs-lookup"><span data-stu-id="3757b-716">Measure</span></span></td>
      <td><span data-ttu-id="3757b-717">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-717">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-718">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-718">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-719">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-719">Address:</span></span> <br><span data-ttu-id="3757b-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-725">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="3757b-725">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="3757b-726">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-726">Table</span></span></td>
      <td><span data-ttu-id="3757b-727">Dimensionen</span><span class="sxs-lookup"><span data-stu-id="3757b-727">Dimension</span></span></td>
      <td><span data-ttu-id="3757b-728">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-728">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-729">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-729">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-730">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-730">Address:</span></span> <br><span data-ttu-id="3757b-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {dimensionen}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-736">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="3757b-736">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="3757b-737">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-737">Container</span></span></td>
      <td><span data-ttu-id="3757b-738">Modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-738">Model</span></span></td>
      <td><span data-ttu-id="3757b-739">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-739">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-740">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-740">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-741">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-741">Address:</span></span> <br><span data-ttu-id="3757b-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-745">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="3757b-745">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="3757b-746">KPI: N</span><span class="sxs-lookup"><span data-stu-id="3757b-746">KPI</span></span></td>
      <td><span data-ttu-id="3757b-747">KPI: N</span><span class="sxs-lookup"><span data-stu-id="3757b-747">KPI</span></span></td>
      <td><span data-ttu-id="3757b-748">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-748">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-749">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-749">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-750">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-750">Address:</span></span> <br><span data-ttu-id="3757b-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-756">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="3757b-756">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="3757b-757">Mått</span><span class="sxs-lookup"><span data-stu-id="3757b-757">Measure</span></span></td>
      <td><span data-ttu-id="3757b-758">Mått</span><span class="sxs-lookup"><span data-stu-id="3757b-758">Measure</span></span></td>
      <td><span data-ttu-id="3757b-759">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-759">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-760">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-760">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-761">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-761">Address:</span></span> <br><span data-ttu-id="3757b-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-767">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="3757b-767">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="3757b-768">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-768">Table</span></span></td>
      <td><span data-ttu-id="3757b-769">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-769">Table</span></span></td>
      <td><span data-ttu-id="3757b-770">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-770">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="3757b-771">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="3757b-771">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="3757b-772">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-772">Address:</span></span> <br><span data-ttu-id="3757b-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-778">SQLServer Reporting Services</span><span class="sxs-lookup"><span data-stu-id="3757b-778">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="3757b-779">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-779">Container</span></span></td>
      <td><span data-ttu-id="3757b-780">Server</span><span class="sxs-lookup"><span data-stu-id="3757b-780">Server</span></span></td>
      <td><span data-ttu-id="3757b-781">
        <font size=2>Protokoll: Rapporteringstjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-781">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="3757b-782">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-782">Authentication: {windows}</span></span> <br><span data-ttu-id="3757b-783">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-783">Address:</span></span> <br><span data-ttu-id="3757b-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-786">SQLServer Reporting Services</span><span class="sxs-lookup"><span data-stu-id="3757b-786">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="3757b-787">Rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-787">Report</span></span></td>
      <td><span data-ttu-id="3757b-788">Rapport</span><span class="sxs-lookup"><span data-stu-id="3757b-788">Report</span></span></td>
      <td><span data-ttu-id="3757b-789">
        <font size=2>Protokoll: Rapporteringstjänster</span><span class="sxs-lookup"><span data-stu-id="3757b-789">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="3757b-790">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-790">Authentication: {windows}</span></span> <br><span data-ttu-id="3757b-791">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-791">Address:</span></span> <br><span data-ttu-id="3757b-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sökväg</span><span class="sxs-lookup"><span data-stu-id="3757b-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path</span></span> <br><span data-ttu-id="3757b-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-795">Teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-795">Teradata</span></span></td>
      <td><span data-ttu-id="3757b-796">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-796">Container</span></span></td>
      <td><span data-ttu-id="3757b-797">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-797">Database</span></span></td>
      <td><span data-ttu-id="3757b-798">
        <font size=2>Protokoll: teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-798">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="3757b-799">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-799">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-800">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-800">Address:</span></span> <br><span data-ttu-id="3757b-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-803">Teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-803">Teradata</span></span></td>
      <td><span data-ttu-id="3757b-804">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-804">Table</span></span></td>
      <td><span data-ttu-id="3757b-805">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-805">Table, view</span></span></td>
      <td><span data-ttu-id="3757b-806">
        <font size=2>Protokoll: teradata</span><span class="sxs-lookup"><span data-stu-id="3757b-806">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="3757b-807">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-807">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="3757b-808">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-808">Address:</span></span> <br><span data-ttu-id="3757b-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-812">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="3757b-812">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="3757b-813">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-813">Container</span></span></td>
      <td><span data-ttu-id="3757b-814">Modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-814">Model</span></span></td>
      <td><span data-ttu-id="3757b-815">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="3757b-815">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="3757b-816">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-816">Authentication: {windows}</span></span> <br><span data-ttu-id="3757b-817">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-817">Address:</span></span> <br><span data-ttu-id="3757b-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="3757b-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="3757b-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-821">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="3757b-821">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="3757b-822">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-822">Table</span></span></td>
      <td><span data-ttu-id="3757b-823">Entitet</span><span class="sxs-lookup"><span data-stu-id="3757b-823">Entity</span></span></td>
      <td><span data-ttu-id="3757b-824">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="3757b-824">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="3757b-825">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-825">Authentication: {windows}</span></span> <br><span data-ttu-id="3757b-826">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-826">Address:</span></span> <br><span data-ttu-id="3757b-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="3757b-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="3757b-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="3757b-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="3757b-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version</span><span class="sxs-lookup"><span data-stu-id="3757b-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version</span></span> <br><span data-ttu-id="3757b-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entitet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-831">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3757b-831">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="3757b-832">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-832">Container</span></span></td>
      <td><span data-ttu-id="3757b-833">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-833">Database</span></span></td>
      <td><span data-ttu-id="3757b-834">
        <font size=2>Protokoll: dokument-db</span><span class="sxs-lookup"><span data-stu-id="3757b-834">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="3757b-835">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-835">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-836">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-836">Address:</span></span> <br><span data-ttu-id="3757b-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="3757b-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="3757b-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-839">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3757b-839">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="3757b-840">Samling</span><span class="sxs-lookup"><span data-stu-id="3757b-840">Collection</span></span></td>
      <td><span data-ttu-id="3757b-841">Samling</span><span class="sxs-lookup"><span data-stu-id="3757b-841">Collection</span></span></td>
      <td><span data-ttu-id="3757b-842">
        <font size=2>Protokoll: dokument-db</span><span class="sxs-lookup"><span data-stu-id="3757b-842">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="3757b-843">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="3757b-843">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="3757b-844">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-844">Address:</span></span> <br><span data-ttu-id="3757b-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="3757b-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="3757b-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;samlingen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-848">ODBC (allmän)</span><span class="sxs-lookup"><span data-stu-id="3757b-848">Generic ODBC</span></span></td>
      <td><span data-ttu-id="3757b-849">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-849">Container</span></span></td>
      <td><span data-ttu-id="3757b-850">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-850">Database</span></span></td>
      <td><span data-ttu-id="3757b-851">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="3757b-851">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="3757b-852">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-852">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-853">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-853">Address:</span></span> <br><span data-ttu-id="3757b-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alternativ</span><span class="sxs-lookup"><span data-stu-id="3757b-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="3757b-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-856">ODBC (allmän)</span><span class="sxs-lookup"><span data-stu-id="3757b-856">Generic ODBC</span></span></td>
      <td><span data-ttu-id="3757b-857">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-857">Table</span></span></td>
      <td><span data-ttu-id="3757b-858">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-858">Table, View</span></span></td>
      <td><span data-ttu-id="3757b-859">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="3757b-859">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="3757b-860">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-860">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-861">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-861">Address:</span></span> <br><span data-ttu-id="3757b-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alternativ</span><span class="sxs-lookup"><span data-stu-id="3757b-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="3757b-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="3757b-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="3757b-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-866">Sybase</span><span class="sxs-lookup"><span data-stu-id="3757b-866">Sybase</span></span></td>
      <td><span data-ttu-id="3757b-867">Behållare</span><span class="sxs-lookup"><span data-stu-id="3757b-867">Container</span></span></td>
      <td><span data-ttu-id="3757b-868">Databas</span><span class="sxs-lookup"><span data-stu-id="3757b-868">Database</span></span></td>
      <td><span data-ttu-id="3757b-869">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="3757b-869">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="3757b-870">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-870">authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-871">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-871">address:</span></span> <br><span data-ttu-id="3757b-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-874">Sybase</span><span class="sxs-lookup"><span data-stu-id="3757b-874">Sybase</span></span></td>
      <td><span data-ttu-id="3757b-875">Tabell</span><span class="sxs-lookup"><span data-stu-id="3757b-875">Table</span></span></td>
      <td><span data-ttu-id="3757b-876">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="3757b-876">Table, View</span></span></td>
      <td><span data-ttu-id="3757b-877">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="3757b-877">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="3757b-878">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="3757b-878">authentication: {basic, windows}</span></span> <br><span data-ttu-id="3757b-879">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-879">address:</span></span> <br><span data-ttu-id="3757b-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="3757b-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="3757b-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="3757b-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="3757b-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="3757b-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="3757b-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="3757b-884">Andra (ingen hälsningspaket ovan)</span><span class="sxs-lookup"><span data-stu-id="3757b-884">Other (none of hello above)</span></span></td>
      <td>\*</td>
      <td>\*</td>
      <td><span data-ttu-id="3757b-885">
        <font size=2>Protokoll: generisk tillgångar</span><span class="sxs-lookup"><span data-stu-id="3757b-885">
        <font size=2> Protocol: generic-asset</span></span> <br><span data-ttu-id="3757b-886">Adress:</span><span class="sxs-lookup"><span data-stu-id="3757b-886">Address:</span></span> <br><span data-ttu-id="3757b-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </span><span class="sxs-lookup"><span data-stu-id="3757b-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </span></span></td>
    </tr>
</table>
