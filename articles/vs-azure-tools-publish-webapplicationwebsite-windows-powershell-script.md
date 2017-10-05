---
title: Publicera-WebApplicationWebSite (Windows PowerShell-skript) | Microsoft Docs
description: "Lär dig hur du publicerar ett webbprojekt till en Azure-webbplats. Det här skriptet skapar resurserna som krävs i din Azure-prenumeration om de inte redan finns."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="18171-104">Publicera-WebApplicationWebSite (Windows PowerShell-skript)</span><span class="sxs-lookup"><span data-stu-id="18171-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="18171-105">Syntax</span><span class="sxs-lookup"><span data-stu-id="18171-105">Syntax</span></span>
<span data-ttu-id="18171-106">Publicerar ett webbprojekt till en Azure-webbplats.</span><span class="sxs-lookup"><span data-stu-id="18171-106">Publishes a web project to an Azure website.</span></span> <span data-ttu-id="18171-107">Skriptet skapar resurserna som krävs i din Azure-prenumeration om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="18171-107">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="18171-108">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="18171-108">Configuration</span></span>
<span data-ttu-id="18171-109">Sökvägen till JSON-konfigurationsfil som innehåller information om distributionen.</span><span class="sxs-lookup"><span data-stu-id="18171-109">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="18171-110">Parameter</span><span class="sxs-lookup"><span data-stu-id="18171-110">Parameter</span></span> | <span data-ttu-id="18171-111">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="18171-112">Alias</span><span class="sxs-lookup"><span data-stu-id="18171-112">Aliases</span></span> |<span data-ttu-id="18171-113">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-113">none</span></span> |
| <span data-ttu-id="18171-114">Krävs?</span><span class="sxs-lookup"><span data-stu-id="18171-114">Required?</span></span> |<span data-ttu-id="18171-115">SANT</span><span class="sxs-lookup"><span data-stu-id="18171-115">true</span></span> |
| <span data-ttu-id="18171-116">Position</span><span class="sxs-lookup"><span data-stu-id="18171-116">Position</span></span> |<span data-ttu-id="18171-117">Med namnet</span><span class="sxs-lookup"><span data-stu-id="18171-117">named</span></span> |
| <span data-ttu-id="18171-118">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-118">Default value</span></span> |<span data-ttu-id="18171-119">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-119">none</span></span> |
| <span data-ttu-id="18171-120">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="18171-120">Accept pipeline input?</span></span> |<span data-ttu-id="18171-121">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-121">false</span></span> |
| <span data-ttu-id="18171-122">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="18171-122">Accept wildcard characters?</span></span> |<span data-ttu-id="18171-123">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="18171-124">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="18171-124">SubscriptionName</span></span>
<span data-ttu-id="18171-125">Namnet på Azure-prenumeration som du vill skapa webbplatsen i.</span><span class="sxs-lookup"><span data-stu-id="18171-125">The name of the Azure subscription that you want to create the website in.</span></span>

| <span data-ttu-id="18171-126">Parameter</span><span class="sxs-lookup"><span data-stu-id="18171-126">Parameter</span></span> | <span data-ttu-id="18171-127">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="18171-128">Alias</span><span class="sxs-lookup"><span data-stu-id="18171-128">Aliases</span></span> |<span data-ttu-id="18171-129">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-129">none</span></span> |
| <span data-ttu-id="18171-130">Krävs?</span><span class="sxs-lookup"><span data-stu-id="18171-130">Required?</span></span> |<span data-ttu-id="18171-131">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-131">false</span></span> |
| <span data-ttu-id="18171-132">Position</span><span class="sxs-lookup"><span data-stu-id="18171-132">Position</span></span> |<span data-ttu-id="18171-133">Med namnet</span><span class="sxs-lookup"><span data-stu-id="18171-133">named</span></span> |
| <span data-ttu-id="18171-134">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-134">Default value</span></span> |<span data-ttu-id="18171-135">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-135">none</span></span> |
| <span data-ttu-id="18171-136">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="18171-136">Accept pipeline input?</span></span> |<span data-ttu-id="18171-137">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-137">false</span></span> |
| <span data-ttu-id="18171-138">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="18171-138">Accept wildcard characters?</span></span> |<span data-ttu-id="18171-139">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="18171-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="18171-140">WebDeployPackage</span></span>
<span data-ttu-id="18171-141">Sökvägen till distributionspaketets att publicera på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="18171-141">The path to the web deployment package to publish to the website.</span></span> <span data-ttu-id="18171-142">Du kan skapa det här paketet med hjälp av guiden Publicera webbplats i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18171-142">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="18171-143">Mer information finns i [Kom igång med Azure Cloud Services och ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="18171-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="18171-144">Parameter</span><span class="sxs-lookup"><span data-stu-id="18171-144">Parameter</span></span> | <span data-ttu-id="18171-145">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="18171-146">Alias</span><span class="sxs-lookup"><span data-stu-id="18171-146">Aliases</span></span> |<span data-ttu-id="18171-147">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-147">none</span></span> |
| <span data-ttu-id="18171-148">Krävs?</span><span class="sxs-lookup"><span data-stu-id="18171-148">Required?</span></span> |<span data-ttu-id="18171-149">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-149">false</span></span> |
| <span data-ttu-id="18171-150">Position</span><span class="sxs-lookup"><span data-stu-id="18171-150">Position</span></span> |<span data-ttu-id="18171-151">Med namnet</span><span class="sxs-lookup"><span data-stu-id="18171-151">named</span></span> |
| <span data-ttu-id="18171-152">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-152">Default value</span></span> |<span data-ttu-id="18171-153">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-153">none</span></span> |
| <span data-ttu-id="18171-154">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="18171-154">Accept pipeline input?</span></span> |<span data-ttu-id="18171-155">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-155">false</span></span> |
| <span data-ttu-id="18171-156">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="18171-156">Accept wildcard characters?</span></span> |<span data-ttu-id="18171-157">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="18171-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="18171-158">DatabaseServerPassword</span></span>
<span data-ttu-id="18171-159">Användarnamn och lösenord för SQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="18171-159">The username and password for the SQL database in Azure.</span></span>

