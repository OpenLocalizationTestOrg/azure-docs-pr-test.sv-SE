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
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standardstorleken TEMP-mappen är för liten för en cloud service web/worker roll
hello standard tillfälliga katalogen för ett moln rolltjänst worker eller webbplats har en maximal storlek på 100 MB, som kan bli fullständig vid något tillfälle. Den här artikeln beskriver hur tooavoid få slut på utrymme för hello tillfällig katalog.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Varför kör slut på diskutrymme?
hello standard Windows-miljövariablerna TEMP och TMP är tillgängliga toocode som körs i ditt program. Både TEMP och TMP peka tooa samma katalog som har en maximal storlek på 100 MB. Alla data som lagras i den här katalogen beständig inte över hello livscykeln för hello Molntjänsten; Om hello rollinstanser i en molnbaserad tjänst har återvunnits har hello directory rensats.

## <a name="suggestion-toofix-hello-problem"></a>Förslag toofix hello problem
Implementera en hello följande alternativ:

* Konfigurera en resurs för lokal lagring och åtkomst till den direkt i stället för TEMP eller TMP. tooaccess en resurs för lokal lagring från kod som körs i ditt program anropet hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metod.
* Konfigurera en resurs för lokal lagring och hello TEMP och TMP kataloger toopoint toohello sökvägen till resursen för hello lokal lagring. Den här ändringen ska utföras i hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metod.

hello visar följande kodexempel hur toomodify hello mål kataloger för TEMP och TMP från inom hello OnStart-metoden:

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

## <a name="next-steps"></a>Nästa steg
Läser en blogg som beskriver [hur tooincrease hello storleken på hello Azure Web rollen ASP.NET tillfällig mapp](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Visa mer [felsökning artiklar](/?tag=top-support-issue&product=cloud-services) för molntjänster.

toolearn hur tootroubleshoot moln rolltjänst problem med hjälp av Azure PaaS diagnostikdata för datorn, visa [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
