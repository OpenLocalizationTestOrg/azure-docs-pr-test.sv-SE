---
title: "aaaConvert ett Azure tooa skaluppsättningen | Microsoft Docs"
description: "Skapa och distribuera en Linux Azure skaluppsättningen för virtuell dator med hello Azure CLI."
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
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a>Konvertera en befintlig skaluppsättning för virtuell dator i Azure tooa

Den här kursen visar hur toouse Azure CLI 2.0 tooconvert en virtuell dator tooa virtuella skaluppsättning. Du lär dig också hur tooautomate hello konfigurationen av hello virtuella datorer i hello skala in. Mer information om hur tooinstall Azure CLI 2.0, se [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-hello-vm"></a>Steg 1 – ta bort etableringen hello VM

Använda SSH tooconnect toohello VM.

Avetablering hello VM använder hello Azure VM-agenten toodelete filer och data. En detaljerad översikt över avetablering finns [avbilda en virtuell Linux-dator](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a>Steg 2 – Skapa en avbildning av hello VM

En detaljerad översikt över avbildning finns [avbilda en virtuell Linux-dator](capture-image.md).

Frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalisera hello virtuell dator med [az vm generalisera](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Skapa en avbildning från hello Virtuella datorresursen med [az bild skapa](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a>Steg 3 – skapa hello skaluppsättning

Hämta hello **id** till hello bild.

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

Det här kommandot också koppla en disk på 10gb data. Tänk på att beroende på hello VM storlek vald (Vi använde **Standard_DS1_v2**), hello antalet tillåtna datadiskar är olika. Mer information finns i hello [storlekar för virtuella datorer](sizes.md).

När hello skaluppsättning har avslutats, ansluta tooit. Hämta en lista över IP-adresser för hello instanser för SSH med [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Nu kan du ansluta toohello virtuella instansen tooinitialize hello datadisk

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a>Steg 4 – initiera hello datadisk

När anslutna toohello virtuella partitionsdisk hello med `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

Skriv en toohello filsystempartition med hello `mkfs` kommando:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Montera hello ny disk så att den är tillgänglig i hello operativsystemet:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

hello disk kan nu vara åtkomst via hello datadrive monteringspunkt, som kan verifieras med `ls /datadrive/`.

Avsluta hello SSH-session.


## <a name="step-5---configure-firewall"></a>Steg 5 – konfigurera brandväggen

Hålslag hål hello brandväggen toohello webbserver hos hello skaluppsättning. När hello skaluppsättning skapades, skapades även belastningsutjämning och använt det **SSH** toohello enskilda virtuella datorer. tooopen en port som du behöver två typer av information som du kan hämta med hjälp av Azure CLI.

* **Frontend-IP-adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Backend-IP-adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

Med dessa två namn, kan du öppna port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>Steg 6 – automatisera konfigurationen

Hej datadisk måste toobe som konfigurerats på varje virtuell dator-instans. Vi kan automatisera hello konfigurationen av hello virtuell dator med hello **CustomScript** tillägg.

Skapa först en *.sh* skript som innehåller hello disk format kommandon.

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

Därefter ladda upp den skriptet filen toowhere hello **CustomScript** tillägg kan komma åt den. En kopia finns [här](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Skapa en lokal fil med namnet **settings.json** och placera hello följande JSON-block i den. Hej `flieUris` egenskapen ska anges toowhere skriptfilen överfördes till.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Distribuera det här kommandot tooyour skala med hello **CustomScript** tillägg, refererar till hello **settings.json** fil som vi just skapade.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Det här tillägget körs automatiskt på alla aktuella instanser och alla instanser som skapats senare genom att skala.

## <a name="step-7---configure-autoscale-rules"></a>Steg 7 – konfigurera autoskalning regler

Autoskala regler kan för närvarande inte anges i Azure CLI. Använd hello [Azure-portalen](https://portal.azure.com) tooconfigure Autoskala.

## <a name="step-8---management-tasks"></a>Steg 8 - administrativa uppgifter

Du kan behöva toorun under hello livscykel för skaluppsättning för hello, en eller flera hanteringsuppgifter. Dessutom du toocreate skript som automatiserar olika livscykel-uppgifter och hello Azure CLI tillhandahåller ett snabbt sätt toodo uppgifterna. Här följer några vanliga uppgifter.

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
toolearn mer om några av hello virtuella skala ange funktionerna i den här kursen finns hello följande information:

- [Översikt över Azure virtuella datorer](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Översikt över Azure Load Balancer](../../load-balancer/load-balancer-overview.md)
- [Styra flödet i nätverkstrafiken med nätverkssäkerhetsgrupper](../../virtual-network/virtual-networks-nsg.md)