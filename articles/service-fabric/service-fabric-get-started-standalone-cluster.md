---
title: "aaaSet in ett fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Skapa ett fristående kluster för utveckling med tre noder körs på hello samma dator. När du har slutfört den här installationen kommer du att redo toocreate ett kluster för flera datorer."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a>Skapa ditt första fristående Service Fabric-kluster
Du kan skapa en fristående Service Fabric-kluster på alla virtuella datorer eller datorer som kör Windows Server 2012 R2 eller Windows Server 2016, lokalt eller i hello molnet. Den här snabbstarten hjälper dig att toocreate ett development fristående kluster på bara några minuter.  När du är klar har du ett kluster med tre noder som körs på en enskild dator som du kan distribuera appar till.

## <a name="before-you-begin"></a>Innan du börjar
Service Fabric innehåller en inställning för paketet toocreate Service Fabric fristående kluster.  [Hämta installationspaketet för hello](http://go.microsoft.com/fwlink/?LinkId=730690).  Packa upp hello installationsprogrammet tooa paketmapp på hello eller virtuell dator där du konfigurerar hello development klustret.  hello innehållet i hello installationspaketet beskrivs i detalj [här](service-fabric-cluster-standalone-package-contents.md).

hello Klusteradministratören distribuera och konfigurera hello klustret måste ha administratörsbehörighet på hello-dator. Du kan inte installera Service Fabric på en domänkontrollant.

## <a name="validate-hello-environment"></a>Validera hello-miljö
Hej *TestConfiguration.ps1* skript i hello fristående paketet används som en best practices analyzer toovalidate om ett kluster kan distribueras på en given miljö. [Förberedelse av distribution](service-fabric-cluster-standalone-deployment-preparation.md) visar hello miljökrav och förutsättningar. Kör hello skriptet tooverify om du kan skapa hello development kluster:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a>Skapa hello-kluster
Flera exempel klustret konfigurationsfiler installeras med hello installationspaketet. *ClusterConfig.Unsecure.DevCluster.json* är hello enklaste klusterkonfigurationen: ett oskyddat, tre noder kluster som körs på en dator.  Andra konfigurationsfiler beskriver kluster med en eller flera datorer som skyddas med X.509-certifikat eller Windows-säkerhet.  Du inte behöver toomodify hello config standardinställningar för den här självstudiekursen, men titta igenom hello konfigurationsfilen och bekanta dig med hello inställningar.  Hej **noder** beskrivs hello tre noder i klustret hello: namn, IP-adress, [nodtypen, feldomän och uppgraderingsdomänen](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  Hej **egenskaper** avsnittet definierar hello [säkerhet, tillförlitlighet nivå, diagnostik samlingen och typer av noder](service-fabric-cluster-manifest.md#cluster-properties) för hello-kluster.

Det här klustret är inte säkert.  Vem som helst kan ansluta anonymt och utföra hanteringsåtgärder, så produktionskluster bör alltid skyddas med X.509-certifikat eller Windows-säkerhet.  Säkerheten är bara konfigurerad vid tidpunkten för skapandet av klustret och den är inte möjligt tooenable säkerhet hello klustret har skapats.  Läs [skydda ett kluster](service-fabric-cluster-security.md) toolearn mer om säkerhet för Service Fabric-klustret.  

toocreate hello tre noder development klustret, kör hello *CreateServiceFabricCluster.ps1* skriptet från en PowerShell-session som administratör:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

hello Service Fabric runtime-paketet automatiskt hämtas och installeras när klustret skapas.

## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster
Utvecklingsklustret med tre noder körs nu. Hej ServiceFabric PowerShell-modulen är installerad med hello körning.  Du kan verifiera hello klustret körs från hello samma dator eller från en fjärrdator med hello Service Fabric runtime.  Hej [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet upprättar en anslutning toohello klustret.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) andra exempel på den anslutande tooa klustret. När du ansluter toohello kluster, använda hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay en lista över noderna i hello kluster och status för varje nod. **HealthState** bör vara *OK* för varje nod.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>Visualisera hello-kluster med hjälp av Service Fabric explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.  Service Fabric Explorer är en tjänst som körs i hello klustret som du kommer åt med en webbläsare genom att navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer). 

Hej klusterinstrumentpanel innehåller en översikt över klustret, inklusive en sammanfattning av program och noden hälsa. hello nod vyn visar hello fysiska struktur hello klustret. För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a>Ta bort hello kluster
tooremove ett kluster som kör hello *RemoveServiceFabricCluster.ps1* PowerShell-skript från hello paketmappen och skicka hello sökvägen toohello JSON-konfigurationsfil. Du kan du ange en plats för hello logg hello borttagningen av.

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

tooremove hello Service Fabric runtime från hello-dator, kör följande PowerShell-skript från hello paketmappen hello.

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett development fristående kluster, försök hello följande artiklar:
* [Konfigurera ett fristående kluster med flera datorer](service-fabric-cluster-creation-for-windows-server.md) och aktivera säkerhet.
* [Distribuera appar med hjälp av PowerShell](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
