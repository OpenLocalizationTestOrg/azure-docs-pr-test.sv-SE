---
title: aaaApplication livscykel i Service Fabric | Microsoft Docs
description: "Beskriver utveckla, distribuera, testa, uppgradera, underhålla och tar bort Service Fabric-program."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 08837cca-5aa7-40da-b087-2b657224a097
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/14/2017
ms.author: ryanwi
ms.openlocfilehash: 36cd6081010e83cb8226c8f85d1e912ac9eebd00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-lifecycle"></a>Livscykel för Service Fabric-program
Som med andra plattformar, ett program på Azure Service Fabric vanligtvis passerar hello följande faser: design, utveckling, testning, distribution, uppgradering, underhåll och borttagning. Service Fabric har förstklassigt stöd för hello fullständiga programmet livscykeln för molnprogram från utveckling till distribution, dagliga hantering och underhåll tooeventual inaktivering. hello tjänstmodell kan flera olika roller tooparticipate oberoende i hello programmet livscykel. Den här artikeln innehåller en översikt över hello API: er och hur de används av hello olika roller i hello faserna i livscykeln för hello Service Fabric-programmet.

hello följande Microsoft Virtual Academy videon beskriver hur toomanage livscykeln för programmet:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-application-lifecycle/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="service-model-roles"></a>Service model-roller
hello service model-rollerna är:

* **Tjänsten developer**: utvecklar modulära och allmänna tjänster som kan nytt säker och används i flera program för hello samma typ eller olika typer. Till exempel kan en kö-tjänst användas för att skapa ett loggnings-program (supportavdelning) eller ett program för e-handel (kundvagn).
* **Programutvecklaren**: skapar program genom att integrera en mängd tjänster toosatisfy vissa särskilda krav eller scenarier. Till exempel webbplatser för e-handel kan integrera ”JSON tillståndslös frontend-tjänst”, ”auktionen Stateful tjänst” och ”Stateful kötjänsten” toobuild en auctioning lösning.
* **Programadministratör**: gör beslut om hello programkonfigurationen (fylla i hello konfigurationsparametrar mall), distribution (mappa tooavailable resurser) och tjänsternas kvalitet. Till exempel beslutar administratör programmet hello språk (på engelska för hello USA eller japanska för Japan, till exempel) av programmet hello. Ett annat distribuerat program kan ha olika inställningar.
* **Operatorn**: distribuerar programmen baserat på hello programkonfigurationen och krav som anges av hello programadministratör. Till exempel en operator etablerar och distribuerar programmet hello och ser till att den körs i Azure. Operatörer övervaka hälsotillstånd och prestanda programinformation och underhålla hello fysisk infrastruktur efter behov.

## <a name="develop"></a>Utveckla
1. En *tjänsten developer* utvecklar olika typer av tjänster med hjälp av hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) eller [Reliable Services](service-fabric-reliable-services-introduction.md) programmeringsmodell.
2. En *tjänsten developer* deklarativt beskriver hello utvecklats tjänsttyper i en manifestfil på tjänsten som består av ett eller flera paket för koden, konfiguration och data.
3. En *programutvecklaren* skapar ett program med hjälp av olika typer.
4. En *programutvecklaren* deklarativt beskriver hello programtyp i ett programmanifest genom att referera till hello service manifest hello dessa tjänster och korrekt åsidosätter och Parameterisera konfiguration och distribution av olika inställningar av hello dessa tjänster.

Se [Kom igång med Reliable Actors](service-fabric-reliable-actors-get-started.md) och [komma igång med tillförlitlig Services](service-fabric-reliable-services-quick-start.md) exempel.

