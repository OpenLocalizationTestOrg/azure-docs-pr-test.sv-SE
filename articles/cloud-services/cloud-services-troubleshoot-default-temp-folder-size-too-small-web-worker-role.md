---
title: "aaaDefault TEMP-mappen är för litet för en roll | Microsoft Docs"
description: "En rolltjänst för molnet har en begränsad mängd utrymme för hello TEMP-mappen. Den här artikeln innehåller några förslag på hur tooavoid utrymmet börjar ta slut."
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
ms.openlocfilehash: 307dc20f3264e29d122a6616be0028d2ec1282c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="b2d57-104">Standardstorleken TEMP-mappen är för liten för en cloud service web/worker roll</span><span class="sxs-lookup"><span data-stu-id="b2d57-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="b2d57-105">hello standard tillfälliga katalogen för ett moln rolltjänst worker eller webbplats har en maximal storlek på 100 MB, som kan bli fullständig vid något tillfälle.</span><span class="sxs-lookup"><span data-stu-id="b2d57-105">hello default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="b2d57-106">Den här artikeln beskriver hur tooavoid få slut på utrymme för hello tillfällig katalog.</span><span class="sxs-lookup"><span data-stu-id="b2d57-106">This article describes how tooavoid running out of space for hello temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="b2d57-107">Varför kör slut på diskutrymme?</span><span class="sxs-lookup"><span data-stu-id="b2d57-107">Why do I run out of space?</span></span>
<span data-ttu-id="b2d57-108">hello standard Windows-miljövariablerna TEMP och TMP är tillgängliga toocode som körs i ditt program.</span><span class="sxs-lookup"><span data-stu-id="b2d57-108">hello standard Windows environment variables TEMP and TMP are available toocode that is running in your application.</span></span> <span data-ttu-id="b2d57-109">Både TEMP och TMP peka tooa samma katalog som har en maximal storlek på 100 MB.</span><span class="sxs-lookup"><span data-stu-id="b2d57-109">Both TEMP and TMP point tooa single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="b2d57-110">Alla data som lagras i den här katalogen beständig inte över hello livscykeln för hello Molntjänsten; Om hello rollinstanser i en molnbaserad tjänst har återvunnits har hello directory rensats.</span><span class="sxs-lookup"><span data-stu-id="b2d57-110">Any data that is stored in this directory is not persisted across hello lifecycle of hello cloud service; if hello role instances in a cloud service are recycled, hello directory is cleaned.</span></span>

## <a name="suggestion-toofix-hello-problem"></a><span data-ttu-id="b2d57-111">Förslag toofix hello problem</span><span class="sxs-lookup"><span data-stu-id="b2d57-111">Suggestion toofix hello problem</span></span>
<span data-ttu-id="b2d57-112">Implementera en hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b2d57-112">Implement one of hello following alternatives:</span></span>

* <span data-ttu-id="b2d57-113">Konfigurera en resurs för lokal lagring och åtkomst till den direkt i stället för TEMP eller TMP.</span><span class="sxs-lookup"><span data-stu-id="b2d57-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="b2d57-114">tooaccess en resurs för lokal lagring från kod som körs i ditt program anropet hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="b2d57-114">tooaccess a local storage resource from code that is running within your application, call hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="b2d57-115">Konfigurera en resurs för lokal lagring och hello TEMP och TMP kataloger toopoint toohello sökvägen till resursen för hello lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="b2d57-115">Configure a local storage resource, and point hello TEMP and TMP directories toopoint toohello path of hello local storage resource.</span></span> <span data-ttu-id="b2d57-116">Den här ändringen ska utföras i hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="b2d57-116">This modification should be performed within hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="b2d57-117">hello visar följande kodexempel hur toomodify hello mål kataloger för TEMP och TMP från inom hello OnStart-metoden:</span><span class="sxs-lookup"><span data-stu-id="b2d57-117">hello following code example shows how toomodify hello target directories for TEMP and TMP from within hello OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // hello local resource declaration must have been added toothe
            // service definition file for hello role named WorkerRole1:
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

            // hello rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="b2d57-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2d57-118">Next steps</span></span>
<span data-ttu-id="b2d57-119">Läser en blogg som beskriver [hur tooincrease hello storleken på hello Azure Web rollen ASP.NET tillfällig mapp](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2d57-119">Read a blog that describes [How tooincrease hello size of hello Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="b2d57-120">Visa mer [felsökning artiklar](/?tag=top-support-issue&product=cloud-services) för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="b2d57-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="b2d57-121">toolearn hur tootroubleshoot moln rolltjänst problem med hjälp av Azure PaaS diagnostikdata för datorn, visa [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2d57-121">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
