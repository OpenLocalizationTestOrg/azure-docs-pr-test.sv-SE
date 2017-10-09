1. <span data-ttu-id="02e98-101">Öppna hello Android SDK Manager genom att klicka på ikonen hello hello verktygsfältet för Android Studio eller genom att klicka på **verktyg** -> **Android** -> **SDK Manager**på hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="02e98-101">Open hello Android SDK Manager by clicking hello icon on hello toolbar of Android Studio or by clicking **Tools** -> **Android** -> **SDK Manager** on hello menu.</span></span> <span data-ttu-id="02e98-102">Hitta hello Målversionen av hello Android SDK som används i ditt projekt, öppna den genom att klicka på **visa Paketinformation**, och välj **Google APIs**, om den inte redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="02e98-102">Locate hello target version of hello Android SDK that is used in your project , open it by clicking **Show Package Details**, and choose **Google APIs**, if it is not already installed.</span></span>
2. <span data-ttu-id="02e98-103">Klicka på hello **SDK-verktyg** fliken. Om du inte redan har installerat Google Play-tjänsten klickar du på **Google Play Services** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="02e98-103">Click hello **SDK Tools** tab. If you haven't already installed Google Play Service, click **Google Play Services** as shown below.</span></span> <span data-ttu-id="02e98-104">Klicka på **tillämpa** tooinstall.</span><span class="sxs-lookup"><span data-stu-id="02e98-104">Then click **Apply** tooinstall.</span></span> 
   
    <span data-ttu-id="02e98-105">Observera hello SDK-sökvägen för användning i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="02e98-105">Note hello SDK path, for use in a later step.</span></span> 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. <span data-ttu-id="02e98-106">Öppna hello **build.gradle** fil i hello app.</span><span class="sxs-lookup"><span data-stu-id="02e98-106">Open hello **build.gradle** file in hello app directory.</span></span>
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. <span data-ttu-id="02e98-107">Lägg till följande rad under *beroenden*:</span><span class="sxs-lookup"><span data-stu-id="02e98-107">Add this line under *dependencies*:</span></span> 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. <span data-ttu-id="02e98-108">Klicka på hello **synkronisera projektet med Gradle-filer** ikonen i verktygsfältet hello.</span><span class="sxs-lookup"><span data-stu-id="02e98-108">Click hello **Sync Project with Gradle Files** icon in hello tool bar.</span></span>
6. <span data-ttu-id="02e98-109">Öppna **AndroidManifest.xml** och Lägg till den här taggen toohello *programmet* tagg.</span><span class="sxs-lookup"><span data-stu-id="02e98-109">Open **AndroidManifest.xml** and add this tag toohello *application* tag.</span></span>
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

