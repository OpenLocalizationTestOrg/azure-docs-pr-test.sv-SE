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
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a><span data-ttu-id="f2a12-103">Tillförlitliga samling objekt serialisering i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f2a12-103">Reliable Collection object serialization in Azure Service Fabric</span></span>
<span data-ttu-id="f2a12-104">Tillförlitliga samlingar replikera och deras objekt toomake är beständig mellan datorn fel och strömavbrott är kvar.</span><span class="sxs-lookup"><span data-stu-id="f2a12-104">Reliable Collections' replicate and persist their items toomake sure they are durable across machine failures and power outages.</span></span>
<span data-ttu-id="f2a12-105">Både tooreplicate och toopersist objekt, tillförlitlig samlingar måste tooserialize dem.</span><span class="sxs-lookup"><span data-stu-id="f2a12-105">Both tooreplicate and toopersist items, Reliable Collections' need tooserialize them.</span></span>

<span data-ttu-id="f2a12-106">Tillförlitliga samlingar hämta hello lämplig serialisering för en viss typ från tillförlitliga Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="f2a12-106">Reliable Collections' get hello appropriate serializer for a given type from Reliable State Manager.</span></span>
<span data-ttu-id="f2a12-107">Tillåter anpassade serializers toobe registrerats för en viss typ tillförlitlig Tillståndshanterare eftersom det innehåller inbyggda serializers.</span><span class="sxs-lookup"><span data-stu-id="f2a12-107">Reliable State Manager contains built-in serializers and allows custom serializers toobe registered for a given type.</span></span>

## <a name="built-in-serializers"></a><span data-ttu-id="f2a12-108">Inbyggda Serializers</span><span class="sxs-lookup"><span data-stu-id="f2a12-108">Built-in Serializers</span></span>

<span data-ttu-id="f2a12-109">Tillstånd för tillförlitlig Manager innehåller inbyggda serialiseraren för vissa typer av vanliga så att de kan serialiseras effektivt som standard.</span><span class="sxs-lookup"><span data-stu-id="f2a12-109">Reliable State Manager includes built-in serializer for some common types, so that they can be serialized efficiently by default.</span></span> <span data-ttu-id="f2a12-110">För andra typer tillförlitliga Tillståndshanterare faller tillbaka toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="f2a12-110">For other types, Reliable State Manager falls back toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span></span>
<span data-ttu-id="f2a12-111">Inbyggda serializers är effektivare eftersom de redan känner till deras typer kan inte ändra och de behöver inte tooinclude information om hello typ som dess namn.</span><span class="sxs-lookup"><span data-stu-id="f2a12-111">Built-in serializers are more efficient since they know their types cannot change and they do not need tooinclude information about hello type like its type name.</span></span>

<span data-ttu-id="f2a12-112">Tillförlitlig Tillståndshanterare har inbyggd serialisering för följande typer:</span><span class="sxs-lookup"><span data-stu-id="f2a12-112">Reliable State Manager has built-in serializer for following types:</span></span> 
- <span data-ttu-id="f2a12-113">GUID</span><span class="sxs-lookup"><span data-stu-id="f2a12-113">Guid</span></span>
- <span data-ttu-id="f2a12-114">bool</span><span class="sxs-lookup"><span data-stu-id="f2a12-114">bool</span></span>
- <span data-ttu-id="f2a12-115">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="f2a12-115">byte</span></span>
- <span data-ttu-id="f2a12-116">sbyte</span><span class="sxs-lookup"><span data-stu-id="f2a12-116">sbyte</span></span>
- <span data-ttu-id="f2a12-117">byte]</span><span class="sxs-lookup"><span data-stu-id="f2a12-117">byte[]</span></span>
- <span data-ttu-id="f2a12-118">Char</span><span class="sxs-lookup"><span data-stu-id="f2a12-118">char</span></span>
- <span data-ttu-id="f2a12-119">Sträng</span><span class="sxs-lookup"><span data-stu-id="f2a12-119">string</span></span>
- <span data-ttu-id="f2a12-120">Decimal</span><span class="sxs-lookup"><span data-stu-id="f2a12-120">decimal</span></span>
- <span data-ttu-id="f2a12-121">dubbla</span><span class="sxs-lookup"><span data-stu-id="f2a12-121">double</span></span>
- <span data-ttu-id="f2a12-122">flyttal</span><span class="sxs-lookup"><span data-stu-id="f2a12-122">float</span></span>
- <span data-ttu-id="f2a12-123">int</span><span class="sxs-lookup"><span data-stu-id="f2a12-123">int</span></span>
- <span data-ttu-id="f2a12-124">uint</span><span class="sxs-lookup"><span data-stu-id="f2a12-124">uint</span></span>
- <span data-ttu-id="f2a12-125">lång</span><span class="sxs-lookup"><span data-stu-id="f2a12-125">long</span></span>
- <span data-ttu-id="f2a12-126">ulong</span><span class="sxs-lookup"><span data-stu-id="f2a12-126">ulong</span></span>
- <span data-ttu-id="f2a12-127">kort</span><span class="sxs-lookup"><span data-stu-id="f2a12-127">short</span></span>
- <span data-ttu-id="f2a12-128">ushort</span><span class="sxs-lookup"><span data-stu-id="f2a12-128">ushort</span></span>

