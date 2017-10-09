### <a name="prerequisites"></a>Krav
* En [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) konto  

Innan du kan använda SFTP-konto i en logikapp, måste du godkänna hello logik app tooconnect tooyour SFTP konto. Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen.  

Här följer hello steg tooauthorize logik app tooconnect tooyour SFTP kontot:  

1. toocreate en anslutning tooSFTP, i hello logik app designer väljer **visa Microsoft hanterade API: er** i hello listrutan och ange sedan *SFTP* i hello sökrutan. Välj hello **SFTP - när en fil har lagts till eller ändrats** utlösare:  
   ![SFTP online bild 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Om du inte skapat några anslutningar tooSFTP innan får du ange tooprovide SFTP-autentiseringsuppgifter. Dessa autentiseringsuppgifter att använda tooauthorize din logik app tooconnect till och komma åt ditt konto SFTP data:  
   ![SFTP online bild 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Observera hello anslutningen har skapats och du är nu ledigt tooproceed med hello andra steg i din logikapp:   
   ![SFTP online bild 3](./media/connectors-create-api-sftp/sftp-3.png) 

