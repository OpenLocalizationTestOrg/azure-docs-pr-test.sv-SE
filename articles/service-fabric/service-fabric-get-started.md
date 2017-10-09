---
title: "aaaSet utvecklingsmiljön för Azure mikrotjänster | Microsoft Docs"
description: "Installera hello runtime, verktyg och SDK och skapa ett kluster för lokal utveckling. När du har slutfört den här installationen kommer du att redo toobuild program."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a>Förbereda utvecklingsmiljön
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 toobuild och kör [Azure Service Fabric program] [ 1] på utvecklingsdatorn, installera hello runtime, SDK och verktyg. Du måste också tooenable körning av hello Windows PowerShell-skript som ingår i hello SDK.

## <a name="prerequisites"></a>Krav
### <a name="supported-operating-system-versions"></a>Operativsystemversioner som stöds
följande versioner av operativsystemet hello stöds för utveckling:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 innehåller bara Windows PowerShell 2.0 som standard. Service Fabric PowerShell-cmdletar kräver PowerShell 3.0 eller senare. Du kan [hämta Windows PowerShell 5.0] [ powershell5-download] från hello Microsoft Download Center.
> 
> 

## <a name="install-hello-sdk-and-tools"></a>Installera verktyg och hello SDK
### <a name="toouse-visual-studio-2017"></a>toouse 2017 för Visual Studio
Service Fabric-verktyg är en del av hello Azure utveckling och hantering av arbetsbelastningen i Visual Studio-2017. Aktivera den här arbetsbelastningen som en del av Visual Studio-installationen.
Du måste dessutom tooinstall hello Microsoft Azure Service Fabric SDK, med hjälp av Web Platform Installer.

* [Installera hello Microsoft Azure Service Fabric-SDK][core-sdk]

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>toouse Visual Studio 2015 (kräver Visual Studio 2015 Update 2 eller senare)
För Visual Studio 2015 installeras Service Fabric tillsammans med hello SDK, med hjälp av hello installationsprogram för webbplattform:

* [Installera verktyg och hello Microsoft Azure Service Fabric-SDK][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>SDK-installation endast
Om du bara behöver hello SDK kan du installera det här paketet:
* [Installera hello Microsoft Azure Service Fabric-SDK][core-sdk]

hello aktuella versioner är:
* Service Fabric SDK 2.7.198
* Service Fabric runtime 5.7.198
* Service Fabric-verktyg för Visual Studio 2015 1.7.50721
* Visual Studio 2017 Update 2 innehåller Service Fabric-verktyg för Visual Studio 1.6.20170504
* Visual Studio 2017 Update 3 förhandsversion 7 (15.3.0 förhandsversion 7.0) innehåller Service Fabric-verktyg för Visual Studio 1.7.20170721

En lista över versioner som stöds finns i [Service Fabric-stöd](service-fabric-support.md)

## <a name="enable-powershell-script-execution"></a>Aktivera körning av PowerShell-skript
Service Fabric använder Windows PowerShell-skript för att skapa ett lokalt utvecklingskluster och för att distribuera program från Visual Studio. Som standard blockerar Windows dessa skript så att de inte kan köras. tooenable dem måste du ändra PowerShell-körningsprincipen. Öppna PowerShell som administratör och ange hello följande kommando:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nästa steg
Nu när du har konfigurerat utvecklingsmiljön ska du börja bygga och köra appar.

* [Skapa ditt första Service Fabric-program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Lär dig hur toodeploy och hantera program på din lokala klustret](service-fabric-get-started-with-a-local-cluster.md)
* [Lär dig mer om hello programmeringsmodeller: Reliable Services och Reliable Actors](service-fabric-choose-framework.md)
* [Kolla in hello Service Fabric-kodexempel på GitHub](https://aka.ms/servicefabricsamples)
* [Visualisera ditt kluster med hjälp av Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Följ hello Service Fabric learning sökvägen tooget en bred introduktion toohello plattform](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-kampanjsida"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Länk till VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Länk till Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Länk till Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
