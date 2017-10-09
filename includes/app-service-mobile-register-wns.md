
1. Högerklicka på hello Windows Store-app-projekt i Visual Studio Solution Explorer och klicka på **Store** > **associera appen med hello Store**.

    ![Associera appen med Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Klicka på hello guiden **nästa**, och logga in med ditt Microsoft-konto. Skriv ett namn för din app i **reservera ett nytt namn för appen**, och klicka sedan på **reservera**.
3. När hello appen registreras har skapats, Välj hello nya namn, klickar du på **nästa**, och klicka sedan på **associera**. Detta lägger till hello som krävs för Windows Store-registrering information toohello programmanifestet.
4. Upprepa steg 1 och 3 för hello Windows Phone Store-app-projekt med hjälp av hello samma registrering som du tidigare skapade för hello Windows Store-app.  
5. Bläddra toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), och logga in med ditt Microsoft-konto. Klicka på hello nya appregistrering i **Mina appar**, och expandera sedan **Services** > **Push-meddelanden**.
6. På hello **Push-meddelanden** klickar du på **webbplatsen Live-tjänster** under **Windows Push Notification Services (WNS) och Microsoft Azure Mobile Apps**. Anteckna hello värdena för hello **paket-SID** och hello *aktuella* värde i **Programhemlighet**. 

    ![Appinställningen i hello developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Hej programhemlighet och paket-SID är viktiga säkerhetsuppgifter. Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.
   >
   >
