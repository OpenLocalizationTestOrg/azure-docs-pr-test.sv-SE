---
title: "aaaCreate arbetsflöden från mallar - Azure Logic Apps | Microsoft Docs"
description: "Komma igång - snabbt skapa arbetsflöden med hjälp av Azure Logikapp mallar tooconnect appar och integrera data."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0127523662a12076772b52968f1e2f9cbad72a1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a>Konfigurera ett arbetsflöde med en förskapad mall eller mönster tooget snabbt igång

## <a name="what-are-logic-app-templates"></a>Vad är logic app mallar
En mall för logik-app är en förskapad logikapp som du kan använda tooquickly komma igång med din egen arbetsflöde. 

Dessa mallar är ett bra sätt toodiscover olika mönster som kan skapas med hjälp av logic apps. Du kan antingen använda mallarna som-är eller ändra dem. toofit ditt scenario.

## <a name="overview-of-available-templates"></a>Översikt över tillgängliga mallar
Det finns många tillgängliga mallar som för närvarande är publicerade i hello logik app plattform. Vissa exempel kategorier, samt hello typ av kopplingar som används i dem, i listan nedan.

### <a name="enterprise-cloud-templates"></a>Företagsmallar i molnet
Mallar som integrerar Dynamics CRM, Salesforce, rutan, Azure Blob och övriga kopplingar för ditt företag moln behov. Några exempel av vad som kan göras med dessa mallar innehåller ordna leads och säkerhetskopiering av filen för företagets data.

### <a name="enterprise-integration-pack-templates"></a>Företagsmallar integration pack
Konfigurationer av VETER (validera, extrahera, transformera, utöka, vidarebefordra) pipelines, tar emot en X12 EDI-dokumentet via AS2 samt omvandla det tooXML, som X12 och AS2 meddelande hantering.

### <a name="protocol-pattern-templates"></a>Protokollet mönster mallar
Dessa mallar består av logikappar med protokollet mönster, till exempel begäran och svar över HTTP samt integreringar över FTP- och SFTP. Använd dessa eftersom de finns eller som bas för att skapa mer komplexa protocol-mönster.  

### <a name="personal-productivity-templates"></a>Personlig produktivitet mallar
Mönster toohelp förbättra personlig produktivitet innehåller mallar som dagliga Påminn, göra viktiga arbetsobjekt till att göra-listor och automatisera tidskrävande uppgifter ned tooa enanvändarläge steg.

### <a name="consumer-cloud-templates"></a>Konsumenten molnet mallar
Enkel mallar som integreras med tjänster för social media som Twitter, Slack och e-post och slutligen kompatibla för att stärka sociala medier marknadsföringskampanjer. Dessa inkluderar även mallar, till exempel grumligt kopierar, som kan hjälpa dig att öka produktiviteten genom att spara tid på traditionellt återkommande uppgifter. 

## <a name="how-toocreate-a-logic-app-using-a-template"></a>Hur toocreate en logikapp med en mall
tooget igång med en app logikmall gå in hello logik app designer. Om du anger hello designer genom att öppna en befintlig logikapp, laddas hello logikapp automatiskt i designern vyn. Men om du skapar en ny logikapp kan se du hello-skärmen nedan.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Du kan antingen välja toostart med en tom logikapp eller en förskapad mall från den här skärmen. Om du väljer en av hello mallar tillhandahålls med ytterligare information. I det här exemplet använder vi hello *när en ny fil skapas i Dropbox, kopiera den tooOneDrive* mall.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Om du väljer toouse hello mallen kan bara välja hello *använder den här mallen* knappen. Du ombeds toosign i tooyour konton baserat på vilka kopplingar hello-mallen använder. Eller om du har redan en anslutning med dessa kopplingar kan du välja fortsätta som visas nedan.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Efter hello anslutning och välja *fortsätta*, hello logikappen öppnas i designern.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

I hello-exemplet ovan, vilket är fallet hello med många mallar kan hello obligatorisk egenskap fälten fyllas i inom hello kopplingar; vissa kan dock fortfarande kräver ett värde innan du kan tooproperly distribuera hello logikapp. Om du försöker toodeploy utan att ange några hello fält saknas, meddelas du med ett felmeddelande.

Om du inte vill tooreturn toohello mall viewer, Välj hello *mallar* knapp i hello övre navigeringsfältet. Genom att växla tillbaka toohello mall viewer förlorar du alla osparade pågår. Tidigare tooswitching tillbaka till mallen viewer visas ett meddelande om detta varningsmeddelande.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a>Hur toodeploy en logikapp skapas från en mall
När du har lästs in din mall och gjort önskade ändringar, kan du välja hello spara knappen i hello övre vänstra hörnet. Detta sparar och publicerar din logikapp.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Om du skulle t.ex. Mer information om hur tooadd fler steg i en befintlig mall för logik app eller göra ändringar i allmänhet kan du läsa mer i [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

