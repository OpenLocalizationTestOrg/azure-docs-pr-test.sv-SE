---
title: "Datakällor som stöds i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln innehåller specifikationer av datakällor som för närvarande stöds."
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
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a><span data-ttu-id="7bd71-103">Datakällor som stöds i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7bd71-103">Supported data sources in Azure Data Catalog</span></span>

<span data-ttu-id="7bd71-104">Du kan publicera metadata med hjälp av en offentlig API eller klicka-registrering när verktyget eller web portal genom att manuellt ange information direkt till Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="7bd71-104">You can publish metadata by using a public API or a click-once registration tool, or by manually entering information directly to the Azure Data Catalog web portal.</span></span> <span data-ttu-id="7bd71-105">I följande tabell sammanfattas alla datakällor som stöds av katalogen idag och publishing funktionerna för var och en.</span><span class="sxs-lookup"><span data-stu-id="7bd71-105">The following table summarizes all data sources that are supported by the catalog today, and the publishing capabilities for each.</span></span> <span data-ttu-id="7bd71-106">Listan finns även externa Dataverktyg som varje datakälla kan starta från vår portal ”öppna i” erfarenhet.</span><span class="sxs-lookup"><span data-stu-id="7bd71-106">Also listed are the external data tools that each data source can launch from our portal "open-in" experience.</span></span> <span data-ttu-id="7bd71-107">Den andra tabellen innehåller en mer teknisk specifikation för varje datakälla Anslutningsegenskapen.</span><span class="sxs-lookup"><span data-stu-id="7bd71-107">The second table contains a more technical specification of each data-source connection property.</span></span>


## <a name="list-of-supported-data-sources"></a><span data-ttu-id="7bd71-108">Lista över datakällor som stöds</span><span class="sxs-lookup"><span data-stu-id="7bd71-108">List of supported data sources</span></span>

