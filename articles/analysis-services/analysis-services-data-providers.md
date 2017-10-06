---
title: "aaaClient bibliotek som krävs för att ansluta tooAzure Analysis Services | Microsoft Docs"
description: "Beskriver klientbibliotek som krävs för klienten program och verktyg tooconnect Azure Analysis Services"
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
ms.openlocfilehash: 74ba5c05ba76c6587c5aed38f200a1ba469aa4f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a><span data-ttu-id="d1420-103">Klientbibliotek för att ansluta tooAzure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="d1420-103">Client libraries for connecting tooAzure Analysis Services</span></span>

<span data-ttu-id="d1420-104">Klientbibliotek krävs för program och verktyg tooconnect tooAnalysis Services-servrar.</span><span class="sxs-lookup"><span data-stu-id="d1420-104">Client libraries are necessary for client applications and tools tooconnect tooAnalysis Services servers.</span></span> 

<span data-ttu-id="d1420-105">Analysis Services använder tre klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="d1420-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="d1420-106">ADOMD.NET och tjänster objekt AMO (Analysis Management), är hanterade klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="d1420-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="d1420-107">hello Analysis Services OLE DB-providern (MSOLAP DLL) är en inbyggd klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="d1420-107">hello Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="d1420-108">Normalt alla tre har installerats på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d1420-108">Typically, all three are installed at hello same time.</span></span> <span data-ttu-id="d1420-109">Azure Analysis Services kräver hello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="d1420-109">Azure Analysis Services requires hello latest versions.</span></span> 

<span data-ttu-id="d1420-110">Microsoft-klientprogram, till exempel Power BI Desktop- och Excel installera alla tre klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="d1420-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="d1420-111">Men, beroende på hello version eller ofta uppdateras klientbibliotek kanske inte hello senaste versionerna som krävs av Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="d1420-111">However, depending on hello version or frequency of updates, client libraries may not be hello latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="d1420-112">hello gäller samma toocustom program eller andra gränssnitt som AsCmd, TOM, ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="d1420-112">hello same applies toocustom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="d1420-113">Dessa program kräver att du manuellt installerar hello-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="d1420-113">These applications require manually installing hello libraries.</span></span> <span data-ttu-id="d1420-114">hello-klientbibliotek för manuell installation ingår i SQL Server-funktionspaket som distribuerbara paket.</span><span class="sxs-lookup"><span data-stu-id="d1420-114">hello client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="d1420-115">Dessa klientbibliotek är bundet toohello SQL Server-versionen och kanske inte hello senaste.</span><span class="sxs-lookup"><span data-stu-id="d1420-115">However, these client libraries are tied toohello SQL Server version and may not be hello latest.</span></span>  

<span data-ttu-id="d1420-116">Klientbibliotek för klientanslutningar skiljer sig från data providers krävs tooconnect från en datakälla för Azure Analysis Services-servern tooa.</span><span class="sxs-lookup"><span data-stu-id="d1420-116">Client libraries for client connections are different from data providers required tooconnect from an Azure Analysis Services server tooa data source.</span></span> <span data-ttu-id="d1420-117">toolearn mer om datasource anslutningar, se [Datasource anslutningar](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="d1420-117">toolearn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-hello-latest-client-libraries"></a><span data-ttu-id="d1420-118">Hämta senaste hello-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="d1420-118">Download hello latest client libraries</span></span>  
<span data-ttu-id="d1420-119">Använd hello följande klientbibliotek om du är i en produktionsmiljö och kräver helt utgivna och stöds versioner.</span><span class="sxs-lookup"><span data-stu-id="d1420-119">Use hello following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="d1420-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="d1420-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="d1420-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="d1420-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="d1420-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="d1420-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="d1420-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="d1420-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="d1420-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1420-124">Next steps</span></span>
<span data-ttu-id="d1420-125">[Ansluta till Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="d1420-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="d1420-126">Anslut med Power BI</span><span class="sxs-lookup"><span data-stu-id="d1420-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)
