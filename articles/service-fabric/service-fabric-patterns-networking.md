---
title: "aaaNetworking mönster för Azure Service Fabric | Microsoft Docs"
description: "Beskriver vanliga nätverk mönster för Service Fabric och hur toocreate ett kluster med hjälp av Azure nätverksfunktioner."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 5973e3f9917076c6a36e71443ec256e0f414ff87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-networking-patterns"></a><span data-ttu-id="865ef-103">Service Fabric nätverk mönster</span><span class="sxs-lookup"><span data-stu-id="865ef-103">Service Fabric networking patterns</span></span>
<span data-ttu-id="865ef-104">Du kan integrera Azure Service Fabric-kluster med andra funktioner för Azure.</span><span class="sxs-lookup"><span data-stu-id="865ef-104">You can integrate your Azure Service Fabric cluster with other Azure networking features.</span></span> <span data-ttu-id="865ef-105">I den här artikeln visar vi du hur toocreate kluster att använda hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="865ef-105">In this article, we show you how toocreate clusters that use hello following features:</span></span>

- [<span data-ttu-id="865ef-106">Befintligt virtuellt nätverk eller undernät</span><span class="sxs-lookup"><span data-stu-id="865ef-106">Existing virtual network or subnet</span></span>](#existingvnet)
- [<span data-ttu-id="865ef-107">Statiska offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="865ef-107">Static public IP address</span></span>](#staticpublicip)
- [<span data-ttu-id="865ef-108">Endast interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="865ef-108">Internal-only load balancer</span></span>](#internallb)
- [<span data-ttu-id="865ef-109">Interna och externa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="865ef-109">Internal and external load balancer</span></span>](#internalexternallb)

<span data-ttu-id="865ef-110">Service Fabric körs i en standard virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="865ef-110">Service Fabric runs in a standard virtual machine scale set.</span></span> <span data-ttu-id="865ef-111">Funktioner som du kan använda i en skaluppsättning för virtuell dator, som du kan använda med ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="865ef-111">Any functionality that you can use in a virtual machine scale set, you can use with a Service Fabric cluster.</span></span> <span data-ttu-id="865ef-112">hello nätverk avsnitt hello Azure Resource Manager-mallar för virtuella datorer och Service Fabric är identiska.</span><span class="sxs-lookup"><span data-stu-id="865ef-112">hello networking sections of hello Azure Resource Manager templates for virtual machine scale sets and Service Fabric are identical.</span></span> <span data-ttu-id="865ef-113">När du distribuerar tooan befintliga virtuella nätverk, är det enkelt tooincorporate andra nätverksfunktioner som Azure ExpressRoute, Azure VPN-Gateway, en säkerhetsgrupp för nätverk och virtuella nätverk peering.</span><span class="sxs-lookup"><span data-stu-id="865ef-113">After you deploy tooan existing virtual network, it's easy tooincorporate other networking features, like Azure ExpressRoute, Azure VPN Gateway, a network security group, and virtual network peering.</span></span>

<span data-ttu-id="865ef-114">Service Fabric är unikt från andra nätverksfunktioner i en aspekt.</span><span class="sxs-lookup"><span data-stu-id="865ef-114">Service Fabric is unique from other networking features in one aspect.</span></span> <span data-ttu-id="865ef-115">Hej [Azure-portalen](https://portal.azure.com) internt använder hello Service Fabric-providern toocall tooa klustret tooget resursinformation om noderna och program.</span><span class="sxs-lookup"><span data-stu-id="865ef-115">hello [Azure portal](https://portal.azure.com) internally uses hello Service Fabric resource provider toocall tooa cluster tooget information about nodes and applications.</span></span> <span data-ttu-id="865ef-116">hello Service Fabric-resursprovidern kräver offentligt tillgänglig inkommande åtkomst toohello HTTP gateway-porten (port 19080, som standard) på hello management slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="865ef-116">hello Service Fabric resource provider requires publicly accessible inbound access toohello HTTP gateway port (port 19080, by default) on hello management endpoint.</span></span> <span data-ttu-id="865ef-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) använder hello management endpoint toomanage klustret.</span><span class="sxs-lookup"><span data-stu-id="865ef-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uses hello management endpoint toomanage your cluster.</span></span> <span data-ttu-id="865ef-118">hello Service Fabric-resursprovidern använder också den här porten tooquery information om klustret toodisplay i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="865ef-118">hello Service Fabric resource provider also uses this port tooquery information about your cluster, toodisplay in hello Azure portal.</span></span> 

<span data-ttu-id="865ef-119">Om port 19080 inte är tillgänglig från hello Service Fabric-resursprovidern, ett meddelande som *noder hittades inte* visas i hello-portalen och listan noden och programmet visas tomt.</span><span class="sxs-lookup"><span data-stu-id="865ef-119">If port 19080 is not accessible from hello Service Fabric resource provider, a message like *Nodes Not Found* appears in hello portal, and your node and application list appears empty.</span></span> <span data-ttu-id="865ef-120">Om du vill toosee klustret i hello Azure-portalen, din belastningsutjämnare måste exponera en offentlig IP-adress och din nätverkssäkerhetsgruppen måste tillåta inkommande Porttrafik 19080.</span><span class="sxs-lookup"><span data-stu-id="865ef-120">If you want toosee your cluster in hello Azure portal, your load balancer must expose a public IP address, and your network security group must allow incoming port 19080 traffic.</span></span> <span data-ttu-id="865ef-121">Om din konfiguration inte uppfyller dessa krav visas inte hello Azure-portalen hello status för klustret.</span><span class="sxs-lookup"><span data-stu-id="865ef-121">If your setup does not meet these requirements, hello Azure portal does not display hello status of your cluster.</span></span>

## <a name="templates"></a><span data-ttu-id="865ef-122">Mallar</span><span class="sxs-lookup"><span data-stu-id="865ef-122">Templates</span></span>

<span data-ttu-id="865ef-123">Alla Service Fabric-mallar finns i [en nedladdning filen](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span><span class="sxs-lookup"><span data-stu-id="865ef-123">All Service Fabric templates are in [one download file](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span></span> <span data-ttu-id="865ef-124">Du bör vara kan toodeploy hello mallar som-är med hjälp av hello följande PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="865ef-124">You should be able toodeploy hello templates as-is by using hello following PowerShell commands.</span></span> <span data-ttu-id="865ef-125">Om du distribuerar hello befintliga Azure-nätverk eller hello statiska offentliga IP-mallen, läsa hello [inledande installationen](#initialsetup) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="865ef-125">If you are deploying hello existing Azure Virtual Network template or hello static public IP template, first read hello [Initial setup](#initialsetup) section of this article.</span></span>

<a id="initialsetup"></a>
## <a name="initial-setup"></a><span data-ttu-id="865ef-126">Installationen</span><span class="sxs-lookup"><span data-stu-id="865ef-126">Initial setup</span></span>

### <a name="existing-virtual-network"></a><span data-ttu-id="865ef-127">Befintligt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="865ef-127">Existing virtual network</span></span>

<span data-ttu-id="865ef-128">I följande exempel hello, vi börjar med ett befintligt virtuellt nätverk med namnet ExistingRG-vnet i hello **ExistingRG** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="865ef-128">In hello following example, we start with an existing virtual network named ExistingRG-vnet, in hello **ExistingRG** resource group.</span></span> <span data-ttu-id="865ef-129">hello undernät kallas standard.</span><span class="sxs-lookup"><span data-stu-id="865ef-129">hello subnet is named default.</span></span> <span data-ttu-id="865ef-130">Dessa standardresurser skapas när du använder hello Azure portal toocreate en vanliga virtuell dator (VM).</span><span class="sxs-lookup"><span data-stu-id="865ef-130">These default resources are created when you use hello Azure portal toocreate a standard virtual machine (VM).</span></span> <span data-ttu-id="865ef-131">Du kan skapa hello virtuellt nätverk och undernät utan att skapa hello VM, men hello Huvudmålet för att lägga till ett kluster tooan befintligt virtuellt nätverk är tooprovide network connectivity tooother virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="865ef-131">You could create hello virtual network and subnet without creating hello VM, but hello main goal of adding a cluster tooan existing virtual network is tooprovide network connectivity tooother VMs.</span></span> <span data-ttu-id="865ef-132">Skapa hello VM ger ett bra exempel på hur ett befintligt virtuellt nätverk normalt används.</span><span class="sxs-lookup"><span data-stu-id="865ef-132">Creating hello VM gives a good example of how an existing virtual network typically is used.</span></span> <span data-ttu-id="865ef-133">Om din Service Fabric-klustret använder endast en intern belastningsutjämnare, utan en offentlig IP-adress kan du använda hello virtuella datorn och dess offentliga IP-adress som en säker *hoppa rutan*.</span><span class="sxs-lookup"><span data-stu-id="865ef-133">If your Service Fabric cluster uses only an internal load balancer, without a public IP address, you can use hello VM and its public IP as a secure *jump box*.</span></span>

### <a name="static-public-ip-address"></a><span data-ttu-id="865ef-134">Statiska offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="865ef-134">Static public IP address</span></span>

<span data-ttu-id="865ef-135">En statisk offentlig IP-adress är vanligtvis en dedikerad resurs som hanteras separat från hello VM eller virtuella datorer som den är tilldelad till.</span><span class="sxs-lookup"><span data-stu-id="865ef-135">A static public IP address generally is a dedicated resource that's managed separately from hello VM or VMs it's assigned to.</span></span> <span data-ttu-id="865ef-136">Det har etablerats i ett dedikerat nätverk resursgrupp (som skillnad från tooin hello Service Fabric klusterresursgrupp sig själv).</span><span class="sxs-lookup"><span data-stu-id="865ef-136">It's provisioned in a dedicated networking resource group (as opposed tooin hello Service Fabric cluster resource group itself).</span></span> <span data-ttu-id="865ef-137">Skapa en statisk offentlig IP-adress med namnet staticIP1 i hello samma ExistingRG resursgrupp, antingen i hello Azure-portalen eller med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="865ef-137">Create a static public IP address named staticIP1 in hello same ExistingRG resource group, either in hello Azure portal or by using PowerShell:</span></span>

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a><span data-ttu-id="865ef-138">Service Fabric-mall</span><span class="sxs-lookup"><span data-stu-id="865ef-138">Service Fabric template</span></span>

<span data-ttu-id="865ef-139">Vi använder hello Service Fabric template.json i hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="865ef-139">In hello examples in this article, we use hello Service Fabric template.json.</span></span> <span data-ttu-id="865ef-140">Du kan använda hello standard portal guiden toodownload hello mallen från hello-portalen innan du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="865ef-140">You can use hello standard portal wizard toodownload hello template from hello portal before you create a cluster.</span></span> <span data-ttu-id="865ef-141">Du kan också använda en av hello mallar i hello [mallgalleriet](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), som hello [Service Fabric-kluster med fem noder](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span><span class="sxs-lookup"><span data-stu-id="865ef-141">You also can use one of hello templates in hello [template gallery](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), like hello [five-node Service Fabric cluster](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span></span>

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a><span data-ttu-id="865ef-142">Befintligt virtuellt nätverk eller undernät</span><span class="sxs-lookup"><span data-stu-id="865ef-142">Existing virtual network or subnet</span></span>

1. <span data-ttu-id="865ef-143">Ändra hello undernät toohello på parameternamn hello befintligt undernät och Lägg sedan till två nya parametrar tooreference hello befintligt virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="865ef-143">Change hello subnet parameter toohello name of hello existing subnet, and then add two new parameters tooreference hello existing virtual network:</span></span>

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. <span data-ttu-id="865ef-144">Ändra hello `vnetID` variabeln toopoint toohello befintligt virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="865ef-144">Change hello `vnetID` variable toopoint toohello existing virtual network:</span></span>

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. <span data-ttu-id="865ef-145">Ta bort `Microsoft.Network/virtualNetworks` från dina resurser, så Azure skapar inte ett nytt virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="865ef-145">Remove `Microsoft.Network/virtualNetworks` from your resources, so Azure does not create a new virtual network:</span></span>

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. <span data-ttu-id="865ef-146">Kommentera hello virtuella nätverket från hello `dependsOn` attribut för `Microsoft.Compute/virtualMachineScaleSets`, så att du inte beror på hur du skapar ett nytt virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="865ef-146">Comment out hello virtual network from hello `dependsOn` attribute of `Microsoft.Compute/virtualMachineScaleSets`, so you don't depend on creating a new virtual network:</span></span>

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. <span data-ttu-id="865ef-147">Distribuera hello mallen:</span><span class="sxs-lookup"><span data-stu-id="865ef-147">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    <span data-ttu-id="865ef-148">Efter distributionen kan det virtuella nätverket bör innehålla hello ny skaluppsättning för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="865ef-148">After deployment, your virtual network should include hello new scale set VMs.</span></span> <span data-ttu-id="865ef-149">hello virtuella scale set nodtypen ska visa hello befintligt virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="865ef-149">hello virtual machine scale set node type should show hello existing virtual network and subnet.</span></span> <span data-ttu-id="865ef-150">Du kan också använda protokollet RDP (Remote Desktop) tooaccess hello virtuell dator som redan finns i hello virtuella nätverk och tooping hello ny skaluppsättning för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="865ef-150">You also can use Remote Desktop Protocol (RDP) tooaccess hello VM that was already in hello virtual network, and tooping hello new scale set VMs:</span></span>

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

<span data-ttu-id="865ef-151">Ett annat exempel är finns [ett som inte är specifikt tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="865ef-151">For another example, see [one that is not specific tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span></span>


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a><span data-ttu-id="865ef-152">Statiska offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="865ef-152">Static public IP address</span></span>

1. <span data-ttu-id="865ef-153">Lägg till parametrar för hello namnet på hello befintlig statisk IP-resursgrupp, namn och fullständigt kvalificerade domännamnet (FQDN):</span><span class="sxs-lookup"><span data-stu-id="865ef-153">Add parameters for hello name of hello existing static IP resource group, name, and fully qualified domain name (FQDN):</span></span>

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. <span data-ttu-id="865ef-154">Ta bort hello `dnsName` parameter.</span><span class="sxs-lookup"><span data-stu-id="865ef-154">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="865ef-155">(hello statisk IP-adress har redan en.)</span><span class="sxs-lookup"><span data-stu-id="865ef-155">(hello static IP address already has one.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. <span data-ttu-id="865ef-156">Lägg till en variabel tooreference hello befintlig statisk IP-adress:</span><span class="sxs-lookup"><span data-stu-id="865ef-156">Add a variable tooreference hello existing static IP address:</span></span>

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. <span data-ttu-id="865ef-157">Ta bort `Microsoft.Network/publicIPAddresses` från dina resurser, så Azure skapar inte en ny IP-adress:</span><span class="sxs-lookup"><span data-stu-id="865ef-157">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. <span data-ttu-id="865ef-158">Kommentera hello IP-adress från hello `dependsOn` attribut för `Microsoft.Network/loadBalancers`, så att du inte beror på hur du skapar en ny IP-adress:</span><span class="sxs-lookup"><span data-stu-id="865ef-158">Comment out hello IP address from hello `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address:</span></span>

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. <span data-ttu-id="865ef-159">I hello `Microsoft.Network/loadBalancers` resurs, ändra hello `publicIPAddress` element av `frontendIPConfigurations` tooreference hello befintlig statisk IP-adress i stället för en nyligen skapade:</span><span class="sxs-lookup"><span data-stu-id="865ef-159">In hello `Microsoft.Network/loadBalancers` resource, change hello `publicIPAddress` element of `frontendIPConfigurations` tooreference hello existing static IP address instead of a newly created one:</span></span>

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. <span data-ttu-id="865ef-160">I hello `Microsoft.ServiceFabric/clusters` resurs, ändra `managementEndpoint` toohello DNS FQDN för hello statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-160">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toohello DNS FQDN of hello static IP address.</span></span> <span data-ttu-id="865ef-161">Om du använder en säker kluster, kontrollerar du att du ändrar *http://* för*https://*.</span><span class="sxs-lookup"><span data-stu-id="865ef-161">If you are using a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="865ef-162">(Observera att det här steget gäller endast tooService Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="865ef-162">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="865ef-163">Om du använder en skaluppsättning för virtuell dator kan du hoppa över det här steget.)</span><span class="sxs-lookup"><span data-stu-id="865ef-163">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. <span data-ttu-id="865ef-164">Distribuera hello mallen:</span><span class="sxs-lookup"><span data-stu-id="865ef-164">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

<span data-ttu-id="865ef-165">Efter distributionen kan du ser att din belastningsutjämnare är bundna toohello offentlig statisk IP-adress från hello annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="865ef-165">After deployment, you can see that your load balancer is bound toohello public static IP address from hello other resource group.</span></span> <span data-ttu-id="865ef-166">Hej Service Fabric-klienten Anslutningens slutpunkt och [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint punkt toohello DNS FQDN för hello statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-166">hello Service Fabric client connection endpoint and [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint point toohello DNS FQDN of hello static IP address.</span></span>

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a><span data-ttu-id="865ef-167">Endast interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="865ef-167">Internal-only load balancer</span></span>

<span data-ttu-id="865ef-168">Det här scenariot ersätter hello extern belastningsutjämnare i hello standardmallen för Service Fabric med en endast är interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-168">This scenario replaces hello external load balancer in hello default Service Fabric template with an internal-only load balancer.</span></span> <span data-ttu-id="865ef-169">Konsekvenser för hello Azure-portalen och hello Service Fabric-resursprovidern finns hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="865ef-169">For implications for hello Azure portal and for hello Service Fabric resource provider, see hello preceding section.</span></span>

1. <span data-ttu-id="865ef-170">Ta bort hello `dnsName` parameter.</span><span class="sxs-lookup"><span data-stu-id="865ef-170">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="865ef-171">(Det krävs inte.)</span><span class="sxs-lookup"><span data-stu-id="865ef-171">(It's not needed.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. <span data-ttu-id="865ef-172">Alternativt kan du använda en statisk tilldelningsmetod kan du lägga till parametern för en statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-172">Optionally, if you use a static allocation method, you can add a static IP address parameter.</span></span> <span data-ttu-id="865ef-173">Om du använder en dynamisk fördelning, behöver du inte toodo det här steget.</span><span class="sxs-lookup"><span data-stu-id="865ef-173">If you use a dynamic allocation method, you do not need toodo this step.</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. <span data-ttu-id="865ef-174">Ta bort `Microsoft.Network/publicIPAddresses` från dina resurser, så Azure skapar inte en ny IP-adress:</span><span class="sxs-lookup"><span data-stu-id="865ef-174">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. <span data-ttu-id="865ef-175">Ta bort IP-adress för hello `dependsOn` attribut för `Microsoft.Network/loadBalancers`, så att du inte beror på hur du skapar en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-175">Remove hello IP address `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address.</span></span> <span data-ttu-id="865ef-176">Lägga till hello virtuellt nätverk `dependsOn` attributet eftersom hello belastningsutjämnaren nu beror på hello undernät från hello virtuella nätverket:</span><span class="sxs-lookup"><span data-stu-id="865ef-176">Add hello virtual network `dependsOn` attribute because hello load balancer now depends on hello subnet from hello virtual network:</span></span>

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. <span data-ttu-id="865ef-177">Ändra hello belastningsutjämnarens `frontendIPConfigurations` från med hjälp av en `publicIPAddress`, toousing ett undernät och `privateIPAddress`.</span><span class="sxs-lookup"><span data-stu-id="865ef-177">Change hello load balancer's `frontendIPConfigurations` setting from using a `publicIPAddress`, toousing a subnet and `privateIPAddress`.</span></span> <span data-ttu-id="865ef-178">`privateIPAddress`använder en fördefinierad statiska interna IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-178">`privateIPAddress` uses a predefined static internal IP address.</span></span> <span data-ttu-id="865ef-179">toouse en dynamisk IP-adress, ta bort hello `privateIPAddress` element och ändrar sedan `privateIPAllocationMethod` för**dynamiska**.</span><span class="sxs-lookup"><span data-stu-id="865ef-179">toouse a dynamic IP address, remove hello `privateIPAddress` element, and then change `privateIPAllocationMethod` too**Dynamic**.</span></span>

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. <span data-ttu-id="865ef-180">I hello `Microsoft.ServiceFabric/clusters` resurs, ändra `managementEndpoint` toopoint toohello interna adressen för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="865ef-180">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toopoint toohello internal load balancer address.</span></span> <span data-ttu-id="865ef-181">Om du använder en säker kluster, se till att du ändrar *http://* för*https://*.</span><span class="sxs-lookup"><span data-stu-id="865ef-181">If you use a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="865ef-182">(Observera att det här steget gäller endast tooService Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="865ef-182">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="865ef-183">Om du använder en skaluppsättning för virtuell dator kan du hoppa över det här steget.)</span><span class="sxs-lookup"><span data-stu-id="865ef-183">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. <span data-ttu-id="865ef-184">Distribuera hello mallen:</span><span class="sxs-lookup"><span data-stu-id="865ef-184">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

<span data-ttu-id="865ef-185">Efter distributionen kan använder din belastningsutjämnare hello 10.0.0.250 för privata statiska IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-185">After deployment, your load balancer uses hello private static 10.0.0.250 IP address.</span></span> <span data-ttu-id="865ef-186">Om du har en annan dator i samma virtuella nätverk, kan du gå toohello interna [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="865ef-186">If you have another machine in that same virtual network, you can go toohello internal [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint.</span></span> <span data-ttu-id="865ef-187">Observera att det ansluter tooone hello noder bakom hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-187">Note that it connects tooone of hello nodes behind hello load balancer.</span></span>

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a><span data-ttu-id="865ef-188">Interna och externa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="865ef-188">Internal and external load balancer</span></span>

<span data-ttu-id="865ef-189">I detta scenario du börjar med hello befintliga nod typen extern belastningsutjämnare och Lägg till en intern belastningsutjämnare för hello samma nodtypen.</span><span class="sxs-lookup"><span data-stu-id="865ef-189">In this scenario, you start with hello existing single-node type external load balancer, and add an internal load balancer for hello same node type.</span></span> <span data-ttu-id="865ef-190">En backend-port som ansluten tooa backend-adresspool kan tilldelas endast tooa som enda belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="865ef-190">A back-end port attached tooa back-end address pool can be assigned only tooa single load balancer.</span></span> <span data-ttu-id="865ef-191">Välj vilka belastningsutjämnare ska ha application-portar och vilka belastningsutjämnaren har dina hanteringsslutpunkter (portar 19000 och 19080).</span><span class="sxs-lookup"><span data-stu-id="865ef-191">Choose which load balancer will have your application ports, and which load balancer will have your management endpoints (ports 19000 and 19080).</span></span> <span data-ttu-id="865ef-192">Om du placerar hello management slutpunkter på hello intern belastningsutjämnare Kom ihåg hello Service Fabric-resurs providern begränsningar som beskrivs i hello ovan.</span><span class="sxs-lookup"><span data-stu-id="865ef-192">If you put hello management endpoints on hello internal load balancer, keep in mind hello Service Fabric resource provider restrictions discussed earlier in hello article.</span></span> <span data-ttu-id="865ef-193">Hello exempelvis använder vi hello management slutpunkter ha kvar hello extern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-193">In hello example we use, hello management endpoints remain on hello external load balancer.</span></span> <span data-ttu-id="865ef-194">Du också lägga till en port 80 programmet port och placera den på hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-194">You also add a port 80 application port, and place it on hello internal load balancer.</span></span>

<span data-ttu-id="865ef-195">En nodtyp finns på hello extern belastningsutjämnare i ett kluster med två nodtypen.</span><span class="sxs-lookup"><span data-stu-id="865ef-195">In a two-node-type cluster, one node type is on hello external load balancer.</span></span> <span data-ttu-id="865ef-196">hello finns andra nodtyp på hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-196">hello other node type is on hello internal load balancer.</span></span> <span data-ttu-id="865ef-197">toouse ett kluster i hello portal skapas två nodtypen mall (som levereras med två belastningsutjämnare), två nodtypen växla hello andra belastningsutjämnare tooan interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-197">toouse a two-node-type cluster, in hello portal-created two-node-type template (which comes with two load balancers), switch hello second load balancer tooan internal load balancer.</span></span> <span data-ttu-id="865ef-198">Mer information finns i hello [endast interna belastningsutjämnare](#internallb) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="865ef-198">For more information, see hello [Internal-only load balancer](#internallb) section.</span></span>

1. <span data-ttu-id="865ef-199">Lägg till parametern hello statiska interna belastningen belastningsutjämnaren IP-adress.</span><span class="sxs-lookup"><span data-stu-id="865ef-199">Add hello static internal load balancer IP address parameter.</span></span> <span data-ttu-id="865ef-200">(Anteckningar relaterade toousing en dynamisk IP-adress, se föregående avsnitten i den här artikeln.)</span><span class="sxs-lookup"><span data-stu-id="865ef-200">(For notes related toousing a dynamic IP address, see earlier sections of this article.)</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. <span data-ttu-id="865ef-201">Lägg till ett program port 80-parameter.</span><span class="sxs-lookup"><span data-stu-id="865ef-201">Add an application port 80 parameter.</span></span>

3. <span data-ttu-id="865ef-202">tooadd interna versioner av hello befintliga nätverk variabler, kopiera och klistra in dem och lägga till ”-Int” toohello namn:</span><span class="sxs-lookup"><span data-stu-id="865ef-202">tooadd internal versions of hello existing networking variables, copy and paste them, and add "-Int" toohello name:</span></span>

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. <span data-ttu-id="865ef-203">Om du börjar med hello portal-genererade mall som använder programmet port 80 hello portal standardmallen lägger till AppPort1 (port 80) på hello extern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-203">If you start with hello portal-generated template that uses application port 80, hello default portal template adds AppPort1 (port 80) on hello external load balancer.</span></span> <span data-ttu-id="865ef-204">I så fall bort AppPort1 från hello extern belastningsutjämnare `loadBalancingRules` och avsökningar, så du kan lägga till den interna belastningsutjämnaren för toohello:</span><span class="sxs-lookup"><span data-stu-id="865ef-204">In this case, remove AppPort1 from hello external load balancer `loadBalancingRules` and probes, so you can add it toohello internal load balancer:</span></span>

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. <span data-ttu-id="865ef-205">Lägga till ett andra `Microsoft.Network/loadBalancers` resurs.</span><span class="sxs-lookup"><span data-stu-id="865ef-205">Add a second `Microsoft.Network/loadBalancers` resource.</span></span> <span data-ttu-id="865ef-206">Det ser ut ungefär interna toohello-belastningsutjämnaren som skapats i hello [endast interna belastningsutjämnare](#internallb) avsnittet, men använder hello ”-Int” belastningsutjämnaren variabler och implementerar endast hello programmet port 80.</span><span class="sxs-lookup"><span data-stu-id="865ef-206">It looks similar toohello internal load balancer created in hello [Internal-only load balancer](#internallb) section, but it uses hello "-Int" load balancer variables, and implements only hello application port 80.</span></span> <span data-ttu-id="865ef-207">Detta tar också bort `inboundNatPools`, tookeep RDP-slutpunkterna på hello offentlig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-207">This also removes `inboundNatPools`, tookeep RDP endpoints on hello public load balancer.</span></span> <span data-ttu-id="865ef-208">Om du vill RDP på hello intern belastningsutjämnare flyttar `inboundNatPools` från hello externa belastningsutjämnare toothis interna belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="865ef-208">If you want RDP on hello internal load balancer, move `inboundNatPools` from hello external load balancer toothis internal load balancer:</span></span>

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and hello "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" toohello name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public tooPrivate IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add hello AppPort rule. Be sure tooreference hello "-Int" versions of backendAddressPool, frontendIPConfiguration, and hello probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add hello probe for hello app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. <span data-ttu-id="865ef-209">I `networkProfile` för hello `Microsoft.Compute/virtualMachineScaleSets` resurs, Lägg till hello interna backend-adresspool:</span><span class="sxs-lookup"><span data-stu-id="865ef-209">In `networkProfile` for hello `Microsoft.Compute/virtualMachineScaleSets` resource, add hello internal back-end address pool:</span></span>

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. <span data-ttu-id="865ef-210">Distribuera hello mallen:</span><span class="sxs-lookup"><span data-stu-id="865ef-210">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

<span data-ttu-id="865ef-211">Efter distributionen kan se du två belastningsutjämnare i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="865ef-211">After deployment, you can see two load balancers in hello resource group.</span></span> <span data-ttu-id="865ef-212">Du kan se hello offentlig IP-adress och hantering av slutpunkter (portar 19000 och 19080) tilldelas toohello offentliga IP-adress om du bläddrar hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-212">If you browse hello load balancers, you can see hello public IP address and management endpoints (ports 19000 and 19080) assigned toohello public IP address.</span></span> <span data-ttu-id="865ef-213">Du kan också se hello statiska interna IP-adress och programmet slutpunkt (port 80) tilldelas toohello interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="865ef-213">You also can see hello static internal IP address and application endpoint (port 80) assigned toohello internal load balancer.</span></span> <span data-ttu-id="865ef-214">Både läsa in belastningsutjämning Använd hello samma virtuella skaluppsättning för backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="865ef-214">Both load balancers use hello same virtual machine scale set back-end pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="865ef-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="865ef-215">Next steps</span></span>
[<span data-ttu-id="865ef-216">Skapa ett kluster</span><span class="sxs-lookup"><span data-stu-id="865ef-216">Create a cluster</span></span>](service-fabric-cluster-creation-via-arm.md)
