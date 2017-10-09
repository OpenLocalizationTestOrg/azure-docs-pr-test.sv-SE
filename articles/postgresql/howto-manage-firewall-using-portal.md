---
title: "aaaCreate och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen | Microsoft Docs"
description: "Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a>Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen
Brandväggsregler på servernivå aktivera administratörer tooaccess en Azure-databas för PostgreSQL-Server från en angiven IP-adress eller intervall av IP-adresser. 

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En server [skapa en Azure-databas för PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Skapa en brandväggsregel på servernivå i hello Azure-portalen
1. På hello serverblad PostgreSQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure-databas för PostgreSQL.

  ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klicka på **Lägg till Min IP** hello i verktygsfältet. Detta skapar automatiskt en regel med hello IP-adressen för datorn som uppfattade av hello Azure system.

  ![Azure portal – Klicka på Lägg till Min IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Kontrollera din IP-adress innan du sparar hello konfiguration. I vissa situationer kan följas av Azure-portalen hello IP-adress skiljer sig från hello IP-adress som används för att komma åt när hello internet och Azure-servrar. Därför kanske du måste toochange hello första IP- och slut-IP toomake hello regeln funktion som förväntat.
Använd en sökmotor eller andra online verktyget toocheck din egen IP-adress (till exempel Bing search som ”hur är Min IP”).

  ![Bing-Sök efter vad som är Min IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Lägg till ytterligare adressintervall. Du kan ange en IP-adress eller ett adressintervall i hello regler för hello Azure-databas för PostgreSQL-brandväggen. Om du vill toolimit hello regeln tooone enskild IP-adress, typen hello samma adress i hello-fältet för första IP- och slut-IP. Om du öppnar hello brandväggen kan administratörer och användare toologin tooany databasen på hello PostgreSQL server toowhich de har giltiga autentiseringsuppgifter.

  ![Azure portal – brandväggsregler ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klicka på **spara** på hello verktygsfältet toosave den här brandväggsregeln på servernivå. Vänta tills hello bekräftelse att hello uppdatering toohello brandväggsregler lyckades.

  ![Azure portal – Klicka på Spara](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Hantera befintliga servernivå brandväggsregler via hello Azure-portalen
Upprepa hello steg toomanage hello brandväggsregler.
* tooadd hello aktuella datorn, klicka på hello för + **Lägg till Min IP**. Klicka på **spara** toosave hello ändringar.
* tooadd ytterligare IP-adresser, Skriv i hello Regelnamn, starta IP-adress och sista IP-adress. Klicka på **spara** toosave hello ändringar.
* toomodify en befintlig regel, klicka på någon av hello fält i hello regeln och ändra. Klicka på **spara** toosave hello ändringar.
* toodelete en befintlig regel hello punkter [...] och klicka på Ta bort ta bort hello regel. Klicka på **spara** toosave hello ändringar.

## <a name="next-steps"></a>Nästa steg
- På liknande sätt kan du skapa skript för[skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI](howto-manage-firewall-using-cli.md)
- Hjälp med att ansluta tooan Azure-databas för PostgreSQL-servern finns [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)