| <span data-ttu-id="18171-160">Parameter</span><span class="sxs-lookup"><span data-stu-id="18171-160">Parameter</span></span> | <span data-ttu-id="18171-161">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="18171-162">Alias</span><span class="sxs-lookup"><span data-stu-id="18171-162">Aliases</span></span> |<span data-ttu-id="18171-163">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-163">none</span></span> |
| <span data-ttu-id="18171-164">Krävs?</span><span class="sxs-lookup"><span data-stu-id="18171-164">Required?</span></span> |<span data-ttu-id="18171-165">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-165">false</span></span> |
| <span data-ttu-id="18171-166">Position</span><span class="sxs-lookup"><span data-stu-id="18171-166">Position</span></span> |<span data-ttu-id="18171-167">Med namnet</span><span class="sxs-lookup"><span data-stu-id="18171-167">named</span></span> |
| <span data-ttu-id="18171-168">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-168">Default value</span></span> |<span data-ttu-id="18171-169">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-169">none</span></span> |
| <span data-ttu-id="18171-170">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="18171-170">Accept pipeline input?</span></span> |<span data-ttu-id="18171-171">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-171">false</span></span> |
| <span data-ttu-id="18171-172">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="18171-172">Accept wildcard characters?</span></span> |<span data-ttu-id="18171-173">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="18171-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="18171-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="18171-175">Om värdet är true, Skriv ut meddelanden från skriptet till utdataströmmen.</span><span class="sxs-lookup"><span data-stu-id="18171-175">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="18171-176">Parameter</span><span class="sxs-lookup"><span data-stu-id="18171-176">Parameter</span></span> | <span data-ttu-id="18171-177">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="18171-178">Alias</span><span class="sxs-lookup"><span data-stu-id="18171-178">Aliases</span></span> |<span data-ttu-id="18171-179">Ingen</span><span class="sxs-lookup"><span data-stu-id="18171-179">none</span></span> |
| <span data-ttu-id="18171-180">Krävs?</span><span class="sxs-lookup"><span data-stu-id="18171-180">Required?</span></span> |<span data-ttu-id="18171-181">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-181">false</span></span> |
| <span data-ttu-id="18171-182">Position</span><span class="sxs-lookup"><span data-stu-id="18171-182">Position</span></span> |<span data-ttu-id="18171-183">Med namnet</span><span class="sxs-lookup"><span data-stu-id="18171-183">named</span></span> |
| <span data-ttu-id="18171-184">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="18171-184">Default value</span></span> |<span data-ttu-id="18171-185">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-185">false</span></span> |
| <span data-ttu-id="18171-186">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="18171-186">Accept pipeline input?</span></span> |<span data-ttu-id="18171-187">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-187">false</span></span> |
| <span data-ttu-id="18171-188">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="18171-188">Accept wildcard characters?</span></span> |<span data-ttu-id="18171-189">FALSKT</span><span class="sxs-lookup"><span data-stu-id="18171-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="18171-190">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="18171-190">Remarks</span></span>
<span data-ttu-id="18171-191">En fullständig förklaring av hur du använder skriptet för att skapa utvecklings- och testmiljöer finns [med hjälp av Windows PowerShell-skript för publicera utvecklings-och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="18171-191">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="18171-192">JSON-konfigurationsfil anger information om vad som distribueras.</span><span class="sxs-lookup"><span data-stu-id="18171-192">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="18171-193">Den innehåller information som du angav när du skapade projektet, till exempel namn och användarnamn för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="18171-193">It includes the information that you specified when you created the project, such as the name and username for the website.</span></span> <span data-ttu-id="18171-194">Den omfattar också databasen för att etablera, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="18171-194">It also includes the database to provision, if any.</span></span> <span data-ttu-id="18171-195">Följande kod visar ett exempel JSON-konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="18171-195">The following code shows an example JSON configuration file:</span></span>

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

<span data-ttu-id="18171-196">Du kan redigera JSON-konfigurationsfil om du vill ändra vilka distribueras.</span><span class="sxs-lookup"><span data-stu-id="18171-196">You can edit the JSON configuration file to change what is deployed.</span></span> <span data-ttu-id="18171-197">Ett avsnitt på webbplatsen krävs, men databasen-avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="18171-197">A webSite section is required, but the database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18171-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18171-198">Next steps</span></span>
<span data-ttu-id="18171-199">Mer information finns i [publicera WebApplicationVM (Windows PowerShell-skript)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="18171-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

