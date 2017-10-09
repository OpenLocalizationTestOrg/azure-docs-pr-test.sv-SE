### <a name="prerequisites"></a>Krav
* En [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) konto  

Innan du kan använda SMTP-kontot i en logikapp, måste du godkänna hello logik app tooconnect tooyour SMTP-kontot. Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen.  

Här följer hello steg tooauthorize logik app tooconnect tooyour SMTP kontot:  

1. toocreate en anslutning tooSMTP, i hello logik app designer väljer **visa Microsoft hanterade API: er** i hello listrutan och ange sedan *SMTP* i hello sökrutan. Välj hello utlösare eller åtgärden som du kommer att gilla toouse:  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Om du inte skapat några anslutningar tooSMTP innan får du ange tooprovide SMTP-autentiseringsuppgifter. Dessa autentiseringsuppgifter att använda tooauthorize din logik app tooconnect till och åtkomst till SMTP-kontot data:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Observera hello anslutningen har skapats och du är nu ledigt tooproceed med hello andra steg i din logikapp:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

