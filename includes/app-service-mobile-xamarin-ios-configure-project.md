#### <a name="configure-hello-ios-project-in-xamarin-studio"></a>Konfigurera hello iOS-projekt i Xamarin Studio
1. Öppna i Xamarin.Studio, **Info.plist**, och uppdatera hello **Paketidentifierare** med hello paketera ID som du skapade tidigare med din nya app-ID.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Rulla nedåt för**bakgrundslägen**. Välj hello **aktivera bakgrundslägen** rutan och hello **Remote notifications** rutan.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Dubbelklicka på ditt projekt i hello lösning panelen tooopen **Projektalternativ**.
4. Under **skapa**, Välj **iOS paket signering**, och välj hello motsvarande identitet och etableringsprofil du bara ställa in för det här projektet.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Detta säkerställer att hello-projekt använder hello ny profil för kodsignering. Hello officiella Xamarin för etablering av enheter dokumentation, se [Xamarin Enhetsetableringen].

#### <a name="configure-hello-ios-project-in-visual-studio"></a>Konfigurera hello iOS-projekt i Visual Studio
1. Högerklicka på hello-projekt i Visual Studio och klicka sedan på **egenskaper**.
2. Klicka i hello egenskapssidor hello **iOS programmet** flik och uppdatera hello **identifierare** med hello-ID som du skapade tidigare.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. I hello **iOS paket signering** fliken väljer hello motsvarande identitet och provisioning-profil du just ställt in för det här projektet.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Detta säkerställer att hello-projekt använder hello ny profil för kodsignering. Hello officiella Xamarin för etablering av enheter dokumentation, se [Xamarin Enhetsetableringen].
4. Dubbelklicka på Info.plist tooopen och sedan aktivera **RemoteNotifications** under **bakgrundslägen**.

[Xamarin Enhetsetableringen]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
