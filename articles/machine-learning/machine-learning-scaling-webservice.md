---
title: "aaaHow tooincrease samtidighet på en Azure Machine Learning-webbtjänst | Microsoft Docs"
description: "Lär dig hur tooincrease samtidighet på en Azure Machine Learning webbtjänsten genom att lägga till ytterligare slutpunkter."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "Azure maskininlärning webbtjänster, operationalization, skalning, slutpunkt, samtidighet"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Skala en Azure Machine Learning-webbtjänst genom att lägga till ytterligare slutpunkter
> [!NOTE]
> Det här avsnittet beskrivs tekniker tillämpliga tooa **klassiska** Machine Learning-webbtjänst. 
> 
> 

Som standard varje publicerade webbtjänst är konfigurerade toosupport 20 samtidiga begäranden och kan vara så högt som 200 samtidiga förfrågningar. Medan hello klassiska Azure-portalen innehåller en sätt tooset det här värdet, Azure Machine Learning optimerar automatiskt hello inställningen tooprovide hello bästa prestanda för webbtjänsten och hello portal värdet ignoreras. 

Om du planerar toocall hello-API med en högre belastning än maximalt antal samtidiga anrop värdet 200 stöder bör du skapa flera slutpunkter på hello samma webbtjänsten. Du kan sedan slumpmässigt distribuera din belastningen över alla.

hello skalning av en webbtjänst är en vanlig aktivitet. Vissa orsaker tooscale är toosupport mer än 200 samtidiga förfrågningar, öka tillgängligheten via flera slutpunkter, eller ange separata slutpunkter för hello-webbtjänsten. Du kan öka hello skala genom att lägga till ytterligare slutpunkter för hello samma webbtjänst via [klassiska Azure-portalen](https://manage.windowsazure.com/) eller hello [Azure Machine Learning-webbtjänst](https://services.azureml.net/) portal.

Mer information om att lägga till nya slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md).

Tänk på att med hjälp av ett antal hög samtidighet kan vara skadliga om du inte anropar hello API med en relativt hög hastighet. Du kan se sporadiska timeout och/eller toppar i hello latens om du placerar en relativt låg belastning på ett API som konfigurerats för hög belastning.

hello synkron API: er används vanligtvis i situationer där en låg latens önskas. Svarstiden här innebär hello tid det tar för en begäran för hello API toocomplete och inte kontot för alla fördröjningar i nätverket. Anta att du har ett API med en 50 ms fördröjning. toofully använda hello tillgänglig kapacitet med begränsningen nivån hög och maximalt antal samtidiga anrop = 20, behöver du toocall detta API 20 * 1000 / 50 = 400 gånger per sekund. Utöka detta ytterligare kan ett maximalt antal samtidiga anrop 200 du toocall hello API 4000 gånger per sekund, förutsatt att en 50 ms fördröjning.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
