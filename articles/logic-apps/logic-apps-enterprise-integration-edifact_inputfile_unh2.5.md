---
title: "aaaLogic appar B2B edifact avkoda lösa UNH2.5 - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps B2B edifact avkoda lösa UNH2.5"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a>Hur toohandle EDIFACT-dokument med UNH2.5 segment
Om UNH2.5 finns i hello EDIFACT dokumentet används för schemat sökning. 

Exempel: hello UNH fältet är **EAN008** i hello EDIFACT-meddelande  
UNH + SSDD1 + ORDER: D: 03B: FN:**EAN008**'  

Steg toofollow toohandle hello-meddelande 
1. Uppdatera hello schema
2. Kontrollera inställningarna för hello-avtal  

## <a name="update-hello-schema"></a>Uppdatera hello schema
tooprocess hello-meddelande måste toodeploy ett schema med hello UNH2.5 rotnodens namn.  För ett exempel hello schemanamnet rot skulle vara **EFACT_D03B_ORDERS_EAN008**  

Du skulle ha en enskild schemat toodeploy för varje D03B_ORDERS med ett annat UNH2.5 segment.  

## <a name="add-schema-toohello-edifact-agreement"></a>Lägga till schemat toohello EDIFACT-avtal
### <a name="edifact-decode"></a>EDIFACT avkoda
tooDecode Hej inkommande meddelande, konfigurera hello schemat i hello EDIFACT-avtal får inställningar
1. Lägg till hello schemat toohello integrering konto    
2. Konfigurera hello schemat i hello EDIFACT-avtal får inställningar. 
3. Välj EDIFACT-avtal och klickar på **redigera som JSON**.  Lägg till UNH2.5 värde i hello Receive avtal **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT koda
tooEncode Hej inkommande meddelande, konfigurera hello schemat i hello EDIFACT-avtal skicka inställningar
1. Lägg till hello schemat toohello integrering konto    
2. Konfigurera hello schemat i hello EDIFACT-avtal skicka inställningar. 
3. Välj EDIFACT-avtal och klickar på **redigera som JSON**.  Lägg till UNH2.5 värde i hello skicka avtal **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Nästa steg
* [Mer information om integration konto avtal](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  