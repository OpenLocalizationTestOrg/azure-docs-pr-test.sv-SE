---
title: "Översikt över aaaMedia Services PlayReady licens mall"
description: "Det här avsnittet ger en översikt över en PlayReady licensmall som används för tooconfigure PlayReady-licenser."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 5a5ba930c56f70038db204681486ebc4308199fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-playready-license-template-overview"></a><span data-ttu-id="4f559-103">Media Services PlayReady licens mall översikt</span><span class="sxs-lookup"><span data-stu-id="4f559-103">Media Services PlayReady license template overview</span></span>
<span data-ttu-id="4f559-104">Azure Media Services innehåller nu en tjänst för att leverera Microsoft PlayReady-licenser.</span><span class="sxs-lookup"><span data-stu-id="4f559-104">Azure Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="4f559-105">När hello slutanvändaren player (till exempel Silverlight) försöker tooplay din PlayReady skyddat innehåll är en begäran skickades toohello licens leverans av tjänsten tooobtain en licens.</span><span class="sxs-lookup"><span data-stu-id="4f559-105">When hello end user player (for example, Silverlight) tries tooplay your PlayReady protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="4f559-106">Om hello Licenstjänsten godkänner hello begäran, den utfärdar hello-licens som skickade toohello klient och kan vara används toodecrypt och play hello angivna innehållet.</span><span class="sxs-lookup"><span data-stu-id="4f559-106">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="4f559-107">Media Services tillhandahåller också API: er som kan du konfigurera PlayReady-licenser.</span><span class="sxs-lookup"><span data-stu-id="4f559-107">Media Services also provides APIs that let you configure your PlayReady licenses.</span></span> <span data-ttu-id="4f559-108">Licenser innehålla hello rättigheter och begränsningar som du vill för hello PlayReady DRM runtime tooenforce när en användare försöker tooplayback skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-108">Licenses contain hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplayback protected content.</span></span>
<span data-ttu-id="4f559-109">Nedan följer några exempel på PlayReady licensbegränsningar som du kan ange:</span><span class="sxs-lookup"><span data-stu-id="4f559-109">Below are some examples of PlayReady license restrictions that you can specify:</span></span>

