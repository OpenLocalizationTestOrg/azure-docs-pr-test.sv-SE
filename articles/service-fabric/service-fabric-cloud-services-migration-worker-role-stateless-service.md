---
title: aaaConvert Azure Cloud Services appar toomicroservices | Microsoft Docs
description: "Den här guiden jämför Cloud Services Web och arbetsroller och Service Fabric tillståndslösa tjänster toohelp migrera från molntjänster tooService Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a><span data-ttu-id="a1b63-103">Guide tooconverting webb- och arbetsroller tooService Fabric tillståndslösa tjänster</span><span class="sxs-lookup"><span data-stu-id="a1b63-103">Guide tooconverting Web and Worker Roles tooService Fabric stateless services</span></span>
<span data-ttu-id="a1b63-104">Den här artikeln beskriver hur toomigrate dina Cloud Services webb- och arbetsroller tooService Fabric tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="a1b63-104">This article describes how toomigrate your Cloud Services Web and Worker Roles tooService Fabric stateless services.</span></span> <span data-ttu-id="a1b63-105">Detta är hello enklaste migreringsvägen från molntjänster tooService Fabric för program vars övergripande arkitektur försätts toostay ungefär hello samma.</span><span class="sxs-lookup"><span data-stu-id="a1b63-105">This is hello simplest migration path from Cloud Services tooService Fabric for applications whose overall architecture is going toostay roughly hello same.</span></span>

## <a name="cloud-service-project-tooservice-fabric-application-project"></a><span data-ttu-id="a1b63-106">Cloud Service-projekt tooService Fabric application-projekt</span><span class="sxs-lookup"><span data-stu-id="a1b63-106">Cloud Service project tooService Fabric application project</span></span>
 <span data-ttu-id="a1b63-107">Cloud Service-projekt och ett tjänstprogram för Fabric-projekt har en liknande struktur och båda representerar hello distributionsenhet för ditt program – det vill säga de definiera fullständig hello-paket som är distribuerade toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="a1b63-107">A Cloud Service project and a Service Fabric Application project have a similar structure and both represent hello deployment unit for your application - that is, they each define hello complete package that is deployed toorun your application.</span></span> <span data-ttu-id="a1b63-108">En Cloud Service-projekt innehåller en eller flera rollerna webb eller arbetare.</span><span class="sxs-lookup"><span data-stu-id="a1b63-108">A Cloud Service project contains one or more Web or Worker Roles.</span></span> <span data-ttu-id="a1b63-109">På liknande sätt kan innehåller ett tjänstprogram för Fabric-projekt en eller flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="a1b63-109">Similarly, a Service Fabric Application project contains one or more services.</span></span> 

<span data-ttu-id="a1b63-110">hello skillnaden är att hello Cloud Service-projekt par hello programdistribution med en distribution av Virtuella datorer och därmed innehåller inställningar för virtuell dator, hello Service Fabric programprojekt bara definierar ett program som ska distribueras tooa uppsättning befintliga virtuella datorer i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="a1b63-110">hello difference is that hello Cloud Service project couples hello application deployment with a VM deployment and thus contains VM configuration settings in it, whereas hello Service Fabric Application project only defines an application that will be deployed tooa set of existing VMs in a Service Fabric cluster.</span></span> <span data-ttu-id="a1b63-111">hello Service Fabric själva klustret distribueras bara en gång, antingen via en Resource Manager-mall eller hello Azure-portalen och flera Service Fabric tillämpningsprogram kan vara distribuerade tooit.</span><span class="sxs-lookup"><span data-stu-id="a1b63-111">hello Service Fabric cluster itself is only deployed once, either through an Resource Manager template or through hello Azure portal, and multiple Service Fabric applications can be deployed tooit.</span></span>

![Jämförelse mellan Service Fabric och Cloud Services-projekt][3]

