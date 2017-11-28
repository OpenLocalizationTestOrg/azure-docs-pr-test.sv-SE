---
title: aaaCloud Services-rollen config XPath fusklapp | Microsoft Docs
description: "Hej olika XPath-inställningar som du kan använda i hello cloud service config tooexpose rollinställningar som en miljövariabel."
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
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a><span data-ttu-id="a0c3d-103">Exponera konfigurationsinställningar för rollen som en miljövariabel med XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-103">Expose role configuration settings as an environment variable with XPath</span></span>
<span data-ttu-id="a0c3d-104">I hello cloud service worker eller web rollen tjänstdefinitionsfilen kan exponera du runtime konfigurationsvärden som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-104">In hello cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables.</span></span> <span data-ttu-id="a0c3d-105">hello följande XPath värden stöds (som motsvarar tooAPI värden).</span><span class="sxs-lookup"><span data-stu-id="a0c3d-105">hello following XPath values are supported (which correspond tooAPI values).</span></span>

<span data-ttu-id="a0c3d-106">Värdena XPath finns också tillgängliga via hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-106">These XPath values are also available through hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library.</span></span> 

## <a name="app-running-in-emulator"></a><span data-ttu-id="a0c3d-107">Appen körs i emulatorn</span><span class="sxs-lookup"><span data-stu-id="a0c3d-107">App running in emulator</span></span>
<span data-ttu-id="a0c3d-108">Anger hello appen körs i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-108">Indicates that hello app is running in hello emulator.</span></span>

| <span data-ttu-id="a0c3d-109">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-109">Type</span></span> | <span data-ttu-id="a0c3d-110">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-110">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-111">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-111">XPath</span></span> |<span data-ttu-id="a0c3d-112">XPath = ”/RoleEnvironment/Deployment/@emulated”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-112">xpath="/RoleEnvironment/Deployment/@emulated"</span></span> |
| <span data-ttu-id="a0c3d-113">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-113">Code</span></span> |<span data-ttu-id="a0c3d-114">var x = RoleEnvironment.IsEmulated;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-114">var x = RoleEnvironment.IsEmulated;</span></span> |

## <a name="deployment-id"></a><span data-ttu-id="a0c3d-115">Distributions-ID</span><span class="sxs-lookup"><span data-stu-id="a0c3d-115">Deployment ID</span></span>
<span data-ttu-id="a0c3d-116">Hämtar hello distributions-ID för hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-116">Retrieves hello deployment ID for hello instance.</span></span>

| <span data-ttu-id="a0c3d-117">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-117">Type</span></span> | <span data-ttu-id="a0c3d-118">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-118">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-119">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-119">XPath</span></span> |<span data-ttu-id="a0c3d-120">XPath = ”/RoleEnvironment/Deployment/@id”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-120">xpath="/RoleEnvironment/Deployment/@id"</span></span> |
| <span data-ttu-id="a0c3d-121">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-121">Code</span></span> |<span data-ttu-id="a0c3d-122">var deploymentId = RoleEnvironment.DeploymentId;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-122">var deploymentId = RoleEnvironment.DeploymentId;</span></span> |

## <a name="role-id"></a><span data-ttu-id="a0c3d-123">Roll-ID</span><span class="sxs-lookup"><span data-stu-id="a0c3d-123">Role ID</span></span>
<span data-ttu-id="a0c3d-124">Hämtar hello aktuella roll-ID för hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-124">Retrieves hello current role ID for hello instance.</span></span>

| <span data-ttu-id="a0c3d-125">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-125">Type</span></span> | <span data-ttu-id="a0c3d-126">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-126">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-127">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-127">XPath</span></span> |<span data-ttu-id="a0c3d-128">XPath = ”/RoleEnvironment/CurrentInstance/@id”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-128">xpath="/RoleEnvironment/CurrentInstance/@id"</span></span> |
| <span data-ttu-id="a0c3d-129">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-129">Code</span></span> |<span data-ttu-id="a0c3d-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span></span> |

## <a name="update-domain"></a><span data-ttu-id="a0c3d-131">Uppdatera domänen</span><span class="sxs-lookup"><span data-stu-id="a0c3d-131">Update domain</span></span>
<span data-ttu-id="a0c3d-132">Hämtar hello uppdateringsdomän av hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-132">Retrieves hello update domain of hello instance.</span></span>

