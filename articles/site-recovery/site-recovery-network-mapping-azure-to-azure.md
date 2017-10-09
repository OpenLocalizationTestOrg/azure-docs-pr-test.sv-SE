---
title: "aaaNetwork mappning mellan två Azure-regioner i Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer och fysiska servrar. Läs mer om tooAzure för växling vid fel eller ett sekundärt datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Nätverksmappningen mellan två Azure-regioner


Den här artikeln beskriver hur toomap Azure virtuella nätverk för två Azure-regioner med varandra. Nätverksmappning garanterar att den replikerade virtuella datorn skapas i mål-hello Azure-region, även skapas på hello virtuellt nätverk som är mappade toovirtual nätverk av hello virtuella källdatorn.  

## <a name="prerequisites"></a>Krav
Innan du mappa nätverk gör att du har skapat [virtuella Azure-nätverk](../virtual-network/virtual-networks-overview.md) i både källa och mål-Azure-regioner.

## <a name="map-networks"></a>Mappa nätverk

toomap virtuellt Azure-nätverk i en Azure-region tooanother virtuellt nätverk i en annan region, gå tooSite Recovery-infrastruktur -> nätverk-mappning (för Azure virtuella datorer) och skapa en nätverksmappning.

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Exemplet nedan min virtuella dator körs i Östasien region och håller på att replikeras i hello tooSoutheast Asien.

Välj hello käll- och nätverk och klicka sedan på OK toocreate nätverksmappning från Östasien tooSoutheast Asien.

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Hello samma sak toocreate nätverksmappning från Sydostasien tooEast Asien.  
![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Mappa nätverk när du aktiverar replikering

Om nätverksmappning inte görs när du replikerar en virtuell dator för hello första gången formuläret en Azure-region tooanother kan du välja målnätverket som en del av hello samma process. Nätverksmappningar skapar site Recovery från källan region tootarget region och region toosource målregionen baserat på det här valet.   

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Som standard skapar Site Recovery ett nätverk i hello målregionen som är identiska toohello källnätverket och genom att lägga till ”-asr' som en suffix toohello namnet på hello Källnätverk. Du kan välja ett nätverk som redan har skapat genom att klicka på Anpassa.

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Om hello nätverksmappning redan har gjort kan du inte ändra virtuella hello målnätverket vid replikering. toochange, ändra befintlig nätverksmappning.  

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Om du ändrar en nätverksmappning från region-1 tooregion-2, kontrollera att du ändrar hello nätverksmappning från region-2-tooregion-1 samt.
>
>


## <a name="subnet-selection"></a>Val av undernät
Undernätet för hello virtuella måldatorn har valts baserat på hello namn för hello undernätet för hello virtuella källdatorn. Om det finns ett undernät för hello namn samma som hello virtuella källdatorn finns i hello målnätverket och sedan som väljs för hello virtuella måldatorn. Om det finns inget undernät med hello namn samma i hello målnätverket och sedan alfabetiskt första undernätet är valt som hello mål undernät. Du kan ändra det här undernätet genom att gå tooCompute och nätverksinställningarna för hello virtuell dator.

![Ändra ett undernät](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-adress

IP-adress för varje hello nätverksgränssnittet för hello virtuella måldatorn är valt på följande sätt:

### <a name="dhcp"></a>DHCP
Om hello nätverksgränssnittet för hello virtuella källdatorn använder DHCP, anges också nätverksgränssnittet för hello virtuella måldatorn som DHCP.

### <a name="static-ip"></a>Statisk IP-adress
Om hello nätverksgränssnittet för hello virtuella källdatorn använder statisk IP-adress, anges toouse statiska IP-Adressen också i nätverksgränssnittet för hello virtuella måldatorn. Statiska IP-Adressen är valt på följande sätt:

#### <a name="same-address-space"></a>Samma-adressutrymme

Om hello käll-undernät och undernät för hello mål har hello adressutrymmet samma mål-IP anges samma som hello IP för hello gränssnitt på hello virtuella källdatorn. Om samma IP inte är tillgänglig, ange vissa tillgängliga IP som hello mål-IP.

#### <a name="different-address-space"></a>Olika adressutrymmen

Om hello käll-undernät och undernät för hello mål har olika adressutrymmen, ange mål-IP som alla tillgängliga IP-adresser i hello mål undernät.

Du kan ändra hello mål-IP-adress på varje gränssnitt genom att gå tooCompute och nätverksinställningarna för hello virtuell dator.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).
