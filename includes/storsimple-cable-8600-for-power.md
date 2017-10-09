<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a><span data-ttu-id="6fa94-101">toocable för enheten</span><span class="sxs-lookup"><span data-stu-id="6fa94-101">toocable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="6fa94-102">Båda höljen på StorSimple-enheten inkluderar redundant PCMs.</span><span class="sxs-lookup"><span data-stu-id="6fa94-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="6fa94-103">För varje höljet hello PCMs måste vara installerad och anslutna toodifferent power källor tooensure hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="6fa94-103">For each enclosure, hello PCMs must be installed and connected toodifferent power sources tooensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="6fa94-104">Kontrollera att hello strömbrytare på alla hello PCMs i hello OFF-läge.</span><span class="sxs-lookup"><span data-stu-id="6fa94-104">Make sure that hello power switches on all hello PCMs are in hello OFF position.</span></span>
2. <span data-ttu-id="6fa94-105">Ansluta hello power sladdar tooboth PCMs på primära hello-hölje.</span><span class="sxs-lookup"><span data-stu-id="6fa94-105">On hello primary enclosure, connect hello power cords tooboth PCMs.</span></span> <span data-ttu-id="6fa94-106">hello strömkablar identifieras i rött i hello power kablar diagrammet nedan.</span><span class="sxs-lookup"><span data-stu-id="6fa94-106">hello power cords are identified in red in hello power cabling diagram, below.</span></span>
3. <span data-ttu-id="6fa94-107">Kontrollera att som hello två PCMs på hello primära höljet Använd separata strömkällor.</span><span class="sxs-lookup"><span data-stu-id="6fa94-107">Make sure that hello two PCMs on hello primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="6fa94-108">Koppla hello power sladdar toohello slå på hello rack distribution enheter enligt hello power kablar diagram.</span><span class="sxs-lookup"><span data-stu-id="6fa94-108">Attach hello power cords toohello power on hello rack distribution units as shown in hello power cabling diagram.</span></span>
5. <span data-ttu-id="6fa94-109">Upprepa steg 2 till 4 för hello EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="6fa94-109">Repeat steps 2 through 4 for hello EBOD enclosure.</span></span>
6. <span data-ttu-id="6fa94-110">Aktivera hello EBOD hölje genom att vända hello strömbrytare på varje PCM toohello ON position.</span><span class="sxs-lookup"><span data-stu-id="6fa94-110">Turn on hello EBOD enclosure by flipping hello power switch on each PCM toohello ON position.</span></span>
7. <span data-ttu-id="6fa94-111">Kontrollera att hello EBOD hölje är aktiverat genom att kontrollera att hello grön indikatorer på hello baksidan hello EBOD domänkontrollant är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="6fa94-111">Verify that hello EBOD enclosure is turned on by checking that hello green LEDs on hello back of hello EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="6fa94-112">Aktivera hello primära höljet genom att vända varje PCM växeln toohello ON position.</span><span class="sxs-lookup"><span data-stu-id="6fa94-112">Turn on hello primary enclosure by flipping each PCM switch toohello ON position.</span></span>
9. <span data-ttu-id="6fa94-113">Kontrollera att hello system är på genom att säkerställa hello enhetskontroll indikatorer har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="6fa94-113">Verify that hello system is on by ensuring hello device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="6fa94-114">Se till att hello anslutningen mellan hello EBOD domänkontrollant och hello enhetskontroll är aktiva genom att verifiera att hello fyra led nästa toohello SAS-port på hello EBOD domänkontrollant är grön.</span><span class="sxs-lookup"><span data-stu-id="6fa94-114">Make sure that hello connection between hello EBOD controller and hello device controller is active by verifying that hello four LEDs next toohello SAS port on hello EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="6fa94-115">tooensure hög tillgänglighet för ditt system, rekommenderar vi strikt följa toohello power kablar schemat visas i följande diagram hello.</span><span class="sxs-lookup"><span data-stu-id="6fa94-115">tooensure high availability for your system, we recommend that you strictly adhere toohello power cabling scheme shown in hello following diagram.</span></span>
    > 
    > 
    
    ![Kabelansluta den 4U för ström](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="6fa94-117">**Kablar**</span><span class="sxs-lookup"><span data-stu-id="6fa94-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="6fa94-118">Etikett</span><span class="sxs-lookup"><span data-stu-id="6fa94-118">Label</span></span> | <span data-ttu-id="6fa94-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6fa94-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="6fa94-120">1</span><span class="sxs-lookup"><span data-stu-id="6fa94-120">1</span></span> |<span data-ttu-id="6fa94-121">Primär enhet</span><span class="sxs-lookup"><span data-stu-id="6fa94-121">Primary enclosure</span></span> |
    | <span data-ttu-id="6fa94-122">2</span><span class="sxs-lookup"><span data-stu-id="6fa94-122">2</span></span> |<span data-ttu-id="6fa94-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="6fa94-123">PCM 0</span></span> |
    | <span data-ttu-id="6fa94-124">3</span><span class="sxs-lookup"><span data-stu-id="6fa94-124">3</span></span> |<span data-ttu-id="6fa94-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="6fa94-125">PCM 1</span></span> |
    | <span data-ttu-id="6fa94-126">4</span><span class="sxs-lookup"><span data-stu-id="6fa94-126">4</span></span> |<span data-ttu-id="6fa94-127">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="6fa94-127">Controller 0</span></span> |
    | <span data-ttu-id="6fa94-128">5</span><span class="sxs-lookup"><span data-stu-id="6fa94-128">5</span></span> |<span data-ttu-id="6fa94-129">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="6fa94-129">Controller 1</span></span> |
    | <span data-ttu-id="6fa94-130">6</span><span class="sxs-lookup"><span data-stu-id="6fa94-130">6</span></span> |<span data-ttu-id="6fa94-131">EBOD styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="6fa94-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="6fa94-132">7</span><span class="sxs-lookup"><span data-stu-id="6fa94-132">7</span></span> |<span data-ttu-id="6fa94-133">EBOD kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="6fa94-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="6fa94-134">8</span><span class="sxs-lookup"><span data-stu-id="6fa94-134">8</span></span> |<span data-ttu-id="6fa94-135">EBOD hölje</span><span class="sxs-lookup"><span data-stu-id="6fa94-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="6fa94-136">9</span><span class="sxs-lookup"><span data-stu-id="6fa94-136">9</span></span> |<span data-ttu-id="6fa94-137">PDU</span><span class="sxs-lookup"><span data-stu-id="6fa94-137">PDUs</span></span> |

