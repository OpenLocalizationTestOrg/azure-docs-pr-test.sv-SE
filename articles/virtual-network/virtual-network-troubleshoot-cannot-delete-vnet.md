---
title: "aaaCannot ta bort ett virtuellt nätverk i Azure | Microsoft Docs"
description: "Lär dig hur tootroubleshoot hello problem som du inte kan ta bort ett virtuellt nätverk i Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a>Felsöka: Det gick inte toodelete ett virtuellt nätverk i Azure

Du kan råka ut för fel vid försök toodelete ett virtuellt nätverk i Microsoft Azure. Den här artikeln innehåller felsökning steg toohelp du lösa problemet. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Felsökningsanvisningar 

1. [Kontrollera om en virtuell nätverksgateway körs i virtuella nätverk för hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Kontrollera om en Programgateway körs i virtuella nätverk för hello](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Kontrollera om Azure Active Directory Domain Service har aktiverats i det virtuella nätverket för hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Kontrollera om hello virtuellt nätverk är anslutna tooother resurs](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Kontrollera om en virtuell dator körs fortfarande i hello virtuellt nätverk](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Kontrollera om hello virtuellt nätverk har fastnat i migrering](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Felsökningssteg

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a>Kontrollera om en virtuell nätverksgateway körs i hello virtuellt nätverk

tooremove hello virtuellt nätverk, måste du först ta bort hello virtuell nätverksgateway.

Klassiska virtuella nätverk finns i toohello **översikt** sidan av hello klassiska virtuella nätverk i hello Azure-portalen. I hello **VPN-anslutningar** avsnittet, om hello gateway körs i hello virtuellt nätverk ser du hello IP-adressen för hello gateway. 

![Kontrollera om gatewayen körs](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Virtuella nätverk finns i toohello **översikt** sidan av hello virtuella nätverk. Kontrollera **anslutna enheter** för hello virtuell nätverksgateway.

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Innan du tar bort hello gateway först ta bort alla **anslutning** objekt i hello gateway. 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a>Kontrollera om en Programgateway körs i hello virtuellt nätverk

Gå toohello **översikt** sidan av hello virtuella nätverk. Kontrollera hello **anslutna enheter** för hello Programgateway.

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Om det finns en Programgateway måste du ta bort det innan du kan ta bort hello virtuellt nätverk.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a>Kontrollera om Azure Active Directory Domain Service har aktiverats i hello virtuellt nätverk

Om hello Active Directory Domain Service är aktiverade och anslutna toohello virtuella nätverk, kan du ta bort det här virtuella nätverket. 

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

toodisable Hej tjänsten, gör du följande:

1. Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. I hello vänster och välj **Active Directory**.
3. Välj hello Azure Active Directory (AD Azure) katalog med Active Directory Domain Service aktiverat.
4. Välj hello **konfigurera** fliken.
5. Under **domäntjänster**, ändra hello **aktivera domain services för den här katalogen** alternativet för**nr**.  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a>Kontrollera om hello virtuellt nätverk är anslutna tooother resurs

Sök efter krets länkar, anslutningar och peerkopplingar mellan virtuella nätverk. Dessa kan orsaka en toofail för borttagning av virtuellt nätverk. 

hello är rekommenderade borttagning ordning följande:

1. Gateway-anslutningar
2. Gateways
3. IP-adresser
4. Peerkopplingar mellan virtuella nätverk
5. Apptjänst-miljö (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a>Kontrollera om en virtuell dator körs fortfarande i hello virtuellt nätverk

Se till att ingen virtuell dator är i hello virtuella nätverk.

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a>Kontrollera om hello virtuellt nätverk har fastnat i migrering

Om hello virtuellt nätverk har fastnat i migreringstillståndet, kan inte tas bort. Kör hello efter kommandot tooabort hello migreringen och tar bort hello virtuellt nätverk.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Nästa steg

- [Azure Virtual Network](virtual-networks-overview.md)
- [Vanliga frågor och svar (FAQ) om Azure Virtual Network](virtual-networks-faq.md)