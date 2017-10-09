<!--author=alkohli last changed: 01/20/17-->


#### <a name="tooadd-a-storage-account-credential-in-hello-same-azure-subscription-as-hello-storsimple-device-manager-service"></a><span data-ttu-id="694bb-101">tooadd ett lagringskonto autentiseringsuppgifter i hello samma Azure-prenumeration som hello StorSimple Enhetshanteraren tjänst</span><span class="sxs-lookup"><span data-stu-id="694bb-101">tooadd a storage account credential in hello same Azure subscription as hello StorSimple Device Manager service</span></span>

1. <span data-ttu-id="694bb-102">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="694bb-102">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="694bb-103">I hello **Configuration** klickar du på **lagringskontouppgifter**.</span><span class="sxs-lookup"><span data-stu-id="694bb-103">In hello **Configuration** section, click **Storage account credentials**.</span></span>

    ![Autentiseringsuppgifter för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. <span data-ttu-id="694bb-105">På hello **lagringskontouppgifter** bladet, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="694bb-105">On hello **Storage account credentials** blade, click **+ Add**.</span></span>

    ![Lägg till autentiseringsuppgift för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. <span data-ttu-id="694bb-107">I hello **lägga till en lagring kontoautentisering** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="694bb-107">In hello **Add a storage account credential** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="694bb-108">När du lägger till en autentiseringsuppgift för storage-konto i hello samma Azure-prenumeration som din tjänst, kontrollerar du att **aktuella** är markerad.</span><span class="sxs-lookup"><span data-stu-id="694bb-108">As you are adding a storage account credential in hello same Azure subscription as your service, ensure that **Current** is selected.</span></span>

    2. <span data-ttu-id="694bb-109">Från hello **lagringskonto** listrutan väljer du ett befintligt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="694bb-109">From hello **storage account** dropdown list, select an existing storage account.</span></span>

    3. <span data-ttu-id="694bb-110">Baserat på hello lagringskonto har valts, hello **plats** visas (nedtonad och kan inte ändras här).</span><span class="sxs-lookup"><span data-stu-id="694bb-110">Based on hello storage account selected, hello **location** will be displayed (grayed out and cannot be changed here).</span></span>

    4. <span data-ttu-id="694bb-111">Välj **aktivera SSL-läge** toocreate en säker kanal för nätverkskommunikation mellan din enhet och hello moln.</span><span class="sxs-lookup"><span data-stu-id="694bb-111">Select **Enable SSL Mode** toocreate a secure channel for network communication between your device and hello cloud.</span></span> <span data-ttu-id="694bb-112">Inaktivera endast **Aktivera SSL** om du arbetar i ett privat moln.</span><span class="sxs-lookup"><span data-stu-id="694bb-112">Disable **Enable SSL** only if you are operating within a private cloud.</span></span>

        ![Bladet Lägg till autentiseringsuppgifter för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. <span data-ttu-id="694bb-114">Klicka på **Lägg till** toostart hello jobb skapas för hello storage-konto autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="694bb-114">Click **Add** toostart hello job creation for hello storage account credential.</span></span> <span data-ttu-id="694bb-115">Du meddelas när hello lagring kontoautentisering har skapats.</span><span class="sxs-lookup"><span data-stu-id="694bb-115">You will be notified after hello storage account credential is successfully created.</span></span>

        ![Meddelande om att autentiseringsuppgifterna för lagringskontot har skapats](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

<span data-ttu-id="694bb-117">hello nyskapad lagring kontoautentisering visas under hello lista över **lagringskontouppgifter**.</span><span class="sxs-lookup"><span data-stu-id="694bb-117">hello newly created storage account credential will be displayed under hello list of **Storage account credentials**.</span></span>

![Lista över autentiseringsuppgifter för lagringskonton](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

