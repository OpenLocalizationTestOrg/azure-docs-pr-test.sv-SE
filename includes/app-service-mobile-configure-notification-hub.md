hello Mobile Apps i Azure Apptjänst använder [Azure Notification Hubs] toosend push-meddelanden, så att du ska konfigurera en meddelandehubb för din mobila app.

1. I hello [Azure-portalen], gå för**Apptjänster**, och klicka sedan på din app-serverdel. Under **inställningar**, klickar du på **Push**.
2. Klicka på **Anslut** tooadd en notification hub resurs toohello app. Du kan antingen skapa ett nav eller ansluta tooan befintliga en.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Nu har du anslutit en notification hub tooyour Mobile Apps backend-projekt. Du kommer senare att konfigurera den här notification hub tooconnect tooa platform notification system (PNS) toopush toodevices.

[Azure-portalen]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
