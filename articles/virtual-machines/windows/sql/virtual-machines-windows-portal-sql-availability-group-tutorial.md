---
title: "aaaSQL Server-Tillgänglighetsgrupper - virtuella datorer i Azure - kursen | Microsoft Docs"
description: "Den här kursen visar hur toocreate en SQL Server alltid på tillgänglighetsgrupp på Azure Virtual Machines."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Konfigurera alltid på Tillgänglighetsgruppen i Azure VM manuellt

Den här kursen visar hur toocreate en SQL Server alltid på tillgänglighetsgrupp på Azure Virtual Machines. hello fullständig självstudiekursen skapar en tillgänglighetsgrupp med en databasreplik på två SQL-servrar.

**Tid uppskattning**: tar cirka 30 minuter toocomplete när hello krav är uppfyllda.

hello diagram visar vad du bygga hello kursen.

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Krav

hello kursen förutsätter att du har en grundläggande förståelse för SQL Server alltid på Tillgänglighetsgrupper. Om du behöver mer information, se [översikt över alltid på Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

hello visas följande tabell hello krav att du behöver toocomplete innan du påbörjar den här kursen:

|  |Krav |Beskrivning |
|----- |----- |----- |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Två SQL-servrar | – I en Azure tillgänglighetsuppsättning <br/> – I en domän <br/> -Med redundanskluster installerat |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Filresurs för klustret vittne |  
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-tjänstkontot | Domänkonto |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Tjänstkontot för SQL Server Agent | Domänkonto |  
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Öppna portar i brandväggen | -SQL Server: **1433** för standardinstans <br/> -Slutpunkten för databasspegling: **5022** eller alla tillgängliga portar <br/> -Azure belastningsutjämningsavsökning: **59999** eller alla tillgängliga portar |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Lägga till Redundansklusterfunktionen | Både SQL-servrar kräver den här funktionen |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Domänkonto för installation | -Lokal administratör på varje SQL Server <br/> -Medlem i SQL Server fasta serverrollen sysadmin för varje instans av SQL Server  |


Innan du påbörjar hello självstudien måste för[slutföra förutsättningar för att skapa Always On-Tillgänglighetsgrupper i Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md). Om dessa krav har redan slutförts, kan du hoppa för[Skapa kluster](#CreateCluster).


<!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Skapa hello-kluster

Efter hello förutsättningar har slutförts, är hello första steget toocreate ett redundanskluster för Windows-Server som innehåller två SQL-servrar och en vittnesserver som.  

1. RDP-toohello första SQL-Server med ett domänkonto som har administratörsbehörighet på SQL Server- och hello vittnesserver.

   >[!TIP]
   >Om du har följt hello [förutsättningsdokumentet](virtual-machines-windows-portal-sql-availability-group-prereq.md), du har skapat ett konto som heter **CORP\Install**. Använd det här kontot.

2. I hello **Serverhanteraren** instrumentpanelen, väljer **verktyg**, och klicka sedan på **Klusterhanteraren**.
3. I hello vänstra fönstret högerklickar du på **Klusterhanteraren**, och klicka sedan på **skapa ett kluster**.
   ![Skapa kluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. I guiden Skapa kluster hello, skapa ett kluster med en nod genom att gå igenom hello sidor med hello inställningarna i följande tabell hello:

   | Sidan | Inställningar |
   | --- | --- |
   | Innan du börjar |Använd standardvärden |
   | Välj servrar |Typen hello första SQL Server-namnet i **ange servernamnet** och på **Lägg till**. |
   | Verifieringsvarning |Välj **testar Nej jag inte behöver support från Microsoft för det här klustret och därför inte vill toorun hello validering. När jag klickar på nästa fortsätta skapa hello klustret**. |
   | Åtkomstpunkt för administration hello kluster |Skriv ett namn för klustret, till exempel **SQLAGCluster1** i **klusternamnet**.|
   | Bekräftelse |Använd standardvärden om du inte använder lagringsutrymmen. Se hello fotnoten efter denna tabell. |

### <a name="set-hello-cluster-ip-address"></a>Ange hello klustrets IP-adress

1. I **Klusterhanteraren**, rulla nedåt för**klustrets kärnresurser** och expandera hello klusterinformation. Du bör se både hello **namn** och hello **IP-adress** resurser i hello **misslyckades** tillstånd. hello IP-adressresurs kunde inte försättas online eftersom hello-klustret tilldelas hello samma IP-adress som dator för hello själva, därför är det en dubblett-adress.

2. Högerklicka på hello misslyckades **IP-adress** resursen och klickar sedan på **egenskaper**.

   ![Egenskaper för klustret](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Välj **statisk IP-adress** och ange en tillgänglig adress från undernätet där hello SQL Server finns i textrutan för hello-adress. Klicka på **OK**.
4. I hello **klustrets kärnresurser** avsnittet, högerklickar du på klusternamnet och klickar på **Anslut**. Vänta tills båda resurser är online. När hello klusterresurs namn är online uppdaterar hello DC-servern med ett nytt AD-datorkonto. Använd den här AD-kontot toorun hello Availability Group klustrade tjänsten senare.

### <a name="addNode"></a>Lägg till hello andra SQL Server-toocluster

Lägg till hello andra toohello SQL Server-kluster.

1. Högerklicka på hello klustret i trädet för hello webbläsare och på **Lägg till nod**.

    ![Lägg till nod toohello kluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. I hello **guiden Lägg till nod**, klickar du på **nästa**. I hello **Välj servrar** lägger du till hello andra SQL Server. Typen hello-servernamnet i **ange servernamnet** och klicka sedan på **Lägg till**. När du är klar klickar du på **nästa**.

1. I hello **verifieringsvarning** klickar du på **nr** (i en form av produktionsscenario du bör utföra hello verifieringstester). Klicka sedan på **Nästa**.

8. I hello **bekräftelse** om du använder lagringsutrymmen, rensa hello kryssrutan **lägga till alla tillgängliga lagringsenheter toohello klustret.**

   ![Lägg till nod bekräftelse](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Om du använder lagringsutrymmen och inte avmarkera **lägga till alla tillgängliga lagringsenheter toohello klustret**, Windows tar bort hello virtuella diskar under hello clustering processen. Därför kan de visas inte i Diskhantering eller Explorer tills hello lagringsutrymmen tas bort från hello kluster och anbringas på nytt med hjälp av PowerShell. Lagringsutrymmen grupper flera diskar i toostorage pooler. Mer information finns i [lagringsutrymmen](https://technet.microsoft.com/library/hh831739).

1. Klicka på **Nästa**.

1. Klicka på **Slutför**.

   Hanteraren för redundanskluster visar att klustret har en ny nod och listor i hello **noder** behållare.

10. Logga ut från hello fjärrskrivbords-sessionen.

### <a name="add-a-cluster-quorum-file-share"></a>Lägg till en filresurs för klustrets kvorum

I det här exemplet använder hello Windows klustret en filresursen toocreate en klustrets kvorum. Den här kursen använder en nod och filresursmajoritet kvorum. Mer information finns i [Om kvorumkonfigurationer i ett Failover-kluster](http://technet.microsoft.com/library/cc731739.aspx).

1. Ansluta toohello filresurs vittne medlem filserver med en fjärrskrivbordssession.

1. På **Serverhanteraren**, klickar du på **verktyg**. Öppna **Datorhantering**.

1. Klicka på **delade mappar**.

1. Högerklicka på **resurser**, och klicka på **ny resurs...** .

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Använd **guiden Skapa en delad mapp** toocreate en resurs.

1. På **mappsökväg**, klickar du på **Bläddra** och hitta eller skapa en sökväg för hello delad mapp. Klicka på **Nästa**.

1. I **namn, beskrivning och inställningar** Kontrollera hello namn och sökväg. Klicka på **Nästa**.

1. På **delade mappbehörigheter** ange **anpassa behörigheter**. Klicka på **anpassad...** .

1. På **anpassa behörigheter**, klickar du på **Lägg till...** .

1. Kontrollera att hello konto som används för toocreate hello kluster har fullständig kontroll.

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. Klicka på **OK**.

1. I **delade mappbehörigheter**, klickar du på **Slutför**. Klicka på **Slutför** igen.  

1. Logga ut från hello-server

### <a name="configure-cluster-quorum"></a>Konfigurera klustrets kvorumdisk

Ange därefter hello klustrets kvorum.

1. Ansluta toohello första klusternoden med fjärrskrivbord.

1. I **Klusterhanteraren**, högerklicka på klustret hello, peka för**fler åtgärder**, och klicka på **konfigurera inställningarna för klusterkvorum...** .

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. I **guiden Konfigurera klusterkvorum**, klickar du på **nästa**.

1. I **Välj alternativ för kvorumkonfiguration**, Välj **Välj kvorumvittne hello**, och klicka på **nästa**.

1. På **Välj Kvorumvittne**, klickar du på **konfigurera ett filresursvittne**.

   >[!TIP]
   >Windows Server 2016 stöder ett vittne i molnet. Om du väljer den här typen av vittne du behöver inte en fil dela vittne. Mer information finns i [distribuera ett moln vittne för ett redundanskluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Den här kursen använder ett filresursvittne som stöds av tidigare operativsystem.

1. På **konfigurera filresursvittne**, ange hello sökväg för hello resurs du skapat. Klicka på **Nästa**.

1. Kontrollera inställningarna för hello på **bekräftelse**. Klicka på **Nästa**.

1. Klicka på **Slutför**.

hello klustrets kärnresurser är konfigurerade med ett filresursvittne.

## <a name="enable-availability-groups"></a>Aktivera Tillgänglighetsgrupper

Aktivera sedan hello **AlwaysOn Availability Groups** funktion. Utföra de här stegen på både SQL-servrar.

1. Från hello **starta** skärmen, starta **SQL Server Configuration Manager**.
2. I trädet för hello webbläsaren klickar du på **SQL Server Services**, högerklicka på hello **SQL Server (MSSQLSERVER)** tjänsten och klickar på **egenskaper**.
3. Klicka på hello **AlwaysOn hög tillgänglighet** fliken och markera sedan **aktivera AlwaysOn Availability Groups**enligt följande:

    ![Aktivera AlwaysOn-Tillgänglighetsgrupper](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Klicka på **Använd**. Klicka på **OK** i hello popup-fönstret.

5. Starta om hello SQL Server-tjänsten.

Upprepa dessa steg på hello andra SQL Server.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a>Skapa en databas på hello första SQL-Server

1. Starta hello RDP-filen toohello första SQL-Server med en domän konto som är medlem i sysadmin fasta serverrollen.
1. Öppna SQL Server Management Studio och Anslut toohello första SQL-servern.
7. I **Object Explorer**, högerklicka på **databaser** och på **ny databas**.
8. I **databasnamnet**, typen **MyDB1**, klicka på **OK**.

### <a name="backupshare"></a>Skapa en säkerhetskopia

1. Hej första SQL-servern på **Serverhanteraren**, klickar du på **verktyg**. Öppna **Datorhantering**.

1. Klicka på **delade mappar**.

1. Högerklicka på **resurser**, och klicka på **ny resurs...** .

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Använd **guiden Skapa en delad mapp** toocreate en resurs.

1. På **mappsökväg**, klickar du på **Bläddra** och hitta eller skapa en sökväg för hello delad mapp för säkerhetskopiering. Klicka på **Nästa**.

1. I **namn, beskrivning och inställningar** Kontrollera hello namn och sökväg. Klicka på **Nästa**.

1. På **delade mappbehörigheter** ange **anpassa behörigheter**. Klicka på **anpassad...** .

1. På **anpassa behörigheter**, klickar du på **Lägg till...** .

1. Se till att hello SQL Server och SQL Server Agent-tjänstkontona för båda servrarna har fullständig behörighet.

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Klicka på **OK**.

1. I **delade mappbehörigheter**, klickar du på **Slutför**. Klicka på **Slutför** igen.  

### <a name="take-a-full-backup-of-hello-database"></a>Ta en fullständig säkerhetskopiering av databasen hello

Du måste tooback in hello ny databas tooinitialize hello loggkedjan. Om du inte gör en säkerhetskopia av den nya hello-databasen kan inte ingå i en tillgänglighetsgrupp.

1. I **Object Explorer**, högerklicka på hello-databasen, peka för**aktiviteter...** , klickar du på **säkerhetskopiera**.

1. Klicka på **OK** tootake en fullständig säkerhetskopiering toohello standardplatsen för säkerhetskopior.

## <a name="create-hello-availability-group"></a>Skapa hello Tillgänglighetsgruppen
Du är nu redo tooconfigure en tillgänglighetsgrupp med hello följande steg:

* Skapa en databas på hello första SQL-servern.
* Ta både en fullständig säkerhetskopia och en säkerhetskopia av transaktionsloggen för databasen hello
* Återställ hello fullständig och logg säkerhetskopieringar toohello andra SQL Server med hello **NORECOVERY** alternativet
* Skapa hello Availability Group (**AG1**) med synkront genomförande och automatisk redundans läsbara sekundära repliker

### <a name="create-hello-availability-group"></a>Skapa hello Tillgänglighetsgruppen:

1. På toohello fjärrskrivbords-sessionen första SQL-servern. I **Object Explorer** i SSMS, högerklickar du på **AlwaysOn hög tillgänglighet** och på **guiden Ny tillgänglighetsgrupp**.

    ![Starta guiden Ny tillgänglighetsgrupp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. I hello **introduktion** klickar du på **nästa**. I hello **ange tillgänglighetsgruppens namn** , ange ett namn för hello Availability Group, till exempel **AG1**i **tillgänglighetsgruppens namn**. Klicka på **Nästa**.

    ![Guiden Ny AG, ange AG-namn](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. I hello **Välj databaser** väljer du databasen och klicka på **nästa**.

   >[!NOTE]
   >hello databasen uppfyller hello krav för en tillgänglighetsgrupp eftersom du har utfört en fullständig säkerhetskopiering på hello avsedda primära repliken.

   ![Guiden Ny AG, Välj databaser](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. I hello **ange repliker** klickar du på **lägga till replik**.

   ![Guiden Ny AG, ange repliker](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. Hej **ansluta tooServer** dialogrutan som öppnas. Hello-typnamn för hello andra servern i **servernamn**. Klicka på **Anslut**.

   Tillbaka i hello **ange repliker** sidan du bör nu se hello andra server som anges i **Tillgänglighetsrepliker**. Konfigurera hello repliker på följande sätt.

   ![Guiden Ny AG, ange repliker (fullständig)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Klicka på **slutpunkter** toosee hello slutpunkten för databasspegling för den här Tillgänglighetsgruppen. Använd hello samma port som du använde när du ställer in hello [brandväggsregel för slutpunkter för databasspegling](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Guiden Ny AG, Välj inledande datasynkronisering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. I hello **Välj inledande datasynkronisering** väljer **fullständig** och ange en plats i nätverket. För hello plats, använder du hello [säkerhetskopieringsresursen du skapade](#backupshare). I hello exempel det var **\\\\\<första SQL Server\>\Backup\**. Klicka på **Nästa**.

   >[!NOTE]
   >Fullständig synkronisering tar en fullständig säkerhetskopiering av databasen hello på hello första instansen av SQL Server och återställer det andra toohello-instans. För stora databaser rekommenderas fullständig synkronisering inte eftersom det kan ta lång tid. Du kan minska nu genom att manuellt ta en säkerhetskopia av databasen hello och återställning med `NO RECOVERY`. Om hello databasen är redan återställd med `NO RECOVERY` hello på andra SQL Server innan du konfigurerar hello Availability Group, Välj **Anslut endast**. Om du vill tootake hello säkerhetskopia när du har konfigurerat hello Availability Group väljer **hoppa över inledande datasynkronisering**.

    ![Guiden Ny AG, Välj inledande datasynkronisering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. I hello **validering** klickar du på **nästa**. Den här sidan bör se ut ungefär toohello följande bild:

    ![Guiden Ny AG, validering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Det finns en varning för hello Lyssnarkonfigurationen eftersom du inte har konfigurerat en tillgänglighetsgruppens lyssnare. Du kan ignorera den här varningen eftersom på Azure virtual machines hello lyssnaren skapa när du har skapat hello Azure belastningsutjämnare.

10. I hello **sammanfattning** klickar du på **Slutför**, och sedan vänta medan hello guiden konfigurerar hello nya Tillgänglighetsgruppen. I hello **förlopp** sidan som du kan klicka på **mer** tooview hello detaljerad pågår. När hello-guiden har slutförts kan du granska hello **resultat** sidan tooverify som hello tillgänglighetsgrupp har skapats.

     ![Guiden för ny AG resultat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Klicka på **Stäng** tooexit hello guiden.

### <a name="check-hello-availability-group"></a>Kontrollera hello Tillgänglighetsgruppen

1. I **Object Explorer**, expandera **AlwaysOn hög tillgänglighet**, expandera **Tillgänglighetsgrupper**. Du bör nu se hello nya Tillgänglighetsgruppen i den här behållaren. Högerklicka på hello Tillgänglighetsgruppen och på **visa instrumentpanelen**.

   ![Visa AG-instrumentpanelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   Din **AlwaysOn instrumentpanelen** bör se ut ungefär toothis.

   ![AG-instrumentpanelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Du kan se hello repliker, hello redundansläge av varje replik och hello synkroniseringstillståndet.

2. I **Klusterhanteraren**, klicka på ditt kluster. Välj **roller**. hello tillgänglighetsgruppens namn som du använde är en roll på hello klustret. Att Tillgänglighetsgruppen inte har en IP-adress för klientanslutningar, eftersom du inte konfigurerar en lyssnare. Du konfigurerar hello lyssnare när du har skapat en Azure belastningsutjämnare.

   ![AG i hanteraren för redundanskluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Försök inte toofail över hello Availability Group från hello hanteraren för redundanskluster. Alla redundansåtgärder ska utföras inifrån **AlwaysOn instrumentpanelen** i SSMS. Mer information finns i [begränsningar med hjälp av hello Klusterhanterare med Tillgänglighetsgrupper](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Du har nu en tillgänglighetsgrupp med repliker på två instanser av SQL Server. Du kan flytta hello Availability Group mellan instanser. Du kan inte ansluta toohello Availability Group ännu eftersom du inte har en lyssnare. I Azure-datorer kräver hello lyssnare en belastningsutjämnare. hello nästa steg är toocreate hello belastningsutjämnare i Azure.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Skapa en Azure belastningsutjämnare

På Azure virtual machines kräver en belastningsutjämnare i en SQL Server-tillgänglighetsgrupp. hello belastningsutjämnaren innehåller hello IP-adress för hello tillgänglighetsgruppens lyssnare. Det här avsnittet beskrivs hur toocreate hello belastningsutjämnare i hello Azure-portalen.

1. I hello Azure-portalen, går toohello resursgruppen där din SQL-servrar är och på **+ Lägg till**.
2. Sök efter **belastningsutjämnare**. Välj hello belastningsutjämnare som publicerats av Microsoft.

   ![AG i hanteraren för redundanskluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  Klicka på **Skapa**.
3. Konfigurera hello följande parametrar för hello belastningsutjämnaren.

   | Inställning | Fält |
   | --- | --- |
   | **Namn** |Använda ett namn för hello belastningsutjämnare, till exempel **sqlLB**. |
   | **Typ** |internt |
   | **Virtuellt nätverk** |Använda hello namnet på hello virtuella Azure-nätverket. |
   | **Undernät** |Använd hello namnet på hello undernät som hello virtuella datorn är på.  |
   | **IP-adresstilldelning** |Statisk |
   | **IP-adress** |Använd en tillgänglig adress från undernätet. |
   | **Prenumeration** |Använd hello samma prenumeration som hello virtuell dator. |
   | **Plats** |Använd hello samma plats som hello virtuell dator. |

   hello Azure portal, blad bör se ut så här:

   ![Skapa belastningsutjämnare](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Klicka på **skapa**, toocreate hello belastningsutjämnaren.

tooconfigure hello belastningsutjämnare, måste toocreate en serverdelspool en avsökning och ange hello belastningsutjämningsregler. Gör du följande i hello Azure-portalen.

### <a name="add-backend-pool"></a>Lägg till serverdelspoolen

1. Gå tooyour tillgänglighetsgruppen i hello Azure-portalen. Du kan behöva toorefresh hello visa toosee hello nyskapad belastningsutjämnaren.

   ![Hitta belastningsutjämnare i resursgruppen.](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Klicka på hello belastningsutjämnare, **serverdelspooler**, och klicka på **+ Lägg till**. Ange hello serverdelspool på följande sätt:

   | Inställning | Beskrivning | Exempel
   | --- | --- |---
   | **Namn** | Skriv ett namn | SQLLBBE
   | **Tillhör** | Välj från listan | Tillgänglighetsuppsättning
   | **Tillgänglighetsuppsättning** | Använd ett namn på hello tillgänglighetsuppsättningen som din SQL Server-datorer finns i | sqlAvailabilitySet |
   | **Virtuella datorer** |Hej två Azure SQL Server-VM-namn | SQLServer-0, sqlserver-1

1. Hello typnamn för hello serverdelspool.

1. Klicka på **+ Lägg till en virtuell dator**.

1. För hello tillgänglighetsuppsättning Välj hello tillgänglighet som hello SQL-servrar finns i.

1. För virtuella datorer, ta med både hello SQL-servrar. Inkludera inte hello filserver filresurs vittne.

1. Klicka på **OK** toocreate hello serverdelspool.

### <a name="set-hello-probe"></a>Ange hello avsökning

1. Klicka på hello belastningsutjämnare, **hälsoavsökning**, och klicka på **+ Lägg till**.

1. Ange hello hälsoavsökningen på följande sätt:

   | Inställning | Beskrivning | Exempel
   | --- | --- |---
   | **Namn** | Text | SQLAlwaysOnEndPointProbe |
   | **Protokoll** | Välj TCP | TCP |
   | **Port** | Alla oanvända portar | 59999 |
   | **Intervall**  | hello tidslängd mellan avsökningen försök i sekunder |5 |
   | **Tröskelvärde för ohälsosamt värde** | Hej antalet avsökningsfel måste ske för en virtuell dator toobe bedöms som dåligt  | 2 |

1. Klicka på **OK** tooset hello hälsoavsökningen.

### <a name="set-hello-load-balancing-rules"></a>Ange hello regler för belastningsutjämning

1. Klicka på hello belastningsutjämnare, **belastningsutjämningsregler**, och klicka på **+ Lägg till**.

1. Ange hello belastningsutjämningsregler på följande sätt.
   | Inställning | Beskrivning | Exempel
   | --- | --- |---
   | **Namn** | Text | SQLAlwaysOnEndPointListener |
   | **Frontend-IP-adress** | Välj en adress |Använda hello-adress som du skapade när du skapade hello belastningsutjämnaren. |
   | **Protokoll** | Välj TCP |TCP |
   | **Port** | Använda hello port för SQL Server-instans för hello | 1433 |
   | **Backend-Port** | Det här fältet används inte när flytande IP är inställd för direkta servern returnerade | 1433 |
   | **Avsökningen** |hello-namn du angav för hello avsökningen | SQLAlwaysOnEndPointProbe |
   | **Persistence för session** | Listrutan | **Ingen** |
   | **Inaktivitetstid** | Öppna minuter tookeep en TCP-anslutning | 4 |
   | **Flytande IP (direkt serverreturnering)** | |Enabled |

   > [!WARNING]
   > Direkt serverreturnering anges när du skapar. Det går inte att ändra.

1. Klicka på **OK** tooset hello belastningsutjämningsregler.

## <a name="configure-listener"></a>Konfigurera hello-lyssnare

hello nästa sak toodo är tooconfigure en tillgänglighetsgruppens lyssnare på hello failover-kluster.

> [!NOTE]
> Den här kursen visar hur toocreate en enda lyssnare - med en ILB IP-adress. toocreate lyssnare med hjälp av en eller flera IP-adresser, se [Create Availability Group listener och belastningsutjämnare | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Ange lyssningsport

Ange hello lyssningsport i SQL Server Management Studio.

1. Starta SQL Server Management Studio och Anslut toohello primära repliken.

1. Navigera för**AlwaysOn hög tillgänglighet** | **Tillgänglighetsgrupper** | **tillgänglighet Tillgänglighetsgruppslyssnarnas**.

1. Du bör nu se hello grupplyssnarens namn som du skapade i hanteraren för redundanskluster. Högerklicka på hello grupplyssnarens namn och på **egenskaper**.

1. I hello **Port** ange hello portnumret för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort du använde tidigare (1433 var hello standard), klicka på **OK**.

Nu har du en SQL Server-tillgänglighetsgrupp i Azure virtuella datorer som körs i läget Resource Manager.

## <a name="test-connection-toolistener"></a>Testa anslutning toolistener

tootest hello anslutning:

1. RDP-tooa SQL Server som är i hello samma virtuella nätverk, men inte egna hello replik. Du kan använda hello andra SQL Server i hello-klustret.

1. Använd **sqlcmd** verktyget tootest hello anslutning. Till exempel följande skript hello upprättar en **sqlcmd** anslutning toohello primära repliken via hello lyssnare med Windows-autentisering:

    ```
    sqlcmd -S <listenerName> -E
    ```

    Om hello lyssnare använder en port än hello standard port (1433), ange hello port i hello anslutningssträngen. Till exempel ansluter hello följande kommando med sqlcmd tooa lyssnaren på port 1435:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

hello SQLCMD anslutning ansluter automatiskt toowhichever instans av SQL Server-värdar hello primära repliken.

> [!TIP]
> Kontrollera att hello-port som du anger är öppen hello-brandväggen för både SQL-servrar. Båda servrarna kräver en inkommande regel för hello TCP-port som du använder. Mer information finns i [Lägg till eller redigera brandväggsregel](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a>Nästa steg

- [Lägg till en IP-adress tooa belastningsutjämnare för en andra tillgänglighetsgruppen](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
