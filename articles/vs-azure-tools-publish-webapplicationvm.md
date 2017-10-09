---
title: aaaPublish WebApplicationVM | Microsoft Docs
description: "Lär dig hur toodeploy en web application tooa virtuell dator. Det här skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="42fe7-104">Publicera-WebApplicationVM (Windows PowerShell-skript)</span><span class="sxs-lookup"><span data-stu-id="42fe7-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="42fe7-105">Distribuerar en web application tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="42fe7-105">Deploys a web application tooa virtual machine.</span></span> <span data-ttu-id="42fe7-106">hello skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="42fe7-106">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a><span data-ttu-id="42fe7-107">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="42fe7-107">Configuration</span></span>
<span data-ttu-id="42fe7-108">hello sökvägen toohello JSON konfigurationsfil som beskriver hello information om hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="42fe7-108">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="42fe7-109">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-109">Aliases</span></span> | <span data-ttu-id="42fe7-110">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-111">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-111">Required?</span></span> |<span data-ttu-id="42fe7-112">SANT</span><span class="sxs-lookup"><span data-stu-id="42fe7-112">true</span></span> |
| <span data-ttu-id="42fe7-113">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-113">Position</span></span> |<span data-ttu-id="42fe7-114">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-114">named</span></span> |
| <span data-ttu-id="42fe7-115">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-115">Default value</span></span> |<span data-ttu-id="42fe7-116">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-116">none</span></span> |
| <span data-ttu-id="42fe7-117">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-117">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-118">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-118">false</span></span> |
| <span data-ttu-id="42fe7-119">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-119">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-120">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="42fe7-121">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="42fe7-121">SubscriptionName</span></span>
<span data-ttu-id="42fe7-122">hello namnet på hello Azure-prenumeration som du vill toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="42fe7-122">hello name of hello Azure subscription in which you want toocreate hello virtual machine.</span></span>

