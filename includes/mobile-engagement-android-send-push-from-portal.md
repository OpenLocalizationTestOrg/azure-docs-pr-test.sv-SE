### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="0025f-101">Bevilja Mobile Engagement åtkomst tooyour GCM API-nyckel</span><span class="sxs-lookup"><span data-stu-id="0025f-101">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="0025f-102">tooallow Mobile Engagement toosend push-meddelanden för din räkning, behöver du toogrant den tillgång till tooyour API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="0025f-102">tooallow Mobile Engagement toosend push notifications on your behalf, you need toogrant it access tooyour API Key.</span></span> <span data-ttu-id="0025f-103">Detta görs genom att konfigurera och ange nyckeln i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="0025f-103">This is done by configuring and entering your key into hello Mobile Engagement portal.</span></span>

1. <span data-ttu-id="0025f-104">Från den klassiska Azure-portalen ser till att du är i hello app vi använder för det här projektet och klicka sedan på hello **starta** knappen längst ned hello:</span><span class="sxs-lookup"><span data-stu-id="0025f-104">From your Azure Classic Portal, ensure you're in hello app we're using for this project, and then click hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. <span data-ttu-id="0025f-105">Klicka på hello **inställningar** -> **Native Push** avsnittet tooenter din GCM-nyckel:</span><span class="sxs-lookup"><span data-stu-id="0025f-105">Then click hello **Settings** -> **Native Push** section tooenter your GCM Key:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. <span data-ttu-id="0025f-106">Klicka på hello **redigera** ikonen framför **API-nyckel** i hello **GCM-inställningar** enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="0025f-106">Click hello **Edit** icon in front of **API Key** in hello **GCM Settings** section as shown below:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. <span data-ttu-id="0025f-107">Klistra in hello GCM-servernyckeln som du erhållit tidigare hello popup-fönstret och klicka sedan på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="0025f-107">In hello pop-up, paste hello GCM Server Key you obtained before and then click **Ok**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <span data-ttu-id="0025f-108"><a id="send"></a>Skicka en avisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="0025f-108"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="0025f-109">Nu skapar vi en enkel push-meddelandekampanj som skickar ett push-meddelande tooour app.</span><span class="sxs-lookup"><span data-stu-id="0025f-109">We will now create a simple push notification campaign that sends a push notification tooour app.</span></span>

1. <span data-ttu-id="0025f-110">Navigera toohello **NÅ** fliken i din Mobile Engagement-portal.</span><span class="sxs-lookup"><span data-stu-id="0025f-110">Navigate toohello **REACH** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="0025f-111">Klicka på **nytt meddelande** toocreate push-meddelandekampanj.</span><span class="sxs-lookup"><span data-stu-id="0025f-111">Click **New announcement** toocreate your push notification campaign.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. <span data-ttu-id="0025f-112">Ställ in hello första fältet i din kampanj med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0025f-112">Set up hello first field of your campaign through hello following steps:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    <span data-ttu-id="0025f-113">a.</span><span class="sxs-lookup"><span data-stu-id="0025f-113">a.</span></span> <span data-ttu-id="0025f-114">Namnge din kampanj.</span><span class="sxs-lookup"><span data-stu-id="0025f-114">Name your campaign.</span></span>
   
    <span data-ttu-id="0025f-115">b.</span><span class="sxs-lookup"><span data-stu-id="0025f-115">b.</span></span> <span data-ttu-id="0025f-116">Välj hello **Leveranstyp** som *Systemmeddelande -> enkelt*: Detta är hello enkel Android push-meddelandetypen som innehåller en rubrik och en kort textrad.</span><span class="sxs-lookup"><span data-stu-id="0025f-116">Select hello **Delivery type** as *System notification -> Simple*: This is hello simple Android push notification type that features a title and a small line of text.</span></span>
   
    <span data-ttu-id="0025f-117">c.</span><span class="sxs-lookup"><span data-stu-id="0025f-117">c.</span></span> <span data-ttu-id="0025f-118">Välj **leveranstiden** som *helst* tooallow hello app tooreceive ett meddelande om hello appen startas eller inte.</span><span class="sxs-lookup"><span data-stu-id="0025f-118">Select **Delivery time** as *Any time* tooallow hello app tooreceive a notification whether hello app is started or not.</span></span>
   
    <span data-ttu-id="0025f-119">d.</span><span class="sxs-lookup"><span data-stu-id="0025f-119">d.</span></span> <span data-ttu-id="0025f-120">I hello notification text typen hello **rubrik** som kommer att vara i fetstil i push hello.</span><span class="sxs-lookup"><span data-stu-id="0025f-120">In hello notification text type hello **Title** which will be in bold in hello push.</span></span>
   
    <span data-ttu-id="0025f-121">e.</span><span class="sxs-lookup"><span data-stu-id="0025f-121">e.</span></span> <span data-ttu-id="0025f-122">Sedan skriver du ditt **meddelande**</span><span class="sxs-lookup"><span data-stu-id="0025f-122">Then type your **Message**</span></span>
4. <span data-ttu-id="0025f-123">Rulla ned och i hello **innehåll** väljer **endast meddelande**.</span><span class="sxs-lookup"><span data-stu-id="0025f-123">Scroll down, and in hello **Content** section, select **Notification only**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. <span data-ttu-id="0025f-124">Du är klar inställningen hello mest grundläggande kampanjen möjligt.</span><span class="sxs-lookup"><span data-stu-id="0025f-124">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="0025f-125">Bläddra nedåt igen och klicka på hello **skapa** knappen toosave din kampanj.</span><span class="sxs-lookup"><span data-stu-id="0025f-125">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
6. <span data-ttu-id="0025f-126">Sista steget: Klicka på **aktivera** tooactivate kampanj toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0025f-126">Last step: click **Activate** tooactivate your campaign toosend push notifications.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

