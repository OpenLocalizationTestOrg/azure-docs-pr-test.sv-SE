
1. <span data-ttu-id="41581-101">I den [Azure-portalen](https://portal.azure.com/), klickar du på **Bläddra bland alla** > **Apptjänster**, och klicka sedan på din Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="41581-101">In the [Azure portal](https://portal.azure.com/), click **Browse All** > **App Services**, and then click your Mobile Apps back end.</span></span> <span data-ttu-id="41581-102">Under **inställningar**, klickar du på **App Service Push**, och klicka sedan på namnet på din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="41581-102">Under **Settings**, click **App Service Push**, and then click your notification hub name.</span></span>
2. <span data-ttu-id="41581-103">Gå till **Google (GCM)**, ange den **servernyckel** värde som du fick från Firebase i föregående procedur och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="41581-103">Go to **Google (GCM)**, enter the **Server Key** value that you obtained from Firebase in the previous procedure, and then click **Save**.</span></span>

    ![Ange GCM API-nyckeln i portalen](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

<span data-ttu-id="41581-105">Mobile Apps serverdel har nu konfigurerats för att använda Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="41581-105">The Mobile Apps back end is now configured to use Firebase Cloud Messaging.</span></span> <span data-ttu-id="41581-106">På så sätt kan du skicka push-meddelanden i appen körs på en Android-enhet med hjälp av notification hub.</span><span class="sxs-lookup"><span data-stu-id="41581-106">This enables you to send push notifications to your app running on an Android device, by using the notification hub.</span></span>

<!-- URLs. -->


<!-- images -->
