---
title: "ett virtuellt Azure-nätverk peering - aaaCreate annan distribution modeller - olika prenumerationer | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk peering mellan virtuella nätverk skapas via olika Azure distributionsmodeller som finns i olika Azure-prenumerationer."
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
ms.openlocfilehash: 865bdabb5b87523ba943d7b5dcbdc2475b78bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Skapa ett virtuellt nätverk peering - olika distributionsmodeller och prenumerationer

I kursen får du lära dig toocreate ett virtuellt nätverk peering mellan virtuella nätverk som skapats via olika distributionsmodeller. hello virtuella nätverk finns i olika prenumerationer. Peering två virtuella nätverk gör resurser i olika virtuella nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om hello resurserna var i hello samma virtuella nätverk. Lär dig mer om [virtuella nätverk peering](virtual-network-peering-overview.md). 

hello steg toocreate ett virtuellt nätverk som peering är olika, beroende på om hello virtuella nätverk är i hello samma eller olika prenumerationer och som [Azure distributionsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuella nätverk skapas via. Lär dig hur toocreate ett virtuellt nätverk peering i andra scenarier genom att klicka på hello scenariot från hello följande tabell:

|Azure-distributionsmodell  | Azure-prenumeration  |
|--------- |---------|
|[Båda Resource Manager](virtual-network-create-peering.md) |samma|
|[Båda Resource Manager](create-peering-different-subscriptions.md) |Olika|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models.md) |samma|

