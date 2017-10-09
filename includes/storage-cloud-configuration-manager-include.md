Hej [Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) erbjuder en klass för parsning av en anslutningssträng från en konfigurationsfil. Hej [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) klassen Parsar konfigurationsinställningar oavsett om klientprogrammet för hello körs på hello skrivbordet, på en mobil enhet i en virtuell Azure-dator eller i en Azure-molntjänst.

tooreference Hej CloudConfigurationManager-paketet, Lägg till följande hello `using` direktiv:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Här är ett exempel som visar hur tooretrieve en anslutningssträngen från en konfigurationsfil:

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Använda hello Azure Configuration Manager är valfritt. Du kan också använda en API som hello .NET Framework [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) klass.

