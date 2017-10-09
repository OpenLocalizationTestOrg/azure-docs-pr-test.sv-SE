## <a name="set-up-your-development-environment"></a>Ställ in din utvecklingsmiljö
Härnäst ska du ställa in din utvecklingsmiljö i Visual Studio så att du är klar tootry hello kodexemplen i den här guiden.

### <a name="create-a-windows-console-application-project"></a>Skapa ett Windows-konsolprogramprojekt
Skapa ett nytt Windows-konsolprogram i Visual Studio. hello följande steg visar hur toocreate ett konsolprogram i Visual Studio 2017 dock hello stegen är ungefär i andra versioner av Visual Studio.

1. Välj **Arkiv** > **Nytt** > **Projekt**
2. Välj **Installerat** > **Mallar** > **Visual C#** > **Windows Classic Desktop**
3. Välj **Konsolprogram (.NET Framework)**
4. Ange ett namn för ditt program i hello **Name:** fält
5. Välj **OK**

![Dialogruta för att skapa projekt i Visual Studio](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Alla kodexempel i den här kursen kan läggas till toohello `Main()` -metoden i konsolen programmets `Program.cs` fil.

Du kan använda hello Azure Storage-klientbibliotek för någon typ av .NET-program, inklusive en Azure cloud service eller web app, och stationära och mobila program. I den här guiden använder vi oss av en konsolapp för enkelhetens skull.

### <a name="use-nuget-tooinstall-hello-required-packages"></a>Använd NuGet tooinstall krävs hello-paket
Det finns två paket som du behöver tooreference i ditt projekt toocomplete för den här kursen:

* [Microsoft Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): det här paketet ger programmatisk åtkomst toodata resurser i ditt lagringskonto.
* [Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): det här paketet tillhandahåller en klass för parsning av en anslutningssträng i en konfigurationsfil, oavsett var ditt program körs.

Du kan använda NuGet tooobtain båda paketen. Följ de här stegen:

1. Högerklicka på ditt projekt i **Solution Explorer** och välj **Hantera NuGet-paket**.
2. Sök online efter ”WindowsAzure.Storage” och klicka på **installera** tooinstall hello Storage-klientbiblioteket och dess beroenden.
3. Sök online efter ”WindowsAzure.ConfigurationManager” och klicka på **installera** tooinstall hello Azure Configuration Manager.

> [!NOTE]
> hello Storage-klientbibliotek paketet ingår också i hello [Azure SDK för .NET](https://azure.microsoft.com/downloads/). Vi rekommenderar dock att du även installerar hello Storage-klientbiblioteket från NuGet tooensure att du alltid har hello senaste versionen av klientbiblioteket hello.
> 
> Hej ODataLib-beroenden i hello Storage-klientbibliotek för .NET matchas genom hello ODataLib-paket som är tillgängliga på NuGet inte från WCF Data Services. Hej ODataLib-biblioteken kan hämtas direkt eller refereras till i ditt Kodprojekt via NuGet. hello specifika ODataLib-paket som används av hello Storage-klientbiblioteket är [OData](http://nuget.org/packages/Microsoft.Data.OData/), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/), och [Spatial](http://nuget.org/packages/System.Spatial/). De här biblioteken används av hello Azure Table storage-klasser är de nödvändiga beroenden för programmering med hello Storage-klientbibliotek.
> 
> 

### <a name="determine-your-target-environment"></a>Fastställ målmiljön
Du har två miljöalternativ för att köra hello exemplen i den här guiden:

* Du kan köra din kod mot ett Azure Storage-konto i hello molnet. 
* Du kan köra din kod mot hello Azure storage-emulatorn. hello storage-emulatorn är en lokal miljö som emulerar ett Azure Storage-konto i hello molnet. hello-emulatorn är ett kostnadsfritt alternativ för att testa och felsöka din kod medan ditt program är under utveckling. hello-emulatorn använder ett välkänt konto och nyckel. Mer information finns i [Använd hello Azure Storage-emulatorn för utveckling och testning](../articles/storage/common/storage-use-emulator.md)

Om du utvecklar ett lagringskonto i molnet hello kopiera hello primärnyckeln för ditt lagringskonto från hello Azure-portalen. Mer information finns i [Visa och kopiera åtkomstnycklar för lagring](../articles/storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [!NOTE]
> Du kan rikta hello storage-emulatorn tooavoid kostnader associerade med Azure Storage. Om du väljer tootarget ett Azure storage-konto i hello molnet, kommer kostnader för att utföra den här självstudiekursen dock försumbar.
> 
> 

### <a name="configure-your-storage-connection-string"></a>Konfigurera anslutningssträngen för lagring
hello Azure Storage-klientbibliotek för .NET stöder användning av en lagring sträng tooconfigure slutpunkter och autentiseringsuppgifter för åtkomst till lagringstjänster. hello bästa sätt toomaintain anslutningssträngen för lagring är i en konfigurationsfil. 

Mer information om anslutningssträngar finns [konfigurera en anslutningssträng tooAzure lagring](../articles/storage/common/storage-configure-connection-string.md).

> [!NOTE]
> Din lagringskontonyckel är liknande toohello rotlösenordet för lagringskontot. Alltid vara försiktig tooprotect din lagringskontonyckel. Undvik att distribuera den tooother användare, hårdkoda den eller spara den i en oformaterad textfil som är tillgänglig tooothers. Återskapa din nyckel med hjälp av hello Azure-portalen om du tror att den komprometterats.
> 
> 

tooconfigure ditt anslutningssträngen, öppna hello `app.config` filen i Solutions Explorer i Visual Studio. Lägg till hello innehållet i hello `<appSettings>` elementet enligt nedan. Ersätt `account-name` med hello namnet på ditt lagringskonto och `account-key` med din åtkomstnyckel:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Din konfigurationsinställning kan till exempel se ut så här:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

Du kan använda ett kortkommando som mappar toohello välkända kontonamnet och nyckeln tootarget hello storage-emulatorn. I så fall är din inställning för anslutningssträngen:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

