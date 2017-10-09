1. Öppna hello Android SDK Manager genom att klicka på ikonen hello hello verktygsfältet för Android Studio eller genom att klicka på **verktyg** -> **Android** -> **SDK Manager**på hello-menyn. Hitta hello Målversionen av hello Android SDK som används i ditt projekt, öppna den genom att klicka på **visa Paketinformation**, och välj **Google APIs**, om den inte redan är installerad.
2. Klicka på hello **SDK-verktyg** fliken. Om du inte redan har installerat Google Play-tjänsten klickar du på **Google Play Services** enligt nedan. Klicka på **tillämpa** tooinstall. 
   
    Observera hello SDK-sökvägen för användning i ett senare steg. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Öppna hello **build.gradle** fil i hello app.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Lägg till följande rad under *beroenden*: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Klicka på hello **synkronisera projektet med Gradle-filer** ikonen i verktygsfältet hello.
6. Öppna **AndroidManifest.xml** och Lägg till den här taggen toohello *programmet* tagg.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

