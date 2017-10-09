---
title: "aaaOverview av SQL Server på Azure Virtual Machines | Microsoft Docs"
description: "Läs mer om hur toorun fullständig SQL Server-utgåvorna på Azure virtuella datorer. Hämta Direktlänkar tooall SQL Server-VM-avbildningar och tillhörande innehåll."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: 07be567c76f4435961592fc0872fe41cd45bd79d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Översikt över SQL Server i Azure Virtual Machines
Det här avsnittet beskriver alternativen för att köra SQL Server på virtuella Azure-datorer (VM), tillsammans med [länkar tooportal bilder](#option-1-create-a-sql-vm-with-per-minute-licensing) och en översikt över [vanliga uppgifter](#manage-your-sql-vm).

> [!NOTE]
> Om du redan är bekant med SQL Server och bara vill toosee hur toodeploy en SQL Server-VM finns [etablera en virtuell dator med SQL Server i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md).
> 
> 

## <a name="overview"></a>Översikt
Om du är en databasadministratör eller en utvecklare kan tillhandahålla virtuella Azure-datorer en sätt toomove dina lokala SQL Server-arbetsbelastningar och program toohello moln. hello ger följande videoklipp en teknisk översikt över Azure virtuella datorer för SQL Server.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

hello video omfattar hello följande områden:

| Tid | Område |
| --- | --- |
| 00:21 |Vad är Azure VM:ar? |
| 01:45 |Säkerhet |
| 02:50 |Anslutning |
| 03:30 |Lagringstillförlitlighet och prestanda |
| 05:20 |VM-storlekar |
| 05:54 |Hög tillgänglighet och SLA |
| 07:30 |Konfigurationssupport |
| 08:00 |Övervakning |
| 08:32 |Demo: Skapa en SQL Server 2016 VM |

> [!NOTE]
> hello video fokuserar på SQL Server 2016, men Azure tillhandahåller VM-avbildningar för flera versioner av SQL Server 2012, 2014 och 2016. 
> 
> 

## <a name="scenarios"></a>Scenarier
Det finns många anledningar som du kan välja toohost dina data i Azure. Om ditt program är flyttar tooAzure, förbättrar prestanda tooalso flytta hello data. Men det finns fler fördelar. Har du automatiskt åtkomst toomultiple Datacenter för en global närvaro och haveriberedskap. hello data är också mycket säkra och beständig.

SQL Server som kör på Azure VM:ar är ett alternativ för att lagra din relationsdata i Azure. Det är bra för flera scenarier. Du kanske exempelvis vill tooconfigure hello Azure VM som på samma sätt som möjligt tooan lokala SQL Server-datorn. Du kan också toorun fler program och tjänster på hello samma databasserver. Det finns två huvudsakliga resurser som hjälper dig att tänka igenom ännu fler scenarier och överväganden:

* [SQL Server på Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) innehåller en översikt över hello bästa scenarier för att använda SQL Server på Azure Virtual Machines. 
* [Välj ett molnbaserat SQL Server-alternativ: Azure SQL (PaaS) Database eller SQL Server på Azure VM:ar (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) ger dig en detaljerad jämförelse mellan SQL Database och SQL Server som körs på en VM.

## <a name="create-a-new-sql-vm"></a>Skapa en ny virtuell SQL-dator
hello följande avsnitt finns direktlänkar toohello Azure-portalen för hello SQL Server virtuella galleriavbildningar. Beroende på hello-avbildning som du väljer, kan du antingen betala för SQL Server-licensieringskostnaderna på per minut eller du kan ta din egen licens (BYOL).

Steg för steg-vägledning för att skapa en ny SQL-VM i hello kursen [etablera en virtuell dator med SQL Server i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md). Granska även hello [prestandarelaterade Metodtips för SQL Server-datorer](virtual-machines-windows-sql-performance.md), som förklarar hur tooselect hello lämplig dator storlek och andra funktioner som finns under etableringen.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Alternativ 1: Skapa en virtuell SQL-dator med licensiering per minut
hello innehåller följande tabell en matris med hello senaste SQL Server-avbildningar i galleriet för hello virtuella datorer. Klicka på någon länk toobegin att skapa en ny SQL-VM med angiven version, version och operativsystem. 

> [!TIP]
> toounderstand hello VM och priser för SQL för dessa avbildningar finns [priser för SQL Server Azure Virtual Machines](virtual-machines-windows-sql-server-pricing-guidance.md).

| Version | Operativsystem | Utgåva |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |

Dessutom toothis lista andra kombinationer av SQL Server-versioner och operativsystem är tillgängliga. Hitta andra avbildningar via en marketplace-sökning i hello Azure-portalen. 

## <a id="BYOL"></a> Alternativ 2: Skapa en virtuell SQL-dator med en befintlig licens
Du kan även använda din egen licens (Bring your own license, BYOL). I detta scenario betalar du bara för hello VM utan ytterligare avgifter för SQL Server-licensiering. toouse din egen licens, Använd hello matris av SQL Server-versioner, utgåvor och operativsystem nedan. I hello portal dessa namnen på avbildningarna föregås med **{BYOL}**.

> [!TIP]
> När du använder din egen licens kan du spara pengar långsiktigt för kontinuerliga arbetsbelastningar under produktion. Mer information finns i [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

| Version | Operativsystem | Utgåva |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard  BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

Dessutom toothis lista andra kombinationer av SQL Server-versioner och operativsystem är tillgängliga. Hitta andra avbildningar via en marketplace-sökning i hello Azure-portalen (Sök efter ”{BYOL} SQLServer”).

> [!IMPORTANT]
> toouse BYOL VM-bilder, måste du ha ett Enterprise-avtal med [Licensmobilitet via Software Assurance på Azure](https://azure.microsoft.com/pricing/license-mobility/). Du måste också en giltig licens hello version/utgåva av SQL Server du vill ha toouse. Du måste [ange hello nödvändiga BYOL information tooMicrosoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) inom **10** dagar från etableringen av den virtuella datorn. 

> [!NOTE]
> Det är inte möjligt toochange hello licensiering modell för en per minut SQL Server-VM toouse din egen licens. I det här fallet måste du skapa en ny BYOL VM och migrera dina databaser toohello ny virtuell dator. 

## <a name="manage-your-sql-vm"></a>Hantera den virtuella SQL-datorn
Det finns flera valfria hanteringsuppgifter som du kan utföra när du har etablerat din virtuella SQL Server-dator. Till stor del konfigurerar du och hanterar SQL Server på exakt samma sätt som när du administrerar en lokal SQL Server-instans. Vissa uppgifter är dock specifika tooAzure. hello följande avsnitt beskrivs några av dessa områden med länkar toomore information.

### <a name="connect-toohello-vm"></a>Ansluta toohello VM
En av hello mest grundläggande hanteringsåtgärder är tooconnect tooyour SQL Server-VM via verktyg, till exempel SQL Server Management Studio (SSMS). Instruktioner om hur tooconnect tooyour nya SQLServer-dator, se [ansluta tooa virtuell dator med SQL Server på Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrera dina data
Om du har en befintlig databas ska du toomove som toohello nyligen etablerats SQL VM. En lista över migreringsalternativ och anvisningar finns i [migrera en databas tooSQL Server på en Azure VM](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfigurera hög tillgänglighet
Om du behöver hög tillgänglighet bör du överväga att konfigurera SQL Server-tillgänglighetsgrupper. Detta betyder att det finns flera virtuella Azure-datorer i ett virtuellt nätverk. hello Azure-portalen har en mall som ställer in den här konfigurationen åt dig. Mer information finns i [Konfigurera en AlwaysOn-tillgänglighetsgrupp för virtuella datorer i Azure Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Om du vill toomanually konfigurera Tillgänglighetsgruppen och associerade lyssnare finns [konfigurera AlwaysOn-Tillgänglighetsgrupper i Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Mer information om hög tillgänglighet finns i [Hög tillgänglighet och haveriberedskap för SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Säkerhetskopiera data
Virtuella Azure-datorer kan dra nytta av [automatisk säkerhetskopiering](virtual-machines-windows-sql-automated-backup.md)som regelbundet skapar säkerhetskopior av din tooblob databaslagring. Du kan också använda den här tekniken manuellt. Mer information finns i [Använda Azure Storage för säkerhetskopiering och återställning av SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md). En översikt över alla alternativ för säkerhetskopiering och återställning finns i [Säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatisera uppdateringar
Virtuella Azure-datorer kan använda [automatisk uppdatering](virtual-machines-windows-sql-automated-patching.md) tooschedule en underhållsperiod för installation av viktiga windows- och SQL Server uppdateras automatiskt.

### <a name="customer-experience-improvement-program-ceip"></a>CEIP (Customer Experience Improvement Program)
hello Customer Experience Improvement Program (CEIP) är aktiverat som standard. Det här regelbundet skickar rapporter tooMicrosoft toohelp förbättra SQL Server. Det finns ingen management-aktivitet som krävs med CEIP såvida du inte vill toodisable den efter etableringen. Du kan anpassa eller inaktivera hello CEIP genom att ansluta toohello virtuella datorn med fjärrskrivbord. Kör sedan hello **SQL Server-fel och användningsrapportering** verktyget. Följ hello instruktioner toodisable reporting. 

Mer information finns i hello CEIP-avsnittet i hello [accepterar licensvillkoren](https://msdn.microsoft.com/library/ms143343.aspx) avsnittet. 

## <a name="next-steps"></a>Nästa steg

Frågor om priser finns i [priser för SQL Server Azure Virtual Machines](virtual-machines-windows-sql-server-pricing-guidance.md) och hello [Azure sida med priser](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Välj din mål-utgåva av SQL Server i hello **OS-programvara** lista. Visa hello priser för olika storlek virtuella datorer.

Har du fler frågor? Kontrollera först hello [SQL Server på Azure virtuella datorer med vanliga frågor om](virtual-machines-windows-sql-server-iaas-faq.md). Men även lägga till dina frågor eller kommentarer toohello längst ned på alla SQL-VM avsnitt toointeract med Microsoft och hello community.
