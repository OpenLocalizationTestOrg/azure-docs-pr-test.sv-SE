---
title: 'Always Encrypted: Azure SQL Database - Windows certifikatarkiv | Microsoft Docs'
description: "Den här artikeln visar hur toosecure känsliga data i en SQL-databas med databaskryptering med hjälp av hello alltid krypteras guiden i SQL Server Management Studio (SSMS). Där visas också hur toostore krypteringsnycklarna i hello Windows certifikatet lagras."
keywords: "kryptera data, sql-kryptering, databaskryptering, känsliga data krypteras alltid"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: ce7e052e-8bf6-4d7c-9204-4c6f4afeba4b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: sstein
ms.openlocfilehash: 483f9a2120cc42b732142fc07807d3d8830a0c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a><span data-ttu-id="bd359-105">Always Encrypted: Skydda känsliga data i SQL-databasen och lagra krypteringsnycklar i hello Windows certifikatarkiv</span><span class="sxs-lookup"><span data-stu-id="bd359-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in hello Windows certificate store</span></span>

<span data-ttu-id="bd359-106">Den här artikeln visar hur toosecure känsliga data i en SQL-databas med databaskryptering med hjälp av hello [krypteras alltid guiden](https://msdn.microsoft.com/library/mt459280.aspx) i [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-106">This article shows you how toosecure sensitive data in a SQL database with database encryption by using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="bd359-107">Där visas också hur toostore krypteringsnycklarna i hello Windows certifikatet lagras.</span><span class="sxs-lookup"><span data-stu-id="bd359-107">It also shows you how toostore your encryption keys in hello Windows certificate store.</span></span>

<span data-ttu-id="bd359-108">Krypterat är alltid en teknik för kryptering av nya data i Azure SQL Database och SQL Server som hjälper till att skydda känsliga data i vila på hello-servern under flytt mellan klienten och servern och medan hello data att säkerställa att känsliga data aldrig visas som klartext i hello databassystem.</span><span class="sxs-lookup"><span data-stu-id="bd359-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use, ensuring that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="bd359-109">När du krypterar data endast klientprogram eller appservrar som har toohello snabbtangenter kan komma åt data i klartext.</span><span class="sxs-lookup"><span data-stu-id="bd359-109">After you encrypt data, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="bd359-110">Detaljerad information finns i [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-110">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="bd359-111">När du har konfigurerat hello databasen toouse Always Encrypted, skapar du ett klientprogram i C# med Visual Studio toowork med hello krypterade data.</span><span class="sxs-lookup"><span data-stu-id="bd359-111">After configuring hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="bd359-112">Följ hello stegen i den här artikeln toolearn hur tooset in Always Encrypted för en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="bd359-112">Follow hello steps in this article toolearn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="bd359-113">I den här artikeln får du lära dig hur tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="bd359-113">In this article, you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="bd359-114">Använd hello Always Encrypted guiden i SSMS toocreate [alltid krypterade nycklar](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="bd359-114">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted Keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="bd359-115">Skapa en [kolumnen Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-115">Create a [Column Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="bd359-116">Skapa en [Kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-116">Create a [Column Encryption Key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="bd359-117">Skapa en databastabell och kryptera kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-117">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="bd359-118">Skapa ett program som infogar, väljer och visar data från hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-118">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd359-119">Krav</span><span class="sxs-lookup"><span data-stu-id="bd359-119">Prerequisites</span></span>
<span data-ttu-id="bd359-120">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="bd359-120">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="bd359-121">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd359-121">An Azure account and subscription.</span></span> <span data-ttu-id="bd359-122">Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd359-122">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="bd359-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bd359-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="bd359-124">[.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) eller senare (på klientdatorn hello).</span><span class="sxs-lookup"><span data-stu-id="bd359-124">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="bd359-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>

## <a name="create-a-blank-sql-database"></a><span data-ttu-id="bd359-126">Skapa en tom SQL-databas</span><span class="sxs-lookup"><span data-stu-id="bd359-126">Create a blank SQL database</span></span>
1. <span data-ttu-id="bd359-127">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bd359-127">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bd359-128">Klicka på **nya** > **Data + lagring** > **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="bd359-128">Click **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="bd359-129">Skapa en **tom** databas med namnet **kurs** på en ny eller befintlig server.</span><span class="sxs-lookup"><span data-stu-id="bd359-129">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="bd359-130">Detaljerade instruktioner om hur du skapar en databas i hello Azure-portalen finns [första Azure SQL database](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bd359-130">For detailed instructions about creating a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Skapa en tom databas](./media/sql-database-always-encrypted/create-database.png)

<span data-ttu-id="bd359-132">Du måste anslutningssträngen hello senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="bd359-132">You will need hello connection string later in hello tutorial.</span></span> <span data-ttu-id="bd359-133">När hello-databasen har skapats går toohello nya kurs databasen och kopiera hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="bd359-133">After hello database is created, go toohello new Clinic database and copy hello connection string.</span></span> <span data-ttu-id="bd359-134">Du kan hämta hello anslutningssträngen när som helst, men det är enkelt toocopy den när du är i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd359-134">You can get hello connection string at any time, but it's easy toocopy it when you're in hello Azure portal.</span></span>

1. <span data-ttu-id="bd359-135">Klicka på **SQL-databaser** > **kurs** > **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="bd359-135">Click **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="bd359-136">Kopiera hello anslutningssträng för **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="bd359-136">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="bd359-138">Ansluta toohello databas med SSMS</span><span class="sxs-lookup"><span data-stu-id="bd359-138">Connect toohello database with SSMS</span></span>
<span data-ttu-id="bd359-139">Öppna SSMS och Anslut toohello server med hello kurs databas.</span><span class="sxs-lookup"><span data-stu-id="bd359-139">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="bd359-140">Öppna SSMS.</span><span class="sxs-lookup"><span data-stu-id="bd359-140">Open SSMS.</span></span> <span data-ttu-id="bd359-141">(Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** om den inte är öppen).</span><span class="sxs-lookup"><span data-stu-id="bd359-141">(Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it is not open).</span></span>
2. <span data-ttu-id="bd359-142">Ange servernamn och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bd359-142">Enter your server name and credentials.</span></span> <span data-ttu-id="bd359-143">hello servernamn kan hittas på bladet för hello SQL-databasen och i anslutningssträngen för hello du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bd359-143">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="bd359-144">Typen hello fullständig server namn inklusive *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="bd359-144">Type hello complete server name including *database.windows.net*.</span></span>
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted/ssms-connect.png)

<span data-ttu-id="bd359-146">Om hello **ny brandväggsregel** öppnas, logga in tooAzure och låter SSMS skapa en ny brandväggsregel för dig.</span><span class="sxs-lookup"><span data-stu-id="bd359-146">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="bd359-147">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="bd359-147">Create a table</span></span>
<span data-ttu-id="bd359-148">I det här avsnittet skapar du en toohold patient tabelldata.</span><span class="sxs-lookup"><span data-stu-id="bd359-148">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="bd359-149">Det här är en vanlig tabell ursprungligen--du vill konfigurera kryptering i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd359-149">This will be a normal table initially--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="bd359-150">Expandera **databaser**.</span><span class="sxs-lookup"><span data-stu-id="bd359-150">Expand **Databases**.</span></span>
2. <span data-ttu-id="bd359-151">Högerklicka på hello **kurs** databasen och klicka på **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="bd359-151">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="bd359-152">Klistra in hello följande Transact-SQL (T-SQL) i hello nytt frågefönster och **Execute** den.</span><span class="sxs-lookup"><span data-stu-id="bd359-152">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="bd359-153">Kryptera kolumner (Konfigurera Always Encrypted)</span><span class="sxs-lookup"><span data-stu-id="bd359-153">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="bd359-154">SSMS innehåller en guide tooeasily konfigurera Always Encrypted genom att ställa in hello CMK CEK och krypterade kolumner för dig.</span><span class="sxs-lookup"><span data-stu-id="bd359-154">SSMS provides a wizard tooeasily configure Always Encrypted by setting up hello CMK, CEK, and encrypted columns for you.</span></span>

1. <span data-ttu-id="bd359-155">Expandera **databaser** > **kurs** > **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="bd359-155">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="bd359-156">Högerklicka på hello **patienter** tabell och välj **kryptera kolumner** tooopen hello Always Encrypted guiden:</span><span class="sxs-lookup"><span data-stu-id="bd359-156">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Kryptera kolumner](./media/sql-database-always-encrypted/encrypt-columns.png)

<span data-ttu-id="bd359-158">hello Always Encrypted guiden innehåller följande avsnitt hello: **kolumnen**, **huvudnyckeln Configuration** (CMK) **validering**, och  **Sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="bd359-158">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration** (CMK), **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="bd359-159">Kolumnen</span><span class="sxs-lookup"><span data-stu-id="bd359-159">Column Selection</span></span>
<span data-ttu-id="bd359-160">Klicka på **nästa** på hello **introduktion** sidan tooopen hello **kolumnen** sidan.</span><span class="sxs-lookup"><span data-stu-id="bd359-160">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="bd359-161">På den här sidan väljer du vilka kolumner du vill tooencrypt, [hello krypteringstyp, och vilka kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="bd359-161">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="bd359-162">Kryptera **SSN** och **födelsedatum** information för varje tålamod.</span><span class="sxs-lookup"><span data-stu-id="bd359-162">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="bd359-163">Hej **SSN** kolumn använder deterministiska kryptering, som stöder likheten sökningar, kopplingar och gruppera efter.</span><span class="sxs-lookup"><span data-stu-id="bd359-163">hello **SSN** column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="bd359-164">Hej **födelsedatum** kolumn använder slumpmässig kryptering, som inte stöder åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bd359-164">hello **BirthDate** column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="bd359-165">Ange hello **krypteringstyp** för hello **SSN** kolumn för**Deterministic** och hello **födelsedatum** kolumn för **Slumpmässig**.</span><span class="sxs-lookup"><span data-stu-id="bd359-165">Set hello **Encryption Type** for hello **SSN** column too**Deterministic** and hello **BirthDate** column too**Randomized**.</span></span> <span data-ttu-id="bd359-166">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bd359-166">Click **Next**.</span></span>

![Kryptera kolumner](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="bd359-168">Huvudnyckeln konfiguration</span><span class="sxs-lookup"><span data-stu-id="bd359-168">Master Key Configuration</span></span>
<span data-ttu-id="bd359-169">Hej **huvudnyckeln Configuration** är där du har skapat din CMK och välj hello KeyStore-providern där hello CMK ska lagras.</span><span class="sxs-lookup"><span data-stu-id="bd359-169">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="bd359-170">För närvarande kan du lagra en CMK i certifikatarkivet för hello Windows Azure Key Vault eller en maskinvarusäkerhetsmodul (HSM).</span><span class="sxs-lookup"><span data-stu-id="bd359-170">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span> <span data-ttu-id="bd359-171">Den här kursen visar hur toostore dina nycklar i certifikat med Windows hello lagrar.</span><span class="sxs-lookup"><span data-stu-id="bd359-171">This tutorial shows how toostore your keys in hello Windows certificate store.</span></span>

<span data-ttu-id="bd359-172">Kontrollera att **Windows certifikatarkiv** är markerad och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bd359-172">Verify that **Windows certificate store** is selected and click **Next**.</span></span>

![Huvudnyckeln konfiguration](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="bd359-174">Validering</span><span class="sxs-lookup"><span data-stu-id="bd359-174">Validation</span></span>
<span data-ttu-id="bd359-175">Du kan kryptera hello kolumner nu eller spara en PowerShell-skriptet toorun senare.</span><span class="sxs-lookup"><span data-stu-id="bd359-175">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="bd359-176">Den här kursen väljer **fortsätta toofinish nu** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bd359-176">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="bd359-177">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bd359-177">Summary</span></span>
<span data-ttu-id="bd359-178">Kontrollera att hello-inställningarna är korrekta och klickar på **Slutför** toocomplete hello installationsprogrammet för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="bd359-178">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Sammanfattning](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="bd359-180">Kontrollera hello guiden åtgärder</span><span class="sxs-lookup"><span data-stu-id="bd359-180">Verify hello wizard's actions</span></span>
<span data-ttu-id="bd359-181">När hello-guiden har slutförts kan har databasen konfigurerats för Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="bd359-181">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="bd359-182">hello hello på guiden utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="bd359-182">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="bd359-183">Skapa en CMK.</span><span class="sxs-lookup"><span data-stu-id="bd359-183">Created a CMK.</span></span>
* <span data-ttu-id="bd359-184">Skapa en CEK.</span><span class="sxs-lookup"><span data-stu-id="bd359-184">Created a CEK.</span></span>
* <span data-ttu-id="bd359-185">Konfigurerade hello markerade kolumner för kryptering.</span><span class="sxs-lookup"><span data-stu-id="bd359-185">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="bd359-186">Din **patienter** tabellen har för närvarande inga data, men alla befintliga data i hello valda kolumner är krypterad.</span><span class="sxs-lookup"><span data-stu-id="bd359-186">Your **Patients** table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="bd359-187">Du kan verifiera hello skapandet av hello nycklar i SSMS genom att gå för**kurs** > **säkerhet** > **alltid krypterade nycklar**.</span><span class="sxs-lookup"><span data-stu-id="bd359-187">You can verify hello creation of hello keys in SSMS by going too**Clinic** > **Security** > **Always Encrypted Keys**.</span></span> <span data-ttu-id="bd359-188">Du kan nu se hello nya nycklar som hello guiden skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="bd359-188">You can now see hello new keys that hello wizard generated for you.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="bd359-189">Skapa ett klientprogram som fungerar med hello krypterade data</span><span class="sxs-lookup"><span data-stu-id="bd359-189">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="bd359-190">Nu när Always Encrypted har konfigurerats, kan du skapa ett program som utför *infogar* och *väljer* på hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-190">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span> <span data-ttu-id="bd359-191">toosuccessfully köra hello exempelprogrammet, måste du köra det på hello samma dator där du körde hello Always Encrypted guiden.</span><span class="sxs-lookup"><span data-stu-id="bd359-191">toosuccessfully run hello sample application, you must run it on hello same computer where you ran hello Always Encrypted wizard.</span></span> <span data-ttu-id="bd359-192">toorun hello program på en annan dator, måste du distribuera Always Encrypted certifikat toohello datorn som kör hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="bd359-192">toorun hello application on another computer, you must deploy your Always Encrypted certificates toohello computer running hello client app.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="bd359-193">Programmet måste använda [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekt när skickas i klartext data toohello server med Always Encrypted kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-193">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="bd359-194">Skicka litterala värden utan att använda SqlParameter objekt resulterar i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="bd359-194">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="bd359-195">Öppna Visual Studio och skapa ett nytt C#-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="bd359-195">Open Visual Studio and create a new C# console application.</span></span> <span data-ttu-id="bd359-196">Kontrollera att projektet är inställd för**.NET Framework 4.6** eller senare.</span><span class="sxs-lookup"><span data-stu-id="bd359-196">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="bd359-197">Namnet hello projektet **AlwaysEncryptedConsoleApp** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd359-197">Name hello project **AlwaysEncryptedConsoleApp** and click **OK**.</span></span>

![Nytt konsolprogram](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="bd359-199">Ändra din anslutning sträng tooenable Always Encrypted</span><span class="sxs-lookup"><span data-stu-id="bd359-199">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="bd359-200">Det här avsnittet beskrivs hur tooenable alltid krypterade i anslutningssträngen för databasen.</span><span class="sxs-lookup"><span data-stu-id="bd359-200">This section explains how tooenable Always Encrypted in your database connection string.</span></span> <span data-ttu-id="bd359-201">Du ändrar hello konsolprogram som du skapade i hello nästa avsnitt, ”Always Encrypted exempelkonsol-program”.</span><span class="sxs-lookup"><span data-stu-id="bd359-201">You will modify hello console app you just created in hello next section, "Always Encrypted sample console application."</span></span>

<span data-ttu-id="bd359-202">tooenable Always Encrypted, behöver du tooadd hello **Kolumnkrypteringsinställningen** nyckelordet tooyour anslutning sträng och ställa in också**aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="bd359-202">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="bd359-203">Du kan ange detta direkt i hello anslutningssträngen eller kan du ändra det med hjälp av en [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-203">You can set this directly in hello connection string, or you can set it by using a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="bd359-204">hello exempelprogrammet i hello nästa avsnitt visar hur toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="bd359-204">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

> [!NOTE]
> <span data-ttu-id="bd359-205">Detta är hello endast ändras i en klient programmet specifika tooAlways krypterad.</span><span class="sxs-lookup"><span data-stu-id="bd359-205">This is hello only change required in a client application specific tooAlways Encrypted.</span></span> <span data-ttu-id="bd359-206">Om du har ett befintligt program som lagrar sin anslutningssträng externt (det vill säga i en config-fil), kanske du kan tooenable Always Encrypted utan att ändra någon kod.</span><span class="sxs-lookup"><span data-stu-id="bd359-206">If you have an existing application that stores its connection string externally (that is, in a config file), you might be able tooenable Always Encrypted without changing any code.</span></span>
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="bd359-207">Aktivera Always Encrypted i hello anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="bd359-207">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="bd359-208">Lägg till hello efter nyckelordet tooyour anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="bd359-208">Add hello following keyword tooyour connection string:</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a><span data-ttu-id="bd359-209">Aktivera alltid krypteras med en SqlConnectionStringBuilder</span><span class="sxs-lookup"><span data-stu-id="bd359-209">Enable Always Encrypted with a SqlConnectionStringBuilder</span></span>
<span data-ttu-id="bd359-210">hello följande kod visar hur tooenable Always Encrypted genom att ange hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) för[aktiverad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-210">hello following code shows how tooenable Always Encrypted by setting hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="bd359-211">Alltid krypterat exempelkonsol-program</span><span class="sxs-lookup"><span data-stu-id="bd359-211">Always Encrypted sample console application</span></span>
<span data-ttu-id="bd359-212">Det här exemplet visar hur du:</span><span class="sxs-lookup"><span data-stu-id="bd359-212">This sample demonstrates how to:</span></span>

* <span data-ttu-id="bd359-213">Ändra din anslutning sträng tooenable Always Encrypted.</span><span class="sxs-lookup"><span data-stu-id="bd359-213">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="bd359-214">Infoga data i hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-214">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="bd359-215">Markera en post genom att filtrera efter ett visst värde i en krypterad kolumn.</span><span class="sxs-lookup"><span data-stu-id="bd359-215">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="bd359-216">Ersätt hello innehållet i **Program.cs** med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="bd359-216">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="bd359-217">Ersätt hello anslutningssträngen för hello globala connectionString variabel i hello rad direkt ovanför hello Main-metoden med en giltig anslutningssträng från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd359-217">Replace hello connection string for hello global connectionString variable in hello line directly above hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="bd359-218">Detta är endast förändring i hello måste toomake toothis kod.</span><span class="sxs-lookup"><span data-stu-id="bd359-218">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="bd359-219">Kör hello app toosee Always Encrypted i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="bd359-219">Run hello app toosee Always Encrypted in action.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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


## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="bd359-220">Kontrollera att hello data krypteras</span><span class="sxs-lookup"><span data-stu-id="bd359-220">Verify that hello data is encrypted</span></span>
<span data-ttu-id="bd359-221">Du kan snabbt kontrollera att hello faktiska data på hello servern krypteras genom att fråga hello **patienter** data med SSMS.</span><span class="sxs-lookup"><span data-stu-id="bd359-221">You can quickly check that hello actual data on hello server is encrypted by querying hello **Patients** data with SSMS.</span></span> <span data-ttu-id="bd359-222">(Använda den aktuella anslutningen där hello kolumnkrypteringsinställningen ännu inte har aktiverats.)</span><span class="sxs-lookup"><span data-stu-id="bd359-222">(Use your current connection where hello column encryption setting is not yet enabled.)</span></span>

<span data-ttu-id="bd359-223">Kör följande fråga på hello kurs databasen hello.</span><span class="sxs-lookup"><span data-stu-id="bd359-223">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="bd359-224">Du kan se att hello krypterade kolumner inte innehåller några data i klartext.</span><span class="sxs-lookup"><span data-stu-id="bd359-224">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-encrypted.png)

<span data-ttu-id="bd359-226">toouse SSMS tooaccess Hej klartext data, kan du lägga till hello **Kolumnkrypteringsinställningen = aktiverat** parametern toohello anslutning.</span><span class="sxs-lookup"><span data-stu-id="bd359-226">toouse SSMS tooaccess hello plaintext data, you can add hello **Column Encryption Setting=enabled** parameter toohello connection.</span></span>

1. <span data-ttu-id="bd359-227">Högerklicka på din server i i SSMS, **Object Explorer**, och klicka sedan på **frånkoppling**.</span><span class="sxs-lookup"><span data-stu-id="bd359-227">In SSMS, right-click your server in **Object Explorer**, and then click **Disconnect**.</span></span>
2. <span data-ttu-id="bd359-228">Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** fönstret och klicka sedan på **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="bd359-228">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window, and then click **Options**.</span></span>
3. <span data-ttu-id="bd359-229">Klicka på **ytterligare anslutningsparametrar** och skriv **Kolumnkrypteringsinställningen = aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="bd359-229">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. <span data-ttu-id="bd359-231">Kör hello följande fråga på hello **kurs** databas.</span><span class="sxs-lookup"><span data-stu-id="bd359-231">Run hello following query on hello **Clinic** database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="bd359-232">Du kan nu se hello klartext data i hello krypterade kolumner.</span><span class="sxs-lookup"><span data-stu-id="bd359-232">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> <span data-ttu-id="bd359-234">Om du ansluter med SSMS (eller klienter) från en annan dator har inte åtkomst toohello krypteringsnycklar och kommer inte att kunna toodecrypt hello data.</span><span class="sxs-lookup"><span data-stu-id="bd359-234">If you connect with SSMS (or any client) from a different computer, it will not have access toohello encryption keys and will not be able toodecrypt hello data.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="bd359-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd359-235">Next steps</span></span>
<span data-ttu-id="bd359-236">När du har skapat en databas som använder Always Encrypted behöva toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="bd359-236">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="bd359-237">Kör det här exemplet från en annan dator.</span><span class="sxs-lookup"><span data-stu-id="bd359-237">Run this sample from a different computer.</span></span> <span data-ttu-id="bd359-238">Den inte åtkomst till krypteringsnycklarna för toohello, så att den inte har åtkomst till toohello klartext data och ska inte köras.</span><span class="sxs-lookup"><span data-stu-id="bd359-238">It won't have access toohello encryption keys, so it will not have access toohello plaintext data and will not run successfully.</span></span>
* <span data-ttu-id="bd359-239">[Rotera och rensa dina nycklar](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-239">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="bd359-240">[Migrera data som redan har krypterats med Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd359-240">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>
* <span data-ttu-id="bd359-241">[Distribuera Always Encrypted klientdatorer för certifikat tooother](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (se hello ”att göra certifikat tillgängliga tooApplications och användare”).</span><span class="sxs-lookup"><span data-stu-id="bd359-241">[Deploy Always Encrypted certificates tooother client machines](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (see hello "Making Certificates Available tooApplications and Users" section).</span></span>

## <a name="related-information"></a><span data-ttu-id="bd359-242">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="bd359-242">Related information</span></span>
* [<span data-ttu-id="bd359-243">Always Encrypted (client-utveckling)</span><span class="sxs-lookup"><span data-stu-id="bd359-243">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="bd359-244">Transparent datakryptering</span><span class="sxs-lookup"><span data-stu-id="bd359-244">Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="bd359-245">SQL Server-kryptering</span><span class="sxs-lookup"><span data-stu-id="bd359-245">SQL Server Encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="bd359-246">Alltid krypterade guiden</span><span class="sxs-lookup"><span data-stu-id="bd359-246">Always Encrypted Wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="bd359-247">Alltid krypterade blogg</span><span class="sxs-lookup"><span data-stu-id="bd359-247">Always Encrypted Blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

