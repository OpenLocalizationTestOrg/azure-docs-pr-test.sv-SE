---
title: "aaaDeploy ett Node.js-program som använder MongoDB | Microsoft Docs"
description: "Genomgång av hur toopackage flera körbara filer toodeploy tooan Azure Service Fabric gästkluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell;mikhegn
ms.openlocfilehash: 2775080f0d9d42d6ba15cca911e23067106be26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-guest-executables"></a>Distribuera flera körbara gäster
Den här artikeln visar hur toopackage och distribuera flera gäst körbara filer tooAzure Service Fabric. För att skapa och distribuera en enda Service Fabric-paket läsa hur för[distribuera en gäst körbara tooService Fabric](service-fabric-deploy-existing-app.md).

När den här genomgången visar hur toodeploy ett program med en Node.js-klientdel som använder MongoDB som datalager hello, du kan använda hello steg tooany program som är beroende av ett annat program.   

Du kan använda Visual Studio tooproduce hello med programpaket som innehåller flera gäst körbara filer. Se [med hjälp av Visual Studio toopackage ett befintligt program](service-fabric-deploy-existing-app.md). När du har lagt till hello första gäst körbar fil högerklickar du på hello projektet och välj hello **Lägg till -> Ny Service Fabric-tjänsten** tooadd hello andra gäst körbara projektet toohello lösningen. Obs: Om du väljer toolink hello källa i hello Visual Studio-projekt, skapa hello Visual Studio-lösning ska kontrollera att programpaketet är upp toodate med ändringar i hello källan. 

## <a name="samples"></a>Exempel
* [Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a>Manuellt flera gäst körbara programmet hello i paketet
Alternativt kan du paketera hello gäst körbara manuellt. Hello manuell förpackningen kan den här artikeln använder hello Service Fabric paketering verktyg som finns på [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-hello-nodejs-application"></a>Paketering hello Node.js-program
Den här artikeln förutsätter att Node.js inte är installerat på hello noder i hello Service Fabric-klustret. Följaktligen måste tooadd Node.exe toohello rotkatalogen för programmet nod innan paketering. hello katalogstrukturen på hello Node.js-program (med snabb webbramverk och Jade mall engine) bör se ut ungefär toohello en nedan:

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Som ett nästa steg kan du skapa ett programpaket för hello Node.js-program. hello koden nedan skapar ett Service Fabric-programpaket som innehåller hello Node.js-program.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Nedan visas en beskrivning av hello-parametrar som används:

* **/ source** punkter toohello katalogen för hello-program som ska paketeras.
* **/ target** definierar hello directory i vilka hello paketet ska skapas. Den här katalogen har toobe skiljer sig från hello källkatalogen.
* **/ AppName** definierar hello programnamn till hello befintliga program. Det är viktigt toounderstand detta innebär toohello tjänstnamn i hello manifestet och inte toohello Service Fabric programnamn.
* **/exe** definierar hello körbara att Service Fabric ska toolaunch, i det här fallet `node.exe`.
* **/Ma** definierar hello-argument som håller på att använda toolaunch hello körbara. Node.js är inte installerad, måste Service Fabric toolaunch hello Node.js webbservern genom att köra `node.exe bin/www`.  `/ma:'bin/www'`Visar hello paketering verktyget toouse `bin/ma` som hello argument för node.exe.
* **/ AppType** definierar hello Service Fabric-programmets typnamn.

Om du bläddrar toohello katalog som angavs i hello/Target-parametern kan du se hello verktyget har skapat ett fullt fungerande Service Fabric-paket som visas nedan:

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
hello genererade ServiceManifest.xml har nu ett avsnitt som beskriver hur hello Node.js webbserver bör startas, enligt hello kodfragmentet nedan:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
I det här exemplet lyssnar hello Node.js webbservern tooport 3000, så du måste tooupdate hello slutpunktsinformation i hello ServiceManifest.xml filen enligt nedan.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a>Paketering hello MongoDB-program
Nu när du har paketerat hello Node.js-program, kan du gå vidare och paketet MongoDB. Som tidigare nämnts, hello steg som du går igenom nu är inte specifik tooNode.js och MongoDB. De gäller i själva verket tooall program som är avsedda toobe packade tillsammans som ett Service Fabric-program.  

toopackage MongoDB, vill du att du paketerar Mongod.exe och Mongo.exe toomake. Båda binärfiler finns i hello `bin` för din MongoDB-installationskatalogen. hello katalogstruktur verkar liknande toohello en nedan.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric behöver toostart MongoDB med ett kommando liknande toohello en nedan, så du behöver toouse hello `/ma` parameter när paketera MongoDB.

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> hello bevaras inte data som ett nodfel hello gäller om du placerar hello MongoDB datakatalog på hello lokal katalog för hello-nod. Du bör använda beständig lagring eller implementera MongoDB replikuppsättning ordning tooprevent förlust av data.  
>
>

I PowerShell eller hello-kommandogränssnittet kör vi hello paketering verktyget med hello följande parametrar:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

I programpaket för Service Fabric ordning tooadd MongoDB tooyour, behöver du toomake att hello/Target parametern pekar toohello samma katalog som innehåller redan hello programmanifestet tillsammans med hello Node.js-program. Du måste också kontrollera att du använder toomake hello samma ApplicationType namn.

Nu ska vi Bläddra i katalog toohello och undersöka vilka hello-verktyget har skapats.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Som du ser hello verktyget lägga till en ny mapp, MongoDB, toohello katalog som innehåller hello MongoDB-binärfiler. Om du öppnar hello `ApplicationManifest.xml` fil, du kan se det hello-paketet innehåller nu både hello Node.js-program och MongoDB. hello koden nedan visar hello innehållet i hello programmanifestet.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-hello-application"></a>Publishing hello-program
hello sista steget är toopublish hello programmet toohello lokala Service Fabric-kluster med hjälp av hello PowerShell-skript nedan:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

När programmet hello har publicerade toohello lokala klustret kan du komma åt hello Node.js-program på hello-port som vi angav i hello tjänstmanifestet av hello Node.js-program, till exempel http://localhost: 3000.

I den här kursen har du sett hur tooeasily paketet två befintliga program som en Service Fabric-program. Du har också fått lära dig hur toodeploy den tooService Fabric så att den kan dra nytta av några av hello Service Fabric-funktioner, till exempel integrering med hög tillgänglighet och hälsa.


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a>Lägga till flera gäst körbara filer tooan befintliga program med hjälp av Yeoman på Linux

tooadd en annan tooan tjänstprogrammet redan skapats med hjälp av `yo`, utföra hello följande steg: 
1. Ändra toohello rotkatalog till hello befintliga program.  Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.
2. Kör `yo azuresfguest:AddService` och ange hello nödvändiga informationen.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om hur du distribuerar behållare med [översikt över Service Fabric och behållare](service-fabric-containers-overview.md)
* [Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
