---
title: "aaaCreate virtuellt Azure-nätverk (klassiskt) med flera undernät | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk (klassiskt) med flera undernät i Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Skapa ett virtuellt nätverk (klassiskt) med flera undernät

> [!IMPORTANT]
> Azure har två [olika distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) för att skapa och arbeta med resurser: Resource Manager och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar de flesta nya virtuella nätverk via hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) distributionsmodell.

I den här kursen lär du dig hur toocreate ett grundläggande Azure virtuella nätverk (klassiska) som har separata offentliga och privata undernät. Du kan skapa Azure-resurser, t.ex. virtuella datorer och molntjänster i ett undernät. Resurser som skapas på virtuella nätverk (klassiskt) kan kommunicera med varandra och resurser i andra nätverk anslutet tooa virtuella nätverket.

Mer information om alla [virtuellt nätverk](virtual-network-manage-network.md) och [undernät](virtual-network-manage-subnet.md) inställningar.

> [!WARNING]
> Virtuella nätverk (klassiskt) omedelbart tas bort av Azure när en [prenumerationen har inaktiverats](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Virtuella nätverk (klassiskt) tas bort oavsett om det finns resurser i hello virtuellt nätverk. Om du senare återaktivera hello prenumeration, måste du återskapa resurser som fanns i hello virtuellt nätverk.

Du kan skapa ett virtuellt nätverk (klassiska) med hjälp av hello [Azure-portalen](#portal), hello [Azure-kommandoradsgränssnittet (CLI) 1.0](#azure-cli), eller [PowerShell](#powershell).

## <a name="portal"></a>Portalen

1. I en webbläsare går toohello [Azure-portalen](https://portal.azure.com). Logga in med ditt [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klicka på **+ ny** hello-portalen.
3. Ange *för virtuella nätverk* i hello **Sök hello Marketplace** rutan hello överst i hello **ny** bladet som visas.  Klicka på **för virtuella nätverk** när den visas i sökresultaten hello.
4. Välj **klassiska** i hello **Välj en distributionsmodell** rutan i hello **virtuellt nätverk** bladet som visas, klicka sedan på **skapa**. 
5. Ange följande värden på hello hello **skapa virtuella nätverk (klassiska)** bladet och klicka sedan på **skapa**:

    |Inställning|Värde|
    |---|---|
    |Namn|myVnet|
    |Adressutrymme|10.0.0.0/16|
    |Namn på undernät|Offentligt|
    |Adressintervall för undernätet|10.0.0.0/24|
    |Resursgrupp|Lämna **Skapa nytt** markerad och ange sedan **myResourceGroup**.|
    |Prenumerationen och platsen|Välj din prenumeration och plats.

    Om du är ny tooAzure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (kallas även tooas *regioner*).
4. Du kan skapa bara ett undernät när du skapar ett virtuellt nätverk i hello-portalen. I den här kursen skapar du ett andra undernät när du har skapat hello virtuellt nätverk. Du kan senare skapar Internet-tillgängliga resurser i hello **offentliga** undernät. Du kan också skapa resurser som inte är tillgänglig från hello Internet i hello **privata** undernät. toocreate Hej andra undernät, ange **myVnet** i hello **söka resurser** rutan hello överst på hello sidan. Klicka på **myVnet** när den visas i sökresultaten hello.
5. Klicka på **undernät** (i hello **inställningar** avsnitt) på hello **skapa virtuella nätverk (klassiska)** bladet som visas.
6. Klicka på **+ Lägg till** på hello **myVnet - undernät** bladet som visas.
7. Ange **privata** för **namn** på hello **Lägg till undernät** bladet. Ange **10.0.1.0/24** för **adressintervall**.  Klicka på **OK**.
8. På hello **myVnet - undernät** bladet hittar du hello **offentliga** och **privata** undernät som du skapade.
9. **Valfria**: när du är klar med den här kursen du kanske vill toodelete hello resurser som du skapade, så att du inte betalar användningsavgifter:
    - Klicka på **översikt** på hello **myVnet** bladet.
    - Klicka på hello **ta bort** ikon på hello **myVnet** bladet.
    - tooconfirm hello borttagning, klickar du på **Ja** i hello **ta bort virtuellt nätverk** rutan.

## <a name="azure-cli"></a>Azure CLI

1. Du kan antingen [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), eller Använd hello CLI inom hello Azure Cloud-gränssnittet. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. tooget hjälp för CLI-kommandona skriver `azure <command> --help`. 
2. Logga in tooAzure med hello-kommando som följer i en CLI-session. Om du klickar på **prova** hello i rutan nedan öppnar ett gränssnitt för molnet. Du kan logga in tooyour Azure-prenumeration, utan att ange hello följande kommando:

    ```azurecli-interactive
    azure login
    ```

3. tooensure hello CLI är i Service Management-läge och ange hello följande kommando:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Skapa ett virtuellt nätverk med ett privat undernät:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Skapa en offentlig undernät inom hello virtuellt nätverk:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Granska hello virtuella nätverk och undernät:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Valfria**: du kanske vill toodelete hello resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> Även om du inte kan ange en resurs grupp toocreate ett virtuellt nätverk (klassiskt) i med hjälp av hello CLI, Azure skapar hello virtuellt nätverk i en resursgrupp med namnet *standard-nätverk*.

## <a name="powershell"></a>PowerShell

1. Installera hello senaste versionen av hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starta en PowerShell-session.
3. Logga in tooAzure i PowerShell genom att ange hello `Add-AzureAccount` kommando.
4. Ändra hello följande sökväg och filnamn efter behov, sedan exportera konfigurationsfilen befintliga nätverk:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. toocreate ett virtuellt nätverk med offentliga och privata undernät, använder alla text editor tooadd hello **VirtualNetworkSite** elementet som följer toohello nätverk konfigurationsfilen.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Granska hello fullständig [nätverk schemat för konfigurationsfilen](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Importera konfigurationsfilen för hello nätverk:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Importera en konfigurationsfil för ändrade nätverket kan orsaka ändringar tooexisting virtuella nätverk (klassiskt) i din prenumeration. Se till att du bara lägga till hello tidigare virtuella nätverket och att du inte ändra eller ta bort alla befintliga virtuella nätverk från prenumerationen. 

7. Granska hello virtuella nätverk och undernät:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Valfria**: du kanske vill toodelete hello resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter. toodelete hello virtuellt nätverk, fullständig steg 4 – 6 igen, denna tid att ta bort hello **VirtualNetworkSite** element som du lade till i steg 5.
 
> [!NOTE]
> Även om du inte kan ange en resurs grupp toocreate ett virtuellt nätverk (klassiskt) i med hjälp av PowerShell, Azure skapar hello virtuellt nätverk i en resursgrupp med namnet *standard-nätverk*.

---

## <a name="next-steps"></a>Nästa steg

- toolearn om alla virtuella nätverk och undernätsinställningar Se [hantera virtuella nätverk](virtual-network-manage-network.md) och [hantera virtuella undernät](virtual-network-manage-subnet.md). Har du olika alternativ för att använda virtuella nätverk och undernät i en produktionsmiljö miljö toomeet olika krav.
- toofilter inkommande och utgående trafik på undernät, skapa och använda [nätverkssäkerhetsgrupper](virtual-networks-nsg.md) toosubnets.
- Skapa en [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller en [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorn och ansluter sedan tooan befintligt virtuellt nätverk.
- tooconnect två virtuella nätverk i hello samma Azure-plats måste du skapa en [virtuellt nätverk peering](create-peering-different-deployment-models.md) mellan hello virtuella nätverk. Du kan peer-virtuella nätverk (Resource Manager) tooa virtuella nätverk (klassiska), men du kan inte skapa en peering mellan två virtuella nätverk (klassiska).
- Ansluta hello virtuellt nätverk tooan lokalt nätverk med hjälp av en [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) krets.
