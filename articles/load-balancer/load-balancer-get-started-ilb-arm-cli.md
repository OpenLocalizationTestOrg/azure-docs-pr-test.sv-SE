---
title: "aaaCreate en intern belastningsutjämnare - Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med hjälp av hello Azure CLI i Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a>Skapa en intern belastningsutjämnare med hello Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a>Distribuera hello lösning med hjälp av hello Azure CLI

hello följande steg visar hur toocreate en Internetriktade belastningsutjämnare med hjälp av Azure Resource Manager med CLI. Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt och sedan sätta ihop toocreate en resurs.

Du behöver toocreate och konfigurera hello följande objekt toodeploy en belastningsutjämnare:

* **IP-konfiguration på klientsidan**: innehåller offentliga IP-adresser för inkommande nätverkstrafik
* **Backend-adresspool**: innehåller nätverksgränssnitt (NIC) som gör att virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren
* **Regler för belastningsutjämning**: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool
* **Inkommande NAT-regler**: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool
* **Avsökningar**: innehåller hälsoavsökningar som används toocheck hello tillgängligheten för virtuella datorer instanser i hello backend-adresspool

Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).

## <a name="set-up-cli-toouse-resource-manager"></a>Ställ in CLI toouse Resource Manager

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md). Följ instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge, enligt följande:

    ```azurecli
    azure config mode arm
    ```

    Förväntad utdata:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Stegvisa anvisningar för hur du skapar en intern belastningsutjämnare

1. Logga in tooAzure.

    ```azurecli
    azure login
    ```

    Ange inte dina autentiseringsuppgifter för Azure när du uppmanas göra det.

2. Ändra hello kommandot verktyg tooAzure Resource Manager-läget.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Alla resurser i Azure Resource Manager är kopplade till en resursgrupp. Om du inte redan gjort det skapar du en resursgrupp nu.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Skapa en intern belastningsutjämningsuppsättning

1. Skapa en intern belastningsutjämnare

    I följande scenario hello, skapas en resursgrupp med namnet nrprg i östra USA.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Alla resurser för en intern belastningsutjämnare, till exempel virtuella nätverk och undernät för virtuellt nätverk, måste vara i samma resursgrupp hello och hello i samma region.

2. Skapa en frontend IP-adress för hello intern belastningsutjämnare.

    hello IP-adress som du använder måste vara inom intervallet för hello undernät för det virtuella nätverket.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Skapa hello backend-adresspool.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    När du har definierat en IP-adress för klientdelen och en serverdelsadresspool kan du skapa belastningsutjämningsregler, ingående NAT-regler och anpassade hälsoavsökningar.

4. Skapa en regel för belastningsutjämnare för hello intern belastningsutjämnare.

    När du följer stegen för hello hello kommando skapar en regel för belastningsutjämning för lyssnande tooport 1433 i hello frontend poolen och skicka belastningsutjämnad trafik toohello backend-adresspool för nätverk, också använder port 1433.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Skapa ingående NAT-regler.

    Inkommande NAT-regler finns används toocreate slutpunkter i en belastningsutjämnare som går tooa specifik virtuell dator-instansen. hello föregående steg skapa två NAT-regler för fjärrskrivbord.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Skapa hälsoavsökningar för hello belastningsutjämnaren.

    En hälsoavsökningen kontrollerar alla virtuella instanser toomake att de kan skicka nätverkstrafik. hello virtuella instans med misslyckade avsökning kontrollerar tas bort från belastningsutjämnaren hello tills den går tillbaka online och en avsökning kontroll anger att den är felfri.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > hello Microsoft Azure-plattformen använder en statisk, offentligt dirigerbara IPv4-adress för en mängd olika administrativa scenarier. hello IP-adressen är 168.63.129.16. Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.
    > Avseende tooAzure intern belastningsutjämning används den här IP-adress genom att övervaka avsökningar från hello belastningen belastningsutjämnaren toodetermine hello hälsotillståndet för virtuella datorer i en belastningsutjämnad uppsättning. Om en nätverkssäkerhetsgrupp används toorestrict trafik tooAzure virtuella datorer i en internt belastningsutjämnad uppsättning eller är tillämpade tooa undernät för virtuellt nätverk, se till att en nätverkssäkerhetsregeln läggs tooallow trafik från 168.63.129.16.

## <a name="create-nics"></a>Skapa nätverkskort

Du behöver toocreate nätverkskort (eller ändra befintliga) och kopplar dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar.

1. Skapa ett nätverkskort med namnet *lb nic1 vara*, och koppla den till hello *rdp1* NAT regeln och hello *beilb* backend-adresspool.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    Förväntad utdata:

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Skapa ett nätverkskort med namnet *lb nic2 vara*, och koppla den till hello *rdp2* NAT regeln och hello *beilb* backend-adresspool.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Skapa en virtuell dator med namnet *DB1*, och koppla den till hello NIC med namnet *lb nic1 vara*. Ett lagringskonto kallas *web1nrp* har skapats innan hello efter kommandot körs:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Virtuella datorer i load belastningsutjämnaren måste toobe i hello samma tillgänglighetsuppsättning. Använd `azure availset create` toocreate en tillgänglighetsuppsättning.

4. Skapa en virtuell dator (VM) med namnet *DB2*, och koppla den till hello NIC med namnet *lb nic2 vara*. Ett lagringskonto kallas *web1nrp* har skapats innan du kör följande kommando hello.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Ta bort en belastningsutjämnare

tooremove en belastningsutjämnare använder hello följande kommando:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för en belastningsutjämnare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

