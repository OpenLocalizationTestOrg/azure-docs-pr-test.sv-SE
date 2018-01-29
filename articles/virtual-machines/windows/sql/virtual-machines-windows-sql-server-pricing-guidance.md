---
title: "Hantera kostnaderna effektivt för SQL Server på Azure virtual machines | Microsoft Docs"
description: "Innehåller metodtips för att välja rätt SQL Server-virtuella prismodell."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/17/2017
ms.author: jroth
ms.openlocfilehash: fa1611944d266001a54c4d78205c942a5226d97b
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Priser för SQL Server Azure virtuella datorer

Den här artikeln vägledning prisnivå för SQL Server-datorer i Azure. Det finns flera alternativ som påverkar kostnad och det är viktigt att välja rätt avbildningen som balanserar kostnader med företagets krav.

## <a name="free-licensed-sql-server-editions"></a>Kostnadsfri-licensierade versioner av SQL Server

Om du vill utveckla, testa eller skapa ett konceptbevis sedan använda den kostnadsfria-licensierade **SQL Server Developer edition**. Den här versionen har allt i SQL Server Enterprise edition, vilket du kan använda den för att skapa det program som du vill. Det har endast inte tillåtet för att köra i produktion. En enda avgifter för SQL Server Developer-VM för kostnaden för den virtuella datorn, inte för SQL Server-licensiering.

Om du vill köra en lätt arbetsbelastning i produktion (< 4 kärnor < 1 GB minne, < 10 GB-databaser), sedan använda den kostnadsfria-licensierade **SQL Server Express edition**. SQL Express VM endast avgifter för kostnaden för den virtuella datorn inte SQL-licensiering.

Du kan också spara pengar genom att välja en mindre VM-storlek som matchar dessa arbetsbelastningar för dessa utveckling och testning eller lightweight produktionsarbetsbelastningar. DS1v2 kan vara ett bra alternativ för dessa arbetsbelastningar.

Om du vill skapa en SQL Server 2017 virtuella Azure-datorn med någon av dessa avbildningar, se följande länkar:

| Plattform | Fritt licensierade bilder |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Betald SQL Server-utgåvorna

Om du har en icke-lightweight produktion arbetsbelastning kan använda något av följande SQL Server-versioner:

| SQL Server-version | Arbetsbelastning |
|-----|-----|
| Webb | Liten webbplatser |
| Standard | Små till medelstora arbetsbelastningar |
| Enterprise | Stora eller verksamhetskritiska arbetsbelastningar|

Du har två alternativ för att betala för SQL Server-licensiering för dessa versioner: *betala per användning* eller *ta med din egen licens (BYOL)*.

### <a name="pay-per-usage"></a>Betala per användning

**Betala per användning i SQL Server-licens** innebär att kostnaden per minut för att köra Azure VM omfattar kostnaden för SQL Server-licens. Du kan se priser för de olika SQL Server-versionerna (Web, Standard, Enterprise) i den [Azure VM sida med priser](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). Kostnaden är samma för alla versioner av SQL Server (2012 SP3 2017). Precis som med SQL Server-licensiering i allmänhet beror licensiering kostnaden per minut på antal kärnor VM.

Betala SQL Server rekommenderas-licensiering per användning för:

- Tillfällig eller periodiska arbetsbelastningar. Till exempel en app som behöver stöd för en händelse för ett antal månader varje år eller Företagsanalys på måndagen.
- Arbetsbelastningar med okänt livslängd eller skala. Till exempel en app som inte kan krävas i några månader eller som kan kräva mer eller mindre datorkraft, beroende på begäran.

Om du vill skapa en SQL Server 2017 virtuella Azure-datorn med någon av dessa avbildningar betala per användning, se följande länkar:

