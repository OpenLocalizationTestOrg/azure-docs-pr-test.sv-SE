---
title: "aaaUpgrade som en fristående Azure Service Fabric-kluster i Windows Server | Microsoft Docs"
description: "Uppgradera hello Azure Service Fabric-kod och/eller konfiguration som kör ett fristående Service Fabric-klustret, inklusive inställning hello uppdateringsläge för klustret."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 5132795e544b6f0185accedbf5092dcaafd66df0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>Uppgradera din fristående Azure Service Fabric på Windows Server-kluster
> [!div class="op_single_selector"]
> * [Azure-kluster](service-fabric-cluster-upgrade.md)
> * [Fristående kluster](service-fabric-cluster-upgrade-windows-server.md)
>
>

För alla moderna system är hello möjlighet tooupgrade viktiga toohello långsiktig framgång med produkten. Ett Azure Service Fabric-kluster är en resurs som du äger. Den här artikeln beskriver hur du kan kontrollera hello klustret körs alltid versioner av Service Fabric-kod och konfigurationer som stöds.

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a>Kontrollen hello Service Fabric-version som körs på klustret
tooset kluster-toodownload uppdateringar av Service Fabric när Microsoft publicerar en ny version, ange hello **fabricClusterAutoupgradeEnabled** kluster configuration tootrue. tooselect en stödd version av Service Fabric som du vill att ditt kluster toobe på uppsättningen hello **fabricClusterAutoupgradeEnabled** kluster configuration toofalse.

> [!NOTE]
> Kontrollera att klustret alltid kör en Service Fabric-version som stöds. När Microsoft tillkännager hello-versionen av en ny version av Service Fabric, har hello tidigare version markerats för slutet av stödet efter minst 60 dagar från hello hello-meddelande. Nya versioner tillkännages [på hello Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/). hello ny version är tillgänglig toochoose vid den punkten.
>
>

Du kan uppgradera den nya versionen för kluster-toohello endast om du använder en konfiguration av noden produktions-format, där varje Service Fabric-nod har allokerats på en separat fysisk eller virtuell dator. Om du har ett kluster för utveckling, där fler än en Service Fabric-nod är på en enda fysisk eller virtuell dator, måste du återskapa hello kluster med hello ny version.

