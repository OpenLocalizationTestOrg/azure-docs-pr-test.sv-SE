---
title: "Ta bort en virtuell nätverksgateway: PowerShell: klassiska Azure-portalen | Microsoft Docs"
description: "Ta bort en virtuell nätverksgateway med PowerShell i den klassiska distributionsmodellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: b1bc18307227a728e2bc8fd95e30fdc1cbdb8c59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a><span data-ttu-id="68e6e-103">Ta bort en virtuell nätverksgateway med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="68e6e-103">Delete a virtual network gateway using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68e6e-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68e6e-104">Resource Manager - Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="68e6e-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="68e6e-105">Resource Manager - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="68e6e-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="68e6e-106">Classic - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="68e6e-107">Den här artikeln hjälper dig att ta bort en VPN-gateway i den klassiska distributionsmodellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="68e6e-107">This article helps you delete a VPN gateway in the classic deployment model by using PowerShell.</span></span> <span data-ttu-id="68e6e-108">När den virtuella nätverksgatewayen har tagits bort, ändra konfigurationsfilen nätverk för att ta bort element som du inte längre använder.</span><span class="sxs-lookup"><span data-stu-id="68e6e-108">After the virtual network gateway has been deleted, modify the network configuration file to remove elements that you are no longer using.</span></span>

##<span data-ttu-id="68e6e-109"><a name="connect"></a>Steg 1: Ansluta till Azure</span><span class="sxs-lookup"><span data-stu-id="68e6e-109"><a name="connect"></a>Step 1: Connect to Azure</span></span>

### <a name="1-install-the-latest-powershell-cmdlets"></a><span data-ttu-id="68e6e-110">1. Installera de senaste PowerShell-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="68e6e-110">1. Install the latest PowerShell cmdlets.</span></span>

<span data-ttu-id="68e6e-111">Hämta och installera den senaste versionen av Azure Service Management (SM) PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="68e6e-111">Download and install the latest version of the Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="68e6e-112">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="68e6e-112">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="68e6e-113">2. Ansluta till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="68e6e-113">2. Connect to your Azure account.</span></span> 

<span data-ttu-id="68e6e-114">Öppna PowerShell-konsolen med utökade rättigheter och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="68e6e-114">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="68e6e-115">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="68e6e-115">Use the following example to help you connect:</span></span>

```powershell
Add-AzureAccount
```

## <span data-ttu-id="68e6e-116"><a name="export"></a>Steg 2: Exportera och visa konfigurationsfilen nätverk</span><span class="sxs-lookup"><span data-stu-id="68e6e-116"><a name="export"></a>Step 2: Export and view the network configuration file</span></span>

<span data-ttu-id="68e6e-117">Skapa en katalog på datorn och exportera sedan nätverkskonfigurationsfilen till katalogen.</span><span class="sxs-lookup"><span data-stu-id="68e6e-117">Create a directory on your computer and then export the network configuration file to the directory.</span></span> <span data-ttu-id="68e6e-118">Du kan använda den här filen till båda visa den aktuella konfigurationsinformationen och även för att ändra nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="68e6e-118">You use this file to both view the current configuration information, and also to modify the network configuration.</span></span>

