---
title: "aaaDeploy och uppgradera lokalt Azure mikrotjänster | Microsoft Docs"
description: "Lär dig hur tooset upp en lokal Service Fabric-kluster, distribuera ett befintligt program tooit och sedan uppgradera programmet."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Komma igång med att distribuera och uppgradera program i det lokala klustret
hello Azure Service Fabric SDK innehåller en fullständig lokal utvecklingsmiljö som du kan använda tooquickly komma igång med att distribuera och hantera program på lokala klustret. I den här artikeln, skapa ett lokalt kluster, distribuera ett befintligt program tooit och uppgradera programmet tooa nya versionen, allt från Windows PowerShell.

> [!NOTE]
> I den här artikeln förutsätter vi att du redan har [konfigurerat utvecklingsmiljön](service-fabric-get-started.md).
> 
> 

## <a name="create-a-local-cluster"></a>Skapa ett lokalt kluster
Ett Service Fabric-kluster representerar en uppsättning maskinvaruresurser som du kan distribuera program till. Vanligtvis består ett kluster av var som helst från fem toomany tusentals datorer. Hello Service Fabric SDK innehåller emellertid en klusterkonfiguration som kan köras på en enskild dator.

Det är viktigt toounderstand som hello lokala Service Fabric-kluster inte är en emulator eller simulatorn. Det körs hello samma plattformskod som finns på flera datorer kluster. hello enda skillnaden är att den körs hello plattform processer som vanligtvis fördelade på fem datorer på en dator.

hello SDK tillhandahåller två sätt tooset in ett lokala kluster: en Windows PowerShell-skript och hello Klusterhanterare för lokala system fack app. I den här kursen använder vi hello PowerShell-skript.

> [!NOTE]
> Om du redan har skapat ett lokalt kluster genom att distribuera ett program från Visual Studio kan du hoppa över det här avsnittet.
> 
> 

1. Starta ett nytt PowerShell-fönster som administratör.
2. Kör installationsskriptet för hello kluster från hello SDK-mappen:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    Klusterinstallationen tar en stund. När installationen är klar bör skärmen visa något som liknar detta:
   
    ![Utdata efter klusterinstallationen][cluster-setup-success]
   
    Du är nu redo tootry distribuera ett program tooyour kluster.

