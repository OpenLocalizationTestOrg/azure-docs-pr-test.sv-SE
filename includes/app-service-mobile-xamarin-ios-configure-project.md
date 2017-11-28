#### <a name="configure-the-ios-project-in-xamarin-studio"></a><span data-ttu-id="35218-101">Konfigurera iOS-projekt i Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="35218-101">Configure the iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="35218-102">Öppna i Xamarin.Studio, **Info.plist**, och uppdatera den **Paketidentifierare** med paket-ID som du skapade tidigare med din nya app-ID.</span><span class="sxs-lookup"><span data-stu-id="35218-102">In Xamarin.Studio, open **Info.plist**, and update the **Bundle Identifier** with the bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="35218-103">Rulla ned till **bakgrundslägen**.</span><span class="sxs-lookup"><span data-stu-id="35218-103">Scroll down to **Background Modes**.</span></span> <span data-ttu-id="35218-104">Välj den **aktivera bakgrundslägen** rutan och **Remote notifications** rutan.</span><span class="sxs-lookup"><span data-stu-id="35218-104">Select the **Enable Background Modes** box and the **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="35218-105">Dubbelklicka på ditt projekt i panelen lösning för att öppna **Projektalternativ**.</span><span class="sxs-lookup"><span data-stu-id="35218-105">Double-click your project in the Solution Panel to open **Project Options**.</span></span>
4. <span data-ttu-id="35218-106">Under **skapa**, Välj **iOS paket signering**, och markera motsvarande identitet och provisioning-profil du just ställt in för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="35218-106">Under **Build**, choose **iOS Bundle Signing**, and select the corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="35218-107">Detta säkerställer att projektet använder den nya profilen för kodsignering.</span><span class="sxs-lookup"><span data-stu-id="35218-107">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="35218-108">Officiell Xamarin enheten etablering dokumentation finns [Xamarin Enhetsetableringen].</span><span class="sxs-lookup"><span data-stu-id="35218-108">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-the-ios-project-in-visual-studio"></a><span data-ttu-id="35218-109">Konfigurera iOS-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35218-109">Configure the iOS project in Visual Studio</span></span>
1. <span data-ttu-id="35218-110">Högerklicka på projektet i Visual Studio och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="35218-110">In Visual Studio, right-click the project, and then click **Properties**.</span></span>
2. <span data-ttu-id="35218-111">I för egenskapssidor, klickar du på den **iOS programmet** fliken och uppdatera den **identifierare** med det ID som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="35218-111">In the properties pages, click the **iOS Application** tab, and update the **Identifier** with the ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="35218-112">I den **iOS paket signering** markerar du motsvarande identitet och etableringsprofil som du precis har ställts in för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="35218-112">In the **iOS Bundle Signing** tab, select the corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="35218-113">Detta säkerställer att projektet använder den nya profilen för kodsignering.</span><span class="sxs-lookup"><span data-stu-id="35218-113">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="35218-114">Officiell Xamarin enheten etablering dokumentation finns [Xamarin Enhetsetableringen].</span><span class="sxs-lookup"><span data-stu-id="35218-114">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="35218-115">Dubbelklicka på filen Info.plist för att öppna den och sedan aktivera **RemoteNotifications** under **bakgrundslägen**.</span><span class="sxs-lookup"><span data-stu-id="35218-115">Double-click Info.plist to open it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

<span data-ttu-id="35218-116">[Xamarin Enhetsetableringen]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span><span class="sxs-lookup"><span data-stu-id="35218-116">[Xamarin Device Provisioning]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span></span>
