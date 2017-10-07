---
title: "(HTTP/2) aaaToken-baserad autentisering för APNS i Azure Notification Hubs | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooleverage hello nya tokenautentisering för APNS"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a><span data-ttu-id="7a5db-103">Autentisering för tokenbaserad (HTTP/2) för APNS</span><span class="sxs-lookup"><span data-stu-id="7a5db-103">Token-based (HTTP/2) Authentication for APNS</span></span>
## <a name="overview"></a><span data-ttu-id="7a5db-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7a5db-104">Overview</span></span>
<span data-ttu-id="7a5db-105">Den här artikeln beskrivs hur toouse hello nya APN HTTP/2-protokollet med token baserade autentisering.</span><span class="sxs-lookup"><span data-stu-id="7a5db-105">This article details how toouse hello new APNS HTTP/2 protocol with token based authentication.</span></span>

<span data-ttu-id="7a5db-106">hello viktiga fördelar med att använda nya hello-protokollet är:</span><span class="sxs-lookup"><span data-stu-id="7a5db-106">hello key benefits of using hello new protocol include:</span></span>
-   <span data-ttu-id="7a5db-107">Token generering är relativt besväret ledigt (jämfört med toocertificates)</span><span class="sxs-lookup"><span data-stu-id="7a5db-107">Token generation is relatively hassle free (compared toocertificates)</span></span>
-   <span data-ttu-id="7a5db-108">Ingen mer förfallodatum – har du kontroll över dina autentiseringstoken och deras återkallelse</span><span class="sxs-lookup"><span data-stu-id="7a5db-108">No more expiry dates – you are in control of your authentication tokens and their revocation</span></span>
-   <span data-ttu-id="7a5db-109">Nyttolaster kan nu vara in too4 KB</span><span class="sxs-lookup"><span data-stu-id="7a5db-109">Payloads can now be up too4 KB</span></span>
- <span data-ttu-id="7a5db-110">Synkron feedback</span><span class="sxs-lookup"><span data-stu-id="7a5db-110">Synchronous feedback</span></span>
-   <span data-ttu-id="7a5db-111">Du är på Apples senaste protokollet – certifikat fortfarande använda hello binära protokoll som har markerats för utfasning</span><span class="sxs-lookup"><span data-stu-id="7a5db-111">You’re on Apple’s latest protocol – certificates still use hello binary protocol, which is marked for deprecation</span></span>

<span data-ttu-id="7a5db-112">Med den här nya funktionen kan du göra i två steg i ett par minuter:</span><span class="sxs-lookup"><span data-stu-id="7a5db-112">Using this new mechanism can be done in two steps in a few minutes:</span></span>
1.  <span data-ttu-id="7a5db-113">Hämta hello nödvändig information från hello på Apple Developer-kontoportalen</span><span class="sxs-lookup"><span data-stu-id="7a5db-113">Obtain hello necessary information from hello Apple Developer Account portal</span></span>
2.  <span data-ttu-id="7a5db-114">Konfigurera din meddelandehubb med hello ny information</span><span class="sxs-lookup"><span data-stu-id="7a5db-114">Configure your notification hub with hello new information</span></span>

<span data-ttu-id="7a5db-115">Notification Hubs är nu alla set toouse hello ny autentisering med APNS.</span><span class="sxs-lookup"><span data-stu-id="7a5db-115">Notification Hubs is now all set toouse hello new authentication system with APNS.</span></span> 

<span data-ttu-id="7a5db-116">Observera att om du migrerat från med hjälp av autentiseringsuppgifter för certifikat för APNS:</span><span class="sxs-lookup"><span data-stu-id="7a5db-116">Note that if you migrated from using certificate credentials for APNS:</span></span>
- <span data-ttu-id="7a5db-117">hello token egenskaper skriver över ditt certifikat i vårt system</span><span class="sxs-lookup"><span data-stu-id="7a5db-117">hello token properties overwrite your certificate in our system,</span></span>
- <span data-ttu-id="7a5db-118">men programmet fortsätter tooreceive meddelanden sömlöst.</span><span class="sxs-lookup"><span data-stu-id="7a5db-118">but your application continues tooreceive notifications seamlessly.</span></span>

