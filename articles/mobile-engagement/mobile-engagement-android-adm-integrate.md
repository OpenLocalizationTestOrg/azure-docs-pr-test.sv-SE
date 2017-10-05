---
title: Azure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 43987962ea2b7b825b88643d18b4db65f1f1670e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-adm-with-engagement"></a><span data-ttu-id="645f6-103">Hur du integrerar ADM med Engagement</span><span class="sxs-lookup"><span data-stu-id="645f6-103">How to Integrate ADM with Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="645f6-104">Du måste följa integration proceduren i hur du integrerar Engagement på Android dokumentet innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="645f6-104">You must follow the integration procedure described in the How to Integrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="645f6-105">Det här dokumentet är användbar bara om du redan har integrerat med Reach-modulen och planera att skicka Amazon-enheter.</span><span class="sxs-lookup"><span data-stu-id="645f6-105">This document is useful only if you already integrated the Reach module and plan to push Amazon devices.</span></span> <span data-ttu-id="645f6-106">Om du vill integrera Reach-kampanjer i ditt program, Läs först hur du integrerar Engagement nå på Android.</span><span class="sxs-lookup"><span data-stu-id="645f6-106">To integrate Reach campaigns in your application, please read first How to Integrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="645f6-107">Introduktion</span><span class="sxs-lookup"><span data-stu-id="645f6-107">Introduction</span></span>
<span data-ttu-id="645f6-108">Integrera ADM kan ditt program ska kunna skickas när måldatorn Amazon Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="645f6-108">Integrating ADM allows your application to be pushed when targeting Amazon Android devices.</span></span>

<span data-ttu-id="645f6-109">ADM-nyttolaster pushas till SDK alltid innehålla den `azme` nyckel i dataobjektet.</span><span class="sxs-lookup"><span data-stu-id="645f6-109">ADM payloads pushed to the SDK always contain the `azme` key in the data object.</span></span> <span data-ttu-id="645f6-110">Därför om du använder ADM för andra ändamål i ditt program, kan du filtrera push-meddelanden baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="645f6-110">Thus if you use ADM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="645f6-111">Endast Amazon Kindle enheter som kör Android 4.0.3 eller senare som stöds av Amazon Device Messaging; Du kan dock integrera koden på ett säkert sätt på andra enheter.</span><span class="sxs-lookup"><span data-stu-id="645f6-111">Only Amazon Kindle devices running Android 4.0.3 or above are supported by Amazon Device Messaging; however, you can integrate this code safely on other devices.</span></span>
> 
> 

## <a name="sign-up-to-adm"></a><span data-ttu-id="645f6-112">Registrera dig för ADM</span><span class="sxs-lookup"><span data-stu-id="645f6-112">Sign up to ADM</span></span>
<span data-ttu-id="645f6-113">Om du inte redan har gjort, måste du aktivera ADM på Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="645f6-113">If not already done, you must enable ADM on your Amazon account.</span></span>

