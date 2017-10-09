---
title: "aaaDeploy befintliga körbara tooAzure Service Fabric | Microsoft Docs"
description: "Genomgång av hur toopackage ett befintligt program som en gäst körbar fil, så det kan vara distribueras tooa Service Fabric-kluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a>Distribuera en gäst körbara tooService Fabric
Du kan köra alla typer av kod, exempelvis Node.js, Java eller C++ i Azure Service Fabric som en tjänst. Service Fabric refererar toothese typer av tjänster som gäst körbara filer.

Gästen körbara filer behandlas av Service Fabric som tillståndslösa tjänster. Detta innebär att de är placerade på noder i ett kluster, baserat på tillgänglighet och andra mått. Den här artikeln beskriver hur toopackage och distribuera ett körbart tooa Service Fabric gästkluster med hjälp av Visual Studio eller ett kommandoradsverktyg.

I den här artikeln vi täcka hello steg toopackage gäst körbara och distribuerar den tooService Fabric.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Fördelar med att köra gäst körbara i Service Fabric
Det finns flera fördelar toorunning gäst körbara i ett Service Fabric-kluster:

* Hög tillgänglighet. Program som körs i Service Fabric görs högtillgänglig. Service Fabric säkerställer att instanser av ett program körs.
* Övervakning av hälsotillstånd. Service Fabric-hälsoövervakning känner av om ett program körs och innehåller diagnostisk information om det uppstår ett fel.   
* Livscykeln för hantering. Förutom att tillhandahålla uppgraderingar utan avbrott, tillhandahåller Service Fabric automatisk återställning toohello tidigare version om det finns en felaktig hälsohändelse som rapporteras vid en uppgradering.    
* Densitet. Du kan köra flera program i ett kluster, vilket eliminerar hello behovet av att varje program toorun på sin egen maskinvara.
* Synlighet: Med hjälp av REST kan du anropa hello Service Fabric Naming service toofind andra tjänster i hello kluster. 

