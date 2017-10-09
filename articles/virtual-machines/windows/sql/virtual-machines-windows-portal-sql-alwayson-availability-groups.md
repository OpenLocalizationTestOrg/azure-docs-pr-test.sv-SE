---
title: "aaaSet hög tillgänglighet för virtuella datorer i Azure Resource Manager | Microsoft Docs"
description: "Den här kursen visar hur toocreate alltid på tillgänglighet grupp med virtuella Azure-datorer i Azure Resource Manager-läge."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: 6f0a253d3502259a487e66fd62d92e41c379a6b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Konfigurera Always On-Tillgänglighetsgrupper i Azure Virtual Machines automatiskt: Resource Manager

Den här kursen visar hur toocreate en SQL Server-tillgänglighet gruppen som använder Azure Resource Manager virtuella datorer. hello kursen används Azure blad tooconfigure en mall. Du kan granska hello standardinställningarna, ange nödvändiga inställningar och uppdatera hello blad i hello portal som du går igenom den här kursen.

hello slutförd guiden skapar en tillgänglighetsgrupp för SQL Server på Azure Virtual Machines som innehåller hello följande element:

* Ett virtuellt nätverk som har flera undernät, inklusive en frontend och backend-undernät
* Två domänkontrollanter som har en Active Directory-domän
* Två virtuella datorer som kör SQL Server och distribuerad toohello backend-undernät och kopplade toohello Active Directory-domän
* Ett redundanskluster med tre noder med hello Nodmajoritet kvorummodellen
* En tillgänglighetsgrupp som har två replikerna med synkront genomförande av en tillgänglighetsdatabas

Följande bild hello representerar hello komplett lösning.

