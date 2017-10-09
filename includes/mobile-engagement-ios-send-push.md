### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a>Bevilja åtkomst tooyour Push-certifikat tooMobile Engagement
tooallow Mobile Engagement toosend Push-meddelanden för din räkning, behöver du toogrant den tillgång till tooyour certifikat. Detta görs genom att konfigurera och ange ditt certifikat i hello Mobile Engagement-portalen. Kontrollera att du har fått ditt .p12-certifikat enligt beskrivningen i [Apples dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

1. Navigera tooyour Mobile Engagement-portalen. Se till att du befinner dig i rätt hello och klicka sedan på hello **starta** knappen längst ned hello:
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. Klicka på hello **inställningar** sida i Engagement-portalen. Därifrån klickar du på hello **Native Push** avsnittet tooupload ditt p12-certifikat:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. Välj ditt p12, överför det och ange ditt lösenord:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Skicka en avisering tooyour app
Nu ska vi skapa en enkel Push-meddelandekampanj som skickar ett push tooour app:

1. Navigera toohello **nå** fliken i din Mobile Engagement-portal.
2. Klicka på **nytt meddelande** toocreate push-kampanj
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. Konfigurera hello första fälten i din kampanj:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * Ge din kampanj ett **namn** 
   * Välj hello **leveranstiden** som **endast utanför appen**: Detta är hello enkla Apple push-meddelandetypen som innehåller text.
   * Ange första hello i hello meddelandetext **rubrik** som kommer att vara hello första raden i hello push.
   * Skriv din **meddelandet** som kommer att vara hello andra raden
4. Bläddra nedåt och i hello innehållsavsnitt väljer **endast meddelande**
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. Du är klar inställningen hello mest grundläggande kampanjen. Bläddra nedåt och klicka på **skapa** knappen toosave push-meddelandekampanj. 
6. Slutligen klickar du på **aktivera** toosend push-meddelande. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Kan ta emot hello-meddelande på din iOS-enhet i hello meddelandecentret hello följande:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Om du har en Apple Watch parad med iOS-enheten ska du se hello-meddelande på din Apple Watch:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

