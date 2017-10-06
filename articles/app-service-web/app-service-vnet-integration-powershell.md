---
title: "aaaConnect app tooyour virtuella nätverket med hjälp av PowerShell"
description: "Instruktioner om hur tooconnect tooand fungerar med virtuella nätverk med hjälp av PowerShell"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: a5c76e77-972a-431c-b14b-3611dae1631b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: ccompy
ms.openlocfilehash: c9d0fa99d02cab7b2c7211a1b2f7b7d0cd27ee8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a>Ansluta appen tooyour virtuella nätverket med hjälp av PowerShell
## <a name="overview"></a>Översikt
Du kan ansluta din app (webb, Mobil eller API) i Azure App Service tooan virtuella Azure-nätverk (VNet) i din prenumeration. Den här funktionen kallas VNet-Integration. Hej VNet integrationsfunktionen ska inte förväxlas med hello Apptjänstmiljö funktionen som gör att du toorun en instans av Azure App Service i ditt virtuella nätverk.

Hej VNet integrationsfunktionen har ett användargränssnitt (UI) i hello nya portalen som du kan använda toointegrate med virtuella nätverk som distribueras med hjälp av hello klassiska distributionsmodellen eller hello Azure Resource Manager-distributionsmodellen. Om du vill toolearn mer om hello-funktionen, se [integrera din app med Azure-nätverk](web-sites-integrate-with-vnet.md).

Den här artikeln är inte om hur toouse hello UI men i stället om hur tooenable integrering med hjälp av PowerShell. Eftersom hello-kommandon för varje distributionsmodell är olika, har den här artikeln ett avsnitt för varje distributionsmodell.  

Se till att du har innan du fortsätter med den här artikeln:

* hello senaste Azure PowerShell SDK har installerats. Du kan installera det med hello installationsprogram för webbplattform.
* En app i Azure App Service körs i en Standard eller Premium-SKU.

## <a name="classic-virtual-networks"></a>Klassiska virtuella nätverk
Det här avsnittet beskrivs tre uppgifter för virtuella nätverk som använder hello klassiska distributionsmodellen:

1. Ansluta din app tooa redan befintliga virtuella nätverk som har en gateway och har konfigurerats för plats-till-plats-anslutning.
2. Uppdatera din information för integrering av virtuellt nätverk för din app.
3. Koppla från din app från det virtuella nätverket.

### <a name="connect-an-app-tooa-classic-vnet"></a>Ansluta en app tooa klassiska virtuella nätverk
tooconnect en app tooa virtuellt nätverk, Följ dessa tre steg:

1. Deklarera toohello webbprogram som den ansluter till ett visst virtuellt nätverk. hello app ska generera ett certifikat som kommer att få toohello virtuellt nätverk för punkt-till-plats-anslutning.
2. Överför hello web app certifikat toohello virtuella nätverk och hämta hello punkt-till-plats VPN-paketet URI.
3. Uppdatera hello webbapp virtuell nätverksanslutning med hello punkt-till-plats-paket URI.

hello första och tredje steg kan användas i skript, men hello andra steget kräver en gång, manuell åtgärd via hello-portalen eller åtkomst tooperform **PLACERA** eller **korrigering** åtgärder på hello virtuellt nätverk Azure Resource Manager-slutpunkt. Kontakta Azure-supporten toohave alternativet är aktiverat. Innan du börjar bör du kontrollera att du har ett klassiskt virtuellt nätverk som har punkt-till-plats-anslutning redan är aktiverad och distribuerad gateway. toocreate hello gateway och aktivera punkt-till-plats-anslutning måste toouse hello portal enligt beskrivningen i [att skapa en VPN-gateway][createvpngateway].

hello klassiska virtuella nätverk måste toobe i hello planerar samma prenumeration som din Apptjänst innehåller hello appen som du integrerar med.

##### <a name="set-up-azure-powershell-sdk"></a>Konfigurera Azure PowerShell SDK
Öppna ett PowerShell-fönster och konfigurera Azure-konto och prenumeration med hjälp av:

    Login-AzureRmAccount