<table>
    <tr>
       <td><span data-ttu-id="7bd71-109"><b>Datakällobjektet</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-109"><b>Data source object</b></span></span></td>
       <td><span data-ttu-id="7bd71-110"><b>API</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-110"><b>API</b></span></span></td>
       <td><span data-ttu-id="7bd71-111"><b>Manuell inmatning</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-111"><b>Manual entry</b></span></span></td>
       <td><span data-ttu-id="7bd71-112"><b>Registreringsverktyget</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-112"><b>Registration tool</b></span></span></td>
       <td><span data-ttu-id="7bd71-113"><b>Öppna i Verktyg</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-113"><b>Open-in tools</b></span></span></td>
       <td><span data-ttu-id="7bd71-114"><b>Anteckningar</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-114"><b>Notes</b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-115">Azure Data Lake Store-katalogen</span><span class="sxs-lookup"><span data-stu-id="7bd71-115">Azure Data Lake Store directory</span></span></td>
      <td><span data-ttu-id="7bd71-116">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-116">✓</span></span></td>
      <td><span data-ttu-id="7bd71-117">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-117">✓</span></span></td>
      <td><span data-ttu-id="7bd71-118">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-118">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-119">Azure Data Lake Store-fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-119">Azure Data Lake Store file</span></span></td>
      <td><span data-ttu-id="7bd71-120">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-120">✓</span></span></td>
      <td><span data-ttu-id="7bd71-121">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-121">✓</span></span></td>
      <td><span data-ttu-id="7bd71-122">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-122">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-123">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="7bd71-123">Azure Blob storage</span></span></td>
      <td><span data-ttu-id="7bd71-124">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-124">✓</span></span></td>
      <td><span data-ttu-id="7bd71-125">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-125">✓</span></span></td>
      <td><span data-ttu-id="7bd71-126">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-126">✓</span></span></td>
      <td><span data-ttu-id="7bd71-127"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-127"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-128">Azure Storage-katalogen</span><span class="sxs-lookup"><span data-stu-id="7bd71-128">Azure Storage directory</span></span></td>
      <td><span data-ttu-id="7bd71-129">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-129">✓</span></span></td>
      <td><span data-ttu-id="7bd71-130">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-130">✓</span></span></td>
      <td><span data-ttu-id="7bd71-131">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-131">✓</span></span></td>
      <td><span data-ttu-id="7bd71-132"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-132"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-133">Azure Storage-tabellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-133">Azure Storage table</span></span></td>
      <td><span data-ttu-id="7bd71-134">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-134">✓</span></span></td>
      <td><span data-ttu-id="7bd71-135">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-135">✓</span></span></td>
      <td><span data-ttu-id="7bd71-136">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-136">✓</span></span></td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-137">HDFS-katalog</span><span class="sxs-lookup"><span data-stu-id="7bd71-137">HDFS directory</span></span></td>
      <td><span data-ttu-id="7bd71-138">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-138">✓</span></span></td>
      <td><span data-ttu-id="7bd71-139">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-139">✓</span></span></td>
      <td><span data-ttu-id="7bd71-140">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-140">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-141">HDFS-fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-141">HDFS file</span></span></td>
      <td><span data-ttu-id="7bd71-142">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-142">✓</span></span></td>
      <td><span data-ttu-id="7bd71-143">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-143">✓</span></span></td>
      <td><span data-ttu-id="7bd71-144">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-144">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-145">Hive-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-145">Hive table</span></span></td>
      <td><span data-ttu-id="7bd71-146">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-146">✓</span></span></td>
      <td><span data-ttu-id="7bd71-147">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-147">✓</span></span></td>
      <td><span data-ttu-id="7bd71-148">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-148">✓</span></span></td>
      <td><span data-ttu-id="7bd71-149"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-149"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-150">Hive-vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-150">Hive view</span></span></td>
      <td><span data-ttu-id="7bd71-151">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-151">✓</span></span></td>
      <td><span data-ttu-id="7bd71-152">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-152">✓</span></span></td>
      <td><span data-ttu-id="7bd71-153">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-153">✓</span></span></td>
      <td><span data-ttu-id="7bd71-154"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-154"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-155">MySQL-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-155">MySQL table</span></span></td>
      <td><span data-ttu-id="7bd71-156">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-156">✓</span></span></td>
      <td><span data-ttu-id="7bd71-157">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-157">✓</span></span></td>
      <td><span data-ttu-id="7bd71-158">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-158">✓</span></span></td>
      <td><span data-ttu-id="7bd71-159"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-159"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-160">MySQL-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-160">MySQL view</span></span></td>
      <td><span data-ttu-id="7bd71-161">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-161">✓</span></span></td>
      <td><span data-ttu-id="7bd71-162">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-162">✓</span></span></td>
      <td><span data-ttu-id="7bd71-163">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-163">✓</span></span></td>
      <td><span data-ttu-id="7bd71-164"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-164"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-165">Oracle Database-tabellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-165">Oracle Database table</span></span></td>
      <td><span data-ttu-id="7bd71-166">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-166">✓</span></span></td>
      <td><span data-ttu-id="7bd71-167">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-167">✓</span></span></td>
      <td><span data-ttu-id="7bd71-168">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-168">✓</span></span></td>
      <td><span data-ttu-id="7bd71-169"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-169"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-170">Oracle databasvy</span><span class="sxs-lookup"><span data-stu-id="7bd71-170">Oracle Database view</span></span></td>
      <td><span data-ttu-id="7bd71-171">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-171">✓</span></span></td>
      <td><span data-ttu-id="7bd71-172">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-172">✓</span></span></td>
      <td><span data-ttu-id="7bd71-173">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-173">✓</span></span></td>
      <td><span data-ttu-id="7bd71-174"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-174"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-175">Andra (allmän tillgång)</span><span class="sxs-lookup"><span data-stu-id="7bd71-175">Other (generic asset)</span></span></td>
      <td><span data-ttu-id="7bd71-176">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-176">✓</span></span></td>
      <td><span data-ttu-id="7bd71-177">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-177">✓</span></span></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-178">Azure SQL Data Warehouse-tabellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-178">Azure SQL Data Warehouse table</span></span></td>
      <td><span data-ttu-id="7bd71-179">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-179">✓</span></span></td>
      <td><span data-ttu-id="7bd71-180">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-180">✓</span></span></td>
      <td><span data-ttu-id="7bd71-181">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-181">✓</span></span></td>
      <td><span data-ttu-id="7bd71-182"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-182"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-183">SQL Data Warehouse-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-183">SQL Data Warehouse view</span></span></td>
      <td><span data-ttu-id="7bd71-184">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-184">✓</span></span></td>
      <td><span data-ttu-id="7bd71-185">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-185">✓</span></span></td>
      <td><span data-ttu-id="7bd71-186">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-186">✓</span></span></td>
      <td><span data-ttu-id="7bd71-187"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-187"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-188">SQL Server Analysis Services-dimension</span><span class="sxs-lookup"><span data-stu-id="7bd71-188">SQL Server Analysis Services dimension</span></span></td>
      <td><span data-ttu-id="7bd71-189">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-189">✓</span></span></td>
      <td><span data-ttu-id="7bd71-190">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-190">✓</span></span></td>
      <td><span data-ttu-id="7bd71-191">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-191">✓</span></span></td>
      <td><span data-ttu-id="7bd71-192"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-192"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-193">SQL Server Analysis Services KPI</span><span class="sxs-lookup"><span data-stu-id="7bd71-193">SQL Server Analysis Services KPI</span></span></td>
      <td><span data-ttu-id="7bd71-194">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-194">✓</span></span></td>
      <td><span data-ttu-id="7bd71-195">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-195">✓</span></span></td>
      <td><span data-ttu-id="7bd71-196">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-196">✓</span></span></td>
      <td><span data-ttu-id="7bd71-197"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-197"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-198">SQL Server Analysis Services-mått</span><span class="sxs-lookup"><span data-stu-id="7bd71-198">SQL Server Analysis Services measure</span></span></td>
      <td><span data-ttu-id="7bd71-199">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-199">✓</span></span></td>
      <td><span data-ttu-id="7bd71-200">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-200">✓</span></span></td>
      <td><span data-ttu-id="7bd71-201">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-201">✓</span></span></td>
      <td><span data-ttu-id="7bd71-202"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-202"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-203">SQL Server Analysis Services-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-203">SQL Server Analysis Services table</span></span></td>
      <td><span data-ttu-id="7bd71-204">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-204">✓</span></span></td>
      <td><span data-ttu-id="7bd71-205">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-205">✓</span></span></td>
      <td><span data-ttu-id="7bd71-206">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-206">✓</span></span></td>
      <td><span data-ttu-id="7bd71-207"><font size=2>Excel, Powerbi</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-207"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-208">SQL Server Reporting Services-rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-208">SQL Server Reporting Services report</span></span></td>
      <td><span data-ttu-id="7bd71-209">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-209">✓</span></span></td>
      <td><span data-ttu-id="7bd71-210">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-210">✓</span></span></td>
      <td><span data-ttu-id="7bd71-211">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-211">✓</span></span></td>
      <td><span data-ttu-id="7bd71-212"><font size=2>Webbläsare</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-212"><font size=2>Browser</font></span></span></td>
      <td><span data-ttu-id="7bd71-213"><font size=2>Enhetligt läge-servrar. SharePoint-läge stöds inte.</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-213"><font size=2>Native mode servers only. SharePoint mode is not supported.</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-214">SQL Server-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-214">SQL Server table</span></span></td>
      <td><span data-ttu-id="7bd71-215">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-215">✓</span></span></td>
      <td><span data-ttu-id="7bd71-216">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-216">✓</span></span></td>
      <td><span data-ttu-id="7bd71-217">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-217">✓</span></span></td>
      <td><span data-ttu-id="7bd71-218"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-218"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-219">SQL Server-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-219">SQL Server view</span></span></td>
      <td><span data-ttu-id="7bd71-220">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-220">✓</span></span></td>
      <td><span data-ttu-id="7bd71-221">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-221">✓</span></span></td>
      <td><span data-ttu-id="7bd71-222">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-222">✓</span></span></td>
      <td><span data-ttu-id="7bd71-223"><font size=2>Excel, Power BI kan SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-223"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-224">Tabell för Teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-224">Teradata table</span></span></td>
      <td><span data-ttu-id="7bd71-225">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-225">✓</span></span></td>
      <td><span data-ttu-id="7bd71-226">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-226">✓</span></span></td>
      <td><span data-ttu-id="7bd71-227">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-227">✓</span></span></td>
      <td><span data-ttu-id="7bd71-228"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-228"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-229">Visa för Teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-229">Teradata view</span></span></td>
      <td><span data-ttu-id="7bd71-230">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-230">✓</span></span></td>
      <td><span data-ttu-id="7bd71-231">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-231">✓</span></span></td>
      <td><span data-ttu-id="7bd71-232">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-232">✓</span></span></td>
      <td><span data-ttu-id="7bd71-233"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-233"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-234">SAP HANA-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-234">SAP HANA view</span></span></td>
      <td><span data-ttu-id="7bd71-235">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-235">✓</span></span></td>
      <td><span data-ttu-id="7bd71-236">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-236">✓</span></span></td>
      <td><span data-ttu-id="7bd71-237">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-237">✓</span></span></td>
      <td><span data-ttu-id="7bd71-238"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-238"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-239">DB2-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-239">DB2 table</span></span></td>
      <td><span data-ttu-id="7bd71-240">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-240">✓</span></span></td>
      <td><span data-ttu-id="7bd71-241">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-241">✓</span></span></td>
      <td><span data-ttu-id="7bd71-242">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-242">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-243">DB2-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-243">DB2 view</span></span></td>
      <td><span data-ttu-id="7bd71-244">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-244">✓</span></span></td>
      <td><span data-ttu-id="7bd71-245">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-245">✓</span></span></td>
      <td><span data-ttu-id="7bd71-246">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-246">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-247">Fil för system</span><span class="sxs-lookup"><span data-stu-id="7bd71-247">File system file</span></span></td>
      <td><span data-ttu-id="7bd71-248">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-248">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-249">FTP-katalog</span><span class="sxs-lookup"><span data-stu-id="7bd71-249">FTP directory</span></span></td>
      <td><span data-ttu-id="7bd71-250">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-250">✓</span></span></td>
      <td><span data-ttu-id="7bd71-251">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-251">✓</span></span></td>
      <td><span data-ttu-id="7bd71-252">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-252">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-253">FTP-fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-253">FTP file</span></span></td>
      <td><span data-ttu-id="7bd71-254">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-254">✓</span></span></td>
      <td><span data-ttu-id="7bd71-255">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-255">✓</span></span></td>
      <td><span data-ttu-id="7bd71-256">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-256">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-257">HTTP-rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-257">HTTP report</span></span></td>
      <td><span data-ttu-id="7bd71-258">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-258">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-259">HTTP-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7bd71-259">HTTP endpoint</span></span></td>
      <td><span data-ttu-id="7bd71-260">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-260">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-261">HTTP-fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-261">HTTP file</span></span></td>
      <td><span data-ttu-id="7bd71-262">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-262">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-263">OData-entitetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="7bd71-263">OData entity set</span></span></td>
      <td><span data-ttu-id="7bd71-264">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-264">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-265">OData-funktion</span><span class="sxs-lookup"><span data-stu-id="7bd71-265">OData function</span></span></td>
      <td><span data-ttu-id="7bd71-266">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-266">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-267">PostgreSQL tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-267">PostgreSQL table</span></span></td>
      <td><span data-ttu-id="7bd71-268">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-268">✓</span></span></td>
      <td><span data-ttu-id="7bd71-269">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-269">✓</span></span></td>
      <td><span data-ttu-id="7bd71-270">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-270">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-271">PostgreSQL-vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-271">PostgreSQL view</span></span></td>
      <td><span data-ttu-id="7bd71-272">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-272">✓</span></span></td>
      <td><span data-ttu-id="7bd71-273">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-273">✓</span></span></td>
      <td><span data-ttu-id="7bd71-274">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-274">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-275">SAP HANA-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-275">SAP HANA view</span></span></td>
      <td><span data-ttu-id="7bd71-276">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-276">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> <span data-ttu-id="7bd71-277">Salesforce-objekt</span><span class="sxs-lookup"><span data-stu-id="7bd71-277">Salesforce object</span></span></td>
      <td><span data-ttu-id="7bd71-278">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-278">✓</span></span></td>
      <td><span data-ttu-id="7bd71-279">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-279">✓</span></span></td>
      <td><span data-ttu-id="7bd71-280">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-280">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-281">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="7bd71-281">SharePoint list</span></span> </td>
      <td><span data-ttu-id="7bd71-282">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-282">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-283">Azure DB Cosmos-samling</span><span class="sxs-lookup"><span data-stu-id="7bd71-283">Azure Cosmos DB collection</span></span></td>
      <td><span data-ttu-id="7bd71-284">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-284">✓</span></span></td>
      <td><span data-ttu-id="7bd71-285">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-285">✓</span></span></td>
      <td><span data-ttu-id="7bd71-286">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-286">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-287">Allmän ODBC-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-287">Generic ODBC table</span></span></td>
      <td><span data-ttu-id="7bd71-288">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-288">✓</span></span></td>
      <td><span data-ttu-id="7bd71-289">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-289">✓</span></span></td>
      <td><span data-ttu-id="7bd71-290">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-290">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-291">Allmän ODBC-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-291">Generic ODBC view</span></span></td>
      <td><span data-ttu-id="7bd71-292">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-292">✓</span></span></td>
      <td><span data-ttu-id="7bd71-293">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-293">✓</span></span></td>
      <td><span data-ttu-id="7bd71-294">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-294">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-295">Cassandra tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-295">Cassandra table</span></span></td>
      <td><span data-ttu-id="7bd71-296">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-296">✓</span></span></td>
      <td><span data-ttu-id="7bd71-297">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-297">✓</span></span></td>
      <td><span data-ttu-id="7bd71-298">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-298">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="7bd71-299"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-299"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-300">Cassandra vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-300">Cassandra view</span></span></td>
      <td><span data-ttu-id="7bd71-301">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-301">✓</span></span></td>
      <td><span data-ttu-id="7bd71-302">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-302">✓</span></span></td>
      <td><span data-ttu-id="7bd71-303">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-303">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="7bd71-304"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-304"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-305">Sybase-tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-305">Sybase table</span></span></td>
      <td><span data-ttu-id="7bd71-306">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-306">✓</span></span></td>
      <td><span data-ttu-id="7bd71-307">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-307">✓</span></span></td>
      <td><span data-ttu-id="7bd71-308">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-308">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-309">Sybase-vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-309">Sybase view</span></span></td>
      <td><span data-ttu-id="7bd71-310">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-310">✓</span></span></td>
      <td><span data-ttu-id="7bd71-311">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-311">✓</span></span></td>
      <td><span data-ttu-id="7bd71-312">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-312">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-313">Tabell för MongoDB</span><span class="sxs-lookup"><span data-stu-id="7bd71-313">MongoDB table</span></span></td>
      <td><span data-ttu-id="7bd71-314">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-314">✓</span></span></td>
      <td><span data-ttu-id="7bd71-315">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-315">✓</span></span></td>
      <td><span data-ttu-id="7bd71-316">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-316">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="7bd71-317"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-317"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-318">Visa MongoDB</span><span class="sxs-lookup"><span data-stu-id="7bd71-318">MongoDB view</span></span></td>
      <td><span data-ttu-id="7bd71-319">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-319">✓</span></span></td>
      <td><span data-ttu-id="7bd71-320">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-320">✓</span></span></td>
      <td><span data-ttu-id="7bd71-321">✓</span><span class="sxs-lookup"><span data-stu-id="7bd71-321">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="7bd71-322"><font size=2>Publicera som en allmän ODBC-tillgång</font></span><span class="sxs-lookup"><span data-stu-id="7bd71-322"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
