---
title: "aaaCreate och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen | Microsoft Docs"
description: "Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a>Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen
Brandväggsregler på servernivå aktivera administratörer tooaccess en Azure-databas för MySQL-Server från en angiven IP-adress eller intervall av IP-adresser. 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Skapa en brandväggsregel på servernivå i hello Azure-portalen

1. På hello serverblad MySQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure för MySQL-databas.

   ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klicka på **Lägg till Min IP** på hello verktygsfältet toocreate en regel med hello IP-adressen för datorn som uppfattas av hello Azure system.

   ![Azure portal – Klicka på Lägg till Min IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Kontrollera din IP-adress innan du sparar hello konfiguration. I vissa situationer kan följas av Azure-portalen hello IP-adress skiljer sig från hello IP-adress som används för att komma åt när hello internet och Azure-servrar. Därför kanske du måste toochange hello första IP- och slut-IP toomake hello regeln funktion som förväntat.

   Använd en sökmotor eller andra online verktyget toocheck din egen IP-adress (till exempel Sök ”vad är IP-adress”).

   ![Bing för vad som är Min IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Lägg till ytterligare adressintervall. Du kan ange en IP-adress eller ett adressintervall i hello regler för hello Azure-databas för MySQL-brandväggen. Om du vill toolimit hello regeln tooone enskild IP-adress, typen hello samma adress i hello-fältet för första IP- och slut-IP. Öppna hello brandväggen kan administratörer och användare tooaccess alla databaser på hello MySQL server toowhich de har giltiga autentiseringsuppgifter.

   ![Azure portal – brandväggsregler ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Klicka på **spara** på hello verktygsfältet toosave den här brandväggsregeln på servernivå. Vänta tills hello bekräftelse att hello uppdatering toohello brandväggsregler lyckades.

   ![Azure portal – Klicka på Spara](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Hantera befintliga servernivå brandväggsregler via hello Azure-portalen
Upprepa hello steg toomanage hello brandväggsregler.
* tooadd hello aktuella datorn, klickar du på **+ Lägg till Min IP**.
* tooadd ytterligare IP-adresser, Skriv i hello **REGELNAMN**, **första IP-**, och **sista IP**.
* toomodify en befintlig regel, klicka på någon av hello fält i hello regeln och ändra.
* toodelete en befintlig regel på hello punkter [...] och klicka på **ta bort**.
* Klicka på **spara** toosave hello ändringar.

## <a name="next-steps"></a>Nästa steg
- Hjälp med att ansluta tooan Azure-databas för MySQL-servern finns [anslutningsbibliotek för Azure-databas för MySQL](./concepts-connection-libraries.md)
