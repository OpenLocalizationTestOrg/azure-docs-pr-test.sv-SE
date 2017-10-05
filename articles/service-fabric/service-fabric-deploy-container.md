---
title: "Service Fabric och distribuera behållare | Microsoft Docs"
description: "Service Fabric och användning av behållare för att distribuera mikrotjänster program. Den här artikeln beskriver de funktioner som Service Fabric ger för behållare och hur du distribuerar en Windows-avbildning för behållare i ett kluster."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 25d6b056421e71fa70ed20a39589f77dbbc25c69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-windows-container-to-service-fabric"></a>Distribuera en Windows-behållare till Service Fabric
> [!div class="op_single_selector"]
> * [Distribuera Windows-behållare](service-fabric-deploy-container.md)
> * [Distribuera Dockerbehållare](service-fabric-deploy-container-linux.md)
> 
> 

Den här artikeln vägleder dig genom processen att bygga av tjänster i Windows-behållare.

Service Fabric har flera funktioner som hjälper dig med att skapa program som består av mikrotjänster som körs i behållare. 

Funktionerna inkluderar:

* Distribution av avbildningar för behållaren och aktivering
* Resurs-styrning
* Autentisering för databasen
* Portmappning för behållaren port-till-värd
* Identifiering av behållare till en annan och kommunikation
* Möjligheten att konfigurera och ange miljövariabler

Nu ska vi titta på hur var och en av funktionerna fungerar när du paketerar ett container service ska tas med i ditt program.

