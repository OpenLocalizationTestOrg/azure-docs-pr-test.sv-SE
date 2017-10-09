---
title: "aaaCreate en Internetriktade belastningsutjämnare - Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare i Resource Manager med hjälp av hello Azure CLI"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a>Skapa en internet-belastningsutjämnare använder hello Azure CLI

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnare använder klassisk distribution](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a>Distribuera hello-lösning med hjälp av hello Azure CLI

hello följande steg visar hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager med CLI. Med Azure Resource Manager varje resurs skapas och konfigureras separat, sedan samlat toocreate en resurs.

Du måste skapa och konfigurera hello följande objekt toodeploy en belastningsutjämnare:

* IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.
* Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.
* Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.
* Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.
* Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.

Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).

## <a name="set-up-cli-toouse-resource-manager"></a>Ställ in CLI toouse Resource Manager

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.

    ```azurecli
        azure config mode arm
    ```

    Förväntad utdata:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool

1. Skapa ett virtuellt nätverk (VNet) med namnet *NRPVnet* i hello östra USA plats med hjälp av en resursgrupp med namnet *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Skapa ett undernät med namnet *NRPVnetSubnet* med CIDR-blocket 10.0.0.0/24 i *NRPVnet*.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Skapa en offentlig IP-adress med namnet *NRPPublicIP* toobe som används av en frontend IP-adresspool med DNS-namnet *loadbalancernrp.eastus.cloudapp.azure.com*. hello kommandot nedan använder hello statisk tilldelningsmetod typ och timeout för inaktivitet 4 minuter.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som dess FQDN. Denna ändring från klassisk distribution som använder hello molnbaserad tjänst som hello belastningsutjämnaren fullständigt kvalificerade domännamn (FQDN).
   > I det här exemplet hello FQDN är *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Skapa en belastningsutjämnare

hello följande kommando skapar en belastningsutjämnare med namnet *NRPlb* i hello *NRPRG* resursgrupp i hello *östra USA* Azure-plats.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Skapa en IP-adresspool på klientsidan och en backend-adresspool
Det här exemplet visar hur toocreate hello frontend IP-pool som tar emot hello inkommande nätverkstrafik på hello belastningsutjämnare och hello IP serverdelspool där hello frontend poolen skickar hello belastningsutjämnad trafik.

1. Skapa en frontend IP-adresspool som kopplar hello offentliga IP-Adressen skapas i hello föregående steg och hello belastningsutjämnare.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Ställ in en backend-adresspool används tooreceive inkommande trafik från hello frontend IP-adresspool.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>Skapa LB-regler, NAT-regler och avsökning

Det här exemplet skapar hello följande objekt.

* en NAT-regel tootranslate all inkommande trafik på port 21 tooport 22<sup>1</sup>
* en NAT-regel tootranslate all inkommande trafik på port 23 tooport 22
* en belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello adresser i hello backend-adresspool.
* en avsökning regeln toocheck hello hälsostatus på en sida med namnet *HealthProbe.aspx*.

<sup>1</sup> NAT-regler är associerade tooa specifik virtuell datorinstans bakom hello belastningsutjämnare. hello nätverkstrafik som inkommer på port 21 skickas tooa specifik virtuell dator på port 22 som associeras med den här NAT-regel. Du måste ange ett protokoll (UDP eller TCP) för en NAT-regel. Båda protokollen kan inte vara tilldelad toohello samma port.

1. Skapa hello NAT-regler.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Skapa en belastningsutjämningsregel.

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Skapa en hälsoavsökning.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Kontrollera inställningarna.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Förväntad utdata:

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Skapa nätverkskort

Du behöver toocreate nätverkskort (eller ändra befintliga) och kopplar dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar.

1. Skapa ett nätverkskort med namnet *lb nic1 vara*, och koppla den till hello *rdp1* NAT-regel och hello *NRPbackendpool* backend-adresspool.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
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

2. Skapa ett nätverkskort med namnet *lb nic2 vara*, och koppla den till hello *rdp2* NAT-regel och hello *NRPbackendpool* backend-adresspool.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Skapa en virtuell dator (VM) med namnet *web1*, och koppla den till hello NIC med namnet *lb nic1 vara*. Ett lagringskonto kallas *web1nrp* har skapats innan du kör hello kommandot nedan.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > Virtuella datorer i load belastningsutjämnaren måste toobe i hello samma tillgänglighetsuppsättning. Använd `azure availset create` toocreate en tillgänglighetsuppsättning.

    hello utdata ska vara liknande toohello följande:

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > hello informationsmeddelande **detta är ett nätverkskort utan publicIP konfigurerad** förväntas eftersom hello skapa nätverkskort för hello belastningsutjämnaren ansluter tooInternet med hello offentliga IP-adressen för belastningsutjämnaren.

    Eftersom hello *lb nic1 vara* NIC som är associerad med hello *rdp1* NAT-regel, du kan ansluta för*web1* med RDP via port 3441 på hello belastningsutjämnare.

4. Skapa en virtuell dator (VM) med namnet *web2*, och koppla den till hello NIC med namnet *lb nic2 vara*. Ett lagringskonto kallas *web1nrp* har skapats innan du kör hello kommandot nedan.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Uppdatera en befintlig belastningsutjämnare
Du kan lägga till regler som refererar till en befintlig belastningsutjämnare. I nästa exempel hello läggs en ny regel för belastningsutjämnare tooan befintliga belastningsutjämnaren **NRPlb**

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Ta bort en belastningsutjämnare
Använd följande kommando tooremove belastningsutjämning hello:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Nästa steg
[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
