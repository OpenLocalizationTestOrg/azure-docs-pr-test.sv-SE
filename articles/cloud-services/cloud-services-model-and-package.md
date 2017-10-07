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
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a><span data-ttu-id="44b7f-103">Vad är hello Cloud Service model och hur jag paketera den?</span><span class="sxs-lookup"><span data-stu-id="44b7f-103">What is hello Cloud Service model and how do I package it?</span></span>
<span data-ttu-id="44b7f-104">En molnbaserad tjänst skapas från tre komponenter, hello tjänstdefinitionen *(.csdef)*, hello service config *(.cscfg)*, och inget tjänstepaket *(.cspkg)*.</span><span class="sxs-lookup"><span data-stu-id="44b7f-104">A cloud service is created from three components, hello service definition *(.csdef)*, hello service config *(.cscfg)*, and a service package *(.cspkg)*.</span></span> <span data-ttu-id="44b7f-105">Båda hello **ServiceDefinition.csdef** och **ServiceConfig.cscfg** filer är XML-baserade och beskriver hello struktur hello tjänst i molnet och hur den är konfigurerad, kallade hello modellen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-105">Both hello **ServiceDefinition.csdef** and **ServiceConfig.cscfg** files are XML-based and describe hello structure of hello cloud service and how it's configured; collectively called hello model.</span></span> <span data-ttu-id="44b7f-106">Hej **ServicePackage.cspkg** är en zip-fil som genereras från hello **ServiceDefinition.csdef** och bland annat innehåller alla hello krävs binary-beroenden.</span><span class="sxs-lookup"><span data-stu-id="44b7f-106">hello **ServicePackage.cspkg** is a zip file that is generated from hello **ServiceDefinition.csdef** and among other things, contains all hello required binary-based dependencies.</span></span> <span data-ttu-id="44b7f-107">Azure skapar en molnbaserad tjänst från båda hello **ServicePackage.cspkg** och hello **ServiceConfig.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="44b7f-107">Azure creates a cloud service from both hello **ServicePackage.cspkg** and hello **ServiceConfig.cscfg**.</span></span>

<span data-ttu-id="44b7f-108">När Molntjänsten hello körs i Azure kan du konfigurera om den via hello **ServiceConfig.cscfg** filen, men du kan inte ändra hello definition.</span><span class="sxs-lookup"><span data-stu-id="44b7f-108">Once hello cloud service is running in Azure, you can reconfigure it through hello **ServiceConfig.cscfg** file, but you cannot alter hello definition.</span></span>