## <a name="worker-role-toostateless-service"></a><span data-ttu-id="a1b63-113">Worker toostateless rolltjänst</span><span class="sxs-lookup"><span data-stu-id="a1b63-113">Worker Role toostateless service</span></span>
<span data-ttu-id="a1b63-114">En Arbetsroll representerar begreppsmässigt en tillståndslös arbetsbelastning, vilket innebär att varje förekomst av hello arbetsbelastningen är identiska och begäranden kan vara routade tooany instans när som helst.</span><span class="sxs-lookup"><span data-stu-id="a1b63-114">Conceptually, a Worker Role represents a stateless workload, meaning every instance of hello workload is identical and requests can be routed tooany instance at any time.</span></span> <span data-ttu-id="a1b63-115">Varje instans är inte förväntade tooremember hello tidigare begäran.</span><span class="sxs-lookup"><span data-stu-id="a1b63-115">Each instance is not expected tooremember hello previous request.</span></span> <span data-ttu-id="a1b63-116">Tillstånd att hello arbetsbelastningar fungerar på hanteras av en extern tillståndslager, till exempel Azure Table Storage eller Azure-dokumentet DB.</span><span class="sxs-lookup"><span data-stu-id="a1b63-116">State that hello workload operates on is managed by an external state store, such as Azure Table Storage or Azure Document DB.</span></span> <span data-ttu-id="a1b63-117">Den här typen av arbetsbelastning representeras av en tjänst för tillståndslösa i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a1b63-117">In Service Fabric, this type of workload is represented by a Stateless Service.</span></span> <span data-ttu-id="a1b63-118">hello enklaste metoden toomigrating en Arbetsroll tooService Fabric kan göras genom att konvertera Arbetsrollen kod tooa tillståndslösa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a1b63-118">hello simplest approach toomigrating a Worker Role tooService Fabric can be done by converting Worker Role code tooa Stateless Service.</span></span>

![Worker-rollen tooStateless Service][4]

## <a name="web-role-toostateless-service"></a><span data-ttu-id="a1b63-120">Webbtjänsten rollen toostateless</span><span class="sxs-lookup"><span data-stu-id="a1b63-120">Web Role toostateless service</span></span>
<span data-ttu-id="a1b63-121">Liknande tooWorker roll, en Webbroll representerar också en tillståndslös arbetsbelastning och så begreppsmässigt för kan vara mappade tooa tillståndslösa Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a1b63-121">Similar tooWorker Role, a Web Role also represents a stateless workload, and so conceptually it too can be mapped tooa Service Fabric stateless service.</span></span> <span data-ttu-id="a1b63-122">Men till skillnad från webbroller stöder Service Fabric inte IIS.</span><span class="sxs-lookup"><span data-stu-id="a1b63-122">However, unlike Web Roles, Service Fabric does not support IIS.</span></span> <span data-ttu-id="a1b63-123">toomigrate ett program från en Webbroll tooa tillståndslösa tjänsten kräver första glidande tooa webbramverk som kan finnas själv och är inte beroende av IIS eller System.Web, till exempel ASP.NET Core 1.</span><span class="sxs-lookup"><span data-stu-id="a1b63-123">toomigrate a web application from a Web Role tooa stateless service requires first moving tooa web framework that can be self-hosted and does not depend on IIS or System.Web, such as ASP.NET Core 1.</span></span>

