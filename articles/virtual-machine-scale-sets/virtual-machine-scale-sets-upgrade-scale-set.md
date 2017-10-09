---
title: "aaaUpgrade en virtuell dator i Azure skaluppsättning | Microsoft Docs"
description: "Uppgradera en skaluppsättning för virtuell Azure-dator"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Uppgradera en virtuella datorns skaluppsättning
Den här artikeln beskriver hur du kan distribuera en OS uppdatering tooan virtuella Azure-datorn skaluppsättningen utan driftavbrott. I den här kontexten innebär en OS-uppdatering ändra hello version eller SKU hello OS eller ändra hello URI för en anpassad avbildning. Uppdaterar utan driftavbrott uppdatera virtuella datorer en i taget eller grupper (till exempel en feldomän i taget) i stället för på en gång. På så sätt, kan alla virtuella datorer som inte uppgraderas att köra.

tooavoid tvetydighet skilja mellan fyra typer av OS-uppdateringar som du kanske vill vi tooperform:

* Ändra hello version eller SKU av en plattformsavbildning. Till exempel ändra Ubuntu 14.04.2-LTS version från 14.04.201506100 too14.04.201507060 eller ändra hello Ubuntu 15.10/senaste SKU too16.04.0-LTS/latest. Det här scenariot beskrivs i den här artikeln.
* Ändra hello URI som pekar tooa ny version av en anpassad avbildning som du skapat (**egenskaper** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **bild** > **uri**). Det här scenariot beskrivs i den här artikeln.
* Ändra hello bildreferens för skaluppsättning som har skapats med hjälp av Azure hanterade diskar.
* Korrigering hello operativsystem från en virtuell dator (exempel på detta är hur du installerar en säkerhetskorrigering och kör Windows Update). Det här scenariot stöds, men som inte omfattas i den här artikeln.

Skalningsuppsättningar i virtuella datorer som distribueras som en del av en [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) kluster beskrivs inte här. Se [korrigering Windows OS i Service Fabric-kluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) mer information om Service Fabric-korrigering.

hello grundläggande aktivitetssekvensen för att ändra hello OS-version/SKU av en plattformsavbildning eller hello URI för en anpassad avbildning ser ut som följer:

1. Hämta hello virtuella skala modellen.
2. Ändra hello version SKU, bildreferens eller URI-värdet i hello modellen.
3. Uppdatera hello modellen.
4. Gör en *manualUpgrade* anropa på hello virtuella datorer i hello skaluppsättning. Det här steget gäller endast om *upgradePolicy* har angetts för**manuell** i din skaluppsättningen. Om det har angetts för**automatisk**, alla hello virtuella datorer uppgraderas samtidigt, vilket orsakar driftstopp.

Med denna information i åtanke kan du nu ska vi se hur du kan uppdatera hello version av en skala som angetts i PowerShell och genom att använda hello REST API. De här exemplen täcker hello fall av en plattformsavbildning, men den här artikeln innehåller tillräckligt med information för tooadapt du den här processen tooa anpassad avbildning.

## <a name="powershell"></a>PowerShell
Det här exemplet uppdaterar en skaluppsättning för Windows virtuell dator (skapa toohello ny version 4.0.20160229. När du har uppdaterat hello modellen har en uppdatering för en virtuell datorinstans i taget.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Om du uppdaterar hello URI för en anpassad avbildning istället för att ändra en plattform Avbildningsversion, ersätter hello ”ange hello nya version” rad med ett kommando som kommer att uppdatera hello Källavbildningen URI. Till exempel om hello skaluppsättning skapades utan att använda Azure hanterade diskar, hello update skulle se ut så här:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Om en anpassad avbildning baserad skaluppsättning har skapats med hjälp av Azure hanterade diskar och sedan hello bildreferens skulle uppdateras. Exempel:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a>hello REST API
Här är några av Python-exempel som använder hello Azure REST API tooroll ut en uppdatering för OS-version. Använder båda hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) bibliotek med Azure REST API-omslutning funktioner toodo GET på hello skala ange modell, följt av en PUT med en uppdaterad modell. De också titta på virtuella instanser vyer tooidentify hello virtuella datorer av uppdateringsdomän.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) Python-skriptet som tooroll ut en uppgradering OS-tooa som kör virtuella datorn har ställts in en uppdateringsdomän i taget.

![Vmssupgrade skript för att välja virtuella datorer eller en uppdateringsdomän](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Det här skriptet kan du välja specifika virtuella datorer tooupdate eller ange en uppdateringsdomän. Det stöder ändra en plattform Avbildningsversion eller ändra hello URI för en anpassad avbildning.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) är en generell redigerare för skalningsuppsättningar i virtuella datorer som visar virtuella datorn status som ett heatmap där en rad representerar en uppdateringsdomän. Bland annat kan du uppdatera hello modell för en skalningsuppsättning med en ny version, en SKU eller en anpassad avbildning URI och välj sedan fel domäner tooupgrade. När du gör det, är alla hello virtuella datorer i domänen update uppgraderade toohello ny modell. Alternativt kan du göra löpande uppgradering baserat på hello batchstorlek önskat.  

hello visar följande skärmbild en modell av en skala som angetts för Ubuntu 14.04-2LTS version 14.04.201507060. Många fler alternativ har lagts till toothis verktyget eftersom den här skärmbilden har tagits.

![Vmsseditor modell för en skala som angetts för Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

När du klickar på **uppgradera** och sedan **Hämta detaljer**, virtuella datorer i UD 0 Starta tooupdate.

![Vmsseditor visar uppdatering pågår](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

