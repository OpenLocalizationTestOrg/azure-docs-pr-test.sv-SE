---
title: "aaaCreate en tillgänglighetsgruppslyssnare för SQL Server på virtuella Azure-datorer | Microsoft Docs"
description: "Stegvisa instruktioner för att skapa en lyssnare för en Always On-tillgänglighetsgrupp för SQL Server på virtuella Azure-datorer"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: c6a44dc5c7c18b572c2bf5772b4651b7210aacbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Konfigurera en belastningsutjämnare för en tillgänglighetsgrupp alltid på i Azure
Den här artikeln förklarar hur toocreate belastningsutjämning för en SQL Server Always On-tillgänglighetsgrupp i Azure virtuella datorer som körs med Azure Resource Manager. En tillgänglighetsgrupp kräver en belastningsutjämnare när hello SQL Server-instanser finns på virtuella Azure-datorer. hello belastningsutjämnaren lagrar hello IP-adress för hello tillgänglighetsgruppens lyssnare. Om en tillgänglighetsgrupp sträcker sig över flera regioner, måste en belastningsutjämnare i varje region.

toocomplete den här uppgiften måste toohave en tillgänglighetsgrupp för SQL Server distribueras på virtuella Azure-datorer som körs med Resource Manager. Både SQL Server-datorer måste tillhöra toohello samma tillgänglighetsuppsättning. Du kan använda hello [Microsoft mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically skapa hello tillgänglighetsgruppen i Resource Manager. Den här mallen skapas automatiskt en intern belastningsutjämnare. 

Om du vill kan du [manuellt konfigurera en tillgänglighetsgrupp](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Den här artikeln kräver att din Tillgänglighetsgrupper har redan konfigurerats.  

Närliggande ämnen innefattar:

* [Konfigurera alltid på Tillgänglighetsgrupper i Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Konfigurera en VNet-till-VNet-anslutning med hjälp av Azure Resource Manager och PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Gå igenom den här artikeln, skapa och konfigurera en belastningsutjämnare i hello Azure-portalen. När hello processen är klar kan du konfigurera hello toouse hello IP-klusteradress från hello belastningsutjämnaren för hello tillgänglighetsgruppens lyssnare.

## <a name="create-and-configure-hello-load-balancer-in-hello-azure-portal"></a>Skapa och konfigurera hello belastningsutjämnare i hello Azure-portalen
I den här delen av hello uppgiften hello följande:

1. Skapa hello belastningsutjämnaren hello Azure-portalen och konfigurera hello IP-adress.
2. Konfigurera hello backend-adresspool.
3. Skapa hello avsökningen. 
4. Ange hello regler för belastningsutjämning.

> [!NOTE]
> Om hello SQL Server-instanser i flera resursgrupper och regioner, utför varje steg två gånger, en gång i varje resursgrupp.
> 
> 

### <a name="step-1-create-hello-load-balancer-and-configure-hello-ip-address"></a>Steg 1: Skapa hello belastningsutjämnare och konfigurera hello IP-adress
Först skapa hello belastningsutjämnaren. 

1. Öppna hello resursgruppen som innehåller hello SQL Server-datorer i hello Azure-portalen. 

2. I hello resursgrupp, klickar du på **Lägg till**.

3. Sök efter **belastningsutjämnare** och markera i hello sökresultat **belastningsutjämnare**, som publiceras av **Microsoft**.

4. På hello **belastningsutjämnaren** bladet, klickar du på **skapa**.

5. I hello **skapa belastningsutjämnaren** dialogrutan Konfigurera hello belastningsutjämnaren på följande sätt:

   | Inställning | Värde |
   | --- | --- |
   | **Namn** |Ett namn som representerar hello belastningsutjämnaren. Till exempel **sqlLB**. |
   | **Typ** |**Internt**: använda en intern belastningsutjämnare, vilket gör att program inom hello för de flesta implementeringar av samma virtuella nätverk tooconnect toohello tillgänglighetsgruppen.  </br> **Externa**: gör att program tooconnect toohello tillgänglighetsgruppen via en offentlig Internetanslutning. |
   | **Virtuellt nätverk** |Välj hello virtuella nätverk som hello intances för SQL Server finns i. |
   | **Undernät** |Välj hello undernät som hello SQL Server-instanser. |
   | **IP-adresstilldelning** |**Statisk** |
   | **Privat IP-adress** |Ange en tillgänglig IP-adress från hello undernät. Använd följande IP-adress när du skapar en lyssnare på hello klustret. Använd den här adressen i ett PowerShell.skript, senare i den här artikeln för hello `$ILBIP` variabeln. |
   | **Prenumeration** |Om du har flera prenumerationer, visas det här fältet. Välj hello prenumeration som du vill tooassociate med den här resursen. Det är normalt hello samma prenumeration som alla hello resurser för hello-tillgänglighetsgruppen. |
   | **Resursgrupp** |Välj hello resursgrupp som hello SQL Server-instanser. |
   | **Plats** |Välj hello Azure-plats som hello SQL Server-instanser. |

6. Klicka på **Skapa**. 

Azure skapar hello belastningsutjämnaren. hello belastningsutjämnaren tillhör tooa nätverk, undernät, resursgrupp och plats. När Azure har slutförts hello aktiviteten Kontrollera hello inställningarna för belastningsutjämnaren i Azure. 

### <a name="step-2-configure-hello-back-end-pool"></a>Steg 2: Konfigurera hello backend-adresspool
Azure anrop hello backend-adresspool *serverdelspool*. I det här fallet är hello backend-adresspool hello-adresserna för hello två SQL Server-instanser i tillgänglighetsgruppen. 

1. Klicka på hello belastningsutjämnare som du skapat i din resursgrupp. 

2. På **inställningar**, klickar du på **serverdelspooler**.

3. På **serverdelspooler**, klickar du på **Lägg till** toocreate en backend-adresspool. 

4. På **lägga till serverdelspoolen**under **namn**, Skriv ett namn för hello backend-adresspool.

5. Under **virtuella datorer**, klickar du på **lägga till en virtuell dator**. 

6. Under **Välj virtuella datorer**, klickar du på **Välj en tillgänglighetsuppsättning**, och sedan ange hello tillgänglighetsuppsättning att hello SQL Server-datorer som tillhör.

7. När du har valt hello tillgänglighetsuppsättning klickar du på **Välj hello virtuella datorerna**, Välj hello två virtuella datorer som hello SQL Server-instanser i hello tillgänglighetsgruppen, och klicka sedan på **Välj**. 

8. Klicka på **OK** tooclose hello blad för **Välj virtuella datorer**, och **lägga till serverdelspoolen**. 

Azure uppdaterar hello inställningar för hello backend-adresspool. Din tillgänglighetsuppsättning har nu en pool med två SQL Server-instanser.

### <a name="step-3-create-a-probe"></a>Steg 3: Skapa en avsökning
hello avsökningen definierar hur Azure verifierar som hello SQL Server-instanser för närvarande äger hello tillgänglighetsgruppens lyssnare. Azure-avsökningar hello tjänsten baserat på hello IP-adress på en port som du anger när du skapar hello avsökning.

1. På hello belastningsutjämnare **inställningar** bladet, klickar du på **hälsoavsökning**. 

2. På hello **hälsoavsökning** bladet, klickar du på **Lägg till**.

3. Konfigurera hello avsökning på hello **Lägg till avsökning** bladet. Använd hello följande värden tooconfigure hello avsökning:

   | Inställning | Värde |
   | --- | --- |
   | **Namn** |Ett namn som representerar hello avsökning. Till exempel **SQLAlwaysOnEndPointProbe**. |
   | **Protokoll** |**TCP** |
   | **Port** |Du kan använda alla tillgängliga portar. Till exempel *59999*. |
   | **Intervall** |*5* |
   | **Tröskelvärde för ohälsosamt värde** |*2* |

4.  Klicka på **OK**. 

> [!NOTE]
> Kontrollera att hello-port som du anger är öppen hello-brandväggen för både SQL Server-instanser. Båda instanser kräver en inkommande regel för hello TCP-port som du använder. Mer information finns i [Lägg till eller redigera brandväggsregel](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure skapar hello avsökning och använder sedan vilka SQL Server-instansen har hello-lyssnare för hello tillgänglighetsgruppen tootest.

### <a name="step-4-set-hello-load-balancing-rules"></a>Steg 4: Ange hello regler för belastningsutjämning
Hej belastningsutjämningsregler konfigurera hur hello belastningsutjämnaren dirigerar trafik toohello SQL Server-instanser. För den här belastningsutjämnaren aktivera direkt serverreturnering eftersom endast en av hello två SQL Server-instanser äger hello lyssnare tillgänglighetsgruppsresursen i taget.

1. På hello belastningsutjämnare **inställningar** bladet, klickar du på **belastningsutjämningsregler**. 

2. På hello **belastningsutjämningsregler** bladet, klickar du på **Lägg till**.

3. På hello **Lägg till regler för belastningsutjämning** bladet konfigurera hello regeln för belastningsutjämning. Använd hello följande inställningar: 

   | Inställning | Värde |
   | --- | --- |
   | **Namn** |Ett namn som representerar hello regler för belastningsutjämning. Till exempel **SQLAlwaysOnEndPointListener**. |
   | **Protokoll** |**TCP** |
   | **Port** |*1433* |
   | **Backend-Port** |*1433*. Det här värdet ignoreras eftersom den här regeln använder **flytande IP (direkt serverreturnering)**. |
   | **Avsökningen** |Använda hello namnet på hello-avsökningen som du skapade för denna belastningsutjämning. |
   | **Persistence för session** |**Ingen** |
   | **Tidsgränsen för inaktivitet (minuter)** |*4* |
   | **Flytande IP (direkt serverreturnering)** |**Aktiverad** |

   > [!NOTE]
   > Du kanske tooscroll ned hello bladet tooview alla hello-inställningar.
   > 

4. Klicka på **OK**. 
5. Azure konfigurerar hello regel för belastningsutjämning. Hello belastningsutjämning är nu konfigurerad tooroute trafik toohello SQL Server-instans som är värd för hello-lyssnare för hello-tillgänglighetsgruppen. 

Hello resursgruppen har nu en belastningsutjämnare som ansluter tooboth SQL Server-datorer. hello belastningsutjämnaren innehåller också en IP-adress för hello SQL Server alltid på tillgänglighetsgruppens lyssnare, så att antingen datorn kan svara toorequests för hello Tillgänglighetsgrupper.

> [!NOTE]
> Om SQL Server-instanser i två olika områden, upprepa hello stegen i hello annan region. Varje region kräver en belastningsutjämnare. 
> 
> 

## <a name="configure-hello-cluster-toouse-hello-load-balancer-ip-address"></a>Konfigurera hello klustret toouse hello IP-adressen för belastningsutjämnaren
Nästa steg i hello tooconfigure hello lyssnare på hello klustret och ta hello-lyssnare. Hej du följande: 

1. Skapa hello tillgänglighetsgruppens lyssnare på hello failover-kluster. 

2. Sätta hello-lyssnaren.

### <a name="step-5-create-hello-availability-group-listener-on-hello-failover-cluster"></a>Steg 5: Skapa hello tillgänglighetsgruppens lyssnare på hello failover-kluster
I det här steget kan skapa du manuellt hello tillgänglighetsgruppens lyssnare i hanteraren för redundanskluster och SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-hello-configuration-of-hello-listener"></a>Verifiera konfigurationen av hello på hello-lyssnare

Om beroenden och hello klusterresurser är korrekt konfigurerade ska kunna tooview hello lyssnare i SQL Server Management Studio. tooset Hej lyssningsport, hello följande:

1. Starta SQL Server Management Studio och Anslut toohello primära repliken.

2. Gå för**AlwaysOn hög tillgänglighet** > **Tillgänglighetsgrupper** > **tillgänglighet Tillgänglighetsgruppslyssnarnas**.  
    Du bör nu se hello grupplyssnarens namn som du skapade i hanteraren för redundanskluster. 

3. Högerklicka på hello grupplyssnarens namn och klicka sedan på **egenskaper**.

4. I hello **Port** ange hello portnummer för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort du använde tidigare (1433 var hello standard), och klicka sedan på **OK**.

Nu har du en tillgänglighetsgrupp i Azure virtuella datorer som körs i läget Resource Manager. 

## <a name="test-hello-connection-toohello-listener"></a>Testa hello anslutning toohello lyssnare
Testa hello anslutning hello följande:

1. RDP tooa SQL Server-instans som är i hello samma virtuella nätverk, men inte egna hello replik. Den här servern kan hello andra SQLServer-instansen i hello kluster.

2. Använd **sqlcmd** verktyget tootest hello anslutning. Till exempel följande skript hello upprättar en **sqlcmd** anslutning toohello primära repliken via hello lyssnare med Windows-autentisering:
   
        sqlcmd -S <listenerName> -E

hello SQLCMD anslutning ansluter automatiskt toohello SQL Server-instans som är värd för hello primära repliken. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Skapa en IP-adress för en ytterligare tillgänglighetsgrupp

Varje tillgänglighetsgruppen använder en separat lyssnare. Varje lyssnare har sin egen IP-adress. Använd hello samma IP-adressen för belastningsutjämnaren toohold hello att läsa in för ytterligare lyssnare. När du har skapat en tillgänglighetsgrupp lägga till hello IP-adress toohello belastningsutjämnare och sedan konfigurera hello-lyssnare.

tooadd en IP-adress tooa belastningsutjämnare med hello Azure-portalen hello följande:

1. Öppna hello resursgruppen som innehåller hello belastningsutjämnare i hello Azure-portalen, och klicka på hello belastningsutjämnaren. 

2. Under **inställningar**, klickar du på **Frontend IP-pool**, och klicka sedan på **Lägg till**. 

3. Under **lägga till IP-adress för klientdel**, tilldela ett namn för hello klientdelen. 

4. Kontrollera att hello **för virtuella nätverk** och hello **undernät** är hello samma som hello SQL Server-instanser.

5. Ange hello IP-adress för hello-lyssnaren. 
   
   >[!TIP]
   >Du kan ange hello IP-adress toostatic och ange en adress som inte används för närvarande i hello undernät. Du kan också ange hello IP-adress toodynamic och spara hello nya frontend IP-adresspool. När du gör det tilldelas en tillgänglig IP-toohello adresspool automatiskt hello Azure-portalen. Du kan sedan öppna hello frontend IP-adresspool och ändra hello tilldelning toostatic. 

6. Spara hello IP-adress för hello-lyssnaren. 

7. Lägg till en hälsoavsökningen med hello följande inställningar:

   |Inställning |Värde
   |:-----|:----
   |**Namn** |Namnet tooidentify hello avsökning.
   |**Protokoll** |TCP
   |**Port** |En inte används TCP-port som måste finnas tillgängligt på alla virtuella datorer. Den kan inte användas för andra ändamål. Inga två lyssnare kan använda hello samma avsökning port. 
   |**Intervall** |hello tidslängd mellan avsökningen försöker. Använd hello standard (5).
   |**Tröskelvärde för ohälsosamt värde** |hello antal på varandra följande tröskelvärden som får misslyckas innan en virtuell dator anses vara felaktig.

8. Klicka på **OK** toosave hello avsökning. 

9. Skapa en regel för belastningsutjämning. Klicka på **belastningsutjämningsregler**, och klicka sedan på **Lägg till**.

10. Konfigurera hello ny regel för belastningsutjämning med hjälp av hello följande inställningar:

   |Inställning |Värde
   |:-----|:----
   |**Namn** |En namn tooidentify hello att läsa in belastningsutjämning regeln. 
   |**Frontend-IP-adress** |Välj hello IP-adress som du skapade. 
   |**Protokoll** |TCP
   |**Port** |Använda hello-port som använder hello SQL Server-instanser. En standardinstans använder port 1433, om du har ändrat den. 
   |**Backend-port** |Använd hello samma värde som **Port**.
   |**Serverdelspool** |hello-pool som innehåller hello virtuella datorer med hello SQL Server-instanser. 
   |**Hälsoavsökningen** |Välj hello-avsökningen som du skapade.
   |**Persistence för session** |Ingen
   |**Tidsgränsen för inaktivitet (minuter)** |Standard (4)
   |**Flytande IP (direkt serverreturnering)** | Enabled

### <a name="configure-hello-availability-group-toouse-hello-new-ip-address"></a>Konfigurera hello tillgänglighet grupp toouse hello nya IP-adress

toofinish konfigurerar hello kluster, upprepa hello steg som du har följt när du har gjort hello första tillgänglighetsgruppen. Det vill säga konfigurerar hello [kluster toouse hello nya IP-adressen](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

När du har lagt till en IP-adress för hello lyssnare konfigurera hello ytterligare tillgänglighetsgruppen hello följande: 

1. Kontrollera att hello avsökningsport för hello nya IP-adressen är öppen på både SQL Server-datorer. 

2. [I Klusterhanteraren, lägga till hello klientåtkomstpunkt](#addcap).

3. [Konfigurera hello IP-resurs för hello tillgänglighetsgruppen](#congroup).

   >[!IMPORTANT]
   >När du skapar hello IP-adress, använder du hello IP-adress som du lagt till toohello belastningsutjämnaren.  

4. [Gör hello SQL Server tillgänglighetsgruppsresursen beroende hello klientåtkomstpunkt](#dependencyGroup).

5. [Se hello klientåtkomst peka resurs som är beroende av hello IP-adress](#listname).
 
6. [Ange hello Klusterparametrar i PowerShell](#setparam).

När du har konfigurerat hello tillgänglighet grupp toouse hello nya IP-adresser, konfigurera hello anslutning toohello lyssnare. 

## <a name="next-steps"></a>Nästa steg

- [Konfigurera en SQL Server Always On availability group på virtuella Azure-datorer i olika regioner](virtual-machines-windows-portal-sql-availability-group-dr.md)
