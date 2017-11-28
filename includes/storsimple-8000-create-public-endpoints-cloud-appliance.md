#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a><span data-ttu-id="f685b-101">Så här skapar du offentliga slutpunkter för molninstallationen</span><span class="sxs-lookup"><span data-stu-id="f685b-101">To create public endpoints on the cloud appliance</span></span>

1. <span data-ttu-id="f685b-102">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f685b-102">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="f685b-103">Gå till **Virtual Machines** och markera och klicka på den virtuella dator som används som molninstallation.</span><span class="sxs-lookup"><span data-stu-id="f685b-103">Go to **Virtual Machines**, and then select and click the virtual machine that is being used as your cloud appliance.</span></span>
    
3. <span data-ttu-id="f685b-104">Du måste skapa en NSG-regel (nätverkssäkerhetsgrupp) för att styra flödet av trafik in till och ut från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f685b-104">You need to create a network security group (NSG) rule to control the flow of traffic in and out of your virtual machine.</span></span> <span data-ttu-id="f685b-105">Skapa en NSG-regel genom att utföra stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="f685b-105">Perform the following steps to create an NSG rule.</span></span>
    1. <span data-ttu-id="f685b-106">Välj **Nätverkssäkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="f685b-106">Select **Network security group**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. <span data-ttu-id="f685b-107">Klicka på standardnätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="f685b-107">Click the default network security group that is presented.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. <span data-ttu-id="f685b-108">Välj **Inkommande säkerhetsregel**.</span><span class="sxs-lookup"><span data-stu-id="f685b-108">Select **Inbound security rules**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. <span data-ttu-id="f685b-109">Skapa en säkerhetsregel för inkommande trafik genom att klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f685b-109">Click **+ Add** to create an inbound security rule.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        <span data-ttu-id="f685b-110">På bladet Lägg till inkommande säkerhetsregel:</span><span class="sxs-lookup"><span data-stu-id="f685b-110">In the Add inbound security rule blade:</span></span>

        1. <span data-ttu-id="f685b-111">I fältet **Namn** anger du följande namn för slutpunkten: WinRMHttps.</span><span class="sxs-lookup"><span data-stu-id="f685b-111">For the **Name**, type the following name for the endpoint: WinRMHttps.</span></span>
        
        2. <span data-ttu-id="f685b-112">För **Prioritet** väljer du ett tal som är mindre än 1 000 (som är prioriteten för standardregeln).</span><span class="sxs-lookup"><span data-stu-id="f685b-112">For the **Priority**, select a number lesser than 1000 (which is the priority for the default rule).</span></span> <span data-ttu-id="f685b-113">Desto högre värde, ju lägre prioritet.</span><span class="sxs-lookup"><span data-stu-id="f685b-113">Higher the value, lower the priority.</span></span>

        3. <span data-ttu-id="f685b-114">Ange **Källa** till **Alla**.</span><span class="sxs-lookup"><span data-stu-id="f685b-114">Set the **Source** to **Any**.</span></span>

        4. <span data-ttu-id="f685b-115">För **Tjänst** väljer du **WinRM**.</span><span class="sxs-lookup"><span data-stu-id="f685b-115">For the **Service**, select **WinRM**.</span></span> <span data-ttu-id="f685b-116">**Protokoll** ställs automatiskt in till **TCP**, och **Portintervall** till **5986**.</span><span class="sxs-lookup"><span data-stu-id="f685b-116">The **Protocol** is automatically set to **TCP** and the **Port range** is set to **5986**.</span></span>

        5. <span data-ttu-id="f685b-117">Skapa regeln genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f685b-117">Click **OK** to create the rule.</span></span>

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. <span data-ttu-id="f685b-118">Det sista steget är att associera nätverkssäkerhetsgruppen med ett undernät eller ett visst nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f685b-118">Your final step is to associate your network security group with a subnet or a specific network interface.</span></span> <span data-ttu-id="f685b-119">Du associerar nätverkssäkerhetsgruppen med ett undernät genom att utföra stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="f685b-119">Perform the following steps to associate your network security group with a subnet.</span></span>
    1. <span data-ttu-id="f685b-120">Gå till **Undernät**.</span><span class="sxs-lookup"><span data-stu-id="f685b-120">Go to **Subnets**.</span></span>
    2. <span data-ttu-id="f685b-121">Klicka på **+ Associera**.</span><span class="sxs-lookup"><span data-stu-id="f685b-121">Click **+ Associate**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. <span data-ttu-id="f685b-122">Välj ditt virtuella nätverk och välj sedan lämpligt undernät.</span><span class="sxs-lookup"><span data-stu-id="f685b-122">Select your virtual network, and then select the appropriate subnet.</span></span>
    4. <span data-ttu-id="f685b-123">Skapa regeln genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f685b-123">Click **OK** to create the rule.</span></span>

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

<span data-ttu-id="f685b-124">När regeln har skapats kan du visa information om regeln för att ta reda på den offentliga virtuella IP-adressen (VIP).</span><span class="sxs-lookup"><span data-stu-id="f685b-124">After the rule is created, you can view its details to determine the Public Virtual IP (VIP) address.</span></span> <span data-ttu-id="f685b-125">Anteckna den adressen.</span><span class="sxs-lookup"><span data-stu-id="f685b-125">Record this address.</span></span>


