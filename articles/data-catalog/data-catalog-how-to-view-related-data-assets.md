---
title: "aaaHow tooview relaterade datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln förklarar hur tooview relaterade datatillgångar för en tillgång markerade data i Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: b69686737070ac563a0318f48e693215c605f90b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a><span data-ttu-id="67504-103">Hur relaterade tooview datatillgångar i Azure Data Catalog?</span><span class="sxs-lookup"><span data-stu-id="67504-103">How tooview related data assets in Azure Data Catalog?</span></span>
<span data-ttu-id="67504-104">Azure Data Catalog kan tooview data tillgångar relaterade tooa valda data tillgången och visa relationer mellan dem.</span><span class="sxs-lookup"><span data-stu-id="67504-104">Azure Data Catalog allows you tooview data assets related tooa selected data asset and view relationships between them.</span></span> 

## <a name="supported-data-sources"></a><span data-ttu-id="67504-105">Datakällor som stöds</span><span class="sxs-lookup"><span data-stu-id="67504-105">Supported data sources</span></span> 
<span data-ttu-id="67504-106">När du registrerar datatillgångar från följande datakällor hello registrerar Azure Data Catalog metadata om kopplingsrelationer mellan hello valda datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="67504-106">When you register data assets from hello following data sources, Azure Data Catalog automatically registers metadata about join relationships between hello selected data assets.</span></span> 

- <span data-ttu-id="67504-107">SQL Server</span><span class="sxs-lookup"><span data-stu-id="67504-107">SQL Server</span></span>
- <span data-ttu-id="67504-108">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="67504-108">Azure SQL Database</span></span>
- <span data-ttu-id="67504-109">MySQL</span><span class="sxs-lookup"><span data-stu-id="67504-109">MySQL</span></span>
- <span data-ttu-id="67504-110">Oracle</span><span class="sxs-lookup"><span data-stu-id="67504-110">Oracle</span></span>

## <a name="view-related-data-assets"></a><span data-ttu-id="67504-111">Visa relaterade datatillgångar</span><span class="sxs-lookup"><span data-stu-id="67504-111">View related data assets</span></span>
<span data-ttu-id="67504-112">tooview datatillgångar som är relaterade tooa markerade datauppsättningen använda hello **relationer** fliken som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="67504-112">tooview data assets that are related tooa selected dataset, use hello **Relationships** tab as shown in hello following image:</span></span> 

![Azure Data Catalog – Visa relaterade datatillgångar](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

<span data-ttu-id="67504-114">I det här exemplet har två relationer för hello valt **ProductSubcategory** datatillgång:</span><span class="sxs-lookup"><span data-stu-id="67504-114">In this example, there are two relationships for hello selected **ProductSubcategory** data asset:</span></span> 

- <span data-ttu-id="67504-115">ProductSubcategoryID kolumn hello produkten tabell har en sekundärnyckelrelation med ProductSubcategoryID kolumn hello valt ProductSubcategory tabell.</span><span class="sxs-lookup"><span data-stu-id="67504-115">ProductSubcategoryID column of hello Product table has a foreign key relationship with ProductSubcategoryID column of hello selected ProductSubcategory table.</span></span> 
- <span data-ttu-id="67504-116">ProductCategoryID kolumn hello ProductSubCategory tabell har en sekundärnyckelrelation med ProductCategoryID kolumn hello valt produktkategori tabell.</span><span class="sxs-lookup"><span data-stu-id="67504-116">ProductCategoryID column of hello ProductSubCategory table has a foreign key relationship with ProductCategoryID column of hello selected ProductCategory table.</span></span>

> [!NOTE]
> <span data-ttu-id="67504-117">Observera hello riktning hello pilen i trädvyn för hello relationer.</span><span class="sxs-lookup"><span data-stu-id="67504-117">Notice hello direction of hello arrow in hello relationships tree view.</span></span>  

<span data-ttu-id="67504-118">toosee mer information, till exempel hello fullständigt kvalificerade namn för hello kolumnen musen hello och du ser en liknande popup-toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="67504-118">toosee more details such as hello fully qualified name of hello column, move hello mouse over and you see a popup similar toohello following image:</span></span> 

![Azure Data Catalog - relation popup](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

<span data-ttu-id="67504-120">tooinclude relationer mellan tillgångar som redan har registrerats, registrera dessa tillgångar.</span><span class="sxs-lookup"><span data-stu-id="67504-120">tooinclude relationships between assets that have already been registered, re-register those assets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67504-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67504-121">Next steps</span></span>
- [<span data-ttu-id="67504-122">Hur toomanage datatillgångar</span><span class="sxs-lookup"><span data-stu-id="67504-122">How toomanage data assets</span></span>](data-catalog-how-to-manage.md)
