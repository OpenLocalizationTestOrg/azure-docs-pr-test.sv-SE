---
title: "aaaScheduler begränsar och är som standard"
description: "Schemaläggaren gränser och standardvärden"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a>Schemaläggaren gränser och standardvärden
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Schemaläggaren kvoter, gränser, standarder och begränsningar
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a>Hej x-ms-begäran-id sidhuvud
Alla begäranden som görs mot hello Schemaläggaren returnerar ett svarshuvud med namnet**x-ms-begäran-id**. Det här sidhuvudet innehåller ett täckande värde som unikt identifierar hello-begäran.

Om en begäran är konsekvent har misslyckas och du verifierat att begäran hello korrekt formulerade, du kan använda det här värdet tooreport hello fel tooMicrosoft. Inkludera i rapporten, hello värdet för x-ms-begäran-id hello ungefärliga tid hello begäran gjordes hello hello prenumeration, jobbsamlingen och/eller jobb-ID och hello typ av åtgärd som hello begäran gjordes ett försök.

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

