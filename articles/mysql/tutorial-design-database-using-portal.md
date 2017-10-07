---
title: "aaaDesign ditt första Azure-databas för MySQL - databas i Azure Portal | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur toocreate och hantera Azure-databas för MySQL-servern och databasen med hjälp av Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Utforma din första Azure-databas för MySQL-databas
Azure MySQL-databas är en hanterad tjänst som gör att du toorun, hantera och skala högtillgänglig MySQL-databaser i hello molnet. Med hello Azure-portalen kan du enkelt hantera servern och skapar en databas.

I den här kursen använder du hello Azure portal toolearn hur till:

> [!div class="checklist"]
> * Skapa en Azure-databas för MySQL
> * Konfigurera hello serverbrandvägg
> * Använda mysql kommandoradsverktyget toocreate en databas
> * Läs in exempeldata
> * Frågedata
> * Uppdatera data
> * Återställa data

## <a name="sign-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen
Öppna valfri webbläsare och gå hello [Microsoft Azure-portalen](https://portal.azure.com/). Ange dina autentiseringsuppgifter toosign toohello-portalen. hello standardvyn är instrumentpanelen service.

## <a name="create-an-azure-database-for-mysql-server"></a>Skapa en Azure Database för MySQL-server
En Azure Database för MySQL-server skapas med en definierad uppsättning [compute- och lagringsresurser](./concepts-compute-unit-and-storage.md). hello server skapas inom en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1. Navigera för**databaser** > **Azure-databas för MySQL**. Om du inte hittar MySQL-servern under **databaser** kategori, klickar du på **se alla** tooshow alla tillgängliga services-databas. Du kan också skriva **Azure-databas för MySQL** hello Sök rutan tooquickly hitta hello-tjänsten.
![2-1 analysera tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klicka på **Azure-databas för MySQL** panelen och klicka sedan på **skapa**.

I vårt exempel fyller du i hello Azure-databas för MySQL formulär med hello följande information:

| **Inställning** | **Föreslaget värde** | **Fältbeskrivning** |
|---|---|---|
| *Servernamn* | myserver4demo  | Servernamn har toobe globalt unika. |
| *Prenumeration* | mysubscription | Välj prenumerationen hello i listrutan. |
| *Resursgrupp* | myresourcegroup | Skapa en resursgrupp eller välj en befintlig. |
| *Inloggning för serveradministratör* | myadmin | Konfigurera admin kontonamn. |
| *Lösenord* |  | Ange ett starkt administratörslösenord. |
| *Bekräfta lösenord* |  | Bekräfta hello admin kontolösenord. |
| *Plats* |  | Välj en tillgänglig region. |
| *Version* | 5.7 | Välj hello senaste versionen. |
| *Konfigurera prestanda* | Basic, 50 compute enheter, 50 GB  | Välj **Prisnivå**, **Compute-enheter**, **Lagring (GB)** och klicka på **OK**. |
| *PIN-kod tooDashboard* | Markera | Rekommenderade toocheck den här rutan så att du kan hitta hello server enkelt vid ett senare tillfälle |
Klicka på **Skapa**. En ny Azure-databas för MySQL-servern körs i en minut eller två i hello molnet. Du kan klicka på **meddelanden** distributionsprocessen för hello verktygsfältet toomonitor hello-knappen.

## <a name="configure-firewall"></a>Konfigurera brandväggen
Azure-databaser för MySQL skyddas av en brandvägg. Som standard är alla anslutningar toohello server och hello databaser hello-servern avvisade. Innan du ansluter tooAzure databas för MySQL för hello första gången, konfigurera hello brandväggen tooadd hello klienten datorns offentliga IP-adress (eller IP-adressintervall).

1. Klicka på din nya server och klicka sedan på **anslutningssäkerhet**.
   ![3-1 anslutningssäkerhet](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Du kan **Lägg till Min IP**, eller konfigurera brandväggsregler här. Kom ihåg tooclick **spara** när du har skapat hello regler.
Nu kan du ansluta toohello server med mysql-kommandoradsverktyget eller MySQL arbetsstationen GUI-verktyg.

> [!TIP]
> Azure-databas för MySQL-servern kommunicerar via port 3306. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 3306 inte av ditt nätverks brandvägg. I så fall, kan du inte ansluta tooAzure MySQL-servern om din IT-avdelning öppnar port 3306.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Get-hello fullständigt kvalificerade **servernamn** och **serverinloggningsnamnet för admin** för din Azure-databas för MySQL-servern från hello Azure-portalen. Du använder kommandoradsverktyget för mysql hello fullständiga server name tooconnect tooyour server. 

1. I [Azure-portalen](https://portal.azure.com/), klickar du på **alla resurser** hello vänstra menyn, hello typnamn och Sök efter din Azure-databas för MySQL-servern. Välj hello namn tooview hello serverinformation.

2. Under hello inställningar rubrik, klickar du på **egenskaper**. Notera **servernamn** och **SERVERINLOGGNINGSNAMNET för ADMIN**. Du kan klicka på hello Kopiera knappen Nästa tooeach fältet toocopy toohello Urklipp.
   ![4-2-serveregenskaper](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

I det här exemplet hello servernamnet är *myserver4demo.mysql.database.azure.com*, och hello inloggning för serveradministratör är  *myadmin@myserver4demo* .

## <a name="connect-toohello-server-using-mysql"></a>Ansluta toohello servern med hjälp av mysql
Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish anslutning tooyour Azure-databas för MySQL-servern. Du kan köra hello mysql-kommandoradsverktyget från hello Azure Cloud Shell i hello webbläsare eller från din egen dator med hjälp av mysql-verktygen som installeras lokalt. toolaunch hello Azure Cloud Shell, klicka på hello `Try It` på ett kodblock i den här artikeln eller besök hello Azure-portalen och klicka sedan på hello `>_` ikon i hello översta högra verktygsfältet. 

Skriv hello kommandot tooconnect:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Skapa en tom databas
När du är ansluten toohello server kan du skapa en tom databas toowork med.
```sql
CREATE DATABASE mysampledb;
```

Kör hello efter kommandot tooswitch toothis nyskapad databas hello Kommandotolken:
```sql
USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a>Skapa tabeller i hello-databas
Nu när du vet hur tooconnect toohello Azure-databas för MySQL-databas, det kan gå igenom hur toocomplete vissa grundläggande uppgifter.

Vi kan först skapa en tabell och läsa in den med vissa data. Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a>Läs in data till hello tabeller
Nu när vi har en tabell kan vi infoga vissa data i den. Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nu har du två rader med exempeldata till hello-tabell som du skapade tidigare.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Fråga efter och uppdatera hello data i hello tabeller
Kör följande fråga tooretrieve information från hello databastabell hello.
```sql
SELECT * FROM inventory;
```

Du kan också uppdatera hello data i hello tabeller.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

hello rad uppdateras i enlighet med detta när du hämtar data.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Återställa en databas tooa tidigare punkt i tiden
Anta att du har tagits bort av misstag en viktigt-databastabell och kan inte återställa hello data enkelt. Azure MySQL-databas kan du toorestore hello server tooa punkt i tiden, skapar en kopia av hello databaser till en ny server. Du kan använda den här nya servern toorecover dina data. hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.

1. Leta upp din Azure-databas för MySQL i hello Azure-portalen. På hello **översikt** klickar du på **återställa** hello i verktygsfältet. hello återställning sidan öppnas.

   ![10-1 Återställ en databas](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Fyll i hello **återställa** formulär med hello krävs information.
   
   ![10-2-formulär för återställning](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Återställningspunkt**: Välj en i tidpunkt som du vill toorestore, inom hello tidsintervall som anges. Se till att tooconvert tooUTC din lokala tidszon.
   - **Återställa toonew server**: Ange ett nytt servernamn som du vill toorestore till.
   - **Plats**: hello region är samma som källservern hello och kan inte ändras.
   - **Prisnivån**: hello prisnivån är hello samma som hello datakälla server och kan inte ändras.
   
3. Klicka på **OK** toorestore hello server för[tooa återställningspunkt i tid](./howto-restore-server-portal.md) innan hello tabellen har tagits bort. Återställa en server skapar en ny kopia av hello-servern, hello tidpunkt du anger. 

## <a name="next-steps"></a>Nästa steg
I den här kursen använder du hello Azure portal toolearned hur till:

> [!div class="checklist"]
> * Skapa en Azure-databas för MySQL
> * Konfigurera hello serverbrandvägg
> * Använda mysql kommandoradsverktyget toocreate en databas
> * Läs in exempeldata
> * Frågedata
> * Uppdatera data
> * Återställa data

> [!div class="nextstepaction"]
> [Hur tooconnect program tooAzure databas för MySQL](./howto-connection-string.md)
