## <a name="scenario"></a><span data-ttu-id="6f0bf-101">Scenario</span><span class="sxs-lookup"><span data-stu-id="6f0bf-101">Scenario</span></span>
<span data-ttu-id="6f0bf-102">toobetter illustrerar hur toocreate ett VNet och undernät, det här dokumentet använder hello scenariot nedan.</span><span class="sxs-lookup"><span data-stu-id="6f0bf-102">toobetter illustrate how toocreate a VNet and subnets, this document will use hello scenario below.</span></span>

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="6f0bf-104">I det här scenariot skapar du ett VNet med namnet **TestVNet** med det reserverade CIDR-blocket **192.168.0.0./16**.</span><span class="sxs-lookup"><span data-stu-id="6f0bf-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="6f0bf-105">Ditt VNet kommer innehålla hello följande undernät:</span><span class="sxs-lookup"><span data-stu-id="6f0bf-105">Your VNet will contain hello following subnets:</span></span> 

* <span data-ttu-id="6f0bf-106">**FrontEnd**, som använder **192.168.1.0/24** som sitt CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="6f0bf-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="6f0bf-107">**BackEnd**, som använder **192.168.2.0/24** som sitt CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="6f0bf-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

