---
title: "Standard TEMP-mappen är för litet för en roll | Microsoft Docs"
description: "En rolltjänst för molnet har en begränsad mängd utrymme för TEMP-mappen. Den här artikeln innehåller några förslag på hur du undviker utrymmet tar slut."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 577d090a009eb2331b401273257c7cc7c1eea772
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="12b4a-104">Standardstorleken TEMP-mappen är för liten för en cloud service web/worker roll</span><span class="sxs-lookup"><span data-stu-id="12b4a-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="12b4a-105">Den tillfälliga standardkatalogen av en cloud service worker eller webbplats roll har en maximal storlek på 100 MB, som kan bli fullständig vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="12b4a-105">The default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="12b4a-106">Den här artikeln beskriver hur du undviker slut på diskutrymme för den temporära katalogen.</span><span class="sxs-lookup"><span data-stu-id="12b4a-106">This article describes how to avoid running out of space for the temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="12b4a-107">Varför kör slut på diskutrymme?</span><span class="sxs-lookup"><span data-stu-id="12b4a-107">Why do I run out of space?</span></span>
<span data-ttu-id="12b4a-108">Standard Windows miljövariablerna TEMP och TMP är tillgängliga för kod som körs i ditt program.</span><span class="sxs-lookup"><span data-stu-id="12b4a-108">The standard Windows environment variables TEMP and TMP are available to code that is running in your application.</span></span> <span data-ttu-id="12b4a-109">Både TEMP och TMP pekar på en katalog som har en maximal storlek på 100 MB.</span><span class="sxs-lookup"><span data-stu-id="12b4a-109">Both TEMP and TMP point to a single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="12b4a-110">Alla data som lagras i den här katalogen är inte beständiga över livscykeln för Molntjänsten; Om rollinstanser i en molnbaserad tjänst har återvunnits kan rensa katalogen.</span><span class="sxs-lookup"><span data-stu-id="12b4a-110">Any data that is stored in this directory is not persisted across the lifecycle of the cloud service; if the role instances in a cloud service are recycled, the directory is cleaned.</span></span>

## <a name="suggestion-to-fix-the-problem"></a><span data-ttu-id="12b4a-111">Förslag på problemet</span><span class="sxs-lookup"><span data-stu-id="12b4a-111">Suggestion to fix the problem</span></span>
<span data-ttu-id="12b4a-112">Implementera ett av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="12b4a-112">Implement one of the following alternatives:</span></span>

* <span data-ttu-id="12b4a-113">Konfigurera en resurs för lokal lagring och åtkomst till den direkt i stället för TEMP eller TMP.</span><span class="sxs-lookup"><span data-stu-id="12b4a-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="12b4a-114">Om du vill komma åt en resurs för lokal lagring från kod som körs i ditt program måste anropa den [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="12b4a-114">To access a local storage resource from code that is running within your application, call the [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="12b4a-115">Konfigurera en resurs för lokal lagring och peka TEMP och TMP kataloger att peka till sökvägen till resursen för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="12b4a-115">Configure a local storage resource, and point the TEMP and TMP directories to point to the path of the local storage resource.</span></span> <span data-ttu-id="12b4a-116">Den här ändringen ska utföras i den [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="12b4a-116">This modification should be performed within the [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="12b4a-117">Följande kodexempel visar hur du ändrar katalogerna som mål för TEMP och TMP från inom OnStart-metoden:</span><span class="sxs-lookup"><span data-stu-id="12b4a-117">The following code example shows how to modify the target directories for TEMP and TMP from within the OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="12b4a-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12b4a-118">Next steps</span></span>
<span data-ttu-id="12b4a-119">Läser en blogg som beskriver [så här ökar du storleken på Azure Web rollen ASP.NET tillfällig mapp](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="12b4a-119">Read a blog that describes [How to increase the size of the Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="12b4a-120">Visa mer [felsökning artiklar](/?tag=top-support-issue&product=cloud-services) för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="12b4a-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="12b4a-121">Lär du dig hur du felsöker cloud service rollen problem med hjälp av Azure PaaS datorn diagnostikdata [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="12b4a-121">To learn how to troubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
