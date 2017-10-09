---
title: "aaaOverview av vanliga Autoskala mönster | Microsoft Docs"
description: "Läs om några av hello vanliga mönster tooauto skala din resurs i Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a>Översikt över vanliga Autoskala mönster
Den här artikeln beskrivs några av hello vanliga mönster tooscale resurs i Azure.

Azure övervakaren Autoskala gäller endast tooVirtual datorn skala uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer. 

# <a name="lets-get-started"></a>Kan komma igång

Den här artikeln förutsätter att du är bekant med automatisk skalning. Du kan [Kom igång här tooscale resurs][1]. hello följande är några av hello vanliga skala mönster.

## <a name="scale-based-on-cpu"></a>Skala baserat på CPU

Du har ett webbprogram (/ VMSS/molnet rolltjänst) och 

- Du vill tooscale out/skala i baserat på CPU.
- Dessutom kan du vill tooensure det finns ett minsta antal instanser. 
- Du bör också, tooensure som du anger en övre gräns toohello antalet instanser som kan skalas till.

![Skala baserat på CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Skala annorlunda veckodagar vs helger

Du har ett webbprogram (/ VMSS/molnet rolltjänst) och

- Du vill 3 instanser som standard (på vardagar)
- Du förväntar dig inte trafik på helger och därför du vill tooscale ned too1 instans på helger.

![Skala annorlunda veckodagar vs helger][3]

## <a name="scale-differently-during-holidays"></a>Skala annorlunda under helgdagar

Du har ett webbprogram (/ VMSS/molnet rolltjänst) och 

- Du vill tooscale upp/ned baserat på CPU-användning som standard
- Dock under köpstarka jul (eller särskilda dagar som är viktiga för ditt företag) toooverride hello standardinställningar och när du har mer kapacitet till din förfogande.

![Skala annorlunda på helgdagar][4]

## <a name="scale-based-on-custom-metric"></a>Skala baserat på anpassade mått

Du har en frontwebb och en API-nivå som kommunicerar med hello backend. 

- Du vill tooscale hello API-nivå baserat på egna händelser i hello klientdelen (exempel: du vill tooscale utcheckningen processen baserat på hello antal objekt i hello kundvagn)

![Skala baserat på anpassade mått][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png