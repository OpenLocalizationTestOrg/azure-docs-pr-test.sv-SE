---
title: 'Always Encrypted: SQL Database - Azure Key Vault | Microsoft Docs'
description: "Den här artikeln visar hur toosecure känsliga data i en SQL-databas med data kryptering hello alltid krypteras guiden i SQL Server Management Studio. Den innehåller också anvisningar som visar hur toostore varje krypteringsnyckeln i Azure Key Vault."
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
ms.openlocfilehash: 8226bfef584e979643f5bb0747d4df16569f8204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="64710-105">Always Encrypted: Skydda känsliga data i SQL-databasen och lagra krypteringsnycklar i Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="64710-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="64710-106">Den här artikeln visar hur toosecure känsliga data i en SQL-databas med kryptering av data med hjälp av hello [krypteras alltid guiden](https://msdn.microsoft.com/library/mt459280.aspx) i [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-106">This article shows you how toosecure sensitive data in a SQL database with data encryption using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="64710-107">Den innehåller också anvisningar som visar hur toostore varje krypteringsnyckeln i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64710-107">It also includes instructions that will show you how toostore each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="64710-108">Alltid krypterat är en teknik för kryptering av nya data i Azure SQL Database och SQL Server som hjälper till att skydda känsliga data i vila på hello-servern under flytt mellan klienten och servern och medan hello data används.</span><span class="sxs-lookup"><span data-stu-id="64710-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use.</span></span> <span data-ttu-id="64710-109">Krypterad garanterar alltid att känsliga data aldrig visas i klartext i hello databassystem.</span><span class="sxs-lookup"><span data-stu-id="64710-109">Always Encrypted ensures that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="64710-110">När du har konfigurerat datakryptering endast klientprogram eller appservrar som har toohello snabbtangenter kan komma åt data i klartext.</span><span class="sxs-lookup"><span data-stu-id="64710-110">After you configure data encryption, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="64710-111">Detaljerad information finns i [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="64710-112">När du har konfigurerat hello databasen toouse Always Encrypted skapar du ett klientprogram i C# med Visual Studio toowork med hello krypterade data.</span><span class="sxs-lookup"><span data-stu-id="64710-112">After you configure hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="64710-113">Följ hello stegen i den här artikeln och lära dig hur tooset in Always Encrypted för en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="64710-113">Follow hello steps in this article and learn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="64710-114">I den här artikeln du lära dig hur tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="64710-114">In this article you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="64710-115">Använd hello Always Encrypted guiden i SSMS toocreate [Always Encrypted nycklar](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="64710-115">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="64710-116">Skapa en [kolumnhuvudnyckeln (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="64710-117">Skapa en [kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="64710-118">Skapa en databastabell och kryptera kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="64710-119">Skapa ett program som infogar, väljer och visar data från hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-119">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64710-120">Krav</span><span class="sxs-lookup"><span data-stu-id="64710-120">Prerequisites</span></span>
<span data-ttu-id="64710-121">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="64710-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="64710-122">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="64710-122">An Azure account and subscription.</span></span> <span data-ttu-id="64710-123">Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64710-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="64710-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 eller senare.</span><span class="sxs-lookup"><span data-stu-id="64710-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="64710-125">[.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) eller senare (på klientdatorn hello).</span><span class="sxs-lookup"><span data-stu-id="64710-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="64710-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="64710-127">[Azure PowerShell](/powershell/azure/overview), version 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="64710-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="64710-128">Typen **(Get-Module azure - ListAvailable). Version** toosee vilken version av PowerShell som du kör.</span><span class="sxs-lookup"><span data-stu-id="64710-128">Type **(Get-Module azure -ListAvailable).Version** toosee what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a><span data-ttu-id="64710-129">Aktivera klienten programmet tooaccess hello SQL-databas</span><span class="sxs-lookup"><span data-stu-id="64710-129">Enable your client application tooaccess hello SQL Database service</span></span>
<span data-ttu-id="64710-130">Du måste aktivera din klient programmet tooaccess hello SQL Database-tjänsten genom att ställa in hello krävs autentisering och förvärv hello *ClientId* och *hemlighet* att du måste tooauthenticate ditt program i hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="64710-130">You must enable your client application tooaccess hello SQL Database service by setting up hello required authentication and acquiring hello *ClientId* and *Secret* that you will need tooauthenticate your application in hello following code.</span></span>

1. <span data-ttu-id="64710-131">Öppna hello [klassiska Azure-portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="64710-131">Open hello [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="64710-132">Välj **Active Directory** och klicka på hello Active Directory-instans som programmet ska använda.</span><span class="sxs-lookup"><span data-stu-id="64710-132">Select **Active Directory** and click hello Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="64710-133">Klicka på **program**, och klicka sedan på **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="64710-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="64710-134">Skriv ett namn för ditt program (till exempel: *myClientApp*), Välj **WEBBPROGRAM**, och klicka på hello pilen toocontinue.</span><span class="sxs-lookup"><span data-stu-id="64710-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click hello arrow toocontinue.</span></span>
5. <span data-ttu-id="64710-135">För hello **SIGN-ON-URL** och **APP-ID URI** du kan ange en giltig URL (t.ex, *http://myClientApp*) och fortsätta.</span><span class="sxs-lookup"><span data-stu-id="64710-135">For hello **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="64710-136">Klicka på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="64710-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="64710-137">Kopiera ditt **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="64710-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="64710-138">(Du behöver det här värdet i koden senare.)</span><span class="sxs-lookup"><span data-stu-id="64710-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="64710-139">I hello **nycklar** väljer **1 års** från hello **Markera varaktighet** listrutan.</span><span class="sxs-lookup"><span data-stu-id="64710-139">In hello **keys** section, select **1 year** from hello  **Select duration** drop-down list.</span></span> <span data-ttu-id="64710-140">(Du kommer att kopiera hello nyckel när du har sparat i steg 13.)</span><span class="sxs-lookup"><span data-stu-id="64710-140">(You will copy hello key after you save in step 13.)</span></span>
9. <span data-ttu-id="64710-141">Bläddra nedåt och klicka **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="64710-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="64710-142">Lämna **visa** ställa in också**Microsoft Apps** och välj **Microsoft Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="64710-142">Leave **SHOW** set too**Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="64710-143">Klicka på hello markering toocontinue.</span><span class="sxs-lookup"><span data-stu-id="64710-143">Click hello checkmark toocontinue.</span></span>
11. <span data-ttu-id="64710-144">Välj **åtkomsthantering för Azure-tjänst...**  från hello **delegerade behörigheter** listrutan.</span><span class="sxs-lookup"><span data-stu-id="64710-144">Select **Access Azure Service Management...** from hello **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="64710-145">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="64710-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="64710-146">Efter hello spara har avslutats, kopiera hello nyckelvärdet i hello **nycklar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="64710-146">After hello save finishes, copy hello key value in hello **keys** section.</span></span> <span data-ttu-id="64710-147">(Du behöver det här värdet i koden senare.)</span><span class="sxs-lookup"><span data-stu-id="64710-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-toostore-your-keys"></a><span data-ttu-id="64710-148">Skapa ett nyckelvalv toostore dina nycklar</span><span class="sxs-lookup"><span data-stu-id="64710-148">Create a key vault toostore your keys</span></span>
<span data-ttu-id="64710-149">Nu när du har klient-ID och din klientapp konfigureras är tid toocreate ett nyckelvalv och konfigurera dess åtkomstprincip så att du och ditt program kan komma åt hello valvet hemligheter (hello Always Encrypted nycklar).</span><span class="sxs-lookup"><span data-stu-id="64710-149">Now that your client app is configured and you have your client ID, it's time toocreate a key vault and configure its access policy so you and your application can access hello vault's secrets (hello Always Encrypted keys).</span></span> <span data-ttu-id="64710-150">Hej *skapa*, *hämta*, *lista*, *logga*, *Kontrollera*, *wrapKey*, och *unwrapKey* behörigheter som krävs för att skapa en ny kolumnhuvudnyckel och konfigurera kryptering med SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="64710-150">hello *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="64710-151">Du kan snabbt skapa en nyckelvalv genom att köra hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="64710-151">You can quickly create a key vault by running hello following script.</span></span> <span data-ttu-id="64710-152">En detaljerad förklaring av dessa cmdletar och mer information om hur du skapar och konfigurerar ett nyckelvalv finns [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64710-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

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




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="64710-153">Skapa en tom SQL-databas</span><span class="sxs-lookup"><span data-stu-id="64710-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="64710-154">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="64710-154">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="64710-155">Gå för**ny** > **Data + lagring** > **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="64710-155">Go too**New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="64710-156">Skapa en **tom** databas med namnet **kurs** på en ny eller befintlig server.</span><span class="sxs-lookup"><span data-stu-id="64710-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="64710-157">För detaljerade instruktioner om hur du toocreate en databas i hello Azure-portalen finns [första Azure SQL database](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="64710-157">For detailed directions about how toocreate a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Skapa en tom databas](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="64710-159">Du behöver hello anslutningssträngen senare i självstudiekursen hello, så när du har skapat databasen hello Bläddra toohello nya kurs databasen och kopiera hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="64710-159">You will need hello connection string later in hello tutorial, so after you create hello database, browse toohello new  Clinic database and copy hello connection string.</span></span> <span data-ttu-id="64710-160">Du kan hämta hello anslutningssträngen när som helst, men det är enkelt toocopy i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="64710-160">You can get hello connection string at any time, but it's easy toocopy it in hello Azure portal.</span></span>

1. <span data-ttu-id="64710-161">Gå för**SQL-databaser** > **kurs** > **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="64710-161">Go too**SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="64710-162">Kopiera hello anslutningssträng för **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="64710-162">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="64710-164">Ansluta toohello databas med SSMS</span><span class="sxs-lookup"><span data-stu-id="64710-164">Connect toohello database with SSMS</span></span>
<span data-ttu-id="64710-165">Öppna SSMS och Anslut toohello server med hello kurs databas.</span><span class="sxs-lookup"><span data-stu-id="64710-165">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="64710-166">Öppna SSMS.</span><span class="sxs-lookup"><span data-stu-id="64710-166">Open SSMS.</span></span> <span data-ttu-id="64710-167">(Gå för**Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** om den inte är öppen.)</span><span class="sxs-lookup"><span data-stu-id="64710-167">(Go too**Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it isn't open.)</span></span>
2. <span data-ttu-id="64710-168">Ange servernamn och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="64710-168">Enter your server name and credentials.</span></span> <span data-ttu-id="64710-169">hello servernamn kan hittas på bladet för hello SQL-databasen och i anslutningssträngen för hello du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="64710-169">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="64710-170">Typen hello fullständiga servernamnet, inklusive *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="64710-170">Type hello complete server name, including *database.windows.net*.</span></span>
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="64710-172">Om hello **ny brandväggsregel** öppnas, logga in tooAzure och låter SSMS skapa en ny brandväggsregel för dig.</span><span class="sxs-lookup"><span data-stu-id="64710-172">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="64710-173">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="64710-173">Create a table</span></span>
<span data-ttu-id="64710-174">I det här avsnittet skapar du en toohold patient tabelldata.</span><span class="sxs-lookup"><span data-stu-id="64710-174">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="64710-175">Det är inte ursprungligen krypterade--du vill konfigurera kryptering i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="64710-175">It's not initially encrypted--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="64710-176">Expandera **databaser**.</span><span class="sxs-lookup"><span data-stu-id="64710-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="64710-177">Högerklicka på hello **kurs** databasen och klicka på **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="64710-177">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="64710-178">Klistra in hello följande Transact-SQL (T-SQL) i hello nytt frågefönster och **Execute** den.</span><span class="sxs-lookup"><span data-stu-id="64710-178">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="64710-179">Kryptera kolumner (Konfigurera Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="64710-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="64710-180">SSMS innehåller en guide som hjälper dig att enkelt konfigurera Always Encrypted genom att ställa in hello kolumnhuvudnyckeln kolumnkrypteringsnyckeln och krypterade kolumner för dig.</span><span class="sxs-lookup"><span data-stu-id="64710-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up hello column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="64710-181">Expandera **databaser** > **kurs** > **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="64710-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="64710-182">Högerklicka på hello **patienter** tabell och välj **kryptera kolumner** tooopen hello Always Encrypted guiden:</span><span class="sxs-lookup"><span data-stu-id="64710-182">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="64710-184">hello Always Encrypted guiden innehåller följande avsnitt hello: **kolumnen**, **huvudnyckeln Configuration**, **validering**, och **sammanfattning** .</span><span class="sxs-lookup"><span data-stu-id="64710-184">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="64710-185">Kolumnen</span><span class="sxs-lookup"><span data-stu-id="64710-185">Column Selection</span></span>
<span data-ttu-id="64710-186">Klicka på **nästa** på hello **introduktion** sidan tooopen hello **kolumnen** sidan.</span><span class="sxs-lookup"><span data-stu-id="64710-186">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="64710-187">På den här sidan väljer du vilka kolumner du vill tooencrypt, [hello krypteringstyp, och vilka kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="64710-187">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="64710-188">Kryptera **SSN** och **födelsedatum** information för varje tålamod.</span><span class="sxs-lookup"><span data-stu-id="64710-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="64710-189">hello SSN kolumn använder deterministiska kryptering, som stöder likheten sökningar, kopplingar och gruppera efter.</span><span class="sxs-lookup"><span data-stu-id="64710-189">hello SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="64710-190">hello födelsedatumet kolumn använder slumpmässig kryptering, som inte stöder åtgärder.</span><span class="sxs-lookup"><span data-stu-id="64710-190">hello BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="64710-191">Ange hello **krypteringstyp** för hello SSN kolumnen för**Deterministic** och hello födelsedatumet kolumnen för**Randomized**.</span><span class="sxs-lookup"><span data-stu-id="64710-191">Set hello **Encryption Type** for hello SSN column too**Deterministic** and hello BirthDate column too**Randomized**.</span></span> <span data-ttu-id="64710-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="64710-192">Click **Next**.</span></span>

![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="64710-194">Huvudnyckeln konfiguration</span><span class="sxs-lookup"><span data-stu-id="64710-194">Master Key Configuration</span></span>
<span data-ttu-id="64710-195">Hej **huvudnyckeln Configuration** är där du har skapat din CMK och välj hello KeyStore-providern där hello CMK ska lagras.</span><span class="sxs-lookup"><span data-stu-id="64710-195">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="64710-196">För närvarande kan du lagra en CMK i certifikatarkivet för hello Windows Azure Key Vault eller en maskinvarusäkerhetsmodul (HSM).</span><span class="sxs-lookup"><span data-stu-id="64710-196">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="64710-197">Den här kursen visar hur toostore dina nycklar i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64710-197">This tutorial shows how toostore your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="64710-198">Välj **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="64710-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="64710-199">Välj hello önskade nyckelvalv hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="64710-199">Select hello desired key vault from hello drop-down list.</span></span>
3. <span data-ttu-id="64710-200">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="64710-200">Click **Next**.</span></span>

![Huvudnyckeln konfiguration](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="64710-202">Validering</span><span class="sxs-lookup"><span data-stu-id="64710-202">Validation</span></span>
<span data-ttu-id="64710-203">Du kan kryptera hello kolumner nu eller spara en PowerShell-skriptet toorun senare.</span><span class="sxs-lookup"><span data-stu-id="64710-203">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="64710-204">Den här kursen väljer **fortsätta toofinish nu** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="64710-204">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="64710-205">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="64710-205">Summary</span></span>
<span data-ttu-id="64710-206">Kontrollera att hello-inställningarna är korrekta och klickar på **Slutför** toocomplete hello installationsprogrammet för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="64710-206">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Sammanfattning](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="64710-208">Kontrollera hello guiden åtgärder</span><span class="sxs-lookup"><span data-stu-id="64710-208">Verify hello wizard's actions</span></span>
<span data-ttu-id="64710-209">När hello-guiden har slutförts kan har databasen konfigurerats för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="64710-209">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="64710-210">hello hello på guiden utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="64710-210">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="64710-211">Skapa en huvudnyckel i kolumnen och lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64710-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="64710-212">Skapa en kolumnkrypteringsnyckel och lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64710-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="64710-213">Konfigurerade hello markerade kolumner för kryptering.</span><span class="sxs-lookup"><span data-stu-id="64710-213">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="64710-214">hello patienter tabellen har för närvarande inga data, men alla befintliga data i hello valda kolumner är krypterad.</span><span class="sxs-lookup"><span data-stu-id="64710-214">hello Patients table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="64710-215">Du kan verifiera hello skapandet av hello nycklar i SSMS genom att expandera **kurs** > **säkerhet** > **alltid krypterade nycklar**.</span><span class="sxs-lookup"><span data-stu-id="64710-215">You can verify hello creation of hello keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="64710-216">Skapa ett klientprogram som fungerar med hello krypterade data</span><span class="sxs-lookup"><span data-stu-id="64710-216">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="64710-217">Nu när Always Encrypted har konfigurerats, kan du skapa ett program som utför *infogar* och *väljer* på hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="64710-218">Programmet måste använda [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekt när skickas i klartext data toohello server med Always Encrypted kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="64710-219">Skicka litterala värden utan att använda SqlParameter objekt resulterar i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="64710-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="64710-220">Öppna Visual Studio och skapa en ny C# **konsolprogram** (Visual Studio 2015 och tidigare) eller **Konsolapp (.NET Framework)** (Visual Studio 2017 och senare).</span><span class="sxs-lookup"><span data-stu-id="64710-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="64710-221">Kontrollera att projektet är inställd för**.NET Framework 4.6** eller senare.</span><span class="sxs-lookup"><span data-stu-id="64710-221">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="64710-222">Namnet hello projektet **AlwaysEncryptedConsoleAKVApp** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="64710-222">Name hello project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="64710-223">Installera följande NuGet-paket genom att gå för hello**verktyg** > **NuGet Package Manager** > **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="64710-223">Install hello following NuGet packages by going too**Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="64710-224">Kör dessa två rader med kod i hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="64710-224">Run these two lines of code in hello Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="64710-225">Ändra din anslutning sträng tooenable Always Encrypted</span><span class="sxs-lookup"><span data-stu-id="64710-225">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="64710-226">Det här avsnittet beskrivs hur tooenable alltid krypterade i anslutningssträngen för databasen.</span><span class="sxs-lookup"><span data-stu-id="64710-226">This section  explains how tooenable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="64710-227">tooenable Always Encrypted, behöver du tooadd hello **Kolumnkrypteringsinställningen** nyckelordet tooyour anslutning sträng och ställa in också**aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="64710-227">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="64710-228">Du kan ange detta direkt i hello anslutningssträngen eller kan du ändra det med hjälp av [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-228">You can set this directly in hello connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="64710-229">hello exempelprogrammet i hello nästa avsnitt visar hur toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="64710-229">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="64710-230">Aktivera Always Encrypted i hello anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="64710-230">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="64710-231">Lägg till hello efter nyckelordet tooyour anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="64710-231">Add hello following keyword tooyour connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="64710-232">Aktivera Always Encrypted med SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="64710-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="64710-233">Hej följande kod visar hur tooenable Always Encrypted genom att ange [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) för[aktiverad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-233">hello following code shows how tooenable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a><span data-ttu-id="64710-234">Registrera hello Azure Key Vault-providern</span><span class="sxs-lookup"><span data-stu-id="64710-234">Register hello Azure Key Vault provider</span></span>
<span data-ttu-id="64710-235">hello följande kod visar hur tooregister hello Azure Key Vault-provider med hello ADO.NET-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="64710-235">hello following code shows how tooregister hello Azure Key Vault provider with hello ADO.NET driver.</span></span>

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



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="64710-236">Alltid krypterat exempelkonsol-program</span><span class="sxs-lookup"><span data-stu-id="64710-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="64710-237">Det här exemplet visar hur du:</span><span class="sxs-lookup"><span data-stu-id="64710-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="64710-238">Ändra din anslutning sträng tooenable Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="64710-238">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="64710-239">Registrera Azure Key Vault som hello programmets KeyStore-providern.</span><span class="sxs-lookup"><span data-stu-id="64710-239">Register Azure Key Vault as hello application's key store provider.</span></span>  
* <span data-ttu-id="64710-240">Infoga data i hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-240">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="64710-241">Markera en post genom att filtrera efter ett visst värde i en krypterad kolumn.</span><span class="sxs-lookup"><span data-stu-id="64710-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="64710-242">Ersätt hello innehållet i **Program.cs** med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="64710-242">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="64710-243">Ersätt hello anslutningssträngen för hello globala connectionString variabel i hello rad direkt före hello Main-metod med en giltig anslutningssträng från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="64710-243">Replace hello connection string for hello global connectionString variable in hello line that directly precedes hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="64710-244">Detta är endast förändring i hello måste toomake toothis kod.</span><span class="sxs-lookup"><span data-stu-id="64710-244">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="64710-245">Kör hello app toosee Always Encrypted i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="64710-245">Run hello app toosee Always Encrypted in action.</span></span>

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
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"<connection string from hello portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from hello Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for hello connection.
            // This is hello only change specific tooAlways Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update hello connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign hello updated connection string tooour global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records toorestart this demo app.
            ResetPatientsTable();

            // Add sample data toohello Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data toohello database...");

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
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // hello example allows duplicate SSN entries so we will return all records
            // that match hello provided value and store hello results in selectedPatients.
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

            Console.WriteLine("Press Enter tooexit...");
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
                throw new InvalidOperationException("Failed tooobtain hello access token");
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
                    Console.WriteLine("hello following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key tooexit");
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


        // This method simply deletes all records in hello Patients table tooreset our demo.
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



## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="64710-246">Kontrollera att hello data krypteras</span><span class="sxs-lookup"><span data-stu-id="64710-246">Verify that hello data is encrypted</span></span>
<span data-ttu-id="64710-247">Du kan snabbt kontrollera att hello faktiska data på hello servern krypteras med datafrågor hello patienter med SSMS (med hjälp av den aktuella anslutningen där **Kolumnkrypteringsinställningen** har inte aktiverats).</span><span class="sxs-lookup"><span data-stu-id="64710-247">You can quickly check that hello actual data on hello server is encrypted by querying hello Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="64710-248">Kör följande fråga på hello kurs databasen hello.</span><span class="sxs-lookup"><span data-stu-id="64710-248">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="64710-249">Du kan se att hello krypterade kolumner inte innehåller några data i klartext.</span><span class="sxs-lookup"><span data-stu-id="64710-249">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="64710-251">toouse SSMS tooaccess Hej klartext data, kan du lägga till hello *Kolumnkrypteringsinställningen = aktiverat* parametern toohello anslutning.</span><span class="sxs-lookup"><span data-stu-id="64710-251">toouse SSMS tooaccess hello plaintext data, you can add hello *Column Encryption Setting=enabled* parameter toohello connection.</span></span>

1. <span data-ttu-id="64710-252">Högerklicka på din server i i SSMS, **Object Explorer** och välj **frånkoppling**.</span><span class="sxs-lookup"><span data-stu-id="64710-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="64710-253">Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** och klicka på **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="64710-253">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window and click **Options**.</span></span>
3. <span data-ttu-id="64710-254">Klicka på **ytterligare anslutningsparametrar** och skriv **Kolumnkrypteringsinställningen = aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="64710-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="64710-256">Kör följande fråga på hello kurs databasen hello.</span><span class="sxs-lookup"><span data-stu-id="64710-256">Run hello following query on hello Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="64710-257">Du kan nu se hello klartext data i hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="64710-257">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="64710-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64710-259">Next steps</span></span>
<span data-ttu-id="64710-260">När du har skapat en databas som använder Always Encrypted behöva toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="64710-260">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="64710-261">[Rotera och rensa dina nycklar](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="64710-262">[Migrera data som redan har krypterats med Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="64710-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="64710-263">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="64710-263">Related information</span></span>
* [<span data-ttu-id="64710-264">Always Encrypted (client-utveckling)</span><span class="sxs-lookup"><span data-stu-id="64710-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="64710-265">Transparent datakryptering</span><span class="sxs-lookup"><span data-stu-id="64710-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="64710-266">SQL Server-kryptering</span><span class="sxs-lookup"><span data-stu-id="64710-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="64710-267">Alltid krypterat guiden</span><span class="sxs-lookup"><span data-stu-id="64710-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="64710-268">Alltid krypterade blogg</span><span class="sxs-lookup"><span data-stu-id="64710-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