## <a name="what-would-you-like-tooknow-more-about"></a><span data-ttu-id="44b7f-109">Vad vill du tooknow mer om?</span><span class="sxs-lookup"><span data-stu-id="44b7f-109">What would you like tooknow more about?</span></span>
* <span data-ttu-id="44b7f-110">Jag vill ha mer information om hello tooknow [ServiceDefinition.csdef](#csdef) och [ServiceConfig.cscfg](#cscfg) filer.</span><span class="sxs-lookup"><span data-stu-id="44b7f-110">I want tooknow more about hello [ServiceDefinition.csdef](#csdef) and [ServiceConfig.cscfg](#cscfg) files.</span></span>
* <span data-ttu-id="44b7f-111">Jag redan vet att ge mig [några exempel](#next-steps) på kan jag konfigurera.</span><span class="sxs-lookup"><span data-stu-id="44b7f-111">I already know about that, give me [some examples](#next-steps) on what I can configure.</span></span>
* <span data-ttu-id="44b7f-112">Jag vill toocreate hello [ServicePackage.cspkg](#cspkg).</span><span class="sxs-lookup"><span data-stu-id="44b7f-112">I want toocreate hello [ServicePackage.cspkg](#cspkg).</span></span>
* <span data-ttu-id="44b7f-113">Jag använder Visual Studio och vill...</span><span class="sxs-lookup"><span data-stu-id="44b7f-113">I am using Visual Studio and I want to...</span></span>
  * <span data-ttu-id="44b7f-114">[Skapa en molntjänst][vs_create]</span><span class="sxs-lookup"><span data-stu-id="44b7f-114">[Create a cloud service][vs_create]</span></span>
  * <span data-ttu-id="44b7f-115">[Konfigurera om en befintlig molntjänst][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="44b7f-115">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
  * <span data-ttu-id="44b7f-116">[Distribuera en Cloud Service-projekt][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="44b7f-116">[Deploy a Cloud Service project][vs_deploy]</span></span>
  * <span data-ttu-id="44b7f-117">[Fjärrskrivbord till en tjänstinstans för molnet][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="44b7f-117">[Remote desktop into a cloud service instance][remotedesktop]</span></span>

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a><span data-ttu-id="44b7f-118">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="44b7f-118">ServiceDefinition.csdef</span></span>
<span data-ttu-id="44b7f-119">Hej **ServiceDefinition.csdef** filen anger hello-inställningar som används av Azure tooconfigure en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="44b7f-119">hello **ServiceDefinition.csdef** file specifies hello settings that are used by Azure tooconfigure a cloud service.</span></span> <span data-ttu-id="44b7f-120">Hej [Azure Service Definition schemat (.csdef-fil)](https://msdn.microsoft.com/library/azure/ee758711.aspx) ger hello tillåtna format för en tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-120">hello [Azure Service Definition Schema (.csdef File)](https://msdn.microsoft.com/library/azure/ee758711.aspx) provides hello allowable format for a service definition file.</span></span> <span data-ttu-id="44b7f-121">hello visar följande exempel hello-inställningar som kan definieras för hello webb- och arbetsroller:</span><span class="sxs-lookup"><span data-stu-id="44b7f-121">hello following example shows hello settings that can be defined for hello Web and Worker roles:</span></span>

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

<span data-ttu-id="44b7f-122">Du kan se toohello [Service Definition schemat](https://msdn.microsoft.com/library/azure/ee758711.aspx) för en bättre förståelse av hello XML-schema används här, men här är en snabb förklaring av vissa hello element:</span><span class="sxs-lookup"><span data-stu-id="44b7f-122">You can refer toohello [Service Definition Schema](https://msdn.microsoft.com/library/azure/ee758711.aspx) for a better understanding of hello XML schema used here, however, here is a quick explanation of some of hello elements:</span></span>

<span data-ttu-id="44b7f-123">**Webbplatser**</span><span class="sxs-lookup"><span data-stu-id="44b7f-123">**Sites**</span></span>  
<span data-ttu-id="44b7f-124">Innehåller hello definitioner för webbplatser eller web program som finns i IIS7.</span><span class="sxs-lookup"><span data-stu-id="44b7f-124">Contains hello definitions for websites or web applications that are hosted in IIS7.</span></span>

<span data-ttu-id="44b7f-125">**InputEndpoints**</span><span class="sxs-lookup"><span data-stu-id="44b7f-125">**InputEndpoints**</span></span>  
<span data-ttu-id="44b7f-126">Innehåller hello definitioner för slutpunkter som är används toocontact hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-126">Contains hello definitions for endpoints that are used toocontact hello cloud service.</span></span>

<span data-ttu-id="44b7f-127">**InternalEndpoints**</span><span class="sxs-lookup"><span data-stu-id="44b7f-127">**InternalEndpoints**</span></span>  
<span data-ttu-id="44b7f-128">Innehåller hello definitioner för slutpunkter som används av rollen instanser toocommunicate med varandra.</span><span class="sxs-lookup"><span data-stu-id="44b7f-128">Contains hello definitions for endpoints that are used by role instances toocommunicate with each other.</span></span>

<span data-ttu-id="44b7f-129">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="44b7f-129">**ConfigurationSettings**</span></span>  
<span data-ttu-id="44b7f-130">Innehåller definitioner för hello inställningen för funktioner på en viss roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-130">Contains hello setting definitions for features of a specific role.</span></span>

<span data-ttu-id="44b7f-131">**Certifikat**</span><span class="sxs-lookup"><span data-stu-id="44b7f-131">**Certificates**</span></span>  
<span data-ttu-id="44b7f-132">Innehåller hello definitioner för certifikat som behövs för en roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-132">Contains hello definitions for certificates that are needed for a role.</span></span> <span data-ttu-id="44b7f-133">hello förra kodexemplet visar ett certifikat som används för att konfigurera hello Azure Connect.</span><span class="sxs-lookup"><span data-stu-id="44b7f-133">hello previous code example shows a certificate that is used for hello configuration of Azure Connect.</span></span>

<span data-ttu-id="44b7f-134">**LocalResources**</span><span class="sxs-lookup"><span data-stu-id="44b7f-134">**LocalResources**</span></span>  
<span data-ttu-id="44b7f-135">Innehåller hello definitioner för lokala lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="44b7f-135">Contains hello definitions for local storage resources.</span></span> <span data-ttu-id="44b7f-136">En lokal lagringsresurs är en reserverad katalog på hello filsystem hello virtuell dator som kör en instans av en roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-136">A local storage resource is a reserved directory on hello file system of hello virtual machine in which an instance of a role is running.</span></span>

<span data-ttu-id="44b7f-137">**Import**</span><span class="sxs-lookup"><span data-stu-id="44b7f-137">**Imports**</span></span>  
<span data-ttu-id="44b7f-138">Innehåller hello definitioner för importerade moduler.</span><span class="sxs-lookup"><span data-stu-id="44b7f-138">Contains hello definitions for imported modules.</span></span> <span data-ttu-id="44b7f-139">hello förra kodexemplet visar hello moduler för anslutning till fjärrskrivbord och Azure Connect.</span><span class="sxs-lookup"><span data-stu-id="44b7f-139">hello previous code example shows hello modules for Remote Desktop Connection and Azure Connect.</span></span>

<span data-ttu-id="44b7f-140">**Start**</span><span class="sxs-lookup"><span data-stu-id="44b7f-140">**Startup**</span></span>  
<span data-ttu-id="44b7f-141">Innehåller uppgifter som körs när hello roll startar.</span><span class="sxs-lookup"><span data-stu-id="44b7f-141">Contains tasks that are run when hello role starts.</span></span> <span data-ttu-id="44b7f-142">hello aktiviteter har definierats i en .cmd eller en körbar fil.</span><span class="sxs-lookup"><span data-stu-id="44b7f-142">hello tasks are defined in a .cmd or executable file.</span></span>

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a><span data-ttu-id="44b7f-143">ServiceConfiguration.cscfg</span><span class="sxs-lookup"><span data-stu-id="44b7f-143">ServiceConfiguration.cscfg</span></span>
<span data-ttu-id="44b7f-144">hello konfigurationen av hello inställningarna för din molntjänst bestäms av hello värden i hello **ServiceConfiguration.cscfg** fil.</span><span class="sxs-lookup"><span data-stu-id="44b7f-144">hello configuration of hello settings for your cloud service is determined by hello values in hello **ServiceConfiguration.cscfg** file.</span></span> <span data-ttu-id="44b7f-145">Du kan ange hello antalet instanser som du vill toodeploy för varje roll i den här filen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-145">You specify hello number of instances that you want toodeploy for each role in this file.</span></span> <span data-ttu-id="44b7f-146">hello värden för hello konfigurationsinställningar som du definierade i hello tjänstdefinitionsfilen läggs toohello tjänstkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-146">hello values for hello configuration settings that you defined in hello service definition file are added toohello service configuration file.</span></span> <span data-ttu-id="44b7f-147">hello tumavtryck för av hanteringscertifikat som är associerade med hello Molntjänsten läggs toohello fil.</span><span class="sxs-lookup"><span data-stu-id="44b7f-147">hello thumbprints for any management certificates that are associated with hello cloud service are also added toohello file.</span></span> <span data-ttu-id="44b7f-148">Hej [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx) ger hello tillåtna format för en konfigurationsfil för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-148">hello [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) provides hello allowable format for a service configuration file.</span></span>

<span data-ttu-id="44b7f-149">hello tjänstkonfigurationsfilen levereras inte med programmet hello, men är överförda tooAzure som en separat fil och använda tooconfigure hello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-149">hello service configuration file is not packaged with hello application, but is uploaded tooAzure as a separate file and is used tooconfigure hello cloud service.</span></span> <span data-ttu-id="44b7f-150">Du kan ladda upp en ny konfigurationsfil för tjänsten utan att omdistribuera din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="44b7f-150">You can upload a new service configuration file without redeploying your cloud service.</span></span> <span data-ttu-id="44b7f-151">hello konfigurationsvärden för hello Molntjänsten kan ändras när hello molnbaserad tjänst körs.</span><span class="sxs-lookup"><span data-stu-id="44b7f-151">hello configuration values for hello cloud service can be changed while hello cloud service is running.</span></span> <span data-ttu-id="44b7f-152">hello visar följande exempel hello konfigurationsinställningar som kan definieras för hello webb- och arbetsroller:</span><span class="sxs-lookup"><span data-stu-id="44b7f-152">hello following example shows hello configuration settings that can be defined for hello Web and Worker roles:</span></span>

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

<span data-ttu-id="44b7f-153">Du kan se toohello [Service Konfigurationsschemat](https://msdn.microsoft.com/library/azure/ee758710.aspx) för bättre förståelse hello XML-schema används här, men här är en snabb förklaring av hello element:</span><span class="sxs-lookup"><span data-stu-id="44b7f-153">You can refer toohello [Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710.aspx) for better understanding hello XML schema used here, however, here is a quick explanation of hello elements:</span></span>

<span data-ttu-id="44b7f-154">**Instanser**</span><span class="sxs-lookup"><span data-stu-id="44b7f-154">**Instances**</span></span>  
<span data-ttu-id="44b7f-155">Konfigurerar hello antalet instanser för hello rollen körs.</span><span class="sxs-lookup"><span data-stu-id="44b7f-155">Configures hello number of running instances for hello role.</span></span> <span data-ttu-id="44b7f-156">tooprevent ditt moln tjänsten potentiellt blir otillgänglig under uppgraderingar, bör du distribuera mer än en instans av web-riktade-roller.</span><span class="sxs-lookup"><span data-stu-id="44b7f-156">tooprevent your cloud service from potentially becoming unavailable during upgrades, it is recommended that you deploy more than one instance of your web-facing roles.</span></span> <span data-ttu-id="44b7f-157">Genom att distribuera mer än en instans du uppfyller toohello riktlinjerna i hello [Azure Compute serviceavtalet (SLA)](http://azure.microsoft.com/support/legal/sla/), vilket garanterar 99,95% extern anslutning för Internet-riktade roller när två eller flera roll instanser distribueras för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="44b7f-157">By deploying more than one instance, you are adhering toohello guidelines in hello [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), which guarantees 99.95% external connectivity for Internet-facing roles when two or more role instances are deployed for a service.</span></span>

<span data-ttu-id="44b7f-158">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="44b7f-158">**ConfigurationSettings**</span></span>  
<span data-ttu-id="44b7f-159">Konfigurerar hello inställningar för hello instanser för en roll som körs.</span><span class="sxs-lookup"><span data-stu-id="44b7f-159">Configures hello settings for hello running instances for a role.</span></span> <span data-ttu-id="44b7f-160">hello namnet på hello `<Setting>` element måste matcha hello inställningsdefinitioner i hello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-160">hello name of hello `<Setting>` elements must match hello setting definitions in hello service definition file.</span></span>

<span data-ttu-id="44b7f-161">**Certifikat**</span><span class="sxs-lookup"><span data-stu-id="44b7f-161">**Certificates**</span></span>  
<span data-ttu-id="44b7f-162">Konfigurerar hello-certifikat som används av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-162">Configures hello certificates that are used by hello service.</span></span> <span data-ttu-id="44b7f-163">hello förra kodexemplet visar hur toodefine hello certifikat för hello RemoteAccess modulen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-163">hello previous code example shows how toodefine hello certificate for hello RemoteAccess module.</span></span> <span data-ttu-id="44b7f-164">Hej värdet för hello *tumavtrycket* attributet måste anges toohello tumavtryck hello certifikat toouse.</span><span class="sxs-lookup"><span data-stu-id="44b7f-164">hello value of hello *thumbprint* attribute must be set toohello thumbprint of hello certificate toouse.</span></span>

<p/>

> [!NOTE]
> <span data-ttu-id="44b7f-165">hello tumavtrycket för certifikatet för hello läggas toohello konfigurationsfilen med hjälp av en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="44b7f-165">hello thumbprint for hello certificate can be added toohello configuration file by using a text editor.</span></span> <span data-ttu-id="44b7f-166">Eller hello värde kan läggas till på hello **certifikat** för hello **egenskaper** sidan hello roll i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44b7f-166">Or, hello value can be added on hello **Certificates** tab of hello **Properties** page of hello role in Visual Studio.</span></span>
> 
> 

## <a name="defining-ports-for-role-instances"></a><span data-ttu-id="44b7f-167">Definiera portar för rollinstanser</span><span class="sxs-lookup"><span data-stu-id="44b7f-167">Defining ports for role instances</span></span>
<span data-ttu-id="44b7f-168">Azure kan endast en post punkt tooa webbroll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-168">Azure allows only one entry point tooa web role.</span></span> <span data-ttu-id="44b7f-169">Vilket innebär att all trafik sker via en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="44b7f-169">Meaning that all traffic occurs through one IP address.</span></span> <span data-ttu-id="44b7f-170">Du kan konfigurera din webbplatser tooshare en port genom att konfigurera hello värden huvud toodirect hello begäran toohello rätt plats.</span><span class="sxs-lookup"><span data-stu-id="44b7f-170">You can configure your websites tooshare a port by configuring hello host header toodirect hello request toohello correct location.</span></span> <span data-ttu-id="44b7f-171">Du kan också konfigurera ditt program toolisten toowell-portar på hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="44b7f-171">You can also configure your applications toolisten toowell-known ports on hello IP address.</span></span>

<span data-ttu-id="44b7f-172">hello visar följande exempel hello konfiguration för en webbroll med ett webbplatsen och program.</span><span class="sxs-lookup"><span data-stu-id="44b7f-172">hello following sample shows hello configuration for a web role with a website and web application.</span></span> <span data-ttu-id="44b7f-173">hello-webbplatsen är konfigurerad som hello post standardplatsen på port 80 och hello webbprogram är konfigurerade tooreceive begäranden från ett huvud för alternativa värden som kallas ”mail.mysite.cloudapp.net”.</span><span class="sxs-lookup"><span data-stu-id="44b7f-173">hello website is configured as hello default entry location on port 80, and hello web applications are configured tooreceive requests from an alternate host header that is called “mail.mysite.cloudapp.net”.</span></span>

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


## <a name="changing-hello-configuration-of-a-role"></a><span data-ttu-id="44b7f-174">Ändra hello konfiguration av en roll</span><span class="sxs-lookup"><span data-stu-id="44b7f-174">Changing hello configuration of a role</span></span>
<span data-ttu-id="44b7f-175">Du kan uppdatera hello konfigurationen av din molntjänst när den körs i Azure, utan att koppla hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-175">You can update hello configuration of your cloud service while it is running in Azure, without taking hello service offline.</span></span> <span data-ttu-id="44b7f-176">toochange konfigurationsinformation, du kan antingen ladda upp en ny konfigurationsfil, eller redigera hello-konfigurationsfilen i placera och tillämpa det tooyour som kör tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44b7f-176">toochange configuration information, you can either upload a new configuration file, or edit hello configuration file in place and apply it tooyour running service.</span></span> <span data-ttu-id="44b7f-177">hello kan följande ändringar göras toohello konfigurationen för en tjänst:</span><span class="sxs-lookup"><span data-stu-id="44b7f-177">hello following changes can be made toohello configuration of a service:</span></span>

* <span data-ttu-id="44b7f-178">**Ändra hello värdena för konfigurationsinställningar**</span><span class="sxs-lookup"><span data-stu-id="44b7f-178">**Changing hello values of configuration settings**</span></span>  
  <span data-ttu-id="44b7f-179">När en konfiguration kan ändras, en rollinstans välja tooapply hello ändringen när hello-instansen är online eller toorecycle hello instans smidigt och hello ändringen gälla när hello-instans är offline.</span><span class="sxs-lookup"><span data-stu-id="44b7f-179">When a configuration setting changes, a role instance can choose tooapply hello change while hello instance is online, or toorecycle hello instance gracefully and apply hello change while hello instance is offline.</span></span>
* <span data-ttu-id="44b7f-180">**Ändra hello service topologi rollinstanser**</span><span class="sxs-lookup"><span data-stu-id="44b7f-180">**Changing hello service topology of role instances**</span></span>  
  <span data-ttu-id="44b7f-181">Ändringar i nätverkstopologin påverkar inte instanser, utom där en instans tas bort.</span><span class="sxs-lookup"><span data-stu-id="44b7f-181">Topology changes do not affect running instances, except where an instance is being removed.</span></span> <span data-ttu-id="44b7f-182">Alla återstående instanser normalt behöver inte toobe återvinns; Du kan dock toorecycle rollinstanser i svaret tooa topologi förändras.</span><span class="sxs-lookup"><span data-stu-id="44b7f-182">All remaining instances generally do not need toobe recycled; however, you can choose toorecycle role instances in response tooa topology change.</span></span>
* <span data-ttu-id="44b7f-183">**Ändra hello certifikatets tumavtryck**</span><span class="sxs-lookup"><span data-stu-id="44b7f-183">**Changing hello certificate thumbprint**</span></span>  
  <span data-ttu-id="44b7f-184">Du kan bara uppdatera certifikat när en rollinstans är offline.</span><span class="sxs-lookup"><span data-stu-id="44b7f-184">You can only update a certificate when a role instance is offline.</span></span> <span data-ttu-id="44b7f-185">Om ett certifikat läggs till, tas bort eller ändras medan en rollinstans är online, tar hello instanscertifikatet offline tooupdate hello Azure smidigt och göra den online när hello ändringen har utförts.</span><span class="sxs-lookup"><span data-stu-id="44b7f-185">If a certificate is added, deleted, or changed while a role instance is online, Azure gracefully takes hello instance offline tooupdate hello certificate and bring it back online after hello change is complete.</span></span>

### <a name="handling-configuration-changes-with-service-runtime-events"></a><span data-ttu-id="44b7f-186">Konfigurationsändringar för hantering med körning av tjänsten-händelser</span><span class="sxs-lookup"><span data-stu-id="44b7f-186">Handling configuration changes with Service Runtime Events</span></span>
<span data-ttu-id="44b7f-187">Hej [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) innehåller hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namnområdet, som innehåller klasser för att interagera med hello Azure-miljön från en roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-187">hello [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) includes hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namespace, which provides classes for interacting with hello Azure environment from a role.</span></span> <span data-ttu-id="44b7f-188">Hej [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definierar hello följande händelser som är före och efter en konfigurationsändring i klassen:</span><span class="sxs-lookup"><span data-stu-id="44b7f-188">hello [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) class defines hello following events that are raised before and after a configuration change:</span></span>

* <span data-ttu-id="44b7f-189">**[Ändra](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) händelse**</span><span class="sxs-lookup"><span data-stu-id="44b7f-189">**[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) event**</span></span>  
  <span data-ttu-id="44b7f-190">Detta inträffar innan hello konfigurationsändring är tillämpade tooa angivna instans av en roll som ger en möjlighet tootake ned hello rollinstanser om det behövs.</span><span class="sxs-lookup"><span data-stu-id="44b7f-190">This occurs before hello configuration change is applied tooa specified instance of a role giving you a chance tootake down hello role instances if necessary.</span></span>
* <span data-ttu-id="44b7f-191">**[Ändra](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) händelse**</span><span class="sxs-lookup"><span data-stu-id="44b7f-191">**[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) event**</span></span>  
  <span data-ttu-id="44b7f-192">Inträffar när hello konfigurationsändring är tillämpade tooa angivna instans av en roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-192">Occurs after hello configuration change is applied tooa specified instance of a role.</span></span>

> [!NOTE]
> <span data-ttu-id="44b7f-193">Eftersom certifikatet ändringar ta alltid hello instanser av en roll som är offline, öka de inte hello RoleEnvironment.Changing eller RoleEnvironment.Changed händelser.</span><span class="sxs-lookup"><span data-stu-id="44b7f-193">Because certificate changes always take hello instances of a role offline, they do not raise hello RoleEnvironment.Changing or RoleEnvironment.Changed events.</span></span>
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a><span data-ttu-id="44b7f-194">ServicePackage.cspkg</span><span class="sxs-lookup"><span data-stu-id="44b7f-194">ServicePackage.cspkg</span></span>
<span data-ttu-id="44b7f-195">toodeploy ett program som en tjänst i molnet i Azure måste du första paketet hello programmet hello rätt format.</span><span class="sxs-lookup"><span data-stu-id="44b7f-195">toodeploy an application as a cloud service in Azure, you must first package hello application in hello appropriate format.</span></span> <span data-ttu-id="44b7f-196">Du kan använda hello **CSPack** kommandoradsverktyget (installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello paketfil som ett alternativt tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="44b7f-196">You can use hello **CSPack** command-line tool (installed with hello [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello package file as an alternative tooVisual Studio.</span></span>

<span data-ttu-id="44b7f-197">**CSPack** använder hello innehållet i hello service definition fil- och service configuration toodefine hello filinnehållet hello-paket.</span><span class="sxs-lookup"><span data-stu-id="44b7f-197">**CSPack** uses hello contents of hello service definition file and service configuration file toodefine hello contents of hello package.</span></span> <span data-ttu-id="44b7f-198">**CSPack** genererar ett filen med programpaketet som du kan ladda upp tooAzure (.cspkg) med hjälp av hello [Azure-portalen](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span><span class="sxs-lookup"><span data-stu-id="44b7f-198">**CSPack** generates an application package file (.cspkg) that you can upload tooAzure by using hello [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span></span> <span data-ttu-id="44b7f-199">Som standard heter hello paketet `[ServiceDefinitionFileName].cspkg`, men du kan ange ett annat namn genom att använda hello `/out` möjlighet att **CSPack**.</span><span class="sxs-lookup"><span data-stu-id="44b7f-199">By default, hello package is named `[ServiceDefinitionFileName].cspkg`, but you can specify a different name by using hello `/out` option of **CSPack**.</span></span>

<span data-ttu-id="44b7f-200">**CSPack** finns på</span><span class="sxs-lookup"><span data-stu-id="44b7f-200">**CSPack** is located at</span></span>  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> <span data-ttu-id="44b7f-201">CSPack.exe (på windows) är tillgängliga genom att köra hello **Kommandotolken för Microsoft Azure** genväg som installeras med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="44b7f-201">CSPack.exe (on windows) is available by running hello **Microsoft Azure Command Prompt** shortcut that is installed with hello SDK.</span></span>  
> 
> <span data-ttu-id="44b7f-202">Programmet hello CSPack.exe ensamt toosee dokumentationen om alla möjliga hello-växlar och kommandon.</span><span class="sxs-lookup"><span data-stu-id="44b7f-202">Run hello CSPack.exe program by itself toosee documentation about all hello possible switches and commands.</span></span>
> 
> 

<p />

> [!TIP]
> <span data-ttu-id="44b7f-203">Kör Molntjänsten lokalt i hello **Microsoft Azure Compute Emulator**, använda hello **/copyonly** alternativet.</span><span class="sxs-lookup"><span data-stu-id="44b7f-203">Run your cloud service locally in hello **Microsoft Azure Compute Emulator**, use hello **/copyonly** option.</span></span> <span data-ttu-id="44b7f-204">Det här alternativet kopieras hello binära filer för hello tooa directory programlayout från vilken de kan köras i hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="44b7f-204">This option copies hello binary files for hello application tooa directory layout from which they can be run in hello compute emulator.</span></span>
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a><span data-ttu-id="44b7f-205">Exempel kommandot toopackage en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="44b7f-205">Example command toopackage a cloud service</span></span>
<span data-ttu-id="44b7f-206">hello skapas följande exempel ett programpaket som innehåller hello information för en webbroll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-206">hello following example creates an application package that contains hello information for a web role.</span></span> <span data-ttu-id="44b7f-207">hello kommandot anger hello service definition filen toouse, hello katalogen där binära filer kan vara hitta och hello namnet på hello paketfilen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-207">hello command specifies hello service definition file toouse, hello directory where binary files can be found, and hello name of hello package file.</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

<span data-ttu-id="44b7f-208">Om programmet hello innehåller både en webbroll och en arbetsroll, används hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="44b7f-208">If hello application contains both a web role and a worker role, hello following command is used:</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

<span data-ttu-id="44b7f-209">Där hello variabler definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="44b7f-209">Where hello variables are defined as follows:</span></span>

| <span data-ttu-id="44b7f-210">Variabel</span><span class="sxs-lookup"><span data-stu-id="44b7f-210">Variable</span></span> | <span data-ttu-id="44b7f-211">Värde</span><span class="sxs-lookup"><span data-stu-id="44b7f-211">Value</span></span> |
| --- | --- |
| <span data-ttu-id="44b7f-212">\[DirectoryName\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-212">\[DirectoryName\]</span></span> |<span data-ttu-id="44b7f-213">hello underkatalog under hello projektets rotkatalog som innehåller hello .csdef-filen för hello Azure-projekt.</span><span class="sxs-lookup"><span data-stu-id="44b7f-213">hello subdirectory under hello root project directory that contains hello .csdef file of hello Azure project.</span></span> |
| <span data-ttu-id="44b7f-214">\[ServiceDefinition\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-214">\[ServiceDefinition\]</span></span> |<span data-ttu-id="44b7f-215">hello namnet på hello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-215">hello name of hello service definition file.</span></span> <span data-ttu-id="44b7f-216">Som standard är den här filen namnet ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="44b7f-216">By default, this file is named ServiceDefinition.csdef.</span></span> |
| <span data-ttu-id="44b7f-217">\[Utdatafilnamn\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-217">\[OutputFileName\]</span></span> |<span data-ttu-id="44b7f-218">hello namn för hello genereras paketfil.</span><span class="sxs-lookup"><span data-stu-id="44b7f-218">hello name for hello generated package file.</span></span> <span data-ttu-id="44b7f-219">Detta anges vanligtvis toohello hello programmets namn.</span><span class="sxs-lookup"><span data-stu-id="44b7f-219">Typically, this is set toohello name of hello application.</span></span> <span data-ttu-id="44b7f-220">Om inget filnamn har angetts skapas hello programpaket som \[ApplicationName\].cspkg.</span><span class="sxs-lookup"><span data-stu-id="44b7f-220">If no file name is specified, hello application package is created as \[ApplicationName\].cspkg.</span></span> |
| <span data-ttu-id="44b7f-221">\[RoleName\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-221">\[RoleName\]</span></span> |<span data-ttu-id="44b7f-222">hello namnet på hello roll som definierats i tjänstdefinitionsfilen hello.</span><span class="sxs-lookup"><span data-stu-id="44b7f-222">hello name of hello role as defined in hello service definition file.</span></span> |
| <span data-ttu-id="44b7f-223">\[RoleBinariesDirectory]</span><span class="sxs-lookup"><span data-stu-id="44b7f-223">\[RoleBinariesDirectory]</span></span> |<span data-ttu-id="44b7f-224">hello plats hello binära filer för hello roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-224">hello location of hello binary files for hello role.</span></span> |
| <span data-ttu-id="44b7f-225">\[VirtualPath\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-225">\[VirtualPath\]</span></span> |<span data-ttu-id="44b7f-226">hello fysiska kataloger för varje virtuell sökväg som anges i hello platser i hello tjänstdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="44b7f-226">hello physical directories for each virtual path defined in hello Sites section of hello service definition.</span></span> |
| <span data-ttu-id="44b7f-227">\[PhysicalPath\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-227">\[PhysicalPath\]</span></span> |<span data-ttu-id="44b7f-228">hello fysiska kataloger hello innehåll för varje virtuell sökväg som definierats i hello nod hello service definition.</span><span class="sxs-lookup"><span data-stu-id="44b7f-228">hello physical directories of hello contents for each virtual path defined in hello site node of hello service definition.</span></span> |
| <span data-ttu-id="44b7f-229">\[RoleAssemblyName\]</span><span class="sxs-lookup"><span data-stu-id="44b7f-229">\[RoleAssemblyName\]</span></span> |<span data-ttu-id="44b7f-230">hello namnet på hello binärfil för hello roll.</span><span class="sxs-lookup"><span data-stu-id="44b7f-230">hello name of hello binary file for hello role.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="44b7f-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44b7f-231">Next steps</span></span>
<span data-ttu-id="44b7f-232">Jag skapar en cloud service-paketet och vill...</span><span class="sxs-lookup"><span data-stu-id="44b7f-232">I'm creating a cloud service package and I want to...</span></span>

* <span data-ttu-id="44b7f-233">[Konfigurera Fjärrskrivbord för en tjänstinstans för molnet][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="44b7f-233">[Setup remote desktop for a cloud service instance][remotedesktop]</span></span>
* <span data-ttu-id="44b7f-234">[Distribuera en Cloud Service-projekt][deploy]</span><span class="sxs-lookup"><span data-stu-id="44b7f-234">[Deploy a Cloud Service project][deploy]</span></span>

<span data-ttu-id="44b7f-235">Jag använder Visual Studio och vill...</span><span class="sxs-lookup"><span data-stu-id="44b7f-235">I am using Visual Studio and I want to...</span></span>

* <span data-ttu-id="44b7f-236">[Skapa en ny molntjänst][vs_create]</span><span class="sxs-lookup"><span data-stu-id="44b7f-236">[Create a new cloud service][vs_create]</span></span>
* <span data-ttu-id="44b7f-237">[Konfigurera om en befintlig molntjänst][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="44b7f-237">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
* <span data-ttu-id="44b7f-238">[Distribuera en Cloud Service-projekt][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="44b7f-238">[Deploy a Cloud Service project][vs_deploy]</span></span>
* <span data-ttu-id="44b7f-239">[Konfigurera Fjärrskrivbord för en tjänstinstans för molnet][vs_remote]</span><span class="sxs-lookup"><span data-stu-id="44b7f-239">[Setup remote desktop for a cloud service instance][vs_remote]</span></span>

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
