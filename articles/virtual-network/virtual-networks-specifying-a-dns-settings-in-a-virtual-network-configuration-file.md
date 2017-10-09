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
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Ange DNS-inställningar i en konfigurationsfil för virtuellt nätverk
En konfigurationsfil för nätverk innehåller två element som du kan använda toospecify Domain Name System (DNS)-inställningar: **DnsServers** och **DnsServerRef**. Du kan lägga till en lista över DNS-servrar genom att ange sina IP-adresser och referera namn toohello **DnsServers** element. Du kan sedan använda en **DnsServerRef** elementet toospecify vilka DNS-server transaktioner från hello DnsServers element som används för olika nätverksplatserna i det virtuella nätverket.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello klassiska distributionsmodellen.

hello nätverket konfigurationsfilen kan innehålla hello följande element. hello titeln på varje element är länkade tooa sida som innehåller ytterligare information om hello elementet värdeinställningarna.

> [!IMPORTANT]
> Information om hur tooconfigure hello nätverket konfigurationsfilen finns [konfigurera ett virtuellt nätverk med en konfigurationsfil för nätverket](virtual-networks-using-network-configuration-file.md). Information om varje element som finns i konfigurationsfilen för hello nätverket finns [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> Hej **namn** attribut i hello **DnsServer** elementet används endast som en referens för hello **DnsServerRef** element. Det är inte ett hello värdnamn för hello DNS-server. Varje **DnsServer** attributvärdet måste vara unikt inom hello hela Microsoft Azure-prenumeration
> 
> 

[Virtuellt nätverk platser Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Order toospecify inställningen för hello virtuella nätverksplatser element, tidigare måste definieras i hello DNS-element. Hej DnsServerRef *namn* i hello virtuella nätverksplatser elementet måste referera tooa namn-värde som angetts i hello DNS-element för DnsServer *namn*.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Förstå hello [Azure Virtual Network Konfigurationsschemat](http://go.microsoft.com/fwlink/?LinkId=248093).
* Förstå hello [Azure Service Konfigurationsschemat](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Konfigurera ett virtuellt nätverk som använder nätverket konfigurationsfiler](virtual-networks-using-network-configuration-file.md).

