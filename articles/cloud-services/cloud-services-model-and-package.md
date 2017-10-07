---
title: "aaaWhat är en tjänst i molnet och paketet | Microsoft Docs"
description: "Beskriver hello cloud-tjänstmodellen (.csdef, .cscfg) och paketet (.cspkg) i Azure"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 5280cdca4810859b6afdbbe1359fc2fabe871894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a>Vad är hello Cloud Service model och hur jag paketera den?
En molnbaserad tjänst skapas från tre komponenter, hello tjänstdefinitionen *(.csdef)*, hello service config *(.cscfg)*, och inget tjänstepaket *(.cspkg)*. Båda hello **ServiceDefinition.csdef** och **ServiceConfig.cscfg** filer är XML-baserade och beskriver hello struktur hello tjänst i molnet och hur den är konfigurerad, kallade hello modellen. Hej **ServicePackage.cspkg** är en zip-fil som genereras från hello **ServiceDefinition.csdef** och bland annat innehåller alla hello krävs binary-beroenden. Azure skapar en molnbaserad tjänst från båda hello **ServicePackage.cspkg** och hello **ServiceConfig.cscfg**.

När Molntjänsten hello körs i Azure kan du konfigurera om den via hello **ServiceConfig.cscfg** filen, men du kan inte ändra hello definition.

