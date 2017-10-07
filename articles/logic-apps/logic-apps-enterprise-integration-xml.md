---
title: "aaaWorking med XML-meddelanden i dina arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Bearbeta, verifiera, transformera och utöka XML meddelanden i logikappar och business-tooscenarios med hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: f90ae89fef0a4bd17286adbce398e573940bb790
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a>Validera och transformera XML, koda och avkoda flat-filer och utöka meddelanden funktioner i logic apps

Med logic apps kan ha du hello möjlighet tooprocess XML-meddelanden som du skickar och tar emot. Den här funktionen ingår i hello Enterprise-Integrationspaket. För de användarna som har en bakgrund för BizTalk Server hello Enterprise-Integrationspaket ger liknande förmågor tootransform och validera meddelanden kan arbeta med flat-filer, och även använda XPath tooenrich eller extrahera specifika egenskaper från ett meddelande. 

För användare som är nya toothis utrymme, utöka dessa funktioner hur du bearbetar meddelanden i arbetsflödet. Till exempel om du är i ett scenario med business-to-business och arbeta med specifika XML-scheman kan du använda hello Enterprise-Integrationspaket tooenhance hur företaget bearbetar dessa meddelanden. 

hello innehåller Enterprise-Integrationspaket: 

* [XML-verifiering](logic-apps-enterprise-integration-xml-validation.md "Lär dig mer om XML-meddelandevalidering") -validera en XML-meddelande för inkommande eller utgående mot ett visst schema.
* [XML-transformeringen](../logic-apps/logic-apps-enterprise-integration-transform.md "Lär dig mer om XML-meddelande omvandlingar och kartor") – konvertera eller anpassa en XML-meddelande baserat på dina krav eller hello kraven i en partner.
* [En platt Filkodning och flat fil avkodning](logic-apps-enterprise-integration-flatfile.md "Lär dig mer om kodning/avkodning flat fil") – koda eller avkoda en flat-fil. Till exempel SAP accepterar och skickar IDOC filer i platt format. Flera olika plattformar integration skapa XML-meddelanden, inklusive Logic Apps. Så kan du skapa en logikapp som använder hello flat fil-kodaren för ”konvertera” XML-filer tooflat filer. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) – utöka ett meddelande och extrahera specifika egenskaper från hello-meddelande. Du kan sedan använda hello extraheras egenskaper tooroute hello meddelandet tooa mål- eller en mellanliggande slutpunkt.

## <a name="try-it-out"></a>Prova
[Distribuera en fullt fungerande logikapp ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub-prov) med hjälp av hello XML-funktionerna i Logikappar i Azure.

## <a name="learn-more"></a>Läs mer
[Mer information om hello Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")
