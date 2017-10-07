---
title: "aaaDesign din första Azure SQL-databas | Microsoft Docs"
description: "Lär dig toodesign din första Azure SQL-databas."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/03/2017
ms.author: carlrab
ms.openlocfilehash: 65f0a1594cbdda7480abf32a847266a073e7560d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-sql-database"></a>Utforma din första Azure SQL-databas

Azure SQL Database är en relationell databas-som-en tjänst (DBaaS) i hello Microsoft Cloud (”Azure”). I kursen får du lära dig hur toouse hello Azure-portalen och [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) till: 

> [!div class="checklist"]
> * Skapa en databas i hello Azure-portalen
> * Konfigurera en brandväggsregel på servernivå i hello Azure-portalen
> * Ansluta toohello databas med SSMS
> * Skapa tabeller med SSMS
> * Massinläsning av data med BCP
> * Fråga data med SSMS
> * Återställa hello databasen tooa tidigare [återställning vid tidpunkt](sql-database-recovery-using-backups.md#point-in-time-restore) i hello Azure-portalen

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

toocomplete den här självstudiekursen, se till att du har installerat:
- hello senaste versionen av [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- hello senaste versionen av [BCP och SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Skapa en tom SQL-databas

Azure SQL-databasen skapas med en definierad uppsättning [beräknings-och lagringsresurser](sql-database-service-tiers.md). hello-databas skapas inom en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) och i en [logisk Azure SQL Database-server](sql-database-features.md). 

Följ dessa steg toocreate en tom SQL-databas. 

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Välj **databaser** från hello **ny** och väljer **SQL-databas** från hello **databaser** sidan. 

   ![Skapa en tom databas](./media/sql-database-design-first-database/create-empty-database.png)

3. Fyll i formuläret om hello SQL-databas med hello följande information som visas i föregående bild hello:   

   | Inställning       | Föreslaget värde | Beskrivning | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Databasnamn** | mySampleDatabase | För giltiga databasnamn, se [databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Prenumeration** | Din prenumeration  | Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions). |
   | **Resursgrupp** | myResourceGroup | Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Välj källa** | Tom databas | Anger att en tom databas ska skapas. |

4. Klicka på **Server** toocreate och konfigurera en ny server för den nya databasen. Fyll i hello **nytt serverformulär** med hello följande information: 

   | Inställning       | Föreslaget värde | Beskrivning | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernamn** | Valfritt globalt unikt namn | Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Inloggning för serveradministratör** | Valfritt giltigt namn | För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Lösenord** | Valfritt giltigt lösenord | Lösenordet måste innehålla minst 8 tecken och måste innehålla tecken från tre av hello följande kategorier: versaler, gemener, siffror och specialtecken. |
   | **Plats** | Valfri giltig plats | För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/). |

   ![skapa databas-server](./media//sql-database-design-first-database/create-database-server.png)

5. Klicka på **Välj**.

6. Klicka på **prisnivå** toospecify hello tjänstnivå och prestandanivå servicenivå för den nya databasen. Den här kursen väljer **20 dtu: er** och **250** GB lagringsutrymme.

   ![skapa databas-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Klicka på **Använd**.  

8. Välj en **sorteringen** för hello tom databas (för den här kursen används hello standardvärdet). Mer information om sorteringar finns [sorteringar](https://docs.microsoft.com/sql/t-sql/statements/collations)

9. Klicka på **skapa** tooprovision hello-databasen. Etablering tar om en minut och en halv toocomplete. 

10. På verktygsfältet hello **meddelanden** toomonitor hello distributionsprocessen.

   ![avisering](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Skapa en brandväggsregel på servernivå

hello SQL Database-tjänsten skapar en brandvägg på hello-servernivå som förhindrar att externa program och verktyg ansluter toohello server eller en databas på servern hello såvida inte en brandväggsregel skapas tooopen hello-brandväggen för specifika IP-adresser. Följ dessa steg toocreate en [SQL-databas brandväggsregel på servernivå](sql-database-firewall-configure.md) för din klients IP-adress och aktivera extern anslutning via hello SQL Database-Brandvägg för din IP-adress. 

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg. I så fall, kan du inte ansluta tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.
>

1. När hello distributionen är klar klickar du på **SQL-databaser** från hello vänstra menyn och klicka sedan på **mySampleDatabase** på hello **SQL-databaser** sidan. hello översiktssidan för din databas öppnas som visar du hello fullständigt kvalificerade servernamnet (exempelvis **mynewserver20170313.database.windows.net**) och innehåller alternativ för ytterligare konfiguration. Kopiera det fullständiga servernamnet för senare användning.

   > [!IMPORTANT]
   > Du behöver den här fullständigt kvalificerade namnet tooconnect tooyour-server och dess databaser i efterföljande snabbstarter.
   > 

   ![servernamn](./media/sql-database-connect-query-dotnet/server-name.png) 

2. Klicka på **ange serverbrandvägg** hello verktygsfältet enligt hello föregående bild. Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server. 

   ![brandväggsregler för server](./media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd din aktuella IP-adressen tooa ny brandväggsregel. Med en brandväggsregel kan du öppna port 1433 för en enskild IP-adress eller för IP-adressintervall.

4. Klicka på **Spara**. En brandväggsregel på servernivå har skapats för din aktuella IP-adress som är öppna port 1433 på hello logisk server.

   ![ange brandväggsregel för server](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicka på **OK** och stäng sedan hello **brandväggsinställningar** sidan.

Nu kan du ansluta toohello SQL Database-server och dess databaser med SQL Server Management Studio eller ett annat verktyg som helst från den här IP-adressen med hello server administratörskonto som skapats tidigare.

> [!IMPORTANT]
> Som standard är åtkomst via hello SQL Database-brandvägg aktiverad för alla Azure-tjänster. Klicka på **OFF** på den här sidan toodisable för alla Azure-tjänster.

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello fullständigt kvalificerade servernamnet för din Azure SQL Database-server i hello Azure-portalen. Du kan använda hello fullständiga server name tooconnect tooyour servern med hjälp av SQL Server Management Studio.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. I hello **Essentials** rutan i hello Azure Portalsida för databasen, leta upp och sedan kopiera hello **servernamn**.

   ![anslutningsinformation](./media/sql-database-connect-query-dotnet/server-name.png)

## <a name="connect-toohello-database-with-ssms"></a>Ansluta toohello databas med SSMS

Använd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) tooestablish en anslutning tooyour Azure SQL Database-server.

1. Öppna SQL Server Management Studio.

2. I hello **ansluta tooServer** dialogrutan Ange hello följande information:

   | Inställning       | Föreslaget värde | Beskrivning | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertyp | Databasmotor | Det här värdet krävs |
   | servernamn | hello fullständigt kvalificerade servernamnet | hello namnet ska vara ungefär så här: **mynewserver20170313.database.windows.net**. |
   | Autentisering | SQL Server-autentisering | SQL-autentisering är hello endast autentiseringstyp som vi har konfigurerat i den här kursen. |
   | Inloggning | Hej server-administratörskontot | Detta är hello-konto som du angav när du skapade hello-server. |
   | Lösenord | hello lösenord för administratörskontot server | Detta är hello lösenord som du angav när du skapade hello-server. |

   ![ansluta tooserver](./media/sql-database-connect-query-ssms/connect.png)

3. Klicka på **alternativ** i hello **ansluta tooserver** dialogrutan. I hello **ansluta toodatabase** ange **mySampleDatabase** tooconnect toothis databas.

   ![ansluta toodb på servern](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klicka på **Anslut**. öppnas med hello Object Explorer i SSMS. 

5. I Object Explorer, expandera **databaser** och expandera sedan **mySampleDatabase** tooview hello objekt i hello-exempeldatabasen.

   ![objekt i databasen](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-hello-database"></a>Skapa tabeller i hello-databas 

Skapa ett databasschema med fyra tabeller som modellen är ett system för hantering av student för universitet med [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):

- Person
- Kursen
- För studenter
- Kredit att modellen ett student hanteringssystem för universitet

hello följande diagram visar hur dessa tabeller är andra relaterade tooeach. Vissa av dessa tabeller referera till kolumner i andra tabeller. Till exempel hello studenttabell refererar till hello **PersonId** kolumn i hello **Person** tabell. Undersökning hello diagram toounderstand hur hello tabeller i den här kursen är relaterade tooone en annan. För en titt på hur toocreate effektiva databastabeller finns [skapa effektiva databastabeller](https://msdn.microsoft.com/library/cc505842.aspx). Information om hur du väljer datatyper finns i [datatyper](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Du kan också använda hello [tabelldesignern i SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) toocreate och utforma tabeller. 

![Relationerna mellan tabellerna](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. I Object Explorer högerklickar du på **mySampleDatabase** och klickar sedan på **Ny fråga**. Ett tomt frågefönster öppnas som är anslutna tooyour databas.

2. I frågefönstret hello kör hello följande fråga toocreate fyra tabeller i databasen: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Skapa tabeller](./media/sql-database-design-first-database/create-tables.png)

3. Expandera hello-tabeller'-nod i hello objektet för SQL Server Management Studio explorer toosee hello tabeller som du skapade.

   ![Skapa tabeller ssms](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-hello-tables"></a>Läs in data till hello tabeller

1. Skapa en mapp med namnet **SampleTableData** i dina hämtningar mappen toostore exempeldata för din databas. 

2. Högerklicka på hello följande länkar och spara dem i hello **SampleTableData** mapp. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Öppna ett kommandotolksfönster och gå toohello SampleTableData mapp.

4. Kör följande kommandon tooinsert exempeldata i hello tabeller ersätter hello värden för hello **ServerName**, **DatabaseName**, **användarnamn**, och **Lösenord** med hello värden för din miljö.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Du har nu lästs in exempeldata i hello-tabeller som du skapade tidigare.

## <a name="query-data"></a>Frågedata

Köra hello följande frågor tooretrieve information från hello databastabeller. Se [skriva SQL-frågor](https://technet.microsoft.com/library/bb264565.aspx) toolearn mer om hur du skriver SQL-frågor. hello första frågan kopplar ihop alla fyra tabeller toofind alla hello elever vilka undervisning förekommer av ' Dominick påven ' som har en klass som är högre än 75% i sin klass. hello andra frågan kopplar ihop alla fyra tabeller och söker efter alla kurser där 'Noe Coleman' någonsin har registrerats.

1. Kör följande fråga hello i en SQL Server Management Studio i frågefönstret:

   ```sql 
   -- Find hello students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. Kör följande fråga i frågefönstret en SQL Server Management Studio:

   ```sql
   -- Find all hello courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Återställa en databas tooa tidigare punkt i tiden

Anta att du av misstag har tagit bort en tabell. Detta är något du lätt kan återställa från. Azure SQL-databas kan du toogo tillbaka tooany punkt tidpunkt i hello senast in too35 dagar och återställningspunkt i tid tooa nya databasen. Du kan den här databasen toorecover dina data. hello återställningspunkt följande hello exempel databasen tooa innan hello tabeller har lagts till.

1. På hello SQL-databasen för databasen klickar du på **återställa** hello i verktygsfältet. Hej **återställa** öppnas.

   ![Återställa](./media/sql-database-design-first-database/restore.png)

2. Fyll i hello **återställa** formulär med hello krävs information:
    * Databasnamn: Ange ett databasnamn 
    * I tidpunkt: Välj hello **i tidpunkt** fliken hello återställning formuläret 
    * Återställningspunkt: Välj en tidpunkt som inträffar innan hello databasen har ändrats
    * Målservern: du kan inte ändra det här värdet när du återställer en databas 
    * Elastisk databaspool: Välj **ingen**  
    * Prisnivån: Välj **20 dtu: er** och **250 GB** lagring.

   ![återställningspunkt](./media/sql-database-design-first-database/restore-point.png)

3. Klicka på **OK** toorestore hello databas för[tooa återställningspunkt i tid](sql-database-recovery-using-backups.md#point-in-time-restore) innan hello tabeller har lagts till. Återställa en databas tooa olika punkt i tiden skapar en dubblett databas i hello samma server som hello originaldatabasen från och med hello tidpunkt som du anger, så länge som befinner sig inom hello kvarhållningsperiod för din [tjänstnivån](sql-database-service-tiers.md).

## <a name="next-steps"></a>Nästa steg 
I kursen får du lärt dig grundläggande uppgifter som skapar en databas och tabeller, läsa in fråga efter data och återställningspunkt hello databasen tooa tidigare tidpunkt. Du har lärt dig att:
> [!div class="checklist"]
> * Skapa en databas
> * Konfigurera en brandväggsregel
> * Ansluta toohello databasen med [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Skapa tabeller
> * Massinläsning av data
> * Fråga data
> * Återställningspunkt hello databasen tooa tidigare tidpunkt med hjälp av SQL-databas [återställning vid tidpunkt](sql-database-recovery-using-backups.md#point-in-time-restore) funktioner

Avancera toohello nästa självstudiekurs toolearn om hur du utformar en databas med Visual Studio och C#.

> [!div class="nextstepaction"]
>[Utforma en Azure SQL database och ansluta med C# och ADO.NET](sql-database-design-first-database-csharp.md)
