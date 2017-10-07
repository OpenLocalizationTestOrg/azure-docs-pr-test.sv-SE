---
title: "aaaVisualize nätverk trafikmönster med öppen källkod verktyg och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan beskrivs hur toouse Nätverksbevakaren paket avbilda med Capanalysis toovisualize trafik mönster tooand från dina virtuella datorer."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a>Visualisera nätverket trafik mönster tooand från dina virtuella datorer med hjälp av verktyg för öppen källkod

Paketet insamlingar innehålla nätverksdata som gör att du tooperform nätverk dataforensik och djup paketinspektion. Det finns många öppnas källa verktyg som du kan använda tooanalyze paket insamlingar toogain insikter om nätverket. Ett sådant verktyg är CapAnalysis, ett verktyg för öppen källkod nätverkspaket avbilda visualiseringen. Visualisera avbilda paketdata är ett bra sätt tooquickly härleda insikter på trafikmönster och avvikelser i nätverket. Visualiseringar ger också möjlighet att dela sådana insikter på ett sätt som är lätta att använda.

Azures Nätverksbevakaren ger du hello möjlighet toocapture värdefulla data genom att låta dig tooperform paket samlar in i nätverket. I den här artikeln får en genomgång av hur toovisualize och få insikter från paketet fångar med Nätverksbevakaren CapAnalysis.

## <a name="scenario"></a>Scenario

Du har en enkel webbapp som distribueras på en virtuell dator i Azure vill toouse öppen källkod verktyg toovisualize dess nätverk trafik tooquickly identifiera flödet mönster och alla eventuella avvikelser. Med Nätverksbevakaren, kan du hämta en paketinsamling för din nätverksmiljö och spara dem direkt på ditt lagringskonto. CapAnalysis kan mata in hello paketinsamling direkt från hello storage-blob och visualisera dess innehåll.

![scenario][1]

## <a name="steps"></a>Steg

### <a name="install-capanalysis"></a>Installera CapAnalysis

tooinstall CapAnalysis på en virtuell dator som du kan se toohello officiella anvisningarna här https://www.capanalysis.net/ca/how-to-install-capanalysis.
I ordning åt CapAnalysis måste vi tooopen port 9877 på den virtuella datorn genom att lägga till en ny regel för inkommande säkerhetsregler. Mer information om hur du skapar regler i Nätverkssäkerhetsgrupper finns för[skapa regler i en befintlig NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg). När hello regeln har lagts bör du kunna tooaccess CapAnalysis från`http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a>Använd Azure Nätverksbevakaren toostart ett paket avbilda session

Nätverksbevakaren tillåter toocapture paket tootrack trafik till och från en virtuell dator. Du kan se toohello instruktionerna på [hantera paket som samlar in med Nätverksbevakaren](network-watcher-packet-capture-manage-portal.md) toostart en paketet avbildningssessionen. Den här paketinsamling kan lagras i en lagring blob toobe nås av CapAnalysis.

### <a name="upload-a-packet-capture-toocapanalysis"></a>Ladda upp en avbildning tooCapAnalysis för paketet
Du kan ladda upp en paketinsamling vidtagit nätverksbevakaren hello ”importera från URL” fliken och tillhandahålla en länk toohello lagringsblob där hello paketinsamling lagras direkt.

När du anger en länk tooCapAnalysis se till att tooappend en SAS-token toohello storage blob-URL.  toodo detta, navigera tooShared åtkomstsignatur från hello lagringskonto, ange hello behörigheter och tryck på hello generera SAS knappen toocreate en token. Du kan sedan lägga till den här SAS token toohello paket avbilda storage blob-URL.

hello resulterande URL: en kommer att se ut ungefär så här: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>När paketet avbildas

CapAnalysis erbjuder olika alternativ toovisualize din paketinsamling, varje med analys från olika perspektiv. Med dessa visual sammanfattningar du förstå trenderna nätverket trafik och snabbt hitta några ovanliga aktiviteten. Några av dessa funktioner visas i följande lista hello:

1. Flödet tabeller

    Den här tabellen kan du hello lista med flödena i hello paketdata hello tidsstämpel som är associerade med hello flöden och hello olika protokoll som är associerade med hello flödet som källa och mål-IP.

    ![capanalysis flödar sida][5]

1. Översikt över Protocol

    Det här fönstret kan du tooquickly Se hello distribution av nätverkstrafik via hello olika protokoll och geografiska områden.

    ![Översikt över capanalysis protocol][6]

1. Statistik

    Det här fönstret kan du tooview trafik nätverksstatistik – byte skickas och tas emot från käll- och IP-adresser, flöden för varje hello käll- och IP-adresser, protokoll som används för olika flöden och hello varaktighet för flöden.

    ![capanalysis statistik][7]

1. geomap

    Det här fönstret ger dig en kartvyn av nätverkstrafiken, med färger skalning toohello mängden trafik från varje land. Du kan välja markerade länder tooview ytterligare flödet av statistik, t.ex hello andel av data som skickas och tas emot från IP-adresser i det landet.

    ![geomap][8]

1. Filter

    CapAnalysis innehåller en uppsättning filter för snabb analys av specifika paket. Exempelvis kan du välja toofilter hello data av protokollet toogain specifika insikter om den del av trafiken.

    ![filter][11]

    Besök [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn mer om alla CapAnalysis funktioner.

## <a name="conclusion"></a>Slutsats

Nätverket Watcher paket capture-funktionen kan du toocapture hello data nödvändiga tooperform nätverk dataforensik och bättre förstå trafik på nätverket. I det här scenariot visade hur paket insamlingar från Nätverksbevakaren enkelt kan integreras med öppen källkod visualiseringsverktyg. Med öppen källkod som samlar in CapAnalysis toovisualize paket kan du utföra djup paketinspektion och snabbt identifiera trender i nätverkstrafiken.

## <a name="next-steps"></a>Nästa steg

toolearn mer information om NSG flödet loggar finns [NSG flödet loggar](network-watcher-nsg-flow-logging-overview.md)

Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