<span data-ttu-id="68e6e-119">I det här exemplet exporteras nätverkskonfigurationsfilen till C:\AzureNet.</span><span class="sxs-lookup"><span data-stu-id="68e6e-119">In this example, the network configuration file is exported to C:\AzureNet.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="68e6e-120">Öppna filen i en textredigerare och visa namnet för din klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="68e6e-120">Open the file with a text editor and view the name for your classic VNet.</span></span> <span data-ttu-id="68e6e-121">När du skapar ett VNet i Azure portal är det fullständiga namnet som använder Azure inte visas på portalen.</span><span class="sxs-lookup"><span data-stu-id="68e6e-121">When you create a VNet in the Azure portal, the full name that Azure uses is not visible in the portal.</span></span> <span data-ttu-id="68e6e-122">Ett VNet som verkar vara med namnet 'ClassicVNet1' i Azure-portalen kan ha en mycket längre namn i konfigurationsfilen på nätverket.</span><span class="sxs-lookup"><span data-stu-id="68e6e-122">For example, a VNet that appears to be named 'ClassicVNet1' in the Azure portal, may have a much longer name in the network configuration file.</span></span> <span data-ttu-id="68e6e-123">Namnet kan se ut ungefär så: 'Grupp ClassicRG1 ClassicVNet1'.</span><span class="sxs-lookup"><span data-stu-id="68e6e-123">The name might look something like: 'Group ClassicRG1 ClassicVNet1'.</span></span> <span data-ttu-id="68e6e-124">Virtuella nätverksnamn listas som **' VirtualNetworkSite name ='**.</span><span class="sxs-lookup"><span data-stu-id="68e6e-124">Virtual network names are listed as **'VirtualNetworkSite name ='**.</span></span> <span data-ttu-id="68e6e-125">Använda namnen i konfigurationsfilen på nätverket när du kör PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="68e6e-125">Use the names in the network configuration file when running your PowerShell cmdlets.</span></span>

## <span data-ttu-id="68e6e-126"><a name="delete"></a>Steg 3: Ta bort den virtuella nätverksgatewayen</span><span class="sxs-lookup"><span data-stu-id="68e6e-126"><a name="delete"></a>Step 3: Delete the virtual network gateway</span></span>

<span data-ttu-id="68e6e-127">När du tar bort en virtuell nätverksgateway kopplas alla anslutningar till virtuella nätverk via gatewayen.</span><span class="sxs-lookup"><span data-stu-id="68e6e-127">When you delete a virtual network gateway, all connections to the VNet through the gateway are disconnected.</span></span> <span data-ttu-id="68e6e-128">Om du har P2S-klienter som är anslutna till det virtuella nätverket, kopplas de utan varning.</span><span class="sxs-lookup"><span data-stu-id="68e6e-128">If you have P2S clients connected to the VNet, they will be disconnected without warning.</span></span>

<span data-ttu-id="68e6e-129">Det här exemplet tar bort den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="68e6e-129">This example deletes the virtual network gateway.</span></span> <span data-ttu-id="68e6e-130">Se till att använda det fullständiga namnet på det virtuella nätverket från konfigurationsfilen nätverk.</span><span class="sxs-lookup"><span data-stu-id="68e6e-130">Make sure to use the full name of the virtual network from the network configuration file.</span></span>

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

<span data-ttu-id="68e6e-131">Om detta lyckas visar tillbaka:</span><span class="sxs-lookup"><span data-stu-id="68e6e-131">If successful, the return shows:</span></span>

```
Status : Successful
```

## <span data-ttu-id="68e6e-132"><a name="modify"></a>Steg 4: Ändra konfigurationsfilen nätverk</span><span class="sxs-lookup"><span data-stu-id="68e6e-132"><a name="modify"></a>Step 4: Modify the network configuration file</span></span>

<span data-ttu-id="68e6e-133">När du tar bort en virtuell nätverksgateway ändrar cmdlet inte konfigurationsfilen nätverk.</span><span class="sxs-lookup"><span data-stu-id="68e6e-133">When you delete a virtual network gateway, the cmdlet does not modify the network configuration file.</span></span> <span data-ttu-id="68e6e-134">Du måste ändra den här filen för att ta bort de element som används inte längre.</span><span class="sxs-lookup"><span data-stu-id="68e6e-134">You need to modify the file to remove the elements that are no longer being used.</span></span> <span data-ttu-id="68e6e-135">I följande avsnitt hjälpa dig att ändra konfigurationsfilen nätverk som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="68e6e-135">The following sections help you modify the network configuration file that you downloaded.</span></span>

### <span data-ttu-id="68e6e-136"><a name="lnsref"></a>Hänvisningar till lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="68e6e-136"><a name="lnsref"></a>Local Network Site References</span></span>

