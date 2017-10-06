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
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a><span data-ttu-id="44666-103">Ansluta appen tooyour virtuella nätverket med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="44666-103">Connect your app tooyour virtual network by using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="44666-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="44666-104">Overview</span></span>
<span data-ttu-id="44666-105">Du kan ansluta din app (webb, Mobil eller API) i Azure App Service tooan virtuella Azure-nätverk (VNet) i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44666-105">In Azure App Service, you can connect your app (web, mobile, or API) tooan Azure virtual network (VNet) in your subscription.</span></span> <span data-ttu-id="44666-106">Den här funktionen kallas VNet-Integration.</span><span class="sxs-lookup"><span data-stu-id="44666-106">This feature is called VNet Integration.</span></span> <span data-ttu-id="44666-107">Hej VNet integrationsfunktionen ska inte förväxlas med hello Apptjänstmiljö funktionen som gör att du toorun en instans av Azure App Service i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-107">hello VNet Integration feature should not be confused with hello App Service Environment feature, which allows you toorun an instance of Azure App Service in your virtual network.</span></span>

<span data-ttu-id="44666-108">Hej VNet integrationsfunktionen har ett användargränssnitt (UI) i hello nya portalen som du kan använda toointegrate med virtuella nätverk som distribueras med hjälp av hello klassiska distributionsmodellen eller hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="44666-108">hello VNet Integration feature has a user interface (UI) in hello new portal that you can use toointegrate with virtual networks that are deployed by using either hello classic deployment model or hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="44666-109">Om du vill toolearn mer om hello-funktionen, se [integrera din app med Azure-nätverk](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="44666-109">If you want toolearn more about hello feature, see [Integrate your app with an Azure virtual network](web-sites-integrate-with-vnet.md).</span></span>

<span data-ttu-id="44666-110">Den här artikeln är inte om hur toouse hello UI men i stället om hur tooenable integrering med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="44666-110">This article is not about how toouse hello UI but rather about how tooenable integration by using PowerShell.</span></span> <span data-ttu-id="44666-111">Eftersom hello-kommandon för varje distributionsmodell är olika, har den här artikeln ett avsnitt för varje distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="44666-111">Because hello commands for each deployment model are different, this article has a section for each deployment model.</span></span>  

<span data-ttu-id="44666-112">Se till att du har innan du fortsätter med den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="44666-112">Before you continue with this article, ensure that you have:</span></span>

* <span data-ttu-id="44666-113">hello senaste Azure PowerShell SDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="44666-113">hello latest Azure PowerShell SDK installed.</span></span> <span data-ttu-id="44666-114">Du kan installera det med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="44666-114">You can install this with hello Web Platform Installer.</span></span>
* <span data-ttu-id="44666-115">En app i Azure App Service körs i en Standard eller Premium-SKU.</span><span class="sxs-lookup"><span data-stu-id="44666-115">An app in Azure App Service running in a Standard or Premium SKU.</span></span>

## <a name="classic-virtual-networks"></a><span data-ttu-id="44666-116">Klassiska virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="44666-116">Classic virtual networks</span></span>
<span data-ttu-id="44666-117">Det här avsnittet beskrivs tre uppgifter för virtuella nätverk som använder hello klassiska distributionsmodellen:</span><span class="sxs-lookup"><span data-stu-id="44666-117">This section explains three tasks for virtual networks that use hello classic deployment model:</span></span>

1. <span data-ttu-id="44666-118">Ansluta din app tooa redan befintliga virtuella nätverk som har en gateway och har konfigurerats för plats-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="44666-118">Connect your app tooa preexisting virtual network that has a gateway and is configured for point-to-site connectivity.</span></span>
2. <span data-ttu-id="44666-119">Uppdatera din information för integrering av virtuellt nätverk för din app.</span><span class="sxs-lookup"><span data-stu-id="44666-119">Update your virtual network integration information for your app.</span></span>
3. <span data-ttu-id="44666-120">Koppla från din app från det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="44666-120">Disconnect your app from your virtual network.</span></span>

### <a name="connect-an-app-tooa-classic-vnet"></a><span data-ttu-id="44666-121">Ansluta en app tooa klassiska virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="44666-121">Connect an app tooa classic VNet</span></span>
<span data-ttu-id="44666-122">tooconnect en app tooa virtuellt nätverk, Följ dessa tre steg:</span><span class="sxs-lookup"><span data-stu-id="44666-122">tooconnect an app tooa virtual network, follow these three steps:</span></span>

1. <span data-ttu-id="44666-123">Deklarera toohello webbprogram som den ansluter till ett visst virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-123">Declare toohello web app that it will be joining a particular virtual network.</span></span> <span data-ttu-id="44666-124">hello app ska generera ett certifikat som kommer att få toohello virtuellt nätverk för punkt-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="44666-124">hello app will generate a certificate that will be given toohello virtual network for point-to-site connectivity.</span></span>
2. <span data-ttu-id="44666-125">Överför hello web app certifikat toohello virtuella nätverk och hämta hello punkt-till-plats VPN-paketet URI.</span><span class="sxs-lookup"><span data-stu-id="44666-125">Upload hello web app certificate toohello virtual network, and then retrieve hello point-to-site VPN package URI.</span></span>
3. <span data-ttu-id="44666-126">Uppdatera hello webbapp virtuell nätverksanslutning med hello punkt-till-plats-paket URI.</span><span class="sxs-lookup"><span data-stu-id="44666-126">Update hello web app's virtual network connection with hello point-to-site package URI.</span></span>

<span data-ttu-id="44666-127">hello första och tredje steg kan användas i skript, men hello andra steget kräver en gång, manuell åtgärd via hello-portalen eller åtkomst tooperform **PLACERA** eller **korrigering** åtgärder på hello virtuellt nätverk Azure Resource Manager-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="44666-127">hello first and third steps are fully scriptable, but hello second step requires a one-time, manual action through hello portal, or access tooperform **PUT** or **PATCH** actions on hello virtual network Azure Resource Manager endpoint.</span></span> <span data-ttu-id="44666-128">Kontakta Azure-supporten toohave alternativet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="44666-128">Contact Azure Support toohave this enabled.</span></span> <span data-ttu-id="44666-129">Innan du börjar bör du kontrollera att du har ett klassiskt virtuellt nätverk som har punkt-till-plats-anslutning redan är aktiverad och distribuerad gateway.</span><span class="sxs-lookup"><span data-stu-id="44666-129">Before you start, make sure that you have a classic virtual network that has point-to-site connectivity already enabled and a deployed gateway.</span></span> <span data-ttu-id="44666-130">toocreate hello gateway och aktivera punkt-till-plats-anslutning måste toouse hello portal enligt beskrivningen i [att skapa en VPN-gateway][createvpngateway].</span><span class="sxs-lookup"><span data-stu-id="44666-130">toocreate hello gateway and enable point-to-site connectivity, you need toouse hello portal as described at [Creating a VPN gateway][createvpngateway].</span></span>

<span data-ttu-id="44666-131">hello klassiska virtuella nätverk måste toobe i hello planerar samma prenumeration som din Apptjänst innehåller hello appen som du integrerar med.</span><span class="sxs-lookup"><span data-stu-id="44666-131">hello classic virtual network needs toobe in hello same subscription as your App Service plan that holds hello app that you are integrating with.</span></span>

##### <a name="set-up-azure-powershell-sdk"></a><span data-ttu-id="44666-132">Konfigurera Azure PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="44666-132">Set up Azure PowerShell SDK</span></span>
<span data-ttu-id="44666-133">Öppna ett PowerShell-fönster och konfigurera Azure-konto och prenumeration med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="44666-133">Open a PowerShell window and set up your Azure account and subscription by using:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="44666-134">Kommandot öppnas en fråga tooget dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="44666-134">That command will open a prompt tooget your Azure credentials.</span></span> <span data-ttu-id="44666-135">När du loggar in med någon av följande kommandon tooselect hello prenumeration som du vill toouse hello.</span><span class="sxs-lookup"><span data-stu-id="44666-135">After you sign in, use either of hello following commands tooselect hello subscription that you want toouse.</span></span> <span data-ttu-id="44666-136">Kontrollera att du använder hello-prenumeration som ditt virtuella nätverk och App Service-plan som finns i.</span><span class="sxs-lookup"><span data-stu-id="44666-136">Make sure that you are using hello subscription that your virtual network and App Service plan are in.</span></span>

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

<span data-ttu-id="44666-137">eller</span><span class="sxs-lookup"><span data-stu-id="44666-137">or</span></span>

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a><span data-ttu-id="44666-138">Variabler som används i den här artikeln</span><span class="sxs-lookup"><span data-stu-id="44666-138">Variables used in this article</span></span>
<span data-ttu-id="44666-139">toosimplify kommandon vi en **$Configuration** PowerShell variabel med hello viss konfiguration.</span><span class="sxs-lookup"><span data-stu-id="44666-139">toosimplify commands, we will set a **$Configuration** PowerShell variable with hello specific configuration.</span></span>

<span data-ttu-id="44666-140">Ange en variabel enligt följande i PowerShell med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="44666-140">Set a variable as follows in PowerShell with hello following parameters:</span></span>

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

<span data-ttu-id="44666-141">hello måste app vara hello plats utan blanksteg.</span><span class="sxs-lookup"><span data-stu-id="44666-141">hello app location should be hello location without any spaces.</span></span> <span data-ttu-id="44666-142">USA, västra är till exempel westus.</span><span class="sxs-lookup"><span data-stu-id="44666-142">For example, West US is westus.</span></span>

    $Configuration.WebAppLocation = "[Your web app Location]"

<span data-ttu-id="44666-143">hello nästa objekt är där hello certifikat ska skrivas.</span><span class="sxs-lookup"><span data-stu-id="44666-143">hello next item is where hello certificate should be written.</span></span> <span data-ttu-id="44666-144">Det bör vara en skrivbar sökväg på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="44666-144">It should be a writable path on your local computer.</span></span> <span data-ttu-id="44666-145">Se till att tooinclude .cer hello slutet.</span><span class="sxs-lookup"><span data-stu-id="44666-145">Make sure tooinclude .cer at hello end.</span></span>

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

<span data-ttu-id="44666-146">toosee vad du ange typen **$Configuration**.</span><span class="sxs-lookup"><span data-stu-id="44666-146">toosee what you set, type **$Configuration**.</span></span>

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

<span data-ttu-id="44666-147">hello resten av det här avsnittet förutsätter att du har en variabel skapas som bara beskrivs.</span><span class="sxs-lookup"><span data-stu-id="44666-147">hello rest of this section assumes that you have a variable created as just described.</span></span>

##### <a name="declare-hello-virtual-network-toohello-app"></a><span data-ttu-id="44666-148">Deklarera hello virtuellt nätverk toohello app</span><span class="sxs-lookup"><span data-stu-id="44666-148">Declare hello virtual network toohello app</span></span>
<span data-ttu-id="44666-149">Använd hello efter att den kommer att använda den här virtuella nätverks kommandot tootell hello app.</span><span class="sxs-lookup"><span data-stu-id="44666-149">Use hello following command tootell hello app that it will be using this particular virtual network.</span></span> <span data-ttu-id="44666-150">Detta innebär att hello app toogenerate nödvändiga certifikat:</span><span class="sxs-lookup"><span data-stu-id="44666-150">This will cause hello app toogenerate necessary certificates:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

<span data-ttu-id="44666-151">Om det här kommandot lyckas, **$vnet** ska ha en **egenskaper** variabel i den.</span><span class="sxs-lookup"><span data-stu-id="44666-151">If this command succeeds, **$vnet** should have a **Properties** variable in it.</span></span> <span data-ttu-id="44666-152">Hej **egenskaper** variabeln ska innehålla både ett certifikat-tumavtryck och hello certifikatdata.</span><span class="sxs-lookup"><span data-stu-id="44666-152">hello **Properties** variable should contain both a certificate thumbprint and hello certificate data.</span></span>

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a><span data-ttu-id="44666-153">Överför hello web app certifikat toohello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="44666-153">Upload hello web app certificate toohello virtual network</span></span>
<span data-ttu-id="44666-154">En manuell enstaka steg krävs för varje prenumeration och en kombination av virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-154">A manual, one-time step is required for each subscription and virtual network combination.</span></span> <span data-ttu-id="44666-155">Det vill säga om du ansluter appar i prenumerationen A tooVirtual nätverk A måste toodo det här steget endast när du konfigurerar oavsett hur många appar.</span><span class="sxs-lookup"><span data-stu-id="44666-155">That is, if you are connecting apps in Subscription A tooVirtual Network A, you will need toodo this step only once regardless of how many apps you configure.</span></span> <span data-ttu-id="44666-156">Om du lägger till ett nytt app tooanother virtuellt nätverk, behöver du toodo igen.</span><span class="sxs-lookup"><span data-stu-id="44666-156">If you are adding a new app tooanother virtual network, you'll need toodo this again.</span></span> <span data-ttu-id="44666-157">hello anledningen är att en uppsättning certifikat som genereras på en prenumerationsnivå i Azure App Service och hello set skapas en gång för varje virtuellt nätverk som hello appar ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="44666-157">hello reason for this is that a set of certificates is generated at a subscription level in Azure App Service, and hello set is generated once for each virtual network that hello apps will connect to.</span></span>

<span data-ttu-id="44666-158">hello certifikat kommer har redan ställts in om du har följt de här stegen eller om du integrerat med hello samma virtuella nätverk med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="44666-158">hello certificates will have already been set if you followed these steps or if you integrated with hello same virtual network by using hello portal.</span></span>

<span data-ttu-id="44666-159">hello första steget är toogenerate hello .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="44666-159">hello first step is toogenerate hello .cer file.</span></span> <span data-ttu-id="44666-160">hello andra steget är tooupload hello .cer-fil tooyour virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-160">hello second step is tooupload hello .cer file tooyour virtual network.</span></span> <span data-ttu-id="44666-161">toogenerate hello .cer-fil från hello API-anrop i hello tidigare steg, genom att köra följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="44666-161">toogenerate hello .cer file from hello API call in hello earlier step, run hello following commands.</span></span>

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

<span data-ttu-id="44666-162">hello certifikat kommer att hittas hello plats som **$Configuration.GeneratedCertificatePath** anger.</span><span class="sxs-lookup"><span data-stu-id="44666-162">hello certificate will be found in hello location that **$Configuration.GeneratedCertificatePath** specifies.</span></span>

<span data-ttu-id="44666-163">tooupload hello certifikatet manuellt, använda hello [Azure-portalen] [ azureportal] och **Bläddra virtuella nätverk (klassiska)** > **-VPN-anslutningar**  >  **Punkt-till-plats** > **hantera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="44666-163">tooupload hello certificate manually, use hello [Azure portal][azureportal] and **Browse Virtual Network (classic)** > **VPN connections** > **Point-to-site** > **Manage certificates**.</span></span> <span data-ttu-id="44666-164">Härifrån kan du överföra ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="44666-164">From here, upload your certificate.</span></span>

##### <a name="get-hello-point-to-site-package"></a><span data-ttu-id="44666-165">Hämta hello punkt-till-plats-paket</span><span class="sxs-lookup"><span data-stu-id="44666-165">Get hello point-to-site package</span></span>
<span data-ttu-id="44666-166">hello nästa steg i hur du konfigurerar en virtuell nätverksanslutning på ett webbprogram är tooget hello punkt-till-plats-paket och tillhandahålla tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="44666-166">hello next step in setting up a virtual network connection on a web app is tooget hello point-to-site package and provide it tooyour web app.</span></span>

<span data-ttu-id="44666-167">Spara hello följande mallfil tooa anropas GetNetworkPackageUri.json någonstans på datorn, till exempel C:\Azure\Templates\GetNetworkPackageUri.json.</span><span class="sxs-lookup"><span data-stu-id="44666-167">Save hello following template tooa file called GetNetworkPackageUri.json somewhere on your computer, for example, C:\Azure\Templates\GetNetworkPackageUri.json.</span></span>

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


<span data-ttu-id="44666-168">Ange indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="44666-168">Set input parameters:</span></span>

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

<span data-ttu-id="44666-169">Anropa hello skript:</span><span class="sxs-lookup"><span data-stu-id="44666-169">Call hello script:</span></span>

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


<span data-ttu-id="44666-170">hello variabeln **$output. Outputs.packageUri** innehåller nu hello paketet URI toobe angivna tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="44666-170">hello variable **$output.Outputs.packageUri** will now contain hello package URI toobe given tooyour web app.</span></span>

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a><span data-ttu-id="44666-171">Överföra hello punkt-till-plats-paketet tooyour app</span><span class="sxs-lookup"><span data-stu-id="44666-171">Upload hello point-to-site package tooyour app</span></span>
<span data-ttu-id="44666-172">hello sista steget är tooprovide hello app med det här paketet.</span><span class="sxs-lookup"><span data-stu-id="44666-172">hello final step is tooprovide hello app with this package.</span></span> <span data-ttu-id="44666-173">Kör bara hello nästa kommando:</span><span class="sxs-lookup"><span data-stu-id="44666-173">Simply run hello next command:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

<span data-ttu-id="44666-174">Om du ombeds att du skriver över en befintlig resurs tooconfirm, kontrollerar du att tooallow den.</span><span class="sxs-lookup"><span data-stu-id="44666-174">If a message asks you tooconfirm that you are overwriting an existing resource, make sure tooallow it.</span></span>

<span data-ttu-id="44666-175">När det här kommandot lyckas, bör din app nu anslutna toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-175">After this command succeeds, your app should now be connected toohello virtual network.</span></span> <span data-ttu-id="44666-176">tooconfirm lyckades, gå tooyour app konsol och Skriv hello följande:</span><span class="sxs-lookup"><span data-stu-id="44666-176">tooconfirm success, go tooyour app console, and type hello following:</span></span>

    SET WEBSITE_

<span data-ttu-id="44666-177">Om det finns en miljövariabel som kallas WEBSITE_VNETNAME som har ett värde som matchar hello namnet på virtuella hello målnätverket, har alla konfigurationer lyckades.</span><span class="sxs-lookup"><span data-stu-id="44666-177">If there is an environment variable called WEBSITE_VNETNAME that has a value that matches hello name of hello target virtual network, all configurations have succeeded.</span></span>

### <a name="update-classic-vnet-integration-information"></a><span data-ttu-id="44666-178">Uppdatera information om klassiska VNet-integrering</span><span class="sxs-lookup"><span data-stu-id="44666-178">Update classic VNet integration information</span></span>
<span data-ttu-id="44666-179">tooupdate eller synkronisera din information, helt enkelt Upprepa hello steg som du har följt när du skapade hello integrering i hello första plats.</span><span class="sxs-lookup"><span data-stu-id="44666-179">tooupdate or resync your information, simply repeat hello steps that you followed when you created hello integration in hello first place.</span></span> <span data-ttu-id="44666-180">De här stegen är:</span><span class="sxs-lookup"><span data-stu-id="44666-180">Those steps are:</span></span>

1. <span data-ttu-id="44666-181">Definiera din konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="44666-181">Define your configuration information.</span></span>
2. <span data-ttu-id="44666-182">Deklarera hello virtuellt nätverk toohello app.</span><span class="sxs-lookup"><span data-stu-id="44666-182">Declare hello virtual network toohello app.</span></span>
3. <span data-ttu-id="44666-183">Hämta hello punkt-till-plats-paket.</span><span class="sxs-lookup"><span data-stu-id="44666-183">Get hello point-to-site package.</span></span>
4. <span data-ttu-id="44666-184">Överföra hello punkt-till-plats-paketet tooyour app.</span><span class="sxs-lookup"><span data-stu-id="44666-184">Upload hello point-to-site package tooyour app.</span></span>

### <a name="disconnect-your-app-from-a-classic-vnet"></a><span data-ttu-id="44666-185">Koppla appen från ett klassiskt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="44666-185">Disconnect your app from a classic VNet</span></span>
<span data-ttu-id="44666-186">toodisconnect hello app måste hello konfigurationsinformation som angavs under virtuell nätverksintegration.</span><span class="sxs-lookup"><span data-stu-id="44666-186">toodisconnect hello app, you need hello configuration information that was set during virtual network integration.</span></span> <span data-ttu-id="44666-187">Använda informationen finns sedan ett kommando toodisconnect appen från det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="44666-187">Using that information, there is then one command toodisconnect your app from your virtual network.</span></span>

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a><span data-ttu-id="44666-188">Hanteraren för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="44666-188">Resource Manager virtual networks</span></span>
<span data-ttu-id="44666-189">Hanteraren för virtuella nätverk har Azure Resource Manager API: er, vilket förenklar vissa processer jämfört med klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-189">Resource Manager virtual networks have Azure Resource Manager APIs, which simplify some processes when compared with classic virtual networks.</span></span> <span data-ttu-id="44666-190">Vi har ett skript som hjälper dig att slutföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="44666-190">We have a script that will help you complete hello following tasks:</span></span>

* <span data-ttu-id="44666-191">Skapa ett virtuellt nätverk för hanteraren för filserverresurser och integrera din app med den.</span><span class="sxs-lookup"><span data-stu-id="44666-191">Create a Resource Manager virtual network and integrate your app with it.</span></span>
* <span data-ttu-id="44666-192">Skapa en gateway, konfigurera punkt-till-plats-anslutning i ett befintligt virtuellt nätverk för Resource Manager och sedan integrera din app med den.</span><span class="sxs-lookup"><span data-stu-id="44666-192">Create a gateway, configure point-to-site connectivity in a preexisting Resource Manager virtual network, and then integrate your app with it.</span></span>
* <span data-ttu-id="44666-193">Integrera din app med en befintlig Resource Manager virtuella nätverk som har en gateway och anslutning aktiverad punkt-till-plats.</span><span class="sxs-lookup"><span data-stu-id="44666-193">Integrate your app with a preexisting Resource Manager virtual network that has a gateway and point-to-site connectivity enabled.</span></span>
* <span data-ttu-id="44666-194">Koppla från din app från det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="44666-194">Disconnect your app from your virtual network.</span></span>

### <a name="resource-manager-vnet-app-service-integration-script"></a><span data-ttu-id="44666-195">Integrationsskriptet i hanteraren för filserverresurser VNet App Service</span><span class="sxs-lookup"><span data-stu-id="44666-195">Resource Manager VNet App Service integration script</span></span>
<span data-ttu-id="44666-196">Kopiera hello följande skript och spara den tooa filen.</span><span class="sxs-lookup"><span data-stu-id="44666-196">Copy hello following script and save it tooa file.</span></span> <span data-ttu-id="44666-197">Om du inte vill toouse hello skript känna sig fria toolearn från den toosee hur tooset saker upp med ett virtuellt nätverk för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="44666-197">If you don’t want toouse hello script, feel free toolearn from it toosee how tooset things up with a Resource Manager virtual network.</span></span>

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

<span data-ttu-id="44666-198">Spara en kopia av hello skript.</span><span class="sxs-lookup"><span data-stu-id="44666-198">Save a copy of hello script.</span></span> <span data-ttu-id="44666-199">Den kallas V2VnetAllinOne.ps1 i den här artikeln, men du kan använda ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="44666-199">In this article, it is called V2VnetAllinOne.ps1, but you can use another name.</span></span> <span data-ttu-id="44666-200">Det finns inga argument för det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="44666-200">There are no arguments for this script.</span></span> <span data-ttu-id="44666-201">Kör bara den.</span><span class="sxs-lookup"><span data-stu-id="44666-201">You simply run it.</span></span> <span data-ttu-id="44666-202">hello först öppna hello skriptet gör är att be dig toosign i.</span><span class="sxs-lookup"><span data-stu-id="44666-202">hello first thing hello script will do is prompt you toosign in.</span></span> <span data-ttu-id="44666-203">När du loggar in hello skript hämtar information om ditt konto och returnerar en lista över prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="44666-203">After you sign in, hello script gets details about your account and returns a list of subscriptions.</span></span> <span data-ttu-id="44666-204">Räknas hello begäran om autentiseringsuppgifterna inte hello inledande skriptkörningen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="44666-204">Not counting hello request for your credentials, hello initial script execution looks like this:</span></span>

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) <span data-ttu-id="44666-205">Demo-prenumeration (af5358e1-acac-2c90-a9eb-722190abf47a)</span><span class="sxs-lookup"><span data-stu-id="44666-205">Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)</span></span>
    2) <span data-ttu-id="44666-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span><span class="sxs-lookup"><span data-stu-id="44666-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span></span>
    3) <span data-ttu-id="44666-207">Lila Demo prenumeration (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span><span class="sxs-lookup"><span data-stu-id="44666-207">Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span></span>

    <span data-ttu-id="44666-208">Välj ett alternativ: 3</span><span class="sxs-lookup"><span data-stu-id="44666-208">Choose an option: 3</span></span>

    <span data-ttu-id="44666-209">Konto: ccompy@microsoft.com miljö: AzureCloud prenumerationen: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klient: 722278f-fef1-499f-91ab-2323d011db47</span><span class="sxs-lookup"><span data-stu-id="44666-209">Account      : ccompy@microsoft.com Environment  : AzureCloud Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant       : 722278f-fef1-499f-91ab-2323d011db47</span></span>

    <span data-ttu-id="44666-210">Ange hello resursgruppen för din App: hcdemo rg genom att ange hello namnet på din App: v2vnetpowershell vad vill du vill toodo?</span><span class="sxs-lookup"><span data-stu-id="44666-210">Please enter hello Resource Group of your App: hcdemo-rg Please enter hello Name of your App: v2vnetpowershell What do you want toodo?</span></span>

    1) <span data-ttu-id="44666-211">Lägg till ett nytt virtuellt nätverk tooan App</span><span class="sxs-lookup"><span data-stu-id="44666-211">Add a NEW Virtual Network tooan App</span></span>
    2) <span data-ttu-id="44666-212">Lägg till ett befintligt virtuellt nätverk tooan App</span><span class="sxs-lookup"><span data-stu-id="44666-212">Add an EXISTING Virtual Network tooan App</span></span>
    3) <span data-ttu-id="44666-213">Ta bort ett virtuellt nätverk från en App</span><span class="sxs-lookup"><span data-stu-id="44666-213">Remove a Virtual Network from an App</span></span>

