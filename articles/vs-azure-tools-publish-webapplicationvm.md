---
title: Publicera WebApplicationVM | Microsoft Docs
description: "Lär dig hur du distribuerar ett webbprogram till en virtuell dator. Det här skriptet skapar resurserna som krävs i din Azure-prenumeration om de inte redan finns."
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
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="c4616-104">Publicera-WebApplicationVM (Windows PowerShell-skript)</span><span class="sxs-lookup"><span data-stu-id="c4616-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="c4616-105">Distribuerar ett webbprogram till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c4616-105">Deploys a web application to a virtual machine.</span></span> <span data-ttu-id="c4616-106">Skriptet skapar resurserna som krävs i din Azure-prenumeration om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c4616-106">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

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

### <a name="configuration"></a><span data-ttu-id="c4616-107">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4616-107">Configuration</span></span>
<span data-ttu-id="c4616-108">Sökvägen till JSON-konfigurationsfil som innehåller information om distributionen.</span><span class="sxs-lookup"><span data-stu-id="c4616-108">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="c4616-109">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-109">Aliases</span></span> | <span data-ttu-id="c4616-110">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-111">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-111">Required?</span></span> |<span data-ttu-id="c4616-112">SANT</span><span class="sxs-lookup"><span data-stu-id="c4616-112">true</span></span> |
| <span data-ttu-id="c4616-113">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-113">Position</span></span> |<span data-ttu-id="c4616-114">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-114">named</span></span> |
| <span data-ttu-id="c4616-115">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-115">Default value</span></span> |<span data-ttu-id="c4616-116">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-116">none</span></span> |
| <span data-ttu-id="c4616-117">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-117">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-118">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-118">false</span></span> |
| <span data-ttu-id="c4616-119">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-119">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-120">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="c4616-121">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="c4616-121">SubscriptionName</span></span>
<span data-ttu-id="c4616-122">Namnet på den Azure-prenumeration som du vill skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4616-122">The name of the Azure subscription in which you want to create the virtual machine.</span></span>

