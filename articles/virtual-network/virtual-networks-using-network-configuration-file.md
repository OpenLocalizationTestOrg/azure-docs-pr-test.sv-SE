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
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverk
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Du kan skapa och konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket med hjälp av hello Azure-kommandoradsgränssnittet (CLI) 1.0 eller Azure PowerShell. Du kan inte skapa eller ändra ett virtuellt nätverk med hello Azure Resource Manager-distributionsmodellen med en konfigurationsfil för nätverket. Du kan inte använda hello Azure portal toocreate eller ändra ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket, men du kan använda hello Azure portal toocreate ett virtuellt nätverk (klassiskt), utan att använda en konfigurationsfil för nätverket.

Skapa och konfigurera ett virtuellt nätverk (klassiska) med en konfigurationsfil för nätverket kräver exportera, ändra och importera hello-fil.

## <a name="export"></a>Exportera en konfigurationsfil för nätverk

Du kan använda PowerShell eller hello Azure CLI tooexport en konfigurationsfil för nätverket. PowerShell exporterar en XML-fil, medan hello Azure CLI exporterar en json-fil.

### <a name="powershell"></a>PowerShell
 
1. [Installera Azure PowerShell och logga in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Ändra hello katalogen (och se till att den finns) och filnamnet i hello följande kommando som önskad och sedan kör hello kommandot tooexport hello nätverket konfigurationsfil:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Installera hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Slutför hello återstående steg från en kommandotolk med Azure CLI 1.0.
2. Logga in tooAzure genom att ange hello `azure login` kommando.
3. Kontrollera att du befinner dig i asm-läge genom att ange hello `azure config mode asm` kommando.
4. Ändra hello katalogen (och se till att den finns) och filnamnet i hello följande kommando som önskad och sedan kör hello kommandot tooexport hello nätverket konfigurationsfil:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Skapa eller ändra en konfigurationsfil för nätverk

En konfigurationsfil för nätverk är en XML-fil (när du använder PowerShell) eller en json-fil (när du använder hello Azure CLI). Du kan redigera hello-fil i en text, eller XML/json-redigerare. Hej [nätverk konfigurationsinställningar filen schemat](https://msdn.microsoft.com/library/azure/jj157100.aspx) artikeln innehåller information om alla inställningar. Ytterligare förklaring av hello inställningar finns [Visa inställningar för virtuella nätverk och](virtual-network-manage-network.md#view-vnet). hello ändringarna toohello fil:

- Måste vara kompatibel med hello schemat eller importera hello nätverk konfigurationsfilen misslyckas.
- Skriv över alla befintliga nätverksinställningarna för din prenumeration, så mycket försiktig när du gör ändringar. Referera exempelvis hello exempel network configuration-filer som följer. Säg hello originalfilen finns två **VirtualNetworkSite** instanser och du ändrade den som visas i hello exempel. När du importerar filen hello Azure tar bort hello virtuellt nätverk för hello **VirtualNetworkSite** instans som du har tagit bort i hello-filen. Det här förenklad scenariot förutsätter att inga resurser har i hello virtuella nätverk, som om det fanns hello virtuellt nätverk kunde inte tas bort och hello import skulle misslyckas.

> [!IMPORTANT]
> Azure tar hänsyn till ett undernät som har något distribueras tooit som **används**. När ett undernät kan inte ändras. Flytta allt som du har distribuerat toohello undernät tooa annat undernät som inte ändras innan du ändrar information om undernät i en konfigurationsfil för nätverket. Se [flytta en virtuell dator eller Rollinstans tooa annat undernät](virtual-networks-move-vm-role-to-subnet.md) mer information.

### <a name="example-xml-for-use-with-powershell"></a>XML-exempel för användning med PowerShell

hello följande exempel nätverket konfigurationsfil skapar ett virtuellt nätverk med namnet *myVirtualNetwork* med ett adressutrymme för *10.0.0.0/16* i hello *östra USA* Azure region. hello virtuellt nätverk innehåller ett undernät med namnet *mySubnet* med en adressprefixet *10.0.0.0/24*.

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

Om hello nätverket konfigurationsfil som du exporterade innehåller inget innehåll, kan du kopiera hello XML i hello föregående exempel och klistra in den i en ny fil.

### <a name="example-json-for-use-with-hello-azure-cli-10"></a>Exempel JSON för användning med hello Azure CLI 1.0

hello följande exempel nätverket konfigurationsfil skapar ett virtuellt nätverk med namnet *myVirtualNetwork* med ett adressutrymme för *10.0.0.0/16* i hello *östra USA* Azure region. hello virtuellt nätverk innehåller ett undernät med namnet *mySubnet* med en adressprefixet *10.0.0.0/24*.

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

Om hello nätverket konfigurationsfil som du exporterade innehåller inget innehåll, kan du kopiera hello json i hello föregående exempel och klistra in den i en ny fil.

## <a name="import"></a>Importera en konfigurationsfil för nätverk

Du kan använda PowerShell eller hello Azure CLI tooimport en konfigurationsfil för nätverket. PowerShell importerar en XML-fil, medan hello Azure CLI importerar en json-fil. Om det inte går att importera hello, bekräfta hello filen uppfyller hello [nätverk Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Installera Azure PowerShell och logga in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Ändra hello katalog och ett filnamn i hello följande kommando vid behov och kör sedan hello tooimport hello network configuration kommandofilen:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Installera hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Slutför hello återstående steg från en kommandotolk med Azure CLI 1.0.
2. Logga in tooAzure genom att ange hello `azure login` kommando.
3. Kontrollera att du befinner dig i asm-läge genom att ange hello `azure config mode asm` kommando.
4. Ändra hello katalog och ett filnamn i hello följande kommando vid behov och kör sedan hello tooimport hello network configuration kommandofilen:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
