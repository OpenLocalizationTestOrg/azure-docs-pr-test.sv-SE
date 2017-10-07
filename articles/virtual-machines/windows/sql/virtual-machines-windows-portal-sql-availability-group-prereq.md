---
title: "aaaSQL servertillgänglighet grupper - virtuella datorer i Azure - förutsättningar för | Microsoft Docs"
description: "Den här kursen visar hur tooconfigure hello förutsättningar för att skapa en SQL Server alltid aktiverad tillgänglighet gruppen på Azure Virtual Machines."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: eed0729ead25c7793bb17a04cd7fd996c7dc8c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="complete-hello-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Slutföra hello förutsättningar för att skapa Always On-Tillgänglighetsgrupper på Azure virtual machines

Den här kursen visar hur toocomplete hello förutsättningar för att skapa en [SQL Server Always On-tillgänglighetsgruppen på virtuella Azure-datorer (VM)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). När du är klar hello krav har en domänkontrollant, två virtuella SQL Server-datorer och ett vittnesserver i en enskild resursgrupp.

**Tid uppskattning**: Det kan ta ett par timmar toocomplete hello krav. Mycket av den här tiden går åt för att skapa virtuella datorer.

hello följande diagram visar vad du bygga hello kursen.

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Läs dokumentationen om tillgänglighet

