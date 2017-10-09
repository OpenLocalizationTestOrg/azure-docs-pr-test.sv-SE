
1. Navigera toohello [Google Cloud-konsolen](https://console.developers.google.com/project), logga in med dina Google-konto. 
2. Klicka på **Skapa projekt**, ange ett projektnamn och klicka sedan på **Skapa**. Om det krävs utföra hello SMS-verifieringen och klicka på **skapa** igen.
   
    ![Skapa nytt projekt](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Ange ditt nya **projektnamn** och klicka på **Skapa projekt**.
3. Klicka på hello **verktyg och mer** knappen och klicka sedan på **projektinformation**. Anteckna hello **projektnumret**. Du behöver tooset värdet som hello `SenderId` variabeln i klientappen hello.
   
    ![Verktyg och mer](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. I hello projektet instrumentpanelen under **mobila API: er**, klickar du på **Google Cloud Messaging**, klicka på nästa sida i hello **aktivera API** och godkänner hello användarvillkoren. 
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Aktivera GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Klicka på instrumentpanelen för projektet hello **autentiseringsuppgifter** > **skapa autentiseringsuppgifter** > **API-nyckeln**. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. I **Skapa en ny nyckel** klickar du på **Servernyckel**, skriver in ett namn för din nyckel och klickar sedan på **Skapa**.
7. Anteckna hello **API-nyckel** värde.
   
    Du använder den här API-nyckelvärdet tooenable Azure tooauthenticate med GCM och skicka push-meddelanden för din app.

