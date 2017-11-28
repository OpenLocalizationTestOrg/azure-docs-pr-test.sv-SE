
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a><span data-ttu-id="ae170-101">Hämta anslutningssträngen från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ae170-101">Obtain the connection string from the Azure portal</span></span>
<span data-ttu-id="ae170-102">Använd den [Azure-portalen](https://portal.azure.com/) att hämta anslutningssträngen som behövs för att klienten ska interagera med Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="ae170-102">Use the [Azure portal](https://portal.azure.com/) to obtain the connection string necessary for your client program to interact with Azure SQL Database:</span></span> 

1. <span data-ttu-id="ae170-103">Klicka på **Bläddra** > **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="ae170-103">Click **BROWSE** > **SQL databases**.</span></span>
2. <span data-ttu-id="ae170-104">Ange namnet på databasen i textrutan filter nästan längst upp till vänster i den **SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="ae170-104">Enter the name of your database into the filter text box near the upper-left of the **SQL databases** blade.</span></span>
3. <span data-ttu-id="ae170-105">Klicka på raden för databasen.</span><span class="sxs-lookup"><span data-stu-id="ae170-105">Click the row for your database.</span></span>
4. <span data-ttu-id="ae170-106">När det visas i bladet för din databas, visual i informationssyfte du kan klicka på standard minimera kontroller om du vill komprimera blad som du använde för sökning och filtrering av databasen.</span><span class="sxs-lookup"><span data-stu-id="ae170-106">After the blade appears for your database, for visual convenience you can click the standard minimize controls to collapse the blades  you used for browsing and database filtering.</span></span> 
   
    ![Filter för att isolera din databas][10-FilterDatabase]
5. <span data-ttu-id="ae170-108">Klicka på bladet för din databas **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="ae170-108">On the blade for your database, click **Show database connection strings**.</span></span>
6. <span data-ttu-id="ae170-109">Om du tänker använda anslutningsbibliotek ADO.NET kopiera strängen med etiketten **ADO**.</span><span class="sxs-lookup"><span data-stu-id="ae170-109">If you intend to use the ADO.NET connection library, copy the string labeled **ADO**.</span></span> 
   
    ![Kopiera ADO anslutningssträngen för databasen][20-CopyAdoConnectionString]
7. <span data-ttu-id="ae170-111">Klistra in informationen i anslutningssträngen i din programkod för klienten i ett format eller någon annan.</span><span class="sxs-lookup"><span data-stu-id="ae170-111">In one format or another, paste the connection string information into your client program code.</span></span>

<span data-ttu-id="ae170-112">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="ae170-112">For more information, see:</span></span><br/><span data-ttu-id="ae170-113">[Anslutningssträngar och konfigurationsfiler](http://msdn.microsoft.com/library/ms254494.aspx).</span><span class="sxs-lookup"><span data-stu-id="ae170-113">[Connection Strings and Configuration Files](http://msdn.microsoft.com/library/ms254494.aspx).</span></span>

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
