---
title: aaaReliable samling objekt serialisering i Azure Service Fabric | Microsoft Docs
description: "Azure Service Fabric tillförlitliga samlingar objektet serialisering"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 248defbe0ae6f65b4ac5dc7c74e80d8f1152ce94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Tillförlitliga samling objekt serialisering i Azure Service Fabric
Tillförlitliga samlingar replikera och deras objekt toomake är beständig mellan datorn fel och strömavbrott är kvar.
Både tooreplicate och toopersist objekt, tillförlitlig samlingar måste tooserialize dem.

Tillförlitliga samlingar hämta hello lämplig serialisering för en viss typ från tillförlitliga Tillståndshanterare.
Tillåter anpassade serializers toobe registrerats för en viss typ tillförlitlig Tillståndshanterare eftersom det innehåller inbyggda serializers.

## <a name="built-in-serializers"></a>Inbyggda Serializers

Tillstånd för tillförlitlig Manager innehåller inbyggda serialiseraren för vissa typer av vanliga så att de kan serialiseras effektivt som standard. För andra typer tillförlitliga Tillståndshanterare faller tillbaka toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Inbyggda serializers är effektivare eftersom de redan känner till deras typer kan inte ändra och de behöver inte tooinclude information om hello typ som dess namn.

Tillförlitlig Tillståndshanterare har inbyggd serialisering för följande typer: 
- GUID
- bool
- Mottagna byte
- sbyte
- byte]
- Char
- Sträng
- Decimal
- dubbla
- flyttal
- int
- uint
- lång
- ulong
- kort
- ushort

## <a name="custom-serialization"></a>Anpassad serialisering

Anpassade serializers är vanliga tooincrease prestanda eller tooencrypt hello data över hello överföring och på disken. Anpassade serializers är ofta effektivare än allmänna serialiseraren anledningarna eftersom de inte behöver tooserialize information om hello typen. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) är används tooregister en anpassad serialisering för hello typ T. Denna registrering ska hända i hello konstruktion hello StatefulServiceBase tooensure innan återställningen startas, har alla tillförlitliga samlingar kommer åt toohello relevanta serialiseraren tooread sina beständiga data.

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed tooset OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Anpassade serializers ges företräde över inbyggda serializers. Till exempel när en anpassad serialisering för int registreras, är det används tooserialize heltal i stället för hello inbyggd serialisering för int.

### <a name="how-tooimplement-a-custom-serializer"></a>Hur tooimplement en anpassad serialisering

En anpassad serialiserare måste tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) gränssnitt.

> [!NOTE]
> IStateSerializer<T> innehåller en överlagring för skrivning och Läs som tar en ytterligare ton kallas Basvärde. Detta API är för differentiell serialisering. För närvarande visas differentiell serialisering funktionen inte. Dessa två överlagringar kallas därför inte förrän differentiell serialisering exponeras och aktiverat.

Följande är ett exempel anpassad typ som kallas OrderKey som innehåller fyra egenskaper

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Följande är exempel på implementering av IStateSerializer<OrderKey>.
Observera att läsa och skriva överlagringar som i baseValue, anropa sina respektive överlagring för vidarebefordran kompatibilitet.

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Möjligheterna
I en [löpande uppgradering av programmet](service-fabric-application-upgrade.md), hello uppgraderingen är tillämpade tooa del noder, en domän i taget. Under den här processen kommer att vissa uppgraderingsdomäner hello nyare version av programmet och vissa uppgraderingsdomäner blir hello äldre versioner av programmet. Under den här fasen hello hello ny version av programmet måste vara kan tooread hello gammal version av dina data och hello gammal version av programmet måste vara kan tooread hello ny version av dina data. Om hello dataformat inte framåt och bakåt kompatibla, hello-uppgraderingen kan misslyckas eller värre, data går förlorat eller skadat.

Om du använder inbyggd funktion för serialisering har inte tooworry om kompatibilitet.
Men om du använder en anpassad serialisering eller hello DataContractSerializer hello data har toobe oändligt bakåtkompatibilitet och vidarebefordrar kompatibel.
Med andra ord varje version av serialiseraren måste toobe kan tooserialize och deserialisera någon version av hello-typen.

Data kontrakt-användare bör följa hello väldefinierade versionshantering regler för att lägga till, ta bort och ändra fält. Datakontrakt har också stöd för behandlar okänt fält, koppla upp i hello serialisering och deserialisering processen och behandlar klassarv. Mer information finns i [med datakontrakt](https://msdn.microsoft.com/library/ms733127.aspx).

Anpassad serialisering användare bör följa toohello riktlinjer hello serialiserare som de använder toomake är bakåtkompatibilitet och vidarebefordrar kompatibel.
Vanligt sätt stöder alla versioner är att lägga till Storleksinformation på hello början och bara att lägga till valfria egenskaper.
Det här sättet varje version kan läsa så mycket kan och hoppa över hello resten av hello dataströmmen.

## <a name="next-steps"></a>Nästa steg
  * [Serialisering och uppgradering](service-fabric-application-upgrade-data-serialization.md)
  * [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.
  * [Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.
  * Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).
  * Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).
  * Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).