| Plattform | Licensierade bilder |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 Standard-Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Standard-Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 Standard-Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 Standard-Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> När du skapar en virtuell dator med SQL Server i portalen på **välja en storlek** innehåller en beräknad kostnad. Det är viktigt att notera att denna uppskattning beräkning kostnader för att köra den virtuella datorn tillsammans med alla Windows licensiering kostnaderna för virtuella Windows-datorer. Det inkluderar inte ytterligare SQL Server-licensieringskostnader för webb-, Standard- och Enterprise-utgåvor. Den också innehåller inte någon extra kostnad för tredje parts Linux-operativsystem för virtuella Linux-datorer. För att få den mest korrekta prisnivå uppskattningen, Välj ditt operativsystem och version av SQL Server på prissättningssidan för [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) och [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).
>
> ![Välj bladet för VM-storlek](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)

### <a name="bring-your-own-license-byol"></a>Bring-your-own-license (BYOL)

**Ta din egen SQL Server-licens genom Licensmobilitet**, vilket även kallas **BYOL**, sätt med hjälp av en befintlig SQL Server-volymlicens med Software Assurance i en Azure VM. En SQL Server-VM med BYOL endast avgifter för kostnaden för att köra den virtuella datorn, inte för SQL Server-licensiering med tanke på att du redan har köpt licenser och Software Assurance via Volume Licensing-program.

> [!NOTE]
> BYOL bilder är bara tillgängliga för Windows-datorer. Du kan manuellt installera SQL Server på en virtuell dator endast Linux. Se riktlinjerna i den [Linux SQL VM vanliga frågor och svar](../../linux/sql/sql-server-linux-faq.md).

Ta din egen SQL rekommenderas-licensiering via licensera Mobility för:

- Kontinuerlig arbetsbelastningar. Till exempel en app som behöver stöd för verksamheten 24 x 7.
- Arbetsbelastningar med kända livstid och skala. Till exempel en app som krävs för hela året och vilka begäran har tagits prognostiserat.

