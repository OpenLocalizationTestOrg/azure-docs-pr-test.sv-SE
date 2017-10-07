---
title: "Azure AD Domain Services: Nätverk riktlinjer | Microsoft Docs"
description: "Överväganden för nätverk för Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: maheshu
ms.openlocfilehash: 804d4ea7d1b3b07b6d224855c7adb90bdfe24022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Överväganden för nätverk för Azure AD Domain Services
## <a name="how-tooselect-an-azure-virtual-network"></a>Hur tooselect virtuellt Azure-nätverk
hello hjälpa följande riktlinjer dig att välja ett virtuellt nätverk toouse med Azure AD Domain Services.

### <a name="type-of-azure-virtual-network"></a>Typ av virtuella Azure-nätverket
* Du kan aktivera Azure AD Domain Services i Azure klassiska virtuella nätverk.
* Azure AD Domain Services **kan inte aktiveras i virtuella nätverk som skapats med Azure Resource Manager**.
* Du kan ansluta en Resource Manager-baserade virtuella nätverk tooa klassiskt virtuellt nätverk i vilka Azure AD Domain Services är aktiverad. Därefter kan du använda Azure AD Domain Services i hello Resource Manager-baserade virtuella nätverk. Mer information finns i hello [nätverksanslutningar](active-directory-ds-networking.md#network-connectivity) avsnitt.
* **Regionala virtuella nätverk**: Om du planerar toouse ett befintligt virtuellt nätverk kontrollerar du att det är ett regionalt virtuellt nätverk.

  * Virtuella nätverk som använder hello mekanismen för äldre tillhörighetsgrupper kan inte användas med Azure AD Domain Services.
  * toouse Azure AD Domain Services [migrera äldre virtuella nätverk tooregional virtuella nätverk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-hello-virtual-network"></a>Azure-region för hello virtuellt nätverk
* Din Azure AD Domain Services hanterad domän som har distribuerats i hello samma Azure-region som hello virtuella nätverk som du väljer tooenable hello tjänst i.
* Välj ett virtuellt nätverk i en Azure-region som stöds av Azure AD Domain Services.
* Se hello [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/) sidan tooknow hello Azure-regioner som Azure AD Domain Services är tillgängligt.

### <a name="requirements-for-hello-virtual-network"></a>Krav för hello virtuellt nätverk
* **Närhet tooyour Azure arbetsbelastningar**: Välj hello virtuella nätverk som för närvarande är värd för/ska vara värd för virtuella datorer som behöver komma åt tooAzure AD DS.
* **Anpassad/sätta egna DNS-servrar**: se till att det inte finns några anpassade DNS-servrar som konfigurerats för hello virtuellt nätverk.
* **Befintliga domäner med hello samma domännamn**: Kontrollera att du inte har en befintlig domän med hello samma domännamn i det virtuella nätverket. Anta exempelvis att du har en domän som heter ”contoso.com” redan finns på hello valda virtuella nätverket. Senare kan du försöka tooenable en Azure AD Domain Services-hanterad domän med hello samma domännamn (som ”contoso.com”) i det virtuella nätverket. Det uppstår ett fel vid försök tooenable Azure AD Domain Services. Det här felet är på grund av tooname konflikter för hello domännamn i det virtuella nätverket. I den här situationen måste du använda ett annat namn tooset in din Azure AD Domain Services-hanterad domän. Du kan också avetablera hello befintlig domän och gå sedan vidare tooenable Azure AD Domain Services.

> [!WARNING]
> Du kan inte flytta Domain Services tooa annat virtuellt nätverk när du har aktiverat hello-tjänsten.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Nätverkssäkerhetsgrupper och undernät
En [Nätverkssäkerhetsgrupp (NSG)](../virtual-network/virtual-networks-nsg.md) innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik tooyour VM-instanser i ett virtuellt nätverk. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet. Dessutom kan trafik tooan enskild VM begränsas ytterligare genom att koppla en NSG direkt toothat VM.

![Rekommenderade undernät](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Metodtips för att välja ett undernät
* Distribuera Azure AD Domain Services tooa **separata dedikerade undernät** i ditt virtuella Azure-nätverket.
* Använd inte NSG: er toohello särskilda undernät för din hanterade domän. Om du måste tillämpa NSG: er toohello särskilda undernät, se till att du **inte blockera hello portar krävs tooservice och hantera din domän**.
* Begränsa inte hello antalet IP-adresser som är tillgängliga i hello särskilda undernät för din hanterade domän alltför. Den här begränsningen förhindrar hello tjänsten gör två domänkontrollanter som är tillgängliga för din hanterade domän.
* **Aktivera inte Azure AD Domain Services i hello gateway-undernätet** av det virtuella nätverket.

> [!WARNING]
> När du kopplar en NSG med ett undernät som Azure AD Domain Services är aktiverat, du kan störa Microsofts möjlighet tooservice och hantera hello domän. Dessutom avbryts synkronisering mellan Azure AD-klienten och din hanterade domän. **hello SLA gäller inte toodeployments om en NSG har tillämpats som blockerar Azure AD Domain Services från uppdatering och hantering av din domän.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Portar som krävs för Azure AD Domain Services
hello följande portar krävs för Azure AD Domain Services tooservice och underhålla din hanterade domän. Kontrollera att portarna inte är blockerade för hello undernät som du har aktiverat din hanterade domän.

| Portnummer | Syfte |
| --- | --- |
| 443 |Synkronisering med Azure AD-klient |
| 3389 |Hantering av din domän |
| 5986 |Hantering av din domän |
| 636 |Säkert LDAP (LDAPS) åtkomst tooyour hanterad domän |

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Exempel NSG för virtuella nätverk med Azure AD Domain Services
hello följande tabell visar ett exempel på en NSG som du kan konfigurera för ett virtuellt nätverk med en Azure AD Domain Services-hanterad domän. Den här regeln tillåter inkommande trafik från hello ovan angivna portar tooensure din hanterade domän förblir korrigeras, uppdateras och kan övervakas av Microsoft. hello 'DenyAll' Standardregeln gäller tooall andra inkommande trafik från hello internet.

Hello NSG visas dessutom också hur hello toolock ned säker LDAP åtkomst via internet. Hoppa över den här regeln om du inte har aktiverat säker LDAP åtkomst tooyour hanterade domänen via hello internet. hello NSG innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser. hello NSG regeln tooallow LDAPS åtkomst via hello internet från den angivna IP-adresser har högre prioritet än hello DenyAll NSG regeln.

![Exempel NSG toosecure LDAPS åtkomst via hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Mer information** - [skapar en Nätverkssäkerhetsgrupp](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Nätverksanslutning
En Azure AD Domain Services-hanterad domän kan aktiveras endast inom ett enda klassiska virtuella nätverk i Azure. Virtuella nätverk som skapats med Azure Resource Manager stöds inte.

### <a name="scenarios-for-connecting-azure-networks"></a>Scenarier för att ansluta Azure-nätverk
Anslut virtuella Azure-nätverk toouse hello-hanterad domän i något av följande distributionsscenarier hello:

#### <a name="use-hello-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Använd hello hanterade domän i mer än ett Azure klassiska virtuella nätverk
Du kan ansluta andra Azure klassiska virtuella nätverk toohello Azure klassiska virtuella nätverk som du har aktiverat Azure AD Domain Services. Den här VPN-anslutningen kan du toouse hello hanterade domänen med din arbetsbelastning distribueras på andra virtuella nätverk.

![Anslutningar för klassiska virtuella nätverk](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-hello-managed-domain-in-a-resource-manager-based-virtual-network"></a>Använd hello-hanterad domän i en Resource Manager-baserade virtuella nätverk
Du kan ansluta en Resource Manager-baserade virtuella nätverk toohello Azure klassiska virtuella nätverk som du har aktiverat Azure AD Domain Services. Den här anslutningen kan du toouse hello hanterade domänen med dina arbetsbelastningar som distribuerats i hello Resource Manager-baserade virtuella nätverk.

![Hanteraren för filserverresurser tooclassic virtuell nätverksanslutning](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Alternativ för nätverksanslutning
* **VNet-till-VNet-anslutningar med plats-till-plats VPN-anslutningar**: ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett virtuellt nätverk tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE.

    ![Anslutningar för virtuella nätverk med hjälp av VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Mer information – ansluta virtuella nätverk som använder VPN-gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **VNet-till-VNet-anslutningar med virtuella nätverk peering**: peering virtuellt nätverk är en mekanism som ansluter två virtuella nätverk i hello samma region via hello Azure stamnät nätverk. När peerkoppla, visas hello två virtuella nätverk som en för alla ändamål för anslutningen. De hanteras fortfarande som separata resurser, men virtuella datorer i dessa virtuella nätverk kan kommunicera med varandra direkt med hjälp av privata IP-adresser.

    ![Anslutningar för virtuella nätverk med hjälp av peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Mer information om-virtuella nätverk peering](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Relaterat innehåll
* [Virtuella Azure-nätverket peering](../virtual-network/virtual-network-peering-overview.md)
* [Konfigurera ett VNet-till-VNet-anslutning för hello klassiska distributionsmodellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Säkerhetsgrupper för Azure-nätverk](../virtual-network/virtual-networks-nsg.md)
* [Skapa en säkerhetsgrupp för nätverk](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