<span data-ttu-id="44666-214">hello resten av det här avsnittet beskrivs var och en av de tre alternativen.</span><span class="sxs-lookup"><span data-stu-id="44666-214">hello rest of this section explains each of those three options.</span></span>

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a><span data-ttu-id="44666-215">Skapa ett VNet med Resource Manager och integrera med den</span><span class="sxs-lookup"><span data-stu-id="44666-215">Create a Resource Manager VNet and integrate with it</span></span>
<span data-ttu-id="44666-216">toocreate ett nytt virtuellt nätverk som använder hello Resource Manager-distributionsmodellen och integrera den med din app, Välj **1) Lägg till ett nytt virtuellt nätverk tooan App**.</span><span class="sxs-lookup"><span data-stu-id="44666-216">toocreate a new virtual network that uses hello Resource Manager deployment model and integrate it with your app, select **1) Add a NEW Virtual Network tooan App**.</span></span> <span data-ttu-id="44666-217">Det här uppmanas du att hello namnet på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-217">This will prompt you for hello name of hello virtual network.</span></span> <span data-ttu-id="44666-218">I min fall som du ser i följande inställningar hello, använde jag hello namn, v2pshell.</span><span class="sxs-lookup"><span data-stu-id="44666-218">In my case, as you can see in hello following settings, I used hello name, v2pshell.</span></span>

