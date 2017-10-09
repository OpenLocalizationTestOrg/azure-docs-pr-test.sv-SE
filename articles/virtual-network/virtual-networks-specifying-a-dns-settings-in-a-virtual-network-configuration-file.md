---
title: "aaaSpecifying DNS-inställningarna i en konfigurationsfil för virtuellt nätverk | Microsoft Docs"
description: "Hur toochange DNS-serverinställningarna i ett virtuellt nätverk med en konfiguration av virtuellt nätverk filen i hello klassiska distributionsmodellen"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: d53a658773e1c930b5a28a701db0be9edd26565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a><span data-ttu-id="12912-103">Ange DNS-inställningar i en konfigurationsfil för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="12912-103">Specifying DNS settings in a virtual network configuration file</span></span>
<span data-ttu-id="12912-104">En konfigurationsfil för nätverk innehåller två element som du kan använda toospecify Domain Name System (DNS)-inställningar: **DnsServers** och **DnsServerRef**.</span><span class="sxs-lookup"><span data-stu-id="12912-104">A network configuration file has two elements that you can use toospecify Domain Name System (DNS) settings: **DnsServers** and **DnsServerRef**.</span></span> <span data-ttu-id="12912-105">Du kan lägga till en lista över DNS-servrar genom att ange sina IP-adresser och referera namn toohello **DnsServers** element.</span><span class="sxs-lookup"><span data-stu-id="12912-105">You can add a list of DNS servers by specifying their IP addresses and reference names toohello **DnsServers** element.</span></span> <span data-ttu-id="12912-106">Du kan sedan använda en **DnsServerRef** elementet toospecify vilka DNS-server transaktioner från hello DnsServers element som används för olika nätverksplatserna i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="12912-106">You can then use a **DnsServerRef** element toospecify which DNS server entries from hello DnsServers element are used for different network sites within your virtual network.</span></span>

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="12912-107">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="12912-107">This article covers hello classic deployment model.</span></span>

<span data-ttu-id="12912-108">hello nätverket konfigurationsfilen kan innehålla hello följande element.</span><span class="sxs-lookup"><span data-stu-id="12912-108">hello network configuration file may contain hello following elements.</span></span> <span data-ttu-id="12912-109">hello titeln på varje element är länkade tooa sida som innehåller ytterligare information om hello elementet värdeinställningarna.</span><span class="sxs-lookup"><span data-stu-id="12912-109">hello title of each element is linked tooa page that provides additional information about hello element value settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12912-110">Information om hur tooconfigure hello nätverket konfigurationsfilen finns [konfigurera ett virtuellt nätverk med en konfigurationsfil för nätverket](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="12912-110">For information about how tooconfigure hello network configuration file, see [Configure a Virtual Network Using a Network Configuration File](virtual-networks-using-network-configuration-file.md).</span></span> <span data-ttu-id="12912-111">Information om varje element som finns i konfigurationsfilen för hello nätverket finns [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="12912-111">For information about each element contained in hello network configuration file, see [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
> 
> 

[<span data-ttu-id="12912-112">DNS-Element</span><span class="sxs-lookup"><span data-stu-id="12912-112">Dns Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> <span data-ttu-id="12912-113">Hej **namn** attribut i hello **DnsServer** elementet används endast som en referens för hello **DnsServerRef** element.</span><span class="sxs-lookup"><span data-stu-id="12912-113">hello **name** attribute in hello **DnsServer** element is used only as a reference for hello **DnsServerRef** element.</span></span> <span data-ttu-id="12912-114">Det är inte ett hello värdnamn för hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="12912-114">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="12912-115">Varje **DnsServer** attributvärdet måste vara unikt inom hello hela Microsoft Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="12912-115">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription</span></span>
> 
> 

[<span data-ttu-id="12912-116">Virtuellt nätverk platser Element</span><span class="sxs-lookup"><span data-stu-id="12912-116">Virtual Network Sites Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> <span data-ttu-id="12912-117">Order toospecify inställningen för hello virtuella nätverksplatser element, tidigare måste definieras i hello DNS-element.</span><span class="sxs-lookup"><span data-stu-id="12912-117">In order toospecify this setting for hello Virtual Network Sites element, it must be previously defined in hello DNS element.</span></span> <span data-ttu-id="12912-118">Hej DnsServerRef *namn* i hello virtuella nätverksplatser elementet måste referera tooa namn-värde som angetts i hello DNS-element för DnsServer *namn*.</span><span class="sxs-lookup"><span data-stu-id="12912-118">hello DnsServerRef *name* in hello Virtual Network Sites element must refer tooa name value specified in hello DNS element for DnsServer *name*.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="12912-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12912-119">Next steps</span></span>
* <span data-ttu-id="12912-120">Förstå hello [Azure Virtual Network Konfigurationsschemat](http://go.microsoft.com/fwlink/?LinkId=248093).</span><span class="sxs-lookup"><span data-stu-id="12912-120">Understand hello [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).</span></span>
* <span data-ttu-id="12912-121">Förstå hello [Azure Service Konfigurationsschemat](https://msdn.microsoft.com/library/windowsazure/ee758710).</span><span class="sxs-lookup"><span data-stu-id="12912-121">Understand hello [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).</span></span>
* <span data-ttu-id="12912-122">[Konfigurera ett virtuellt nätverk som använder nätverket konfigurationsfiler](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="12912-122">[Configure a virtual network using Network configuration files](virtual-networks-using-network-configuration-file.md).</span></span>

