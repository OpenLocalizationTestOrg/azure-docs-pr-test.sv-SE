---
title: "Hur tooRestore en Server i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Den här artikeln beskriver hur toorestore en server i Azure-databas för att använda PostgreSQL hello Azure-portalen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: bc7351f384607397806d837afd3e1d7a26575e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-postgresql-using-hello-azure-portal"></a>Hur tooBackup och återställa en server i Azure-databas för att använda PostgreSQL hello Azure-portalen

## <a name="backup-happens-automatically"></a>Säkerhetskopieringen sker automatiskt
När du använder Azure-databas för PostgreSQL görs hello databastjänsten en säkerhetskopia av hello tjänsten var femte minut. 

hello säkerhetskopior är tillgängliga för 7 dagar vid användning av grundläggande nivån och 35 dagar när du använder standardnivån. Mer information finns i [Azure-databas för PostgreSQL-servicenivåer](concepts-service-tiers.md)

Med hjälp av funktionen för automatisk säkerhetskopiering kan du återställa hello server och alla databaser i en ny server tooan tidigare punkt i tiden.

## <a name="restore-in-hello-azure-portal"></a>Återställa hello Azure-portalen
Azure PostgreSQL-databas kan du toorestore hello servern tillbaka tooa punkt i tiden och till tooa nya kopian av hello server. Du kan använda den här nya servern toorecover dina data. 

Till exempel om en tabell av misstag kan bort kl. tolv idag, du återställa toohello gång innan på dagen och hämta hello saknas tabellen och data från den nya kopian av hello-server.

hello återställningspunkt följande hello exempel server tooa i tid:
1. Logga in på hello [Azure-portalen](https://portal.azure.com/)
2. Hitta din Azure-databas för PostgreSQL-servern. I hello Azure-portalen klickar du på **alla resurser** från hello vänstra menyn och Skriv hello namn som **mypgserver 20170401**, toosearch för din befintliga server. Klicka på hello servernamn som anges i hello sökresultatet. Hej **översikt** sidan för servern öppnas och visar alternativ för ytterligare konfiguration.

   ![Azure portal – Sök toolocate servern](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Klicka på hello överkant hello serverblad översikt **återställa** hello i verktygsfältet. hello återställning blad öppnas.

   ![Azure-databas för knappen PostgreSQL - översikt – Återställ](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Fyll i hello återställning formuläret med hello krävs information:

   ![Azure-databas för PostgreSQL - information för återställningspunkter ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Återställningspunkt**: Välj en i tidpunkt som inträffar innan hello-servern har ändrats
  - **Målservern**: Ange ett nytt servernamn som du vill toorestore till
  - **Plats**: du kan inte välja hello region, som standard är det samma som källservern hello
  - **Prisnivån**: du kan inte ändra det här värdet när du återställer en server. Det är samma som hello källservern. 

5. Klicka på **OK** toorestore hello server toorestore tooa tidpunkt. 

6. Leta upp hello nya server som skapas tooverify hello data har återställts korrekt när hello återställning har slutförts.

## <a name="next-steps"></a>Nästa steg
- [Anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)
