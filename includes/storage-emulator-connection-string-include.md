hello storage-emulatorn stöder ett fast konto och en välkänd autentiseringsnyckel för autentisering med delad nyckel. Det här kontot och nyckeln är hello endast delad nyckel autentiseringsuppgifter som tillåts för användning med hello storage-emulatorn. De är:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> hello autentiseringsnyckel som stöds av hello storage-emulatorn är endast avsett för testning hello-funktionerna i din klientkod för autentisering. Den har inte något syfte säkerhet. Du kan inte använda ditt lagringskonto för produktion och en nyckel med hello storage-emulatorn. Du bör inte använda hello development konto med produktionsdata.
> 
> hello storage-emulatorn stöder endast anslutningar via HTTP. HTTPS är dock hello rekommenderas protokoll för att komma åt resurser i en Azure storage-konto.
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a>Ansluta toohello emulatorn konto med hjälp av en genväg
hello enklaste sättet tooconnect toohello storage-emulatorn från ditt program är tooconfigure en anslutningssträng i programmets konfigurationsfil som refererar till hello genvägen `UseDevelopmentStorage=true`. Här är ett exempel på en anslutning sträng toohello storage-emulatorn i en *app.config* fil: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a>Ansluta toohello emulatorn konto med hjälp av hello välkända kontonamnet och nyckeln
toocreate en anslutningssträng att referenser hello emulatorn kontonamnet och nyckeln, måste du ange hello slutpunkter för var och en av hello tjänster du vill toouse från hello emulatorn i hello anslutningssträngen. Detta är nödvändigt så att hello anslutningssträngen refererar hello emulatorn slutpunkter som är annorlunda än för ett lagringskonto för produktion. Hello-värdet för anslutningssträngen ska se ut så här:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Det här värdet är identiska toohello genväg som visas ovan, `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Ange en HTTP-proxy
Du kan även ange en HTTP-proxy toouse när du testar din tjänst mot hello storage-emulatorn. Detta kan vara användbart för sett HTTP-begäranden och -svar när du felsöker åtgärder mot hello lagringstjänster. toospecify en proxy, lägga till hello `DevelopmentStorageProxyUri` alternativet toohello anslutningssträngen och ange dess värde toohello proxy-URI. Här är till exempel en anslutningssträng som pekar toohello storage-emulatorn och konfigurerar en HTTP-proxy:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

