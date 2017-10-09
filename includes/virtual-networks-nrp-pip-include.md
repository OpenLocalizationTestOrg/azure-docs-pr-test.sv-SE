## <a name="public-ip-address"></a>Offentlig IP-adress
En offentlig IP-adressresurs innehåller antingen en reserverad eller dynamiska Internetuppkopplad IP-adress. Även om du kan skapa en offentlig IP-adress som ett fristående objekt, måste du tooassociate den tooanother objektet tooactually använda hello-adress. Du kan associera en offentlig belastningsutjämnare för IP-adress tooa, Programgateway eller en NIC tooprovide Internet access toothose resurser.  

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **publicIPAllocationMethod** |Definierar om hello IP-adressen är *Statiska* eller *dynamiska*. |statisk, dynamisk |
| **idleTimeoutInMinutes** |Definierar hello inaktiv timeout med ett standardvärde på 4 minuter. Om inga fler paket för en viss session har tagits emot inom den angivna tiden avslutas hello sessionen. |ett värde mellan 4 och 30 |
| **IP-adress** |IP-adress som tilldelats tooobject. Det här är en skrivskyddad egenskap. |104.42.233.77 |

### <a name="dns-settings"></a>DNS-inställningar
Offentliga IP-adresser ha ett underordnat objekt med namnet **dnsSettings** som innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **domainNameLabel** |Värden som har namnet används för namnmatchning. |www, ftp, vm1 |
| **FQDN** |Fullständigt kvalificerade namnet för hello offentlig IP. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |Fullständigt kvalificerat domännamn som matchar toohello IP-adress och är registrerad i DNS som en PTR-post. |www.contoso.com. |

Exempel offentliga IP-adressen i JSON-format:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Ytterligare resurser
* Hämta mer information [offentliga IP-adresser](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* Lär dig mer om [instansen nivån offentliga IP-adresser](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) för det offentliga IP-adresser.

