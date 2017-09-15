> [!IMPORTANT]
> <span data-ttu-id="41478-101">För att kunna ta emot push-meddelanden från Mobile Engagement måste du aktivera `Silent Remote Notifications` i ditt program.</span><span class="sxs-lookup"><span data-stu-id="41478-101">To receive Push Notifications from Mobile Engagement, you need to enable `Silent Remote Notifications` in your application.</span></span> <span data-ttu-id="41478-102">Du måste lägga till värdet remote-notification i UIBackgroundModes-matrisen i din Info.plist-fil.</span><span class="sxs-lookup"><span data-stu-id="41478-102">You need to add the remote-notification value to the UIBackgroundModes array in your Info.plist file.</span></span>
> 
> 

1. <span data-ttu-id="41478-103">Öppna `info.plist`-filen i projektet</span><span class="sxs-lookup"><span data-stu-id="41478-103">Open `info.plist` file in the project</span></span>
2. <span data-ttu-id="41478-104">Högerklicka på det översta objektet i listan (`Information Property List`) och lägg till en ny rad</span><span class="sxs-lookup"><span data-stu-id="41478-104">Right click on the top item in the list (`Information Property List`) and add a new row</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. <span data-ttu-id="41478-105">På den nya raden anger du `Required background modes`</span><span class="sxs-lookup"><span data-stu-id="41478-105">In the new row enter `Required background modes`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. <span data-ttu-id="41478-106">Klicka på vänsterpilen för att expandera raden</span><span class="sxs-lookup"><span data-stu-id="41478-106">Click on the left arrow to expand the row</span></span>
5. <span data-ttu-id="41478-107">Lägg till följande värde till objektet 0 `App downloads content in response to push notifications`</span><span class="sxs-lookup"><span data-stu-id="41478-107">Add the following value to the item 0 `App downloads content in response to push notifications`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. <span data-ttu-id="41478-108">När du gjort ändringen borde XML-filen info.plist innehålla följande nyckel och värde:</span><span class="sxs-lookup"><span data-stu-id="41478-108">Once you make the change, the info.plist XML should contain the following key and value:</span></span>
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. <span data-ttu-id="41478-109">Om du använder **Xcode 7+** och **iOS 9+**:</span><span class="sxs-lookup"><span data-stu-id="41478-109">If you are using **Xcode 7+** and **iOS 9+**:</span></span>
   
   * <span data-ttu-id="41478-110">Aktivera **Push-meddelanden** i Mål > Ditt målnamn > Funktioner.</span><span class="sxs-lookup"><span data-stu-id="41478-110">Enable **Push Notifications** in Targets > Your Target Name > Capabilities.</span></span>

