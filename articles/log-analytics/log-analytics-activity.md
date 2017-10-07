---
title: aaaView Azure aktivitet loggar med Log Analytics | Microsoft Docs
description: "Du kan använda hello Azure aktivitetsloggar lösning tooanalyze och Sök hello Azure-aktivitetsloggen över alla dina Azure-prenumerationer."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: 171d0d604d03a5714a9599cc0b448fc5f6471f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-azure-activity-logs"></a>Visa Azure aktivitetsloggar

![Azure aktivitetsloggar symbol](./media/log-analytics-activity/activity-log-analytics.png)

hello aktivitet logganalys lösningen hjälper dig att analysera och söka hello [Azure aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) över alla dina Azure-prenumerationer. hello Azure-aktivitetsloggen är en logg som ger insikter om hello operationer för resurser i dina prenumerationer. hello aktivitetsloggen kallades tidigare *granskningsloggar* eller *Arbetsloggarna* eftersom den rapporterar händelser för dina prenumerationer.

Använder hello aktivitetsloggen, kan du bestämma hello *vad*, *som*, och *när* för någon skrivåtgärder (PUT, POST, ta bort) som har gjorts för hello resurser i din prenumeration. Du kan också förstå hello status för hello åtgärder och andra relevanta egenskaper. hello aktivitetsloggen innehåller inte skrivskyddade (GET) åtgärder eller åtgärder för resurser som använder hello klassiska distributionsmodellen.

När du ansluter din Azure aktivitet loggar tooLog Analytics kan du:

- Analysera hello aktivitetsloggar med fördefinierade vyer
- Analysera och Sök- och loggar från Azure-prenumerationer
- Behåll aktivitetsloggar mer än 90 dagar<sup>1</sup>
- Korrelera aktivitetsloggar med andra Azure-plattformen och programdata
- Se driften aggregeras efter status
- Visa trender för aktiviteter som händer på varje Azure-tjänster
- Rapport om auktorisering ändringar på alla dina Azure-resurser
- Identifiera eller problem påverkar dina resurser
- Använda loggen Sök toocorrelate användaraktiviteter, Autoskala operations, auktorisering ändringar och tjänstloggar hälsa tooother eller mått från din miljö

<sup>1</sup>som standard logganalys sparas din Azure aktivitetsloggar i 90 dagar, även om du är på hello kostnadsfria nivån. Eller, om du har en inställning för kvarhållning av arbetsytan mindre än 90 dagar. Om ditt arbetsområde har kvarhållning är längre än 90 dagar, behålls hello aktivitetsloggar för hello kvarhållningsperiod på arbetsytan.

Logganalys samlar in kostnadsfritt aktivitetsloggar och lagrar hello loggar under 90 dagar utan kostnad. Om du sparar loggar under längre tid än 90 dagar, kommer du betalar avgifter för kvarhållning av data för hello data som lagrats längre än 90 dagar.

När du är på hello kostnadsfritt prisnivån gäller inte aktivitetsloggar tooyour dagliga data förbrukning.

## <a name="connected-sources"></a>Anslutna källor

Till skillnad från de flesta andra logganalys-lösningar kan inte samlas in för aktivitetsloggar av agenter. Alla data som används av hello lösningen kommer direkt från Azure.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej | hello lösningen samlar inte in information från Windows-agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | hello lösningen samlar inte in information från Linux-agenter. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | hello lösningen samlar inte in information från agenter i en ansluten SCOM-hanteringsgrupp. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | hello lösningen samlar inte in information från Azure storage. |

## <a name="prerequisites"></a>Krav

- Du måste ha en Azure-prenumeration tooaccess logginformation för Azure-aktivitet.

## <a name="configuration"></a>Konfiguration

Utför följande steg tooconfigure hello aktivitet logganalys lösningen för dina arbetsytor hello.

1. Aktivera hello aktivitet logganalys lösningar från hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. Konfigurera aktiviteten loggar toogo tooyour logganalys-arbetsytan.
    1. I hello Azure-portalen, Välj din arbetsyta och klicka sedan på **Azure-aktivitetsloggen**.
    2. Klicka på hello prenumerationsnamn för varje prenumeration.  
        ![Lägg till prenumeration](./media/log-analytics-activity/add-subscription.png)
    3. I hello *SubscriptionName* bladet, klickar du på **Anslut**.  
        ![ansluta prenumeration](./media/log-analytics-activity/subscription-connect.png)

Om du lägger till hello-lösning med hjälp av hello OMS-portalen visas hello följande panelen. Logga in toohello Azure portal tooconnect en Azure-prenumeration tooyour arbetsyta.  
![utvärdering](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-hello-solution"></a>Med hello-lösning

När du lägger till hello aktivitet logganalys lösning tooyour arbetsytan hello **Azure aktivitetsloggar** panel har lagts tooyour översikt över instrumentpanelen. Den här panelen visar antalet hello antal Azure aktivitetsposter för hello Azure-prenumerationer som hello-lösningen har åtkomst till.

![Azure aktivitetsloggar sida vid sida](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Visa Azure aktivitet loggar

Klicka på hello **Azure aktivitetsloggar** panelen tooopen hello **Azure aktivitetsloggar** instrumentpanelen. hello instrumentpanelen innehåller hello blad i hello i den följande tabellen. Varje bladet visar upp too10 objekt som matchar att bladets kriterier för hello angetts scope och ett tidsintervall. Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på **se alla** längst ned hello hello bladet eller genom att klicka på hello bladet sidhuvud.

Loggdata för aktivitet visas bara *när* du har konfigurerat din aktivitet loggar toogo toohello lösning, så att du kan visa data innan dess.

| Bladet | Beskrivning |
| --- | --- |
| Azure aktivitet loggposter | Visar ett stapeldiagram för hello top Azure aktivitetsloggpost poster summor för hello datumintervall som du har valt och visar en lista över hello översta 10 aktivitet anropare. Klicka på hello stapeldiagram toorun en logg sökning efter <code>Type=AzureActivity</code>. Klicka på en anroparen objektet toorun en logg sökning returnerar alla aktiviteten loggposter för objektet. |
| Aktivitetsloggar efter Status | Visar ett ringdiagram för Azure log aktivitetsstatus för hello datumintervallet som du har valt. Visar en lista också en lista över hello översta tio status poster. Klicka på hello diagram toorun en logg sökning efter <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>. Klicka på en status objektet toorun en logg sökning returnerar alla aktiviteten loggposter för posten status. |
| Aktivitetsloggar per resurs | Visar hello Totalt antal resurser med aktivitetsloggar och visar hello upp tio resurser med posten antalet för varje resurs. Klicka på hello Totalt område toorun en logg sökning efter <code>Type=AzureActivity &#124; measure count() by Resource</code>, vilket visar alla Azure-resurser tillgängliga toohello lösning. Klicka på en resurs toorun en logg sökning returnera alla aktivitetsposter för den här resursen. |
| Aktivitetsloggar av resursen. | Visar hello Totalt antal resursproviders som producerar aktivitet loggar och visar hello tio. Klicka på hello Totalt område toorun en logg sökning efter <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, vilket visar alla Azure-resurs-leverantörer. Klicka på en resurs providern toorun en logg sökning returnerar alla aktivitetsposter för hello-providern. |

![Azure aktivitetsloggar instrumentpanelen](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Nästa steg

- Skapa en [avisering](log-analytics-alerts-creating.md) när en viss aktivitet inträffar.
- Använd [loggen Sök](log-analytics-log-searches.md) tooview detaljerad information från din aktivitetsloggar.