<span data-ttu-id="68e6e-137">Om du vill ta bort platsen referensinformation, göra konfigurationsändringar i **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span><span class="sxs-lookup"><span data-stu-id="68e6e-137">To remove site reference information, make configuration changes to **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span></span> <span data-ttu-id="68e6e-138">Tar bort en lokal plats referens utlösare Azure för att ta bort en tunnel.</span><span class="sxs-lookup"><span data-stu-id="68e6e-138">Removing a local site reference triggers Azure to delete a tunnel.</span></span> <span data-ttu-id="68e6e-139">Beroende på konfigurationen som du har skapat, kanske du inte har en **LocalNetworkSiteRef** visas.</span><span class="sxs-lookup"><span data-stu-id="68e6e-139">Depending on the configuration that you created, you may not have a **LocalNetworkSiteRef** listed.</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

<span data-ttu-id="68e6e-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68e6e-140">Example:</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

###<span data-ttu-id="68e6e-141"><a name="lns"></a>Lokala nätverksplatser</span><span class="sxs-lookup"><span data-stu-id="68e6e-141"><a name="lns"></a>Local Network Sites</span></span>

<span data-ttu-id="68e6e-142">Ta bort alla lokala webbplatser som du inte längre använder.</span><span class="sxs-lookup"><span data-stu-id="68e6e-142">Remove any local sites that you are no longer using.</span></span> <span data-ttu-id="68e6e-143">Beroende på konfigurationen som du har skapat, är det möjligt att du inte har en **LocalNetworkSite** visas.</span><span class="sxs-lookup"><span data-stu-id="68e6e-143">Depending on the configuration you created, it is possible that you don't have a **LocalNetworkSite** listed.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

<span data-ttu-id="68e6e-144">I det här exemplet bort vi bara Site3.</span><span class="sxs-lookup"><span data-stu-id="68e6e-144">In this example, we removed only Site3.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <span data-ttu-id="68e6e-145"><a name="clientaddresss"></a>Klientadresspool</span><span class="sxs-lookup"><span data-stu-id="68e6e-145"><a name="clientaddresss"></a>Client AddressPool</span></span>

<span data-ttu-id="68e6e-146">Om du har en P2S-anslutning till ditt VNet, har du en **VPNClientAddressPool**.</span><span class="sxs-lookup"><span data-stu-id="68e6e-146">If you had a P2S connection to your VNet, you will have a **VPNClientAddressPool**.</span></span> <span data-ttu-id="68e6e-147">Ta bort de klient-adresspooler som motsvarar den virtuella nätverksgatewayen som tagits bort.</span><span class="sxs-lookup"><span data-stu-id="68e6e-147">Remove the client address pools that correspond to the virtual network gateway that you deleted.</span></span>

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

<span data-ttu-id="68e6e-148">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68e6e-148">Example:</span></span>

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <span data-ttu-id="68e6e-149"><a name="gwsub"></a>GatewaySubnet</span><span class="sxs-lookup"><span data-stu-id="68e6e-149"><a name="gwsub"></a>GatewaySubnet</span></span>

<span data-ttu-id="68e6e-150">Ta bort den **GatewaySubnet** som motsvarar VNet.</span><span class="sxs-lookup"><span data-stu-id="68e6e-150">Delete the **GatewaySubnet** that corresponds to the VNet.</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

<span data-ttu-id="68e6e-151">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68e6e-151">Example:</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <span data-ttu-id="68e6e-152"><a name="upload"></a>Steg 5: Överför konfigurationsfilen nätverk</span><span class="sxs-lookup"><span data-stu-id="68e6e-152"><a name="upload"></a>Step 5: Upload the network configuration file</span></span>

<span data-ttu-id="68e6e-153">Spara dina ändringar och överför konfigurationsfilen nätverk till Azure.</span><span class="sxs-lookup"><span data-stu-id="68e6e-153">Save your changes and upload the network configuration file to Azure.</span></span> <span data-ttu-id="68e6e-154">Kontrollera att du ändrar sökvägen till filen som behövs för din miljö.</span><span class="sxs-lookup"><span data-stu-id="68e6e-154">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="68e6e-155">Om detta lyckas visar tillbaka något liknar det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="68e6e-155">If successful, the return shows something similar to this example:</span></span>

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded