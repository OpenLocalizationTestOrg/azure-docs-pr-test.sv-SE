#### <a name="configure-hello-ios-project-in-xamarin-studio"></a><span data-ttu-id="40d79-101">Konfigurera hello iOS-projekt i Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="40d79-101">Configure hello iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="40d79-102">Öppna i Xamarin.Studio, **Info.plist**, och uppdatera hello **Paketidentifierare** med hello paketera ID som du skapade tidigare med din nya app-ID.</span><span class="sxs-lookup"><span data-stu-id="40d79-102">In Xamarin.Studio, open **Info.plist**, and update hello **Bundle Identifier** with hello bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="40d79-103">Rulla nedåt för**bakgrundslägen**.</span><span class="sxs-lookup"><span data-stu-id="40d79-103">Scroll down too**Background Modes**.</span></span> <span data-ttu-id="40d79-104">Välj hello **aktivera bakgrundslägen** rutan och hello **Remote notifications** rutan.</span><span class="sxs-lookup"><span data-stu-id="40d79-104">Select hello **Enable Background Modes** box and hello **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="40d79-105">Dubbelklicka på ditt projekt i hello lösning panelen tooopen **Projektalternativ**.</span><span class="sxs-lookup"><span data-stu-id="40d79-105">Double-click your project in hello Solution Panel tooopen **Project Options**.</span></span>
4. <span data-ttu-id="40d79-106">Under **skapa**, Välj **iOS paket signering**, och välj hello motsvarande identitet och etableringsprofil du bara ställa in för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="40d79-106">Under **Build**, choose **iOS Bundle Signing**, and select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="40d79-107">Detta säkerställer att hello-projekt använder hello ny profil för kodsignering.</span><span class="sxs-lookup"><span data-stu-id="40d79-107">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="40d79-108">Hello officiella Xamarin för etablering av enheter dokumentation, se [Xamarin Enhetsetableringen].</span><span class="sxs-lookup"><span data-stu-id="40d79-108">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-hello-ios-project-in-visual-studio"></a><span data-ttu-id="40d79-109">Konfigurera hello iOS-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40d79-109">Configure hello iOS project in Visual Studio</span></span>
1. <span data-ttu-id="40d79-110">Högerklicka på hello-projekt i Visual Studio och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="40d79-110">In Visual Studio, right-click hello project, and then click **Properties**.</span></span>
2. <span data-ttu-id="40d79-111">Klicka i hello egenskapssidor hello **iOS programmet** flik och uppdatera hello **identifierare** med hello-ID som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="40d79-111">In hello properties pages, click hello **iOS Application** tab, and update hello **Identifier** with hello ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="40d79-112">I hello **iOS paket signering** fliken väljer hello motsvarande identitet och provisioning-profil du just ställt in för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="40d79-112">In hello **iOS Bundle Signing** tab, select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="40d79-113">Detta säkerställer att hello-projekt använder hello ny profil för kodsignering.</span><span class="sxs-lookup"><span data-stu-id="40d79-113">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="40d79-114">Hello officiella Xamarin för etablering av enheter dokumentation, se [Xamarin Enhetsetableringen].</span><span class="sxs-lookup"><span data-stu-id="40d79-114">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="40d79-115">Dubbelklicka på Info.plist tooopen och sedan aktivera **RemoteNotifications** under **bakgrundslägen**.</span><span class="sxs-lookup"><span data-stu-id="40d79-115">Double-click Info.plist tooopen it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

[Xamarin Enhetsetableringen]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
