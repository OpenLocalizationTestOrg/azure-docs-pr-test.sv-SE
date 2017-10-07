---
title: "aaaVisualizing Azure Network Security Group flödet loggar med Power BI | Microsoft Docs"
description: "Den här sidan beskrivs hur toovisualize NSG flödet loggar med Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Visualizing Nätverkssäkerhetsgruppen flöde loggar med Power BI

Nätverkssäkerhetsgruppen flöde loggar Tillåt tooview information om ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper. Loggarna flödet visar utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP, källan/målet Port Protocol), och om hello trafik tillåts eller nekas.

Det kan vara svårt toogain insikter om flödet loggningsdata genom att söka manuellt hello loggfiler. I den här artikeln får en lösning toovisualize din senaste flödet loggar och lär dig mer om trafik i nätverket.

## <a name="scenario"></a>Scenario

I följande scenario hello, ansluta vi Power BI desktop toohello storage-konto som vi har konfigurerats som hello sink för våra data för NSG flöda loggning. När vi har anslutit tooour storage-konto Power BI hämtar och analyserar hello loggar tooprovide en bild av hello trafik som loggats av nätverkssäkerhetsgrupper.

Använda hello visuell information tillhandahålls i hello-mallen som du kan undersöka:

* Övre Talkers
* Serien flöda tidsdata genom riktning och regeln beslut
* Flöden av Network Interface MAC-adress
* Flöden av NSG och regel
* Flöden av målport

hello-mall kan redigeras så att du kan ändra den tooadd nya data, visuell information, eller redigera frågor toosuit dina behov.

## <a name="setup"></a>Konfiguration

Du måste ha nätverket grupp flöda säkerhetsloggning aktiverad på en eller flera Nätverkssäkerhetsgrupper i ditt konto innan du börjar. Anvisningar om hur du aktiverar Network Security flöda loggar finns i följande artikel toohello: [introduktion tooflow loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).

Du måste också ha hello Power BI Desktop-klienten är installerad på datorn och tillräckligt med ledigt utrymme på din dator toodownload och Läs in hello loggdata som finns i ditt lagringskonto.

![Visio-diagram][1]

### <a name="steps"></a>Steg

1. Ladda ned och öppna hello följande Power BI-mallen i hello Power BI Desktop Application [nätverk Watcher PowerBI flöde loggar mall](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Ange frågeparametrar Hej krävs
    1. **StorageAccountName** – anger toohello namnet på hello storage-konto som innehåller hello NSG flödet loggar som du skulle t.ex tooload och visualisera.
    1. **NumberOfLogFiles** – anger hello antal loggfiler som du skulle t.ex. toodownload och visualisera i Power BI. Till exempel om 50 anges hello 50 senaste loggfiler. FF som vi har 2 NSG: er som är aktiverad och konfigurerad toosend NSG flödet loggar toothis konto och sedan hello senaste 25 timmarna loggar kan visas.

    ![Power BI main][2]

1. Ange hello åtkomstnyckeln för ditt lagringskonto. Du kan hitta giltig åtkomstnycklar genom att gå tooyour storage-konto i hello Azure portal och markera **åtkomstnycklar** hello inställningsmenyn. Klicka på **Anslut** tillämpa ändringarna.

    ![Snabbtangenter][3]

    ![åtkomstnyckeln 2][4]

4.  Loggarna är hämta och analysera och du kan nu använda hello förskapade visuell information.

## <a name="understanding-hello-visuals"></a>Förstå hello visuell information

Förutsatt att i hello mallen är en uppsättning visuell information som hjälper passar för hello NSG flöda loggdata. hello visar följande bilder ett exempel på vilka hello instrumentpanel ser ut när fylls i med data. Nedan igenom vi varje visual i större detalj 

![powerbi][5]
 
hello upp Talkers visual visar hello IP-adresser som har initierat hello de flesta anslutningar över hello tidsperioden. hello storleken på hello rutor motsvarar toohello relativa antalet anslutningar. 

![toptalkers][6]

hello visar följande tid serien hello Antal flöden under hello period. hello övre diagrammet åtskilda med hello flödesriktning och hello lägre åtskilda med hello beslut om (Tillåt eller neka). Med den här visuella informationen du undersöka trenderna trafik över tid, och upptäcka eventuella onormala toppar eller neka trafik eller trafiksegmenteringen.

![flowsoverperiod][7]

hello visar följande diagram hello flöden per nätverksgränssnitt med hello övre åtskilda med flödesriktning och hello lägre åtskilda med beslut. Med den här informationen kan du få insikter om vilka av dina virtuella datorer kommunicerat hello de flesta relativa tooothers och om trafik tooa specifik VM används tillåts eller nekas.

![flowspernic][8]

hello följande hjul ringdiagram visar en sammanfattning av flöden av målport. Med den här informationen kan du visa hello vanligaste målportar används i hello angivna perioden.

![Ring][9]

hello visar följande stapeldiagram hello flödet av NSG och regeln. Med den här informationen visas hello NSG: er som är ansvarig för hello de flesta trafik och hello fördelning av trafik på en NSG av regeln.

![barchart][10]
 
hello följande information diagram visar information om hello NSG: er finns i hello loggar kan hello Antal flöden som avbildas under hello tid och hello datum hello tidigaste loggen avbildas. Den här informationen ger dig en uppfattning om vilken NSG: er loggas och hello datumintervall för flöden.

![infochart1][11]

![infochart2][12]

Den här mallen innehåller hello följande utsnitt tooallow du tooview endast hello data som du är mest intresserad av. Du kan filtrera på resursgrupper, NSG: er och regler. Du kan också filtrera på 5-tuppel information och beslut hello tid hello loggen har skrivits.

![utsnitt][13]

## <a name="conclusion"></a>Slutsats

Beskrevs i det här scenariot att genom att använda Network Security Group flöda loggarna som tillhandahålls av Nätverksbevakaren och Power BI kan vi är kan toovisualize och förstå hello trafik. Använd hello som mall Power BI hämtar hello loggarna direkt från lagring och bearbetar dem lokalt. Tidsåtgång tooload hello mallen varierar beroende på hello antalet filer som begärs och totala storleken på filerna hämtas.

Känna sig fria toocustomize mallen för dina behov. Det finns många flera olika sätt som du kan använda Power BI med Network Security Group flöda loggar. 

## <a name="notes"></a>Anteckningar

* Loggar som standard lagras i`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Om det finns andra data i en annan katalog de hello frågor toopull och bearbeta hello data måste ändras.

* hello tillhandahålls mallen rekommenderas inte för användning med mer än 1 GB loggar.

* Om du har en stor mängd loggar rekommenderar vi att du undersöker en lösning med hjälp av ett annat datalager som Data Lake eller SQL server.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toovisualize NSG-flöde loggar med hello Elastick-stacken genom att besöka [visualisera nätverket trafik mönster tooand från dina virtuella datorer med hjälp av verktyg för öppen källkod](network-watcher-using-open-source-tools.md)

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
