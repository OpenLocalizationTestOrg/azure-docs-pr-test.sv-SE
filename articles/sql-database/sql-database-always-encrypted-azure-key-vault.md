---
title: 'Always Encrypted: SQL Database - Azure Key Vault | Microsoft Docs'
description: "Den här artikeln visar hur du skyddar känsliga data i en SQL-databas med datakryptering guiden alltid krypterade i SQL Server Management Studio. Den innehåller också anvisningar som visar hur du lagrar varje krypteringsnyckel i Azure Key Vault."
keywords: datakryptering krypteringsnyckeln molnet kryptering
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 6ca16644-5969-497b-a413-d28c3b835c9b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: sstein
ms.openlocfilehash: 61bfd420425b4740f6d4ebc01a403a88ff351382
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="44efa-105">Always Encrypted: Skydda känsliga data i SQL-databasen och lagra krypteringsnycklar i Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="44efa-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="44efa-106">Den här artikeln visar hur du skyddar känsliga data i en SQL-databas med data kryptering med hjälp av den [krypteras alltid guiden](https://msdn.microsoft.com/library/mt459280.aspx) i [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-106">This article shows you how to secure sensitive data in a SQL database with data encryption using the [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="44efa-107">Den innehåller också anvisningar som visar hur du lagrar varje krypteringsnyckel i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="44efa-107">It also includes instructions that will show you how to store each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="44efa-108">Krypterad är alltid en teknik för kryptering av nya data i Azure SQL Database och SQL Server som skyddar känsliga data i vila på servern, under flytt mellan klienten och servern, och när data används.</span><span class="sxs-lookup"><span data-stu-id="44efa-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on the server, during movement between client and server, and while the data is in use.</span></span> <span data-ttu-id="44efa-109">Krypterad garanterar alltid att känsliga data aldrig visas i klartext i databassystemet.</span><span class="sxs-lookup"><span data-stu-id="44efa-109">Always Encrypted ensures that sensitive data never appears as plaintext inside the database system.</span></span> <span data-ttu-id="44efa-110">När du har konfigurerat datakryptering endast klientprogram eller appservrar som har tillgång till nycklarna kan komma åt data i klartext.</span><span class="sxs-lookup"><span data-stu-id="44efa-110">After you configure data encryption, only client applications or app servers that have access to the keys can access plaintext data.</span></span> <span data-ttu-id="44efa-111">Detaljerad information finns i [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="44efa-112">När du har konfigurerat databasen om du vill använda Always Encrypted skapar du ett klientprogram i C# med Visual Studio för att arbeta med krypterade data.</span><span class="sxs-lookup"><span data-stu-id="44efa-112">After you configure the database to use Always Encrypted, you will create a client application in C# with Visual Studio to work with the encrypted data.</span></span>

<span data-ttu-id="44efa-113">Följ stegen i den här artikeln och lär dig hur du ställer in Always Encrypted för en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="44efa-113">Follow the steps in this article and learn how to set up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="44efa-114">I den här artikeln får du lära dig hur du utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="44efa-114">In this article you will learn how to perform the following tasks:</span></span>

* <span data-ttu-id="44efa-115">Använd guiden Always Encrypted i SSMS för att skapa [Always Encrypted nycklar](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="44efa-115">Use the Always Encrypted wizard in SSMS to create [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="44efa-116">Skapa en [kolumnhuvudnyckeln (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="44efa-117">Skapa en [kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="44efa-118">Skapa en databastabell och kryptera kolumner.</span><span class="sxs-lookup"><span data-stu-id="44efa-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="44efa-119">Skapa ett program som infogar, väljer och visar data från de krypterade kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="44efa-119">Create an application that inserts, selects, and displays data from the encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44efa-120">Krav</span><span class="sxs-lookup"><span data-stu-id="44efa-120">Prerequisites</span></span>
<span data-ttu-id="44efa-121">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="44efa-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="44efa-122">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44efa-122">An Azure account and subscription.</span></span> <span data-ttu-id="44efa-123">Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44efa-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="44efa-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 eller senare.</span><span class="sxs-lookup"><span data-stu-id="44efa-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="44efa-125">[.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) eller senare (på klientdatorn).</span><span class="sxs-lookup"><span data-stu-id="44efa-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on the client computer).</span></span>
* <span data-ttu-id="44efa-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="44efa-127">[Azure PowerShell](/powershell/azure/overview), version 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="44efa-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="44efa-128">Typen **(Get-Module azure - ListAvailable). Version** att se vilken version av PowerShell som du kör.</span><span class="sxs-lookup"><span data-stu-id="44efa-128">Type **(Get-Module azure -ListAvailable).Version** to see what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a><span data-ttu-id="44efa-129">Aktivera ditt klientprogram att komma åt SQL Database-tjänsten</span><span class="sxs-lookup"><span data-stu-id="44efa-129">Enable your client application to access the SQL Database service</span></span>
<span data-ttu-id="44efa-130">Du måste aktivera ditt klientprogram att komma åt SQL Database-tjänsten genom att ställa in nödvändig autentisering och hämtning av den *ClientId* och *hemlighet* som du behöver att autentisera din program i följande kod.</span><span class="sxs-lookup"><span data-stu-id="44efa-130">You must enable your client application to access the SQL Database service by setting up the required authentication and acquiring the *ClientId* and *Secret* that you will need to authenticate your application in the following code.</span></span>

1. <span data-ttu-id="44efa-131">Öppna den [klassiska Azure-portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="44efa-131">Open the [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="44efa-132">Välj **Active Directory** och klicka på Active Directory-instans som programmet ska använda.</span><span class="sxs-lookup"><span data-stu-id="44efa-132">Select **Active Directory** and click the Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="44efa-133">Klicka på **program**, och klicka sedan på **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="44efa-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="44efa-134">Skriv ett namn för ditt program (till exempel: *myClientApp*), Välj **WEBBPROGRAM**, och klicka på pilen för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="44efa-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click the arrow to continue.</span></span>
5. <span data-ttu-id="44efa-135">För den **SIGN-ON-URL** och **APP-ID URI** du kan ange en giltig URL (t.ex, *http://myClientApp*) och fortsätta.</span><span class="sxs-lookup"><span data-stu-id="44efa-135">For the **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="44efa-136">Klicka på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="44efa-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="44efa-137">Kopiera ditt **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="44efa-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="44efa-138">(Du behöver det här värdet i koden senare.)</span><span class="sxs-lookup"><span data-stu-id="44efa-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="44efa-139">I den **nycklar** väljer **1 års** från den **Markera varaktighet** listrutan.</span><span class="sxs-lookup"><span data-stu-id="44efa-139">In the **keys** section, select **1 year** from the  **Select duration** drop-down list.</span></span> <span data-ttu-id="44efa-140">(Du kommer att kopiera nyckeln när du har sparat i steg 13.)</span><span class="sxs-lookup"><span data-stu-id="44efa-140">(You will copy the key after you save in step 13.)</span></span>
9. <span data-ttu-id="44efa-141">Bläddra nedåt och klicka **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="44efa-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="44efa-142">Lämna **visa** inställd på **Microsoft Apps** och välj **Microsoft Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="44efa-142">Leave **SHOW** set to **Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="44efa-143">Klicka på bockmarkeringen för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="44efa-143">Click the checkmark to continue.</span></span>
11. <span data-ttu-id="44efa-144">Välj **åtkomsthantering för Azure-tjänst...**  från den **delegerade behörigheter** listrutan.</span><span class="sxs-lookup"><span data-stu-id="44efa-144">Select **Access Azure Service Management...** from the **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="44efa-145">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="44efa-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="44efa-146">När det är klar att spara, kopiera nyckelvärdet i den **nycklar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="44efa-146">After the save finishes, copy the key value in the **keys** section.</span></span> <span data-ttu-id="44efa-147">(Du behöver det här värdet i koden senare.)</span><span class="sxs-lookup"><span data-stu-id="44efa-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-to-store-your-keys"></a><span data-ttu-id="44efa-148">Skapa ett nyckelvalv för att lagra dina nycklar</span><span class="sxs-lookup"><span data-stu-id="44efa-148">Create a key vault to store your keys</span></span>
<span data-ttu-id="44efa-149">Nu när du har klient-ID och din klientapp konfigureras är det dags att skapa en nyckelvalvet och konfigurera dess åtkomstprincip så att du och ditt program kan komma åt på valvet hemligheter (Always Encrypted nycklarna).</span><span class="sxs-lookup"><span data-stu-id="44efa-149">Now that your client app is configured and you have your client ID, it's time to create a key vault and configure its access policy so you and your application can access the vault's secrets (the Always Encrypted keys).</span></span> <span data-ttu-id="44efa-150">Den *skapa*, *hämta*, *lista*, *logga*, *Kontrollera*, *wrapKey*, och *unwrapKey* behörigheter som krävs för att skapa en ny kolumnhuvudnyckel och konfigurera kryptering med SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="44efa-150">The *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="44efa-151">Du kan snabbt skapa en nyckelvalv genom att köra följande skript.</span><span class="sxs-lookup"><span data-stu-id="44efa-151">You can quickly create a key vault by running the following script.</span></span> <span data-ttu-id="44efa-152">En detaljerad förklaring av dessa cmdletar och mer information om hur du skapar och konfigurerar ett nyckelvalv finns [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="44efa-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="44efa-153">Skapa en tom SQL-databas</span><span class="sxs-lookup"><span data-stu-id="44efa-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="44efa-154">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="44efa-154">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="44efa-155">Gå till **nya** > **Data + lagring** > **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="44efa-155">Go to **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="44efa-156">Skapa en **tom** databas med namnet **kurs** på en ny eller befintlig server.</span><span class="sxs-lookup"><span data-stu-id="44efa-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="44efa-157">Detaljerade anvisningar om hur du skapar en databas i Azure portal finns [första Azure SQL database](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="44efa-157">For detailed directions about how to create a database in the Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Skapa en tom databas](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="44efa-159">Du behöver anslutningen sträng senare under kursen, så när du har skapat databasen, bläddra till den nya e-kurs databasen och kopiera anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="44efa-159">You will need the connection string later in the tutorial, so after you create the database, browse to the new  Clinic database and copy the connection string.</span></span> <span data-ttu-id="44efa-160">Du kan hämta anslutningssträngen när som helst, men det är lätt att kopiera den i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44efa-160">You can get the connection string at any time, but it's easy to copy it in the Azure portal.</span></span>

1. <span data-ttu-id="44efa-161">Gå till **SQL-databaser** > **kurs** > **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="44efa-161">Go to **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="44efa-162">Kopiera anslutningssträngen för **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="44efa-162">Copy the connection string for **ADO.NET**.</span></span>
   
    ![Kopiera anslutningssträngen](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a><span data-ttu-id="44efa-164">Ansluta till databasen med SSMS</span><span class="sxs-lookup"><span data-stu-id="44efa-164">Connect to the database with SSMS</span></span>
<span data-ttu-id="44efa-165">Öppna SSMS och ansluta till servern med kurs-databasen.</span><span class="sxs-lookup"><span data-stu-id="44efa-165">Open SSMS and connect to the server with the Clinic database.</span></span>

1. <span data-ttu-id="44efa-166">Öppna SSMS.</span><span class="sxs-lookup"><span data-stu-id="44efa-166">Open SSMS.</span></span> <span data-ttu-id="44efa-167">(Gå till **Anslut** > **databasmotorn** att öppna den **Anslut till Server** om den inte är öppen.)</span><span class="sxs-lookup"><span data-stu-id="44efa-167">(Go to **Connect** > **Database Engine** to open the **Connect to Server** window if it isn't open.)</span></span>
2. <span data-ttu-id="44efa-168">Ange servernamn och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="44efa-168">Enter your server name and credentials.</span></span> <span data-ttu-id="44efa-169">Namnet på servern kan hittas på bladet SQL-databasen och i anslutningssträngen du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="44efa-169">The server name can be found on the SQL database blade and in the connection string you copied earlier.</span></span> <span data-ttu-id="44efa-170">Fullständiga servernamnet, inklusive *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="44efa-170">Type the complete server name, including *database.windows.net*.</span></span>
   
    ![Kopiera anslutningssträngen](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="44efa-172">Om den **ny brandväggsregel** öppnas, logga in på Azure och låter SSMS skapa en ny brandväggsregel för dig.</span><span class="sxs-lookup"><span data-stu-id="44efa-172">If the **New Firewall Rule** window opens, sign in to Azure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="44efa-173">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="44efa-173">Create a table</span></span>
<span data-ttu-id="44efa-174">I det här avsnittet skapar du en tabell för att lagra Patientdata.</span><span class="sxs-lookup"><span data-stu-id="44efa-174">In this section, you will create a table to hold patient data.</span></span> <span data-ttu-id="44efa-175">Det är inte ursprungligen krypterade--du vill konfigurera kryptering i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="44efa-175">It's not initially encrypted--you will configure encryption in the next section.</span></span>

1. <span data-ttu-id="44efa-176">Expandera **databaser**.</span><span class="sxs-lookup"><span data-stu-id="44efa-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="44efa-177">Högerklicka på den **kurs** databasen och klicka på **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="44efa-177">Right-click the **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="44efa-178">Klistra in följande Transact-SQL (T-SQL) i nytt frågefönster och **Execute** den.</span><span class="sxs-lookup"><span data-stu-id="44efa-178">Paste the following Transact-SQL (T-SQL) into the new query window and **Execute** it.</span></span>

        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="44efa-179">Kryptera kolumner (Konfigurera Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="44efa-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="44efa-180">SSMS innehåller en guide som hjälper dig att enkelt konfigurera Always Encrypted genom att ställa in kolumnhuvudnyckeln, kolumnkrypteringsnyckeln och krypterade kolumner för dig.</span><span class="sxs-lookup"><span data-stu-id="44efa-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up the column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="44efa-181">Expandera **databaser** > **kurs** > **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="44efa-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="44efa-182">Högerklicka på den **patienter** tabell och välj **kryptera kolumner** att öppna guiden Always Encrypted:</span><span class="sxs-lookup"><span data-stu-id="44efa-182">Right-click the **Patients** table and select **Encrypt Columns** to open the Always Encrypted wizard:</span></span>
   
    ![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="44efa-184">Guiden Always Encrypted innehåller följande avsnitt: **kolumnen**, **huvudnyckeln Configuration**, **validering**, och **sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="44efa-184">The Always Encrypted wizard includes the following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="44efa-185">Kolumnen</span><span class="sxs-lookup"><span data-stu-id="44efa-185">Column Selection</span></span>
<span data-ttu-id="44efa-186">Klicka på **nästa** på den **introduktion** kan du öppna den **kolumnen** sidan.</span><span class="sxs-lookup"><span data-stu-id="44efa-186">Click **Next** on the **Introduction** page to open the **Column Selection** page.</span></span> <span data-ttu-id="44efa-187">På den här sidan väljer du vilka kolumner som du vill kryptera, [typen av kryptering, och vilka kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) ska användas.</span><span class="sxs-lookup"><span data-stu-id="44efa-187">On this page, you will select which columns you want to encrypt, [the type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) to use.</span></span>

<span data-ttu-id="44efa-188">Kryptera **SSN** och **födelsedatum** information för varje tålamod.</span><span class="sxs-lookup"><span data-stu-id="44efa-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="44efa-189">Kolumnen SSN använder deterministiska kryptering, som stöder likheten sökningar, kopplingar och gruppera efter.</span><span class="sxs-lookup"><span data-stu-id="44efa-189">The SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="44efa-190">Kolumnen födelsedatum använder slumpmässig kryptering, som inte stöder åtgärder.</span><span class="sxs-lookup"><span data-stu-id="44efa-190">The BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="44efa-191">Ange den **krypteringstyp** för SSN kolumnen som **Deterministic** och födelsedatum kolumnen till **Randomized**.</span><span class="sxs-lookup"><span data-stu-id="44efa-191">Set the **Encryption Type** for the SSN column to **Deterministic** and the BirthDate column to **Randomized**.</span></span> <span data-ttu-id="44efa-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="44efa-192">Click **Next**.</span></span>

![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="44efa-194">Huvudnyckeln konfiguration</span><span class="sxs-lookup"><span data-stu-id="44efa-194">Master Key Configuration</span></span>
<span data-ttu-id="44efa-195">Den **huvudnyckeln Configuration** är där du ställer in din CMK och välj KeyStore-providern där CMK ska lagras.</span><span class="sxs-lookup"><span data-stu-id="44efa-195">The **Master Key Configuration** page is where you set up your CMK and select the key store provider where the CMK will be stored.</span></span> <span data-ttu-id="44efa-196">För närvarande kan du lagra en CMK i Windows certifikatarkiv, Azure Key Vault eller en maskinvarusäkerhetsmodul (HSM).</span><span class="sxs-lookup"><span data-stu-id="44efa-196">Currently, you can store a CMK in the Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="44efa-197">Den här kursen visar hur du lagrar dina nycklar i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="44efa-197">This tutorial shows how to store your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="44efa-198">Välj **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="44efa-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="44efa-199">Välj önskad nyckelvalvet från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="44efa-199">Select the desired key vault from the drop-down list.</span></span>
3. <span data-ttu-id="44efa-200">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="44efa-200">Click **Next**.</span></span>

![Huvudnyckeln konfiguration](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="44efa-202">Validering</span><span class="sxs-lookup"><span data-stu-id="44efa-202">Validation</span></span>
<span data-ttu-id="44efa-203">Du kan kryptera kolumnerna nu eller sparar ett PowerShell-skript och köra den senare.</span><span class="sxs-lookup"><span data-stu-id="44efa-203">You can encrypt the columns now or save a PowerShell script to run later.</span></span> <span data-ttu-id="44efa-204">Den här kursen väljer **fortsätta att avsluta nu** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="44efa-204">For this tutorial, select **Proceed to finish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="44efa-205">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="44efa-205">Summary</span></span>
<span data-ttu-id="44efa-206">Kontrollera att inställningarna är korrekta och klicka på **Slutför** att slutföra installationen för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="44efa-206">Verify that the settings are all correct and click **Finish** to complete the setup for Always Encrypted.</span></span>

![Sammanfattning](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a><span data-ttu-id="44efa-208">Kontrollera i guiden åtgärder</span><span class="sxs-lookup"><span data-stu-id="44efa-208">Verify the wizard's actions</span></span>
<span data-ttu-id="44efa-209">När guiden har slutförts kan har databasen konfigurerats för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="44efa-209">After the wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="44efa-210">Guiden utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="44efa-210">The wizard performed the following actions:</span></span>

* <span data-ttu-id="44efa-211">Skapa en huvudnyckel i kolumnen och lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="44efa-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="44efa-212">Skapa en kolumnkrypteringsnyckel och lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="44efa-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="44efa-213">Konfigurera de markerade kolumnerna för kryptering.</span><span class="sxs-lookup"><span data-stu-id="44efa-213">Configured the selected columns for encryption.</span></span> <span data-ttu-id="44efa-214">Tabellen patienter har för närvarande inga data, men alla befintliga data i de markerade kolumnerna är krypterad.</span><span class="sxs-lookup"><span data-stu-id="44efa-214">The Patients table currently has no data, but any existing data in the selected columns is now encrypted.</span></span>

<span data-ttu-id="44efa-215">Du kan verifiera att skapa nycklar i SSMS genom att expandera **kurs** > **säkerhet** > **alltid krypterade nycklar**.</span><span class="sxs-lookup"><span data-stu-id="44efa-215">You can verify the creation of the keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a><span data-ttu-id="44efa-216">Skapa ett klientprogram som fungerar med krypterade data</span><span class="sxs-lookup"><span data-stu-id="44efa-216">Create a client application that works with the encrypted data</span></span>
<span data-ttu-id="44efa-217">Nu när Always Encrypted har konfigurerats, kan du skapa ett program som utför *infogar* och *väljer* för krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="44efa-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on the encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="44efa-218">Programmet måste använda [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekt när skickas i klartext data till servern med Always Encrypted kolumner.</span><span class="sxs-lookup"><span data-stu-id="44efa-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data to the server with Always Encrypted columns.</span></span> <span data-ttu-id="44efa-219">Skicka litterala värden utan att använda SqlParameter objekt resulterar i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="44efa-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="44efa-220">Öppna Visual Studio och skapa en ny C# **konsolprogram** (Visual Studio 2015 och tidigare) eller **Konsolapp (.NET Framework)** (Visual Studio 2017 och senare).</span><span class="sxs-lookup"><span data-stu-id="44efa-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="44efa-221">Kontrollera att projektet har angetts till **.NET Framework 4.6** eller senare.</span><span class="sxs-lookup"><span data-stu-id="44efa-221">Make sure your project is set to **.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="44efa-222">Namnge projektet **AlwaysEncryptedConsoleAKVApp** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="44efa-222">Name the project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="44efa-223">Installera följande NuGet-paket genom att gå till **verktyg** > **NuGet Package Manager** > **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="44efa-223">Install the following NuGet packages by going to **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="44efa-224">Kör dessa två rader med kod i Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="44efa-224">Run these two lines of code in the Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a><span data-ttu-id="44efa-225">Ändra anslutningssträngen för att aktivera Always Encrypted</span><span class="sxs-lookup"><span data-stu-id="44efa-225">Modify your connection string to enable Always Encrypted</span></span>
<span data-ttu-id="44efa-226">Det här avsnittet beskrivs hur du aktiverar Always Encrypted i anslutningssträngen för databasen.</span><span class="sxs-lookup"><span data-stu-id="44efa-226">This section  explains how to enable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="44efa-227">Om du vill aktivera Always Encrypted, måste du lägga till den **Kolumnkrypteringsinställningen** nyckelord i anslutningen sträng och värdet **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="44efa-227">To enable Always Encrypted, you need to add the **Column Encryption Setting** keyword to your connection string and set it to **Enabled**.</span></span>

<span data-ttu-id="44efa-228">Du kan ange detta direkt i anslutningssträngen eller kan du ändra det med hjälp av [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-228">You can set this directly in the connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="44efa-229">Exempelprogrammet i nästa avsnitt visar hur du använder **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="44efa-229">The sample application in the next section shows how to use **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-the-connection-string"></a><span data-ttu-id="44efa-230">Aktivera Always Encrypted i anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="44efa-230">Enable Always Encrypted in the connection string</span></span>
<span data-ttu-id="44efa-231">Lägg till följande nyckelord i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="44efa-231">Add the following keyword to your connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="44efa-232">Aktivera Always Encrypted med SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="44efa-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="44efa-233">Följande kod visar hur du aktiverar Always Encrypted genom att ange [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) till [aktiverad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-233">The following code shows how to enable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) to [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a><span data-ttu-id="44efa-234">Registrera Azure Key Vault-providern</span><span class="sxs-lookup"><span data-stu-id="44efa-234">Register the Azure Key Vault provider</span></span>
<span data-ttu-id="44efa-235">Följande kod visar hur du registrerar Azure Key Vault-providern med ADO.NET-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="44efa-235">The following code shows how to register the Azure Key Vault provider with the ADO.NET driver.</span></span>

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="44efa-236">Alltid krypterat exempelkonsol-program</span><span class="sxs-lookup"><span data-stu-id="44efa-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="44efa-237">Det här exemplet visar hur du:</span><span class="sxs-lookup"><span data-stu-id="44efa-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="44efa-238">Ändra anslutningssträngen för att aktivera Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="44efa-238">Modify your connection string to enable Always Encrypted.</span></span>
* <span data-ttu-id="44efa-239">Registrera Azure Key Vault som programmets KeyStore-providern.</span><span class="sxs-lookup"><span data-stu-id="44efa-239">Register Azure Key Vault as the application's key store provider.</span></span>  
* <span data-ttu-id="44efa-240">Infoga data i de krypterade kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="44efa-240">Insert data into the encrypted columns.</span></span>
* <span data-ttu-id="44efa-241">Markera en post genom att filtrera efter ett visst värde i en krypterad kolumn.</span><span class="sxs-lookup"><span data-stu-id="44efa-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="44efa-242">Ersätt innehållet i **Program.cs** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="44efa-242">Replace the contents of **Program.cs** with the following code.</span></span> <span data-ttu-id="44efa-243">Ersätt anslutningssträngen för globala connectionString variabeln i rad som föregår direkt Main-metod med en giltig anslutningssträng från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44efa-243">Replace the connection string for the global connectionString variable in the line that directly precedes the Main method with your valid connection string from the Azure portal.</span></span> <span data-ttu-id="44efa-244">Det här är den enda förändringen som du behöver göra i den här koden.</span><span class="sxs-lookup"><span data-stu-id="44efa-244">This is the only change you need to make to this code.</span></span>

<span data-ttu-id="44efa-245">Kör appen att se Always Encrypted fungerar.</span><span class="sxs-lookup"><span data-stu-id="44efa-245">Run the app to see Always Encrypted in action.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a><span data-ttu-id="44efa-246">Kontrollera att informationen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="44efa-246">Verify that the data is encrypted</span></span>
<span data-ttu-id="44efa-247">Du kan snabbt kontrollera att den faktiska data på servern krypteras med datafrågor patienter med SSMS (med hjälp av den aktuella anslutningen där **Kolumnkrypteringsinställningen** har inte aktiverats).</span><span class="sxs-lookup"><span data-stu-id="44efa-247">You can quickly check that the actual data on the server is encrypted by querying the Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="44efa-248">Kör följande fråga på kurs-databasen.</span><span class="sxs-lookup"><span data-stu-id="44efa-248">Run the following query on the Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="44efa-249">Du kan se att krypterade kolumner inte innehåller några data i klartext.</span><span class="sxs-lookup"><span data-stu-id="44efa-249">You can see that the encrypted columns do not contain any plaintext data.</span></span>

   ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="44efa-251">Om du vill använda SSMS för att komma åt data i klartext, kan du lägga till den *Kolumnkrypteringsinställningen = aktiverat* parameter för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="44efa-251">To use SSMS to access the plaintext data, you can add the *Column Encryption Setting=enabled* parameter to the connection.</span></span>

1. <span data-ttu-id="44efa-252">Högerklicka på din server i i SSMS, **Object Explorer** och välj **frånkoppling**.</span><span class="sxs-lookup"><span data-stu-id="44efa-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="44efa-253">Klicka på **Anslut** > **databasmotorn** att öppna den **Anslut till Server** och klicka på **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="44efa-253">Click **Connect** > **Database Engine** to open the **Connect to Server** window and click **Options**.</span></span>
3. <span data-ttu-id="44efa-254">Klicka på **ytterligare anslutningsparametrar** och skriv **Kolumnkrypteringsinställningen = aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="44efa-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="44efa-256">Kör följande fråga på kurs-databasen.</span><span class="sxs-lookup"><span data-stu-id="44efa-256">Run the following query on the Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="44efa-257">Du kan nu se data i klartext i krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="44efa-257">You can now see the plaintext data in the encrypted columns.</span></span>

    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="44efa-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44efa-259">Next steps</span></span>
<span data-ttu-id="44efa-260">När du har skapat en databas som använder Always Encrypted kanske du vill göra följande:</span><span class="sxs-lookup"><span data-stu-id="44efa-260">After you create a database that uses Always Encrypted, you may want to do the following:</span></span>

* <span data-ttu-id="44efa-261">[Rotera och rensa dina nycklar](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="44efa-262">[Migrera data som redan har krypterats med Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="44efa-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="44efa-263">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="44efa-263">Related information</span></span>
* [<span data-ttu-id="44efa-264">Always Encrypted (client-utveckling)</span><span class="sxs-lookup"><span data-stu-id="44efa-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="44efa-265">Transparent datakryptering</span><span class="sxs-lookup"><span data-stu-id="44efa-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="44efa-266">SQL Server-kryptering</span><span class="sxs-lookup"><span data-stu-id="44efa-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="44efa-267">Alltid krypterat guiden</span><span class="sxs-lookup"><span data-stu-id="44efa-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="44efa-268">Alltid krypterade blogg</span><span class="sxs-lookup"><span data-stu-id="44efa-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