Att går inte skapa ett virtuellt nätverk som peering mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen. Ett virtuellt nätverk som peering kan bara skapas mellan två virtuella nätverk som finns i hello samma Azure-region. När du skapar ett virtuellt nätverk peering mellan virtuella nätverk som finns i olika prenumerationer, hello prenumerationer måste båda vara associerad toohello samma Azure Active Directory-klient. Om du inte redan har en Azure Active Directory-klient, kan du snabbt [skapar du en](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Om du behöver tooconnect virtuella nätverk som skapats både via hello klassiska distributionsmodellen, som finns i olika Azure-regioner eller som finns i prenumerationer som är associerade med toodifferent Azure Active Directory-klienter kan du använda en Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuella nätverk. 

> [!WARNING]
> Skapa ett virtuellt nätverk peering mellan virtuella nätverk i skapats via olika Azure distributionsmodeller som finns i olika prenumerationer är för närvarande under förhandsgranskning. Peerkopplingar mellan virtuella nätverk skapas i det här scenariot kan inte ha hello samma nivå av tillgänglighet och tillförlitlighet som du skapar ett virtuellt nätverk peering i scenarier i allmänhet tillgänglighet versionen. Peerkopplingar mellan virtuella nätverk skapas i det här scenariot stöds inte, kan ha begränsad kapacitet och kanske inte är tillgänglig i alla Azure-regioner. Hello senaste meddelanden på tillgänglighet och status för den här funktionen finns hello [Azure Virtual Network uppdaterar](https://azure.microsoft.com/updates/?product=virtual-network) sidan.

Du kan använda hello [Azure-portalen](#portal), hello Azure [kommandoradsgränssnittet](#cli) (CLI) eller Azure [PowerShell](#powershell) toocreate peering ett virtuellt nätverk. Klicka på någon av hello tidigare verktyget länkar toogo direkt toohello steg för att skapa ett virtuellt nätverk peering verktyget dina val.

## <a name="register"></a>Registrera dig för hello preview

tooregister för hello preview fullständig hello steg som följer för båda prenumerationer som innehåller hello virtuella nätverk som du vill toopeer. hello är endast verktyg som du kan använda tooregister för hello förhandsgranskning PowerShell.

1. Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starta en PowerShell-session och logga in med hjälp av hello tooAzure `login-azurermaccount` kommando.
3. Registrera prenumerationen för hello preview genom att ange hello följande kommandon:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
    Utför inte hello åtgärder i hello-portalen, Azure CLI eller PowerShell avsnitt i den här artikeln tills hello **RegistrationState** utdata efter att skriva följande kommando hello är **registrerade** för båda prenumerationer:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

## <a name="portal"></a>Skapa peering - Azure-portalen

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla åtgärder, hoppa över hello steg för att logga utanför hello portal och hoppa över hello steg för att tilldela en annan användare behörighet toohello virtuella nätverk. Innan du utför någon av följande hello, måste du registrera för hello förhandsgranskning. tooregister, fullständig hello stegen i hello [registrera sig för hello förhandsversion](#register) i den här artikeln. Fortsätt inte med hello återstående steg tills båda prenumerationer har registrerats för hello preview.
 
1. Logga in toohello [Azure-portalen](https://portal.azure.com) som UserA. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
2. Klicka på **+ ny**, klickar du på **nätverk**, klicka på **för virtuella nätverk**.
3. I hello **skapa virtuellt nätverk** bladet anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:
    - **Namnet**: *myVnetA*
    - **Adressutrymmet**: *10.0.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.0.0.0/24*
    - **Prenumerationen**: Välj prenumeration A.
    - **Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroupA*
    - **Plats**: *östra USA*
4. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetA*. Klicka på **myVnetA** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnetA** virtuellt nätverk.
5. I hello **myVnetA** bladet som visas, klickar du på **åtkomstkontroll (IAM)** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
6. I hello **myVnetA - åtkomstkontroll (IAM)** bladet som visas, klickar du på **+ Lägg till**.
7. I hello **lägga till behörigheter** bladet som visas, väljer **Network-deltagare** i hello **rollen** rutan.
8. I hello **Välj** markerar b, eller ange användare BS e-postadress toosearch för den. hello listan över användare som visas är från hello samma Azure Active Directory-klient som hello virtuellt nätverk du ställer in hello-peering för. Klicka på b när den visas i hello-listan.
9. Klicka på **Spara**.
10. Logga ut från hello portal som UserA och sedan logga in som användare b.
11. Klicka på **+ ny**, typ *för virtuella nätverk* i hello **Sök hello Marketplace** rutan och klicka sedan på **för virtuella nätverk** i hello sökresultat .
12. I hello **virtuellt nätverk** bladet som visas, väljer **klassiska** i hello **Välj en distributionsmodell** rutan och klicka sedan på **skapa**.
13.   I hello skapa virtuella nätverk (klassiska) som visas ange hello följande värden:

    - **Namnet**: *myVnetB*
    - **Adressutrymmet**: *10.1.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.1.0.0/24*
    - **Prenumerationen**: Välj prenumeration B.
    - **Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroupB*
    - **Plats**: *östra USA*

14. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetB*. Klicka på **myVnetB** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnetB** virtuellt nätverk.
15. I hello **myVnetB** bladet som visas, klickar du på **egenskaper** hello lodräta listan med alternativ på hello vänster sida av hello-bladet. Kopiera hello **resurs-ID**, som används i ett senare steg. hello resurs-ID är liknande toohello följande exempel: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Slutför steg 5 – 9 för myVnetB, ange **UserA** i steg 8.
17. Logga ut från hello-portalen som användare b och logga in som UserA.
18. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetA*. Klicka på **myVnetA** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnet** virtuellt nätverk.
19. Klicka på **myVnetA**.
20. I hello **myVnetA** bladet som visas, klickar du på **Peerkopplingar** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
21. I hello **myVnetA - Peerkopplingar** bladet som visas, klickar du på **+ Lägg till**
22. I hello **Lägg till peering** bladet som visas anger, eller välj hello följande alternativ och klicka på **OK**:
     - **Namnet**: *myVnetAToMyVnetB*
     - **Virtuellt nätverk distributionsmodell**: Välj **klassiska**.
     - **Jag vet mitt resurs-ID**: den här kryssrutan.
     - **Resurs-ID**: Ange hello resurs-ID för myVnetB från steg 15.
     - **Tillåt åtkomst till virtuella nätverk:** se till att **aktiverad** är markerad.
    Inga andra inställningar används i den här kursen. Läs toolearn om inställningar för alla peering [hantera peerkopplingar mellan virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
23. När du klickar på **OK** i föregående steg hello hello **Lägg till peering** blad stängs och du ser hello **myVnetA - Peerkopplingar** bladet igen. Efter några sekunder visas hello peering du skapade i hello-bladet. **Ansluten** visas i hello **PEERING STATUS** för hello **myVnetAToMyVnetB** peering du skapat. Hej peering upprättas. Det finns inga behov toopeer hello virtuella nätverk (klassiska) toohello virtuella nätverk (Resource Manager).

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
25. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete-portal) i den här artikeln.

## <a name="cli"></a>Skapa peering - Azure CLI

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla steg hoppa över hello steg för att logga ut ur Azure och ta bort hello rader i skriptet som skapar användaren rolltilldelningar. Ersätt UserA@azure.com och UserB@azure.com i alla hello följande skript med hello användarnamn som du använder för användare a och b. 

Innan du utför någon av följande hello, måste du registrera för hello förhandsgranskning. tooregister, fullständig hello stegen i hello [registrera sig för hello förhandsversion](#register) i den här artikeln. Fortsätt inte med hello återstående steg tills båda prenumerationer har registrerats för hello preview.

1. [Installera](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuella nätverk (klassiska).
2. Öppna en CLI-session och logga in tooAzure som användare b använder hello `azure login` kommando.
3. Kör hello CLI i Service Management-läge genom att ange hello `azure config mode asm` kommando.
4. Ange följande kommando toocreate hello virtuella nätverk (klassiska) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. hello återstående steg måste utföras med hjälp av ett bash-gränssnitt med hello Azure CLI 2.0.4 eller senare [installerat](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), eller genom att använda hello Azure Cloud-gränssnittet. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. Klicka på hello **prova** knapp i hello skript fram, vilket öppnar ett moln-gränssnitt som loggar du in tooyour Azure-konto. Alternativen på körs bash CLI-skript på en Windows-klient, finns [kör Windows hello Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Kopiera hello följande skript tooa textredigerare på datorn. Ersätt `<SubscriptionB-Id>` med ditt prenumerations-ID. Om du inte vet ditt prenumerations-Id, ange hello `az account show` kommando. Hej värde för **id** i hello utdata är ditt prenumerations-Id. Kopiera hello ändrade skript, klistra in den i tooyour CLI 2.0 session och tryck sedan på `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    När du skapade hello virtuella nätverk (klassiskt) i steg 4 Azure skapade hello virtuellt nätverk i hello *standard-nätverk* resursgruppen.
7. Logga b utanför Azure och logga in som UserA i hello CLI 2.0.
8. Skapa en resursgrupp och ett virtuellt nätverk (Resource Manager). Kopiera hello följande skript, klistra in den i tooyour CLI-sessionen och tryck sedan på `Enter`. 

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get hello id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions toomyVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

9. Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk skapas med hello olika distributionsmodeller. Kopiera hello följande skript tooa textredigerare på datorn. Ersätt `<SubscriptionB-id>` med ditt prenumerations-Id. Om du inte vet ditt prenumerations-Id, ange hello `az account show` kommando. Hej värde för **id** i hello utdata är ditt prenumerations-Id. Azure skapat hello virtuella nätverk (klassiska) du skapade i steg 4 i en resursgrupp med namnet *standard-nätverk*. Klistra in hello ändrade skript i CLI-sessionen och tryck sedan på `Enter`.

    ```azurecli-interactive
    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. När hello skriptet körs, kan du granska hello-peering för hello virtuella nätverk (Resource Manager). Kopiera hello följande skript och klistra in den i sessionen CLI:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    Visar utdata i hello **ansluten** i hello **PeeringState** kolumn.

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
12. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.

## <a name="powershell"></a>Skapa peering - PowerShell

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla steg hoppa över hello steg för att logga ut ur Azure och ta bort hello rader i skriptet som skapar användaren rolltilldelningar. Ersätt UserA@azure.com och UserB@azure.com i alla hello följande skript med hello användarnamn som du använder för användare a och b. 

Innan du utför någon av följande hello, måste du registrera för hello förhandsgranskning. tooregister, fullständig hello stegen i hello [registrera sig för hello förhandsversion](#register) i den här artikeln. Fortsätt inte med hello återstående steg tills båda prenumerationer har registrerats för hello preview.

1. Installera hello senaste versionen av hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) och [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) moduler. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starta en PowerShell-session.
3. Logga in Toouserb's prenumeration som användare b i PowerShell genom att ange hello `Add-AzureAccount` kommando.
4. toocreate ett virtuellt nätverk (klassiskt) med PowerShell, du måste skapa en ny eller ändra en befintlig konfigurationsfil för nätverket. Lär dig hur för[exportera, uppdatera och importera konfigurationsfiler för nätverket](virtual-networks-using-network-configuration-file.md). hello filen bör inkludera hello följande **VirtualNetworkSite** element för hello virtuella nätverk som används i den här självstudiekursen:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Logga in Toouserb's prenumeration som b toouse Resource Manager kommandon genom att ange hello `login-azurermaccount` kommando.
6. Tilldela UserA behörigheter toovirtual nätverk B. kopiera hello följande skript tooa textredigerare på datorn och Ersätt `<SubscriptionB-id>` med hello-ID för prenumerationen B. Om du inte vet hello prenumerations-Id, ange hello `Get-AzureRmSubscription` kommandot tooview den. Hej värde för **Id** hello returnerade utdata är ditt prenumerations-ID. Azure skapat hello virtuella nätverk (klassiska) du skapade i steg 4 i en resursgrupp med namnet *standard-nätverk*. tooexecute hello skriptet kopiera hello ändrade skript, klistra in den i tooPowerShell och tryck sedan på `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Logga ut från Azure som b och logga in Toousera's prenumeration som UserA genom att ange hello `login-azurermaccount` kommando. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
8. Skapa hello virtuella nätverk (Resource Manager) genom att kopiera hello följande skript, klistra in den i tooPowerShell och sedan trycka på `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Tilldela användare b behörigheter toomyVnetA. Kopiera hello följande skript tooa textredigerare på datorn och Ersätt `<SubscriptionA-Id>` med hello-ID för prenumerationen A. Om du inte vet hello prenumerations-Id, ange hello `Get-AzureRmSubscription` kommandot tooview den. Hej värde för **Id** hello returnerade utdata är ditt prenumerations-ID. Klistra in hello modifierad version av hello skriptet i PowerShell och tryck sedan på `Enter` tooexecute den.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Kopiera hello följande skript tooa textredigerare på datorn och Ersätt `<SubscriptionB-id>` med hello-ID för prenumerationen B. toopeer myVnetA toomyVNetB kopiera hello ändrade skript, klistra in den i tooPowerShell och tryck sedan på `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Visa hello peering status för myVnetA genom att kopiera hello följande skript, klistrar in det i PowerShell och trycker på `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    hello tillstånd är **ansluten**. Ändras för**ansluten** när du konfigurera hello peering toomyVnetA från myVnetB.

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
13. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.

## <a name="permissions"></a>Behörigheter

hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter. Till exempel om du peering två virtuella nätverk som heter myVnetA och myVnetB, måste ditt konto tilldelas hello följande minsta eller behörigheter för varje virtuellt nätverk:
    
|Virtuellt nätverk|Distributionsmodell|Roll|Behörigheter|
|---|---|---|---|
|myVnetA|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Saknas|
|myVnetB|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).

## <a name="delete"></a>Ta bort resurser
När du är klar med den här kursen kan kanske du vill toodelete hello resurser som du skapade i hello självstudier, så att du inte betalar användningsavgifter. En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.

### <a name="delete-portal"></a>Azure-portalen

1. Ange i hello portal sökrutan **myResourceGroupA**. I hello sökresultaten klickar du på **myResourceGroupA**.
2. På hello **myResourceGroupA** bladet, klickar du på hello **ta bort** ikon.
3. tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroupA**, och klicka sedan på **ta bort**.
4. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetB*. Klicka på **myVnetB** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnetB** virtuellt nätverk.
5. I hello **myVnetB** bladet, klickar du på **ta bort**.
6. tooconfirm hello borttagning, klickar du på **Ja** i hello **ta bort virtuellt nätverk** rutan.

### <a name="delete-cli"></a>Azure CLI

1. Logga in med hjälp av hello tooAzure CLI 2.0 toodelete hello virtuella nätverk (Resource Manager) med hello följande kommando:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Logga in tooAzure med hello Azure CLI 1.0 toodelete hello virtuella nätverk (klassiska) med hello följande kommandon:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Ange hello följande kommando toodelete hello virtuella nätverk (Resource Manager) på hello PowerShell-Kommandotolken:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. toodelete hello virtuella nätverk (klassiskt) med PowerShell, måste du ändra en befintlig konfigurationsfil i nätverket. Lär dig hur för[exportera, uppdatera och importera konfigurationsfiler för nätverket](virtual-networks-using-network-configuration-file.md). Ta bort hello följande VirtualNetworkSite element för hello virtuella nätverk som används i den här självstudiekursen:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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
