<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a><span data-ttu-id="8b3a6-101">Att kabelanslut din enhet för ström</span><span class="sxs-lookup"><span data-stu-id="8b3a6-101">To cable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="8b3a6-102">Båda höljen på StorSimple-enheten inkluderar redundant PCMs.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="8b3a6-103">PCMs måste installeras och ansluten till olika strömkällor att säkerställa hög tillgänglighet för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-103">For each enclosure, the PCMs must be installed and connected to different power sources to ensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="8b3a6-104">Kontrollera att strömbrytare på alla PCMs i OFF-läge.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-104">Make sure that the power switches on all the PCMs are in the OFF position.</span></span>
2. <span data-ttu-id="8b3a6-105">Anslut strömkablar till båda PCMs från primära höljet.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-105">On the primary enclosure, connect the power cords to both PCMs.</span></span> <span data-ttu-id="8b3a6-106">Strömkablar identifieras i rött i power går diagrammet nedan.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-106">The power cords are identified in red in the power cabling diagram, below.</span></span>
3. <span data-ttu-id="8b3a6-107">Kontrollera att två PCMs på primära enheten använder separata strömkällor.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-107">Make sure that the two PCMs on the primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="8b3a6-108">Bifoga strömkablar slå på rack distribution enheter som visas i power kablar diagram.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-108">Attach the power cords to the power on the rack distribution units as shown in the power cabling diagram.</span></span>
5. <span data-ttu-id="8b3a6-109">Upprepa steg 2 till 4 för EBOD höljet.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-109">Repeat steps 2 through 4 for the EBOD enclosure.</span></span>
6. <span data-ttu-id="8b3a6-110">Aktivera EBOD höljet genom att vända på strömknappen på varje PCM till på plats.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-110">Turn on the EBOD enclosure by flipping the power switch on each PCM to the ON position.</span></span>
7. <span data-ttu-id="8b3a6-111">Kontrollera att EBOD höljet är aktiverat genom att kontrollera att grön indikatorer på baksidan av EBOD domänkontrollant är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-111">Verify that the EBOD enclosure is turned on by checking that the green LEDs on the back of the EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="8b3a6-112">Aktivera primära höljet genom att vända varje PCM växeln till på plats.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-112">Turn on the primary enclosure by flipping each PCM switch to the ON position.</span></span>
9. <span data-ttu-id="8b3a6-113">Kontrollera att systemet är på genom att säkerställa enhetskontroll indikatorer har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-113">Verify that the system is on by ensuring the device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="8b3a6-114">Kontrollera att anslutningen mellan EBOD domänkontrollanten och enhetskontroll är aktiva genom att verifiera att fyra led bredvid SAS-port på EBOD domänkontrollant är grön.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-114">Make sure that the connection between the EBOD controller and the device controller is active by verifying that the four LEDs next to the SAS port on the EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="8b3a6-115">För att säkerställa hög tillgänglighet för ditt system, rekommenderar vi att du noggrant följer power kablar schemat visas i följande diagram.</span><span class="sxs-lookup"><span data-stu-id="8b3a6-115">To ensure high availability for your system, we recommend that you strictly adhere to the power cabling scheme shown in the following diagram.</span></span>
    > 
    > 
    
    ![Kabelansluta den 4U för ström](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="8b3a6-117">**Kablar**</span><span class="sxs-lookup"><span data-stu-id="8b3a6-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="8b3a6-118">Etikett</span><span class="sxs-lookup"><span data-stu-id="8b3a6-118">Label</span></span> | <span data-ttu-id="8b3a6-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8b3a6-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="8b3a6-120">1</span><span class="sxs-lookup"><span data-stu-id="8b3a6-120">1</span></span> |<span data-ttu-id="8b3a6-121">Primär enhet</span><span class="sxs-lookup"><span data-stu-id="8b3a6-121">Primary enclosure</span></span> |
    | <span data-ttu-id="8b3a6-122">2</span><span class="sxs-lookup"><span data-stu-id="8b3a6-122">2</span></span> |<span data-ttu-id="8b3a6-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="8b3a6-123">PCM 0</span></span> |
    | <span data-ttu-id="8b3a6-124">3</span><span class="sxs-lookup"><span data-stu-id="8b3a6-124">3</span></span> |<span data-ttu-id="8b3a6-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="8b3a6-125">PCM 1</span></span> |
    | <span data-ttu-id="8b3a6-126">4</span><span class="sxs-lookup"><span data-stu-id="8b3a6-126">4</span></span> |<span data-ttu-id="8b3a6-127">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="8b3a6-127">Controller 0</span></span> |
    | <span data-ttu-id="8b3a6-128">5</span><span class="sxs-lookup"><span data-stu-id="8b3a6-128">5</span></span> |<span data-ttu-id="8b3a6-129">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="8b3a6-129">Controller 1</span></span> |
    | <span data-ttu-id="8b3a6-130">6</span><span class="sxs-lookup"><span data-stu-id="8b3a6-130">6</span></span> |<span data-ttu-id="8b3a6-131">EBOD styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="8b3a6-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="8b3a6-132">7</span><span class="sxs-lookup"><span data-stu-id="8b3a6-132">7</span></span> |<span data-ttu-id="8b3a6-133">EBOD kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="8b3a6-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="8b3a6-134">8</span><span class="sxs-lookup"><span data-stu-id="8b3a6-134">8</span></span> |<span data-ttu-id="8b3a6-135">EBOD hölje</span><span class="sxs-lookup"><span data-stu-id="8b3a6-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="8b3a6-136">9</span><span class="sxs-lookup"><span data-stu-id="8b3a6-136">9</span></span> |<span data-ttu-id="8b3a6-137">PDU</span><span class="sxs-lookup"><span data-stu-id="8b3a6-137">PDUs</span></span> |