Två olika arbetsflöden kan uppgradera din klustret toohello senaste version eller en Service Fabric-version som stöds. Ett arbetsflöde är för kluster som har anslutning toodownload hello senaste versionen automatiskt. hello andra arbetsflöde är för kluster som inte har anslutning toodownload hello senaste Service Fabric versionen.

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a>Uppgradera kluster som har anslutning toodownload hello senaste kod och konfiguration
Använd dessa steg tooupgrade ditt kluster tooa som stöds av version om klusternoderna är anslutna till Internet för[http://download.microsoft.com](http://download.microsoft.com).

För kluster som har anslutning för[http://download.microsoft.com](http://download.microsoft.com), Microsoft söker regelbundet efter hello tillgängligheten för nya Service Fabric-versioner.

När en ny Service Fabric-version är tillgänglig hello paketet hämtas lokalt toohello klustret och etableras för uppgradering. Dessutom tooinform hello kunden för den här nya versionen, hello system visar en varning om hälsa explicit kluster som är liknande toohello följande:

”Hej aktuella versionen [version #] klusterstöd slutar [Date]”.

När hello klustret kör hello senaste versionen, försvinner hello varningen.

#### <a name="cluster-upgrade-workflow"></a>Uppgradera arbetsflödet för kluster
Efter att du ser hello klustret hälsotillstånd varning hello följande:

1. Ansluta toohello kluster från en dator som har administratören åtkomst tooall hello datorer som listas som noder i klustret hello. hello-dator som det här skriptet körs på har inte toobe tillhör hello kluster.

    ```powershell

    ###### connect toohello secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Hämta hello lista över Service Fabric-versioner som du kan uppgradera till.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Du bör få en liknande toothis utdata:

    ![Hämta fabric versioner][getfabversions]
3. Starta en klustret uppgradera tooan tillgängliga versionen med hjälp av den [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   toomonitor hello utvecklingen av hello uppgraderingen, som du kan använda Service Fabric-Utforskaren eller kör hello följande Windows PowerShell-kommando.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. toospecify anpassade hälsoprinciper för hello **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

När du har åtgärdat hello problem som resulterade i hello återställning initiera hello uppgraderingen igen med följande hello likadant enligt beskrivningen ovan.

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a>Uppgradera kluster som har <U>ingen nätverksanslutning</u> toodownload hello senaste kod och konfiguration
Använda dessa steg tooupgrade ditt kluster tooa som stöds av version om klusternoderna inte har Internetanslutning för[http://download.microsoft.com](http://download.microsoft.com).

> [!NOTE]
> Om du kör ett kluster som inte är anslutna toohello Internet, har du toomonitor hello Service Fabric team-blogg toolearn om en ny version. hello system visar inte ett kluster hälsa varning tooalert du en ny version.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatisk etablering jämfört med manuella etablering
Konfigurera Service Fabric-uppdateringstjänsten tooenable automatisk hämtning och registrering för hello senaste code-versionen. Se toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inuti hello [fristående paketet](service-fabric-cluster-standalone-package-contents.md) anvisningar.
Följ hello anvisningarna nedan för manuell process.

Ändra ditt kluster configuration tooset hello efter egenskapen toofalse innan du påbörjar en uppgradering av configuration.

        "fabricClusterAutoupgradeEnabled": false,

Se för[Start ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) för användningsinformation. Gör att tooupdate 'clusterConfigurationVersion' i din JSON innan du börjar hello configuration uppgraderingen.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a>Uppgradera arbetsflödet för kluster

1. Kör Get-ServiceFabricClusterUpgrade från en hello noder i klustret hello och notera hello TargetCodeVersion.
2. Kör hello följande från en internet-anslutna datorn toolist alla uppgradera kompatibla versioner med nuvarande version av hello och hämta hello motsvarande paket från hello associerade länkar.

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Ansluta toohello kluster från en dator som har administratören åtkomst tooall hello datorer som listas som noder i klustret hello. hello-dator som det här skriptet körs på inte har toobe tillhör hello kluster

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Kopiera hello hämtade paketet till hello avbildningsarkivet för klustret.

5. Registrera hello kopierade paketet.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Starta en klustret uppgradera tooan tillgängliga versionen.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Du kan övervaka hello hello uppgraderingen på Service Fabric-Utforskaren eller köra hello följande PowerShell-kommando.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. toospecify anpassade hälsoprinciper för hello **Start ServiceFabricClusterUpgrade** finns i dokumentationen hello [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

När du har åtgärdat hello problem som resulterade i hello återställning initiera hello uppgraderingen igen med följande hello likadant enligt beskrivningen ovan.


## <a name="upgrade-hello-cluster-configuration"></a>Uppgradera hello klusterkonfigurationen
Innan du startar hello configuration uppgradering kan du testa din nya kluster configuration json genom att köra hello powershell-skript i hello fristående paketet.

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
eller

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

Vissa konfigurationer måste uppgraderas, till exempel slutpunkter, klusternamnet, noden IP osv. Detta testar hello nya kluster configuration json mot hello gamla och utlösa fel i hello Powershell-fönstret om det inte finns några problem.

tooupgrade hello konfiguration av klusteruppgradering, kör **Start ServiceFabricClusterConfigurationUpgrade**. hello configuration uppgradering är bearbetade uppgraderingsdomän av uppgraderingsdomänen.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Klusteruppgradering certifikat config  
Klustret certifikat används för autentisering mellan klusternoder, så hello certifikat rulla över ska utföras med extra försiktig eftersom fel blockerar hello kommunikation mellan klusternoder.  
Tekniskt sett stöds tre alternativ:  

1. Enskilt certifikat uppgraderingen: hello uppgraderingsväg är ' certifikat C (primär)-certifikat (primär) -> certifikatet B (primär) > ->... ”.   
2. Dubbla certifikat uppgraderingen: hello uppgraderingsväg är ' B (sekundär) -> certifikatet B (primär) och certifikat (primär) -> certifikat (primär) > certifikatet B (primär) och C (sekundär) -> certifikatet C (primär) ->... ”.
3. Av Typuppgradering av certifikat: tumavtryck-baserade certifikat <>--Vanligtnamn-baserade Certifikatkonfiguration. Till exempel certifikatets tumavtryck (primär) och tumavtrycket B (sekundär) -> certifikatet CommonName C.


## <a name="next-steps"></a>Nästa steg
* Lär dig hur toocustomize vissa [Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).
* Lär dig hur för[skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md).
* Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
