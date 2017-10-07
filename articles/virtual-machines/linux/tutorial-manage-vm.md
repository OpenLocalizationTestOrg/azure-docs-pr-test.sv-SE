---
title: aaaCreate och hantera virtuella Linux-datorer med hello Azure CLI | Microsoft Docs
description: "Självstudiekurs – skapa och hantera virtuella Linux-datorer med hello Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a>Skapa och hantera virtuella Linux-datorer med hello Azure CLI

Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö. Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator. Lär dig att:

> [!div class="checklist"]
> * Skapa och ansluta tooa VM
> * Välj och Använd VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Ändra storlek på en virtuell dator
> * Visa och förstå tillstånd för virtuell dator


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Skapa resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando. 

En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas innan en virtuell dator. I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i hello *eastus* region. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

hello resursgrupp anges när du skapar eller ändrar en VM som kan ses i hela den här kursen.

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell dator med hello [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create) kommando. 

När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken. I det här exemplet skapas en virtuell dator med namnet *myVM* kör Ubuntu Server. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

En gång hello VM har skapats, hello Azure CLI matar ut information om hello VM. Anteckna hello `publicIpAddress`, den här adressen kan vara används tooaccess hello virtuell dator... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-toovm"></a>Ansluta tooVM

Nu kan du ansluta toohello VM via SSH. Ersätt hello IP-adressen med hello `publicIpAddress` anges i hello föregående steg.

```bash
ssh 52.174.34.95
```

När du är klar med hello VM Stäng hello SSH-session. 

```bash
exit
```

## <a name="understand-vm-images"></a>Förstå VM-avbildningar

hello Azure marketplace innehåller många avbildningar som kan använda toocreate virtuella datorer. I föregående steg hello skapades en virtuell dator med en Ubuntu-bild. I det här steget används hello Azure CLI är används toosearch hello marketplace för en CentOS avbildningen, som sedan toodeploy en andra virtuell dator.  

toosee en lista över hello vanligaste bilder använder hello [az vm bildlista](/cli/azure/vm/image#list) kommando.

```azurecli-interactive 
az vm image list --output table
```

hello kommandoutdata returnerar hello populäraste VM-avbildningar i Azure.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

En fullständig lista över kan ses genom att lägga till hello `--all` argumentet. hello bildlista kan också filtreras efter `--publisher` eller `–-offer`. I det här exemplet hello listan filtreras för alla bilder med ett erbjudande som matchar *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Partiella utdata:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

toodeploy en virtuell dator med hjälp av en viss bild anteckna hello värdet i hello *Urn* kolumn. När du anger hello avbildningen kan versionsnumret för hello avbildningen ersättas med ”senaste”, som väljer hello senaste versionen av hello distribution. I det här exemplet hello `--image` argumentet är används toospecify hello senaste versionen av en CentOS 6.5-bild.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>Förstå VM-storlekar

Storlek på en virtuell dator bestäms hello beräkningsresurser som Processorn och GPU-minne som görs tillgängliga toohello virtuella datorn. Virtuella datorer måste toobe storlek på lämpligt sätt för hello förväntades arbetsbelastning. Om belastningen ökar, kan en befintlig virtuell dator ändras.

### <a name="vm-sizes"></a>VM-storlekar

i den följande tabellen hello kategoriserar storlekar i användningsfall.  

| Typ                     | Storlekar           |    Beskrivning       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Generellt syfte](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7| Belastningsutjämnade CPU-till-minne. Idealiskt för dev / test och lösningar för små toomedium program och data.  |
| [Beräkningsoptimerad](sizes-compute.md)   | FS, F             | Hög CPU-till-minne. Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.        |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Hög minne-till-core. Perfekt för relationsdatabaser, medelhög toolarge cacheminnen och analyser i minnet.                 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md)      | Ls                | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser.                                                         |
| [GPU](sizes-gpu.md)          | NV NC            | Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.       |
| [Hög prestanda](sizes-hpc.md) | H, A8-11          | Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA). 


### <a name="find-available-vm-sizes"></a>Hitta tillgängliga storlekar på VM

