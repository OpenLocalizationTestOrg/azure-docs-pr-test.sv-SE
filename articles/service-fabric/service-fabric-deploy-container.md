---
title: "aaaService infrastrukturen och distribuera behållare | Microsoft Docs"
description: "Service Fabric och hello användning av behållare toodeploy mikrotjänster program. Den här artikeln beskriver hello-funktioner med Service Fabric för behållare och hur toodeploy en behållare för Windows image i ett kluster."
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
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a>Distribuera en Windows-behållaren tooService Fabric
> [!div class="op_single_selector"]
> * [Distribuera Windows-behållare](service-fabric-deploy-container.md)
> * [Distribuera Dockerbehållare](service-fabric-deploy-container-linux.md)
> 
> 

Den här artikeln vägleder dig genom hello processen att bygga av tjänster i Windows-behållare.

Service Fabric har flera funktioner som hjälper dig med att skapa program som består av mikrotjänster som körs i behållare. 

hello funktioner omfattar:

* Distribution av avbildningar för behållaren och aktivering
* Resurs-styrning
* Autentisering för databasen
* Portmappning för behållaren port-till-värd
* Identifiering av behållare till en annan och kommunikation
* Möjlighet tooconfigure och ange miljövariabler

Nu ska vi titta på hur var och en av funktionerna fungerar när du paketerar ett container service-toobe som ingår i ditt program.

