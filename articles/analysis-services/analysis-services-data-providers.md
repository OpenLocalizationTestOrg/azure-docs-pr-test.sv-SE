---
title: "Klientbibliotek som krävs för att ansluta till Azure Analysis Services | Microsoft Docs"
description: "Beskriver klientbibliotek som krävs för program och verktyg att ansluta Azure Analysis Services"
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
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a><span data-ttu-id="418c8-103">Klientbibliotek för att ansluta till Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="418c8-103">Client libraries for connecting to Azure Analysis Services</span></span>

<span data-ttu-id="418c8-104">Klientbibliotek är nödvändiga för program och verktyg att ansluta till Analysis Services-servrar.</span><span class="sxs-lookup"><span data-stu-id="418c8-104">Client libraries are necessary for client applications and tools to connect to Analysis Services servers.</span></span> 

<span data-ttu-id="418c8-105">Analysis Services använder tre klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="418c8-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="418c8-106">ADOMD.NET och tjänster objekt AMO (Analysis Management), är hanterade klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="418c8-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="418c8-107">Analysis Services OLE DB-providern (MSOLAP DLL) är en inbyggd klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="418c8-107">The Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="418c8-108">Alla tre installeras vanligtvis på samma gång.</span><span class="sxs-lookup"><span data-stu-id="418c8-108">Typically, all three are installed at the same time.</span></span> <span data-ttu-id="418c8-109">Azure Analysis Services kräver den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="418c8-109">Azure Analysis Services requires the latest versions.</span></span> 

<span data-ttu-id="418c8-110">Microsoft-klientprogram, till exempel Power BI Desktop- och Excel installera alla tre klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="418c8-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="418c8-111">Men, beroende på version eller ofta uppdateras, klientbibliotek kanske inte de senaste versionerna som krävs av Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="418c8-111">However, depending on the version or frequency of updates, client libraries may not be the latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="418c8-112">Detsamma gäller för anpassade program eller andra gränssnitt som AsCmd, TOM, ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="418c8-112">The same applies to custom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="418c8-113">Dessa program kräver att du manuellt installerar biblioteken.</span><span class="sxs-lookup"><span data-stu-id="418c8-113">These applications require manually installing the libraries.</span></span> <span data-ttu-id="418c8-114">Klientbibliotek för manuell installation ingår i SQL Server-funktionspaket som distribuerbara paket.</span><span class="sxs-lookup"><span data-stu-id="418c8-114">The client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="418c8-115">Dessa klientbibliotek är knutna till SQL Server-versionen och kanske inte den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="418c8-115">However, these client libraries are tied to the SQL Server version and may not be the latest.</span></span>  

<span data-ttu-id="418c8-116">Klientbibliotek för klientanslutningar skiljer sig från dataleverantörer som krävs för att ansluta från Azure Analysis Services-servern till en datakälla.</span><span class="sxs-lookup"><span data-stu-id="418c8-116">Client libraries for client connections are different from data providers required to connect from an Azure Analysis Services server to a data source.</span></span> <span data-ttu-id="418c8-117">Mer information om datakällan anslutningar finns [Datasource anslutningar](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="418c8-117">To learn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-the-latest-client-libraries"></a><span data-ttu-id="418c8-118">Hämta senaste klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="418c8-118">Download the latest client libraries</span></span>  
<span data-ttu-id="418c8-119">Använd följande klientbibliotek om du är i en produktionsmiljö och kräver helt utgivna och stöds versioner.</span><span class="sxs-lookup"><span data-stu-id="418c8-119">Use the following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="418c8-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="418c8-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="418c8-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="418c8-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="418c8-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="418c8-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="418c8-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="418c8-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="418c8-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="418c8-124">Next steps</span></span>
<span data-ttu-id="418c8-125">[Ansluta till Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="418c8-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="418c8-126">Anslut med Power BI</span><span class="sxs-lookup"><span data-stu-id="418c8-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)
