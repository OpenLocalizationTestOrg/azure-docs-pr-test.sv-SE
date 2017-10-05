---
title: Cloud Services-rollen config XPath fusklapp | Microsoft Docs
description: "Olika XPath-inställningar du kan använda i cloud service rollen config för att exponera inställningar som en miljövariabel."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: fd6efac829d3fd9e2840362b8d2ff423add566d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a><span data-ttu-id="e6138-103">Exponera konfigurationsinställningar för rollen som en miljövariabel med XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-103">Expose role configuration settings as an environment variable with XPath</span></span>
<span data-ttu-id="e6138-104">I cloud service worker eller web rollen tjänstdefinitionsfilen exponera du runtime konfigurationsvärden som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="e6138-104">In the cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables.</span></span> <span data-ttu-id="e6138-105">Följande XPath-värden stöds (som motsvarar API värden).</span><span class="sxs-lookup"><span data-stu-id="e6138-105">The following XPath values are supported (which correspond to API values).</span></span>

<span data-ttu-id="e6138-106">Värdena XPath finns också tillgängliga via den [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="e6138-106">These XPath values are also available through the [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library.</span></span> 

## <a name="app-running-in-emulator"></a><span data-ttu-id="e6138-107">Appen körs i emulatorn</span><span class="sxs-lookup"><span data-stu-id="e6138-107">App running in emulator</span></span>
<span data-ttu-id="e6138-108">Anger att programmet körs i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="e6138-108">Indicates that the app is running in the emulator.</span></span>

| <span data-ttu-id="e6138-109">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-109">Type</span></span> | <span data-ttu-id="e6138-110">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-110">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-111">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-111">XPath</span></span> |<span data-ttu-id="e6138-112">XPath = ”/RoleEnvironment/Deployment/@emulated”</span><span class="sxs-lookup"><span data-stu-id="e6138-112">xpath="/RoleEnvironment/Deployment/@emulated"</span></span> |
| <span data-ttu-id="e6138-113">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-113">Code</span></span> |<span data-ttu-id="e6138-114">var x = RoleEnvironment.IsEmulated;</span><span class="sxs-lookup"><span data-stu-id="e6138-114">var x = RoleEnvironment.IsEmulated;</span></span> |

## <a name="deployment-id"></a><span data-ttu-id="e6138-115">Distributions-ID</span><span class="sxs-lookup"><span data-stu-id="e6138-115">Deployment ID</span></span>
<span data-ttu-id="e6138-116">Hämtar distributions-ID för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-116">Retrieves the deployment ID for the instance.</span></span>

| <span data-ttu-id="e6138-117">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-117">Type</span></span> | <span data-ttu-id="e6138-118">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-118">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-119">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-119">XPath</span></span> |<span data-ttu-id="e6138-120">XPath = ”/RoleEnvironment/Deployment/@id”</span><span class="sxs-lookup"><span data-stu-id="e6138-120">xpath="/RoleEnvironment/Deployment/@id"</span></span> |
| <span data-ttu-id="e6138-121">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-121">Code</span></span> |<span data-ttu-id="e6138-122">var deploymentId = RoleEnvironment.DeploymentId;</span><span class="sxs-lookup"><span data-stu-id="e6138-122">var deploymentId = RoleEnvironment.DeploymentId;</span></span> |

## <a name="role-id"></a><span data-ttu-id="e6138-123">Roll-ID</span><span class="sxs-lookup"><span data-stu-id="e6138-123">Role ID</span></span>
<span data-ttu-id="e6138-124">Hämtar den aktuella roll-ID för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-124">Retrieves the current role ID for the instance.</span></span>

| <span data-ttu-id="e6138-125">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-125">Type</span></span> | <span data-ttu-id="e6138-126">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-126">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-127">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-127">XPath</span></span> |<span data-ttu-id="e6138-128">XPath = ”/RoleEnvironment/CurrentInstance/@id”</span><span class="sxs-lookup"><span data-stu-id="e6138-128">xpath="/RoleEnvironment/CurrentInstance/@id"</span></span> |
| <span data-ttu-id="e6138-129">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-129">Code</span></span> |<span data-ttu-id="e6138-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span><span class="sxs-lookup"><span data-stu-id="e6138-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span></span> |

## <a name="update-domain"></a><span data-ttu-id="e6138-131">Uppdatera domänen</span><span class="sxs-lookup"><span data-stu-id="e6138-131">Update domain</span></span>
<span data-ttu-id="e6138-132">Hämtar uppdateringsdomän för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-132">Retrieves the update domain of the instance.</span></span>

| <span data-ttu-id="e6138-133">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-133">Type</span></span> | <span data-ttu-id="e6138-134">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-134">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-135">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-135">XPath</span></span> |<span data-ttu-id="e6138-136">XPath = ”/RoleEnvironment/CurrentInstance/@updateDomain”</span><span class="sxs-lookup"><span data-stu-id="e6138-136">xpath="/RoleEnvironment/CurrentInstance/@updateDomain"</span></span> |
| <span data-ttu-id="e6138-137">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-137">Code</span></span> |<span data-ttu-id="e6138-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span><span class="sxs-lookup"><span data-stu-id="e6138-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span></span> |

## <a name="fault-domain"></a><span data-ttu-id="e6138-139">feldomän</span><span class="sxs-lookup"><span data-stu-id="e6138-139">Fault domain</span></span>
<span data-ttu-id="e6138-140">Hämtar feldomänen för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-140">Retrieves the fault domain of the instance.</span></span>

| <span data-ttu-id="e6138-141">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-141">Type</span></span> | <span data-ttu-id="e6138-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-142">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-143">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-143">XPath</span></span> |<span data-ttu-id="e6138-144">XPath = ”/RoleEnvironment/CurrentInstance/@faultDomain”</span><span class="sxs-lookup"><span data-stu-id="e6138-144">xpath="/RoleEnvironment/CurrentInstance/@faultDomain"</span></span> |
| <span data-ttu-id="e6138-145">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-145">Code</span></span> |<span data-ttu-id="e6138-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span><span class="sxs-lookup"><span data-stu-id="e6138-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span></span> |

## <a name="role-name"></a><span data-ttu-id="e6138-147">Rollnamn</span><span class="sxs-lookup"><span data-stu-id="e6138-147">Role name</span></span>
<span data-ttu-id="e6138-148">Hämtar rollnamnet av instanserna.</span><span class="sxs-lookup"><span data-stu-id="e6138-148">Retrieves the role name of the instances.</span></span>

| <span data-ttu-id="e6138-149">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-149">Type</span></span> | <span data-ttu-id="e6138-150">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-150">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-151">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-151">XPath</span></span> |<span data-ttu-id="e6138-152">XPath = ”/RoleEnvironment/CurrentInstance/@roleName”</span><span class="sxs-lookup"><span data-stu-id="e6138-152">xpath="/RoleEnvironment/CurrentInstance/@roleName"</span></span> |
| <span data-ttu-id="e6138-153">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-153">Code</span></span> |<span data-ttu-id="e6138-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span><span class="sxs-lookup"><span data-stu-id="e6138-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span></span> |

## <a name="config-setting"></a><span data-ttu-id="e6138-155">Konfigurationsinställningen</span><span class="sxs-lookup"><span data-stu-id="e6138-155">Config setting</span></span>
<span data-ttu-id="e6138-156">Hämtar värdet för den angivna Konfigurationsinställningen.</span><span class="sxs-lookup"><span data-stu-id="e6138-156">Retrieves the value of the specified configuration setting.</span></span>

| <span data-ttu-id="e6138-157">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-157">Type</span></span> | <span data-ttu-id="e6138-158">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-158">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-159">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-159">XPath</span></span> |<span data-ttu-id="e6138-160">XPath = ”/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value”</span><span class="sxs-lookup"><span data-stu-id="e6138-160">xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value"</span></span> |
| <span data-ttu-id="e6138-161">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-161">Code</span></span> |<span data-ttu-id="e6138-162">var inställningen = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span><span class="sxs-lookup"><span data-stu-id="e6138-162">var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span></span> |

## <a name="local-storage-path"></a><span data-ttu-id="e6138-163">Sökvägen till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="e6138-163">Local storage path</span></span>
<span data-ttu-id="e6138-164">Hämtar lokal lagringssökvägen för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-164">Retrieves the local storage path for the instance.</span></span>

| <span data-ttu-id="e6138-165">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-165">Type</span></span> | <span data-ttu-id="e6138-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-166">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-167">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-167">XPath</span></span> |<span data-ttu-id="e6138-168">XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path”</span><span class="sxs-lookup"><span data-stu-id="e6138-168">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path"</span></span> |
| <span data-ttu-id="e6138-169">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-169">Code</span></span> |<span data-ttu-id="e6138-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath;</span><span class="sxs-lookup"><span data-stu-id="e6138-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1").RootPath;</span></span> |

## <a name="local-storage-size"></a><span data-ttu-id="e6138-171">Lokal lagringsstorlek</span><span class="sxs-lookup"><span data-stu-id="e6138-171">Local storage size</span></span>
<span data-ttu-id="e6138-172">Hämtar storleken på den lokala lagringen för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-172">Retrieves the size of the local storage for the instance.</span></span>

| <span data-ttu-id="e6138-173">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-173">Type</span></span> | <span data-ttu-id="e6138-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-174">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-175">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-175">XPath</span></span> |<span data-ttu-id="e6138-176">XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB”</span><span class="sxs-lookup"><span data-stu-id="e6138-176">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB"</span></span> |
| <span data-ttu-id="e6138-177">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-177">Code</span></span> |<span data-ttu-id="e6138-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes;</span><span class="sxs-lookup"><span data-stu-id="e6138-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1").MaximumSizeInMegabytes;</span></span> |

