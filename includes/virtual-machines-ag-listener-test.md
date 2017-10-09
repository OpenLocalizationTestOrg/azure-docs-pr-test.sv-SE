I det här steget kan du testa hello tillgänglighetsgruppens lyssnare med hjälp av ett klientprogram som körs på hello samma nätverk.

Klientanslutning har hello följande krav:

* Klienten anslutningar toohello lyssnare måste komma från datorer som finns i en annan molntjänst än hello något att värdar hello alltid på tillgänglighetsrepliker.
* Om hello alltid på repliker är i olika undernät, måste klienter ange *MultisubnetFailover = True* i hello anslutningssträngen. Det här villkoret resulterar i parallella anslutning försöker tooreplicas i hello olika undernät. Det här scenariot omfattar en mellan region alltid på tillgänglighet distribution.

Ett exempel är tooconnect toohello lyssnare från en hello virtuella datorer i hello samma virtuella Azure-nätverk (men inte som värd för en replik). Ett enkelt sätt toocomplete det här testet är tootry tooconnect SQL Server Management Studio toohello tillgänglighetsgruppens lyssnare. En annan enkel metod är toorun [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx), enligt följande:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Om hello EndpointPort värde är *1433*, du är inte obligatoriska toospecify i hello-anrop. hello tidigare anrop förutsätts även att hello klientdatorn är domänansluten toohello samma domän och att hello anroparen har beviljats behörigheter för hello-databasen med hjälp av Windows-autentisering.
> 
> 

När du testar hello lyssnare vara att toofail via hello tillgänglighet grupp toomake till att klienter kan ansluta toohello lyssnare för växling vid fel.

