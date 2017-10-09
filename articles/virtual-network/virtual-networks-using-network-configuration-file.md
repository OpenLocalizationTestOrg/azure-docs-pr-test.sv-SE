---
title: "aaaConfigure ett Azure Virtual Network (klassisk) - nätverket konfigurationsfilen | Microsoft Docs"
description: "Lär dig hur toocreate och ändra virtuella nätverk (klassiska) genom att exportera, ändra och importera en konfigurationsfil för nätverket."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 009108d4315b4b7146d3f1cf2436ee211d2d89ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a><span data-ttu-id="08394-103">Konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverk</span><span class="sxs-lookup"><span data-stu-id="08394-103">Configure a virtual network (classic) using a network configuration file</span></span>
> [!IMPORTANT]
> <span data-ttu-id="08394-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08394-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="08394-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="08394-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="08394-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="08394-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="08394-107">Du kan skapa och konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket med hjälp av hello Azure-kommandoradsgränssnittet (CLI) 1.0 eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08394-107">You can create and configure a virtual network (classic) with a network configuration file using hello Azure command-line interface (CLI) 1.0 or Azure PowerShell.</span></span> <span data-ttu-id="08394-108">Du kan inte skapa eller ändra ett virtuellt nätverk med hello Azure Resource Manager-distributionsmodellen med en konfigurationsfil för nätverket.</span><span class="sxs-lookup"><span data-stu-id="08394-108">You cannot create or modify a virtual network through hello Azure Resource Manager deployment model using a network configuration file.</span></span> <span data-ttu-id="08394-109">Du kan inte använda hello Azure portal toocreate eller ändra ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket, men du kan använda hello Azure portal toocreate ett virtuellt nätverk (klassiskt), utan att använda en konfigurationsfil för nätverket.</span><span class="sxs-lookup"><span data-stu-id="08394-109">You cannot use hello Azure portal toocreate or modify a virtual network (classic) using a network configuration file, however you can use hello Azure portal toocreate a virtual network (classic), without using a network configuration file.</span></span>

<span data-ttu-id="08394-110">Skapa och konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket kräver exportera, ändra och importera hello-fil.</span><span class="sxs-lookup"><span data-stu-id="08394-110">Creating and configuring a virtual network (classic) with a network configuration file requires exporting, changing, and importing hello file.</span></span>

## <span data-ttu-id="08394-111"><a name="export"></a>Exportera en konfigurationsfil för nätverk</span><span class="sxs-lookup"><span data-stu-id="08394-111"><a name="export"></a>Export a network configuration file</span></span>

<span data-ttu-id="08394-112">Du kan använda PowerShell eller hello Azure CLI tooexport en konfigurationsfil för nätverket.</span><span class="sxs-lookup"><span data-stu-id="08394-112">You can use PowerShell or hello Azure CLI tooexport a network configuration file.</span></span> <span data-ttu-id="08394-113">PowerShell exporterar en XML-fil, medan hello Azure CLI exporterar en json-fil.</span><span class="sxs-lookup"><span data-stu-id="08394-113">PowerShell exports an XML file, while hello Azure CLI exports a json file.</span></span>

### <a name="powershell"></a><span data-ttu-id="08394-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08394-114">PowerShell</span></span>
 
