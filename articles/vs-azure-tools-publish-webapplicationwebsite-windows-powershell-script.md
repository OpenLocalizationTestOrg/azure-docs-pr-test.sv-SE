---
title: aaaPublish WebApplicationWebSite (Windows PowerShell-skript) | Microsoft Docs
description: "Lär dig hur toopublish en web project tooan Azure-webbplatsen. Det här skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns."
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
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="34159-104">Publicera-WebApplicationWebSite (Windows PowerShell-skript)</span><span class="sxs-lookup"><span data-stu-id="34159-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="34159-105">Syntax</span><span class="sxs-lookup"><span data-stu-id="34159-105">Syntax</span></span>
<span data-ttu-id="34159-106">Publicerar en web project tooan Azure-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="34159-106">Publishes a web project tooan Azure website.</span></span> <span data-ttu-id="34159-107">hello skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="34159-107">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="34159-108">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="34159-108">Configuration</span></span>
<span data-ttu-id="34159-109">hello sökvägen toohello JSON konfigurationsfil som beskriver hello information om hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="34159-109">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="34159-110">Parameter</span><span class="sxs-lookup"><span data-stu-id="34159-110">Parameter</span></span> | <span data-ttu-id="34159-111">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="34159-112">Alias</span><span class="sxs-lookup"><span data-stu-id="34159-112">Aliases</span></span> |<span data-ttu-id="34159-113">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-113">none</span></span> |
| <span data-ttu-id="34159-114">Krävs?</span><span class="sxs-lookup"><span data-stu-id="34159-114">Required?</span></span> |<span data-ttu-id="34159-115">SANT</span><span class="sxs-lookup"><span data-stu-id="34159-115">true</span></span> |
| <span data-ttu-id="34159-116">Position</span><span class="sxs-lookup"><span data-stu-id="34159-116">Position</span></span> |<span data-ttu-id="34159-117">Med namnet</span><span class="sxs-lookup"><span data-stu-id="34159-117">named</span></span> |
| <span data-ttu-id="34159-118">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-118">Default value</span></span> |<span data-ttu-id="34159-119">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-119">none</span></span> |
| <span data-ttu-id="34159-120">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="34159-120">Accept pipeline input?</span></span> |<span data-ttu-id="34159-121">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-121">false</span></span> |
| <span data-ttu-id="34159-122">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="34159-122">Accept wildcard characters?</span></span> |<span data-ttu-id="34159-123">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="34159-124">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="34159-124">SubscriptionName</span></span>
<span data-ttu-id="34159-125">hello namnet på hello Azure-prenumeration som du vill ha toocreate hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="34159-125">hello name of hello Azure subscription that you want toocreate hello website in.</span></span>

| <span data-ttu-id="34159-126">Parameter</span><span class="sxs-lookup"><span data-stu-id="34159-126">Parameter</span></span> | <span data-ttu-id="34159-127">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="34159-128">Alias</span><span class="sxs-lookup"><span data-stu-id="34159-128">Aliases</span></span> |<span data-ttu-id="34159-129">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-129">none</span></span> |
| <span data-ttu-id="34159-130">Krävs?</span><span class="sxs-lookup"><span data-stu-id="34159-130">Required?</span></span> |<span data-ttu-id="34159-131">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-131">false</span></span> |
| <span data-ttu-id="34159-132">Position</span><span class="sxs-lookup"><span data-stu-id="34159-132">Position</span></span> |<span data-ttu-id="34159-133">Med namnet</span><span class="sxs-lookup"><span data-stu-id="34159-133">named</span></span> |
| <span data-ttu-id="34159-134">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-134">Default value</span></span> |<span data-ttu-id="34159-135">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-135">none</span></span> |
| <span data-ttu-id="34159-136">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="34159-136">Accept pipeline input?</span></span> |<span data-ttu-id="34159-137">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-137">false</span></span> |
| <span data-ttu-id="34159-138">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="34159-138">Accept wildcard characters?</span></span> |<span data-ttu-id="34159-139">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="34159-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="34159-140">WebDeployPackage</span></span>
<span data-ttu-id="34159-141">hello sökvägen toohello web distribution paketet toopublish toohello webbplats.</span><span class="sxs-lookup"><span data-stu-id="34159-141">hello path toohello web deployment package toopublish toohello website.</span></span> <span data-ttu-id="34159-142">Du kan skapa det här paketet med hjälp av guiden för hello Publicera webbplats i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34159-142">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="34159-143">Mer information finns i [Kom igång med Azure Cloud Services och ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="34159-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="34159-144">Parameter</span><span class="sxs-lookup"><span data-stu-id="34159-144">Parameter</span></span> | <span data-ttu-id="34159-145">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="34159-146">Alias</span><span class="sxs-lookup"><span data-stu-id="34159-146">Aliases</span></span> |<span data-ttu-id="34159-147">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-147">none</span></span> |
| <span data-ttu-id="34159-148">Krävs?</span><span class="sxs-lookup"><span data-stu-id="34159-148">Required?</span></span> |<span data-ttu-id="34159-149">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-149">false</span></span> |
| <span data-ttu-id="34159-150">Position</span><span class="sxs-lookup"><span data-stu-id="34159-150">Position</span></span> |<span data-ttu-id="34159-151">Med namnet</span><span class="sxs-lookup"><span data-stu-id="34159-151">named</span></span> |
| <span data-ttu-id="34159-152">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-152">Default value</span></span> |<span data-ttu-id="34159-153">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-153">none</span></span> |
| <span data-ttu-id="34159-154">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="34159-154">Accept pipeline input?</span></span> |<span data-ttu-id="34159-155">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-155">false</span></span> |
| <span data-ttu-id="34159-156">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="34159-156">Accept wildcard characters?</span></span> |<span data-ttu-id="34159-157">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="34159-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="34159-158">DatabaseServerPassword</span></span>
<span data-ttu-id="34159-159">hello användarnamn och lösenord för hello SQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="34159-159">hello username and password for hello SQL database in Azure.</span></span>

