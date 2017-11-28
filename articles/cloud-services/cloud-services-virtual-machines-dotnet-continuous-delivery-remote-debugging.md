---
title: "aaaEnable fjärrfelsökning med kontinuerlig leverans | Microsoft Docs"
description: "Lär dig hur tooenable fjärrfelsökning när du använder kontinuerlig leverans toodeploy tooAzure"
services: cloud-services
documentationcenter: .net
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: d9d9d1cfe5304c9526586a9164f172746a448e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a><span data-ttu-id="99e76-103">Aktivera fjärrfelsökning när du använder kontinuerlig leverans toopublish tooAzure</span><span class="sxs-lookup"><span data-stu-id="99e76-103">Enable remote debugging when using continuous delivery toopublish tooAzure</span></span>
<span data-ttu-id="99e76-104">Du kan aktivera fjärrfelsökning i Azure, för molntjänster eller virtuella datorer när du använder [kontinuerlig leverans](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="99e76-104">You can enable remote debugging in Azure, for cloud services or virtual machines, when you use [continuous delivery](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure by following these steps.</span></span>

## <a name="enabling-remote-debugging-for-cloud-services"></a><span data-ttu-id="99e76-105">Aktivera fjärrfelsökning för molntjänster</span><span class="sxs-lookup"><span data-stu-id="99e76-105">Enabling remote debugging for cloud services</span></span>
1. <span data-ttu-id="99e76-106">På hello build-agent, konfigurera hello inledande miljö för Azure som beskrivs i [kommandoradsverktyget skapa för Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span><span class="sxs-lookup"><span data-stu-id="99e76-106">On hello build agent, set up hello initial environment for Azure as outlined in [Command-Line Build for Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span></span>
2. <span data-ttu-id="99e76-107">Eftersom hello remote debug runtime (msvsmon.exe) krävs för hello paketet, installera hello **fjärrverktyg för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="99e76-107">Because hello remote debug runtime (msvsmon.exe) is required for hello package, install hello **Remote Tools for Visual Studio**.</span></span>

    * [<span data-ttu-id="99e76-108">Fjärrverktyg för Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="99e76-108">Remote Tools for Visual Studio 2017</span></span>](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [<span data-ttu-id="99e76-109">Fjärrverktyg för Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="99e76-109">Remote Tools for Visual Studio 2015</span></span>](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [<span data-ttu-id="99e76-110">Fjärrverktyg för Visual Studio 2013 Update 5</span><span class="sxs-lookup"><span data-stu-id="99e76-110">Remote Tools for Visual Studio 2013 Update 5</span></span>](https://www.microsoft.com/download/details.aspx?id=48156)
    
    <span data-ttu-id="99e76-111">Alternativt kan kopiera du hello remote debug-binärfiler från ett system som har Visual Studio installerat.</span><span class="sxs-lookup"><span data-stu-id="99e76-111">As an alternative, you can copy hello remote debug binaries from a system that has Visual Studio installed.</span></span>

3. <span data-ttu-id="99e76-112">Skapa ett certifikat som beskrivs i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="99e76-112">Create a certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="99e76-113">Hålla hello .pfx och RDP tumavtryck för certifikat och överföra hello toohello mål moln certifikattjänst.</span><span class="sxs-lookup"><span data-stu-id="99e76-113">Keep hello .pfx and RDP certificate thumbprint and upload hello certificate toohello target cloud service.</span></span>
4. <span data-ttu-id="99e76-114">Använd följande alternativ i hello MSBuild kommandoraden toobuild i paketet med fjärråtkomst felsökning aktiverat hello.</span><span class="sxs-lookup"><span data-stu-id="99e76-114">Use hello following options in hello MSBuild command line toobuild and package with remote debug enabled.</span></span> <span data-ttu-id="99e76-115">(Ersätt faktiska sökvägarna tooyour system- och filer för hello vinkel omges objekt).</span><span class="sxs-lookup"><span data-stu-id="99e76-115">(Substitute actual paths tooyour system and project files for hello angle-bracketed items.)</span></span>
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    <span data-ttu-id="99e76-116">`VSX64RemoteDebuggerPath`är hello toohello sökväg som innehåller msvsmon.exe i hello fjärrverktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99e76-116">`VSX64RemoteDebuggerPath` is hello path toohello folder containing msvsmon.exe in hello Remote Tools for Visual Studio.</span></span>
    <span data-ttu-id="99e76-117">`RemoteDebuggerConnectorVersion`är hello Azure SDK-version i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="99e76-117">`RemoteDebuggerConnectorVersion` is hello Azure SDK version in your cloud service.</span></span> <span data-ttu-id="99e76-118">Det bör också matcha hello-version som installeras med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99e76-118">It should also match hello version installed with Visual Studio.</span></span>
5. <span data-ttu-id="99e76-119">Publicera toohello Måltjänsten moln med hjälp av hello-paket- och .cscfg-fil som skapas i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="99e76-119">Publish toohello target cloud service by using hello package and .cscfg file generated in hello previous step.</span></span>
6. <span data-ttu-id="99e76-120">Importera hello certifikatet (.pfx-filen) toohello dator som har Visual Studio med Azure SDK för .NET installerats.</span><span class="sxs-lookup"><span data-stu-id="99e76-120">Import hello certificate (.pfx file) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span> <span data-ttu-id="99e76-121">Se till att tooimport toohello `CurrentUser\My` certifikatarkiv, annars koppling toohello felsökning i Visual Studio misslyckas.</span><span class="sxs-lookup"><span data-stu-id="99e76-121">Make sure tooimport toohello `CurrentUser\My` certificate store, otherwise attaching toohello debugger in Visual Studio will fail.</span></span>

## <a name="enabling-remote-debugging-for-virtual-machines"></a><span data-ttu-id="99e76-122">Aktivera fjärrfelsökning för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="99e76-122">Enabling remote debugging for virtual machines</span></span>
1. <span data-ttu-id="99e76-123">Skapa en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="99e76-123">Create an Azure virtual machine.</span></span> <span data-ttu-id="99e76-124">Se [skapa en virtuell dator som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [skapa och hantera virtuella Azure-datorer i Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="99e76-124">See [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [Create and Manage Azure Virtual Machines in Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
2. <span data-ttu-id="99e76-125">På hello [Azure klassiska Portalsida](http://go.microsoft.com/fwlink/p/?LinkID=269851), visa hello virtuella datorns instrumentpanelen toosee hello virtuella datorns **RDP CERTIFIKATETS TUMAVTRYCK**.</span><span class="sxs-lookup"><span data-stu-id="99e76-125">On hello [Azure classic portal page](http://go.microsoft.com/fwlink/p/?LinkID=269851), view hello virtual machine's dashboard toosee hello virtual machine’s **RDP CERTIFICATE THUMBPRINT**.</span></span> <span data-ttu-id="99e76-126">Det här värdet används för hello `ServerThumbprint` värdet i hello tilläggets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="99e76-126">This value is used for hello `ServerThumbprint` value in hello extension configuration.</span></span>
3. <span data-ttu-id="99e76-127">Skapa ett klientcertifikat som beskrivs i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md) (Håll hello .pfx och tumavtrycket för RDP).</span><span class="sxs-lookup"><span data-stu-id="99e76-127">Create a client certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md) (keep hello .pfx and RDP certificate thumbprint).</span></span>
4. <span data-ttu-id="99e76-128">Installera Azure Powershell (version 0.7.4 eller senare) som beskrivs i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99e76-128">Install Azure Powershell (version 0.7.4 or later) as outlined in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
5. <span data-ttu-id="99e76-129">Kör hello följande skript tooenable hello RemoteDebug tillägg.</span><span class="sxs-lookup"><span data-stu-id="99e76-129">Run hello following script tooenable hello RemoteDebug extension.</span></span> <span data-ttu-id="99e76-130">Ersätt hello sökvägar och personliga data med ditt eget, till exempel din prenumerationsnamn, tjänstnamn och tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="99e76-130">Replace hello paths and personal data with your own, such as your subscription name, service name, and thumbprint.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="99e76-131">Det här skriptet har konfigurerats för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="99e76-131">This script is configured for Visual Studio 2015.</span></span> <span data-ttu-id="99e76-132">Om du använder Visual Studio 2013 eller Visual Studio 2017 ändra hello `$referenceName` och `$extensionName` tilldelningar nedan för`RemoteDebugVS2013` eller `RemoteDebugVS2017`.</span><span class="sxs-lookup"><span data-stu-id="99e76-132">If you’re using Visual Studio 2013 or Visual Studio 2017, modify hello `$referenceName` and `$extensionName` assignments below too`RemoteDebugVS2013` or `RemoteDebugVS2017`.</span></span>

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. <span data-ttu-id="99e76-133">Importera hello certifikatet (.pfx) toohello dator som har Visual Studio med Azure SDK för .NET installerats.</span><span class="sxs-lookup"><span data-stu-id="99e76-133">Import hello certificate (.pfx) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span>