## <a name="obtaining-authentication-information-from-apple"></a><span data-ttu-id="7a5db-119">Hämta autentiseringsuppgifter från Apple</span><span class="sxs-lookup"><span data-stu-id="7a5db-119">Obtaining authentication information from Apple</span></span>
<span data-ttu-id="7a5db-120">tooenable tokenbaserad autentisering, behöver du hello följande egenskaper från Apple Developer-konto:</span><span class="sxs-lookup"><span data-stu-id="7a5db-120">tooenable token-based authentication, you need hello following properties from your Apple Developer Account:</span></span>
### <a name="key-identifier"></a><span data-ttu-id="7a5db-121">Nyckel-ID</span><span class="sxs-lookup"><span data-stu-id="7a5db-121">Key Identifier</span></span>
<span data-ttu-id="7a5db-122">hello nyckelidentifierare kan hämtas från hello ”nycklar” sida i Apple Developer-konto</span><span class="sxs-lookup"><span data-stu-id="7a5db-122">hello key identifier can be obtained from hello "Keys" page in your Apple Developer Account</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a><span data-ttu-id="7a5db-123">Program-ID och programnamn</span><span class="sxs-lookup"><span data-stu-id="7a5db-123">Application Identifier & Application Name</span></span>
<span data-ttu-id="7a5db-124">hello programnamnet är tillgängliga via hello App-ID: N sida i hello utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="7a5db-124">hello application name is available via hello App IDs page in hello Developer Account.</span></span> 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