</table>

<span data-ttu-id="7bd71-323">Om du behöver stöd för ytterligare källor kan skicka en funktionsbegäran till den [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="7bd71-323">If you need support for additional sources, submit a feature request to the [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span></span>


## <a name="data-source-reference-specification"></a><span data-ttu-id="7bd71-324">Referens-specifikationen för datakälla</span><span class="sxs-lookup"><span data-stu-id="7bd71-324">Data-source reference specification</span></span>
> [!NOTE]
> <span data-ttu-id="7bd71-325">Den **DSL-strukturen** kolumnen i tabellen nedan visas endast de anslutningsegenskaper ”adress” egenskapsuppsättningen som används av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="7bd71-325">The **DSL structure** column in the following table lists only the connection properties for "address" property bag that are used by Azure Data Catalog.</span></span> <span data-ttu-id="7bd71-326">Det vill säga kan ”adress” egenskapsuppsättning innehålla andra anslutningsegenskaper för datakällan som Azure Data Catalog kvarstår, men använder inte.</span><span class="sxs-lookup"><span data-stu-id="7bd71-326">That is, "address" property bag can contain other connection properties of the data source which Azure Data Catalog persists, but does not use.</span></span>

<table>
    <tr>
       <td><span data-ttu-id="7bd71-327"><b>Typ av datakälla</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-327"><b>Source type</b></span></span></td>
       <td><span data-ttu-id="7bd71-328"><b>Tillgångstypen</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-328"><b>Asset type</b></span></span></td>
       <td><span data-ttu-id="7bd71-329"><b>Objekttyper</b></span><span class="sxs-lookup"><span data-stu-id="7bd71-329"><b>Object types</b></span></span></td>
       <td><span data-ttu-id="7bd71-330"><b>DSL-struktur<b></span><span class="sxs-lookup"><span data-stu-id="7bd71-330"><b>DSL structure<b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-331">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7bd71-331">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="7bd71-332">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-332">Container</span></span></td>
      <td><span data-ttu-id="7bd71-333">Data Lake</span><span class="sxs-lookup"><span data-stu-id="7bd71-333">Data Lake</span></span></td>
      <td><span data-ttu-id="7bd71-334">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="7bd71-334">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="7bd71-335">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-335">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="7bd71-336">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-336">Address:</span></span> <br><span data-ttu-id="7bd71-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-338">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7bd71-338">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="7bd71-339">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-339">Table</span></span></td>
      <td><span data-ttu-id="7bd71-340">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-340">Directory, file</span></span></td>
      <td><span data-ttu-id="7bd71-341">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="7bd71-341">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="7bd71-342">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-342">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="7bd71-343">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-343">Address:</span></span> <br><span data-ttu-id="7bd71-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-345">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7bd71-345">Azure Storage</span></span></td>
      <td><span data-ttu-id="7bd71-346">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-346">Container</span></span></td>
      <td><span data-ttu-id="7bd71-347">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-347">Container</span></span></td>
      <td><span data-ttu-id="7bd71-348">
        <font size=2>Protokoll: azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="7bd71-348">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="7bd71-349">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-349">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-350">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-350">Address:</span></span> <br><span data-ttu-id="7bd71-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="7bd71-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="7bd71-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="7bd71-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="7bd71-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;behållaren</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-354">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7bd71-354">Azure Storage</span></span></td>
      <td><span data-ttu-id="7bd71-355">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-355">Table</span></span></td>
      <td><span data-ttu-id="7bd71-356">BLOB, directory</span><span class="sxs-lookup"><span data-stu-id="7bd71-356">Blob, directory</span></span></td>
      <td><span data-ttu-id="7bd71-357">
        <font size=2>Protokoll: azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="7bd71-357">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="7bd71-358">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-358">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-359">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-359">Address:</span></span> <br><span data-ttu-id="7bd71-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="7bd71-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="7bd71-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="7bd71-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="7bd71-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;behållaren</span><span class="sxs-lookup"><span data-stu-id="7bd71-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container</span></span> <br><span data-ttu-id="7bd71-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namn</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-364">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7bd71-364">Azure Storage</span></span></td>
      <td><span data-ttu-id="7bd71-365">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-365">Container</span></span></td>
      <td><span data-ttu-id="7bd71-366">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-366">Container</span></span></td>
      <td><span data-ttu-id="7bd71-367">
        <font size=2>Protokoll: azure-tabeller</span><span class="sxs-lookup"><span data-stu-id="7bd71-367">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="7bd71-368">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-368">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-369">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-369">Address:</span></span> <br><span data-ttu-id="7bd71-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="7bd71-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="7bd71-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-372">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7bd71-372">Azure Storage</span></span></td>
      <td><span data-ttu-id="7bd71-373">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-373">Table</span></span></td>
      <td><span data-ttu-id="7bd71-374">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-374">Table</span></span></td>
      <td><span data-ttu-id="7bd71-375">
        <font size=2>Protokoll: azure-tabeller</span><span class="sxs-lookup"><span data-stu-id="7bd71-375">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="7bd71-376">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-376">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-377">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-377">Address:</span></span> <br><span data-ttu-id="7bd71-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domän</span><span class="sxs-lookup"><span data-stu-id="7bd71-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="7bd71-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</span><span class="sxs-lookup"><span data-stu-id="7bd71-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="7bd71-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namn</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-381">Cosmos</span><span class="sxs-lookup"><span data-stu-id="7bd71-381">Cosmos</span></span></td>
      <td><span data-ttu-id="7bd71-382">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-382">Container</span></span></td>
      <td><span data-ttu-id="7bd71-383">Virtuellt kluster</span><span class="sxs-lookup"><span data-stu-id="7bd71-383">Virtual cluster</span></span></td>
      <td><span data-ttu-id="7bd71-384">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="7bd71-384">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="7bd71-385">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-385">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-386">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-386">Address:</span></span> <br><span data-ttu-id="7bd71-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-388">Cosmos</span><span class="sxs-lookup"><span data-stu-id="7bd71-388">Cosmos</span></span></td>
      <td><span data-ttu-id="7bd71-389">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-389">Table</span></span></td>
      <td><span data-ttu-id="7bd71-390">Stream, stream set vy</span><span class="sxs-lookup"><span data-stu-id="7bd71-390">Stream, stream set, view</span></span></td>
      <td><span data-ttu-id="7bd71-391">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="7bd71-391">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="7bd71-392">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-392">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-393">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-393">Address:</span></span> <br><span data-ttu-id="7bd71-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-395">Datazen</span><span class="sxs-lookup"><span data-stu-id="7bd71-395">Datazen</span></span></td>
      <td><span data-ttu-id="7bd71-396">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-396">Container</span></span></td>
      <td><span data-ttu-id="7bd71-397">plats</span><span class="sxs-lookup"><span data-stu-id="7bd71-397">Site</span></span></td>
      <td><span data-ttu-id="7bd71-398">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-398">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-399">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-399">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-400">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-400">Address:</span></span> <br><span data-ttu-id="7bd71-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-402">Datazen</span><span class="sxs-lookup"><span data-stu-id="7bd71-402">Datazen</span></span></td>
      <td><span data-ttu-id="7bd71-403">Rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-403">Report</span></span></td>
      <td><span data-ttu-id="7bd71-404">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7bd71-404">Report, dashboard</span></span></td>
      <td><span data-ttu-id="7bd71-405">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-405">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-406">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-406">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-407">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-407">Address:</span></span> <br><span data-ttu-id="7bd71-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-409">DB2</span><span class="sxs-lookup"><span data-stu-id="7bd71-409">DB2</span></span></td>
      <td><span data-ttu-id="7bd71-410">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-410">Container</span></span></td>
      <td><span data-ttu-id="7bd71-411">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-411">Database</span></span></td>
      <td><span data-ttu-id="7bd71-412">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="7bd71-412">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="7bd71-413">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-413">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-414">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-414">Address:</span></span> <br><span data-ttu-id="7bd71-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-417">DB2</span><span class="sxs-lookup"><span data-stu-id="7bd71-417">DB2</span></span></td>
      <td><span data-ttu-id="7bd71-418">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-418">Table</span></span></td>
      <td><span data-ttu-id="7bd71-419">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-419">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-420">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="7bd71-420">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="7bd71-421">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-421">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-422">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-422">Address:</span></span> <br><span data-ttu-id="7bd71-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-427">Filsystem</span><span class="sxs-lookup"><span data-stu-id="7bd71-427">File system</span></span></td>
      <td><span data-ttu-id="7bd71-428">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-428">Table</span></span></td>
      <td><span data-ttu-id="7bd71-429">Fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-429">File</span></span></td>
      <td><span data-ttu-id="7bd71-430">
        <font size=2>Protokoll: filen</span><span class="sxs-lookup"><span data-stu-id="7bd71-430">
        <font size=2> Protocol: file</span></span> <br><span data-ttu-id="7bd71-431">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-431">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="7bd71-432">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-432">Address:</span></span> <br><span data-ttu-id="7bd71-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sökväg</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-434">FTP</span><span class="sxs-lookup"><span data-stu-id="7bd71-434">FTP</span></span></td>
      <td><span data-ttu-id="7bd71-435">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-435">Table</span></span></td>
      <td><span data-ttu-id="7bd71-436">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-436">Directory, file</span></span></td>
      <td><span data-ttu-id="7bd71-437">
        <font size=2>Protokoll: ftp</span><span class="sxs-lookup"><span data-stu-id="7bd71-437">
        <font size=2> Protocol: ftp</span></span> <br><span data-ttu-id="7bd71-438">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-438">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="7bd71-439">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-439">Address:</span></span> <br><span data-ttu-id="7bd71-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-441">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="7bd71-441">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="7bd71-442">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-442">Container</span></span></td>
      <td><span data-ttu-id="7bd71-443">Kluster</span><span class="sxs-lookup"><span data-stu-id="7bd71-443">Cluster</span></span></td>
      <td><span data-ttu-id="7bd71-444">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="7bd71-444">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="7bd71-445">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-445">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="7bd71-446">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-446">Address:</span></span> <br><span data-ttu-id="7bd71-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-448">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="7bd71-448">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="7bd71-449">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-449">Table</span></span></td>
      <td><span data-ttu-id="7bd71-450">Katalogen fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-450">Directory, file</span></span></td>
      <td><span data-ttu-id="7bd71-451">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="7bd71-451">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="7bd71-452">Autentisering: {basic, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-452">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="7bd71-453">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-453">Address:</span></span> <br><span data-ttu-id="7bd71-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-455">Hive</span><span class="sxs-lookup"><span data-stu-id="7bd71-455">Hive</span></span></td>
      <td><span data-ttu-id="7bd71-456">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-456">Container</span></span></td>
      <td><span data-ttu-id="7bd71-457">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-457">Database</span></span></td>
      <td><span data-ttu-id="7bd71-458">
        <font size=2>Protokoll: hive</span><span class="sxs-lookup"><span data-stu-id="7bd71-458">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="7bd71-459">Autentisering: {HDInsight, basic, username, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-459">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="7bd71-460">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-460">Address:</span></span> <br><span data-ttu-id="7bd71-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-463">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="7bd71-463">connectionProperties:</span></span> <br><span data-ttu-id="7bd71-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-465">Hive</span><span class="sxs-lookup"><span data-stu-id="7bd71-465">Hive</span></span></td>
      <td><span data-ttu-id="7bd71-466">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-466">Table</span></span></td>
      <td><span data-ttu-id="7bd71-467">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-467">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-468">
        <font size=2>Protokoll: hive</span><span class="sxs-lookup"><span data-stu-id="7bd71-468">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="7bd71-469">Autentisering: {HDInsight, basic, username, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-469">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="7bd71-470">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-470">Address:</span></span> <br><span data-ttu-id="7bd71-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-474">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="7bd71-474">connectionProperties:</span></span> <br><span data-ttu-id="7bd71-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-476">HTTP</span><span class="sxs-lookup"><span data-stu-id="7bd71-476">HTTP</span></span></td>
      <td><span data-ttu-id="7bd71-477">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-477">Container</span></span></td>
      <td><span data-ttu-id="7bd71-478">plats</span><span class="sxs-lookup"><span data-stu-id="7bd71-478">Site</span></span></td>
      <td><span data-ttu-id="7bd71-479">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-479">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-480">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-480">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-481">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-481">Address:</span></span> <br><span data-ttu-id="7bd71-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-483">HTTP</span><span class="sxs-lookup"><span data-stu-id="7bd71-483">HTTP</span></span></td>
      <td><span data-ttu-id="7bd71-484">Rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-484">Report</span></span></td>
      <td><span data-ttu-id="7bd71-485">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7bd71-485">Report, dashboard</span></span></td>
      <td><span data-ttu-id="7bd71-486">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-486">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-487">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-487">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-488">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-488">Address:</span></span> <br><span data-ttu-id="7bd71-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-490">HTTP</span><span class="sxs-lookup"><span data-stu-id="7bd71-490">HTTP</span></span></td>
      <td><span data-ttu-id="7bd71-491">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-491">Table</span></span></td>
      <td><span data-ttu-id="7bd71-492">Slutpunkt, fil</span><span class="sxs-lookup"><span data-stu-id="7bd71-492">Endpoint, file</span></span></td>
      <td><span data-ttu-id="7bd71-493">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-493">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-494">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-494">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-495">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-495">Address:</span></span> <br><span data-ttu-id="7bd71-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-497">MySQL</span><span class="sxs-lookup"><span data-stu-id="7bd71-497">MySQL</span></span></td>
      <td><span data-ttu-id="7bd71-498">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-498">Container</span></span></td>
      <td><span data-ttu-id="7bd71-499">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-499">Database</span></span></td>
      <td><span data-ttu-id="7bd71-500">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="7bd71-500">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="7bd71-501">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-501">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-502">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-502">Address:</span></span> <br><span data-ttu-id="7bd71-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-505">MySQL</span><span class="sxs-lookup"><span data-stu-id="7bd71-505">MySQL</span></span></td>
      <td><span data-ttu-id="7bd71-506">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-506">Table</span></span></td>
      <td><span data-ttu-id="7bd71-507">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-507">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-508">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="7bd71-508">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="7bd71-509">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-509">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-510">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-510">Address:</span></span> <br><span data-ttu-id="7bd71-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-514">OData</span><span class="sxs-lookup"><span data-stu-id="7bd71-514">OData</span></span></td>
      <td><span data-ttu-id="7bd71-515">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-515">Container</span></span></td>
      <td><span data-ttu-id="7bd71-516">Entitetsbehållaren</span><span class="sxs-lookup"><span data-stu-id="7bd71-516">Entity container</span></span></td>
      <td><span data-ttu-id="7bd71-517">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="7bd71-517">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="7bd71-518">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-518">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="7bd71-519">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-519">Address:</span></span> <br><span data-ttu-id="7bd71-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-521">OData</span><span class="sxs-lookup"><span data-stu-id="7bd71-521">OData</span></span></td>
      <td><span data-ttu-id="7bd71-522">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-522">Table</span></span></td>
      <td><span data-ttu-id="7bd71-523">Entitetsuppsättningen funktion</span><span class="sxs-lookup"><span data-stu-id="7bd71-523">Entity set, function</span></span></td>
      <td><span data-ttu-id="7bd71-524">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="7bd71-524">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="7bd71-525">Autentisering: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-525">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="7bd71-526">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-526">Address:</span></span> <br><span data-ttu-id="7bd71-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="7bd71-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="7bd71-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resursen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-529">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-529">Oracle Database</span></span></td>
      <td><span data-ttu-id="7bd71-530">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-530">Container</span></span></td>
      <td><span data-ttu-id="7bd71-531">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-531">Database</span></span></td>
      <td><span data-ttu-id="7bd71-532">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="7bd71-532">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="7bd71-533">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-533">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-534">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-534">Address:</span></span> <br><span data-ttu-id="7bd71-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-537">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-537">Oracle Database</span></span></td>
      <td><span data-ttu-id="7bd71-538">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-538">Table</span></span></td>
      <td><span data-ttu-id="7bd71-539">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-539">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-540">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="7bd71-540">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="7bd71-541">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-541">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-542">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-542">Address:</span></span> <br><span data-ttu-id="7bd71-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-547">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="7bd71-547">PostgreSQL</span></span></td>
      <td><span data-ttu-id="7bd71-548">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-548">Container</span></span></td>
      <td><span data-ttu-id="7bd71-549">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-549">Database</span></span></td>
      <td><span data-ttu-id="7bd71-550">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="7bd71-550">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="7bd71-551">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-551">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-552">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-552">Address:</span></span> <br><span data-ttu-id="7bd71-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-555">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="7bd71-555">PostgreSQL</span></span></td>
      <td><span data-ttu-id="7bd71-556">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-556">Table</span></span></td>
      <td><span data-ttu-id="7bd71-557">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-557">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-558">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="7bd71-558">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="7bd71-559">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-559">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-560">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-560">Address:</span></span> <br><span data-ttu-id="7bd71-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-565">Power BI</span><span class="sxs-lookup"><span data-stu-id="7bd71-565">Power BI</span></span></td>
      <td><span data-ttu-id="7bd71-566">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-566">Container</span></span></td>
      <td><span data-ttu-id="7bd71-567">plats</span><span class="sxs-lookup"><span data-stu-id="7bd71-567">Site</span></span></td>
      <td><span data-ttu-id="7bd71-568">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-568">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-569">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-569">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-570">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-570">Address:</span></span> <br><span data-ttu-id="7bd71-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-572">Power BI</span><span class="sxs-lookup"><span data-stu-id="7bd71-572">Power BI</span></span></td>
      <td><span data-ttu-id="7bd71-573">Rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-573">Report</span></span></td>
      <td><span data-ttu-id="7bd71-574">Rapporten, instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7bd71-574">Report, dashboard</span></span></td>
      <td><span data-ttu-id="7bd71-575">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="7bd71-575">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="7bd71-576">Autentisering: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-576">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="7bd71-577">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-577">Address:</span></span> <br><span data-ttu-id="7bd71-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-579">Power Query</span><span class="sxs-lookup"><span data-stu-id="7bd71-579">Power Query</span></span></td>
      <td><span data-ttu-id="7bd71-580">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-580">Table</span></span></td>
      <td><span data-ttu-id="7bd71-581">Data mashup</span><span class="sxs-lookup"><span data-stu-id="7bd71-581">Data mashup</span></span></td>
      <td><span data-ttu-id="7bd71-582">
        <font size=2>Protokoll: power-fråga</span><span class="sxs-lookup"><span data-stu-id="7bd71-582">
        <font size=2> Protocol: power-query</span></span> <br><span data-ttu-id="7bd71-583">Autentisering: {oauth}</span><span class="sxs-lookup"><span data-stu-id="7bd71-583">Authentication: {oauth}</span></span> <br><span data-ttu-id="7bd71-584">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-584">Address:</span></span> <br><span data-ttu-id="7bd71-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-586">Salesforce</span><span class="sxs-lookup"><span data-stu-id="7bd71-586">Salesforce</span></span></td>
      <td><span data-ttu-id="7bd71-587">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-587">Table</span></span></td>
      <td><span data-ttu-id="7bd71-588">Objekt</span><span class="sxs-lookup"><span data-stu-id="7bd71-588">Object</span></span></td>
      <td><span data-ttu-id="7bd71-589">
        <font size=2>Protokoll: salesforce-com</span><span class="sxs-lookup"><span data-stu-id="7bd71-589">
        <font size=2> Protocol: salesforce-com</span></span> <br><span data-ttu-id="7bd71-590">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-590">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-591">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-591">Address:</span></span> <br><span data-ttu-id="7bd71-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer</span><span class="sxs-lookup"><span data-stu-id="7bd71-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer</span></span> <br><span data-ttu-id="7bd71-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klass</span><span class="sxs-lookup"><span data-stu-id="7bd71-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class</span></span> <br><span data-ttu-id="7bd71-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-595">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="7bd71-595">SAP HANA</span></span></td>
      <td><span data-ttu-id="7bd71-596">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-596">Container</span></span></td>
      <td><span data-ttu-id="7bd71-597">Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-597">Server</span></span></td>
      <td><span data-ttu-id="7bd71-598">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="7bd71-598">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="7bd71-599">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-599">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-600">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-600">Address:</span></span> <br><span data-ttu-id="7bd71-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-602">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="7bd71-602">SAP HANA</span></span></td>
      <td><span data-ttu-id="7bd71-603">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-603">Table</span></span></td>
      <td><span data-ttu-id="7bd71-604">Visa</span><span class="sxs-lookup"><span data-stu-id="7bd71-604">View</span></span></td>
      <td><span data-ttu-id="7bd71-605">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="7bd71-605">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="7bd71-606">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-606">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-607">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-607">Address:</span></span> <br><span data-ttu-id="7bd71-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-611">SharePoint</span><span class="sxs-lookup"><span data-stu-id="7bd71-611">SharePoint</span></span></td>
      <td><span data-ttu-id="7bd71-612">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-612">Table</span></span></td>
      <td><span data-ttu-id="7bd71-613">Visa lista</span><span class="sxs-lookup"><span data-stu-id="7bd71-613">List</span></span></td>
      <td><span data-ttu-id="7bd71-614">
        <font size=2>Protokoll: sharepoint-lista över</span><span class="sxs-lookup"><span data-stu-id="7bd71-614">
        <font size=2> Protocol: sharepoint-list</span></span> <br><span data-ttu-id="7bd71-615">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-615">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-616">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-616">Address:</span></span> <br><span data-ttu-id="7bd71-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-618">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7bd71-618">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="7bd71-619">Kommando</span><span class="sxs-lookup"><span data-stu-id="7bd71-619">Command</span></span></td>
      <td><span data-ttu-id="7bd71-620">Lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="7bd71-620">Stored procedure</span></span></td>
      <td><span data-ttu-id="7bd71-621">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-621">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-622">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-622">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-623">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-623">Address:</span></span> <br><span data-ttu-id="7bd71-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-628">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7bd71-628">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="7bd71-629">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="7bd71-629">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="7bd71-630">Tabellvärdesfunktion</span><span class="sxs-lookup"><span data-stu-id="7bd71-630">Table-valued function</span></span></td>
      <td><span data-ttu-id="7bd71-631">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-631">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-632">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-632">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-633">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-633">Address:</span></span> <br><span data-ttu-id="7bd71-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-638">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7bd71-638">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="7bd71-639">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-639">Container</span></span></td>
      <td><span data-ttu-id="7bd71-640">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-640">Database</span></span></td>
      <td><span data-ttu-id="7bd71-641">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-641">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-642">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-642">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-643">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-643">Address:</span></span> <br><span data-ttu-id="7bd71-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-646">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7bd71-646">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="7bd71-647">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-647">Table</span></span></td>
      <td><span data-ttu-id="7bd71-648">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-648">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-649">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-649">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-650">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-650">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-651">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-651">Address:</span></span> <br><span data-ttu-id="7bd71-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-656">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-656">SQL Server</span></span></td>
      <td><span data-ttu-id="7bd71-657">Kommando</span><span class="sxs-lookup"><span data-stu-id="7bd71-657">Command</span></span></td>
      <td><span data-ttu-id="7bd71-658">Lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="7bd71-658">Stored procedure</span></span></td>
      <td><span data-ttu-id="7bd71-659">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-659">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-660">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-660">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-661">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-661">Address:</span></span> <br><span data-ttu-id="7bd71-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-666">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-666">SQL Server</span></span></td>
      <td><span data-ttu-id="7bd71-667">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="7bd71-667">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="7bd71-668">Tabellvärdesfunktion</span><span class="sxs-lookup"><span data-stu-id="7bd71-668">Table-valued function</span></span></td>
      <td><span data-ttu-id="7bd71-669">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-669">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-670">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-670">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-671">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-671">Address:</span></span> <br><span data-ttu-id="7bd71-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-676">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-676">SQL Server</span></span></td>
      <td><span data-ttu-id="7bd71-677">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-677">Container</span></span></td>
      <td><span data-ttu-id="7bd71-678">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-678">Database</span></span></td>
      <td><span data-ttu-id="7bd71-679">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-679">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-680">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-680">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-681">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-681">Address:</span></span> <br><span data-ttu-id="7bd71-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-684">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-684">SQL Server</span></span></td>
      <td><span data-ttu-id="7bd71-685">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-685">Table</span></span></td>
      <td><span data-ttu-id="7bd71-686">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-686">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-687">
        <font size=2>Protokoll: tds</span><span class="sxs-lookup"><span data-stu-id="7bd71-687">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="7bd71-688">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-688">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-689">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-689">Address:</span></span> <br><span data-ttu-id="7bd71-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-694">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="7bd71-694">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="7bd71-695">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-695">Container</span></span></td>
      <td><span data-ttu-id="7bd71-696">Modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-696">Model</span></span></td>
      <td><span data-ttu-id="7bd71-697">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-697">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-698">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-698">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-699">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-699">Address:</span></span> <br><span data-ttu-id="7bd71-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-703">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="7bd71-703">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="7bd71-704">KPI: N</span><span class="sxs-lookup"><span data-stu-id="7bd71-704">KPI</span></span></td>
      <td><span data-ttu-id="7bd71-705">KPI: N</span><span class="sxs-lookup"><span data-stu-id="7bd71-705">KPI</span></span></td>
      <td><span data-ttu-id="7bd71-706">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-706">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-707">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-707">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-708">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-708">Address:</span></span> <br><span data-ttu-id="7bd71-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-714">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="7bd71-714">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="7bd71-715">Mått</span><span class="sxs-lookup"><span data-stu-id="7bd71-715">Measure</span></span></td>
      <td><span data-ttu-id="7bd71-716">Mått</span><span class="sxs-lookup"><span data-stu-id="7bd71-716">Measure</span></span></td>
      <td><span data-ttu-id="7bd71-717">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-717">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-718">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-718">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-719">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-719">Address:</span></span> <br><span data-ttu-id="7bd71-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-725">SQL Server Analysis Services flerdimensionella</span><span class="sxs-lookup"><span data-stu-id="7bd71-725">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="7bd71-726">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-726">Table</span></span></td>
      <td><span data-ttu-id="7bd71-727">Dimensionen</span><span class="sxs-lookup"><span data-stu-id="7bd71-727">Dimension</span></span></td>
      <td><span data-ttu-id="7bd71-728">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-728">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-729">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-729">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-730">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-730">Address:</span></span> <br><span data-ttu-id="7bd71-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {dimensionen}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-736">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="7bd71-736">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="7bd71-737">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-737">Container</span></span></td>
      <td><span data-ttu-id="7bd71-738">Modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-738">Model</span></span></td>
      <td><span data-ttu-id="7bd71-739">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-739">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-740">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-740">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-741">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-741">Address:</span></span> <br><span data-ttu-id="7bd71-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-745">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="7bd71-745">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="7bd71-746">KPI: N</span><span class="sxs-lookup"><span data-stu-id="7bd71-746">KPI</span></span></td>
      <td><span data-ttu-id="7bd71-747">KPI: N</span><span class="sxs-lookup"><span data-stu-id="7bd71-747">KPI</span></span></td>
      <td><span data-ttu-id="7bd71-748">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-748">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-749">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-749">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-750">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-750">Address:</span></span> <br><span data-ttu-id="7bd71-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-756">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="7bd71-756">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="7bd71-757">Mått</span><span class="sxs-lookup"><span data-stu-id="7bd71-757">Measure</span></span></td>
      <td><span data-ttu-id="7bd71-758">Mått</span><span class="sxs-lookup"><span data-stu-id="7bd71-758">Measure</span></span></td>
      <td><span data-ttu-id="7bd71-759">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-759">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-760">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-760">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-761">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-761">Address:</span></span> <br><span data-ttu-id="7bd71-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-767">SQL Server Analysis Services tabular</span><span class="sxs-lookup"><span data-stu-id="7bd71-767">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="7bd71-768">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-768">Table</span></span></td>
      <td><span data-ttu-id="7bd71-769">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-769">Table</span></span></td>
      <td><span data-ttu-id="7bd71-770">
        <font size=2>Protokoll: Analystjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-770">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="7bd71-771">Autentisering: {windows, basic, anonyma, ingen}</span><span class="sxs-lookup"><span data-stu-id="7bd71-771">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="7bd71-772">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-772">Address:</span></span> <br><span data-ttu-id="7bd71-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-778">SQLServer Reporting Services</span><span class="sxs-lookup"><span data-stu-id="7bd71-778">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="7bd71-779">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-779">Container</span></span></td>
      <td><span data-ttu-id="7bd71-780">Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-780">Server</span></span></td>
      <td><span data-ttu-id="7bd71-781">
        <font size=2>Protokoll: Rapporteringstjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-781">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="7bd71-782">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-782">Authentication: {windows}</span></span> <br><span data-ttu-id="7bd71-783">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-783">Address:</span></span> <br><span data-ttu-id="7bd71-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-786">SQLServer Reporting Services</span><span class="sxs-lookup"><span data-stu-id="7bd71-786">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="7bd71-787">Rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-787">Report</span></span></td>
      <td><span data-ttu-id="7bd71-788">Rapport</span><span class="sxs-lookup"><span data-stu-id="7bd71-788">Report</span></span></td>
      <td><span data-ttu-id="7bd71-789">
        <font size=2>Protokoll: Rapporteringstjänster</span><span class="sxs-lookup"><span data-stu-id="7bd71-789">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="7bd71-790">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-790">Authentication: {windows}</span></span> <br><span data-ttu-id="7bd71-791">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-791">Address:</span></span> <br><span data-ttu-id="7bd71-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sökväg</span><span class="sxs-lookup"><span data-stu-id="7bd71-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path</span></span> <br><span data-ttu-id="7bd71-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-795">Teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-795">Teradata</span></span></td>
      <td><span data-ttu-id="7bd71-796">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-796">Container</span></span></td>
      <td><span data-ttu-id="7bd71-797">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-797">Database</span></span></td>
      <td><span data-ttu-id="7bd71-798">
        <font size=2>Protokoll: teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-798">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="7bd71-799">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-799">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-800">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-800">Address:</span></span> <br><span data-ttu-id="7bd71-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-803">Teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-803">Teradata</span></span></td>
      <td><span data-ttu-id="7bd71-804">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-804">Table</span></span></td>
      <td><span data-ttu-id="7bd71-805">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-805">Table, view</span></span></td>
      <td><span data-ttu-id="7bd71-806">
        <font size=2>Protokoll: teradata</span><span class="sxs-lookup"><span data-stu-id="7bd71-806">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="7bd71-807">Autentisering: {protokoll, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-807">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="7bd71-808">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-808">Address:</span></span> <br><span data-ttu-id="7bd71-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-812">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="7bd71-812">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="7bd71-813">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-813">Container</span></span></td>
      <td><span data-ttu-id="7bd71-814">Modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-814">Model</span></span></td>
      <td><span data-ttu-id="7bd71-815">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="7bd71-815">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="7bd71-816">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-816">Authentication: {windows}</span></span> <br><span data-ttu-id="7bd71-817">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-817">Address:</span></span> <br><span data-ttu-id="7bd71-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="7bd71-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="7bd71-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-821">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="7bd71-821">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="7bd71-822">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-822">Table</span></span></td>
      <td><span data-ttu-id="7bd71-823">Entitet</span><span class="sxs-lookup"><span data-stu-id="7bd71-823">Entity</span></span></td>
      <td><span data-ttu-id="7bd71-824">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="7bd71-824">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="7bd71-825">Autentisering: {windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-825">Authentication: {windows}</span></span> <br><span data-ttu-id="7bd71-826">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-826">Address:</span></span> <br><span data-ttu-id="7bd71-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="7bd71-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="7bd71-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modellen</span><span class="sxs-lookup"><span data-stu-id="7bd71-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="7bd71-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version</span><span class="sxs-lookup"><span data-stu-id="7bd71-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version</span></span> <br><span data-ttu-id="7bd71-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entitet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-831">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7bd71-831">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="7bd71-832">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-832">Container</span></span></td>
      <td><span data-ttu-id="7bd71-833">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-833">Database</span></span></td>
      <td><span data-ttu-id="7bd71-834">
        <font size=2>Protokoll: dokument-db</span><span class="sxs-lookup"><span data-stu-id="7bd71-834">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="7bd71-835">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-835">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-836">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-836">Address:</span></span> <br><span data-ttu-id="7bd71-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="7bd71-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="7bd71-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-839">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7bd71-839">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="7bd71-840">Samling</span><span class="sxs-lookup"><span data-stu-id="7bd71-840">Collection</span></span></td>
      <td><span data-ttu-id="7bd71-841">Samling</span><span class="sxs-lookup"><span data-stu-id="7bd71-841">Collection</span></span></td>
      <td><span data-ttu-id="7bd71-842">
        <font size=2>Protokoll: dokument-db</span><span class="sxs-lookup"><span data-stu-id="7bd71-842">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="7bd71-843">Autentisering: {azure-åtkomstnyckel}</span><span class="sxs-lookup"><span data-stu-id="7bd71-843">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="7bd71-844">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-844">Address:</span></span> <br><span data-ttu-id="7bd71-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL: en</span><span class="sxs-lookup"><span data-stu-id="7bd71-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="7bd71-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;samlingen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-848">ODBC (allmän)</span><span class="sxs-lookup"><span data-stu-id="7bd71-848">Generic ODBC</span></span></td>
      <td><span data-ttu-id="7bd71-849">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-849">Container</span></span></td>
      <td><span data-ttu-id="7bd71-850">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-850">Database</span></span></td>
      <td><span data-ttu-id="7bd71-851">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="7bd71-851">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="7bd71-852">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-852">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-853">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-853">Address:</span></span> <br><span data-ttu-id="7bd71-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alternativ</span><span class="sxs-lookup"><span data-stu-id="7bd71-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="7bd71-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-856">ODBC (allmän)</span><span class="sxs-lookup"><span data-stu-id="7bd71-856">Generic ODBC</span></span></td>
      <td><span data-ttu-id="7bd71-857">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-857">Table</span></span></td>
      <td><span data-ttu-id="7bd71-858">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-858">Table, View</span></span></td>
      <td><span data-ttu-id="7bd71-859">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="7bd71-859">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="7bd71-860">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-860">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-861">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-861">Address:</span></span> <br><span data-ttu-id="7bd71-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alternativ</span><span class="sxs-lookup"><span data-stu-id="7bd71-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="7bd71-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</span><span class="sxs-lookup"><span data-stu-id="7bd71-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="7bd71-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-866">Sybase</span><span class="sxs-lookup"><span data-stu-id="7bd71-866">Sybase</span></span></td>
      <td><span data-ttu-id="7bd71-867">Behållare</span><span class="sxs-lookup"><span data-stu-id="7bd71-867">Container</span></span></td>
      <td><span data-ttu-id="7bd71-868">Databas</span><span class="sxs-lookup"><span data-stu-id="7bd71-868">Database</span></span></td>
      <td><span data-ttu-id="7bd71-869">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="7bd71-869">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="7bd71-870">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-870">authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-871">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-871">address:</span></span> <br><span data-ttu-id="7bd71-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-874">Sybase</span><span class="sxs-lookup"><span data-stu-id="7bd71-874">Sybase</span></span></td>
      <td><span data-ttu-id="7bd71-875">Tabell</span><span class="sxs-lookup"><span data-stu-id="7bd71-875">Table</span></span></td>
      <td><span data-ttu-id="7bd71-876">Tabellen, vyn</span><span class="sxs-lookup"><span data-stu-id="7bd71-876">Table, View</span></span></td>
      <td><span data-ttu-id="7bd71-877">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="7bd71-877">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="7bd71-878">Autentisering: {basic, windows}</span><span class="sxs-lookup"><span data-stu-id="7bd71-878">authentication: {basic, windows}</span></span> <br><span data-ttu-id="7bd71-879">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-879">address:</span></span> <br><span data-ttu-id="7bd71-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="7bd71-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="7bd71-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databasen</span><span class="sxs-lookup"><span data-stu-id="7bd71-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="7bd71-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schemat</span><span class="sxs-lookup"><span data-stu-id="7bd71-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="7bd71-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektet</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="7bd71-884">Andra (inget av ovanstående)</span><span class="sxs-lookup"><span data-stu-id="7bd71-884">Other (none of the above)</span></span></td>
      <td>\*</td>
      <td>\*</td>
      <td><span data-ttu-id="7bd71-885">
        <font size=2>Protokoll: generisk tillgångar</span><span class="sxs-lookup"><span data-stu-id="7bd71-885">
        <font size=2> Protocol: generic-asset</span></span> <br><span data-ttu-id="7bd71-886">Adress:</span><span class="sxs-lookup"><span data-stu-id="7bd71-886">Address:</span></span> <br><span data-ttu-id="7bd71-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </span><span class="sxs-lookup"><span data-stu-id="7bd71-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </span></span></td>
    </tr>
</table>
