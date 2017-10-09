### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Bevilja Mobile Engagement åtkomst tooyour GCM API-nyckel
tooallow Mobile Engagement toosend push-meddelanden för din räkning, behöver du toogrant den tillgång till tooyour API-nyckel. Detta görs genom att konfigurera och ange nyckeln i hello Mobile Engagement-portalen.

1. Från den klassiska Azure-portalen ser till att du är i hello app vi använder för det här projektet och klicka sedan på hello **starta** knappen längst ned hello:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Klicka på hello **inställningar** -> **Native Push** avsnittet tooenter din GCM-nyckel:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. Klicka på hello **redigera** ikonen framför **API-nyckel** i hello **GCM-inställningar** enligt nedan:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. Klistra in hello GCM-servernyckeln som du erhållit tidigare hello popup-fönstret och klicka sedan på **Ok**.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Skicka en avisering tooyour app
Nu skapar vi en enkel push-meddelandekampanj som skickar ett push-meddelande tooour app.

1. Navigera toohello **NÅ** fliken i din Mobile Engagement-portal.
2. Klicka på **nytt meddelande** toocreate push-meddelandekampanj.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. Ställ in hello första fältet i din kampanj med hello följande steg:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    a. Namnge din kampanj.
   
    b. Välj hello **Leveranstyp** som *Systemmeddelande -> enkelt*: Detta är hello enkel Android push-meddelandetypen som innehåller en rubrik och en kort textrad.
   
    c. Välj **leveranstiden** som *helst* tooallow hello app tooreceive ett meddelande om hello appen startas eller inte.
   
    d. I hello notification text typen hello **rubrik** som kommer att vara i fetstil i push hello.
   
    e. Sedan skriver du ditt **meddelande**
4. Rulla ned och i hello **innehåll** väljer **endast meddelande**.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Du är klar inställningen hello mest grundläggande kampanjen möjligt. Bläddra nedåt igen och klicka på hello **skapa** knappen toosave din kampanj.
6. Sista steget: Klicka på **aktivera** tooactivate kampanj toosend push-meddelanden.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

