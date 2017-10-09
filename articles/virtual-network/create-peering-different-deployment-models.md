---
title: aaaCreate ett Azure virtual network peering - olika distributionsmodeller - samma prenumeration | Microsoft Docs
description: "Lär dig hur toocreate ett virtuellt nätverk peering mellan virtuella nätverk skapas via olika Azure distributionsmodeller som finns i hello samma Azure-prenumeration."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: 365156d651c9042ed52baeb15bf629fcc5329af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Skapa ett virtuellt nätverk peering - olika distributionsmodeller, samma prenumeration 

I kursen får du lära dig toocreate ett virtuellt nätverk peering mellan virtuella nätverk som skapats via olika distributionsmodeller. Båda virtuella nätverk finns i hello samma prenumeration. Peering två virtuella nätverk gör resurser i olika virtuella nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om hello resurserna var i hello samma virtuella nätverk. Lär dig mer om [virtuella nätverk peering](virtual-network-peering-overview.md). 

hello steg toocreate ett virtuellt nätverk som peering är olika, beroende på om hello virtuella nätverk är i hello samma eller olika prenumerationer och som [Azure distributionsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuella nätverk skapas via. Lär dig hur toocreate ett virtuellt nätverk peering i andra scenarier genom att klicka på hello scenariot från hello följande tabell:

|Azure-distributionsmodell  | Azure-prenumeration  |
|--------- |---------|
|[Båda Resource Manager](virtual-network-create-peering.md) |samma|
|[Båda Resource Manager](create-peering-different-subscriptions.md) |Olika|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models-subscriptions.md) |Olika|

Att går inte skapa ett virtuellt nätverk som peering mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen. Ett virtuellt nätverk som peering kan bara skapas mellan två virtuella nätverk som finns i hello samma Azure-region. Om du behöver tooconnect virtuella nätverk som skapats både via hello klassiska distributionsmodellen eller som finns i olika Azure-regioner kan du använda en Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuella nätverk. 

