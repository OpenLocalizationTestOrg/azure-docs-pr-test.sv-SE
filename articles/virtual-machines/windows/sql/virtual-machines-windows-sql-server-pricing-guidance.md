---
title: "aaaManage kostnader effektivt för SQL Server på Azure virtual machines | Microsoft Docs"
description: "Innehåller metodtips för att välja hello rätt SQL Server-datorn prismodell."
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/18/2017
ms.author: jroth
ms.openlocfilehash: 6c6a4394e95b5a915ea3e7d5965730000d331036
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Priser för SQL Server Azure virtuella datorer

Det här avsnittet beskrivs priser för SQL Server-datorer i Azure. Det finns flera alternativ som påverkar kostnad och det är viktigt toopick hello rätt avbildning som balanserar kostnader med företagets krav.

## <a name="free-licensed-sql-server-editions"></a>Kostnadsfri-licensierade versioner av SQL Server

Om du vill toodevelop, testa, eller skapa ett konceptbevis och sedan använda hello kostnadsfri-licensierade **SQL Server Developer edition**. Den här versionen har allt i SQL Server Enterprise edition, därför kan du använda den toobuild det program som du vill. Det är inte tillåtet toorun i produktion. En SQL Server Developer VM debiterar endast för hello kostnaden för hello VM, inte för SQL Server-licensiering.

Om du vill toorun en lätt arbetsbelastning i produktion (< 4 kärnor < 1 GB minne, < 10 GB-databaser), Använd hello kostnadsfri-licensierade **SQL Server Express edition**. En SQL Express virtuell dator tar bara betalt för hello kostnaden för hello VM, inte SQL-licensiering.

Du kan också spara pengar genom att välja en mindre VM-storlek som matchar dessa arbetsbelastningar för dessa utveckling och testning eller lightweight produktionsarbetsbelastningar. Hej DS1v2 kan vara ett bra alternativ för dessa arbetsbelastningar.

toocreate en SQL Server 2016 Azure virtuell dator med någon av dessa avbildningar finns hello följande länkar:

