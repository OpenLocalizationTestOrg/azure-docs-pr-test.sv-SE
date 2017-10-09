### <a name="prerequisites"></a>Krav
Du måste ha en [Service Bus](https://azure.microsoft.com/services/service-bus/) konto.  

Innan du kan använda Azure Service Bus-konto i en logikapp, måste du godkänna hello logik app tooconnect tooyour bus tjänstkonto. Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen.  

Här följer hello steg tooauthorize din logik app tooconnect tooyour Service Bus-konto:  

1. toocreate anslutning tooService Bus, i hello logik app designer väljer **visa Microsoft hanterade API: er** i hello nedrullningsbara listan. Ange **service bus** i hello sökrutan. Välj hello utlösare eller åtgärden som du vill toouse.  
    ![Bild 1 Service Bus](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Om du inte har skapat några anslutningar tooService Bus innan du kommer att tillfrågas tooprovide Service Bus-autentiseringsuppgifter. Dessa autentiseringsuppgifter har använt tooauthorize din logik app tooconnect tooand dataåtkomst för ditt Service Bus-konto. hello Service Bus-anslutningen behöver hello anslutningssträngen för hello Service Bus-namnrymd. Det kräver också **hantera** behörigheter. Ett bra sätt tooknow om anslutningssträngen för hello namnområde eller en specifik entitet om den innehåller hello `EntityPath` parameter. Om den finns, men det är inte hello rätt anslutningssträng för en logikapp.  
    ![Service Bus-anslutningssträng](./media/connectors-create-api-servicebus/connectionstring.png)
3. När du har fått hello anslutningssträngen för hello namnområdet, kan du använda det för hello API-anslutningen i Logic Apps.  
    ![Bild 2 Service Bus](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Observera hello anslutningen har skapats och du är nu ledigt tooproceed med hello andra steg i din logikapp.  
    ![Bild 3 Service Bus](./media/connectors-create-api-servicebus/servicebus-3.png)   

