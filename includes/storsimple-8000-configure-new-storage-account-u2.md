<!--author=alkohli last changed: 01/20/17-->


#### <a name="to-add-a-storage-account-credential-in-the-same-azure-subscription-as-the-storsimple-device-manager-service"></a><span data-ttu-id="3509f-101">Så här lägger du till autentiseringsuppgifter för ett lagringskonto i samma Azure-prenumeration som StorSimple Device Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="3509f-101">To add a storage account credential in the same Azure subscription as the StorSimple Device Manager service</span></span>

1. <span data-ttu-id="3509f-102">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3509f-102">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="3509f-103">Klicka på **Autentiseringsuppgifter för lagringskonto** i avsnittet **Konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="3509f-103">In the **Configuration** section, click **Storage account credentials**.</span></span>

    ![Autentiseringsuppgifter för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. <span data-ttu-id="3509f-105">Klicka på **+ Lägg till** på bladet **Autentiseringsuppgifter för lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="3509f-105">On the **Storage account credentials** blade, click **+ Add**.</span></span>

    ![Lägg till autentiseringsuppgift för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. <span data-ttu-id="3509f-107">Utför följande steg på bladet **Lägg till autentiseringsuppgift för lagringskonto**:</span><span class="sxs-lookup"><span data-stu-id="3509f-107">In the **Add a storage account credential** blade, do the following steps:</span></span>

    1. <span data-ttu-id="3509f-108">När du lägger till en autentiseringsuppgift för ett lagringskonto i samma Azure-prenumeration som din tjänst kontrollerar du att **Aktuell** är markerat.</span><span class="sxs-lookup"><span data-stu-id="3509f-108">As you are adding a storage account credential in the same Azure subscription as your service, ensure that **Current** is selected.</span></span>

    2. <span data-ttu-id="3509f-109">Välj ett befintligt lagringskonto i listrutan **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="3509f-109">From the **storage account** dropdown list, select an existing storage account.</span></span>

    3. <span data-ttu-id="3509f-110">**Platsen** visas baserat på vilket lagringskonto du valt (nedtonad och kan inte ändras här).</span><span class="sxs-lookup"><span data-stu-id="3509f-110">Based on the storage account selected, the **location** will be displayed (grayed out and cannot be changed here).</span></span>

    4. <span data-ttu-id="3509f-111">Välj **Aktivera SSL-läge** om du vill skapa en säker kanal för nätverkskommunikation mellan din enhet och molnet.</span><span class="sxs-lookup"><span data-stu-id="3509f-111">Select **Enable SSL Mode** to create a secure channel for network communication between your device and the cloud.</span></span> <span data-ttu-id="3509f-112">Inaktivera endast **Aktivera SSL** om du arbetar i ett privat moln.</span><span class="sxs-lookup"><span data-stu-id="3509f-112">Disable **Enable SSL** only if you are operating within a private cloud.</span></span>

        ![Bladet Lägg till autentiseringsuppgifter för lagringskonto](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. <span data-ttu-id="3509f-114">Skapa autentiseringsuppgifterna för lagringskontot genom att klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3509f-114">Click **Add** to start the job creation for the storage account credential.</span></span> <span data-ttu-id="3509f-115">Du meddelas när autentiseringsuppgifterna för lagringskontot har skapats.</span><span class="sxs-lookup"><span data-stu-id="3509f-115">You will be notified after the storage account credential is successfully created.</span></span>

        ![Meddelande om att autentiseringsuppgifterna för lagringskontot har skapats](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

<span data-ttu-id="3509f-117">De nya autentiseringsuppgifterna för lagringskontot visas under listan **Autentiseringsuppgifter för lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="3509f-117">The newly created storage account credential will be displayed under the list of **Storage account credentials**.</span></span>

![Lista över autentiseringsuppgifter för lagringskonton](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

