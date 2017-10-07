---
title: "aaaWorking med stora Azure Virtual Machine-Skalningsuppsättningar | Microsoft Docs"
description: "Vad du behöver tooknow toouse stor virtuell dator i Azure skala uppsättningar"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/7/2017
ms.author: guybo
ms.openlocfilehash: a39aab25925d7fc50763f0a20148b1f2213b492f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-large-virtual-machine-scale-sets"></a>Arbeta med stora skalningsuppsättningar för virtuella datorer
Du kan nu skapa Azure [skalningsuppsättningar i virtuella](/azure/virtual-machine-scale-sets/) med en kapacitet på upp too1 000 virtuella datorer. I detta dokument, en _stora virtuella datorns skaluppsättning_ definieras som en skala som angetts för skalning toogreater än 100 virtuella datorer. Den här funktionen ställs in med skalningsuppsättningsegenskapen (_singlePlacementGroup=False_). 

Vissa aspekter av storskaliga anger, t.ex. load balancing och feltolerans domäner fungerar annorlunda tooa standard skaluppsättning. Det här dokumentet beskriver hello egenskaper i stor skala uppsättningar och beskriver vad du behöver tooknow toosuccessfully använda dem i dina program. 

En vanlig metod för att distribuera moln-infrastruktur i stor skala är toocreate en uppsättning _skalningsenheter_, till exempel genom att skapa flera virtuella datorer skala anger över flera virtuella nätverk och lagringskonton. Den här metoden erbjuder toosingle för enklare hantering jämfört med virtuella datorer och flera skalenheter är användbara för många program, särskilt de som kräver andra staplade komponenter som flera virtuella nätverk och slutpunkter. Om ditt program kräver ett enda stort kluster men, kan det vara enklare toodeploy som en enda skala konfigurera av too1 000 virtuella datorer. Exempelscenarier omfattar centraliserade distributioner av stordata eller beräkningsnät som kräver enkel hantering av en stor pool av arbetsnoder. I kombination med skalningsuppsättning [anslutna datadiskar](virtual-machine-scale-sets-attached-disks.md), storskaliga anger att du toodeploy en skalbar infrastruktur som består av tusentals kärnor och petabyte av lagring, som en enda åtgärd.

## <a name="placement-groups"></a>Placeringsgrupper 
Vad gör en _stora_ skaluppsättningen särskilda är inte hello antal virtuella datorer, men hello antalet _placering grupper_ den innehåller. En grupp för placering är en konstruktion liknande tooan Azure tillgänglighetsuppsättning med feldomäner och uppgraderingsdomäner. Som standard består en skalningsuppsättning av en enda placeringsgrupp med maximalt 100 virtuella datorer. Om en egenskap som kallas skaluppsättning _singlePlacementGroup_ anges too_false_, hello skaluppsättning kan bestå av flera grupper för placering och har ett intervall med 0 och 1 000 virtuella datorer. Ange när toohello standardvärdet _SANT_, en skaluppsättning består av en enda placering grupp och har en uppsättning 0-100 virtuella datorer.

## <a name="checklist-for-using-large-scale-sets"></a>Checklista för att använda stora skalningsuppsättningar
toodecide om ditt program kan göra effektiv användning av storskaliga uppsättningar, Överväg hello följande krav:

