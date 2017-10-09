---
title: "aaaHow tooscale Azure tid serien Insights miljön | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooscale Azure tid serien Insights-miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 55eda388997589185bd34228762b95e182b228ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-your-time-series-insights-environment"></a>Hur tooscale tid serien insikter miljön

Den här självstudiekursen beskrivs hur tooscale tid serien insikter miljön.

> [!NOTE]
> Skala upp över sku-typer tillåts inte. En miljö med en S1 Sku kan inte konverteras till en S2 miljö.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1-SKU ingång priser och kapacitet

| S1 Artikelnummerkapaciteten | Ingång hastighet | Maximal lagringskapacitet
| --- | --- | --- |
| 1 | 1 GB (1 miljon händelser) | 30 GB (30 miljoner händelser) per månad |
| 10 | 10 GB (10 miljoner händelser) | 300 GB (300 miljoner händelser) per månad |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU ingång priser och kapacitet

| S2 Artikelnummerkapaciteten | Ingång hastighet | Maximal lagringskapacitet
| --- | --- | --- |
| 1 | 10 GB (10 miljoner händelser) | 300 GB (300 miljoner händelser) per månad |
| 10 | 100 GB (100 miljoner händelser) | 3 TB (3 miljarder händelser) per månad |

Kapaciteter skalas linjärt, så en S1 sku med kapacitet 2 stöder 2 GB (2 miljoner) händelser per dag ingång hastighet och 60 GB (60 miljoner händelser) per månad.

## <a name="changing-hello-capacity-of-your-environment"></a>Ändra hello kapaciteten för din miljö

1. I hello Azure-portalen, Välj hello miljö vars kapacitet du vill toochange.
1. Klicka på Konfigurera under inställningar.
1. Använd hello kapacitet skjutreglaget tooselect hello kapaciteten som uppfyller hello för meddelanden om ingångs-priser och lagringskapacitet.

## <a name="next-steps"></a>Nästa steg

* Kontrollera att det räcker hello ny kapacitet tooprevent begränsning. Mer information finns i hello *din miljö kan att komma begränsas* avsnittet [här](time-series-insights-diagnose-and-solve-problems.md).
