---
title: "Uppgradera en fristående Azure Service Fabric-kluster i Windows Server | Microsoft Docs"
description: "Uppgradera Azure Service Fabric-kod och/eller konfiguration som kör ett fristående Service Fabric-klustret, inklusive inställning uppdateringsläget klustret."
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
ms.openlocfilehash: ac40775ca62362a32184207857a0b965a798e135
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>Uppgradera din fristående Azure Service Fabric på Windows Server-kluster
> [!div class="op_single_selector"]
> * [Azure-kluster](service-fabric-cluster-upgrade.md)
> * [Fristående kluster](service-fabric-cluster-upgrade-windows-server.md)
>
>

För alla moderna system är möjligheten att uppgradera en nyckel till långsiktig framgång för en produkt. Ett Azure Service Fabric-kluster är en resurs som du äger. Den här artikeln beskriver hur du kan kontrollera att klustret alltid körs versioner av Service Fabric-kod och konfigurationer som stöds.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>Kontrollera Service Fabric-version som körs på klustret
Att ställa in klustret för att hämta uppdateringar av Service Fabric när Microsoft publicerar en ny version av **fabricClusterAutoupgradeEnabled** klusterkonfigurationen till true. En version av Service Fabric som du vill att klustret ska finnas på stöds kan ange den **fabricClusterAutoupgradeEnabled** klusterkonfigurationen till false.

> [!NOTE]
> Kontrollera att klustret alltid kör en Service Fabric-version som stöds. När Microsoft Beskriver utgivningen av en ny version av Service Fabric, markeras den tidigare versionen för slutet av stödet efter minst 60 dagar från datumet då meddelandet. Nya versioner tillkännages [på Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/). Den nya versionen finns att välja vid den punkten.
>
>

Du kan uppgradera klustret till den nya versionen endast om du använder en konfiguration av noden produktions-format, där varje Service Fabric-nod har allokerats på en separat fysisk eller virtuell dator. Om du har ett kluster för utveckling, där fler än en Service Fabric-nod är på en enda fysisk eller virtuell dator, måste du återskapa klustret med den nya versionen.

