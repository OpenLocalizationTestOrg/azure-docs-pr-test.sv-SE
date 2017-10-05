---
title: "Service Fabric och hur du distribuerar behållare i Linux | Microsoft Docs"
description: "Service Fabric och användning av Linux-behållare för att distribuera mikrotjänster program. Den här artikeln beskriver de funktioner som Service Fabric ger för behållare och hur du distribuerar en avbildning för Linux-behållaren i ett kluster"
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
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a>Distribuera en Linux-behållare till Service Fabric
> [!div class="op_single_selector"]
> * [Distribuera Windows-behållare](service-fabric-deploy-container.md)
> * [Distribuera behållare för Linux](service-fabric-deploy-container-linux.md)
>
>

Den här artikeln vägleder dig genom att bygga av tjänster i Docker behållare på Linux.

Service Fabric har flera behållare funktioner som hjälper dig med att skapa program som består av mikrotjänster som är behållare. Dessa tjänster kallas av tjänster.

Funktionerna innehåller;

* Distribution av avbildningar för behållaren och aktivering
* Resurs-styrning
* Autentisering för databasen
* Behållaren port till värden portmappning
* Identifiering av behållare till en annan och kommunikation
* Möjligheten att konfigurera och ange miljövariabler

## <a name="packaging-a-docker-container-with-yeoman"></a>Paketera en dockerbehållare med yeoman
När en behållare på Linux-paket, kan du välja antingen att använda en mall för yeoman eller [skapa programpaketet manuellt](#manually).

Ett Service Fabric-program kan innehålla en eller flera behållare med en viss roll i att leverera programmets funktioner. I Service Fabric SDK för Linux finns en [Yeoman](http://yeoman.io/)-generator som gör det enkelt att skapa ditt program och lägga till en behållaravbildning. Nu ska vi använda Yeoman för att skapa ett program med en enda dockerbehållare som kallas *SimpleContainerApp*. Du kan lägga till fler tjänster senare genom att redigera den genererade manifest filer.

## <a name="install-docker-on-your-development-box"></a>Installera Docker på utveckling-ruta

Kör följande kommandon för att installera docker på rutan ditt Linux-utveckling (om du använder vagrant bilden i OSX docker redan har installerats):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a>Skapa programmet
1. I en terminal, skriver du in `yo azuresfcontainer`.
2. Namnge programmet – till exempel mycontainerap
3. Ange en URL för bilden behållare från en DockerHub lagringsplatsen. Bild-parametern har formatet [lagringsplatsen] / [image name]
4. Om avbildningen inte har en arbetsbelastning startpunkten definierat, måste du uttryckligen ange inkommande kommandon med en kommaavgränsad uppsättning kommandon som ska köras i behållare som kommer att hålla den behållare som körs efter start.

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="deploy-the-application"></a>Distribuera programmet

### <a name="using-xplat-cli"></a>Med XPlat CLI
När du har skapat programmet kan du distribuera det till det lokala klustret med Azure CLI.

1. Anslut till det lokala Service Fabric-klustret.

    ```bash
    azure servicefabric cluster connect
    ```

2. Använd installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.

    ```bash
    ./install.sh
    ```

3. Öppna en webbläsare och gå till Service Fabric Explorer på http://localhost:19080/Explorer (ersätt localhost med den virtuella datorns privata IP om du använder Vagrant på Mac OS X).
4. Expandera programnoden och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.
5. Använda avinstallera skriptet i mallen för att ta bort instansen av programmet och avregistrera programtypen.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Med Azure CLI 2.0

Se referens doc om hur du hanterar en [livscykel för program som använder Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).

Ett exempelprogram [utcheckningen Service Fabric behållarens kod exempel på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-to-an-existing-application"></a>Lägga till fler tjänster till ett befintligt program

Att lägga till en annan behållartjänst till ett program som redan har skapats med hjälp av `yo`, utför följande steg:

1. Ändra katalogen till roten för det befintliga programmet.  Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.
2. Kör `yo azuresfcontainer:AddService`

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

Du kan ange inkommande kommandon genom att ange den valfria `Commands` element med en kommaavgränsad uppsättning kommandon som ska köras i behållaren.

> [!NOTE]
> Om bilden inte har en arbetsbelastning startpunkten definierats, så måste du uttryckligen ange inkommande kommandon i `Commands` element med en kommaavgränsad uppsättning kommandon som ska köras i behållare som kommer att hålla den behållare som körs efter start.

## <a name="understand-resource-governance"></a>Förstå resource-styrning
Resurs-styrning är en funktion på behållaren som begränsar de resurser som behållaren kan använda på värden. Den `ResourceGovernancePolicy`, som har angetts i applikationsmanifestet används för att deklarera gränserna för ett paket för service-kod. Gränserna kan anges för följande resurser:

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

## <a name="configure-container-port-to-host-port-mapping"></a>Konfigurera portmappning behållaren port-till-värd
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
Med hjälp av den `PortBinding` princip, kan du mappa en behållare port till en `Endpoint` i service manifest. Slutpunkten `Endpoint1` kan ange en fast port (till exempel port 80). Det kan också ange ingen port, och då en slumpmässigt vald port från klustrets programmet portintervallet är valt.

Om du anger en slutpunkt med hjälp av den `Endpoint` tagg i tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten till Naming service. Andra tjänster som körs i klustret kan därför att identifiera den här behållaren med hjälp av REST-frågor för att lösa.

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

Genom att registrera med Naming service kan du enkelt göra behållare till en annan kommunikation i koden i din behållaren med hjälp av den [omvänd proxy](service-fabric-reverseproxy.md). Kommunikation utförs genom att tillhandahålla omvänd proxy http-lyssningsporten och namnet på de tjänster som du vill kommunicera med som miljövariabler. Mer information finns i nästa avsnitt.

## <a name="configure-and-set-environment-variables"></a>Konfigurera och ange miljövariabler
Miljövariabler kan anges för varje kodpaketet i service manifest, både för tjänster som har distribuerats i behållare eller tjänster som distribueras som processer/gäst körbara filer. Dessa miljö variabelvärden kan åsidosättas i programmanifestet eller anges under distributionen som parametrar för programmet.

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
* [Interagera med Service Fabric-kluster med Azure CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Relaterade artiklar

* [Kom igång med Service Fabric och Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Kom igång med Service Fabric XPlat CLI](service-fabric-azure-cli.md)