Du kan använda hello [Azure-portalen](#portal), hello Azure [kommandoradsgränssnittet](#cli) (CLI) eller Azure [PowerShell](#powershell) toocreate peering ett virtuellt nätverk. Klicka på någon av hello tidigare verktyget länkar toogo direkt toohello steg för att skapa ett virtuellt nätverk peering verktyget dina val.

## <a name="cli"></a>Skapa peering - Portal

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
4. Klicka på **+ ny**. I hello **Sök hello Marketplace** skriver *för virtuella nätverk*. Klicka på **för virtuella nätverk** när den visas i sökresultaten hello. 
5. I hello **för virtuella nätverk** bladet väljer **klassiska** i hello **Välj en distributionsmodell** rutan och klicka på **skapa**.
6. I hello **skapa virtuellt nätverk** bladet anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:
    - **Namnet**: *myVnet2*
    - **Adressutrymmet**: *10.1.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.1.0.0/24*
    - **Prenumerationen**: väljer din prenumeration
    - **Resursgruppen**: Välj **använda befintliga** och välj *myResourceGroup*
    - **Plats**: *östra USA*
7. I hello **söka resurser** rutan hello överst i hello portal, typen *myResourceGroup*. Klicka på **myResourceGroup** när den visas i sökresultaten hello. Ett blad som visas för hello **myresourcegroup** resursgruppen. hello resursgruppen innehåller hello två virtuella nätverk skapas i föregående steg.
8. Klicka på **myVNet1**.
9. I hello **myVnet1** bladet som visas, klickar du på **Peerkopplingar** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
10. I hello **myVnet1 - Peerkopplingar** bladet som visas, klickar du på **+ Lägg till**
11. I hello **Lägg till peering** bladet som visas anger, eller välj hello följande alternativ och klicka på **OK**:
     - **Namnet**: *myVnet1ToMyVnet2*
     - **Virtuellt nätverk distributionsmodell**: Välj **klassiska**. 
     - **Prenumerationen**: väljer din prenumeration
     - **Virtuellt nätverk**: Klicka på **Välj ett virtuellt nätverk**, klicka på **myVnet2**.
     - **Tillåt åtkomst till virtuella nätverk:** se till att **aktiverad** är markerad.
    Inga andra inställningar används i den här kursen. Läs toolearn om inställningar för alla peering [hantera peerkopplingar mellan virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
12. När du klickar på **OK** i föregående steg hello hello **Lägg till peering** blad stängs och du ser hello **myVnet1 - Peerkopplingar** bladet igen. Efter några sekunder visas hello peering du skapade i hello-bladet. **Ansluten** visas i hello **PEERING STATUS** för hello **myVnet1ToMyVnet2** peering du skapat.

    Hej peering upprättas. Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
13. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
14. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete-portal) i den här artikeln.

## <a name="cli"></a>Skapa peering - Azure CLI

1. [Installera](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuella nätverk (klassiska).
2. Öppna en kommandosession och logga in med hjälp av hello tooAzure `azure login` kommando.
3. Kör hello CLI i Service Management-läge genom att ange hello `azure config mode asm` kommando.
4. Ange följande kommando toocreate hello virtuella nätverk (klassiska) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Skapa en resursgrupp och ett virtuellt nätverk (Resource Manager). Du kan använda hello CLI 1.0 eller 2.0 ([installera](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)). I den här kursen är hello CLI 2.0 används toocreate hello virtuella nätverk (Resource Manager), eftersom 2.0 måste vara används toocreate hello peering. Köra hello följande bash CLI-skriptet från din lokala dator med hello CLI 2.0.4 eller senare installerat. Alternativen på körs bash CLI-skript på Windows-klient, finns [kör Windows hello Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Du kan också köra hello skript med hello Azure Cloud-gränssnittet. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. Klicka på hello **prova** knappen i hello-skript som anropar ett moln-gränssnitt som loggar du kan logga in tooyour Azure-konto med. tooexecute hello skript klickar du på hello **kopiera** knappen och klistra in hello innehållet i molnet gränssnittet, tryck på `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create hello virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk skapas med hello olika distributionsmodeller. Kopiera hello följande skript tooa textredigerare på datorn. Ersätt `<subscription id>` med ditt prenumerations-Id. Om du inte vet ditt prenumerations-Id, ange hello `az account show` kommando. Hej värde för **id** i hello utdata är ditt prenumerations-Id. Klistra in hello ändrade skript i tooyour CLI-sessionen och tryck sedan på `Enter`.

    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. När hello skriptet körs, kan du granska hello-peering för hello virtuella nätverk (Resource Manager). Kopiera hello följande kommando, klistra in den i CLI-sessionen och tryck sedan på `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Visar utdata i hello **ansluten** i hello **PeeringState** kolumn. 

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
8. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
9. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.

## <a name="powershell"></a>Skapa peering - PowerShell

1. Installera hello senaste versionen av hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) och [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) moduler. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starta en PowerShell-session.
3. Logga in tooAzure i PowerShell genom att ange hello `Add-AzureAccount` kommando.
4. toocreate ett virtuellt nätverk (klassiskt) med PowerShell, du måste skapa en ny eller ändra en befintlig konfigurationsfil för nätverket. Lär dig hur för[exportera, uppdatera och importera konfigurationsfiler för nätverket](virtual-networks-using-network-configuration-file.md). hello filen bör inkludera hello följande **VirtualNetworkSite** element för hello virtuella nätverk som används i den här självstudiekursen:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Importera en konfigurationsfil för ändrade nätverket kan orsaka ändringar tooexisting virtuella nätverk (klassiskt) i din prenumeration. Se till att du bara lägga till hello tidigare virtuella nätverket och att du inte ändra eller ta bort alla befintliga virtuella nätverk från prenumerationen. 
5. Logga in tooAzure toocreate hello virtuella nätverk (Resource Manager) genom att ange hello `login-azurermaccount` kommando. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
6. Skapa en resursgrupp och ett virtuellt nätverk (Resource Manager). Kopiera hello skript, klistrar in den i PowerShell och tryck sedan på `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create hello virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk skapas med hello olika distributionsmodeller. Kopiera hello följande skript tooa textredigerare på datorn. Ersätt `<subscription id>` med ditt prenumerations-Id. Om du inte vet ditt prenumerations-Id, ange hello `Get-AzureRmSubscription` kommandot tooview den. Hej värde för **Id** hello returnerade utdata är ditt prenumerations-ID. tooexecute hello skriptet kopiera hello ändrade skriptet från en textredigerare, högerklicka på PowerShell-sessionen och tryck sedan på `Enter`.

    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. När hello skriptet körs, kan du granska hello-peering för hello virtuella nätverk (Resource Manager). Kopiera hello följande kommando, klistra in den i PowerShell-sessionen och tryck sedan på `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Visar utdata i hello **ansluten** i hello **PeeringState** kolumn.

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

9. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
10. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.
 
## <a name="permissions"></a>Behörigheter

hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter. Till exempel om du peering två virtuella nätverk som heter myVnet1 och myVnet2, måste ditt konto tilldelas hello följande minsta eller behörigheter för varje virtuellt nätverk:
    
|Virtuellt nätverk|Distributionsmodell|Roll|Behörigheter|
|---|---|---|---|
|myVnet1|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Saknas|
|myVnet2|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).

## <a name="delete"></a>Ta bort resurser
När du är klar med den här kursen kan kanske du vill toodelete hello resurser som du skapade i hello självstudier, så att du inte betalar användningsavgifter. En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.

### <a name="delete-portal"></a>Azure-portalen

1. Ange i hello portal sökrutan **myResourceGroup**. I hello sökresultaten klickar du på **myResourceGroup**.
2. På hello **myResourceGroup** bladet, klickar du på hello **ta bort** ikon.
3. tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroup**, och klicka sedan på **ta bort**.

### <a name="delete-cli"></a>Azure CLI

1. Använd hello Azure CLI 2.0 toodelete hello virtuella nätverk (Resource Manager) med hello följande kommando:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Använd hello Azure CLI 1.0 toodelete hello virtuella nätverk (klassiska) med hello följande kommandon:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Ange följande kommando toodelete hello virtuella nätverk (Resource Manager) hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. toodelete hello virtuella nätverk (klassiskt) med PowerShell, måste du ändra en befintlig konfigurationsfil i nätverket. Lär dig hur för[exportera, uppdatera och importera konfigurationsfiler för nätverket](virtual-networks-using-network-configuration-file.md). Ta bort hello följande VirtualNetworkSite element för hello virtuella nätverk som används i den här självstudiekursen:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Importera en konfigurationsfil för ändrade nätverket kan orsaka ändringar tooexisting virtuella nätverk (klassiskt) i din prenumeration. Se till att du bara tar bort hello tidigare virtuella nätverket och att du inte ändra eller ta bort andra befintliga virtuella nätverk från prenumerationen. 

## <a name="next-steps"></a>Nästa steg

- Väl bekanta dig med viktiga [peering begränsningar för virtuellt nätverk och beteenden](virtual-network-manage-peering.md#requirements-and-constraints) innan du skapar ett virtuellt nätverk peering för produktion använder.
- Lär dig mer om alla [peering inställningar för virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
- Lär dig hur för[skapa ett nav och eker nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) med virtuella nätverk peering.
