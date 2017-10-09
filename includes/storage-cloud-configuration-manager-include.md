<span data-ttu-id="4731e-101">Hej [Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) erbjuder en klass för parsning av en anslutningssträng från en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="4731e-101">hello [Microsoft Azure Configuration Manager Library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) provides a class for parsing a connection string from a configuration file.</span></span> <span data-ttu-id="4731e-102">Hej [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) klassen Parsar konfigurationsinställningar oavsett om klientprogrammet för hello körs på hello skrivbordet, på en mobil enhet i en virtuell Azure-dator eller i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="4731e-102">hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) class parses configuration settings regardless of whether hello client application is running on hello desktop, on a mobile device, in an Azure virtual machine, or in an Azure cloud service.</span></span>

<span data-ttu-id="4731e-103">tooreference Hej CloudConfigurationManager-paketet, Lägg till följande hello `using` direktiv:</span><span class="sxs-lookup"><span data-stu-id="4731e-103">tooreference hello CloudConfigurationManager package, add hello following `using` directive:</span></span>

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

<span data-ttu-id="4731e-104">Här är ett exempel som visar hur tooretrieve en anslutningssträngen från en konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="4731e-104">Here's an example that shows how tooretrieve a connection string from a configuration file:</span></span>

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

<span data-ttu-id="4731e-105">Använda hello Azure Configuration Manager är valfritt.</span><span class="sxs-lookup"><span data-stu-id="4731e-105">Using hello Azure Configuration Manager is optional.</span></span> <span data-ttu-id="4731e-106">Du kan också använda en API som hello .NET Framework [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="4731e-106">You can also use an API like hello .NET Framework's [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) class.</span></span>

