> [!IMPORTANT]
> <span data-ttu-id="1ef2b-101">tooreceive Push-meddelanden från Mobile Engagement måste tooenable `Silent Remote Notifications` i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1ef2b-101">tooreceive Push Notifications from Mobile Engagement, you need tooenable `Silent Remote Notifications` in your application.</span></span> <span data-ttu-id="1ef2b-102">Du måste tooadd hello värdet remote-notification toohello UIBackgroundModes-matrisen i din Info.plist-fil.</span><span class="sxs-lookup"><span data-stu-id="1ef2b-102">You need tooadd hello remote-notification value toohello UIBackgroundModes array in your Info.plist file.</span></span>
> 
> 

1. <span data-ttu-id="1ef2b-103">Öppna `info.plist` fil i hello-projekt</span><span class="sxs-lookup"><span data-stu-id="1ef2b-103">Open `info.plist` file in hello project</span></span>
2. <span data-ttu-id="1ef2b-104">Högerklicka på hello översta objektet i listan hello (`Information Property List`) och Lägg till en ny rad</span><span class="sxs-lookup"><span data-stu-id="1ef2b-104">Right click on hello top item in hello list (`Information Property List`) and add a new row</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. <span data-ttu-id="1ef2b-105">Ange i hello ny rad`Required background modes`</span><span class="sxs-lookup"><span data-stu-id="1ef2b-105">In hello new row enter `Required background modes`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. <span data-ttu-id="1ef2b-106">Klicka på hello VÄNSTERPIL tooexpand hello rad</span><span class="sxs-lookup"><span data-stu-id="1ef2b-106">Click on hello left arrow tooexpand hello row</span></span>
5. <span data-ttu-id="1ef2b-107">Lägg till följande värde toohello objektet 0 hello`App downloads content in response toopush notifications`</span><span class="sxs-lookup"><span data-stu-id="1ef2b-107">Add hello following value toohello item 0 `App downloads content in response toopush notifications`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. <span data-ttu-id="1ef2b-108">När du har gjort hello ändra hello info.plist XML ska innehålla hello följande nyckel och värde:</span><span class="sxs-lookup"><span data-stu-id="1ef2b-108">Once you make hello change, hello info.plist XML should contain hello following key and value:</span></span>
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. <span data-ttu-id="1ef2b-109">Om du använder **Xcode 7+** och **iOS 9+**:</span><span class="sxs-lookup"><span data-stu-id="1ef2b-109">If you are using **Xcode 7+** and **iOS 9+**:</span></span>
   
   * <span data-ttu-id="1ef2b-110">Aktivera **Push-meddelanden** i Mål > Ditt målnamn > Funktioner.</span><span class="sxs-lookup"><span data-stu-id="1ef2b-110">Enable **Push Notifications** in Targets > Your Target Name > Capabilities.</span></span>