## <a name="deploy-an-application"></a>Distribuera ett program
hello Service Fabric SDK innehåller en omfattande uppsättning ramverk och utvecklare tooling för att skapa program. Om du vill veta hur toocreate program i Visual Studio, se [skapa ditt första Service Fabric-program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

I kursen får du använder en befintlig exempelprogrammet (kallas WordCount) så att du kan fokusera på hello management aspekter av hello plattform: distribution, övervakning och uppgradering.

1. Starta ett nytt PowerShell-fönster som administratör.
2. Importera hello Service Fabric SDK PowerShell-modulen.
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. Skapa en katalog toostore hello-program som du vill hämta och distribuera, till exempel C:\ServiceFabric.
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. [Hämta hello WordCount program](http://aka.ms/servicefabric-wordcountapp) toohello plats som du skapade.  Obs: hello Microsoft Edge-webbläsaren sparar hello-fil med en *.zip* tillägg.  Ändra hello filnamnstillägget för*.sfpkg*.
5. Anslut toohello lokala klustret:
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. Skapa ett nytt program kommandot på hello SDK-distribution med ett namn och en sökväg toohello programpaket.
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    Om allt går bra bör du se hello följande utdata:
   
    ![Distribuera ett program toohello lokala kluster][deploy-app-to-local-cluster]
7. toosee hello program i åtgärden, starta hello webbläsare och gå för[http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Du bör se:
   
    ![Distribuerat programgränssnitt][deployed-app-ui]
   
    Hej WordCount program är enkelt. Den omfattar klientens JavaScript-kod toogenerate slumpmässiga fem tecken ”ord”, som sedan vidarebefordras toohello program via ASP.NET Web API. En tillståndskänslig service spårar hello antalet ord räknas. De partitioneras baserat på hello första tecknet i hello word. Du kan hitta hello källkoden för hello WordCount-app i hello [klassiska komma igång exempel](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).
   
    hello-programmet som vi har distribuerats innehåller fyra partitioner. Så ord som inleds med ett via G lagras i hello första partitionen som ord som inleds med H till N lagras i andra hello-partition och så vidare.

## <a name="view-application-details-and-status"></a>Visa programinformation och programstatus
Nu när vi har distribuerat programmet hello ska vi titta på några av hello appinformation i PowerShell.

1. Fråga alla distribuerade program på hello klustret:
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    Förutsatt att du har bara distribuerat hello WordCount app kan se du något som liknar:
   
    ![Fråga alla distribuerade program i PowerShell][ps-getsfapp]
2. Gå toohello nästa nivå genom att fråga hello uppsättning tjänster som ingår i hello WordCount program.
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Visa lista över tjänster för programmet hello i PowerShell][ps-getsfsvc]
   
    hello program består av två tjänster och hello frontwebb hello tillståndskänslig tjänsten som hanterar hello ord.
3. Slutligen vill titta på hello lista över partitioner för WordCountService:
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Visa hello service partitioner i PowerShell][ps-getsfpartitions]
   
    Hej uppsättning kommandon som du använde som alla Service Fabric PowerShell-kommandon, är tillgängliga för ett kluster som du kan ansluta till lokal eller fjärransluten.
   
    För en mer visuell sätt toointeract med hello kluster, du kan använda hello Service Fabric Explorer Webbverktyg genom att navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer) i hello webbläsare.
   
    ![Visa programinformation i Service Fabric Explorer][sfx-service-overview]
   
   > [!NOTE]
   > toolearn mer om Service Fabric Explorer finns [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
   > 
   > 

## <a name="upgrade-an-application"></a>Uppgradera ett program
Service Fabric ger ingen avbrottstid uppgraderingar genom övervakning hello hälsotillståndet för programmet hello när den samlar över hello kluster. Utföra en uppgradering av hello WordCount-programmet.

hello ny version av hello programmet nu räknar ord som börjar med en vokal. Eftersom hello uppgraderingen implementerar, visas två ändringar i hello programmets beteende. Först bör hello hastighet som hello antalet växer långsammare, eftersom färre ord räknas. Andra, eftersom hello första partitionen har två vokal (A- och E) och alla andra partitioner endast innehålla en varje, antalet slutligen att starta toooutpace hello andra.

1. [Hämta hello WordCount version 2](http://aka.ms/servicefabric-wordcountappv2) toohello samma plats där du sparade hello version 1 paket.
2. Returnera tooyour PowerShell-fönster och Använd hello SDK kommandot uppgradera tooregister hello den nya versionen i hello klustret. Sedan börjar uppgradera hello fabric: / WordCount-programmet.
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    Du bör se hello följande utdata i PowerShell som hello uppgraderingen börjar.
   
    ![Uppgraderingsförlopp i PowerShell][ps-appupgradeprogress]
3. Medan du fortsätter hello uppgraderingen kan du enklare toomonitor dess status från Service Fabric Explorer. Starta ett webbläsarfönster och navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer). Expandera **program** i trädet för hello hello vänster Välj **WordCount**, och slutligen **fabric: / WordCount**. Hello essentials på fliken finns hello status hello uppgradering när det fortsätter via hello klustret uppgraderingsdomäner.
   
    ![Uppgraderingsförlopp i Service Fabric Explorer][sfx-upgradeprogress]
   
    Medan hello uppgraderingen utförs via varje domän, är hälsokontroller utförs tooensure som hello programmet fungerar korrekt.
4. Om du kör hello tidigare fråga efter hello uppsättning tjänster i hello fabric: / WordCount-programmet, Lägg märke till att hello WordCountService version har ändrats men hello WordCountWebService version inte:
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Fråga programtjänster efter uppgraderingen][ps-getsfsvc-postupgrade]
   
    I det här exemplet ser du hur Service Fabric hanterar programuppgraderingar. Den når endast hello uppsättning tjänster (eller konfigurations och kod paket i dessa tjänster) som har ändrats, vilket gör hello processen med att uppgradera snabbare och mer tillförlitlig.
