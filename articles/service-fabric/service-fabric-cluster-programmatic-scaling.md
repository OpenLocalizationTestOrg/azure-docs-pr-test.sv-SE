---
title: "aaaAzure Service Fabric programmässiga skalning | Microsoft Docs"
description: "Skala ett Azure Service Fabric-kluster in eller ut programmässigt, enligt toocustom utlösare"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: mikerou
ms.openlocfilehash: a0c6499b1a143a173006248cf8a15380632637e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Skala Service Fabric-klustret via programmering 

Grunderna i skala ett Service Fabric-kluster i Azure beskrivs i dokumentationen på [skalning av klustret](./service-fabric-cluster-scale-up-down.md). Artikeln beskriver hur Service Fabric-kluster är byggda på virtuella datorer och kan skalas manuellt eller med Autoskala regler. Det här dokumentet tittar på programmering koordinerande Azure skalning åtgärder för mer avancerade scenarier. 

## <a name="reasons-for-programmatic-scaling"></a>Orsaker till programmässiga skalning
I många fall är är skalning manuellt eller via Autoskala regler bra lösningar. I andra scenarier, men får de inte vara hello rätt plats. Potentiella nackdelar toothese metoder är:

- Skalning manuellt måste du toolog i och uttryckligen begära skalning åtgärder. Om skalning åtgärder krävs ofta eller vid oväntade tidpunkter, kanske inte en bra lösning i den här metoden.
- När Autoskala regler för att ta bort en instans från en skaluppsättning för virtuell dator, de inte bort automatiskt kunskap om noden från hello associerade Service Fabric-klustret såvida inte hello nodtyp har en hållbarhet Silver eller guld. Eftersom Autoskala regler fungerar i hello skala ange nivån (i stället för vid hello Service Fabric-nivå), Autoskala regler kan ta bort Service Fabric-noder utan att dem avslutas. Borttagningen oartigt nod lämnar 'ghost-tillstånd för Service Fabric-noden efter efter skala i operations. En person (eller en tjänst) måste tooperiodically Rensa borttagna noden tillstånd i hello Service Fabric-klustret.
  - Observera att en nodtyp med hållbarhet guld eller Silver automatiskt rensar bort noder.  
- Även om det finns [många mått](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) stöds av Autoskala regler, är det fortfarande en begränsad uppsättning. Om ditt scenario anrop för att skala baserat på vissa mått som inte omfattas i uppsättningen kan sedan kanske Autoskala regler inte ett bra alternativ.

Baserat på dessa begränsningar du tooimplement mer anpassade automatisk skalning modeller. 

## <a name="scaling-apis"></a>Skalning API: er
Azure API: er finns som tillåter program tooprogrammatically fungerar med skalningsuppsättningar i virtuella datorer och Service Fabric-kluster. Om den befintliga Autoskala alternativ inte fungerar för ditt scenario, dessa API: er som gör att den möjliga tooimplement anpassade skalning logik. 

En metod som tooimplementing detta ”home-tillverkade” automatisk skalning funktioner är tooadd en ny tillståndslösa tjänsten toohello Service Fabric application toomanage skalning åtgärder. I hello tjänsten `RunAsync` metod, en uppsättning utlösare kan avgöra om skalning krävs (inklusive kontrollerar parametrar, till exempel klustrets maximala storlek och skala cooldowns).   

hello API som används för virtual machine scale set interaktioner (både toocheck hello aktuellt antal instanser för virtuella datorer och toomodify den) är hello [flytande Azure Management Compute biblioteket](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). hello flytande beräkning biblioteket tillhandahåller ett enkelt att använda API för att interagera med virtuella datorer.

