toobegin med hjälp av Service Bus-köer i Azure, måste du först skapa ett namnområde med ett namn som är unik i Azure. Ett namnområde innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.

toocreate ett namnområde:

1. Logga in toohello [Azure-portalen][Azure portal].
2. Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **Enterprise Integration**, och klicka sedan på **Service Bus**.
3. I hello **skapa namnområdet** dialogrutan, ange ett namn för namnområdet. hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.
4. När gör att hello namnområdesnamnet är tillgänglig, kan du välja hello prisnivån (Basic, Standard och Premium).
5. I hello **prenumeration** , Välj en Azure-prenumeration i vilka toocreate hello-namnområdet.
6. I hello **resursgruppen** , Välj en befintlig resursgrupp i vilka hello namnområdet ska live eller skapa en ny.      
7. I **plats**, Välj hello land eller region där namnområdet ska finnas.
   
    ![Skapa namnområde][create-namespace]
8. Klicka på **Skapa**. hello systemet skapar namnområdet och aktiverar den. Du kanske toowait några minuter medan hello systemet tilldelar resurser för ditt konto.

### <a name="obtain-hello-management-credentials"></a>Hämta autentiseringsuppgifter för hantering av hello

1. Hello nyskapad namnområdesnamnet på hello listan namnområden.
2. I hello namnområde bladet, klickar du på **principer för delad åtkomst**.
3. I hello **principer för delad åtkomst** bladet, klickar du på **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. I hello **princip: RootManageSharedAccessKey** bladet Klicka hello kopiera bredvid för**sträng – primära anslutningsnyckel**, toocopy hello anslutning sträng tooyour Urklipp för senare användning. Klistra in det här värdet i Anteckningar eller på en tillfällig plats.
   
    ![connection-string][connection-string]

5. Hello Upprepa föregående steg, kopiera och klistra in hello värdet för **primärnyckel** tooa tillfällig plats för senare användning.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
