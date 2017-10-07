---
title: "aaaVM med flera IP-adresser med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuella datorn använder hello Azure CLI 1.0 | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a>Tilldela flera IP-adresser toovirtual datorer med hjälp av Azure CLI 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hello Azure CLI 1.0. Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen. Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Skapa en virtuell dator med flera IP-adresser

Du kan göra detta med hjälp av hello Azure CLI 1.0 (den här artikeln) eller hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario. Ändra variabeln namn och IP-adresstyper som krävs för din implementering.

1. Installera och konfigurera hello Azure CLI 1.0 av följande hello steg i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in på ditt Azure-konto med hello `azure-login` kommando.

2. Skapa en resursgrupp:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Skapa ett virtuellt nätverk:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Skapa ett undernät i hello virtuellt nätverk:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Skapa ett lagringskonto för hello VM. Innan du kör hello följande kommando, Ersätt *mittlagringskonto* med ett unikt namn. hello namn måste vara unikt i Azure:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Skapa hello IP-konfigurationer, ett nätverkskort och tilldela hello IP-konfigurationer toohello NIC. Du kan lägga till, ta bort eller ändra hello konfigurationer efter behov. hello följande konfigurationer som beskrivs i hello scenariot:

    **IPConfig-1**

    Ange hello-kommandon som följer toocreate:

    - En offentlig IP-adressresurs med en statisk offentlig IP-adress
    - Ett nätverkskort, tilldela hello offentlig IP-adress och en statisk privat IP-adress tooit.
    
    Ersätt *mypublicdns* med ett namn som är unikt inom hello Azure-plats.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.

    **IPConfig-2**

     Ange följande kommandon toocreate hello en ny offentlig IP-adressresurs och en ny IP-konfiguration med en statisk offentlig IP-adress och en statisk privat IP-adress:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig-3**

    Ange hello följande kommandon toocreate en IP-konfiguration med en statisk privat IP-adress och ingen offentlig IP-adress:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Även om den här artikeln tilldelar alla IP-konfigurationer tooa nätverkskort, kan du också tilldela flera IP-konfigurationer tooany NIC på en virtuell dator. toolearn hur toocreate en virtuell dator med flera nätverkskort, läsa hello skapa en virtuell dator med flera nätverkskort artikel.

7. Skapa en virtuell Linux-dator 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    toochange hello VM storlek tooStandard DS2 v2, till exempel lägger du till följande egenskapen hello `--vm-size Standard_DS3_v2` toohello `azure vm create` kommandot i steg 6.

8. Ange följande kommando tooview hello NIC hello och hello associerade IP-konfigurationer:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.

## <a name="add"></a>Lägg till IP-adresser tooa VM

Du kan lägga till ytterligare privata och offentliga IP-adresser tooan befintliga nätverkskort genom att slutföra hello steg som följer. hello exempel bygger på hello [scenariot](#Scenario) beskrivs i den här artikeln.

1. Öppna Azure CLI och fullständig hello återstående stegen i det här avsnittet i en enda CLI-session. Om du inte redan har installerat och konfigurerat Azure-CLI, fullständig hello stegen i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in på ditt Azure-konto.

2. Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:

    - **Lägg till en privat IP-adress**
    
        tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration med hjälp av hello kommandot nedan. hello statisk adress måste vara en oanvända adress för hello undernät.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).

    - **Lägg till en offentlig IP-adress**
    
        En offentlig IP-adress har lagts till genom att associera den tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration. Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.

        > [!NOTE]
        > Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.
        >

        **Associera hello resurs tooa nya IP-konfiguration**
    
        När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress. Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny. toocreate en ny ange hello följande kommando:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIP3* offentlig IP adress resursen måste du ange hello följande kommando:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Associera hello resurs tooan befintliga IP-konfiguration**

        En offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad. Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Leta efter en rad liknande toohello som följer för IPConfig 3 i hello returnerade utdata:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Eftersom hello **offentliga IP-Adressen** för *IpConfig-3* är tomt, inga offentliga IP-adressresurs är för närvarande associerad tooit. Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IPConfig-3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Visa hello privata IP-adresser och hello offentliga IP-adress resurser tilldelade toohello NIC genom att ange hello följande kommando:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      hello returnerade utdata är liknande toohello följande:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Lägg till hello privata IP-adresser som du har lagt till toohello NIC toohello VM operativsystem genom att följa instruktionerna hello i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