toointeract med hello Service Fabric-kluster, använda [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Naturligtvis hello skalning kod behöver inte toorun som en tjänst i hello klustret toobe skalas. Båda `IAzure` och `FabricClient` kan fjärransluta tootheir associerade Azure-resurser, så hello skalning service kan enkelt ett konsolprogram eller Windows-tjänsten körs från utanför hello Service Fabric-program. 

## <a name="credential-management"></a>Hantering av autentiseringsuppgifter
En utmaning för att skriva en service toohandle skalning är att hello tjänsten måste vara kan tooaccess scale set datorresurser utan interaktiv inloggning. Åtkomst till hello Service Fabric klustret är enkelt om hello skalning tjänsten ändrar sin egen Service Fabric-program, men autentiseringsuppgifterna är nödvändiga tooaccess hello skaluppsättning. toolog i som du kan använda en [tjänstens huvudnamn](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) skapats med hello [Azure CLI 2.0](https://github.com/azure/azure-cli).

Du kan skapa ett huvudnamn för tjänsten med hello följande steg:

1. Logga in toohello Azure CLI (`az login`) som en användare med åtkomst toohello virtuella skala anger
2. Skapa hello tjänstens huvudnamn med`az ad sp create-for-rbac`
    1. Anteckna hello appId (kallas klient-ID någon annanstans), namn, lösenord och klientorganisations för senare användning.
    2. Du måste också ditt prenumerations-ID som kan visas med`az account list`

hello flytande beräknings-biblioteket kan logga in med autentiseringsuppgifterna enligt följande (Observera att core flytande Azure typer som `IAzure` i hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) paketet):

```C#
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed toologin tooAzure");
}
```

När du loggade in scale set-instanser kan efterfrågas `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Skala ut
Med hjälp av flytande hello Azure compute SDK, instanser kan läggas till toohello virtuella skaluppsättningen med bara några få anrop-

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Du kan också kan virtuella skala storlek också hanteras med PowerShell-cmdlets. [`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)Hämta hello virtuella Skala uppsättningsobjekt. hello aktuella kapaciteten kommer att lagras i hello `.sku.capacity` egenskapen. När du ändrar hello kapacitet toohello önskas värdet hello virtuella skaluppsättningen i Azure kan uppdateras med hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) kommando.

Som när du lägger till en nod manuellt lägga till en skaluppsättning för instansen bör vara alla som behövs toostart som innehåller en ny Service Fabric-nod eftersom hello skaluppsättning mall Anslut tillägg tooautomatically nya instanser toohello Service Fabric-klustret. 

## <a name="scaling-in"></a>Skalning i

Skalning i är liknande tooscaling ut. hello den faktiska virtuella datorns skaluppsättning ändringar är praktiskt taget hello samma. Men som har diskuterats tidigare, Service Fabric endast rensas automatiskt bort noder med en hållbarhet guld eller Silver. Så hello Brons hållbarhet skala i om det är nödvändigt toointeract med hello Service Fabric-kluster tooshut ned hello nod toobe tas bort och tooremove dess tillstånd.

Förbereda hello nod för avstängning gäller att hitta hello nod toobe bort (hello nyligen tillagda noden) och inaktivera den. För icke-seed noder nyare noder kan hittas genom att jämföra `NodeInstanceId`. 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Tänk som *seed* noder inte verkar vara tooalways följer hello konventionen större instans-ID: N tas bort först.

När hello nod toobe bort hittas kan inaktiveras och tas bort med hjälp av hello samma `FabricClient` instansen och hello `IAzure` instansen från tidigare.

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove hello node from hello Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up tooa timeout) for hello node toogracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Som med skala ut PowerShell-cmdlets för att ändra virtuella datorn kan set kapacitet även användas här om en scripting metod är att föredra. När hello virtuella instansen har tagits bort, kan tas bort Service Fabric nodens tillstånd.

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Nackdelarna

Som visas i föregående kodfragment hello ger skapa egna skalning tjänst hello största möjliga kontroll och anpassningsbarheten över ditt program skalning beteende. Detta kan vara användbart för scenarier som kräver exakt kontroll över när eller hur ett program skalas in eller ut. Den här kontrollen har dock en kompromiss förenklar koden. Med den här metoden innebär att du måste tooown skalning kod, som är icke-trivial.

Hur ska du bör Service Fabric skalning beror på ditt scenario. Om det är ovanligt att skalning, hello möjlighet tooadd eller ta bort noder manuellt räcker förmodligen. För mer komplicerade scenarier erbjuder Autoskala regler och SDK exponera hello möjlighet tooscale programmässigt kraftfulla alternativ.

## <a name="next-steps"></a>Nästa steg

tooget igång implementera egna automatisk skalning logik bekanta dig med hello följande begrepp och användbara API: er:

- [Skalning manuellt eller med Autoskala regler](./service-fabric-cluster-scale-up-down.md)
- [Flytande Azure-bibliotek för .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (användbart för att interagera med Service Fabric-klustret underliggande skalningsuppsättningar i virtuella datorer)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (användbart för att interagera med ett Service Fabric-kluster och noder)