Den här kursen förutsätter att du har en grundläggande förståelse för SQL Server Always On-Tillgänglighetsgrupper. Om du inte är bekant med den här tekniken finns [översikt över alltid på Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Skapa ett Azure-konto
Du behöver ett Azure-konto. Du kan [öppna ett kostnadsfritt Azure-konto](/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
1. Logga in toohello [Azure-portalen](http://portal.azure.com).
2. Klicka på  **+**  toocreate ett nytt objekt i hello-portalen.

   ![Nytt objekt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Typen **resursgruppen** i hello **Marketplace** sökfönstret.

   ![Resursgrupp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Klicka på **resursgruppen**.
5. Klicka på **Skapa**.
6. På hello **resursgruppen** bladet under **resursgruppens namn**, Skriv ett namn för hello resursgruppen. Skriv till exempel **sql-hög tillgänglighet-rg**.
7. Om du har flera Azure-prenumerationer måste du kontrollera att hello prenumeration är hello Azure-prenumeration som du vill ha toocreate hello-tillgänglighetsgruppen.
8. Välj en plats. hello platsen är hello Azure-region där du vill att toocreate hello-tillgänglighetsgruppen. Den här självstudiekursen kommer vi toobuild alla resurser i en Azure-plats.
9. Kontrollera att **PIN-kod toodashboard** är markerad. Den här valfria inställningen placerar en genväg för hello resursgrupp på hello Azure portalens instrumentpanel.

   ![Resursgrupp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Klicka på **skapa** toocreate hello resursgruppen.

Azure skapar hello resursgrupp och PIN-koder som en genväg toohello resursgrupp hello-portalen.

## <a name="create-hello-network-and-subnets"></a>Skapa hello nätverk och undernät
hello nästa steg är toocreate hello nätverk och undernät i hello Azure-resursgrupp.

hello lösningen använder ett virtuellt nätverk med två undernät. Hej [översikt över virtuella nätverk](../../../virtual-network/virtual-networks-overview.md) innehåller mer information om nätverk i Azure.

toocreate hello virtuellt nätverk:

1. Klicka på hello Azure-portalen i resursgruppen, **+ Lägg till**. Azure öppnas hello **allt** bladet.

   ![Nytt objekt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Sök efter **virtuellt nätverk**.

     ![Sökningen virtuellt nätverk](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Klicka på **för virtuella nätverk**.
4. På hello **för virtuella nätverk** bladet, klickar du på hello **Resource Manager** distributionsmodell och klicka sedan på **skapa**.

    hello visar följande tabell hello inställningarna för hello-nätverket:

   | **Fält** | Värde |
   | --- | --- |
   | **Namn** |autoHAVNET |
   | **Adressutrymme** |10.33.0.0/24 |
   | **Namn på undernät** |Admin |
   | **Adressintervall för undernätet** |10.33.0.0/29 |
   | **Prenumeration** |Ange hello-prenumeration som du avser toouse. **Prenumerationen** är tom om du bara har en prenumeration. |
   | **Resursgrupp** |Välj **använda befintliga** och välj hello namnet på hello resursgrupp. |
   | **Plats** |Ange hello Azure-plats. |

   Adress adressutrymmet och undernätsegenskaperna adressintervallet kan skilja sig från hello tabell. Beroende på din prenumeration föreslår hello portal en tillgängliga adressutrymmet och motsvarande adressintervallet i undernätet. Om det finns inte tillräckligt med adressutrymme Använd en annan prenumeration.

   hello-exempel som använder hello undernätsnamn **Admin**. Det här undernätet är hello-domänkontrollanter.

5. Klicka på **Skapa**.

   ![Konfigurera hello virtuellt nätverk](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure returnerar toohello portalens instrumentpanel och meddelar dig när hello nytt nätverk skapas.

### <a name="create-a-second-subnet"></a>Skapa ett andra undernät
hello nytt virtuellt nätverk har ett undernät, med namnet **Admin**. hello domänkontrollanter använder det här undernätet. hello virtuella SQL Server-datorer använder ett andra undernät med namnet **SQL**. tooconfigure undernätet:

1. Klicka på din instrumentpanel hello resursgrupp som du skapade **SQL-hög tillgänglighet-RG**. Hitta hello nätverk i hello resursgruppen under **resurser**.

    Om **SQL-hög tillgänglighet-RG** inte visas, hitta genom att klicka på **resursgrupper** och filtrering av hello resursgruppens namn.
2. Klicka på **autoHAVNET** hello listan över resurser. Azure öppnas hello nätverksblad konfiguration.
3. På hello **autoHAVNET** virtuellt nätverksblad under **inställningar** , klickar du på **undernät**.

    Observera hello undernät som du redan skapat.

   ![Konfigurera hello virtuellt nätverk](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Skapa en andra undernät. Klicka på **+ undernät**.
6. På hello **Lägg till undernät** bladet konfigurera hello-undernät genom att skriva **sqlsubnet** under **namn**. Azure automatiskt anger en giltig **adressintervall**. Kontrollera att detta adressintervall har minst 10 adresser i den. Du kan behöva flera adresser i en produktionsmiljö.
7. Klicka på **OK**.

    ![Konfigurera hello virtuellt nätverk](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

hello följande tabell sammanfattas hello nätverksinställningar för konfiguration:

| **Fält** | Värde |
| --- | --- |
| **Namn** |**autoHAVNET** |
| **Adressutrymme** |Det här värdet är beroende av hello tillgängliga adressutrymmen i din prenumeration. Ett vanligt värde är 10.0.0.0/16. |
| **Namn på undernät** |**Admin** |
| **Adressintervall för undernätet** |Det här värdet är beroende av hello tillgängliga adressintervall i din prenumeration. Ett vanligt värde är 10.0.0.0/24. |
| **Namn på undernät** |**sqlsubnet** |
| **Adressintervall för undernätet** |Det här värdet är beroende av hello tillgängliga adressintervall i din prenumeration. Ett vanligt värde är 10.0.1.0/24. |
| **Prenumeration** |Ange hello-prenumeration som du avser toouse. |
| **Resursgrupp** |**SQL-HÖG TILLGÄNGLIGHET-RG** |
| **Plats** |Ange hello samma plats som du valde för hello resursgruppen. |

## <a name="create-availability-sets"></a>Skapa tillgänglighetsuppsättningar

Innan du skapar virtuella datorer måste toocreate tillgänglighetsuppsättningar. Tillgänglighetsuppsättningar minska hello driftstopp för planerat eller oplanerat underhållshändelser. En Azure tillgänglighetsuppsättning är en logisk grupp med resurser som Azure placerar fysiska feldomäner och update-domäner. En feldomän garanterar att hello medlemmar i hello tillgänglighetsuppsättning har separata power och nätverksresurser. En uppdateringsdomän garanterar att medlemmar i hello tillgänglighetsuppsättning inte återspeglas för underhåll på hello samma tid. Mer information finns i [hantera hello tillgängligheten för virtuella datorer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Du behöver två tillgänglighetsuppsättningar. En är hello-domänkontrollanter. hello gäller andra hello SQL Server-datorer.

toocreate tillgänglighet ange går toohello resursgrupp och klicka på **Lägg till**. Filtrera hello resultat genom att skriva **tillgänglighetsuppsättning**. Klicka på **Tillgänglighetsuppsättning** i hello resultaten och klickar sedan på **skapa**.

Konfigurera två tillgänglighetsuppsättningar enligt toohello parametrar i den följande tabellen hello:

| **Fält** | Domain controller tillgänglighetsuppsättning | Tillgänglighetsuppsättningen för SQL Server |
| --- | --- | --- |
| **Namn** |adavailabilityset |sqlavailabilityset |
| **Resursgrupp** |SQL-HÖG TILLGÄNGLIGHET-RG |SQL-HÖG TILLGÄNGLIGHET-RG |
| **Feldomäner** |3 |3 |
| **Uppdatera domäner** |5 |3 |

När du har skapat hello tillgänglighetsuppsättningar returnera toohello resursgrupp i hello Azure-portalen.

## <a name="create-domain-controllers"></a>Skapa domänkontrollanter
När du har skapat hello nätverk, undernät, tillgänglighetsuppsättningar och en Internetriktade belastningsutjämnare, är du redo toocreate hello virtuella datorer för hello-domänkontrollanter.

### <a name="create-virtual-machines-for-hello-domain-controllers"></a>Skapa virtuella datorer för hello-domänkontrollanter
toocreate och konfigurera hello domänkontrollanter, returnera toohello **SQL-hög tillgänglighet-RG** resursgruppen.

1. Klicka på **Lägg till**. Hej **allt** blad öppnas.
2. Typen **Windows Server 2016 Datacenter**.
3. Klicka på **Windows Server 2016 Datacenter**. I hello **Windows Server 2016 Datacenter** bladet, kontrollera att hello distributionsmodell är **Resource Manager**, och klicka sedan på **skapa**. Azure öppnas hello **Skapa virtuell dator** bladet.

Upprepa föregående steg toocreate två virtuella datorer hello. Namn hello två virtuella datorer:

* AD-primära-dc
* AD-sekundär-dc

  > [!NOTE]
  > Hej **ad-sekundär-dc** virtuella datorn är valfritt, tooprovide hög tillgänglighet för Active Directory Domain Services.
  >
  >

hello visar följande tabell hello inställningarna för dessa två datorer:

| **Fält** | Värde |
| --- | --- |
| **Namn** |Första domänkontrollant: *ad primär domänkontrollant*.</br>Andra domänkontrollanten *ad-sekundär-dc*. |
| **Typ av virtuell datordisk** |SSD |
| **Användarnamn** |DomainAdmin |
| **Lösenord** |Contoso! 0000 |
| **Prenumeration** |*Din prenumeration* |
| **Resursgrupp** |SQL-HÖG TILLGÄNGLIGHET-RG |
| **Plats** |*Din plats* |
| **Storlek** |DS1_V2 |
| **Storage** | **Använda hanterade diskar** - **Ja** |
| **Virtuellt nätverk** |autoHAVNET |
| **Undernät** |Admin |
| **Offentlig IP-adress** |*Samma namn som hello VM* |
| **Nätverkssäkerhetsgrupp** |*Samma namn som hello VM* |
| **Tillgänglighetsuppsättning** |adavailabilityset </br>**Fault domäner**: 2</br>**Uppdatera domäner**: 2|
| **Diagnostik** |Enabled |
| **Diagnostiklagringskonto** |*Skapas automatiskt* |

   >[!IMPORTANT]
   >Du kan bara placera en virtuell dator i en tillgänglighetsuppsättning när du skapar den. Du kan inte ändra hello tillgänglighetsuppsättning när en virtuell dator skapas. Se [hantera hello tillgängligheten för virtuella datorer](../manage-availability.md).

Azure skapar hello virtuella datorer.

När hello virtuella datorer har skapats kan du konfigurera hello domänkontrollant.

### <a name="configure-hello-domain-controller"></a>Konfigurera hello-domänkontrollant
Följande steg, konfigurera hello hello **ad primär domänkontrollant** maskin som en domänkontrollant för corp.contoso.com.

1. Öppna hello i hello portal **SQL-hög tillgänglighet-RG** resursgrupp och välj hello **ad primär domänkontrollant** datorn. På hello **ad primär domänkontrollant** bladet, klickar du på **Anslut** tooopen en RDP-filen för fjärråtkomst till skrivbordet.

    ![Ansluta tooa virtuell dator](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Logga in med ditt konfigurerade administratörskonto (**\DomainAdmin**) och lösenord (**Contoso! 0000**).
3. Som standard hello **Serverhanteraren** instrumentpanelen ska visas.
4. Klicka på hello **Lägg till roller och funktioner** länk på hello instrumentpanelen.

    ![Server Manager - Lägg till roller](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Välj **nästa** tills toohello **serverroller** avsnitt.
6. Välj hello **Active Directory Domain Services** och **DNS-Server** roller. När du uppmanas att lägga till eventuella ytterligare funktioner som krävs av rollerna.

   > [!NOTE]
   > Windows varnar dig om att det finns ingen statisk IP-adress. Om du testar hello-konfiguration klickar du på **Fortsätt**. Ange hello IP-adress toostatic för produktion scenarier i hello Azure-portalen eller [PowerShell tooset hello statisk IP-adress för hello domain controller datorn](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Lägg till roller dialogrutan](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Klicka på **nästa** tills du når hello **bekräftelse** avsnitt. Välj hello **omstart hello-målservern automatiskt om det behövs** kryssrutan.
8. Klicka på **Installera**.
9. När hello funktioner Slutför installation, returnerar toohello **Serverhanteraren** instrumentpanelen.
10. Välj ny hello **AD DS** alternativet på hello vänstra fönstret.
11. Klicka på hello **mer** länk hello gul varning-fältet.

    ![AD DS-dialogen för hello DNS Server VM](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. I hello **åtgärd** kolumn i hello **alla Server aktivitetsinformation** dialogrutan klickar du på **befordra den här servern tooa domänkontrollanten**.
13. I hello **konfigurationsguiden Active Directory Domain Services**, använda hello följande värden:

    | **Sidan** | Inställning |
    | --- | --- |
    | **Distributionskonfiguration** |**Lägg till en ny skog**<br/> **Rotdomännamn** = corp.contoso.com |
    | **Alternativ för domänkontrollant** |**DSRM-lösenordet** = Contoso! 0000<br/>**Bekräfta lösenord** = Contoso! 0000 |
14. Klicka på **nästa** toogo via hello andra sidor i guiden hello. På hello **Kravkontroll** kontrollerar du att du ser följande meddelande hello: **alla kravkontroller har lyckats**. Du kan granska alla meddelanden, men det är möjligt toocontinue med hello-installation.
15. Klicka på **Installera**. Hej **ad primär domänkontrollant** virtuella datorn startar om automatiskt.

### <a name="note-hello-ip-address-of-hello-primary-domain-controller"></a>Observera hello IP-adressen för primär domänkontrollant för hello

Använd hello primära domänkontrollanten för DNS. Observera hello primär domän controller IP-adress.

Enkelriktade tooget hello primär domän controller IP-adress är via hello Azure-portalen.

1. Öppna hello resursgruppen på hello Azure-portalen.

2. Klicka på hello primär domänkontrollant.

3. Klicka på hello primär domän controller bladet **nätverksgränssnitt**.

![Nätverksgränssnitt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Observera hello privata IP-adressen för den här servern.

### <a name="configure-hello-virtual-network-dns"></a>Konfigurera hello virtuellt nätverk DNS
När du skapar hello första domänkontrollanten och aktivera DNS på hello första servern kan du konfigurera hello virtuellt nätverk toouse servern för DNS.

1. Klicka på hello virtuellt nätverk i hello Azure-portalen.

2. Under **inställningar**, klickar du på **DNS-Server**.

3. Klicka på **anpassade**, och Skriv hello privata IP-adressen för hello primära domänkontrollanten.

4. Klicka på **Spara**.

### <a name="configure-hello-second-domain-controller"></a>Konfigurera hello andra domänkontrollanter
När hello primära domänkontrollanten startar om, kan du konfigurera hello andra domänkontrollanten. Det här valfria steget är för hög tillgänglighet. Följ dessa steg tooconfigure hello andra domänkontrollanten:

1. Öppna hello i hello portal **SQL-hög tillgänglighet-RG** resursgrupp och välj hello **ad-sekundär-dc** datorn. På hello **ad-sekundär-dc** bladet, klickar du på **Anslut** tooopen en RDP-filen för fjärråtkomst till skrivbordet.
2. Logga in toohello VM med dina konfigurerade administratörskonto (**BUILTIN\DomainAdmin**) och lösenord (**Contoso! 0000**).
3. Ändra hello önskad DNS-adress toohello serveradress för hello-domänkontrollant.
4. I **nätverks- och delningscenter**, klicka på hello nätverksgränssnitt.
   ![Nätverksgränssnitt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Klicka på **Egenskaper**.
6. Välj **Internet Protocol Version 4 (TCP/IPv4)** och på **egenskaper**.
7. Välj **Använd hello följande DNS-serveradresser** och ange hello-adressen för hello primära domänkontrollanten i **önskad DNS-server**.
8. Klicka på **OK**, och sedan **Stäng** toocommit hello ändringar. Du är nu kan toojoin hello VM för**corp.contoso.com**.

   >[!IMPORTANT]
   >Om du förlorar anslutningen hello tooyour Fjärrskrivbord när du har ändrat hello DNS-inställningen går toohello virtuella Azure-portalen och starta om hello-datorn.

9. Hello remote desktop toohello sekundär domänkontrollant, öppna **instrumentpanelen Serverhanteraren**.
10. Klicka på hello **Lägg till roller och funktioner** länk på hello instrumentpanelen.

    ![Server Manager - Lägg till roller](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Välj **nästa** tills toohello **serverroller** avsnitt.
12. Välj hello **Active Directory Domain Services** och **DNS-Server** roller. När du uppmanas att lägga till eventuella ytterligare funktioner som krävs av rollerna.
13. När hello funktioner Slutför installation, returnerar toohello **Serverhanteraren** instrumentpanelen.
14. Välj ny hello **AD DS** alternativet på hello vänstra fönstret.
15. Klicka på hello **mer** länk hello gul varning-fältet.
16. I hello **åtgärd** kolumn i hello **alla Server aktivitetsinformation** dialogrutan klickar du på **befordra den här servern tooa domänkontrollanten**.
17. Under **distributionskonfigurationen**väljer **lägga till en befintlig domän för domain controller tooan**.
   ![Distributionskonfiguration](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. Klicka på **Välj**.
19. Ansluta med hjälp av hello administratörskonto (**CORP. Contoso.COM\domainadmin**) och lösenord (**Contoso! 0000**).
20. I **Välj en domän från hello skog**, klickar du på din domän och klicka sedan på **OK**.
21. I **alternativ för domänkontrollant**, använda hello standardvärden och ange DSRM-lösenordet.

   >[!NOTE]
   >Hej **DNS-alternativ** sida kan varna dig att det inte går att skapa en delegering för den här DNS-servern. Du kan ignorera den här varningen i icke-produktionsmiljöer.
22. Klicka på **nästa** tills hello dialogrutan hello **krav** kontrollera. Klicka på **Installera**.

Starta om hello server när hello server har slutförts hello konfigurationsändringar.

### <a name="add-hello-private-ip-address-toohello-second-domain-controller-toohello-vpn-dns-server"></a>Lägg till hello privata IP-adressen toohello andra domain controller toohello VPN-DNS-Server

Ändra hello DNS-Server tooinclude hello IP-adressen för hello sekundär domänkontrollant i hello Azure-portalen under virtuellt nätverk. Detta gör att hello DNS-tjänsten redundans.

### <a name=DomainAccounts></a>Konfigurera hello domänkonton

I nästa steg för hello konfigurerar du hello Active Directory-konton. hello visar följande tabell hello konton:

| |Kontot för installation<br/> |SQLServer-0 <br/>Konto för SQL Server och SQL Agent-tjänsten |SQLServer-1<br/>Konto för SQL Server och SQL Agent-tjänsten
| --- | --- | --- | ---
|**Förnamn** |Installera |SQLSvc1 | SQLSvc2
|**Användare SamAccountName** |Installera |SQLSvc1 | SQLSvc2

Använd följande steg toocreate hello varje konto.

1. Logga in toohello **ad primär domänkontrollant** datorn.
2. I **Serverhanteraren**väljer **verktyg**, och klicka sedan på **Active Directory Administrationscenter**.   
3. Välj **corp (lokal)** från hello till vänster.
4. På hello rätt **uppgifter** väljer **ny**, och klicka sedan på **användaren**.
   ![Active Directory Administrationscenter](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Ange ett komplext lösenord för varje konto.<br/> För icke-produktionsmiljöer, ange hello användarens konto toonever upphöra att gälla.

5. Klicka på **OK** toocreate hello användare.
6. Upprepa föregående steg för varje hello tre konton hello.

### <a name="grant-hello-required-permissions-toohello-installation-account"></a>Ge hello krävs behörigheter toohello installationskonto
1. I hello **Active Directory Administrationscenter**väljer **corp (lokal)** hello vänster. I högra hello **uppgifter** rutan klickar du på **egenskaper**.

    ![Egenskaper för CORP-användare](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Välj **tillägg**, och klicka sedan på hello **Avancerat** hello-knappen **säkerhet** fliken.
3. I hello **avancerade säkerhetsinställningar för corp** dialogrutan klickar du på **Lägg till**.
4. Klicka på **Välj en huvudansvarig**, söka efter **CORP\Install**, och klicka sedan på **OK**.
5. Välj hello **läsa alla egenskaper** kryssrutan.

6. Välj hello **skapa datorobjekt** kryssrutan.

     ![Corp användarbehörighet](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Klicka på **OK**, och klicka sedan på **OK** igen. Stäng hello **corp** egenskapsfönstret.

Nu när du har konfigurerat Active Directory och hello användarobjekt, skapa två virtuella SQL Server-datorer och en vittnesserver som VM. Ansluta alla tre toohello-domänen.

## <a name="create-sql-server-vms"></a>Skapa virtuella SQL Server-datorer

Skapa tre ytterligare virtuella datorer. hello lösning kräver två virtuella datorer med SQL Server-instanser. En tredje virtuell dator fungerar som ett vittne. Windows Server 2016 kan använda en [molnet vittne](http://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), men adressöverensstämmelse tidigare operativsystem för det här dokumentet använder en virtuell dator för ett vittne.  

Överväg följande beslut deisign hello innan du fortsätter.

* **Lagring – Azure hanterade diskar**

   Använd Azure hanterade diskar för hello den virtuella datorns lagringsutrymme. Microsoft rekommenderar hanterade diskar för virtuella datorer för SQL Server. Hanterade diskar handtag lagring hello bakgrunden. Dessutom när virtuella datorer med hanterade diskar är i hello samma tillgänglighetsuppsättning, Azure distribuerar hello lagring resurser tooprovide lämplig redundans. Mer information finns i [Översikt över Azure Managed Disks](../managed-disks-overview.md). Närmare information om hanterade diskar i en tillgänglighetsuppsättning, se [använda hanterade diskar för virtuella datorer i en tillgänglighetsuppsättning](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Nätverk - privata IP-adresser i produktion**

   Den här kursen använder offentliga IP-adresser för hello virtuella datorer. Detta gör att fjärranslutning direkt toohello virtuella datorn via hello internet - det underlättar konfigurationssteg. I produktionsmiljöer rekommenderas endast privata IP-adresser i ordning tooreduce hello sårbarhet av hello SQL Server-instansen Virtuella datorresursen.

### <a name="create-and-configure-hello-sql-server-vms"></a>Skapa och konfigurera hello SQL Server-datorer
Skapa därefter tre virtuella datorer – två virtuella SQL Server-datorer och en virtuell dator för en ny klusternod. toocreate hello virtuella datorer, gå tillbaka toohello **SQL-hög tillgänglighet-RG** resursgrupp, klickar du på **Lägg till**, söka efter hello lämpliga galleriobjektet, klickar du på **virtuella**, och sedan Klicka på **från galleriet**. Använd hello information i följande tabell toohelp som du skapar virtuella datorer hello hello:


| Sidan | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Välj hello lämpliga galleriobjektet |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise på Windows Server 2016** |**SQL Server 2016 SP1 Enterprise på Windows Server 2016** |
| Konfiguration av virtuell dator **grunderna** |**Namnet** = klustret fsw<br/>**Användarnamnet** = DomainAdmin<br/>**Lösenordet** = Contoso! 0000<br/>**Prenumerationen** = din prenumeration<br/>**Resursgruppen** = SQL-hög tillgänglighet-RG<br/>**Plats** = azure-plats |**Namnet** = sqlserver-0<br/>**Användarnamnet** = DomainAdmin<br/>**Lösenordet** = Contoso! 0000<br/>**Prenumerationen** = din prenumeration<br/>**Resursgruppen** = SQL-hög tillgänglighet-RG<br/>**Plats** = azure-plats |**Namnet** = sqlserver-1<br/>**Användarnamnet** = DomainAdmin<br/>**Lösenordet** = Contoso! 0000<br/>**Prenumerationen** = din prenumeration<br/>**Resursgruppen** = SQL-hög tillgänglighet-RG<br/>**Plats** = azure-plats |
| Konfiguration av virtuell dator **storlek** |**STORLEK** = DS1\_V2 (1 kärna, 3.5 GB) |**STORLEK** = DS2\_V2 (2 kärnor, 7 GB)</br>hello storlek måste ha stöd för SSD-lagring (stöd för Premium-diskar. )) |**STORLEK** = DS2\_V2 (2 kärnor, 7 GB) |
| Konfiguration av virtuell dator **inställningar** |**Lagring**: Använd hanterade diskar.<br/>**Virtuellt nätverk** = autoHAVNET<br/>**Undernät** = sqlsubnet(10.1.1.0/24)<br/>**Offentliga IP-adressen** skapas automatiskt.<br/>**Nätverkssäkerhetsgruppen** = None<br/>**Övervaka diagnostik** = aktiverat<br/>**Diagnostiklagringskonto** = Använd en automatiskt genererad storage-konto<br/>**Tillgänglighetsuppsättningen** = sqlAvailabilitySet<br/> |**Lagring**: Använd hanterade diskar.<br/>**Virtuellt nätverk** = autoHAVNET<br/>**Undernät** = sqlsubnet(10.1.1.0/24)<br/>**Offentliga IP-adressen** skapas automatiskt.<br/>**Nätverkssäkerhetsgruppen** = None<br/>**Övervaka diagnostik** = aktiverat<br/>**Diagnostiklagringskonto** = Använd en automatiskt genererad storage-konto<br/>**Tillgänglighetsuppsättningen** = sqlAvailabilitySet<br/> |**Lagring**: Använd hanterade diskar.<br/>**Virtuellt nätverk** = autoHAVNET<br/>**Undernät** = sqlsubnet(10.1.1.0/24)<br/>**Offentliga IP-adressen** skapas automatiskt.<br/>**Nätverkssäkerhetsgruppen** = None<br/>**Övervaka diagnostik** = aktiverat<br/>**Diagnostiklagringskonto** = Använd en automatiskt genererad storage-konto<br/>**Tillgänglighetsuppsättningen** = sqlAvailabilitySet<br/> |
| Konfiguration av virtuell dator **SQL Server-inställningar** |Inte tillämpligt |**SQL-anslutningen** = privat (inom Virtual Network)<br/>**Port** = 1433<br/>**SQL-autentisering** = inaktivera<br/>**Lagringskonfigurationen** = Allmänt<br/>**Automatisk uppdatering** = söndag 2:00<br/>**Automatisk säkerhetskopiering** = inaktiverad</br>**Azure Key Vault-integrering** = inaktiverad |**SQL-anslutningen** = privat (inom Virtual Network)<br/>**Port** = 1433<br/>**SQL-autentisering** = inaktivera<br/>**Lagringskonfigurationen** = Allmänt<br/>**Automatisk uppdatering** = söndag 2:00<br/>**Automatisk säkerhetskopiering** = inaktiverad</br>**Azure Key Vault-integrering** = inaktiverad |

<br/>

> [!NOTE]
> Hej datorstorlekar föreslås här är avsedda för att testa Tillgänglighetsgrupper i virtuella Azure-datorer. Hello bästa prestanda på produktionsarbetsbelastningar finns hello rekommendationer för SQL Server-datorstorlekar och konfigurationen i [prestandarelaterade Metodtips för SQL Server på virtuella Azure-datorer](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

När hello tre virtuella datorer är helt etablerad, måste toojoin dem toohello **corp.contoso.com** domän och bevilja CORP\Install administratörsbehörighet toohello datorer.

### <a name="joinDomain"></a>Anslut till hello servrar toohello domän

Du är nu kan toojoin hello virtuella datorer för**corp.contoso.com**. Delar hello följande för både hello SQL Server-datorer och hello vittnesserver:

1. Fjärransluta toohello virtuell dator med **BUILTIN\DomainAdmin**.
2. I **Serverhanteraren**, klickar du på **lokal Server**.
3. Klicka på hello **arbetsgrupp** länk.
4. I hello **datornamn** klickar du på **ändra**.
5. Välj hello **domän** kryssrutan och ange **corp.contoso.com** i hello textruta. Klicka på **OK**.
6. I hello **Windows-säkerhet** popup dialogrutan Ange hello autentiseringsuppgifter för hello standard domänadministratörskonto (**CORP\DomainAdmin**) och hello lösenord (**Contoso! 0000**).
7. När du ser ”Välkommen toohello corp.contoso.com domänen” hello-meddelande, klickar du på **OK**.
8. Klicka på **Stäng**, och klicka sedan på **starta om nu** i dialogrutan för hello popup-fönster.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Lägg till hello Corp\Install användaren som administratör på varje kluster VM

Varje virtuell dator startar om som en medlem i hello domän och lägga till **CORP\Install** som medlem i gruppen lokala administratörer för hello.

1. Vänta tills hello VM har startats om och sedan starta hello RDP-filen igen från hello primär domän controller toosign i för**sqlserver-0** med hjälp av hello **CORP\DomainAdmin** konto.
   >[!TIP]
   >Kontrollera att du loggar in med hello domänadministratörskonto. I föregående steg hello använde hello i inbyggda administratörskontot. Nu hello servern finns i hello domän, använda hello domänkonto. Ange i RDP-session *domän*\\*användarnamn*.

2. I **Serverhanteraren**väljer **verktyg**, och klicka sedan på **Datorhantering**.
3. I hello **Datorhantering** fönstret expanderar **lokala användare och grupper**, och välj sedan **grupper**.
4. Dubbelklicka på hello **administratörer** grupp.
5. I hello **egenskaper för administratörer** dialogrutan klickar du på hello **Lägg till** knappen.
6. Ange hello användare **CORP\Install**, och klicka sedan på **OK**.
7. Klicka på **OK** tooclose hello **Administratörsegenskaper** dialogrutan.
8. Upprepa föregående steg för hello på **sqlserver-1** och **klustret fsw**.

### <a name="setServiceAccount"></a>Ange hello SQL Server-tjänstkonton

Ange hello SQL Server-tjänstkontot på varje SQL Server-VM. Använd hello konton som du skapade när du [konfigurerats hello domänkonton](#DomainAccounts).

1. Öppna **SQL Server Configuration Manager**.
2. Högerklicka på hello SQL Server-tjänsten och klicka sedan på **egenskaper**.
3. Ange hello konto och lösenord.
4. Upprepa dessa steg på hello andra SQL Server-VM.  

För SQL Server-Tillgänglighetsgrupper måste varje SQL Server-VM toorun som ett domänkonto.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-hello-installation-account"></a>Skapa en inloggning på varje SQL Server-VM för kontot för installation av hello

Använd hello installationen konto (CORP\install) tooconfigure hello-tillgänglighetsgruppen. Det här kontot måste toobe medlem i hello **sysadmin** fasta serverrollen på varje SQL Server-VM. hello följande steg skapar en inloggning för hello installationskonto:

1. Ansluta toohello server via hello Remote Desktop Protocol (RDP) med hjälp av hello  *\<MachineName\>\DomainAdmin* konto.

1. Öppna SQL Server Management Studio och Anslut toohello lokal instans av SQL Server.

1. I **Object Explorer**, klickar du på **säkerhet**.

1. Högerklicka på **inloggningar**. Klicka på **ny inloggning**.

1. I **inloggning - ny**, klickar du på **Sök**.

1. Klicka på **platser**.

1. Ange hello autentiseringsuppgifter som domänadministratör nätverk.

1. Använd hello kontot för installation.

1. Ange hello inloggning toobe medlem i hello **sysadmin** fasta serverrollen.

1. Klicka på **OK**.

Upprepa föregående steg på hello andra SQL Server-VM hello.

## <a name="add-failover-clustering-features-tooboth-sql-server-vms"></a>Lägg till redundanskluster funktioner tooboth virtuella SQL Server-datorer

funktioner för tooadd redundanskluster, hello följa på både SQL Server-datorer:

1. Ansluta toohello SQL Server-dator via hello Remote Desktop Protocol (RDP) med hjälp av hello *CORP\install* konto. Öppna **instrumentpanelen Serverhanteraren**.
2. Klicka på hello **Lägg till roller och funktioner** länk på hello instrumentpanelen.

    ![Server Manager - Lägg till roller](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Välj **nästa** tills toohello **serverfunktioner** avsnitt.
4. I **funktioner**väljer **redundanskluster**.
5. Lägg till eventuella ytterligare funktioner som krävs.
6. Klicka på **installera** tooadd hello funktioner.

Upprepa hello steg på hello andra SQL Server-VM.

## <a name="a-nameendpoint-firewall-configure-hello-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall">Konfigurera hello-brandväggen på varje virtuell dator med SQL Server

hello lösning kräver hello följande TCP-portar toobe öppna i hello brandväggen:

- **SQLServer VM**:<br/>
   Port 1433 för en standardinstans av SQL Server.
- **Azure belastningsutjämningsavsökning:**<br/>
   Alla tillgängliga portar. Exemplen använder ofta 59999.
- **Slutpunkten för databasspegling:** <br/>
   Alla tillgängliga portar. Exemplen använder ofta 5022.

hello brandväggen portar måste toobe öppen på både SQL Server-datorer.

hello metod för att öppna portar hello beror på hello brandvägg som du använder. hello nästa avsnitt beskrivs hur tooopen hello portar i Windows-brandväggen. Öppna portar hello krävs på alla virtuella din SQL Server-datorer.

### <a name="open-a-tcp-port-in-hello-firewall"></a>Öppna en TCP-port i brandväggen för hello

1. Hej första SQL Server på **starta** skärmen, starta **Windows-brandväggen med avancerad säkerhet**.
2. Hello vänster, Välj **regler för inkommande trafik**. Hello högra rutan, klicka på **ny regel**.
3. För **regeltyp**, Välj **Port**.
4. Hello-port, ange **TCP** och typen hello lämpliga portnummer. Se följande exempel hello:

   ![SQL-brandvägg](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. Klicka på **Nästa**.
6. På hello **åtgärd** behåller **Tillåt hello anslutning** markerad och klicka sedan på **nästa**.
7. På hello **profil** , acceptera hello standardinställningar, och klickar sedan på **nästa**.
8. På hello **namn** anger du ett namn för regeln (exempelvis **Azure LB avsökning**) i hello **namn** textrutan och klicka sedan på **Slutför**.

Upprepa dessa steg på hello andra SQL Server-VM.

## <a name="next-steps"></a>Nästa steg

* [Skapa en SQL Server Always On availability group på virtuella Azure-datorer](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
