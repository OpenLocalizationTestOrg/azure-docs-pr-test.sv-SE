## <a name="what-are-service-bus-topics-and-subscriptions"></a>Vad är Service Bus-ämnen och -prenumerationer?
Service Bus-ämnen och -prenumerationer stöder en *publicera/prenumerera*-modell för meddelandekommunikation. När du använder ämnen och prenumerationer så kommunicerar inte komponenterna i ett distribuerat program direkt med varandra. Istället så utbyter de meddelanden via ett ämne, som agerar mellanhand.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Till skillnad från Service Bus-köer, där varje meddelande bearbetas av en enskild konsument så ger ämnen och prenumerationer en ”ett till många”-kommunikation med hjälp av ett publicera/prenumerera-mönster. Det är möjligt att registrera flera prenumerationer tooa avsnittet. När ett meddelande skickas tooa avsnittet, görs sedan tillgänglig tooeach prenumeration toohandle/bearbeta oberoende av varandra.

Ett prenumeration tooa ämne liknar en virtuell kö som tar emot kopior av hello-meddelanden som skickades toohello avsnittet. Alternativt kan du registrera filterregler för ett ämne på grundval av per prenumeration, vilket gör att du toofilter eller begränsa vilka meddelanden tooa avsnittet tas emot av vilka ämnesprenumerationer.

Service Bus-ämnen och prenumerationer du tooscale och bearbeta ett mycket stort antal meddelanden för många användare och program.

## <a name="create-a-namespace"></a>Skapa ett namnområde
toobegin som använder Service Bus-ämnen och prenumerationer i Azure, måste du först skapa en *namnområde för tjänsten*. Ett namnområde innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.

toocreate ett namnområde:

1. Logga in toohello [Azure-portalen][Azure portal].
2. Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **Enterprise Integration**, och klicka sedan på **Service Bus**.
3. I hello **skapa namnområdet** dialogrutan, ange ett namn för namnområdet. hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.
4. När gör att hello namnområdesnamnet är tillgänglig, kan du välja hello prisnivån (Basic, Standard och Premium).
5. I hello **prenumeration** , Välj en Azure-prenumeration i vilka toocreate hello-namnområdet.
6. I hello **resursgruppen** , Välj en befintlig resursgrupp i vilka hello namnområdet ska live eller skapa en ny.      
7. I **plats**, Välj hello land eller region där namnområdet ska finnas.
   
    ![Skapa namnområde][create-namespace]
8. Klicka på hello **skapa** knappen. hello systemet skapar namnområdet och aktiverar den. Du kanske toowait några minuter medan hello systemet tilldelar resurser för ditt konto.

### <a name="obtain-hello-credentials"></a>Hämta hello autentiseringsuppgifter
1. Hello nyskapad namnområdesnamnet på hello listan namnområden.
2. I hello **Service Bus-namnrymd** bladet, klickar du på **principer för delad åtkomst**.
3. I hello **principer för delad åtkomst** bladet, klickar du på **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. I hello **princip: RootManageSharedAccessKey** bladet Klicka hello kopiera bredvid för**sträng – primära anslutningsnyckel**, toocopy hello anslutning sträng tooyour Urklipp för senare användning.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


