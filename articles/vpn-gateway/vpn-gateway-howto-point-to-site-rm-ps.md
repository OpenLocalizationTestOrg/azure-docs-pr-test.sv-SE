---
title: "Ansluta en dator tooan Azure virtuellt nätverk med punkt-till-plats och certifikatautentisering: PowerShell | Microsoft Docs"
description: "På ett säkert sätt ansluta ett virtuellt nätverk för dator-tooyour genom att skapa en punkt-till-plats VPN-gateway-anslutningen genom att använda certifikat. Den här artikeln gäller toohello Resource Manager-modellen och använder PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a><span data-ttu-id="91750-104">Konfigurera en punkt-till-plats-anslutning tooa VNet med hjälp av autentisering med datorcertifikat: PowerShell</span><span class="sxs-lookup"><span data-stu-id="91750-104">Configure a Point-to-Site connection tooa VNet using certificate authentication: PowerShell</span></span>

<span data-ttu-id="91750-105">Den här artikeln visar hur toocreate ett VNet med en punkt-till-plats-anslutning i hello Resource Manager distribution modellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91750-105">This article shows you how toocreate a VNet with a Point-to-Site connection in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="91750-106">Den här konfigurationen använder certifikat tooauthenticate hello ansluter klienten.</span><span class="sxs-lookup"><span data-stu-id="91750-106">This configuration uses certificates tooauthenticate hello connecting client.</span></span> <span data-ttu-id="91750-107">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="91750-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="91750-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="91750-108">Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="91750-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="91750-109">PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="91750-110">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="91750-110">Azure portal (classic)</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

<span data-ttu-id="91750-111">En punkt-till-plats (P2S) VPN-gateway kan du skapa en säker anslutning tooyour virtuellt nätverk från en enskild klientdator.</span><span class="sxs-lookup"><span data-stu-id="91750-111">A Point-to-Site (P2S) VPN gateway lets you create a secure connection tooyour virtual network from an individual client computer.</span></span> <span data-ttu-id="91750-112">Punkt-till-plats VPN-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en annan plats, exempelvis när du hemifrån home eller en konferens.</span><span class="sxs-lookup"><span data-stu-id="91750-112">Point-to-Site VPN connections are useful when you want tooconnect tooyour VNet from a remote location, such when you are telecommuting from home or a conference.</span></span> <span data-ttu-id="91750-113">P2S VPN är också en bra lösning toouse i stället för en plats-till-plats VPN-anslutning när du har bara ett fåtal klienter som behöver tooconnect tooa VNet.</span><span class="sxs-lookup"><span data-stu-id="91750-113">A P2S VPN is also a useful solution toouse instead of a Site-to-Site VPN when you have only a few clients that need tooconnect tooa VNet.</span></span>

<span data-ttu-id="91750-114">P2S använder hello Secure Socket Tunneling Protocol (SSTP), vilket är ett SSL-baserad VPN-protokoll.</span><span class="sxs-lookup"><span data-stu-id="91750-114">P2S uses hello Secure Socket Tunneling Protocol (SSTP), which is an SSL-based VPN protocol.</span></span> <span data-ttu-id="91750-115">En P2S VPN-anslutning upprättas genom att starta från hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="91750-115">A P2S VPN connection is established by starting it from hello client computer.</span></span>

