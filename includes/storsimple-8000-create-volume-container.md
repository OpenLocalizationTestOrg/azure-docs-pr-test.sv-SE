<!--author=alkohli last changed: 06/22/17-->

#### <a name="toocreate-a-volume-container"></a><span data-ttu-id="d5c57-101">toocreate en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="d5c57-101">toocreate a volume container</span></span>
1. <span data-ttu-id="d5c57-102">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="d5c57-102">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="d5c57-103">Välj hello tabular lista över hello enheter, och klicka på en enhet.</span><span class="sxs-lookup"><span data-stu-id="d5c57-103">From hello tabular listing of hello devices, select and click a device.</span></span> 

    ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. <span data-ttu-id="d5c57-105">I hello enheten instrumentpanelen, klickar du på **+ Lägg till volymbehållare**</span><span class="sxs-lookup"><span data-stu-id="d5c57-105">In hello device dashboard, click **+ Add volume container**</span></span>

    ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. <span data-ttu-id="d5c57-107">I hello **Lägg till volymbehållare** bladet:</span><span class="sxs-lookup"><span data-stu-id="d5c57-107">In hello **Add volume container** blade:</span></span>
   
   1. <span data-ttu-id="d5c57-108">hello enhet väljs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d5c57-108">hello device is automatically selected.</span></span>
   2. <span data-ttu-id="d5c57-109">Ange ett **namn** för din volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="d5c57-109">Supply a **Name** for your volume container.</span></span> <span data-ttu-id="d5c57-110">hello namn måste vara 3 too32 tecken.</span><span class="sxs-lookup"><span data-stu-id="d5c57-110">hello name must be 3 too32 characters long.</span></span> <span data-ttu-id="d5c57-111">Du kan byta namn på en volymbehållare när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="d5c57-111">You cannot rename a volume container once it is created.</span></span>
   3. <span data-ttu-id="d5c57-112">Välj **aktivera molnet Lagringskryptering** tooenable kryptering av hello data som skickas från hello enhet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="d5c57-112">Select **Enable Cloud Storage Encryption** tooenable encryption of hello data sent from hello device toohello cloud.</span></span>
   4. <span data-ttu-id="d5c57-113">Ange och bekräfta en **krypteringsnyckel för Molnlagring** som är 8 too32 tecken.</span><span class="sxs-lookup"><span data-stu-id="d5c57-113">Provide and confirm a **Cloud Storage Encryption Key** that is 8 too32 characters long.</span></span> <span data-ttu-id="d5c57-114">Den här nyckeln används av hello tooaccess krypterade data på enheten.</span><span class="sxs-lookup"><span data-stu-id="d5c57-114">This key is used by hello device tooaccess encrypted data.</span></span>
   5. <span data-ttu-id="d5c57-115">Välj en **Lagringskonto** tooassociate med volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="d5c57-115">Select a **Storage Account** tooassociate with this volume container.</span></span> <span data-ttu-id="d5c57-116">Du kan välja ett befintligt lagringskonto eller hello standardkonto som genereras när hello dags att skapa en tjänst.</span><span class="sxs-lookup"><span data-stu-id="d5c57-116">You can choose an existing storage account or hello default account that is generated at hello time of service creation.</span></span> <span data-ttu-id="d5c57-117">Du kan också använda hello **Lägg till ny** alternativet toospecify ett lagringskonto som inte är länkad toothis prenumeration på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5c57-117">You can also use hello **Add new** option toospecify a storage account that is not linked toothis service subscription.</span></span>
   6. <span data-ttu-id="d5c57-118">Välj **obegränsad** i hello **ange bandbredd** nedrullningsbara listan om du inte vill tooconsume all hello tillgänglig bandbredd.</span><span class="sxs-lookup"><span data-stu-id="d5c57-118">Select **Unlimited** in hello **Specify bandwidth** drop-down list if you wish tooconsume all hello available bandwidth.</span></span> <span data-ttu-id="d5c57-119">Du kan också ange det här alternativet för**anpassad** tooemploy bandbreddskontroller, och ange ett värde mellan 1 och 1 000 Mbps.</span><span class="sxs-lookup"><span data-stu-id="d5c57-119">You can also set this option too**Custom** tooemploy bandwidth controls, and specify a value between 1 Mbps and 1,000 Mbps.</span></span>
      <span data-ttu-id="d5c57-120">Om du har din bandbredd användningsinformation som är tillgängliga kan du kanske kan tooallocate bandbredd enligt ett schema genom att ange **Välj en bandbreddsmall**.</span><span class="sxs-lookup"><span data-stu-id="d5c57-120">If you have your bandwidth usage information available, you may be able tooallocate bandwidth based on a schedule by specifying **Select a bandwidth template**.</span></span> <span data-ttu-id="d5c57-121">Stegvisa anvisningar finns för[Lägg till en bandbreddsmall](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span><span class="sxs-lookup"><span data-stu-id="d5c57-121">For a step-by-step procedure, go too[Add a bandwidth template](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span></span>

      ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. <span data-ttu-id="d5c57-123">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d5c57-123">Click **Create**.</span></span>

        ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       <span data-ttu-id="d5c57-125">Du meddelas när hello volymbehållare har skapats.</span><span class="sxs-lookup"><span data-stu-id="d5c57-125">You are notified when hello volume container is successfully created.</span></span>

       ![Meddelande om att volymbehållaren har skapats](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   <span data-ttu-id="d5c57-127">hello nyskapad volymbehållare listas i hello lista över volymbehållare för din enhet.</span><span class="sxs-lookup"><span data-stu-id="d5c57-127">hello newly created volume container is listed in hello list of volume containers for your device.</span></span>

   ![Bladet Lägg till volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


