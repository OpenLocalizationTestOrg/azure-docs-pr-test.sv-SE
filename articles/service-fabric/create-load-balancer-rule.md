---
title: "aaaCreate en belastningsutjämnare för Azure-regel för ett kluster"
description: "Konfigurera en Azure-belastningsutjämnaren tooopen portar för Azure Service Fabric-klustret."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Öppna portar för ett Service Fabric-kluster

hello belastningsutjämnaren distribueras med Azure Service Fabric-klustret dirigerar trafik tooyour app som körs på en nod. Om du ändrar din app toouse en annan port måste du exponera porten (eller vidarebefordra en annan port) i hello Azure belastningsutjämnare.

När du har distribuerat din service fabric-kluster tooAzure skapades automatiskt en belastningsutjämnare för dig. Om du inte har en belastningsutjämnare, se [konfigurera en Internetriktade belastningsutjämnare](..\load-balancer\load-balancer-get-started-internet-portal.md).

## <a name="configure-service-fabric"></a>Konfigurera service fabric

Service Fabric-programmet **ServiceManifest.xml** config-fil som definierar ditt program förväntar sig toouse hello-slutpunkter. När hello-konfigurationsfilen har uppdaterats toodefine en slutpunkt kan hello belastningsutjämnare måste vara uppdaterade tooexpose som (eller en annan) port. Mer information om hur toocreate hello fabric tjänstslutpunkten finns [konfigurera en slutpunkt](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Skapa en regel för belastningsutjämnare

En regel för belastningsutjämnare öppnar en port mot internet och vidarebefordrar trafik toohello interna nodens port som används av ditt program. Om du inte har en belastningsutjämnare, se [konfigurera en Internetriktade belastningsutjämnare](..\load-balancer\load-balancer-get-started-internet-portal.md).

toocreate belastningsutjämning regel måste toocollect hello följande information:

- Belastningsutjämnarens namn.
- Resursgruppen för hello läsa in belastningsutjämning och service fabric-kluster.
- Externa porten.
- Intern port.

## <a name="azure-cli"></a>Azure CLI
>[!NOTE]
>Om du måste toodetermine hello namnet på hello belastningsutjämnare använder du det här kommandot tooquickly hämta en lista över alla belastningsutjämnare och hello associerade resursgrupper.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

Det tar bara en enda kommando toocreate en belastningsutjämningsregel med hello **Azure CLI**. Du behöver bara tooknow båda hello namnet på hello load balancer och resurs grupp toocreate en ny regel.

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

hello Azure CLI-kommandot har några parametrar som beskrivs i följande tabell hello:

| Parameter | Beskrivning |
| --------- | ----------- |
| `--backend-port`  | hello port hello service fabric program lyssnar på. |
| `--frontend-port` | hello port hello att läsa in belastningsutjämning visar för externa anslutningar. |
| `-lb-name` | hello namnet på hello att läsa in belastningsutjämning toochange. |
| `-g`       | hello resursgrupp som har både hello belastningsutjämnare och service fabric-kluster. |
| `-n`       | hello valt namn på hello regel. |


>[!NOTE]
>Mer information om hur toocreate en belastningsutjämnare med hello Azure CLI, se [skapar en belastningsutjämnare med hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).

## <a name="powershell"></a>PowerShell

>[!NOTE]
>Om du behöver toodetermine hello namnet på hello belastningsutjämnare kan använda det här kommandot tooquickly get en lista över alla belastningsutjämnare och associerade resursgrupper.
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

PowerShell är lite mer komplicerat än hello Azure CLI. Göra begreppsmässigt hello följande steg toocreate en regel.

1. Hämta hello belastningsutjämnaren från Azure.
2. Skapa en regel.
3. Lägga till hello regeln toohello belastningsutjämnare.
4. Uppdatera hello belastningsutjämnaren.

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

Om hello `New-AzureRmLoadBalancerRuleConfig` kommandot hello `-FrontendPort` representerar hello port hello belastningsutjämnaren visar för externa anslutningar och hello `-BackendPort` representerar hello port hello service fabric-appen lyssnar på.

>[!NOTE]
>Mer information om hur toocreate en belastningsutjämnare med PowerShell, se [skapar en belastningsutjämnare med PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).

