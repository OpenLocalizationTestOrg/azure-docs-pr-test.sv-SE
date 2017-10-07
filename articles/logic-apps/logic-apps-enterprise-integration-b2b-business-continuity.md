---
title: "aaaDisaster återställning för B2B-integrering konto - Azure Logic Apps | Microsoft Docs"
description: "Logic Apps B2B-katastrofåterställning"
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
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B mellan region-katastrofåterställning

B2B arbetsbelastningar involverar pengar transaktioner som order och fakturor. Det är viktigt för ett företag tooquickly Återställ toomeet hello affärsnivå SLA överens med sina partner under en katastrofåterställning-händelse. Den här artikeln visar hur toobuild kontinuitet för företag planerar för B2B-arbetsbelastningar. 

* Disaster Recovery beredskap 
* Växla över toosecondary region under en katastrofåterställning-händelse 
* Infaller tooprimary region efter en katastrof-händelse

## <a name="disaster-recovery-readiness"></a>Disaster Recovery beredskap  

1. Ange en sekundär region och skapa en [integrering konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) i hello sekundär region.

2. Lägg till partners, scheman och avtal för hello krävs meddelandet flöden där hello kör status måste toobe replikeras toosecondary region integration konto.

   > [!TIP]
   > Kontrollera att det finns konsekvens i hello integration konto artefakts namngivningskonvention över regioner. 

3. toopull hello kör status från hello primära regionen, skapa en logikapp i hello sekundär region. 

   Den här logikapp ska ha en *utlösaren* och en *åtgärd*. 
   hello utlösaren ska ansluta tooprimary region integration konto och hello-åtgärd ska ansluta toosecondary region integration konto. 
   Baserat på hello tidsintervall hello utlösare avsöker hello primära region som kör status tabell och hämtar hello nya poster, om sådana finns. hello åtgärden uppdaterar dem toosecondary region integration konto. 
   Detta hjälper tooget inkrementell Körningsstatus från primära region toosecondary region.

4. Kontinuitet för företag i Logic Apps integration kontot är utformad toosupport baserat på B2B - X12 AS2 och EDIFACT-protokoll. toofind beskrivs stegen, Välj hello respektive länkar.

5. Hej rekommendation är toodeploy alla primära region resurser i en sekundär region för. 

   Primär region resurser inkluderar Azure SQL Database eller Azure Cosmos DB, Azure Service Bus och Azure Event Hubs används för meddelanden, Azure API Management och hello Azure Logic Apps-funktionen i Azure App Service.   

6. Upprätta en anslutning från en primär region tooa sekundär region. toopull hello kör status från en primär region, skapa en logikapp i en sekundär region. 

   Hej logikapp ska ha en utlösare och en åtgärd. 
   hello utlösaren ska ansluta tooa primär region integration konto. 
   hello åtgärden bör ansluta tooa sekundär region integration konto. 
   Baserat på hello tidsintervall hello utlösare avsöker hello primära region som kör status tabell och hämtar hello nya poster, om sådana finns. 
   hello åtgärden uppdaterar dem tooa sekundär region integration konto. 
   Den här processen kan tooget inkrementell Körningsstatus från hello primär region toohello sekundär region.

Kontinuitet för företag i ett konto för Logic Apps-integration stöder baserat på hello B2B-protokollen X12, AS2 och EDIFACT. Detaljerade anvisningar om hur du använder X12 och AS2 finns [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) och [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) i den här artikeln.

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a>Växla över tooa sekundär region under en katastrofåterställning-händelse

Under en katastrofåterställning händelse, när hello primära region inte är tillgänglig för affärskontinuitet, trafiken toohello sekundär region. En sekundär region hjälper en business toorecover fungerar snabbt toomeet hello Återställningspunktmål/Återställningstidsmål överens med sina partner. Det minskar också ansträngningar toofail över från en region tooanother region. 