## <a name="endpoint-protocol"></a><span data-ttu-id="e6138-179">Protokoll för slutpunkten</span><span class="sxs-lookup"><span data-stu-id="e6138-179">Endpoint protocol</span></span>
<span data-ttu-id="e6138-180">Hämtar endpoint-protokollet för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-180">Retrieves the endpoint protocol for the instance.</span></span>

| <span data-ttu-id="e6138-181">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-181">Type</span></span> | <span data-ttu-id="e6138-182">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-182">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-183">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-183">XPath</span></span> |<span data-ttu-id="e6138-184">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@protocol”</span><span class="sxs-lookup"><span data-stu-id="e6138-184">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol"</span></span> |
| <span data-ttu-id="e6138-185">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-185">Code</span></span> |<span data-ttu-id="e6138-186">var skydd = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. Protokollet.</span><span class="sxs-lookup"><span data-stu-id="e6138-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].Protocol;</span></span> |

## <a name="endpoint-ip"></a><span data-ttu-id="e6138-187">Slutpunkten IP</span><span class="sxs-lookup"><span data-stu-id="e6138-187">Endpoint IP</span></span>
<span data-ttu-id="e6138-188">Hämtar den angivna slutpunkten IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e6138-188">Gets the specified endpoint's IP address.</span></span>