1. <span data-ttu-id="08394-115">[Installera Azure PowerShell och logga in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08394-115">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="08394-116">Ändra hello katalogen (och se till att den finns) och filnamnet i hello följande kommando som önskad och sedan kör hello kommandot tooexport hello nätverket konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="08394-116">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="08394-117">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08394-117">Azure CLI 1.0</span></span>

1. <span data-ttu-id="08394-118">[Installera hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08394-118">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="08394-119">Slutför hello återstående steg från en kommandotolk med Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="08394-119">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="08394-120">Logga in tooAzure genom att ange hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="08394-120">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="08394-121">Kontrollera att du befinner dig i asm-läge genom att ange hello `azure config mode asm` kommando.</span><span class="sxs-lookup"><span data-stu-id="08394-121">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="08394-122">Ändra hello katalogen (och se till att den finns) och filnamnet i hello följande kommando som önskad och sedan kör hello kommandot tooexport hello nätverket konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="08394-122">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a><span data-ttu-id="08394-123">Skapa eller ändra en konfigurationsfil för nätverk</span><span class="sxs-lookup"><span data-stu-id="08394-123">Create or modify a network configuration file</span></span>

<span data-ttu-id="08394-124">En konfigurationsfil för nätverk är en XML-fil (när du använder PowerShell) eller en json-fil (när du använder hello Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="08394-124">A network configuration file is an XML file (when using PowerShell) or a json file (when using hello Azure CLI).</span></span> <span data-ttu-id="08394-125">Du kan redigera hello-fil i en text, eller XML/json-redigerare.</span><span class="sxs-lookup"><span data-stu-id="08394-125">You can edit hello file in any text, or XML/json editor.</span></span> <span data-ttu-id="08394-126">Hej [nätverk konfigurationsinställningar filen schemat](https://msdn.microsoft.com/library/azure/jj157100.aspx) artikeln innehåller information om alla inställningar.</span><span class="sxs-lookup"><span data-stu-id="08394-126">hello [Network configuration file schema settings](https://msdn.microsoft.com/library/azure/jj157100.aspx) article includes details for all settings.</span></span> <span data-ttu-id="08394-127">Ytterligare förklaring av hello inställningar finns [Visa inställningar för virtuella nätverk och](virtual-network-manage-network.md#view-vnet).</span><span class="sxs-lookup"><span data-stu-id="08394-127">For additional explanation of hello settings, see [View virtual networks and settings](virtual-network-manage-network.md#view-vnet).</span></span> <span data-ttu-id="08394-128">hello ändringarna toohello fil:</span><span class="sxs-lookup"><span data-stu-id="08394-128">hello changes you make toohello file:</span></span>

- <span data-ttu-id="08394-129">Måste vara kompatibel med hello schemat eller importera hello nätverk konfigurationsfilen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="08394-129">Must comply with hello schema, or importing hello network configuration file will fail.</span></span>
- <span data-ttu-id="08394-130">Skriv över alla befintliga nätverksinställningarna för din prenumeration, så mycket försiktig när du gör ändringar.</span><span class="sxs-lookup"><span data-stu-id="08394-130">Overwrite any existing network settings for your subscription, so use extreme caution when making modifications.</span></span> <span data-ttu-id="08394-131">Referera exempelvis hello exempel network configuration-filer som följer.</span><span class="sxs-lookup"><span data-stu-id="08394-131">For example, reference hello example network configuration files that follow.</span></span> <span data-ttu-id="08394-132">Säg hello originalfilen finns två **VirtualNetworkSite** instanser och du ändrade den som visas i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="08394-132">Say hello original file contained two **VirtualNetworkSite** instances, and you changed it, as shown in hello examples.</span></span> <span data-ttu-id="08394-133">När du importerar filen hello Azure tar bort hello virtuellt nätverk för hello **VirtualNetworkSite** instans som du har tagit bort i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="08394-133">When you import hello file, Azure deletes hello virtual network for hello **VirtualNetworkSite** instance you removed in hello file.</span></span> <span data-ttu-id="08394-134">Det här förenklad scenariot förutsätter att inga resurser har i hello virtuella nätverk, som om det fanns hello virtuellt nätverk kunde inte tas bort och hello import skulle misslyckas.</span><span class="sxs-lookup"><span data-stu-id="08394-134">This simplified scenario assumes no resources were in hello virtual network, as if there were, hello virtual network could not be deleted, and hello import would fail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08394-135">Azure tar hänsyn till ett undernät som har något distribueras tooit som **används**.</span><span class="sxs-lookup"><span data-stu-id="08394-135">Azure considers a subnet that has something deployed tooit as **in use**.</span></span> <span data-ttu-id="08394-136">När ett undernät kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="08394-136">When a subnet is in use, it cannot be modified.</span></span> <span data-ttu-id="08394-137">Flytta allt som du har distribuerat toohello undernät tooa annat undernät som inte ändras innan du ändrar information om undernät i en konfigurationsfil för nätverket.</span><span class="sxs-lookup"><span data-stu-id="08394-137">Before modifying subnet information in a network configuration file, move anything that you have deployed toohello subnet tooa different subnet that isn't being modified.</span></span> <span data-ttu-id="08394-138">Se [flytta en virtuell dator eller Rollinstans tooa annat undernät](virtual-networks-move-vm-role-to-subnet.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="08394-138">See [Move a VM or Role Instance tooa Different Subnet](virtual-networks-move-vm-role-to-subnet.md) for details.</span></span>

### <a name="example-xml-for-use-with-powershell"></a><span data-ttu-id="08394-139">XML-exempel för användning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="08394-139">Example XML for use with PowerShell</span></span>

<span data-ttu-id="08394-140">hello följande exempel nätverket konfigurationsfil skapar ett virtuellt nätverk med namnet *myVirtualNetwork* med ett adressutrymme för *10.0.0.0/16* i hello *östra USA* Azure region.</span><span class="sxs-lookup"><span data-stu-id="08394-140">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="08394-141">hello virtuellt nätverk innehåller ett undernät med namnet *mySubnet* med en adressprefixet *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="08394-141">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

<span data-ttu-id="08394-142">Om hello nätverket konfigurationsfil som du exporterade innehåller inget innehåll, kan du kopiera hello XML i hello föregående exempel och klistra in den i en ny fil.</span><span class="sxs-lookup"><span data-stu-id="08394-142">If hello network configuration file you exported contains no contents, you can copy hello XML in hello previous example, and paste it into a new file.</span></span>

### <a name="example-json-for-use-with-hello-azure-cli-10"></a><span data-ttu-id="08394-143">Exempel JSON för användning med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08394-143">Example JSON for use with hello Azure CLI 1.0</span></span>

<span data-ttu-id="08394-144">hello följande exempel nätverket konfigurationsfil skapar ett virtuellt nätverk med namnet *myVirtualNetwork* med ett adressutrymme för *10.0.0.0/16* i hello *östra USA* Azure region.</span><span class="sxs-lookup"><span data-stu-id="08394-144">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="08394-145">hello virtuellt nätverk innehåller ett undernät med namnet *mySubnet* med en adressprefixet *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="08394-145">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

<span data-ttu-id="08394-146">Om hello nätverket konfigurationsfil som du exporterade innehåller inget innehåll, kan du kopiera hello json i hello föregående exempel och klistra in den i en ny fil.</span><span class="sxs-lookup"><span data-stu-id="08394-146">If hello network configuration file you exported contains no contents, you can copy hello json in hello previous example, and paste it into a new file.</span></span>

## <span data-ttu-id="08394-147"><a name="import"></a>Importera en konfigurationsfil för nätverk</span><span class="sxs-lookup"><span data-stu-id="08394-147"><a name="import"></a>Import a network configuration file</span></span>

<span data-ttu-id="08394-148">Du kan använda PowerShell eller hello Azure CLI tooimport en konfigurationsfil för nätverket.</span><span class="sxs-lookup"><span data-stu-id="08394-148">You can use PowerShell or hello Azure CLI tooimport a network configuration file.</span></span> <span data-ttu-id="08394-149">PowerShell importerar en XML-fil, medan hello Azure CLI importerar en json-fil.</span><span class="sxs-lookup"><span data-stu-id="08394-149">PowerShell imports an XML file, while hello Azure CLI imports a json file.</span></span> <span data-ttu-id="08394-150">Om det inte går att importera hello, bekräfta hello filen uppfyller hello [nätverk Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="08394-150">If hello import fails, confirm that hello file complies with hello [network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span> 

### <a name="powershell"></a><span data-ttu-id="08394-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08394-151">PowerShell</span></span>
 
1. <span data-ttu-id="08394-152">[Installera Azure PowerShell och logga in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08394-152">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="08394-153">Ändra hello katalog och ett filnamn i hello följande kommando vid behov och kör sedan hello tooimport hello network configuration kommandofilen:</span><span class="sxs-lookup"><span data-stu-id="08394-153">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="08394-154">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08394-154">Azure CLI 1.0</span></span>

1. <span data-ttu-id="08394-155">[Installera hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08394-155">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="08394-156">Slutför hello återstående steg från en kommandotolk med Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="08394-156">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="08394-157">Logga in tooAzure genom att ange hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="08394-157">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="08394-158">Kontrollera att du befinner dig i asm-läge genom att ange hello `azure config mode asm` kommando.</span><span class="sxs-lookup"><span data-stu-id="08394-158">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="08394-159">Ändra hello katalog och ett filnamn i hello följande kommando vid behov och kör sedan hello tooimport hello network configuration kommandofilen:</span><span class="sxs-lookup"><span data-stu-id="08394-159">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