<span data-ttu-id="44666-219">hello skriptet ger hello information om hello virtuella nätverk som håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="44666-219">hello script gives hello details about hello virtual network that's being created.</span></span> <span data-ttu-id="44666-220">Om användarna vill ändra jag hello värden.</span><span class="sxs-lookup"><span data-stu-id="44666-220">If I want, I can change any of hello values.</span></span> <span data-ttu-id="44666-221">Jag har skapat ett virtuellt nätverk som har hello följande inställningar i det här exemplet körning:</span><span class="sxs-lookup"><span data-stu-id="44666-221">In this example execution, I created a virtual network that has hello following settings:</span></span>

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

<span data-ttu-id="44666-222">Om du vill toochange hello värden, skriver **Y** hello ändrar.</span><span class="sxs-lookup"><span data-stu-id="44666-222">If you want toochange any of hello values, type **Y** and make hello changes.</span></span> <span data-ttu-id="44666-223">När du är nöjd med hello virtuella nätverksinställningar skriver **N** eller helt enkelt tryck på RETUR när du tillfrågas om hur du ändrar hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="44666-223">When you are happy with hello virtual network settings, type **N** or simply press Enter when prompted about changing hello settings.</span></span> <span data-ttu-id="44666-224">Därifrån på tills den har slutförts hello skript kommer att meddela dig några av IT-avdelningen ”bindningsikonerna göra förrän den startar toocreate hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="44666-224">From there on until completion, hello script will tell you some of what it' i's doing until it starts toocreate hello virtual network gateway.</span></span> <span data-ttu-id="44666-225">Steget kan ta upp tooan timme.</span><span class="sxs-lookup"><span data-stu-id="44666-225">That step can take up tooan hour.</span></span> <span data-ttu-id="44666-226">Det finns ingen förloppsindikator under den här fasen men hello skriptet att meddela när hello gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="44666-226">There is no progress indicator during this phase, but hello script will let you know when hello gateway has been created.</span></span>

<span data-ttu-id="44666-227">När hello skriptet är klar ska det stå **avslutad**.</span><span class="sxs-lookup"><span data-stu-id="44666-227">When hello script finishes, it will say **Finished**.</span></span> <span data-ttu-id="44666-228">Nu har du ett virtuellt nätverk för Resource Manager som har hello namn och inställningarna som du valt.</span><span class="sxs-lookup"><span data-stu-id="44666-228">At this point, you will have a Resource Manager virtual network that has hello name and settings that you selected.</span></span> <span data-ttu-id="44666-229">Den här nytt virtuellt nätverk ska också integreras med din app.</span><span class="sxs-lookup"><span data-stu-id="44666-229">This new virtual network will also be integrated with your app.</span></span>

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a><span data-ttu-id="44666-230">Integrera din app med en befintlig Resource Manager-VNet</span><span class="sxs-lookup"><span data-stu-id="44666-230">Integrate your app with a preexisting Resource Manager VNet</span></span>
<span data-ttu-id="44666-231">När du integrerar med ett befintligt virtuellt nätverk om du anger ett virtuellt nätverk för Resource Manager som inte har en gateway eller en punkt-till-plats-anslutning kan skapar hello skriptet som.</span><span class="sxs-lookup"><span data-stu-id="44666-231">When you're integrating with a preexisting virtual network, if you provide a Resource Manager virtual network that doesn’t have a gateway or point-to-site connectivity, hello script will set that up.</span></span> <span data-ttu-id="44666-232">Om hello VNET har redan sakerna ställa in, går hello skript raka toohello appintegrering.</span><span class="sxs-lookup"><span data-stu-id="44666-232">If hello VNET already has those things set up, hello script goes straight toohello app integration.</span></span> <span data-ttu-id="44666-233">toostart den här processen kan bara markera **2) Lägg till ett befintligt virtuellt nätverk tooan App**.</span><span class="sxs-lookup"><span data-stu-id="44666-233">toostart this process, simply select **2) Add an EXISTING Virtual Network tooan App**.</span></span>

<span data-ttu-id="44666-234">Det här alternativet fungerar bara om du har en befintlig Resource Manager virtuellt nätverk som är i hello samma prenumeration som din app.</span><span class="sxs-lookup"><span data-stu-id="44666-234">This option works only if you have a preexisting Resource Manager virtual network that is in hello same subscription as your app.</span></span> <span data-ttu-id="44666-235">När du har valt alternativet hello visas en lista över dina virtuella Resource Manager-nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-235">After you select hello option, you will be presented with a list of your Resource Manager virtual networks.</span></span>   

    Select a VNET toointegrate with

    1) <span data-ttu-id="44666-236">v2demonetwork</span><span class="sxs-lookup"><span data-stu-id="44666-236">v2demonetwork</span></span>
    2) <span data-ttu-id="44666-237">v2pshell</span><span class="sxs-lookup"><span data-stu-id="44666-237">v2pshell</span></span>
    3) <span data-ttu-id="44666-238">v2vnetintdemo</span><span class="sxs-lookup"><span data-stu-id="44666-238">v2vnetintdemo</span></span>
    4) <span data-ttu-id="44666-239">v2asenetwork</span><span class="sxs-lookup"><span data-stu-id="44666-239">v2asenetwork</span></span>
    5) <span data-ttu-id="44666-240">v2pshell2</span><span class="sxs-lookup"><span data-stu-id="44666-240">v2pshell2</span></span>

    <span data-ttu-id="44666-241">Välj ett alternativ: 5</span><span class="sxs-lookup"><span data-stu-id="44666-241">Choose an option: 5</span></span>

<span data-ttu-id="44666-242">Markera bara hello virtuella nätverk som du vill toointegrate med.</span><span class="sxs-lookup"><span data-stu-id="44666-242">Simply select hello virtual network that you want toointegrate with.</span></span> <span data-ttu-id="44666-243">Om du redan har en gateway med punkt-till-plats-anslutning aktiverad hello skript helt enkelt att integrera din app med ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-243">If you already have a gateway that has point-to-site connectivity enabled, hello script will simply integrate your app with your virtual network.</span></span> <span data-ttu-id="44666-244">Om du inte har en gateway måste toospecify hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="44666-244">If you do not have a gateway, you will need toospecify hello gateway subnet.</span></span> <span data-ttu-id="44666-245">Gateway-undernätet måste vara i ditt virtuella nätverk och det får inte finnas i andra undernät.</span><span class="sxs-lookup"><span data-stu-id="44666-245">Your gateway subnet must be in your virtual network address space, and it cannot be in any other subnet.</span></span> <span data-ttu-id="44666-246">Om du har ett virtuellt nätverk utan en gateway och kör det här steget saker se ut så här:</span><span class="sxs-lookup"><span data-stu-id="44666-246">If you have a virtual network without a gateway and run this step, things look like this:</span></span>

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

<span data-ttu-id="44666-247">Jag har skapat en virtuell nätverksgateway som har hello följande inställningar i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="44666-247">In this example, I created a virtual network gateway that has hello following settings:</span></span>

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

