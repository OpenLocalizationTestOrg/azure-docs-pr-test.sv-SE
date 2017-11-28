1. <span data-ttu-id="b9364-101">Öppna Android SDK Manager genom att klicka på ikonen i verktygsfältet för Android Studio eller genom att klicka på **Verktyg** -> **Android** -> **SDK Manager** i menyn.</span><span class="sxs-lookup"><span data-stu-id="b9364-101">Open the Android SDK Manager by clicking the icon on the toolbar of Android Studio or by clicking **Tools** -> **Android** -> **SDK Manager** on the menu.</span></span> <span data-ttu-id="b9364-102">Hitta målversionen av Android SDK:n som används i ditt projekt, öppna den genom att klicka på **Visa paketinformation** och välj **Google API:er** om det inte redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="b9364-102">Locate the target version of the Android SDK that is used in your project , open it by clicking **Show Package Details**, and choose **Google APIs**, if it is not already installed.</span></span>
2. <span data-ttu-id="b9364-103">Klicka på fliken **SDK-verktyg**.</span><span class="sxs-lookup"><span data-stu-id="b9364-103">Click the **SDK Tools** tab.</span></span> <span data-ttu-id="b9364-104">Om du inte redan har installerat Google Play-tjänsten klickar du på **Google Play Services** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b9364-104">If you haven't already installed Google Play Service, click **Google Play Services** as shown below.</span></span> <span data-ttu-id="b9364-105">Klicka sedan på **Tillämpa** för att installera.</span><span class="sxs-lookup"><span data-stu-id="b9364-105">Then click **Apply** to install.</span></span> 
   
    <span data-ttu-id="b9364-106">Anteckna SDK-sökvägen för användning i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="b9364-106">Note the SDK path, for use in a later step.</span></span> 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. <span data-ttu-id="b9364-107">Öppna filen **build.gradle** i appkatalogen.</span><span class="sxs-lookup"><span data-stu-id="b9364-107">Open the **build.gradle** file in the app directory.</span></span>
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. <span data-ttu-id="b9364-108">Lägg till följande rad under *beroenden*:</span><span class="sxs-lookup"><span data-stu-id="b9364-108">Add this line under *dependencies*:</span></span> 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. <span data-ttu-id="b9364-109">Klicka på ikonen **Synkronisera projektet med Gradle-filer** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="b9364-109">Click the **Sync Project with Gradle Files** icon in the tool bar.</span></span>
6. <span data-ttu-id="b9364-110">Öppna **AndroidManifest.xml** och lägg till den här taggen till *program*-taggen.</span><span class="sxs-lookup"><span data-stu-id="b9364-110">Open **AndroidManifest.xml** and add this tag to the *application* tag.</span></span>
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

