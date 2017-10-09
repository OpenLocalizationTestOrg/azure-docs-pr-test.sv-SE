---
title: aaaCapture en toouse virtuella Azure Linux-datorn som en mall | Microsoft Docs
description: "Lär dig hur toocapture och generalisera en avbildning av en Linux-baserade Azure virtuell dator (VM) skapas med hello Azure Resource Manager-distributionsmodellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Avbilda en Linux-dator som körs på Azure
Följ hello stegen i den här artikeln toogeneralize och avbilda dina Azure Linux-dator (VM) i hello Resource Manager-distributionsmodellen. När du generaliserar hello VM ta bort personlig information och förbereda hello VM toobe används som en bild. Du sedan avbilda en generaliserad virtuell hårddisk (VHD) bild för hello OS, virtuella hårddiskar för bifogade datadiskar och en [Resource Manager-mall](../../azure-resource-manager/resource-group-overview.md) för nya VM-distributioner. Den här artikeln beskrivs hur toocapture en virtuell dator av avbildning med hello Azure CLI 1.0 för en virtuell dator med hjälp av ohanterade diskar. Du kan också [avbilda en virtuell dator med hjälp av Azure hanterade diskar med hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Hanterade diskar som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem. Mer information finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

toocreate virtuella datorer med hjälp av hello avbildning konfigurera nätverksresurser för varje ny virtuell dator och använda hello mall (en JavaScript Object Notation eller JSON,-fil) toodeploy från hello avbildas VHD-avbildningar. På så sätt kan replikera du en virtuell dator med dess aktuella programvarukonfiguration, liknande toohello sätt du använda bilder i hello Azure Marketplace.

> [!TIP]
> Om du vill toocreate en kopia av din befintliga Linux VM med det särskilda tillståndet för säkerhetskopiering eller felsökning, se [skapar en kopia av en Linux-dator som körs på Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Och om du vill tooupload en Linux VHD från en lokal virtuell dator, se [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#before-you-begin) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="before-you-begin"></a>Innan du börjar
Se till att du uppfyller hello följande krav:

* **Azure VM som skapats i hello Resource Manager-distributionsmodellen** -om du inte har skapat en Linux VM, kan du använda hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), eller [Resource Manager mallar](create-ssh-secured-vm-from-template.md). 
  
    Konfigurera hello VM efter behov. Till exempel [lägga till datadiskar](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), tillämpa uppdateringar och installera program. 
* **Azure CLI** -installera hello [Azure CLI](../../cli-install-nodejs.md) på en lokal dator.

## <a name="step-1-remove-hello-azure-linux-agent"></a>Steg 1: Ta bort hello Azure Linux-agent
Kör först hello **waagent** med hello **avetablering** parameter på hello Linux VM. Det här kommandot tar bort filer och data toomake hello VM redo för att generalisera. Mer information finns i hello [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Ansluta tooyour Linux VM som använder en SSH-klient.
2. Ange följande kommando hello i hello SSH fönstret:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Endast köra kommandot på en virtuell dator som du avser toocapture som en bild. Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för omfördelning.
 
3. Typen **y** toocontinue. Du kan lägga till hello **-tvinga** parametern tooavoid åtgärden bekräftelse.
4. När hello-kommandot har slutförts genom att skriva **avsluta**. Det här steget stänger hello SSH-klienten.

## <a name="step-2-capture-hello-vm"></a>Steg 2: Samla in hello VM
Använd hello Azure CLI toogeneralize och avbilda hello VM. Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar **myResourceGroup**, **myVnet**, och **myVM**.

1. Öppna hello Azure CLI från den lokala datorn och [inloggning tooyour Azure-prenumeration](../../xplat-cli-connect.md). 
2. Kontrollera att du är i läget Resource Manager.
   
    ```azurecli
    azure config mode arm
    ```
3. Stänga av hello virtuell dator som du redan avetableras med hjälp av hello följande kommando:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. Generalisera hello VM med hello följande kommando:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Kör nu hello **azure vm-avbildning** kommando, som registrerar hello VM. I följande exempel hello, hello avbildningen virtuella hårddiskar till med namn som börjar med **MyVHDNamePrefix**, och hello **-t** alternativet anger en sökväg toohello mall **MyTemplate.json**. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > hello avbildningen VHD-filer skapas som standard i hello används samma lagringskonto som hello ursprungliga virtuella datorn. Använd hello *samma lagringskonto* toostore hello virtuella hårddiskar för alla nya virtuella datorer som du skapar från hello avbildning. 

6. toofind hello platsen för avbildning öppna hello JSON-mall i en textredigerare. I hello **storageProfile**, hitta hello **uri** av hello **bild** finns i hello **system** behållare. Till exempel är hello diskavbildning hello OS-URI liknande för`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>Steg 3: Skapa en virtuell dator från hello avbildas avbildning
Nu använda hello avbildning med en mall toocreate en Linux VM. Dessa steg visar hur toouse hello Azure CLI och hello JSON mall du hämtat toocreate hello VM i ett nytt virtuellt nätverk.

### <a name="create-network-resources"></a>Skapa nätverksresurser
toouse hello mall måste du först tooset ett virtuellt nätverk och nätverkskort för den nya virtuella datorn. Vi rekommenderar att du skapar en resursgrupp för dessa resurser i hello plats där VM-avbildning lagras. Kör kommandon liknande toohello efter, ersätter namn för dina resurser och en lämplig Azure plats (”centralus” i de här kommandona):

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a>Hämta hello-Id för hello NIC
toodeploy en virtuell dator från hello avbildningen med hjälp av hello JSON som du sparade under hämtning, behöver du hello-Id för hello NIC. Du kan skaffa det genom att köra följande kommando hello:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

Hej **Id** i hello utdata är liknande för`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`

### <a name="create-a-vm"></a>Skapa en virtuell dator
Nu kör hello följande kommando toocreate avbildas den virtuella datorn från hello VM-avbildning. Använd hello **-f** parametern toospecify hello sökvägen toohello mallens JSON-filen som du sparade.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

I hello kommandoutdata du tillfrågas toosupply ett nytt namn på virtuell dator, hello admin-användarnamn och lösenord och hello-Id för hello NIC som du skapade tidigare.

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

hello som följande exempel visar vad som visas för en lyckad distribution:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-hello-deployment"></a>Kontrollera distributionen av hello
Nu SSH toohello virtuella datorn du har skapat tooverify hello distribution och börjar använda hello ny virtuell dator. tooconnect via SSH, hitta hello IP-adressen för hello VM som du skapade genom att köra följande kommando hello:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

hello offentliga IP-adressen listas i hello kommandoutdata. Som standard ansluter toohello Linux VM av SSH på port 22.

## <a name="create-additional-vms"></a>Skapa ytterligare virtuella datorer
Använd hello avbildas avbildningen och mallen toodeploy ytterligare virtuella datorer med hello stegen i föregående avsnitt hello. Andra alternativ toocreate virtuella datorer från hello bilden innehåller en mall i Snabbstart eller kör hello **azure vm skapa** kommando.

### <a name="use-hello-captured-template"></a>Använd hello avbildade mall
toouse hello avbildas avbildningen och mallen, Följ dessa steg (beskrivs i föregående avsnitt hello):

* Se till att VM-avbildning hello samma lagringskonto som är värd för den Virtuella datorns virtuella Hårddisken.
* Kopiera hello mallen JSON-fil och ange ett unikt namn för hello OS-disken hello nya VM VHD (eller virtuella hårddiskar). Till exempel i hello **storageProfile**under **vhd**i **uri**, ange ett unikt namn för hello **osDisk** virtuell Hårddisk, liknande för`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Skapa ett nätverkskort i antingen hello samma eller ett annat virtuellt nätverk.
* Skapa en distribution i hello resursgrupp som du ställer in hello virtuellt nätverk med hello ändrade mallen JSON-fil.

### <a name="use-a-quickstart-template"></a>Använd en mall för Snabbstart
Om du vill hello nätverksinstallation automatiskt när du skapar en virtuell dator från hello avbildning kan ange du resurserna i en mall. Se exempelvis hello [101-vm-från--användaravbildning mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) från GitHub. Den här mallen skapar en virtuell dator från din anpassade avbildningen och hello nödvändiga virtuella nätverk, offentlig IP-adress och NIC-resurser. En genomgång av hello mallen i hello Azure-portalen finns [hur toocreate en virtuell dator från en anpassad avbildning med hjälp av en Resource Manager-mall](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-hello-azure-vm-create-command"></a>Använd hello azure vm skapa kommando
Vanligtvis är det enklaste toouse toocreate en Resource Manager-mallen en virtuell dator från hello avbildning. Du kan dock skapa hello VM *imperatively* med hjälp av hello **azure vm skapa** med hello **-Q** (**--bild urn**) parameter . Om du använder den här metoden kan du också ange hello **-d** (**--os-disk-vhd**) parametern toospecify hello platsen för hello OS VHD-filen för hello ny virtuell dator. Den här filen måste vara i hello VHD-behållare för hello storage-konto där hello avbildningen VHD-filen lagras. Hej kommandot kopior hello VHD för hello nya virtuella datorn automatiskt toohello **virtuella hårddiskar** behållare.

Innan du kör **azure vm skapa** med hello avbildningen slutföra hello följande steg:

1. Skapa en resursgrupp eller identifiera en befintlig resursgrupp för hello-distribution.
2. Skapa en offentlig IP-adressresurs och en NIC-resurs för hello ny virtuell dator. För steg toocreate finns ett virtuellt nätverk, offentliga IP-adress och NIC med hjälp av hello CLI, tidigare i den här artikeln. (**azure vm skapa** kan också skapa ett nätverkskort, men du måste toopass ytterligare parametrar för ett virtuellt nätverk och undernät.)

Kör ett kommando som klarar URI: er tooboth hello nya OS VHD-filen och hello befintlig avbildning. I det här exemplet en storlek Standard_A1 VM har skapats i hello östra USA region.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

Ytterligare alternativ för, kör `azure help vm create`.

## <a name="next-steps"></a>Nästa steg
toomanage dina virtuella datorer med hello CLI, se hello uppgifter i [distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI](create-ssh-secured-vm-from-template.md).

