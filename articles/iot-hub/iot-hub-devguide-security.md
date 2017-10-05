---
title: "Förstå Azure IoT Hub säkerheten | Microsoft Docs"
description: "Utvecklare guide - hur du styr åtkomst till IoT-hubb för appar för enheter och backend-appar. Innehåller information om säkerhetstoken och stöd för X.509-certifikat."
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
ms.openlocfilehash: e4fe5400ffcf4446392015aada031dd4dfbf238a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="control-access-to-iot-hub"></a><span data-ttu-id="18dfd-104">Styra åtkomst till IoT Hub</span><span class="sxs-lookup"><span data-stu-id="18dfd-104">Control access to IoT Hub</span></span>

<span data-ttu-id="18dfd-105">Den här artikeln beskrivs alternativ för att skydda din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-105">This article describes the options for securing your IoT hub.</span></span> <span data-ttu-id="18dfd-106">IoT-hubb använder *behörigheter* att bevilja åtkomst till varje IoT hub-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="18dfd-106">IoT Hub uses *permissions* to grant access to each IoT hub endpoint.</span></span> <span data-ttu-id="18dfd-107">Behörigheter som begränsar åtkomsten till en IoT-hubb som baseras på funktionen.</span><span class="sxs-lookup"><span data-stu-id="18dfd-107">Permissions limit the access to an IoT hub based on functionality.</span></span>

<span data-ttu-id="18dfd-108">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="18dfd-108">This article describes:</span></span>

* <span data-ttu-id="18dfd-109">De olika behörigheterna som du kan bevilja en enhet eller backend-app åtkomst till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-109">The different permissions that you can grant to a device or back-end app to access your IoT hub.</span></span>
* <span data-ttu-id="18dfd-110">Autentiseringen och token som används för att verifiera behörigheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-110">The authentication process and the tokens it uses to verify permissions.</span></span>
* <span data-ttu-id="18dfd-111">Så här scope autentiseringsuppgifter för att begränsa åtkomsten till specifika resurser.</span><span class="sxs-lookup"><span data-stu-id="18dfd-111">How to scope credentials to limit access to specific resources.</span></span>
* <span data-ttu-id="18dfd-112">IoT-hubb stöd för X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="18dfd-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="18dfd-113">Anpassade autentiseringsmekanismer som använder befintliga enhetens identitet register eller autentiseringsscheman.</span><span class="sxs-lookup"><span data-stu-id="18dfd-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="18dfd-114">När du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="18dfd-114">When to use</span></span>

<span data-ttu-id="18dfd-115">Du måste ha behörighet att komma åt IoT-hubb-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-115">You must have appropriate permissions to access any of the IoT Hub endpoints.</span></span> <span data-ttu-id="18dfd-116">Till exempel måste en enhet innehålla en token som innehåller säkerhetsreferenser tillsammans med alla meddelanden som skickas till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-116">For example, a device must include a token containing security credentials along with every message it sends to IoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="18dfd-117">Åtkomstkontroll och behörigheter</span><span class="sxs-lookup"><span data-stu-id="18dfd-117">Access control and permissions</span></span>

