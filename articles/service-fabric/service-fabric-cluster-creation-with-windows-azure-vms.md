---
title: "aaaCreate en fristående kluster med Azure virtuella datorer som kör Windows | Microsoft Docs"
description: "Lär dig hur toocreate och hantera en Azure Service Fabric-kluster på Azure virtuella datorer som kör Windows Server."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Skapa en tre noder fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows Server
Den här artikeln beskriver hur toocreate ett kluster på Windows-baserade virtuella Azure-datorer (VM) med hjälp av hello fristående Service Fabric-installationsprogram för Windows Server. hello scenario är ett specialfall av [skapa och hantera ett kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md) där hello är [Azure virtuella datorer som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), men du inte skapar [en Azure molnbaserad Service Fabric-kluster](service-fabric-cluster-creation-via-portal.md). hello skillnad i följande det här mönstret är att hello fristående Service Fabric-kluster som skapats av hello följande hanteras helt av dig, medan hello Azure molnbaserade Service Fabric-kluster hanteras och uppgraderas av hello Service Fabric resursprovidern.

## <a name="steps-toocreate-hello-standalone-cluster"></a>Steg toocreate hello fristående kluster
1. Logga in toohello Azure-portalen och skapa en ny Windows Server 2012 R2 Datacenter eller Windows Server 2016 Datacenter VM i en resursgrupp. Läs hello artikel [skapa en virtuell Windows-dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för mer information.
2. Lägga till några fler virtuella datorer toohello samma resursgrupp. Se till att varje hello virtuella datorer har hello samma administratörsanvändarnamn och lösenord när de skapas. En gång skapade du bör se alla tre virtuella datorer i hello samma virtuella nätverk.
3. Ansluta tooeach av hello virtuella datorer och inaktiverar hello Windows-brandväggen med hjälp av hello [Serverhanteraren instrumentpanelen lokal Server](https://technet.microsoft.com/library/jj134147.aspx). Detta säkerställer att hello nätverkstrafik kan kommunicera mellan hello datorer. När anslutna tooeach datorn hämta hello IP-adress genom att öppna en kommandotolk och skriva `ipconfig`. Du kan också se hello IP-adressen för varje dator på hello-portalen genom att välja hello virtuella nätverksresurs för hello resursgrupp och kontrollerar hello nätverksgränssnitt som skapats för var och en av dessa datorer.
4. Anslut tooone av hello virtuella datorer och kontrollera att du kan pinga hello andra två virtuella datorer har.
5. Ansluta tooone av hello virtuella datorer och [hämta hello fristående Service Fabric-paket för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) till en ny mapp på hello datorn och extrahera hello-paketet.
6. Öppna hello *ClusterConfig.Unsecure.MultiMachine.json* filen i anteckningar och redigera varje nod med hello tre IP-adresser hello datorer. Ändra hello klusternamnet hello överst och spara hello-filen.  En partiell exempel på hello klustermanifestet visas nedan.
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. Om du planerar denna toobe säker kluster, bestämma hello säkerhetsåtgärd du gillar toouse och följ anvisningarna hello i hello associerade länk: [X509 certifikat](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md). Om du ställer in hello-kluster med hjälp av Windows-säkerhet behöver tooset upp en domain controller toomanage Active Directory. Observera att en domänkontrollant dator som en Service Fabric nod inte stöds.
8. Öppna en [fönstret PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Navigera toohello mappen där du extraherade hello hämtade fristående installer-paketet och sparat hello konfigurationsfilen för klustret. Kör följande PowerShell-kommandot toodeploy hello klustret hello:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

hello skriptet fjärrkonfigurera hello Service Fabric-kluster och bör rapportera förlopp som distributionen samlar via.

9. Efter ungefär en minut kan du kontrollera om hello klustret fungerar genom att ansluta toohello Service Fabric Explorer med någon av hello dator-IP-adresser, till exempel med hjälp av `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Nästa steg
* [Skapa fristående Service Fabric-kluster i Windows Server eller Linux](service-fabric-deploy-anywhere.md)
* [Lägg till eller ta bort noder tooa fristående Service Fabric-kluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Konfigurationsinställningar för fristående Windows-kluster](service-fabric-cluster-manifest.md)
* [Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet](service-fabric-windows-cluster-windows-security.md)
* [Skydda ett fristående kluster på Windows med X509 certifikat](service-fabric-windows-cluster-x509-security.md)

