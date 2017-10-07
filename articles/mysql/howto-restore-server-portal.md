---
title: "aaaHow tooRestore en Server i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskriver hur toorestore en server i Azure-databas för att använda MySQL hello Azure-portalen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a>Hur tooBackup och återställa en server i Azure-databas för att använda MySQL hello Azure-portalen

## <a name="backup-happens-automatically"></a>Säkerhetskopieringen sker automatiskt
När du använder Azure-databas för MySQL görs hello databastjänsten en säkerhetskopia av hello tjänsten var femte minut. 

hello säkerhetskopior är tillgängliga för 7 dagar vid användning av grundläggande nivån och 35 dagar när du använder standardnivån. Mer information finns i [Azure-databas för MySQL-servicenivåer](concepts-service-tiers.md)

Med hjälp av funktionen för automatisk säkerhetskopiering kan du återställa hello server och alla databaser i en ny server tooan tidigare punkt i tiden.

## <a name="restore-in-hello-azure-portal"></a>Återställa hello Azure-portalen
Azure MySQL-databas kan du toorestore hello servern tillbaka tooa punkt i tiden och till tooa nya kopian av hello-server. Du kan använda den här nya servern toorecover dina data. 

Till exempel om en tabell av misstag kan bort kl. tolv idag, du återställa toohello gång innan på dagen och hämta hello saknas tabellen och data från den nya kopian av hello-server.

hello återställningspunkt följande hello exempel server tooa i tid:

1. Logga in på hello [Azure-portalen](https://portal.azure.com/)

2. Hitta din Azure-databas för MySQL-servern. I hello vänster och välj **alla resurser**, markera din server hello-listan.

3.  Klicka på hello överkant hello serverblad översikt **återställa** hello i verktygsfältet. hello återställning blad öppnas.
![Klicka på återställningsknappen](./media/howto-restore-server-portal/click-restore-button.png)

4. Fyll i hello återställning formuläret med hello krävs information:

- **Återställningspunkt (UTC)**: använder hello väljaren för datum och tid Väljaren, Välj en tidpunkt i toorestore till. hello-tid som anges är i UTC-format, så du måste förmodligen tooconvert hello lokaltid till UTC.
- **Återställa toonew server**: Ange en ny server name toorestore hello befintlig server till.
- **Plats**: hello region val automatiskt fylls med hello källa server region och kan inte ändras.
- **Prisnivån**: hello priser nivå val automatiskt fylls med hello priser för samma nivå som hello källservern och kan inte ändras här. 
![PITR-återställning](./media/howto-restore-server-portal/pitr-restore.png)

5. Klicka på **OK** toorestore hello server toorestore tooa tidpunkt. 

6. Leta upp hello ny server som har skapats tooverify hello databaser har återställts korrekt när hello återställning har slutförts.

## <a name="next-steps"></a>Nästa steg
- [Anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)