![Test lab-arkitektur för Tillgänglighetsgrupper i Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alla resurser i den här lösningen hör tooa enskild resursgrupp.

Innan du börjar den här kursen bekräfta hello följande:

* Du har redan ett Azure-konto. Om du inte har någon [registrera dig för ett utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/).
* Du vet redan hur hello toouse GUI tooprovision en virtuell dator i SQL Server från hello galleriet för virtuella datorer. Mer information finns i [etablera en virtuell dator i SQL Server på Azure](virtual-machines-windows-portal-sql-server-provision.md).
* Du har redan en god förståelse av Tillgänglighetsgrupper. Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Om du vill använda Tillgänglighetsgrupper med SharePoint finns även [Konfigurera SQL Server 2012 Always On-Tillgänglighetsgrupper för SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).
>
>

I den här kursen använder hello Azure portal till:

* Välj hello alltid på mallen hello-portalen.
* Granska inställningarna för hello och uppdatera några konfigurationsinställningar för din miljö.
* Övervaka Azure eftersom det skapar hello hela miljön.
* Anslut tooa domänkontrollant och sedan tooa server som kör SQL Server.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-hello-cluster-from-hello-gallery"></a>Etablera hello kluster från hello-galleriet
Azure tillhandahåller en bild i galleriet för hello hela lösningen. toolocate hello mall:

1. Logga in toohello Azure-portalen med ditt konto.
2. I hello Azure-portalen klickar du på **+ ny** tooopen hello **ny** bladet.
3. På hello **ny** bladet, söka efter **AlwaysOn**.
   ![Hitta AlwaysOn-mall](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Leta upp i hello sökresultat **SQL Server AlwaysOn-kluster**.
   ![AlwaysOn-mall](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. På **Välj en distributionsmodell**, Välj **Resource Manager**.

### <a name="basics"></a>Grundläggande inställningar
Klicka på **grunderna** och konfigurera hello följande inställningar:

* **Administratörsanvändarnamn** är ett användarkonto som har administratörsbehörighet för domänen och är medlem i hello SQL Server fasta serverrollen sysadmin på båda instanser av SQL Server. Den här kursen använder **DomainAdmin**.
* **Lösenordet** är hello lösenord för administratörskontot för hello domän. Använd ett komplext lösenord. Bekräfta hello lösenord.
* **Prenumerationen** är hello prenumeration att Azure debiterar toorun alla distribuerade resurser för hello-tillgänglighetsgruppen. Om ditt konto har flera prenumerationer, kan du ange en annan prenumeration.
* **Resursgruppen** är hello hello grupp toowhich alla Azure-resurser som har skapats med den här mallen tillhör namn. Den här kursen använder **SQL-hög tillgänglighet-RG**. Mer information finns i [Översikt över Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Plats** är hello Azure-region där hello självstudiekursen skapar hello resurser. Välj en Azure-region.

hello följande skärmbild är en slutförd **grunderna** bladet:

![Grundläggande inställningar](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Klicka på **OK**.

### <a name="domain-and-network-settings"></a>Domän-och nätverksinställningar
Den här mallen för Azure-galleriet skapar en domän och domänkontrollanter. Det skapar också ett nätverk och två undernät. hello mallen kan inte skapa servrar i en befintlig domän eller ett virtuellt nätverk. Nästa steg i hello konfigurerar hello domän- och inställningar.

På hello **domän-och nätverksinställningarna** bladet granska hello förinställda värden för hello domän- och inställningar:

* **Namn för skogsrotsdomänen** är hello domännamnet för Active Directory-domän för hello värdar hello klustret. Hello självstudiekursen använder **contoso.com**.
* **Virtuella nätverksnamnet** är hello nätverksnamn för hello virtuella Azure-nätverket. Hello självstudiekursen använder **autohaVNET**.
* **Namnet på domänkontrollanten undernät** är hello namnet på en del av hello virtuellt nätverk värdar hello domänkontrollanten. Använd **undernät 1**. Det här undernätet använder adressprefixet **10.0.0.0/24**.
* **SQL Server-undernätsnamn** är en del av hello virtuella nätverk som värdar hello servrar som kör SQL Server och hello filen delar vittne hello namn. Använd **undernät 2**. Det här undernätet använder adressprefixet **10.0.1.0/26**.

toolearn mer om virtuella nätverk i Azure, se [översikt över virtuella nätverk](../../../virtual-network/virtual-networks-overview.md).  

Hej **domän-och nätverksinställningarna** ser ut som hello följande skärmbild:

![Domän-och nätverksinställningar](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Du kan ändra dessa värden om det behövs. Den här kursen använder hello förinställda värden.

Granska hello inställningarna och klicka sedan på **OK**.

### <a name="availability-group-settings"></a>Inställningarna för tillgänglighet
På **tillgänglighet gruppinställningar**, granska hello förinställda värden för hello tillgänglighetsgruppen och hello lyssnare.

* **Tillgänglighetsgruppens namn** är hello klusterresurs namn för hello-tillgänglighetsgruppen. Den här kursen använder **Contoso ag**.
* **Tillgänglighetsgruppens lyssnare namn** används av hello klustret och hello intern belastningsutjämnare. Klienter som ansluter tooSQL Server kan använda det här namnet tooconnect toohello lämplig replik av hello databas. Den här kursen använder **Contoso-lyssnaren**.
* **Porten för tillgänglighetsgruppens lyssnare** anger hello TCP-port för hello SQL Server-lyssnare. För den här kursen använder hello standardporten **1433**.

Du kan ändra dessa värden om det behövs. Den här kursen använder hello förinställda värden.  

![Inställningarna för tillgänglighet](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Klicka på **OK**.

### <a name="virtual-machine-size-storage-settings"></a>Storlek på virtuell dator, inställningar för lagring
På **VM-storlek, Lagringsinställningar**, Välj en SQL Server-storlek för virtuell dator och granska hello andra inställningar.

* **Storlek för virtuell dator i SQL Server** hello storlek för både virtuella datorer som kör SQL Server. Välj en lämplig virtuell datorstorlek för din arbetsbelastning. Om du skapar den här miljön hello genomgång, använda **DS2**. Välj en storlek på virtuell dator som har stöd för hello arbetsbelastningen för produktionsarbetsbelastningar. Många produktionsarbetsbelastningar kräver **DS4** eller större. hello mallen skapar två virtuella datorer av den här storleken och installerar SQL Server på var och en. Mer information finns i [storlekar för virtuella datorer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure installerar hello Enterprise-utgåvan av SQL Server. hello kostnaden är beroende av hello edition och hello storlek på virtuell dator. Detaljerad information om aktuella kostnaderna finns [virtuella datorer priser](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Domain controller virtuella datorstorleken** är hello virtuella storlek i hello-domänkontrollanter. För den här självstudiekursen **D2**.
* **Dela vittne virtuella filstorlek** hello virtuella storlek för hello filresursvittne. Den här kursen använder **A1**.
* **SQL Storage-konto** är hello namnet på hello storage-konto som innehåller hello SQL Server-data och operativsystemet diskar. Den här kursen använder **alwaysonsql01**.
* **DC-lagringskonto** är hello namnet på hello storage-konto för hello-domänkontrollanter. Den här kursen använder **alwaysondc01**.
* **SQL Server-data diskstorlek** i TB är hello storleken på hello SQL Server data-disk i TB. Ange ett tal från 1 till 4. Den här kursen använder **1**.
* **Lagringsoptimering** anger särskilda lagrings-konfigurationsinställningar för virtuella datorer med hello SQL Server baserat på hello arbetsbelastning. Alla SQL Server-datorer i det här scenariot kan du använda premium-lagring med Azure värden diskcache Ange endast tooread. Dessutom kan du optimera SQL Server-inställningar för hello arbetsbelastning genom att välja ett av dessa tre inställningar:

  * **Allmän arbetsbelastning** anger inga specifika konfigurationsinställningar.
  * **Transaktionell bearbetning** anger spåra flaggan 1117 och 1118.
  * **Datalagring** anger spåra flaggan 1117 och 610.

Den här kursen använder **allmänna arbetsbelastning**.

![Lagringsinställningarna för VM-storlek](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Granska hello inställningarna och klicka sedan på **OK**.

#### <a name="a-note-about-storage"></a>En anteckning om lagring
Ytterligare optimeringar beror på hello storleken på hello diskar för SQL Server-data. Lägger till ytterligare 1 TB premium-lagring för varje terabyte datadisk, Azure. När en server kräver 2 TB eller mer, skapas hello en lagringspool på varje virtuell dator med SQL Server. En lagringspool är en form av lagringsvirtualisering där flera skivor är konfigurerade tooprovide högre kapacitet, återhämtning och prestanda.  hello mallen sedan skapar ett lagringsutrymme på hello lagringspoolen och anger en enda data toohello operativsystemet. hello mallen anger den här disken hello datadisk för SQL Server. hello mallen justerar hello lagringspoolen för SQL Server med hjälp av hello följande inställningar:

* Stripestorleken är hello interleave inställning för hello virtuell disk. Transaktionell arbetsbelastningar använder 64 KB. Arbetsbelastningar i informationslager använda 256 KB.
* Återhämtning är enkel (ingen återhämtning).

> [!NOTE]
> Azure premium-lagring är lokalt redundant och behålls tre kopior av hello data inom en enskild region så att ytterligare återhämtning vid hello lagringspoolen inte krävs.
>
>

* Kolumnantal är lika med hello antalet diskar i lagringspoolen hello.

För ytterligare information om lagringsutrymme och lagringspooler, se:

* [Översikt för lagringsutrymmen](http://technet.microsoft.com/library/hh831739.aspx)
* [Windows Serverbackup och lagringspooler](http://technet.microsoft.com/library/dn390929.aspx)

Läs mer om bästa praxis för SQL Server configuration [prestandarelaterade Metodtips för SQL Server på virtuella Azure-datorer](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server-inställningar
På **SQL Server-inställningar**, granska och ändra hello namnprefix för SQL Server virtuella datorn, version av SQL Server, SQL Server-tjänstkontot och lösenord och hello SQL underhållsschema för automatisk uppdatering.

* **SQL Server-namnet Prefix** är används toocreate ett namn för varje virtuell dator med SQL Server. Den här kursen använder **sqlserver**. hello mallen namn hello SQL Server-datorer *sqlserver-0* och *sqlserver-1*.
* **SQL Server-version** är hello version av SQL Server. För den här självstudiekursen **SQL Server 2014**. Du kan också välja **SQL Server 2012** eller **SQL Server 2016**.
* **Användarnamn för SQL Server-tjänsten** är hello domänkontonamnet för hello SQL Server-tjänsten. Den här kursen använder **sqlservice**.
* **Lösenordet** hello lösenord för hello SQL Server-tjänstkontot.  Använd ett komplext lösenord. Bekräfta hello lösenord.
* **SQL automatisk uppdatering underhållsschema** identifierar hello veckodag hello att Azure automatiskt korrigeringsfiler hello SQL-servrar. Den här kursen skriver **söndag**.
* **Starttimme för underhåll SQL automatisk uppdatering** är hello tid för hello Azure-region när automatisk korrigering börjar.

> [!NOTE]
> hello korrigering fönster för varje virtuell dator ut en timme. Endast en virtuell dator är skyddad på en gång tooprevent avbrott i tjänsterna.
>
>

![SQL Server-inställningar](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Granska hello inställningarna och klicka sedan på **OK**.

### <a name="summary"></a>Sammanfattning
På sammanfattningssidan hello verifierar Azure hello inställningar. Du kan också hämta hello mallen. Granska hello sammanfattning. Klicka på **OK**.

### <a name="buy"></a>Köp
Det här sista bladet innehåller **användningsvillkoren**, och **sekretesspolicy**. Gå igenom informationen. När du är redo för Azure toostart toocreate hello virtuella datorer och alla hello andra nödvändiga resurser för hello tillgänglighetsgruppen, klickar du på **skapa**.

hello Azure-portalen skapar hello resursgruppen och alla hello-resurser.

## <a name="monitor-deployment"></a>Övervakaren distribution
Övervaka hello förlopp från hello Azure-portalen. En ikon som representerar hello distribution är automatiskt Fäst toohello Azure portalens instrumentpanel.

![Instrumentpanelen för Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-toosql-server"></a>Ansluta tooSQL Server
hello nya instanser av SQL Server körs på virtuella datorer som har internet-ansluten IP-adresser. Du kan remote desktop (RDP) direkt tooeach SQL Server-datorn.

tooRDP tooa SQL Server, Följ dessa steg:

1. Från hello Azure portalens instrumentpanel, verifiera att hello distributionen har slutförts.
2. Klicka på **resurser**.
3. I hello **resurser** bladet, klickar du på **sqlserver-0**, vilket är hello datornamnet för en av hello virtuella datorer som kör SQL Server.
4. På bladet hello för **sqlserver-0**, klickar du på **Anslut**. Webbläsaren frågar om du vill tooopen eller spara hello fjärranslutning objekt. Klicka på **öppna**.
5. **Anslutning till fjärrskrivbord** kan varna dig att hello utgivaren av den här fjärranslutningen inte kan identifieras. Klicka på **Anslut**.
6. Windows-säkerhet uppmanas du tooenter dina autentiseringsuppgifter tooconnect toohello IP-adressen för hello primära domänkontrollanten. Klicka på **använder ett annat konto**. För **användarnamn**, typen **contoso\DomainAdmin**. Du har konfigurerat det här kontot när du ställer in hello administratörsanvändarnamn i hello mallen. Använd hello komplext lösenord som du valde när du konfigurerade hello mallen.
7. **Fjärrskrivbord** kan varna dig att hello fjärrdatorn inte kunde autentiseras på grund av tooproblems med säkerhetscertifikatet. Den visar hello säkerhet, certifikatnamnet. Om du har följt hello kursen hello heter **sqlserver 0.contoso.com**. Klicka på **Ja**.

Du är nu ansluten med RDP toohello SQL Server-datorn. Du kan öppna SQL Server Management Studio, ansluta toohello standardinstansen av SQL Server och kontrollera att hello tillgänglighetsgruppen har konfigurerats.
