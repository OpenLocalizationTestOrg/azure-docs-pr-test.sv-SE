---
title: "aaaInstall Visual Studio och SSDT för SQL Data Warehouse | Microsoft Docs"
description: "Installera Visual Studio och SQL Server Development Tools (SSDT) för Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: anvang;barbkess
ms.openlocfilehash: cf49c13d5cab598ed127f5702c04168b62ede0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a><span data-ttu-id="ae6f7-103">Installera Visual Studio och SSDT för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="ae6f7-103">Install Visual Studio and SSDT for SQL Data Warehouse</span></span>
<span data-ttu-id="ae6f7-104">toodevelop program för SQL Data Warehouse, bör du använda hello senaste version av Visual Studio med hello den senaste versionen av SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="ae6f7-104">toodevelop applications for SQL Data Warehouse, we recommend using hello most recent version of Visual Studio with hello most recent version of SQL Server Data Tools (SSDT).</span></span>  <span data-ttu-id="ae6f7-105">Visual Studio 2013, uppdatering 5 med SSDT, stöds också för bakåtkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-105">Visual Studio 2013 Update 5 with SSDT is also supported for backward compatibility.</span></span>  

<span data-ttu-id="ae6f7-106">Visual Studio med SSDT kan du toouse hello SQL Server Object Explorer toovisually Utforska tabeller, vyer, lagrade procedurer och många fler objekt i ditt SQL Data Warehouse samt att köra frågor.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-106">Using Visual Studio with SSDT will allow you toouse hello SQL Server Object Explorer toovisually explore tables, views, stored procedures and many more objects in your SQL Data Warehouse as well as run queries.</span></span>

> [!NOTE]
> <span data-ttu-id="ae6f7-107">SQL Data Warehouse stöder ännu inte Visual Studio Database Projects.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-107">SQL Data Warehouse does not yet support Visual Studio Database Projects.</span></span>  <span data-ttu-id="ae6f7-108">Den funktionen kommer läggas till i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-108">This feature will be added in a future version.</span></span>
> 
> 

## <a name="step-1-install-visual-studio"></a><span data-ttu-id="ae6f7-109">Steg 1: Installera Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae6f7-109">Step 1: Install Visual Studio</span></span>
<span data-ttu-id="ae6f7-110">Följ dessa länkar toodownload och installera Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-110">Follow these links toodownload and install Visual Studio.</span></span> <span data-ttu-id="ae6f7-111">Om du redan har Visual Studio 2013 eller senare, kan du hoppa över tooStep 2, installera SSDT.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-111">If you already have Visual Studio 2013 or later installed, you can skip tooStep 2, install SSDT.</span></span>

1. <span data-ttu-id="ae6f7-112">[Hämta Visual Studio][].</span><span class="sxs-lookup"><span data-stu-id="ae6f7-112">[Download Visual Studio][].</span></span>
2. <span data-ttu-id="ae6f7-113">Följ hello [installera Visual Studio] [ Installing Visual Studio] på MSDN och välj hello standardkonfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-113">Follow hello [Installing Visual Studio][Installing Visual Studio] guide on MSDN and choose hello default configurations.</span></span>

## <a name="step-2-install-ssdt"></a><span data-ttu-id="ae6f7-114">Steg 2: Installera SSDT</span><span class="sxs-lookup"><span data-stu-id="ae6f7-114">Step 2: Install SSDT</span></span>
<span data-ttu-id="ae6f7-115">tooinstall SSDT för Visual Studio Sök bara efter en SSDT uppdatering i Visual Studio genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-115">tooinstall SSDT for Visual Studio simply check for an SSDT update from within Visual Studio by following these steps.</span></span>

1. <span data-ttu-id="ae6f7-116">I Visual Studio klickar du på **verktyg** / **tillägg och uppdateringar...**</span><span class="sxs-lookup"><span data-stu-id="ae6f7-116">In Visual Studio click on **Tools** / **Extensions and Updates…**</span></span><span data-ttu-id="ae6f7-117"> / **Uppdateringar**</span><span class="sxs-lookup"><span data-stu-id="ae6f7-117"> / **Updates**</span></span>
2. <span data-ttu-id="ae6f7-118">Välj **Produktuppdateringar** och hitta sedan **Microsoft SQL Server-uppdatering för databas-tooling**</span><span class="sxs-lookup"><span data-stu-id="ae6f7-118">Select **Product Updates** and then look for **Microsoft SQL Server Update for database tooling**</span></span>

<span data-ttu-id="ae6f7-119">Om en uppdatering hittas, bör du ha hello senaste versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-119">If an update is not found, then you should have hello latest version installed.</span></span>  <span data-ttu-id="ae6f7-120">tooconfirm SSDT har installerats, klicka på **hjälp** / **om Microsoft Visual Studio** och leta efter SQL Server Data Tools i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-120">tooconfirm SSDT is installed, click on **Help** / **About Microsoft Visual Studio** and look for SQL Server Data Tools in hello list.</span></span>  <span data-ttu-id="ae6f7-121">hello senaste versionen av SSDT är 14.0.60525.0.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-121">hello latest version of SSDT is 14.0.60525.0.</span></span>  <span data-ttu-id="ae6f7-122">Hello alternativet tooinstall inte är tillgänglig från Visual Studio, alternativt kan du besöka hello [SSDT hämta] [ SSDT Download] sidan toodownload och installera SSDT manuellt.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-122">If hello option tooinstall is not available from Visual Studio, alternatively you can visit hello [SSDT Download][SSDT Download] page toodownload and install SSDT manually.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae6f7-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae6f7-123">Next steps</span></span>
<span data-ttu-id="ae6f7-124">Nu när du har hello senaste versionen av SSDT, är du redo för[ansluta] [ connect] tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ae6f7-124">Now that you have hello latest version of SSDT, you are ready too[connect][connect] tooyour SQL Data Warehouse.</span></span>

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Hämta Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
