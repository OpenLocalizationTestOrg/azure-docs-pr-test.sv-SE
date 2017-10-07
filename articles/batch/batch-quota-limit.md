---
title: "aaaService kvoter och gränser för Azure Batch | Microsoft Docs"
description: "Lär dig mer om standard Azure Batch-kvoter gränser och begränsningar och hur toorequest kvot ökar"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a>Kvoter och begränsningar för Batch-tjänsten

Som med andra Azure-tjänster, det finns begränsningar på vissa resurser som är associerade med hello Batch-tjänsten. Många av dessa gränser är standard kvoterna som används av Azure på hello prenumeration eller konto. Den här artikeln beskrivs dessa standardinställningar och hur du kan begära kvoten ökar.

Tänk på dessa kvoter när du utformar och skalar upp dina Batch-arbetsbelastningar. Till exempel om din pool inte når hello mål antalet compute-noder som du har angett, kanske du har nått hello core kvotgränsen för Batch-kontot eller en regionala Virtuella kärnor kvot för din prenumeration.

Du kan köra flera Batch arbetsbelastningar i en enskild Batch-kontot eller distribuera dina arbetsbelastningar bland Batch-konton som finns i hello samma prenumeration, men i olika Azure-regioner.

Om du planerar toorun produktionsarbetsbelastningar i Batch behöva tooincrease en eller flera av hello kvoter ovan hello standard. Om du vill tooraise en kvot kan du öppna en online [kundsupport](#increase-a-quota) utan kostnad.

> [!NOTE]
> En kvot är en kreditgräns, inte en kapacitet garanti. Kontakta Azure-supporten om du har stora kapacitetsbehov.
> 
> 

## <a name="resource-quotas"></a>Resurskvoter
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Kvoter i användarläge för prenumeration

För Batch-kontot med poolen allokering inställd för**användarens prenumeration**, Batch-virtuella datorer och andra resurser, till exempel storage-konton, skapas direkt i din prenumeration när poolen har skapats. hello Azure Batch kärnor kvoten gäller inte tooan konto som har skapats i det här läget. I stället tillämpas hello kvoter i din prenumeration för regional beräkning kärnor och andra resurser. Mer information om dessa kvoter i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

När du planerar resursanvändningen för ett konto som har skapats i användarläge för prenumerationen Obs hello följande Batch resurser (tillägg toocompute kärnor) krävs för varje 40 virtuella Linux-datorer eller 20 virtuella Windows-datorer:

| Resurs | Kvot | Leverantör |
| --- | ---| --- |
| Ett lagringskonto | Lagringskonton | Microsoft.Storage |
| En offentlig IP-adress | Offentliga IP-adresser | Microsoft.Network | 
| Ett virtuellt nätverk | Virtuella nätverk | Microsoft.Network | 
| En nätverkssäkerhetsgrupp | Nätverkssäkerhetsgrupper | Microsoft.Network | 
| En virtuella datorns skaluppsättning | Skalningsuppsättningar för Virtual Machines | Microsoft.Compute | 
| En belastningsutjämnare | Belastningsutjämnare | Microsoft.Network | 

hello kärnor kvot på regional nivå eller per VM familj ska ange bl.a toohello VM-storlek krävs för Batch-pool eller pooler:

| Kvot | Leverantör |
| --- | ---- |
| Totalt antal regionala kärnor | Microsoft.Compute |
| … Family kärnor | Microsoft.Compute |



## <a name="other-limits"></a>Övriga begränsningar
| **Resurs** | **Övre gräns** |
| --- | --- |
| [Samtidiga uppgifter](batch-parallel-node-tasks.md) per compute-nod |4 x antal nod kärnor |
| [Program](batch-application-packages.md) per Batch-kontot |20 |
| Programpaket per program |40 |
| Storlek för paketet (alla) |Uppskattat 195GB<sup>1</sup> |
| Maximal startstorlek för aktiviteten | 32768 tecken<sup>2</sup> |

<sup>1</sup> azure Storage-gränsen för högsta blob-blockstorlek<br />
<sup>2</sup> innehåller resursfiler och miljövariabler

## <a name="view-batch-quotas"></a>Visa Batch-kvoter
Visa dina kvoter för Batch-kontot i hello [Azure-portalen][portal].

1. Välj **Batch-konton** i hello portal, välj sedan hello Batch-kontot du är intresserad av.
2. Välj **egenskaper** hello batchkonto menyn bladet.
3. hello egenskapsbladet visar hello **kvoter** toohello Batch-kontot som för närvarande används
   
    ![Kvoter för batch-konto][account_quotas]

Visa hello relaterade prenumeration kvoter i hello Azure-portalen för en Batch-kontot som skapats i användarläge för prenumerationen.

1. Välj **prenumerationer**, och välj hello-prenumeration som du använder för hello Batch-kontot.

2. På hello **prenumeration** bladet väljer **användning + kvoter**.



## <a name="increase-a-quota"></a>Öka en kvot
Följ dessa steg toorequest en kvot öka för Batch-kontot eller med hjälp av hello [Azure-portalen][portal]. hello beror databaskvot på hello poolen allokering läge av Batch-kontot.

### <a name="increase-a-batch-cores-quota"></a>Öka kvoten för kärnor en Batch 

Om Batch-kontot har skapats i **Batch-tjänsten** läge, Följ dessa steg toorequest databaskvot en Batch kärnor:

1. Välj hello **hjälp + support** panelen på instrumentpanelen i portalen eller hello frågetecken (**?**) i hello övre högra hörnet av hello-portalen.
2. Välj **ny supportbegäran** > **grunderna**.
3. På hello **grunderna** bladet:
   
    a. **Utfärda typ** > **kvot**
   
    b. Välj din prenumeration.
   
    c. **Typ av kvot** > **Batch**
   
    d. **Supportplan** > **kvot support - ingår**
   
    Klicka på **Nästa**.
4. På hello **problemet** bladet:
   
    a. Välj en **allvarlighetsgrad** enligt tooyour [inverkan på verksamheten][support_sev].
   
    b. I **information**, ange varje kvot som du vill toochange hello Batch-kontonamn och hello ny gräns.
   
    Klicka på **Nästa**.
5. På hello **kontaktinformation** bladet:
   
    a. Välj en **önskad kontaktmetod**.
   
    b. Verifiera och ange hello krävs kontaktinformation.
   
    Klicka på **skapa** toosubmit hello supportbegäran.

När du har skickat supportförfrågan Azure-supporten kommer att kontakta dig. Observera att slutföra hello begäran kan ta upp too2 arbetsdagar.

### <a name="increase-a-subscription-cores-quota"></a>Öka kvoten för kärnor en prenumeration

Om Batch-kontot har skapats i **användarens prenumeration** läge och du behöver ytterligare regionala eller VM family kärnor, begäran en kvot ökar i din prenumeration. Anvisningar finns [Resource Manager kärnkvot öka begäranden](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Relaterade ämnen
* [Skapa ett Azure Batch-konto med hello Azure-portalen](batch-account-create-portal.md)
* [Översikt av Azure Batch-funktion](batch-api-basics.md)
* [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
