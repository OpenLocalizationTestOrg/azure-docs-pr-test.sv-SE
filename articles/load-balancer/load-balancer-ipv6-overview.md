---
title: "aaaOverview IPv6 för Azure-belastningsutjämnaren | Microsoft Docs"
description: "Så här fungerar IPv6-stöd för Azure belastningsutjämnare och belastningsutjämnade virtuella datorer."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5b203f77d86cc1ad455f4ebb297097aef46b658d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Översikt över IPv6 för Azure belastningsutjämnare

Internetriktade belastningsutjämnare kan distribueras med en IPv6-adress. Dessutom tooIPv4 anslutningen detta gör att hello följande funktioner:

* Intern slutpunkt till slutpunkt IPv6-anslutning mellan offentliga Internet-klienter och Azure virtuella datorer (VM) via hello belastningsutjämnaren.
* Intern slutpunkt till slutpunkt IPv6 utgående anslutningar mellan virtuella datorer och offentliga Internet IPv6-aktiverade klienter.

hello följande bild illustrerar hello IPv6-funktioner för Azure-belastningsutjämnaren.

![Azure belastningsutjämnare med IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

När har distribuerats, kan en IPv4- eller IPv6-aktiverad Internet-klient kommunicera med hello offentliga IPv4- eller IPv6-adresser (eller värdnamn) av hello Azure Internetriktade belastningsutjämnare. hello belastningen belastningsutjämnaren vägar hello IPv6-paket toohello privata IPv6-adresser för hello virtuella datorer med hjälp av network address translation (NAT). hello IPv6-Internet-klienten inte kan kommunicera direkt med hello IPv6-adressen för hello virtuella datorer.

## <a name="features"></a>Funktioner

Inbyggt IPv6-stöd för virtuella datorer som distribueras via Azure Resource Manager innehåller:

1. Utjämning av nätverksbelastning IPv6-tjänster för IPv6-klienter i hello Internet
2. Inbyggd IPv6 och IPv4-slutpunkterna på virtuella datorer (”dubbla Staplad”)
3. Inkommande och utgående initieras inbyggda IPv6-anslutningar
4. Protokoll som stöds till exempel TCP, UDP- och HTTP (S) Aktivera en mängd olika arkitekturer för tjänsten

## <a name="benefits"></a>Fördelar

Den här funktionen möjliggör hello följande viktiga fördelar:

* Uppfyller offentliga regleringar som kräver att nya program är tillgängligt endast tooIPv6-klienter
* Aktivera mobil- och Internet av saker (IOT) utvecklare toouse dubbla Staplad (IPv4 + IPv6) Azure Virtual Machines tooaddress hello växande mobile & IOT marknader

## <a name="details-and-limitations"></a>Information och begränsningar

Information

* hello Azure DNS-tjänsten innehåller poster för både IPv4-A- och IPv6-AAAA namn och svarar med båda posterna för hello belastningsutjämnaren. hello klienten väljer vilka adress (IPv4 eller IPv6) toocommunicate med.
* När en virtuell dator initierar anslutning tooa offentliga Internet IPv6-anslutna enheter, hello VM källa IPv6-adress som är nätverksadress översättas (NAT) toohello offentliga IPv6-adressen för hello belastningsutjämnaren.
* Virtuella datorer som kör hello Linux-operativsystem måste vara konfigurerade tooreceive en IPv6-IP-adress via DHCP. Många av hello Linux bilder i hello Azure-galleriet redan är konfigurerad toosupport IPv6 utan modifiering. Mer information finns i [konfigurera DHCPv6 för virtuella Linux-datorer](load-balancer-ipv6-for-linux.md)
* Om du väljer att skapa en IPv4-avsökning toouse en avsökning med din belastningsutjämnare och använda den med både hello IPv4 och IPv6-slutpunkter. Om hello-tjänsten på den virtuella datorn stängs av, tas både hello IPv4 och IPv6-slutpunkter bort från rotationen.

Begränsningar

* Du kan inte lägga till IPv6-belastningsutjämningsregler i hello Azure-portalen. hello-regler kan endast skapas med hello mall CLI, PowerShell.
* Du kan inte uppgradera befintlig VMs toouse IPv6-adresser. Du måste distribuera nya virtuella datorer.
* En IPv6-adress kan tilldelas tooa nätverksgränssnitt på varje virtuell dator.
* hello offentliga IPv6-adresser kan inte tilldelas tooa VM. De kan endast tilldelas tooa belastningsutjämnaren.
* Du kan konfigurera hello omvänd DNS-sökning för din offentliga IPv6-adresser.
* hello virtuella datorer med hello IPv6-adresser kan inte vara medlemmar i en Azure-molntjänst. De kan anslutna tooan Azure Virtual Network (VNet) och kommunicera med varandra via sina IPv4-adresser.
* Privat IPv6-adresser kan distribueras på enskilda virtuella datorer i en resursgrupp, men kan inte distribueras i en resursgrupp via Skaluppsättningar.
* Virtuella Azure-datorer kan inte ansluta över IPv6 tooother virtuella datorer, andra Azure-tjänster eller lokala enheter. De kan endast kommunicera med hello Azure belastningsutjämnare via IPv6. De kan dock kommunicera med andra resurserna med IPv4.
* Nätverkssäkerhetsgrupp (NSG)-skydd för IPv4 stöds i dual stack (IPv4 + IPv6) distributioner. NSG: er gäller inte toohello IPv6-slutpunkter.
* hello IPv6-slutpunkten på hello virtuella datorn inte är direkt exponerade toohello internet. Det är bakom en belastningsutjämnare. Endast hello portar som angetts i hello regler för inläsning av belastningsutjämnaren är tillgänglig via IPv6.
* Ändra hello IdleTimeout parametern för IPv6 är **stöds för närvarande inte**. hello standardvärdet är fyra minuter.
* Ändra hello loadDistributionMethod parameter för IPv6 är **stöds för närvarande inte**.
* Reserverad IP-IPv6-adresser (där ipallocationmethod inställt = statisk) är **stöds för närvarande inte**.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toodeploy en belastningsutjämnare med IPv6.

* [Tillgängligheten för IPv6 efter region](https://go.microsoft.com/fwlink/?linkid=828357)
* [Distribuera en belastningsutjämnare med IPv6 med en mall](load-balancer-ipv6-internet-template.md)
* [Distribuera en belastningsutjämnare med IPv6 med hjälp av Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Distribuera en belastningsutjämnare med IPv6 med Azure CLI](load-balancer-ipv6-internet-cli.md)
