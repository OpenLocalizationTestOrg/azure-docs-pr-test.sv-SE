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
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a>Klientbibliotek för att ansluta tooAzure Analysis Services

Klientbibliotek krävs för program och verktyg tooconnect tooAnalysis Services-servrar. 

Analysis Services använder tre klientbibliotek. ADOMD.NET och tjänster objekt AMO (Analysis Management), är hanterade klientbibliotek. hello Analysis Services OLE DB-providern (MSOLAP DLL) är en inbyggd klientbiblioteket. Normalt alla tre har installerats på hello samtidigt. Azure Analysis Services kräver hello senaste versionerna. 

Microsoft-klientprogram, till exempel Power BI Desktop- och Excel installera alla tre klientbibliotek. Men, beroende på hello version eller ofta uppdateras klientbibliotek kanske inte hello senaste versionerna som krävs av Azure Analysis Services. hello gäller samma toocustom program eller andra gränssnitt som AsCmd, TOM, ADOMD.NET. Dessa program kräver att du manuellt installerar hello-bibliotek. hello-klientbibliotek för manuell installation ingår i SQL Server-funktionspaket som distribuerbara paket. Dessa klientbibliotek är bundet toohello SQL Server-versionen och kanske inte hello senaste.  

Klientbibliotek för klientanslutningar skiljer sig från data providers krävs tooconnect från en datakälla för Azure Analysis Services-servern tooa. toolearn mer om datasource anslutningar, se [Datasource anslutningar](analysis-services-datasource.md).

## <a name="download-hello-latest-client-libraries"></a>Hämta senaste hello-klientbibliotek  
Använd hello följande klientbibliotek om du är i en produktionsmiljö och kräver helt utgivna och stöds versioner.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Nästa steg
[Ansluta till Excel](analysis-services-connect-excel.md)    
[Anslut med Power BI](analysis-services-connect-pbi.md)
