---
title: "Azure Apptjänst: Skala program för Apptjänst"
description: "Läs och nackdelar med skalning programmet i App Service."
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
ms.openlocfilehash: 4ebaafe69fc1f91c7890611b14e8600df5c40cde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a>Azure Apptjänst: Skala program för Apptjänst
Program som finns i Azure App Service kan [uppnå massiv skala](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Skala ett program är dock komplexa problem som inte har en ”passar alla”-lösning. Korrekt skala ditt program det är 3 viktiga områden som bidrar till din fungerande program:

1. Förstå din programarkitektur och dess svagheter.
   * Är programmet-Stateful? Tillståndslösa?
   * Vad är alla komponenter i ditt program?
     * Var finns flaskhalsar i programmet?
   * När belastningen tillämpas på appen, vad bryts första?
2. För att förstå kraven på förväntad belastning och prestanda.
   * Behöver programmet fungerar tusen användare? eller en miljon?
   * Kommer trafik från en enda geografisk plats eller globalt?
   * Finns det säsongsbaserade variationer? trafik toppar?
   * Hur snabbt svara appen? 1 sekund? 1 millisekund?
3. Förstå och korrekt utnyttja den plattform som värd för din app.
   * Vilka funktioner ska använda för att uppnå Mina skala mål?

Det här avsnittet hjälper dig att förstå alla faktorer och hjälp du utforma en strategi som drar nytta av de nödvändiga Apptjänst-funktioner för att nå dina skalbarhetsmål.

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

