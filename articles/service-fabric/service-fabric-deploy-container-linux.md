---
title: "aaaService infrastruktur och distribuerar behållare i Linux | Microsoft Docs"
description: "Service Fabric och hello används av Linux behållare toodeploy mikrotjänster program. Den här artikeln beskriver hello-funktioner med Service Fabric för behållare och hur toodeploy en Linux-behållare bild i ett kluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a>Distribuera en Linux-behållaren tooService Fabric
> [!div class="op_single_selector"]
> * [Distribuera Windows-behållare](service-fabric-deploy-container.md)
> * [Distribuera behållare för Linux](service-fabric-deploy-container-linux.md)
>
>

Den här artikeln vägleder dig genom att bygga av tjänster i Docker behållare på Linux.

Service Fabric har flera behållare funktioner som hjälper dig med att skapa program som består av mikrotjänster som är behållare. Dessa tjänster kallas av tjänster.

hello funktionerna innehåller;

* Distribution av avbildningar för behållaren och aktivering
* Resurs-styrning
* Autentisering för databasen
* Behållaren toohost port portmappning
* Identifiering av behållare till en annan och kommunikation
* Möjlighet tooconfigure och ange miljövariabler

## <a name="packaging-a-docker-container-with-yeoman"></a>Paketera en dockerbehållare med yeoman
När en behållare på Linux-paket, kan du välja antingen toouse yeoman mall eller [skapa hello programpaket manuellt](#manually).

Ett Service Fabric-program kan innehålla en eller flera behållare med en viss roll i att leverera hello-funktionerna. hello Service Fabric-SDK för Linux innehåller en [Yeoman](http://yeoman.io/) generator som gör det enkelt toocreate ditt program och lägga till en behållare avbildning. Nu ska vi använda Yeoman toocreate kallas för ett program med en enda dockerbehållare *SimpleContainerApp*. Du kan senare lägga till fler tjänster genom att redigera hello genereras manifest-filer.

## <a name="install-docker-on-your-development-box"></a>Installera Docker på utveckling-ruta

Kör hello följande kommandon tooinstall docker på rutan ditt Linux-utveckling (om du använder hello vagrant i OSX docker redan har installerats):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a>Skapa hello program
1. I en terminal, skriver du in `yo azuresfcontainer`.
2. Namnge programmet – till exempel mycontainerap
3. Ange hello URL för hello behållaren avbildningen från en DockerHub lagringsplatsen. Hej avbildningen parametern tar hello formuläret [lagringsplatsen] / [image name]
4. Om hello avbildningen inte har en arbetsbelastning startpunkten definierats, måste du tooexplicitly ange inkommande kommandon med en kommaavgränsad uppsättning kommandon toorun inuti hello behållare som kommer att hålla hello-behållare som körs efter start.

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="deploy-hello-application"></a>Distribuera programmet hello

### <a name="using-xplat-cli"></a>Med XPlat CLI
När hello programmet är skapat kan distribuera du den toohello lokala kluster med hjälp av hello Azure CLI.

1. Ansluta toohello lokala Service Fabric-klustret.

    ```bash
    azure servicefabric cluster connect
    ```

2. Använd hello installera skript hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.

    ```bash
    ./install.sh
    ```

3. Öppna en webbläsare och gå tooService Fabric-Utforskaren på http://localhost:19080/Explorer (Ersätt localhost med hello privat IP hello VM om använder Vagrant på Mac OS X).
4. Expandera noden för hello-program och Lägg märke till att det finns en post för din typ av program och en annan för hello första instansen av den typen.
5. Använd hello avinstallera skript i hello toodelete hello programmet mallinstansen och avregistrering av programtyp hello.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Med Azure CLI 2.0

Se hello referens doc om hur du hanterar en [livscykel som använder hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).

Ett exempelprogram [utcheckningen hello Service Fabric-behållaren kod exempel på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-tooan-existing-application"></a>Lägga till fler tjänster tooan befintliga program

tooadd en annan behållare tjänstprogram tooan redan har skapats med hjälp av `yo`, utföra hello följande steg:

1. Ändra toohello rotkatalog till hello befintliga program.  Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.
2. Kör `yo azuresfcontainer:AddService`

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

Du kan ange inkommande kommandon genom att ange hello valfria `Commands` element med en kommaavgränsad uppsättning kommandon toorun i hello behållare.

> [!NOTE]
> Om hello avbildningen inte har en arbetsbelastning startpunkten definierats, måste du tooexplicitly ange inkommande kommandon i `Commands` element med en kommaavgränsad uppsättning kommandon toorun inuti hello behållare som kommer att hålla hello-behållare som körs Start.

## <a name="understand-resource-governance"></a>Förstå resource-styrning
Resurs-styrning är en funktion för hello-behållare som begränsar hello resurser som hello behållare kan använda på hello värden. Hej `ResourceGovernancePolicy`, som har angetts i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod. Resursen gränser kan anges för hello följande resurser:

* Minne
* MemorySwap
* CpuShares (CPU relativa viktade)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relativa vikt).

> [!NOTE]
> I en framtida utgåva, stöd för att ange specifika block-i/o-gränser, till exempel IOPs, läsning och skrivning bit/s och andra ska inkluderas.
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

## <a name="configure-container-port-to-host-port-mapping"></a>Konfigurera portmappning behållaren port-till-värd
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
Med hjälp av hello `PortBinding` princip, kan du mappa en behållare port tooan `Endpoint` i hello service manifest. Hej endpoint `Endpoint1` kan ange en fast port (till exempel port 80). Det kan också ange ingen port, och då en slumpmässigt vald port från portintervall för hello kluster programmet är valt.

Om du anger en slutpunkt med hello `Endpoint` tagg i hello tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten toohello Naming service. Andra tjänster som körs i klustret hello kan därför att identifiera den här behållaren med hjälp av hello REST-frågor för att lösa.

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

Genom att registrera med hello Naming service kan du enkelt göra behållare till en annan kommunikation i hello kod i din behållaren med hjälp av hello [omvänd proxy](service-fabric-reverseproxy.md). Kommunikation utförs genom att tillhandahålla omvänd proxy hello http lyssningsport och hello namn hello-tjänster som du vill använda toocommunicate med som miljövariabler. Mer information finns i hello nästa avsnitt.

## <a name="configure-and-set-environment-variables"></a>Konfigurera och ange miljövariabler
Miljövariabler kan anges för varje kodpaketet i hello tjänstmanifestet, både för tjänster som har distribuerats i behållare eller tjänster som distribueras som processer/gäst körbara filer. Dessa miljö variabelvärden kan åsidosättas i programmanifestet hello eller anges under distributionen som parametrar för programmet.

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
* [Interaktion med Service Fabric-kluster med hello Azure CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Relaterade artiklar

* [Kom igång med Service Fabric och Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Kom igång med Service Fabric XPlat CLI](service-fabric-azure-cli.md)