<span data-ttu-id="18dfd-118">Du kan ge [behörigheter](#iot-hub-permissions) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="18dfd-118">You can grant [permissions](#iot-hub-permissions) in the following ways:</span></span>

* <span data-ttu-id="18dfd-119">**IoT-hubb-nivå delade åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="18dfd-120">Principer för delad åtkomst kan ge en kombination av [behörigheter](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="18dfd-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="18dfd-121">Du kan definiera principer i den [Azure-portalen][lnk-management-portal], eller programmässigt med hjälp av den [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="18dfd-121">You can define policies in the [Azure portal][lnk-management-portal], or programmatically by using the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="18dfd-122">En nyligen skapade IoT-hubben har standardprinciperna för följande:</span><span class="sxs-lookup"><span data-stu-id="18dfd-122">A newly created IoT hub has the following default policies:</span></span>

  * <span data-ttu-id="18dfd-123">**iothubowner**: princip med alla behörigheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="18dfd-124">**tjänsten**: princip med **ServiceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="18dfd-125">**enheten**: princip med **DeviceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="18dfd-126">**registryRead**: princip med **RegistryRead** behörighet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="18dfd-127">**registryReadWrite**: princip med **RegistryRead** och RegistryWrite behörigheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="18dfd-128">**Per enhet säkerhetsreferenser**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-128">**Per-device security credentials**.</span></span> <span data-ttu-id="18dfd-129">Varje IoT-hubb innehåller en [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="18dfd-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="18dfd-130">Du kan konfigurera säkerhetsreferenser som ger för varje enhet i den här identitetsregistret **DeviceConnect** behörigheter som är begränsade till motsvarande enhet slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped to the corresponding device endpoints.</span></span>

<span data-ttu-id="18dfd-131">Till exempel i en typisk IoT-lösningen:</span><span class="sxs-lookup"><span data-stu-id="18dfd-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="18dfd-132">Komponenten device management använder den *registryReadWrite* princip.</span><span class="sxs-lookup"><span data-stu-id="18dfd-132">The device management component uses the *registryReadWrite* policy.</span></span>
* <span data-ttu-id="18dfd-133">Processorkomponent händelse använder den *service* princip.</span><span class="sxs-lookup"><span data-stu-id="18dfd-133">The event processor component uses the *service* policy.</span></span>
* <span data-ttu-id="18dfd-134">Komponenten körning enheten business logic använder den *service* princip.</span><span class="sxs-lookup"><span data-stu-id="18dfd-134">The run-time device business logic component uses the *service* policy.</span></span>
* <span data-ttu-id="18dfd-135">Enskilda enheter ansluter med autentiseringsuppgifter som lagras i IoT-hubben identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-135">Individual devices connect using credentials stored in the IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="18dfd-136">Se [behörigheter](#iot-hub-permissions) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="18dfd-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="18dfd-137">Autentisering</span><span class="sxs-lookup"><span data-stu-id="18dfd-137">Authentication</span></span>

<span data-ttu-id="18dfd-138">Azure IoT-hubb ger åtkomst till slutpunkter genom att verifiera en token mot principer för delad åtkomst och identitet säkerhetsreferenser för registret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-138">Azure IoT Hub grants access to endpoints by verifying a token against the shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="18dfd-139">Säkerhetsreferenser, till exempel symmetriska nycklar skickas aldrig via kabel.</span><span class="sxs-lookup"><span data-stu-id="18dfd-139">Security credentials, such as symmetric keys, are never sent over the wire.</span></span>

> [!NOTE]
> <span data-ttu-id="18dfd-140">Azure IoT Hub-resursprovidern skyddas via Azure-prenumerationen, eftersom alla providrar på den [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="18dfd-140">The Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in the [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="18dfd-141">Mer information om hur du skapar och använder säkerhetstoken finns [IoT-hubb säkerhetstoken][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="18dfd-141">For more information about how to construct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="18dfd-142">Protokollet programvarukrav</span><span class="sxs-lookup"><span data-stu-id="18dfd-142">Protocol specifics</span></span>

<span data-ttu-id="18dfd-143">Varje protokoll som stöds, till exempel MQTT, AMQP och HTTP, transport token på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="18dfd-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="18dfd-144">När du använder MQTT CONNECT-paketet har deviceId som ClientId, {iothubhostname} / {deviceId} i fältet för användarnamn och en SAS-token i fältet lösenord.</span><span class="sxs-lookup"><span data-stu-id="18dfd-144">When using MQTT, the CONNECT packet has the deviceId as the ClientId, {iothubhostname}/{deviceId} in the Username field, and a SAS token in the Password field.</span></span> <span data-ttu-id="18dfd-145">{iothubhostname} ska vara fullständig CName för IoT-hubb (till exempel contoso.azure-devices.net).</span><span class="sxs-lookup"><span data-stu-id="18dfd-145">{iothubhostname} should be the full CName of the IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="18dfd-146">När du använder [AMQP][lnk-amqp], IoT-hubb stöder [SASL OFORMATERAD] [ lnk-sasl-plain] och [AMQP anspråk baserade-säkerhet] [ lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="18dfd-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="18dfd-147">Om du använder AMQP anspråk baserade-säkerhet anger standarden så att överföra dessa token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-147">If you use AMQP claims-based-security, the standard specifies how to transmit these tokens.</span></span>

<span data-ttu-id="18dfd-148">För SASL OFORMATERAD den **användarnamn** kan vara:</span><span class="sxs-lookup"><span data-stu-id="18dfd-148">For SASL PLAIN, the **username** can be:</span></span>

* <span data-ttu-id="18dfd-149">`{policyName}@sas.root.{iothubName}`Om du använder token för IoT-hubb-nivå.</span><span class="sxs-lookup"><span data-stu-id="18dfd-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="18dfd-150">`{deviceId}@sas.{iothubname}`Om du använder enheten omfång token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="18dfd-151">I båda fallen lösenordsfältet innehåller token som beskrivs i [IoT-hubb säkerhetstoken][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="18dfd-151">In both cases, the password field contains the token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="18dfd-152">HTTP implementerar autentisering genom att inkludera en giltig token i den **auktorisering** huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="18dfd-152">HTTP implements authentication by including a valid token in the **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="18dfd-153">Exempel</span><span class="sxs-lookup"><span data-stu-id="18dfd-153">Example</span></span>

<span data-ttu-id="18dfd-154">Användarnamn (DeviceId är skiftlägeskänsligt):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="18dfd-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="18dfd-155">Lösenord (generera SAS-token med den [enheten explorer] [ lnk-device-explorer] verktyget):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="18dfd-155">Password (Generate SAS token with the [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="18dfd-156">Den [Azure IoT SDK] [ lnk-sdks] automatiskt generera token när du ansluter till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-156">The [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting to the service.</span></span> <span data-ttu-id="18dfd-157">I vissa fall stöder inte Azure IoT-SDK: er alla protokoll eller alla autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="18dfd-157">In some cases, the Azure IoT SDKs do not support all the protocols or all the authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="18dfd-158">Att tänka på SASL OFORMATERAD</span><span class="sxs-lookup"><span data-stu-id="18dfd-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="18dfd-159">När du använder SASL OFORMATERAD med AMQP, kan en klient som ansluter till en IoT-hubb använda en enda token för varje TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="18dfd-159">When using SASL PLAIN with AMQP, a client connecting to an IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="18dfd-160">När token upphör att gälla TCP-anslutningen kopplas från tjänsten och utlöser en återanslutning.</span><span class="sxs-lookup"><span data-stu-id="18dfd-160">When the token expires, the TCP connection disconnects from the service and triggers a reconnect.</span></span> <span data-ttu-id="18dfd-161">Det här beteendet skadas när det inte problematisk för en backend-app för en enhetsapp av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="18dfd-161">This behavior, while not problematic for a back-end app, is damaging for a device app for the following reasons:</span></span>

* <span data-ttu-id="18dfd-162">Gateway ansluter vanligtvis för många enheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="18dfd-163">När du använder SASL OFORMATERAD behöva de skapa en distinkt TCP-anslutning för varje enhet som ansluter till en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-163">When using SASL PLAIN, they have to create a distinct TCP connection for each device connecting to an IoT hub.</span></span> <span data-ttu-id="18dfd-164">Det här scenariot avsevärt ökar förbrukningen av ström och nätverksresurser och ökar svarstiden för varje enhetsanslutning.</span><span class="sxs-lookup"><span data-stu-id="18dfd-164">This scenario considerably increases the consumption of power and networking resources, and increases the latency of each device connection.</span></span>
* <span data-ttu-id="18dfd-165">Begränsad resurs enheter påverkas negativt av ökad användning av resurser för att återansluta efter varje token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="18dfd-165">Resource-constrained devices are adversely affected by the increased use of resources to reconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="18dfd-166">Autentiseringsuppgifter för scope IoT hub-nivå</span><span class="sxs-lookup"><span data-stu-id="18dfd-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="18dfd-167">Du kan definiera säkerhetsprinciper för IoT-hubb-nivå genom att skapa token med en skyddad resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="18dfd-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="18dfd-168">Slutpunkten för att skicka meddelanden från enhet till moln från en enhet är till exempel **/devices/ {deviceId} / meddelanden/händelser**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-168">For example, the endpoint to send device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="18dfd-169">Du kan också använda en IoT-hubb-nivå delad åtkomstprincip med **DeviceConnect** behörighet för att signera en token vars resourceURI är **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions to sign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="18dfd-170">Den här metoden skapar en token som endast kan användas för att skicka meddelanden för enheten **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-170">This approach creates a token that is only usable to send messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="18dfd-171">Den här mekanismen liknar den [Händelsehubbar utgivarprincip][lnk-event-hubs-publisher-policy], och du kan implementera anpassade autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="18dfd-171">This mechanism is similar to the [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you to implement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="18dfd-172">Säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="18dfd-172">Security tokens</span></span>

<span data-ttu-id="18dfd-173">IoT-hubb använder säkerhetstoken för att autentisera enheter och tjänster för att undvika att skicka nycklar i uppkopplat läge.</span><span class="sxs-lookup"><span data-stu-id="18dfd-173">IoT Hub uses security tokens to authenticate devices and services to avoid sending keys on the wire.</span></span> <span data-ttu-id="18dfd-174">Dessutom är säkerhetstoken begränsad giltighetstid och omfång.</span><span class="sxs-lookup"><span data-stu-id="18dfd-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="18dfd-175">[Azure IoT-SDK] [ lnk-sdks] automatiskt generera token utan någon specialkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="18dfd-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="18dfd-176">Vissa scenarier behöver du skapa och använda säkerhetstoken direkt.</span><span class="sxs-lookup"><span data-stu-id="18dfd-176">Some scenarios do require you to generate and use security tokens directly.</span></span> <span data-ttu-id="18dfd-177">Sådana scenarier är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-177">Such scenarios include:</span></span>

* <span data-ttu-id="18dfd-178">Direkt användning av de MQTT, AMQP och HTTP.</span><span class="sxs-lookup"><span data-stu-id="18dfd-178">The direct use of the MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="18dfd-179">Implementeringen av tokentjänsten mönster, enligt beskrivningen i [anpassad enhetsautentisering][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="18dfd-179">The implementation of the token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="18dfd-180">IoT-hubb kan också enheter att autentisera med IoT-hubb med [X.509-certifikat][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="18dfd-180">IoT Hub also allows devices to authenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="18dfd-181">Säkerhet token struktur</span><span class="sxs-lookup"><span data-stu-id="18dfd-181">Security token structure</span></span>

<span data-ttu-id="18dfd-182">Du kan använda säkerhetstoken för att ge tid begränsas åtkomsten till enheter och tjänster till specifika funktioner i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-182">You use security tokens to grant time-bounded access to devices and services to specific functionality in IoT Hub.</span></span> <span data-ttu-id="18dfd-183">För att få behörighet att ansluta till IoT-hubb, skickar enheter och tjänster säkerhetstoken som signerade med en delad åtkomst eller en symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="18dfd-183">To get authorization to connect to IoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="18dfd-184">Dessa nycklar lagras med en enhetsidentitet i identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-184">These keys are stored with a device identity in the identity registry.</span></span>

<span data-ttu-id="18dfd-185">En token som signerats med en delad åtkomst viktiga beviljar åtkomst till alla funktioner som är associerade med principen-behörigheter för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="18dfd-185">A token signed with a shared access key grants access to all the functionality associated with the shared access policy permissions.</span></span> <span data-ttu-id="18dfd-186">En token som signerats med en enhetsidentitet symmetrisk nyckel endast ger det **DeviceConnect** för associerade enhetens identitet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-186">A token signed with a device identity's symmetric key only grants the **DeviceConnect** permission for the associated device identity.</span></span>

<span data-ttu-id="18dfd-187">Säkerhetstoken har följande format:</span><span class="sxs-lookup"><span data-stu-id="18dfd-187">The security token has the following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="18dfd-188">Här är de förväntade värdena:</span><span class="sxs-lookup"><span data-stu-id="18dfd-188">Here are the expected values:</span></span>

| <span data-ttu-id="18dfd-189">Värde</span><span class="sxs-lookup"><span data-stu-id="18dfd-189">Value</span></span> | <span data-ttu-id="18dfd-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="18dfd-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="18dfd-191">{signatur}</span><span class="sxs-lookup"><span data-stu-id="18dfd-191">{signature}</span></span> |<span data-ttu-id="18dfd-192">En HMAC SHA256 signatur sträng med formatet: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="18dfd-192">An HMAC-SHA256 signature string of the form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="18dfd-193">**Viktiga**: nyckeln avkoda från base64 och används som nyckel för att utföra HMAC SHA256-beräkningen.</span><span class="sxs-lookup"><span data-stu-id="18dfd-193">**Important**: The key is decoded from base64 and used as key to perform the HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="18dfd-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="18dfd-194">{resourceURI}</span></span> |<span data-ttu-id="18dfd-195">URI-prefix (av segment) slutpunkter som kan användas med denna token som börjar med värdnamnet för IoT-hubb (ingen protocol).</span><span class="sxs-lookup"><span data-stu-id="18dfd-195">URI prefix (by segment) of the endpoints that can be accessed with this token, starting with host name of the IoT hub (no protocol).</span></span> <span data-ttu-id="18dfd-196">Till exempel, `myHub.azure-devices.net/devices/device1`</span><span class="sxs-lookup"><span data-stu-id="18dfd-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="18dfd-197">{utgången}</span><span class="sxs-lookup"><span data-stu-id="18dfd-197">{expiry}</span></span> |<span data-ttu-id="18dfd-198">UTF8 strängar för antal sekunder sedan epok 00:00:00 UTC på 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="18dfd-198">UTF8 strings for number of seconds since the epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="18dfd-199">{URL-kodade-resourceURI}</span><span class="sxs-lookup"><span data-stu-id="18dfd-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="18dfd-200">Lägre fall URL-kodning av gemen resurs-URI</span><span class="sxs-lookup"><span data-stu-id="18dfd-200">Lower case URL-encoding of the lower case resource URI</span></span> |
| <span data-ttu-id="18dfd-201">{policyName}</span><span class="sxs-lookup"><span data-stu-id="18dfd-201">{policyName}</span></span> |<span data-ttu-id="18dfd-202">Namnet på den princip för delad åtkomst som den här variabeln refererar.</span><span class="sxs-lookup"><span data-stu-id="18dfd-202">The name of the shared access policy to which this token refers.</span></span> <span data-ttu-id="18dfd-203">Inte fram om avser token enhetsregistret autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-203">Absent if the token refers to device-registry credentials.</span></span> |

<span data-ttu-id="18dfd-204">**Observera på prefixet**: URI: N prefix beräknas av segment och inte av tecken.</span><span class="sxs-lookup"><span data-stu-id="18dfd-204">**Note on prefix**: The URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="18dfd-205">Till exempel `/a/b` är ett prefix för `/a/b/c` men inte för `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="18dfd-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="18dfd-206">Node.js följande utdrag visar en funktion kallad **generateSasToken** som beräknar token från indata `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="18dfd-206">The following Node.js snippet shows a function called **generateSasToken** that computes the token from the inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="18dfd-207">I nästa avsnitt innehåller information om hur du initierar olika indata för olika token användningsfall.</span><span class="sxs-lookup"><span data-stu-id="18dfd-207">The next sections detail how to initialize the different inputs for the different token use cases.</span></span>

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

<span data-ttu-id="18dfd-208">Som en jämförelse är likvärdiga Python-kod för att generera en säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="18dfd-208">As a comparison, the equivalent Python code to generate a security token is:</span></span>

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
> <span data-ttu-id="18dfd-209">Eftersom tokens giltighet verifieras på IoT-hubb datorer måste drift på klockan på den dator som genererar token vara minimal.</span><span class="sxs-lookup"><span data-stu-id="18dfd-209">Since the time validity of the token is validated on IoT Hub machines, the drift on the clock of the machine that generates the token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="18dfd-210">Använd SAS-token i en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="18dfd-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="18dfd-211">Det finns två sätt att hämta **DeviceConnect** behörigheter med IoT-hubb med säkerhetstoken: använda en [symmetriska enhetsnyckel från identitetsregistret](#use-a-symmetric-key-in-the-identity-registry), eller Använd en [delade åtkomstnyckeln](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="18dfd-211">There are two ways to obtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from the identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="18dfd-212">Kom ihåg att alla funktioner som är tillgängliga från enheter exponeras avsiktligt på slutpunkter med prefixet `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="18dfd-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18dfd-213">Det enda sättet att IoT-hubb autentiserar en enhet använder enheten identitet symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="18dfd-213">The only way that IoT Hub authenticates a specific device is using the device identity symmetric key.</span></span> <span data-ttu-id="18dfd-214">I fall när en princip för delad åtkomst för att komma åt enheten funktioner lösningen måste ta hänsyn till den komponent som utfärdar säkerhetstoken som en betrodd underkomponenten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-214">In cases when a shared access policy is used to access device functionality, the solution must consider the component issuing the security token as a trusted subcomponent.</span></span>

<span data-ttu-id="18dfd-215">Enheten riktade slutpunkter är (oavsett protocol):</span><span class="sxs-lookup"><span data-stu-id="18dfd-215">The device-facing endpoints are (irrespective of the protocol):</span></span>

| <span data-ttu-id="18dfd-216">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="18dfd-216">Endpoint</span></span> | <span data-ttu-id="18dfd-217">Funktioner</span><span class="sxs-lookup"><span data-stu-id="18dfd-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="18dfd-218">Skicka meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="18dfd-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="18dfd-219">Ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a><span data-ttu-id="18dfd-220">Använd en symmetrisk nyckel i identitetsregistret</span><span class="sxs-lookup"><span data-stu-id="18dfd-220">Use a symmetric key in the identity registry</span></span>

<span data-ttu-id="18dfd-221">När du använder en enhetsidentitet symmetrisk nyckel för att generera en token av principnamn (`skn`) elementet för token har utelämnats.</span><span class="sxs-lookup"><span data-stu-id="18dfd-221">When using a device identity's symmetric key to generate a token, the policyName (`skn`) element of the token is omitted.</span></span>

<span data-ttu-id="18dfd-222">Till exempel måste en token som skapats för att få åtkomst till alla funktioner i enheten ha följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="18dfd-222">For example, a token created to access all device functionality should have the following parameters:</span></span>

* <span data-ttu-id="18dfd-223">resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="18dfd-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="18dfd-224">nyckel för signeringscertifikatet: en symmetrisk nyckel för den `{device id}` identitet,</span><span class="sxs-lookup"><span data-stu-id="18dfd-224">signing key: any symmetric key for the `{device id}` identity,</span></span>
* <span data-ttu-id="18dfd-225">Inga Principnamn</span><span class="sxs-lookup"><span data-stu-id="18dfd-225">no policy name,</span></span>
* <span data-ttu-id="18dfd-226">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="18dfd-226">any expiration time.</span></span>

<span data-ttu-id="18dfd-227">Ett exempel med hjälp av funktionen föregående Node.js är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-227">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="18dfd-228">Det resultat som ger åtkomst till alla funktioner för device1 skulle vara:</span><span class="sxs-lookup"><span data-stu-id="18dfd-228">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="18dfd-229">Det är möjligt att generera en SAS-token med hjälp av .NET [enheten explorer] [ lnk-device-explorer] verktyget eller på flera plattformar, baserat på noden [iothub explorer] [ lnk-iothub-explorer]kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="18dfd-229">It is possible to generate a SAS token using the .NET [device explorer][lnk-device-explorer] tool or the cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="18dfd-230">Använda en princip för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="18dfd-230">Use a shared access policy</span></span>

<span data-ttu-id="18dfd-231">När du skapar en token från en princip för delad åtkomst i `skn` till namnet på principen.</span><span class="sxs-lookup"><span data-stu-id="18dfd-231">When you create a token from a shared access policy, set the `skn` field to the name of the policy.</span></span> <span data-ttu-id="18dfd-232">Den här principen måste ge den **DeviceConnect** behörighet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-232">This policy must grant the **DeviceConnect** permission.</span></span>

<span data-ttu-id="18dfd-233">De två huvudsakliga för att använda principer för delad åtkomst för åtkomst till enheten funktionerna är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-233">The two main scenarios for using shared access policies to access device functionality are:</span></span>

* <span data-ttu-id="18dfd-234">[molnet protokollet gateways][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="18dfd-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="18dfd-235">[token services] [ lnk-custom-auth] används för att implementera anpassade autentiseringsscheman.</span><span class="sxs-lookup"><span data-stu-id="18dfd-235">[token services][lnk-custom-auth] used to implement custom authentication schemes.</span></span>

<span data-ttu-id="18dfd-236">Eftersom princip för delad åtkomst kan potentiellt bevilja åtkomst till Anslut som alla enheter, är det viktigt att du använder rätt resurs-URI när du skapar säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="18dfd-236">Since the shared access policy can potentially grant access to connect as any device, it is important to use the correct resource URI when creating security tokens.</span></span> <span data-ttu-id="18dfd-237">Den här inställningen är särskilt viktigt för token-tjänster med att definiera omfattningen av token till en specifik enhet med hjälp av resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="18dfd-237">This setting is especially important for token services, which have to scope the token to a specific device using the resource URI.</span></span> <span data-ttu-id="18dfd-238">Den här platsen är relevanta för protokollet gateways som de redan fördelar trafik för alla enheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="18dfd-239">Exempelvis en token tjänst med det skapats i förväg delad åtkomstprincip som heter **enhet** skulle skapa en token med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="18dfd-239">As an example, a token service using the pre-created shared access policy called **device** would create a token with the following parameters:</span></span>

* <span data-ttu-id="18dfd-240">resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="18dfd-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="18dfd-241">nyckel för signeringscertifikatet: en av nycklarna för den `device` principen</span><span class="sxs-lookup"><span data-stu-id="18dfd-241">signing key: one of the keys of the `device` policy,</span></span>
* <span data-ttu-id="18dfd-242">Principnamn: `device`,</span><span class="sxs-lookup"><span data-stu-id="18dfd-242">policy name: `device`,</span></span>
* <span data-ttu-id="18dfd-243">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="18dfd-243">any expiration time.</span></span>

<span data-ttu-id="18dfd-244">Ett exempel med hjälp av funktionen föregående Node.js är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-244">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="18dfd-245">Det resultat som ger åtkomst till alla funktioner för device1 skulle vara:</span><span class="sxs-lookup"><span data-stu-id="18dfd-245">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="18dfd-246">En protocol-gateway kan använda samma token för alla enheter som bara ange resurs-URI för `myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="18dfd-246">A protocol gateway could use the same token for all devices simply setting the resource URI to `myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="18dfd-247">Använd säkerhetstoken från tjänstkomponenter</span><span class="sxs-lookup"><span data-stu-id="18dfd-247">Use security tokens from service components</span></span>

<span data-ttu-id="18dfd-248">Tjänstens komponenter kan bara generera säkerhetstoken med hjälp av principer för delad åtkomst som beviljar behörighet som tidigare förklarats.</span><span class="sxs-lookup"><span data-stu-id="18dfd-248">Service components can only generate security tokens using shared access policies granting the appropriate permissions as explained previously.</span></span>

<span data-ttu-id="18dfd-249">Här är servicefunktioner exponerad på slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="18dfd-249">Here is the service functions exposed on the endpoints:</span></span>

| <span data-ttu-id="18dfd-250">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="18dfd-250">Endpoint</span></span> | <span data-ttu-id="18dfd-251">Funktioner</span><span class="sxs-lookup"><span data-stu-id="18dfd-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="18dfd-252">Skapa, uppdatera, hämta och ta bort enheten identiteter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="18dfd-253">Ta emot meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="18dfd-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="18dfd-254">Ta emot feedback för moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="18dfd-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="18dfd-255">Skicka meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="18dfd-256">Som exempel, en tjänst som genereras med hjälp av i förväg skapade delad åtkomstprincip som heter **registryRead** skulle skapa en token med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="18dfd-256">As an example, a service generating using the pre-created shared access policy called **registryRead** would create a token with the following parameters:</span></span>

* <span data-ttu-id="18dfd-257">resurs-URI: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="18dfd-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="18dfd-258">nyckel för signeringscertifikatet: en av nycklarna för den `registryRead` principen</span><span class="sxs-lookup"><span data-stu-id="18dfd-258">signing key: one of the keys of the `registryRead` policy,</span></span>
* <span data-ttu-id="18dfd-259">Principnamn: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="18dfd-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="18dfd-260">helst upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="18dfd-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="18dfd-261">Resultatet som skulle ge åtkomst för att läsa alla enheten identiteter, är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-261">The result, which would grant access to read all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="18dfd-262">Stöds X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="18dfd-262">Supported X.509 certificates</span></span>

<span data-ttu-id="18dfd-263">Du kan använda alla X.509-certifikat för autentisering av en enhet med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-263">You can use any X.509 certificate to authenticate a device with IoT Hub.</span></span> <span data-ttu-id="18dfd-264">Certifikat är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-264">Certificates include:</span></span>

* <span data-ttu-id="18dfd-265">**Ett befintligt X.509-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="18dfd-266">En enhet kanske redan har ett X.509-certifikat som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="18dfd-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="18dfd-267">Enheten kan använda certifikatet för att autentisera med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-267">The device can use this certificate to authenticate with IoT Hub.</span></span>
* <span data-ttu-id="18dfd-268">**En egen genereras och självsignerade certifikat för X-509**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="18dfd-269">En enhetstillverkaren eller interna deployer kan generera dessa certifikat och lagra motsvarande privata nyckel (och certifikat) på enheten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-269">A device manufacturer or in-house deployer can generate these certificates and store the corresponding private key (and certificate) on the device.</span></span> <span data-ttu-id="18dfd-270">Du kan använda verktyg som [OpenSSL] [ lnk-openssl] och [Windows SelfSignedCertificate] [ lnk-selfsigned] utility för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="18dfd-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="18dfd-271">**X.509-certifikat som signerats av Certifikatutfärdaren**.</span><span class="sxs-lookup"><span data-stu-id="18dfd-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="18dfd-272">För att identifiera en enhet och autentisera med IoT-hubb, kan du använda ett X.509-certifikat som genereras och signerade av en certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="18dfd-272">To identify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="18dfd-273">IoT-hubb verifierar endast att tumavtrycket visas överensstämmer med det konfigurerade tumavtrycket.</span><span class="sxs-lookup"><span data-stu-id="18dfd-273">IoT Hub only verifies that the thumbprint presented matches the configured thumbprint.</span></span> <span data-ttu-id="18dfd-274">IotHub kan inte valideras certifikatkedjan.</span><span class="sxs-lookup"><span data-stu-id="18dfd-274">IotHub does not validate the certificate chain.</span></span>

<span data-ttu-id="18dfd-275">En enhet kan antingen använda ett X.509-certifikat eller en säkerhetstoken för autentisering, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="18dfd-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="18dfd-276">Registrera ett X.509-certifikat för en enhet</span><span class="sxs-lookup"><span data-stu-id="18dfd-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="18dfd-277">Den [Azure IoT Service SDK för C#] [ lnk-service-sdk] (version 1.0.8+) har stöd för registrering av en enhet som använder ett X.509-certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="18dfd-277">The [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="18dfd-278">Andra API: er som import och export av enheter har också stöd för X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="18dfd-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="18dfd-279">C\# stöd</span><span class="sxs-lookup"><span data-stu-id="18dfd-279">C\# Support</span></span>

<span data-ttu-id="18dfd-280">Den **RegistryManager** klassen innehåller en programmässiga sättet att registrera en enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-280">The **RegistryManager** class provides a programmatic way to register a device.</span></span> <span data-ttu-id="18dfd-281">I synnerhet de **AddDeviceAsync** och **UpdateDeviceAsync** metoder kan du registrera och uppdatera en enhet i IoT-hubb identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-281">In particular, the **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you to register and update a device in the IoT Hub identity registry.</span></span> <span data-ttu-id="18dfd-282">Dessa två metoder ta en **enhet** instansen som indata.</span><span class="sxs-lookup"><span data-stu-id="18dfd-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="18dfd-283">Den **enhet** klassen innehåller en **autentisering** egenskap som kan du ange primära och sekundära X.509-certifikattumavtryck.</span><span class="sxs-lookup"><span data-stu-id="18dfd-283">The **Device** class includes an **Authentication** property that allows you to specify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="18dfd-284">Certifikatets tumavtryck representerar en SHA-1-hash för X.509-certifikat (som lagras med hjälp av binär kodning DER).</span><span class="sxs-lookup"><span data-stu-id="18dfd-284">The thumbprint represents a SHA-1 hash of the X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="18dfd-285">Du har möjlighet att ange tumavtryck för en primär eller sekundär tumavtryck eller båda.</span><span class="sxs-lookup"><span data-stu-id="18dfd-285">You have the option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="18dfd-286">Primära och sekundära tumavtryck stöds för att hantera scenarier för förnyelse av certifikat.</span><span class="sxs-lookup"><span data-stu-id="18dfd-286">Primary and secondary thumbprints are supported to handle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="18dfd-287">IoT-hubb inte kräver eller lagra hela X.509-certifikat, endast tumavtrycket.</span><span class="sxs-lookup"><span data-stu-id="18dfd-287">IoT Hub does not require or store the entire X.509 certificate, only the thumbprint.</span></span>

<span data-ttu-id="18dfd-288">Här följer ett exempel på C\# kodfragmentet att registrera en enhet med ett X.509-certifikat:</span><span class="sxs-lookup"><span data-stu-id="18dfd-288">Here is a sample C\# code snippet to register a device using an X.509 certificate:</span></span>

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

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="18dfd-289">Använd ett X.509-certifikat under körning</span><span class="sxs-lookup"><span data-stu-id="18dfd-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="18dfd-290">Den [Azure IoT-enhet SDK för .NET] [ lnk-client-sdk] (version 1.0.11+) stöder användning av X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="18dfd-290">The [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports the use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="18dfd-291">C\# stöd</span><span class="sxs-lookup"><span data-stu-id="18dfd-291">C\# Support</span></span>

<span data-ttu-id="18dfd-292">Klassen **DeviceAuthenticationWithX509Certificate** stöder skapandet av **DeviceClient** instanser med hjälp av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="18dfd-292">The class **DeviceAuthenticationWithX509Certificate** supports the creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="18dfd-293">X.509-certifikatet måste vara i formatet PFX (kallas även PKCS #12) som innehåller den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="18dfd-293">The X.509 certificate must be in the PFX (also called PKCS #12) format that includes the private key.</span></span>

<span data-ttu-id="18dfd-294">Här är ett kodfragment som exempel:</span><span class="sxs-lookup"><span data-stu-id="18dfd-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="18dfd-295">Anpassad autentisering</span><span class="sxs-lookup"><span data-stu-id="18dfd-295">Custom device authentication</span></span>

<span data-ttu-id="18dfd-296">Du kan använda IoT-hubben [identitetsregistret] [ lnk-identity-registry] att konfigurera per enhet säkerhetsreferenser och komma åt styra med hjälp av [token][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="18dfd-296">You can use the IoT Hub [identity registry][lnk-identity-registry] to configure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="18dfd-297">Om det finns redan en anpassad identitet registret och/eller autentisering schemat för en IoT-lösning, kan du överväga att skapa en *token service* att integrera infrastrukturen med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* to integrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="18dfd-298">På så sätt kan använda du andra IoT-funktioner i din lösning.</span><span class="sxs-lookup"><span data-stu-id="18dfd-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="18dfd-299">En token service är en anpassad molntjänst.</span><span class="sxs-lookup"><span data-stu-id="18dfd-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="18dfd-300">Den använder en IoT-hubb *delad åtkomstprincip* med **DeviceConnect** behörighet att skapa *enheten omfång* token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions to create *device-scoped* tokens.</span></span> <span data-ttu-id="18dfd-301">Dessa token kan en enhet för att ansluta till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-301">These tokens enable a device to connect to your IoT hub.</span></span>

![Steg för tokentjänsten mönster][img-tokenservice]

<span data-ttu-id="18dfd-303">Här följer Huvudsteg för tokentjänsten mönster:</span><span class="sxs-lookup"><span data-stu-id="18dfd-303">Here are the main steps of the token service pattern:</span></span>

1. <span data-ttu-id="18dfd-304">Skapa en IoT-hubb delad åtkomstprincip med **DeviceConnect** behörigheter för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="18dfd-305">Du kan skapa den här principen i den [Azure-portalen] [ lnk-management-portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="18dfd-305">You can create this policy in the [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="18dfd-306">Tokentjänsten som använder den här principen för att signera token skapas.</span><span class="sxs-lookup"><span data-stu-id="18dfd-306">The token service uses this policy to sign the tokens it creates.</span></span>
1. <span data-ttu-id="18dfd-307">När en enhet behöver åtkomst till din IoT-hubb, begär en signerade token från token-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-307">When a device needs to access your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="18dfd-308">Enheten kan autentisera med din anpassade identiteten registret-/ autentiseringsschema att fastställa enhetens identitet som tokentjänsten som används för att skapa token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-308">The device can authenticate with your custom identity registry/authentication scheme to determine the device identity that the token service uses to create the token.</span></span>
1. <span data-ttu-id="18dfd-309">Tokentjänsten som returnerar en token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-309">The token service returns a token.</span></span> <span data-ttu-id="18dfd-310">Token som skapas med hjälp av `/devices/{deviceId}` som `resourceURI`, med `deviceId` som den enhet som autentiseras.</span><span class="sxs-lookup"><span data-stu-id="18dfd-310">The token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as the device being authenticated.</span></span> <span data-ttu-id="18dfd-311">Token tjänsten använder princip för delad åtkomst för att skapa token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-311">The token service uses the shared access policy to construct the token.</span></span>
1. <span data-ttu-id="18dfd-312">Enheten använder token direkt med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-312">The device uses the token directly with the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="18dfd-313">Du kan använda .NET-klass [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] eller Java-klass [IotHubServiceSasToken] [ lnk-java-sas] att skapa en token i din token-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-313">You can use the .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or the Java class [IotHubServiceSasToken][lnk-java-sas] to create a token in your token service.</span></span>

<span data-ttu-id="18dfd-314">Tjänsten token kan ange token upphör att gälla efter behov.</span><span class="sxs-lookup"><span data-stu-id="18dfd-314">The token service can set the token expiration as desired.</span></span> <span data-ttu-id="18dfd-315">När token upphör att gälla ned IoT-hubben enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-315">When the token expires, the IoT hub severs the device connection.</span></span> <span data-ttu-id="18dfd-316">Enheten måste sedan begära en ny token från token-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18dfd-316">Then, the device must request a new token from the token service.</span></span> <span data-ttu-id="18dfd-317">En kort förfallotid ökar belastningen på både enheten och tjänsten token.</span><span class="sxs-lookup"><span data-stu-id="18dfd-317">A short expiry time increases the load on both the device and the token service.</span></span>

<span data-ttu-id="18dfd-318">För en enhet kan ansluta till din hubb, måste du ändå lägga till identitetsregistret IoT-hubb – även om enheten använder en token och inte en enhet för att ansluta.</span><span class="sxs-lookup"><span data-stu-id="18dfd-318">For a device to connect to your hub, you must still add it to the IoT Hub identity registry — even though the device is using a token and not a device key to connect.</span></span> <span data-ttu-id="18dfd-319">Därför kan du kan fortsätta att använda per enhet åtkomstkontroll genom att aktivera eller inaktivera enheten identiteter i den [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="18dfd-319">Therefore, you can continue to use per-device access control by enabling or disabling device identities in the [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="18dfd-320">Den här metoden minskar riskerna med att använda token med lång upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="18dfd-320">This approach mitigates the risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="18dfd-321">Jämförelse med en anpassad gateway</span><span class="sxs-lookup"><span data-stu-id="18dfd-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="18dfd-322">Tokentjänsten mönstret är det rekommenderade sättet att implementera en anpassad identitet registret/autentiseringsschema med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-322">The token service pattern is the recommended way to implement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="18dfd-323">Det här mönstret rekommenderas eftersom IoT-hubb fortsätter att hantera de flesta av lösningen trafiken.</span><span class="sxs-lookup"><span data-stu-id="18dfd-323">This pattern is recommended because IoT Hub continues to handle most of the solution traffic.</span></span> <span data-ttu-id="18dfd-324">Om anpassade autentiseringsschemat så sammanflätade med protokollet, du kan dock kräva en *anpassade gateway* bearbeta all trafik.</span><span class="sxs-lookup"><span data-stu-id="18dfd-324">However, if the custom authentication scheme is so intertwined with the protocol, you may require a *custom gateway* to process all the traffic.</span></span> <span data-ttu-id="18dfd-325">Ett exempel på en sådan situation använder[Transport Layer Security (TLS) och i förväg delade nycklar (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="18dfd-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="18dfd-326">Mer information finns i [protokollgatewayen] [ lnk-protocols] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-326">For more information, see the [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="18dfd-327">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="18dfd-327">Reference topics:</span></span>

<span data-ttu-id="18dfd-328">Följande referensavsnitt ge mer information om hur du styr åtkomst till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-328">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="18dfd-329">Behörigheter för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="18dfd-329">IoT Hub permissions</span></span>

<span data-ttu-id="18dfd-330">I följande tabell visas de behörigheter som du kan använda för att styra åtkomsten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-330">The following table lists the permissions you can use to control access to your IoT hub.</span></span>

| <span data-ttu-id="18dfd-331">Behörighet</span><span class="sxs-lookup"><span data-stu-id="18dfd-331">Permission</span></span> | <span data-ttu-id="18dfd-332">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="18dfd-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="18dfd-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="18dfd-333">**RegistryRead**</span></span> |<span data-ttu-id="18dfd-334">Ger läsbehörighet till identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-334">Grants read access to the identity registry.</span></span> <span data-ttu-id="18dfd-335">Mer information finns i [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="18dfd-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="18dfd-336">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="18dfd-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="18dfd-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="18dfd-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="18dfd-338">Ger Läs- och skrivåtkomst till identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="18dfd-338">Grants read and write access to the identity registry.</span></span> <span data-ttu-id="18dfd-339">Mer information finns i [identitetsregistret][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="18dfd-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="18dfd-340">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="18dfd-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="18dfd-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="18dfd-341">**ServiceConnect**</span></span> |<span data-ttu-id="18dfd-342">Ger åtkomst till cloud service-kommunikation och övervakning av slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-342">Grants access to cloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="18dfd-343">Ger behörighet att ta emot meddelanden från enhet till moln, skicka meddelanden moln till enhet och hämta motsvarande leverans bekräftelser.</span><span class="sxs-lookup"><span data-stu-id="18dfd-343">Grants permission to receive device-to-cloud messages, send cloud-to-device messages, and retrieve the corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="18dfd-344">Ger behörighet att hämta leverans bekräftelser för filen filöverföringar.</span><span class="sxs-lookup"><span data-stu-id="18dfd-344">Grants permission to retrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="18dfd-345">Ger behörighet att komma åt enheten twins för att uppdatera taggar och önskade egenskaper, hämta rapporterade egenskaper och köra frågor.</span><span class="sxs-lookup"><span data-stu-id="18dfd-345">Grants permission to access device twins to update tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="18dfd-346">Den här behörigheten används av backend-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="18dfd-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="18dfd-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="18dfd-347">**DeviceConnect**</span></span> |<span data-ttu-id="18dfd-348">Ger åtkomst till enheten riktade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-348">Grants access to device-facing endpoints.</span></span> <br/><span data-ttu-id="18dfd-349">Ger behörighet att skicka meddelanden från enhet till moln och ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-349">Grants permission to send device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="18dfd-350">Ger behörighet att utföra Filöverföring från en enhet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-350">Grants permission to perform file upload from a device.</span></span> <br/><span data-ttu-id="18dfd-351">Ger behörighet att ta emot enheten dubbla önskad egenskapen meddelanden och uppdatera enheten dubbla rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="18dfd-351">Grants permission to receive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="18dfd-352">Ger behörighet att utföra filen filöverföringar.</span><span class="sxs-lookup"><span data-stu-id="18dfd-352">Grants permission to perform file uploads.</span></span> <br/><span data-ttu-id="18dfd-353">Den här behörigheten används av enheter.</span><span class="sxs-lookup"><span data-stu-id="18dfd-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="18dfd-354">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="18dfd-354">Additional reference material</span></span>

<span data-ttu-id="18dfd-355">Andra referensavsnitten i utvecklarhandboken för IoT-hubben är:</span><span class="sxs-lookup"><span data-stu-id="18dfd-355">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="18dfd-356">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver de olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="18dfd-356">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="18dfd-357">[Begränsning och kvoter] [ lnk-quotas] beskriver kvoter och begränsning beteenden som tillämpas på tjänsten IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-357">[Throttling and quotas][lnk-quotas] describes the quotas and throttling behaviors that apply to the IoT Hub service.</span></span>
* <span data-ttu-id="18dfd-358">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] Listar olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="18dfd-358">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="18dfd-359">[IoT-hubb frågespråket] [ lnk-query] beskriver frågespråk som du kan använda för att hämta information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="18dfd-359">[IoT Hub query language][lnk-query] describes the query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="18dfd-360">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="18dfd-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18dfd-361">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18dfd-361">Next steps</span></span>

<span data-ttu-id="18dfd-362">Nu du har lärt dig hur du styr åtkomst IoT-hubb, kan du är intresserad av i följande avsnitt för IoT-hubb developer-guide:</span><span class="sxs-lookup"><span data-stu-id="18dfd-362">Now you have learned how to control access IoT Hub, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="18dfd-363">[Använd twins för enheten för att synkronisera tillstånd och konfigurationer][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="18dfd-363">[Use device twins to synchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="18dfd-364">[Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="18dfd-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="18dfd-365">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="18dfd-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="18dfd-366">Om du vill testa vissa av de begrepp som beskrivs i den här artikeln får du är intresserad av IoT-hubb följande kurser:</span><span class="sxs-lookup"><span data-stu-id="18dfd-366">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="18dfd-367">[Kom igång med Azure IoT-hubb][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="18dfd-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="18dfd-368">[Hur du skickar meddelanden moln till enhet med IoT-hubb][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="18dfd-368">[How to send cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="18dfd-369">[Bearbeta meddelanden från enhet till moln IoT-hubb][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="18dfd-369">[How to process IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

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
