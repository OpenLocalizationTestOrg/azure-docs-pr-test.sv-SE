---
title: "aaaCreate virtuellt Azure-nätverk med flera undernät | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk med flera undernät i Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 0f56fa6ac24537d33b8e217f5b03f387826ab487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Skapa ett virtuellt nätverk med flera undernät

I den här kursen lär du dig hur toocreate en grundläggande Azure virtuellt nätverk med separata offentliga och privata undernät. Du kan skapa Azure-resurser, t.ex. virtuella datorer, apptjänstmiljöer, skalningsuppsättningar i virtuella datorer, Azure HDInsight och molntjänster i ett undernät. Resurser i virtuella nätverk kan kommunicera med varandra och resurser i andra nätverk anslutet tooa virtuella nätverket.

hello följande avsnitt innehåller steg som du kan vidta toocreate ett virtuellt nätverk med hjälp av hello [Azure-portalen](#portal), hello Azure-kommandoradsgränssnittet ([Azure CLI](#azure-cli)), [Azure PowerShell ](#powershell), och en [Azure Resource Manager-mall](#resource-manager-template). hello resultatet är hello samma, oavsett vilket verktyg du använda toocreate hello virtuellt nätverk. Klicka på verktyget länken toogo toothat avsnitt av kursen hello. Mer information om alla [virtuellt nätverk](virtual-network-manage-network.md) och [undernät](virtual-network-manage-subnet.md) inställningar.

Den här artikeln innehåller steg toocreate ett virtuellt nätverk med hello Resource Manager-distributionsmodellen, som är hello distributionsmodell som vi rekommenderar att du använder när du skapar nya virtuella nätverk. Om du behöver toocreate ett virtuellt nätverk (klassiska), se [skapa ett virtuellt nätverk (klassiska)](create-virtual-network-classic.md). Om du inte är bekant med Azures distributionsmodeller [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Azure-portalen

1. I en webbläsare går toohello [Azure-portalen](https://portal.azure.com). Logga in med ditt [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
2. I hello-portalen klickar du på **+ ny** > **nätverk** > **för virtuella nätverk**.
3. På hello **skapa virtuellt nätverk** bladet ange hello följande värden och klicka sedan på **skapa**:

    |Inställning|Värde|
    |---|---|
    |Namn|myVnet|
    |Adressutrymme|10.0.0.0/16|
    |Namn på undernät|Offentligt|
    |Adressintervall för undernätet|10.0.0.0/24|
    |Resursgrupp|Lämna **Skapa nytt** markerad och ange sedan **myResourceGroup**.|
    |Prenumerationen och platsen|Välj din prenumeration och plats.

    Om du är ny tooAzure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (kallas även tooas *regioner*).
4. Du kan skapa bara ett undernät när du skapar ett virtuellt nätverk i hello-portalen. I den här kursen skapar du ett andra undernät när du har skapat hello virtuellt nätverk. Du kan senare skapar Internet-tillgängliga resurser i hello **offentliga** undernät. Du kan också skapa resurser som inte är tillgänglig från hello Internet i hello **privata** undernät. toocreate hello andra undernät i hello **söka resurser** hello överst på hello sidan och ange **myVnet**. I hello sökresultaten klickar du på **myVnet**. Om du har flera virtuella nätverk med samma namn i prenumerationen hello markerar hello resursgrupper som visas under varje virtuellt nätverk. Se till att du klickar på hello **myVnet** sökresultat som har hello resursgruppen **myResourceGroup**.
5. På hello **myVnet** bladet under **inställningar**, klickar du på **undernät**.
6. På hello **myVnet - undernät** bladet, klickar du på **+ undernät**.
7. På hello **Lägg till undernät** bladet för **namn**, ange **privata**. För **adressintervall**, ange **10.0.1.0/24**.  Klicka på **OK**.
8. På hello **myVnet - undernät** bladet, granska hello undernät. Du kan se hello **offentliga** och **privata** undernät som du skapade.
9. **Valfritt:** toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-portal) i den här artikeln.

## <a name="azure-cli"></a>Azure CLI

Azure CLI-kommandona är hello samma, oavsett om du köra hello kommandon från Windows, Linux eller macOS. Det finns dock scripting skillnader mellan gränssnitt för operativsystemet. hello skriptet i hello följande steg utförs i ett Bash-gränssnitt. 

1. [Installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. hello molnet Shell har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell (**> _**) knappen hello överst i hello [portal](https://portal.azure.com) eller klicka bara på hello *prova* knapp i hello steg som följer. 
2. Om du kör hello CLI lokalt, loggar du in tooAzure med hello `az login` kommando. Om du använder hello molnet Shell är du redan inloggad.
3. Granska hello följande skript och dess kommentarer. Kopiera hello skript i webbläsaren och klistra in den i sessionen CLI:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in hello virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. När hello skriptet är klar kör, granska hello undernät för hello virtuellt nätverk. Kopiera följande kommando hello och klistra in den i sessionen CLI:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.

## <a name="powershell"></a>PowerShell

1. Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. I PowerShell-sessionen, logga in tooAzure med din [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) med hello `login-azurermaccount` kommando.

3. Granska hello följande skript och dess kommentarer. Kopiera hello skript i webbläsaren och klistra in den i PowerShell-sessionen:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create hello public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. tooreview hello undernät för virtuellt nätverk hello kopiera hello följande kommando och klistra in den i PowerShell-sessionen:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.

## <a name="resource-manager-template"></a>Resource Manager-mall

Du kan distribuera ett virtuellt nätverk med en Azure Resource Manager-mall. toolearn mer information om mallar finns [vad är Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). tooaccess hello mallen och toolearn om dess parametrar finns hello [skapa ett virtuellt nätverk med två undernät](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) mall. Du kan distribuera hello mallen med hjälp av hello [portal](#template-portal), [Azure CLI](#template-cli), eller [PowerShell](#template-powershell).

**Valfritt:** toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i alla avsnitt i [bort resurser](#delete) i den här artikeln.

### <a name="template-portal"></a>Azure-portalen

1. Öppna i din webbläsare hello [mallsidan](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klicka på hello **distribuera tooAzure** knappen. Om du inte redan är inloggad i tooAzure kan du logga in på hello Azure portal inloggningsskärm som visas.
3. Logga in på toohello portal med din [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Ange följande värden för parametrar hello hello:

    |Parameter|Värde|
    |---|---|
    |Prenumeration|Välj din prenumeration|
    |Resursgrupp|myResourceGroup|
    |Plats|Välj en plats|
    |Namn på virtuella nätverk|myVnet|
    |Vnet-adressprefix|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Offentligt|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Privat|

5. Godkänn toohello villkoren och klicka sedan på **inköp** toodeploy hello virtuellt nätverk.

### <a name="template-cli"></a>Azure CLI

1. [Installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. hello molnet Shell har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com), eller klicka bara på hello **prova** knapp i hello steg som följer. 
2. Om du kör hello CLI lokalt, loggar du in tooAzure med hello `az login` kommando. Om du använder hello molnet Shell är du redan inloggad.
3. toocreate en resursgrupp för hello virtuellt nätverk, kopiera hello följande kommando och klistra in den i sessionen CLI:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Du kan distribuera hello mallen med hjälp av något av följande alternativ för parametrar hello:
    - **Standardvärdena för parametern**. Ange hello följande kommando:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Anpassade parametervärden**. Hämta och ändra hello mallen innan du distribuerar hello mallen. Du också distribuera hello mallen med parametrar på kommandoraden för hello eller distribuera hello mallen med en separat parameterfil. toodownload hello-mallen och parametrar filer, klicka på hello **Bläddra på GitHub** hello-knappen [skapa ett virtuellt nätverk med två undernät](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) mall. I GitHub klickar du på hello **azuredeploy.parameters.json** eller **azuredeploy.json** fil. Klicka på hello **Raw** knappen toodisplay hello-filen. Kopiera hello innehållet i hello-filen i webbläsaren. Spara hello innehållet tooa fil på din dator. Du kan ändra hello parametervärden i hello mallen, eller distribuera hello mallen med en separat parameterfil.  

    toolearn mer om hur toodeploy mallar med hjälp av följande metoder, Skriv `az group deployment create --help`.

### <a name="template-powershell"></a>PowerShell

1. Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. I PowerShell-sessionen, toosign in med ditt [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), ange `login-azurermaccount`.
3. toocreate en resursgrupp för hello virtuellt nätverk, ange hello följande kommando:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Du kan distribuera hello mallen med hjälp av något av följande alternativ för parametrar hello:
    - **Standardvärdena för parametern**. Ange hello följande kommando:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Anpassade parametervärden**. Hämta och ändra hello mallen innan du distribuerar den. Du också distribuera hello mallen med parametrar på kommandoraden för hello eller distribuera hello mallen med en separat parameterfil. toodownload hello-mallen och parametrar filer, klicka på hello **Bläddra på GitHub** hello-knappen [skapa ett virtuellt nätverk med två undernät](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) mall. I GitHub klickar du på hello **azuredeploy.parameters.json** eller **azuredeploy.json** fil. Klicka på hello **Raw** knappen toodisplay hello-filen. Kopiera hello innehållet i hello-filen i webbläsaren. Spara hello innehållet tooa fil på din dator. Du kan ändra hello parametervärden i hello mallen, eller distribuera hello mallen med en separat parameterfil.  

    toolearn mer om hur toodeploy mallar med hjälp av följande metoder, Skriv `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Ta bort resurser

När du är klar med den här kursen kanske du vill toodelete hello resurser som du skapade, så att du inte betalar användningsavgifter. En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.

### <a name="delete-portal"></a>Azure-portalen

1. Ange i hello portal sökrutan **myResourceGroup**. I hello sökresultaten klickar du på **myResourceGroup**.
2. På hello **myResourceGroup** bladet, klickar du på hello **ta bort** ikon.
3. tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroup**, och klicka sedan på **ta bort**.

### <a name="delete-cli"></a>Azure CLI

Ange hello följande kommando i en CLI-session:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Ange hello följande kommando i en PowerShell-session:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nästa steg

- toolearn om alla virtuella nätverk och undernätsinställningar Se [hantera virtuella nätverk](virtual-network-manage-network.md#view-vnet) och [hantera virtuella undernät](virtual-network-manage-subnet.md#create-subnet). Har du olika alternativ för att använda virtuella nätverk och undernät i en produktionsmiljö miljö toomeet olika krav.
- toofilter inkommande och utgående trafik på undernät, skapa och använda [nätverkssäkerhetsgrupper](virtual-networks-nsg.md) toosubnets.
- Skapa en [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller en [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorn och ansluter sedan tooan befintligt virtuellt nätverk.
- tooconnect två virtuella nätverk i hello samma Azure-plats måste du skapa en [virtuellt nätverk peering](virtual-network-peering-overview.md) mellan hello virtuella nätverk.
- Ansluta hello virtuellt nätverk tooan lokalt nätverk med hjälp av en [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) krets.
