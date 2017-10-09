### <a name="prerequisites"></a>Krav
* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [Azure Blob Storage-konto](../articles/storage/common/storage-create-storage-account.md) inklusive hello lagringskontonamn och dess snabbtangent. Den här informationen visas i hello egenskaper för hello storage-konto i hello Azure-portalen. Läs mer om [Azure Storage](../articles/storage/common/storage-introduction.md).

Innan du använder Azure Blob Storage-konto i en logikapp, ansluta till tooyour Azure Blob Storage-konto. Du kan göra detta enkelt i din logikapp på hello Azure-portalen.  

Anslut tooyour Azure Blob Storage-konto med hello följande steg:  

1. Skapa en logikapp. Lägg till en utlösare i hello Logic Apps designer och lägga till en åtgärd. Välj **visa Microsoft hanterade API: er** i hello listrutan och ange sedan ”blob” i sökrutan för hello. Välj någon av hello åtgärder:  
   
    ![Azure Blob Storage anslutning skapas steg](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Om du tidigare har skapat alla anslutningar tooAzure lagring efterfrågas hello anslutningsinformation:   
   
    ![Azure Blob Storage anslutning skapas steg](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Ange hello lagringskontouppgifter. Egenskaper med en asterisk krävs.
   
   | Egenskap | Information |
   | --- | --- |
   | Anslutningsnamn * |Ange ett namn för anslutningen. |
   | Azure Storage-kontonamnet * |Ange hello lagringskontonamn. Hej lagringskontonamn visas i hello lagringsegenskaper i hello Azure-portalen. |
   | Azure Storage åtkomstnyckel * |Ange hello lagringskontonyckel. hello snabbtangenter visas i hello lagringsegenskaper i hello Azure-portalen. |
   
    Autentiseringsuppgifterna är används tooauthorize din logik app tooconnect och komma åt dina data. 
4. Välj **Skapa**.
5. Meddelande hello anslutningen har skapats. Nu kan fortsätta med hello andra steg i din logikapp: 
   
    ![Azure Blob Storage anslutning skapas steg](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

