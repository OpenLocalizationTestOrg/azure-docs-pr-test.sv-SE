> [!IMPORTANT]
> tooreceive Push-meddelanden från Mobile Engagement måste tooenable `Silent Remote Notifications` i ditt program. Du måste tooadd hello värdet remote-notification toohello UIBackgroundModes-matrisen i din Info.plist-fil.
> 
> 

1. Öppna `info.plist` fil i hello-projekt
2. Högerklicka på hello översta objektet i listan hello (`Information Property List`) och Lägg till en ny rad
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. Ange i hello ny rad`Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Klicka på hello VÄNSTERPIL tooexpand hello rad
5. Lägg till följande värde toohello objektet 0 hello`App downloads content in response toopush notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. När du har gjort hello ändra hello info.plist XML ska innehålla hello följande nyckel och värde:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. Om du använder **Xcode 7+** och **iOS 9+**:
   
   * Aktivera **Push-meddelanden** i Mål > Ditt målnamn > Funktioner.

