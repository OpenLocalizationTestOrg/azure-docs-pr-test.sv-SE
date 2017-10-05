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
# <a name="azure-database-for-mysql-use-mysql-workbench-to-connect-and-query-data"></a><span data-ttu-id="f2b58-103">Azure-databas för MySQL: Använd MySQL-arbetsstationen för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="f2b58-103">Azure Database for MySQL: Use MySQL Workbench to connect and query data</span></span>
<span data-ttu-id="f2b58-104">Den här snabbstarten visar hur du ansluter till en Azure-databas för MySQL med programmet MySQL-arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="f2b58-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using the MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f2b58-105">Krav</span><span class="sxs-lookup"><span data-stu-id="f2b58-105">Prerequisites</span></span>
<span data-ttu-id="f2b58-106">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="f2b58-106">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="f2b58-107">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f2b58-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="f2b58-108">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f2b58-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="f2b58-109">Installera MySQL arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="f2b58-109">Install MySQL Workbench</span></span>
<span data-ttu-id="f2b58-110">Hämta och installera MySQL-arbetsstationen på din dator från [webbplatsen MySQL](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="f2b58-110">Download and install MySQL Workbench on your computer from [the MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="f2b58-111">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="f2b58-111">Get connection information</span></span>
<span data-ttu-id="f2b58-112">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f2b58-112">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="f2b58-113">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f2b58-113">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="f2b58-114">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f2b58-114">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="f2b58-115">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="f2b58-115">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="f2b58-116">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="f2b58-116">Click the server name.</span></span>

4. <span data-ttu-id="f2b58-117">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="f2b58-117">Select the server's **Properties** page.</span></span> <span data-ttu-id="f2b58-118">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="f2b58-118">Make a note of the **Server name** and **Server admin login name**.</span></span>

 ![Azure-databas för MySQL-servernamn](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="f2b58-120">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="f2b58-120">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-to-the-server-using-mysql-workbench"></a><span data-ttu-id="f2b58-121">Ansluta till servern med MySQL arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="f2b58-121">Connect to the server using MySQL Workbench</span></span> 
<span data-ttu-id="f2b58-122">Om du vill ansluta till Azure MySQL-servern med GUI-verktyget MySQL Workbench:</span><span class="sxs-lookup"><span data-stu-id="f2b58-122">To connect to Azure MySQL server using the GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="f2b58-123">Starta i MySQL Workbench på datorn.</span><span class="sxs-lookup"><span data-stu-id="f2b58-123">Launch the MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="f2b58-124">I **installera ny anslutning** dialogrutan Ange följande information den **parametrar** fliken:</span><span class="sxs-lookup"><span data-stu-id="f2b58-124">In **Setup New Connection** dialog box, enter the following information on the **Parameters** tab:</span></span>

    ![konfigurera ny anslutning](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="f2b58-126">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="f2b58-126">**Setting**</span></span> | <span data-ttu-id="f2b58-127">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="f2b58-127">**Suggested value**</span></span> | <span data-ttu-id="f2b58-128">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="f2b58-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="f2b58-129">Anslutningsnamn</span><span class="sxs-lookup"><span data-stu-id="f2b58-129">Connection Name</span></span> | <span data-ttu-id="f2b58-130">Demoanslutning</span><span class="sxs-lookup"><span data-stu-id="f2b58-130">Demo Connection</span></span> | <span data-ttu-id="f2b58-131">Ange ett namn på anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f2b58-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="f2b58-132">Anslutningsmetod</span><span class="sxs-lookup"><span data-stu-id="f2b58-132">Connection Method</span></span> | <span data-ttu-id="f2b58-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="f2b58-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="f2b58-134">Standard (TCP/IP) är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="f2b58-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="f2b58-135">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="f2b58-135">Hostname</span></span> | <span data-ttu-id="f2b58-136">*servernamn*</span><span class="sxs-lookup"><span data-stu-id="f2b58-136">*server name*</span></span> | <span data-ttu-id="f2b58-137">Ange det värde för servernamn som användes när du tidigare skapade Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f2b58-137">Specify the server name value that was used when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="f2b58-138">Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd det fullständiga domännamnet (\*.mysql.database.azure.com) som i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f2b58-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use the fully qualified domain name (\*.mysql.database.azure.com) as shown in the example.</span></span> <span data-ttu-id="f2b58-139">Följ anvisningarna i föregående avsnitt för att hitta anslutningsinformation om du inte kommer ihåg namnet på servern.</span><span class="sxs-lookup"><span data-stu-id="f2b58-139">Follow the steps in the previous section to get the connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="f2b58-140">Port</span><span class="sxs-lookup"><span data-stu-id="f2b58-140">Port</span></span> | <span data-ttu-id="f2b58-141">3306</span><span class="sxs-lookup"><span data-stu-id="f2b58-141">3306</span></span> | <span data-ttu-id="f2b58-142">Använd alltid port 3306 när du ansluter till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f2b58-142">Always use port 3306 when connecting to Azure Database for MySQL.</span></span> |
    | <span data-ttu-id="f2b58-143">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="f2b58-143">Username</span></span> |  <span data-ttu-id="f2b58-144">*inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="f2b58-144">*server admin login name*</span></span> | <span data-ttu-id="f2b58-145">Ange det användarnamn för serveradministratörsinloggning som användes när du tidigare skapade Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f2b58-145">Type in the server admin login username supplied when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="f2b58-146">Vår användarnamn i exemplet är myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="f2b58-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="f2b58-147">Följ anvisningarna i föregående avsnitt för att hitta anslutningsinformation om du inte kommer ihåg användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="f2b58-147">Follow the steps in the previous section to get the connection information if you do not remember the username.</span></span> <span data-ttu-id="f2b58-148">Formatet är *username@servername*.</span><span class="sxs-lookup"><span data-stu-id="f2b58-148">The format is *username@servername*.</span></span>
    | <span data-ttu-id="f2b58-149">Lösenord</span><span class="sxs-lookup"><span data-stu-id="f2b58-149">Password</span></span> | <span data-ttu-id="f2b58-150">ditt lösenord</span><span class="sxs-lookup"><span data-stu-id="f2b58-150">your password</span></span> | <span data-ttu-id="f2b58-151">Klicka på **Store i valvet...**  för att spara lösenordet.</span><span class="sxs-lookup"><span data-stu-id="f2b58-151">Click **Store in Vault...** button to save the password.</span></span> |

3.   <span data-ttu-id="f2b58-152">Klicka på **Testanslutning** för att testa om alla parametrar är rätt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="f2b58-152">Click **Test Connection** to test if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="f2b58-153">Klicka på **OK** spara anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f2b58-153">Click **OK** to save the connection.</span></span> 

5.   <span data-ttu-id="f2b58-154">I listan med **MySQL anslutningar**, klickar du på panelen som motsvarar din server och vänta på att anslutningen ska upprättas.</span><span class="sxs-lookup"><span data-stu-id="f2b58-154">In the listing of **MySQL Connections**, click the tile corresponding to your server and wait for the connection to be established.</span></span>

6.   <span data-ttu-id="f2b58-155">En ny SQL-flik öppnas med en tom redigerare kan du ange dina frågor.</span><span class="sxs-lookup"><span data-stu-id="f2b58-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2b58-156">Som standard, krävs SSL-anslutning, säkerhet och tillämpas på din Azure-databas för MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="f2b58-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="f2b58-157">Vanligtvis krävs ingen ytterligare konfiguration med SSL-certifikat för MySQL-arbetsstationen för att ansluta till servern.</span><span class="sxs-lookup"><span data-stu-id="f2b58-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench to connect to your server.</span></span> <span data-ttu-id="f2b58-158">Mer information om SSL finns [Konfigurera SSL-anslutning i ditt program för att ansluta säkert till Azure-databas för MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="f2b58-158">For more information on SSL, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="f2b58-159">Om du måste inaktivera SSL, finns på Azure-portalen och på sidan anslutning säkerhet om du vill inaktivera växlingsknappen framtvinga SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="f2b58-159">If you need to disable SSL, visit the Azure portal and click the Connection security page to disable the Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="f2b58-160">Skapa en tabell, infoga data, läsa data, uppdatera data, ta bort data</span><span class="sxs-lookup"><span data-stu-id="f2b58-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="f2b58-161">Kopiera och klistra in SQL exempelkoden i en tom SQL-flik för att illustrera exempeldata.</span><span class="sxs-lookup"><span data-stu-id="f2b58-161">Copy and paste the sample SQL code into a blank SQL tab to illustrate some sample data.</span></span>

    <span data-ttu-id="f2b58-162">Den här koden skapar en tom databas med namnet quickstartdb och skapar sedan exempeltabell med namnet inventering.</span><span class="sxs-lookup"><span data-stu-id="f2b58-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="f2b58-163">Vissa rader infogas läser sedan raderna.</span><span class="sxs-lookup"><span data-stu-id="f2b58-163">It inserts some rows, then reads the rows.</span></span> <span data-ttu-id="f2b58-164">Det ändrar data med en update-instruktion och läser rader igen.</span><span class="sxs-lookup"><span data-stu-id="f2b58-164">It changes the data with an update statement, and reads the rows again.</span></span> <span data-ttu-id="f2b58-165">Slutligen den tar bort en rad och läser rader igen.</span><span class="sxs-lookup"><span data-stu-id="f2b58-165">Finally it deletes a row, and reads the rows again.</span></span>
    
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

    <span data-ttu-id="f2b58-166">Skärmbilden visar ett exempel på SQL-koden i SQL-arbetsstationen och utdata när den har körts.</span><span class="sxs-lookup"><span data-stu-id="f2b58-166">The screenshot shows an example of the SQL code in SQL Workbench and the output after it has been run.</span></span>
    
    ![MySQL-arbetsstationen SQL fliken för att köra SQL-exempelkod](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="f2b58-168">Om du vill köra exemplet SQL-kod, klickar du på ikonen lightening bult i verktygsfältet på den **SQL-filen** fliken.</span><span class="sxs-lookup"><span data-stu-id="f2b58-168">To run the sample SQL Code, click the lightening bolt icon in the toolbar of the **SQL File** tab.</span></span>
3. <span data-ttu-id="f2b58-169">Lägg märke till de tre flikar resultaten i den **rutnätet** avsnitt i mitten på sidan.</span><span class="sxs-lookup"><span data-stu-id="f2b58-169">Notice the three tabbed results in the **Result Grid** section in the middle of the page.</span></span> 
4. <span data-ttu-id="f2b58-170">Observera den **utdata** lista längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="f2b58-170">Notice the **Output** list at the bottom of the page.</span></span> <span data-ttu-id="f2b58-171">Status för varje kommando visas.</span><span class="sxs-lookup"><span data-stu-id="f2b58-171">The status of each command is shown.</span></span> 

<span data-ttu-id="f2b58-172">Nu kan du har anslutit till Azure-databas för MySQL med MySQL arbetsstationen och har frågat data med hjälp av SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="f2b58-172">Now, you have connected to Azure Database for MySQL using MySQL Workbench, and have queried data using the SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2b58-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2b58-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="f2b58-174">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="f2b58-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
