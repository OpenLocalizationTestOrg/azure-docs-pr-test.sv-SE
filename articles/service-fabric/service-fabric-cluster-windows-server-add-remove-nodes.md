---
title: "aaaAdd eller ta bort noder tooa fristående Service Fabric-kluster | Microsoft Docs"
description: "Lär dig hur tooadd eller ta bort noder tooan Azure Service Fabric-kluster på en fysisk eller virtuell dator som kör Windows Server, som kan vara lokalt eller i något moln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a>Lägg till eller ta bort noder tooa fristående Service Fabric-kluster körs på Windows Server
När du har [skapa fristående Service Fabric-klustret på Windows Server-datorer](service-fabric-cluster-creation-for-windows-server.md)behoven för din verksamhet kan ändras och du kanske behöver tooadd eller ta bort noder tooyour klustret. Den här artikeln innehåller detaljerade anvisningar tooachieve detta. Observera att lägga till/ta bort noden funktionen inte stöds i kluster för lokal utveckling.

## <a name="add-nodes-tooyour-cluster"></a>Lägga till noder tooyour kluster
1. Förbered hello VM/dator du vill tooadd tooyour klustret genom att följa anvisningarna i hello hello [Förbered hello datorer toomeet hello krav för klusterdistribution](service-fabric-cluster-creation-for-windows-server.md) avsnitt
2. Identifiera vilka feldomän och uppgraderingsdomänen som du är pågående tooadd den VM/datorn
3. Fjärrskrivbord (RDP) till hello VM/dator som du vill tooadd toohello kluster
4. Kopiera eller [hämta hello fristående paketet för Service Fabric för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello VM/datorn och packa hello-paket
5. Kör Powershell med förhöjd behörighet och gå toohello platsen för hello uppackade
6. Kör hello *AddNode.ps1* skript med hello parametrar som beskriver hello nya nod tooadd. hello exemplet nedan lägger till en ny nod med namnet VM5, med typen NodeType0 och IP-adress 182.17.34.52, i UD1 och fd: / dc1/r0. Hej *ExistingClusterConnectionEndPoint* finns redan en Anslutningens slutpunkt för en nod i hello befintligt kluster, vilket kan vara hello IP-adressen för *alla* nod i klustret hello.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    När hello skriptet är klar kan du kontrollera om hello ny nod har lagts till genom att köra hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.

7. tooensure konsekvent på olika noder i klustret hello, måste du påbörja en uppgradering av konfiguration. Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello senaste konfigurationsfilen och Lägg till hello nyligen tillagda noden för avsnittet ”noder”. Det rekommenderas också tooalways har hello senaste klusterkonfigurationen tillgängliga hello om du behöver tooredploy ett kluster med hello samma konfiguration.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer. Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a>Lägg till noder tooclusters konfigurerade med Windows-säkerhet som använder gMSA
För kluster som har konfigurerats med gruppen hanteras Service Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx), kan en ny nod läggas till med hjälp av en uppgradering av konfiguration:
1. Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) på någon av hello befintliga noderna tooget hello senaste konfigurationsfilen och Lägg till information om hello ny nod som du vill att tooadd i-noder ”hello” avsnittet. Kontrollera att nya hello-noden är en del av hello samma grupp hanterat konto. Det här kontot måste vara administratör på alla datorer.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer. Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-tooyour-cluster"></a>Lägga till noden typer tooyour kluster
I ordning tooadd en ny nodtyp, ändra din konfiguration tooinclude hello nya nodtyp i ”nodetypes får” under ”egenskaper” och börjar en konfiguration uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). När hello uppgraderingen är klar kan du lägga till nya noder tooyour klustret med den här nodtypen.

## <a name="remove-nodes-from-your-cluster"></a>Ta bort noder från klustret
En nod kan tas bort från ett kluster med en konfiguration uppgradering i hello följande sätt:

1. Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello senaste konfigurationsfilen och *ta bort* hello nod från avsnittet ”noder”.
Lägg till hello ”NodesToBeRemoved” parametern för ”inställningar” avsnittet i avsnittet ”FabricSettings”. Hej ”värde” ska vara en kommaavgränsad lista över nodnamn noder som behöver toobe tas bort.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer. Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Borttagning av noder kan starta flera uppgraderingar. Vissa noder har markerats med `IsSeedNode=”true”` tagga och kan identifieras genom att fråga hello klustret manifest med `Get-ServiceFabricClusterManifest`. Borttagning av dessa noder kan ta längre tid än andra eftersom hello seed noder måste toobe flyttas i dessa scenarier. hello klustret måste upprätthålla minst 3 primära noden typen noder.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Ta bort nod från klustret
Innan du tar bort en nodtyp, kontrollera om det finns några noder som refererar till hello nodtypen. Ta bort dessa noder innan du tar bort hello motsvarande nodtypen. När alla motsvarande noder har tagits bort kan du ta bort hello NodeType från hello klusterkonfigurationen och börja en konfiguration uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Ersätt primära noder i klustret
hello ersättning av primära noder måste vara utförs en nod efter en annan, i stället för att ta bort och sedan lägga till i batchar.


## <a name="next-steps"></a>Nästa steg
* [Konfigurationsinställningar för fristående Windows-kluster](service-fabric-cluster-manifest.md)
* [Skydda ett fristående kluster på Windows med X509 certifikat](service-fabric-windows-cluster-x509-security.md)
* [Skapa ett fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)

