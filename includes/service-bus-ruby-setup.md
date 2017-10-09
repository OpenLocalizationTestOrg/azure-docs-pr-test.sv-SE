## <a name="create-a-ruby-application"></a>Skapa ett Ruby-program
Instruktioner finns i [skapa ett Ruby-program på Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program tooUse Service Bus
toouse Service Bus, hämtar och använder hello Azure Ruby paket som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-rubygems-tooobtain-hello-package"></a>Använd RubyGems tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).
2. Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.

### <a name="import-hello-package"></a>Importera hello-paket
Med hjälp av valfri textredigerare, Lägg till följande toohello överkant hello Ruby hello filen som du avser att toouse lagring:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Konfigurera en Service Bus-anslutning
Använd hello följande kod tooset hello värden för namnområde, namnet på hello nyckel, nyckel, Signerare och värden:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ange hello namnområde värde toohello värdet du skapade i stället för hela hello-URL. Till exempel använda **”yourexamplenamespace”**, inte ”yourexamplenamespace.servicebus.windows.net”.