## <a name="deploy"></a>Distribuera
1. En *programadministratör* anpassar hello programmet typen tooa specifika program distribueras toobe tooa Service Fabric-kluster genom att ange hello lämpliga parametrar för hello **ApplicationType**element i hello programmanifestet.
2. En *operatorn* överföringar hello avbildningsarkivet för programmet paketet toohello kluster med hjälp av hello [ **CopyApplicationPackage** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) eller hello [  **Kopiera ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). hello programpaketet innehåller hello programmanifestet och servicepaket hello samling. Service Fabric distribuerar program från hello-programpaket som lagras i hello image store, som kan vara ett Azure blobstore eller hello Service Fabric-systemtjänsten.
3. Hej *operatorn* etablerar hello programtyp i hello målkluster från hello överförda applikationspaket med hjälp av hello [ **ProvisionApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), hello [ **registrera ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), eller hello [ **etablera ett program** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. När du har etablerat hello program, en *operatorn* startar hello programmet hello parametrar som tillhandahålls av hello *programadministratör* med hello [  **CreateApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CreateApplicationAsync_System_Fabric_Description_ApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [ **ny ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricapplication), eller hello [ **skapa Programmet** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. När programmet hello har distribuerats, en *operatorn* använder hello [ **CreateServiceAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_CreateServiceAsync_System_Fabric_Description_ServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Nya ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice), eller hello [ **skapa tjänst** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) toocreate nya instanser av tjänsten för hello program baserat på tillgängliga typer.
6. hello programmet körs nu i hello Service Fabric-klustret.

Se [distribuera ett program](service-fabric-deploy-remove-applications.md) exempel.

## <a name="test"></a>Testa
1. När du har distribuerat toohello lokal utveckling kluster eller ett testkluster en *tjänsten developer* körs hello inbyggd redundans Testscenario med hjälp av hello [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters#System_Fabric_Testability_Scenario_FailoverTestScenarioParameters) och [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario#System_Fabric_Testability_Scenario_FailoverTestScenario) klasser eller hello [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet ](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). hello redundans Testscenario kör en angiven tjänst via viktiga övergångar och växling vid fel tooensure att det är fortfarande tillgängliga och fungera.
2. Hej *tjänsten developer* och sedan kör hello inbyggda chaos test-scenario med hello [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters#System_Fabric_Testability_Scenario_ChaosTestScenarioParameters) och [  **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario#System_Fabric_Testability_Scenario_ChaosTestScenario) klasser eller hello [ **Invoke-ServiceFabricChaosTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). hello chaos Testscenario startar slumpmässigt flera nod kodpaketet och repliken fel i hello kluster.
3. Hej *tjänsten developer* [testar service-to-service-kommunikation](service-fabric-testability-scenarios-service-communication.md) genom att skriva testscenarier som flyttar primära repliker runt hello klustret.

Se [introduktion toohello fel Analysis Service](service-fabric-testability-overview.md) för mer information.

## <a name="upgrade"></a>Uppgradera
1. En *tjänsten developer* uppdaterar hello ingående services av programmet hello instansieras och/eller åtgärdar buggar och innehåller en ny version av hello tjänstmanifestet.
2. En *programutvecklaren* åsidosätter parameterizes hello konfiguration och distribution inställningar av hello konsekvent tjänster och innehåller en ny version av hello programmanifestet. hello programutvecklaren sedan införlivar hello nya versioner av hello service manifest i hello program och ger en ny version av hello programtyp i programpaket uppdaterade.
3. En *programadministratör* införlivar hello ny version av hello programtyp i hello målprogrammet genom att uppdatera hello lämpliga parametrar.
4. En *operatorn* överföringar hello uppdaterade programmet paketet toohello klustret image store med hjälp av hello [ **CopyApplicationPackage** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) eller hello [ **Kopiera ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). hello programpaketet innehåller hello programmanifestet och servicepaket hello samling.
5. En *operatorn* bestämmelser hello ny version av programmet hello i hello målkluster med hjälp av hello [ **ProvisionApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Registrera ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), eller hello [ **etablera ett program** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. En *operatorn* uppgraderingar hello programmet toohello nya målversionen med hello [ **UpgradeApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpgradeApplicationAsync_System_Fabric_Description_ApplicationUpgradeDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Start-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationupgrade), eller hello [ **uppgradera ett program** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. En *operatorn* kontrollerar hello förloppet för uppgradera med hjälp av hello [ **GetApplicationUpgradeProgressAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_GetApplicationUpgradeProgressAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Get-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplicationupgrade), eller hello [ **hämta programmet uppgradering pågår** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. Om det behövs hello *operatorn* ändrar och återställer hello parametrarna för hello aktuella uppgradering av programmet med hjälp av hello [ **UpdateApplicationUpgradeAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpdateApplicationUpgradeAsync_System_Fabric_Description_ApplicationUpgradeUpdateDescription_System_TimeSpan_System_Threading_CancellationToken_), Hej [ **uppdatering ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/update-servicefabricapplicationupgrade), eller hello [ **uppdatering programmet uppgradera** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. Om det behövs hello *operatorn* återställer hello aktuella uppgradering av programmet med hjälp av hello [ **RollbackApplicationUpgradeAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RollbackApplicationUpgradeAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Start ServiceFabricApplicationRollback** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationrollback), eller hello [ **återställning programmet uppgradera** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric uppgraderingar hello målprogrammet körs i hello klustret utan att förlora hello tillgänglighet för någon av dessa tjänster.

Se hello [uppgradera självstudien](service-fabric-application-upgrade-tutorial.md) exempel.

## <a name="maintain"></a>Underhåll
1. För uppgradering av operativsystemet och korrigeringar, Service Fabric-gränssnitt med hello Azure-infrastrukturen tooguarantee tillgängligheten för alla hello-program som körs i hello kluster.
2. För uppgraderingar och korrigeringar toohello Service Fabric-plattformen uppgraderar Service Fabric själva utan att förlora tillgänglighet för någon av hello-program som körs på hello klustret.
3. En *programadministratör* godkänner hello tillägg eller borttagning av noder från ett kluster när du har analyserat historiska kapacitet användningsdata och planerade framtida behov.
4. En *operatorn* lägger till och tar bort noder som anges av hello *programadministratör*.
5. När nya noder läggs tooor befintliga noderna tas bort från hello kluster, Service Fabric automatiskt belastningsutjämnar hello program som körs på alla noder i klustret hello tooachieve optimalt.

## <a name="remove"></a>Ta bort
1. En *operatorn* kan ta bort en specifik instans av en tjänst som körs i hello klustret utan att ta bort hello hela program med hjälp av hello [ **DeleteServiceAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_DeleteServiceAsync_System_Fabric_Description_DeleteServiceDescription_System_TimeSpan_System_Threading_CancellationToken_) , hello [ **ta bort ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricservice), eller hello [ **ta bort tjänst** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. En *operatorn* kan också ta bort en instans av programmet och alla dess tjänster med hjälp av hello [ **DeleteApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_DeleteApplicationAsync_System_Fabric_Description_DeleteApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Ta bort ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricapplication), eller hello [ **ta bort programmet** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. När hello program och tjänster har stoppats kan hello *operatorn* kan avetablera hello programtyp med hello [ **UnprovisionApplicationAsync** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UnprovisionApplicationAsync_System_String_System_String_System_TimeSpan_System_Threading_CancellationToken_), Hej [ **Unregister-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype), eller hello [ **avetablera ett program** REST-åtgärden](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Avetableras hello programtyp tas inte bort hello programpaketet från hello Avbildningsarkiv. Du måste manuellt ta bort hello programpaket.
4. En *operatorn* tar bort hello programpaketet från hello Avbildningsarkiv med hello [ **RemoveApplicationPackage** metoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RemoveApplicationPackage_System_String_System_String_) eller hello [ **Ta bort ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

Se [distribuera ett program](service-fabric-deploy-remove-applications.md) exempel.

## <a name="next-steps"></a>Nästa steg
Mer information om hur du utvecklar Se testning och hantera Service Fabric-program och tjänster:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Distribuera ett program](service-fabric-deploy-remove-applications.md)
* [Uppgradering av programmet](service-fabric-application-upgrade.md)
* [Möjlighet att testa översikt](service-fabric-testability-overview.md)
