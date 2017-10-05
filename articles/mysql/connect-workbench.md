---
title: "Ansluta till Azure-databas för MySQL från MySQL arbetsstationen | Microsoft Docs"
description: "Denna Snabbstart innehåller steg för att använda MySQL-arbetsstationen för att ansluta och fråga efter data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: 20a1f31ce42abb924504c4008f85420fc49aec89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-to-connect-and-query-data"></a>Azure-databas för MySQL: Använd MySQL-arbetsstationen för att ansluta och fråga efter data
Den här snabbstarten visar hur du ansluter till en Azure-databas för MySQL med programmet MySQL-arbetsstationen. 

## <a name="prerequisites"></a>Krav
I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>Installera MySQL arbetsstationen
Hämta och installera MySQL-arbetsstationen på din dator från [webbplatsen MySQL](https://dev.mysql.com/downloads/workbench/).

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Skaffa den information som du behöver för att ansluta till Azure Database för MySQL. Du behöver det fullständiga servernamnet och inloggningsuppgifter.

1. Logga in på [Azure-portalen](https://portal.azure.com/).

2. På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.

3. Klicka på servernamnet.

4. Välj sidan **Egenskaper** för servern. Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.

 ![Azure-databas för MySQL-servernamn](./media/connect-workbench/1-server-properties-name-login.png)
 
5. Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.

## <a name="connect-to-the-server-using-mysql-workbench"></a>Ansluta till servern med MySQL arbetsstationen 
Om du vill ansluta till Azure MySQL-servern med GUI-verktyget MySQL Workbench:

1.  Starta i MySQL Workbench på datorn. 

2.  I **installera ny anslutning** dialogrutan Ange följande information den **parametrar** fliken:

    ![konfigurera ny anslutning](./media/connect-workbench/2-setup-new-connection.png)

    | **Inställning** | **Föreslaget värde** | **Fältbeskrivning** |
    |---|---|---|
    |   Anslutningsnamn | Demoanslutning | Ange ett namn på anslutningen. |
    | Anslutningsmetod | Standard (TCP/IP) | Standard (TCP/IP) är tillräckligt. |
    | Värdnamn | *servernamn* | Ange det värde för servernamn som användes när du tidigare skapade Azure Database för MySQL. Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd det fullständiga domännamnet (\*.mysql.database.azure.com) som i det här exemplet. Följ anvisningarna i föregående avsnitt för att hitta anslutningsinformation om du inte kommer ihåg namnet på servern.  |
    | Port | 3306 | Använd alltid port 3306 när du ansluter till Azure Database för MySQL. |
    | Användarnamn |  *inloggning för serveradministratör* | Ange det användarnamn för serveradministratörsinloggning som användes när du tidigare skapade Azure Database för MySQL. Vår användarnamn i exemplet är myadmin@myserver4demo. Följ anvisningarna i föregående avsnitt för att hitta anslutningsinformation om du inte kommer ihåg användarnamnet. Formatet är *username@servername*.
    | Lösenord | ditt lösenord | Klicka på **Store i valvet...**  för att spara lösenordet. |

3.   Klicka på **Testanslutning** för att testa om alla parametrar är rätt konfigurerade. 

4.   Klicka på **OK** spara anslutningen. 

5.   I listan med **MySQL anslutningar**, klickar du på panelen som motsvarar din server och vänta på att anslutningen ska upprättas.

6.   En ny SQL-flik öppnas med en tom redigerare kan du ange dina frågor.

    > [!NOTE]
    > Som standard, krävs SSL-anslutning, säkerhet och tillämpas på din Azure-databas för MySQL-server. Vanligtvis krävs ingen ytterligare konfiguration med SSL-certifikat för MySQL-arbetsstationen för att ansluta till servern. Mer information om SSL finns [Konfigurera SSL-anslutning i ditt program för att ansluta säkert till Azure-databas för MySQL](./howto-configure-ssl.md).  Om du måste inaktivera SSL, finns på Azure-portalen och på sidan anslutning säkerhet om du vill inaktivera växlingsknappen framtvinga SSL-anslutning.

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a>Skapa en tabell, infoga data, läsa data, uppdatera data, ta bort data
1. Kopiera och klistra in SQL exempelkoden i en tom SQL-flik för att illustrera exempeldata.

    Den här koden skapar en tom databas med namnet quickstartdb och skapar sedan exempeltabell med namnet inventering. Vissa rader infogas läser sedan raderna. Det ändrar data med en update-instruktion och läser rader igen. Slutligen den tar bort en rad och läser rader igen.
    
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

    Skärmbilden visar ett exempel på SQL-koden i SQL-arbetsstationen och utdata när den har körts.
    
    ![MySQL-arbetsstationen SQL fliken för att köra SQL-exempelkod](media/connect-workbench/3-workbench-sql-tab.png)

2. Om du vill köra exemplet SQL-kod, klickar du på ikonen lightening bult i verktygsfältet på den **SQL-filen** fliken.
3. Lägg märke till de tre flikar resultaten i den **rutnätet** avsnitt i mitten på sidan. 
4. Observera den **utdata** lista längst ned på sidan. Status för varje kommando visas. 

Nu kan du har anslutit till Azure-databas för MySQL med MySQL arbetsstationen och har frågat data med hjälp av SQL-språket.

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./concepts-migrate-import-export.md)
