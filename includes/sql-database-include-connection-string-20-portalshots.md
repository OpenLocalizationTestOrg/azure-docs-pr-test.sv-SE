
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a><span data-ttu-id="a8f55-101">Hämta hello anslutningssträngen från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a8f55-101">Obtain hello connection string from hello Azure portal</span></span>
<span data-ttu-id="a8f55-102">Använd hello [Azure-portalen](https://portal.azure.com/) tooobtain hello anslutningssträng krävs för din klient programmet toointeract med Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="a8f55-102">Use hello [Azure portal](https://portal.azure.com/) tooobtain hello connection string necessary for your client program toointeract with Azure SQL Database:</span></span> 

1. <span data-ttu-id="a8f55-103">Klicka på **Bläddra** > **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="a8f55-103">Click **BROWSE** > **SQL databases**.</span></span>
2. <span data-ttu-id="a8f55-104">Ange hello namnet på databasen i hello filter-textrutan nära hello längst upp till vänster i hello **SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="a8f55-104">Enter hello name of your database into hello filter text box near hello upper-left of hello **SQL databases** blade.</span></span>
3. <span data-ttu-id="a8f55-105">Klicka på hello raden för databasen.</span><span class="sxs-lookup"><span data-stu-id="a8f55-105">Click hello row for your database.</span></span>
4. <span data-ttu-id="a8f55-106">När hello bladet visas för databasen, minimera hello som standard i informationssyfte visual kan du klicka på kontroller toocollapse hello blad som du använde för sökning och filtrering av databasen.</span><span class="sxs-lookup"><span data-stu-id="a8f55-106">After hello blade appears for your database, for visual convenience you can click hello standard minimize controls toocollapse hello blades  you used for browsing and database filtering.</span></span> 
   
    ![Filtrera tooisolate databasen][10-FilterDatabase]
5. <span data-ttu-id="a8f55-108">Klicka på hello bladet för din databas **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="a8f55-108">On hello blade for your database, click **Show database connection strings**.</span></span>
6. <span data-ttu-id="a8f55-109">Om du planerar toouse hello ADO.NET anslutningsbibliotek, kopiera hello sträng med etiketten **ADO**.</span><span class="sxs-lookup"><span data-stu-id="a8f55-109">If you intend toouse hello ADO.NET connection library, copy hello string labeled **ADO**.</span></span> 
   
    ![Kopiera hello ADO-anslutningssträng för databasen][20-CopyAdoConnectionString]
7. <span data-ttu-id="a8f55-111">Klistra in hello informationen i anslutningssträngen i din programkod för klienten i ett format eller någon annan.</span><span class="sxs-lookup"><span data-stu-id="a8f55-111">In one format or another, paste hello connection string information into your client program code.</span></span>

<span data-ttu-id="a8f55-112">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="a8f55-112">For more information, see:</span></span><br/><span data-ttu-id="a8f55-113">[Anslutningssträngar och konfigurationsfiler](http://msdn.microsoft.com/library/ms254494.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f55-113">[Connection Strings and Configuration Files](http://msdn.microsoft.com/library/ms254494.aspx).</span></span>

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
