


## <a name="using-vm-extensions"></a>Med hjälp av VM-tillägg
Azure VM-tillägg implementerar beteenden eller funktioner som antingen hjälper andra program som fungerar på virtuella Azure-datorer (till exempel hello **WebDeployForVSDevTest** tillägget kan Visual Studio tooWeb distribuera lösningar på Azure VM) eller ange Hej möjligheten för toointeract med hello VM toosupport vissa andra beteende (t.ex, du kan använda hello VM tillägg från PowerShell, hello Azure CLI och RESTEN klienter tooreset eller ändra värden för fjärråtkomst på Azure VM).

> [!IMPORTANT]
> En fullständig lista över tillägg av hello funktioner de stöder finns i [Azure VM-tillägg och funktioner](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Eftersom varje VM-tillägget har stöd för en specifik funktion, beroende exakt vad du kan och inte kan göra med ett tillägg hello-tillägget. Kontrollera därför att du har läst hello dokumentationen för hello VM-tillägget som du vill toouse innan du ändrar den virtuella datorn. Det går inte att ta bort vissa VM-tillägg. andra har egenskaper som kan anges som ändrar VM beteende radikalt.
> 
> 

hello vanligaste uppgifterna är:

1. Söka efter tillgängliga tillägg
2. Uppdatering av inlästa tillägg
3. Lägga till tillägg
4. Ta bort tillägg

## <a name="find-available-extensions"></a>Hitta tillgängliga tillägg
Du kan hitta tillägget och utökad information med hjälp av:

* PowerShell
* Kommandoraden för Azure plattformsoberoende gränssnitt (Azure CLI)
* REST API för Service Management

### <a name="azure-powershell"></a>Azure PowerShell
Vissa tillägg har PowerShell-cmdlets som är specifika toothem som kan underlätta konfigurationen från PowerShell; men hello följande cmdlets fungerar för alla VM-tillägg.

Du kan använda följande cmdlet: ar tooobtain information om tillgängliga tillägg hello:

* För instanser av webbroller eller arbetsroller kan du använda hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.
* För instanser av virtuella datorer, kan du använda hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.
  
   Till exempel hello följande exempel visas hur toolist information för hello **IaaSDiagnostics** tillägget med hjälp av PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Azure Command Line Interface (Azure CLI)
Vissa tillägg har Azure CLI-kommandon som är specifika toothem (hello Docker VM-tillägget är ett exempel), som kan underlätta konfigurationen; men hello följande kommandon för alla VM-tillägg.

Du kan använda hello **azure vm tilläggslistan** kommandot tooobtain information om tillgängliga tillägg och använda hello **–-json** alternativet toodisplay all tillgänglig information om en eller flera filnamnstillägg. Om du inte använder en Tilläggsnamn returnerar hello kommando en JSON-beskrivning av alla tillgängliga tillägg.

Till exempel hello följande kodexempel visar hur toolist hello information för hello **IaaSDiagnostics** tillägget med hjälp av hello Azure CLI **azure vm tilläggslistan** kommandot och använder hello **–-json** alternativet tooreturn fullständig information.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>REST API:er för tjänsthantering
Du kan använda följande REST API: er tooobtain information om tillgängliga tillägg hello:

* För instanser av webbroller eller arbetsroller kan du använda hello [lista över tillgängliga tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen. toolist hello versioner av tillgängliga tillägg kan du använda [lista tillägget versioner](https://msdn.microsoft.com/library/dn495437.aspx).
* För instanser av virtuella datorer, kan du använda hello [lista Resurstillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen. toolist hello versioner av tillgängliga tillägg kan du använda [lista resurs tillägget versioner](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Lägga till, uppdatera, eller inaktivera tillägg
Tillägg kan läggas till när en instans skapas eller de kan läggas till tooa kör-instansen. Tillägg kan uppdateras, inaktiveras eller tas bort. Du kan utföra dessa åtgärder med hjälp av Azure PowerShell-cmdlets eller genom att använda hello Service Management REST API-åtgärder. Parametrar krävs tooinstall och konfigurera vissa tillägg. Offentliga och privata parametrar stöds för tillägg.

### <a name="azure-powershell"></a>Azure PowerShell
Använda Azure PowerShell-cmdlets är hello enklaste sättet tooadd och uppdatera tillägg. När du använder hello tillägget cmdlets görs största delen av hello konfiguration hello-tillägget för dig. Ibland kan du behöva tooprogrammatically lägga till ett tillägg. När du behöver toodo detta, måste du ange hello konfigurationen av hello-tillägget.

Du kan använda följande cmdlets tooknow om filnamnstillägget kräver en konfiguration av offentliga och privata parametrar hello:

* För instanser av webbroller eller arbetsroller kan du använda hello **Get-AzureServiceAvailableExtension** cmdlet.
* För instanser av virtuella datorer, kan du använda hello **Get-AzureVMAvailableExtension** cmdlet.

### <a name="service-management-rest-apis"></a>REST API:er för tjänsthantering
När du hämtar en lista över tillgängliga tillägg genom att använda hello REST API: er, får du information om hur hello tillägget är toobe konfigurerats. hello information som returneras kan visa parameterinformation som representeras av ett schema med offentliga och privata schemat. Gemensamma parametervärden returneras i frågor om hello instanser. Privata parametervärden returneras inte.

Du kan använda hello följande REST API: er tooknow om filnamnstillägget kräver en konfiguration av offentliga och privata parametrar:

* För instanser av webbroller eller arbetsroller, hello **PublicConfigurationSchema** och **PrivateConfigurationSchema** element innehåller hello information i hello-svar från hello [lista Tillgängliga tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen.
* För instanser av virtuella datorer, hello **PublicConfigurationSchema** och **PrivateConfigurationSchema** element innehåller hello information i hello-svar från hello [lista Resurstillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen.

> [!NOTE]
> Tillägg kan också använda konfigurationer som har definierats med JSON. När dessa typer av tillägg används endast hello **SampleConfig** elementet används.
> 
> 

