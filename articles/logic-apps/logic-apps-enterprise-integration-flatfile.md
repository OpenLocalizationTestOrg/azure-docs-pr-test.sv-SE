---
title: aaaEncode eller avkoda flat-filer i Azure logikappar | Microsoft Docs
description: Hur toouse hello filen fil-kodaren eller avkodarens i hello Enterprise-Integrationspaket i dina logic apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 2c295586625fd84366ec7cbafdcebf0489ba234d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Översikt över enterprise integration med flat-filer

Du kanske vill tooencode XML innehåll innan du skickar den tooa affärspartner i ett scenario med business-to-business (B2B). Du kan använda hello flat filkodningen connector toodo detta i en logikapp. Hej logikappen som du skapar kan hämta dess XML innehåll från en mängd olika källor, till exempel från en HTTP-begäran-utlösare, från ett annat program eller även från någon av hello många [kopplingar](../connectors/apis-list.md). Mer information om logikappar finns hello [logic apps dokumentationen](logic-apps-what-are-logic-apps.md "Lär dig mer om logikappar").  

## <a name="create-hello-flat-file-encoding-connector"></a>Skapa hello flat fil kodning koppling
Följ dessa steg tooadd kodning connector tooyour logikapp en flat-fil.

1. Skapa en logikapp och [länka det tooyour integrering konto](logic-apps-enterprise-integration-accounts.md "Läs toolink en logikapp integrering konto tooa"). Det här kontot innehåller hello schema som du vill använda tooencode hello XML-data.  
2. Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.  
   ![Skärmbild av utlösare tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
3. Lägg till hello flat filkodningen åtgärd, enligt följande:
   
    a. Välj hello **plus** tecken.
   
    b. Välj hello **lägga till en åtgärd** länk (visas när du har valt hello plustecken).
   
    c. Skriv i sökrutan hello *Flat* toofilter alla hello åtgärder toohello något som du vill toouse.
   
    d. Välj hello **Flat Filkodning** alternativet hello-listan.   
   ![Skärmbild av Flat fil kodning](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
4. På hello **Flat Filkodning** dialogrutan, Välj hello **innehåll** textruta.  
   ![Skärmbild av innehåll textruta](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
5. Välj hello body-tagg som hello innehåll som du vill tooencode. hello body-taggen ska fylla hello innehåll fältet.     
   ![Skärmbild av body-tagg](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. Välj hello **schemanamnet** listruta och välj hello schema som du vill använda toouse tooencode hello indata innehåll.    
   ![Skärmbild av schemanamnet listruta](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
7. Spara ditt arbete.   
   ![Skärmbild av spara ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Nu är du klar med att ställa in din kodning anslutningstjänst flat-fil. Du kanske vill toostore hello-kodade data i en line-of-business-program, till exempel Salesforce i ett verkligt program. Eller så kan du skicka den kodade data tooa handelspartner. Du kan enkelt lägga till åtgärden toosend hello utdata för hello kodning åtgärden tooSalesforce eller tooyour handelspartner, med hjälp av någon av hello andra kopplingar som har angetts.

Nu kan du testa din koppling genom att göra en förfrågan toohello HTTP-slutpunkten och inklusive hello XML-innehåll i hello brödtext hello-begäran.  

## <a name="create-hello-flat-file-decoding-connector"></a>Skapa hello flat fil avkoda connector

> [!NOTE]
> toocomplete dessa steg, behöver du toohave en schemafil redan överförts till integration konto.

1. Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.  
   ![Skärmbild av utlösare tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
2. Lägg till hello flat fil avkoda åtgärd, enligt följande:
   
    a. Välj hello **plus** tecken.
   
    b. Välj hello **lägga till en åtgärd** länk (visas när du har valt hello plustecken).
   
    c. Skriv i sökrutan hello *Flat* toofilter alla hello åtgärder toohello något som du vill toouse.
   
    d. Välj hello **Flat fil avkoda** alternativet hello-listan.   
   ![Skärmbild av Flat fil avkoda alternativet](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
3. Välj hello **innehåll** kontroll. Detta genererar en lista över hello innehåll från tidigare steg som du kan använda som hello innehåll toodecode. Observera att hello *brödtext* från hello inkommande HTTP-begäran är tillgängliga toobe används som hello innehåll toodecode. Du kan också ange hello innehåll toodecode direkt i hello **innehåll** kontroll.     
4. Välj hello *brödtext* tagg. Meddelande hello body-taggen är nu i hello **innehåll** kontroll.
5. Välj hello namnet på hello schema som du vill toouse toodecode hello innehåll. hello följande skärmbild visar att *OrderFile* är hello valda schemanamnet. Den här schemanamnet har överförts hello integration hänsyn tidigare.
   
   ![Skärmbild av Flat fil avkodning av dialogrutan](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Spara ditt arbete.  
   ![Skärmbild av spara ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Nu är du klar med att ställa in din flat fil avkodning av anslutningen. Du kanske vill toostore hello avkoda data i ett line-of-business-program, till exempel Salesforce i ett verkligt program. Du kan enkelt lägga till åtgärden toosend hello utdata för hello avkoda åtgärden tooSalesforce.

Nu kan du testa din koppling genom att göra en förfrågan toohello HTTP-slutpunkten och inklusive hello XML-innehåll som du vill toodecode i hello brödtext hello-begäran.  

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket").  

