---
title: Azure reservera VM-instanser Windows programvarukostnader | Microsoft Docs
description: "Information om vilka mätare används för Windows-program för Windows virtuell dator som uppfyller kraven för reserverade."
services: billing
documentationcenter: 
author: manish-shukla01
manager: manshuk
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: b985e6e9575ffeedcac5bcb3f94a43d23fdbb85e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/16/2017
---
# <a name="windows-software-costs-not-included-with-reserved-instances"></a>Windows programvarukostnader ingår inte i reserverade instanser

Om du inte har Azure Hybrid Använd förmånen på den reserverade virtuella datorn med instans, så debiteras du för Windows software mätare som visas i följande tabell:

| meterId | MeterName i filen för användning | Används av virtuell dator |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Windows-reservation Svr Burst (1 kärna) | B-serien |
| cac255a2-9f0f-4c62-8BD6-f0fa449c5f76 | Windows-reservation Svr Burst (2 kärnor) | B-serien |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Windows-reservation Svr Burst (4 kärnor) | B-serien |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Windows-reservation Svr Burst (8 kärnor) | B-serien |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Windows-reservation Svr (1 kärna) | Allt utom B-serien |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Windows-reservation Svr (2 kärnor) | Allt utom B-serien |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Windows-reservation Svr (4 kärnor) | Allt utom B-serien |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Windows-reservation Svr (6 kärnor) | Allt utom B-serien |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Windows-reservation Svr (8 kärnor) | Allt utom B-serien |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Windows-reservation Svr (12 kärnor) | Allt utom B-serien |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Windows-reservation Svr (16 kärnor) | Allt utom B-serien |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Windows-reservation Svr (20 kärnor) | Allt utom B-serien |
| 77eb6dd0-88f5-4A16-ab39-05d1742efb25 | Windows-reservation Svr (24 kärnor) | Allt utom B-serien |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Windows-reservation Svr (32 kärnor) | Allt utom B-serien |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Windows-reservation Svr (40 kärnor) | Allt utom B-serien |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Windows-reservation Svr (64 kärnor) | Allt utom B-serien |
| da612742-e7cc-4CA3-9334-0fb7234059cd | Windows-reservation Svr (72 kärnor) | Allt utom B-serien |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Windows-reservation Svr (128 kärnor) | Allt utom B-serien |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Windows-reservation Svr (256 kärnor) | Allt utom B-serien |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Windows-reservation Svr (96 kärnor) | Allt utom B-serien |

Du kan hämta kostnaden för var och en av dessa mätare via Azure RateCard API. Mer information om hur du hämtar priser för azure-mätare finns [Hämta pris- och metadata för resurser som används i en Azure-prenumeration](https://msdn.microsoft.com/library/azure/mt219004).