5. Slutligen returneras toohello webbläsare tooobserve hello beteendet för hello ny programversion. Som förväntat, hello antal fortlöper långsammare och hello första partitionen slutar med lite mer hello volym.
   
    ![Visa hello ny version av programmet hello i hello webbläsare][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Rensa
Det är viktigt tooremember som hello lokala klustret är verkliga innan du omsluter. Programmen toorun i bakgrunden hello tills du tar bort dem.  Beroende på hello uppbyggnad dina appar, kan en app som körs ta upp viktiga resurser på din dator. Du har flera alternativ toomanage program och hello kluster:

1. tooremove ett enskilt program och alla den datorns informationen genom att köra följande kommando hello:
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    Ta bort programmet hello från hello Service Fabric Explorer **åtgärder** -menyn eller hello snabbmenyn i listvyn för hello vänstra program.
   
    ![Ta bort ett program i Service Fabric Explorer][sfe-delete-application]
2. Avregistrera version 1.0.0 och 2.0.0 av hello WordCount programtyp när du tar bort programmet hello från hello kluster. Ta bort tar bort hello programpaket, inklusive hello koden och konfigurationen från avbildningsarkivet hello klustret.
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    Eller välj i Service Fabric Explorer **avetablera typen** för hello program.
3. tooshut ned hello klustret men behåll hello programdata och spårning, klickar du på **stoppa lokala klustret** i hello system fack app.
4. toodelete hello klustret helt, klickar du på **ta bort lokala klustret** i hello system fack app. Det här alternativet resulterar i en annan långsam distribution hello nästa gång du trycka på F5 i Visual Studio. Ta bort hello lokala klustret bara om du inte tänker toouse den under en viss tid eller om du behöver tooreclaim resurser.

## <a name="one-node-and-five-node-cluster-mode"></a>Läge för kluster med en nod och fem noder
När du utvecklar program gör du ofta snabba iterationer när du skriver, felsöker och ändrar kod. toohelp optimera den här processen, hello lokala klustret kan köras i två lägen: en nod eller fem noder. Båda klusterlägena har sina fördelar. Läget för kluster med fem noder kan du toowork med en verklig kluster. Du kan testa redundansscenarier, samt arbeta med flera instanser och repliker av dina tjänster. Kluster med en nod läge är optimerad toodo snabb distribution och registrering av tjänster, toohelp du snabbt Validera kod med hello Service Fabric runtime.

Varken läget för kluster med en nod eller fem noder är en emulator eller simulator. hello lokal utveckling klustret körs hello samma plattformskod som finns på flera datorer kluster.

> [!WARNING]
> När du ändrar hello klustret läge hello aktuella klustret tas bort från systemet och ett nytt kluster skapas. hello-data som lagras i hello klustret tas bort när du ändrar läget för klustret.
> 
> 

toochange hello läge tooone kluster, Välj **växla klustret läge** i hello Klusterhanterare för Service Fabric lokalt.

![Växla klusterläge][switch-cluster-mode]

Eller ändra hello klustret läge med hjälp av PowerShell:

1. Starta ett nytt PowerShell-fönster som administratör.
2. Kör installationsskriptet för hello kluster från hello SDK-mappen:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Klusterinstallationen tar en stund. När installationen är klar bör skärmen visa något som liknar detta:
   
    ![Utdata efter klusterinstallationen][cluster-setup-success-1-node]

## <a name="next-steps"></a>Nästa steg
* Nu när du har distribuerat och uppgraderat vissa fördefinierade program kan du [skapa ett eget program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
* Alla hello-åtgärder som utförs på hello lokala klustret i den här artikeln kan utföras på en [Azure klustret](service-fabric-cluster-creation-via-portal.md) samt.
* Det gick grundläggande hello uppgradering som vi utförs i den här artikeln. Se hello [uppgraderingsinformationen](service-fabric-application-upgrade.md) toolearn mer om hello kraften och flexibiliteten hos Service Fabric-uppgraderingar.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
