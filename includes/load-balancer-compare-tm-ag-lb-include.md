## <a name="load-balancer-differences"></a>Skillnader i belastningsutjämnare

Det finns olika alternativ toodistribute nätverkstrafik med hjälp av Microsoft Azure. De här alternativen fungerar på olika sätt, har olika funktionsuppsättningar och stöder olika scenarier. De kan användas var för sig eller i kombination med varandra.

* **Azure belastningsutjämnare** fungerar på hello transportlagret (lager 4 i hello OSI-nätverksstacken referens). Det ger på nätverksnivå fördelning av trafik mellan instanser av ett program som körs i hello samma Azure-datacenter.
* **Programgateway** fungerar på programnivå hello (Layer 7 i hello OSI-nätverksstacken referens). Det fungerar som en omvänd proxy-tjänst, avslutar hello klientanslutningen och vidarebefordran begär tooback slutpunkt slutpunkter.
* **Traffic Manager** fungerar på hello DNS-nivå.  Den använder DNS-svar toodirect slutanvändarens trafik tooglobally distribuerade slutpunkter. Klienter ansluter sedan toothose slutpunkter som är direkt.

hello följande tabell sammanfattas hello-funktioner som erbjuds av varje tjänst:

| Tjänst | Azure Load Balancer | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Teknologi |Transportnivå (Layer 4) |Programnivå (Layer 7) |DNS-nivå |
| Programprotokoll som stöds |Alla |HTTP, HTTPS och WebSockets |Alla (en HTTP-slutpunkt krävs för slutpunktsövervakning) |
| Slutpunkter |Azure VM:ar och Cloud Services-rollinstanser |Alla interna Azure-IP-adresser, offentliga Internet-IP-adresser, virtuella Azure-datorer och Azure-molntjänster |Azure VM:ar, Cloud Services, Azure Web Apps och externa slutpunkter |
| Vnet-stöd |Kan användas för både Internetriktade och interna (Vnet)-program |Kan användas för både Internetriktade och interna (Vnet)-program |Stöder enbart Internetriktade program |
| Slutpunktsövervakning |Stöds via avsökningar |Stöds via avsökningar |Stöds via HTTP/HTTPS GET |

Azure belastningsutjämnare och Programgateway väg nätverket trafik tooendpoints, men de har olika användning scenarier toowhich trafik toohandle. hello kan följande tabell förstå hello skillnaden mellan hello två belastningsutjämnare:

| Typ | Azure Load Balancer | Application Gateway |
| --- | --- | --- |
| Protokoll |UDP/TCP |HTTP, HTTPS och WebSockets |
| IP-reservation |Stöds |Stöds inte |
| Belastningsutjämningsläge |5-tuppel(käll-IP, källport, mål-IP, målport, protokolltyp) |Resursallokering<br>Routning baserat på URL |
| Belastningsutjämningsläge(käll-IP/fästa sessioner) |2-tuppel (käll-IP och mål-IP), 3-tuppel (käll-IP, mål-IP och port). Skala upp eller ned baserat på hello antalet virtuella datorer |Cookie-baserad tillhörighet<br>Routning baserat på URL |
| Hälsotillståndsavsökningar |Standardvärde: avsökningsintervall - 15 sekunder. Tagen ur rotation: 2 kontinuerlig fel. Stöder användardefinierade avsökningar |Vilande avsökningsintervall 30 sekunder. Tas ut efter 5 efterföljande trafikfel live eller ett enda avsökningsfel i vilande läge. Stöder användardefinierade avsökningar |
| SSL-avlastning |Stöds inte |Stöds |
| URL-baserad routning | Stöds inte | Stöds|
| SSL-princip | Stöds inte | Stöds|
