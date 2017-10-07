---
title: "aaaVirtual datorstorlekar för Azure-molntjänster | Microsoft Docs"
description: "Visar hello olika virtuella datorstorlekar (och ID: N) för Azure cloud service webb- och arbetsroller roller."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 93d91a67afc352f3d18c31e0dd5cf976bf46350c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-cloud-services"></a>Storlekar för molntjänster
Det här avsnittet beskriver hello tillgängliga storlekar och alternativ för Molntjänsten rollinstanser (webb- och arbetsroller). Det ger också distribution överväganden toobe när du planerar toouse dessa resurser. Varje storlek har ett ID som du lägger till i din [tjänstdefinitionsfilen](cloud-services-model-and-package.md#csdef). Priser för varje storlek är tillgängliga på hello [Cloud Services priser](https://azure.microsoft.com/pricing/details/cloud-services/) sidan.

> [!NOTE]
> toosee relaterade Azure-gränser, se [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Storlekar för webb- och arbetsroller rollinstanser
Det finns flera standard storlekar toochoose från på Azure. Det finns några saker som du bör tänka på när du väljer storlek:

* D-serien är utformad toorun program som kräver högre datorkraft och tillfällig diskprestanda. Virtuella datorer i D-serien ger snabbare processorer och minne till core förhållandet SSD (SSD) för hello diskutrymme. Mer information finns i hello-meddelande på hello Azure blogg [nya storlekar för virtuella datorer av D-serien](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).
* Dv2-serien, produkter toohello ursprungliga D-serien, har en kraftfullare processor. Hej Dv2-serien CPU är ungefär 35% snabbare än hello D-serien CPU. Den är baserad på hello senaste generationens 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) processor, och med hello Intel Turbo förstärkningen Technology 2.0, gå upp too3.1 GHz. Hej Dv2-serien har hello samma minne och konfigurationer som hello D-serien.
* G-serien virtuella datorer och erbjuder hello mest minne och kör på värdar som har Intel Xeon E5 V3 family processorer.
* hello A-series virtuella datorer kan distribueras på olika maskinvarutyper och processorer. hello storleken begränsas baserat på hello maskinvara, toooffer konsekvent processorprestanda för hello körs oavsett hello maskinvara som den har distribuerats på-instansen. toodetermine hello fysisk maskinvara som den här storleken är distribuerad, fråga hello virtuell maskinvara från inom hello virtuell dator.
* hello A0 storlek är över prenumererade på hello fysisk maskinvara. För den här specifika storleken kan andra kunddistributioner påverka hello prestandan för din arbetsbelastning. nedan beskrivs hello relativa prestanda som hello förväntades baslinje, ämne tooan ungefärliga variationen 15 procent.

hello storlek hello virtuella datorn påverkar hello priser. hello storlek påverkar också hello bearbetning, minne och lagringsutrymme kapacitet hello virtuell dator. Lagringskostnader beräknas separat använda sidor i hello storage-konto. Mer information finns i [prisinformation för Cloud Services](https://azure.microsoft.com/pricing/details/cloud-services/) och [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

hello följande överväganden kan hjälpa dig att bestämma en storlek:

* hello A8 A11 och H-serien storlek kallas även *beräkningsintensiva instanser*. hello-maskinvara som kör dessa storlekar har utformats och optimerats för beräkningsintensiva och nätverks-intensiva program, inklusive högpresterande Datorsystem kluster program, modellering och simulering. hello A8 A11 serien använder Intel Xeon E5-2670 @ 2.6 GHZ och hello H-serien använder Intel Xeon E5-2667 v3 @ 3,2 GHz. Detaljerad information och överväganden om hur du använder dessa storlekar finns [högpresterande compute VM-storlekar](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Dv2-serien, D-serien, G-serien, lämpar sig för program som kräver snabbare processorer, bättre lokal disk prestanda eller har högre krav på minne. De utgör en kraftfull kombination för många program i företagsklass.
* Några av hello fysiska värdar i Azure-Datacenter kanske inte stöder större storlekar för virtuella datorer, till exempel A5 – A11. Därför kan du se hello felmeddelande **misslyckades tooconfigure virtuella datorn {datornamnet}** eller **misslyckades toocreate virtuella datorn {machine name}** när du ändrar storlek på en befintlig virtuell dator tooa nya storlek. Skapa en ny virtuell dator i ett virtuellt nätverk som skapats före 16 April 2013; eller lägga till en ny virtuell dator tooan befintlig molntjänst. Se [fel: ”Det gick inte tooconfigure virtuell dator”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) på hello Supportforum för lösningar för varje scenario för distribution.
* Din prenumeration kan också begränsa hello antal kärnor som du kan distribuera för vissa storlek. tooincrease en kvot kontakta Azure-supporten.

## <a name="performance-considerations"></a>Saker att tänka på gällande prestanda
Vi har skapat hello begreppet hello Azure Compute-enhet (ACU) tooprovide ett sätt för att jämföra bearbetningsprestanda (CPU) över Azure SKU: er och tooidentify som SKU är sannolikt toosatisfy prestandan måste.  ACU är för närvarande standardiserat på en liten virtuell dator (Standard_A1) och är 100, och alla andra SKU:er representerar ungefär hur mycket snabbare den SKU:n kan köra ett benchmark-standardtest.

> [!IMPORTANT]
> hello ACU är endast en rekommendation. hello resultat för din arbetsbelastning kan variera.
>
>

<br>

| SKU-familj | ACU/kärna |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Liten Extrastora](#a-series) |100 |
| [A5 7](#a-series) |100 |
| [Standard_A1-8v2](#av2-series) |100 |
| [Standard_A2m-8mv2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-14](#d-series) |160 |
| [D1-15v2](#dv2-series) |210 - 250* |
| [G1-5](#g-series) |180 - 240* |
| [H](#h-series) |290 - 300* |

ACUs markeras med ett * använda Intel® Turbo teknik tooincrease CPU frekvens och ange en prestandaökning. hello mängden hello förstärkningen kan variera beroende på hello VM-storlek, arbetsbelastning och andra arbetsbelastningar som körs på hello samma värd.

## <a name="size-tables"></a>Storlekstabeller
hello följande tabeller visar hello storlekar och hello kapaciteter som de tillhandahåller.

* Lagringskapaciteten visas i GiB, eller 1 024^3 byte. När jämföra diskar mätt i GB (1000 ^ 3 byte) toodisks mätt i GiB (1024 ^ 3) Kom ihåg att kapacitet siffrorna i GiB visas mindre. Exempel: 1 023 GiB = 1 098,4 GB
* Diskgenomflödet mäts i indata-/utdataåtgärder per sekund (IOPS) och Mbit/s där Mbit/s = 10^6 byte/sek.
* Datadiskar kan köras i cachelagrat eller icke cachelagrat läge. För cachelagrade data disk åtgärden hello värden cacheläge har angetts för**ReadOnly** eller **ReadWrite**. För cachelagrade data disk åtgärden hello värden cacheläge har angetts för**ingen**.
* Maximal bandbredd är hello högsta sammanlagda bandbredden allokerade och tilldelade per typ av VM. Maximal bandbredd för hello ger riktlinjer för att välja hello rätt VM typen tooensure tillräcklig nätverkskapacitet är tillgängliga. Hello genomströmning ökar när du flyttar mellan låg, Måttlig, hög och mycket hög. Faktiska nätverksprestanda beror på många faktorer, bland annat nätverks- och programbelastningar och programmets nätverksinställningar.

## <a name="a-series"></a>A-serien
| Storlek            | Processorkärnor | Minne: GiB  | Lokal hårddisk: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0.768        | 20                   | 1 / låg |
| Liten           | 1         | 1.75         | 225                  | 1 / måttlig |
| Medel          | 2         | 3,5 GB       | 490                  | 1 / måttlig |
| Stor           | 4         | 7            | 1000                 | 2 / hög |
| Extrastora      | 8         | 14           | 2040                 | 4 / hög |
| A5              | 2         | 14           | 490                  | 1 / måttlig |
| A6              | 4         | 28           | 1000                 | 2 / hög |
| A7              | 8         | 56           | 2040                 | 4 / hög |

## <a name="a-series---compute-intensive-instances"></a>A-serien – beräkningsintensiva instanser
Mer information och överväganden om hur du använder dessa storlekar finns [högpresterande compute VM-storlekar](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Storlek            | Processorkärnor | Minne: GiB  | Lokal hårddisk: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8 *             |8          | 56           | 1817                 | 2 / hög |
| A9 *             |16         | 112          | 1817                 | 4 / mycket hög |
| A10             |8          | 56           | 1817                 | 2 / hög |
| A11             |16         | 112          | 1817                 | 4 / mycket hög |

\*RDMA-kompatibla

## <a name="av2-series"></a>Av2-serien

| Storlek            | Processorkärnor | Minne: GiB  | Lokal SSD: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1 / måttlig                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2 / måttlig                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4 / hög                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8 / hög                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2 / måttlig                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4 / hög                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8 / hög                     |


## <a name="d-series"></a>D-serien
| Storlek            | Processorkärnor | Minne: GiB  | Lokal SSD: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3.5          | 50                   | 1 / måttlig |
| Standard_D2     | 2         | 7            | 100                  | 2 / hög |
| Standard_D3     | 4         | 14           | 200                  | 4 / hög |
| Standard_D4     | 8         | 28           | 400                  | 8 / hög |
| Standard_D11    | 2         | 14           | 100                  | 2 / hög |
| Standard_D12    | 4         | 28           | 200                  | 4 / hög |
| Standard_D13    | 8         | 56           | 400                  | 8 / hög |
| Standard_D14    | 16        | 112          | 800                  | 8 / mycket hög |

## <a name="dv2-series"></a>Dv2-serien
| Storlek            | Processorkärnor | Minne: GiB  | Lokal SSD: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3.5          | 50                   | 1 / måttlig |
| Standard_D2_v2  | 2         | 7            | 100                  | 2 / hög |
| Standard_D3_v2  | 4         | 14           | 200                  | 4 / hög |
| Standard_D4_v2  | 8         | 28           | 400                  | 8 / hög |
| Standard_D5_v2  | 16        | 56           | 800                  | 8 / extremt hög |
| Standard_D11_v2 | 2         | 14           | 100                  | 2 / hög |
| Standard_D12_v2 | 4         | 28           | 200                  | 4 / hög |
| Standard_D13_v2 | 8         | 56           | 400                  | 8 / hög |
| Standard_D14_v2 | 16        | 112          | 800                  | 8 / extremt hög |
| Standard_D15_v2 | 20        | 140          | 1,000                | 8 / extremt hög |

## <a name="g-series"></a>G-serien
| Storlek            | Processorkärnor | Minne: GiB  | Lokal SSD: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1 / hög |
| Standard_G2     | 4         | 56           | 768                  |2 / hög |
| Standard_G3     | 8         | 112          | 1,536                |4 / mycket hög |
| Standard_G4     | 16        | 224          | 3,072                |8 / extremt hög |
| Standard_G5     | 32        | 448          | 6,144                |8 / extremt hög |

## <a name="h-series"></a>H-serien
Virtuella datorer i Azure H-serien är hello nästa generation med höga prestanda VMs syftar till att avancerad beräkningar behov som molekylär utformning och beräkningsprogram. Dessa 8 och 16 kärnor VMs bygger på hello Intel Haswell E5 2667 V3 processor technology med DDR4 minne och lokala SSD-baserad lagring.

Dessutom toohello betydande CPU-ström hello H-serien finns olika alternativ för låg latens RDMA nätverk med hjälp av FDR InfiniBand och flera konfigurationer toosupport minne beräkningsintensiva beräkningar minneskrav.

| Storlek            | Processorkärnor | Minne: GiB  | Lokal SSD: GiB       | Maximalt antal nätverkskort/nätverksbandbredd |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8 / hög |
| Standard_H16    | 16        | 112          | 2000                 | 8 / mycket hög |
| Standard_H8m    | 8         | 112          | 1000                 | 8 / hög |
| Standard_H16m   | 16        | 224          | 2000                 | 8 / mycket hög |
| Standard_H16r*  | 16        | 112          | 2000                 | 8 / mycket hög |
| Standard_H16mr* | 16        | 224          | 2000                 | 8 / mycket hög |

\*RDMA-kompatibla

## <a name="configure-sizes-for-cloud-services"></a>Konfigurera storlekar för molntjänster
Du kan ange hello storlek på virtuell dator på en rollinstans av som en del av hello tjänstmodell som beskrivs av hello [tjänstdefinitionsfilen](cloud-services-model-and-package.md#csdef). hello storleken på hello roll avgör hello antalet CPU-kärnor, hello minneskapacitet och hello lokala system filstorlek som har allokerats tooa kör-instansen. Välj hello rollstorleken baserat på ditt program resurskrav.

Här är ett exempel för att ange hello rollen storlek toobe [Standard_D2](#general-purpose-d) för en Webbroll-instans:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-hello-size-of-an-existing-role"></a>Ändra hello storlek på en befintlig roll

Du kanske vill toochange hello storleken på din roll som hello karaktär frågearbetsbelastningsändringar eller nya VM-storlekar som blir tillgängliga. toodo så du måste ändra hello VM-storlek i din tjänstdefinitionsfilen (som visas ovan), paketera din molntjänst och distribuera den. Det är inte möjligt toochange VM-storlekar direkt från hello-portalen eller PowerShell.

>[!TIP]
> Du kanske vill toouse olika VM-storlek för din roll i olika miljöer (t.ex. Testa vs produktion). Enkelriktade toodo detta är toocreate flera service definition (.csdef)-filer i projektet och sedan skapa annat moln servicepaket per miljö under din automatiserade build hello CSPack verktyget. toolearn mer om hello element i ett moln services paket och hur toocreate dem, se [vad är hello cloud services modellen och hur jag paketera den?](cloud-services-model-and-package.md)
>
>

## <a name="get-a-list-of-sizes"></a>Hämta en lista över storlekar
Du kan använda PowerShell eller hello REST API tooget en lista över storlekar. hello REST API dokumenteras [här](https://msdn.microsoft.com/library/azure/dn469422.aspx). hello är följande kod ett PowerShell-kommando som visar en lista över alla hello storlekar som är tillgängliga för din molntjänst.

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize
```

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [Azure-prenumerationer, tjänstbegränsningar, kvoter och krav](../azure-subscription-service-limits.md).
* Lär dig mer [om högpresterande compute VM-storlekar](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för HPC-arbetsbelastning.
