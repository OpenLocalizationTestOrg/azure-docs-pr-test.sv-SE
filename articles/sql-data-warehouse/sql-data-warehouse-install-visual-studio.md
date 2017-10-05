---
title: "Installera Visual Studio och SSDT för SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: f7023b78c241a7bc8014276cd0bfa455165b42cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a><span data-ttu-id="cb2a0-103">Installera Visual Studio och SSDT för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cb2a0-103">Install Visual Studio and SSDT for SQL Data Warehouse</span></span>
<span data-ttu-id="cb2a0-104">För att utveckla program för SQL Data Warehouse, rekommenderar vi använder den senaste versionen av Visual Studio med den senaste versionen av SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="cb2a0-104">To develop applications for SQL Data Warehouse, we recommend using the most recent version of Visual Studio with the most recent version of SQL Server Data Tools (SSDT).</span></span>  <span data-ttu-id="cb2a0-105">Visual Studio 2013, uppdatering 5 med SSDT, stöds också för bakåtkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-105">Visual Studio 2013 Update 5 with SSDT is also supported for backward compatibility.</span></span>  

<span data-ttu-id="cb2a0-106">Visual Studio med SSDT låter dig använda SQL Server Object Explorer för att visuellt utforska tabeller, vyer, lagrade procedurer och många fler objekt i ditt SQL Data Warehouse och köra frågor.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-106">Using Visual Studio with SSDT will allow you to use the SQL Server Object Explorer to visually explore tables, views, stored procedures and many more objects in your SQL Data Warehouse as well as run queries.</span></span>

> [!NOTE]
> <span data-ttu-id="cb2a0-107">SQL Data Warehouse stöder ännu inte Visual Studio Database Projects.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-107">SQL Data Warehouse does not yet support Visual Studio Database Projects.</span></span>  <span data-ttu-id="cb2a0-108">Den funktionen kommer läggas till i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-108">This feature will be added in a future version.</span></span>
> 
> 

## <a name="step-1-install-visual-studio"></a><span data-ttu-id="cb2a0-109">Steg 1: Installera Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb2a0-109">Step 1: Install Visual Studio</span></span>
<span data-ttu-id="cb2a0-110">Följa dessa länkar om du vill hämta och installera Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-110">Follow these links to download and install Visual Studio.</span></span> <span data-ttu-id="cb2a0-111">Om du redan har Visual Studio 2013 eller senare, du kan gå vidare till steg 2, installera SSDT.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-111">If you already have Visual Studio 2013 or later installed, you can skip to Step 2, install SSDT.</span></span>

1. <span data-ttu-id="cb2a0-112">[Hämta Visual Studio][].</span><span class="sxs-lookup"><span data-stu-id="cb2a0-112">[Download Visual Studio][].</span></span>
2. <span data-ttu-id="cb2a0-113">Följ den [installera Visual Studio] [ Installing Visual Studio] på MSDN och välj standardkonfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-113">Follow the [Installing Visual Studio][Installing Visual Studio] guide on MSDN and choose the default configurations.</span></span>

## <a name="step-2-install-ssdt"></a><span data-ttu-id="cb2a0-114">Steg 2: Installera SSDT</span><span class="sxs-lookup"><span data-stu-id="cb2a0-114">Step 2: Install SSDT</span></span>
<span data-ttu-id="cb2a0-115">För att installera SSDT för Visual Studio, kollar du efter en SSDT uppdatering i Visual Studio genom att följa de här stegen.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-115">To install SSDT for Visual Studio simply check for an SSDT update from within Visual Studio by following these steps.</span></span>

1. <span data-ttu-id="cb2a0-116">I Visual Studio klickar du på **verktyg** / **tillägg och uppdateringar...**</span><span class="sxs-lookup"><span data-stu-id="cb2a0-116">In Visual Studio click on **Tools** / **Extensions and Updates…**</span></span><span data-ttu-id="cb2a0-117"> / **Uppdateringar**</span><span class="sxs-lookup"><span data-stu-id="cb2a0-117"> / **Updates**</span></span>
2. <span data-ttu-id="cb2a0-118">Välj **Produktuppdateringar** och hitta sedan **Microsoft SQL Server-uppdatering för databas-tooling**</span><span class="sxs-lookup"><span data-stu-id="cb2a0-118">Select **Product Updates** and then look for **Microsoft SQL Server Update for database tooling**</span></span>

<span data-ttu-id="cb2a0-119">Om ingen uppdatering hittas, bör du ha den senaste versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-119">If an update is not found, then you should have the latest version installed.</span></span>  <span data-ttu-id="cb2a0-120">Bekräfta SSDT har installerats genom att klicka på **Hjälp** / **Om Microsoft Visual Studio** och leta efter SQL Server Data Tools i listan.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-120">To confirm SSDT is installed, click on **Help** / **About Microsoft Visual Studio** and look for SQL Server Data Tools in the list.</span></span>  <span data-ttu-id="cb2a0-121">Den senaste versionen av SSDT är 14.0.60525.0.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-121">The latest version of SSDT is 14.0.60525.0.</span></span>  <span data-ttu-id="cb2a0-122">Om alternativet att installera inte är tillgänglig från Visual Studio alternativt så kan du besöka den [SSDT hämta] [ SSDT Download] sidan om du vill hämta och installera SSDT manuellt.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-122">If the option to install is not available from Visual Studio, alternatively you can visit the [SSDT Download][SSDT Download] page to download and install SSDT manually.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb2a0-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb2a0-123">Next steps</span></span>
<span data-ttu-id="cb2a0-124">Nu när du har den senaste versionen av SSDT, är du redo att [ansluta] [ connect] till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb2a0-124">Now that you have the latest version of SSDT, you are ready to [connect][connect] to your SQL Data Warehouse.</span></span>

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
<span data-ttu-id="cb2a0-125">[Hämta Visual Studio]: https://www.visualstudio.com/downloads/</span><span class="sxs-lookup"><span data-stu-id="cb2a0-125">[Download Visual Studio]: https://www.visualstudio.com/downloads/</span></span>
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
