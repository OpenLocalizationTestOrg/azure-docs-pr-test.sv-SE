#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a><span data-ttu-id="c851e-101">toocreate offentliga slutpunkter på hello moln-enhet</span><span class="sxs-lookup"><span data-stu-id="c851e-101">toocreate public endpoints on hello cloud appliance</span></span>

1. <span data-ttu-id="c851e-102">Logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c851e-102">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="c851e-103">Gå för**virtuella datorer**, markera och klickar på hello virtuell dator som används som din enhet för molnet.</span><span class="sxs-lookup"><span data-stu-id="c851e-103">Go too**Virtual Machines**, and then select and click hello virtual machine that is being used as your cloud appliance.</span></span>
    
3. <span data-ttu-id="c851e-104">Du måste toocreate en network security group (NSG) regel toocontrol hello trafikflödet och från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c851e-104">You need toocreate a network security group (NSG) rule toocontrol hello flow of traffic in and out of your virtual machine.</span></span> <span data-ttu-id="c851e-105">Utföra hello följande steg toocreate en NSG-regel.</span><span class="sxs-lookup"><span data-stu-id="c851e-105">Perform hello following steps toocreate an NSG rule.</span></span>
    1. <span data-ttu-id="c851e-106">Välj **Nätverkssäkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="c851e-106">Select **Network security group**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. <span data-ttu-id="c851e-107">Klicka på hello standard nätverkssäkerhetsgruppen som visas.</span><span class="sxs-lookup"><span data-stu-id="c851e-107">Click hello default network security group that is presented.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. <span data-ttu-id="c851e-108">Välj **Inkommande säkerhetsregel**.</span><span class="sxs-lookup"><span data-stu-id="c851e-108">Select **Inbound security rules**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. <span data-ttu-id="c851e-109">Klicka på **+ Lägg till** toocreate en ingående säkerhetsregel.</span><span class="sxs-lookup"><span data-stu-id="c851e-109">Click **+ Add** toocreate an inbound security rule.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        <span data-ttu-id="c851e-110">I bladet för hello Lägg till inkommande säkerhetsregler regel:</span><span class="sxs-lookup"><span data-stu-id="c851e-110">In hello Add inbound security rule blade:</span></span>

        1. <span data-ttu-id="c851e-111">För hello **namn**, typen hello följande namn för hello slutpunkt: WinRMHttps.</span><span class="sxs-lookup"><span data-stu-id="c851e-111">For hello **Name**, type hello following name for hello endpoint: WinRMHttps.</span></span>
        
        2. <span data-ttu-id="c851e-112">För hello **prioritet**väljer ett tal som är mindre än 1000 och (vilket är hello prioritet för hello standardregel).</span><span class="sxs-lookup"><span data-stu-id="c851e-112">For hello **Priority**, select a number lesser than 1000 (which is hello priority for hello default rule).</span></span> <span data-ttu-id="c851e-113">Högre hello värde i lägre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="c851e-113">Higher hello value, lower hello priority.</span></span>

        3. <span data-ttu-id="c851e-114">Ange hello **källa** för**alla**.</span><span class="sxs-lookup"><span data-stu-id="c851e-114">Set hello **Source** too**Any**.</span></span>

        4. <span data-ttu-id="c851e-115">För hello **Service**väljer **WinRM**.</span><span class="sxs-lookup"><span data-stu-id="c851e-115">For hello **Service**, select **WinRM**.</span></span> <span data-ttu-id="c851e-116">Hej **protokollet** anges automatiskt för**TCP** och hello **portintervall** har angetts för**5986**.</span><span class="sxs-lookup"><span data-stu-id="c851e-116">hello **Protocol** is automatically set too**TCP** and hello **Port range** is set too**5986**.</span></span>

        5. <span data-ttu-id="c851e-117">Klicka på **OK** toocreate hello regeln.</span><span class="sxs-lookup"><span data-stu-id="c851e-117">Click **OK** toocreate hello rule.</span></span>

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. <span data-ttu-id="c851e-118">Det sista steget är tooassociate nätverkssäkerheten gruppen med ett undernät eller ett visst nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c851e-118">Your final step is tooassociate your network security group with a subnet or a specific network interface.</span></span> <span data-ttu-id="c851e-119">Utför följande steg tooassociate hello säkerhetsgrupp för nätverk med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="c851e-119">Perform hello following steps tooassociate your network security group with a subnet.</span></span>
    1. <span data-ttu-id="c851e-120">Gå för**undernät**.</span><span class="sxs-lookup"><span data-stu-id="c851e-120">Go too**Subnets**.</span></span>
    2. <span data-ttu-id="c851e-121">Klicka på **+ Associera**.</span><span class="sxs-lookup"><span data-stu-id="c851e-121">Click **+ Associate**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. <span data-ttu-id="c851e-122">Välj det virtuella nätverket och välj sedan hello rätt undernät.</span><span class="sxs-lookup"><span data-stu-id="c851e-122">Select your virtual network, and then select hello appropriate subnet.</span></span>
    4. <span data-ttu-id="c851e-123">Klicka på **OK** toocreate hello regeln.</span><span class="sxs-lookup"><span data-stu-id="c851e-123">Click **OK** toocreate hello rule.</span></span>

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

<span data-ttu-id="c851e-124">När hello regeln har skapats kan visa du dess information toodetermine hello offentliga virtuella IP-adresser (VIP)-adress.</span><span class="sxs-lookup"><span data-stu-id="c851e-124">After hello rule is created, you can view its details toodetermine hello Public Virtual IP (VIP) address.</span></span> <span data-ttu-id="c851e-125">Anteckna den adressen.</span><span class="sxs-lookup"><span data-stu-id="c851e-125">Record this address.</span></span>


