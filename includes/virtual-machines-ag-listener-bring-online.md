1. <span data-ttu-id="b85f2-101">I hanteraren för redundanskluster, expandera **roller**, och sedan markera tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="b85f2-101">In Failover Cluster Manager, expand **Roles**, and then highlight your availability group.</span></span>  

2. <span data-ttu-id="b85f2-102">På den **resurser** fliken, högerklicka på grupplyssnarens namn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-102">On the **Resources** tab, right-click the listener name, and then click **Properties**.</span></span>

3. <span data-ttu-id="b85f2-103">Klicka på den **beroenden** fliken.</span><span class="sxs-lookup"><span data-stu-id="b85f2-103">Click the **Dependencies** tab.</span></span> <span data-ttu-id="b85f2-104">Om flera resurser visas, kontrollera att IP-adresser har eller inte och beroenden.</span><span class="sxs-lookup"><span data-stu-id="b85f2-104">If multiple resources are listed, verify that the IP addresses have OR, not AND, dependencies.</span></span>  

4. <span data-ttu-id="b85f2-105">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-105">Click **OK**.</span></span>

5. <span data-ttu-id="b85f2-106">Högerklicka på grupplyssnarens namn och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-106">Right-click the listener name, and then click **Bring Online**.</span></span>

6. <span data-ttu-id="b85f2-107">När lyssnaren är online på den **resurser** fliken, högerklicka på tillgänglighetsgruppen och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-107">After the listener is online, on the **Resources** tab, right-click the availability group, and then click **Properties**.</span></span>
   
    ![Konfigurera tillgänglighetsgruppresursen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. <span data-ttu-id="b85f2-109">Skapa ett beroende på resursen lyssnare namn (inte IP-adress resurser namnet) och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-109">Create a dependency on the listener name resource (not the IP address resources name), and then click **OK**.</span></span>
   
    ![Lägg till beroende på grupplyssnarens namn](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. <span data-ttu-id="b85f2-111">Starta SQL Server Management Studio och Anslut till den primära repliken.</span><span class="sxs-lookup"><span data-stu-id="b85f2-111">Start SQL Server Management Studio, and then connect to the primary replica.</span></span>

9. <span data-ttu-id="b85f2-112">Gå till **AlwaysOn hög tillgänglighet** > **Tillgänglighetsgrupper** > **\<AvailabilityGroupName\>**   >  **Tillgänglighetsgruppens lyssnare**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-112">Go to **AlwaysOn High Availability** > **Availability Groups** > **\<AvailabilityGroupName\>** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="b85f2-113">Lyssnare namnet som du skapade i hanteraren för redundanskluster ska visas.</span><span class="sxs-lookup"><span data-stu-id="b85f2-113">The listener name that you created in Failover Cluster Manager should be displayed.</span></span>

10. <span data-ttu-id="b85f2-114">Högerklicka på grupplyssnarens namn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-114">Right-click the listener name, and then click **Properties**.</span></span>

11. <span data-ttu-id="b85f2-115">I den **Port** anger du portnumret för tillgänglighetsgruppens lyssnare med hjälp av $EndpointPort som du använde tidigare (1433 var standard i den här självstudiekursen), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b85f2-115">In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort that you used earlier (in this tutorial, 1433 was the default), and then click **OK**.</span></span>

