Lagring är begränsad av diskutrymme eller av en hård gräns hello *maximalt antal* av index eller dokument, beroende på vilket som kommer först.

| Resurs | Kostnadsfri | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Serviceavtal (SLA) |Nr <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Lagringsutrymme per partition |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partitioner per tjänst |Saknas |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsstorlek |Saknas |2 GB |25 GB |100 GB |200 GB |200 GB |
| Repliker |Saknas |3 |12 |12 |12 |12 |
| Maximalt antal index |3 |5 |50 |200 |200 |1 000 per partition eller 3 000 per tjänst |
| Maximalt antal indexerare |3 |5 |50 |200 |200 |Inget stöd för indexerare |
| Maximalt antal datakällor |3 |5 |50 |200 |200 |Inget stöd för indexerare |
| Maximalt antal dokument |10 000 |1 miljon |15 miljoner per partition eller 180 miljoner per tjänst |60 miljoner per partition eller 720 miljoner per tjänst |120 miljoner per partition eller 1,4 miljarder per tjänst |1 miljon per index eller 200 miljoner per partition |
| Uppskattat antal frågor per sekund (QPS) |Saknas |~ 3 per replik |~ 15 per replik |~ 60 per replik |~ 60 per replik |>60 per replik |

<sup>1</sup> ledigt nivå och förhandsgranska funktioner inte kommer med servicenivåavtal (SLA). För alla fakturerbar nivåer börjar SLA gälla när du etablerar tillräcklig redundans för din tjänst. Två eller flera repliker krävs för SERVICENIVÅAVTAL för frågan (läsa). Tre eller flera repliker krävs för fråga och indexering SLA (skrivskyddad). hello antalet partitioner är inte ett SLA-faktor. 

<sup>2</sup> S3 HD har en fast gräns 3 partitioner som är lägre än hello partition gränsen för S3. hello begränsningar lägre partition eftersom hello index antal för S3 HD är avsevärt högre. Baserat på att tjänsten gränser finns för både dataresurser (lagring och bearbetning) och innehåll (index och dokument) hello innehåll gränsen har nåtts först.
