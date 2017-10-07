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
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a>Aktivera fjärrfelsökning när du använder kontinuerlig leverans toopublish tooAzure
Du kan aktivera fjärrfelsökning i Azure, för molntjänster eller virtuella datorer när du använder [kontinuerlig leverans](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure genom att följa dessa steg.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Aktivera fjärrfelsökning för molntjänster
1. På hello build-agent, konfigurera hello inledande miljö för Azure som beskrivs i [kommandoradsverktyget skapa för Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Eftersom hello remote debug runtime (msvsmon.exe) krävs för hello paketet, installera hello **fjärrverktyg för Visual Studio**.

    * [Fjärrverktyg för Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [Fjärrverktyg för Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [Fjärrverktyg för Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
    Alternativt kan kopiera du hello remote debug-binärfiler från ett system som har Visual Studio installerat.

3. Skapa ett certifikat som beskrivs i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md). Hålla hello .pfx och RDP tumavtryck för certifikat och överföra hello toohello mål moln certifikattjänst.
4. Använd följande alternativ i hello MSBuild kommandoraden toobuild i paketet med fjärråtkomst felsökning aktiverat hello. (Ersätt faktiska sökvägarna tooyour system- och filer för hello vinkel omges objekt).
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    `VSX64RemoteDebuggerPath`är hello toohello sökväg som innehåller msvsmon.exe i hello fjärrverktyg för Visual Studio.
    `RemoteDebuggerConnectorVersion`är hello Azure SDK-version i Molntjänsten. Det bör också matcha hello-version som installeras med Visual Studio.
5. Publicera toohello Måltjänsten moln med hjälp av hello-paket- och .cscfg-fil som skapas i hello föregående steg.
6. Importera hello certifikatet (.pfx-filen) toohello dator som har Visual Studio med Azure SDK för .NET installerats. Se till att tooimport toohello `CurrentUser\My` certifikatarkiv, annars koppling toohello felsökning i Visual Studio misslyckas.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Aktivera fjärrfelsökning för virtuella datorer
1. Skapa en virtuell Azure-dator. Se [skapa en virtuell dator som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [skapa och hantera virtuella Azure-datorer i Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. På hello [Azure klassiska Portalsida](http://go.microsoft.com/fwlink/p/?LinkID=269851), visa hello virtuella datorns instrumentpanelen toosee hello virtuella datorns **RDP CERTIFIKATETS TUMAVTRYCK**. Det här värdet används för hello `ServerThumbprint` värdet i hello tilläggets konfiguration.
3. Skapa ett klientcertifikat som beskrivs i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md) (Håll hello .pfx och tumavtrycket för RDP).
4. Installera Azure Powershell (version 0.7.4 eller senare) som beskrivs i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
5. Kör hello följande skript tooenable hello RemoteDebug tillägg. Ersätt hello sökvägar och personliga data med ditt eget, till exempel din prenumerationsnamn, tjänstnamn och tumavtryck.
   
   > [!NOTE]
   > Det här skriptet har konfigurerats för Visual Studio 2015. Om du använder Visual Studio 2013 eller Visual Studio 2017 ändra hello `$referenceName` och `$extensionName` tilldelningar nedan för`RemoteDebugVS2013` eller `RemoteDebugVS2017`.

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

6. Importera hello certifikatet (.pfx) toohello dator som har Visual Studio med Azure SDK för .NET installerats.

