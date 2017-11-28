
1. <span data-ttu-id="5c4c6-101">Navigera till [Google Cloud-konsolen](https://console.developers.google.com/project) och logga in med dina Google-kontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-101">Navigate to the [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="5c4c6-102">Klicka på **Skapa projekt**, ange ett projektnamn och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="5c4c6-103">Utför SMS-verifieringen om den efterfrågas och klicka på **Skapa** en gång till.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-103">If requested, carry out the SMS Verification, and click **Create** again.</span></span>
   
    ![Skapa nytt projekt](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="5c4c6-105">Ange ditt nya **projektnamn** och klicka på **Skapa projekt**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="5c4c6-106">Klicka på **Verktyg och mer**-knappen och klicka sedan på **Projektinformation**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-106">Click the **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="5c4c6-107">Anteckna **projektnumret**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-107">Make a note of the **Project Number**.</span></span> <span data-ttu-id="5c4c6-108">Du kommer behöva ange det värdet som `SenderId`-variabeln i klientappen.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-108">You will need to set this value as the `SenderId` variable in the client app.</span></span>
   
    ![Verktyg och mer](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="5c4c6-110">I instrumentpanelen för projektet under **Mobila API:er** klickar du på **Google Cloud Messaging** och på nästa sida klickar du på **Aktivera API** och accepterar villkoren.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-110">In the project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on the next page click **Enable API** and accept the terms of service.</span></span> 
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="5c4c6-113">I instrumentpanelen för projektet klickar du på **Autentiseringsuppgifter** > **Skapa autentiseringsuppgifter** > **API-nyckel**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-113">In the project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="5c4c6-114">I **Skapa en ny nyckel** klickar du på **Servernyckel**, skriver in ett namn för din nyckel och klickar sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="5c4c6-115">Anteckna **API-NYCKEL**-värdet.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-115">Make a note of the **API KEY** value.</span></span>
   
    <span data-ttu-id="5c4c6-116">Du kommer att använda API-nyckelvärdet för att autentisera med GCM och skicka push-meddelanden för din app.</span><span class="sxs-lookup"><span data-stu-id="5c4c6-116">You will use this API key value to enable Azure to authenticate with GCM and send push notifications on behalf of your app.</span></span>

