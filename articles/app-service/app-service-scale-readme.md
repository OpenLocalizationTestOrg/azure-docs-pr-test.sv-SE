---
title: "Azure Apptjänst: Skala program för Apptjänst"
description: "Lär dig hello och nackdelar med skalning programmet i App Service."
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a>Azure Apptjänst: Skala program för Apptjänst
Program som finns i Azure App Service kan [uppnå massiv skala](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Skala ett program är dock komplexa problem som inte har en ”passar alla”-lösning. toocorrectly skala ditt program finns på 3 viktiga områden som kommer att bidra tooyour program lyckades:

1. Förstå din programarkitektur och dess svagheter.
   * Är programmet-Stateful? Tillståndslösa?
   * Vad är alla hello komponenter i ditt program?
     * Var finns hello flaskhalsar i hello program?
   * När belastningen är tillämpade tooyour app, vad bryts första?
2. Förstå hello förväntad belastning och prestandakrav.
   * Behöver programmet hello tooserve tusen användare? eller en miljon?
   * Kommer trafik från en enda geografisk plats eller globalt?
   * Finns det säsongsbaserade variationer? trafik toppar?
   * Hur snabbt svara hello app? 1 sekund? 1 millisekund?
3. Förstå och korrekt utnyttjar hello plattform som är värd för din app.
   * Funktioner ska jag använda tooachieve Mina skala mål?

Det här avsnittet hjälper dig att förstå alla hello faktorer och hjälper dig att du utforma en strategi som drar nytta av hello nödvändiga Apptjänst funktioner tooachieve dina skalbarhetsmål.

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