| <span data-ttu-id="34159-160">Parameter</span><span class="sxs-lookup"><span data-stu-id="34159-160">Parameter</span></span> | <span data-ttu-id="34159-161">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="34159-162">Alias</span><span class="sxs-lookup"><span data-stu-id="34159-162">Aliases</span></span> |<span data-ttu-id="34159-163">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-163">none</span></span> |
| <span data-ttu-id="34159-164">Krävs?</span><span class="sxs-lookup"><span data-stu-id="34159-164">Required?</span></span> |<span data-ttu-id="34159-165">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-165">false</span></span> |
| <span data-ttu-id="34159-166">Position</span><span class="sxs-lookup"><span data-stu-id="34159-166">Position</span></span> |<span data-ttu-id="34159-167">Med namnet</span><span class="sxs-lookup"><span data-stu-id="34159-167">named</span></span> |
| <span data-ttu-id="34159-168">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-168">Default value</span></span> |<span data-ttu-id="34159-169">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-169">none</span></span> |
| <span data-ttu-id="34159-170">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="34159-170">Accept pipeline input?</span></span> |<span data-ttu-id="34159-171">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-171">false</span></span> |
| <span data-ttu-id="34159-172">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="34159-172">Accept wildcard characters?</span></span> |<span data-ttu-id="34159-173">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="34159-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="34159-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="34159-175">Om värdet är true utdata ut meddelanden från hello skriptet toohello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="34159-175">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="34159-176">Parameter</span><span class="sxs-lookup"><span data-stu-id="34159-176">Parameter</span></span> | <span data-ttu-id="34159-177">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="34159-178">Alias</span><span class="sxs-lookup"><span data-stu-id="34159-178">Aliases</span></span> |<span data-ttu-id="34159-179">Ingen</span><span class="sxs-lookup"><span data-stu-id="34159-179">none</span></span> |
| <span data-ttu-id="34159-180">Krävs?</span><span class="sxs-lookup"><span data-stu-id="34159-180">Required?</span></span> |<span data-ttu-id="34159-181">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-181">false</span></span> |
| <span data-ttu-id="34159-182">Position</span><span class="sxs-lookup"><span data-stu-id="34159-182">Position</span></span> |<span data-ttu-id="34159-183">Med namnet</span><span class="sxs-lookup"><span data-stu-id="34159-183">named</span></span> |
| <span data-ttu-id="34159-184">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="34159-184">Default value</span></span> |<span data-ttu-id="34159-185">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-185">false</span></span> |
| <span data-ttu-id="34159-186">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="34159-186">Accept pipeline input?</span></span> |<span data-ttu-id="34159-187">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-187">false</span></span> |
| <span data-ttu-id="34159-188">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="34159-188">Accept wildcard characters?</span></span> |<span data-ttu-id="34159-189">FALSKT</span><span class="sxs-lookup"><span data-stu-id="34159-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="34159-190">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="34159-190">Remarks</span></span>
<span data-ttu-id="34159-191">En fullständig förklaring av hur toouse hello skriptet toocreate utvecklings- och testmiljöer, se [med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="34159-191">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="34159-192">hello JSON-konfigurationsfil anger hello information om vad är toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="34159-192">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="34159-193">Den omfattar hello information som du angav när du skapade hello projekt, till exempel hello namn och användarnamn för hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="34159-193">It includes hello information that you specified when you created hello project, such as hello name and username for hello website.</span></span> <span data-ttu-id="34159-194">Den omfattar också hello databasen tooprovision eventuella.</span><span class="sxs-lookup"><span data-stu-id="34159-194">It also includes hello database tooprovision, if any.</span></span> <span data-ttu-id="34159-195">hello följande kod visar ett exempel JSON-konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="34159-195">hello following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="34159-196">Du kan redigera hello JSON configuration file toochange vad distribueras.</span><span class="sxs-lookup"><span data-stu-id="34159-196">You can edit hello JSON configuration file toochange what is deployed.</span></span> <span data-ttu-id="34159-197">Ett avsnitt på webbplatsen krävs men hello databasen avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="34159-197">A webSite section is required, but hello database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34159-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34159-198">Next steps</span></span>
<span data-ttu-id="34159-199">Mer information finns i [publicera WebApplicationVM (Windows PowerShell-skript)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="34159-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