| <span data-ttu-id="c4616-123">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-123">Aliases</span></span> | <span data-ttu-id="c4616-124">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-125">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-125">Required?</span></span> |<span data-ttu-id="c4616-126">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-126">false</span></span> |
| <span data-ttu-id="c4616-127">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-127">Position</span></span> |<span data-ttu-id="c4616-128">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-128">named</span></span> |
| <span data-ttu-id="c4616-129">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-129">Default value</span></span> |<span data-ttu-id="c4616-130">Använder den första prenumerationen i prenumerationsfilen</span><span class="sxs-lookup"><span data-stu-id="c4616-130">Uses the first subscription in the subscription file</span></span> |
| <span data-ttu-id="c4616-131">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-131">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-132">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-132">false</span></span> |
| <span data-ttu-id="c4616-133">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-133">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-134">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="c4616-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="c4616-135">WebDeployPackage</span></span>
<span data-ttu-id="c4616-136">Sökvägen till distributionspaketets att publicera till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4616-136">The path to the web deployment package to publish to the virtual machine.</span></span> <span data-ttu-id="c4616-137">Du kan skapa det här paketet med hjälp av guiden Publicera webbplats i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4616-137">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="c4616-138">Se [så här: skapa ett Webbdistributionspaket i Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4616-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="c4616-139">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-139">Aliases</span></span> | <span data-ttu-id="c4616-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-141">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-141">Required?</span></span> |<span data-ttu-id="c4616-142">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-142">false</span></span> |
| <span data-ttu-id="c4616-143">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-143">Position</span></span> |<span data-ttu-id="c4616-144">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-144">named</span></span> |
| <span data-ttu-id="c4616-145">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-145">Default value</span></span> |<span data-ttu-id="c4616-146">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-146">none</span></span> |
| <span data-ttu-id="c4616-147">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-147">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-148">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-148">false</span></span> |
| <span data-ttu-id="c4616-149">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-149">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-150">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="c4616-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="c4616-151">AllowUntrusted</span></span>
<span data-ttu-id="c4616-152">Tillåt användning av certifikat som inte är signerat av en betrodd rotcertifikatutfärdare om värdet är true.</span><span class="sxs-lookup"><span data-stu-id="c4616-152">If true, allow the use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="c4616-153">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-153">Aliases</span></span> | <span data-ttu-id="c4616-154">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-155">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-155">Required?</span></span> |<span data-ttu-id="c4616-156">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-156">false</span></span> |
| <span data-ttu-id="c4616-157">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-157">Position</span></span> |<span data-ttu-id="c4616-158">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-158">named</span></span> |
| <span data-ttu-id="c4616-159">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-159">Default value</span></span> |<span data-ttu-id="c4616-160">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-160">false</span></span> |
| <span data-ttu-id="c4616-161">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-161">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-162">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-162">false</span></span> |
| <span data-ttu-id="c4616-163">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-163">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-164">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="c4616-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="c4616-165">VMPassword</span></span>
<span data-ttu-id="c4616-166">Autentiseringsuppgifterna för kontot för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c4616-166">The credentials for the virtual machine account.</span></span> <span data-ttu-id="c4616-167">Exempel: - VMPassword @{Name = ”admin”; Lösenord = ”password”}</span><span class="sxs-lookup"><span data-stu-id="c4616-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="c4616-168">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-168">Aliases</span></span> | <span data-ttu-id="c4616-169">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-170">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-170">Required?</span></span> |<span data-ttu-id="c4616-171">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-171">false</span></span> |
| <span data-ttu-id="c4616-172">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-172">Position</span></span> |<span data-ttu-id="c4616-173">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-173">named</span></span> |
| <span data-ttu-id="c4616-174">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-174">Default value</span></span> |<span data-ttu-id="c4616-175">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-175">none</span></span> |
| <span data-ttu-id="c4616-176">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-176">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-177">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-177">false</span></span> |
| <span data-ttu-id="c4616-178">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-178">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-179">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="c4616-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="c4616-180">DatabaseServerPassword</span></span>
<span data-ttu-id="c4616-181">Autentiseringsuppgifterna för SQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="c4616-181">The credentials for the SQL database in Azure.</span></span> <span data-ttu-id="c4616-182">Exempel: - DatabaseServerPassword @{Name = ”admin”; Lösenord = ”password”}</span><span class="sxs-lookup"><span data-stu-id="c4616-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="c4616-183">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-183">Aliases</span></span> | <span data-ttu-id="c4616-184">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-185">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-185">Required?</span></span> |<span data-ttu-id="c4616-186">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-186">false</span></span> |
| <span data-ttu-id="c4616-187">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-187">Position</span></span> |<span data-ttu-id="c4616-188">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-188">named</span></span> |
| <span data-ttu-id="c4616-189">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-189">Default value</span></span> |<span data-ttu-id="c4616-190">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-190">none</span></span> |
| <span data-ttu-id="c4616-191">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-191">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-192">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-192">false</span></span> |
| <span data-ttu-id="c4616-193">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-193">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-194">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="c4616-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="c4616-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="c4616-196">Om värdet är true, Skriv ut meddelanden från skriptet till utdataströmmen.</span><span class="sxs-lookup"><span data-stu-id="c4616-196">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="c4616-197">Alias</span><span class="sxs-lookup"><span data-stu-id="c4616-197">Aliases</span></span> | <span data-ttu-id="c4616-198">Ingen</span><span class="sxs-lookup"><span data-stu-id="c4616-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="c4616-199">Krävs?</span><span class="sxs-lookup"><span data-stu-id="c4616-199">Required?</span></span> |<span data-ttu-id="c4616-200">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-200">false</span></span> |
| <span data-ttu-id="c4616-201">Position</span><span class="sxs-lookup"><span data-stu-id="c4616-201">Position</span></span> |<span data-ttu-id="c4616-202">Med namnet</span><span class="sxs-lookup"><span data-stu-id="c4616-202">named</span></span> |
| <span data-ttu-id="c4616-203">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c4616-203">Default value</span></span> |<span data-ttu-id="c4616-204">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-204">false</span></span> |
| <span data-ttu-id="c4616-205">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="c4616-205">Accept pipeline input?</span></span> |<span data-ttu-id="c4616-206">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-206">false</span></span> |
| <span data-ttu-id="c4616-207">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="c4616-207">Accept wildcard characters?</span></span> |<span data-ttu-id="c4616-208">FALSKT</span><span class="sxs-lookup"><span data-stu-id="c4616-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="c4616-209">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="c4616-209">Remarks</span></span>
<span data-ttu-id="c4616-210">En fullständig förklaring av hur du använder skriptet för att skapa utvecklings- och testmiljöer finns [med hjälp av Windows PowerShell-skript för publicera utvecklings-och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="c4616-210">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="c4616-211">JSON-konfigurationsfil anger information om vad som distribueras.</span><span class="sxs-lookup"><span data-stu-id="c4616-211">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="c4616-212">Den innehåller den information som du angav när du har skapat projektet, till exempel namn, tillhörighetsgrupp, VHD-avbildningen och storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4616-212">It includes the information that you specified when you created the project, such as the name, affinity group, VHD image, and size of the virtual machine.</span></span> <span data-ttu-id="c4616-213">Dessutom innehåller slutpunkter på den virtuella datorn, databaser att etablera, om sådana finns, och web distributionsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="c4616-213">It also includes the endpoints on the virtual machine, the databases to provision, if any, and web deployment parameters.</span></span> <span data-ttu-id="c4616-214">Följande kod visar ett exempel JSON-konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="c4616-214">The following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="c4616-215">Du kan redigera JSON-konfigurationsfil om du vill ändra vilka etableras.</span><span class="sxs-lookup"><span data-stu-id="c4616-215">You can edit the JSON configuration file to change what is provisioned.</span></span> <span data-ttu-id="c4616-216">En virtuell dator och en molnbaserad tjänst som krävs, men databasen-avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="c4616-216">A virtual machine and a cloud service are required, but the database section is optional.</span></span>

