## <a name="scenario"></a><span data-ttu-id="b915c-101">Scenario</span><span class="sxs-lookup"><span data-stu-id="b915c-101">Scenario</span></span>
<span data-ttu-id="b915c-102">För att ge en bättre illustrering av hur man skapa VNet och undernät, använder det här dokumentet sig av scenariot nedan.</span><span class="sxs-lookup"><span data-stu-id="b915c-102">To better illustrate how to create a VNet and subnets, this document will use the scenario below.</span></span>

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="b915c-104">I det här scenariot skapar du ett VNet med namnet **TestVNet** med det reserverade CIDR-blocket **192.168.0.0./16**.</span><span class="sxs-lookup"><span data-stu-id="b915c-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="b915c-105">Ditt VNet kommer innehålla följande undernät:</span><span class="sxs-lookup"><span data-stu-id="b915c-105">Your VNet will contain the following subnets:</span></span> 

* <span data-ttu-id="b915c-106">**FrontEnd**, som använder **192.168.1.0/24** som sitt CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="b915c-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="b915c-107">**BackEnd**, som använder **192.168.2.0/24** som sitt CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="b915c-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

