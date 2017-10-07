---
title: aaaSQL Azure med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toouse SQL Azure med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fec4cb1f1ab3cde03b6ff613650e01bae3552824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Ofta när kunder väljer toohost sina Windows-program i hello moln med Azure RemoteApp vill de också toomigrate sina data, till exempel SQL-servrar i hello molnet för en hel molndistribution. Det gör att hela den molnbaserade lösningen kan användas när och var som helst av alla enheter, med Azure RemoteApp. Nedan följer länkar och referenser tillsammans med riktlinjer toohelp du med den här processen.  

## <a name="migrate-your-sql-data"></a>Migrera dina SQL-data
Börja med [migrera en SQL-databas med SQL Server-databasen tooAzure](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurera Azure RemoteApp
Använd Azure RemoteApp som värd för Windows-programmet. Nedan finns steg för steg-anvisningar på hög nivå:

1. Skapa hello [VM-mallen för Azure RemoteApp](remoteapp-imageoptions.md). 
2. Installera programmet hello krävs på hello VM.
3. Konfigurera hello program så att det ansluter toohello SQL DB och bekräfta att det fungerar.
4. Sysprep och Stäng hello VM. Samla in det som en avbildning för användning med Azure. **Obs:** måste tooensure att hello program är kan tooretain hello DB anslutningsinformation via hello sysprep-processen. Om programmet hello är tooretain hello DB-anslutningsinformationen, kanske du vill tooengage hello leverantören av hello programmet toocheck hur vi kan ange hello anslutningssträngen.
5. Importera hello anpassad avbildning till din Azure RemoteApp-biblioteket genom att välja hello geografiska plats där din SQL Azure-distribution finns. 
6. Distribuera en RemoteApp-samlingen i hello samma data center som din SQL Azure-distribution med hello ovan mall och publicera programmet hello. Distribuera Azure RemoteApp i hello samma datacenter som SQL Azure-distribution hjälper dig att kontrollera hello snabbaste anslutningshastigheter och minska svarstiden. 

## <a name="app-and-sql-configuration-considerations"></a>Tänk på följande när du konfigurerar appen och SQL:
Det finns några punkter tooconsider när du använder Azure SQL med RemoteApp:

Läs [hur tooconfigure en Azure SQL-databas brandväggen](../sql-database/sql-database-firewall-configure.md). Hämtad från hello artikeln står följande: ”först alla åtkomst tooyour Azure SQL Database-server har blockerats av hello brandväggen. I ordning toobegin med Azure SQL Database-server, måste du gå toohello klassiska portalen och ange en eller flera servernivå brandväggsregler som tillåter åtkomst tooyour Azure SQL Database-server. Använda vilka IP-adressintervallen från hello Internet tillåts hello brandväggen regler toospecify och huruvida Azure-program kan försöka tooconnect tooyour Azure SQL Database-server ”.

Dessutom när en dator försöker tooconnect tooyour databasservern från hello Internet, kontrollerar brandväggen hello hello kommer IP-adressen för hello begäran mot hello fullständig uppsättning på servernivå och (vid behov) brandväggsregler på objektnivå databasen. ”Om hello IP-adressen för hello-begäran är inom ett hello-intervall som anges i hello servernivå brandväggsregler, hello anslutning beviljas tooyour Azure SQL Database-server”. Därför kan vi utnyttja IP-intervall och inte bara enskilda IP-adresser.

Följ hello steg-för-steg-instruktioner i [så här: konfigurera brandväggsinställningar på SQL-databas med hello Azure Portal](../sql-database/sql-database-configure-firewall-settings.md) toospecify hello IP-adressintervall. När du konfigurerar hello SQL-brandväggsregler ange hello IP-intervall för hello undernät som har angetts för hello Azure RemoteApp-samling. Det här ger hello ARA-servrar tooconnect toohello SQL DB även om de kommer har dynamiskt tilldelade IP-adresser.

## <a name="troubleshooting"></a>Felsökning
Om hello får med ett klientprogram som finns i Azure RemoteApp som ansluter tooa SQL-databas som finns på Azure eller lokalt är långsam det kan finnas flera skäl varför.  

* Nätverksfördröjningen från enheten-tooAzure är hög. Flytta toohello bästa och snabbaste nätverksanslutningen du kan för bästa prestanda. Använd [azurespeed.com](http://azurespeed.com/) som ett allmänt verktyg tootest dina enheter latens tooAzure datacenter.  
* Klientappen som finns i Azure RemoteApp är överbelastad. Öka prestandan genom att välja en annan faktureringsplan, till exempel Premiumfakturering. Ett annat tips är toomonitor hello resurser som programmet förbrukar: utför en ctrl + alt + end tangentsekvensen som startar hello SAS-skärmen, väljer Aktivitetshanteraren och se resursanvändningen för din app under en aktiv session.
* SQL-servern är överbelastad eller har inte optimerats. Följ SQL-riktlinjerna för felsökning. 

