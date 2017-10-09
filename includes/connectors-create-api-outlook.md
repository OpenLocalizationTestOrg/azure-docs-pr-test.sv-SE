## <a name="connect-toooutlookcom"></a>Ansluta tooOutlook.com
### <a name="prerequisites"></a>Krav
* En Outlook.com-konto

Innan du kan använda din Outlook.com-konto i en logikapp, måste du godkänna hello logik app tooconnect tooyour Outlook.com konto. Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen. 

Här följer hello steg tooauthorize din logik app tooconnect tooyour Outlook.com konto:

1. Alla Logic apps behöver toobe startas av en utlösare så när du skapar din logikapp, hello designer öppnas och visar en lista över utlöser som du kan använda toostart logikappen:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Ange ”outlook” i sökrutan för hello. Meddelande hello listan är filtrerad toolist alla hello utlösare med ”Outlook” hello namn:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Välj **Office 365 Outlook - på nytt e-postmeddelande**.   
   Om du inte skapat några anslutningar tooOutlook innan får du ange tooprovide Outlook.com-autentiseringsuppgifter. Dessa autentiseringsuppgifter att använda tooauthorize din logik app tooconnect till och komma åt data i din Outlook.com-konto:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Ange dina autentiseringsuppgifter för Outlook och logga in:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   Det var allt. Du har nu skapat en anslutning tooOutlook. Den här anslutningen ska vara tillgängligt för användning i andra logikappen som du skapar.

