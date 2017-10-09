## <a name="create-a-ruby-application"></a><span data-ttu-id="57fbc-101">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="57fbc-101">Create a Ruby application</span></span>
<span data-ttu-id="57fbc-102">Instruktioner finns i [skapa ett Ruby-program på Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="57fbc-102">For instructions, see [Create a Ruby Application on Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="57fbc-103">Konfigurera ditt program tooUse Service Bus</span><span class="sxs-lookup"><span data-stu-id="57fbc-103">Configure Your application tooUse Service Bus</span></span>
<span data-ttu-id="57fbc-104">toouse Service Bus, hämtar och använder hello Azure Ruby paket som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="57fbc-104">toouse Service Bus, download and use hello Azure Ruby package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="57fbc-105">Använd RubyGems tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="57fbc-105">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="57fbc-106">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="57fbc-106">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="57fbc-107">Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="57fbc-107">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="57fbc-108">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="57fbc-108">Import hello package</span></span>
<span data-ttu-id="57fbc-109">Med hjälp av valfri textredigerare, Lägg till följande toohello överkant hello Ruby hello filen som du avser att toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="57fbc-109">Using your favorite text editor, add hello following toohello top of hello Ruby file in which you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="57fbc-110">Konfigurera en Service Bus-anslutning</span><span class="sxs-lookup"><span data-stu-id="57fbc-110">Set up a Service Bus connection</span></span>
<span data-ttu-id="57fbc-111">Använd hello följande kod tooset hello värden för namnområde, namnet på hello nyckel, nyckel, Signerare och värden:</span><span class="sxs-lookup"><span data-stu-id="57fbc-111">Use hello following code tooset hello values of namespace, name of hello key, key, signer and host:</span></span>

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

<span data-ttu-id="57fbc-112">Ange hello namnområde värde toohello värdet du skapade i stället för hela hello-URL.</span><span class="sxs-lookup"><span data-stu-id="57fbc-112">Set hello namespace value toohello value you created rather than hello entire URL.</span></span> <span data-ttu-id="57fbc-113">Till exempel använda **”yourexamplenamespace”**, inte ”yourexamplenamespace.servicebus.windows.net”.</span><span class="sxs-lookup"><span data-stu-id="57fbc-113">For example, use **"yourexamplenamespace"**, not "yourexamplenamespace.servicebus.windows.net".</span></span>