| <span data-ttu-id="a0c3d-133">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-133">Type</span></span> | <span data-ttu-id="a0c3d-134">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-134">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-135">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-135">XPath</span></span> |<span data-ttu-id="a0c3d-136">XPath = ”/RoleEnvironment/CurrentInstance/@updateDomain”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-136">xpath="/RoleEnvironment/CurrentInstance/@updateDomain"</span></span> |
| <span data-ttu-id="a0c3d-137">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-137">Code</span></span> |<span data-ttu-id="a0c3d-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span></span> |

## <a name="fault-domain"></a><span data-ttu-id="a0c3d-139">feldomän</span><span class="sxs-lookup"><span data-stu-id="a0c3d-139">Fault domain</span></span>
<span data-ttu-id="a0c3d-140">Hämtar hello feldomän av hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-140">Retrieves hello fault domain of hello instance.</span></span>

| <span data-ttu-id="a0c3d-141">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-141">Type</span></span> | <span data-ttu-id="a0c3d-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-142">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-143">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-143">XPath</span></span> |<span data-ttu-id="a0c3d-144">XPath = ”/RoleEnvironment/CurrentInstance/@faultDomain”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-144">xpath="/RoleEnvironment/CurrentInstance/@faultDomain"</span></span> |
| <span data-ttu-id="a0c3d-145">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-145">Code</span></span> |<span data-ttu-id="a0c3d-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span></span> |

## <a name="role-name"></a><span data-ttu-id="a0c3d-147">Rollnamn</span><span class="sxs-lookup"><span data-stu-id="a0c3d-147">Role name</span></span>
<span data-ttu-id="a0c3d-148">Hämtar hello rollnamn hello instanser.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-148">Retrieves hello role name of hello instances.</span></span>

| <span data-ttu-id="a0c3d-149">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-149">Type</span></span> | <span data-ttu-id="a0c3d-150">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-150">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-151">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-151">XPath</span></span> |<span data-ttu-id="a0c3d-152">XPath = ”/RoleEnvironment/CurrentInstance/@roleName”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-152">xpath="/RoleEnvironment/CurrentInstance/@roleName"</span></span> |
| <span data-ttu-id="a0c3d-153">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-153">Code</span></span> |<span data-ttu-id="a0c3d-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span></span> |

## <a name="config-setting"></a><span data-ttu-id="a0c3d-155">Konfigurationsinställningen</span><span class="sxs-lookup"><span data-stu-id="a0c3d-155">Config setting</span></span>
<span data-ttu-id="a0c3d-156">Hämtar hello värdet för hello angetts konfigurationsinställning.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-156">Retrieves hello value of hello specified configuration setting.</span></span>

| <span data-ttu-id="a0c3d-157">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-157">Type</span></span> | <span data-ttu-id="a0c3d-158">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-158">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-159">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-159">XPath</span></span> |<span data-ttu-id="a0c3d-160">XPath = ”/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-160">xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value"</span></span> |
| <span data-ttu-id="a0c3d-161">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-161">Code</span></span> |<span data-ttu-id="a0c3d-162">var inställningen = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span><span class="sxs-lookup"><span data-stu-id="a0c3d-162">var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span></span> |

## <a name="local-storage-path"></a><span data-ttu-id="a0c3d-163">Sökvägen till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="a0c3d-163">Local storage path</span></span>
<span data-ttu-id="a0c3d-164">Hämtar hello lokal lagringssökväg för hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-164">Retrieves hello local storage path for hello instance.</span></span>

| <span data-ttu-id="a0c3d-165">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-165">Type</span></span> | <span data-ttu-id="a0c3d-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-166">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-167">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-167">XPath</span></span> |<span data-ttu-id="a0c3d-168">XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-168">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path"</span></span> |
| <span data-ttu-id="a0c3d-169">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-169">Code</span></span> |<span data-ttu-id="a0c3d-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1").RootPath;</span></span> |

## <a name="local-storage-size"></a><span data-ttu-id="a0c3d-171">Lokal lagringsstorlek</span><span class="sxs-lookup"><span data-stu-id="a0c3d-171">Local storage size</span></span>
<span data-ttu-id="a0c3d-172">Hämtar hello storleken på hello lokal lagring för hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-172">Retrieves hello size of hello local storage for hello instance.</span></span>

