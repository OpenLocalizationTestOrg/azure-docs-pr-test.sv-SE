

1. På en Mac starta **Nyckelhanterare**. På hello kvar navigeringsfältet under **kategori**öppnar **mina certifikat**. Hitta hello SSL-certifikat du hämtade i hello föregående avsnitt och lämna ut innehållet. Välj endast hello certifikat (Välj inte hello privat nyckel), och [exportera det](https://support.apple.com/kb/PH20122?locale=en_US).
2. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **Bläddra bland alla** > **Apptjänster**, och klicka på din Mobile Apps-serverdel. Under **inställningar**, klickar du på **App Service Push**, och klicka sedan på namnet på din meddelandehubb. Gå för**Apple Push Notification Services** > **överför certifikat**. Överför hello .p12-filen som du väljer rätt hello **läge** (beroende på om din SSL-klientcertifikat från tidigare är produktion eller sandbox). Spara ändringarna.

Tjänsten är nu konfigurerad toowork med push-meddelanden i iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