| <span data-ttu-id="42fe7-123">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-123">Aliases</span></span> | <span data-ttu-id="42fe7-124">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-125">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-125">Required?</span></span> |<span data-ttu-id="42fe7-126">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-126">false</span></span> |
| <span data-ttu-id="42fe7-127">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-127">Position</span></span> |<span data-ttu-id="42fe7-128">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-128">named</span></span> |
| <span data-ttu-id="42fe7-129">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-129">Default value</span></span> |<span data-ttu-id="42fe7-130">Använder hello första prenumeration i hello prenumerationsfilen</span><span class="sxs-lookup"><span data-stu-id="42fe7-130">Uses hello first subscription in hello subscription file</span></span> |
| <span data-ttu-id="42fe7-131">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-131">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-132">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-132">false</span></span> |
| <span data-ttu-id="42fe7-133">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-133">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-134">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="42fe7-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="42fe7-135">WebDeployPackage</span></span>
<span data-ttu-id="42fe7-136">hello sökvägen toohello distribution paketet toopublish toohello virtuella datorn på webbservern.</span><span class="sxs-lookup"><span data-stu-id="42fe7-136">hello path toohello web deployment package toopublish toohello virtual machine.</span></span> <span data-ttu-id="42fe7-137">Du kan skapa det här paketet med hjälp av guiden för hello Publicera webbplats i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="42fe7-137">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="42fe7-138">Se [så här: skapa ett Webbdistributionspaket i Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="42fe7-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="42fe7-139">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-139">Aliases</span></span> | <span data-ttu-id="42fe7-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-141">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-141">Required?</span></span> |<span data-ttu-id="42fe7-142">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-142">false</span></span> |
| <span data-ttu-id="42fe7-143">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-143">Position</span></span> |<span data-ttu-id="42fe7-144">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-144">named</span></span> |
| <span data-ttu-id="42fe7-145">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-145">Default value</span></span> |<span data-ttu-id="42fe7-146">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-146">none</span></span> |
| <span data-ttu-id="42fe7-147">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-147">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-148">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-148">false</span></span> |
| <span data-ttu-id="42fe7-149">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-149">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-150">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="42fe7-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="42fe7-151">AllowUntrusted</span></span>
<span data-ttu-id="42fe7-152">Om värdet är true Tillåt hello användning av certifikat som inte är signerat av en betrodd rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="42fe7-152">If true, allow hello use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="42fe7-153">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-153">Aliases</span></span> | <span data-ttu-id="42fe7-154">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-155">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-155">Required?</span></span> |<span data-ttu-id="42fe7-156">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-156">false</span></span> |
| <span data-ttu-id="42fe7-157">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-157">Position</span></span> |<span data-ttu-id="42fe7-158">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-158">named</span></span> |
| <span data-ttu-id="42fe7-159">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-159">Default value</span></span> |<span data-ttu-id="42fe7-160">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-160">false</span></span> |
| <span data-ttu-id="42fe7-161">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-161">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-162">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-162">false</span></span> |
| <span data-ttu-id="42fe7-163">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-163">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-164">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="42fe7-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="42fe7-165">VMPassword</span></span>
<span data-ttu-id="42fe7-166">hello autentiseringsuppgifterna för kontot för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="42fe7-166">hello credentials for hello virtual machine account.</span></span> <span data-ttu-id="42fe7-167">Exempel: - VMPassword @{Name = ”admin”; Lösenord = ”password”}</span><span class="sxs-lookup"><span data-stu-id="42fe7-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="42fe7-168">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-168">Aliases</span></span> | <span data-ttu-id="42fe7-169">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-170">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-170">Required?</span></span> |<span data-ttu-id="42fe7-171">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-171">false</span></span> |
| <span data-ttu-id="42fe7-172">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-172">Position</span></span> |<span data-ttu-id="42fe7-173">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-173">named</span></span> |
| <span data-ttu-id="42fe7-174">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-174">Default value</span></span> |<span data-ttu-id="42fe7-175">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-175">none</span></span> |
| <span data-ttu-id="42fe7-176">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-176">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-177">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-177">false</span></span> |
| <span data-ttu-id="42fe7-178">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-178">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-179">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="42fe7-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="42fe7-180">DatabaseServerPassword</span></span>
<span data-ttu-id="42fe7-181">hello autentiseringsuppgifter för hello SQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="42fe7-181">hello credentials for hello SQL database in Azure.</span></span> <span data-ttu-id="42fe7-182">Exempel: - DatabaseServerPassword @{Name = ”admin”; Lösenord = ”password”}</span><span class="sxs-lookup"><span data-stu-id="42fe7-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="42fe7-183">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-183">Aliases</span></span> | <span data-ttu-id="42fe7-184">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-185">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-185">Required?</span></span> |<span data-ttu-id="42fe7-186">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-186">false</span></span> |
| <span data-ttu-id="42fe7-187">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-187">Position</span></span> |<span data-ttu-id="42fe7-188">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-188">named</span></span> |
| <span data-ttu-id="42fe7-189">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-189">Default value</span></span> |<span data-ttu-id="42fe7-190">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-190">none</span></span> |
| <span data-ttu-id="42fe7-191">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-191">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-192">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-192">false</span></span> |
| <span data-ttu-id="42fe7-193">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-193">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-194">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="42fe7-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="42fe7-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="42fe7-196">Om värdet är true utdata ut meddelanden från hello skriptet toohello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="42fe7-196">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="42fe7-197">Alias</span><span class="sxs-lookup"><span data-stu-id="42fe7-197">Aliases</span></span> | <span data-ttu-id="42fe7-198">Ingen</span><span class="sxs-lookup"><span data-stu-id="42fe7-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="42fe7-199">Krävs?</span><span class="sxs-lookup"><span data-stu-id="42fe7-199">Required?</span></span> |<span data-ttu-id="42fe7-200">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-200">false</span></span> |
| <span data-ttu-id="42fe7-201">Position</span><span class="sxs-lookup"><span data-stu-id="42fe7-201">Position</span></span> |<span data-ttu-id="42fe7-202">Med namnet</span><span class="sxs-lookup"><span data-stu-id="42fe7-202">named</span></span> |
| <span data-ttu-id="42fe7-203">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="42fe7-203">Default value</span></span> |<span data-ttu-id="42fe7-204">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-204">false</span></span> |
| <span data-ttu-id="42fe7-205">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="42fe7-205">Accept pipeline input?</span></span> |<span data-ttu-id="42fe7-206">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-206">false</span></span> |
| <span data-ttu-id="42fe7-207">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="42fe7-207">Accept wildcard characters?</span></span> |<span data-ttu-id="42fe7-208">FALSKT</span><span class="sxs-lookup"><span data-stu-id="42fe7-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="42fe7-209">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="42fe7-209">Remarks</span></span>
<span data-ttu-id="42fe7-210">En fullständig förklaring av hur toouse hello skriptet toocreate utvecklings- och testmiljöer, se [med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="42fe7-210">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="42fe7-211">hello JSON-konfigurationsfil anger hello information om vad är toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="42fe7-211">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="42fe7-212">Den omfattar hello information som du angav när du skapade hello projekt, till exempel hello namn, tillhörighetsgrupp, VHD-avbildningen och storleken på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42fe7-212">It includes hello information that you specified when you created hello project, such as hello name, affinity group, VHD image, and size of hello virtual machine.</span></span> <span data-ttu-id="42fe7-213">Dessutom innehåller hello slutpunkter på hello databaser tooprovision hello virtuell dator, och web distributionsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="42fe7-213">It also includes hello endpoints on hello virtual machine, hello databases tooprovision, if any, and web deployment parameters.</span></span> <span data-ttu-id="42fe7-214">hello följande kod visar ett exempel JSON-konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="42fe7-214">hello following code shows an example JSON configuration file:</span></span>

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="42fe7-215">Du kan redigera hello JSON configuration file toochange vad etableras.</span><span class="sxs-lookup"><span data-stu-id="42fe7-215">You can edit hello JSON configuration file toochange what is provisioned.</span></span> <span data-ttu-id="42fe7-216">En virtuell dator och en molnbaserad tjänst som krävs, men hello databasen avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="42fe7-216">A virtual machine and a cloud service are required, but hello database section is optional.</span></span>