* <span data-ttu-id="4f559-110">hello DateTime från vilka hello licensen är giltig.</span><span class="sxs-lookup"><span data-stu-id="4f559-110">hello DateTime from which hello license is valid.</span></span>
* <span data-ttu-id="4f559-111">hello DateTime-värdet när hello licensen går ut.</span><span class="sxs-lookup"><span data-stu-id="4f559-111">hello DateTime value when hello license expires.</span></span> 
* <span data-ttu-id="4f559-112">För hello licens toobe sparas i permanent lagringsutrymme på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="4f559-112">For hello license toobe saved in persistent storage on hello client.</span></span> <span data-ttu-id="4f559-113">Beständiga licenser är brukar användas tooallow offline uppspelning av hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-113">Persistent licenses are typically used tooallow offline playback of hello content.</span></span>
* <span data-ttu-id="4f559-114">hello minsta säkerhetsnivå som en spelare måste ha tooplay ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-114">hello minimum security level that a player must have tooplay your content.</span></span> 
* <span data-ttu-id="4f559-115">hello utdata skyddsnivån för hello utdata kontroller för audio\video innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-115">hello output protection level for hello output controls for audio\video content.</span></span> 
* <span data-ttu-id="4f559-116">Mer information finns i avsnittet (3.5) i hello hello utdata-kontroller [PlayReady Kompatibilitetsregler](https://www.microsoft.com/playready/licensing/compliance/) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4f559-116">For more information, see hello Output Controls section (3.5) in hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>

> [!NOTE]
> <span data-ttu-id="4f559-117">För närvarande kan du bara konfigurera hello PlayRight hello PlayReady-licens (den här behörigheten krävs).</span><span class="sxs-lookup"><span data-stu-id="4f559-117">Currently, you can only configure hello PlayRight of hello PlayReady license (this right is required).</span></span> <span data-ttu-id="4f559-118">Hej PlayRight ger hello klient hello möjlighet tooplayback hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-118">hello PlayRight gives hello client hello ability tooplayback hello content.</span></span> <span data-ttu-id="4f559-119">Hej PlayRight kan också konfigurera begränsningar för specifika tooplayback.</span><span class="sxs-lookup"><span data-stu-id="4f559-119">hello PlayRight also allows configuring restrictions specific tooplayback.</span></span> <span data-ttu-id="4f559-120">Mer information finns i [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span><span class="sxs-lookup"><span data-stu-id="4f559-120">For more information, see [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span></span>
> 
> 

<span data-ttu-id="4f559-121">tooconfigure PlayReady-licenser med Media Services, måste du konfigurera hello Media Services PlayReady license-mall.</span><span class="sxs-lookup"><span data-stu-id="4f559-121">tooconfigure PlayReady licenses using Media Services, you must configure hello Media Services PlayReady license template.</span></span> <span data-ttu-id="4f559-122">hello mallen har definierats i XML.</span><span class="sxs-lookup"><span data-stu-id="4f559-122">hello template is defined in XML.</span></span>

<span data-ttu-id="4f559-123">hello visar följande exempel hello enklaste (och de vanligaste) mall som konfigurerar en grundläggande strömmande licens.</span><span class="sxs-lookup"><span data-stu-id="4f559-123">hello following example shows hello simplest (and most common) template that configures a basic streaming license.</span></span> <span data-ttu-id="4f559-124">Med denna licens, är dina klienter kan tooplayback din PlayReady skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="4f559-124">With this license, your clients would be able tooplayback your PlayReady protected content.</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

<span data-ttu-id="4f559-125">hello XML överensstämmer definierats i mallen för hello PlayReady licens XML schemaavsnittet toohello PlayReady licens mallen XML-schema.</span><span class="sxs-lookup"><span data-stu-id="4f559-125">hello XML conforms toohello PlayReady license template XML schema defined in hello PlayReady license template XML schema section.</span></span>

<span data-ttu-id="4f559-126">Media Services definierar också en uppsättning .NET-klasser som kan använda tooserialized och avserialiserat tooand från hello XML.</span><span class="sxs-lookup"><span data-stu-id="4f559-126">Media Services also defines a set of .NET classes that could be used tooserialized and deserialized tooand from hello XML.</span></span> <span data-ttu-id="4f559-127">Beskrivning av huvudsakliga klasser finns [Media Services .NET-klasser](media-services-playready-license-template-overview.md#classes).</span><span class="sxs-lookup"><span data-stu-id="4f559-127">For description of main classes, see [Media Services .NET classes](media-services-playready-license-template-overview.md#classes).</span></span> <span data-ttu-id="4f559-128">som har använt tooconfigure licens mallar.</span><span class="sxs-lookup"><span data-stu-id="4f559-128">that are used tooconfigure license templates.</span></span>

<span data-ttu-id="4f559-129">En slutpunkt till slutpunkt-exempel som använder .NET klasserna tooconfigure hello PlayReady license-mall, finns [med PlayReady dynamisk kryptering och Licensleveranstjänst](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="4f559-129">For an end-to-end example that uses .NET classes tooconfigure hello PlayReady license template, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

## <span data-ttu-id="4f559-130"><a id="classes"></a>Media Services .NET-klasser som har använt tooconfigure licens mallar</span><span class="sxs-lookup"><span data-stu-id="4f559-130"><a id="classes"></a>Media Services .NET classes that are used tooconfigure license templates</span></span>
<span data-ttu-id="4f559-131">hello följande är hello huvudsakliga .NET-klasser är mallar som används tooconfigure Media Services PlayReady licens.</span><span class="sxs-lookup"><span data-stu-id="4f559-131">hello following are hello main .NET classes are used tooconfigure Media Services PlayReady license templates.</span></span> <span data-ttu-id="4f559-132">De här klasserna mappa toohello typer som definieras i [PlayReady licens mallen XML-schema](media-services-playready-license-template-overview.md#schema).</span><span class="sxs-lookup"><span data-stu-id="4f559-132">These classes map toohello types defined in [PlayReady license template XML schema](media-services-playready-license-template-overview.md#schema).</span></span>

<span data-ttu-id="4f559-133">Hej [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) klass är används tooserialize och deserialisera tooand från hello Media Services licensmall-XML.</span><span class="sxs-lookup"><span data-stu-id="4f559-133">hello [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) class is used tooserialize and deserialize tooand from hello Media Services license template XML.</span></span>

### <a name="playreadylicenseresponsetemplate"></a><span data-ttu-id="4f559-134">PlayReadyLicenseResponseTemplate</span><span class="sxs-lookup"><span data-stu-id="4f559-134">PlayReadyLicenseResponseTemplate</span></span>
<span data-ttu-id="4f559-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) -den här klassen representerar hello mall för hello svar skickas tillbaka toohello slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="4f559-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - This class represents hello template for hello response sent back toohello end user.</span></span> <span data-ttu-id="4f559-136">Den innehåller ett fält för en anpassad sträng mellan hello licensservern och hello program (kan vara användbart för anpassad app logik) samt en lista över mallar för en eller flera licens.</span><span class="sxs-lookup"><span data-stu-id="4f559-136">It contains a field for a custom data string between hello license server and hello application (may be useful for custom app logic) as well as a list of one or more license templates.</span></span>

<span data-ttu-id="4f559-137">Detta är hello ”toppnivå” klass i hello Mallhierarki.</span><span class="sxs-lookup"><span data-stu-id="4f559-137">This is hello “top level” class in hello template hierarchy.</span></span> <span data-ttu-id="4f559-138">Vilket innebär att hello svar mallen innehåller en lista över licens mallar och hello licens mallar innehåller (direkt eller indirekt) alla hello andra klasser som utgör hello mallen data toobe serialiseras.</span><span class="sxs-lookup"><span data-stu-id="4f559-138">Meaning that hello response template includes a list of license templates and hello license templates include (directly or indirectly) all of hello other classes that make up hello template data toobe serialized.</span></span>

### <a name="playreadylicensetemplate"></a><span data-ttu-id="4f559-139">PlayReadyLicenseTemplate</span><span class="sxs-lookup"><span data-stu-id="4f559-139">PlayReadyLicenseTemplate</span></span>
<span data-ttu-id="4f559-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) -hello klassen representerar en licensmall för att skapa PlayReady-licenser toobe returnerade toohello slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="4f559-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - hello class represents a license template for creating PlayReady licenses toobe returned toohello end users.</span></span> <span data-ttu-id="4f559-141">Den innehåller hello data på hello innehållsnyckeln i hello licensen och några rättigheter eller begränsningar toobe upprätthållas av hello PlayReady DRM runtime när du använder hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="4f559-141">It contains hello data on hello content key in hello license and any rights or restrictions toobe enforced by hello PlayReady DRM runtime when using hello content key.</span></span>

### <span data-ttu-id="4f559-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span><span class="sxs-lookup"><span data-stu-id="4f559-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span></span>
<span data-ttu-id="4f559-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) -den här klassen representerar hello PlayRight för en PlayReady-licens.</span><span class="sxs-lookup"><span data-stu-id="4f559-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - This class represents hello PlayRight of a PlayReady license.</span></span> <span data-ttu-id="4f559-144">Det ger hello användaren hello möjlighet tooplayback hello innehåll ämne toohello noll eller fler begränsningar konfigurerad i hello licens och på hello PlayRight (för uppspelning specifik princip).</span><span class="sxs-lookup"><span data-stu-id="4f559-144">It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions configured in hello license and on hello PlayRight itself (for playback specific policy).</span></span> <span data-ttu-id="4f559-145">Mycket av hello principen på hello PlayRight har toodo med utdata begränsningar som styr hello typer av utdata som hello innehåll kan spelas över och eventuella begränsningar som ska föras på plats när du använder en viss utdata.</span><span class="sxs-lookup"><span data-stu-id="4f559-145">Much of hello policy on hello PlayRight has toodo with output restrictions which control hello types of outputs that hello content can be played over and any restrictions that must be put in place when using a given output.</span></span> <span data-ttu-id="4f559-146">Till exempel om hello DigitalVideoOnlyContentRestriction aktiveras sedan hello kan DRM körning hello video toobe visas över digitala utdata (analog video utdata inte tillåts toopass hello innehåll).</span><span class="sxs-lookup"><span data-stu-id="4f559-146">For example, if hello DigitalVideoOnlyContentRestriction is enabled, then hello DRM runtime will only allow hello video toobe displayed over digital outputs (analog video outputs won’t be allowed toopass hello content).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4f559-147">Dessa typer av begränsningar kan vara mycket kraftfulla men kan även påverka hello användarfunktioner.</span><span class="sxs-lookup"><span data-stu-id="4f559-147">These types of restrictions can be very powerful but can also affect hello consumer experience.</span></span> <span data-ttu-id="4f559-148">Om hello utdata skydd konfigureras för begränsande kanske hello innehåll är på vissa klienter.</span><span class="sxs-lookup"><span data-stu-id="4f559-148">If hello output protections are configured too restrictive, hello content might be unplayable on some clients.</span></span> <span data-ttu-id="4f559-149">Mer information finns i hello [PlayReady Kompatibilitetsregler](https://www.microsoft.com/playready/licensing/compliance/) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4f559-149">For more information, see hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>
> 
> 

<span data-ttu-id="4f559-150">Ett exempel på vilka skydd nivåer har stöd för Silverlight finns: [Silverlight stöd för skydd av utdata](http://go.microsoft.com/fwlink/?LinkId=617318).</span><span class="sxs-lookup"><span data-stu-id="4f559-150">For an example of what protection levels Silverlight supports, see: [Silverlight support for output protections](http://go.microsoft.com/fwlink/?LinkId=617318).</span></span>

## <span data-ttu-id="4f559-151"><a id="schema"></a>PlayReady-licens mallen XML-schema</span><span class="sxs-lookup"><span data-stu-id="4f559-151"><a id="schema"></a>PlayReady license template XML schema</span></span>
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a><span data-ttu-id="4f559-152">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="4f559-152">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4f559-153">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4f559-153">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

