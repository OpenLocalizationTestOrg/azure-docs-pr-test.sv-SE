
1. <span data-ttu-id="0b704-101">Navigera toohello [Google Cloud-konsolen](https://console.developers.google.com/project), logga in med dina Google-konto.</span><span class="sxs-lookup"><span data-stu-id="0b704-101">Navigate toohello [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="0b704-102">Klicka på **Skapa projekt**, ange ett projektnamn och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0b704-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="0b704-103">Om det krävs utföra hello SMS-verifieringen och klicka på **skapa** igen.</span><span class="sxs-lookup"><span data-stu-id="0b704-103">If requested, carry out hello SMS Verification, and click **Create** again.</span></span>
   
    ![Skapa nytt projekt](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="0b704-105">Ange ditt nya **projektnamn** och klicka på **Skapa projekt**.</span><span class="sxs-lookup"><span data-stu-id="0b704-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="0b704-106">Klicka på hello **verktyg och mer** knappen och klicka sedan på **projektinformation**.</span><span class="sxs-lookup"><span data-stu-id="0b704-106">Click hello **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="0b704-107">Anteckna hello **projektnumret**.</span><span class="sxs-lookup"><span data-stu-id="0b704-107">Make a note of hello **Project Number**.</span></span> <span data-ttu-id="0b704-108">Du behöver tooset värdet som hello `SenderId` variabeln i klientappen hello.</span><span class="sxs-lookup"><span data-stu-id="0b704-108">You will need tooset this value as hello `SenderId` variable in hello client app.</span></span>
   
    ![Verktyg och mer](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="0b704-110">I hello projektet instrumentpanelen under **mobila API: er**, klickar du på **Google Cloud Messaging**, klicka på nästa sida i hello **aktivera API** och godkänner hello användarvillkoren.</span><span class="sxs-lookup"><span data-stu-id="0b704-110">In hello project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on hello next page click **Enable API** and accept hello terms of service.</span></span> 
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="0b704-113">Klicka på instrumentpanelen för projektet hello **autentiseringsuppgifter** > **skapa autentiseringsuppgifter** > **API-nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="0b704-113">In hello project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="0b704-114">I **Skapa en ny nyckel** klickar du på **Servernyckel**, skriver in ett namn för din nyckel och klickar sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0b704-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="0b704-115">Anteckna hello **API-nyckel** värde.</span><span class="sxs-lookup"><span data-stu-id="0b704-115">Make a note of hello **API KEY** value.</span></span>
   
    <span data-ttu-id="0b704-116">Du använder den här API-nyckelvärdet tooenable Azure tooauthenticate med GCM och skicka push-meddelanden för din app.</span><span class="sxs-lookup"><span data-stu-id="0b704-116">You will use this API key value tooenable Azure tooauthenticate with GCM and send push notifications on behalf of your app.</span></span>