| <span data-ttu-id="a0c3d-173">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-173">Type</span></span> | <span data-ttu-id="a0c3d-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-174">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-175">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-175">XPath</span></span> |<span data-ttu-id="a0c3d-176">XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-176">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB"</span></span> |
| <span data-ttu-id="a0c3d-177">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-177">Code</span></span> |<span data-ttu-id="a0c3d-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1").MaximumSizeInMegabytes;</span></span> |

## <a name="endpoint-protocol"></a><span data-ttu-id="a0c3d-179">Protokoll för slutpunkten</span><span class="sxs-lookup"><span data-stu-id="a0c3d-179">Endpoint protocol</span></span>
<span data-ttu-id="a0c3d-180">Hämtar hello endpoint-protokollet för hello-instans.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-180">Retrieves hello endpoint protocol for hello instance.</span></span>

| <span data-ttu-id="a0c3d-181">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-181">Type</span></span> | <span data-ttu-id="a0c3d-182">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-182">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-183">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-183">XPath</span></span> |<span data-ttu-id="a0c3d-184">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@protocol”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-184">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol"</span></span> |
| <span data-ttu-id="a0c3d-185">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-185">Code</span></span> |<span data-ttu-id="a0c3d-186">var skydd = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. Protokollet.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].Protocol;</span></span> |

## <a name="endpoint-ip"></a><span data-ttu-id="a0c3d-187">Slutpunkten IP</span><span class="sxs-lookup"><span data-stu-id="a0c3d-187">Endpoint IP</span></span>
<span data-ttu-id="a0c3d-188">Hämtar hello angetts slutpunktens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-188">Gets hello specified endpoint's IP address.</span></span>

| <span data-ttu-id="a0c3d-189">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-189">Type</span></span> | <span data-ttu-id="a0c3d-190">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-190">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-191">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-191">XPath</span></span> |<span data-ttu-id="a0c3d-192">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@address”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-192">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address"</span></span> |
| <span data-ttu-id="a0c3d-193">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-193">Code</span></span> |<span data-ttu-id="a0c3d-194">var adress = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Address</span><span class="sxs-lookup"><span data-stu-id="a0c3d-194">var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Address</span></span> |

## <a name="endpoint-port"></a><span data-ttu-id="a0c3d-195">port för slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a0c3d-195">Endpoint port</span></span>
<span data-ttu-id="a0c3d-196">Hämtar hello endpoint port för hello-instans.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-196">Retrieves hello endpoint port for hello instance.</span></span>

| <span data-ttu-id="a0c3d-197">Typ</span><span class="sxs-lookup"><span data-stu-id="a0c3d-197">Type</span></span> | <span data-ttu-id="a0c3d-198">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-198">Example</span></span> |
| --- | --- |
| <span data-ttu-id="a0c3d-199">XPath</span><span class="sxs-lookup"><span data-stu-id="a0c3d-199">XPath</span></span> |<span data-ttu-id="a0c3d-200">XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@port”</span><span class="sxs-lookup"><span data-stu-id="a0c3d-200">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port"</span></span> |
| <span data-ttu-id="a0c3d-201">Kod</span><span class="sxs-lookup"><span data-stu-id="a0c3d-201">Code</span></span> |<span data-ttu-id="a0c3d-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Port;</span><span class="sxs-lookup"><span data-stu-id="a0c3d-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Port;</span></span> |

## <a name="example"></a><span data-ttu-id="a0c3d-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="a0c3d-203">Example</span></span>
<span data-ttu-id="a0c3d-204">Här är ett exempel på en arbetsroll som skapar en startåtgärd med miljövariabeln `TestIsEmulated` ange toohello [ @emulated xpath-värdet](#app-running-in-emulator).</span><span class="sxs-lookup"><span data-stu-id="a0c3d-204">Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set toohello [@emulated xpath value](#app-running-in-emulator).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="a0c3d-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0c3d-205">Next steps</span></span>
<span data-ttu-id="a0c3d-206">Mer information om hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fil.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-206">Learn more about hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.</span></span>

<span data-ttu-id="a0c3d-207">Skapa en [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketet.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-207">Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.</span></span>

<span data-ttu-id="a0c3d-208">Aktivera [fjärrskrivbord](cloud-services-role-enable-remote-desktop.md) för en roll.</span><span class="sxs-lookup"><span data-stu-id="a0c3d-208">Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.</span></span>

