---
title: "aaaConnect befintliga Azure App Service tooAzure för MySQL-databas | Microsoft Docs"
description: "Instruktioner för hur tooproperly ansluter en befintlig Azure App Service-tooAzure databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6d5b16f316e186d665370adcd8b7c7bb38c8d51a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a>Anslut en befintlig Azure App Service-tooAzure databas för MySQL-server
Det här dokumentet förklarar hur tooconnect en befintlig Azure App Service-tooyour Azure-databas för MySQL-servern.

## <a name="before-you-begin"></a>Innan du börjar
Logga in toohello [Azure-portalen](https://portal.azure.com). Skapa en Azure-databas för MySQL-servern. Mer information finns för[hur toocreate Azure-databas för MySQL-servern från portalen](quickstart-create-mysql-server-database-using-azure-portal.md) eller [hur toocreate Azure-databas för MySQL-servern med hjälp av CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

Det finns två lösningar tooenable åtkomst från en Azure App Service-tooan Azure-databas för MySQL. Båda lösningarna omfattar att konfigurera brandväggsregler på servernivå.

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a>Lösning 1 - skapa en brandvägg regeln tooallow alla IP-adresser
Azure-databas för MySQL skyddar åtkomst med hjälp av en brandvägg tooprotect dina data. När du ansluter från Azure App Service-tooAzure databas för MySQL-server, Tänk på att hello utgående är IP-adresser Apptjänst dynamiska. 

tooensure hello tillgängligheten för Azure App Service, bör du använda den här lösningen tooallow alla IP-adresser.

> [!NOTE]
> Microsoft arbetar för en långsiktig lösning tooavoid så att alla IP-adresser för Azure-tjänster tooconnect tooAzure databas för MySQL.

1. På hello serverblad MySQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure för MySQL-databas.

   ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Ange **REGELNAMN**, **START-IP**, och **sista IP**. Klicka sedan på **Spara**.
   - Regelnamnet: Tillåt-alla-IP-adresser
   - Start-IP: 0.0.0.0
   - Avsluta IP: 255.255.255.255

   ![Azure portal – Lägg till alla IP-adresser](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a>Lösning 2 – Skapa en brandvägg regeln tooexplicitly Tillåt utgående IP-adresser
Du kan uttryckligen lägga till alla hello utgående IP-adresser i Azure App Service.

1. På hello App Service egenskapsbladet, visa din **utgående IP-adress**.

   ![Azure portal – Visa utgående IP-adresser](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. Lägg till utgående IP-adresser i taget på hello MySQL-anslutning säkerhet bladet.

   ![Azure portal – Lägg till explicita IP-adresser](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Kom ihåg för**spara** brandväggsreglerna.

Även om hello Azure App service försöker tookeep IP-adresser konstant över tid, finns det fall där hello IP-adresser kan ändra. Till exempel hello när app återanvänder en skalningsåtgärden inträffar eller när nya datorer läggs till i Azure regionala data centers tooincrease hello kapacitet. När hello IP-adresserna ändras, hello app kan uppleva avbrottstid hello händelsen som den kan inte längre ansluta toohello MySQL-servern. Överväg att denna möjlighet när du väljer ett av hello föregående lösningar.

## <a name="ssl-configuration"></a>SSL-konfiguration
Azure MySQL-databas har SSL aktiverat som standard. Om programmet inte använder SSL tooconnect toohello databas måste toodisable SSL på MySQL-servern. Mer information om hur tooconfigure SSL, se [med hjälp av SSL med Azure-databas för MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Nästa steg
Mer information om anslutningssträngar finns för[anslutningssträngar](howto-connection-string.md).