Kommandot öppnas en fråga tooget dina Azure-autentiseringsuppgifter. När du loggar in med någon av följande kommandon tooselect hello prenumeration som du vill toouse hello. Kontrollera att du använder hello-prenumeration som ditt virtuella nätverk och App Service-plan som finns i.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

eller

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Variabler som används i den här artikeln
toosimplify kommandon vi en **$Configuration** PowerShell variabel med hello viss konfiguration.

Ange en variabel enligt följande i PowerShell med hello följande parametrar:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

hello måste app vara hello plats utan blanksteg. USA, västra är till exempel westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

hello nästa objekt är där hello certifikat ska skrivas. Det bör vara en skrivbar sökväg på den lokala datorn. Se till att tooinclude .cer hello slutet.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

toosee vad du ange typen **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

hello resten av det här avsnittet förutsätter att du har en variabel skapas som bara beskrivs.

##### <a name="declare-hello-virtual-network-toohello-app"></a>Deklarera hello virtuellt nätverk toohello app
Använd hello efter att den kommer att använda den här virtuella nätverks kommandot tootell hello app. Detta innebär att hello app toogenerate nödvändiga certifikat:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Om det här kommandot lyckas, **$vnet** ska ha en **egenskaper** variabel i den. Hej **egenskaper** variabeln ska innehålla både ett certifikat-tumavtryck och hello certifikatdata.

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a>Överför hello web app certifikat toohello virtuellt nätverk
En manuell enstaka steg krävs för varje prenumeration och en kombination av virtuellt nätverk. Det vill säga om du ansluter appar i prenumerationen A tooVirtual nätverk A måste toodo det här steget endast när du konfigurerar oavsett hur många appar. Om du lägger till ett nytt app tooanother virtuellt nätverk, behöver du toodo igen. hello anledningen är att en uppsättning certifikat som genereras på en prenumerationsnivå i Azure App Service och hello set skapas en gång för varje virtuellt nätverk som hello appar ska ansluta till.

hello certifikat kommer har redan ställts in om du har följt de här stegen eller om du integrerat med hello samma virtuella nätverk med hjälp av hello-portalen.

hello första steget är toogenerate hello .cer-fil. hello andra steget är tooupload hello .cer-fil tooyour virtuellt nätverk. toogenerate hello .cer-fil från hello API-anrop i hello tidigare steg, genom att köra följande kommandon hello.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

hello certifikat kommer att hittas hello plats som **$Configuration.GeneratedCertificatePath** anger.

tooupload hello certifikatet manuellt, använda hello [Azure-portalen] [ azureportal] och **Bläddra virtuella nätverk (klassiska)** > **-VPN-anslutningar**  >  **Punkt-till-plats** > **hantera certifikat**. Härifrån kan du överföra ditt certifikat.

##### <a name="get-hello-point-to-site-package"></a>Hämta hello punkt-till-plats-paket
hello nästa steg i hur du konfigurerar en virtuell nätverksanslutning på ett webbprogram är tooget hello punkt-till-plats-paket och tillhandahålla tooyour webbprogram.

Spara hello följande mallfil tooa anropas GetNetworkPackageUri.json någonstans på datorn, till exempel C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Ange indataparametrar:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Anropa hello skript:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


hello variabeln **$output. Outputs.packageUri** innehåller nu hello paketet URI toobe angivna tooyour webbprogram.

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a>Överföra hello punkt-till-plats-paketet tooyour app
hello sista steget är tooprovide hello app med det här paketet. Kör bara hello nästa kommando:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Om du ombeds att du skriver över en befintlig resurs tooconfirm, kontrollerar du att tooallow den.

När det här kommandot lyckas, bör din app nu anslutna toohello virtuellt nätverk. tooconfirm lyckades, gå tooyour app konsol och Skriv hello följande:

    SET WEBSITE_

Om det finns en miljövariabel som kallas WEBSITE_VNETNAME som har ett värde som matchar hello namnet på virtuella hello målnätverket, har alla konfigurationer lyckades.