| <span data-ttu-id="a1b63-124">**Programmet**</span><span class="sxs-lookup"><span data-stu-id="a1b63-124">**Application**</span></span> | <span data-ttu-id="a1b63-125">**Stöds**</span><span class="sxs-lookup"><span data-stu-id="a1b63-125">**Supported**</span></span> | <span data-ttu-id="a1b63-126">**Sökväg för migrering**</span><span class="sxs-lookup"><span data-stu-id="a1b63-126">**Migration path**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1b63-127">ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="a1b63-127">ASP.NET Web Forms</span></span> |<span data-ttu-id="a1b63-128">Nej</span><span class="sxs-lookup"><span data-stu-id="a1b63-128">No</span></span> |<span data-ttu-id="a1b63-129">Konvertera tooASP.NET Core 1 MVC</span><span class="sxs-lookup"><span data-stu-id="a1b63-129">Convert tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="a1b63-130">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a1b63-130">ASP.NET MVC</span></span> |<span data-ttu-id="a1b63-131">Med migrering</span><span class="sxs-lookup"><span data-stu-id="a1b63-131">With Migration</span></span> |<span data-ttu-id="a1b63-132">Uppgradera tooASP.NET Core 1 MVC</span><span class="sxs-lookup"><span data-stu-id="a1b63-132">Upgrade tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="a1b63-133">ASP.NET Webb-API</span><span class="sxs-lookup"><span data-stu-id="a1b63-133">ASP.NET Web API</span></span> |<span data-ttu-id="a1b63-134">Med migrering</span><span class="sxs-lookup"><span data-stu-id="a1b63-134">With Migration</span></span> |<span data-ttu-id="a1b63-135">Använda automatisk värdbaserade servern eller ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="a1b63-135">Use self-hosted server or ASP.NET Core 1</span></span> |
| <span data-ttu-id="a1b63-136">ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="a1b63-136">ASP.NET Core 1</span></span> |<span data-ttu-id="a1b63-137">Ja</span><span class="sxs-lookup"><span data-stu-id="a1b63-137">Yes</span></span> |<span data-ttu-id="a1b63-138">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-138">N/A</span></span> |

## <a name="entry-point-api-and-lifecycle"></a><span data-ttu-id="a1b63-139">Posten punkt API och livscykel</span><span class="sxs-lookup"><span data-stu-id="a1b63-139">Entry point API and lifecycle</span></span>
<span data-ttu-id="a1b63-140">Worker-rollen och Service Fabric-API: er erbjuder liknande startpunkter:</span><span class="sxs-lookup"><span data-stu-id="a1b63-140">Worker Role and Service Fabric service APIs offer similar entry points:</span></span> 

| <span data-ttu-id="a1b63-141">**Startpunkt**</span><span class="sxs-lookup"><span data-stu-id="a1b63-141">**Entry Point**</span></span> | <span data-ttu-id="a1b63-142">**Worker-rollen**</span><span class="sxs-lookup"><span data-stu-id="a1b63-142">**Worker Role**</span></span> | <span data-ttu-id="a1b63-143">**Service Fabric-tjänsten**</span><span class="sxs-lookup"><span data-stu-id="a1b63-143">**Service Fabric service**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1b63-144">Bearbetning av</span><span class="sxs-lookup"><span data-stu-id="a1b63-144">Processing</span></span> |`Run()` |`RunAsync()` |
| <span data-ttu-id="a1b63-145">Starta VM</span><span class="sxs-lookup"><span data-stu-id="a1b63-145">VM start</span></span> |`OnStart()` |<span data-ttu-id="a1b63-146">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-146">N/A</span></span> |
| <span data-ttu-id="a1b63-147">Stoppa VM</span><span class="sxs-lookup"><span data-stu-id="a1b63-147">VM stop</span></span> |`OnStop()` |<span data-ttu-id="a1b63-148">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-148">N/A</span></span> |
| <span data-ttu-id="a1b63-149">Öppna lyssnaren för klientbegäranden</span><span class="sxs-lookup"><span data-stu-id="a1b63-149">Open listener for client requests</span></span> |<span data-ttu-id="a1b63-150">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-150">N/A</span></span> |<ul><li> <span data-ttu-id="a1b63-151">`CreateServiceInstanceListener()`för tillståndslösa</span><span class="sxs-lookup"><span data-stu-id="a1b63-151">`CreateServiceInstanceListener()` for stateless</span></span></li><li><span data-ttu-id="a1b63-152">`CreateServiceReplicaListener()`för stateful</span><span class="sxs-lookup"><span data-stu-id="a1b63-152">`CreateServiceReplicaListener()` for stateful</span></span></li></ul> |

### <a name="worker-role"></a><span data-ttu-id="a1b63-153">Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="a1b63-153">Worker Role</span></span>
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a><span data-ttu-id="a1b63-154">Service Fabric tillståndslösa tjänsten</span><span class="sxs-lookup"><span data-stu-id="a1b63-154">Service Fabric Stateless Service</span></span>
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

