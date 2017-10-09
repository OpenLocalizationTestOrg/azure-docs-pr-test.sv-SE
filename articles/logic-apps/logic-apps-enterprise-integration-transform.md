---
title: aaaConvert XML-data med transformeringar - Azure Logic Apps | Microsoft Docs
description: "Skapa transformeringar eller mapps tooconvert XML-data mellan formaten i logikappar med hjälp av hello Enterprise Integration-SDK"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise integration med XML-transformeringar
## <a name="overview"></a>Översikt
hello Enterprise integration transformeringen connector konverterar data från ett format tooanother format. Du kan till exempel ha ett inkommande meddelande som innehåller hello aktuellt datum hello YearMonthDay format. Du kan använda en transformering tooreformat hello datum toobe hello MonthDayYear format.

## <a name="what-does-a-transform-do"></a>Vad är en omvandling?
En transformering som kallas även en karta, består av källa XML-schema (hello indata) och ett mål XML-schema (hello utdata). Du kan använda olika inbyggda funktioner toohelp ändra eller kontrollera hello data, inklusive strängändringar, villkorlig tilldelningar, aritmetiska uttryck, datum tid formaterare och även slingor konstruktioner.

## <a name="how-toocreate-a-transform"></a>Hur toocreate en omvandling?
Du kan skapa en transformering/karta med hello Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas). När du är klar med att skapa och testa hello transformeringen överför hello transformeringen till ditt konto för integrering. 

## <a name="how-toouse-a-transform"></a>Hur toouse en transformering
När du har överfört hello transformering/mappa till kontot integration kan du använda den toocreate en logikapp. Hej logikapp körs din transformationer när hello logikapp utlöses (och det finns indata innehåll som måste omvandlas toobe).

**Här följer hello steg toouse en transformering**:

### <a name="prerequisites"></a>Krav

* Skapa ett konto för integrering och lägga till en karta tooit  

Nu när du har åtgärdat hello krav, är det tid toocreate logikappen:  

1. Skapa en logikapp och [länka det tooyour integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Läs toolink en logikapp integrering konto tooa") som innehåller hello kartan.
2. Lägg till en **begära** utlösaren tooyour logikapp  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Lägg till hello **transformera XML** åtgärden genom att först välja **lägga till en åtgärd**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Ange hello word *transformera* i hello Sök rutan toofilter alla hello åtgärder toohello något som du vill toouse  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Välj hello **transformera XML** åtgärd   
6. Lägg till hello XML **innehåll** som du omvandla. Du kan använda alla XML-data som visas i hello HTTP-begäran som hello **innehåll**. I det här exemplet väljer du hello brödtext hello HTTP-begäran som utlöste hello logikapp.
7. Välj hello namnet på hello **KARTAN** som du vill toouse tooperform hello omvandling. hello karta måste redan finnas i ditt konto för integrering. I ett tidigare steg gav du redan din logik app tooyour integrering åtkomstkonto som innehåller kartan.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Spara ditt arbete  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Nu är du klar med att ställa in kartan. Du kanske vill toostore hello omvandlas data i en LOB-program, till exempel SalesForce i ett verkligt program. Du kan enkelt som åtgärden toosend hello utdata för hello Omforma tooSalesforce. 

Nu kan du testa din transformeringen genom att göra en förfrågan toohello HTTP-slutpunkten.  

## <a name="features-and-use-cases"></a>Funktioner och användningsområden
* hello-transformation som skapats i en karta kan vara enkel, till exempel kopiera ett namn och adress från ett dokument tooanother. Eller så kan du skapa mer komplexa omformningar med hjälp av hello out box kartan åtgärder.  
* Flera kartan operations eller funktioner är tillgängliga, inklusive strängar, datum tidsfunktioner och så vidare.  
* Du kan göra en kopia direkt mellan hello scheman. I hello Mapper ingår i hello SDK, är så enkelt som att rita en linje som sammanbinder hello element i hello datakällans schema med deras motsvarigheter i hello mål schemat.  
* När du skapar en karta, kan du visa en grafisk representation av hello karta som visar alla hello relationer och länkar som du skapar.
* Använd hello Test kartan funktionen tooadd ett exempelmeddelande XML. Du kan testa hello karta du skapat och se hello genererade utdata med en enkel klickning.  
* Överför befintliga mappningar  
* Innehåller stöd för hello XML-format.

## <a name="learn-more"></a>Läs mer
* [Mer information om hello Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [Mer information om maps](../logic-apps/logic-apps-enterprise-integration-maps.md "Lär dig mer om enterprise integration maps")  