<span data-ttu-id="645f6-114">Förfarandet som beskrivs i: [ <https://developer.amazon.com/sdk/adm/credentials.html>].</span><span class="sxs-lookup"><span data-stu-id="645f6-114">The procedure is detailed at: [<https://developer.amazon.com/sdk/adm/credentials.html>].</span></span>

<span data-ttu-id="645f6-115">När du slutför proceduren får du:</span><span class="sxs-lookup"><span data-stu-id="645f6-115">Upon completing the procedure, you get:</span></span>

* <span data-ttu-id="645f6-116">OAuth-autentiseringsuppgifter (klient-ID och en Klienthemlighet) för Engagement för att kunna push dina enheter.</span><span class="sxs-lookup"><span data-stu-id="645f6-116">OAuth credentials (a Client ID and a Client Secret) for Engagement to be able to push your devices.</span></span>
* <span data-ttu-id="645f6-117">En API-nyckel som integreras i ditt program.</span><span class="sxs-lookup"><span data-stu-id="645f6-117">An API Key that must be integrated into your application.</span></span>

## <a name="sdk-integration"></a><span data-ttu-id="645f6-118">SDK-integration</span><span class="sxs-lookup"><span data-stu-id="645f6-118">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="645f6-119">Hantera registreringar för enhet</span><span class="sxs-lookup"><span data-stu-id="645f6-119">Managing device registrations</span></span>
<span data-ttu-id="645f6-120">Varje enhet måste skicka ett kommando för registrering till ADM-servrar, annars de inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="645f6-120">Each device must send a registration command to the ADM servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="645f6-121">Om du redan använder den [ADM-klientbiblioteket], och redan har [integrerad ADM] du kan gå direkt till android-sdk-adm-ta emot.</span><span class="sxs-lookup"><span data-stu-id="645f6-121">If you already use the [ADM client library], and already have [integrated ADM] you can directly go to android-sdk-adm-receive.</span></span>

<span data-ttu-id="645f6-122">Om du inte har integrerat ADM, har Engagement ett enklare sätt att aktivera den i ditt program:</span><span class="sxs-lookup"><span data-stu-id="645f6-122">If you have not integrated ADM yet, Engagement has a simpler way to enable it in your application:</span></span>

<span data-ttu-id="645f6-123">Redigera din `AndroidManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="645f6-123">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="645f6-124">Lägg till namnområdet för Amazon filen ska börja så här:</span><span class="sxs-lookup"><span data-stu-id="645f6-124">Add the Amazon namespace, the file should begin like this:</span></span>
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* <span data-ttu-id="645f6-125">I den `<application/>` tagg, lägga till det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="645f6-125">Inside the `<application/>` tag, add this section:</span></span>
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* <span data-ttu-id="645f6-126">Du kan ha ett build-fel om ditt mål för projektgenerering är lägre än Android 2.1 när du lägger till amazon-taggen.</span><span class="sxs-lookup"><span data-stu-id="645f6-126">After adding the amazon tag, you may have a build error if your Project Build Target is below Android 2.1.</span></span> <span data-ttu-id="645f6-127">Du måste använda ett **Android 2.1 +** skapa mål (oroa dig inte, kan du fortfarande ha en `minSdkVersion` värdet 4).</span><span class="sxs-lookup"><span data-stu-id="645f6-127">You have to use an **Android 2.1+** build target (don't worry, you can still have a `minSdkVersion` set to 4).</span></span>
* <span data-ttu-id="645f6-128">Integrera ADM API-nyckeln som en tillgång genom att följa [proceduren].</span><span class="sxs-lookup"><span data-stu-id="645f6-128">Integrate the ADM API Key as an asset by following [this procedure].</span></span>

<span data-ttu-id="645f6-129">Följ sedan anvisningarna i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="645f6-129">Then follow the instructions of the next sections.</span></span>

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="645f6-130">Kommunicera registrerings-id till Engagement Push-tjänsten och ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="645f6-130">Communicate registration id to the Engagement Push service and receive notifications</span></span>
<span data-ttu-id="645f6-131">För att kunna kommunicera registrerings-id för enheten till Engagement Push-tjänsten och ta emot dess meddelanden, lägger du till följande till din `AndroidManifest.xml` filen inuti den `<application/>` tagga (även om du använder ADM utan behov av):</span><span class="sxs-lookup"><span data-stu-id="645f6-131">In order to communicate the registration id of the device to the Engagement Push service and receive its notifications, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag (even if you use ADM without Engagement):</span></span>

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

<span data-ttu-id="645f6-132">Se till att du har följande behörigheter i din `AndroidManifest.xml` (innan den `</application>` tagg).</span><span class="sxs-lookup"><span data-stu-id="645f6-132">Ensure you have the following permissions in your `AndroidManifest.xml` (before the `</application>` tag).</span></span>

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a><span data-ttu-id="645f6-133">Bevilja Engagement OAuth-autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="645f6-133">Grant Engagement OAuth credentials</span></span>
<span data-ttu-id="645f6-134">Skicka dina inloggningsuppgifter för OAuth (klient-ID och Klienthemlighet) i Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="645f6-134">Submit your OAuth Credentials (Client ID and Client Secret) in Engagement Portal.</span></span>

<span data-ttu-id="645f6-135">[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html</span><span class="sxs-lookup"><span data-stu-id="645f6-135">[<https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html</span></span>
<span data-ttu-id="645f6-136">[ADM-klientbiblioteket]:https://developer.amazon.com/sdk/adm/setup.html</span><span class="sxs-lookup"><span data-stu-id="645f6-136">[ADM client library]:https://developer.amazon.com/sdk/adm/setup.html</span></span>
<span data-ttu-id="645f6-137">[integrerad ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html</span><span class="sxs-lookup"><span data-stu-id="645f6-137">[integrated ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html</span></span>
<span data-ttu-id="645f6-138">[proceduren]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset</span><span class="sxs-lookup"><span data-stu-id="645f6-138">[this procedure]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset</span></span>
