---
title: "aaaChange FabricTransport inställningar i Azure mikrotjänster | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar Azure Service Fabric aktören kommunikationsinställningar."
services: Service-Fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: e312b475407eb95a435b93d80c0f2e9618b9ea1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-fabrictransport-settings-for-reliable-actors"></a><span data-ttu-id="ec253-103">Konfigurera inställningar för FabricTransport för Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="ec253-103">Configure FabricTransport settings for Reliable Actors</span></span>

<span data-ttu-id="ec253-104">Här följer hello-inställningar som du kan konfigurera:</span><span class="sxs-lookup"><span data-stu-id="ec253-104">Here are hello settings that you can configure:</span></span>
- <span data-ttu-id="ec253-105">C#: [FabricTransportRemotingSettings](
https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span><span class="sxs-lookup"><span data-stu-id="ec253-105">C#: [FabricTransportRemotingSettings](
https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span></span>
- <span data-ttu-id="ec253-106">Java: [FabricTransportRemotingSettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span><span class="sxs-lookup"><span data-stu-id="ec253-106">Java: [FabricTransportRemotingSettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)</span></span>

<span data-ttu-id="ec253-107">Du kan ändra hello standardkonfigurationen av FabricTransport på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="ec253-107">You can modify hello default configuration of FabricTransport in following ways.</span></span>

## <a name="assembly-attribute"></a><span data-ttu-id="ec253-108">Sammansättningsattributet</span><span class="sxs-lookup"><span data-stu-id="ec253-108">Assembly attribute</span></span>

<span data-ttu-id="ec253-109">Hej [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute) måste attributet toobe som tillämpats på hello aktören klienten och aktören service sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="ec253-109">hello [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute) attribute needs toobe applied on hello actor client and actor service assemblies.</span></span>

<span data-ttu-id="ec253-110">hello som följande exempel visar hur toochange hello standardvärdet FabricTransport OperationTimeout inställningar:</span><span class="sxs-lookup"><span data-stu-id="ec253-110">hello following example shows how toochange hello default value of FabricTransport OperationTimeout settings:</span></span>

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

   <span data-ttu-id="ec253-111">Andra exempel ändrar standard värden för FabricTransport MaxMessageSize och OperationTimeoutInSeconds.</span><span class="sxs-lookup"><span data-stu-id="ec253-111">Second example changes default Values of FabricTransport MaxMessageSize and OperationTimeoutInSeconds.</span></span>

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
   ```

## <a name="config-package"></a><span data-ttu-id="ec253-112">Konfigurationspaketet</span><span class="sxs-lookup"><span data-stu-id="ec253-112">Config package</span></span>

<span data-ttu-id="ec253-113">Du kan använda en [konfigurationspaketet](service-fabric-application-model.md) toomodify hello standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ec253-113">You can use a [config package](service-fabric-application-model.md) toomodify hello default configuration.</span></span>

### <a name="configure-fabrictransport-settings-for-hello-actor-service"></a><span data-ttu-id="ec253-114">Konfigurera inställningar för FabricTransport för hello aktören service</span><span class="sxs-lookup"><span data-stu-id="ec253-114">Configure FabricTransport settings for hello actor service</span></span>

<span data-ttu-id="ec253-115">Lägg till ett TransportSettings avsnitt i hello settings.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="ec253-115">Add a TransportSettings section in hello settings.xml file.</span></span>

<span data-ttu-id="ec253-116">Som standard söker aktören koden för SectionName som ”&lt;ActorName&gt;TransportSettings”.</span><span class="sxs-lookup"><span data-stu-id="ec253-116">By default, actor code looks for SectionName as "&lt;ActorName&gt;TransportSettings".</span></span> <span data-ttu-id="ec253-117">Om som hittas söker efter SectionName som ”TransportSettings”.</span><span class="sxs-lookup"><span data-stu-id="ec253-117">If that's not found, it checks for SectionName as "TransportSettings".</span></span>

  ```xml
  <Section Name="MyActorServiceTransportSettings">
       <Parameter Name="MaxMessageSize" Value="10000000" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
       <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
       <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
       <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
       <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
       <Parameter Name="CertificateStoreName" Value="My" />
       <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
       <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
   </Section>
  ```

### <a name="configure-fabrictransport-settings-for-hello-actor-client-assembly"></a><span data-ttu-id="ec253-118">Konfigurera inställningar för FabricTransport för hello aktören klientsammansättningen</span><span class="sxs-lookup"><span data-stu-id="ec253-118">Configure FabricTransport settings for hello actor client assembly</span></span>

<span data-ttu-id="ec253-119">Om hello klienten inte körs som en del av en tjänst, kan du skapa en ”&lt;klienten Exe-namn&gt;. settings.xml” filen i hello samma plats som hello klienten .exe-fil.</span><span class="sxs-lookup"><span data-stu-id="ec253-119">If hello client is not running as part of a service, you can create a "&lt;Client Exe Name&gt;.settings.xml" file in hello same location as hello client .exe file.</span></span> <span data-ttu-id="ec253-120">Lägg sedan till ett TransportSettings avsnitt i filen.</span><span class="sxs-lookup"><span data-stu-id="ec253-120">Then add a TransportSettings section in that file.</span></span> <span data-ttu-id="ec253-121">SectionName ska vara ”TransportSettings”.</span><span class="sxs-lookup"><span data-stu-id="ec253-121">SectionName should be "TransportSettings".</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
    <Section Name="TransportSettings">
      <Parameter Name="SecurityCredentialsType" Value="X509" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
      <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
      <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
      <Parameter Name="CertificateStoreName" Value="My" />
      <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    </Section>
  </Settings>
   ```

  * <span data-ttu-id="ec253-122">Konfigurera FabricTransport inställningar för säker aktören Tjänstklienten med sekundärt certifikat.</span><span class="sxs-lookup"><span data-stu-id="ec253-122">Configuring FabricTransport Settings for Secure Actor Service/Client With Secondary Certificate.</span></span>
  <span data-ttu-id="ec253-123">Sekundär certifikatinformationen kan läggas till genom att lägga till parametern CertificateFindValuebySecondary.</span><span class="sxs-lookup"><span data-stu-id="ec253-123">Secondary certificate information can be added by adding parameter CertificateFindValuebySecondary.</span></span>
  <span data-ttu-id="ec253-124">Nedan visas hello exempel hello lyssnare TransportSettings.</span><span class="sxs-lookup"><span data-stu-id="ec253-124">Below is hello example for hello Listener TransportSettings.</span></span>

    ```xml
    <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
    <Parameter Name="CertificateFindValuebySecondary" Value="h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
    <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C,a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
     <span data-ttu-id="ec253-125">Nedan visas hello exempel hello klienten TransportSettings.</span><span class="sxs-lookup"><span data-stu-id="ec253-125">Below is hello example for hello Client TransportSettings.</span></span>

    ```xml
   <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateFindValuebySecondary" Value="a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
    <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662,h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
    * <span data-ttu-id="ec253-126">Konfigurera inställningar för FabricTransport för att skydda aktören Service eller-klienten med hjälp av ämnesnamnet.</span><span class="sxs-lookup"><span data-stu-id="ec253-126">Configuring FabricTransport  Settings for Securing Actor Service/Client Using Subject Name.</span></span>
    <span data-ttu-id="ec253-127">Användaren måste tooprovide findType som FindBySubjectName, lägga till CertificateIssuerThumbprints och CertificateRemoteCommonNames värden.</span><span class="sxs-lookup"><span data-stu-id="ec253-127">User needs tooprovide findType as FindBySubjectName,add CertificateIssuerThumbprints and CertificateRemoteCommonNames values.</span></span>
  <span data-ttu-id="ec253-128">Nedan visas hello exempel hello lyssnare TransportSettings.</span><span class="sxs-lookup"><span data-stu-id="ec253-128">Below is hello example for hello Listener TransportSettings.</span></span>

     ```xml
    <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
    <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Alice" />
    <Parameter Name="CertificateIssuerThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
    <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Bob" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
    ```
  <span data-ttu-id="ec253-129">Nedan visas hello exempel hello klienten TransportSettings.</span><span class="sxs-lookup"><span data-stu-id="ec253-129">Below is hello example for hello Client TransportSettings.</span></span>

    ```xml
     <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
    <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Bob" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
     ```
