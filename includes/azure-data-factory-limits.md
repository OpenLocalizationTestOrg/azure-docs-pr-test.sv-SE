Datafabriken är en tjänst med flera klienter som har hello följer standardgränser som i plats toomake till kundprenumerationer skyddas från varandras arbetsbelastningar. Många av hello gränser kan enkelt höjas för din prenumeration in toohello högsta gränsen genom att kontakta supporten.

| **Resurs** | **Standardgräns** | **Övre gräns** |
| --- | --- | --- |
| datafabriker i en Azure-prenumeration |50 |[Kontakta supporten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| pipelines inom en datafabrik |2500 |[Kontakta supporten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| datauppsättningar inom en datafabrik |5000 |[Kontakta supporten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| samtidiga segment per dataset |10 |10 |
| byte per objekt för pipeline-objekt <sup>1</sup> |200 KB |200 KB |
| byte per objekt för datauppsättningen och länkade tjänstobjekt <sup>1</sup> |100 KB |2000 KB |
| HDInsight-kluster på begäran kärnor inom en prenumeration <sup>2</sup> |60 |[Kontakta supporten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Molnet flytt dataenheten <sup>3</sup> |32 |[Kontakta supporten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Antal för pipeline aktivitetskörningar försök |1000 |MaxInt (32-bitars) |

<sup>1</sup> pipeline, datamängd och länkade tjänstobjekt representerar en logisk gruppering av din arbetsbelastning. Gränser för de här objekten är tooamount av data som du kan flytta och bearbeta med hello Azure Data Factory-tjänsten inte är relaterade. Data factory är utformad tooscale toohandle petabyte med data.

<sup>2</sup> kärnor på begäran HDInsight tilldelas utanför hello-prenumeration som innehåller hello data factory. Därför är hello överskrider hello Data Factory tvingande core gränsen för på begäran HDInsight kärnor och skiljer sig från hello core gräns som är associerad med din Azure-prenumeration.

<sup>3</sup> moln data movement enhet (dmu här) används i en moln-to-cloud kopieringsåtgärd. Det är ett mått som representerar hello styrka (en kombination av CPU, minne och nätverksresursallokering) en enhet i Data Factory. Du kan uppnå högre kopiera genomströmning genom att utnyttja mer DMUs för vissa scenarier. Se för[molnet data movement enheter](../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) avsnittet detaljer.

| **Resurs** | **Standard nedre gräns** | **Minimigräns** |
| --- | --- | --- |
| Planering intervallet |15 minuter |15 minuter |
| Intervall mellan försök |1 sekund |1 sekund |
| Gör timeout-värde |1 sekund |1 sekund |

### <a name="web-service-call-limits"></a>Web service anropet gränser
Azure Resource Manager har begränsningar för API-anrop. Du kan göra API-anrop med en hastighet inom hello [Azure Resource Manager API begränsar](../articles/azure-subscription-service-limits.md#resource-group-limits).