Två olika arbetsflöden kan uppgradera klustret till den senaste versionen eller en Service Fabric-version som stöds. Ett arbetsflöde är för kluster som har anslutning till den senaste versionen automatiskt. Andra arbetsflödet är för kluster som inte har anslutning till den senaste versionen Service Fabric.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Uppgradera kluster som har anslutning att hämta de senaste kod och konfiguration
Följ dessa steg för att uppgradera ditt kluster till en version som stöds om klusternoderna är anslutna till Internet att [http://download.microsoft.com](http://download.microsoft.com).

För kluster som har anslutning till [http://download.microsoft.com](http://download.microsoft.com), Microsoft regelbundet kontrollerar tillgängligheten för nya Service Fabric-versioner.

När en ny Service Fabric-version är tillgänglig, hämtas lokalt till klustret paketet och etableras för uppgradering. För att informera kunden om den här nya versionen visas dessutom systemet en explicit klustret hälsotillstånd varning som liknar följande:

”Den aktuella versionen [version #] klusterstöd slutar [Date]”.

När klustret kör den senaste versionen, försvinner varningen.

#### <a name="cluster-upgrade-workflow"></a>Uppgradera arbetsflödet för kluster
När du ser varningen för klustret hälsa gör du följande:

1. Ansluta till klustret från en dator som har administratörsåtkomst till alla datorer som listas som noder i klustret. Den dator som det här skriptet körs på behöver inte vara en del av klustret.

    ```powershell

    ###### connect to the secure cluster using certs
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

2. Hämta en lista över Service Fabric-versioner som du kan uppgradera till.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Du bör få ett utgående ut ungefär så här:

    ![Hämta fabric versioner][getfabversions]
3. Starta en klusteruppgradering till en version som är tillgängliga med hjälp av den [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Du kan använda Service Fabric-Utforskaren eller kör följande Windows PowerShell-kommando för att övervaka förloppet för uppgraderingen.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda. Ange anpassade hälsoprinciper för den **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

När du har åtgärdat problemen som resulterade i återställningen kan du initiera uppgraderingen igen genom att följa samma steg som beskrivits tidigare.

### <a name="upgrade-clusters-that-have-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Uppgradera kluster som har <U>ingen nätverksanslutning</u> att ladda ned de senaste kod och konfiguration
Följ dessa steg för att uppgradera ditt kluster till en version som stöds om klusternoderna inte har Internetanslutning till [http://download.microsoft.com](http://download.microsoft.com).

> [!NOTE]
> Om du använder ett kluster som inte är ansluten till Internet, behöver du övervaka Service Fabric-teamets blogg att lära dig om en ny version. Systemet visar inte ett kluster hälsotillstånd varning att varna dig för en ny version.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatisk etablering jämfört med manuella etablering
Om du vill aktivera automatisk överföring och registrering för den senaste versionen av koden, ställa in Service Fabric-uppdateringstjänsten. Referera till Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inuti den [fristående paketet](service-fabric-cluster-standalone-package-contents.md) anvisningar.
Följ anvisningarna nedan för manuell process.

Ändra klusterkonfigurationen för att ange följande egenskap till false innan du påbörjar en uppgradering av konfiguration.

        "fabricClusterAutoupgradeEnabled": false,

Referera till [Start ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) för användningsinformation. Se till att uppdatera clusterConfigurationVersion om du i din JSON innan du börjar uppgradera konfiguration.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Uppgradera arbetsflödet för kluster

1. Kör Get-ServiceFabricClusterUpgrade från en av noderna i klustret och notera TargetCodeVersion.
2. Kör följande från en internet-ansluten dator att visa en lista med alla uppgradera kompatibla versioner med den aktuella versionen och hämta motsvarande paketet från de associera länkarna.

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Ansluta till klustret från en dator som har administratörsåtkomst till alla datorer som listas som noder i klustret. Den dator som det här skriptet körs på behöver inte vara en del av klustret

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Kopiera det Hämta paketet till klustret image store.

5. Registrera kopierade paketet.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Starta en klusteruppgradering till en version som är tillgängliga.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Du kan övervaka förloppet för uppgraderingen på Service Fabric-Utforskaren eller genom att köra följande PowerShell-kommando.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda. Ange anpassade hälsoprinciper för den **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

När du har åtgärdat problemen som resulterade i återställningen kan du initiera uppgraderingen igen genom att följa samma steg som beskrivits tidigare.


## <a name="upgrade-the-cluster-configuration"></a>Uppgradera klusterkonfigurationen
Innan du startar uppgraderingen konfiguration kan du testa din nya kluster configuration json genom att köra PowerShell.skript i fristående paketet.

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
eller

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Vissa konfigurationer måste uppgraderas, till exempel slutpunkter, klusternamnet, noden IP osv. Detta kommer att testa nya kluster configuration json mot den gamla servern och utlösa fel i Powershell-fönstret om det inte finns några problem.

Om du vill uppgradera configuration klusteruppgradering kör **Start ServiceFabricClusterConfigurationUpgrade**. Konfiguration av uppgraderingen är bearbetade uppgraderingsdomän av uppgraderingsdomänen.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Klusteruppgradering certifikat config  
Klustret certifikatet används för autentisering mellan klusternoder, så certifikatet sammanslagning via ska utföras med extra försiktig eftersom fel blockerar kommunikation mellan klusternoder.  
Tekniskt sett stöds tre alternativ:  

1. Enskilt certifikat uppgraderingen: sökvägen för uppgraderingen är ' certifikat C (primär)-certifikat (primär) -> certifikatet B (primär) > ->... ”.   
2. Dubbla certifikat uppgraderingen: sökvägen för uppgraderingen är ' B (sekundär) -> certifikatet B (primär) och certifikat (primär) -> certifikat (primär) > certifikatet B (primär) och C (sekundär) -> certifikatet C (primär) ->... ”.
3. Av Typuppgradering av certifikat: tumavtryck-baserade certifikat <>--Vanligtnamn-baserade Certifikatkonfiguration. Till exempel certifikatets tumavtryck (primär) och tumavtrycket B (sekundär) -> certifikatet CommonName C.


## <a name="next-steps"></a>Nästa steg
* Lär dig hur du anpassar vissa [Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).
* Lär dig hur du [skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md).
* Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
