---
title: "Ansluta tooAzure databas för MySQL från MySQL arbetsstationen | Microsoft Docs"
description: "Denna Snabbstart innehåller hello steg toouse MySQL arbetsstationen tooconnect och fråga data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: c64fcb9bb99ba06aa3a95eec420d5d5ef4a31d14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd MySQL arbetsstationen tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure Database för att använda MySQL hello MySQL arbetsstationen program. 

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>Installera MySQL arbetsstationen
Hämta och installera MySQL-arbetsstationen på din dator från [hello MySQL webbplats](https://dev.mysql.com/downloads/workbench/).

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).

2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **myserver4demo**.

3. Klicka på hello servernamn.

4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.

 ![Azure-databas för MySQL-servernamn](./media/connect-workbench/1-server-properties-name-login.png)
 
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="connect-toohello-server-using-mysql-workbench"></a>Ansluta toohello servern med hjälp av MySQL arbetsstationen 
tooconnect tooAzure MySQL server verktyget hello GUI MySQL arbetsstationen:

1.  Starta hello MySQL arbetsstationen program på datorn. 

2.  I **installera ny anslutning** dialogrutan Ange följande information på hello hello **parametrar** fliken:

    ![konfigurera ny anslutning](./media/connect-workbench/2-setup-new-connection.png)

    | **Inställning** | **Föreslaget värde** | **Fältbeskrivning** |
    |---|---|---|
    |   Anslutningsnamn | Demoanslutning | Ange ett namn på anslutningen. |
    | Anslutningsmetod | Standard (TCP/IP) | Standard (TCP/IP) är tillräckligt. |
    | Värdnamn | *servernamn* | Ange hello server namn-värde som används när du skapade hello Azure-databas för MySQL tidigare. Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd hello fullständigt kvalificerade domännamnet (\*. mysql.database.azure.com) enligt hello exempel. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg namnet på servern.  |
    | Port | 3306 | Använd alltid port 3306 vid anslutning tooAzure databas för MySQL. |
    | Användarnamn |  *inloggning för serveradministratör* | Skriv i hello server inloggningen administratörsanvändarnamnet angavs när du skapade hello Azure-databas för MySQL tidigare. Vår användarnamn i exemplet är myadmin@myserver4demo. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg hello användarnamn. hello-formatet är  *username@servername* .
    | Lösenord | ditt lösenord | Klicka på **Store i valvet...**  knappen toosave hello lösenord. |

3.   Klicka på **Testanslutningen** tootest om alla parametrar är korrekt konfigurerade. 

4.   Klicka på **OK** toosave hello anslutning. 

5.   I hello lista över **MySQL anslutningar**och klicka på hello motsvarande tooyour bildrutsservern vänta hello anslutning toobe upprättas.

6.   En ny SQL-flik öppnas med en tom redigerare kan du ange dina frågor.

    > [!NOTE]
    > Som standard, krävs SSL-anslutning, säkerhet och tillämpas på din Azure-databas för MySQL-server. Vanligtvis krävs ingen ytterligare konfiguration med SSL-certifikat för MySQL arbetsstationen tooconnect tooyour server. Mer information om SSL finns [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).  Om du behöver toodisable SSL finns hello Azure-portalen och klicka hello anslutning säkerhet sidan toodisable hello framtvinga SSL-anslutning växla.

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a>Skapa en tabell, infoga data, läsa data, uppdatera data, ta bort data
1. Kopiera och klistra in hello SQL exempelkod till en tom SQL fliken tooillustrate exempeldata.

    Den här koden skapar en tom databas med namnet quickstartdb och skapar sedan exempeltabell med namnet inventering. Infogar vissa rader därefter läser hello rader. Hello data med en update-instruktion ändras och läser hello rader igen. Slutligen den tar bort en rad och läser hello rader igen.
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    hello skärmbild visar ett exempel på hello SQL-kod i SQL-arbetsstationen och hello utdata när den har körts.
    
    ![MySQL-arbetsstationen SQL fliken toorun exempelkoden SQL](media/connect-workbench/3-workbench-sql-tab.png)

2. toorun hello exempel SQL-kod, klickar du på hello ljusare bult ikonen i hello verktygsfältet för hello **SQL-filen** fliken.
3. Lägg märke till hello tre flikar resulterar i hello **rutnätet** del hello mitten av hello sida. 
4. Meddelande hello **utdata** lista på hello hello sidans nederkant. hello status för varje kommando visas. 

Nu kan du har anslutit tooAzure databas för MySQL med MySQL arbetsstationen och har frågat data med hjälp av hello SQL-språket.

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./concepts-migrate-import-export.md)
