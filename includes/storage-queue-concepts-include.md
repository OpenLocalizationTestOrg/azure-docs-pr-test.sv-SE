## <a name="what-is-queue-storage"></a>Vad är Queue Storage?
Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.

Vanliga användningsområden för Queue Storage är:

* Skapa en eftersläpning i arbetet tooprocess asynkront
* Skicka meddelanden från en Azure web rollen tooan Azure-arbetsroll

## <a name="queue-service-concepts"></a>Kötjänst-koncept
hello kötjänsten innehåller hello följande komponenter:

![Kö1](./media/storage-queue-concepts-include/queue1.png)

* **URL-format:** köer är adresserbara via följande URL-format hello:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    hello följande URL adresserar en kö i hello diagram:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto. Se [Skalbarhets- och prestandamål för Azure Storage](../articles/storage/common/storage-scalability-targets.md) för information om kapacitet för lagringskonton.
* **Kö:** en kö innehåller en uppsättning meddelanden. Alla meddelanden måste vara i en kö. Observera att hello könamnet måste vara gemener. Mer information om namngivning av köer finns i [namngivning av köer och metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Meddelande:** A meddelandet, i valfritt format för in too64 KB. hello maximala tid som ett meddelande kan finnas i hello kön är 7 dagar.