- [SQL Server 2016 Developer Azure VM](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [SQL Server 2016-Express Azure VM](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>Betald SQL Server-utgåvorna

Om du har en icke-lightweight produktion arbetsbelastning kan använda något av följande SQL Server-utgåvorna hello:

| SQL Server-version | Arbetsbelastning |
|-----|-----|
| Webb | Liten webbplatser |
| Standard | Liten toomedium arbetsbelastningar |
| Enterprise | Stora eller verksamhetskritiska arbetsbelastningar|

Du har två alternativ toopay för SQL Server-licensiering för dessa versioner: *betala per användning* eller *ta med din egen licens*.

### <a name="pay-per-usage"></a>Betala per användning

**Betalande hello SQL Server-licens per användning** innebär att hello per minut kostnaden för att köra hello Azure VM omfattar hello kostnaden för hello SQL Server-licens. Du kan se hello priser för hello olika SQL Server-utgåvorna (Web, Standard, Enterprise) i hello [Azure VM sida med priser](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). hello kostnaden är hello samma för alla versioner av SQL Server (2008 R2 too2016). Som med SQL Server-licensiering i allmänhet hello beror per minut på hello antal VM kärnor.

Betala hello SQL Server rekommenderas-licensiering per användning för:

- Tillfällig eller periodiska arbetsbelastningar. Till exempel en app som behöver toosupport en händelse för ett antal månader varje år eller Företagsanalys på måndagen.
- Arbetsbelastningar med okänt livslängd eller skala. Till exempel en app som inte kan krävas i några månader eller som kan kräva mer eller mindre datorkraft, beroende på begäran.

toocreate en SQL Server 2016 Azure virtuell dator med någon av dessa betala per användning avbildningar finns hello följande länkar:

- [SQL Server 2016 Web Azure VM](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [SQL Server 2016 Standard-Azure VM](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [SQL Server 2016 Enterprise Azure VM](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> När du skapar en virtuell dator med SQL Server i hello Azure-portalen hello uppskattad månadskostnaden visas på hello **välja en storlek** bladet innehåller inte SQL Server-licensieringskostnaderna. Detta är hello kostnaden för hello VM enbart.
>
> ![Välj bladet för VM-storlek](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Detta är hello totala uppskattade kostnaden för hello kostnadsfri-licensierade Express och utvecklare utgåvor av SQL Server. Men för webb-, Standard- och Enterprise hitta hello extra kostnad för SQL på hello [sida med priser virtuella Windows-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Hello sida med priser, Välj ditt mål-utgåva av SQL Server.

### <a name="bring-your-own-license-byol"></a>Bring-your-own-license (BYOL)

**Ta din egen SQL Server-licens genom Licensmobilitet**, kallas även tooas **BYOL**, sätt med hjälp av en befintlig SQL Server-volymlicens med Software Assurance i en Azure VM. En SQL Server-VM med BYOL debiterar endast för hello kostnaden för att köra hello VM, inte för SQL Server-licensiering med tanke på att du redan har köpt licenser och Software Assurance via Volume Licensing-program.

Ta din egen SQL rekommenderas-licensiering via licensera Mobility för:

- Kontinuerlig arbetsbelastningar. Till exempel en app som behöver toosupport verksamheten 24 x 7.
- Arbetsbelastningar med kända livstid och skala. Till exempel en app som krävs för hello hela år och har varit prognostiserat tillgång och efterfrågan.

toouse BYOL med en virtuell dator med SQL Server, måste du ha en licens för SQL Server Standard eller Enterprise och [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), vilket är ett alternativ som krävs för igenom några [volymlicensiering](https://www.microsoft.com/en-us/download/details.aspx?id=10585) program och en valfri köpa med andra.  hello priser nivåer som tillhandahålls genom volymlicensieringsprogram varierar utifrån hello typ av avtalet och hello kvantitet och eller åtagande tooSQL Server. Men som en tumregel ta fram din egen licens för kontinuerlig produktionsarbetsbelastningar har hello följande fördelar:

| BYOL förmån | Beskrivning |
|-----|-----|
| **Besparingar** | Ta din egen SQL Server-licens är mer kostnadseffektivt än betala per användning om en arbetsbelastning körs alltid SQL Server Standard eller Enterprise för *mer än 10 månader*. |
| **Långsiktig besparingar** | Det är i genomsnitt *30% billigare per år* toobuy eller förnya en SQL Server-licens för hello första 3 år. Dessutom efter 3 år behöver du inte toorenew hello licens längre bara betala för Software Assurance. Då är det *200% billigare*. |
| **Ledigt passiva sekundär replik** | En annan fördel med att din egen licens är hello [ledigt licensiering för en sekundär replik för passiva](https://azure.microsoft.com/pricing/licensing-faq/) per SQL Server för hög tillgänglighet. Detta minskar i halva hello licensiering kostnaden för en högtillgänglig SQL Server-distribution (t.ex. med Always On-Tillgänglighetsgrupper). hello rättigheter toorun hello passiva sekundära tillhandahålls genom hello failover servrar Software Assurance-förmånen. |

toocreate en SQL Server 2016 virtuella Azure-datorn med en av avbildningarna bring-your-äger-licens finns hello VMs prefixet ”{BYOL}”:

- [SQL Server 2016 Enterprise Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 Standard-Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!NOTE]
> Meddela oss inom 10 dagar hur många SQL Server-licenser som du ska använda i Azure. hello länkar toohello tidigare bilder har instruktioner om hur toodo detta.

## <a name="avoid-unecessary-costs"></a>Undvika onödiga kostnader

Om du använder alla arbetsbelastningar som inte kör kontinuerligt, bör du stänger av hello virtuell dator under hello inaktiva perioder. Betala endast för det du använder.

Till exempel om du bara i SQL Server på en Azure VM, kan du inte tooincur kostnader genom att låta den kör för veckor av misstag. En lösning är toouse hello [automatisk avstängning](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL-VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatisk avstängning är en del av en större uppsättning liknande funktioner som tillhandahålls av [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Överväg att automatiskt stänger och startar om virtuella Azure-datorer med en skript-lösning som för andra arbetsflöden [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Stänga av och det har frigjorts den virtuella datorn är hello endast sätt tooavoid avgifter. Bara stoppas eller med hjälp av power alternativ tooshut ned hello VM fortfarande debiteras användning.

## <a name="next-steps"></a>Nästa steg

För allmän Azure priser vägledning, se [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](../../../billing/billing-getting-started.md).

Hello senaste virtuella datorer prissättning, inklusive SQL Server finns i hello [Azure VM sida med priser](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Granska andra virtuell dator med SQL Server-ämnen på [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).
