---
title: "aaaAzure Resource health översikt | Microsoft Docs"
description: "Översikt över Azure Resource health"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: f06153864090487829f717dc3e8972c78a4a58af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a>Översikt över Azure-resurs-hälsa
 
Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser. Det informerar om hello aktuella och tidigare dina resursers hälsa och hjälper dig att minska problem. Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.

Medan [Azure Status](https://status.azure.com) informerar om tjänsten problem som påverkar en bred uppsättning Azure-kunder, resurshälsa ger dig en anpassad instrumentpanel hello hälsotillståndet hos dina resurser. Resurshälsa visar hello alltid resurserna var inte tillgängliga i hello förfallit till betalning tooAzure problem med tjänsten. Detta gör det enkelt för du toounderstand om ett SLA har överskridits. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Vad som anses en resurs och hur fungerar resurshälsa bestämmer om en resurs är felfri?
En resurs är en instans av en resurstyp som erbjuds av en Azure-tjänst via Azure Resource Manager, till exempel: en virtuell dator, ett webbprogram eller en SQL-databas.

Resurshälsa är beroende av signaler sänds av hello olika Azure-tjänster tooassess om en resurs är felfri eller inte. Om en resurs är i feltillstånd, analyserar resurshälsa ytterligare information toodetermine hello källan hello problemet. Den visar även de åtgärder som Microsoft tar toofix hello problem eller vad du kan göra tooaddress hello orsaka hello problemet. 

Granska hello fullständig lista över resurstyper och hälsa checkar in [Azure resource health](resource-health-checks-resource-types.md) för mer information om hur hälsa utvärderas.

## <a name="health-status-provided-by-resource-health"></a>Hälsostatus som tillhandahålls av resurshälsa
hello hälsotillståndet för en resurs är en av följande statusar hello:

### <a name="available"></a>Tillgänglig
hello-tjänsten har inte identifierat eventuella händelser som påverkar hello hälsotillstånd hello resurs. I fall där hello resurs har återställts från oplanerade driftsavbrott under hello senaste 24 timmarna visas hello **har nyligen återställts** meddelande.

![Resource health tillgänglig virtuell dator](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Inte tillgänglig
hello-tjänsten har upptäckt en pågående plattform eller -plattform händelse som påverkar hello hälsotillstånd hello resurs.

#### <a name="platform-events"></a>Plattform händelser
Dessa händelser har utlösts av flera komponenter i hello Azure-infrastrukturen och inkluderar både schemalagda åtgärder som planerat underhåll och oväntat incidenter som en oplanerad värden omstart.

Resurshälsa innehåller ytterligare information om hello händelse hello återställningsprocessen och aktiverar du stöd för toocontact även om du inte har en aktiv Microsoft supportavtal.

![Resursen hälsa tillgänglig virtuell dator på grund av tooplatform händelse](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Icke-plattform händelser
Dessa händelser som utlöses av åtgärder som vidtas av användare, till exempel stoppar en virtuell dator eller når hello maximalt antal anslutningar tooa Redis-Cache.

![Resource health tillgänglig virtuella på grund av toonon plattformshändelsen](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Okänd
Den här hälsostatus anger att resurshälsa inte har tagit emot information om den här resursen för mer än 10 minuter. Denna status inte är en slutgiltig indikation på hello hello resource tillstånd, men det är ett viktigt datapunkten i hello felsökningsprocessen:
* Om hello resurs körs som förväntat hello status för hello resurs uppdateras tooAvailable efter några minuter.
* Om du har problem med hello resurs kan hello okänd hälsostatus föreslå hello resurs påverkas av en händelse i hello plattform.

![Resource health okänd virtuell dator](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Rapportera felaktig status
Om du tror hello aktuella status är felaktigt att när som helst, du kan berätta för oss genom att klicka på **Rapportera felaktig hälsostatus**. I fall där du påverkas av en Azure problem gärna du toocontact stöd från hello resursbladet hälsa. 

![Resurshälsa rapporten felaktig Status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historisk Information
Du kan komma åt in too14 dagar historiska hälsa data genom att klicka på **Visa historik över** i hello resursbladet hälsa. 

![Resource health rapporthistorik](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Komma igång
tooopen Resource health för en resurs
1.  Logga in till hello Azure-portalen.
2.  Navigera tooyour resurs.
3.  Resurs-hello menyn finns i hello vänster **Resource health**.

![Öppna Resource health från resursbladet](./media/resource-health-overview/from-resource-blade.png)

Du kan också komma åt resurshälsa genom att klicka på **fler tjänster**, och skriva **resurshälsa** i filtret text rutan tooopen hello **hjälp + stöd** bladet. Klicka slutligen på [ **resurshälsa**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Öppna Resource health från flera service](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Nästa steg

Checka ut dessa resurser toolearn mer om resurshälsa:
-  [Hälsa och resurstyper kontrollerar i Azure resurshälsa](resource-health-checks-resource-types.md)
-  [Vanliga frågor och svar om Azure-resurs hälsotillstånd](resource-health-faq.md)




