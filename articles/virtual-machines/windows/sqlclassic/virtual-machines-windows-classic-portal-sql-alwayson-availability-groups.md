---
title: "aaaConfigure Always On-tillgänglighetsgrupp i Azure Virtual Machines (klassisk) | Microsoft Docs"
description: "Skapa en Always On-tillgänglighetsgrupp med Azure virtuella datorer. Den här kursen används främst hello användargränssnittet och verktyg i stället skript."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: f428ad994a55378c281c959f4696fdcaf50632b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Konfigurera Always On-tillgänglighetsgrupp i Azure Virtual Machines (klassisk)
> [!div class="op_single_selector"]
> * [Klassiska: UI](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassiska: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Innan du börjar bör du överväga att du kan nu slutföra den här aktiviteten i Azure Resource Manager-modellen. Vi rekommenderar Azure Resource Manager-modellen för nya distributioner. Se [SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Azure har två olika distribution modeller toocreate och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln förklarar hur toouse hello klassiska distributionsmodellen. 

toocomplete uppgiften med hello Azure Resource Manager-modellen, se [SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

Den här slutpunkt till slutpunkt-kursen visar hur tooimplement tillgänglighet grupper med hjälp av SQL Server Always On körs på Azure Virtual Machines.

Hello slutet av hello kursen består SQL Server alltid på lösningen i Azure av hello följande element:

* Ett virtuellt nätverk som innehåller flera undernät och en frontend och backend-undernät
* En domänkontrollant med en domän i Active Directory (AD Azure)
* Två virtuella datorer som kör SQL Server och distribuerad toohello backend-undernät och kopplade toohello Azure AD-domän
* Ett redundanskluster med tre noder med hello Nodmajoritet kvorummodellen
* En tillgänglighetsgrupp som har två replikerna med synkront genomförande av en tillgänglighetsdatabas

hello följande bild är en grafisk representation av hello lösning.

![Test lab-arkitektur för Tillgänglighetsgrupper i Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Observera att detta är en konfiguration som möjligt. Exempelvis kan du minimera hello antalet virtuella datorer för en två-replik-tillgänglighetsgrupp. Den här konfigurationen sparar beräkningstimmar i Azure med hjälp av hello domänkontrollant som filresursvittne i hello kvorum i ett kluster med två noder. Den här metoden minskar hello för antal virtuella datorer med ett från hello visas konfigurationen.

Den här självstudiekursen förutsätts hello följande:

* Du har redan ett Azure-konto.
* Du vet redan hur toouse hello GUI i hello virtuella galleriet tooprovision en klassisk virtuell dator som kör SQL Server.
* Du har redan en god förståelse av Always On-Tillgänglighetsgrupper. Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Om du vill använda Always On-Tillgänglighetsgrupper med SharePoint finns även [Konfigurera SQL Server 2012 Always On-Tillgänglighetsgrupper för SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-hello-virtual-network-and-domain-controller-server"></a>Skapa hello virtuella nätverk och Domänkontrollantservern
Du börjar med ett nytt Azure utvärderingskonto. När du har skapat ditt konto ska på hello startsidan för hello klassiska Azure-portalen.

1. Klicka på hello **ny** knappen längst ned hello hello sidan som visas i följande skärmbild hello hello vänster.
   
    ![Klicka på ny hello-portalen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Klicka på **nätverkstjänster** > **virtuellt nätverk** > **skapa anpassade**som visas i följande skärmbild hello.
   
    ![Skapa ett virtuellt nätverk](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. I hello **skapa ett VIRTUELLT nätverk** dialogrutan skapar ett nytt virtuellt nätverk genom att gå igenom hello sidor och hello inställningar i hello i den följande tabellen. 
   
   | Sidan | Inställningar |
   | --- | --- |
   | Information om virtuellt nätverk |**NAME = ContosoNET**<br/>**REGION = västra USA** |
   | DNS-servrar och VPN-anslutning |Ingen |
   | Virtuellt nätverk adressutrymmen |Inställningarna visas i följande skärmbild hello: ![Skapa ett virtuellt nätverk](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Skapa hello virtuell dator som ska användas som hello-domänkontrollant (DC). Klicka på **ny** > **Compute** > **virtuella** > **från galleriet**som visas i Hej följande skärmbild.
   
    ![Skapa en virtuell dator](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. I hello **skapa en virtuell dator** dialogrutan konfigurerar du en ny virtuell dator genom att gå igenom hello sidor och hello inställningar i hello i den följande tabellen. 
   
   | Sidan | Inställningar |
   | --- | --- |
   | Välj hello virtuella operativsystem |Windows Server 2012 R2 Datacenter |
   | Konfiguration av virtuell dator |**VERSION LANSERINGSDATUMET** = (senaste)<br/>**NAMN på virtuell dator** = ContosoDC<br/>**NIVÅN** = STANDARD<br/>**STORLEK** = A2 (2 kärnor)<br/>**NYTT användarnamn** = AzureAdmin<br/>**NYTT lösenord** = Contoso! 000<br/>**BEKRÄFTA** = Contoso! 000 |
   | Konfiguration av virtuell dator |**MOLNTJÄNSTEN** = skapa en ny molntjänst<br/>**DNS-MOLNTJÄNSTNAMNET** = en unik molntjänstnamnet<br/>**DNS-namnet** = ett unikt namn (ex: ContosoDC123)<br/>**REGION/TILLHÖRIGHETSGRUPP/VIRTUELLT nätverk** = ContosoNET<br/>**VIRTUELLA UNDERNÄT** = Back(10.10.2.0/24)<br/>**STORAGE-konto** = Använd en automatiskt genererad storage-konto<br/>**TILLGÄNGLIGHETSUPPSÄTTNINGEN** = (ingen) |
   | Alternativ för virtuell dator |Använd standardvärden |

När du konfigurerar hello nya virtuella datorn, vänta hello virtuella toobe provsioned. Den här processen tar några tid toofinish. Om du klickar på hello **virtuella** fliken i hello Azure klassiska portal, kan du se ContosoDC reglering tillstånd från **start (etablering)** för**stoppad**, **Från**, **körs (etablering)**, och slutligen **kör**.

hello DC-servern är nu etablerats. Därefter konfigurerar du hello Active Directory-domän på den här DC-servern.

## <a name="configure-hello-domain-controller"></a>Konfigurera hello-domänkontrollant
I följande steg hello, konfigurerar du hello ContosoDC datorn som en domänkontrollant för corp.contoso.com.

1. Välj hello i hello portal **ContosoDC** datorn. På hello **instrumentpanelen** klickar du på **Anslut** tooopen fjärrskrivbord (RDP)-filen för fjärråtkomst till skrivbordet.
   
    ![Ansluta tooVritual datorn](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Logga in med ditt konfigurerade administratörskonto (**\AzureAdmin**) och lösenord (**Contoso! 000**).
3. Som standard hello **Serverhanteraren** instrumentpanelen ska visas.
4. Klicka på hello **Lägg till roller och funktioner** länk på hello instrumentpanelen.
   
    ![Server Explorer Lägg till roller](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Klicka på **nästa** tills toohello **serverroller** avsnitt.
6. Välj hello **Active Directory Domain Services** och **DNS-Server** roller. När du uppmanas, lägger du till fler funktioner som kräver dessa roller.
   
   > [!NOTE]
   > Du får en varning om att det finns ingen statisk IP-adress verifiering. Om du vill testa hello konfiguration, klickar du på **Fortsätt**. För produktion scenarier [PowerShell tooset hello statisk IP-adress för hello domain controller datorn](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Lägg till roller dialogrutan](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Klicka på **nästa** tills du når hello **bekräftelse** avsnitt. Välj hello **omstart hello-målservern automatiskt om det behövs** kryssrutan.
8. Klicka på **Installera**.
9. När hello funktionerna installeras tillbaka toohello **Serverhanteraren** instrumentpanelen.
10. Välj ny hello **AD DS** alternativet hello vänster.
11. Klicka på hello **mer** länk hello gul varning-fältet.
    
     ![Dialogrutan för AD DS på virtuell dator med DNS-server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. I hello **åtgärd** kolumn i hello **alla Server aktivitetsinformation** dialogrutan klickar du på **befordra den här servern tooa domänkontrollanten**.
13. I hello **konfigurationsguiden Active Directory Domain Services**, använda hello följande värden:
    
    | Sidan | Inställning |
    | --- | --- |
    | Distributionskonfiguration |**Lägg till en ny skog** = valda<br/>**Rotdomännamn** = corp.contoso.com |
    | Alternativ för domänkontrollant |**Lösenordet** = Contoso! 000<br/>**Bekräfta lösenord** = Contoso! 000 |
14. Klicka på **nästa** toogo via hello andra sidor i guiden hello. På hello **Kravkontroll** kontrollerar du att du ser följande meddelande hello: **alla kravkontroller har lyckats**. Observera att bör du granska alla meddelanden, men det är möjligt toocontinue med hello-installation.
15. Klicka på **Installera**. Hej **ContosoDC** virtuella datorn ska startas om automatiskt.

## <a name="configure-domain-accounts"></a>Konfigurera domänkonton
Nästa steg i hello konfigurera hello Active Directory-konton för senare användning.

1. Logga in toohello **ContosoDC** datorn.
2. I **Serverhanteraren**, klickar du på **verktyg** > **Active Directory Administrationscenter**.
   
    ![Active Directory Administrationscenter](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. I hello **Active Directory Administrationscenter**väljer **corp (lokal)** hello vänster.
4. På hello rätt **uppgifter** rutan klickar du på **ny** > **användaren**. Använd hello följande inställningar:
   
   | Inställning | Värde |
   | --- | --- |
   | **Förnamn** |Installera |
   | **Användare SamAccountName** |Installera |
   | **Lösenord** |Contoso! 000 |
   | **Bekräfta lösenord** |Contoso! 000 |
   | **Andra alternativ för lösenord** |Vald |
   | **Lösenordet upphör aldrig att gälla** |Markerad |
5. Klicka på **OK** toocreate hello **installera** användare. Det här kontot kommer att använda tooconfigure hello redundans-kluster och hello tillgänglighetsgruppen.
6. Skapa två ytterligare användare **CORP\SQLSvc1** och **CORP\SQLSvc2**, med hello samma steg. Dessa konton används för hello SQL Server-instanser. Sedan måste toogive **CORP\Install** hello behörighet tooconfigure redundanskluster i Windows.
7. I hello **Active Directory Administrationscenter**, klickar du på **corp (lokal)** hello vänster. I hello **uppgifter** rutan klickar du på **egenskaper**.
   
    ![Egenskaper för CORP-användare](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Välj **tillägg**, och klicka sedan på hello **Avancerat** hello-knappen **säkerhet** fliken.
9. I hello **avancerade säkerhetsinställningar för corp** dialogrutan klickar du på **Lägg till**.
10. Klicka på **Välj en huvudansvarig**, söka efter **CORP\Install**, och klicka sedan på **OK**.
11. Välj hello **läsa alla egenskaper** och **skapa datorobjekt** behörigheter.
    
     ![Corp användarbehörighet](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Klicka på **OK**, och klicka sedan på **OK** igen. Stäng hello corp i fönstret.

Nu när du har konfigurerat Active Directory och hello användarobjekt du skapa tre virtuella datorer för SQL Server och Anslut dem toothis domän.

## <a name="create-hello-sql-server-virtual-machines"></a>Skapa hello SQL Server-datorer
Skapa tre virtuella datorer. En är en klusternod och två är för SQL Server. toocreate hello virtuella datorer, gå tillbaka toohello klassiska Azure-portalen klickar du på **ny** > **Compute** > **virtuella**  >  **Från galleriet**. Använd sedan hello mallar i hello efter tabellen toohelp du skapa hello virtuella datorer.

| Sidan | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Välj hello virtuella operativsystem |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Konfiguration av virtuell dator |**VERSION LANSERINGSDATUMET** = (senaste)<br/>**NAMN på virtuell dator** = ContosoWSFCNode<br/>**NIVÅN** = STANDARD<br/>**STORLEK** = A2 (2 kärnor)<br/>**NYTT användarnamn** = AzureAdmin<br/>**NYTT lösenord** = Contoso! 000<br/>**BEKRÄFTA** = Contoso! 000 |**VERSION LANSERINGSDATUMET** = (senaste)<br/>**NAMN på virtuell dator** = ContosoSQL1<br/>**NIVÅN** = STANDARD<br/>**STORLEK** = A3 (4 kärnor)<br/>**NYTT användarnamn** = AzureAdmin<br/>**NYTT lösenord** = Contoso! 000<br/>**BEKRÄFTA** = Contoso! 000 |**VERSION LANSERINGSDATUMET** = (senaste)<br/>**NAMN på virtuell dator** = ContosoSQL2<br/>**NIVÅN** = STANDARD<br/>**STORLEK** = A3 (4 kärnor)<br/>**NYTT användarnamn** = AzureAdmin<br/>**NYTT lösenord** = Contoso! 000<br/>**BEKRÄFTA** = Contoso! 000 |
| Konfiguration av virtuell dator |**MOLNTJÄNSTEN** = tidigare skapade unikt DNS-Molntjänstnamnet (ex: ContosoDC123)<br/>**REGION/TILLHÖRIGHETSGRUPP/VIRTUELLT nätverk** = ContosoNET<br/>**VIRTUELLA UNDERNÄT** = Back(10.10.2.0/24)<br/>**STORAGE-konto** = Använd en automatiskt genererad storage-konto<br/>**TILLGÄNGLIGHETSUPPSÄTTNING** = skapa en tillgänglighet ange<br/>**NAMNET PÅ TILLGÄNGLIGHETSUPPSÄTTNINGEN** = SQLHADR |**MOLNTJÄNSTEN** = tidigare skapade unikt DNS-Molntjänstnamnet (ex: ContosoDC123)<br/>**REGION/TILLHÖRIGHETSGRUPP/VIRTUELLT nätverk** = ContosoNET<br/>**VIRTUELLA UNDERNÄT** = Back(10.10.2.0/24)<br/>**STORAGE-konto** = Använd en automatiskt genererad storage-konto<br/>**TILLGÄNGLIGHETSUPPSÄTTNING** = SQLHADR (du kan också konfigurera hello tillgänglighetsuppsättning efter hello datorn har skapats. Alla tre datorer ska tilldelas toohello SQLHADR tillgänglighetsuppsättning.) |**MOLNTJÄNSTEN** = tidigare skapade unikt DNS-Molntjänstnamnet (ex: ContosoDC123)<br/>**REGION/TILLHÖRIGHETSGRUPP/VIRTUELLT nätverk** = ContosoNET<br/>**VIRTUELLA UNDERNÄT** = Back(10.10.2.0/24)<br/>**STORAGE-konto** = Använd en automatiskt genererad storage-konto<br/>**TILLGÄNGLIGHETSUPPSÄTTNING** = SQLHADR (du kan också konfigurera hello tillgänglighetsuppsättning efter hello datorn har skapats. Alla tre datorer ska tilldelas toohello SQLHADR tillgänglighetsuppsättning.) |
| Alternativ för virtuell dator |Använd standardvärden |Använd standardvärden |Använd standardvärden |

<br/>

> [!NOTE]
> hello tidigare konfigurationen föreslår standardnivån virtuella datorer eftersom grundnivån datorer inte stöder belastningsutjämnade slutpunkter. Du behöver belastningsutjämnade slutpunkter senare toocreate en tillgänglighetsgruppslyssnare. Dessutom är hello datorstorlekar som föreslås här avsedda för att testa Tillgänglighetsgrupper i Azure Virtual Machines. Hello bästa prestanda på produktionsarbetsbelastningar finns hello rekommendationer för SQL Server-datorstorlekar och konfigurationen i [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

När hello tre virtuella datorer är helt etablerad, måste toojoin dem toohello **corp.contoso.com** domän och bevilja CORP\Install administratörsbehörighet toohello datorer. toodo detta, Använd hello följande steg för varje hello tre virtuella datorer.

1. Ändra hello som önskad DNS-serveradress. Hämta varje virtuell dator RDP-filen tooyour lokal katalog genom att markera hello virtuella hello listan och klicka på hello **Anslut** knappen. tooselect en virtuell dator, klicka någonstans men hello första cellen hello rad som visas i följande skärmbild hello.
   
    ![Hämta hello RDP-filen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Öppna hello RDP-filen som du hämtade och logga in toohello virtuell dator med dina konfigurerade administratörskonto (**BUILTIN\AzureAdmin**) och lösenord (**Contoso! 000**).
3. När du loggar in, bör du se hello **Serverhanteraren** instrumentpanelen. Klicka på **lokal Server** hello vänster.
4. Klicka på hello **IPv4-adress av DHCP, IPv6 aktiverat** länk.
5. I hello **nätverksanslutningar** dialogrutan klickar du på ikonen för hello-nätverk.
   
    ![Ändra hello virtuella datorn som önskad DNS-server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Klicka på hello kommandofältet **ändra hello inställningar för den här anslutningen**. (Beroende på hello storleken på fönstret kan du kanske tooclick hello Dubbel högerpil toosee det här kommandot).
7. Välj **Internet Protocol Version 4 (TCP/IPv4)**, och klicka sedan på **egenskaper**.
8. Välj **Använd hello följande DNS-serveradresser** och sedan ange **10.10.2.4** i **önskad DNS-server**.
9. Hej **10.10.2.4** adress är hello-adress som är tilldelade tooa virtuell dator i hello 10.10.2.0/24 undernät i Azure-nätverk. Den virtuella datorn är **ContosoDC**. tooverify **ContosoDC**'s IP-adress, Använd **nslookup contosodc** i hello kommandotolk-fönster som visas i följande skärmbild hello.
   
    ![Använda NSLOOKUP toofind IP-adress för domänkontrollant](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Klicka på **OK** > **Stäng** toocommit hello ändringar. Du kan nu koppla hello virtuell dator för**corp.contoso.com**.
11. Tillbaka i hello **lokal Server** -fönstret klickar du på hello **arbetsgrupp** länk.
12. I hello **datornamn** klickar du på **ändra**.
13. Välj hello **domän** kryssrutan, skriver **corp.contoso.com** i hello textrutan och klicka sedan på **OK**.
14. I hello **Windows-säkerhet** dialogrutan Ange hello autentiseringsuppgifter för hello standard domänadministratörskonto (**CORP\AzureAdmin**) och hello lösenord (**Contoso! 000**).
15. När du ser ”Välkommen toohello corp.contoso.com domänen” hello-meddelande, klickar du på **OK**.
16. Klicka på **Stäng** > **starta om nu** hello i dialogrutan.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Lägg till hello Corp\Install användaren som administratör på varje virtuell dator
1. Vänta tills hello virtuella datorn startas om och sedan öppna hello RDP-filen igen toosign i toohello virtuell dator med hjälp av hello **BUILTIN\AzureAdmin** konto.
2. I **Serverhanteraren** klickar du på **verktyg** > **Datorhantering**.
   
    ![Datorhantering](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. I hello **Datorhantering** dialogrutan Expandera **lokala användare och grupper**, och klicka sedan på **grupper**.
4. Dubbelklicka på hello **administratörer** grupp.
5. I hello **egenskaper för administratörer** dialogrutan klickar du på hello **Lägg till** knappen.
6. Ange hello användare **CORP\Install**, och klicka sedan på **OK**. När du tillfrågas om autentiseringsuppgifter, Använd hello **AzureAdmin** konto med hello **Contoso! 000** lösenord.
7. Klicka på **OK** tooclose hello **Administratörsegenskaper** dialogrutan.

### <a name="add-hello-failover-clustering-feature-tooeach-virtual-machine"></a>Lägg till hello redundanskluster funktionen tooeach virtuell dator
1. I hello **Serverhanteraren** instrumentpanelen, klickar du på **Lägg till roller och funktioner**.
2. I hello **guiden Lägg till roller och funktioner**, klickar du på **nästa** tills toohello **funktioner** sidan.
3. Välj **redundanskluster**. När du uppmanas att lägga till andra beroende funktioner.
   
    ![Lägg till klusterfunktionen för växling vid fel toovirtual dator](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Klicka på **nästa**, och klicka sedan på **installera** på hello **bekräftelse** sidan.
5. När hello **Redundansklustring** funktionsinstallationen är klar klickar du på **Stäng**.
6. Logga ut från hello virtuell dator.
7. Upprepa hello stegen i det här avsnittet för alla tre servrar: **ContosoWSFCNode**, **ContosoSQL1**, och **ContosoSQL2**.

hello SQL Server virtuella datorer har nu etablerats och körs, men varje har hello standardalternativen för SQL Server.

## <a name="create-hello-failover-cluster"></a>Skapa hello failover-kluster
I det här avsnittet skapar du hello failover-kluster som är värd för hello-tillgänglighetsgrupp som du vill skapa senare. Nu, bör du gjort hello följande tooeach hello tre virtuella datorer som du ska använda i hello failover-kluster:

* Helt hello virtuell dator i Azure
* Ansluten hello virtuella toohello domän
* Lägga till **CORP\Install** toohello lokala administratörsgruppen
* Tillagda hello redundansklusterfunktionen

Alla dessa är krav på varje virtuell dator innan du kan ansluta den toohello failover-kluster.

Dessutom Observera att hello Azure-nätverk fungerar inte i hello samma sätt som ett lokalt nätverk. Du behöver toocreate hello klustret i hello följande ordning:

1. Skapa ett enkelnods kluster på en nod (**ContosoSQL1**).
2. Ändra hello klustrets IP-adress tooan oanvänd IP-adress (**10.10.2.101**).
3. Sätta hello-klusternamnet.
4. Lägg till andra noder hello (**ContosoSQL2** och **ContosoWSFCNode**).

Använd hello följande steg toocomplete hello uppgifter som helt konfigurerar hello-kluster.

1. Öppna hello RDP-filen för **ContosoSQL1**, och logga in med hjälp av hello domänkonto **CORP\Install**.
2. I hello **Serverhanteraren** instrumentpanelen, klickar du på **verktyg** > **Klusterhanteraren**.
3. I hello vänstra fönstret högerklickar du på **Klusterhanteraren**, och klicka sedan på **skapa ett kluster**som visas i följande skärmbild hello.
   
    ![Skapa kluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. Hej i guiden Skapa kluster, skapa ett kluster med en nod genom att gå igenom hello sidor och hello inställningar i den följande tabellen hello:
   
   | Sidan | Inställningar |
   | --- | --- |
   | Innan du börjar |Använd standardvärden |
   | Välj servrar |Typen **ContosoSQL1** i **ange servernamnet** och på **Lägg till** |
   | Verifieringsvarning |Välj **testar Nej jag inte behöver support från Microsoft för det här klustret och därför inte vill toorun hello validering. När jag klickar på nästa fortsätta skapa hello klustret**. |
   | Åtkomstpunkt för administration hello kluster |Typen **Cluster1** i **klusternamn** |
   | Bekräftelse |Använd standardvärden om du inte använder lagringsutrymmen. Se hello varning nedan. |
   
   > [!WARNING]
   > Om du använder [lagringsutrymmen](https://technet.microsoft.com/library/hh831739)som grupperar flera diskar i lagringspooler, måste du ta bort hello **lägga till alla tillgängliga lagringsenheter toohello klustret** kryssrutan på hello **bekräftelse** sidan. Om du inte avmarkerar det här alternativet, koppla hello virtuella diskar från under hello clustering processen. Därför kan visas de också inte i Diskhantering eller Explorer förrän hello lagringsutrymmen tas bort från hello kluster och anbringas på nytt med hjälp av PowerShell.
   > 
   > 
5. I hello till vänster och expanderar **Klusterhanteraren**, och klicka sedan på **Cluster1.corp.contoso.com**.
6. Rulla ned toohello i mitten av fönstret hello **klustrets kärnresurser** avsnittet och expandera hello **namn: Clutser1** information. Du bör se både hello **namn** och hello **IP-adress** resurser i hello **misslyckades** tillstånd. hello IP-adressresurs kunde inte försättas online eftersom hello-klustret tilldelas hello samma IP-adress som dator för hello, vilket är en dubblett-adress.
7. Högerklicka på hello misslyckades **IP-adress** resursen och klickar sedan på **egenskaper**.
   
    ![Egenskaper för klustret](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Välj **statisk IP-adress**, ange **10.10.2.101** i hello **adress** textrutan och klicka sedan på **OK**.
9. I hello **klustrets kärnresurser** avsnittet högerklickar du på **namn: Cluster1**, och klicka sedan på **Anslut**. Vänta tills båda resurser är online. När hello klusterresurs namn är online, uppdateras hello DC-servern med en ny Active Directory-datorkontot. Den här Active Directory-kontot kommer att användas toorun hello tillgänglighetsgruppen klustrade tjänsten senare.
10. Lägg till hello återstående noder toohello klustret. Högerklicka i trädet för hello webbläsare **Cluster1.corp.contoso.com**, och klicka sedan på **Lägg till nod**som visas i följande skärmbild hello.
    
     ![Lägga till noden toohello kluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. I hello **guiden Lägg till nod**, klickar du på **nästa** på hello **Välj servrar** lägger du till **ContosoSQL2** och **ContosoWSFCNode**  toohello listan genom att skriva hello servernamnet i **ange servernamnet** och sedan klicka på **Lägg till**. När du är klar klickar du på **nästa**.
12. På hello **verifieringsvarning** klickar du på **nr**, men i ett produktionsscenario, ska du utföra hello verifieringstester. Klicka sedan på **Nästa**.
13. På hello **bekräftelse** klickar du på **nästa** tooadd hello noder.
    
    > [!WARNING]
    > Om du använder [lagringsutrymmen](https://technet.microsoft.com/library/hh831739)som grupperar flera diskar i lagringspooler, måste du ta bort hello **lägga till alla tillgängliga lagringsenheter toohello klustret** kryssrutan. Om du inte avmarkerar det här alternativet, koppla hello virtuella diskar från under hello clustering processen. Därför kan de också visas inte i Diskhantering eller Explorer förrän hello lagringsutrymmen tas bort från klustret och anbringas på nytt med hjälp av PowerShell.
    > 
    > 
14. När hello noder läggs toohello kluster, klickar du på **Slutför**. Hanteraren för redundanskluster visas nu att klustret har tre noder och visa dem i hello **noder** behållare.
15. Logga ut från hello fjärrskrivbords-sessionen.

## <a name="prepare-hello-sql-server-instances-for-availability-groups"></a>Förbereda hello SQL Server-instanser för Tillgänglighetsgrupper
I det här avsnittet ska du göra hello följa på båda **ContosoSQL1** och **contosoSQL2**:

* Lägg till en inloggning för **NT AUTHORITY\System** med tillräcklig behörighet för att ange toohello standardinstansen för SQL Server.
* Lägg till **CORP\Install** som en sysadmin-rollen toohello standard SQL Server-instans.
* Öppna hello-brandväggen för fjärråtkomst av SQL Server.
* Aktivera hello alltid på tillgänglighet grupper.
* Ändra hello SQL Server-tjänstkontot för**CORP\SQLSvc1** och **CORP\SQLSvc2**respektive.

Dessa åtgärder kan utföras i valfri ordning. Dock hello följande kommer att gå igenom dem i ordning. Hello gör för både **ContosoSQL1** och **ContosoSQL2**:

1. Om du inte har loggat ut från hello värdserver för fjärrskrivbordssession för hello virtuell dator, ska du göra det nu.
2. Öppna hello RDP-filer för **ContosoSQL1** och **ContosoSQL2**, och logga in som **BUILTIN\AzureAdmin**.
3. Lägg till **NT AUTHORITY\System** toohello SQL Server-inloggningar med nödvändiga behörigheter. Öppna **SQL Server Management Studio**.
4. Klicka på **Anslut** tooconnect toohello standard SQL Server-instansen.
5. I **Object Explorer**, expandera **säkerhet**, och expandera sedan **inloggningar**.
6. Högerklicka på hello **NT AUTHORITY\System** inloggning och klicka sedan på **egenskaper**.
7. På hello **objekt** sidan för hello lokala servern, Välj **bevilja** för hello följande behörigheter och klicka sedan på **OK**.
   
   * ALTER någon tillgänglighetsgrupp
   * Ansluta SQL
   * Visa-Servertillstånd
8. Lägg till **CORP\Install** som en **sysadmin** rollen toohello standard SQL Server-instansen. I **Object Explorer**, högerklicka på **inloggningar**, och klicka sedan på **ny inloggning**.
9. Typen **CORP\Install** i **inloggningsnamnet**.
10. På hello **serverroller** väljer **sysadmin**, och klicka sedan på **OK**. När du har skapat hello inloggning kan du se den genom att expandera **inloggningar** i **Object Explorer**.
11. toocreate en brandväggsregel för SQL Server på hello **starta** skärmen öppnar **Windows-brandväggen med avancerad säkerhet**.
12. I hello vänster och välj **regler för inkommande trafik**. I hello högra rutan, klickar du på **ny regel**.
13. På hello **regeltyp** klickar du på **programmet** > **nästa**.
14. På hello **programmet** väljer **följande programsökväg**, typen **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** i hello textrutan och klicka sedan på **nästa**. Om du följer de här anvisningarna men använder SQL Server 2012, hello SQL Server-katalog är **MSSQL11. MSSQLSERVER**.
15. På hello **åtgärd** behåller **Tillåt hello anslutning** markerad och klicka sedan på **nästa**.
16. På hello **profil** , acceptera hello standardinställningar, och klickar sedan på **nästa**.
17. På hello **namn** anger du ett namn för regeln som **SQL Server (programregel)**, i hello **namn** text och sedan klicka på **Slutför**.
18. tooenable hello **Always On-Tillgänglighetsgrupper** funktionen på hello **starta** skärmen öppnar **SQL Server Configuration Manager**.
19. I trädet för hello webbläsaren klickar du på **SQL Server Services**, högerklicka på hello **SQL Server (MSSQLSERVER)** tjänst och klicka sedan på **egenskaper**.
20. Klicka på hello **alltid på hög tillgänglighet** väljer **aktivera Always On-Tillgänglighetsgrupper**som visas i hello följande skärmbild och klicka sedan på **tillämpa**. Klicka på **OK** i hello dialogrutan och Stäng inte hello **egenskaper** dialogrutan ännu. Hello SQL Server-tjänsten startas efter att du ändrat hello-tjänstkontot.
    
     ![Aktivera alltid på Tillgänglighetsgrupper](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. toochange hello SQL Server-tjänstkontot, klicka på hello **logga in** Skriv **CORP\SQLSvc1** (för **ContosoSQL1**) eller **CORP\SQLSvc2** () för **ContosoSQL2**) i **kontonamn**, Fyll i och bekräfta hello lösenord och klicka sedan på **OK**.
22. Hello dialogrutan som öppnas klickar du på **Ja** toorestart hello SQL Server-tjänsten. När hello SQL Server-tjänsten har startats om ändringar som du gjort i hello **egenskaper** dialogrutan är effektiva.
23. Logga ut från hello virtuella datorer.

## <a name="create-hello-availability-group"></a>Skapa hello tillgänglighetsgrupp
Du är nu redo tooconfigure en tillgänglighetsgrupp. Nedan visas en sammanfattning av vad du ska göra:

* Skapa en ny databas (**MyDB1**) på **ContosoSQL1**.
* Ta både en fullständig säkerhetskopia och en säkerhetskopia av transaktionsloggen av hello-databasen.
* Återställ hello fullständig och för loggsäkerhetskopior**ContosoSQL2** med hello **NORECOVERY** alternativet.
* Skapa hello tillgänglighetsgruppen (**AG1**) med synkront genomförande och automatisk redundans läsbara sekundära repliker.

### <a name="create-hello-mydb1-database-on-contososql1"></a>Skapa hello MyDB1 databas på ContosoSQL1
1. Om du redan inte har registrerat utanför hello fjärrskrivbordssessioner för **ContosoSQL1** och **ContosoSQL2**, gör du det nu.
2. Öppna hello RDP-filen för **ContosoSQL1**, och logga in som **CORP\Install**.
3. I **Utforskaren**under **C:\\**, skapa en katalog med namnet **säkerhetskopiering**. Du använder den här katalogen tooback upp och återställa databasen.
4. Högerklicka på hello ny katalog, peka för**dela med**, och klicka sedan på **specifika personer**som visas i följande skärmbild hello.
   
    ![Skapa en mapp för säkerhetskopiering](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Lägg till **CORP\SQLSvc1**, och sedan ge hello **Skrivskyddstyp** behörighet. Lägg till **CORP\SQLSvc2**, och sedan ge hello **Läs** behörighet enligt hello följande skärmbild och klicka sedan på **resursen**. När hello fildelning processen är klar klickar du på **klar**.
   
    ![Bevilja behörigheter för mapp för säkerhetskopiering](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. toocreate hello-databasen, från hello **starta** öppna menyn **SQL Server Management Studio**, och klicka sedan på **Anslut** tooconnect toohello standard SQL Server-instansen.
7. I **Object Explorer**, högerklicka på **databaser**, och klicka sedan på **ny databas**.
8. I **databasnamnet**, typen **MyDB1**, och klicka sedan på **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Skapa en fullständig säkerhetskopia av MyDB1 och återställa det på ContosoSQL2
1. en fullständig säkerhetskopiering av hello databasen i toomake **Object Explorer**, expandera **databaser**, högerklickar du på **MyDB1**, peka för**uppgifter**, och Klicka på **säkerhetskopiera**.
2. I hello **källa** och hålla **typ av säkerhetskopiering** ställa in också**fullständig**. I hello **mål** klickar du på **ta bort** tooremove hello standardsökväg för hello säkerhetskopian.
3. I hello **mål** klickar du på **Lägg till**.
4. I hello **filnamn** skriver  **\\ContosoSQL1\backup\MyDB1.bak**, klickar du på **OK**, och klicka sedan på **OK** igen tooback hello-databasen. När hello säkerhetskopieringen är klar klickar du på **OK** igen tooclose hello dialogrutan.
5. toomake en transaktion logga säkerhetskopiering av hello-databasen **Object Explorer**, expandera **databaser**, högerklicka på **MyDB1**, peka för**uppgifter**, och klicka sedan på **säkerhetskopiera**.
6. I **typ av säkerhetskopiering**väljer **transaktionsloggen**. Behåll hello **mål** filen sökväg set toohello något du angav tidigare och klicka sedan på **OK**. När hello säkerhetskopieringen är klar klickar du på **OK** igen.
7. toorestore hello fullständig och transaktionen loggsäkerhetskopior **ContosoSQL2**öppnar hello RDP-filen för **ContosoSQL2**, och logga in som **CORP\Install**. Lämna hello värdserver för fjärrskrivbordssession för **ContosoSQL1** öppna.
8. Från hello **starta** öppna menyn **SQL Server Management Studio**, och klicka sedan på **Anslut** tooconnect toohello standard SQL Server-instansen.
9. I **Object Explorer**, högerklicka på **databaser**, och klicka sedan på **Restore Database**.
10. I hello **källa** väljer **enhet**, och klicka på ellipsknappen hello **...** till.
11. I **Välj säkerhetskopieringsenheter**, klickar du på **Lägg till**.
12. I **säkerhetskopiera filplats**, typen  **\\ContosoSQL1\backup**, klickar du på **uppdatera**väljer **MyDB1.bak**, klickar du på  **OK**, och klicka sedan på **OK** igen. Du bör nu se hello fullständig säkerhetskopiering och hello loggsäkerhetskopiering i hello **säkerhetskopiorna toorestore** fönstret.
13. Gå toohello **alternativ** väljer **RESTORE WITH NORECOVERY** i **återställningstillstånd**, och klicka sedan på **OK** toorestore hello-databasen. Efter hello återställer åtgärden är klar klickar du på **OK**.

### <a name="create-hello-availability-group"></a>Skapa hello tillgänglighetsgrupp
1. Gå tillbaka toohello värdserver för fjärrskrivbordssession för **ContosoSQL1**. I **Object Explorer** i SQL Server Management Studio högerklickar du på **alltid på hög tillgänglighet**, och klicka sedan på **guiden Ny tillgänglighetsgrupp**som visas i hello följande skärmbild.
   
    ![Starta guiden Ny tillgänglighetsgrupp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. På hello **introduktion** klickar du på **nästa**. På hello **ange tillgänglighetsgruppens namn** anger **AG1** i **tillgänglighetsgruppens namn**, klicka på **nästa** igen.
   
    ![Guiden Ny tillgänglighetsgrupp, ange tillgänglighetsgruppens namn](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. På hello **Välj databaser** väljer **MyDB1**, och klicka sedan på **nästa**. hello databasen uppfyller hello krav för en tillgänglighetsgrupp eftersom du har utfört en fullständig säkerhetskopiering på hello avsedda primära repliken.
   
    ![Guiden Ny Availabilty grupp, Välj databaser](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. på hello **ange repliker** klickar du på **lägga till replik**.
   
    ![Guiden Ny Availabilty grupp, ange repliker](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. I hello **ansluta tooServer** skriver **ContosoSQL2** i **servernamn**, och klicka sedan på **Anslut**.
   
    ![Ny Availabilty grupp guiden Anslut tooserver](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Tillbaka på hello **ange repliker** sidan du bör nu se **ContosoSQL2** som anges i **tillgängliga repliker**. Konfigurera hello repliker som visas i följande skärmbild hello. När du är klar klickar du på **nästa**.
   
    ![Guiden Ny Availabilty grupp, ange repliker (slutför)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. På hello **Välj inledande datasynkronisering** väljer **Anslut endast**, och klicka sedan på **nästa**. Du redan utfört datasynkronisering manuellt när du har gjort hello full och transaktionen säkerhetskopieringar på **ContosoSQL1** och återställa dem på **ContosoSQL2**. Du kan välja inte tooperform hello åtgärder för säkerhetskopiering och återställning på databasen och välj i stället **fullständig** toolet hello guiden Ny tillgänglighetsgrupp utföra datasynkronisering åt dig. Vi rekommenderar dock inte det här alternativet för mycket stora databaser som hittas i vissa företag.
   
    ![Guiden Ny Availabilty, Välj inledande datasynkronisering](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. På hello **validering** klickar du på **nästa**. Den här sidan bör se ut ungefär toohello följande skärmbild. Det finns en varning för hello Lyssnarkonfigurationen eftersom du inte har konfigurerat en tillgänglighetsgruppslyssnare. Du kan ignorera den här varningen, eftersom den här kursen inte konfigurera en lyssnare. tooconfigure hello lyssnare när du har slutfört den här självstudiekursen, se [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).
   
    ![Guiden Ny Availabilty servergrupp, validering](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. På hello **sammanfattning** klickar du på **Slutför**, och sedan vänta medan hello guiden konfigurerar hello nya tillgänglighetsgruppen. På hello **förlopp** sidan som du kan klicka på **mer** tooview hello detaljerad pågår. När hello-guiden har slutförts inspektera hello **resultat** sidan tooverify hello tillgänglighetsgruppen har skapats, som visas i följande skärmbild hello och klicka sedan på **Stäng** tooexit hello guiden.
   
    ![Guiden Ny Availabilty servergrupp, resultat](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. I **Object Explorer**, expandera **alltid på hög tillgänglighet**, och expandera sedan **Tillgänglighetsgrupper**. Du bör nu se hello nya tillgänglighetsgruppen i den här behållaren. Högerklicka på **AG1 (primär)**, och klicka sedan på **visa instrumentpanelen**.
    
     ![Visa Availability Group-instrumentpanelen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Din **alltid på instrumentpanelen** utseende liknande toohello något i hello följande skärmbild. Du kan se hello repliker, hello redundansläge för varje replik och tillstånd för synkronisering av hello.
    
     ![Tillgänglighetsgruppen instrumentpanelen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Returnera för**Serverhanteraren**, klickar du på **verktyg**, och sedan öppna **Klusterhanteraren**.
13. Expandera **Cluster1.corp.contoso.com**, och expandera sedan **tjänster och program**. Välj **roller** och Observera att hello **AG1** tillgänglighet grupp rollen har skapats. Observera att AG1 inte har en IP-adress genom vilken databas som klienter kan ansluta toohello tillgänglighetsgruppen, eftersom du inte konfigurerar en lyssnare. Du kan ansluta direkt toohello primära noden för Läs-och skrivåtgärder och hello sekundära noden för skrivskyddade frågor.
    
     ![AG i hanteraren för redundanskluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Försök inte toofail över hello tillgänglighetsgruppen från hello hanteraren för redundanskluster. Alla redundansåtgärder ska utföras inifrån **alltid på instrumentpanelen** i SQL Server Management Studio. Mer information finns i [begränsningar med hjälp av hello Klusterhanterare med Tillgänglighetsgrupper](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Nästa steg
Du har nu har implementerat SQL Server alltid aktiverad genom att skapa en tillgänglighetsgrupp i Azure. tooconfigure en lyssnare för den här tillgänglighetsgruppen finns [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).

Annan information om hur du använder SQL Server i Azure, se [SQL Server på Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

