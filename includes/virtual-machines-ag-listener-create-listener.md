<span data-ttu-id="2bd54-101">I det här steget kan skapa du manuellt tillgänglighetsgruppens lyssnare i hanteraren för redundanskluster och SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="2bd54-101">In this step, you manually create the availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

1. <span data-ttu-id="2bd54-102">Öppna Klusterhanteraren för växling från den nod som värd för den primära repliken.</span><span class="sxs-lookup"><span data-stu-id="2bd54-102">Open Failover Cluster Manager from the node that hosts the primary replica.</span></span>

2. <span data-ttu-id="2bd54-103">Välj den **nätverk** nod och Observera klustrets nätverksnamn.</span><span class="sxs-lookup"><span data-stu-id="2bd54-103">Select the **Networks** node, and then note the cluster network name.</span></span> <span data-ttu-id="2bd54-104">Det här namnet används i variabeln $ClusterNetworkName i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="2bd54-104">This name is used in the $ClusterNetworkName variable in the PowerShell script.</span></span>

3. <span data-ttu-id="2bd54-105">Expandera klusternamnet och klicka sedan på **roller**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-105">Expand the cluster name, and then click **Roles**.</span></span>

4. <span data-ttu-id="2bd54-106">I den **roller** fönstret, högerklicka på tillgänglighetsgruppens namn och välj sedan **Lägg till resurs** > **klientåtkomstpunkt**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-106">In the **Roles** pane, right-click the availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>
   
    ![Lägg till klientåtkomstpunkten för tillgänglighetsgruppen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. <span data-ttu-id="2bd54-108">I den **namn** , skapar du ett namn för den här lyssnaren, klickar du på **nästa** två gånger, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-108">In the **Name** box, create a name for this new listener, click **Next** twice, and then click **Finish**.</span></span>  
    <span data-ttu-id="2bd54-109">Tar inte med lyssnare eller resursen i det här läget.</span><span class="sxs-lookup"><span data-stu-id="2bd54-109">Do not bring the listener or resource online at this point.</span></span>

6. <span data-ttu-id="2bd54-110">Klicka på den **resurser** fliken och expandera sedan den klientåtkomstpunkt som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="2bd54-110">Click the **Resources** tab, and then expand the client access point you just created.</span></span> 
    <span data-ttu-id="2bd54-111">IP-adressresurs för varje klusternätverk i klustret visas.</span><span class="sxs-lookup"><span data-stu-id="2bd54-111">The IP address resource for each cluster network in your cluster is displayed.</span></span> <span data-ttu-id="2bd54-112">Om detta är en Azure-only-lösning, visas endast en IP-adressresurs.</span><span class="sxs-lookup"><span data-stu-id="2bd54-112">If this is an Azure-only solution, only one IP address resource is displayed.</span></span>

7. <span data-ttu-id="2bd54-113">Gör något av följande:</span><span class="sxs-lookup"><span data-stu-id="2bd54-113">Do either of the following:</span></span>
   
   * <span data-ttu-id="2bd54-114">Så här konfigurerar du en hybridlösning:</span><span class="sxs-lookup"><span data-stu-id="2bd54-114">To configure a hybrid solution:</span></span>
     
        <span data-ttu-id="2bd54-115">a.</span><span class="sxs-lookup"><span data-stu-id="2bd54-115">a.</span></span> <span data-ttu-id="2bd54-116">Högerklicka på IP-adressresurs som motsvarar det lokala undernätet och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-116">Right-click the IP address resource that corresponds to your on-premises subnet, and then select **Properties**.</span></span> <span data-ttu-id="2bd54-117">Observera namn på IP-adressen och nätverksnamnet.</span><span class="sxs-lookup"><span data-stu-id="2bd54-117">Note the IP address name and network name.</span></span>
   
        <span data-ttu-id="2bd54-118">b.</span><span class="sxs-lookup"><span data-stu-id="2bd54-118">b.</span></span> <span data-ttu-id="2bd54-119">Välj **statisk IP-adress**, tilldela en IP-adress som inte används och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-119">Select **Static IP Address**, assign an unused IP address, and then click **OK**.</span></span>
 
   * <span data-ttu-id="2bd54-120">Så här konfigurerar du en endast Azure-lösning:</span><span class="sxs-lookup"><span data-stu-id="2bd54-120">To configure an Azure-only solution:</span></span>

        <span data-ttu-id="2bd54-121">a.</span><span class="sxs-lookup"><span data-stu-id="2bd54-121">a.</span></span> <span data-ttu-id="2bd54-122">Högerklicka på IP-adressresurs som motsvarar din Azure-undernät och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-122">Right-click the IP address resource that corresponds to your Azure subnet, and then select **Properties**.</span></span>
       
       > [!NOTE]
       > <span data-ttu-id="2bd54-123">Om lyssnaren senare misslyckas anslutas på grund av en konflikt IP-adress som valts av DHCP kan du konfigurera en statisk IP-adress i egenskapsfönstret.</span><span class="sxs-lookup"><span data-stu-id="2bd54-123">If the listener later fails to come online because of a conflicting IP address selected by DHCP, you can configure a valid static IP address in this properties window.</span></span>
       > 
       > 

       <span data-ttu-id="2bd54-124">b.</span><span class="sxs-lookup"><span data-stu-id="2bd54-124">b.</span></span> <span data-ttu-id="2bd54-125">I samma **IP-adress** i fönstret Ändra den **IP-adressnamn**.</span><span class="sxs-lookup"><span data-stu-id="2bd54-125">In the same **IP Address** properties window, change the **IP Address Name**.</span></span>  
        <span data-ttu-id="2bd54-126">Det här namnet används i variabeln $IPResourceName på PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="2bd54-126">This name is used in the $IPResourceName variable of the PowerShell script.</span></span> <span data-ttu-id="2bd54-127">Om din lösning sträcker sig över flera virtuella Azure-nätverk, kan du upprepa det här steget för varje IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="2bd54-127">If your solution spans multiple Azure virtual networks, repeat this step for each IP resource.</span></span>

