---
title: GitHub-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. GitHub är en webbaserad Git-lagringsplats värd för tjänsten. Den erbjuder alla distribuerade revision kontroll- och koden management (SCM) funktioner av Git samt lägga till egna funktioner."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-github-connector"></a>Kom igång med GitHub-koppling
GitHub är en webbaserad Git-lagringsplats värd för tjänsten. Den erbjuder alla distribuerade revision kontroll- och koden management (SCM) funktioner av Git samt lägga till egna funktioner.

Du kan komma igång genom att skapa en logikapp nu, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-github"></a>Skapa en anslutning till GitHub
För att skapa logikappar med GitHub, måste du först skapa en **anslutning** ange detaljer för följande egenskaper: 

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Token |Ja |Ange autentiseringsuppgifter för GitHub |

Du kan använda den köra åtgärder och lyssna för utlösare som beskrivs i den här artikeln när du har skapat anslutningen. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/github/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka till den [API: er listan](apis-list.md).