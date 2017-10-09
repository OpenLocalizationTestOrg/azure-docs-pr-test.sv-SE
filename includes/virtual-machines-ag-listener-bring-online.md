1. <span data-ttu-id="09f26-101">I hanteraren för redundanskluster, expandera **roller**, och sedan markera tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="09f26-101">In Failover Cluster Manager, expand **Roles**, and then highlight your availability group.</span></span>  

2. <span data-ttu-id="09f26-102">På hello **resurser** fliken, högerklicka på hello grupplyssnarens namn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="09f26-102">On hello **Resources** tab, right-click hello listener name, and then click **Properties**.</span></span>

3. <span data-ttu-id="09f26-103">Klicka på hello **beroenden** fliken. Om flera resurser visas, kontrollerar du att hello IP-adresser har eller inte och beroenden.</span><span class="sxs-lookup"><span data-stu-id="09f26-103">Click hello **Dependencies** tab. If multiple resources are listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span>  

4. <span data-ttu-id="09f26-104">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f26-104">Click **OK**.</span></span>

5. <span data-ttu-id="09f26-105">Högerklicka på hello grupplyssnarens namn och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="09f26-105">Right-click hello listener name, and then click **Bring Online**.</span></span>

6. <span data-ttu-id="09f26-106">Efter hello lyssnare är online på hello **resurser** fliken, högerklicka på hello tillgänglighetsgruppen och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="09f26-106">After hello listener is online, on hello **Resources** tab, right-click hello availability group, and then click **Properties**.</span></span>
   
    ![Konfigurera hello tillgänglighetsgruppsresursen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. <span data-ttu-id="09f26-108">Skapa ett beroende på hello lyssnare resurs (inte hello IP-adress resurser namn) och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f26-108">Create a dependency on hello listener name resource (not hello IP address resources name), and then click **OK**.</span></span>
   
    ![Lägg till beroende på hello grupplyssnarens namn](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. <span data-ttu-id="09f26-110">Starta SQL Server Management Studio och Anslut toohello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="09f26-110">Start SQL Server Management Studio, and then connect toohello primary replica.</span></span>

9. <span data-ttu-id="09f26-111">Gå för**AlwaysOn hög tillgänglighet** > **Tillgänglighetsgrupper** > **\<AvailabilityGroupName\>**   >  **Tillgänglighetsgruppens lyssnare**.</span><span class="sxs-lookup"><span data-stu-id="09f26-111">Go too**AlwaysOn High Availability** > **Availability Groups** > **\<AvailabilityGroupName\>** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="09f26-112">Hej grupplyssnarens namn som du skapade i hanteraren för redundanskluster ska visas.</span><span class="sxs-lookup"><span data-stu-id="09f26-112">hello listener name that you created in Failover Cluster Manager should be displayed.</span></span>

10. <span data-ttu-id="09f26-113">Högerklicka på hello grupplyssnarens namn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="09f26-113">Right-click hello listener name, and then click **Properties**.</span></span>

11. <span data-ttu-id="09f26-114">I hello **Port** ange hello portnummer för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort som du använde tidigare (i den här självstudiekursen var 1433 hello standard), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f26-114">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort that you used earlier (in this tutorial, 1433 was hello default), and then click **OK**.</span></span>