<span data-ttu-id="7a5db-125">hello identifierare är tillgänglig via hello medlemskap informationssidan i hello utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="7a5db-125">hello application identifier is available via hello membership details page in hello Developer Account.</span></span>
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a><span data-ttu-id="7a5db-126">Token för autentisering</span><span class="sxs-lookup"><span data-stu-id="7a5db-126">Authentication token</span></span>
<span data-ttu-id="7a5db-127">hello autentiseringstoken hämtas när du generera en token för ditt program.</span><span class="sxs-lookup"><span data-stu-id="7a5db-127">hello authentication token can be downloaded after you generate a token for your application.</span></span> <span data-ttu-id="7a5db-128">Information om hur toogenerate detta token, som finns för[Apples utvecklardokumentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span><span class="sxs-lookup"><span data-stu-id="7a5db-128">For details on how toogenerate this token, refer too[Apple’s Developer documentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span></span>

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a><span data-ttu-id="7a5db-129">Konfigurera din notification hub toouse tokenbaserad autentisering</span><span class="sxs-lookup"><span data-stu-id="7a5db-129">Configuring your notification hub toouse token-based authentication</span></span>
### <a name="configure-via-hello-azure-portal"></a><span data-ttu-id="7a5db-130">Konfigurera via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7a5db-130">Configure via hello Azure portal</span></span>
<span data-ttu-id="7a5db-131">tooenable token autentisering i hello portal, logga in toohello Azure-portalen och gå tooyour Notification Hub > Notification Services > APN-panelen.</span><span class="sxs-lookup"><span data-stu-id="7a5db-131">tooenable token based authentication in hello portal, log in toohello Azure portal and go tooyour Notification Hub > Notification Services > APNS panel.</span></span> 

<span data-ttu-id="7a5db-132">Det finns en ny egenskap – *autentiseringsläge*.</span><span class="sxs-lookup"><span data-stu-id="7a5db-132">There is a new property – *Authentication Mode*.</span></span> <span data-ttu-id="7a5db-133">När Token kan du tooupdate din hubb med alla hello relevanta token egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7a5db-133">Selecting Token allows you tooupdate your hub with all hello relevant token properties.</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- <span data-ttu-id="7a5db-134">Ange hello egenskaper som du hämtade från Apple developer-konto</span><span class="sxs-lookup"><span data-stu-id="7a5db-134">Enter hello properties you retrieved from your Apple developer account,</span></span> 
- <span data-ttu-id="7a5db-135">Välj din programläge (produktion eller Sandbox)</span><span class="sxs-lookup"><span data-stu-id="7a5db-135">choose your application mode (Production or Sandbox)</span></span> 
- <span data-ttu-id="7a5db-136">Klicka på Spara tooupdate APNS-autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="7a5db-136">click Save tooupdate your APNS credentials.</span></span> 

### <a name="configure-via-management-api-rest"></a><span data-ttu-id="7a5db-137">Konfigurera via Management API (REST)</span><span class="sxs-lookup"><span data-stu-id="7a5db-137">Configure via Management API (REST)</span></span>

<span data-ttu-id="7a5db-138">Du kan använda vår [management API: er](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate din notification hub toouse token-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="7a5db-138">You can use our [management APIs](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate your notification hub toouse token-based authentication.</span></span>
<span data-ttu-id="7a5db-139">Beroende på om är hello-programmet som du konfigurerar en Sandbox eller produktion app (anges i Apple Developer-konto), använder du någon av hello motsvarande slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="7a5db-139">Depending on whether hello application you’re configuring is a Sandbox or Production app (specified in your Apple Developer Account), use one of hello corresponding endpoints:</span></span>

- <span data-ttu-id="7a5db-140">Sandboxslutpunkt: [https://api.development.push.apple.com:443-3-enhet](https://api.development.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="7a5db-140">Sandbox Endpoint: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)</span></span>
- <span data-ttu-id="7a5db-141">Produktion slutpunkt: [https://api.push.apple.com:443-3-enhet](https://api.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="7a5db-141">Production Endpoint: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7a5db-142">Tokenbaserad autentisering kräver en API-version: **2017-04 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="7a5db-142">Token-based authentication requires an API version of: **2017-04 or later**.</span></span>
> 
> 

<span data-ttu-id="7a5db-143">Här är ett exempel på en PUT-begäran tooupdate en hubb med tokenbaserad autentisering:</span><span class="sxs-lookup"><span data-stu-id="7a5db-143">Here’s an example of a PUT request tooupdate a hub with token-based authentication:</span></span>


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-hello-net-sdk"></a><span data-ttu-id="7a5db-144">Konfigurera via hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7a5db-144">Configure via hello .NET SDK</span></span>
<span data-ttu-id="7a5db-145">Du kan konfigurera din hubb toouse token autentisering med hjälp av vår [senaste klient-SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span><span class="sxs-lookup"><span data-stu-id="7a5db-145">You can configure your hub toouse token based authentication using our [latest client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span></span> 

<span data-ttu-id="7a5db-146">Här är ett kodexempel illustrerar hello korrekt syntax:</span><span class="sxs-lookup"><span data-stu-id="7a5db-146">Here’s a code sample illustrating hello correct usage:</span></span>


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a><span data-ttu-id="7a5db-147">Återför toousing certifikatbaserad autentisering</span><span class="sxs-lookup"><span data-stu-id="7a5db-147">Reverting toousing certificate-based authentication</span></span>
<span data-ttu-id="7a5db-148">Du kan återgå vid någon tidpunkt toousing certifikatbaserad autentisering med föregående metod och skicka hello certifikat i stället för hello token egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7a5db-148">You can revert at any time toousing certificate-based authentication by using any preceding method and passing hello certificate instead of hello token properties.</span></span> <span data-ttu-id="7a5db-149">Att åtgärden skriver över hello tidigare lagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7a5db-149">That action overwrites hello previously stored credentials.</span></span>