Det finns en förväntade fördröjning vid kopiering av kontrollen siffror från en primär region tooa sekundär region. tooavoid skicka dubbla genererade kontrollen siffror toopartners under en katastrofåterställning händelse är hello rekommendation tooincrement hello kontrollen siffror i hello sekundär region avtal med hjälp av [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a>Återgå tooa primär region efter katastrofåterställning händelse

toofall tillbaka tooa primär region när den är tillgänglig, Följ dessa steg:

1. Stoppa ta emot meddelanden från partner i hello sekundär region.  

2. Öka hello genereras kontrollen siffror för alla hello primär region avtal med hjälp av [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Trafiken från hello sekundär region toohello primär region.

4. Kontrollera att hello logikappen som skapats i hello sekundär region för att ta emot kör status från hello primär region har aktiverats.

## <a name="x12"></a>X12 

Kontinuitet för företag för EDI X 12 dokument baserat på kontrollen siffror:

> [!TIP]
> Du kan också använda hello [X12 snabb start mallen](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic apps. Skapa primära och sekundära integrationskonton är förutsättningar toouse hello mallen. hello kan mall toocreate två logikappar, en för mottagna kontrollen tal och en annan för genererade kontrollen tal. Respektive utlösare och åtgärder skapas i hello logikappar ansluter hello utlösaren toohello primära integration konto och hello åtgärdskontot toohello sekundära integrering.

**Förutsättningar**

tooenable katastrofåterställning för inkommande meddelanden väljer hello dubbla Kontrollera inställningarna i hello X12 avtal får inställningarna.

![Välj kontrollen av dubblett-inställningar](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i en sekundär region.    

2. Sök på **X12**, och välj **X12-när ett kontrollnummer ändras**.   

   ![Sök efter X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   hello utlösaren uppmanar tooestablish ett anslutningskonto tooan integrering. 
   hello utlösaren ska vara anslutna tooa primär region integration konto.

3. Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.   

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. Hej **DateTime toostart kontroll nummer sync** inställningen är valfri. Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.   

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Välj **nytt steg** > **lägga till en åtgärd**.

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Sök på **X12**, och välj **X12-Lägg till eller uppdatera kontroll siffror**.   

   ![Lägg till eller uppdatera kontroll siffror](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. tooconnect tooa sekundär region integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton. Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**. 

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.

   ![Dynamiskt innehåll fält](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Baserat på hello tidsintervall hello utlösare avsöker hello primära region som tagits emot kontrollen register och hämtar hello nya poster. 
   hello åtgärden uppdaterar hello posterna i hello sekundär region integration konto. 
   Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.   

   ![Register för kontrollen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från en primär region tooa sekundär region. Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet. 

## <a name="edifact"></a>EDIFACT 

Kontinuitet för företag för EDI EDIFACT-dokument baseras på kontrollen siffror.

**Förutsättningar**

tooenable katastrofåterställning för inkommande meddelanden väljer hello dubbla Kontrollera inställningarna i din EDIFACT-avtal får inställningarna.

![Välj kontrollen av dubblett-inställningar](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i en sekundär region.    

2. Sök på **EDIFACT**, och välj **EDIFACT - när ett kontrollnummer ändras**.

   ![Sök efter EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   hello utlösaren uppmanar tooestablish ett anslutningskonto tooan integrering. 
   hello utlösaren ska vara anslutna tooa primär region integration konto. 

3. Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.    

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. Hej **DateTime toostart kontroll nummer sync** inställningen är valfri. Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.    

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Välj **nytt steg** > **lägga till en åtgärd**.    

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Sök på **EDIFACT**, och välj **EDIFACT - Lägg till eller uppdatera kontroll siffror**.   

   ![Lägg till eller uppdatera kontroll siffror](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. tooconnect tooa sekundär region integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton. Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**.

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.   

   ![Dynamiskt innehåll fält](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Baserat på hello tidsintervall hello utlösare avsöker hello primära region som tagits emot kontrollen register och hämtar hello nya poster.
   hello åtgärden uppdaterar hello poster toohello sekundär region integration konto. 
   Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.

   ![Register för kontrollen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från en primär region tooa sekundär region. Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet. 

## <a name="as2"></a>AS2 

Kontinuitet för företag för dokument som använder hello AS2-protokollet är baserat på hello-meddelande-ID och hello MIC värdet.

> [!TIP]
> Du kan också använda hello [AS2 Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic apps. Skapa primära och sekundära integrationskonton är förutsättningar toouse hello mallen. hello mallen hjälper dig att skapa en logikapp som har en utlösare och en åtgärd. Hej logikapp skapar en anslutning från ett konto för utlösaren tooa primära integrering och ett åtgärdskonto tooa sekundära integrering.

1. Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i hello sekundär region.  

2. Sök på **AS2**, och välj **AS2 - när ett MIC värdet skapas**.   

   ![Sök efter AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   En utlösare efterfrågar tooestablish ett anslutningskonto tooan integrering. 
   hello utlösaren ska vara anslutna tooa primär region integration konto. 
   
3. Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. Hej **DateTime toostart MIC värdet sync** inställningen är valfri. Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.   

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Välj **nytt steg** > **lägga till en åtgärd**.  

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Sök på **AS2**, och välj **AS2 - Lägg till eller uppdatera MIC innehållet**.  

   ![MIC tillägg eller uppdatering](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. tooconnect tooa sekundära integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton. Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**.

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.   

   ![Dynamiskt innehåll](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Baserat på hello tidsintervall hello utlösare avsöker hello primär region tabell och hämtar hello nya poster. hello åtgärden uppdaterar dem toohello sekundär region integration konto. 
   Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.  

   ![Primär region tabell](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från hello primär region toohello sekundär region. Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet. 

## <a name="next-steps"></a>Nästa steg

[Övervaka B2B-meddelanden](logic-apps-monitor-b2b-message.md)

