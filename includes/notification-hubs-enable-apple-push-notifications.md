

## <a name="generate-hello-certificate-signing-request-file"></a><span data-ttu-id="2d0cd-101">Generera filen för hello begäran om Certifikatsignering</span><span class="sxs-lookup"><span data-stu-id="2d0cd-101">Generate hello Certificate Signing Request file</span></span>
<span data-ttu-id="2d0cd-102">hello Apple Push Notification Service (APNS) använder certifikat tooauthenticate push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-102">hello Apple Push Notification Service (APNS) uses certificates tooauthenticate your push notifications.</span></span> <span data-ttu-id="2d0cd-103">Följ dessa instruktioner toocreate hello nödvändiga push-certifikat toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-103">Follow these instructions toocreate hello necessary push certificate toosend and receive notifications.</span></span> <span data-ttu-id="2d0cd-104">Mer information om dessa koncept finns hello officiella [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-104">For more information on these concepts see hello official [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) documentation.</span></span>

<span data-ttu-id="2d0cd-105">Generera hello begäran om certifikatsignering (CSR)-filen används av Apple toogenerate ett signerat push-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-105">Generate hello Certificate Signing Request (CSR) file, which is used by Apple toogenerate a signed push certificate.</span></span>

1. <span data-ttu-id="2d0cd-106">Kör hello Nyckelhanteraren på din Mac..</span><span class="sxs-lookup"><span data-stu-id="2d0cd-106">On your Mac, run hello Keychain Access tool.</span></span> <span data-ttu-id="2d0cd-107">Det kan öppnas från hello **verktyg** mapp eller hello **andra** mapp på hello startmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-107">It can be opened from hello **Utilities** folder or hello **Other** folder on hello launch pad.</span></span>
2. <span data-ttu-id="2d0cd-108">Klicka på **Nyckelhanteraren**, expandera **Certifikatassistenten** och klicka sedan på **Begär ett certifikat från en certifikatutfärdare...**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-108">Click **Keychain Access**, expand **Certificate Assistant**, then click **Request a Certificate from a Certificate Authority...**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. <span data-ttu-id="2d0cd-109">Välj din **användarens e-postadress** och **nätverksnamn** , se till att **sparade toodisk** är markerad och klicka sedan på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-109">Select your **User Email Address** and **Common Name** , make sure that **Saved toodisk** is selected, and then click **Continue**.</span></span> <span data-ttu-id="2d0cd-110">Lämna hello **CA e-postadress** fältet tomt eftersom det inte är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-110">Leave hello **CA Email Address** field blank as it is not required.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. <span data-ttu-id="2d0cd-111">Skriv ett namn för hello begäran om certifikatsignering (CSR)-fil i **Spara som**, Välj hello plats **där**, klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-111">Type a name for hello Certificate Signing Request (CSR) file in **Save As**, select hello location in **Where**, then click **Save**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      <span data-ttu-id="2d0cd-112">Detta sparar hello CSR-filen i hello valt plats. hello standardplatsen är hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-112">This saves hello CSR file in hello selected location; hello default location is in hello Desktop.</span></span> <span data-ttu-id="2d0cd-113">Kom ihåg hello plats du valde för den här filen.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-113">Remember hello location chosen for this file.</span></span>

<span data-ttu-id="2d0cd-114">Du kommer sedan registrera din app med Apple, aktivera push-meddelanden och ladda upp det här exporterade CSR-toocreate ett push-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-114">Next, you will register your app with Apple, enable push notifications, and upload this exported CSR toocreate a push certificate.</span></span>

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="2d0cd-115">Registrera din app för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="2d0cd-115">Register your app for push notifications</span></span>
<span data-ttu-id="2d0cd-116">toobe kan toosend push-meddelanden tooan iOS-app, måste du registrera programmet med Apple och registrera dig för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-116">toobe able toosend push notifications tooan iOS app, you must register your application with Apple and also register for push notifications.</span></span>  

1. <span data-ttu-id="2d0cd-117">Om du inte redan har registrerat appen navigerar toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center, loggar in med ditt Apple-ID, klickar du på **identifierare**, klicka på **App-ID: N** , och klickar slutligen på hello  **+**  logga tooregister en ny app.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-117">If you have not already registered your app, navigate toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> at hello Apple Developer Center, log on with your Apple ID, click **Identifiers**, then click **App IDs**, and finally click on hello **+** sign tooregister a new app.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. <span data-ttu-id="2d0cd-118">Uppdatera hello följande tre fält för din nya app och klicka sedan på **Fortsätt**:</span><span class="sxs-lookup"><span data-stu-id="2d0cd-118">Update hello following three fields for your new app and then click **Continue**:</span></span>
   
   * <span data-ttu-id="2d0cd-119">**Namnet**: Ange ett beskrivande namn för din app i hello **namn** i hello **App-ID beskrivning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-119">**Name**: Type a descriptive name for your app in hello **Name** field in hello **App ID Description** section.</span></span>
   * <span data-ttu-id="2d0cd-120">**Paketidentifierare**: Under hello **uttrycklig App-ID** ange en **Paketidentifierare** i form av hello `<Organization Identifier>.<Product Name>` som anges i hello [App-Distribution Guiden](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span><span class="sxs-lookup"><span data-stu-id="2d0cd-120">**Bundle Identifier**: Under hello **Explicit App ID** section, enter a **Bundle Identifier** in hello form `<Organization Identifier>.<Product Name>` as mentioned in hello [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span></span> <span data-ttu-id="2d0cd-121">Hej *Organisationsidentifierare* och *produktnamn* du använder måste matcha hello organisations-ID och produktnamn som du använder när du skapar ditt XCode-projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-121">hello *Organization Identifier* and *Product Name* you use must match hello organization identifier and product name you will use when you create your XCode project.</span></span> <span data-ttu-id="2d0cd-122">I hello skärmbilden nedan *NotificationHubs* används som en organisationsidentifierare och *GetStarted* används som hello produktnamn.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-122">In hello screeshot below *NotificationHubs* is used as a organization idenitifier and *GetStarted* is used as hello product name.</span></span> <span data-ttu-id="2d0cd-123">Kontrollera att detta matchar hello-värden som du ska använda i ditt XCode-projekt kan du toouse hello rätt publiceringsprofil med XCode.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-123">Making sure this matches hello values you will use in your XCode project will allow you toouse hello correct publishing profile with XCode.</span></span> 
   * <span data-ttu-id="2d0cd-124">**Push-meddelanden**: Kontrollera hello **Push-meddelanden** alternativ i hello **Apptjänster** avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-124">**Push Notifications**: Check hello **Push Notifications** option in hello **App Services** section, .</span></span>
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      <span data-ttu-id="2d0cd-125">Detta genererar din App-ID och begär tooconfirm hello information.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-125">This generates your App ID and requests you tooconfirm hello information.</span></span> <span data-ttu-id="2d0cd-126">Klicka på **registrera** tooconfirm hello nya App-ID.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-126">Click **Register** tooconfirm hello new App ID.</span></span>
     
      <span data-ttu-id="2d0cd-127">När du klickar på **registrera**, ser du hello **registreringen har slutförts** skärmen, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-127">Once you click **Register**, you will see hello **Registration complete** screen, as shown below.</span></span> <span data-ttu-id="2d0cd-128">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-128">Click **Done**.</span></span>
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. <span data-ttu-id="2d0cd-129">Leta upp hello app-ID som du just skapade och klickar på dess rad i hello Developer Center under App-ID: N.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-129">In hello Developer Center, under App IDs, locate hello app ID that you just created, and click on its row.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      <span data-ttu-id="2d0cd-130">Klicka på hello app-ID visas information om hello appar.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-130">Clicking on hello app ID will display hello app details.</span></span> <span data-ttu-id="2d0cd-131">Klicka på hello **redigera** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-131">Click hello **Edit** button at hello bottom.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. <span data-ttu-id="2d0cd-132">Bläddra toohello ned hello-skärmen och klicka på hello **skapa certifikat...**  knappen under hello avsnittet **Development Push SSL-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-132">Scroll toohello bottom of hello screen, and click hello **Create Certificate...** button under hello section **Development Push SSL Certificate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      <span data-ttu-id="2d0cd-133">Detta visar hello ”Lägg till iOS-certifikat” Installationsassistenten.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-133">This displays hello "Add iOS Certificate" assistant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2d0cd-134">Den här guiden använder ett utvecklarcertifikat.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-134">This tutorial uses a development certificate.</span></span> <span data-ttu-id="2d0cd-135">hello samma process som används när du registrerar ett driftscertifikat.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-135">hello same process is used when registering a production certificate.</span></span> <span data-ttu-id="2d0cd-136">Kontrollera att du använder hello samma certifikattyp när du skickar meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-136">Just make sure that you use hello same certificate type when sending notifications.</span></span>
   > 
   > 
3. <span data-ttu-id="2d0cd-137">Klicka på **Välj fil**, bläddra toohello plats där du sparade hello CSR-filen som du skapade i hello första uppgiften och klicka på **generera**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-137">Click **Choose File**, browse toohello location where you saved hello CSR file that you created in hello first task, then click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. <span data-ttu-id="2d0cd-138">När hello certifikatet har skapats av hello portal, klickar du på hello **hämta** och klickar på **klar**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-138">After hello certificate is created by hello portal, click hello **Download** button, and click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      <span data-ttu-id="2d0cd-139">Detta hämtar hello certifikat och sparar den tooyour dator i din mapp för nedladdningar.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-139">This downloads hello certificate and saves it tooyour computer in your Downloads folder.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > <span data-ttu-id="2d0cd-140">Som standard hello ned filen ett utvecklingscertifikat **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-140">By default, hello downloaded file a development certificate is named **aps_development.cer**.</span></span>
   > 
   > 
5. <span data-ttu-id="2d0cd-141">Dubbelklicka på hello hämtade push-certifikatet **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-141">Double-click hello downloaded push certificate **aps_development.cer**.</span></span>
   
      <span data-ttu-id="2d0cd-142">Detta installerar hello nytt certifikat i hello nyckelringen enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="2d0cd-142">This installs hello new certificate in hello Keychain, as shown below:</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > <span data-ttu-id="2d0cd-143">hello namn i certifikatet kan skilja sig, men det inleds med **Apple Development iOS Push Services:**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-143">hello name in your certificate might be different, but it will be prefixed with **Apple Development iOS Push Services:**.</span></span>
   > 
   > 
6. <span data-ttu-id="2d0cd-144">I Nyckelhanteraren högerklickar du på hello nya push-certifikat som du skapade i hello **certifikat** kategori.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-144">In Keychain Access, right-click hello new push certificate that you created in hello **Certificates** category.</span></span> <span data-ttu-id="2d0cd-145">Klicka på **exportera**, namnge hello-filen, Välj hello **.p12** formatera och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-145">Click **Export**, name hello file, select hello **.p12** format, and then click **Save**.</span></span>
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    <span data-ttu-id="2d0cd-146">Se anteckna hello filnamnet och platsen för hello .p12-certifikatet exporterats.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-146">Make a note of hello file name and location of hello exported .p12 certificate.</span></span> <span data-ttu-id="2d0cd-147">Den kommer att använda tooenable autentisering med APNS.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-147">It will be used tooenable authentication with APNS.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2d0cd-148">Den här guiden skapar en QuickStart.p12-fil.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-148">This tutorial creates a QuickStart.p12 file.</span></span> <span data-ttu-id="2d0cd-149">Filnamnet och sökvägen kan vara olika för dig.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-149">Your file name and location might be different.</span></span>
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a><span data-ttu-id="2d0cd-150">Skapa en etableringsprofil för hello app</span><span class="sxs-lookup"><span data-stu-id="2d0cd-150">Create a provisioning profile for hello app</span></span>
1. <span data-ttu-id="2d0cd-151">Tillbaka i hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>väljer **Etableringsprofiler**väljer **alla**, och klicka sedan på hello  **+**  knappen toocreate en ny profil.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-151">Back in hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, select **Provisioning Profiles**, select **All**, and then click hello **+** button toocreate a new profile.</span></span> <span data-ttu-id="2d0cd-152">Detta startar hello **Lägg till iOS-Etableringsprofil** guiden</span><span class="sxs-lookup"><span data-stu-id="2d0cd-152">This launches hello **Add iOS Provisiong Profile** Wizard</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. <span data-ttu-id="2d0cd-153">Välj **iOS App Development** under **Development** som hello etableringsprofiltyp och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-153">Select **iOS App Development** under **Development** as hello provisiong profile type, and click **Continue**.</span></span> 
3. <span data-ttu-id="2d0cd-154">Välj därefter hello app-ID som du nyss skapat från hello **App-ID** listrutan och klicka på **Fortsätt**</span><span class="sxs-lookup"><span data-stu-id="2d0cd-154">Next, select hello app ID you just created from hello **App ID** drop-down list, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. <span data-ttu-id="2d0cd-155">I hello **Välj certifikat** skärmen, Välj ditt vanliga utvecklingscertifikat som används för kodsignering och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-155">In hello **Select certificates** screen, select your usual development certificate used for code signing, and click **Continue**.</span></span> <span data-ttu-id="2d0cd-156">Detta är inte hello push-certifikat som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-156">This is not hello push certificate you just created.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. <span data-ttu-id="2d0cd-157">Välj därefter hello **enheter** toouse för testning och klickar på **Fortsätt**</span><span class="sxs-lookup"><span data-stu-id="2d0cd-157">Next, select hello **Devices** toouse for testing, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. <span data-ttu-id="2d0cd-158">Slutligen väljer du ett namn för profilen hello i **profilnamn**, klickar du på **generera**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-158">Finally, pick a name for hello profile in **Profile Name**, click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. <span data-ttu-id="2d0cd-159">När hello nya etableringsprofil som har skapats klickar du på toodownload den och installera den på din Xcode-utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-159">When hello new provisioning profile is created click toodownload it and install it on your Xcode development machine.</span></span> <span data-ttu-id="2d0cd-160">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="2d0cd-160">Then click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
