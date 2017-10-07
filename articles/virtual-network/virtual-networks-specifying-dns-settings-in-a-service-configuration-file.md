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
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Ange DNS-inställningar i en konfigurationsfil för tjänsten
## <a name="dns-elements"></a>DNS-element
En konfigurationsfil för tjänsten kan innehålla ett DnsServers-element med en lista över IPv4-adresser för hello Domain Name System (DNS)-servrar som ska användas hello-tjänsten. Inställningarna i konfigurationsfilen för hello service företräde framför inställningar i konfigurationsfilen för hello nätverk. Mer information finns i [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-elementet**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Hej **namn** attribut i hello **DnsServer** elementet används endast som en referensnamn. Det är inte ett hello värdnamn för hello DNS-server. Varje **DnsServer** attributvärdet måste vara unikt inom hello hela Microsoft Azure-prenumeration.
> 
> 

## <a name="see-also"></a>Se även
[Konfigurationsschemat för Azure-tjänsten (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Virtuella Azure-nätverket Konfigurationsschemat](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurera ett virtuellt nätverk med Network Configuration-filer](http://go.microsoft.com/fwlink/?LinkId=248094)

[Om inställningar för virtuellt nätverk i hello-hanteringsportalen](http://go.microsoft.com/fwlink/?LinkId=248092)