### <a name="update-classic-vnet-integration-information"></a>Uppdatera information om klassiska VNet-integrering
tooupdate eller synkronisera din information, helt enkelt Upprepa hello steg som du har följt när du skapade hello integrering i hello första plats. De här stegen är:

1. Definiera din konfigurationsinformation.
2. Deklarera hello virtuellt nätverk toohello app.
3. Hämta hello punkt-till-plats-paket.
4. Överföra hello punkt-till-plats-paketet tooyour app.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Koppla appen från ett klassiskt virtuellt nätverk
toodisconnect hello app måste hello konfigurationsinformation som angavs under virtuell nätverksintegration. Använda informationen finns sedan ett kommando toodisconnect appen från det virtuella nätverket.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Hanteraren för virtuella nätverk
Hanteraren för virtuella nätverk har Azure Resource Manager API: er, vilket förenklar vissa processer jämfört med klassiska virtuella nätverk. Vi har ett skript som hjälper dig att slutföra hello följande uppgifter:

* Skapa ett virtuellt nätverk för hanteraren för filserverresurser och integrera din app med den.
* Skapa en gateway, konfigurera punkt-till-plats-anslutning i ett befintligt virtuellt nätverk för Resource Manager och sedan integrera din app med den.
* Integrera din app med en befintlig Resource Manager virtuella nätverk som har en gateway och anslutning aktiverad punkt-till-plats.
* Koppla från din app från det virtuella nätverket.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Integrationsskriptet i hanteraren för filserverresurser VNet App Service
Kopiera hello följande skript och spara den tooa filen. Om du inte vill toouse hello skript känna sig fria toolearn från den toosee hur tooset saker upp med ett virtuellt nätverk för hanteraren för filserverresurser.

    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate toothis VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up tooan hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with hello following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create hello virtual network and add it here. hello way this works is:
        # 1) Add hello VNET association toohello App. This allows hello App toogenerate certificates, etc. for hello VNET.
        # 2) Create hello VNET and VNET gateway, add hello certificates, create hello public IP, etc., required for hello gateway
        # 3) Get hello VPN package from hello gateway and pass it back toohello App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association tooVNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, hello gateway should be able toobe joined tooan App, but may require some minor tweaking. We will declare toohello App now toouse this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET toointegrate with" $vnets $vnetNames

        # We need toocheck if this VNET is able toobe joined tooa App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have hello right certificate, we will need tooadd it.
                # If it doesn't have a point-to-site range, we will need tooadd it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need toocreate one.
            Write-Host "This Virtual Network has no gateway. I will need toocreate one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in hello address space $($vnet.AddressSpace.AddressPrefixes), with hello following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with hello following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need toocreate one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of hello Vpn type. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need toocheck if hello certificate here exists in hello gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting hello VPN package and giving it toohello App
        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected tooa VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound toothis account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter hello Resource Group of your App"

    $appName = Read-Host "Please enter hello Name of your App"

    $options = @("Add a NEW Virtual Network tooan App", "Add an EXISTING Virtual Network tooan App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want toodo?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Spara en kopia av hello skript. Den kallas V2VnetAllinOne.ps1 i den här artikeln, men du kan använda ett annat namn. Det finns inga argument för det här skriptet. Kör bara den. hello först öppna hello skriptet gör är att be dig toosign i. När du loggar in hello skript hämtar information om ditt konto och returnerar en lista över prenumerationer. Räknas hello begäran om autentiseringsuppgifterna inte hello inledande skriptkörningen ser ut så här:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo-prenumeration (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lila Demo prenumeration (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Välj ett alternativ: 3

    Konto: ccompy@microsoft.com miljö: AzureCloud prenumerationen: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klient: 722278f-fef1-499f-91ab-2323d011db47

    Ange hello resursgruppen för din App: hcdemo rg genom att ange hello namnet på din App: v2vnetpowershell vad vill du vill toodo?

    1) Lägg till ett nytt virtuellt nätverk tooan App
    2) Lägg till ett befintligt virtuellt nätverk tooan App
    3) Ta bort ett virtuellt nätverk från en App