| <span data-ttu-id="e6138-189">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-189">Type</span></span> | <span data-ttu-id="e6138-190">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-190">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-191">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-191">XPath</span></span> |<span data-ttu-id="e6138-192">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@address”</span><span class="sxs-lookup"><span data-stu-id="e6138-192">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address"</span></span> |
| <span data-ttu-id="e6138-193">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-193">Code</span></span> |<span data-ttu-id="e6138-194">var adress = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Address</span><span class="sxs-lookup"><span data-stu-id="e6138-194">var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Address</span></span> |

## <a name="endpoint-port"></a><span data-ttu-id="e6138-195">port för slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6138-195">Endpoint port</span></span>
<span data-ttu-id="e6138-196">Hämtar endpoint-port för instansen.</span><span class="sxs-lookup"><span data-stu-id="e6138-196">Retrieves the endpoint port for the instance.</span></span>

| <span data-ttu-id="e6138-197">Typ</span><span class="sxs-lookup"><span data-stu-id="e6138-197">Type</span></span> | <span data-ttu-id="e6138-198">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-198">Example</span></span> |
| --- | --- |
| <span data-ttu-id="e6138-199">XPath</span><span class="sxs-lookup"><span data-stu-id="e6138-199">XPath</span></span> |<span data-ttu-id="e6138-200">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@port”</span><span class="sxs-lookup"><span data-stu-id="e6138-200">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port"</span></span> |
| <span data-ttu-id="e6138-201">Kod</span><span class="sxs-lookup"><span data-stu-id="e6138-201">Code</span></span> |<span data-ttu-id="e6138-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Port;</span><span class="sxs-lookup"><span data-stu-id="e6138-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Port;</span></span> |

## <a name="example"></a><span data-ttu-id="e6138-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6138-203">Example</span></span>
<span data-ttu-id="e6138-204">Här är ett exempel på en arbetsroll som skapar en startåtgärd med miljövariabeln `TestIsEmulated` inställd på den [ @emulated xpath-värdet](#app-running-in-emulator).</span><span class="sxs-lookup"><span data-stu-id="e6138-204">Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set to the [@emulated xpath value](#app-running-in-emulator).</span></span> 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a><span data-ttu-id="e6138-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6138-205">Next steps</span></span>
<span data-ttu-id="e6138-206">Lär dig mer om den [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fil.</span><span class="sxs-lookup"><span data-stu-id="e6138-206">Learn more about the [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.</span></span>

<span data-ttu-id="e6138-207">Skapa en [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketet.</span><span class="sxs-lookup"><span data-stu-id="e6138-207">Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.</span></span>

<span data-ttu-id="e6138-208">Aktivera [fjärrskrivbord](cloud-services-role-enable-remote-desktop.md) för en roll.</span><span class="sxs-lookup"><span data-stu-id="e6138-208">Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.</span></span>

