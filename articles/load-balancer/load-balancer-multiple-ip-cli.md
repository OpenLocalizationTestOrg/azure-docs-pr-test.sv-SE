---
title: "Belastningsutjämning på flera IP-konfigurationer med hjälp av Azure CLI | Microsoft Docs"
description: "Lär dig hur du tilldelar flera IP-adresser till en virtuell dator med Azure CLI | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: annahar
ms.openlocfilehash: bd15713752ea01ad403d8e3dcfed0c9a7adcc7fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="load-balancing-on-multiple-ip-configurations"></a><span data-ttu-id="c3ac6-103">Belastningsutjämning på flera IP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="c3ac6-103">Load balancing on multiple IP configurations</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c3ac6-104">Portal</span><span class="sxs-lookup"><span data-stu-id="c3ac6-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="c3ac6-105">CLI</span><span class="sxs-lookup"><span data-stu-id="c3ac6-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="c3ac6-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3ac6-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="c3ac6-107">Den här artikeln beskriver hur du använder Azure-belastningsutjämnaren med flera IP-adresser på sekundärt nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="c3ac6-107">This article describes how to use Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="c3ac6-108">I det här scenariot har vi två virtuella datorer som kör Windows med en primär och sekundär NIC.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="c3ac6-109">Var och en av de sekundära har två IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-109">Each of the secondary NICs have two IP configurations.</span></span> <span data-ttu-id="c3ac6-110">Varje virtuell värd för både webbplatser contoso.com och fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-110">Each VM hosts both websites contoso.com and fabrikam.com.</span></span> <span data-ttu-id="c3ac6-111">Varje webbplats är bunden till en IP-konfigurationer på sekundära nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-111">Each website is bound to one of the IP configurations on the secondary NIC.</span></span> <span data-ttu-id="c3ac6-112">Vi kan använda Azure belastningsutjämnare för att visa två klientdelens IP-adresser, en för varje webbplats för att distribuera trafik till respektive IP-konfiguration för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-112">We use Azure Load Balancer to expose two frontend IP addresses, one for each website, to distribute traffic to the respective IP configuration for the website.</span></span> <span data-ttu-id="c3ac6-113">Det här scenariot använder samma portnummer på både frontends som båda backend poolen IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-113">This scenario uses the same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![LB scenariot bild](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a><span data-ttu-id="c3ac6-115">Steg för att belastningsutjämna på flera IP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="c3ac6-115">Steps to load balance on multiple IP configurations</span></span>

<span data-ttu-id="c3ac6-116">Följ stegen nedan för att uppnå det scenario som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-116">Follow the steps below to achieve the scenario outlined in this article:</span></span>

1. <span data-ttu-id="c3ac6-117">[Installera och konfigurera Azure CLI](../cli-install-nodejs.md) Azure CLI genom att följa stegen i den länkade artikeln och loggar till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-117">[Install and Configure the Azure CLI](../cli-install-nodejs.md) the Azure CLI by following the steps in the linked article and log into your Azure account.</span></span>
2. <span data-ttu-id="c3ac6-118">[Skapa en resursgrupp](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) kallas *contosofabrikam* enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-118">[Create a resource group](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) called *contosofabrikam* as described above.</span></span>

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. <span data-ttu-id="c3ac6-119">[Skapa en tillgänglighetsuppsättning](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) till för de två virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-119">[Create an availability set](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) to for the two VMs.</span></span> <span data-ttu-id="c3ac6-120">I det här scenariot använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-120">For this scenario, use the following command:</span></span>

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. <span data-ttu-id="c3ac6-121">[Skapa ett virtuellt nätverk](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) kallas *myVNet* och ett undernät som kallas *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-121">[Create a virtual network](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) called *myVNet* and a subnet called *mySubnet*:</span></span>

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. <span data-ttu-id="c3ac6-122">[Skapa belastningsutjämnaren](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kallas *mylb*:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-122">[Create the load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) called *mylb*:</span></span>

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. <span data-ttu-id="c3ac6-123">Skapa två dynamiska offentliga IP-adresser för klientdelens IP-konfigurationer för din belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-123">Create two dynamic public IP addresses for the frontend IP configurations of your load balancer:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. <span data-ttu-id="c3ac6-124">Skapa två klientdelens IP-konfigurationer, *contosofe* och *fabrikamfe* respektive:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-124">Create the two frontend IP configurations, *contosofe* and *fabrikamfe* respectively:</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. <span data-ttu-id="c3ac6-125">Skapa din serverdel för adresspool - *contosopool* och *fabrikampool*, [avsökningen](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, och din belastningsutjämning regler - *HTTPc* och *HTTPf*:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-125">Create your backend address pools - *contosopool* and *fabrikampool*, a [probe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, and your load balancing rules - *HTTPc* and *HTTPf*:</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. <span data-ttu-id="c3ac6-126">Kör följande kommando nedan och kontrollera sedan utdata till [Kontrollera din belastningsutjämnare](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) skapades på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-126">Run the following command below and then check the output to [verify your load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) was created correctly:</span></span>

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. <span data-ttu-id="c3ac6-127">[Skapa en offentlig IP-adress](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, och [lagringskonto](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* för din första virtuella dator VM1 enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-127">[Create a public IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, and [storage account](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* for your first virtual machine VM1 as shown below:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. <span data-ttu-id="c3ac6-128">[Skapa nätverksgränssnitten](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) för VM1 och lägga till en andra IP-konfiguration, *VM1 ipconfig2*, och [skapa den virtuella datorn](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-128">[Create the network interfaces](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) for VM1 and add a second IP configuration, *VM1-ipconfig2*, and [create the VM](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) as shown below:</span></span>

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. <span data-ttu-id="c3ac6-129">Upprepa steg 10-11 för den virtuella datorn med andra:</span><span class="sxs-lookup"><span data-stu-id="c3ac6-129">Repeat steps 10-11 for your second VM:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. <span data-ttu-id="c3ac6-130">Slutligen måste du konfigurera DNS-resursposter för att peka till respektive klientdelens IP-adressen för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-130">Finally, you must configure DNS resource records to point to the respective frontend IP address of the Load Balancer.</span></span> <span data-ttu-id="c3ac6-131">Du kan vara värd för dina domäner i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c3ac6-131">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="c3ac6-132">Mer information om hur du använder Azure DNS med belastningsutjämnaren finns [med hjälp av Azure DNS med andra Azure-tjänster](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="c3ac6-132">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3ac6-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3ac6-133">Next steps</span></span>
- <span data-ttu-id="c3ac6-134">Mer information om hur du kombinerar belastningsutjämning för tjänster i Azure i [med belastningsutjämning tjänster i Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c3ac6-134">Learn more about how to combine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="c3ac6-135">Lär dig hur du kan använda olika typer av loggar i Azure för att hantera och felsöka belastningsutjämnare i [logga analytics för Azure belastningsutjämnare](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="c3ac6-135">Learn how you can use different types of logs in Azure to manage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