- Stora skalningsuppsättningar kräver Azure Managed Disks. Skalningsuppsättningar som inte har skapats med Managed Disks kräver flera lagringskonton (ett konto kan användas för 20 virtuella dator). Stor skala som är utformad toowork uteslutande med hanterade diskar tooreduce storage management omkostnader och tooavoid hello risken som körs i prenumerationen begränsar för lagringskonton. Om du inte använder hanterade diskar är begränsad too100 virtuella datorer i din skaluppsättning.
- Skaluppsättningar för den skapade från Azure Marketplace-bilder kan skala upp too1 000 virtuella datorer.
- Skaluppsättningar som skapas från anpassade avbildningar (VM-avbildningar du skapa och överföra själv) kan för närvarande skala upp too100 virtuella datorer.
- Nivå 4 belastningsutjämning med hello Azure belastningsutjämnare stöds inte ännu för skalningsuppsättningar består av flera grupper för placering. Om du behöver toouse hello Azure belastningsutjämnare gör att hello skaluppsättningen är konfigurerade toouse en enda placering grupp, vilket är standardinställningen för hello.
- Layer 7 belastningsutjämning med hello Azure Application Gateway har stöd för alla skaluppsättningar.
- En skalningsuppsättning definieras med ett enda undernät - Kontrollera att ditt undernät har ett adressutrymme som är tillräckligt stor för alla hello virtuella datorer du behöver. En skala som standard overprovisions (skapar extra virtuella datorer vid tidpunkten för distribution eller skala ut, vilket inte debiteras du för) tooimprove distribution tillförlitlighet och prestanda. Tillåt att en adressutrymme 20% större än hello antalet virtuella datorer som du planerar tooscale till.
- Om du planerar toodeploy många virtuella datorer kan din kvotgränserna för beräkning core måste toobe ökat.
- Feldomäner och uppgraderingsdomäner är endast konsekventa i en placeringsgrupp. Den här arkitekturen ändras inte hello övergripande tillgängligheten för en skala har angetts som virtuella datorer är jämnt fördelad över distinkta fysisk maskinvara, men den visas innebär att om du behöver tooguarantee två virtuella datorer finns på olika typer av maskinvara, se till att de tillhör olika fel domäner i hello samma grupp för placering. Fel domän och placering av grupp-ID som visas i hello _instansen visa_ på en skala ange VM. Du kan visa hello instansvyn för en skaluppsättning för virtuell dator i hello [resursutforskaren Azure](https://resources.azure.com/).


## <a name="creating-a-large-scale-set"></a>Skapa en stor skalningsuppsättning
När du skapar en skala som angetts i hello Azure-portalen kan du låta det tooscale toomultiple placering grupper genom att ange hello _gränsen tooa enda placering grupp_ alternativet too_False_ i hello _grunderna_ bladet. Med det här alternativet set too_False_, kan du ange en _instansen antal_ värdet in too1 000.

![](./media/virtual-machine-scale-sets-placement-groups/portal-large-scale.png)

Du kan skapa en storskalig virtuell dator med hello [Azure CLI](https://github.com/Azure/azure-cli) _az vmss skapa_ kommando. Det här kommandot anger intelligent standarder, till exempel undernät storlek baserat på hello _antal instanser_ argument:

```bash
az group create -l southcentralus -n biginfra
az vmss create -g biginfra -n bigvmss --image ubuntults --instance-count 1000
```
Observera att hello _vmss skapa_ vissa konfigurationsvärden standardplatsen om du inte anger dem. toosee hello tillgängliga alternativ som du kan åsidosätta försök:
```bash
az vmss create --help
```

Om du skapar en stor skala som fastställdes en Azure Resource Manager-mall, kontrollerar du hello skapas en skaluppsättning för baserat på Azure-hanterade diskar. Du kan ange hello _singlePlacementGroup_ egenskapen too_false_ i hello _egenskaper_ avsnitt i hello _Microsoft.Compute/virtualMAchineScaleSets_ resurs. hello följande JSON-fragment visar hello början av en scale set mall, inklusive hello 1 000 VM-kapacitet och hello _”singlePlacementGroup”: false_ inställningen:
```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "location": "australiaeast",
  "name": "bigvmss",
  "sku": {
    "name": "Standard_DS1_v2",
    "tier": "Standard",
    "capacity": 1000
  },
  "properties": {
    "singlePlacementGroup": false,
    "upgradePolicy": {
      "mode": "Automatic"
    }
```
Ange mall för en komplett exempel på en stor skala finns för[https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json).

## <a name="converting-an-existing-scale-set-toospan-multiple-placement-groups"></a>Konvertera en befintlig skala ange toospan flera placering grupper
toomake en befintlig skaluppsättning för virtuell dator kan skalning toomore än 100 virtuella datorer, måste toochange hello _singplePlacementGroup_ egenskapen too_false_ i hello skala ange modell. Du kan testa att ändra egenskapen med hello [resursutforskaren Azure](https://resources.azure.com/). Hitta en befintlig skaluppsättning, Välj _redigera_ och ändra hello _singlePlacementGroup_ egenskapen. Om du inte ser den här egenskapen kan du visa hello skala med en äldre version av hello Microsoft.Compute-API.

>[!NOTE] 
Du kan ändra en skala från stöder en enda placering gruppen endast (hello standardbeteendet) tooa stöder flera placering grupper, men du kan inte konvertera hello tvärtom. Kontrollera därför att du förstår hello egenskaper för storskaliga anger innan du konverterar. I synnerhet Kontrollera att du inte behöver lager 4 belastningsutjämning med hello Azure belastningsutjämnare.


