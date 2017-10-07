---
title: "aaaNetworking för skalningsuppsättningar i virtuella Azure-datorn | Microsoft Docs"
description: "Konfigurationsnätverksegenskaper för skalningsuppsättningar för virtuella Azure-datorer."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: guybo
ms.openlocfilehash: ef3f0cfe648d2195c051a73987e654f0e15d13bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Nätverk för skalningsuppsättningar för virtuella Azure-datorer

När du distribuerar en virtuell dator i Azure-skala anges via hello portal vissa egenskaper för nätverk som är standard, till exempel en Azure belastningsutjämnare med inkommande NAT-regler. Den här artikeln beskriver hur toouse vissa hello mer avancerade funktioner som du kan konfigurera med en skala anger.

Du kan konfigurera alla hello-funktioner som beskrivs i den här artikeln med hjälp av Azure Resource Manager-mallar. Azure CLI- och PowerShell-exempel ingår också i de valda funktionerna. Använd CLI 2.10 och PowerShell 4.2.0 eller senare.

## <a name="accelerated-networking"></a>Accelererat nätverk
Azure [snabbare nätverk](../virtual-network/virtual-network-create-vm-accelerated-networking.md) förbättrar nätverkets prestanda genom att aktivera single-root I/O virtualization (SR-IOV) tooa virtuella datorn. toouse snabbare nätverk med skaluppsättningar genom att ange enableAcceleratedNetworking för**SANT** i inställningarna för din skaluppsättning networkInterfaceConfigurations. Exempel:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Skapa en skalningsuppsättning som refererar till en befintlig Azure Load Balancer
När en skaluppsättning skapas med hjälp av hello Azure-portalen, skapas en ny belastningsutjämnare för de flesta konfigurationsalternativ. Om du skapar en skalningsuppsättning som måste tooreference en befintlig belastningsutjämnare, kan du göra detta med hjälp av CLI. Följande exempelskript hello skapar en belastningsutjämnare och skapar sedan en skalningsuppsättning som refererar till den:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Konfigurera DNS-inställningar
Som standard utför skalningsuppsättningar på hello specifika DNS-inställningarna för hello VNET och undernät som de skapades i. Du kan dock konfigurera hello DNS-inställningarna för en skala som anges direkt.
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Skapa en skalningsuppsättning med konfigurerbara DNS-servrar
toocreate en skala som anges med en anpassad DNS-konfiguration med hjälp av CLI 2.0, lägga till hello **--dns-servrar** argumentet toohello **vmss skapa** följt av utrymme avgränsade ip-adresser. Exempel:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
tooconfigure anpassade DNS-servrar i en Azure-mall, lägga till en dnsSettings egenskapen toohello skala networkInterfaceConfigurations avsnitt. Exempel:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Skapa en skalningsuppsättning med konfigurerbara domännamn för virtuella datorer
toocreate skaluppsättning med en anpassad DNS-namnet för virtuella datorer med hjälp av CLI 2.0, lägga till hello **--vm domännamn** argumentet toohello **vmss skapa** följt av en sträng som representerar hello domännamn.

tooset hello-domännamnet i en Azure-mall, lägga till en **dnsSettings** skaluppsättning för egenskapen toohello **networkInterfaceConfigurations** avsnitt. Exempel:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

hello utdata för en enskild virtuell dator dns-namn är i hello följande format: 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>Offentlig IPv4 per virtuell dator
I allmänhet kräver inte skalningsuppsättningar för virtuella Azure-datorer sina egna offentliga IP-adresser. För de flesta fall är det mer ekonomiska och säker tooassociate en offentlig IP-adress tooa belastningen belastningsutjämnare eller tooan enskild virtuell dator (även kallat en jumpbox) som sedan vidarebefordrar inkommande anslutningar tooscale uppsättning virtuella datorer efter behov (till exempel via inkommande NAT-regler).

Dock vissa scenarier kräver skaluppsättning för virtuella datorer toohave sina egna offentliga IP-adresser. Ett exempel spel, där en konsol måste toomake en direktanslutning tooa moln virtuella datorn, vilket gör spelet fysik bearbetning. Ett annat exempel är där virtuella datorer måste toomake externa anslutningar tooone tvärs över regioner i en distribuerad databas.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Skapa en skalningsuppsättning med en offentlig IP per virtuell dator
toocreate en skalningsuppsättning som tilldelar en offentlig IP-adress tooeach virtuell dator med CLI 2.0, lägga till hello **--offentlig ip per vm** parametern toohello **vmss skapa** kommando. 

toocreate en skala som anges med en Azure-mall, kontrollera hello API-versionen av hello Microsoft.Compute/virtualMachineScaleSets resursen är minst **2017-03-30**, och Lägg till en **publicIpAddressConfiguration**JSON egenskapen toohello skaluppsättning ipConfigurations avsnitt. Exempel:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Exempelmall: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a>Frågar hello offentliga IP-adresser för hello virtuella datorer på en skala ange
toolist hello offentliga IP-adresser tilldelas tooscale uppsättning virtuella datorer med hjälp av CLI 2.0 använder hello **az vmss lista-instans-offentliga-IP-adresser** kommando.

toolist skaluppsättning för den offentliga IP-adresser med hjälp av PowerShell använder hello _Get-AzureRmPublicIpAddress_ kommando. Exempel:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

Du kan också fråga hello offentliga IP-adresser genom att referera hello resurs-id för hello offentliga IP-adresskonfigurationen direkt. Exempel:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

tooquery hello offentliga IP-adresser tilldelas tooscale uppsättning virtuella datorer med hello [resursutforskaren Azure](https://resources.azure.com), eller hello Azure REST-API med version **2017-03-30** eller högre.

tooview offentliga IP-adresser för en skala som anges med hello resursutforskaren, titta på hello **publicipaddresses** avsnitt under din skaluppsättning. Till exempel: https://resources.azure.com/subscriptions/_your_sub_id_/resourceGroups/_your_rg_/providers/Microsoft.Compute/virtualMachineScaleSets/_your_vmss_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Exempel på utdata:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Flera IP-adresser per nätverkskort
Varje nätverkskort anslutet tooa virtuell dator i en skaluppsättning kan ha en eller flera IP-konfigurationer som är kopplade till den. Varje konfiguration tilldelas en privat IP-adress. Varje konfiguration kan också ha en associerad offentlig IP-adressresurs. toounderstand hur många IP-adresser kan tilldelas tooa NIC, och hur många offentliga IP-adresser som du kan använda i en Azure-prenumeration finns för[Azure gränser](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Flera nätverkskort per virtuell dator
Du kan ha upp too8 nätverkskort per virtuell dator, beroende på storleken på datorn. hello maximalt antal nätverkskort per dator är tillgängliga i hello [VM storlek artikel](../virtual-machines/windows/sizes.md). hello följande exempel är en skaluppsättning för nätverksprofil visar flera poster för NIC och flera offentliga IP-adresser per virtuell dator:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>Nätverkssäkerhetsgrupp per skalningsuppsättning
Nätverkssäkerhetsgrupper kan tillämpas direkt tooa skaluppsättning genom att lägga till en referens toohello network interface konfigurationsavsnittet hello skalan ange egenskaper för virtuell dator.

Exempel: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nästa steg
Mer information om virtuella Azure-nätverk finns för[denna dokumentation](../virtual-network/virtual-networks-overview.md).