## <a name="what-would-you-like-tooknow-more-about"></a>Vad vill du tooknow mer om?
* Jag vill ha mer information om hello tooknow [ServiceDefinition.csdef](#csdef) och [ServiceConfig.cscfg](#cscfg) filer.
* Jag redan vet att ge mig [några exempel](#next-steps) på kan jag konfigurera.
* Jag vill toocreate hello [ServicePackage.cspkg](#cspkg).
* Jag använder Visual Studio och vill...
  * [Skapa en molntjänst][vs_create]
  * [Konfigurera om en befintlig molntjänst][vs_reconfigure]
  * [Distribuera en Cloud Service-projekt][vs_deploy]
  * [Fjärrskrivbord till en tjänstinstans för molnet][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Hej **ServiceDefinition.csdef** filen anger hello-inställningar som används av Azure tooconfigure en tjänst i molnet. Hej [Azure Service Definition schemat (.csdef-fil)](https://msdn.microsoft.com/library/azure/ee758711.aspx) ger hello tillåtna format för en tjänstdefinitionsfilen. hello visar följande exempel hello-inställningar som kan definieras för hello webb- och arbetsroller:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Du kan se toohello [Service Definition schemat](https://msdn.microsoft.com/library/azure/ee758711.aspx) för en bättre förståelse av hello XML-schema används här, men här är en snabb förklaring av vissa hello element:

**Webbplatser**  
Innehåller hello definitioner för webbplatser eller web program som finns i IIS7.

**InputEndpoints**  
Innehåller hello definitioner för slutpunkter som är används toocontact hello-Molntjänsten.

**InternalEndpoints**  
Innehåller hello definitioner för slutpunkter som används av rollen instanser toocommunicate med varandra.

**ConfigurationSettings**  
Innehåller definitioner för hello inställningen för funktioner på en viss roll.

**Certifikat**  
Innehåller hello definitioner för certifikat som behövs för en roll. hello förra kodexemplet visar ett certifikat som används för att konfigurera hello Azure Connect.

**LocalResources**  
Innehåller hello definitioner för lokala lagringsresurser. En lokal lagringsresurs är en reserverad katalog på hello filsystem hello virtuell dator som kör en instans av en roll.

**Import**  
Innehåller hello definitioner för importerade moduler. hello förra kodexemplet visar hello moduler för anslutning till fjärrskrivbord och Azure Connect.

**Start**  
Innehåller uppgifter som körs när hello roll startar. hello aktiviteter har definierats i en .cmd eller en körbar fil.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
hello konfigurationen av hello inställningarna för din molntjänst bestäms av hello värden i hello **ServiceConfiguration.cscfg** fil. Du kan ange hello antalet instanser som du vill toodeploy för varje roll i den här filen. hello värden för hello konfigurationsinställningar som du definierade i hello tjänstdefinitionsfilen läggs toohello tjänstkonfigurationsfilen. hello tumavtryck för av hanteringscertifikat som är associerade med hello Molntjänsten läggs toohello fil. Hej [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx) ger hello tillåtna format för en konfigurationsfil för tjänsten.

hello tjänstkonfigurationsfilen levereras inte med programmet hello, men är överförda tooAzure som en separat fil och använda tooconfigure hello Molntjänsten. Du kan ladda upp en ny konfigurationsfil för tjänsten utan att omdistribuera din tjänst i molnet. hello konfigurationsvärden för hello Molntjänsten kan ändras när hello molnbaserad tjänst körs. hello visar följande exempel hello konfigurationsinställningar som kan definieras för hello webb- och arbetsroller:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Du kan se toohello [Service Konfigurationsschemat](https://msdn.microsoft.com/library/azure/ee758710.aspx) för bättre förståelse hello XML-schema används här, men här är en snabb förklaring av hello element:

**Instanser**  
Konfigurerar hello antalet instanser för hello rollen körs. tooprevent ditt moln tjänsten potentiellt blir otillgänglig under uppgraderingar, bör du distribuera mer än en instans av web-riktade-roller. Genom att distribuera mer än en instans du uppfyller toohello riktlinjerna i hello [Azure Compute serviceavtalet (SLA)](http://azure.microsoft.com/support/legal/sla/), vilket garanterar 99,95% extern anslutning för Internet-riktade roller när två eller flera roll instanser distribueras för en tjänst.

**ConfigurationSettings**  
Konfigurerar hello inställningar för hello instanser för en roll som körs. hello namnet på hello `<Setting>` element måste matcha hello inställningsdefinitioner i hello tjänstdefinitionsfilen.

**Certifikat**  
Konfigurerar hello-certifikat som används av hello-tjänsten. hello förra kodexemplet visar hur toodefine hello certifikat för hello RemoteAccess modulen. Hej värdet för hello *tumavtrycket* attributet måste anges toohello tumavtryck hello certifikat toouse.

<p/>

> [!NOTE]
> hello tumavtrycket för certifikatet för hello läggas toohello konfigurationsfilen med hjälp av en textredigerare. Eller hello värde kan läggas till på hello **certifikat** för hello **egenskaper** sidan hello roll i Visual Studio.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Definiera portar för rollinstanser
Azure kan endast en post punkt tooa webbroll. Vilket innebär att all trafik sker via en IP-adress. Du kan konfigurera din webbplatser tooshare en port genom att konfigurera hello värden huvud toodirect hello begäran toohello rätt plats. Du kan också konfigurera ditt program toolisten toowell-portar på hello IP-adress.

hello visar följande exempel hello konfiguration för en webbroll med ett webbplatsen och program. hello-webbplatsen är konfigurerad som hello post standardplatsen på port 80 och hello webbprogram är konfigurerade tooreceive begäranden från ett huvud för alternativa värden som kallas ”mail.mysite.cloudapp.net”.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-hello-configuration-of-a-role"></a>Ändra hello konfiguration av en roll
Du kan uppdatera hello konfigurationen av din molntjänst när den körs i Azure, utan att koppla hello-tjänsten. toochange konfigurationsinformation, du kan antingen ladda upp en ny konfigurationsfil, eller redigera hello-konfigurationsfilen i placera och tillämpa det tooyour som kör tjänsten. hello kan följande ändringar göras toohello konfigurationen för en tjänst:

* **Ändra hello värdena för konfigurationsinställningar**  
  När en konfiguration kan ändras, en rollinstans välja tooapply hello ändringen när hello-instansen är online eller toorecycle hello instans smidigt och hello ändringen gälla när hello-instans är offline.
* **Ändra hello service topologi rollinstanser**  
  Ändringar i nätverkstopologin påverkar inte instanser, utom där en instans tas bort. Alla återstående instanser normalt behöver inte toobe återvinns; Du kan dock toorecycle rollinstanser i svaret tooa topologi förändras.
* **Ändra hello certifikatets tumavtryck**  
  Du kan bara uppdatera certifikat när en rollinstans är offline. Om ett certifikat läggs till, tas bort eller ändras medan en rollinstans är online, tar hello instanscertifikatet offline tooupdate hello Azure smidigt och göra den online när hello ändringen har utförts.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Konfigurationsändringar för hantering med körning av tjänsten-händelser
Hej [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) innehåller hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namnområdet, som innehåller klasser för att interagera med hello Azure-miljön från en roll. Hej [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definierar hello följande händelser som är före och efter en konfigurationsändring i klassen:

* **[Ändra](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) händelse**  
  Detta inträffar innan hello konfigurationsändring är tillämpade tooa angivna instans av en roll som ger en möjlighet tootake ned hello rollinstanser om det behövs.
* **[Ändra](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) händelse**  
  Inträffar när hello konfigurationsändring är tillämpade tooa angivna instans av en roll.

> [!NOTE]
> Eftersom certifikatet ändringar ta alltid hello instanser av en roll som är offline, öka de inte hello RoleEnvironment.Changing eller RoleEnvironment.Changed händelser.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
toodeploy ett program som en tjänst i molnet i Azure måste du första paketet hello programmet hello rätt format. Du kan använda hello **CSPack** kommandoradsverktyget (installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello paketfil som ett alternativt tooVisual Studio.

**CSPack** använder hello innehållet i hello service definition fil- och service configuration toodefine hello filinnehållet hello-paket. **CSPack** genererar ett filen med programpaketet som du kan ladda upp tooAzure (.cspkg) med hjälp av hello [Azure-portalen](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Som standard heter hello paketet `[ServiceDefinitionFileName].cspkg`, men du kan ange ett annat namn genom att använda hello `/out` möjlighet att **CSPack**.

**CSPack** finns på  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (på windows) är tillgängliga genom att köra hello **Kommandotolken för Microsoft Azure** genväg som installeras med hello SDK.  
> 
> Programmet hello CSPack.exe ensamt toosee dokumentationen om alla möjliga hello-växlar och kommandon.
> 
> 

<p />

> [!TIP]
> Kör Molntjänsten lokalt i hello **Microsoft Azure Compute Emulator**, använda hello **/copyonly** alternativet. Det här alternativet kopieras hello binära filer för hello tooa directory programlayout från vilken de kan köras i hello beräkningsemulatorn.
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a>Exempel kommandot toopackage en tjänst i molnet
hello skapas följande exempel ett programpaket som innehåller hello information för en webbroll. hello kommandot anger hello service definition filen toouse, hello katalogen där binära filer kan vara hitta och hello namnet på hello paketfilen.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Om programmet hello innehåller både en webbroll och en arbetsroll, används hello följande kommando:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Där hello variabler definieras enligt följande:

| Variabel | Värde |
| --- | --- |
| \[DirectoryName\] |hello underkatalog under hello projektets rotkatalog som innehåller hello .csdef-filen för hello Azure-projekt. |
| \[ServiceDefinition\] |hello namnet på hello tjänstdefinitionsfilen. Som standard är den här filen namnet ServiceDefinition.csdef. |
| \[Utdatafilnamn\] |hello namn för hello genereras paketfil. Detta anges vanligtvis toohello hello programmets namn. Om inget filnamn har angetts skapas hello programpaket som \[ApplicationName\].cspkg. |
| \[RoleName\] |hello namnet på hello roll som definierats i tjänstdefinitionsfilen hello. |
| \[RoleBinariesDirectory] |hello plats hello binära filer för hello roll. |
| \[VirtualPath\] |hello fysiska kataloger för varje virtuell sökväg som anges i hello platser i hello tjänstdefinitionen. |
| \[PhysicalPath\] |hello fysiska kataloger hello innehåll för varje virtuell sökväg som definierats i hello nod hello service definition. |
| \[RoleAssemblyName\] |hello namnet på hello binärfil för hello roll. |

## <a name="next-steps"></a>Nästa steg
Jag skapar en cloud service-paketet och vill...

* [Konfigurera Fjärrskrivbord för en tjänstinstans för molnet][remotedesktop]
* [Distribuera en Cloud Service-projekt][deploy]

Jag använder Visual Studio och vill...

* [Skapa en ny molntjänst][vs_create]
* [Konfigurera om en befintlig molntjänst][vs_reconfigure]
* [Distribuera en Cloud Service-projekt][vs_deploy]
* [Konfigurera Fjärrskrivbord för en tjänstinstans för molnet][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
