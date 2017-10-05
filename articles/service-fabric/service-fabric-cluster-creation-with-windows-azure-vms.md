---
title: "Skapa ett fristående kluster med Azure virtuella datorer som kör Windows | Microsoft Docs"
description: "Lär dig hur du skapar och hanterar Azure Service Fabric-kluster på Azure virtuella datorer som kör Windows Server."
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
ms.openlocfilehash: f8a0305a22c00f9bdbdb1bdb06dc299cccee23dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Skapa en tre noder fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows Server
Den här artikeln beskriver hur du skapar ett kluster på Windows-baserade virtuella Azure-datorer (VM) med fristående Service Fabric-installationsprogram för Windows Server. Scenariot är ett specialfall av [skapa och hantera ett kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md) där de virtuella datorerna är [Azure virtuella datorer som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), men du inte skapar [en Azure molnbaserad Service Fabric-kluster](service-fabric-cluster-creation-via-portal.md). Skillnad i följande det här mönstret är att fristående Service Fabric-kluster skapas med följande steg hanteras helt av dig, medan Azure molnbaserade Service Fabric klustren hanteras och uppgraderas av Service Fabric-resurs providern.

## <a name="steps-to-create-the-standalone-cluster"></a>Steg för att skapa fristående klustret
1. Logga in på Azure-portalen och skapa en ny Windows Server 2012 R2 Datacenter eller Windows Server 2016 Datacenter VM i en resursgrupp. Läs artikeln [skapa en virtuell Windows-dator i Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för mer information.
2. Lägga till några fler virtuella datorer i samma resursgrupp. Kontrollera att var och en av de virtuella datorerna har samma administratörsanvändarnamn och lösenord när de skapas. En gång skapade du bör se alla tre virtuella datorer i samma virtuella nätverk.
3. Ansluta till var och en av de virtuella datorerna och inaktivera Windows-brandväggen med den [Serverhanteraren instrumentpanelen lokal Server](https://technet.microsoft.com/library/jj134147.aspx). Detta säkerställer att nätverkstrafik kan kommunicera mellan datorerna. När du är ansluten till varje dator hämta IP-adress genom att öppna en kommandotolk och skriva `ipconfig`. Du kan också se IP-adressen för varje dator på portalen genom att välja resursen virtuellt nätverk för resursgruppen och kontrollera nätverksgränssnitt som skapats för var och en av dessa datorer.
4. Ansluta till en av de virtuella datorerna och testa att du kan pinga har de andra två virtuella datorerna.
5. Ansluta till en av de virtuella datorerna och [hämta fristående Service Fabric-paket för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) till en ny mapp på datorn och extrahera paketet.
6. Öppna den *ClusterConfig.Unsecure.MultiMachine.json* filen i anteckningar och redigera varje nod med tre IP-adresser för datorer. Ändra klusternamnet överst och spara filen.  En partiell exempel på klustermanifestet visas nedan.
   
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
7. Om du vill att detta ska vara en säker, besluta säkerhetsåtgärd som du vill använda och följ stegen i den associerade länken: [X509 certifikat](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md). Om klustret med Windows-säkerhet, behöver du konfigurera en domänkontrollant för att hantera Active Directory. Observera att en domänkontrollant dator som en Service Fabric nod inte stöds.
8. Öppna en [fönstret PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Navigera till mappen där du extraherade installationspaketet hämtade fristående och spara konfigurationsfilen för klustret. Kör följande PowerShell-kommando för att distribuera klustret:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

Skriptet fjärrkonfigurera Service Fabric-kluster och bör rapportera förlopp som distributionen samlar via.

9. Efter ungefär en minut kan du kontrollera om klustret fungerar genom att ansluta till Service Fabric-Utforskaren med ett av datorns IP-adresser, till exempel med hjälp av `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Nästa steg
* [Skapa fristående Service Fabric-kluster i Windows Server eller Linux](service-fabric-deploy-anywhere.md)
* [Lägg till eller ta bort noder i ett fristående Service Fabric-kluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Konfigurationsinställningar för fristående Windows-kluster](service-fabric-cluster-manifest.md)
* [Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet](service-fabric-windows-cluster-windows-security.md)
* [Skydda ett fristående kluster på Windows med X509 certifikat](service-fabric-windows-cluster-x509-security.md)

