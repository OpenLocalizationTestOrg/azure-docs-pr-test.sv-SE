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
# <a name="azure-database-for-mysql-use-mysql-workbench-tooconnect-and-query-data"></a><span data-ttu-id="6918f-103">Azure-databas för MySQL: Använd MySQL arbetsstationen tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="6918f-103">Azure Database for MySQL: Use MySQL Workbench tooconnect and query data</span></span>
<span data-ttu-id="6918f-104">Den här snabbstarten visar hur tooconnect tooan Azure Database för att använda MySQL hello MySQL arbetsstationen program.</span><span class="sxs-lookup"><span data-stu-id="6918f-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using hello MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6918f-105">Krav</span><span class="sxs-lookup"><span data-stu-id="6918f-105">Prerequisites</span></span>
<span data-ttu-id="6918f-106">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="6918f-106">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="6918f-107">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6918f-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="6918f-108">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6918f-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="6918f-109">Installera MySQL arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="6918f-109">Install MySQL Workbench</span></span>
<span data-ttu-id="6918f-110">Hämta och installera MySQL-arbetsstationen på din dator från [hello MySQL webbplats](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="6918f-110">Download and install MySQL Workbench on your computer from [hello MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="6918f-111">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="6918f-111">Get connection information</span></span>
<span data-ttu-id="6918f-112">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6918f-112">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="6918f-113">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="6918f-113">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="6918f-114">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6918f-114">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="6918f-115">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6918f-115">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="6918f-116">Klicka på hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="6918f-116">Click hello server name.</span></span>

4. <span data-ttu-id="6918f-117">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="6918f-117">Select hello server's **Properties** page.</span></span> <span data-ttu-id="6918f-118">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="6918f-118">Make a note of hello **Server name** and **Server admin login name**.</span></span>

 ![Azure-databas för MySQL-servernamn](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="6918f-120">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="6918f-120">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-toohello-server-using-mysql-workbench"></a><span data-ttu-id="6918f-121">Ansluta toohello servern med hjälp av MySQL arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="6918f-121">Connect toohello server using MySQL Workbench</span></span> 
<span data-ttu-id="6918f-122">tooconnect tooAzure MySQL server verktyget hello GUI MySQL arbetsstationen:</span><span class="sxs-lookup"><span data-stu-id="6918f-122">tooconnect tooAzure MySQL server using hello GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="6918f-123">Starta hello MySQL arbetsstationen program på datorn.</span><span class="sxs-lookup"><span data-stu-id="6918f-123">Launch hello MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="6918f-124">I **installera ny anslutning** dialogrutan Ange följande information på hello hello **parametrar** fliken:</span><span class="sxs-lookup"><span data-stu-id="6918f-124">In **Setup New Connection** dialog box, enter hello following information on hello **Parameters** tab:</span></span>

    ![konfigurera ny anslutning](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="6918f-126">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="6918f-126">**Setting**</span></span> | <span data-ttu-id="6918f-127">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="6918f-127">**Suggested value**</span></span> | <span data-ttu-id="6918f-128">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="6918f-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="6918f-129">Anslutningsnamn</span><span class="sxs-lookup"><span data-stu-id="6918f-129">Connection Name</span></span> | <span data-ttu-id="6918f-130">Demoanslutning</span><span class="sxs-lookup"><span data-stu-id="6918f-130">Demo Connection</span></span> | <span data-ttu-id="6918f-131">Ange ett namn på anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6918f-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="6918f-132">Anslutningsmetod</span><span class="sxs-lookup"><span data-stu-id="6918f-132">Connection Method</span></span> | <span data-ttu-id="6918f-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="6918f-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="6918f-134">Standard (TCP/IP) är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="6918f-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="6918f-135">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="6918f-135">Hostname</span></span> | <span data-ttu-id="6918f-136">*servernamn*</span><span class="sxs-lookup"><span data-stu-id="6918f-136">*server name*</span></span> | <span data-ttu-id="6918f-137">Ange hello server namn-värde som används när du skapade hello Azure-databas för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="6918f-137">Specify hello server name value that was used when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="6918f-138">Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd hello fullständigt kvalificerade domännamnet (\*. mysql.database.azure.com) enligt hello exempel.</span><span class="sxs-lookup"><span data-stu-id="6918f-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use hello fully qualified domain name (\*.mysql.database.azure.com) as shown in hello example.</span></span> <span data-ttu-id="6918f-139">Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg namnet på servern.</span><span class="sxs-lookup"><span data-stu-id="6918f-139">Follow hello steps in hello previous section tooget hello connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="6918f-140">Port</span><span class="sxs-lookup"><span data-stu-id="6918f-140">Port</span></span> | <span data-ttu-id="6918f-141">3306</span><span class="sxs-lookup"><span data-stu-id="6918f-141">3306</span></span> | <span data-ttu-id="6918f-142">Använd alltid port 3306 vid anslutning tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6918f-142">Always use port 3306 when connecting tooAzure Database for MySQL.</span></span> |
    | <span data-ttu-id="6918f-143">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="6918f-143">Username</span></span> |  <span data-ttu-id="6918f-144">*inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="6918f-144">*server admin login name*</span></span> | <span data-ttu-id="6918f-145">Skriv i hello server inloggningen administratörsanvändarnamnet angavs när du skapade hello Azure-databas för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="6918f-145">Type in hello server admin login username supplied when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="6918f-146">Vår användarnamn i exemplet är myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="6918f-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="6918f-147">Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="6918f-147">Follow hello steps in hello previous section tooget hello connection information if you do not remember hello username.</span></span> <span data-ttu-id="6918f-148">hello-formatet är  *username@servername* .</span><span class="sxs-lookup"><span data-stu-id="6918f-148">hello format is *username@servername*.</span></span>
    | <span data-ttu-id="6918f-149">Lösenord</span><span class="sxs-lookup"><span data-stu-id="6918f-149">Password</span></span> | <span data-ttu-id="6918f-150">ditt lösenord</span><span class="sxs-lookup"><span data-stu-id="6918f-150">your password</span></span> | <span data-ttu-id="6918f-151">Klicka på **Store i valvet...**  knappen toosave hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="6918f-151">Click **Store in Vault...** button toosave hello password.</span></span> |

3.   <span data-ttu-id="6918f-152">Klicka på **Testanslutningen** tootest om alla parametrar är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="6918f-152">Click **Test Connection** tootest if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="6918f-153">Klicka på **OK** toosave hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="6918f-153">Click **OK** toosave hello connection.</span></span> 

5.   <span data-ttu-id="6918f-154">I hello lista över **MySQL anslutningar**och klicka på hello motsvarande tooyour bildrutsservern vänta hello anslutning toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6918f-154">In hello listing of **MySQL Connections**, click hello tile corresponding tooyour server and wait for hello connection toobe established.</span></span>

6.   <span data-ttu-id="6918f-155">En ny SQL-flik öppnas med en tom redigerare kan du ange dina frågor.</span><span class="sxs-lookup"><span data-stu-id="6918f-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6918f-156">Som standard, krävs SSL-anslutning, säkerhet och tillämpas på din Azure-databas för MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="6918f-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="6918f-157">Vanligtvis krävs ingen ytterligare konfiguration med SSL-certifikat för MySQL arbetsstationen tooconnect tooyour server.</span><span class="sxs-lookup"><span data-stu-id="6918f-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench tooconnect tooyour server.</span></span> <span data-ttu-id="6918f-158">Mer information om SSL finns [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="6918f-158">For more information on SSL, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="6918f-159">Om du behöver toodisable SSL finns hello Azure-portalen och klicka hello anslutning säkerhet sidan toodisable hello framtvinga SSL-anslutning växla.</span><span class="sxs-lookup"><span data-stu-id="6918f-159">If you need toodisable SSL, visit hello Azure portal and click hello Connection security page toodisable hello Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="6918f-160">Skapa en tabell, infoga data, läsa data, uppdatera data, ta bort data</span><span class="sxs-lookup"><span data-stu-id="6918f-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="6918f-161">Kopiera och klistra in hello SQL exempelkod till en tom SQL fliken tooillustrate exempeldata.</span><span class="sxs-lookup"><span data-stu-id="6918f-161">Copy and paste hello sample SQL code into a blank SQL tab tooillustrate some sample data.</span></span>

    <span data-ttu-id="6918f-162">Den här koden skapar en tom databas med namnet quickstartdb och skapar sedan exempeltabell med namnet inventering.</span><span class="sxs-lookup"><span data-stu-id="6918f-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="6918f-163">Infogar vissa rader därefter läser hello rader.</span><span class="sxs-lookup"><span data-stu-id="6918f-163">It inserts some rows, then reads hello rows.</span></span> <span data-ttu-id="6918f-164">Hello data med en update-instruktion ändras och läser hello rader igen.</span><span class="sxs-lookup"><span data-stu-id="6918f-164">It changes hello data with an update statement, and reads hello rows again.</span></span> <span data-ttu-id="6918f-165">Slutligen den tar bort en rad och läser hello rader igen.</span><span class="sxs-lookup"><span data-stu-id="6918f-165">Finally it deletes a row, and reads hello rows again.</span></span>
    
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

    <span data-ttu-id="6918f-166">hello skärmbild visar ett exempel på hello SQL-kod i SQL-arbetsstationen och hello utdata när den har körts.</span><span class="sxs-lookup"><span data-stu-id="6918f-166">hello screenshot shows an example of hello SQL code in SQL Workbench and hello output after it has been run.</span></span>
    
    ![MySQL-arbetsstationen SQL fliken toorun exempelkoden SQL](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="6918f-168">toorun hello exempel SQL-kod, klickar du på hello ljusare bult ikonen i hello verktygsfältet för hello **SQL-filen** fliken.</span><span class="sxs-lookup"><span data-stu-id="6918f-168">toorun hello sample SQL Code, click hello lightening bolt icon in hello toolbar of hello **SQL File** tab.</span></span>
3. <span data-ttu-id="6918f-169">Lägg märke till hello tre flikar resulterar i hello **rutnätet** del hello mitten av hello sida.</span><span class="sxs-lookup"><span data-stu-id="6918f-169">Notice hello three tabbed results in hello **Result Grid** section in hello middle of hello page.</span></span> 
4. <span data-ttu-id="6918f-170">Meddelande hello **utdata** lista på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="6918f-170">Notice hello **Output** list at hello bottom of hello page.</span></span> <span data-ttu-id="6918f-171">hello status för varje kommando visas.</span><span class="sxs-lookup"><span data-stu-id="6918f-171">hello status of each command is shown.</span></span> 

<span data-ttu-id="6918f-172">Nu kan du har anslutit tooAzure databas för MySQL med MySQL arbetsstationen och har frågat data med hjälp av hello SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="6918f-172">Now, you have connected tooAzure Database for MySQL using MySQL Workbench, and have queried data using hello SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6918f-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6918f-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6918f-174">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="6918f-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