<span data-ttu-id="a1b63-155">Båda har en primär ”kör” åsidosättning toobegin behandling.</span><span class="sxs-lookup"><span data-stu-id="a1b63-155">Both have a primary "Run" override in which toobegin processing.</span></span> <span data-ttu-id="a1b63-156">Service Fabric services kombinera `Run`, `Start`, och `Stop` till en enda kontaktpunkt `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="a1b63-156">Service Fabric services  combine `Run`, `Start`, and `Stop` into a single entry point, `RunAsync`.</span></span> <span data-ttu-id="a1b63-157">Din tjänst ska börja arbeta när `RunAsync` startar och ska sluta fungera när hello `RunAsync` metodens CancellationToken signaleras.</span><span class="sxs-lookup"><span data-stu-id="a1b63-157">Your service should begin working when `RunAsync` starts, and should stop working when hello `RunAsync` method's CancellationToken is signaled.</span></span> 

<span data-ttu-id="a1b63-158">Det finns flera viktiga skillnader mellan hello livscykel och livslängden för arbetsroller och Service Fabric-tjänster:</span><span class="sxs-lookup"><span data-stu-id="a1b63-158">There are several key differences between hello lifecycle and lifetime of Worker Roles and Service Fabric services:</span></span>

* <span data-ttu-id="a1b63-159">**Livscykel:** hello största skillnaden är att en Arbetsroll är en virtuell dator och dess livscykel är därför bundet toohello VM, som innefattar händelser för när hello VM startar och stoppar.</span><span class="sxs-lookup"><span data-stu-id="a1b63-159">**Lifecycle:** hello biggest difference is that a Worker Role is a VM and so its lifecycle is tied toohello VM, which includes events for when hello VM starts and stops.</span></span> <span data-ttu-id="a1b63-160">En Service Fabric-tjänst har en livscykel som skiljer sig från hello VM livscykel, så att den inte innehåller händelser för när hello värddator VM eller datorn startar och stoppa, eftersom de inte är relaterade.</span><span class="sxs-lookup"><span data-stu-id="a1b63-160">A Service Fabric service has a lifecycle that is separate from hello VM lifecycle, so it does not include events for when hello host VM or machine starts and stop, as they are not related.</span></span>
* <span data-ttu-id="a1b63-161">**Livslängd:** Arbetsrollen-instansen kommer att återanvändas om hello `Run` metoden avslutar.</span><span class="sxs-lookup"><span data-stu-id="a1b63-161">**Lifetime:** A Worker Role instance will recycle if hello `Run` method exits.</span></span> <span data-ttu-id="a1b63-162">Hej `RunAsync` metod i en tjänst för Service Fabric men kan köra toocompletion och hello tjänstinstans ska fortsätta.</span><span class="sxs-lookup"><span data-stu-id="a1b63-162">hello `RunAsync` method in a Service Fabric service however can run toocompletion and hello service instance will stay up.</span></span> 

<span data-ttu-id="a1b63-163">Service Fabric ger en startpunkt för valfri kommunikation installationsprogrammet för tjänster som lyssnar efter förfrågningar från klienter.</span><span class="sxs-lookup"><span data-stu-id="a1b63-163">Service Fabric provides an optional communication setup entry point for services that listen for client requests.</span></span> <span data-ttu-id="a1b63-164">Både hello RunAsync och kommunikation startpunkten är valfria åsidosättningar i Service Fabric-services - tjänsten kan välja tooonly lyssna tooclient begäranden eller endast köra en loop i bearbetning eller båda - vilket är anledningen till hello RunAsync-metoden tillåts tooexit utan Starta om hello tjänstinstansen eftersom den kan fortsätta toolisten för klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="a1b63-164">Both hello RunAsync and communication entry point are optional overrides in Service Fabric services - your service may choose tooonly listen tooclient requests, or only run a processing loop, or both - which is why hello RunAsync method is allowed tooexit without restarting hello service instance, because it may continue toolisten for client requests.</span></span>

## <a name="application-api-and-environment"></a><span data-ttu-id="a1b63-165">Program-API och miljö</span><span class="sxs-lookup"><span data-stu-id="a1b63-165">Application API and environment</span></span>
<span data-ttu-id="a1b63-166">hello molntjänster miljö API innehåller information och funktioner för hello aktuella VM-instans, samt information om andra instanser för VM-roll.</span><span class="sxs-lookup"><span data-stu-id="a1b63-166">hello Cloud Services environment API provides information and functionality for hello current VM instance as well as information about other VM role instances.</span></span> <span data-ttu-id="a1b63-167">Service Fabric innehåller information som rör tooits runtime och viss information om hello nod en tjänst körs på.</span><span class="sxs-lookup"><span data-stu-id="a1b63-167">Service Fabric provides information related tooits runtime and some information about hello node a service is currently running on.</span></span> 

| <span data-ttu-id="a1b63-168">**Miljö aktivitet**</span><span class="sxs-lookup"><span data-stu-id="a1b63-168">**Environment Task**</span></span> | <span data-ttu-id="a1b63-169">**Cloud Services**</span><span class="sxs-lookup"><span data-stu-id="a1b63-169">**Cloud Services**</span></span> | <span data-ttu-id="a1b63-170">**Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="a1b63-170">**Service Fabric**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1b63-171">Konfigurationsinställningar och ändringsmeddelande</span><span class="sxs-lookup"><span data-stu-id="a1b63-171">Configuration Settings and change notification</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="a1b63-172">Lokal lagring</span><span class="sxs-lookup"><span data-stu-id="a1b63-172">Local Storage</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="a1b63-173">Slutpunkten</span><span class="sxs-lookup"><span data-stu-id="a1b63-173">Endpoint Information</span></span> |`RoleInstance` <ul><li><span data-ttu-id="a1b63-174">Aktuell instans:`RoleEnvironment.CurrentRoleInstance`</span><span class="sxs-lookup"><span data-stu-id="a1b63-174">Current instance: `RoleEnvironment.CurrentRoleInstance`</span></span></li><li><span data-ttu-id="a1b63-175">Andra roller och instans:`RoleEnvironment.Roles`</span><span class="sxs-lookup"><span data-stu-id="a1b63-175">Other roles and instance: `RoleEnvironment.Roles`</span></span></li> |<ul><li><span data-ttu-id="a1b63-176">`NodeContext`för den aktuella noden adressen</span><span class="sxs-lookup"><span data-stu-id="a1b63-176">`NodeContext` for current Node address</span></span></li><li><span data-ttu-id="a1b63-177">`FabricClient`och `ServicePartitionResolver` för identifiering av tjänst slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a1b63-177">`FabricClient` and `ServicePartitionResolver` for service endpoint discovery</span></span></li> |
| <span data-ttu-id="a1b63-178">Miljö emulering</span><span class="sxs-lookup"><span data-stu-id="a1b63-178">Environment Emulation</span></span> |`RoleEnvironment.IsEmulated` |<span data-ttu-id="a1b63-179">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-179">N/A</span></span> |
| <span data-ttu-id="a1b63-180">Samtidiga Ändringshändelse</span><span class="sxs-lookup"><span data-stu-id="a1b63-180">Simultaneous change event</span></span> |`RoleEnvironment` |<span data-ttu-id="a1b63-181">Saknas</span><span class="sxs-lookup"><span data-stu-id="a1b63-181">N/A</span></span> |

## <a name="configuration-settings"></a><span data-ttu-id="a1b63-182">Konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="a1b63-182">Configuration settings</span></span>
<span data-ttu-id="a1b63-183">Konfigurationsinställningarna i molntjänster har angetts för en VM-roll och tillämpa tooall instanser av den Virtuella datorrollen.</span><span class="sxs-lookup"><span data-stu-id="a1b63-183">Configuration settings in Cloud Services are set for a VM role and apply tooall instances of that VM role.</span></span> <span data-ttu-id="a1b63-184">De här inställningarna är nyckel / värde-par i ServiceConfiguration.*.cscfg filer och kan nås direkt via RoleEnvironment.</span><span class="sxs-lookup"><span data-stu-id="a1b63-184">These settings are key-value pairs set in ServiceConfiguration.*.cscfg files and can be accessed directly through RoleEnvironment.</span></span> <span data-ttu-id="a1b63-185">I Service Fabric inställningarna individuellt tooeach service och tooeach program snarare än tooa VM, eftersom en virtuell dator kan vara värd för flera tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="a1b63-185">In Service Fabric, settings apply individually tooeach service and tooeach application, rather than tooa VM, because a VM can host multiple services and applications.</span></span> <span data-ttu-id="a1b63-186">En tjänst består av tre paket:</span><span class="sxs-lookup"><span data-stu-id="a1b63-186">A service is composed of three packages:</span></span>

* <span data-ttu-id="a1b63-187">**Kod:** innehåller hello service körbara filer, binärfiler, DLL-filer och alla andra filer som en tjänst behöver toorun.</span><span class="sxs-lookup"><span data-stu-id="a1b63-187">**Code:** contains hello service executables, binaries, DLLs, and any other files a service needs toorun.</span></span>
* <span data-ttu-id="a1b63-188">**Config:** alla konfigurationsfiler och inställningarna för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="a1b63-188">**Config:** all configuration files and settings for a service.</span></span>
* <span data-ttu-id="a1b63-189">**Data:** Statiska filer som är associerade med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a1b63-189">**Data:** static data files associated with hello service.</span></span>

<span data-ttu-id="a1b63-190">Var och en av dessa paket kan vara oberoende versionsnummer och uppgraderade.</span><span class="sxs-lookup"><span data-stu-id="a1b63-190">Each of these packages can be independently versioned and upgraded.</span></span> <span data-ttu-id="a1b63-191">Liknande tooCloud tjänster, en konfigurationspaketet kan nås via programmering genom en API och händelser som är tillgängliga toonotify hello tjänsten av en ändring för config-paketet.</span><span class="sxs-lookup"><span data-stu-id="a1b63-191">Similar tooCloud Services, a config package can be accessed programmatically through an API and events are available toonotify hello service of a config package change.</span></span> <span data-ttu-id="a1b63-192">En Settings.xml-fil kan användas för nyckel / värde-konfiguration och Programmeringsåtkomst liknande toohello app inställningar för en App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="a1b63-192">A Settings.xml file can be used for key-value configuration and programmatic access similar toohello app settings section of an App.config file.</span></span> <span data-ttu-id="a1b63-193">Men till skillnad från molntjänster, kan ett Service Fabric config-paket innehålla konfigurationsfiler i valfritt format, oavsett om det är XML, JSON, YAML eller ett anpassat binärt format.</span><span class="sxs-lookup"><span data-stu-id="a1b63-193">However, unlike Cloud Services, a Service Fabric config package can contain any configuration files in any format, whether it's XML, JSON, YAML, or a custom binary format.</span></span> 

### <a name="accessing-configuration"></a><span data-ttu-id="a1b63-194">Åtkomst till konfiguration</span><span class="sxs-lookup"><span data-stu-id="a1b63-194">Accessing configuration</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="a1b63-195">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="a1b63-195">Cloud Services</span></span>
<span data-ttu-id="a1b63-196">Konfigurationsinställningar från ServiceConfiguration.*.cscfg kan nås via `RoleEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="a1b63-196">Configuration settings from ServiceConfiguration.*.cscfg can be accessed through `RoleEnvironment`.</span></span> <span data-ttu-id="a1b63-197">De här inställningarna är globalt tillgänglig tooall rollinstanser i hello distribution för samma tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="a1b63-197">These settings are globally available tooall role instances in hello same Cloud Service deployment.</span></span>

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a><span data-ttu-id="a1b63-198">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a1b63-198">Service Fabric</span></span>
<span data-ttu-id="a1b63-199">Varje tjänst har sin egen enskilda konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="a1b63-199">Each service has its own individual configuration package.</span></span> <span data-ttu-id="a1b63-200">Det finns ingen inbyggd mekanism för globala inställningar nås av alla program i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="a1b63-200">There is no built-in mechanism for global configuration settings accessible by all applications in a cluster.</span></span> <span data-ttu-id="a1b63-201">När du använder Service Fabric särskilda Settings.xml konfigurationsfilen inom ett konfigurationspaket kan värden i Settings.xml åsidosättas på hello programnivå gör det möjligt på programnivå konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="a1b63-201">When using Service Fabric's special Settings.xml configuration file within a configuration package, values in Settings.xml can be overwritten at hello application level, making application-level configuration settings possible.</span></span>

