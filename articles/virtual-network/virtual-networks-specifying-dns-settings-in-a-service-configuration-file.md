---
title: "aaaSpecifying DNS-inställningarna i en konfigurationsfil för tjänsten | Microsoft Docs"
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
ms.openlocfilehash: f192e33566dd8e669da04e6378a0c8e4b0b35ecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="7636e-103">Ange DNS-inställningar i en konfigurationsfil för tjänsten</span><span class="sxs-lookup"><span data-stu-id="7636e-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="7636e-104">DNS-element</span><span class="sxs-lookup"><span data-stu-id="7636e-104">DNS elements</span></span>
<span data-ttu-id="7636e-105">En konfigurationsfil för tjänsten kan innehålla ett DnsServers-element med en lista över IPv4-adresser för hello Domain Name System (DNS)-servrar som ska användas hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7636e-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for hello Domain Name System (DNS) servers that hello service will use.</span></span> <span data-ttu-id="7636e-106">Inställningarna i konfigurationsfilen för hello service företräde framför inställningar i konfigurationsfilen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="7636e-106">Settings in hello service configuration file take precedence over settings in hello network configuration file.</span></span> <span data-ttu-id="7636e-107">Mer information finns i [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="7636e-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="7636e-108">**NetworkConfiguration-elementet**</span><span class="sxs-lookup"><span data-stu-id="7636e-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="7636e-109">Hej **namn** attribut i hello **DnsServer** elementet används endast som en referensnamn.</span><span class="sxs-lookup"><span data-stu-id="7636e-109">hello **name** attribute in hello **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="7636e-110">Det är inte ett hello värdnamn för hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="7636e-110">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="7636e-111">Varje **DnsServer** attributvärdet måste vara unikt inom hello hela Microsoft Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7636e-111">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="7636e-112">Se även</span><span class="sxs-lookup"><span data-stu-id="7636e-112">See Also</span></span>
[<span data-ttu-id="7636e-113">Konfigurationsschemat för Azure-tjänsten (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="7636e-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="7636e-114">Virtuella Azure-nätverket Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="7636e-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="7636e-115">Konfigurera ett virtuellt nätverk med Network Configuration-filer</span><span class="sxs-lookup"><span data-stu-id="7636e-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="7636e-116">Om inställningar för virtuellt nätverk i hello-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="7636e-116">About Virtual Network settings in hello Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

