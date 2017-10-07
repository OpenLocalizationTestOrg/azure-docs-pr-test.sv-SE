---
title: "ett virtuellt Azure-nätverk peering - aaaCreate Resource Manager - samma prenumeration | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk peering mellan virtuella nätverk skapas via Resource Manager som finns i hello samma Azure-prenumeration."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Skapa ett virtuellt nätverk peering - hanteraren för filserverresurser, samma prenumeration

I kursen får du lära dig toocreate ett virtuellt nätverk peering mellan virtuella nätverk som skapats via Resource Manager. Båda virtuella nätverk finns i hello samma prenumeration. Peering två virtuella nätverk gör resurser i olika virtuella nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om hello resurserna var i hello samma virtuella nätverk. Lär dig mer om [virtuella nätverk peering](virtual-network-peering-overview.md). 

hello steg toocreate ett virtuellt nätverk som peering är olika, beroende på om hello virtuella nätverk är i hello samma eller olika prenumerationer och som [Azure distributionsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuella nätverk skapas via. Lär dig hur toocreate ett virtuellt nätverk peering i andra scenarier genom att klicka på hello scenariot från hello följande tabell:

|Azure-distributionsmodell  | Azure-prenumeration  |
|--------- |---------|
|[Båda Resource Manager](create-peering-different-subscriptions.md) |Olika|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models.md) |samma|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models-subscriptions.md) |Olika|

Att går inte skapa ett virtuellt nätverk som peering mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen. Ett virtuellt nätverk som peering kan bara skapas mellan två virtuella nätverk som finns i hello samma Azure-region. Om du behöver tooconnect virtuella nätverk som skapats både via hello klassiska distributionsmodellen eller som finns i olika Azure-regioner kan du använda en Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuella nätverk. 

