### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a><span data-ttu-id="b3b3d-101">Skapa en ny logisk SQLServer i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b3b3d-101">Create a new logical SQL server in hello Azure portal</span></span>

1. <span data-ttu-id="b3b3d-102">Klicka på **Nytt**, sök efter **logisk server** och tryck på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-102">Click **New**, search **logical server**, and then hit **ENTER**.</span></span>

    ![sök efter logisk server](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. <span data-ttu-id="b3b3d-104">Välj **SQL-server (logisk server)**</span><span class="sxs-lookup"><span data-stu-id="b3b3d-104">Select **SQL server (logical server)**</span></span> 

    ![välj logisk server](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. <span data-ttu-id="b3b3d-106">Klicka på **skapa** tooopen hello-bladet ny SQL Server (logisk server).</span><span class="sxs-lookup"><span data-stu-id="b3b3d-106">Click **Create** tooopen hello new SQL Server (logical server) blade.</span></span>

   <span data-ttu-id="b3b3d-107"><kbd>![öppna logiska serverblad](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logiska serverblad](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd></span><span class="sxs-lookup"><span data-stu-id="b3b3d-107"><kbd> ![open logical server blade](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![logical server blade](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd></span></span>
  
3. <span data-ttu-id="b3b3d-108">Ange ett giltigt namn för hello ny logisk server i hello SQL Server (logisk server) bladet server name textruta.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-108">In hello SQL Server (logical server) blade's server name text box, provide a valid name for hello new logical server.</span></span> <span data-ttu-id="b3b3d-109">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-109">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Namn på ny server](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > <span data-ttu-id="b3b3d-111">hello fullständigt kvalificerade namn för den nya servern kommer att < namn_på_servern >. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-111">hello fully qualified name for your new server will be <your_server_name>.database.windows.net.</span></span>
    >
    
4. <span data-ttu-id="b3b3d-112">Ange ett användarnamn för inloggning för hello SQL-autentisering för den här servern hello Server-administratören logga in i textrutan.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-112">In hello Server admin login text box, provide a user name for hello SQL authentication login for this server.</span></span> <span data-ttu-id="b3b3d-113">Den här inloggningen kallas hello huvudsaklig inloggning på servern.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-113">This login is known as hello server principal login.</span></span> <span data-ttu-id="b3b3d-114">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-114">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![SQL-administratörsinloggning](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. <span data-ttu-id="b3b3d-116">I hello **lösenord** och **Bekräfta lösenord** textrutor, ange ett lösenord för hello server huvudsaklig inloggning på kontot.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-116">In hello **Password** and **Confirm password** text boxes, provide a password for hello server principal login account.</span></span> <span data-ttu-id="b3b3d-117">En grön kryssmarkering visar att du har angett ett giltigt lösenord.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-117">A green check mark indicates that you have provided a valid password.</span></span>
    
    ![SQL-administratörslösenord](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. <span data-ttu-id="b3b3d-119">Välj en prenumeration där du har behörighet toocreate objekt.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-119">Select a subscription in which you have permission toocreate objects.</span></span>

    ![prenumeration](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. <span data-ttu-id="b3b3d-121">Hello resurs gruppen i textrutan Välj **Skapa nytt** och ange ett giltigt namn för hello ny resursgrupp (du kan också använda en befintlig resursgrupp om du redan har skapat en själv) hello resurs gruppen i textrutan.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-121">In hello Resource group text box, select **Create new** and then, in hello resource group text box, provide a valid name for hello new resource group (you can also use an existing resource group if you have already created one for yourself).</span></span> <span data-ttu-id="b3b3d-122">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-122">A green check mark indicates that you have provided a valid name.</span></span>

    ![Ny resursgrupp](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. <span data-ttu-id="b3b3d-124">I hello **plats** textruta, Välj en datatyp center lämpliga tooyour plats - till exempel ”östra”.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-124">In hello **Location** text box, select a data center appropriate tooyour location - such as "Australia East".</span></span>
    
    ![Serverplats](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > <span data-ttu-id="b3b3d-126">Hej kryssrutan för **Tillåt azure-tjänster tooaccess server** kan inte ändras på det här bladet.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-126">hello checkbox for **Allow azure services tooaccess server** cannot be changed on this blade.</span></span> <span data-ttu-id="b3b3d-127">Du kan ändra den här inställningen på hello serverblad för brandväggen.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-127">You can change this setting on hello server firewall blade.</span></span> <span data-ttu-id="b3b3d-128">Mer information finns i [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md) (Komma igång med säkerhet).</span><span class="sxs-lookup"><span data-stu-id="b3b3d-128">For more information, see [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md).</span></span>
    >
    
9. <span data-ttu-id="b3b3d-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b3b3d-129">Click **Create**.</span></span>

    ![Knappen Skapa](./media/sql-data-warehouse-create-logical-server/create.png)