## <a name="custom-serialization"></a><span data-ttu-id="f2a12-129">Anpassad serialisering</span><span class="sxs-lookup"><span data-stu-id="f2a12-129">Custom Serialization</span></span>

<span data-ttu-id="f2a12-130">Anpassade serializers är vanliga tooincrease prestanda eller tooencrypt hello data över hello överföring och på disken.</span><span class="sxs-lookup"><span data-stu-id="f2a12-130">Custom serializers are commonly used tooincrease performance or tooencrypt hello data over hello wire and on disk.</span></span> <span data-ttu-id="f2a12-131">Anpassade serializers är ofta effektivare än allmänna serialiseraren anledningarna eftersom de inte behöver tooserialize information om hello typen.</span><span class="sxs-lookup"><span data-stu-id="f2a12-131">Among other reasons, custom serializers are commonly more efficient than generic serializer since they don't need tooserialize information about hello type.</span></span> 

<span data-ttu-id="f2a12-132">[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) är används tooregister en anpassad serialisering för hello typ T. Denna registrering ska hända i hello konstruktion hello StatefulServiceBase tooensure innan återställningen startas, har alla tillförlitliga samlingar kommer åt toohello relevanta serialiseraren tooread sina beständiga data.</span><span class="sxs-lookup"><span data-stu-id="f2a12-132">[IReliableStateManager.TryAddStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) is used tooregister a custom serializer for hello given type T. This registration should happen in hello construction of hello StatefulServiceBase tooensure that before recovery starts, all Reliable Collections have access toohello relevant serializer tooread their persisted data.</span></span>

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
> <span data-ttu-id="f2a12-133">Anpassade serializers ges företräde över inbyggda serializers.</span><span class="sxs-lookup"><span data-stu-id="f2a12-133">Custom serializers are given precedence over built-in serializers.</span></span> <span data-ttu-id="f2a12-134">Till exempel när en anpassad serialisering för int registreras, är det används tooserialize heltal i stället för hello inbyggd serialisering för int.</span><span class="sxs-lookup"><span data-stu-id="f2a12-134">For example, when a custom serializer for int is registered, it is used tooserialize integers instead of hello built-in serializer for int.</span></span>

### <a name="how-tooimplement-a-custom-serializer"></a><span data-ttu-id="f2a12-135">Hur tooimplement en anpassad serialisering</span><span class="sxs-lookup"><span data-stu-id="f2a12-135">How tooimplement a custom serializer</span></span>

<span data-ttu-id="f2a12-136">En anpassad serialiserare måste tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f2a12-136">A custom serializer needs tooimplement hello [IStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.</span></span>

> [!NOTE]
> <span data-ttu-id="f2a12-137">IStateSerializer<T> innehåller en överlagring för skrivning och Läs som tar en ytterligare ton kallas Basvärde.</span><span class="sxs-lookup"><span data-stu-id="f2a12-137">IStateSerializer<T> includes an overload for Write and Read that takes in an additional T called base value.</span></span> <span data-ttu-id="f2a12-138">Detta API är för differentiell serialisering.</span><span class="sxs-lookup"><span data-stu-id="f2a12-138">This API is for differential serialization.</span></span> <span data-ttu-id="f2a12-139">För närvarande visas differentiell serialisering funktionen inte.</span><span class="sxs-lookup"><span data-stu-id="f2a12-139">Currently differential serialization feature is not exposed.</span></span> <span data-ttu-id="f2a12-140">Dessa två överlagringar kallas därför inte förrän differentiell serialisering exponeras och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="f2a12-140">Hence, these two overloads are not called until differential serialization is exposed and enabled.</span></span>

<span data-ttu-id="f2a12-141">Följande är ett exempel anpassad typ som kallas OrderKey som innehåller fyra egenskaper</span><span class="sxs-lookup"><span data-stu-id="f2a12-141">Following is an example custom type called OrderKey that contains four properties</span></span>

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

<span data-ttu-id="f2a12-142">Följande är exempel på implementering av IStateSerializer<OrderKey>.</span><span class="sxs-lookup"><span data-stu-id="f2a12-142">Following is an example implementation of IStateSerializer<OrderKey>.</span></span>
<span data-ttu-id="f2a12-143">Observera att läsa och skriva överlagringar som i baseValue, anropa sina respektive överlagring för vidarebefordran kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="f2a12-143">Note that Read and Write overloads that take in baseValue, call their respective overload for forwards compatibility.</span></span>

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

