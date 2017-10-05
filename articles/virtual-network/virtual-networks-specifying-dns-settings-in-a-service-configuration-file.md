---
title: "Ange DNS-inställningar i en konfigurationsfil för tjänsten | Microsoft Docs"
description: "Ange anpassade DNS-inställningar med hjälp av tjänstkonfigurationsfilen för det virtuella nätverket"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="28d00-103">Ange DNS-inställningar i en konfigurationsfil för tjänsten</span><span class="sxs-lookup"><span data-stu-id="28d00-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="28d00-104">DNS-element</span><span class="sxs-lookup"><span data-stu-id="28d00-104">DNS elements</span></span>
<span data-ttu-id="28d00-105">En konfigurationsfil för tjänsten kan innehålla ett DnsServers-element med en lista över IPv4-adresser för Domain Name System (DNS)-servrar som ska användas av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="28d00-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for the Domain Name System (DNS) servers that the service will use.</span></span> <span data-ttu-id="28d00-106">Inställningarna i tjänstkonfigurationsfilen företräde framför inställningar i konfigurationsfilen på nätverket.</span><span class="sxs-lookup"><span data-stu-id="28d00-106">Settings in the service configuration file take precedence over settings in the network configuration file.</span></span> <span data-ttu-id="28d00-107">Mer information finns i [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="28d00-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="28d00-108">**NetworkConfiguration-elementet**</span><span class="sxs-lookup"><span data-stu-id="28d00-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="28d00-109">Den **namn** attribut i den **DnsServer** elementet används endast som en referensnamn.</span><span class="sxs-lookup"><span data-stu-id="28d00-109">The **name** attribute in the **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="28d00-110">Det är inte ett värdnamn för DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="28d00-110">It does not represent the host name for the DNS server.</span></span> <span data-ttu-id="28d00-111">Varje **DnsServer** attributvärdet måste vara unikt i hela Microsoft Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="28d00-111">Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="28d00-112">Se även</span><span class="sxs-lookup"><span data-stu-id="28d00-112">See Also</span></span>
[<span data-ttu-id="28d00-113">Konfigurationsschemat för Azure-tjänsten (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="28d00-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="28d00-114">Virtuella Azure-nätverket Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="28d00-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="28d00-115">Konfigurera ett virtuellt nätverk med Network Configuration-filer</span><span class="sxs-lookup"><span data-stu-id="28d00-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="28d00-116">Om inställningarna för virtuella nätverk i hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="28d00-116">About Virtual Network settings in the Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