Om du vill använda BYOL med en SQL Server-VM, måste du ha en licens för SQL Server Standard eller Enterprise och [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), vilket är ett alternativ som krävs för igenom några [volymlicensiering](https://www.microsoft.com/en-us/download/details.aspx?id=10585) program och en valfri inköp med andra.  Nivån prisnivå genom volymlicensieringsprogram varierar beroende på vilken typ av avtalet och kvantitet och eller åtagande till SQL Server. Men som en tumregel ta fram din egen licens för kontinuerlig produktionsarbetsbelastningar har följande fördelar:

| BYOL förmån | Beskrivning |
|-----|-----|
| **Besparingar** | Ta din egen SQL Server-licens är mer kostnadseffektivt än betala per användning om en arbetsbelastning som körs kontinuerligt SQL Server Standard eller Enterprise för *mer än 10 månader*. |
| **Långsiktig besparingar** | Det är i genomsnitt *30% billigare per år* att köpa eller förnya en SQL Server-licens för de första 3 år. Dessutom efter 3 år behöver du inte längre förnya licensen, betalar bara för Software Assurance. Då är det *200% billigare*. |
| **Ledigt passiva sekundär replik** | En annan fördel med att din egen licens är de [ledigt licensiering för en sekundär replik för passiva](https://azure.microsoft.com/pricing/licensing-faq/) per SQL Server för hög tillgänglighet. Detta minskar i hälften av licensieringen kostnaden för en högtillgänglig SQL Server-distribution (till exempel använder alltid på Tillgänglighetsgrupper). Behörighet för att köra den passiva sekundärt tillhandahålls via failover servrar Software Assurance-förmånen. |

Om du vill skapa en SQL Server 2016 virtuella Azure-datorn med någon av dessa bring-your-äger-licens avbildningar finns i de virtuella datorerna prefixet ”{BYOL}”:

- [SQL Server 2016 Enterprise Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 Standard-Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!IMPORTANT]
> Berätta för oss inom 10 dagar hur många SQL Server-licenser som du ska använda i Azure. Länkar till föregående bilder har instruktioner om hur du gör detta.

> [!NOTE]
> Det går inte att ändra så att licensieringsmodellen för en SQL Server VM som betalas per minut ska använda din egen licens. I så fall måste du skapa en ny virtuell dator med BYOL och migrera dina databaser till den nya virtuella datorn. 

## <a name="avoid-unnecessary-costs"></a>Undvika onödiga kostnader

Välj en optimal virtuell datorstorlek för att undvika onödiga kostnader och Överväg återkommande avstängningar för icke-kontinuerliga arbetsbelastningar.

### <a id="machinesize"></a>Storleken på rätt sätt den virtuella datorn

Licensiering kostnaden för SQL Server är direkt relaterat till antal kärnor. Välj en VM-storlek som matchar dina förväntade behov av CPU, minne, lagring och i/o-bandbredd. En fullständig lista över storleksalternativ för datorer, se [Windows VM-storlekar](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) och [Linux VM-storlekar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Det finns nya storlekar för datorer som fungerar bra med vissa typer av SQL Server-arbetsbelastningar. Dessa datorer storlekar upprätthålla hög minne, lagring och i/o-bandbredd, men de har ett lägre antal virtualiserade kärnor. Tänk dig följande exempel:

| Storlek på virtuell dator | vCPUs | Minne | Maximalt antal diskar | Högsta antal i/o-genomflöde | SQL-licensieringskostnaderna | Totalkostnader (beräkning + licensing) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51,200 IOPS eller 768 MB/s | | |
| **Standard_DS14 4v2** | 4 | 112 GB | 32 | 51,200 IOPS eller 768 MB/s | 75% lägre | 57% lägre |

> [!IMPORTANT]
> Det här är ett exempel på tidpunkt. Senaste specifikationer finns datorn storlekar artiklar och priser för Azure [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) och [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

I exemplet ovan kan du se att specifikationerna för **Standard_DS14v2** och **Standard_DS14 4v2** är identiska förutom vCPUs. Suffixet **-4v2** i slutet av den **Standard_DS14 4v2** datorstorlek visar antalet aktiva vCPUs. Eftersom SQL Server-licensieringskostnaderna är knutna till antal kärnor, minskar vilket avsevärt kostnaden för den virtuella datorn i scenarier där extra vCPUs inte behövs. Det här är ett exempel och det finns många datorstorlekar med begränsad vCPUs som identifieras med det här mönstret suffix. Mer information finns i bloggposten [om nya Azure VM-storlekar för mer kostnadseffektivt databasen arbete](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Stäng av den virtuella datorn när det är möjligt

Om du använder alla arbetsbelastningar som inte kör kontinuerligt, bör du stänga av den virtuella datorn under inaktiva perioder. Betala endast för det du använder.

Till exempel om du bara i SQL Server på en Azure VM, vill du inte avgifter genom att låta den kör för veckor av misstag. En lösning är att använda den [automatisk avstängning](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL-VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatisk avstängning är en del av en större uppsättning liknande funktioner som tillhandahålls av [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Överväg att automatiskt stänger och startar om virtuella Azure-datorer med en skript-lösning som för andra arbetsflöden [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Stänga av och det har frigjorts din virtuella dator är det enda sättet att undvika kostnader. Bara stoppas eller med Energialternativ för att stänga av den virtuella datorn fortfarande debiteras användning.

## <a name="next-steps"></a>Nästa steg

För allmän Azure priser vägledning, se [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](../../../billing/billing-getting-started.md).

Senaste virtuella datorer prissättning, inklusive SQL Server, finns det [Azure VM sida med priser](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Mer information om SQL Server-datorer för både [SQL Server Windows VMs](virtual-machines-windows-sql-server-iaas-overview.md) och [Linux virtuella datorer för SQL Server](../../linux/sql/sql-server-linux-virtual-machines-overview.md).
