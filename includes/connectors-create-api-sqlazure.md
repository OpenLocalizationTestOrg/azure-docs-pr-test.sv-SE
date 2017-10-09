### <a name="prerequisites"></a>Krav
* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [Azure SQL Database](../articles/sql-database/sql-database-get-started.md) med dess anslutningsinformation, inklusive hello servernamnet, databasnamnet och användarnamn/lösenord. Den här informationen ingår i hello anslutningssträng för SQL-databasen:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalog =*yourqldbname*; Spara säkerhetsinformation = False; Användar-ID = {your_username}; Lösenord = {your_password}; MultipleActiveResultSets = False; Kryptera = True; TrustServerCertificate = False; Timeout för anslutningen = 30.
  
    Läs mer om [Azure SQL-databaser](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> När du skapar en Azure SQL Database, kan du också skapa hello-exempeldatabaserna som ingår i SQL. 
> 
> 

Innan du använder Azure SQL Database i en logikapp, ansluta tooyour SQL-databas. Du kan göra detta enkelt i din logikapp på hello Azure-portalen.  

Anslut tooyour Azure SQL Database med hello följande steg:  

1. Skapa en logikapp. Lägg till en utlösare i hello Logic Apps designer och lägga till en åtgärd. Välj **visa Microsoft hanterade API: er** i hello listrutan och ange sedan ”sql” i sökrutan för hello. Välj någon av hello åtgärder:  
   
    ![Steg i att skapa SQL Azure-anslutning](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Om du inte har skapat några anslutningar tooSQL databasen efterfrågas hello anslutningsinformation:  
   
    ![Steg i att skapa SQL Azure-anslutning](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Ange information om hello SQL-databas. Egenskaper med en asterisk krävs.
   
   | Egenskap | Information |
   | --- | --- |
   | Anslut via Gateway |Lämna det här alternativet är avmarkerat. Det här används när du ansluter tooan lokala SQL Server. |
   | Anslutningsnamn * |Ange ett namn för anslutningen. |
   | SQL Server-namnet * |Ange hello servernamn; vilket är något som liknar *servername.database.windows.net*. hello servernamn visas i hello SQL Database-egenskaper i hello Azure-portalen och visas också i hello anslutningssträngen. |
   | Databasnamn för SQL * |Ange hello namn du gav din SQL-databas. Detta visas i egenskaperna för hello SQL-databas i anslutningssträngen för hello: Initial Catalog =*yoursqldbname*. |
   | Användarnamn * |Ange hello användarnamn som du skapade när hello SQL-databas har skapats. Detta visas i hello SQL Database-egenskaper i hello Azure-portalen. |
   | Lösenord * |Ange hello-lösenord som du skapade när hello SQL-databas har skapats. |
   
    Autentiseringsuppgifterna är används tooauthorize din logik app tooconnect och komma åt SQL-data. När du är klar, se detaljer om serveranslutning liknande toohello följande:  
   
    ![Steg i att skapa SQL Azure-anslutning](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Välj **Skapa**. 
5. Meddelande hello anslutningen har skapats. Nu kan fortsätta med hello andra steg i din logikapp: 
   
    ![Steg i att skapa SQL Azure-anslutning](./media/connectors-create-api-sqlazure/table.png)