<span data-ttu-id="44666-248">Om du vill toochange någon av dessa inställningar kan göra du detta.</span><span class="sxs-lookup"><span data-stu-id="44666-248">If you want toochange any of those settings, you can do so.</span></span> <span data-ttu-id="44666-249">I annat fall trycker på RETUR och hello skriptet skapar din gateway och ansluta din app tooyour virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-249">Otherwise, press Enter and hello script will create your gateway and attach your app tooyour virtual network.</span></span> <span data-ttu-id="44666-250">hello gateway Skapandetid är dock fortfarande en timma, så se till att du komma ihåg.</span><span class="sxs-lookup"><span data-stu-id="44666-250">hello gateway creation time is still an hour, though, so make sure you keep that in mind.</span></span> <span data-ttu-id="44666-251">När allt är klart hello skript står **avslutad**.</span><span class="sxs-lookup"><span data-stu-id="44666-251">When everything is finished, hello script will say **Finished**.</span></span>

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a><span data-ttu-id="44666-252">Koppla från din app från en Resource Manager-VNet</span><span class="sxs-lookup"><span data-stu-id="44666-252">Disconnect your app from a Resource Manager VNet</span></span>
<span data-ttu-id="44666-253">Koppla appen från det virtuella nätverket inte ta bort hello gateway eller inaktivera punkt-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="44666-253">Disconnecting your app from your virtual network does not take down hello gateway or disable point-to-site connectivity.</span></span> <span data-ttu-id="44666-254">Du kanske, när alla använder det något annat.</span><span class="sxs-lookup"><span data-stu-id="44666-254">You might, after all, be using it for something else.</span></span> <span data-ttu-id="44666-255">Den också kopplar inte den från andra appar än hello något du har angett.</span><span class="sxs-lookup"><span data-stu-id="44666-255">It also does not disconnect it from any other apps other than hello one you provided.</span></span> <span data-ttu-id="44666-256">tooperform åtgärden väljer **3) ta bort ett virtuellt nätverk från en App**.</span><span class="sxs-lookup"><span data-stu-id="44666-256">tooperform this action, select **3) Remove a Virtual Network from an App**.</span></span> <span data-ttu-id="44666-257">När du gör det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="44666-257">When you do so, you will see something like this:</span></span>

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

<span data-ttu-id="44666-258">Även om hello skript står ta bort, tas inte bort hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="44666-258">Although hello script says delete, it does not delete hello virtual network.</span></span> <span data-ttu-id="44666-259">Det bara att ta bort hello-integration.</span><span class="sxs-lookup"><span data-stu-id="44666-259">It’s just removing hello integration.</span></span> <span data-ttu-id="44666-260">När du har bekräftat att det här är vad du vill toodo, hello kommandot bearbetas ganska snabbt och anger **SANT** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="44666-260">After you confirm that this is what you want toodo, hello command is processed quite quickly and tells you **True** when it is done.</span></span>

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
