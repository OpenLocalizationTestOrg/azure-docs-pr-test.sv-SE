
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a><span data-ttu-id="2abdb-101">C#-program-exempel</span><span class="sxs-lookup"><span data-stu-id="2abdb-101">C# program example</span></span>

<span data-ttu-id="2abdb-102">hello nästa avsnitt i den här artikeln finns ett C#-program som använder ADO.NET toosend Transact-SQL-instruktioner toohello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2abdb-102">hello next sections of this article present a C# program that uses ADO.NET toosend Transact-SQL statements toohello SQL database.</span></span> <span data-ttu-id="2abdb-103">hello C# utförs hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="2abdb-103">hello C# program performs hello following actions:</span></span>

1. <span data-ttu-id="2abdb-104">[Ansluter tooour SQL-databas med hjälp av ADO.NET](#cs_1_connect).</span><span class="sxs-lookup"><span data-stu-id="2abdb-104">[Connects tooour SQL database using ADO.NET](#cs_1_connect).</span></span>
2. <span data-ttu-id="2abdb-105">[Skapar tabeller](#cs_2_createtables).</span><span class="sxs-lookup"><span data-stu-id="2abdb-105">[Creates tables](#cs_2_createtables).</span></span>
3. <span data-ttu-id="2abdb-106">[Fyller hello tabeller med data, genom att utfärda T-SQL INSERT-instruktioner](#cs_3_insert).</span><span class="sxs-lookup"><span data-stu-id="2abdb-106">[Populates hello tables with data, by issuing T-SQL INSERT statements](#cs_3_insert).</span></span>
4. <span data-ttu-id="2abdb-107">[Uppdaterar data med hjälp av en koppling](#cs_4_updatejoin).</span><span class="sxs-lookup"><span data-stu-id="2abdb-107">[Updates data by use of a join](#cs_4_updatejoin).</span></span>
5. <span data-ttu-id="2abdb-108">[Tar bort data med hjälp av en koppling](#cs_5_deletejoin).</span><span class="sxs-lookup"><span data-stu-id="2abdb-108">[Deletes data by use of a join](#cs_5_deletejoin).</span></span>
6. <span data-ttu-id="2abdb-109">[Väljer datarader med hjälp av en koppling](#cs_6_selectrows).</span><span class="sxs-lookup"><span data-stu-id="2abdb-109">[Selects data rows by use of a join](#cs_6_selectrows).</span></span>
7. <span data-ttu-id="2abdb-110">Stänger hello-anslutningen (som utelämnar eventuella temporära tabeller från tempdb).</span><span class="sxs-lookup"><span data-stu-id="2abdb-110">Closes hello connection (which drops any temporary tables from tempdb).</span></span>

<span data-ttu-id="2abdb-111">hello C#-program innehåller:</span><span class="sxs-lookup"><span data-stu-id="2abdb-111">hello C# program contains:</span></span>

- <span data-ttu-id="2abdb-112">C#-kod tooconnect toohello databas.</span><span class="sxs-lookup"><span data-stu-id="2abdb-112">C# code tooconnect toohello database.</span></span>
- <span data-ttu-id="2abdb-113">Metoder som returnerar hello T-SQL-källkoden.</span><span class="sxs-lookup"><span data-stu-id="2abdb-113">Methods that return hello T-SQL source code.</span></span>
- <span data-ttu-id="2abdb-114">Två metoder som skickar hello T-SQL toohello databas.</span><span class="sxs-lookup"><span data-stu-id="2abdb-114">Two methods that submit hello T-SQL toohello database.</span></span>

#### <a name="toocompile-and-run"></a><span data-ttu-id="2abdb-115">toocompile och kör</span><span class="sxs-lookup"><span data-stu-id="2abdb-115">toocompile and run</span></span>

<span data-ttu-id="2abdb-116">Den här C#-program är logiskt en .cs fil.</span><span class="sxs-lookup"><span data-stu-id="2abdb-116">This C# program is logically one .cs file.</span></span> <span data-ttu-id="2abdb-117">Men här hello programmet fysiskt är uppdelad i flera kodblock, toomake enklare toosee där varje block och förstå.</span><span class="sxs-lookup"><span data-stu-id="2abdb-117">But here hello program is physically divided into several code blocks, toomake each block easier toosee and understand.</span></span> <span data-ttu-id="2abdb-118">toocompile och kör det här programmet, hello följande:</span><span class="sxs-lookup"><span data-stu-id="2abdb-118">toocompile and run this program, do hello following:</span></span>

1. <span data-ttu-id="2abdb-119">Skapa ett C#-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2abdb-119">Create a C# project in Visual Studio.</span></span>
    - <span data-ttu-id="2abdb-120">hello projekttypen ska vara en *konsolen* program från något som liknar följande hierarki hello: **mallar** > **Visual C#** > **Klassiska Windows-skrivbordet** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="2abdb-120">hello project type should be a *console* application, from something like hello following hierarchy: **Templates** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
3. <span data-ttu-id="2abdb-121">I filen hello **Program.cs**, radera hello små starter rader med kod.</span><span class="sxs-lookup"><span data-stu-id="2abdb-121">In hello file **Program.cs**, erase hello small starter lines of code.</span></span>
3. <span data-ttu-id="2abdb-122">I Program.cs, kopiera och klistra in varje av hello efter block i hello samma ordning som de visas här.</span><span class="sxs-lookup"><span data-stu-id="2abdb-122">Into Program.cs, copy and paste each of hello following blocks, in hello same sequence they are presented here.</span></span>
4. <span data-ttu-id="2abdb-123">I Program.cs redigera hello följande värden i hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="2abdb-123">In Program.cs, edit hello following values in hello **Main** method:</span></span>

   - <span data-ttu-id="2abdb-124">**CB. DataSource**</span><span class="sxs-lookup"><span data-stu-id="2abdb-124">**cb.DataSource**</span></span>
   - <span data-ttu-id="2abdb-125">**CD: n. Användar-ID**</span><span class="sxs-lookup"><span data-stu-id="2abdb-125">**cd.UserID**</span></span>
   - <span data-ttu-id="2abdb-126">**CB. Lösenord**</span><span class="sxs-lookup"><span data-stu-id="2abdb-126">**cb.Password**</span></span>
   - <span data-ttu-id="2abdb-127">**InitialCatalog**</span><span class="sxs-lookup"><span data-stu-id="2abdb-127">**InitialCatalog**</span></span>

5. <span data-ttu-id="2abdb-128">Kontrollera att sammansättningen hello **System.Data.dll** refererar till.</span><span class="sxs-lookup"><span data-stu-id="2abdb-128">Verify that hello assembly **System.Data.dll** is referenced.</span></span> <span data-ttu-id="2abdb-129">tooverify, expandera hello **referenser** nod i hello **Solution Explorer** fönstret.</span><span class="sxs-lookup"><span data-stu-id="2abdb-129">tooverify, expand hello **References** node in hello **Solution Explorer** pane.</span></span>
6. <span data-ttu-id="2abdb-130">toobuild hello program i Visual Studio klickar du på hello **skapa** menyn.</span><span class="sxs-lookup"><span data-stu-id="2abdb-130">toobuild hello program in Visual Studio, click hello **Build** menu.</span></span>
7. <span data-ttu-id="2abdb-131">toorun hello programmet från Visual Studio klickar du på hello **starta** knappen.</span><span class="sxs-lookup"><span data-stu-id="2abdb-131">toorun hello program from Visual Studio, click hello **Start** button.</span></span> <span data-ttu-id="2abdb-132">hello rapportutdata visas i ett fönster för cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="2abdb-132">hello report output is displayed in a cmd.exe window.</span></span>

> [!NOTE]
> <span data-ttu-id="2abdb-133">Du har hello möjlighet att redigera hello T-SQL tooadd inledande  **#**  toohello tabellnamn som skapar dem som temporära tabeller i **tempdb**.</span><span class="sxs-lookup"><span data-stu-id="2abdb-133">You have hello option of editing hello T-SQL tooadd a leading **#** toohello table names, which creates them as temporary tables in **tempdb**.</span></span> <span data-ttu-id="2abdb-134">Detta kan vara användbart i demonstrationssyfte, när ingen testdatabasen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2abdb-134">This can be useful for demonstration purposes, when no test database is available.</span></span> <span data-ttu-id="2abdb-135">Temporära tabeller tas bort automatiskt när hello anslutningen stängs.</span><span class="sxs-lookup"><span data-stu-id="2abdb-135">Temporary tables are deleted automatically when hello connection closes.</span></span> <span data-ttu-id="2abdb-136">Alla referenser till sekundärnycklar tillämpas inte för temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="2abdb-136">Any REFERENCES for foreign keys are not enforced for temporary tables.</span></span>
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a><span data-ttu-id="2abdb-137">C#-blocket 1: ansluta med hjälp av ADO.NET</span><span class="sxs-lookup"><span data-stu-id="2abdb-137">C# block 1: Connect by using ADO.NET</span></span>

- [<span data-ttu-id="2abdb-138">Nästa</span><span class="sxs-lookup"><span data-stu-id="2abdb-138">Next</span></span>](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View hello report output here, then press any key tooend hello program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-toocreate-tables"></a><span data-ttu-id="2abdb-139">C# block 2: T-SQL toocreate tabeller</span><span class="sxs-lookup"><span data-stu-id="2abdb-139">C# block 2: T-SQL toocreate tables</span></span>

- <span data-ttu-id="2abdb-140">[Tidigare](#cs_1_connect) &nbsp;  /  &nbsp; [nästa](#cs_3_insert)</span><span class="sxs-lookup"><span data-stu-id="2abdb-140">[Previous](#cs_1_connect) &nbsp; / &nbsp; [Next](#cs_3_insert)</span></span>

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a><span data-ttu-id="2abdb-141">Diagram för entiteten relation (ERD)</span><span class="sxs-lookup"><span data-stu-id="2abdb-141">Entity Relationship Diagram (ERD)</span></span>

<span data-ttu-id="2abdb-142">hello föregående CREATE TABLE-instruktioner omfattar hello **referenser** nyckelordet toocreate en *sekundärnyckeln* (FK) relation mellan två tabeller.</span><span class="sxs-lookup"><span data-stu-id="2abdb-142">hello preceding CREATE TABLE statements involve hello **REFERENCES** keyword toocreate a *foreign key* (FK) relationship between two tables.</span></span>  <span data-ttu-id="2abdb-143">Om du använder tempdb kommentera ut hello `--REFERENCES` nyckelordet med ett par med inledande streck.</span><span class="sxs-lookup"><span data-stu-id="2abdb-143">If you are using tempdb, comment out hello `--REFERENCES` keyword using a pair of leading dashes.</span></span>

<span data-ttu-id="2abdb-144">Nästa är en Reparationsdiskett som visar hello relation mellan hello två tabeller.</span><span class="sxs-lookup"><span data-stu-id="2abdb-144">Next is an ERD that displays hello relationship between hello two tables.</span></span> <span data-ttu-id="2abdb-145">Hej värden i hello #tabEmployee.DepartmentCode *underordnade* kolumnen är begränsad toohello värden finns i hello #tabDepartment.Department *överordnade* kolumn.</span><span class="sxs-lookup"><span data-stu-id="2abdb-145">hello values in hello #tabEmployee.DepartmentCode *child* column are limited toohello values present in hello #tabDepartment.Department *parent* column.</span></span>

![ERD visar sekundärnyckeln](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-tooinsert-data"></a><span data-ttu-id="2abdb-147">C# block 3: T-SQL tooinsert data</span><span class="sxs-lookup"><span data-stu-id="2abdb-147">C# block 3: T-SQL tooinsert data</span></span>

- <span data-ttu-id="2abdb-148">[Tidigare](#cs_2_createtables) &nbsp;  /  &nbsp; [nästa](#cs_4_updatejoin)</span><span class="sxs-lookup"><span data-stu-id="2abdb-148">[Previous](#cs_2_createtables) &nbsp; / &nbsp; [Next](#cs_4_updatejoin)</span></span>


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- hello company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- hello company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-tooupdate-join"></a><span data-ttu-id="2abdb-149">C# block 4: T-SQL tooupdate-koppling</span><span class="sxs-lookup"><span data-stu-id="2abdb-149">C# block 4: T-SQL tooupdate-join</span></span>

- <span data-ttu-id="2abdb-150">[Tidigare](#cs_3_insert) &nbsp;  /  &nbsp; [nästa](#cs_5_deletejoin)</span><span class="sxs-lookup"><span data-stu-id="2abdb-150">[Previous](#cs_3_insert) &nbsp; / &nbsp; [Next](#cs_5_deletejoin)</span></span>


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-toodelete-join"></a><span data-ttu-id="2abdb-151">C# block 5: T-SQL toodelete-koppling</span><span class="sxs-lookup"><span data-stu-id="2abdb-151">C# block 5: T-SQL toodelete-join</span></span>

- <span data-ttu-id="2abdb-152">[Tidigare](#cs_4_updatejoin) &nbsp;  /  &nbsp; [nästa](#cs_6_selectrows)</span><span class="sxs-lookup"><span data-stu-id="2abdb-152">[Previous](#cs_4_updatejoin) &nbsp; / &nbsp; [Next](#cs_6_selectrows)</span></span>


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size hello Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband hello Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-tooselect-rows"></a><span data-ttu-id="2abdb-153">C# block 6: T-SQL tooselect rader</span><span class="sxs-lookup"><span data-stu-id="2abdb-153">C# block 6: T-SQL tooselect rows</span></span>

- <span data-ttu-id="2abdb-154">[Tidigare](#cs_5_deletejoin) &nbsp;  /  &nbsp; [nästa](#cs_6b_datareader)</span><span class="sxs-lookup"><span data-stu-id="2abdb-154">[Previous](#cs_5_deletejoin) &nbsp; / &nbsp; [Next](#cs_6b_datareader)</span></span>


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all hello final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a><span data-ttu-id="2abdb-155">C# block 6b: ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="2abdb-155">C# block 6b: ExecuteReader</span></span>

- <span data-ttu-id="2abdb-156">[Tidigare](#cs_6_selectrows) &nbsp;  /  &nbsp; [nästa](#cs_7_executenonquery)</span><span class="sxs-lookup"><span data-stu-id="2abdb-156">[Previous](#cs_6_selectrows) &nbsp; / &nbsp; [Next](#cs_7_executenonquery)</span></span>

<span data-ttu-id="2abdb-157">Den här metoden är utformad toorun hello T-SQL SELECT-instruktion som skapas genom hello **Build_6_Tsql_SelectEmployees** metod.</span><span class="sxs-lookup"><span data-stu-id="2abdb-157">This method is designed toorun hello T-SQL SELECT statement that is built by hello **Build_6_Tsql_SelectEmployees** method.</span></span>


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a><span data-ttu-id="2abdb-158">C# block 7: ExecuteNonQuery</span><span class="sxs-lookup"><span data-stu-id="2abdb-158">C# block 7: ExecuteNonQuery</span></span>

- <span data-ttu-id="2abdb-159">[Tidigare](#cs_6b_datareader) &nbsp;  /  &nbsp; [nästa](#cs_8_output)</span><span class="sxs-lookup"><span data-stu-id="2abdb-159">[Previous](#cs_6b_datareader) &nbsp; / &nbsp; [Next](#cs_8_output)</span></span>

<span data-ttu-id="2abdb-160">Den här metoden anropas för åtgärder som ändrar hello datainnehållet tabeller utan att returnera alla rader med data.</span><span class="sxs-lookup"><span data-stu-id="2abdb-160">This method is called for operations that modify hello data content of tables without returning any data rows.</span></span>


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL too{0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-toohello-console"></a><span data-ttu-id="2abdb-161">C# block 8: faktiska test utdata toohello konsolen</span><span class="sxs-lookup"><span data-stu-id="2abdb-161">C# block 8: Actual test output toohello console</span></span>

- [<span data-ttu-id="2abdb-162">Tidigare</span><span class="sxs-lookup"><span data-stu-id="2abdb-162">Previous</span></span>](#cs_7_executenonquery)

<span data-ttu-id="2abdb-163">Det här avsnittet innehåller hello utdata som hello program som skickas toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="2abdb-163">This section captures hello output that hello program sent toohello console.</span></span> <span data-ttu-id="2abdb-164">Endast hello guid värdena variera mellan testkörningar.</span><span class="sxs-lookup"><span data-stu-id="2abdb-164">Only hello guid values vary between test runs.</span></span>


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
