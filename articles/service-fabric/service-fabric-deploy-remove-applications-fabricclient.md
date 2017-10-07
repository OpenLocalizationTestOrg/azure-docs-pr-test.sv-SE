---
title: aaaAzure Service Fabric-programdistribution | Microsoft Docs
description: "Använd hello FabricClient APIs toodeploy och ta bort program i Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Distribuera och ta bort program med hjälp av FabricClient
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient-API:er](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

När en [programtyp har paketerats][10], den är klar för distribution till Azure Service Fabric-kluster. Distributionen omfattar hello följande tre steg:

1. Överför hello programmet paketet toohello avbildningsarkivet
2. Registrera hello programtyp
3. Skapa hello programinstansen

När ett program distribueras och kör en instans i hello kluster kan du ta bort hello programinstansen och dess programtyp. toocompletely ta bort ett program från hello kluster omfattar hello följande steg:

1. Ta bort (eller ta bort) hello kör programinstansen
2. Avregistrera hello programtyp om du inte längre behöver
3. Ta bort hello programpaketet från hello avbildningsarkivet

Om du använder [Visual Studio för att distribuera och felsöka program](service-fabric-publish-app-remote-cluster.md) på klustret för lokal utveckling hello alla ovanstående steg hanteras automatiskt via ett PowerShell-skript.  Det här skriptet finns i hello *skript* för hello projektet. Den här artikeln innehåller bakgrund på vad skriptet gör så att du kan utföra samma åtgärder utanför Visual Studio hello. 
 
## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster
Ansluta toohello klustret genom att skapa en [FabricClient](/dotnet/api/system.fabric.fabricclient) instansen innan du kör någon av hello kodexempel i den här artikeln. Exempel på anslutande tooa lokal utveckling kluster eller ett kluster eller kluster som skyddas med Azure Active Directory, X509 certifikat eller Windows Active Directory finns [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). tooconnect toohello lokal utveckling klustret, kör hello följande:

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a>Överför hello programpaket
Anta att du skapar och paketerar ett program med namnet *MyApplication* i Visual Studio. Som standard är hello programmets typnamn som anges i hello ApplicationManifest.xml ”MyApplicationType”.  hello-programpaket som innehåller hello nödvändiga programmanifestet, service manifest och paket i kod-config-data, finns i *C:\Users\&lt; användarnamnet&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Ladda upp programpaket för hello placeras på en plats som kan nås av hello interna Service Fabric-komponenter. Service Fabric verifierar hello programpaketet under hello registrering av hello programpaket. Om du vill tooverify hello programpaket lokalt (d.v.s. innan du laddar upp) använda hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

Hej [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API överför hello paketet toohello klustret avbildningen appbutik. 

Om hello programpaket är stort och/eller har många filer, kan du [komprimera](service-fabric-package-apps.md#compress-a-package) och kopiera den toohello image store med hjälp av PowerShell. hello komprimeringen minskar hello storlek och hello antal filer.

Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.

## <a name="register-hello-application-package"></a>Registrera hello programpaket
hello programtypen och versionen som deklarerats i hello programmanifestet blir tillgängliga för användning när hello programpaketet har registrerats. hello system läser hello paketet på hello föregående steg, verifierar hello paketet, bearbetar hello paketets innehåll och kopierar hello bearbetas paketplatsen tooan internt system.  

Hej [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registren hello programtyp i hello klustret och gör den tillgänglig för distribution.

Hej [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API ger information om alla programtyper av som har registrerats. Du kan använda den här API-toodetermine när hello registreringen är klar.

## <a name="create-an-application-instance"></a>Skapa en instans av programmet
Du kan skapa en instans av ett program från alla program som har registrerats med hjälp av hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. hello-namnet för varje program måste börja med hello *”fabric”:* schemat och måste vara unikt för varje förekomst av programmet (inom ett kluster). Alla standardtjänster som definierats i hello programmanifestet av hello Målprogramstyp skapas också.

Flera instanser av programmet kan skapas för en viss version av typen registrerade programmet. Varje instans av programmet körs i isolering med sin egen arbetskatalog och processer.

toosee som heter program och tjänster som körs i hello klustret, kör hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) och [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API: er.

## <a name="create-a-service-instance"></a>Skapa en instans av tjänsten
Du kan skapa en instans av en tjänst från en typ med hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Om hello-tjänsten har deklarerats som en standardtjänst i programmanifestet hello instansieras hello-tjänsten när programmet hello instansieras.  Anropar hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API för en tjänst som redan har instansierats returneras ett undantag av typen FabricException som innehåller en felkod med värdet FabricErrorCode.ServiceAlreadyExists.

## <a name="remove-a-service-instance"></a>Ta bort en instans av tjänsten
När en instans av tjänsten är inte längre behövs, kan du ta bort den hello kör programinstansen genom att anropa hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Den här åtgärden kan inte ångras och tillståndet för tjänsten kan inte återställas.

## <a name="remove-an-application-instance"></a>Ta bort en instans av programmet
När en instans av programmet inte längre behövs, om du permanent ta bort den namn med hjälp av hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) tar automatiskt bort alla tjänster som tillhör toohello program samt, permanent tar du bort alla tillstånd för tjänsten.

> [!WARNING]
> Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.

## <a name="unregister-an-application-type"></a>Avregistrera en programtyp
När en viss version av en typ av program inte längre behövs, bör du avregistrera den viss versionen av hello programtyp med hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. Avregistrerar oanvända versioner av programmet typer versioner lagringsutrymme som används av hello image store. En version av en typ av program kan avregistreras så länge inga program instansieras mot den här versionen av hello programtyp och inga väntande programuppgraderingar refererar till den här versionen av hello programtyp.

## <a name="remove-an-application-package-from-hello-image-store"></a>Ta bort ett programpaket från hello avbildningsarkivet
När ett programpaket inte längre behövs, kan du ta bort den från hello image store toofree system-resurser med hjälp av hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="troubleshooting"></a>Felsökning
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiera ServiceFabricApplicationPackage begär en ImageStoreConnectionString
hello Service Fabric SDK miljö har redan hello rätt standardvärden ställa in. Men om det behövs, hello ImageStoreConnectionString för alla kommandon bör matcha hello-värde som hello Service Fabric-klustret. Du kan hitta hello ImageStoreConnectionString i hello klustermanifestet hämtades med hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) och Get-ImageStoreConnectionStringFromClusterManifest kommandon:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.  tooimport hello SDK modul, kör:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


Hej ImageStoreConnectionString hittades i klustermanifestet hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.

### <a name="deploy-large-application-package"></a>Distribuera stora programpaket
Problem: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API tiden av stora programpaket (ordning GB).
Försök:
- Ange en längre tidsgräns för [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) -metoden med `timeout` parameter. Som standard är hello timeout 30 minuter.
- Kontrollera hello nätverksanslutning mellan källdatorn och kluster. Om hello är långsam, Överväg att använda en dator med en bättre nätverksanslutning.
Om hello klientdatorn finns i en annan region än hello kluster, Överväg att använda en klientdator i en närmare eller samma region som hello kluster.
- Kontrollera om du träffa externa begränsning. Till exempel när hello avbildningsarkivet är konfigurerade toouse azure storage, kan överför att begränsas.

Problem: Överför paketet slutfördes, men [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API timeout. Försök:
- [Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.
hello komprimeringen minskar hello storlek och hello antal filer, vilket i sin tur minskar hello mängden trafik och fungerar som Service Fabric måste utföra. hello överföringen kan ta längre tid (särskilt om du inkluderar hello komprimering tid), men registrera och avregistrera hello programtyp är snabbare.
- Ange en längre tidsgräns för [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API med `timeout` parameter.

### <a name="deploy-application-package-with-many-files"></a>Distribuera programpaket med många filer
Problem: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) tidsgränsen uppnås för ett programpaket med många filer (efter tusentalsavgränsare).
Försök:
- [Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet. hello komprimeringen minskar hello antal filer.
- Ange en längre tidsgräns för [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) med `timeout` parameter.

## <a name="code-example"></a>Kodexempel
hello följande exempel kopierar ett paket toohello avbildningen programarkiv, etablerar hello programtyp, skapas en instans av programmet, skapar en tjänstinstans, tar bort hello programinstansen, avregistrera bestämmelser hello programtyp, och tar bort hello programpaketet från avbildningsarkivet hello.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create hello application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from hello application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision hello application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete hello application package from a location in hello image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Nästa steg
[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

[Service Fabric hälsa introduktion](service-fabric-health-introduction.md)

[Diagnostisera och felsöka en Service Fabric-tjänst](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellen är ett program i Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