## <a name="upgradability"></a><span data-ttu-id="f2a12-144">Möjligheterna</span><span class="sxs-lookup"><span data-stu-id="f2a12-144">Upgradability</span></span>
<span data-ttu-id="f2a12-145">I en [löpande uppgradering av programmet](service-fabric-application-upgrade.md), hello uppgraderingen är tillämpade tooa del noder, en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="f2a12-145">In a [rolling application upgrade](service-fabric-application-upgrade.md), hello upgrade is applied tooa subset of nodes, one upgrade domain at a time.</span></span> <span data-ttu-id="f2a12-146">Under den här processen kommer att vissa uppgraderingsdomäner hello nyare version av programmet och vissa uppgraderingsdomäner blir hello äldre versioner av programmet.</span><span class="sxs-lookup"><span data-stu-id="f2a12-146">During this process, some upgrade domains will be on hello newer version of your application, and some upgrade domains will be on hello older version of your application.</span></span> <span data-ttu-id="f2a12-147">Under den här fasen hello hello ny version av programmet måste vara kan tooread hello gammal version av dina data och hello gammal version av programmet måste vara kan tooread hello ny version av dina data.</span><span class="sxs-lookup"><span data-stu-id="f2a12-147">During hello rollout, hello new version of your application must be able tooread hello old version of your data, and hello old version of your application must be able tooread hello new version of your data.</span></span> <span data-ttu-id="f2a12-148">Om hello dataformat inte framåt och bakåt kompatibla, hello-uppgraderingen kan misslyckas eller värre, data går förlorat eller skadat.</span><span class="sxs-lookup"><span data-stu-id="f2a12-148">If hello data format is not forward and backward compatible, hello upgrade may fail, or worse, data may be lost or corrupted.</span></span>

<span data-ttu-id="f2a12-149">Om du använder inbyggd funktion för serialisering har inte tooworry om kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="f2a12-149">If you are using  built-in serializer, you do not have tooworry about compatibility.</span></span>
<span data-ttu-id="f2a12-150">Men om du använder en anpassad serialisering eller hello DataContractSerializer hello data har toobe oändligt bakåtkompatibilitet och vidarebefordrar kompatibel.</span><span class="sxs-lookup"><span data-stu-id="f2a12-150">However, if you are using a custom serializer or hello DataContractSerializer, hello data have toobe infinitely backwards and forwards compatible.</span></span>
<span data-ttu-id="f2a12-151">Med andra ord varje version av serialiseraren måste toobe kan tooserialize och deserialisera någon version av hello-typen.</span><span class="sxs-lookup"><span data-stu-id="f2a12-151">In other words, each version of serializer needs toobe able tooserialize and de-serialize any version of hello type.</span></span>

<span data-ttu-id="f2a12-152">Data kontrakt-användare bör följa hello väldefinierade versionshantering regler för att lägga till, ta bort och ändra fält.</span><span class="sxs-lookup"><span data-stu-id="f2a12-152">Data Contract users should follow hello well-defined versioning rules for adding, removing, and changing fields.</span></span> <span data-ttu-id="f2a12-153">Datakontrakt har också stöd för behandlar okänt fält, koppla upp i hello serialisering och deserialisering processen och behandlar klassarv.</span><span class="sxs-lookup"><span data-stu-id="f2a12-153">Data Contract also has support for dealing with unknown fields, hooking into hello serialization and deserialization process, and dealing with class inheritance.</span></span> <span data-ttu-id="f2a12-154">Mer information finns i [med datakontrakt](https://msdn.microsoft.com/library/ms733127.aspx).</span><span class="sxs-lookup"><span data-stu-id="f2a12-154">For more information, see [Using Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).</span></span>

<span data-ttu-id="f2a12-155">Anpassad serialisering användare bör följa toohello riktlinjer hello serialiserare som de använder toomake är bakåtkompatibilitet och vidarebefordrar kompatibel.</span><span class="sxs-lookup"><span data-stu-id="f2a12-155">Custom serializer users should adhere toohello guidelines of hello serializer they are using toomake sure it is backwards and forwards compatible.</span></span>
<span data-ttu-id="f2a12-156">Vanligt sätt stöder alla versioner är att lägga till Storleksinformation på hello början och bara att lägga till valfria egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f2a12-156">Common way of supporting all versions is adding size information at hello beginning and only adding optional properties.</span></span>
<span data-ttu-id="f2a12-157">Det här sättet varje version kan läsa så mycket kan och hoppa över hello resten av hello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="f2a12-157">This way each version can read as much it can and jump over hello remaining part of hello stream.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2a12-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2a12-158">Next steps</span></span>
  * [<span data-ttu-id="f2a12-159">Serialisering och uppgradering</span><span class="sxs-lookup"><span data-stu-id="f2a12-159">Serialization and upgrade</span></span>](service-fabric-application-upgrade-data-serialization.md)
  * [<span data-ttu-id="f2a12-160">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="f2a12-160">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * <span data-ttu-id="f2a12-161">[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2a12-161">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>
  * <span data-ttu-id="f2a12-162">[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f2a12-162">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>
  * <span data-ttu-id="f2a12-163">Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="f2a12-163">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>
  * <span data-ttu-id="f2a12-164">Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="f2a12-164">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
  * <span data-ttu-id="f2a12-165">Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f2a12-165">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
