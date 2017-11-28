---
title: aaaAzure Mobile Engagement Android SDK-Integration
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
ms.openlocfilehash: c57132ff49cf8c335627a72b37f9b78529e84f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-adm-with-engagement"></a><span data-ttu-id="1c602-103">Hur tooIntegrate ADM med Engagement</span><span class="sxs-lookup"><span data-stu-id="1c602-103">How tooIntegrate ADM with Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1c602-104">Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="1c602-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="1c602-105">Det här dokumentet är användbar bara om du redan har integrerat hello Reach-modulen och planera toopush Amazon-enheter.</span><span class="sxs-lookup"><span data-stu-id="1c602-105">This document is useful only if you already integrated hello Reach module and plan toopush Amazon devices.</span></span> <span data-ttu-id="1c602-106">toointegrate Reach-kampanjer i ditt program, Läs först hur tooIntegrate Engagement nå på Android.</span><span class="sxs-lookup"><span data-stu-id="1c602-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="1c602-107">Introduktion</span><span class="sxs-lookup"><span data-stu-id="1c602-107">Introduction</span></span>
<span data-ttu-id="1c602-108">Integrera ADM kan ditt program toobe pushas när måldatorn Amazon Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="1c602-108">Integrating ADM allows your application toobe pushed when targeting Amazon Android devices.</span></span>

<span data-ttu-id="1c602-109">ADM-nyttolaster pushas toohello SDK alltid innehålla hello `azme` nyckel i hello dataobjekt.</span><span class="sxs-lookup"><span data-stu-id="1c602-109">ADM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="1c602-110">Därför om du använder ADM för andra ändamål i ditt program, kan du filtrera push-meddelanden baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c602-110">Thus if you use ADM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c602-111">Endast Amazon Kindle enheter som kör Android 4.0.3 eller senare som stöds av Amazon Device Messaging; Du kan dock integrera koden på ett säkert sätt på andra enheter.</span><span class="sxs-lookup"><span data-stu-id="1c602-111">Only Amazon Kindle devices running Android 4.0.3 or above are supported by Amazon Device Messaging; however, you can integrate this code safely on other devices.</span></span>
> 
> 

## <a name="sign-up-tooadm"></a><span data-ttu-id="1c602-112">Registrera dig tooADM</span><span class="sxs-lookup"><span data-stu-id="1c602-112">Sign up tooADM</span></span>
<span data-ttu-id="1c602-113">Om du inte redan har gjort, måste du aktivera ADM på Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="1c602-113">If not already done, you must enable ADM on your Amazon account.</span></span>

<span data-ttu-id="1c602-114">hello proceduren beskrivs på: [ <https://developer.amazon.com/sdk/adm/credentials.html>].</span><span class="sxs-lookup"><span data-stu-id="1c602-114">hello procedure is detailed at: [<https://developer.amazon.com/sdk/adm/credentials.html>].</span></span>

<span data-ttu-id="1c602-115">När du slutför hello proceduren får du:</span><span class="sxs-lookup"><span data-stu-id="1c602-115">Upon completing hello procedure, you get:</span></span>

* <span data-ttu-id="1c602-116">OAuth autentiseringsuppgifter (klient-ID och en Klienthemlighet) för Engagement toobe kan toopush dina enheter.</span><span class="sxs-lookup"><span data-stu-id="1c602-116">OAuth credentials (a Client ID and a Client Secret) for Engagement toobe able toopush your devices.</span></span>
* <span data-ttu-id="1c602-117">En API-nyckel som integreras i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1c602-117">An API Key that must be integrated into your application.</span></span>

## <a name="sdk-integration"></a><span data-ttu-id="1c602-118">SDK-integration</span><span class="sxs-lookup"><span data-stu-id="1c602-118">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="1c602-119">Hantera registreringar för enhet</span><span class="sxs-lookup"><span data-stu-id="1c602-119">Managing device registrations</span></span>
<span data-ttu-id="1c602-120">Varje enhet måste skicka en registrering kommandot toohello ADM-servrar, annars de inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="1c602-120">Each device must send a registration command toohello ADM servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="1c602-121">Om du redan använder hello [ADM-klientbiblioteket], och redan har [integrerad ADM] du kan gå direkt tooandroid och sdk-adm-ta emot.</span><span class="sxs-lookup"><span data-stu-id="1c602-121">If you already use hello [ADM client library], and already have [integrated ADM] you can directly go tooandroid-sdk-adm-receive.</span></span>

<span data-ttu-id="1c602-122">Om du inte har integrerat ADM ännu Engagement har ett enklare sätt tooenable den i ditt program:</span><span class="sxs-lookup"><span data-stu-id="1c602-122">If you have not integrated ADM yet, Engagement has a simpler way tooenable it in your application:</span></span>

<span data-ttu-id="1c602-123">Redigera din `AndroidManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="1c602-123">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="1c602-124">Lägg till Hej namnområdet för Amazon, hello filen börja så här:</span><span class="sxs-lookup"><span data-stu-id="1c602-124">Add hello Amazon namespace, hello file should begin like this:</span></span>
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* <span data-ttu-id="1c602-125">I hello `<application/>` tagg, lägga till det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="1c602-125">Inside hello `<application/>` tag, add this section:</span></span>
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* <span data-ttu-id="1c602-126">Du kan ha ett build-fel om ditt mål för projektgenerering är lägre än Android 2.1 när du lägger till hello amazon-tagg.</span><span class="sxs-lookup"><span data-stu-id="1c602-126">After adding hello amazon tag, you may have a build error if your Project Build Target is below Android 2.1.</span></span> <span data-ttu-id="1c602-127">Du har toouse en **Android 2.1 +** skapa mål (oroa dig inte, kan du fortfarande ha en `minSdkVersion` ange too4).</span><span class="sxs-lookup"><span data-stu-id="1c602-127">You have toouse an **Android 2.1+** build target (don't worry, you can still have a `minSdkVersion` set too4).</span></span>
* <span data-ttu-id="1c602-128">Integrera hello ADM API-nyckel som en tillgång genom att följa [proceduren].</span><span class="sxs-lookup"><span data-stu-id="1c602-128">Integrate hello ADM API Key as an asset by following [this procedure].</span></span>

<span data-ttu-id="1c602-129">Följ sedan instruktionerna för hello hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c602-129">Then follow hello instructions of hello next sections.</span></span>

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="1c602-130">Kommunicera id toohello Engagement Push registreringstjänsten och ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="1c602-130">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="1c602-131">I ordning toocommunicate hello registrerings-id för hello enheten toohello Engagement Push-tjänst och ta emot dess meddelanden, lägga till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagga (även om du använder ADM utan behov av):</span><span class="sxs-lookup"><span data-stu-id="1c602-131">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you use ADM without Engagement):</span></span>

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

<span data-ttu-id="1c602-132">Kontrollera att du har följande behörigheter i hello din `AndroidManifest.xml` (innan hello `</application>` tagg).</span><span class="sxs-lookup"><span data-stu-id="1c602-132">Ensure you have hello following permissions in your `AndroidManifest.xml` (before hello `</application>` tag).</span></span>

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a><span data-ttu-id="1c602-133">Bevilja Engagement OAuth-autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1c602-133">Grant Engagement OAuth credentials</span></span>
<span data-ttu-id="1c602-134">Skicka dina inloggningsuppgifter för OAuth (klient-ID och Klienthemlighet) i Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="1c602-134">Submit your OAuth Credentials (Client ID and Client Secret) in Engagement Portal.</span></span>

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM-klientbiblioteket]:https://developer.amazon.com/sdk/adm/setup.html
[integrerad ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[proceduren]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
