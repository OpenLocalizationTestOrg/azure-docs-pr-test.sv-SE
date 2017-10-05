---
title: "Tillförlitliga samling objekt serialisering i Azure Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: c14794b71ce7340d9e90a56d781c712e247ded06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a><span data-ttu-id="1e519-103">Tillförlitliga samling objekt serialisering i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e519-103">Reliable Collection object serialization in Azure Service Fabric</span></span>
<span data-ttu-id="1e519-104">Tillförlitliga samlingar replikera och deras objekt att kontrollera att de är beständig mellan datorn fel och strömavbrott är kvar.</span><span class="sxs-lookup"><span data-stu-id="1e519-104">Reliable Collections' replicate and persist their items to make sure they are durable across machine failures and power outages.</span></span>
<span data-ttu-id="1e519-105">Både replikeras och för att spara objekt, måste tillförlitlig samlingar serialisera dem.</span><span class="sxs-lookup"><span data-stu-id="1e519-105">Both to replicate and to persist items, Reliable Collections' need to serialize them.</span></span>

<span data-ttu-id="1e519-106">Tillförlitliga samlingar hämta lämplig serialisering för en viss typ från tillförlitliga Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="1e519-106">Reliable Collections' get the appropriate serializer for a given type from Reliable State Manager.</span></span>
<span data-ttu-id="1e519-107">Tillåter anpassade serializers som ska registreras för en viss typ tillförlitlig Tillståndshanterare eftersom det innehåller inbyggda serializers.</span><span class="sxs-lookup"><span data-stu-id="1e519-107">Reliable State Manager contains built-in serializers and allows custom serializers to be registered for a given type.</span></span>

## <a name="built-in-serializers"></a><span data-ttu-id="1e519-108">Inbyggda Serializers</span><span class="sxs-lookup"><span data-stu-id="1e519-108">Built-in Serializers</span></span>