toosee en lista över Virtuella storlekar tillgängliga i en viss region, Använd hello [az lista-storlekar på vm](/cli/azure/vm#list-sizes) kommando. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Partiella utdata:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Skapa virtuell dator med specifika storlek

I hello tidigare VM skapa exempelvis har en storlek inte angetts, vilket resulterar i en standardstorlek. En VM-storlek kan väljas i Skapa en gång med hjälp av [az vm skapa](/cli/azure/vm#create) och hello `--size` argumentet. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>Ändra storlek på en virtuell dator

När en virtuell dator har distribuerats kan storleksändrade tooincrease eller minska resursallokering.

Innan du ändrar storlek på en virtuell dator, kontrollera om hello önskad storlek är tillgänglig på hello aktuella Azure-klustret. Hej [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options) kommandot returnerar hello lista över storlekar. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
Eventuellt hello storleken är tillgänglig hello VM storlek kan ändras från ett slås på tillstånd, men den startas under hello-åtgärd. Använd hello [az vm ändra storlek på]( /cli/azure/vm#resize) kommandot tooperform hello storlek.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

Om hello önskad storlek är inte på hello aktuella klustret hello VM behov toobe frigjorts innan hello att ändra storlek på kan uppstå. Använd hello [az vm frigöra]( /cli/azure/vm#deallocate) kommando toostop och frigöra hello VM. Observera att när hello VM är påslagen tillbaka, några data på hello temporär disk kan tas bort. hello offentliga IP-adressen ändras även om inte en statisk IP-adress används. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

När frigjorts, kan det uppstå hello storlek. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

När hello ändra storlek på kan hello VM startas.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>Energisparfunktioner för VM

En virtuell Azure-dator kan ha en av många energisparfunktioner. Det här tillståndet representerar hello aktuell status för hello VM från hello synvinkel av hello hypervisor. 

### <a name="power-states"></a>Energisparfunktioner

| Energisparläge | Beskrivning
|----|----|
| Startar | Anger hello virtuella datorn startas. |
| Körs | Anger att hello den virtuella datorn körs. |
| Stoppas | Anger att hello den virtuella datorn stoppas. | 
| Stoppad | Anger att hello den virtuella datorn har stoppats. Virtuella datorer i hello stoppats tillstånd fortfarande avgifter beräkning.  |
| Det har frigjorts | Anger att hello den virtuella datorn har flyttats. |
| Frigöra | Anger att hello den virtuella datorn tas bort från hello hypervisor men fortfarande tillgängliga i hello kontrollplan. Virtuella datorer i hello Deallocated tillstånd inte avgifter beräkning. |
| - | Anger att hello energisparläge för hello virtuell dator är okänt. |

### <a name="find-power-state"></a>Hitta energiläge

tooretrieve hello tillståndet för en viss virtuell dator, Använd hello [az vm hämta Instansvy](/cli/azure/vm#get-instance-view) kommando. Vara säker på att toospecify ett giltigt namn för en virtuell dator och resursgruppen. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Resultat:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Administrativa uppgifter

Under hello livscykeln för en virtuell dator, kan du toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator. Dessutom kan du toocreate skript tooautomate repetitiva och komplicerade uppgifter. Använder hello Azure CLI, kan många vanliga administrativa uppgifter köras från hello kommandoraden eller i skript. 

### <a name="get-ip-address"></a>Hämta IP-adress

Det här kommandot returnerar hello privata och offentliga IP-adresser för en virtuell dator.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Stoppa den virtuella datorn

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Starta den virtuella datorn

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Ta bort resursgruppen

En resursgrupp också tar du bort alla resurser som ingår i.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:

> [!div class="checklist"]
> * Skapa och ansluta tooa VM
> * Välj och Använd VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Ändra storlek på en virtuell dator
> * Visa och förstå tillstånd för virtuell dator

Avancera toohello nästa självstudiekurs toolearn om Virtuella diskar.  

> [!div class="nextstepaction"]
> [Skapa och hantera Virtuella diskar](./tutorial-manage-disks.md)