## <a name="package-a-windows-container"></a>Paketet en Windows-behållare
När du paketerar en behållare kan du använda antingen en projektmall för Visual Studio eller [skapa programpaketet manuellt](#manually).  När du använder Visual Studio skapas application paketet struktur och manifest-filer av mallen nytt projekt.

> [!TIP]
> Det enklaste sättet att paketera en befintlig behållare-avbildning till en tjänst är att använda Visual Studio.

## <a name="use-visual-studio-to-package-an-existing-container-image"></a>Använda Visual Studio för att paketera en befintlig avbildning i behållaren
Visual Studio har ett Service Fabric-tjänstmall som hjälper dig att distribuera en behållare till ett Service Fabric-kluster.

1. Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.
2. Välj **gäst behållaren** som tjänstmallen.
3. Välj **avbildningsnamn** och ange sökvägen till bilden i behållaren databasen. Till exempel `myrepo/myimage:v1` i https://hub.docker.com
4. Namnge tjänsten och klicka på **OK**.
5. Om av tjänsten måste en slutpunkt för kommunikation, du kan nu lägga till protokollet, porten och typ ServiceManifest.xml-filen. Exempel: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Genom att tillhandahålla den `UriScheme`, Service Fabric automatiskt registrerar behållaren slutpunkten med tjänsten Naming för synlighet. Porten kan vara antingen fasta (som visas i föregående exempel) eller dynamiskt allokerade. Om du inte anger en port är den dynamiskt allokerade från portintervall program (som skulle hända med alla service).
    Du måste också konfigurera värden portmappning behållaren genom att ange en `PortBinding` princip i programmanifestet. Mer information finns i [konfigurera behållare till värden portmappning](#Portsection).
6. Om din behållaren måste resursen styrning och lägger till en `ResourceGovernancePolicy`.
8. Om behållaren behöver autentiseras med en privat lagringsplats lägger du till `RepositoryCredentials`.
7. Om du kör på en Windows Server 2016-dator med aktiverat stöd för behållare, kan du använda paketet och publicera åtgärder för att distribuera till det lokala klustret. 
8. När du är klar kan du publicera program till ett kluster eller kontrollera i lösningen till källkontroll. 

Ett exempel checka ut den [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>Skapa ett kluster för Windows Server 2016
Om du vill distribuera av programmet, måste du skapa ett kluster som kör Windows Server 2016 med aktiverat stöd för behållaren. Klustret kan köras lokalt eller distribueras via Azure Resource Manager i Azure. 

Om du vill distribuera ett kluster med Azure Resource Manager, Välj den **Windows Server 2016 med behållare** bild-alternativet i Azure. Se artikeln [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Kontrollera att du använder följande Azure Resource Manager-inställningar:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Du kan också använda den [fem nod Azure Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) att skapa ett kluster. Du kan också läsa en grupp [blogginlägget](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) med hjälp av Service Fabric- och Windows-behållare.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Paketera och distribuera en avbildning för behållaren manuellt
Manuellt paketera container service baseras på följande:

1. Publicera behållarna i databasen.
2. Skapa katalogstrukturen paketet.
3. Redigera service manifest-filen.
4. Redigera programmanifestfilen.

## <a name="deploy-and-activate-a-container-image"></a>Distribuera och aktivera en avbildning av behållare
I Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras. Om du vill distribuera och aktiverar en behållare, placerar du namnet på behållaren avbildningen till en `ContainerHost` element i service manifest.

Tjänstmanifestet, lägga till en `ContainerHost` för startpunkten. Ange den `ImageName` namnet på behållaren databasen och image. Följande partiella manifestet visar ett exempel på hur du distribuerar behållare som kallas `myimage:v1` från en databas som heter `myrepo`:

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

Du kan ange valfri kommandon som ska köras vid start av behållare under den `Commands` element. För flera kommandon, komma-avgränsa dem. 

## <a name="understand-resource-governance"></a>Förstå resource-styrning
Resurs-styrning är en funktion på behållaren som begränsar de resurser som behållaren kan använda på värden. Den `ResourceGovernancePolicy`, som har angetts i applikationsmanifestet används för att deklarera gränserna för ett paket för service-kod. Gränserna kan anges för följande resurser:

* Minne
* MemorySwap
* CpuShares (CPU relativa viktade)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relativa vikt).

> [!NOTE]
> Stöd för att ange specifika blockera i/o-gränser som IOPs och läsning/skrivning BPS planeras för framtida versioner.
> 
> 

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a>Autentisera en databas
Du kan behöva ange inloggningsuppgifter i behållaren databasen om du vill hämta en behållare. Logga in autentiseringsuppgifter anges i programmanifestet, används för att ange inloggningsinformation eller SSH-nyckeln för att ladda ned avbildningen behållare från avbildningslagringsplatsen. I följande exempel visas ett konto som heter *TestUser* tillsammans med lösenordet i klartext (*inte* rekommenderas):

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

Vi rekommenderar att du krypterar lösenordet genom att använda ett certifikat som har distribuerats till datorn.

I följande exempel visas ett konto som heter *TestUser*, där lösenordet som har krypterats med ett certifikat som kallas *MyCert*. Du kan använda den `Invoke-ServiceFabricEncryptText` PowerShell-kommando för att skapa den hemliga chiffertext för lösenordet. Mer information finns i artikeln [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).

Den privata nyckeln för certifikatet som används för att dekryptera lösenordet måste distribueras till den lokala datorn i en out-of-band-metod. (Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar tjänstepaketet till datorn, kan det sedan dekryptera hemligheten. Med hjälp av hemlighet tillsammans med namnet på kontot kan den autentisera med behållaren lagringsplatsen.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name ="Portsection"></a>Konfigurera behållare för att värden portmappning
Du kan konfigurera en värd-port som används för att kommunicera med behållaren genom att ange en `PortBinding` i programmanifestet. Portbindningen mappar den port som tjänsten lyssnar i behållare till en port på värden.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a>Konfigurera identifiering av behållare till en annan och kommunikation
Du kan använda den `PortBinding` element att mappa en behållare-port till en slutpunkt i service manifest. I följande exempel slutpunkten `Endpoint1` och anger en fast port 8905. Det kan också ange ingen port, och då en slumpmässigt vald port från klustrets programmet portintervallet är valt.


```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```
Om du anger en slutpunkt med hjälp av den `Endpoint` tagg i tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten till Naming service. Andra tjänster som körs i klustret kan därför att identifiera den här behållaren med hjälp av REST-frågor för att lösa.

Genom att registrera med Naming service kan du utföra behållare till en annan kommunikation i en behållare med den [omvänd proxy](service-fabric-reverseproxy.md). Kommunikation utförs genom att tillhandahålla omvänd proxy http-lyssningsporten och namnet på de tjänster som du vill kommunicera med som miljövariabler. Mer information finns i nästa avsnitt. 

## <a name="configure-and-set-environment-variables"></a>Konfigurera och ange miljövariabler
Miljövariabler kan anges för varje kodpaketet i tjänstmanifestet. Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster. Du kan åsidosätta värden för miljövariabler i applikationsmanifestet eller ange dem under distributionen som programparametrar.

Följande XML-kodfragment i tjänstmanifestet visar ett exempel på hur du anger miljövariabler för ett kodpaket:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

De här miljövariablerna kan åsidosättas på programnivå manifest:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

I föregående exempel vi angav ett explicit värde för den `HttpGateway` miljövariabeln (19000), medan vi anger du värdet för `BackendServiceName` parametern via den `[BackendSvc]` program-parametern. De här inställningarna kan du ange värdet för `BackendServiceName`värde när du distribuerar programmet och inte har ett fast värde i manifestet.

## <a name="configure-isolation-mode"></a>Konfigurera isoleringsläge

Windows stöder två arbetslägen för behållare - processen och Hyper-V.  Om processisoleringsläget används delar alla behållare som körs på samma värddator kärna med värden. Om Hyper-V-isoleringsläget används isoleras kärnorna mellan varje Hyper-V-behållare och behållarvärden. Isoleringsläge har angetts i den `ContainerHostPolicies` tagg i programmanifestfilen.  Isoleringslägena som kan anges är `process`, `hyperv` och `default`. Den `default` isoleringsläge som standard `process` på Windows Server-värdar och standardvärdet är `hyperv` på Windows 10-värdar.  Följande kodfragment visar hur isoleringsläget har angetts i applikationsmanifestfilen.

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a>Slutföra exempel för programmet och tjänstmanifestet

Ett exempel programmanifest följande:

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

Ett exempel tjänstmanifestet (anges i föregående programmanifestet) visas nedan:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a>Nästa steg
Nu när du har använt ett container service lär du dig hur du hanterar livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).

* [Översikt över Service Fabric och behållare](service-fabric-containers-overview.md)
* Ett exempel utcheckningen [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
