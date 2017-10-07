---
title: "aaaCreate en Azure tid serien Insights miljö | Microsoft Docs"
description: "I kursen får du lära dig hur toocreate serie miljön, ansluter du det tooan händelsekälla och klara tooanalyze din händelsedata i minuter."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a>Skapa en ny tid serien insikter miljö i hello Azure-portalen

Time Series Insights-miljön är en Azure-resurs med ingångs- och lagringskapacitet. Kunder etablera miljöer via hello Azure-portalen med hello krävs kapacitet.

## <a name="steps-toocreate-hello-environment"></a>Steg toocreate hello miljö

Följ dessa steg toocreate din miljö:

1.  Logga in toohello [Azure-portalen](https://portal.azure.com).
2.  Klicka hello plustecken (”+”) i hello övre vänstra hörnet.
3.  Sök efter ”tid serien insikter” i sökrutan för hello.

  ![Skapa hello tid serien insikter miljö](media/get-started/getstarted-create-environment1.png)

4.  Välj ”Time Series Insights”, klicka på ”Skapa”.

  ![Skapa hello tid serien insikter resursgrupp](media/get-started/getstarted-create-environment2.png)

5.  Ange ett namn på miljön. Det här namnet representerar hello-miljö i [tid serien explorer](https://insights.timeseries.azure.com).
6.  Välj en prenumeration. Välj ett namn som innehåller din händelsekälla. Tid serien insikter kan automatisk identifiering av Azure IoT Hub och Event Hub-resurser finns i hello samma prenumeration.
7.  Välj eller skapa en Resursgrupp. En resursgrupp är en samling med Azure-resurser som används tillsammans.
8.  Välj en värdplats. tooavoid flytta över data datacenter, välj platsen som innehåller din händelsekälla.
9.  Välj en prisnivå.
10. Välj kapacitet. Du kan ändra kapacitet för en miljö när den har skapats.
11. Skapa din miljö. Du kan även fästa miljö toohello instrumentpanelen för enkel åtkomst när du loggar in.

  ![Skapa hello tid serien insikter PIN-kod toodashboard](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Nästa steg

* [Definiera principer för åtkomst av data](time-series-insights-data-access.md) tooaccess miljön i [tid serien Insights-portalen](https://insights.timeseries.azure.com)
* [Skapa en händelsekälla](time-series-insights-add-event-source.md)
* [Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla
