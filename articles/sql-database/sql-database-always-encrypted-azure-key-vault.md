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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Always Encrypted: Skydda känsliga data i SQL-databasen och lagra krypteringsnycklar i Azure Key Vault

Den här artikeln visar hur toosecure känsliga data i en SQL-databas med kryptering av data med hjälp av hello [krypteras alltid guiden](https://msdn.microsoft.com/library/mt459280.aspx) i [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Den innehåller också anvisningar som visar hur toostore varje krypteringsnyckeln i Azure Key Vault.

Alltid krypterat är en teknik för kryptering av nya data i Azure SQL Database och SQL Server som hjälper till att skydda känsliga data i vila på hello-servern under flytt mellan klienten och servern och medan hello data används. Krypterad garanterar alltid att känsliga data aldrig visas i klartext i hello databassystem. När du har konfigurerat datakryptering endast klientprogram eller appservrar som har toohello snabbtangenter kan komma åt data i klartext. Detaljerad information finns i [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

När du har konfigurerat hello databasen toouse Always Encrypted skapar du ett klientprogram i C# med Visual Studio toowork med hello krypterade data.

Följ hello stegen i den här artikeln och lära dig hur tooset in Always Encrypted för en Azure SQL database. I den här artikeln du lära dig hur tooperform hello följande uppgifter:

* Använd hello Always Encrypted guiden i SSMS toocreate [Always Encrypted nycklar](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Skapa en [kolumnhuvudnyckeln (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Skapa en [kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Skapa en databastabell och kryptera kolumner.
* Skapa ett program som infogar, väljer och visar data från hello krypterade kolumner.

## <a name="prerequisites"></a>Krav
Den här kursen behöver du:

* Ett Azure-konto och prenumeration. Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 eller senare.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) eller senare (på klientdatorn hello).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Azure PowerShell](/powershell/azure/overview), version 1.0 eller senare. Typen **(Get-Module azure - ListAvailable). Version** toosee vilken version av PowerShell som du kör.

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a>Aktivera klienten programmet tooaccess hello SQL-databas
Du måste aktivera din klient programmet tooaccess hello SQL Database-tjänsten genom att ställa in hello krävs autentisering och förvärv hello *ClientId* och *hemlighet* att du måste tooauthenticate ditt program i hello följande kod.

1. Öppna hello [klassiska Azure-portalen](http://manage.windowsazure.com).
2. Välj **Active Directory** och klicka på hello Active Directory-instans som programmet ska använda.
3. Klicka på **program**, och klicka sedan på **lägga till**.
4. Skriv ett namn för ditt program (till exempel: *myClientApp*), Välj **WEBBPROGRAM**, och klicka på hello pilen toocontinue.
5. För hello **SIGN-ON-URL** och **APP-ID URI** du kan ange en giltig URL (t.ex, *http://myClientApp*) och fortsätta.
6. Klicka på **konfigurera**.
7. Kopiera ditt **klient-ID**. (Du behöver det här värdet i koden senare.)
8. I hello **nycklar** väljer **1 års** från hello **Markera varaktighet** listrutan. (Du kommer att kopiera hello nyckel när du har sparat i steg 13.)
9. Bläddra nedåt och klicka **lägga till program**.
10. Lämna **visa** ställa in också**Microsoft Apps** och välj **Microsoft Azure Service Management API**. Klicka på hello markering toocontinue.
11. Välj **åtkomsthantering för Azure-tjänst...**  från hello **delegerade behörigheter** listrutan.
12. Klicka på **SPARA**.
13. Efter hello spara har avslutats, kopiera hello nyckelvärdet i hello **nycklar** avsnitt. (Du behöver det här värdet i koden senare.)

## <a name="create-a-key-vault-toostore-your-keys"></a>Skapa ett nyckelvalv toostore dina nycklar
Nu när du har klient-ID och din klientapp konfigureras är tid toocreate ett nyckelvalv och konfigurera dess åtkomstprincip så att du och ditt program kan komma åt hello valvet hemligheter (hello Always Encrypted nycklar). Hej *skapa*, *hämta*, *lista*, *logga*, *Kontrollera*, *wrapKey*, och *unwrapKey* behörigheter som krävs för att skapa en ny kolumnhuvudnyckel och konfigurera kryptering med SQL Server Management Studio.

Du kan snabbt skapa en nyckelvalv genom att köra hello följande skript. En detaljerad förklaring av dessa cmdletar och mer information om hur du skapar och konfigurerar ett nyckelvalv finns [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).

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




## <a name="create-a-blank-sql-database"></a>Skapa en tom SQL-databas
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Gå för**ny** > **Data + lagring** > **SQL-databas**.
3. Skapa en **tom** databas med namnet **kurs** på en ny eller befintlig server. För detaljerade instruktioner om hur du toocreate en databas i hello Azure-portalen finns [första Azure SQL database](sql-database-get-started-portal.md).
   
    ![Skapa en tom databas](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Du behöver hello anslutningssträngen senare i självstudiekursen hello, så när du har skapat databasen hello Bläddra toohello nya kurs databasen och kopiera hello anslutningssträngen. Du kan hämta hello anslutningssträngen när som helst, men det är enkelt toocopy i hello Azure-portalen.

1. Gå för**SQL-databaser** > **kurs** > **visa databasanslutningssträngar**.
2. Kopiera hello anslutningssträng för **ADO.NET**.
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Ansluta toohello databas med SSMS
Öppna SSMS och Anslut toohello server med hello kurs databas.

1. Öppna SSMS. (Gå för**Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** om den inte är öppen.)
2. Ange servernamn och autentiseringsuppgifter. hello servernamn kan hittas på bladet för hello SQL-databasen och i anslutningssträngen för hello du kopierade tidigare. Typen hello fullständiga servernamnet, inklusive *database.windows.net*.
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Om hello **ny brandväggsregel** öppnas, logga in tooAzure och låter SSMS skapa en ny brandväggsregel för dig.

## <a name="create-a-table"></a>Skapa en tabell
I det här avsnittet skapar du en toohold patient tabelldata. Det är inte ursprungligen krypterade--du vill konfigurera kryptering i hello nästa avsnitt.

1. Expandera **databaser**.
2. Högerklicka på hello **kurs** databasen och klicka på **ny fråga**.
3. Klistra in hello följande Transact-SQL (T-SQL) i hello nytt frågefönster och **Execute** den.

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


## <a name="encrypt-columns-configure-always-encrypted"></a>Kryptera kolumner (Konfigurera Always Encrypted)
SSMS innehåller en guide som hjälper dig att enkelt konfigurera Always Encrypted genom att ställa in hello kolumnhuvudnyckeln kolumnkrypteringsnyckeln och krypterade kolumner för dig.

1. Expandera **databaser** > **kurs** > **tabeller**.
2. Högerklicka på hello **patienter** tabell och välj **kryptera kolumner** tooopen hello Always Encrypted guiden:
   
    ![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

hello Always Encrypted guiden innehåller följande avsnitt hello: **kolumnen**, **huvudnyckeln Configuration**, **validering**, och **sammanfattning** .

### <a name="column-selection"></a>Kolumnen
Klicka på **nästa** på hello **introduktion** sidan tooopen hello **kolumnen** sidan. På den här sidan väljer du vilka kolumner du vill tooencrypt, [hello krypteringstyp, och vilka kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Kryptera **SSN** och **födelsedatum** information för varje tålamod. hello SSN kolumn använder deterministiska kryptering, som stöder likheten sökningar, kopplingar och gruppera efter. hello födelsedatumet kolumn använder slumpmässig kryptering, som inte stöder åtgärder.

Ange hello **krypteringstyp** för hello SSN kolumnen för**Deterministic** och hello födelsedatumet kolumnen för**Randomized**. Klicka på **Nästa**.

![Kryptera kolumner](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Huvudnyckeln konfiguration
Hej **huvudnyckeln Configuration** är där du har skapat din CMK och välj hello KeyStore-providern där hello CMK ska lagras. För närvarande kan du lagra en CMK i certifikatarkivet för hello Windows Azure Key Vault eller en maskinvarusäkerhetsmodul (HSM).

Den här kursen visar hur toostore dina nycklar i Azure Key Vault.

1. Välj **Azure Key Vault**.
2. Välj hello önskade nyckelvalv hello nedrullningsbara listan.
3. Klicka på **Nästa**.

![Huvudnyckeln konfiguration](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Validering
Du kan kryptera hello kolumner nu eller spara en PowerShell-skriptet toorun senare. Den här kursen väljer **fortsätta toofinish nu** och på **nästa**.

### <a name="summary"></a>Sammanfattning
Kontrollera att hello-inställningarna är korrekta och klickar på **Slutför** toocomplete hello installationsprogrammet för Always Encrypted.

![Sammanfattning](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a>Kontrollera hello guiden åtgärder
När hello-guiden har slutförts kan har databasen konfigurerats för Always Encrypted. hello hello på guiden utförs följande åtgärder:

* Skapa en huvudnyckel i kolumnen och lagras i Azure Key Vault.
* Skapa en kolumnkrypteringsnyckel och lagras i Azure Key Vault.
* Konfigurerade hello markerade kolumner för kryptering. hello patienter tabellen har för närvarande inga data, men alla befintliga data i hello valda kolumner är krypterad.

Du kan verifiera hello skapandet av hello nycklar i SSMS genom att expandera **kurs** > **säkerhet** > **alltid krypterade nycklar**.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Skapa ett klientprogram som fungerar med hello krypterade data
Nu när Always Encrypted har konfigurerats, kan du skapa ett program som utför *infogar* och *väljer* på hello krypterade kolumner.  

> [!IMPORTANT]
> Programmet måste använda [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekt när skickas i klartext data toohello server med Always Encrypted kolumner. Skicka litterala värden utan att använda SqlParameter objekt resulterar i ett undantag.
> 
> 

1. Öppna Visual Studio och skapa en ny C# **konsolprogram** (Visual Studio 2015 och tidigare) eller **Konsolapp (.NET Framework)** (Visual Studio 2017 och senare). Kontrollera att projektet är inställd för**.NET Framework 4.6** eller senare.
2. Namnet hello projektet **AlwaysEncryptedConsoleAKVApp** och på **OK**.
3. Installera följande NuGet-paket genom att gå för hello**verktyg** > **NuGet Package Manager** > **Pakethanterarkonsolen**.

Kör dessa två rader med kod i hello Package Manager-konsolen.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>Ändra din anslutning sträng tooenable Always Encrypted
Det här avsnittet beskrivs hur tooenable alltid krypterade i anslutningssträngen för databasen.

tooenable Always Encrypted, behöver du tooadd hello **Kolumnkrypteringsinställningen** nyckelordet tooyour anslutning sträng och ställa in också**aktiverad**.

Du kan ange detta direkt i hello anslutningssträngen eller kan du ändra det med hjälp av [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). hello exempelprogrammet i hello nästa avsnitt visar hur toouse **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Aktivera Always Encrypted i hello anslutningssträng
Lägg till hello efter nyckelordet tooyour anslutningssträngen.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Aktivera Always Encrypted med SqlConnectionStringBuilder
Hej följande kod visar hur tooenable Always Encrypted genom att ange [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) för[aktiverad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a>Registrera hello Azure Key Vault-providern
hello följande kod visar hur tooregister hello Azure Key Vault-provider med hello ADO.NET-drivrutinen.

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



## <a name="always-encrypted-sample-console-application"></a>Alltid krypterat exempelkonsol-program
Det här exemplet visar hur du:

* Ändra din anslutning sträng tooenable Always Encrypted.
* Registrera Azure Key Vault som hello programmets KeyStore-providern.  
* Infoga data i hello krypterade kolumner.
* Markera en post genom att filtrera efter ett visst värde i en krypterad kolumn.

Ersätt hello innehållet i **Program.cs** med hello följande kod. Ersätt hello anslutningssträngen för hello globala connectionString variabel i hello rad direkt före hello Main-metod med en giltig anslutningssträng från hello Azure-portalen. Detta är endast förändring i hello måste toomake toothis kod.

Kör hello app toosee Always Encrypted i åtgärden.

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



## <a name="verify-that-hello-data-is-encrypted"></a>Kontrollera att hello data krypteras
Du kan snabbt kontrollera att hello faktiska data på hello servern krypteras med datafrågor hello patienter med SSMS (med hjälp av den aktuella anslutningen där **Kolumnkrypteringsinställningen** har inte aktiverats).

Kör följande fråga på hello kurs databasen hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Du kan se att hello krypterade kolumner inte innehåller några data i klartext.

   ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

toouse SSMS tooaccess Hej klartext data, kan du lägga till hello *Kolumnkrypteringsinställningen = aktiverat* parametern toohello anslutning.

1. Högerklicka på din server i i SSMS, **Object Explorer** och välj **frånkoppling**.
2. Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** och klicka på **alternativ**.
3. Klicka på **ytterligare anslutningsparametrar** och skriv **Kolumnkrypteringsinställningen = aktiverat**.
   
    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Kör följande fråga på hello kurs databasen hello.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Du kan nu se hello klartext data i hello krypterade kolumner.

    ![Nytt konsolprogram](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Nästa steg
När du har skapat en databas som använder Always Encrypted behöva toodo hello följande:

* [Rotera och rensa dina nycklar](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migrera data som redan har krypterats med Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>Relaterad information
* [Always Encrypted (client-utveckling)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparent datakryptering](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-kryptering](https://msdn.microsoft.com/library/bb510663.aspx)
* [Alltid krypterat guiden](https://msdn.microsoft.com/library/mt459280.aspx)
* [Alltid krypterade blogg](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