<span data-ttu-id="a1b63-202">Konfigurationsinställningarna är öppningar i varje tjänstinstans via hello tjänsten `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="a1b63-202">Configuration settings are accesses within each service instance through hello service's `CodePackageActivationContext`.</span></span>

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a><span data-ttu-id="a1b63-203">Update-konfigurationshändelser</span><span class="sxs-lookup"><span data-stu-id="a1b63-203">Configuration update events</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="a1b63-204">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="a1b63-204">Cloud Services</span></span>
<span data-ttu-id="a1b63-205">Hej `RoleEnvironment.Changed` händelsen är används toonotify alla rollinstanser när en ändring inträffar i hello miljö, till exempel en konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="a1b63-205">hello `RoleEnvironment.Changed` event is used toonotify all role instances when a change occurs in hello environment, such as a configuration change.</span></span> <span data-ttu-id="a1b63-206">Detta är används tooconsume konfigurationsuppdateringar utan återvinning rollinstanser eller omstart av en arbetsprocess.</span><span class="sxs-lookup"><span data-stu-id="a1b63-206">This is used tooconsume configuration updates without recycling role instances or restarting a worker process.</span></span>

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a><span data-ttu-id="a1b63-207">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a1b63-207">Service Fabric</span></span>
<span data-ttu-id="a1b63-208">Varje hello tre pakettyper i en tjänst - kod, konfiguration och Data - ha händelser som meddelar en tjänstinstans när ett paket uppdateras, lägga till eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="a1b63-208">Each of hello three package types in a service - Code, Config, and Data - have events that notify a service instance when a package is updated, added, or removed.</span></span> <span data-ttu-id="a1b63-209">En tjänst kan innehålla flera paket för varje typ.</span><span class="sxs-lookup"><span data-stu-id="a1b63-209">A service can contain multiple packages of each type.</span></span> <span data-ttu-id="a1b63-210">En tjänst kan till exempel ha flera config paket, varje enskilt version och kan uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="a1b63-210">For example, a service may have multiple config packages, each individually versioned and upgradeable.</span></span> 

<span data-ttu-id="a1b63-211">Dessa händelser finns tillgängliga tooconsume ändringar i servicepaket utan att starta om hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="a1b63-211">These events are available tooconsume changes in service packages without restarting hello service instance.</span></span>

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a><span data-ttu-id="a1b63-212">Start-uppgifter</span><span class="sxs-lookup"><span data-stu-id="a1b63-212">Startup tasks</span></span>
<span data-ttu-id="a1b63-213">Start-aktiviteter är åtgärder som vidtas innan ett program startas.</span><span class="sxs-lookup"><span data-stu-id="a1b63-213">Startup tasks are actions that are taken before an application starts.</span></span> <span data-ttu-id="a1b63-214">En startaktivitet är brukar användas toorun installationsprogrammet skript med utökade privilegier.</span><span class="sxs-lookup"><span data-stu-id="a1b63-214">A startup task is typically used toorun setup scripts using elevated privileges.</span></span> <span data-ttu-id="a1b63-215">Både molntjänster och Service Fabric stöder uppstart uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a1b63-215">Both Cloud Services and Service Fabric support start-up tasks.</span></span> <span data-ttu-id="a1b63-216">hello största skillnaden är att i Cloud Services, en startaktivitet är bundet tooa VM eftersom den är en del av en rollinstans i Service Fabric en startaktivitet är bundet tooa tjänsten, som inte är bundet tooany viss VM.</span><span class="sxs-lookup"><span data-stu-id="a1b63-216">hello main difference is that in Cloud Services, a startup task is tied tooa VM because it is part of a role instance, whereas in Service Fabric a startup task is tied tooa service, which is not tied tooany particular VM.</span></span>

| <span data-ttu-id="a1b63-217">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="a1b63-217">Cloud Services</span></span> | <span data-ttu-id="a1b63-218">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a1b63-218">Service Fabric</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1b63-219">Platsen för konfigurationen</span><span class="sxs-lookup"><span data-stu-id="a1b63-219">Configuration location</span></span> |<span data-ttu-id="a1b63-220">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="a1b63-220">ServiceDefinition.csdef</span></span> |
| <span data-ttu-id="a1b63-221">Privilegier</span><span class="sxs-lookup"><span data-stu-id="a1b63-221">Privileges</span></span> |<span data-ttu-id="a1b63-222">”begränsad” eller ”utökade”</span><span class="sxs-lookup"><span data-stu-id="a1b63-222">"limited" or "elevated"</span></span> |
| <span data-ttu-id="a1b63-223">Sekvensering</span><span class="sxs-lookup"><span data-stu-id="a1b63-223">Sequencing</span></span> |<span data-ttu-id="a1b63-224">”enkla”, ”bakgrund”, ”förgrunden”</span><span class="sxs-lookup"><span data-stu-id="a1b63-224">"simple", "background", "foreground"</span></span> |

### <a name="cloud-services"></a><span data-ttu-id="a1b63-225">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="a1b63-225">Cloud Services</span></span>
<span data-ttu-id="a1b63-226">I Cloud Services konfigureras en startpunkt för start per roll i ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="a1b63-226">In Cloud Services a startup entry point is configured per role in ServiceDefinition.csdef.</span></span> 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a><span data-ttu-id="a1b63-227">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a1b63-227">Service Fabric</span></span>
<span data-ttu-id="a1b63-228">I Service Fabric konfigureras en startpunkt för start per tjänst i ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="a1b63-228">In Service Fabric a startup entry point is configured per service in ServiceManifest.xml:</span></span>

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a><span data-ttu-id="a1b63-229">En anteckning om utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="a1b63-229">A note about development environment</span></span>
<span data-ttu-id="a1b63-230">Både molntjänster och Service Fabric är integrerade med Visual Studio med projektmallar och stöd för felsökning, konfigurera och distribuera både lokalt och tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a1b63-230">Both Cloud Services and Service Fabric are integrated with Visual Studio with project templates and support for debugging, configuring, and deploying both locally and tooAzure.</span></span> <span data-ttu-id="a1b63-231">Både molntjänster och Service Fabric ger också en körningsmiljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="a1b63-231">Both Cloud Services and Service Fabric also provide a local development runtime environment.</span></span> <span data-ttu-id="a1b63-232">hello är skillnaden att när hello Molntjänsten development runtime emulerar hello Azure-miljön där den körs, Service Fabric inte använder en emulator - hello fullständig Service Fabric runtime används.</span><span class="sxs-lookup"><span data-stu-id="a1b63-232">hello difference is that while hello Cloud Service development runtime emulates hello Azure environment on which it runs, Service Fabric does not use an emulator - it uses hello complete Service Fabric runtime.</span></span> <span data-ttu-id="a1b63-233">hello hello Service Fabric-miljö som du kör på utvecklingsdatorn lokala är samma miljö som körs i produktion.</span><span class="sxs-lookup"><span data-stu-id="a1b63-233">hello Service Fabric environment you run on your local development machine is hello same environment that runs in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1b63-234">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1b63-234">Next steps</span></span>
<span data-ttu-id="a1b63-235">Läs mer om Service Fabric Reliable Services och hello viktiga skillnader mellan Cloud Services och Service Fabric application arkitektur toounderstand hur tootake nytta av hello fullständig uppsättning funktioner för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a1b63-235">Read more about Service Fabric Reliable Services and hello fundamental differences between Cloud Services and Service Fabric application architecture toounderstand how tootake advantage of hello full set of Service Fabric features.</span></span>

* [<span data-ttu-id="a1b63-236">Komma igång med Service Fabric Reliable Services</span><span class="sxs-lookup"><span data-stu-id="a1b63-236">Getting started with Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="a1b63-237">Konceptuell guiden toohello skillnaderna mellan molntjänster och Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a1b63-237">Conceptual guide toohello differences between Cloud Services and Service Fabric</span></span>](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
