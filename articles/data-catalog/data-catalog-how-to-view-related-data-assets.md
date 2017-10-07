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
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a>Hur relaterade tooview datatillgångar i Azure Data Catalog?
Azure Data Catalog kan tooview data tillgångar relaterade tooa valda data tillgången och visa relationer mellan dem. 

## <a name="supported-data-sources"></a>Datakällor som stöds 
När du registrerar datatillgångar från följande datakällor hello registrerar Azure Data Catalog metadata om kopplingsrelationer mellan hello valda datatillgångar. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Visa relaterade datatillgångar
tooview datatillgångar som är relaterade tooa markerade datauppsättningen använda hello **relationer** fliken som visas i följande bild hello: 

![Azure Data Catalog – Visa relaterade datatillgångar](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

I det här exemplet har två relationer för hello valt **ProductSubcategory** datatillgång: 

- ProductSubcategoryID kolumn hello produkten tabell har en sekundärnyckelrelation med ProductSubcategoryID kolumn hello valt ProductSubcategory tabell. 
- ProductCategoryID kolumn hello ProductSubCategory tabell har en sekundärnyckelrelation med ProductCategoryID kolumn hello valt produktkategori tabell.

> [!NOTE]
> Observera hello riktning hello pilen i trädvyn för hello relationer.  

toosee mer information, till exempel hello fullständigt kvalificerade namn för hello kolumnen musen hello och du ser en liknande popup-toohello följande bild: 

![Azure Data Catalog - relation popup](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

tooinclude relationer mellan tillgångar som redan har registrerats, registrera dessa tillgångar.

## <a name="next-steps"></a>Nästa steg
- [Hur toomanage datatillgångar](data-catalog-how-to-manage.md)