![Ansluta en dator tooan Azure VNet - punkt-till-plats-anslutning diagram](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

<span data-ttu-id="91750-117">Punkt-till-plats certifikat autentisering anslutningar kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="91750-117">Point-to-Site certificate authentication connections require hello following:</span></span>

* <span data-ttu-id="91750-118">En RouteBased VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="91750-118">A RouteBased VPN gateway.</span></span>
* <span data-ttu-id="91750-119">hello offentliga nyckel (.cer-fil) för ett rotcertifikat som är överförda tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91750-119">hello public key (.cer file) for a root certificate, which is uploaded tooAzure.</span></span> <span data-ttu-id="91750-120">När hello certifikat har överförts betraktas som ett betrott certifikat och används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="91750-120">Once hello certificate is uploaded, it is considered a trusted certificate and is used for authentication.</span></span>
* <span data-ttu-id="91750-121">Ett klientcertifikat som genereras från hello rotcertifikat och installeras på varje klientdator som ansluter toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="91750-121">A client certificate that is generated from hello root certificate and installed on each client computer that will connect toohello VNet.</span></span> <span data-ttu-id="91750-122">Det här certifikatet används för klientautentisering.</span><span class="sxs-lookup"><span data-stu-id="91750-122">This certificate is used for client authentication.</span></span>
* <span data-ttu-id="91750-123">Ett konfigurationspaket för VPN-klienten.</span><span class="sxs-lookup"><span data-stu-id="91750-123">A VPN client configuration package.</span></span> <span data-ttu-id="91750-124">hello VPN-klientpaketet konfiguration innehåller hello nödvändig information för hello klienten tooconnect toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="91750-124">hello VPN client configuration package contains hello necessary information for hello client tooconnect toohello VNet.</span></span> <span data-ttu-id="91750-125">hello paketet konfigurerar hello befintliga VPN-klient som är inbyggda toohello Windows-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="91750-125">hello package configures hello existing VPN client that is native toohello Windows operating system.</span></span> <span data-ttu-id="91750-126">Alla klienter som ansluter måste konfigureras med hjälp av hello konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="91750-126">Each client that connects must be configured using hello configuration package.</span></span>

<span data-ttu-id="91750-127">Punkt-till-plats-anslutningar kräver inte någon VPN-enhet eller en lokal offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="91750-127">Point-to-Site connections do not require a VPN device or an on-premises public-facing IP address.</span></span> <span data-ttu-id="91750-128">hello VPN-anslutningen har skapats via SSTP (Secure Socket Tunneling Protocol).</span><span class="sxs-lookup"><span data-stu-id="91750-128">hello VPN connection is created over SSTP (Secure Socket Tunneling Protocol).</span></span> <span data-ttu-id="91750-129">Vi stöder SSTP version 1.0, 1.1 och 1.2 på hello serversidan.</span><span class="sxs-lookup"><span data-stu-id="91750-129">On hello server side, we support SSTP versions 1.0, 1.1, and 1.2.</span></span> <span data-ttu-id="91750-130">hello klienten avgör vilken version toouse.</span><span class="sxs-lookup"><span data-stu-id="91750-130">hello client decides which version toouse.</span></span> <span data-ttu-id="91750-131">För Windows 8.1 och senare, använder SSTP version 1.2 som standard.</span><span class="sxs-lookup"><span data-stu-id="91750-131">For Windows 8.1 and above, SSTP uses 1.2 by default.</span></span> 

<span data-ttu-id="91750-132">Mer information om anslutningar för punkt-till-plats finns hello [punkt-till-plats vanliga frågor och svar](#faq) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="91750-132">For more information about Point-to-Site connections, see hello [Point-to-Site FAQ](#faq) at hello end of this article.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="91750-133">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="91750-133">Before beginning</span></span>

* <span data-ttu-id="91750-134">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="91750-134">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="91750-135">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="91750-135">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="91750-136">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="91750-136">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="91750-137">Mer information om hur du installerar PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91750-137">For more information about installing PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="91750-138"><a name="example"></a>Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="91750-138"><a name="example"></a>Example values</span></span>

<span data-ttu-id="91750-139">Du kan använda hello exempel värden toocreate en testmiljö eller referera toothese värden toobetter förstå hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="91750-139">You can use hello example values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article.</span></span> <span data-ttu-id="91750-140">Vi ange hello variabler i avsnittet [1](#declare) av hello artikel.</span><span class="sxs-lookup"><span data-stu-id="91750-140">We set hello variables in section [1](#declare) of hello article.</span></span> <span data-ttu-id="91750-141">Du kan antingen använda hello steg som en genomgång och använda hello värden utan att ändra dem eller ändra dem tooreflect din miljö.</span><span class="sxs-lookup"><span data-stu-id="91750-141">You can either use hello steps as a walk-through and use hello values without changing them, or change them tooreflect your environment.</span></span> 

* <span data-ttu-id="91750-142">**Namn: VNet1**</span><span class="sxs-lookup"><span data-stu-id="91750-142">**Name: VNet1**</span></span>
* <span data-ttu-id="91750-143">**Adressutrymme: 192.168.0.0/16** and **10.254.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="91750-143">**Address space: 192.168.0.0/16** and **10.254.0.0/16**</span></span><br><span data-ttu-id="91750-144">Det här exemplet använder vi mer än en adressutrymme tooillustrate som den här konfigurationen fungerar med flera adressutrymmen.</span><span class="sxs-lookup"><span data-stu-id="91750-144">For this example, we use more than one address space tooillustrate that this configuration works with multiple address spaces.</span></span> <span data-ttu-id="91750-145">Flera adressutrymmen krävs dock inte för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="91750-145">However, multiple address spaces are not required for this configuration.</span></span>
* <span data-ttu-id="91750-146">**Undernätsnamn: FrontEnd**</span><span class="sxs-lookup"><span data-stu-id="91750-146">**Subnet name: FrontEnd**</span></span>
  * <span data-ttu-id="91750-147">**Adressintervall för undernätet: 192.168.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="91750-147">**Subnet address range: 192.168.1.0/24**</span></span>
* <span data-ttu-id="91750-148">**Undernätsnamn: BackEnd**</span><span class="sxs-lookup"><span data-stu-id="91750-148">**Subnet name: BackEnd**</span></span>
  * <span data-ttu-id="91750-149">**Adressintervall för undernätet: 10.254.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="91750-149">**Subnet address range: 10.254.1.0/24**</span></span>
* <span data-ttu-id="91750-150">**Undernätsnamn: GatewaySubnet**</span><span class="sxs-lookup"><span data-stu-id="91750-150">**Subnet name: GatewaySubnet**</span></span><br><span data-ttu-id="91750-151">Hej undernätsnamn *GatewaySubnet* är obligatoriskt för hello VPN-gateway toowork.</span><span class="sxs-lookup"><span data-stu-id="91750-151">hello Subnet name *GatewaySubnet* is mandatory for hello VPN gateway toowork.</span></span>
  * <span data-ttu-id="91750-152">**Adressintervall för gateway-undernätet: 192.168.200.0/24**</span><span class="sxs-lookup"><span data-stu-id="91750-152">**GatewaySubnet address range: 192.168.200.0/24**</span></span> 
* <span data-ttu-id="91750-153">**VPN-klientadresspool: 172.16.201.0/24**</span><span class="sxs-lookup"><span data-stu-id="91750-153">**VPN client address pool: 172.16.201.0/24**</span></span><br><span data-ttu-id="91750-154">VPN-klienter som ansluter toohello VNet med den här punkt-till-plats-anslutningen för att ta emot en IP-adress från hello VPN-klientadresspool.</span><span class="sxs-lookup"><span data-stu-id="91750-154">VPN clients that connect toohello VNet using this Point-to-Site connection receive an IP address from hello VPN client address pool.</span></span>
* <span data-ttu-id="91750-155">**Prenumerationen:** om du har mer än en prenumeration, kontrollera att du använder hello korrekt.</span><span class="sxs-lookup"><span data-stu-id="91750-155">**Subscription:** If you have more than one subscription, verify that you are using hello correct one.</span></span>
* <span data-ttu-id="91750-156">**Resursgrupp: TestRG**</span><span class="sxs-lookup"><span data-stu-id="91750-156">**Resource Group: TestRG**</span></span>
* <span data-ttu-id="91750-157">**Plats: Östra USA**</span><span class="sxs-lookup"><span data-stu-id="91750-157">**Location: East US**</span></span>
* <span data-ttu-id="91750-158">**DNS-Server: IP-adress** för hello DNS-server som du vill toouse för namnmatchning.</span><span class="sxs-lookup"><span data-stu-id="91750-158">**DNS Server: IP address** of hello DNS server that you want toouse for name resolution.</span></span>
* <span data-ttu-id="91750-159">**GW-namn: Vnet1GW**</span><span class="sxs-lookup"><span data-stu-id="91750-159">**GW Name: Vnet1GW**</span></span>
* <span data-ttu-id="91750-160">**Offentligt IP-namn: VNet1GWPIP**</span><span class="sxs-lookup"><span data-stu-id="91750-160">**Public IP name: VNet1GWPIP**</span></span>
* <span data-ttu-id="91750-161">**VPNType: RouteBased**</span><span class="sxs-lookup"><span data-stu-id="91750-161">**VpnType: RouteBased**</span></span> 

## <span data-ttu-id="91750-162"><a name="declare"></a>1. Logga in och ange variabler</span><span class="sxs-lookup"><span data-stu-id="91750-162"><a name="declare"></a>1. Log in and set variables</span></span>

<span data-ttu-id="91750-163">I det här avsnittet, logga in och deklarera hello-värden som används för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="91750-163">In this section, you log in and declare hello values used for this configuration.</span></span> <span data-ttu-id="91750-164">hello deklareras värden används i hello exempelskript.</span><span class="sxs-lookup"><span data-stu-id="91750-164">hello declared values are used in hello sample scripts.</span></span> <span data-ttu-id="91750-165">Ändra hello värden tooreflect din egen miljö.</span><span class="sxs-lookup"><span data-stu-id="91750-165">Change hello values tooreflect your own environment.</span></span> <span data-ttu-id="91750-166">Eller, du kan använda hello deklarerats värden och gå igenom hello steg som övning.</span><span class="sxs-lookup"><span data-stu-id="91750-166">Or, you can use hello declared values and go through hello steps as an exercise.</span></span>

1. <span data-ttu-id="91750-167">Öppna PowerShell-konsol med utökade privilegier och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="91750-167">Open your PowerShell console with elevated privileges, and log in tooyour Azure account.</span></span> <span data-ttu-id="91750-168">Denna cmdlet efterfrågar hello inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="91750-168">This cmdlet prompts you for hello login credentials.</span></span> <span data-ttu-id="91750-169">När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91750-169">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```
2. <span data-ttu-id="91750-170">Hämta en lista över dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="91750-170">Get a list of your Azure subscriptions.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
3. <span data-ttu-id="91750-171">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="91750-171">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. <span data-ttu-id="91750-172">Deklarera hello variabler som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="91750-172">Declare hello variables that you want toouse.</span></span> <span data-ttu-id="91750-173">Använd hello följande exempel, ersätter hello värden för din egen vid behov.</span><span class="sxs-lookup"><span data-stu-id="91750-173">Use hello following sample, substituting hello values for your own when necessary.</span></span>

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <span data-ttu-id="91750-174"><a name="ConfigureVNet"></a>2. Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="91750-174"><a name="ConfigureVNet"></a>2. Configure a VNet</span></span>

1. <span data-ttu-id="91750-175">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="91750-175">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. <span data-ttu-id="91750-176">Skapa hello undernät för virtuellt nätverk hello, namnge dem *klientdel*, *BackEnd*, och *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="91750-176">Create hello subnet configurations for hello virtual network, naming them *FrontEnd*, *BackEnd*, and *GatewaySubnet*.</span></span> <span data-ttu-id="91750-177">Dessa prefix måste vara en del av hello VNet-adressutrymmet som du har deklarerats.</span><span class="sxs-lookup"><span data-stu-id="91750-177">These prefixes must be part of hello VNet address space that you declared.</span></span>

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. <span data-ttu-id="91750-178">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="91750-178">Create hello virtual network.</span></span>

  <span data-ttu-id="91750-179">I det här exemplet är hello DNS-servern valfria.</span><span class="sxs-lookup"><span data-stu-id="91750-179">In this example, hello DNS server is optional.</span></span> <span data-ttu-id="91750-180">Ingen ny DNS-server skapas när du anger ett värde.</span><span class="sxs-lookup"><span data-stu-id="91750-180">Specifying a value does not create a new DNS server.</span></span> <span data-ttu-id="91750-181">hello ska DNS-serverns IP-adress som du anger vara en DNS-server som kan lösa hello namn för hello-resurser som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="91750-181">hello DNS server IP address that you specify should be a DNS server that can resolve hello names for hello resources you are connecting to.</span></span> <span data-ttu-id="91750-182">Vi använde en privat IP-adress för det här exemplet, men det är troligt att detta inte är hello IP-adressen för DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="91750-182">For this example, we used a private IP address, but it is likely that this is not hello IP address of your DNS server.</span></span> <span data-ttu-id="91750-183">Vara säker på att toouse egna värden.</span><span class="sxs-lookup"><span data-stu-id="91750-183">Be sure toouse your own values.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. <span data-ttu-id="91750-184">Ange hello variabler för hello virtuella nätverk som du skapade.</span><span class="sxs-lookup"><span data-stu-id="91750-184">Specify hello variables for hello virtual network you created.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="91750-185">En VPN-gateway måste ha en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="91750-185">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="91750-186">Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="91750-186">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="91750-187">hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="91750-187">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="91750-188">VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering.</span><span class="sxs-lookup"><span data-stu-id="91750-188">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="91750-189">Du kan inte begära en statisk offentlig IP-adresstilldelning.</span><span class="sxs-lookup"><span data-stu-id="91750-189">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="91750-190">Dock betyder det att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="91750-190">However, it doesn't mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="91750-191">hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="91750-191">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="91750-192">Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="91750-192">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

  <span data-ttu-id="91750-193">Begär en dynamiskt tilldelad offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="91750-193">Request a dynamically assigned public IP address.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <span data-ttu-id="91750-194"><a name="creategateway"></a>3. Skapa hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="91750-194"><a name="creategateway"></a>3. Create hello VPN gateway</span></span>

<span data-ttu-id="91750-195">Konfigurera och skapa hello virtuell nätverksgateway för din VNet.</span><span class="sxs-lookup"><span data-stu-id="91750-195">Configure and create hello virtual network gateway for your VNet.</span></span>

* <span data-ttu-id="91750-196">Hej *- GatewayType* måste vara **Vpn** och hello *- VpnType* måste vara **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="91750-196">hello *-GatewayType* must be **Vpn** and hello *-VpnType* must be **RouteBased**.</span></span>
* <span data-ttu-id="91750-197">En VPN-gateway kan ta upp too45 minuter toocomplete, beroende på hello [gateway-sku](vpn-gateway-about-vpn-gateway-settings.md) du väljer.</span><span class="sxs-lookup"><span data-stu-id="91750-197">A VPN gateway can take up too45 minutes toocomplete, depending on hello [gateway sku](vpn-gateway-about-vpn-gateway-settings.md) you select.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <span data-ttu-id="91750-198"><a name="addresspool"></a>4. Lägg till hello VPN-klientadresspool</span><span class="sxs-lookup"><span data-stu-id="91750-198"><a name="addresspool"></a>4. Add hello VPN client address pool</span></span>

<span data-ttu-id="91750-199">Du kan lägga till hello VPN-klientadresspool när hello VPN-gateway är klar att skapa.</span><span class="sxs-lookup"><span data-stu-id="91750-199">After hello VPN gateway finishes creating, you can add hello VPN client address pool.</span></span> <span data-ttu-id="91750-200">hello VPN-klientadresspool är hello-intervall som hello VPN-klienter tar emot en IP-adress vid anslutning.</span><span class="sxs-lookup"><span data-stu-id="91750-200">hello VPN client address pool is hello range from which hello VPN clients receive an IP address when connecting.</span></span> <span data-ttu-id="91750-201">Använd en privat IP-adressintervall som inte överlappar hello lokal plats som du ansluter från eller med hello virtuella nätverk som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="91750-201">Use a private IP address range that does not overlap with hello on-premises location that you connect from, or with hello VNet that you want tooconnect to.</span></span> <span data-ttu-id="91750-202">I det här exemplet hello VPN-klientadresspool har deklarerats som en [variabeln](#declare) i steg 1.</span><span class="sxs-lookup"><span data-stu-id="91750-202">In this example, hello VPN client address pool is declared as a [variable](#declare) in Step 1.</span></span>

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <span data-ttu-id="91750-203"><a name="Certificates"></a>5. Generera certifikat</span><span class="sxs-lookup"><span data-stu-id="91750-203"><a name="Certificates"></a>5. Generate certificates</span></span>

<span data-ttu-id="91750-204">Certifikat som används av Azure tooauthenticate VPN-klienter för plats-till-plats-VPN.</span><span class="sxs-lookup"><span data-stu-id="91750-204">Certificates are used by Azure tooauthenticate VPN clients for Point-to-Site VPNs.</span></span> <span data-ttu-id="91750-205">Du kan överföra hello offentlig nyckelinformation för hello root certificate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91750-205">You upload hello public key information of hello root certificate tooAzure.</span></span> <span data-ttu-id="91750-206">'betrodda' anses hello offentliga nyckel.</span><span class="sxs-lookup"><span data-stu-id="91750-206">hello public key is then considered 'trusted'.</span></span> <span data-ttu-id="91750-207">Klientcertifikat måste genereras från hello betrodda rotcertifikat och sedan installeras på varje dator i hello certifikat-aktuell användare/personliga certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="91750-207">Client certificates must be generated from hello trusted root certificate, and then installed on each client computer in hello Certificates-Current User/Personal certificate store.</span></span> <span data-ttu-id="91750-208">hello certifikat är används tooauthenticate hello klient när den upprättar en anslutning toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="91750-208">hello certificate is used tooauthenticate hello client when it initiates a connection toohello VNet.</span></span> 

<span data-ttu-id="91750-209">Om du använder självsignerade certifikat, måste de skapas med specifika parametrar.</span><span class="sxs-lookup"><span data-stu-id="91750-209">If you use self-signed certificates, they must be created using specific parameters.</span></span> <span data-ttu-id="91750-210">Du kan skapa ett självsignerat certifikat med hjälp av hello instruktioner för [PowerShell och Windows 10](vpn-gateway-certificates-point-to-site.md), eller om du inte har Windows 10 kan du använda [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span><span class="sxs-lookup"><span data-stu-id="91750-210">You can create a self-signed certificate using hello instructions for [PowerShell and Windows 10](vpn-gateway-certificates-point-to-site.md), or, if you don't have Windows 10, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span></span> <span data-ttu-id="91750-211">Det är viktigt att du följer hello stegen i hello instruktioner vid generering av självsignerade rotcertifikat och klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-211">It's important that you follow hello steps in hello instructions when generating self-signed root certificates and client certificates.</span></span> <span data-ttu-id="91750-212">Annars hello-certifikat som du skapar är inte kompatibel med P2S-anslutningar och du får ett anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="91750-212">Otherwise, hello certificates you generate will not be compatible with P2S connections and you will receive a connection error.</span></span>

### <span data-ttu-id="91750-213"><a name="cer"></a>1. Hämta hello .cer-fil för hello rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-213"><a name="cer"></a>1. Obtain hello .cer file for hello root certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <span data-ttu-id="91750-214"><a name="generate"></a>2. Generera ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-214"><a name="generate"></a>2. Generate a client certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <span data-ttu-id="91750-215"><a name="upload"></a>6. Överför hello root certificate offentlig nyckelinformation</span><span class="sxs-lookup"><span data-stu-id="91750-215"><a name="upload"></a>6. Upload hello root certificate public key information</span></span>

<span data-ttu-id="91750-216">Kontrollera att din VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="91750-216">Verify that your VPN gateway has finished creating.</span></span> <span data-ttu-id="91750-217">Du kan överföra hello .cer-filen (som innehåller information om hello offentliga nycklar) för en betrodd rot certifikat tooAzure när den är klar.</span><span class="sxs-lookup"><span data-stu-id="91750-217">Once it has completed, you can upload hello .cer file (which contains hello public key information) for a trusted root certificate tooAzure.</span></span> <span data-ttu-id="91750-218">När a.cer filen har överförts kan Azure använda den tooauthenticate klienter som har installerat ett klientcertifikat som genereras från hello betrott rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-218">Once a.cer file is uploaded, Azure can use it tooauthenticate clients that have installed a client certificate generated from hello trusted root certificate.</span></span> <span data-ttu-id="91750-219">Du kan överföra ytterligare betrodda rotcertifikat certifikatfiler - upp tooa totalt 20 – senare, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="91750-219">You can upload additional trusted root certificate files - up tooa total of 20 - later, if needed.</span></span>

1. <span data-ttu-id="91750-220">Deklarera hello variabel för din certifikatnamn ersätta hello värdet med din egen.</span><span class="sxs-lookup"><span data-stu-id="91750-220">Declare hello variable for your certificate name, replacing hello value with your own.</span></span>

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. <span data-ttu-id="91750-221">Ersätt hello sökväg med din egen och kör sedan hello-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="91750-221">Replace hello file path with your own, and then run hello cmdlets.</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. <span data-ttu-id="91750-222">Överför hello offentlig nyckelinformation tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91750-222">Upload hello public key information tooAzure.</span></span> <span data-ttu-id="91750-223">När hello certifikatinformationen överförs Azure tar hänsyn till den här toobe ett betrott rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-223">Once hello certificate information is uploaded, Azure considers this toobe a trusted root certificate.</span></span>

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <span data-ttu-id="91750-224"><a name="clientconfig"></a>7. Hämta hello VPN-klientpaketet konfiguration</span><span class="sxs-lookup"><span data-stu-id="91750-224"><a name="clientconfig"></a>7. Download hello VPN client configuration package</span></span>

<span data-ttu-id="91750-225">tooconnect tooa VNet med en punkt-till-plats-VPN varje klient måste installera ett konfigurationspaket till klienten som konfigurerar hello inbyggda VPN-klienten med hello inställningar och filer som är nödvändiga tooconnect toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="91750-225">tooconnect tooa VNet using a Point-to-Site VPN, each client must install a client configuration package that configures hello native VPN client with hello settings and files that are necessary tooconnect toohello virtual network.</span></span> <span data-ttu-id="91750-226">hello VPN-klientpaketet configuration konfigurerar hello inbyggda Windows VPN-klienten, den installera inte en ny eller en annan VPN-klienten.</span><span class="sxs-lookup"><span data-stu-id="91750-226">hello VPN client configuration package configures hello native Windows VPN client, it doesn't install a new or different VPN client.</span></span> 

<span data-ttu-id="91750-227">Du kan använda samma VPN-klientkonfiguration paketet på varje klientdator hello så länge hello versionen matchar hello arkitektur för hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="91750-227">You can use hello same VPN client configuration package on each client computer, as long as hello version matches hello architecture for hello client.</span></span> <span data-ttu-id="91750-228">Hello lista av klientoperativsystem som stöds finns i hello [punkt-till-plats-anslutningar och svar](#faq) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="91750-228">For hello list of client operating systems that are supported, see hello [Point-to-Site connections FAQ](#faq) at hello end of this article.</span></span>

1. <span data-ttu-id="91750-229">När hello gateway har skapats kan du generera och hämta hello klientpaketet för konfiguration.</span><span class="sxs-lookup"><span data-stu-id="91750-229">After hello gateway has been created, you can generate and download hello client configuration package.</span></span> <span data-ttu-id="91750-230">Det här exemplet hämtar hello-paket för 64-bitarsklienter.</span><span class="sxs-lookup"><span data-stu-id="91750-230">This example downloads hello package for 64-bit clients.</span></span> <span data-ttu-id="91750-231">Om du vill toodownload hello 32-bitars klienten Ersätt 'Amd64' med 'x86'.</span><span class="sxs-lookup"><span data-stu-id="91750-231">If you want toodownload hello 32-bit client, replace 'Amd64' with 'x86'.</span></span> <span data-ttu-id="91750-232">Du kan också hämta hello VPN-klienten med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="91750-232">You can also download hello VPN client by using hello Azure portal.</span></span>

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. <span data-ttu-id="91750-233">Kopiera och klistra in hello länk som returneras tooa web webbläsare toodownload hello paketet, tar hand tooremove hello citattecken runt hello länk.</span><span class="sxs-lookup"><span data-stu-id="91750-233">Copy and paste hello link that is returned tooa web browser toodownload hello package, taking care tooremove hello quotes surrounding hello link.</span></span> 
3. <span data-ttu-id="91750-234">Hämta och installera hello paketet på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="91750-234">Download and install hello package on hello client computer.</span></span> <span data-ttu-id="91750-235">Om du ser ett SmartScreen-popup-fönster klickar du på **Mer information** och sedan på **Kör ändå**.</span><span class="sxs-lookup"><span data-stu-id="91750-235">If you see a SmartScreen popup, click **More info**, then **Run anyway**.</span></span> <span data-ttu-id="91750-236">Du kan också spara hello paketet tooinstall på andra datorer.</span><span class="sxs-lookup"><span data-stu-id="91750-236">You can also save hello package tooinstall on other client computers.</span></span>
4. <span data-ttu-id="91750-237">Hello klientdator, navigera för**nätverksinställningar** och på **VPN**.</span><span class="sxs-lookup"><span data-stu-id="91750-237">On hello client computer, navigate too**Network Settings** and click **VPN**.</span></span> <span data-ttu-id="91750-238">hello VPN-anslutningen visar hello namnet på hello virtuellt nätverk som den ansluter till.</span><span class="sxs-lookup"><span data-stu-id="91750-238">hello VPN connection shows hello name of hello virtual network that it connects to.</span></span>

## <span data-ttu-id="91750-239"><a name="clientcertificate"></a>8. Installera ett exporterat klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-239"><a name="clientcertificate"></a>8. Install an exported client certificate</span></span>

<span data-ttu-id="91750-240">Om du vill toocreate en P2S-anslutning från en klientdator än hello du används klientcertifikat för toogenerate hello måste du tooinstall ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-240">If you want toocreate a P2S connection from a client computer other than hello one you used toogenerate hello client certificates, you need tooinstall a client certificate.</span></span> <span data-ttu-id="91750-241">När du installerar ett klientcertifikat, måste hello lösenordet som skapades när hello klientcertifikat exporterades.</span><span class="sxs-lookup"><span data-stu-id="91750-241">When installing a client certificate, you need hello password that was created when hello client certificate was exported.</span></span> <span data-ttu-id="91750-242">Vanligtvis är det bara gäller att om du dubbelklickar på hello certifikat och installera den.</span><span class="sxs-lookup"><span data-stu-id="91750-242">Typically, it is just a matter of double-clicking hello certificate and installing it.</span></span>

<span data-ttu-id="91750-243">Kontrollera att hello Klientcertifikatet har exporterats som en .pfx tillsammans med hello hela certifikatkedjan (som standard hello).</span><span class="sxs-lookup"><span data-stu-id="91750-243">Make sure hello client certificate was exported as a .pfx along with hello entire certificate chain (which is hello default).</span></span> <span data-ttu-id="91750-244">Annars hello rot certifikatinformationen inte finns på hello klientdator och hello-klienten inte kan tooauthenticate korrekt.</span><span class="sxs-lookup"><span data-stu-id="91750-244">Otherwise, hello root certificate information isn't present on hello client computer and hello client won't be able tooauthenticate properly.</span></span> <span data-ttu-id="91750-245">Mer information finns i [Installera ett exporterat klientcertifikat](vpn-gateway-certificates-point-to-site.md#install).</span><span class="sxs-lookup"><span data-stu-id="91750-245">For more information, see [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install).</span></span> 

## <span data-ttu-id="91750-246"><a name="connect"></a>9. Ansluta tooAzure</span><span class="sxs-lookup"><span data-stu-id="91750-246"><a name="connect"></a>9. Connect tooAzure</span></span>

1. <span data-ttu-id="91750-247">tooconnect tooyour VNet, hello klientdator, navigera tooVPN anslutningar och leta upp hello VPN-anslutning som du skapade.</span><span class="sxs-lookup"><span data-stu-id="91750-247">tooconnect tooyour VNet, on hello client computer, navigate tooVPN connections and locate hello VPN connection that you created.</span></span> <span data-ttu-id="91750-248">Det heter hello samma namn som det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="91750-248">It is named hello same name as your virtual network.</span></span> <span data-ttu-id="91750-249">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="91750-249">Click **Connect**.</span></span> <span data-ttu-id="91750-250">Ett popup-meddelande kan visas som refererar toousing hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-250">A pop-up message may appear that refers toousing hello certificate.</span></span> <span data-ttu-id="91750-251">Klicka på **Fortsätt** toouse utökade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="91750-251">Click **Continue** toouse elevated privileges.</span></span> 
2. <span data-ttu-id="91750-252">På hello **anslutning** statussidan, klickar du på **Anslut** toostart hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="91750-252">On hello **Connection** status page, click **Connect** toostart hello connection.</span></span> <span data-ttu-id="91750-253">Om du ser en **Välj certifikat** skärmen och kontrollera att hello klienten certifikatet visar är hello något som du vill toouse tooconnect.</span><span class="sxs-lookup"><span data-stu-id="91750-253">If you see a **Select Certificate** screen, verify that hello client certificate showing is hello one that you want toouse tooconnect.</span></span> <span data-ttu-id="91750-254">Om det inte använda hello listrutepilen tooselect hello rätt certifikat och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="91750-254">If it is not, use hello drop-down arrow tooselect hello correct certificate, and then click **OK**.</span></span>

  ![VPN-klienten ansluter tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. <span data-ttu-id="91750-256">Anslutningen upprättas.</span><span class="sxs-lookup"><span data-stu-id="91750-256">Your connection is established.</span></span>

  ![Anslutning upprättad](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a><span data-ttu-id="91750-258">Felsöka P2S-anslutningar</span><span class="sxs-lookup"><span data-stu-id="91750-258">Troubleshooting P2S connections</span></span>

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <span data-ttu-id="91750-259"><a name="verify"></a>10. Verifiera din anslutning</span><span class="sxs-lookup"><span data-stu-id="91750-259"><a name="verify"></a>10. Verify your connection</span></span>

1. <span data-ttu-id="91750-260">tooverify att VPN-anslutningen är aktiv, öppna en upphöjd kommandotolk och kör *ipconfig/all*.</span><span class="sxs-lookup"><span data-stu-id="91750-260">tooverify that your VPN connection is active, open an elevated command prompt, and run *ipconfig/all*.</span></span>
2. <span data-ttu-id="91750-261">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="91750-261">View hello results.</span></span> <span data-ttu-id="91750-262">Observera att hello IP-adress som du fått är en av hello-adresser inom hello punkt-till-plats VPN-Klientadresspool som du angav i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="91750-262">Notice that hello IP address you received is one of hello addresses within hello Point-to-Site VPN Client Address Pool that you specified in your configuration.</span></span> <span data-ttu-id="91750-263">hello resultatet är liknande toothis exempel:</span><span class="sxs-lookup"><span data-stu-id="91750-263">hello results are similar toothis example:</span></span>

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <span data-ttu-id="91750-264"><a name="connectVM"></a>Ansluta tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="91750-264"><a name="connectVM"></a>Connect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <span data-ttu-id="91750-265"><a name="addremovecert"></a>Lägga till eller ta bort ett rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-265"><a name="addremovecert"></a>Add or remove a root certificate</span></span>

<span data-ttu-id="91750-266">Du kan lägga till och ta bort betrodda rotcertifikat från Azure.</span><span class="sxs-lookup"><span data-stu-id="91750-266">You can add and remove trusted root certificates from Azure.</span></span> <span data-ttu-id="91750-267">När du tar bort ett rotcertifikat klienter som har ett certifikat som genereras från hello rotcertifikatet inte kan autentisera och inte kan tooconnect.</span><span class="sxs-lookup"><span data-stu-id="91750-267">When you remove a root certificate, clients that have a certificate generated from hello root certificate can't authenticate and won't be able tooconnect.</span></span> <span data-ttu-id="91750-268">Om du vill att en klient tooauthenticate och ansluter behöver du ett nytt klientcertifikat genereras från ett rotcertifikat som är betrodd (överförda) tooAzure tooinstall.</span><span class="sxs-lookup"><span data-stu-id="91750-268">If you want a client tooauthenticate and connect, you need tooinstall a new client certificate generated from a root certificate that is trusted (uploaded) tooAzure.</span></span>

### <span data-ttu-id="91750-269"><a name="addtrustedroot"></a>tooadd ett betrott rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-269"><a name="addtrustedroot"></a>tooadd a trusted root certificate</span></span>

<span data-ttu-id="91750-270">Du kan lägga upp too20 root certificate .cer filer tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91750-270">You can add up too20 root certificate .cer files tooAzure.</span></span> <span data-ttu-id="91750-271">hello följande steg hjälp om du lägger till ett rotcertifikat:</span><span class="sxs-lookup"><span data-stu-id="91750-271">hello following steps help you add a root certificate:</span></span>

#### <span data-ttu-id="91750-272"><a name="certmethod1"></a>Metod 1</span><span class="sxs-lookup"><span data-stu-id="91750-272"><a name="certmethod1"></a>Method 1</span></span>

<span data-ttu-id="91750-273">Detta är hello effektivaste metoden tooupload ett rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-273">This is hello most efficient method tooupload a root certificate.</span></span>

1. <span data-ttu-id="91750-274">Förbered hello .cer-filen tooupload:</span><span class="sxs-lookup"><span data-stu-id="91750-274">Prepare hello .cer file tooupload:</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. <span data-ttu-id="91750-275">Överför hello-fil.</span><span class="sxs-lookup"><span data-stu-id="91750-275">Upload hello file.</span></span> <span data-ttu-id="91750-276">Du kan bara ladda upp en fil i taget.</span><span class="sxs-lookup"><span data-stu-id="91750-276">You can only upload one file at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. <span data-ttu-id="91750-277">tooverify som hello certifikatfilen upp:</span><span class="sxs-lookup"><span data-stu-id="91750-277">tooverify that hello certificate file uploaded:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <span data-ttu-id="91750-278"><a name="certmethod2"></a>Metod 2</span><span class="sxs-lookup"><span data-stu-id="91750-278"><a name="certmethod2"></a>Method 2</span></span>

<span data-ttu-id="91750-279">Den här metoden är har fler steg än metod 1, men har hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="91750-279">This method is has more steps than Method 1, but has hello same result.</span></span> <span data-ttu-id="91750-280">Det ingår om du behöver information om tooview hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-280">It is included in case you need tooview hello certificate data.</span></span>

1. <span data-ttu-id="91750-281">Skapa och förbereda hello nya root certificate tooadd tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91750-281">Create and prepare hello new root certificate tooadd tooAzure.</span></span> <span data-ttu-id="91750-282">Exportera hello offentlig nyckel som en Base64-kodad X.509 (. CER) och öppna den med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="91750-282">Export hello public key as a Base-64 encoded X.509 (.CER) and open it with a text editor.</span></span> <span data-ttu-id="91750-283">Kopiera hello värden som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="91750-283">Copy hello values, as shown in hello following example:</span></span>

  ![certifikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > <span data-ttu-id="91750-285">När du kopierar hello certifikatdata måste du kopiera hello text som en kontinuerlig rad utan vagnreturer eller radmatningstecken.</span><span class="sxs-lookup"><span data-stu-id="91750-285">When copying hello certificate data, make sure that you copy hello text as one continuous line without carriage returns or line feeds.</span></span> <span data-ttu-id="91750-286">Du kan behöva toomodify vyn i hello text editor too'Show symbolen/Visa alla tecken toosee hello transport returnerar och radmatningar.</span><span class="sxs-lookup"><span data-stu-id="91750-286">You may need toomodify your view in hello text editor too'Show Symbol/Show all characters' toosee hello carriage returns and line feeds.</span></span>
  >
  >

2. <span data-ttu-id="91750-287">Ange hello certifikatets namn och viktig information som en variabel.</span><span class="sxs-lookup"><span data-stu-id="91750-287">Specify hello certificate name and key information as a variable.</span></span> <span data-ttu-id="91750-288">Ersätt hello information med ditt eget, vilket visas i hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91750-288">Replace hello information with your own, as shown in hello following example:</span></span>

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. <span data-ttu-id="91750-289">Lägg till nytt rotcertifikat för hello.</span><span class="sxs-lookup"><span data-stu-id="91750-289">Add hello new root certificate.</span></span> <span data-ttu-id="91750-290">Du kan bara lägga till ett certifikat i taget.</span><span class="sxs-lookup"><span data-stu-id="91750-290">You can only add one certificate at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. <span data-ttu-id="91750-291">Du kan verifiera att hello det nya certifikatet har lagts till korrekt genom att använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91750-291">You can verify that hello new certificate was added correctly by using hello following example:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <span data-ttu-id="91750-292"><a name="removerootcert"></a>tooremove ett rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-292"><a name="removerootcert"></a>tooremove a root certificate</span></span>

1. <span data-ttu-id="91750-293">Deklarera hello variabler.</span><span class="sxs-lookup"><span data-stu-id="91750-293">Declare hello variables.</span></span>

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. <span data-ttu-id="91750-294">Ta bort hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-294">Remove hello certificate.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. <span data-ttu-id="91750-295">Använd hello följande exempel tooverify som hello certifikatet har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="91750-295">Use hello following example tooverify that hello certificate was removed successfully.</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <span data-ttu-id="91750-296"><a name="revoke"></a>Återkalla ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-296"><a name="revoke"></a>Revoke a client certificate</span></span>

<span data-ttu-id="91750-297">Du kan återkalla certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-297">You can revoke client certificates.</span></span> <span data-ttu-id="91750-298">hello certifikat listan över återkallade certifikat kan du tooselectively neka punkt-till-plats-anslutning baserat på enskilda klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-298">hello certificate revocation list allows you tooselectively deny Point-to-Site connectivity based on individual client certificates.</span></span> <span data-ttu-id="91750-299">Det här skiljer sig från att ta bort ett betrott rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-299">This is different than removing a trusted root certificate.</span></span> <span data-ttu-id="91750-300">Om du tar bort en betrodda certifikat .cer från Azure återkallar hello åtkomst för alla klientcertifikat som genererats/signerats av hello återkallade rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-300">If you remove a trusted root certificate .cer from Azure, it revokes hello access for all client certificates generated/signed by hello revoked root certificate.</span></span> <span data-ttu-id="91750-301">Återkalla ett certifikat kan i stället för hello rotcertifikatet, hello andra certifikat som har genererats från hello root certificate toocontinue toobe används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="91750-301">Revoking a client certificate, rather than hello root certificate, allows hello other certificates that were generated from hello root certificate toocontinue toobe used for authentication.</span></span>

<span data-ttu-id="91750-302">hello vanligt är toouse hello certifikat toomanage rotåtkomst på grupp eller organisation nivåer, medan återkallade klientcertifikaten för detaljerad åtkomstkontroll på enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="91750-302">hello common practice is toouse hello root certificate toomanage access at team or organization levels, while using revoked client certificates for fine-grained access control on individual users.</span></span>

### <span data-ttu-id="91750-303"><a name="revokeclientcert"></a>toorevoke ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-303"><a name="revokeclientcert"></a>toorevoke a client certificate</span></span>

1. <span data-ttu-id="91750-304">Hämta hello Klientcertifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="91750-304">Retrieve hello client certificate thumbprint.</span></span> <span data-ttu-id="91750-305">Mer information finns i [hur tooretrieve hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx).</span><span class="sxs-lookup"><span data-stu-id="91750-305">For more information, see [How tooretrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695.aspx).</span></span>
2. <span data-ttu-id="91750-306">Kopiera hello information tooa textredigerare och ta bort alla blanksteg så att det är en kontinuerlig sträng.</span><span class="sxs-lookup"><span data-stu-id="91750-306">Copy hello information tooa text editor and remove all spaces so that it is a continuous string.</span></span> <span data-ttu-id="91750-307">Den här strängen har deklarerats som en variabel i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="91750-307">This string is declared as a variable in hello next step.</span></span>
3. <span data-ttu-id="91750-308">Deklarera hello variabler.</span><span class="sxs-lookup"><span data-stu-id="91750-308">Declare hello variables.</span></span> <span data-ttu-id="91750-309">Kontrollera att toodeclare hello tumavtryck som du hämtade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="91750-309">Make sure toodeclare hello thumbprint you retrieved in hello previous step.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. <span data-ttu-id="91750-310">Lägg till hello tumavtrycket toohello lista över återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-310">Add hello thumbprint toohello list of revoked certificates.</span></span> <span data-ttu-id="91750-311">”Lyckades” visas när hello tumavtryck har lagts till.</span><span class="sxs-lookup"><span data-stu-id="91750-311">You see "Succeeded" when hello thumbprint has been added.</span></span>

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. <span data-ttu-id="91750-312">Kontrollera att hello tumavtryck har lagts till toohello lista över återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-312">Verify that hello thumbprint was added toohello certificate revocation list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. <span data-ttu-id="91750-313">När hello tumavtryck har lagts att hello certifikatet inte längre använda tooconnect.</span><span class="sxs-lookup"><span data-stu-id="91750-313">After hello thumbprint has been added, hello certificate can no longer be used tooconnect.</span></span> <span data-ttu-id="91750-314">Klienter som försöker tooconnect med det här certifikatet får ett meddelande om hello certifikatet är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="91750-314">Clients that try tooconnect using this certificate receive a message saying that hello certificate is no longer valid.</span></span>

### <span data-ttu-id="91750-315"><a name="reinstateclientcert"></a>tooreinstate ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="91750-315"><a name="reinstateclientcert"></a>tooreinstate a client certificate</span></span>

<span data-ttu-id="91750-316">Du kan återställa ett klientcertifikat genom att ta bort hello tumavtrycket hello listan över återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-316">You can reinstate a client certificate by removing hello thumbprint from hello list of revoked client certificates.</span></span>

1. <span data-ttu-id="91750-317">Deklarera hello variabler.</span><span class="sxs-lookup"><span data-stu-id="91750-317">Declare hello variables.</span></span> <span data-ttu-id="91750-318">Kontrollera att du deklarera hello rätt tumavtrycket för hello certifikat som du vill tooreinstate.</span><span class="sxs-lookup"><span data-stu-id="91750-318">Make sure you declare hello correct thumbprint for hello certificate that you want tooreinstate.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. <span data-ttu-id="91750-319">Ta bort hello certifikatets tumavtryck från hello lista över återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="91750-319">Remove hello certificate thumbprint from hello certificate revocation list.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. <span data-ttu-id="91750-320">Kontrollera om hello tumavtrycket tas bort från hello återkallas lista.</span><span class="sxs-lookup"><span data-stu-id="91750-320">Check if hello thumbprint is removed from hello revoked list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <span data-ttu-id="91750-321"><a name="faq"></a>Vanliga frågor och svar om punkt-till-plats</span><span class="sxs-lookup"><span data-stu-id="91750-321"><a name="faq"></a>Point-to-Site FAQ</span></span>

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="91750-322">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91750-322">Next steps</span></span>
<span data-ttu-id="91750-323">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="91750-323">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="91750-324">Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="91750-324">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span> <span data-ttu-id="91750-325">toounderstand mer information om nätverk och virtuella datorer, se [översikt över Azure och Linux VM](../virtual-machines/linux/azure-vm-network-overview.md).</span><span class="sxs-lookup"><span data-stu-id="91750-325">toounderstand more about networking and virtual machines, see [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md).</span></span>
