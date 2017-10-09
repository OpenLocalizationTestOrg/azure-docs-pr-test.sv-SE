### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a><span data-ttu-id="2ceb0-101">Bevilja åtkomst tooyour Push-certifikat tooMobile Engagement</span><span class="sxs-lookup"><span data-stu-id="2ceb0-101">Grant access tooyour Push Certificate tooMobile Engagement</span></span>
<span data-ttu-id="2ceb0-102">tooallow Mobile Engagement toosend Push-meddelanden för din räkning, behöver du toogrant den tillgång till tooyour certifikat.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-102">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour certificate.</span></span> <span data-ttu-id="2ceb0-103">Detta görs genom att konfigurera och ange ditt certifikat i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-103">This is done by configuring and entering your certificate into hello Mobile Engagement portal.</span></span> <span data-ttu-id="2ceb0-104">Kontrollera att du har fått ditt .p12-certifikat enligt beskrivningen i [Apples dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span><span class="sxs-lookup"><span data-stu-id="2ceb0-104">Make sure you obtain your .p12 certificate as explained in [Apple's documentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

1. <span data-ttu-id="2ceb0-105">Navigera tooyour Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-105">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="2ceb0-106">Se till att du befinner dig i rätt hello och klicka sedan på hello **starta** knappen längst ned hello:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-106">Ensure you're in hello correct and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. <span data-ttu-id="2ceb0-107">Klicka på hello **inställningar** sida i Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-107">Click on hello **Settings** page in your Engagement Portal.</span></span> <span data-ttu-id="2ceb0-108">Därifrån klickar du på hello **Native Push** avsnittet tooupload ditt p12-certifikat:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-108">From there click on hello **Native Push** section tooupload your p12 certificate:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. <span data-ttu-id="2ceb0-109">Välj ditt p12, överför det och ange ditt lösenord:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-109">Select your p12, upload it and type your password:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <span data-ttu-id="2ceb0-110"><a id="send"></a>Skicka en avisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="2ceb0-110"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="2ceb0-111">Nu ska vi skapa en enkel Push-meddelandekampanj som skickar ett push tooour app:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-111">We will now create a simple Push Notification campaign that will send a push tooour app:</span></span>

1. <span data-ttu-id="2ceb0-112">Navigera toohello **nå** fliken i din Mobile Engagement-portal.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-112">Navigate toohello **Reach** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="2ceb0-113">Klicka på **nytt meddelande** toocreate push-kampanj</span><span class="sxs-lookup"><span data-stu-id="2ceb0-113">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. <span data-ttu-id="2ceb0-114">Konfigurera hello första fälten i din kampanj:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-114">Setup hello first fields of your campaign:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * <span data-ttu-id="2ceb0-115">Ge din kampanj ett **namn**</span><span class="sxs-lookup"><span data-stu-id="2ceb0-115">Provide a **Name** for your campaign</span></span> 
   * <span data-ttu-id="2ceb0-116">Välj hello **leveranstiden** som **endast utanför appen**: Detta är hello enkla Apple push-meddelandetypen som innehåller text.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-116">Select hello **Delivery time** as **Out of app only**: this is hello simple Apple push notification type that features some text.</span></span>
   * <span data-ttu-id="2ceb0-117">Ange första hello i hello meddelandetext **rubrik** som kommer att vara hello första raden i hello push.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-117">In hello notification text, type first hello **Title** which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="2ceb0-118">Skriv din **meddelandet** som kommer att vara hello andra raden</span><span class="sxs-lookup"><span data-stu-id="2ceb0-118">Then type your **Message** which will be hello second line</span></span>
4. <span data-ttu-id="2ceb0-119">Bläddra nedåt och i hello innehållsavsnitt väljer **endast meddelande**</span><span class="sxs-lookup"><span data-stu-id="2ceb0-119">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. <span data-ttu-id="2ceb0-120">Du är klar inställningen hello mest grundläggande kampanjen.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-120">You're done setting hello most basic campaign.</span></span> <span data-ttu-id="2ceb0-121">Bläddra nedåt och klicka på **skapa** knappen toosave push-meddelandekampanj.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-121">Now scroll down and click on **Create** button toosave your push notification campaign.</span></span> 
6. <span data-ttu-id="2ceb0-122">Slutligen klickar du på **aktivera** toosend push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="2ceb0-122">Finally - click on **Activate** toosend push notification.</span></span> 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. <span data-ttu-id="2ceb0-123">Kan ta emot hello-meddelande på din iOS-enhet i hello meddelandecentret hello följande:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-123">You will be able receive hello notification on your iOS device in hello notification center like hello following:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. <span data-ttu-id="2ceb0-124">Om du har en Apple Watch parad med iOS-enheten ska du se hello-meddelande på din Apple Watch:</span><span class="sxs-lookup"><span data-stu-id="2ceb0-124">If you have an Apple Watch paired with this iOS device then you will see hello notification on your Apple Watch:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

