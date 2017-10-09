## <a name="configure-your-application-tooaccess-azure-storage"></a>Konfigurera ditt program tooaccess Azure Storage
Det finns två sätt tooauthenticate ditt program tooaccess Storage-tjänster:

* Delad nyckel: Använd delad nyckel endast för testning
* Signatur för delad åtkomst (SAS): Använd SAS för program i produktion

### <a name="shared-key"></a>Delad nyckel
Autentisering med delad nyckel innebär att programmet ska använda ditt kontonamn och kontot viktiga tooaccess lagringstjänster. Hello enligt snabbt visar hur toouse det här biblioteket, kommer vi att använda autentisering med delad nyckel i den här komma igång.

> [!WARNING] 
> **Endast använda autentisering med delad nyckel för testning!** Kontonamn och kontonyckel, vilket ger fullständig läsning och skrivning åtkomst toohello associerade lagringskontot, distribuerade tooevery person som hämtas av din app. Detta är **inte** en bra rutin som riskerar du att din nyckel av icke-betrodda klienter.
> 
> 

När du använder autentisering med delad nyckel, skapar du en [anslutningssträngen](../articles/storage/common/storage-configure-connection-string.md). hello anslutningssträngen består av:  

* Hej **DefaultEndpointsProtocol** -du kan välja HTTP eller HTTPS. Men rekommenderas om du använder HTTPS starkt.
* Hej **kontonamn** - hello namnet på ditt lagringskonto
* Hej **Kontonyckel** - på hello [Azure Portal](https://portal.azure.com), navigera tooyour storage-konto och klicka på hello **nycklar** ikonen toofind informationen.
* (Valfritt) **EndpointSuffix** -används för lagringstjänster i områden med olika endpoint suffix, till exempel Azure Kina eller Azure-styrning.

Här är ett exempel på anslutningssträngen med hjälp av autentisering med delad nyckel:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Signaturer för delad åtkomst (SAS)
För ett mobilt program är hello rekommenderad metod för att autentisera en begäran av en klient mot hello Azure Storage-tjänst genom att använda en delad signatur åtkomst (SAS). SAS kan du toogrant en klient åtkomst tooa resurs för en angiven tidsperiod, med en angiven uppsättning behörigheter.
Som hello lagringskontoägaren behöver toogenerate en SAS för din mobila klienter tooconsume. toogenerate Hej SAS, vill du förmodligen toowrite en separat tjänst som genererar hello SAS toobe distribuerade tooyour klienter. I testsyfte kan du använda hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) eller hello [Azure Portal](https://portal.azure.com) toogenerate en SAS. När du skapar hello SAS kan ange du hello tidsintervall under vilka hello SAS är giltigt och hello behörigheter som hello SAS ger toohello klienten.

hello som följande exempel visar hur hello toouse Microsoft Azure Lagringsutforskaren toogenerate en SAS.

1. Om du inte redan har gjort [installera hello Microsoft Azure Lagringsutforskaren](http://storageexplorer.com)
2. Ansluta tooyour prenumeration.
3. Klicka på ditt lagringskonto och klicka på fliken hello ”åtgärd” längst ned hello kvar. Klicka på ”Hämta signatur för delad åtkomst” toogenerate ”anslutningssträngen” för din SAS.
4. Här är ett exempel på en SAS-anslutningssträng att ger Läs- och skrivbehörighet på hello service, behållare och objektnivå för hello blob-tjänsten för hello Storage-konto.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Som du kan se när du använder en SAS, är det inte att exponera din kontonyckel i ditt program. Du kan lära dig mer om SAS och bästa praxis för att använda SAS genom att checka ut [signaturer för delad åtkomst: Förstå SAS-modellen för hello](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

