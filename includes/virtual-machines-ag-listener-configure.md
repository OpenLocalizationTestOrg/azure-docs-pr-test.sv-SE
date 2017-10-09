<span data-ttu-id="71453-101">hello tillgänglighetsgruppens lyssnare är en IP-adress och namn som hello SQL Server-tillgänglighetsgrupp som lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="71453-101">hello availability group listener is an IP address and network name that hello SQL Server availability group listens on.</span></span> <span data-ttu-id="71453-102">toocreate hello tillgänglighetsgruppens lyssnare, hello följande:</span><span class="sxs-lookup"><span data-stu-id="71453-102">toocreate hello availability group listener, do hello following:</span></span>

1. <span data-ttu-id="71453-103"><a name="getnet"></a>Hämta hello namn för hello klusterresurs för nätverket.</span><span class="sxs-lookup"><span data-stu-id="71453-103"><a name="getnet"></a>Get hello name of hello cluster network resource.</span></span>

    <span data-ttu-id="71453-104">a.</span><span class="sxs-lookup"><span data-stu-id="71453-104">a.</span></span> <span data-ttu-id="71453-105">Använd RDP tooconnect toohello virtuell Azure-dator som är värd för hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="71453-105">Use RDP tooconnect toohello Azure virtual machine that hosts hello primary replica.</span></span> 

    <span data-ttu-id="71453-106">b.</span><span class="sxs-lookup"><span data-stu-id="71453-106">b.</span></span> <span data-ttu-id="71453-107">Öppna Hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="71453-107">Open Failover Cluster Manager.</span></span>

    <span data-ttu-id="71453-108">c.</span><span class="sxs-lookup"><span data-stu-id="71453-108">c.</span></span> <span data-ttu-id="71453-109">Välj hello **nätverk** nod och Observera hello klustrets nätverksnamn.</span><span class="sxs-lookup"><span data-stu-id="71453-109">Select hello **Networks** node, and note hello cluster network name.</span></span> <span data-ttu-id="71453-110">Använd det här namnet i hello `$ClusterNetworkName` variabeln i hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="71453-110">Use this name in hello `$ClusterNetworkName` variable in hello PowerShell script.</span></span> <span data-ttu-id="71453-111">Hello följande bild hello Klusternätverksnamnet är **klustret nätverk 1**:</span><span class="sxs-lookup"><span data-stu-id="71453-111">In hello following image hello cluster network name is **Cluster Network 1**:</span></span>

   ![Klustrets nätverksnamn](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <span data-ttu-id="71453-113"><a name="addcap"></a>Lägg till hello klientåtkomstpunkt.</span><span class="sxs-lookup"><span data-stu-id="71453-113"><a name="addcap"></a>Add hello client access point.</span></span>  
    <span data-ttu-id="71453-114">hello klientåtkomstpunkt är hello nätverksnamn att program använder tooconnect toohello databaser i en tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="71453-114">hello client access point is hello network name that applications use tooconnect toohello databases in an availability group.</span></span> <span data-ttu-id="71453-115">Skapa hello klientåtkomstpunkt i hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="71453-115">Create hello client access point in Failover Cluster Manager.</span></span>

    <span data-ttu-id="71453-116">a.</span><span class="sxs-lookup"><span data-stu-id="71453-116">a.</span></span> <span data-ttu-id="71453-117">Expandera klusternamnet hello och klicka sedan på **roller**.</span><span class="sxs-lookup"><span data-stu-id="71453-117">Expand hello cluster name, and then click **Roles**.</span></span>

    <span data-ttu-id="71453-118">b.</span><span class="sxs-lookup"><span data-stu-id="71453-118">b.</span></span> <span data-ttu-id="71453-119">I hello **roller** fönstret, högerklicka på hello-tillgänglighetsgrupp och sedan markera **Lägg till resurs** > **klientåtkomstpunkt**.</span><span class="sxs-lookup"><span data-stu-id="71453-119">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>

   ![Klientåtkomstpunkt](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    <span data-ttu-id="71453-121">c.</span><span class="sxs-lookup"><span data-stu-id="71453-121">c.</span></span> <span data-ttu-id="71453-122">I hello **namn** skapar du ett namn för den här lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="71453-122">In hello **Name** box, create a name for this new listener.</span></span> 
   <span data-ttu-id="71453-123">hello namnet hello lyssnaren är hello nätverksnamn att program använder tooconnect toodatabases i hello SQL Server-tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="71453-123">hello name for hello new listener is hello network name that applications use tooconnect toodatabases in hello SQL Server availability group.</span></span>
   
    <span data-ttu-id="71453-124">d.</span><span class="sxs-lookup"><span data-stu-id="71453-124">d.</span></span> <span data-ttu-id="71453-125">toofinish skapar hello lyssnare, klickar du på **nästa** två gånger, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="71453-125">toofinish creating hello listener, click **Next** twice, and then click **Finish**.</span></span> <span data-ttu-id="71453-126">Tar inte med hello lyssnare eller resursen i det här läget.</span><span class="sxs-lookup"><span data-stu-id="71453-126">Do not bring hello listener or resource online at this point.</span></span>

3. <span data-ttu-id="71453-127"><a name="congroup"></a>Konfigurera hello IP-resurs för hello-tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="71453-127"><a name="congroup"></a>Configure hello IP resource for hello availability group.</span></span>

    <span data-ttu-id="71453-128">a.</span><span class="sxs-lookup"><span data-stu-id="71453-128">a.</span></span> <span data-ttu-id="71453-129">Klicka på hello **resurser** fliken och expandera sedan hello klientåtkomstpunkt som du skapade.</span><span class="sxs-lookup"><span data-stu-id="71453-129">Click hello **Resources** tab, and then expand hello client access point you created.</span></span>  
    <span data-ttu-id="71453-130">hello klientåtkomstpunkt är offline.</span><span class="sxs-lookup"><span data-stu-id="71453-130">hello client access point is offline.</span></span>

   ![Klientåtkomstpunkt](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    <span data-ttu-id="71453-132">b.</span><span class="sxs-lookup"><span data-stu-id="71453-132">b.</span></span> <span data-ttu-id="71453-133">Högerklicka på hello IP-resurs och klicka sedan på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="71453-133">Right-click hello IP resource, and then click properties.</span></span> <span data-ttu-id="71453-134">Anteckna namnet på hello hello IP-adress och använda det i hello `$IPResourceName` variabeln i hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="71453-134">Note hello name of hello IP address, and use it in hello `$IPResourceName` variable in hello PowerShell script.</span></span>

    <span data-ttu-id="71453-135">c.</span><span class="sxs-lookup"><span data-stu-id="71453-135">c.</span></span> <span data-ttu-id="71453-136">Under **IP-adress**, klickar du på **statisk IP-adress**.</span><span class="sxs-lookup"><span data-stu-id="71453-136">Under **IP Address**, click **Static IP Address**.</span></span> <span data-ttu-id="71453-137">Ange hello IP-adress som hello samma adress som du använde när du har ställt in hello-adressen för belastningsutjämnaren på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="71453-137">Set hello IP address as hello same address that you used when you set hello load balancer address on hello Azure portal.</span></span>

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <span data-ttu-id="71453-139"><a name = "dependencyGroup"></a>Gör hello SQL Server tillgänglighetsgruppsresursen beroende hello klientåtkomstpunkt.</span><span class="sxs-lookup"><span data-stu-id="71453-139"><a name = "dependencyGroup"></a>Make hello SQL Server availability group resource dependent on hello client access point.</span></span>

    <span data-ttu-id="71453-140">a.</span><span class="sxs-lookup"><span data-stu-id="71453-140">a.</span></span> <span data-ttu-id="71453-141">I hanteraren för redundanskluster, klickar du på **roller**, och klicka sedan på tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="71453-141">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span>

    <span data-ttu-id="71453-142">b.</span><span class="sxs-lookup"><span data-stu-id="71453-142">b.</span></span> <span data-ttu-id="71453-143">På hello **resurser** fliken, under **andra resurser**högerklickar du på hello tillgänglighet resursgruppen och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="71453-143">On hello **Resources** tab, under **Other Resources**, right-click hello availability resource group, and then click **Properties**.</span></span> 

    <span data-ttu-id="71453-144">c.</span><span class="sxs-lookup"><span data-stu-id="71453-144">c.</span></span> <span data-ttu-id="71453-145">Lägg till hello namnet på hello klienten åtkomst punkt (hello lyssnare) resursen hello beroenden på fliken.</span><span class="sxs-lookup"><span data-stu-id="71453-145">On hello dependencies tab, add hello name of hello client access point (hello listener) resource.</span></span>

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    <span data-ttu-id="71453-147">d.</span><span class="sxs-lookup"><span data-stu-id="71453-147">d.</span></span> <span data-ttu-id="71453-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="71453-148">Click **OK**.</span></span>

5. <span data-ttu-id="71453-149"><a name="listname"></a>Se hello klientåtkomst peka resurs som är beroende av hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71453-149"><a name="listname"></a>Make hello client access point resource dependent on hello IP address.</span></span>

    <span data-ttu-id="71453-150">a.</span><span class="sxs-lookup"><span data-stu-id="71453-150">a.</span></span> <span data-ttu-id="71453-151">I hanteraren för redundanskluster, klickar du på **roller**, och klicka sedan på tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="71453-151">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span> 

    <span data-ttu-id="71453-152">b.</span><span class="sxs-lookup"><span data-stu-id="71453-152">b.</span></span> <span data-ttu-id="71453-153">På hello **resurser** , högerklickar på hello klienten åtkomst punkt resurs under **servernamn**, och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="71453-153">On hello **Resources** tab, right-click hello client access point resource under **Server Name**, and then click **Properties**.</span></span> 

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    <span data-ttu-id="71453-155">c.</span><span class="sxs-lookup"><span data-stu-id="71453-155">c.</span></span> <span data-ttu-id="71453-156">Klicka på hello **beroenden** fliken. Kontrollera att hello IP-adress är ett beroende.</span><span class="sxs-lookup"><span data-stu-id="71453-156">Click hello **Dependencies** tab. Verify that hello IP address is a dependency.</span></span> <span data-ttu-id="71453-157">Om det inte ange ett beroende på hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71453-157">If it is not, set a dependency on hello IP address.</span></span> <span data-ttu-id="71453-158">Om det finns flera resurser i listan, kontrollera att hello IP-adresser har eller inte och beroenden.</span><span class="sxs-lookup"><span data-stu-id="71453-158">If there are multiple resources listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span> <span data-ttu-id="71453-159">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="71453-159">Click **OK**.</span></span> 

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    <span data-ttu-id="71453-161">d.</span><span class="sxs-lookup"><span data-stu-id="71453-161">d.</span></span> <span data-ttu-id="71453-162">Högerklicka på hello grupplyssnarens namn och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="71453-162">Right-click hello listener name, and then click **Bring Online**.</span></span> 

    >[!TIP]
    ><span data-ttu-id="71453-163">Du kan verifiera att hello beroenden är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="71453-163">You can validate that hello dependencies are correctly configured.</span></span> <span data-ttu-id="71453-164">I hanteraren för redundanskluster, gå tooRoles, högerklicka på hello tillgänglighetsgruppen, klickar du på **fler åtgärder**, och klicka sedan på **visa beroenderapport**.</span><span class="sxs-lookup"><span data-stu-id="71453-164">In Failover Cluster Manager, go tooRoles, right-click hello availability group, click **More Actions**, and then click  **Show Dependency Report**.</span></span> <span data-ttu-id="71453-165">När hello beroenden är korrekt konfigurerade hello tillgänglighetsgruppen är beroende av hello nätverksnamnet och hello nätverksnamn är beroende av hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71453-165">When hello dependencies are correctly configured, hello availability group is dependent on hello network name, and hello network name is dependent on hello IP address.</span></span> 


6. <span data-ttu-id="71453-166"><a name="setparam"></a>Ange hello Klusterparametrar i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="71453-166"><a name="setparam"></a>Set hello cluster parameters in PowerShell.</span></span>
    
    <span data-ttu-id="71453-167">a.</span><span class="sxs-lookup"><span data-stu-id="71453-167">a.</span></span> <span data-ttu-id="71453-168">Kopiera hello följande PowerShell-skriptet tooone av SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="71453-168">Copy hello following PowerShell script tooone of your SQL Server instances.</span></span> <span data-ttu-id="71453-169">Uppdatera hello variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="71453-169">Update hello variables for your environment.</span></span>     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    <span data-ttu-id="71453-170">b.</span><span class="sxs-lookup"><span data-stu-id="71453-170">b.</span></span> <span data-ttu-id="71453-171">Ange hello Klusterparametrar genom att köra hello PowerShell-skript på en av klusternoderna hello.</span><span class="sxs-lookup"><span data-stu-id="71453-171">Set hello cluster parameters by running hello PowerShell script on one of hello cluster nodes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="71453-172">Om SQL Server-instanser i olika regioner, måste toorun hello PowerShell-skriptet två gånger.</span><span class="sxs-lookup"><span data-stu-id="71453-172">If your SQL Server instances are in separate regions, you need toorun hello PowerShell script twice.</span></span> <span data-ttu-id="71453-173">Hej första gången, använda hello `$ILBIP` och `$ProbePort` från hello första region.</span><span class="sxs-lookup"><span data-stu-id="71453-173">hello first time, use hello `$ILBIP` and `$ProbePort` from hello first region.</span></span> <span data-ttu-id="71453-174">Hej gång, Använd hello `$ILBIP` och `$ProbePort` från andra hello-region.</span><span class="sxs-lookup"><span data-stu-id="71453-174">hello second time, use hello `$ILBIP` and `$ProbePort` from hello second region.</span></span> <span data-ttu-id="71453-175">hello Klusternätverksnamnet och hello klusternamnet IP-resurs är hello samma.</span><span class="sxs-lookup"><span data-stu-id="71453-175">hello cluster network name and hello cluster IP resource name are hello same.</span></span> 