## <a name="package-a-windows-container"></a>Paketet en Windows-behållare
När du paketerar en behållare kan du välja toouse antingen en projektmall för Visual Studio eller [skapa hello programpaket manuellt](#manually).  När du använder Visual Studio hello programmet paketet struktur och manifest-filer skapas av hello nytt projekt mallen.

> [!TIP]
> hello enklaste sättet toopackage en befintlig behållare avbildning till en tjänst är toouse Visual Studio.

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a>Använd Visual Studio toopackage en befintlig avbildning i behållaren
Visual Studio har ett Service Fabric service mallen toohelp du distribuerar en behållare tooa Service Fabric-klustret.

1. Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.
2. Välj **gäst behållaren** som mall för hello-tjänster.
3. Välj **avbildningsnamn** och ange hello sökvägen toohello bilden i lagringsplatsen för behållaren. Till exempel `myrepo/myimage:v1` i https://hub.docker.com
4. Namnge tjänsten och klicka på **OK**.
5. Om av tjänsten måste en slutpunkt för kommunikation, kan du nu lägga till hello-protokollet, porten och typen toohello ServiceManifest.xml fil. Exempel: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Genom att tillhandahålla hello `UriScheme`, Service Fabric automatiskt registrerar hello behållaren slutpunkt med hello Naming service för synlighet. hello porten kan vara fast (som visas i föregående exempel hello) eller dynamiskt allokerade. Om du inte anger en port är den dynamiskt allokerade från hello programmet portintervall (som skulle hända med alla service).
    Du måste också tooconfigure hello behållaren toohost portmappning genom att ange en `PortBinding` princip i hello programmanifestet. Mer information finns i [konfigurera behållaren toohost portmappning](#Portsection).
6. Om din behållaren måste resursen styrning och lägger till en `ResourceGovernancePolicy`.
8. Om din behållaren måste tooauthenticate med en privat databas, Lägg sedan till `RepositoryCredentials`.
7. Om du kör på en Windows Server 2016-dator med aktiverat stöd för behållare, kan du använda hello paketet och publicera åtgärd toodeploy tooyour lokala klustret. 
8. När du är klar kan du publicera programmet hello tooa fjärrkluster eller kontrollera hello lösning toosource kontrollen. 

Ett exempel utcheckningen hello [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>Skapa ett kluster för Windows Server 2016
toodeploy av programmet, du behöver toocreate aktiverad i ett kluster som kör Windows Server 2016 med stöd för behållaren. Klustret kan köras lokalt eller distribueras via Azure Resource Manager i Azure. 

toodeploy ett kluster med Azure Resource Manager, Välj hello **Windows Server 2016 med behållare** bild-alternativet i Azure. Se artikeln hello [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Kontrollera att du använder hello följande Azure Resource Manager-inställningar:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Du kan också använda hello [fem nod Azure Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate ett kluster. Du kan också läsa en grupp [blogginlägget](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) med hjälp av Service Fabric- och Windows-behållare.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Paketera och distribuera en avbildning för behållaren manuellt
hello processen manuellt paketera container service baseras på hello följande steg:

1. Publicera hello behållare tooyour databasen.
2. Skapa hello paketet katalogstruktur.
3. Redigera hello service manifest-filen.
4. Redigera hello programmanifestfilen.

## <a name="deploy-and-activate-a-container-image"></a>Distribuera och aktivera en avbildning av behållare
I hello Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras. toodeploy och aktivera en behållare, placera hello namnet på hello behållaren bilden i en `ContainerHost` element i hello service manifest.

Hello tjänstmanifestet, lägga till en `ContainerHost` för hello startpunkt. Därefter uppsättning hello `ImageName` toobe hello namnet på hello behållaren databasen och image. hello följande partiella manifestet visar ett exempel på hur toodeploy hello behållaren kallas `myimage:v1` från en databas som heter `myrepo`:

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

Du kan ange ytterligare kommandon toorun vid start av hello behållare under hello `Commands` element. För flera kommandon, komma-avgränsa dem. 

## <a name="understand-resource-governance"></a>Förstå resource-styrning
Resurs-styrning är en funktion för hello-behållare som begränsar hello resurser som hello behållare kan använda på hello värden. Hej `ResourceGovernancePolicy`, som har angetts i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod. Resursen gränser kan anges för hello följande resurser:

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
Du kan ha tooprovide inloggningsuppgifter toohello behållare databasen toodownload en behållare. hello inloggningsuppgifter, anges i hello programmanifestet är används toospecify hello inloggningsinformation eller SSH-nyckeln för att ladda ned hello behållaren avbildningen från hello avbildningslagringsplatsen. hello följande exempel visas ett konto som heter *TestUser* tillsammans med hello lösenord i klartext (*inte* rekommenderas):

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

Vi rekommenderar att du krypterar hello lösenord med hjälp av ett certifikat som har distribuerats toohello datorn.

hello följande exempel visas ett konto som heter *TestUser*, där hello lösenord har krypterats med ett certifikat som kallas *MyCert*. Du kan använda hello `Invoke-ServiceFabricEncryptText` PowerShell-kommandot toocreate hello hemliga chiffertext hello lösenord. Mer information finns i artikeln hello [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).

hello privat nyckel för hello-certifikat som har använt toodecrypt hello lösenord måste vara distribuerad toohello lokal dator i en out-of-band-metod. (Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar hello paketet toohello maskin, kan den sedan dekryptera hello hemlighet. Med hjälp av hello hemlighet tillsammans med hello kontonamn kan den autentisera med hello behållaren databasen.

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

## <a name ="Portsection"></a>Konfigurera mappning för behållaren toohost port
Du kan konfigurera en toocommunicate för porten som används av värden med hello behållare genom att ange en `PortBinding` i hello programmanifestet. hello port bindning maps hello port toowhich hello tjänsten lyssnar inuti hello behållaren tooa port på hello värden.

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
Du kan använda hello `PortBinding` elementet toomap en behållare port tooan slutpunkt i hello service manifest. I följande exempel hello, hello endpoint `Endpoint1` och anger en fast port 8905. Det kan också ange ingen port, och då en slumpmässigt vald port från portintervall för hello kluster programmet är valt.


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
Om du anger en slutpunkt med hello `Endpoint` tagg i hello tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten toohello Naming service. Andra tjänster som körs i klustret hello kan därför att identifiera den här behållaren med hjälp av hello REST-frågor för att lösa.

Genom att registrera med hello Naming service kan du utföra behållare till en annan kommunikation i en behållare med hello [omvänd proxy](service-fabric-reverseproxy.md). Kommunikation utförs genom att tillhandahålla omvänd proxy hello http lyssningsport och hello namn hello-tjänster som du vill använda toocommunicate med som miljövariabler. Mer information finns i hello nästa avsnitt. 

## <a name="configure-and-set-environment-variables"></a>Konfigurera och ange miljövariabler
Miljövariabler kan anges för varje kodpaketet i hello service manifest. Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster. Du kan åsidosätta miljövariabeln värden i hello application manifest eller ange dem under distributionen som parametrar för programmet.

hello följande service manifest XML-fragment visar ett exempel på hur toospecify miljövariabler för ett paket med koden:

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

De här miljövariablerna kan åsidosättas på hello manifestet programnivå:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

I föregående exempel hello vi angav ett explicit värde för hello `HttpGateway` miljövariabeln (19000), medan vi ordnar hello värde för `BackendServiceName` parametern via hello `[BackendSvc]` program-parametern. Dessa inställningar kan du toospecify hello värdet för `BackendServiceName`värde när du distribuerar hello program och inte har ett fast värde i hello manifest.

## <a name="configure-isolation-mode"></a>Konfigurera isoleringsläge

Windows stöder två arbetslägen för behållare - processen och Hyper-V.  Med hello arbetsprocesser hello alla hello-behållare som körs på samma värd datorn resursen hello kernel med hello-värden. Med hello Hyper-V-isoleringsläge isoleras hello kärnor mellan varje Hyper-V-behållaren och hello behållaren värden. hello isoleringsläge har angetts i hello `ContainerHostPolicies` tagg i hello programmanifestfilen.  hello arbetslägen som kan anges är `process`, `hyperv`, och `default`. Hej `default` isoleringsläge standardvärden för`process` på Windows Server är värd för, och är som standard för`hyperv` på Windows 10-värdar.  hello följande utdrag visar hur hello isoleringsläge har angetts i hello programmanifestfilen.

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

Ett exempel tjänstmanifestet (anges i föregående programmanifestet hello) visas nedan:

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
Nu när du har använt ett container service lära dig hur toomanage livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).

* [Översikt över Service Fabric och behållare](service-fabric-containers-overview.md)
* Ett exempel utcheckningen [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