Du kan använda hello [Azure-portalen](#portal), hello Azure [kommandoradsgränssnittet](#cli) (CLI) Azure [PowerShell](#powershell), eller en [Azure Resource Manager-mall](#template) toocreate peering ett virtuellt nätverk. Klicka på någon av hello tidigare verktyget länkar toogo direkt toohello steg för att skapa ett virtuellt nätverk peering verktyget dina val.

## <a name="portal"></a>Skapa peering - Azure-portalen

1. Logga in toohello [Azure-portalen](https://portal.azure.com). hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
2. Klicka på **+ ny**, klickar du på **nätverk**, klicka på **för virtuella nätverk**.
3. I hello **skapa virtuellt nätverk** bladet anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:
    - **Namnet**: *myVnet1*
    - **Adressutrymmet**: *10.0.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.0.0.0/24*
    - **Prenumerationen**: väljer din prenumeration
    - **Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroup*
    - **Plats**: *östra USA*
4. Slutför steg 2 – 3 igen att ange hello följande värden i steg 3:
    - **Namnet**: *myVnet2*
    - **Adressutrymmet**: *10.1.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.1.0.0/24*
    - **Prenumerationen**: väljer din prenumeration
    - **Resursgruppen**: Välj **använda befintliga** och välj *myResourceGroup*
    - **Plats**: *östra USA*
5. I hello **söka resurser** rutan hello överst i hello portal, typen *myResourceGroup*. Klicka på **myResourceGroup** när den visas i sökresultaten hello. Ett blad som visas för hello **myresourcegroup** resursgruppen. hello resursgruppen innehåller hello två virtuella nätverk skapas i föregående steg.
6. Klicka på **myVNet1**.
7. I hello **myVnet1** bladet som visas, klickar du på **Peerkopplingar** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
8. I hello **myVnet1 - Peerkopplingar** bladet som visas, klickar du på **+ Lägg till**
9. I hello **Lägg till peering** bladet som visas anger, eller välj hello följande alternativ och klicka på **OK**:
     - **Namnet**: *myVnet1ToMyVnet2*
     - **Virtuellt nätverk distributionsmodell**: Välj **Resource Manager**. 
     - **Prenumerationen**: väljer din prenumeration
     - **Virtuellt nätverk**: Klicka på **Välj ett virtuellt nätverk**, klicka på **myVnet2**.
     - **Tillåt åtkomst till virtuella nätverk:** se till att **aktiverad** är markerad.
    Inga andra inställningar används i den här kursen. Läs toolearn om inställningar för alla peering [hantera peerkopplingar mellan virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
10. När du klickar på **OK** i föregående steg hello hello **Lägg till peering** blad stängs och du ser hello **myVnet1 - Peerkopplingar** bladet igen. Efter några sekunder visas hello peering du skapade i hello-bladet. **Initierade** visas i hello **PEERING STATUS** för hello **myVnet1ToMyVnet2** peering du skapat. Du har peerkoppla Vnet1 tooVnet2, men du måste nu peer-myVnet2 toomyVnet1. Hej peering måste skapas i båda riktningarna tooenable resurser i hello virtuella nätverk toocommunicate med varandra.
11. Slutför steg 5 – 10 igen för myVnet2.  Namnet hello peering *myVnet2ToMyVnet1*.
12. Några sekunder när du klickar på **OK** toocreate hello-peering för MyVnet2, hello **myVnet2ToMyVnet1** peering du precis har skapat visas med **ansluten** i hello  **PEERING STATUS** kolumn.
13. Slutför steg 5 – 7 igen för MyVnet1. Hej **PEERING STATUS** för hello **myVnet1ToVNet2** peering är nu också **ansluten**. Hej peering etablerats efter att du ser **ansluten** i hello **PEERING STATUS** för både virtuella nätverk i hello peering.
14. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
15. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete-portal) i den här artikeln.

Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Skapa peering - Azure CLI

Hej följande skript:

- Kräver hello Azure CLI version 2.0.4 eller senare. toofind hello version, kör hello `az --version` kommando. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Fungerar i ett Bash-gränssnitt. Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows hello Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Du kan använda hello Azure Cloud Shell istället för att installera hello CLI och dess beroenden. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. Klicka på hello **prova** knappen i hello-skript som anropar ett moln-gränssnitt som loggar du kan logga in tooyour Azure-konto med. tooexecute hello skript klickar du på hello **kopiera** knappen och klistra in hello innehållet i molnet-gränssnittet.

1. Skapa en resursgrupp och två virtuella nätverk.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk.
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. När hello skriptet körs, granska hello peerkopplingar för varje virtuellt nätverk. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Kör hello föregående kommando igen, ersätter *myVnet1* med *myVnet2*. hello utdata från både kommandon visar **ansluten** i hello **PeeringState** kolumn.

     Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
5. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.


## <a name="powershell"></a>Skapa peering - PowerShell

1. Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Gå toostart en PowerShell-session för**starta**, ange **powershell**, och klicka sedan på **PowerShell**.
3. Logga in tooAzure i PowerShell genom att ange hello `login-azurermaccount` kommando. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
4. Skapa en resursgrupp och två virtuella nätverk. tooexecute hello skriptet kopiera hello följande skript, klistra in den i PowerShell och tryck sedan på `Enter` när hello sista raden visas på hello-skärmen:

    ```powershell
    # Variables for common values used throughout hello script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk. Kopiera hello följande skript, klistra in i tooPowerShell och tryck sedan på `Enter` när hello sista raden visas på hello-skärmen:
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. tooreview hello undernät för virtuellt nätverk för hello, kopiera hello följande kommando, klistra in i tooPowerShell och tryck sedan på `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Kör hello föregående kommando igen, ersätter *myVnet1* med *myVnet2*. hello utdata från både kommandon visar **ansluten** i hello **PeeringState** kolumn.
7. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
8. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.

Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Skapa peering - Resource Manager-mall

1. Referens [skapa ett virtuellt nätverk som peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager-mall. Instruktioner finns angivna med hello mallen för distribution av hello mallen med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI. Logga in toowhichever verktyg du väljer toodeploy hello mall med ett konto som har hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
2. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
3. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete) i den här artikeln använder antingen hello Azure-portalen, PowerShell eller hello Azure CLI.

## <a name="permissions"></a>Behörigheter

hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter. Till exempel om du peering två virtuella nätverk som heter VNet1 och VNet2, måste ditt konto tilldelas hello följande minsta eller behörigheter för varje virtuellt nätverk:
    
|Virtuellt nätverk|Roll|Behörigheter|
|---|---|---|
|VNet1|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).

## <a name="delete"></a>Ta bort resurser
När du är klar med den här kursen kan kanske du vill toodelete hello resurser som du skapade i hello självstudier, så att du inte betalar användningsavgifter. En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.

### <a name="delete-portal"></a>Azure-portalen

1. Ange i hello portal sökrutan **myResourceGroup**. I hello sökresultaten klickar du på **myResourceGroup**.
2. På hello **myResourceGroup** bladet, klickar du på hello **ta bort** ikon.
3. tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroup**, och klicka sedan på **ta bort**.

### <a name="delete-cli"></a>Azure CLI

Ange hello följande kommando:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Ange hello följande kommando:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a>Nästa steg

- Väl bekanta dig med viktiga [peering begränsningar för virtuellt nätverk och beteenden](virtual-network-manage-peering.md#requirements-and-constraints) innan du skapar ett virtuellt nätverk peering för produktion använder.
- Lär dig mer om alla [peering inställningar för virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
- Lär dig hur för[skapa ett nav och eker nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) med virtuella nätverk peering.
