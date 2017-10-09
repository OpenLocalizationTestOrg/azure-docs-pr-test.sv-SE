<span data-ttu-id="e71c8-101">I det här steget kan skapa du manuellt hello tillgänglighetsgruppens lyssnare i hanteraren för redundanskluster och SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="e71c8-101">In this step, you manually create hello availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

1. <span data-ttu-id="e71c8-102">Öppna Hanteraren för redundanskluster från hello-noden som är värd för hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="e71c8-102">Open Failover Cluster Manager from hello node that hosts hello primary replica.</span></span>

2. <span data-ttu-id="e71c8-103">Välj hello **nätverk** noden och Observera hello klustrets nätverksnamn.</span><span class="sxs-lookup"><span data-stu-id="e71c8-103">Select hello **Networks** node, and then note hello cluster network name.</span></span> <span data-ttu-id="e71c8-104">Det här namnet används i hello $ClusterNetworkName variabel i hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="e71c8-104">This name is used in hello $ClusterNetworkName variable in hello PowerShell script.</span></span>

3. <span data-ttu-id="e71c8-105">Expandera klusternamnet hello och klicka sedan på **roller**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-105">Expand hello cluster name, and then click **Roles**.</span></span>

4. <span data-ttu-id="e71c8-106">I hello **roller** fönstret, högerklicka på hello-tillgänglighetsgrupp och sedan markera **Lägg till resurs** > **klientåtkomstpunkt**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-106">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>
   
    ![Lägg till klientåtkomstpunkten för tillgänglighetsgruppen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. <span data-ttu-id="e71c8-108">I hello **namn** , skapar du ett namn för den här lyssnaren, klickar du på **nästa** två gånger, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-108">In hello **Name** box, create a name for this new listener, click **Next** twice, and then click **Finish**.</span></span>  
    <span data-ttu-id="e71c8-109">Tar inte med hello lyssnare eller resursen i det här läget.</span><span class="sxs-lookup"><span data-stu-id="e71c8-109">Do not bring hello listener or resource online at this point.</span></span>

6. <span data-ttu-id="e71c8-110">Klicka på hello **resurser** fliken och expandera sedan hello klientåtkomstpunkt som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="e71c8-110">Click hello **Resources** tab, and then expand hello client access point you just created.</span></span> 
    <span data-ttu-id="e71c8-111">hello IP-adressresurs för varje klusternätverk i klustret visas.</span><span class="sxs-lookup"><span data-stu-id="e71c8-111">hello IP address resource for each cluster network in your cluster is displayed.</span></span> <span data-ttu-id="e71c8-112">Om detta är en Azure-only-lösning, visas endast en IP-adressresurs.</span><span class="sxs-lookup"><span data-stu-id="e71c8-112">If this is an Azure-only solution, only one IP address resource is displayed.</span></span>

7. <span data-ttu-id="e71c8-113">Gör något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="e71c8-113">Do either of hello following:</span></span>
   
   * <span data-ttu-id="e71c8-114">tooconfigure hybridlösning:</span><span class="sxs-lookup"><span data-stu-id="e71c8-114">tooconfigure a hybrid solution:</span></span>
     
        <span data-ttu-id="e71c8-115">a.</span><span class="sxs-lookup"><span data-stu-id="e71c8-115">a.</span></span> <span data-ttu-id="e71c8-116">Högerklicka på hello IP-adressresurs som motsvarar tooyour lokala undernät och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-116">Right-click hello IP address resource that corresponds tooyour on-premises subnet, and then select **Properties**.</span></span> <span data-ttu-id="e71c8-117">Observera hello IP-adress och nätverksnamn.</span><span class="sxs-lookup"><span data-stu-id="e71c8-117">Note hello IP address name and network name.</span></span>
   
        <span data-ttu-id="e71c8-118">b.</span><span class="sxs-lookup"><span data-stu-id="e71c8-118">b.</span></span> <span data-ttu-id="e71c8-119">Välj **statisk IP-adress**, tilldela en IP-adress som inte används och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-119">Select **Static IP Address**, assign an unused IP address, and then click **OK**.</span></span>
 
   * <span data-ttu-id="e71c8-120">tooconfigure en endast Azure-lösning:</span><span class="sxs-lookup"><span data-stu-id="e71c8-120">tooconfigure an Azure-only solution:</span></span>

        <span data-ttu-id="e71c8-121">a.</span><span class="sxs-lookup"><span data-stu-id="e71c8-121">a.</span></span> <span data-ttu-id="e71c8-122">Högerklicka på hello IP-adressresurs som motsvarar tooyour Azure-undernät och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-122">Right-click hello IP address resource that corresponds tooyour Azure subnet, and then select **Properties**.</span></span>
       
       > [!NOTE]
       > <span data-ttu-id="e71c8-123">Du kan konfigurera en statisk IP-adress för i egenskapsfönstret om lyssnaren hello senare misslyckas toocome online på grund av en konflikt IP-adress som valts av DHCP.</span><span class="sxs-lookup"><span data-stu-id="e71c8-123">If hello listener later fails toocome online because of a conflicting IP address selected by DHCP, you can configure a valid static IP address in this properties window.</span></span>
       > 
       > 

       <span data-ttu-id="e71c8-124">b.</span><span class="sxs-lookup"><span data-stu-id="e71c8-124">b.</span></span> <span data-ttu-id="e71c8-125">Hej i samma **IP-adress** egenskapsfönstret, ändra hello **IP-adressnamn**.</span><span class="sxs-lookup"><span data-stu-id="e71c8-125">In hello same **IP Address** properties window, change hello **IP Address Name**.</span></span>  
        <span data-ttu-id="e71c8-126">Det här namnet används i hello $IPResourceName variabel av hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="e71c8-126">This name is used in hello $IPResourceName variable of hello PowerShell script.</span></span> <span data-ttu-id="e71c8-127">Om din lösning sträcker sig över flera virtuella Azure-nätverk, kan du upprepa det här steget för varje IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="e71c8-127">If your solution spans multiple Azure virtual networks, repeat this step for each IP resource.</span></span>

