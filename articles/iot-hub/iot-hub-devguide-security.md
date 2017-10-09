---
title: "aaaUnderstand Azure IoT Hub säkerhet | Microsoft Docs"
description: "Utvecklare guide - hur toocontrol åt tooIoT hubb för appar för enheter och backend-appar. Innehåller information om säkerhetstoken och stöd för X.509-certifikat."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a><span data-ttu-id="85fbb-104">Kontrollera åtkomst tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="85fbb-104">Control access tooIoT Hub</span></span>

<span data-ttu-id="85fbb-105">Den här artikeln beskrivs hello alternativ för att skydda din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-105">This article describes hello options for securing your IoT hub.</span></span> <span data-ttu-id="85fbb-106">IoT-hubb använder *behörigheter* toogrant åtkomst tooeach IoT-hubb slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="85fbb-106">IoT Hub uses *permissions* toogrant access tooeach IoT hub endpoint.</span></span> <span data-ttu-id="85fbb-107">Behörigheter begränsa hello åtkomst tooan IoT-hubb baseras på funktionen.</span><span class="sxs-lookup"><span data-stu-id="85fbb-107">Permissions limit hello access tooan IoT hub based on functionality.</span></span>

<span data-ttu-id="85fbb-108">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="85fbb-108">This article describes:</span></span>

* <span data-ttu-id="85fbb-109">hello olika behörigheter som du kan bevilja tooa enhet eller backend-app tooaccess din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-109">hello different permissions that you can grant tooa device or back-end app tooaccess your IoT hub.</span></span>
* <span data-ttu-id="85fbb-110">Hej processen och hello autentiseringstoken används tooverify behörigheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-110">hello authentication process and hello tokens it uses tooverify permissions.</span></span>
* <span data-ttu-id="85fbb-111">Hur autentiseringsuppgifter tooscope toolimit åtkomst toospecific resurser.</span><span class="sxs-lookup"><span data-stu-id="85fbb-111">How tooscope credentials toolimit access toospecific resources.</span></span>
* <span data-ttu-id="85fbb-112">IoT-hubb stöd för X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="85fbb-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="85fbb-113">Anpassade autentiseringsmekanismer som använder befintliga enhetens identitet register eller autentiseringsscheman.</span><span class="sxs-lookup"><span data-stu-id="85fbb-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="85fbb-114">När toouse</span><span class="sxs-lookup"><span data-stu-id="85fbb-114">When toouse</span></span>

<span data-ttu-id="85fbb-115">Du måste ha behörighet tooaccess någon hello IoT-hubbslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-115">You must have appropriate permissions tooaccess any of hello IoT Hub endpoints.</span></span> <span data-ttu-id="85fbb-116">Till exempel måste en enhet innehålla en token som innehåller säkerhetsreferenser tillsammans med alla meddelanden som skickas tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-116">For example, a device must include a token containing security credentials along with every message it sends tooIoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="85fbb-117">Åtkomstkontroll och behörigheter</span><span class="sxs-lookup"><span data-stu-id="85fbb-117">Access control and permissions</span></span>