## <a name="samples"></a>Exempel
* [Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Översikt över programmet och service manifest-filer
Som del av distributionen av en gäst körbar fil är användbara toounderstand hello Service Fabric paketering och distributionsmodell som beskrivs i [programmodell](service-fabric-application-model.md). hello Service Fabric paketering modellen förlitar sig på två XML-filer: hello applikationen eller tjänsten manifest. hello schemadefinition för hello ApplicationManifest.xml och ServiceManifest.xml filer har installerats med hello Service Fabric SDK i *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Applikationsmanifestet** hello programmanifestet är används toodescribe hello program. Den visar hello-tjänster som ska utgöra den och andra parametrar som används toodefine hur en eller flera tjänster ska distribueras, till exempel hello antal instanser.

  Ett program är en enhet av distribution och uppgradering i Service Fabric. Ett program kan uppgraderas som en enhet där potentiella problem och potentiella återställningar hanteras. Service Fabric garanterar att hello uppgraderingsprocessen är antingen lyckas, eller om hello uppgraderingen misslyckas, lämnar då inte hello program i ett okänt eller instabilt tillstånd.
* **Tjänstmanifestet** hello tjänstmanifestet beskriver hello komponenter av en tjänst. Den innehåller data, till exempel hello namn och typ av tjänst, och koden och konfiguration. hello tjänstmanifestet även vissa ytterligare parametrar som kan vara används tooconfigure hello-tjänsten när den har distribuerats.

## <a name="application-package-file-structure"></a>Filstruktur för programmets paket
toodeploy ett program tooService Fabric hello program bör följa en fördefinierad katalogstruktur. hello följande är ett exempel på denna struktur.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

Hej ApplicationPackageRoot innehåller hello ApplicationManifest.xml-fil som definierar hello program. En underkatalog för varje tjänst som ingår i programmet hello är används toocontain alla hello artefakter hello tjänsten kräver. Dessa underkataloger är hello ServiceManifest.xml och normalt hello följande:

* *Koden*. Den här katalogen innehåller hello Tjänstkod.
* *Config*. Den här katalogen innehåller en Settings.xml (och andra filer vid behov) att hello-tjänsten kan komma åt vid körning tooretrieve specifika konfigurationsinställningar.
* *Data*. Det här är en ytterligare katalog toostore ytterligare lokala data som kan behöva hello-tjänsten. Data ska använda toostore endast tillfälliga data. Service Fabric stöder inte att kopiera eller replikera ändringar toohello datakatalog om hello-tjänsten måste toobe flyttas (till exempel under växling vid fel).

> [!NOTE]
> Du har inte toocreate hello `config` och `data` kataloger om du inte behöver dem.
>
>

## <a name="package-an-existing-executable"></a>Paketet en befintlig körbar fil
När Paketera en gäst körbar fil kan du välja antingen toouse en projektmall för Visual Studio eller för[skapa hello programpaket manuellt](#manually). Med Visual Studio hello programmet paketet struktur och manifest-filer skapas av hello ny projektmall för dig.

> [!TIP]
> hello enklaste sättet toopackage en befintlig Windows körbara till en tjänst är toouse Visual Studio och på Linux toouse Yeoman
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a>Använda Visual Studio toopackage och distribuera en befintlig körbar fil
Visual Studio har ett Service Fabric service mallen toohelp du distribuera ett gästkluster körbara tooa Service Fabric.

1. Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.
2. Välj **gäst körbara** som mall för hello-tjänster.
3. Klicka på **Bläddra** tooselect hello mappen med den körbara filen och Fyll i hello resten av hello parametrar toocreate hello-tjänsten.
   * *Code paketet beteende*. Kan vara set toocopy alla hello innehållet i din mapp toohello Visual Studio-projekt, vilket är användbart om hello körbara inte ändras. Om du förväntar dig hello körbara toochange och vill hello möjlighet toopick in nya versioner dynamiskt, kan du välja toolink toohello mapp i stället. Du kan använda länkade mappar när du skapar hello application-projekt i Visual Studio. Detta länkar toohello källplats från inom hello-projektet att göra det möjligt för du tooupdate hello gäst körbara i dess källa mål. Dessa uppdateringar blir en del av hello programpaket version.
   * *Programmet* anger hello körbar fil som ska köras toostart hello-tjänsten.
   * *Argument* anger hello-argument som ska skickas toohello körbara. Det kan vara en lista över parametrar med argument.
   * *WorkingFolder* anger hello arbetskatalog för hello-processen som ska toobe igång. Du kan ange tre värden:
     * `CodeBase`Anger att hello arbetskatalogen försätts toobe ange toohello kod katalog i hello-programpaket (`Code` directory som visas i föregående filstruktur hello).
     * `CodePackage`Anger att hello arbetskatalogen försätts toobe ange toohello rot hello-programpaket (`GuestService1Pkg` visas i föregående filstruktur hello).
     * `Work`Anger att hello filer placeras i en underkatalog med namnet arbete.
4. Namnge tjänsten och klicka på **OK**.
5. Om din tjänst måste en slutpunkt för kommunikation, kan du nu lägga till hello-protokollet, port och typen toohello ServiceManifest.xml filen. Till exempel: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Du kan nu använda hello paketet och publicera åtgärd mot ditt lokala kluster med felsökning hello lösningen i Visual Studio. När du är klar kan du publicera programmet hello tooa fjärrkluster eller kontrollera hello lösning toosource kontrollen.
7. Gå toohello slutet av den här artikeln toosee hur tooview din körbara gäst-tjänst körs i Service Fabric Explorer.

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a>Använda Yoeman toopackage och distribuera en befintlig körbar fil på Linux

hello proceduren för att skapa och distribuera gäst körbara på Linux är hello samma som distribuerar ett csharp eller java-program.

1. I en terminal, skriver du in `yo azuresfguest`.
2. Namnge ditt program.
3. Namnge din tjänst och ange hello information, inklusive sökvägen till hello körbara och hello parametrar det måste anropas med.

Yeoman skapar ett programpaket med lämpliga hello-program och manifestfiler tillsammans med installera och avinstallera skript.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Paketera och distribuera en befintlig körbar fil manuellt
hello processen om paketeringen manuellt av en gäst körbar fil baserat på hello följande allmänna steg:

1. Skapa hello paketet katalogstruktur.
2. Lägg till hello-kod och configuration-filer.
3. Redigera hello service manifest-filen.
4. Redigera hello programmanifestfilen.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a>Skapa katalogstruktur för hello-paket
Du kan börja med att skapa hello katalogstrukturen, enligt beskrivningen i föregående avsnitt, hello ”programmet paketet filstruktur”.

### <a name="add-hello-applications-code-and-configuration-files"></a>Lägg till hello-koden och konfigurationen filer
När du har skapat hello katalogstrukturen, du kan lägga till hello programmet koden och konfigurationen under hello koden och konfigurationsversionerna kataloger. Du kan också skapa ytterligare kataloger eller underkataloger på hello kod eller konfigurationsversion kataloger.

Service Fabric har en `xcopy` av hello i hello tillämpningsprogrammets rotkatalog, så det finns inga fördefinierade strukturen toouse än att skapa två översta kataloger, kod och inställningar. (Du kan välja olika namn om du vill. Mer information finns i hello nästa avsnitt.)

> [!NOTE]
> Kontrollera att du inkluderar alla hello filer och beroenden som hello programbehov. Service Fabric kopierar hello innehåll i hello programpaket på alla noder i hello kluster där hello programtjänster är pågående toobe distribueras. hello paketet innehåller alla hello kod att programmet hello måste toorun. Förutsätter inte att hello beroenden redan är installerade.
>
>

### <a name="edit-hello-service-manifest-file"></a>Redigera hello service manifest-filen
hello nästa steg är tooedit hello service manifestfilen tooinclude hello följande information:

* hello namnet på hello service-typen. Detta är ett ID som använder Service Fabric tooidentify en tjänst.
* hello kommandot toouse toolaunch hello program (ExeHost).
* Alla skript som behöver toobe kör tooset in hello program (SetupEntrypoint).

hello följande är ett exempel på en `ServiceManifest.xml` fil:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

följande avsnitt hello gå igenom hello olika delar av hello-fil som du behöver tooupdate.

#### <a name="update-servicetypes"></a>Uppdatera ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* Du kan välja vilket namn som du vill använda för `ServiceTypeName`. hello värdet används i hello `ApplicationManifest.xml` filen tooidentify hello-tjänsten.
* Ange `UseImplicitHost="true"`. Det här attributet anger Service Fabric att hello tjänsten baserat på en fristående app, så måste alla Service Fabric toodo är toolaunch den som en process och övervaka dess hälsa.

#### <a name="update-codepackage"></a>Uppdatera CodePackage
Hej CodePackage element anger hello plats (och version) av hello service-kod.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Hej `Name` element är används toospecify hello namn i hello katalog i hello programpaket som innehåller hello service-kod. `CodePackage`har också hello `version` attribut. Detta kan vara används toospecify hello version av hello kod och kan också vara tooupgrade hello service koden används med hjälp av hello livscykel infrastruktur för hantering i Service Fabric.

#### <a name="optional-update-setupentrypoint"></a>Valfritt: Uppdatera SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Hej SetupEntryPoint element är används toospecify en körbar fil eller en batch-fil som ska köras innan hello service-koden har startats. Det är ett valfritt steg så inte behöver toobe om det finns inga initiering krävs. Hej SetupEntryPoint körs varje gång hello-tjänsten startas.

Det finns endast en SetupEntryPoint installationsskript måste toobe grupperade i en enda kommandofil om hello programinställningar kräver flera skript. Hej SetupEntryPoint kan köra alla typer av filer: körbara filer, batch-filer och PowerShell-cmdlets. Mer information finns i [konfigurera SetupEntryPoint](service-fabric-application-runas-security.md).

I föregående exempel hello, hello SetupEntryPoint körs en batchfil som kallas `LaunchConfig.cmd` som finns i hello `scripts` hello kod-mappen (förutsatt att hello WorkingFolder element anges tooCodeBase).

#### <a name="update-entrypoint"></a>Uppdatera EntryPoint
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Hej `EntryPoint` -elementet i hello service manifest-filen är används toospecify hur toolaunch hello-tjänsten. hello `ExeHost` element anger hello körbara (och argument) som ska användas toolaunch hello-tjänsten.

* `Program`Anger hello för hello körbar fil som ska starta hello-tjänsten.
* `Arguments`Anger hello-argument som ska skickas toohello körbara. Det kan vara en lista över parametrar med argument.
* `WorkingFolder`Anger hello arbetskatalog för hello-processen som ska toobe igång. Du kan ange tre värden:
  * `CodeBase`Anger att hello arbetskatalogen försätts toobe ange toohello kod katalog i hello-programpaket (`Code` katalogen i hello föregående filstruktur).
  * `CodePackage`Anger att hello arbetskatalogen försätts toobe ange toohello rot hello-programpaket (`GuestService1Pkg` i hello föregående filstruktur).
    * `Work`Anger att hello filer placeras i en underkatalog med namnet arbete.

Hej WorkingFolder är användbara tooset hello rätt arbetskatalogen så att relativa sökvägar kan användas av antingen hello program eller initiering skript.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Uppdatera slutpunkter och registrera med namngivningstjänst för kommunikation
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
I föregående exempel hello, hello `Endpoint` element anger hello-slutpunkter som hello programmet kan lyssna på. I det här exemplet hello Node.js-program som lyssnar på http på 3000-port.

Dessutom be du Service Fabric toopublish den här slutpunkten toohello Naming Service så att andra tjänster kan identifiera hello endpoint adress toothis service. Detta gör att du toobe kan toocommunicate mellan tjänster som gäst körbara filer.
hello publicerade slutpunktsadress har hello form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`och `PathSuffix` är valfria attribut. `IPAddressOrFQDN`är hello IP-adress eller fullständigt domännamn för hello nod den körbara filen hämtar placerad och den beräknas åt dig.

I hello följande exempel visas en gång hello-tjänsten har distribuerats, Service Fabric Explorer visas i en liknande slutpunkt för`http://10.1.4.92:3000/myapp/` publicerats för hello tjänstinstansen. Eller om det är en lokal dator kan du se `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Du kan använda dessa adresser med [omvänd proxy](service-fabric-reverseproxy.md) toocommunicate olika tjänster.

### <a name="edit-hello-application-manifest-file"></a>Redigera hello programmanifestfilen
När du har konfigurerat hello `Servicemanifest.xml` filen, behöver du toomake vissa ändringar toohello `ApplicationManifest.xml` filen tooensure hello rätt typ av tjänst och namnet används.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
I hello `ServiceManifestImport` element, kan du ange en eller flera tjänster som du vill tooinclude i hello app. Tjänster refereras med `ServiceManifestName`, vilket anger hello namnet på hello katalog där hello `ServiceManifest.xml` filen finns.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Konfigurera loggning
Gästen körbara filer är det användbart toobe kan toosee konsolen loggar toofind ut om hello program och konfiguration skript visar eventuella fel.
Omdirigering av konsol kan konfigureras i hello `ServiceManifest.xml` fil med hello `ConsoleRedirection` element.

> [!WARNING]
> Använd aldrig hello konsolen omdirigeringspolicyn i ett program som distribuerats i produktionsmiljön eftersom detta kan påverka hello programmet redundans. *Endast* använda detta för lokal utveckling och felsökning.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`kan vara används tooredirect konsolen utdata (stdout och stderr) tooa arbetskatalogen. Detta ger hello möjlighet tooverify att det inte finns några fel vid hello installation eller körning av programmet hello i hello Service Fabric-klustret.

`FileRetentionCount`Anger hur många filer sparas i hello arbetskatalogen. Värdet 5, till exempel innebär att hello loggfiler för hello tidigare fem körningar lagras i hello arbetskatalogen.

`FileMaxSizeInKb`Anger hello maxstorleken för hello loggfiler.

Loggfilerna sparas i en av hello-tjänsten fungerar kataloger. toodetermine där hello filer finns, Använd Service Fabric Explorer toodetermine vilken nod hello-tjänsten körs på och vilka arbetskatalogen används. Den här processen beskrivs senare i den här artikeln.

## <a name="deployment"></a>Distribution
hello sista steget är för[distribuera programmet](service-fabric-deploy-remove-applications.md). Hej följande PowerShell-skript visar hur toodeploy dina program toohello lokal utveckling klustret och starta en ny Service Fabric-tjänsten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet om hello paketet är stora eller många filer. Läs mer [här](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Ett Service Fabric-tjänsten kan distribueras i olika ”konfigurationer”. Till exempel den kan distribueras som en eller flera instanser eller mallen kan distribueras på ett sådant sätt att det finns en instans av hello-tjänsten på varje nod i hello Service Fabric-klustret.

Hej `InstanceCount` parametern för hello `New-ServiceFabricService` cmdlet har använt toospecify hur många instanser av hello tjänst ska startas i hello Service Fabric-klustret. Du kan ange hello `InstanceCount` värde, beroende på hello typ av program som du distribuerar. hello två vanligaste scenarierna är:

* `InstanceCount = "1"`. Endast en instans av hello tjänsten distribueras i det här fallet i hello klustret. Service Fabric scheduler avgör vilken nod hello tjänsten kommer toobe distribueras på.
* `InstanceCount ="-1"`. I det här fallet distribueras en instans av hello tjänsten på varje nod i hello Service Fabric-klustret. hello resultatet har ett (och endast ett) instans av hello-tjänsten för varje nod i klustret hello.

Detta är en användbar konfiguration för frontend-program (till exempel en REST-slutpunkt), eftersom klientprogram måste för ”ansluta” tooany hello noder i hello klustret toouse hello slutpunkt. Den här konfigurationen kan också användas när alla noder i hello Service Fabric-kluster är till exempel anslutna tooa belastningsutjämnaren. Klienttrafik kan sedan distribueras över hello-tjänst som körs på alla noder i klustret hello.

## <a name="check-your-running-application"></a>Kontrollera att programmet körs
I Service Fabric Explorer identifiera hello-nod där hello-tjänsten körs. I det här exemplet körs den på Nod1:

![Noden som tjänsten körs](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Om du navigerar toohello nod och bläddra toohello program, kan du se hello väsentliga nod information, inklusive dess plats på disken.

![Plats på disken](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Om du bläddrar toohello directory med hjälp av Server Explorer hittar du hello arbetskatalogen och hello service loggmappen, som visas i följande skärmbild hello: 

![Plats för logg](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toopackage en gäst körbar fil och distribuera den tooService Fabric. Se följande artiklar för relaterad information och uppgifter hello.

* [Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), inklusive en länk toohello förhandsversion av hello paketering verktyget
* [Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [Distribuera flera körbara gäster](service-fabric-deploy-multiple-apps.md)
* [Skapa ditt första Service Fabric-program med Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
