---
title: "aaaOverview av Azure serverlösa | Microsoft Docs"
description: "Skapa kraftfulla lösningar i hello molnet utan toothink om infrastruktur."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7c9c09d96e472edd1631892982ac60aae97342a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Översikt över Azure serverlösa med funktioner och Logic Apps

Serverlösa program ger fördelar för ökad hastighet för utveckling, minskad kod obligatorisk och enkelhet med en skala.  Den här artikeln hamnar i hello olika attribut i serverlösa lösningar och Azure serverlösa erbjudanden.

## <a name="what-is-serverless"></a>Vad är serverlösa?

Utan Server betyder det finns inga servrar – det betyder bara hello utvecklare inte har tooworry om servrar.  En stor del av traditionella programutveckling svarar frågor kring skalning, värd och lösningar toomeet hello kraven för programmet hello-övervakning.  Dessa frågor är tagit hand om med utan server, som en del av hello lösning.  Dessutom debiteras serverlösa program på en plan för förbrukningsbaserad.  Om programmet hello används aldrig, påförs en avgift aldrig.  De här funktionerna kan utvecklare toofocus enbart på hello affärslogik hello lösning.

hello kärntjänsterna i Azure runt utan Server är [Azure Functions](https://azure.microsoft.com/services/functions/) och [Azure Logikappar](https://azure.microsoft.com/services/logic-apps/).  Båda lösningarna följer hello principer ovan och utvecklare toobuild robust molnprogram med minimal kod.

## <a name="what-are-azure-functions"></a>Vad är Azure Functions?

Azure Functions är en lösning för att enkelt köra små delar av kod eller ”funktioner”, i hello molnet. Du kan skriva precis hello kod du behöver för hello problemet, utan att oroa en hela programmet eller hello infrastruktur toorun den. Funktioner kan göra utvecklingen ännu mer produktiv och du kan använda programmeringsspråk du föredrar, till exempel C#, F #, Node.js, Python eller PHP. Betalar bara för hello tiden koden körs och Azure skalas efter behov.

Om du vill använda toojump höger och kom igång med Azure Functions, börja med [skapa din första Azure-funktion](../azure-functions/functions-create-first-azure-function.md). Om du behöver mer teknisk information om funktioner finns hello [för utvecklare](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Vad är Azure Logic Apps?

Med Azure Logikappar ger ett sätt toosimplify och genomföra skalbara integreringar och arbetsflöden i hello molnet. Den ger en visuell designer toomodel och automatisera processen som en serie steg kallas ett arbetsflöde.  Det finns [många kopplingar](../connectors/apis-list.md) för molnet och lokala tjänster ansluta tooquickly serverlösa app-tooother API: er.  En logikapp börjar med en utlösare (som ”när ett konto läggs tooDynamics CRM-) och efter att börja många kombinationer åtgärder konverteringar och logik för villkoret.  Logic Apps är ett bra val när samordna olika Azure-funktionerna i en process, särskilt när hello processen kräver interaktion med ett externt system eller API.

tooget igång med Logic Apps genom att börja med [skapa din första logikapp](logic-apps-create-a-logic-app.md).  Om du behöver mer teknisk information om Logic Apps finns hello [för utvecklare](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Hur kan skapa och distribuera serverlösa program i Azure?

Azure tillhandahåller en omfattande uppsättning av verktyg för utveckling, distribution och hantering av serverlösa appar.  Appar kan skapas direkt i hello Azure-portalen eller med [tooling från Visual Studio](logic-apps-serverless-get-started-vs.md).  När ett program som har utvecklats kan det vara [distribueras direkt](logic-apps-create-deploy-template.md).  Azure tillhandahåller också övervakning för serverlösa appar.  Denna övervakning kan nås från hello Azure-portalen via hello API eller SDK eller integrerad verktygsuppsättning tooOMS och Application Insights.

## <a name="next-steps"></a>Nästa steg

* [Komma igång med att skapa en serverlösa app i Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Skapa en kund insikter instrumentpanel med utan Server](logic-apps-scenario-social-serverless.md)
* [Skapa en Distributionsmall för en logikapp](logic-apps-create-deploy-template.md)