<span data-ttu-id="85fbb-118">Du kan ge [behörigheter](#iot-hub-permissions) i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="85fbb-118">You can grant [permissions](#iot-hub-permissions) in hello following ways:</span></span>

* <span data-ttu-id="85fbb-119">**IoT-hubb-nivå delade åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="85fbb-120">Principer för delad åtkomst kan ge en kombination av [behörigheter](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="85fbb-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="85fbb-121">Du kan definiera principer i hello [Azure-portalen][lnk-management-portal], eller programmässigt med hjälp av hello [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="85fbb-121">You can define policies in hello [Azure portal][lnk-management-portal], or programmatically by using hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="85fbb-122">En nyligen skapade IoT-hubben har hello följande standardprinciperna:</span><span class="sxs-lookup"><span data-stu-id="85fbb-122">A newly created IoT hub has hello following default policies:</span></span>

  * <span data-ttu-id="85fbb-123">**iothubowner**: princip med alla behörigheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="85fbb-124">**tjänsten**: princip med **ServiceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="85fbb-125">**enheten**: princip med **DeviceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="85fbb-126">**registryRead**: princip med **RegistryRead** behörighet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="85fbb-127">**registryReadWrite**: princip med **RegistryRead** och RegistryWrite behörigheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="85fbb-128">**Per enhet säkerhetsreferenser**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-128">**Per-device security credentials**.</span></span> <span data-ttu-id="85fbb-129">Varje IoT-hubb innehåller en [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="85fbb-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="85fbb-130">Du kan konfigurera säkerhetsreferenser som ger för varje enhet i den här identitetsregistret **DeviceConnect** behörigheter omfång toohello motsvarande enhet slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped toohello corresponding device endpoints.</span></span>

<span data-ttu-id="85fbb-131">Till exempel i en typisk IoT-lösningen:</span><span class="sxs-lookup"><span data-stu-id="85fbb-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="85fbb-132">hello device management komponenten använder hello *registryReadWrite* princip.</span><span class="sxs-lookup"><span data-stu-id="85fbb-132">hello device management component uses hello *registryReadWrite* policy.</span></span>
* <span data-ttu-id="85fbb-133">hello händelse processorkomponent använder hello *service* princip.</span><span class="sxs-lookup"><span data-stu-id="85fbb-133">hello event processor component uses hello *service* policy.</span></span>
* <span data-ttu-id="85fbb-134">hello körning enheten business logic komponenten använder hello *service* princip.</span><span class="sxs-lookup"><span data-stu-id="85fbb-134">hello run-time device business logic component uses hello *service* policy.</span></span>
* <span data-ttu-id="85fbb-135">Enskilda enheter ansluter med autentiseringsuppgifter som lagras i registret för hello IoT hub-identitet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-135">Individual devices connect using credentials stored in hello IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="85fbb-136">Se [behörigheter](#iot-hub-permissions) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="85fbb-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="85fbb-137">Autentisering</span><span class="sxs-lookup"><span data-stu-id="85fbb-137">Authentication</span></span>

<span data-ttu-id="85fbb-138">Azure IoT-hubb beviljar åtkomst tooendpoints genom att verifiera en token mot hello delade åtkomstprinciper och identitet säkerhetsreferenser för registret.</span><span class="sxs-lookup"><span data-stu-id="85fbb-138">Azure IoT Hub grants access tooendpoints by verifying a token against hello shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="85fbb-139">Säkerhetsreferenser, till exempel symmetriska nycklar skickas aldrig via hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="85fbb-139">Security credentials, such as symmetric keys, are never sent over hello wire.</span></span>

> [!NOTE]
> <span data-ttu-id="85fbb-140">hello Azure IoT Hub-resursprovidern skyddas via Azure-prenumerationen, eftersom alla providrar på hello [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="85fbb-140">hello Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in hello [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="85fbb-141">Mer information om hur tooconstruct och använda säkerhetstoken finns [IoT-hubb säkerhetstoken][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="85fbb-141">For more information about how tooconstruct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="85fbb-142">Protokollet programvarukrav</span><span class="sxs-lookup"><span data-stu-id="85fbb-142">Protocol specifics</span></span>

<span data-ttu-id="85fbb-143">Varje protokoll som stöds, till exempel MQTT, AMQP och HTTP, transport token på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="85fbb-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="85fbb-144">När du använder MQTT hello Anslut paket har hello deviceId som hello ClientId, {iothubhostname} / {deviceId} i hello användarnamn och en SAS-token i hello lösenordsfältet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-144">When using MQTT, hello CONNECT packet has hello deviceId as hello ClientId, {iothubhostname}/{deviceId} in hello Username field, and a SAS token in hello Password field.</span></span> <span data-ttu-id="85fbb-145">{iothubhostname} bör hello fullständig CName för hello IoT-hubb (till exempel contoso.azure-devices.net).</span><span class="sxs-lookup"><span data-stu-id="85fbb-145">{iothubhostname} should be hello full CName of hello IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="85fbb-146">När du använder [AMQP][lnk-amqp], IoT-hubb stöder [SASL OFORMATERAD] [ lnk-sasl-plain] och [AMQP anspråk baserade-säkerhet] [ lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="85fbb-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="85fbb-147">Om du använder AMQP anspråk baserade-säkerhet, hello standard anger hur tootransmit dessa token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-147">If you use AMQP claims-based-security, hello standard specifies how tootransmit these tokens.</span></span>

<span data-ttu-id="85fbb-148">SASL OFORMATERAD hello **användarnamn** kan vara:</span><span class="sxs-lookup"><span data-stu-id="85fbb-148">For SASL PLAIN, hello **username** can be:</span></span>

* <span data-ttu-id="85fbb-149">`{policyName}@sas.root.{iothubName}`Om du använder token för IoT-hubb-nivå.</span><span class="sxs-lookup"><span data-stu-id="85fbb-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="85fbb-150">`{deviceId}@sas.{iothubname}`Om du använder enheten omfång token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="85fbb-151">I båda fallen hello lösenordsfältet innehåller hello-token i [IoT-hubb säkerhetstoken][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="85fbb-151">In both cases, hello password field contains hello token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="85fbb-152">HTTP implementerar autentisering genom att inkludera en giltig token i hello **auktorisering** huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="85fbb-152">HTTP implements authentication by including a valid token in hello **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="85fbb-153">Exempel</span><span class="sxs-lookup"><span data-stu-id="85fbb-153">Example</span></span>

<span data-ttu-id="85fbb-154">Användarnamn (DeviceId är skiftlägeskänsligt):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="85fbb-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="85fbb-155">Lösenord (generera SAS-token med hello [enheten explorer] [ lnk-device-explorer] verktyget):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="85fbb-155">Password (Generate SAS token with hello [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="85fbb-156">Hej [Azure IoT SDK] [ lnk-sdks] automatiskt generera token när du ansluter toohello service.</span><span class="sxs-lookup"><span data-stu-id="85fbb-156">hello [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting toohello service.</span></span> <span data-ttu-id="85fbb-157">I vissa fall stöder inte hello Azure IoT SDK alla hello protokoll eller alla hello autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="85fbb-157">In some cases, hello Azure IoT SDKs do not support all hello protocols or all hello authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="85fbb-158">Att tänka på SASL OFORMATERAD</span><span class="sxs-lookup"><span data-stu-id="85fbb-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="85fbb-159">När du använder SASL OFORMATERAD med AMQP kan kan en klient som ansluter tooan IoT-hubb använda en enda token för varje TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="85fbb-159">When using SASL PLAIN with AMQP, a client connecting tooan IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="85fbb-160">När hello-token upphör att gälla, hello TCP-anslutningen kopplas från hello-tjänsten och utlöser en återanslutning.</span><span class="sxs-lookup"><span data-stu-id="85fbb-160">When hello token expires, hello TCP connection disconnects from hello service and triggers a reconnect.</span></span> <span data-ttu-id="85fbb-161">Det här beteendet skadas när det inte problematisk för en backend-app, för en enhetsapp för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="85fbb-161">This behavior, while not problematic for a back-end app, is damaging for a device app for hello following reasons:</span></span>

* <span data-ttu-id="85fbb-162">Gateway ansluter vanligtvis för många enheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="85fbb-163">När du använder SASL OFORMATERAD, har de toocreate en distinkta TCP-anslutning för varje enhet som ansluter tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-163">When using SASL PLAIN, they have toocreate a distinct TCP connection for each device connecting tooan IoT hub.</span></span> <span data-ttu-id="85fbb-164">Det här scenariot avsevärt ökar hello förbrukningen av ström och nätverksresurser och ökar hello svarstiden för varje enhetsanslutning.</span><span class="sxs-lookup"><span data-stu-id="85fbb-164">This scenario considerably increases hello consumption of power and networking resources, and increases hello latency of each device connection.</span></span>
* <span data-ttu-id="85fbb-165">Begränsad resurs enheter påverkas negativt av hello ökad användning av resurser tooreconnect efter varje token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="85fbb-165">Resource-constrained devices are adversely affected by hello increased use of resources tooreconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="85fbb-166">Autentiseringsuppgifter för scope IoT hub-nivå</span><span class="sxs-lookup"><span data-stu-id="85fbb-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="85fbb-167">Du kan definiera säkerhetsprinciper för IoT-hubb-nivå genom att skapa token med en skyddad resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="85fbb-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="85fbb-168">Slutpunkten toosend enhet till moln hälsningsmeddelande från en enhet är till exempel **/devices/ {deviceId} / meddelanden/händelser**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-168">For example, hello endpoint toosend device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="85fbb-169">Du kan också använda en IoT-hubb-nivå delad åtkomstprincip med **DeviceConnect** behörigheter toosign en token vars resourceURI är **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions toosign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="85fbb-170">Den här metoden skapar en token som är bara användbara toosend meddelanden för enheten **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-170">This approach creates a token that is only usable toosend messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="85fbb-171">Den här mekanismen är liknande toohello [Händelsehubbar utgivarprincip][lnk-event-hubs-publisher-policy], och möjliggör tooimplement anpassade autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="85fbb-171">This mechanism is similar toohello [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you tooimplement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="85fbb-172">Säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="85fbb-172">Security tokens</span></span>

<span data-ttu-id="85fbb-173">IoT-hubb använder säkerhet tokens tooauthenticate enheter och tjänster tooavoid skicka nycklar hello uppkopplat läge.</span><span class="sxs-lookup"><span data-stu-id="85fbb-173">IoT Hub uses security tokens tooauthenticate devices and services tooavoid sending keys on hello wire.</span></span> <span data-ttu-id="85fbb-174">Dessutom är säkerhetstoken begränsad giltighetstid och omfång.</span><span class="sxs-lookup"><span data-stu-id="85fbb-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="85fbb-175">[Azure IoT-SDK] [ lnk-sdks] automatiskt generera token utan någon specialkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="85fbb-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="85fbb-176">Vissa scenarier göra kräver toogenerate och använda säkerhetstoken direkt.</span><span class="sxs-lookup"><span data-stu-id="85fbb-176">Some scenarios do require you toogenerate and use security tokens directly.</span></span> <span data-ttu-id="85fbb-177">Sådana scenarier är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-177">Such scenarios include:</span></span>

* <span data-ttu-id="85fbb-178">hello direkt användning av hello MQTT, AMQP och HTTP.</span><span class="sxs-lookup"><span data-stu-id="85fbb-178">hello direct use of hello MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="85fbb-179">Hej implementering av hello tokentjänsten mönster, enligt beskrivningen i [anpassad enhetsautentisering][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="85fbb-179">hello implementation of hello token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="85fbb-180">IoT-hubb kan också enheter tooauthenticate med IoT-hubb med [X.509-certifikat][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="85fbb-180">IoT Hub also allows devices tooauthenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="85fbb-181">Säkerhet token struktur</span><span class="sxs-lookup"><span data-stu-id="85fbb-181">Security token structure</span></span>

<span data-ttu-id="85fbb-182">Hjälp säkerhet token toogrant tid begränsas åtkomsten toodevices och tjänster toospecific funktioner i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-182">You use security tokens toogrant time-bounded access toodevices and services toospecific functionality in IoT Hub.</span></span> <span data-ttu-id="85fbb-183">tooget auktorisering tooconnect tooIoT Hub, enheter och tjänster måste skicka säkerhetstoken som signerade med en delad åtkomst eller en symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="85fbb-183">tooget authorization tooconnect tooIoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="85fbb-184">Dessa nycklar lagras med en enhetsidentitet i hello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="85fbb-184">These keys are stored with a device identity in hello identity registry.</span></span>

<span data-ttu-id="85fbb-185">En token som signerats med en delad åtkomst viktiga beviljar åtkomst tooall hello funktionalitet som är associerade med hello delade åtkomstbehörigheter för principen.</span><span class="sxs-lookup"><span data-stu-id="85fbb-185">A token signed with a shared access key grants access tooall hello functionality associated with hello shared access policy permissions.</span></span> <span data-ttu-id="85fbb-186">En token som signerats med en enhetsidentitet symmetrisk nyckel endast beviljar hello **DeviceConnect** för hello associerade enhetens identitet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-186">A token signed with a device identity's symmetric key only grants hello **DeviceConnect** permission for hello associated device identity.</span></span>

<span data-ttu-id="85fbb-187">hello säkerhetstoken har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="85fbb-187">hello security token has hello following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="85fbb-188">Här följer hello förväntade värden:</span><span class="sxs-lookup"><span data-stu-id="85fbb-188">Here are hello expected values:</span></span>

| <span data-ttu-id="85fbb-189">Värde</span><span class="sxs-lookup"><span data-stu-id="85fbb-189">Value</span></span> | <span data-ttu-id="85fbb-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85fbb-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="85fbb-191">{signatur}</span><span class="sxs-lookup"><span data-stu-id="85fbb-191">{signature}</span></span> |<span data-ttu-id="85fbb-192">En sträng med HMAC SHA256 signatur hello formatet: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="85fbb-192">An HMAC-SHA256 signature string of hello form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="85fbb-193">**Viktiga**: hello nyckeln avkoda från base64 och användas som nyckel tooperform hello HMAC SHA256 beräkning.</span><span class="sxs-lookup"><span data-stu-id="85fbb-193">**Important**: hello key is decoded from base64 and used as key tooperform hello HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="85fbb-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="85fbb-194">{resourceURI}</span></span> |<span data-ttu-id="85fbb-195">URI-prefix (av segment) hello-slutpunkter som kan användas med denna token som börjar med värdnamnet för hello IoT-hubb (ingen protocol).</span><span class="sxs-lookup"><span data-stu-id="85fbb-195">URI prefix (by segment) of hello endpoints that can be accessed with this token, starting with host name of hello IoT hub (no protocol).</span></span> <span data-ttu-id="85fbb-196">Till exempel, `myHub.azure-devices.net/devices/device1`</span><span class="sxs-lookup"><span data-stu-id="85fbb-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="85fbb-197">{utgången}</span><span class="sxs-lookup"><span data-stu-id="85fbb-197">{expiry}</span></span> |<span data-ttu-id="85fbb-198">UTF8 strängar för antal sekunder sedan hello epok 00:00:00 UTC på 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="85fbb-198">UTF8 strings for number of seconds since hello epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="85fbb-199">{URL-kodade-resourceURI}</span><span class="sxs-lookup"><span data-stu-id="85fbb-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="85fbb-200">Lägre fall URL-kodning av hello gemen resurs-URI</span><span class="sxs-lookup"><span data-stu-id="85fbb-200">Lower case URL-encoding of hello lower case resource URI</span></span> |
| <span data-ttu-id="85fbb-201">{policyName}</span><span class="sxs-lookup"><span data-stu-id="85fbb-201">{policyName}</span></span> |<span data-ttu-id="85fbb-202">hello namnet på hello delad åtkomst princip toowhich refererar detta token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-202">hello name of hello shared access policy toowhich this token refers.</span></span> <span data-ttu-id="85fbb-203">Hello token refererar inte fram om toodevice registret autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-203">Absent if hello token refers toodevice-registry credentials.</span></span> |

<span data-ttu-id="85fbb-204">**Observera på prefixet**: hello URI-prefix beräknas av segment och inte av tecken.</span><span class="sxs-lookup"><span data-stu-id="85fbb-204">**Note on prefix**: hello URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="85fbb-205">Till exempel `/a/b` är ett prefix för `/a/b/c` men inte för `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="85fbb-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="85fbb-206">hello följande Node.js utdrag visar en funktion kallad **generateSasToken** som beräknar hello token från hello indata `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="85fbb-206">hello following Node.js snippet shows a function called **generateSasToken** that computes hello token from hello inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="85fbb-207">hello nästa avsnitt innehåller information om hur tooinitialize hello olika indata för hello annan token användningsfall.</span><span class="sxs-lookup"><span data-stu-id="85fbb-207">hello next sections detail how tooinitialize hello different inputs for hello different token use cases.</span></span>

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

<span data-ttu-id="85fbb-208">Som en jämförelse code hello motsvarande Python toogenerate en säkerhetstoken är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-208">As a comparison, hello equivalent Python code toogenerate a security token is:</span></span>

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> <span data-ttu-id="85fbb-209">Eftersom hello giltighetstid hello token verifieras på IoT-hubb datorer måste hello drift på hello klocka hello-dator som genererar hello token vara minimal.</span><span class="sxs-lookup"><span data-stu-id="85fbb-209">Since hello time validity of hello token is validated on IoT Hub machines, hello drift on hello clock of hello machine that generates hello token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="85fbb-210">Använd SAS-token i en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="85fbb-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="85fbb-211">Det finns två sätt tooobtain **DeviceConnect** behörigheter med IoT-hubb med säkerhetstoken: använda en [symmetriska enhetsnyckel från hello identitetsregistret](#use-a-symmetric-key-in-the-identity-registry), eller Använd en [delade åtkomstnyckeln](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="85fbb-211">There are two ways tooobtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from hello identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="85fbb-212">Kom ihåg att alla funktioner som är tillgängliga från enheter exponeras avsiktligt på slutpunkter med prefixet `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="85fbb-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85fbb-213">hello enda sättet att IoT-hubb autentiserar en enhet använder hello enheten identitet symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="85fbb-213">hello only way that IoT Hub authenticates a specific device is using hello device identity symmetric key.</span></span> <span data-ttu-id="85fbb-214">I fall när en princip för delad åtkomst är används tooaccess enhetsfunktion hello lösningen måste ta hänsyn till hello-komponent som utfärdar hello säkerhetstoken som en betrodd underkomponenten.</span><span class="sxs-lookup"><span data-stu-id="85fbb-214">In cases when a shared access policy is used tooaccess device functionality, hello solution must consider hello component issuing hello security token as a trusted subcomponent.</span></span>

<span data-ttu-id="85fbb-215">hello enheten riktade slutpunkter är (oavsett hello protocol):</span><span class="sxs-lookup"><span data-stu-id="85fbb-215">hello device-facing endpoints are (irrespective of hello protocol):</span></span>

| <span data-ttu-id="85fbb-216">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="85fbb-216">Endpoint</span></span> | <span data-ttu-id="85fbb-217">Funktioner</span><span class="sxs-lookup"><span data-stu-id="85fbb-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="85fbb-218">Skicka meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="85fbb-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="85fbb-219">Ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a><span data-ttu-id="85fbb-220">Använd en symmetrisk nyckel i registret för hello identitet</span><span class="sxs-lookup"><span data-stu-id="85fbb-220">Use a symmetric key in hello identity registry</span></span>

<span data-ttu-id="85fbb-221">När du använder en enhetsidentitet symmetrisk nyckel toogenerate en token hello principnamn (`skn`) element av hello token utelämnas.</span><span class="sxs-lookup"><span data-stu-id="85fbb-221">When using a device identity's symmetric key toogenerate a token, hello policyName (`skn`) element of hello token is omitted.</span></span>

<span data-ttu-id="85fbb-222">Exempelvis skapas en token tooaccess alla enhetsfunktion bör ha hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="85fbb-222">For example, a token created tooaccess all device functionality should have hello following parameters:</span></span>

* <span data-ttu-id="85fbb-223">resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="85fbb-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="85fbb-224">nyckel för signeringscertifikatet: en symmetrisk nyckel för hello `{device id}` identitet,</span><span class="sxs-lookup"><span data-stu-id="85fbb-224">signing key: any symmetric key for hello `{device id}` identity,</span></span>
* <span data-ttu-id="85fbb-225">Inga Principnamn</span><span class="sxs-lookup"><span data-stu-id="85fbb-225">no policy name,</span></span>
* <span data-ttu-id="85fbb-226">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="85fbb-226">any expiration time.</span></span>

<span data-ttu-id="85fbb-227">Ett exempel som använder hello föregående Node.js-funktion är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-227">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="85fbb-228">hello resultat som beviljar åtkomst tooall funktioner för device1 är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-228">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="85fbb-229">Det är möjligt toogenerate en SAS-token med hjälp av hello .NET [enheten explorer] [ lnk-device-explorer] verktyget eller hello plattformsoberoende, baserat på noden [iothub explorer] [ lnk-iothub-explorer] kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="85fbb-229">It is possible toogenerate a SAS token using hello .NET [device explorer][lnk-device-explorer] tool or hello cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="85fbb-230">Använda en princip för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="85fbb-230">Use a shared access policy</span></span>

<span data-ttu-id="85fbb-231">När du skapar en token från en princip för delad åtkomst, ange hello `skn` fältnamnet toohello hello princip.</span><span class="sxs-lookup"><span data-stu-id="85fbb-231">When you create a token from a shared access policy, set hello `skn` field toohello name of hello policy.</span></span> <span data-ttu-id="85fbb-232">Den här principen måste ge hello **DeviceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-232">This policy must grant hello **DeviceConnect** permission.</span></span>

<span data-ttu-id="85fbb-233">hello två huvudscenarier för att använda funktionen för delad åtkomst principer tooaccess enhet är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-233">hello two main scenarios for using shared access policies tooaccess device functionality are:</span></span>

* <span data-ttu-id="85fbb-234">[molnet protokollet gateways][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="85fbb-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="85fbb-235">[token services] [ lnk-custom-auth] används tooimplement anpassade autentiseringsscheman.</span><span class="sxs-lookup"><span data-stu-id="85fbb-235">[token services][lnk-custom-auth] used tooimplement custom authentication schemes.</span></span>

<span data-ttu-id="85fbb-236">Sedan hello kan princip för delad åtkomst eventuellt ge åtkomst tooconnect som en enhet, det är viktigt toouse hello rätt resurs URI när du skapar säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="85fbb-236">Since hello shared access policy can potentially grant access tooconnect as any device, it is important toouse hello correct resource URI when creating security tokens.</span></span> <span data-ttu-id="85fbb-237">Den här inställningen är särskilt viktigt för token-tjänster med tooscope hello token tooa enhet med hjälp av hello resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="85fbb-237">This setting is especially important for token services, which have tooscope hello token tooa specific device using hello resource URI.</span></span> <span data-ttu-id="85fbb-238">Den här platsen är relevanta för protokollet gateways som de redan fördelar trafik för alla enheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="85fbb-239">Exempelvis en token tjänst med hello som skapats i förväg delad åtkomstprincip som heter **enhet** skulle skapa en token med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="85fbb-239">As an example, a token service using hello pre-created shared access policy called **device** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="85fbb-240">resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="85fbb-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="85fbb-241">nyckel för signeringscertifikatet: en av hello nycklar av hello `device` princip</span><span class="sxs-lookup"><span data-stu-id="85fbb-241">signing key: one of hello keys of hello `device` policy,</span></span>
* <span data-ttu-id="85fbb-242">Principnamn: `device`,</span><span class="sxs-lookup"><span data-stu-id="85fbb-242">policy name: `device`,</span></span>
* <span data-ttu-id="85fbb-243">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="85fbb-243">any expiration time.</span></span>

<span data-ttu-id="85fbb-244">Ett exempel som använder hello föregående Node.js-funktion är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-244">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="85fbb-245">hello resultat som beviljar åtkomst tooall funktioner för device1 är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-245">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="85fbb-246">En protocol-gateway kan använda samma token för alla enheter som bara ange hello resurs-URI för hello`myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="85fbb-246">A protocol gateway could use hello same token for all devices simply setting hello resource URI too`myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="85fbb-247">Använd säkerhetstoken från tjänstkomponenter</span><span class="sxs-lookup"><span data-stu-id="85fbb-247">Use security tokens from service components</span></span>

<span data-ttu-id="85fbb-248">Tjänstens komponenter kan bara generera säkerhetstoken med hjälp av principer för delad åtkomst ger hello lämpliga behörigheter som tidigare förklarats.</span><span class="sxs-lookup"><span data-stu-id="85fbb-248">Service components can only generate security tokens using shared access policies granting hello appropriate permissions as explained previously.</span></span>

<span data-ttu-id="85fbb-249">Här är hello servicefunktioner exponerad på hello slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="85fbb-249">Here is hello service functions exposed on hello endpoints:</span></span>

| <span data-ttu-id="85fbb-250">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="85fbb-250">Endpoint</span></span> | <span data-ttu-id="85fbb-251">Funktioner</span><span class="sxs-lookup"><span data-stu-id="85fbb-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="85fbb-252">Skapa, uppdatera, hämta och ta bort enheten identiteter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="85fbb-253">Ta emot meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="85fbb-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="85fbb-254">Ta emot feedback för moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="85fbb-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="85fbb-255">Skicka meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="85fbb-256">Som exempel, en tjänst som genereras med hjälp av hello som skapats i förväg delad åtkomstprincip som heter **registryRead** skulle skapa en token med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="85fbb-256">As an example, a service generating using hello pre-created shared access policy called **registryRead** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="85fbb-257">resurs-URI: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="85fbb-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="85fbb-258">nyckel för signeringscertifikatet: en av hello nycklar av hello `registryRead` princip</span><span class="sxs-lookup"><span data-stu-id="85fbb-258">signing key: one of hello keys of hello `registryRead` policy,</span></span>
* <span data-ttu-id="85fbb-259">Principnamn: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="85fbb-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="85fbb-260">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="85fbb-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="85fbb-261">hello resultat som beviljar åtkomst tooread identiteter för alla enheter, är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-261">hello result, which would grant access tooread all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="85fbb-262">Stöds X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="85fbb-262">Supported X.509 certificates</span></span>

<span data-ttu-id="85fbb-263">Du kan använda en enhet för tooauthenticate alla X.509-certifikat med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="85fbb-263">You can use any X.509 certificate tooauthenticate a device with IoT Hub.</span></span> <span data-ttu-id="85fbb-264">Certifikat är:</span><span class="sxs-lookup"><span data-stu-id="85fbb-264">Certificates include:</span></span>

* <span data-ttu-id="85fbb-265">**Ett befintligt X.509-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="85fbb-266">En enhet kanske redan har ett X.509-certifikat som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="85fbb-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="85fbb-267">hello enheten kan använda det här certifikatet tooauthenticate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="85fbb-267">hello device can use this certificate tooauthenticate with IoT Hub.</span></span>
* <span data-ttu-id="85fbb-268">**En egen genereras och självsignerade certifikat för X-509**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="85fbb-269">En enhetstillverkaren eller interna deployer kan generera dessa certifikat och lagra hello motsvarande privata nyckel (och certifikat) på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-269">A device manufacturer or in-house deployer can generate these certificates and store hello corresponding private key (and certificate) on hello device.</span></span> <span data-ttu-id="85fbb-270">Du kan använda verktyg som [OpenSSL] [ lnk-openssl] och [Windows SelfSignedCertificate] [ lnk-selfsigned] utility för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="85fbb-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="85fbb-271">**X.509-certifikat som signerats av Certifikatutfärdaren**.</span><span class="sxs-lookup"><span data-stu-id="85fbb-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="85fbb-272">tooidentify en enhet och autentisera med IoT-hubb, kan du använda ett X.509-certifikat som genereras och signerade av en certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="85fbb-272">tooidentify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="85fbb-273">IoT-hubb kontrolleras endast att hello tumavtrycket presenteras matchar hello konfigurerade tumavtrycket.</span><span class="sxs-lookup"><span data-stu-id="85fbb-273">IoT Hub only verifies that hello thumbprint presented matches hello configured thumbprint.</span></span> <span data-ttu-id="85fbb-274">IotHub kan inte valideras hello certifikatkedjan.</span><span class="sxs-lookup"><span data-stu-id="85fbb-274">IotHub does not validate hello certificate chain.</span></span>

<span data-ttu-id="85fbb-275">En enhet kan antingen använda ett X.509-certifikat eller en säkerhetstoken för autentisering, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="85fbb-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="85fbb-276">Registrera ett X.509-certifikat för en enhet</span><span class="sxs-lookup"><span data-stu-id="85fbb-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="85fbb-277">Hej [Azure IoT Service SDK för C#] [ lnk-service-sdk] (version 1.0.8+) har stöd för registrering av en enhet som använder ett X.509-certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="85fbb-277">hello [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="85fbb-278">Andra API: er som import och export av enheter har också stöd för X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="85fbb-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="85fbb-279">C\# stöd</span><span class="sxs-lookup"><span data-stu-id="85fbb-279">C\# Support</span></span>

<span data-ttu-id="85fbb-280">Hej **RegistryManager** klassen innehåller en programmässiga sätt tooregister en enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-280">hello **RegistryManager** class provides a programmatic way tooregister a device.</span></span> <span data-ttu-id="85fbb-281">I synnerhet hello **AddDeviceAsync** och **UpdateDeviceAsync** metoder aktivera tooregister och uppdatera en enhet i hello IoT-hubb identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="85fbb-281">In particular, hello **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you tooregister and update a device in hello IoT Hub identity registry.</span></span> <span data-ttu-id="85fbb-282">Dessa två metoder ta en **enhet** instansen som indata.</span><span class="sxs-lookup"><span data-stu-id="85fbb-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="85fbb-283">Hej **enhet** klassen innehåller en **autentisering** egenskapen som du kan använda toospecify primära och sekundära X.509 tumavtryck för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-283">hello **Device** class includes an **Authentication** property that allows you toospecify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="85fbb-284">hello tumavtrycket representerar en SHA-1-hash för hello X.509-certifikat (som lagras med hjälp av binär kodning DER).</span><span class="sxs-lookup"><span data-stu-id="85fbb-284">hello thumbprint represents a SHA-1 hash of hello X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="85fbb-285">Du har hello möjlighet att ange tumavtryck för en primär eller sekundär tumavtryck eller båda.</span><span class="sxs-lookup"><span data-stu-id="85fbb-285">You have hello option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="85fbb-286">Primära och sekundära tumavtryck är stöds toohandle certifikat förnyelse scenarier.</span><span class="sxs-lookup"><span data-stu-id="85fbb-286">Primary and secondary thumbprints are supported toohandle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="85fbb-287">IoT-hubb inte kräver eller lagra hello hela X.509-certifikat, endast hello tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="85fbb-287">IoT Hub does not require or store hello entire X.509 certificate, only hello thumbprint.</span></span>

<span data-ttu-id="85fbb-288">Här följer ett exempel på C\# code fragment tooregister en enhet med ett X.509-certifikat:</span><span class="sxs-lookup"><span data-stu-id="85fbb-288">Here is a sample C\# code snippet tooregister a device using an X.509 certificate:</span></span>

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="85fbb-289">Använd ett X.509-certifikat under körning</span><span class="sxs-lookup"><span data-stu-id="85fbb-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="85fbb-290">Hej [Azure IoT-enhet SDK för .NET] [ lnk-client-sdk] (version 1.0.11+) kan du använda hello X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="85fbb-290">hello [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports hello use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="85fbb-291">C\# stöd</span><span class="sxs-lookup"><span data-stu-id="85fbb-291">C\# Support</span></span>

<span data-ttu-id="85fbb-292">Hej klassen **DeviceAuthenticationWithX509Certificate** stöder hello skapandet av **DeviceClient** instanser med hjälp av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="85fbb-292">hello class **DeviceAuthenticationWithX509Certificate** supports hello creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="85fbb-293">hello X.509-certifikat måste ha formatet hello PFX (kallas även PKCS #12) som innehåller hello privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="85fbb-293">hello X.509 certificate must be in hello PFX (also called PKCS #12) format that includes hello private key.</span></span>

<span data-ttu-id="85fbb-294">Här är ett kodfragment som exempel:</span><span class="sxs-lookup"><span data-stu-id="85fbb-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="85fbb-295">Anpassad autentisering</span><span class="sxs-lookup"><span data-stu-id="85fbb-295">Custom device authentication</span></span>

<span data-ttu-id="85fbb-296">Du kan använda hello IoT-hubb [identitetsregistret] [ lnk-identity-registry] tooconfigure per enhet säkerhetsreferenser och åtkomst kontroll med hjälp av [token] [ lnk-sas-tokens] .</span><span class="sxs-lookup"><span data-stu-id="85fbb-296">You can use hello IoT Hub [identity registry][lnk-identity-registry] tooconfigure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="85fbb-297">Om det finns redan en anpassad identitet registret och/eller autentisering schemat för en IoT-lösning, kan du överväga att skapa en *token service* toointegrate infrastrukturen med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="85fbb-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* toointegrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="85fbb-298">På så sätt kan använda du andra IoT-funktioner i din lösning.</span><span class="sxs-lookup"><span data-stu-id="85fbb-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="85fbb-299">En token service är en anpassad molntjänst.</span><span class="sxs-lookup"><span data-stu-id="85fbb-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="85fbb-300">Den använder en IoT-hubb *delad åtkomstprincip* med **DeviceConnect** behörigheter toocreate *enheten omfång* token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions toocreate *device-scoped* tokens.</span></span> <span data-ttu-id="85fbb-301">Dessa token kan en enhet tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-301">These tokens enable a device tooconnect tooyour IoT hub.</span></span>

![Steg för hello tokentjänsten mönster][img-tokenservice]

<span data-ttu-id="85fbb-303">Här följer hello Huvudsteg för hello tokentjänsten mönster:</span><span class="sxs-lookup"><span data-stu-id="85fbb-303">Here are hello main steps of hello token service pattern:</span></span>

1. <span data-ttu-id="85fbb-304">Skapa en IoT-hubb delad åtkomstprincip med **DeviceConnect** behörigheter för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="85fbb-305">Du kan skapa den här principen i hello [Azure-portalen] [ lnk-management-portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="85fbb-305">You can create this policy in hello [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="85fbb-306">hello-tokentjänsten använder den här principen toosign hello-token som skapas.</span><span class="sxs-lookup"><span data-stu-id="85fbb-306">hello token service uses this policy toosign hello tokens it creates.</span></span>
1. <span data-ttu-id="85fbb-307">När en enhet måste tooaccess din IoT-hubb, begär en signerade token från token-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="85fbb-307">When a device needs tooaccess your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="85fbb-308">hello enheten kan autentisera med din anpassade identiteten registret/schemat toodetermine hello enheten autentiseringsidentitet att hello tokentjänsten använder toocreate hello-token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-308">hello device can authenticate with your custom identity registry/authentication scheme toodetermine hello device identity that hello token service uses toocreate hello token.</span></span>
1. <span data-ttu-id="85fbb-309">hello-tokentjänsten returnerar en token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-309">hello token service returns a token.</span></span> <span data-ttu-id="85fbb-310">hello token skapas med hjälp av `/devices/{deviceId}` som `resourceURI`, med `deviceId` som hello enhet som autentiseras.</span><span class="sxs-lookup"><span data-stu-id="85fbb-310">hello token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as hello device being authenticated.</span></span> <span data-ttu-id="85fbb-311">hello-tokentjänsten använder hello delad princip tooconstruct hello åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="85fbb-311">hello token service uses hello shared access policy tooconstruct hello token.</span></span>
1. <span data-ttu-id="85fbb-312">hello enhet använder hello token direkt med hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-312">hello device uses hello token directly with hello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="85fbb-313">Du kan använda hello .NET-klass [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] eller hello Java-klass [IotHubServiceSasToken] [ lnk-java-sas] toocreate en token i din token tjänst.</span><span class="sxs-lookup"><span data-stu-id="85fbb-313">You can use hello .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or hello Java class [IotHubServiceSasToken][lnk-java-sas] toocreate a token in your token service.</span></span>

<span data-ttu-id="85fbb-314">hello-tokentjänsten kan ange hello token upphör att gälla efter behov.</span><span class="sxs-lookup"><span data-stu-id="85fbb-314">hello token service can set hello token expiration as desired.</span></span> <span data-ttu-id="85fbb-315">När hello-token upphör att gälla servrarna hello enhetsanslutning hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-315">When hello token expires, hello IoT hub severs hello device connection.</span></span> <span data-ttu-id="85fbb-316">Hello enheten måste sedan begära en ny token från token hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="85fbb-316">Then, hello device must request a new token from hello token service.</span></span> <span data-ttu-id="85fbb-317">En kort förfallotid ökar hello belastningen på både hello enhets- och hello token.</span><span class="sxs-lookup"><span data-stu-id="85fbb-317">A short expiry time increases hello load on both hello device and hello token service.</span></span>

<span data-ttu-id="85fbb-318">För enheten tooconnect tooyour hubb måste du ändå lägga toohello identitetsregistret för IoT-hubb – även om hello enheten använder en token och inte en enhet viktiga tooconnect.</span><span class="sxs-lookup"><span data-stu-id="85fbb-318">For a device tooconnect tooyour hub, you must still add it toohello IoT Hub identity registry — even though hello device is using a token and not a device key tooconnect.</span></span> <span data-ttu-id="85fbb-319">Därför kan du fortsätta toouse per enhet åtkomstkontroll genom att aktivera eller inaktivera enheten identiteter i hello [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="85fbb-319">Therefore, you can continue toouse per-device access control by enabling or disabling device identities in hello [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="85fbb-320">Den här metoden minskar hello riskerna med att använda token med lång upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="85fbb-320">This approach mitigates hello risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="85fbb-321">Jämförelse med en anpassad gateway</span><span class="sxs-lookup"><span data-stu-id="85fbb-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="85fbb-322">hello-tokentjänsten mönstret är hello rekommenderat sätt tooimplement register-/ autentiseringsschema en anpassad identitet med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="85fbb-322">hello token service pattern is hello recommended way tooimplement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="85fbb-323">Det här mönstret rekommenderas eftersom IoT-hubb fortsätter toohandle de flesta av hello lösning trafik.</span><span class="sxs-lookup"><span data-stu-id="85fbb-323">This pattern is recommended because IoT Hub continues toohandle most of hello solution traffic.</span></span> <span data-ttu-id="85fbb-324">Om hello anpassade autentiseringsschemat så sammanflätade med hello-protokollet, du kan dock kräva en *anpassade gateway* tooprocess alla hello trafik.</span><span class="sxs-lookup"><span data-stu-id="85fbb-324">However, if hello custom authentication scheme is so intertwined with hello protocol, you may require a *custom gateway* tooprocess all hello traffic.</span></span> <span data-ttu-id="85fbb-325">Ett exempel på en sådan situation använder[Transport Layer Security (TLS) och i förväg delade nycklar (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="85fbb-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="85fbb-326">Mer information finns i hello [protokollgatewayen] [ lnk-protocols] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-326">For more information, see hello [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="85fbb-327">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="85fbb-327">Reference topics:</span></span>

<span data-ttu-id="85fbb-328">hello ger följande Referensinformation dig mer om styra åtkomst tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-328">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="85fbb-329">Behörigheter för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="85fbb-329">IoT Hub permissions</span></span>

<span data-ttu-id="85fbb-330">hello visar följande tabell hello behörigheter som du kan använda toocontrol åtkomst tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-330">hello following table lists hello permissions you can use toocontrol access tooyour IoT hub.</span></span>

| <span data-ttu-id="85fbb-331">Behörighet</span><span class="sxs-lookup"><span data-stu-id="85fbb-331">Permission</span></span> | <span data-ttu-id="85fbb-332">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="85fbb-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="85fbb-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="85fbb-333">**RegistryRead**</span></span> |<span data-ttu-id="85fbb-334">Ger läsbehörighet toohello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="85fbb-334">Grants read access toohello identity registry.</span></span> <span data-ttu-id="85fbb-335">Mer information finns i [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="85fbb-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="85fbb-336">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="85fbb-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="85fbb-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="85fbb-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="85fbb-338">Ger Läs- och skrivbehörighet toohello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="85fbb-338">Grants read and write access toohello identity registry.</span></span> <span data-ttu-id="85fbb-339">Mer information finns i [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="85fbb-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="85fbb-340">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="85fbb-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="85fbb-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="85fbb-341">**ServiceConnect**</span></span> |<span data-ttu-id="85fbb-342">Ger åtkomst till toocloud service-riktade kommunikation och övervakning av slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-342">Grants access toocloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="85fbb-343">Beviljar behörighet tooreceive enhet till moln meddelanden, skicka meddelanden moln till enhet och hämta hello motsvarande leverans bekräftelser.</span><span class="sxs-lookup"><span data-stu-id="85fbb-343">Grants permission tooreceive device-to-cloud messages, send cloud-to-device messages, and retrieve hello corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="85fbb-344">Beviljar behörighet tooretrieve leverans bekräftelser för filen filöverföringar.</span><span class="sxs-lookup"><span data-stu-id="85fbb-344">Grants permission tooretrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="85fbb-345">Beviljar behörighet tooaccess enhet twins tooupdate taggar och önskade egenskaper hämta rapporterade egenskaper och köra frågor.</span><span class="sxs-lookup"><span data-stu-id="85fbb-345">Grants permission tooaccess device twins tooupdate tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="85fbb-346">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="85fbb-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="85fbb-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="85fbb-347">**DeviceConnect**</span></span> |<span data-ttu-id="85fbb-348">Ger åtkomst till toodevice riktade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-348">Grants access toodevice-facing endpoints.</span></span> <br/><span data-ttu-id="85fbb-349">Beviljar behörighet toosend enhet till moln meddelanden och ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-349">Grants permission toosend device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="85fbb-350">Beviljar behörighet tooperform Filöverföring från en enhet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-350">Grants permission tooperform file upload from a device.</span></span> <br/><span data-ttu-id="85fbb-351">Beviljar behörighet tooreceive enheten dubbla önskad egenskapen meddelanden och uppdatera enheten dubbla rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="85fbb-351">Grants permission tooreceive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="85fbb-352">Beviljar filöverföringar behörighet tooperform.</span><span class="sxs-lookup"><span data-stu-id="85fbb-352">Grants permission tooperform file uploads.</span></span> <br/><span data-ttu-id="85fbb-353">Den här behörigheten används av enheter.</span><span class="sxs-lookup"><span data-stu-id="85fbb-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="85fbb-354">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="85fbb-354">Additional reference material</span></span>

<span data-ttu-id="85fbb-355">Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="85fbb-355">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="85fbb-356">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="85fbb-356">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="85fbb-357">[Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter och begränsning beteenden som tillämpas toohello IoT-hubb-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="85fbb-357">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="85fbb-358">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="85fbb-358">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="85fbb-359">[IoT-hubb frågespråket] [ lnk-query] beskriver hello frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="85fbb-359">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="85fbb-360">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.</span><span class="sxs-lookup"><span data-stu-id="85fbb-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85fbb-361">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85fbb-361">Next steps</span></span>

<span data-ttu-id="85fbb-362">Nu har du fått veta hur toocontrol åt IoT-hubb, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:</span><span class="sxs-lookup"><span data-stu-id="85fbb-362">Now you have learned how toocontrol access IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="85fbb-363">[Använda enhetstillstånd twins toosynchronize och konfigurationer][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="85fbb-363">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="85fbb-364">[Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="85fbb-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="85fbb-365">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="85fbb-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="85fbb-366">Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb Självstudier:</span><span class="sxs-lookup"><span data-stu-id="85fbb-366">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="85fbb-367">[Kom igång med Azure IoT-hubb][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="85fbb-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="85fbb-368">[Hur toosend moln till enhet meddelanden med IoT-hubb][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="85fbb-368">[How toosend cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="85fbb-369">[Hur tooprocess IoT-hubb meddelanden från enhet till moln][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="85fbb-369">[How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