hello resten av det här avsnittet beskrivs var och en av de tre alternativen.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Skapa ett VNet med Resource Manager och integrera med den
toocreate ett nytt virtuellt nätverk som använder hello Resource Manager-distributionsmodellen och integrera den med din app, Välj **1) Lägg till ett nytt virtuellt nätverk tooan App**. Det här uppmanas du att hello namnet på hello virtuellt nätverk. I min fall som du ser i följande inställningar hello, använde jag hello namn, v2pshell.

hello skriptet ger hello information om hello virtuella nätverk som håller på att skapas. Om användarna vill ändra jag hello värden. Jag har skapat ett virtuellt nätverk som har hello följande inställningar i det här exemplet körning:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Om du vill toochange hello värden, skriver **Y** hello ändrar. När du är nöjd med hello virtuella nätverksinställningar skriver **N** eller helt enkelt tryck på RETUR när du tillfrågas om hur du ändrar hello inställningar. Därifrån på tills den har slutförts hello skript kommer att meddela dig några av IT-avdelningen ”bindningsikonerna göra förrän den startar toocreate hello virtuell nätverksgateway. Steget kan ta upp tooan timme. Det finns ingen förloppsindikator under den här fasen men hello skriptet att meddela när hello gateway har skapats.

När hello skriptet är klar ska det stå **avslutad**. Nu har du ett virtuellt nätverk för Resource Manager som har hello namn och inställningarna som du valt. Den här nytt virtuellt nätverk ska också integreras med din app.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integrera din app med en befintlig Resource Manager-VNet
När du integrerar med ett befintligt virtuellt nätverk om du anger ett virtuellt nätverk för Resource Manager som inte har en gateway eller en punkt-till-plats-anslutning kan skapar hello skriptet som. Om hello VNET har redan sakerna ställa in, går hello skript raka toohello appintegrering. toostart den här processen kan bara markera **2) Lägg till ett befintligt virtuellt nätverk tooan App**.

Det här alternativet fungerar bara om du har en befintlig Resource Manager virtuellt nätverk som är i hello samma prenumeration som din app. När du har valt alternativet hello visas en lista över dina virtuella Resource Manager-nätverk.   

    Select a VNET toointegrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Välj ett alternativ: 5

Markera bara hello virtuella nätverk som du vill toointegrate med. Om du redan har en gateway med punkt-till-plats-anslutning aktiverad hello skript helt enkelt att integrera din app med ditt virtuella nätverk. Om du inte har en gateway måste toospecify hello gateway-undernätet. Gateway-undernätet måste vara i ditt virtuella nätverk och det får inte finnas i andra undernät. Om du har ett virtuellt nätverk utan en gateway och kör det här steget saker se ut så här:

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Jag har skapat en virtuell nätverksgateway som har hello följande inställningar i det här exemplet:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association tooVNET

Om du vill toochange någon av dessa inställningar kan göra du detta. I annat fall trycker på RETUR och hello skriptet skapar din gateway och ansluta din app tooyour virtuellt nätverk. hello gateway Skapandetid är dock fortfarande en timma, så se till att du komma ihåg. När allt är klart hello skript står **avslutad**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Koppla från din app från en Resource Manager-VNet
Koppla appen från det virtuella nätverket inte ta bort hello gateway eller inaktivera punkt-till-plats-anslutning. Du kanske, när alla använder det något annat. Den också kopplar inte den från andra appar än hello något du har angett. tooperform åtgärden väljer **3) ta bort ett virtuellt nätverk från en App**. När du gör det ser ut så här:

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Även om hello skript står ta bort, tas inte bort hello virtuellt nätverk. Det bara att ta bort hello-integration. När du har bekräftat att det här är vad du vill toodo, hello kommandot bearbetas ganska snabbt och anger **SANT** när du är klar.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
