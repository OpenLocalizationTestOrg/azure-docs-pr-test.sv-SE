

## <a name="generate-hello-certificate-signing-request-file"></a>Generera filen för hello begäran om Certifikatsignering
hello Apple Push Notification Service (APNS) använder certifikat tooauthenticate push-meddelanden. Följ dessa instruktioner toocreate hello nödvändiga push-certifikat toosend och ta emot meddelanden. Mer information om dessa koncept finns hello officiella [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentation.

Generera hello begäran om certifikatsignering (CSR)-filen används av Apple toogenerate ett signerat push-certifikat.

1. Kör hello Nyckelhanteraren på din Mac.. Det kan öppnas från hello **verktyg** mapp eller hello **andra** mapp på hello startmenyn.
2. Klicka på **Nyckelhanteraren**, expandera **Certifikatassistenten** och klicka sedan på **Begär ett certifikat från en certifikatutfärdare...**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Välj din **användarens e-postadress** och **nätverksnamn** , se till att **sparade toodisk** är markerad och klicka sedan på **Fortsätt**. Lämna hello **CA e-postadress** fältet tomt eftersom det inte är obligatoriskt.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Skriv ett namn för hello begäran om certifikatsignering (CSR)-fil i **Spara som**, Välj hello plats **där**, klicka på **spara**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Detta sparar hello CSR-filen i hello valt plats. hello standardplatsen är hello skrivbordet. Kom ihåg hello plats du valde för den här filen.

Du kommer sedan registrera din app med Apple, aktivera push-meddelanden och ladda upp det här exporterade CSR-toocreate ett push-certifikat.

## <a name="register-your-app-for-push-notifications"></a>Registrera din app för push-meddelanden
toobe kan toosend push-meddelanden tooan iOS-app, måste du registrera programmet med Apple och registrera dig för push-meddelanden.  

1. Om du inte redan har registrerat appen navigerar toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center, loggar in med ditt Apple-ID, klickar du på **identifierare**, klicka på **App-ID: N** , och klickar slutligen på hello  **+**  logga tooregister en ny app.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. Uppdatera hello följande tre fält för din nya app och klicka sedan på **Fortsätt**:
   
   * **Namnet**: Ange ett beskrivande namn för din app i hello **namn** i hello **App-ID beskrivning** avsnitt.
   * **Paketidentifierare**: Under hello **uttrycklig App-ID** ange en **Paketidentifierare** i form av hello `<Organization Identifier>.<Product Name>` som anges i hello [App-Distribution Guiden](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Hej *Organisationsidentifierare* och *produktnamn* du använder måste matcha hello organisations-ID och produktnamn som du använder när du skapar ditt XCode-projekt. I hello skärmbilden nedan *NotificationHubs* används som en organisationsidentifierare och *GetStarted* används som hello produktnamn. Kontrollera att detta matchar hello-värden som du ska använda i ditt XCode-projekt kan du toouse hello rätt publiceringsprofil med XCode. 
   * **Push-meddelanden**: Kontrollera hello **Push-meddelanden** alternativ i hello **Apptjänster** avsnittet.
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      Detta genererar din App-ID och begär tooconfirm hello information. Klicka på **registrera** tooconfirm hello nya App-ID.
     
      När du klickar på **registrera**, ser du hello **registreringen har slutförts** skärmen, enligt nedan. Klicka på **Klar**.
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. Leta upp hello app-ID som du just skapade och klickar på dess rad i hello Developer Center under App-ID: N.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      Klicka på hello app-ID visas information om hello appar. Klicka på hello **redigera** knappen längst ned hello.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. Bläddra toohello ned hello-skärmen och klicka på hello **skapa certifikat...**  knappen under hello avsnittet **Development Push SSL-certifikat**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      Detta visar hello ”Lägg till iOS-certifikat” Installationsassistenten.
   
   > [!NOTE]
   > Den här guiden använder ett utvecklarcertifikat. hello samma process som används när du registrerar ett driftscertifikat. Kontrollera att du använder hello samma certifikattyp när du skickar meddelanden.
   > 
   > 
3. Klicka på **Välj fil**, bläddra toohello plats där du sparade hello CSR-filen som du skapade i hello första uppgiften och klicka på **generera**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. När hello certifikatet har skapats av hello portal, klickar du på hello **hämta** och klickar på **klar**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      Detta hämtar hello certifikat och sparar den tooyour dator i din mapp för nedladdningar.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Som standard hello ned filen ett utvecklingscertifikat **aps_development.cer**.
   > 
   > 
5. Dubbelklicka på hello hämtade push-certifikatet **aps_development.cer**.
   
      Detta installerar hello nytt certifikat i hello nyckelringen enligt nedan:
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > hello namn i certifikatet kan skilja sig, men det inleds med **Apple Development iOS Push Services:**.
   > 
   > 
6. I Nyckelhanteraren högerklickar du på hello nya push-certifikat som du skapade i hello **certifikat** kategori. Klicka på **exportera**, namnge hello-filen, Välj hello **.p12** formatera och klicka sedan på **spara**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    Se anteckna hello filnamnet och platsen för hello .p12-certifikatet exporterats. Den kommer att använda tooenable autentisering med APNS.
   
   > [!NOTE]
   > Den här guiden skapar en QuickStart.p12-fil. Filnamnet och sökvägen kan vara olika för dig.
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a>Skapa en etableringsprofil för hello app
1. Tillbaka i hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>väljer **Etableringsprofiler**väljer **alla**, och klicka sedan på hello  **+**  knappen toocreate en ny profil. Detta startar hello **Lägg till iOS-Etableringsprofil** guiden
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Välj **iOS App Development** under **Development** som hello etableringsprofiltyp och klicka på **Fortsätt**. 
3. Välj därefter hello app-ID som du nyss skapat från hello **App-ID** listrutan och klicka på **Fortsätt**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. I hello **Välj certifikat** skärmen, Välj ditt vanliga utvecklingscertifikat som används för kodsignering och klicka på **Fortsätt**. Detta är inte hello push-certifikat som du nyss skapade.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Välj därefter hello **enheter** toouse för testning och klickar på **Fortsätt**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Slutligen väljer du ett namn för profilen hello i **profilnamn**, klickar du på **generera**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. När hello nya etableringsprofil som har skapats klickar du på toodownload den och installera den på din Xcode-utvecklingsdator. Klicka sedan på **Klar**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
