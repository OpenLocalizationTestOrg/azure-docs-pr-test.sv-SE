I följande tabell listar kvoter och gränser för Händelsehubbar i Azure. Information om priser för Händelsehubbar finns [priser för Händelsehubbar](https://azure.microsoft.com/pricing/details/event-hubs/).

| Gräns | Omfång | Typ | Beteende när överskridits | Värde |
| --- | --- | --- | --- | --- |
| Antal Händelsehubbar per namnområde |namnområde |Statisk |Efterföljande förfrågningar för att skapa ett nytt namnområde avvisas. |10 |
| Antalet partitioner per Event Hub |Entitet |Statisk |- |32 |
| Antal konsumentgrupper per Event Hub |Entitet |Statisk |- |20 |
| Antalet AMQP anslutningar per namnområde |namnområde |Statisk |Efterföljande begäranden om ytterligare anslutningar avvisas och ett undantag tas emot av den anropande koden. |5,000 |
| Maximal storlek för Händelsehubbar händelse|Hela systemet |Statisk |- |256 kB |
| Maximal storlek för en Event Hub-namn |Entitet |Statisk |- |50 tecken |
| Antal icke-epok mottagare per konsumentgrupp |Entitet |Statisk |- |5 |
| Högsta bevarandeperioden av händelsedata |Entitet |Statisk |- |1-7 dagar |
| Högsta antal Throughput Units |namnområde |Statisk |Genomströmning enhet överskrids gör att dina data så att begränsas och genererar en  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Du kan begära ett stort antal enheter för en Standard-lagret med ansökan en [supportbegäran](/azure/azure-supportability/how-to-create-azure-support-request). [Ytterligare genomflödesenheter](../articles/event-hubs/event-hubs-auto-inflate.md) är tillgängliga i block på 20 på bas allokerade inköp. |20 |

