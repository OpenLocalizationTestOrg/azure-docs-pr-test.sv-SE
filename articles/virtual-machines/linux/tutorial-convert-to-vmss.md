---
title: "Konvertera en virtuell Azure-dator till en skalningsuppsättning | Microsoft Docs"
description: "Skapa och distribuera en Linux Azure skaluppsättningen för virtuell dator med Azure CLI."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a>Konvertera en befintlig Azure virtuell dator till en skaluppsättning

Den här kursen visar hur du använder Azure CLI 2.0 för att konvertera en virtuell dator till en skaluppsättning för virtuell dator. Du också lära dig hur du kan automatisera konfigurationen av de virtuella datorerna i skaluppsättning. Läs mer om hur du installerar Azure CLI 2.0 [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-the-vm"></a>Steg 1 – ta bort etableringen av den virtuella datorn

Använda SSH för att ansluta till den virtuella datorn.

Ta bort etableringen av den virtuella datorn med Virtuella Azure-agenten att ta bort filer och data. En detaljerad översikt över avetablering finns [avbilda en virtuell Linux-dator](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a>Steg 2 – Skapa en avbildning av den virtuella datorn

En detaljerad översikt över avbildning finns [avbilda en virtuell Linux-dator](capture-image.md).

Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalisera den virtuella datorn med [az vm generalisera](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Skapa en avbildning från den Virtuella datorresursen med [az bild skapa](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a>Steg 3 – skapa skaluppsättning

Hämta den **id** för bilden.

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

Skapa en virtuell dator från din bildresurs med [az vmss skapa](/cli/azure/vmss#create):

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

Det här kommandot också koppla en disk på 10gb data. Tänk på att beroende på den virtuella datorn storlek vald (Vi använde **Standard_DS1_v2**), antalet datadiskar som tillåts är olika. Mer information finns i [storlekar för virtuella datorer](sizes.md).

När skaluppsättning har avslutats, ansluta till den. Hämta en lista över IP-adresser för instanserna för SSH med [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Nu kan du ansluta till den virtuella dator instansen att initiera datadisk

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a>Steg 4 – initiera datadisken

När du är ansluten till den virtuella datorn, partitionera hårddisken med `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

Skriva ett filsystem till partitionen med det `mkfs` kommando:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Montera den nya disken så att den är tillgänglig i operativsystemet:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

Disken kan nu vara åtkomst via datadrive monteringspunkt, som kan verifieras med `ls /datadrive/`.

Avsluta SSH-session.


## <a name="step-5---configure-firewall"></a>Steg 5 – konfigurera brandväggen

Hålslag brandväggen för att den webbserver som värd av skaluppsättningen hål. När skaluppsättning skapades, skapades även en belastningsutjämnare och använt det **SSH** till enskilda virtuella datorer. Om du vill öppna en port som du behöver två typer av information som du kan hämta med hjälp av Azure CLI.

* **Frontend-IP-adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Backend-IP-adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

Med dessa två namn, kan du öppna port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>Steg 6 – automatisera konfigurationen

Datadisken måste konfigureras på varje virtuell dator-instans. Vi kan automatisera konfigurationen av den virtuella datorn med den **CustomScript** tillägg.

Skapa först en *.sh* skript som innehåller kommandon för disk-format.

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

Därefter ladda upp den skriptfilen till platsen där den **CustomScript** tillägg kan komma åt den. En kopia finns [här](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Skapa en lokal fil med namnet **settings.json** och placera följande JSON-blocket i den. Den `flieUris` egenskapen ska anges till där skriptfilen överfördes till.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Distribuera det här kommandot till nivå med den **CustomScript** tillägg, refererar till den **settings.json** fil som vi just skapade.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Det här tillägget körs automatiskt på alla aktuella instanser och alla instanser som skapats senare genom att skala.

## <a name="step-7---configure-autoscale-rules"></a>Steg 7 – konfigurera autoskalning regler

Autoskala regler kan för närvarande inte anges i Azure CLI. Använd den [Azure-portalen](https://portal.azure.com) så här konfigurerar du Autoskala.

## <a name="step-8---management-tasks"></a>Steg 8 - administrativa uppgifter

Du kan behöva köra en eller flera administrativa uppgifter i hela livscykeln för skaluppsättning. Dessutom kanske du vill skapa skript som automatiserar olika livscykel-uppgifter och Azure CLI tillhandahåller ett snabbt sätt att utföra dessa uppgifter. Här följer några vanliga uppgifter.

### <a name="get-connection-info"></a>Hämta anslutningsinformation

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a>Ange instansantal (manuell skala)

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a>Ta bort resursgruppen

En resursgrupp också tar du bort alla resurser som ingår i.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg
Mer information om några av de virtuella skala set-funktionerna som introducerades i den här kursen finns följande information:

- [Översikt över Azure virtuella datorer](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Översikt över Azure Load Balancer](../../load-balancer/load-balancer-overview.md)
- [Styra flödet i nätverkstrafiken med nätverkssäkerhetsgrupper](../../virtual-network/virtual-networks-nsg.md)