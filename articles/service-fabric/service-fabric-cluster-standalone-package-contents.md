---
title: "aaaAzure Service Fabric-fristående paket för Windows Server | Microsoft Docs"
description: "Beskrivning och innehållet i hello Azure Service Fabric fristående paketet för Windows Server."
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik
ms.openlocfilehash: e4c6cb9128d659144e559fcad06bb78c9969dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Innehållet i Service Fabric fristående paketet för Windows Server
I hello [hämtas](http://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric fristående paketet, hittar du hello följande filer:

| **Filnamn** | **Kort beskrivning** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Ett PowerShell-skript som skapar hello-kluster med hjälp av hello inställningar i ClusterConfig.json. |
| RemoveServiceFabricCluster.ps1 |Ett PowerShell-skript som tar bort ett kluster med hello inställningar i ClusterConfig.json. |
| AddNode.ps1 |Ett PowerShell-skript för att lägga till en nod tooan befintliga distribuerade klustret på hello aktuella datorn. |
| RemoveNode.ps1 |Ett PowerShell-skript för att ta bort en nod från ett befintligt distribuerat kluster från hello aktuella datorn. |
| CleanFabric.ps1 |Ett PowerShell-skript för att rensa en fristående Service Fabric-installation av hello aktuella datorn. Tidigare MSI-installationer bör tas bort med hjälp av sina egna associerade uninstallers. |
| TestConfiguration.ps1 |Ett PowerShell-skript för att analysera hello infrastruktur som anges i hello Cluster.json. |
| DownloadServiceFabricRuntimePackage.ps1 |Ett PowerShell-skript som används för att ladda ned hello senaste runtime-paketet out-of-band, för scenarier där hello distribuera datorn inte är anslutna toohello internet. |
| DeploymentComponentsAutoextractor.exe |Självextraherande arkiv som innehåller komponenter som används av hello fristående paketet skript. |
| EULA_ENU.txt |hello licensvillkoren för hello användning av Microsoft Azure Service Fabric fristående installationspaketet för Windows Server. Du kan [hämta en kopia av hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nu. |
| Viktigt.txt |En länk toohello viktig information och instruktioner för grundläggande installation. Det är en delmängd av hello instruktionerna i det här dokumentet. |
| ThirdPartyNotice.rtf |Meddelande om tredjepartsprogram som finns i hello-paketet. |
| Tools\Microsoft.Azure.ServiceFabric.windowsserver.SupportPackage.zip |StandaloneLogCollector.exe som körs på begäran toocollect och ladda upp trace loggar tooMicrosoft för support ändamål. |
| Tools\ServiceFabricUpdateService.zip |Ett verktyg som används tooenable automatiskt koden uppgradering för kluster som inte har tillgång till internet. Mer information hittar du [här](service-fabric-cluster-upgrade-windows-server.md)|

**Mallar** 
| **Filnamn** | **Kort beskrivning** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Ett kluster exempel konfigurationsfil som innehåller hello inställningar för ett oskyddat, tre-nod, enskild dator (eller virtuella) development kluster, inklusive hello information för varje nod i klustret hello. |
| ClusterConfig.Unsecure.MultiMachine.json |Ett kluster exempel konfigurationsfil som innehåller hello inställningar för ett oskyddat, flera datorer (eller virtuella) kluster, inklusive hello information för varje dator i hello kluster. |
| ClusterConfig.Windows.DevCluster.json |Ett kluster exempel konfigurationsfil som innehåller alla hello inställningar för en säker, tre-nod, enskild dator (eller en virtuell dator) development klustret, inklusive hello information för varje nod i klustret hello. hello kluster skyddas med hjälp av [Windows identiteter](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Ett kluster exempel konfigurationsfil som innehåller alla hello-inställningar för ett säkert, flera datorer (eller virtuella datorn)-kluster med Windows-säkerhet, inklusive hello information för varje dator som finns i säker hello-klustret. hello kluster skyddas med hjälp av [Windows identiteter](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Ett kluster exempel konfigurationsfil som innehåller alla hello inställningar för en säker, tre-nod, enskild dator (eller virtuella) development kluster, inklusive hello information för varje nod i klustret hello. hello kluster skyddas med x509 certifikat. |
| ClusterConfig.x509.MultiMachine.json |Ett kluster exempel konfigurationsfil som innehåller alla hello inställningar för hello säker, flera datorer (eller virtuella) kluster, inklusive hello information för varje nod i hello säker kluster. hello kluster skyddas med x509 certifikat. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Ett kluster exempel konfigurationsfil som innehåller alla hello inställningar för hello säker, flera datorer (eller virtuella) kluster, inklusive hello information för varje nod i hello säker kluster. hello kluster skyddas med [Grupphanterade tjänstkonton](https://technet.microsoft.com/en-us/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Klustret Configuration-exempel
Senaste versionerna av klustrets Konfigurationsmallar finns på GitHub-sidan hello: [fristående kluster Configuration exempel](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Oberoende Runtime-paketet
hello senaste runtime-paketet hämtas automatiskt under Klusterdistribution från [Hämta länk - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>Relaterat
* [Skapa ett fristående Azure Service Fabric-kluster](service-fabric-cluster-creation-for-windows-server.md)
* [Säkerhetsscenarier för Service Fabric-kluster](service-fabric-windows-cluster-windows-security.md)
