
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a>Hämta hello anslutningssträngen från hello Azure-portalen
Använd hello [Azure-portalen](https://portal.azure.com/) tooobtain hello anslutningssträng krävs för din klient programmet toointeract med Azure SQL Database: 

1. Klicka på **Bläddra** > **SQL-databaser**.
2. Ange hello namnet på databasen i hello filter-textrutan nära hello längst upp till vänster i hello **SQL-databaser** bladet.
3. Klicka på hello raden för databasen.
4. När hello bladet visas för databasen, minimera hello som standard i informationssyfte visual kan du klicka på kontroller toocollapse hello blad som du använde för sökning och filtrering av databasen. 
   
    ![Filtrera tooisolate databasen][10-FilterDatabase]
5. Klicka på hello bladet för din databas **visa databasanslutningssträngar**.
6. Om du planerar toouse hello ADO.NET anslutningsbibliotek, kopiera hello sträng med etiketten **ADO**. 
   
    ![Kopiera hello ADO-anslutningssträng för databasen][20-CopyAdoConnectionString]
7. Klistra in hello informationen i anslutningssträngen i din programkod för klienten i ett format eller någon annan.

Mer information finns i:<br/>[Anslutningssträngar och konfigurationsfiler](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
