---
title: "aaaExtend lokalt Always On-Tillgänglighetsgrupper tooAzure | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen och beskriver hur hello toouse guiden Lägg till replik i SQL Server Management Studio (SSMS) tooadd en Always On-Tillgänglighetsgruppen replik i Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 51fe117b40d69706b35f30101538a7a36116e128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extend-on-premises-always-on-availability-groups-tooazure"></a>Utöka lokala Always On-Tillgänglighetsgrupper tooAzure
Always On-Tillgänglighetsgrupper ger hög tillgänglighet för grupper av databasen genom att lägga till sekundära repliker. De här replikeringarna tillåter inte att redundansväxla databaser om ett fel uppstår. Dessutom kan de vara används toooffload läsa arbetsbelastningar eller aktiviteter för säkerhetskopiering.

Du kan utöka lokala Tillgänglighetsgrupper tooMicrosoft Azure genom att etablera en eller flera virtuella Azure-datorer med SQL Server och sedan lägga till dem som repliker tooyour lokalt Tillgänglighetsgrupper.

Den här kursen förutsätter att du har hello följande:

* En aktiv Azure-prenumeration. Du kan [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* En befintlig alltid på tillgänglighetsgrupp lokalt. Mer information om Tillgänglighetsgrupper i finns [alltid på Tillgänglighetsgrupper](https://msdn.microsoft.com/library/hh510230.aspx).
* Anslutningen mellan hello lokalt nätverk och dina virtuella Azure-nätverket. Mer information om hur du skapar det här virtuella nätverket finns [skapa en plats-till-plats-anslutning med hello Azure-portalen (klassisk)](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

## <a name="add-azure-replica-wizard"></a>Azure replik guiden Lägg till
Det här avsnittet beskrivs hur du toouse hello **guiden för Lägg till Azure-replik** tooextend din alltid på Tillgänglighetsgruppen lösning tooinclude Azure repliker.

> [!IMPORTANT]
> Hej **guiden för Lägg till Azure-replik** stöder endast virtuella datorer som skapats med hello klassiska distributionsmodellen. Den nya VM distributioner bör använda hello senare Resource Manager-modellen. Om du använder virtuella datorer med Resource Manager, måste du manuellt till hello sekundära Azure repliken med hjälp av Transact-SQL-commmands (visas inte här). Den här guiden fungerar inte i hello Resource Manager scenario.

1. Från inom SQL Server Management Studio, expandera **alltid på hög tillgänglighet** > **Tillgänglighetsgrupper** > **[namn på Tillgänglighetsgruppen]**.
2. Högerklicka på **Tillgänglighetsrepliker**, klicka på **lägga till replik**.
3. Som standard hello **lägga till replik tooAvailability guiden** visas. Klicka på **Nästa**.  Om du har valt hello **Visa inte den här sidan igen** alternativet hello längst ned på sidan för hello under en föregående lanseringen av den här guiden kan den här skärmen visas inte.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Du kommer att nödvändiga tooconnect tooall befintliga sekundära repliker. Du kan klicka på **Anslut...** bredvid varje replik eller du kan klicka på **ansluta alla...** längst ned hello hello-skärmen. När autentisering, klickar du på **nästa** tooadvance toohello nästa skärm.
5. På hello **ange repliker** sidan flera flikar visas överst hello: **repliker**, **slutpunkter**, **säkerhetskopiering inställningar**, och **Lyssnare**. Från hello **repliker** klickar du på **lägga till Azure-replik...** toolaunch hello guiden för Lägg till Azure-repliken.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Välj en befintlig Azure-Hanteringscertifikatet från certifikatarkivet hello lokala Windows om du har installerat en innan. Välj eller ange hello-id för Azure-prenumeration om du har använt en innan. Du kan klicka på Hämta toodownload och installera Azure-Hanteringscertifikatet och hämta hello listan över prenumerationer med hjälp av ett Azure-konto.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Du kan du fylla i fälten på sidan hello med värden som kommer att använda toocreate hello Azure virtuell dator (VM) som är värd för hello replik.
   
   | Inställning | Beskrivning |
   | --- | --- |
   | **Bild** |Välj önskad hello kombination av OS- och SQL Server |
   | **VM-storlek** |Välj hello storlek för virtuell dator som bäst passar dina affärsbehov |
   | **Namn på virtuell dator** |Ange ett unikt namn för hello ny virtuell dator. hello namn måste innehålla mellan 3 och 15 tecken, kan innehålla endast bokstäver, siffror och bindestreck, och måste börja med en bokstav och sluta med en bokstav eller siffra. |
   | **VM-användarnamn** |Ange ett användarnamn som ska bli hello administratörskontot på hello VM |
   | **Administratörslösenord för VM** |Ange ett lösenord för hello nytt konto |
   | **Bekräfta lösenord** |Bekräfta lösenord som hello hello nytt konto |
   | **Virtual Network** |Ange hello Azure virtuella nätverk som hello ny virtuell dator ska använda. Mer information om virtuella nätverk finns [översikt över virtuella nätverk](../../../virtual-network/virtual-networks-overview.md). |
   | **Undernät för virtuellt nätverk** |Ange hello undernät för virtuellt nätverk som hello ny virtuell dator ska använda |
   | **Domän** |Bekräfta hello förifyllda värde för hello domän är korrekta |
   | **Namnet på användaren** |Ange ett konto som är i hello lokala administratörsgruppen på hello lokala klusternoder |
   | **Lösenord** |Ange hello lösenord för hello domänanvändarnamn |
8. Klicka på **OK** toovalidate hello distributionsinställningar.
9. Juridiska villkoren visas bredvid. Läs och klicka på **OK** om toothese godkänner villkoren.
10. Hej **ange repliker** visas igen. Kontrollera hello inställningar för nya hello Azure repliken på hello **repliker**, **slutpunkter**, och **säkerhetskopiering inställningar** flikar. Ändra inställningar toomeet dina affärsbehov.  Mer information om hello parametrar som finns på de här flikarna, se [ange repliker sida (guiden Ny tillgänglighetsgrupp guiden/Add replik)](https://msdn.microsoft.com/library/hh213088.aspx). Observera att lyssnare inte kan skapas med hello lyssnare fliken för Tillgänglighetsgrupper som innehåller Azure repliker. Dessutom, om en lyssnare har redan skapats tidigare toolaunching hello guiden, visas ett meddelande som anger att den inte stöds i Azure. Vi ska titta på hur toocreate lyssnare i hello **skapa en Tillgänglighetsgruppslyssnare** avsnitt.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Klicka på **Nästa**.
12. Välj synkroniseringsmetod hello data du vill använda toouse på hello **Välj inledande datasynkronisering** och klickar på **nästa**. För de flesta fall väljer **Fullständig datasynkronisering**. Mer information om metoder för synkronisering av data finns [inledande Data synkronisering på sidan Välj (alltid på tillgänglighet grupp guider)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Granska resultatet från hello på hello **validering** sidan. Korrigera utestående problem och kör hello verifiering igen om det behövs. Klicka på **Nästa**.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Granska inställningarna för hello på hello **sammanfattning** sidan och klicka sedan på **Slutför**.
15. hello etableringsprocessen börjar. När hello guiden är klar klickar du på **Stäng** tooexit utanför hello guiden.

> [!NOTE]
> hello guiden för Lägg till Azure-replikeringen en loggfil skapas i Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Den här loggfilen kan vara används tootroubleshoot misslyckades Azure replik distributioner. Om hello guiden misslyckas alla åtgärden kördes alla tidigare åtgärder återställs, inklusive ta bort hello etablerats VM.
> 
> 

## <a name="create-an-availability-group-listener"></a>Skapa en tillgänglighetsgruppslyssnare
När hello tillgänglighetsgrupp har skapats, bör du skapa en lyssnare för klienter tooconnect toohello repliker. Lyssnare direkt inkommande anslutningar tooeither hello primära eller en skrivskyddad sekundär replik. Mer information om lyssnare finns [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Nästa steg
I tillägg toousing hello **guiden för Lägg till Azure-replik** tooextend din alltid på Tillgänglighetsgruppen tooAzure du kan också flytta vissa SQL Server-arbetsbelastningar helt tooAzure. tooget igång, se [etablering av en SQL Server-dator på Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

