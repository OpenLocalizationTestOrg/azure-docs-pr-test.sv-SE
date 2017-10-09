### <a name="prerequisites"></a>Krav
* Ett Twilio-konto
* Ett verifierade Twilio-telefonnummer som kan ta emot SMS
* Ett verifierade Twilio-telefonnummer som kan skicka SMS

> [!NOTE]
> Om du använder en utvärderingsversion Twilio-konto, du kan bara skicka SMS för**verifieras** telefonnummer.  
> 
> 

Innan du kan använda ditt Twilio-konto i en logikapp, måste du godkänna hello logik app tooconnect tooyour Twilio-konto. Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen. 

Här följer hello steg tooauthorize din logik app tooconnect tooyour Twilio-konto:

1. toocreate en anslutning tooTwilio, i hello logik app designer väljer **visa Microsoft hanterade API: er** i hello listrutan och ange sedan *Twilio* i hello sökrutan. Välj hello utlösare eller åtgärden som du kommer att gilla toouse:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Om du inte skapat några anslutningar tooTwilio innan får du ange tooprovide Twilio-autentiseringsuppgifter. Dessa autentiseringsuppgifter att använda tooauthorize din logik app tooconnect till och komma åt data i ditt Twilio-konto:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Du behöver hello **Twilio-konto-id** och **Twilio åtkomsttoken** hello instrumentpanel i Twilio, så logga in i tooyour Twilio-konto nu toograb dessa två typer av information:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio och Logic apps använda olika namn tooidentify dessa två typer av information. Här är hur måste du mappa dem toohello Logic apps dialogrutan:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Välj hello **Skapa anslutning** knappen:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Observera hello anslutningen har skapats och du är nu ledigt tooproceed med hello andra steg i din logikapp:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

