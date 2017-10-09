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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a>Always Encrypted: Skydda känsliga data i SQL-databasen och lagra krypteringsnycklar i hello Windows certifikatarkiv

Den här artikeln visar hur toosecure känsliga data i en SQL-databas med databaskryptering med hjälp av hello [krypteras alltid guiden](https://msdn.microsoft.com/library/mt459280.aspx) i [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Där visas också hur toostore krypteringsnycklarna i hello Windows certifikatet lagras.

Krypterat är alltid en teknik för kryptering av nya data i Azure SQL Database och SQL Server som hjälper till att skydda känsliga data i vila på hello-servern under flytt mellan klienten och servern och medan hello data att säkerställa att känsliga data aldrig visas som klartext i hello databassystem. När du krypterar data endast klientprogram eller appservrar som har toohello snabbtangenter kan komma åt data i klartext. Detaljerad information finns i [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

När du har konfigurerat hello databasen toouse Always Encrypted, skapar du ett klientprogram i C# med Visual Studio toowork med hello krypterade data.

Följ hello stegen i den här artikeln toolearn hur tooset in Always Encrypted för en Azure SQL database. I den här artikeln får du lära dig hur tooperform hello följande uppgifter:

* Använd hello Always Encrypted guiden i SSMS toocreate [alltid krypterade nycklar](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Skapa en [kolumnen Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Skapa en [Kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Skapa en databastabell och kryptera kolumner.
* Skapa ett program som infogar, väljer och visar data från hello krypterade kolumner.

## <a name="prerequisites"></a>Krav
Den här kursen behöver du:

* Ett Azure-konto och prenumeration. Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 eller senare.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) eller senare (på klientdatorn hello).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Skapa en tom SQL-databas
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **nya** > **Data + lagring** > **SQL-databas**.
3. Skapa en **tom** databas med namnet **kurs** på en ny eller befintlig server. Detaljerade instruktioner om hur du skapar en databas i hello Azure-portalen finns [första Azure SQL database](sql-database-get-started-portal.md).
   
    ![Skapa en tom databas](./media/sql-database-always-encrypted/create-database.png)

Du måste anslutningssträngen hello senare i självstudiekursen hello. När hello-databasen har skapats går toohello nya kurs databasen och kopiera hello anslutningssträngen. Du kan hämta hello anslutningssträngen när som helst, men det är enkelt toocopy den när du är i hello Azure-portalen.

1. Klicka på **SQL-databaser** > **kurs** > **visa databasanslutningssträngar**.
2. Kopiera hello anslutningssträng för **ADO.NET**.
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Ansluta toohello databas med SSMS
Öppna SSMS och Anslut toohello server med hello kurs databas.

1. Öppna SSMS. (Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** om den inte är öppen).
2. Ange servernamn och autentiseringsuppgifter. hello servernamn kan hittas på bladet för hello SQL-databasen och i anslutningssträngen för hello du kopierade tidigare. Typen hello fullständig server namn inklusive *database.windows.net*.
   
    ![Kopiera hello anslutningssträng](./media/sql-database-always-encrypted/ssms-connect.png)

Om hello **ny brandväggsregel** öppnas, logga in tooAzure och låter SSMS skapa en ny brandväggsregel för dig.

## <a name="create-a-table"></a>Skapa en tabell
I det här avsnittet skapar du en toohold patient tabelldata. Det här är en vanlig tabell ursprungligen--du vill konfigurera kryptering i hello nästa avsnitt.

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
SSMS innehåller en guide tooeasily konfigurera Always Encrypted genom att ställa in hello CMK CEK och krypterade kolumner för dig.

1. Expandera **databaser** > **kurs** > **tabeller**.
2. Högerklicka på hello **patienter** tabell och välj **kryptera kolumner** tooopen hello Always Encrypted guiden:
   
    ![Kryptera kolumner](./media/sql-database-always-encrypted/encrypt-columns.png)

hello Always Encrypted guiden innehåller följande avsnitt hello: **kolumnen**, **huvudnyckeln Configuration** (CMK) **validering**, och  **Sammanfattning**.

### <a name="column-selection"></a>Kolumnen
Klicka på **nästa** på hello **introduktion** sidan tooopen hello **kolumnen** sidan. På den här sidan väljer du vilka kolumner du vill tooencrypt, [hello krypteringstyp, och vilka kolumnkrypteringsnyckeln (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Kryptera **SSN** och **födelsedatum** information för varje tålamod. Hej **SSN** kolumn använder deterministiska kryptering, som stöder likheten sökningar, kopplingar och gruppera efter. Hej **födelsedatum** kolumn använder slumpmässig kryptering, som inte stöder åtgärder.

Ange hello **krypteringstyp** för hello **SSN** kolumn för**Deterministic** och hello **födelsedatum** kolumn för **Slumpmässig**. Klicka på **Nästa**.

![Kryptera kolumner](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Huvudnyckeln konfiguration
Hej **huvudnyckeln Configuration** är där du har skapat din CMK och välj hello KeyStore-providern där hello CMK ska lagras. För närvarande kan du lagra en CMK i certifikatarkivet för hello Windows Azure Key Vault eller en maskinvarusäkerhetsmodul (HSM). Den här kursen visar hur toostore dina nycklar i certifikat med Windows hello lagrar.

Kontrollera att **Windows certifikatarkiv** är markerad och klicka på **nästa**.

![Huvudnyckeln konfiguration](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Validering
Du kan kryptera hello kolumner nu eller spara en PowerShell-skriptet toorun senare. Den här kursen väljer **fortsätta toofinish nu** och på **nästa**.

### <a name="summary"></a>Sammanfattning
Kontrollera att hello-inställningarna är korrekta och klickar på **Slutför** toocomplete hello installationsprogrammet för Always Encrypted.

![Sammanfattning](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a>Kontrollera hello guiden åtgärder
När hello-guiden har slutförts kan har databasen konfigurerats för Always Encrypted. hello hello på guiden utförs följande åtgärder:

* Skapa en CMK.
* Skapa en CEK.
* Konfigurerade hello markerade kolumner för kryptering. Din **patienter** tabellen har för närvarande inga data, men alla befintliga data i hello valda kolumner är krypterad.

Du kan verifiera hello skapandet av hello nycklar i SSMS genom att gå för**kurs** > **säkerhet** > **alltid krypterade nycklar**. Du kan nu se hello nya nycklar som hello guiden skapas automatiskt.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Skapa ett klientprogram som fungerar med hello krypterade data
Nu när Always Encrypted har konfigurerats, kan du skapa ett program som utför *infogar* och *väljer* på hello krypterade kolumner. toosuccessfully köra hello exempelprogrammet, måste du köra det på hello samma dator där du körde hello Always Encrypted guiden. toorun hello program på en annan dator, måste du distribuera Always Encrypted certifikat toohello datorn som kör hello-klientappen.  

> [!IMPORTANT]
> Programmet måste använda [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekt när skickas i klartext data toohello server med Always Encrypted kolumner. Skicka litterala värden utan att använda SqlParameter objekt resulterar i ett undantag.
> 
> 

1. Öppna Visual Studio och skapa ett nytt C#-konsolprogram. Kontrollera att projektet är inställd för**.NET Framework 4.6** eller senare.
2. Namnet hello projektet **AlwaysEncryptedConsoleApp** och på **OK**.

![Nytt konsolprogram](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>Ändra din anslutning sträng tooenable Always Encrypted
Det här avsnittet beskrivs hur tooenable alltid krypterade i anslutningssträngen för databasen. Du ändrar hello konsolprogram som du skapade i hello nästa avsnitt, ”Always Encrypted exempelkonsol-program”.

tooenable Always Encrypted, behöver du tooadd hello **Kolumnkrypteringsinställningen** nyckelordet tooyour anslutning sträng och ställa in också**aktiverad**.

Du kan ange detta direkt i hello anslutningssträngen eller kan du ändra det med hjälp av en [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). hello exempelprogrammet i hello nästa avsnitt visar hur toouse **SqlConnectionStringBuilder**.

> [!NOTE]
> Detta är hello endast ändras i en klient programmet specifika tooAlways krypterad. Om du har ett befintligt program som lagrar sin anslutningssträng externt (det vill säga i en config-fil), kanske du kan tooenable Always Encrypted utan att ändra någon kod.
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Aktivera Always Encrypted i hello anslutningssträng
Lägg till hello efter nyckelordet tooyour anslutningssträngen:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Aktivera alltid krypteras med en SqlConnectionStringBuilder
hello följande kod visar hur tooenable Always Encrypted genom att ange hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) för[aktiverad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Alltid krypterat exempelkonsol-program
Det här exemplet visar hur du:

* Ändra din anslutning sträng tooenable Always Encrypted.
* Infoga data i hello krypterade kolumner.
* Markera en post genom att filtrera efter ett visst värde i en krypterad kolumn.

Ersätt hello innehållet i **Program.cs** med hello följande kod. Ersätt hello anslutningssträngen för hello globala connectionString variabel i hello rad direkt ovanför hello Main-metoden med en giltig anslutningssträng från hello Azure-portalen. Detta är endast förändring i hello måste toomake toothis kod.

Kör hello app toosee Always Encrypted i åtgärden.

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


## <a name="verify-that-hello-data-is-encrypted"></a>Kontrollera att hello data krypteras
Du kan snabbt kontrollera att hello faktiska data på hello servern krypteras genom att fråga hello **patienter** data med SSMS. (Använda den aktuella anslutningen där hello kolumnkrypteringsinställningen ännu inte har aktiverats.)

Kör följande fråga på hello kurs databasen hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Du kan se att hello krypterade kolumner inte innehåller några data i klartext.

   ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-encrypted.png)

toouse SSMS tooaccess Hej klartext data, kan du lägga till hello **Kolumnkrypteringsinställningen = aktiverat** parametern toohello anslutning.

1. Högerklicka på din server i i SSMS, **Object Explorer**, och klicka sedan på **frånkoppling**.
2. Klicka på **Anslut** > **databasmotorn** tooopen hello **ansluta tooServer** fönstret och klicka sedan på **alternativ**.
3. Klicka på **ytterligare anslutningsparametrar** och skriv **Kolumnkrypteringsinställningen = aktiverat**.
   
    ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Kör hello följande fråga på hello **kurs** databas.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Du kan nu se hello klartext data i hello krypterade kolumner.

    ![Nytt konsolprogram](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> Om du ansluter med SSMS (eller klienter) från en annan dator har inte åtkomst toohello krypteringsnycklar och kommer inte att kunna toodecrypt hello data.
> 
> 

## <a name="next-steps"></a>Nästa steg
När du har skapat en databas som använder Always Encrypted behöva toodo hello följande:

* Kör det här exemplet från en annan dator. Den inte åtkomst till krypteringsnycklarna för toohello, så att den inte har åtkomst till toohello klartext data och ska inte köras.
* [Rotera och rensa dina nycklar](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migrera data som redan har krypterats med Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).
* [Distribuera Always Encrypted klientdatorer för certifikat tooother](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (se hello ”att göra certifikat tillgängliga tooApplications och användare”).

## <a name="related-information"></a>Relaterad information
* [Always Encrypted (client-utveckling)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparent datakryptering](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-kryptering](https://msdn.microsoft.com/library/bb510663.aspx)
* [Alltid krypterade guiden](https://msdn.microsoft.com/library/mt459280.aspx)
* [Alltid krypterade blogg](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