<span data-ttu-id="1e519-109">Tillstånd för tillförlitlig Manager innehåller inbyggda serialiseraren för vissa typer av vanliga så att de kan serialiseras effektivt som standard.</span><span class="sxs-lookup"><span data-stu-id="1e519-109">Reliable State Manager includes built-in serializer for some common types, so that they can be serialized efficiently by default.</span></span> <span data-ttu-id="1e519-110">För andra typer tillförlitliga Tillståndshanterare faller tillbaka om du vill använda den [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="1e519-110">For other types, Reliable State Manager falls back to use the [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span></span>
<span data-ttu-id="1e519-111">Inbyggda serializers är effektivare eftersom de redan känner till deras typer kan inte ändra och de behöver inte inkludera information om vilken typ som dess namn.</span><span class="sxs-lookup"><span data-stu-id="1e519-111">Built-in serializers are more efficient since they know their types cannot change and they do not need to include information about the type like its type name.</span></span>

<span data-ttu-id="1e519-112">Tillförlitlig Tillståndshanterare har inbyggd serialisering för följande typer:</span><span class="sxs-lookup"><span data-stu-id="1e519-112">Reliable State Manager has built-in serializer for following types:</span></span> 
- <span data-ttu-id="1e519-113">GUID</span><span class="sxs-lookup"><span data-stu-id="1e519-113">Guid</span></span>
- <span data-ttu-id="1e519-114">bool</span><span class="sxs-lookup"><span data-stu-id="1e519-114">bool</span></span>
- <span data-ttu-id="1e519-115">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="1e519-115">byte</span></span>
- <span data-ttu-id="1e519-116">sbyte</span><span class="sxs-lookup"><span data-stu-id="1e519-116">sbyte</span></span>
- <span data-ttu-id="1e519-117">byte]</span><span class="sxs-lookup"><span data-stu-id="1e519-117">byte[]</span></span>
- <span data-ttu-id="1e519-118">Char</span><span class="sxs-lookup"><span data-stu-id="1e519-118">char</span></span>
- <span data-ttu-id="1e519-119">Sträng</span><span class="sxs-lookup"><span data-stu-id="1e519-119">string</span></span>
- <span data-ttu-id="1e519-120">Decimal</span><span class="sxs-lookup"><span data-stu-id="1e519-120">decimal</span></span>
- <span data-ttu-id="1e519-121">dubbla</span><span class="sxs-lookup"><span data-stu-id="1e519-121">double</span></span>
- <span data-ttu-id="1e519-122">flyttal</span><span class="sxs-lookup"><span data-stu-id="1e519-122">float</span></span>
- <span data-ttu-id="1e519-123">int</span><span class="sxs-lookup"><span data-stu-id="1e519-123">int</span></span>
- <span data-ttu-id="1e519-124">uint</span><span class="sxs-lookup"><span data-stu-id="1e519-124">uint</span></span>
- <span data-ttu-id="1e519-125">lång</span><span class="sxs-lookup"><span data-stu-id="1e519-125">long</span></span>
- <span data-ttu-id="1e519-126">ulong</span><span class="sxs-lookup"><span data-stu-id="1e519-126">ulong</span></span>
- <span data-ttu-id="1e519-127">kort</span><span class="sxs-lookup"><span data-stu-id="1e519-127">short</span></span>
- <span data-ttu-id="1e519-128">ushort</span><span class="sxs-lookup"><span data-stu-id="1e519-128">ushort</span></span>

## <a name="custom-serialization"></a><span data-ttu-id="1e519-129">Anpassad serialisering</span><span class="sxs-lookup"><span data-stu-id="1e519-129">Custom Serialization</span></span>

<span data-ttu-id="1e519-130">Anpassade serializers används ofta för att öka prestandan eller för att kryptera data över nätverket och på disken.</span><span class="sxs-lookup"><span data-stu-id="1e519-130">Custom serializers are commonly used to increase performance or to encrypt the data over the wire and on disk.</span></span> <span data-ttu-id="1e519-131">Anpassade serializers är ofta effektivare än allmänna serialiseraren anledningarna eftersom de inte behöver att serialisera information om typen.</span><span class="sxs-lookup"><span data-stu-id="1e519-131">Among other reasons, custom serializers are commonly more efficient than generic serializer since they don't need to serialize information about the type.</span></span> 

<span data-ttu-id="1e519-132">[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) används för att registrera en anpassad serialisering för den angivna typen T. Denna registrering ska inträffa i konstruktion StatefulServiceBase så att alla tillförlitliga samlingar innan återställningen startas har åtkomst till relevanta serialiseraren att läsa deras beständiga data.</span><span class="sxs-lookup"><span data-stu-id="1e519-132">[IReliableStateManager.TryAddStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) is used to register a custom serializer for the given type T. This registration should happen in the construction of the StatefulServiceBase to ensure that before recovery starts, all Reliable Collections have access to the relevant serializer to read their persisted data.</span></span>

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed to set OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> <span data-ttu-id="1e519-133">Anpassade serializers ges företräde över inbyggda serializers.</span><span class="sxs-lookup"><span data-stu-id="1e519-133">Custom serializers are given precedence over built-in serializers.</span></span> <span data-ttu-id="1e519-134">Till exempel när en anpassad serialisering för int registreras används den för att serialisera heltal i stället för inbyggd serialisering för int.</span><span class="sxs-lookup"><span data-stu-id="1e519-134">For example, when a custom serializer for int is registered, it is used to serialize integers instead of the built-in serializer for int.</span></span>

### <a name="how-to-implement-a-custom-serializer"></a><span data-ttu-id="1e519-135">Hur du implementerar en anpassad serialisering</span><span class="sxs-lookup"><span data-stu-id="1e519-135">How to implement a custom serializer</span></span>

<span data-ttu-id="1e519-136">En anpassad serialiserare måste implementera den [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1e519-136">A custom serializer needs to implement the [IStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.</span></span>

> [!NOTE]
> <span data-ttu-id="1e519-137">IStateSerializer<T> innehåller en överlagring för skrivning och Läs som tar en ytterligare ton kallas Basvärde.</span><span class="sxs-lookup"><span data-stu-id="1e519-137">IStateSerializer<T> includes an overload for Write and Read that takes in an additional T called base value.</span></span> <span data-ttu-id="1e519-138">Detta API är för differentiell serialisering.</span><span class="sxs-lookup"><span data-stu-id="1e519-138">This API is for differential serialization.</span></span> <span data-ttu-id="1e519-139">För närvarande visas differentiell serialisering funktionen inte.</span><span class="sxs-lookup"><span data-stu-id="1e519-139">Currently differential serialization feature is not exposed.</span></span> <span data-ttu-id="1e519-140">Dessa två överlagringar kallas därför inte förrän differentiell serialisering exponeras och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="1e519-140">Hence, these two overloads are not called until differential serialization is exposed and enabled.</span></span>

<span data-ttu-id="1e519-141">Följande är ett exempel anpassad typ som kallas OrderKey som innehåller fyra egenskaper</span><span class="sxs-lookup"><span data-stu-id="1e519-141">Following is an example custom type called OrderKey that contains four properties</span></span>

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

<span data-ttu-id="1e519-142">Följande är exempel på implementering av IStateSerializer<OrderKey>.</span><span class="sxs-lookup"><span data-stu-id="1e519-142">Following is an example implementation of IStateSerializer<OrderKey>.</span></span>
<span data-ttu-id="1e519-143">Observera att läsa och skriva överlagringar som i baseValue, anropa sina respektive överlagring för vidarebefordran kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="1e519-143">Note that Read and Write overloads that take in baseValue, call their respective overload for forwards compatibility.</span></span>

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

## <a name="upgradability"></a><span data-ttu-id="1e519-144">Möjligheterna</span><span class="sxs-lookup"><span data-stu-id="1e519-144">Upgradability</span></span>
<span data-ttu-id="1e519-145">I en [löpande uppgradering av programmet](service-fabric-application-upgrade.md), uppgraderingen tillämpas på en del noder, en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="1e519-145">In a [rolling application upgrade](service-fabric-application-upgrade.md), the upgrade is applied to a subset of nodes, one upgrade domain at a time.</span></span> <span data-ttu-id="1e519-146">Vissa uppgraderingsdomäner görs på den nya versionen av ditt program under den här processen och vissa uppgraderingsdomäner görs på den äldre versionen av programmet.</span><span class="sxs-lookup"><span data-stu-id="1e519-146">During this process, some upgrade domains will be on the newer version of your application, and some upgrade domains will be on the older version of your application.</span></span> <span data-ttu-id="1e519-147">Den nya versionen av programmet måste kunna läsa den tidigare versionen av data under driftsättningen, och den gamla versionen av programmet måste kunna läsa den nya versionen av dina data.</span><span class="sxs-lookup"><span data-stu-id="1e519-147">During the rollout, the new version of your application must be able to read the old version of your data, and the old version of your application must be able to read the new version of your data.</span></span> <span data-ttu-id="1e519-148">Om formatet inte är kompatibel med framåt och bakåt, misslyckas uppgraderingen eller värre, data kan försvinna eller skadas.</span><span class="sxs-lookup"><span data-stu-id="1e519-148">If the data format is not forward and backward compatible, the upgrade may fail, or worse, data may be lost or corrupted.</span></span>

<span data-ttu-id="1e519-149">Om du använder inbyggd serialisering, behöver du inte bekymra dig om kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="1e519-149">If you are using  built-in serializer, you do not have to worry about compatibility.</span></span>
<span data-ttu-id="1e519-150">Men om du använder en anpassad serialiserare eller DataContractSerializer måste data vara oändligt kompatibel bakåt och framåt.</span><span class="sxs-lookup"><span data-stu-id="1e519-150">However, if you are using a custom serializer or the DataContractSerializer, the data have to be infinitely backwards and forwards compatible.</span></span>
<span data-ttu-id="1e519-151">Varje version av serialiseraren måste med andra ord ska kunna serialisera och deserialisera någon version av typen.</span><span class="sxs-lookup"><span data-stu-id="1e519-151">In other words, each version of serializer needs to be able to serialize and de-serialize any version of the type.</span></span>

<span data-ttu-id="1e519-152">Data kontrakt-användare bör följa väldefinierade versionshantering regler för att lägga till, ta bort och ändra fält.</span><span class="sxs-lookup"><span data-stu-id="1e519-152">Data Contract users should follow the well-defined versioning rules for adding, removing, and changing fields.</span></span> <span data-ttu-id="1e519-153">Datakontrakt har också stöd för behandlar okänt fält och koppla upp i processen för serialisering och deserialisering samt hantera klassarv.</span><span class="sxs-lookup"><span data-stu-id="1e519-153">Data Contract also has support for dealing with unknown fields, hooking into the serialization and deserialization process, and dealing with class inheritance.</span></span> <span data-ttu-id="1e519-154">Mer information finns i [med datakontrakt](https://msdn.microsoft.com/library/ms733127.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e519-154">For more information, see [Using Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).</span></span>

<span data-ttu-id="1e519-155">Anpassad serialisering användare bör följa riktlinjerna i serialiseraren som de använder för att kontrollera att den är bakåtkompatibilitet och vidarebefordrar kompatibel.</span><span class="sxs-lookup"><span data-stu-id="1e519-155">Custom serializer users should adhere to the guidelines of the serializer they are using to make sure it is backwards and forwards compatible.</span></span>
<span data-ttu-id="1e519-156">Vanliga sätt stöder alla versioner är att lägga till informationen i början och bara att lägga till valfria egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1e519-156">Common way of supporting all versions is adding size information at the beginning and only adding optional properties.</span></span>
<span data-ttu-id="1e519-157">Det här sättet varje version kan läsa mycket kan och hoppa över resten av dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="1e519-157">This way each version can read as much it can and jump over the remaining part of the stream.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e519-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e519-158">Next steps</span></span>
  * [<span data-ttu-id="1e519-159">Serialisering och uppgradering</span><span class="sxs-lookup"><span data-stu-id="1e519-159">Serialization and upgrade</span></span>](service-fabric-application-upgrade-data-serialization.md)
  * [<span data-ttu-id="1e519-160">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="1e519-160">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * <span data-ttu-id="1e519-161">[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e519-161">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>
  * <span data-ttu-id="1e519-162">[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e519-162">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>
  * <span data-ttu-id="1e519-163">Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="1e519-163">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>
  * <span data-ttu-id="1e519-164">Lär dig hur du använder avancerade funktioner när du uppgraderar ditt program genom att referera till [avancerade ämnen](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="1e519-164">Learn how to use advanced functionality while upgrading your application by referring to [Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
  * <span data-ttu-id="1e519-165">Lösa vanliga problem i programuppgraderingar genom att referera till stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1e519-165">Fix common problems in application upgrades by referring to the steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
