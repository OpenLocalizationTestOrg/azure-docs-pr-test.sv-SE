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
# <a name="service-fabric-networking-patterns"></a>Service Fabric nätverk mönster
Du kan integrera Azure Service Fabric-kluster med andra funktioner för Azure. I den här artikeln visar vi du hur toocreate kluster att använda hello följande funktioner:

- [Befintligt virtuellt nätverk eller undernät](#existingvnet)
- [Statiska offentliga IP-adressen](#staticpublicip)
- [Endast interna belastningsutjämnare](#internallb)
- [Interna och externa belastningsutjämnare](#internalexternallb)

Service Fabric körs i en standard virtuella datorns skaluppsättning. Funktioner som du kan använda i en skaluppsättning för virtuell dator, som du kan använda med ett Service Fabric-kluster. hello nätverk avsnitt hello Azure Resource Manager-mallar för virtuella datorer och Service Fabric är identiska. När du distribuerar tooan befintliga virtuella nätverk, är det enkelt tooincorporate andra nätverksfunktioner som Azure ExpressRoute, Azure VPN-Gateway, en säkerhetsgrupp för nätverk och virtuella nätverk peering.

Service Fabric är unikt från andra nätverksfunktioner i en aspekt. Hej [Azure-portalen](https://portal.azure.com) internt använder hello Service Fabric-providern toocall tooa klustret tooget resursinformation om noderna och program. hello Service Fabric-resursprovidern kräver offentligt tillgänglig inkommande åtkomst toohello HTTP gateway-porten (port 19080, som standard) på hello management slutpunkt. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) använder hello management endpoint toomanage klustret. hello Service Fabric-resursprovidern använder också den här porten tooquery information om klustret toodisplay i hello Azure-portalen. 

Om port 19080 inte är tillgänglig från hello Service Fabric-resursprovidern, ett meddelande som *noder hittades inte* visas i hello-portalen och listan noden och programmet visas tomt. Om du vill toosee klustret i hello Azure-portalen, din belastningsutjämnare måste exponera en offentlig IP-adress och din nätverkssäkerhetsgruppen måste tillåta inkommande Porttrafik 19080. Om din konfiguration inte uppfyller dessa krav visas inte hello Azure-portalen hello status för klustret.

## <a name="templates"></a>Mallar

Alla Service Fabric-mallar finns i [en nedladdning filen](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip). Du bör vara kan toodeploy hello mallar som-är med hjälp av hello följande PowerShell-kommandon. Om du distribuerar hello befintliga Azure-nätverk eller hello statiska offentliga IP-mallen, läsa hello [inledande installationen](#initialsetup) i den här artikeln.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Installationen

### <a name="existing-virtual-network"></a>Befintligt virtuellt nätverk

I följande exempel hello, vi börjar med ett befintligt virtuellt nätverk med namnet ExistingRG-vnet i hello **ExistingRG** resursgruppen. hello undernät kallas standard. Dessa standardresurser skapas när du använder hello Azure portal toocreate en vanliga virtuell dator (VM). Du kan skapa hello virtuellt nätverk och undernät utan att skapa hello VM, men hello Huvudmålet för att lägga till ett kluster tooan befintligt virtuellt nätverk är tooprovide network connectivity tooother virtuella datorer. Skapa hello VM ger ett bra exempel på hur ett befintligt virtuellt nätverk normalt används. Om din Service Fabric-klustret använder endast en intern belastningsutjämnare, utan en offentlig IP-adress kan du använda hello virtuella datorn och dess offentliga IP-adress som en säker *hoppa rutan*.

### <a name="static-public-ip-address"></a>Statiska offentliga IP-adressen

En statisk offentlig IP-adress är vanligtvis en dedikerad resurs som hanteras separat från hello VM eller virtuella datorer som den är tilldelad till. Det har etablerats i ett dedikerat nätverk resursgrupp (som skillnad från tooin hello Service Fabric klusterresursgrupp sig själv). Skapa en statisk offentlig IP-adress med namnet staticIP1 i hello samma ExistingRG resursgrupp, antingen i hello Azure-portalen eller med hjälp av PowerShell:

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

### <a name="service-fabric-template"></a>Service Fabric-mall

Vi använder hello Service Fabric template.json i hello exemplen i den här artikeln. Du kan använda hello standard portal guiden toodownload hello mallen från hello-portalen innan du skapar ett kluster. Du kan också använda en av hello mallar i hello [mallgalleriet](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), som hello [Service Fabric-kluster med fem noder](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Befintligt virtuellt nätverk eller undernät

1. Ändra hello undernät toohello på parameternamn hello befintligt undernät och Lägg sedan till två nya parametrar tooreference hello befintligt virtuellt nätverk:

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


2. Ändra hello `vnetID` variabeln toopoint toohello befintligt virtuellt nätverk:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Ta bort `Microsoft.Network/virtualNetworks` från dina resurser, så Azure skapar inte ett nytt virtuellt nätverk:

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

4. Kommentera hello virtuella nätverket från hello `dependsOn` attribut för `Microsoft.Compute/virtualMachineScaleSets`, så att du inte beror på hur du skapar ett nytt virtuellt nätverk:

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

5. Distribuera hello mallen:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Efter distributionen kan det virtuella nätverket bör innehålla hello ny skaluppsättning för virtuella datorer. hello virtuella scale set nodtypen ska visa hello befintligt virtuellt nätverk och undernät. Du kan också använda protokollet RDP (Remote Desktop) tooaccess hello virtuell dator som redan finns i hello virtuella nätverk och tooping hello ny skaluppsättning för virtuella datorer:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Ett annat exempel är finns [ett som inte är specifikt tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statiska offentliga IP-adressen

1. Lägg till parametrar för hello namnet på hello befintlig statisk IP-resursgrupp, namn och fullständigt kvalificerade domännamnet (FQDN):

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

2. Ta bort hello `dnsName` parameter. (hello statisk IP-adress har redan en.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Lägg till en variabel tooreference hello befintlig statisk IP-adress:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Ta bort `Microsoft.Network/publicIPAddresses` från dina resurser, så Azure skapar inte en ny IP-adress:

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

5. Kommentera hello IP-adress från hello `dependsOn` attribut för `Microsoft.Network/loadBalancers`, så att du inte beror på hur du skapar en ny IP-adress:

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

6. I hello `Microsoft.Network/loadBalancers` resurs, ändra hello `publicIPAddress` element av `frontendIPConfigurations` tooreference hello befintlig statisk IP-adress i stället för en nyligen skapade:

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

7. I hello `Microsoft.ServiceFabric/clusters` resurs, ändra `managementEndpoint` toohello DNS FQDN för hello statisk IP-adress. Om du använder en säker kluster, kontrollerar du att du ändrar *http://* för*https://*. (Observera att det här steget gäller endast tooService Fabric-kluster. Om du använder en skaluppsättning för virtuell dator kan du hoppa över det här steget.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Distribuera hello mallen:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Efter distributionen kan du ser att din belastningsutjämnare är bundna toohello offentlig statisk IP-adress från hello annan resursgrupp. Hej Service Fabric-klienten Anslutningens slutpunkt och [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint punkt toohello DNS FQDN för hello statisk IP-adress.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Endast interna belastningsutjämnare

Det här scenariot ersätter hello extern belastningsutjämnare i hello standardmallen för Service Fabric med en endast är interna belastningsutjämnare. Konsekvenser för hello Azure-portalen och hello Service Fabric-resursprovidern finns hello föregående avsnitt.

1. Ta bort hello `dnsName` parameter. (Det krävs inte.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. Alternativt kan du använda en statisk tilldelningsmetod kan du lägga till parametern för en statisk IP-adress. Om du använder en dynamisk fördelning, behöver du inte toodo det här steget.

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Ta bort `Microsoft.Network/publicIPAddresses` från dina resurser, så Azure skapar inte en ny IP-adress:

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

4. Ta bort IP-adress för hello `dependsOn` attribut för `Microsoft.Network/loadBalancers`, så att du inte beror på hur du skapar en ny IP-adress. Lägga till hello virtuellt nätverk `dependsOn` attributet eftersom hello belastningsutjämnaren nu beror på hello undernät från hello virtuella nätverket:

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

5. Ändra hello belastningsutjämnarens `frontendIPConfigurations` från med hjälp av en `publicIPAddress`, toousing ett undernät och `privateIPAddress`. `privateIPAddress`använder en fördefinierad statiska interna IP-adress. toouse en dynamisk IP-adress, ta bort hello `privateIPAddress` element och ändrar sedan `privateIPAllocationMethod` för**dynamiska**.

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

6. I hello `Microsoft.ServiceFabric/clusters` resurs, ändra `managementEndpoint` toopoint toohello interna adressen för belastningsutjämnaren. Om du använder en säker kluster, se till att du ändrar *http://* för*https://*. (Observera att det här steget gäller endast tooService Fabric-kluster. Om du använder en skaluppsättning för virtuell dator kan du hoppa över det här steget.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Distribuera hello mallen:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Efter distributionen kan använder din belastningsutjämnare hello 10.0.0.250 för privata statiska IP-adress. Om du har en annan dator i samma virtuella nätverk, kan du gå toohello interna [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) slutpunkt. Observera att det ansluter tooone hello noder bakom hello belastningsutjämnare.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>Interna och externa belastningsutjämnare

I detta scenario du börjar med hello befintliga nod typen extern belastningsutjämnare och Lägg till en intern belastningsutjämnare för hello samma nodtypen. En backend-port som ansluten tooa backend-adresspool kan tilldelas endast tooa som enda belastningsutjämnaren. Välj vilka belastningsutjämnare ska ha application-portar och vilka belastningsutjämnaren har dina hanteringsslutpunkter (portar 19000 och 19080). Om du placerar hello management slutpunkter på hello intern belastningsutjämnare Kom ihåg hello Service Fabric-resurs providern begränsningar som beskrivs i hello ovan. Hello exempelvis använder vi hello management slutpunkter ha kvar hello extern belastningsutjämnare. Du också lägga till en port 80 programmet port och placera den på hello intern belastningsutjämnare.

En nodtyp finns på hello extern belastningsutjämnare i ett kluster med två nodtypen. hello finns andra nodtyp på hello intern belastningsutjämnare. toouse ett kluster i hello portal skapas två nodtypen mall (som levereras med två belastningsutjämnare), två nodtypen växla hello andra belastningsutjämnare tooan interna belastningsutjämnare. Mer information finns i hello [endast interna belastningsutjämnare](#internallb) avsnitt.

1. Lägg till parametern hello statiska interna belastningen belastningsutjämnaren IP-adress. (Anteckningar relaterade toousing en dynamisk IP-adress, se föregående avsnitten i den här artikeln.)

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Lägg till ett program port 80-parameter.

3. tooadd interna versioner av hello befintliga nätverk variabler, kopiera och klistra in dem och lägga till ”-Int” toohello namn:

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

4. Om du börjar med hello portal-genererade mall som använder programmet port 80 hello portal standardmallen lägger till AppPort1 (port 80) på hello extern belastningsutjämnare. I så fall bort AppPort1 från hello extern belastningsutjämnare `loadBalancingRules` och avsökningar, så du kan lägga till den interna belastningsutjämnaren för toohello:

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

5. Lägga till ett andra `Microsoft.Network/loadBalancers` resurs. Det ser ut ungefär interna toohello-belastningsutjämnaren som skapats i hello [endast interna belastningsutjämnare](#internallb) avsnittet, men använder hello ”-Int” belastningsutjämnaren variabler och implementerar endast hello programmet port 80. Detta tar också bort `inboundNatPools`, tookeep RDP-slutpunkterna på hello offentlig belastningsutjämnare. Om du vill RDP på hello intern belastningsutjämnare flyttar `inboundNatPools` från hello externa belastningsutjämnare toothis interna belastningsutjämnare:

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

6. I `networkProfile` för hello `Microsoft.Compute/virtualMachineScaleSets` resurs, Lägg till hello interna backend-adresspool:

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

7. Distribuera hello mallen:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Efter distributionen kan se du två belastningsutjämnare i hello resursgruppen. Du kan se hello offentlig IP-adress och hantering av slutpunkter (portar 19000 och 19080) tilldelas toohello offentliga IP-adress om du bläddrar hello belastningsutjämnare. Du kan också se hello statiska interna IP-adress och programmet slutpunkt (port 80) tilldelas toohello interna belastningsutjämnare. Både läsa in belastningsutjämning Använd hello samma virtuella skaluppsättning för backend-adresspool.

## <a name="next-steps"></a>Nästa steg
[Skapa ett kluster](service-fabric-cluster-creation-via-arm.md)
