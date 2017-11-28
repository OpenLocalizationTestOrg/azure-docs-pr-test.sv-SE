---
title: "aaaHow toouse hello Azure Mobile Apps-SDK för Android | Microsoft Docs"
description: "Hur toouse hello Azure Mobile Apps-SDK för Android"
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: glenga
ms.openlocfilehash: 56eb73c4e1703d69877be499a09fc2130f1d68e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a><span data-ttu-id="ce701-103">Hur toouse hello Azure Mobile Apps-SDK för Android</span><span class="sxs-lookup"><span data-stu-id="ce701-103">How toouse hello Azure Mobile Apps SDK for Android</span></span>

<span data-ttu-id="ce701-104">Den här guiden visar hur toouse hello Android klient-SDK för Mobile Apps tooimplement vanliga scenarier, såsom:</span><span class="sxs-lookup"><span data-stu-id="ce701-104">This guide shows you how toouse hello Android client SDK for Mobile Apps tooimplement common scenarios, such as:</span></span>

* <span data-ttu-id="ce701-105">Frågar efter data (Infoga, uppdatera och ta bort).</span><span class="sxs-lookup"><span data-stu-id="ce701-105">Querying for data (inserting, updating, and deleting).</span></span>
* <span data-ttu-id="ce701-106">Autentisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-106">Authentication.</span></span>
* <span data-ttu-id="ce701-107">Hantera fel.</span><span class="sxs-lookup"><span data-stu-id="ce701-107">Handling errors.</span></span>
* <span data-ttu-id="ce701-108">Anpassa hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="ce701-108">Customizing hello client.</span></span>

<span data-ttu-id="ce701-109">Den här guiden fokuserar på hello klientsidan Android SDK.</span><span class="sxs-lookup"><span data-stu-id="ce701-109">This guide focuses on hello client-side Android SDK.</span></span>  <span data-ttu-id="ce701-110">toolearn mer om hello serversidan SDK: er för Mobile Apps finns i avsnittet [arbeta med .NET-serverdel SDK] [ 10] eller [hur toouse hello Node.js-serverdel SDK] [ 11].</span><span class="sxs-lookup"><span data-stu-id="ce701-110">toolearn more about hello server-side SDKs for Mobile Apps, see [Work with .NET backend SDK][10] or [How toouse hello Node.js backend SDK][11].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="ce701-111">Referensdokumentationen</span><span class="sxs-lookup"><span data-stu-id="ce701-111">Reference Documentation</span></span>

<span data-ttu-id="ce701-112">Du kan hitta hello [Javadocs API-referens] [ 12] för hello Android klientbiblioteket på GitHub.</span><span class="sxs-lookup"><span data-stu-id="ce701-112">You can find hello [Javadocs API reference][12] for hello Android client library on GitHub.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ce701-113">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="ce701-113">Supported Platforms</span></span>

<span data-ttu-id="ce701-114">hello Azure Mobile Apps-SDK för Android stöder API nivåer 19 till 24 (KitKat via Nougat) för Telefoner och surfplattor formfaktorer.</span><span class="sxs-lookup"><span data-stu-id="ce701-114">hello Azure Mobile Apps SDK for Android supports API levels 19 through 24 (KitKat through Nougat) for phone and tablet form factors.</span></span>  <span data-ttu-id="ce701-115">Autentisering, i synnerhet använder ett vanliga web framework metoden toogather autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ce701-115">Authentication, in particular, utilizes a common web framework approach toogather credentials.</span></span>  <span data-ttu-id="ce701-116">Server-flöde autentisering fungerar inte med liten faktor enheter, till exempel ur.</span><span class="sxs-lookup"><span data-stu-id="ce701-116">Server-flow authentication does not work with small form factor devices such as watches.</span></span>

## <a name="setup-and-prerequisites"></a><span data-ttu-id="ce701-117">Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="ce701-117">Setup and Prerequisites</span></span>

<span data-ttu-id="ce701-118">Fullständig hello [Mobile Apps quickstart](app-service-mobile-android-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ce701-118">Complete hello [Mobile Apps quickstart](app-service-mobile-android-get-started.md) tutorial.</span></span>  <span data-ttu-id="ce701-119">Den här åtgärden säkerställer att alla förutsättningar för att utveckla Azure Mobile Apps har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="ce701-119">This task ensures all pre-requisites for developing Azure Mobile Apps have been met.</span></span>  <span data-ttu-id="ce701-120">hello Quickstart hjälper dig att konfigurera ditt konto och skapa din första mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-120">hello Quickstart also helps you configure your account and create your first Mobile App backend.</span></span>

<span data-ttu-id="ce701-121">Om du inte toocomplete hello Snabbstartsguide slutföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ce701-121">If you decide not toocomplete hello Quickstart tutorial, complete hello following tasks:</span></span>

* <span data-ttu-id="ce701-122">[Skapa en mobilappsserverdel] [ 13] toouse med din Android-app.</span><span class="sxs-lookup"><span data-stu-id="ce701-122">[create a Mobile App backend][13] toouse with your Android app.</span></span>
* <span data-ttu-id="ce701-123">I Android Studio [uppdatering hello Gradle skapa filer](#gradle-build).</span><span class="sxs-lookup"><span data-stu-id="ce701-123">In Android Studio, [update hello Gradle build files](#gradle-build).</span></span>
* <span data-ttu-id="ce701-124">[Aktivera internet behörigheten](#enable-internet).</span><span class="sxs-lookup"><span data-stu-id="ce701-124">[Enable internet permission](#enable-internet).</span></span>

### <span data-ttu-id="ce701-125"><a name="gradle-build"></a>Uppdatera hello Gradle skapa filen</span><span class="sxs-lookup"><span data-stu-id="ce701-125"><a name="gradle-build"></a>Update hello Gradle build file</span></span>

<span data-ttu-id="ce701-126">Ändra både **build.gradle** filer:</span><span class="sxs-lookup"><span data-stu-id="ce701-126">Change both **build.gradle** files:</span></span>

1. <span data-ttu-id="ce701-127">Lägg till den här koden toohello *projekt* nivå **build.gradle** fil i hello *buildscript* tagg:</span><span class="sxs-lookup"><span data-stu-id="ce701-127">Add this code toohello *Project* level **build.gradle** file inside hello *buildscript* tag:</span></span>

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. <span data-ttu-id="ce701-128">Lägg till den här koden toohello *modulen app* nivå **build.gradle** fil i hello *beroenden* tagg:</span><span class="sxs-lookup"><span data-stu-id="ce701-128">Add this code toohello *Module app* level **build.gradle** file inside hello *dependencies* tag:</span></span>

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    <span data-ttu-id="ce701-129">Hello senaste versionen är för närvarande 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="ce701-129">Currently hello latest version is 3.3.0.</span></span> <span data-ttu-id="ce701-130">hello stöds versioner visas [på bintray][14].</span><span class="sxs-lookup"><span data-stu-id="ce701-130">hello supported versions are listed [on bintray][14].</span></span>

### <span data-ttu-id="ce701-131"><a name="enable-internet"></a>Aktivera internet behörighet</span><span class="sxs-lookup"><span data-stu-id="ce701-131"><a name="enable-internet"></a>Enable internet permission</span></span>

<span data-ttu-id="ce701-132">tooaccess Azure din app måste behörighet hello INTERNET aktiverad.</span><span class="sxs-lookup"><span data-stu-id="ce701-132">tooaccess Azure, your app must have hello INTERNET permission enabled.</span></span> <span data-ttu-id="ce701-133">Om den inte redan är aktiverat lägger du till följande rad med kod tooyour hello **AndroidManifest.xml** fil:</span><span class="sxs-lookup"><span data-stu-id="ce701-133">If it's not already enabled, add hello following line of code tooyour **AndroidManifest.xml** file:</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a><span data-ttu-id="ce701-134">Skapa en klientanslutning</span><span class="sxs-lookup"><span data-stu-id="ce701-134">Create a Client Connection</span></span>

<span data-ttu-id="ce701-135">Azure Mobile Apps innehåller fyra funktioner tooyour mobila program:</span><span class="sxs-lookup"><span data-stu-id="ce701-135">Azure Mobile Apps provides four functions tooyour mobile application:</span></span>

* <span data-ttu-id="ce701-136">Dataåtkomst och offlinesynkronisering med en Azure Mobile Apps-tjänst.</span><span class="sxs-lookup"><span data-stu-id="ce701-136">Data Access and Offline Synchronization with an Azure Mobile Apps Service.</span></span>
* <span data-ttu-id="ce701-137">Anropa anpassade API: er skrivs med hello Azure Mobile Apps Server SDK.</span><span class="sxs-lookup"><span data-stu-id="ce701-137">Call Custom APIs written with hello Azure Mobile Apps Server SDK.</span></span>
* <span data-ttu-id="ce701-138">Autentisering med Azure App Service-autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-138">Authentication with Azure App Service Authentication and Authorization.</span></span>
* <span data-ttu-id="ce701-139">Registrera push-meddelande med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="ce701-139">Push Notification Registration with Notification Hubs.</span></span>

<span data-ttu-id="ce701-140">Dessa funktioner först måste du skapa en `MobileServiceClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-140">Each of these functions first requires that you create a `MobileServiceClient` object.</span></span>  <span data-ttu-id="ce701-141">Endast en `MobileServiceClient` objekt ska skapas i din mobila klienten (det vill säga att det ska vara en Singleton-mönster).</span><span class="sxs-lookup"><span data-stu-id="ce701-141">Only one `MobileServiceClient` object should be created within your mobile client (that is, it should be a Singleton pattern).</span></span>  <span data-ttu-id="ce701-142">toocreate en `MobileServiceClient` objekt:</span><span class="sxs-lookup"><span data-stu-id="ce701-142">toocreate a `MobileServiceClient` object:</span></span>

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

<span data-ttu-id="ce701-143">Hej `<MobileAppUrl>` är en sträng eller ett URL-objekt som pekar tooyour mobilserverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-143">hello `<MobileAppUrl>` is either a string or a URL object that points tooyour mobile backend.</span></span>  <span data-ttu-id="ce701-144">Om du använder Azure App Service toohost din mobila serverdel sedan kontrollera att du använder hello säker `https://` version av hello-URL.</span><span class="sxs-lookup"><span data-stu-id="ce701-144">If you are using Azure App Service toohost your mobile backend, then ensure you use hello secure `https://` version of hello URL.</span></span>

<span data-ttu-id="ce701-145">hello klienten kräver också åtkomst toohello aktivitet eller kontext - hello `this` parameter i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="ce701-145">hello client also requires access toohello Activity or Context - hello `this` parameter in hello example.</span></span>  <span data-ttu-id="ce701-146">Hej MobileServiceClient konstruktionen ska ske inom hello `onCreate()` metod för hello aktiviteten som refereras i hello `AndroidManifest.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="ce701-146">hello MobileServiceClient construction should happen within hello `onCreate()` method of hello Activity referenced in hello `AndroidManifest.xml` file.</span></span>

<span data-ttu-id="ce701-147">Som bästa praxis bör du abstrakt serverkommunikation i sin egen (singleton-mönster)-klassen.</span><span class="sxs-lookup"><span data-stu-id="ce701-147">As a best practice, you should abstract server communication into its own (singleton-pattern) class.</span></span>  <span data-ttu-id="ce701-148">I det här fallet bör du skickar hello aktiviteten inom hello konstruktorn tooappropriately konfigurera hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ce701-148">In this case, you should pass hello Activity within hello constructor tooappropriately configure hello service.</span></span>  <span data-ttu-id="ce701-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ce701-149">For example:</span></span>

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

<span data-ttu-id="ce701-150">Nu kan du anropa `AzureServiceAdapter.Initialize(this);` i hello `onCreate()` metod för den huvudsakliga aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="ce701-150">You can now call `AzureServiceAdapter.Initialize(this);` in hello `onCreate()` method of your main activity.</span></span>  <span data-ttu-id="ce701-151">Använda andra metoder som behöver toohello åtkomstklienten `AzureServiceAdapter.getInstance();` tooobtain ett referens toohello service-kort.</span><span class="sxs-lookup"><span data-stu-id="ce701-151">Any other methods needing access toohello client use `AzureServiceAdapter.getInstance();` tooobtain a reference toohello service adapter.</span></span>

## <a name="data-operations"></a><span data-ttu-id="ce701-152">Dataåtgärder</span><span class="sxs-lookup"><span data-stu-id="ce701-152">Data Operations</span></span>

<span data-ttu-id="ce701-153">hello core av hello Azure Mobile Apps-SDK är tooprovide åtkomst toodata lagras i SQL Azure på hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-153">hello core of hello Azure Mobile Apps SDK is tooprovide access toodata stored within SQL Azure on hello Mobile App backend.</span></span>  <span data-ttu-id="ce701-154">Du kan komma åt dessa data med strikt typkontroll klasser (rekommenderas) eller utan angiven frågor (rekommenderas inte).</span><span class="sxs-lookup"><span data-stu-id="ce701-154">You can access this data using strongly typed classes (preferred) or untyped queries (not recommended).</span></span>  <span data-ttu-id="ce701-155">hello huvuddelen av det här avsnittet behandlar med strikt typkontroll klasser.</span><span class="sxs-lookup"><span data-stu-id="ce701-155">hello bulk of this section deals with using strongly typed classes.</span></span>

### <a name="define-client-data-classes"></a><span data-ttu-id="ce701-156">Definiera dataklasser som klienten</span><span class="sxs-lookup"><span data-stu-id="ce701-156">Define client data classes</span></span>

<span data-ttu-id="ce701-157">tooaccess data från SQL Azure-tabeller, definiera dataklasser som klienten som motsvarar toohello tabeller i hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-157">tooaccess data from SQL Azure tables, define client data classes that correspond toohello tables in hello Mobile App backend.</span></span> <span data-ttu-id="ce701-158">Exemplen i det här avsnittet förutsätter att en tabell med namnet **MyDataTable**, som har hello följande kolumner:</span><span class="sxs-lookup"><span data-stu-id="ce701-158">Examples in this topic assume a table named **MyDataTable**, which has hello following columns:</span></span>

* <span data-ttu-id="ce701-159">id</span><span class="sxs-lookup"><span data-stu-id="ce701-159">id</span></span>
* <span data-ttu-id="ce701-160">Text</span><span class="sxs-lookup"><span data-stu-id="ce701-160">text</span></span>
* <span data-ttu-id="ce701-161">Slutför</span><span class="sxs-lookup"><span data-stu-id="ce701-161">complete</span></span>

<span data-ttu-id="ce701-162">hello motsvarande skrivna klientsidan objektet finns i en fil med namnet **MyDataTable.java**:</span><span class="sxs-lookup"><span data-stu-id="ce701-162">hello corresponding typed client-side object resides in a file called **MyDataTable.java**:</span></span>

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

<span data-ttu-id="ce701-163">Lägg till get och set-metoder för varje fält som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="ce701-163">Add getter and setter methods for each field that you add.</span></span>  <span data-ttu-id="ce701-164">Om din SQL Azure-tabellen innehåller fler kolumner, kan du lägga till hello motsvarande fält toothis klass.</span><span class="sxs-lookup"><span data-stu-id="ce701-164">If your SQL Azure table contains more columns, you would add hello corresponding fields toothis class.</span></span>  <span data-ttu-id="ce701-165">Till exempel, om hello DTO (data transfer objekt) hade en heltalskolumn prioritet och sedan kan du lägga till det här fältet och dess get- och set-metoder:</span><span class="sxs-lookup"><span data-stu-id="ce701-165">For example, if hello DTO (data transfer object) had an integer Priority column, then you might add this field, along with its getter and setter methods:</span></span>

```java
private Integer priority;

/**
* Returns hello item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets hello item priority
*
* @param priority
*            priority tooset
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

<span data-ttu-id="ce701-166">toolearn hur toocreate ytterligare tabeller i Mobile Apps-serverdel Se [så här: definiera en tabell styrenhet] [ 15] (.NET-serverdel) eller [definiera tabeller med en dynamisk Schema] [ 16] (Node.js-serverdel).</span><span class="sxs-lookup"><span data-stu-id="ce701-166">toolearn how toocreate additional tables in your Mobile Apps backend, see [How to: Define a table controller][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js backend).</span></span>

<span data-ttu-id="ce701-167">En Azure Mobile Apps serverdel tabell definierar fem särskilda fält fyra som är tillgängliga tooclients:</span><span class="sxs-lookup"><span data-stu-id="ce701-167">An Azure Mobile Apps backend table defines five special fields, four of which are available tooclients:</span></span>

* <span data-ttu-id="ce701-168">`String id`: hello globalt unikt ID för hello-post.</span><span class="sxs-lookup"><span data-stu-id="ce701-168">`String id`: hello globally unique ID for hello record.</span></span>  <span data-ttu-id="ce701-169">Som bästa praxis, se hello id hello strängrepresentation av en [UUID] [ 17] objekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-169">As a best practice, make hello id hello String representation of a [UUID][17] object.</span></span>
* <span data-ttu-id="ce701-170">`DateTimeOffset updatedAt`: hello datum/tid för senaste uppdatering av hello.</span><span class="sxs-lookup"><span data-stu-id="ce701-170">`DateTimeOffset updatedAt`: hello date/time of hello last update.</span></span>  <span data-ttu-id="ce701-171">Hej updatedAt fältet anges av hello-servern och aldrig ställas in av din klientkod.</span><span class="sxs-lookup"><span data-stu-id="ce701-171">hello updatedAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="ce701-172">`DateTimeOffset createdAt`: hello tidsvärdet hello objektet skapades.</span><span class="sxs-lookup"><span data-stu-id="ce701-172">`DateTimeOffset createdAt`: hello date/time that hello object was created.</span></span>  <span data-ttu-id="ce701-173">Hej createdAt fältet anges av hello-servern och aldrig ställas in av din klientkod.</span><span class="sxs-lookup"><span data-stu-id="ce701-173">hello createdAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="ce701-174">`byte[] version`: Hello-version ställs normalt representeras som en sträng, också in av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="ce701-174">`byte[] version`: Normally represented as a string, hello version is also set by hello server.</span></span>
* <span data-ttu-id="ce701-175">`boolean deleted`: Anger att hello post har tagits bort men inte bort ännu.</span><span class="sxs-lookup"><span data-stu-id="ce701-175">`boolean deleted`: Indicates that hello record has been deleted but not purged yet.</span></span>  <span data-ttu-id="ce701-176">Använd inte `deleted` som en egenskap i klassen.</span><span class="sxs-lookup"><span data-stu-id="ce701-176">Do not use `deleted` as a property in your class.</span></span>

<span data-ttu-id="ce701-177">Hej `id` fältet är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="ce701-177">hello `id` field is required.</span></span>  <span data-ttu-id="ce701-178">Hej `updatedAt` fält och `version` används för offlinesynkronisering (för matchning av inkrementell synkronisering och konflikt respektive).</span><span class="sxs-lookup"><span data-stu-id="ce701-178">hello `updatedAt` field and `version` field are used for offline synchronization (for incremental sync and conflict resolution respectively).</span></span>  <span data-ttu-id="ce701-179">Hej `createdAt` fältet är en referensfält och används inte av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="ce701-179">hello `createdAt` field is a reference field and is not used by hello client.</span></span>  <span data-ttu-id="ce701-180">hello namn är ”över överföring” namnen på hello egenskaper och är inte ställas in.</span><span class="sxs-lookup"><span data-stu-id="ce701-180">hello names are "across-the-wire" names of hello properties and are not adjustable.</span></span>  <span data-ttu-id="ce701-181">Du kan dock skapa en mappning mellan objekt och hello ”över överföring” namn genom att använda hello [gson] [ 3] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ce701-181">However, you can create a mapping between your object and hello "across-the-wire" names using hello [gson][3] library.</span></span>  <span data-ttu-id="ce701-182">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ce701-182">For example:</span></span>

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getText() { return mVersion; }
    public final void setText(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a><span data-ttu-id="ce701-183">Skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="ce701-183">Create a Table Reference</span></span>

<span data-ttu-id="ce701-184">tooaccess en tabell, först skapa en [MobileServiceTable] [ 8] objekt genom att anropa hello **getTable** metod på hello [MobileServiceClient][9].</span><span class="sxs-lookup"><span data-stu-id="ce701-184">tooaccess a table, first create a [MobileServiceTable][8] object by calling hello **getTable** method on hello [MobileServiceClient][9].</span></span>  <span data-ttu-id="ce701-185">Den här metoden har två överlagringar:</span><span class="sxs-lookup"><span data-stu-id="ce701-185">This method has two overloads:</span></span>

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

<span data-ttu-id="ce701-186">I följande kod, hello **mClient** är ett referensobjekt tooyour MobileServiceClient.</span><span class="sxs-lookup"><span data-stu-id="ce701-186">In hello following code, **mClient** is a reference tooyour MobileServiceClient object.</span></span>  <span data-ttu-id="ce701-187">hello första överlagring används där hello klassnamn och hello tabellnamnet är hello samma och hello en används i hello Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="ce701-187">hello first overload is used where hello class name and hello table name are hello same, and is hello one used in hello Quickstart:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

<span data-ttu-id="ce701-188">hello andra överlagring används när hello tabellnamn skiljer sig från hello klassnamn: hello första parametern är hello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="ce701-188">hello second overload is used when hello table name is different from hello class name: hello first parameter is hello table name.</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <span data-ttu-id="ce701-189"><a name="query"></a>Fråga en Backend-tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-189"><a name="query"></a>Query a Backend Table</span></span>

<span data-ttu-id="ce701-190">Skaffa först en tabellreferens.</span><span class="sxs-lookup"><span data-stu-id="ce701-190">First, obtain a table reference.</span></span>  <span data-ttu-id="ce701-191">Kör en fråga på hello tabellreferens.</span><span class="sxs-lookup"><span data-stu-id="ce701-191">Then execute a query on hello table reference.</span></span>  <span data-ttu-id="ce701-192">En fråga är en kombination av:</span><span class="sxs-lookup"><span data-stu-id="ce701-192">A query is any combination of:</span></span>

* <span data-ttu-id="ce701-193">En `.where()` [filtersats](#filtering).</span><span class="sxs-lookup"><span data-stu-id="ce701-193">A `.where()` [filter clause](#filtering).</span></span>
* <span data-ttu-id="ce701-194">En `.orderBy()` [ordning satsen](#sorting).</span><span class="sxs-lookup"><span data-stu-id="ce701-194">An `.orderBy()` [ordering clause](#sorting).</span></span>
* <span data-ttu-id="ce701-195">En `.select()` [fältet markeringen satsen](#selection).</span><span class="sxs-lookup"><span data-stu-id="ce701-195">A `.select()` [field selection clause](#selection).</span></span>
* <span data-ttu-id="ce701-196">En `.skip()` och `.top()` för [växlingsbart systemminne resultat](#paging).</span><span class="sxs-lookup"><span data-stu-id="ce701-196">A `.skip()` and `.top()` for [paged results](#paging).</span></span>

<span data-ttu-id="ce701-197">hello-satser måste vara angiven i hello föregående ordning.</span><span class="sxs-lookup"><span data-stu-id="ce701-197">hello clauses must be presented in hello preceding order.</span></span>

### <span data-ttu-id="ce701-198"><a name="filter"></a>Filtrerar resultaten</span><span class="sxs-lookup"><span data-stu-id="ce701-198"><a name="filter"></a> Filtering Results</span></span>

<span data-ttu-id="ce701-199">hello allmänna form av en fråga är:</span><span class="sxs-lookup"><span data-stu-id="ce701-199">hello general form of a query is:</span></span>

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

<span data-ttu-id="ce701-200">hello returnerar föregående exempel alla resultat (upp toohello största storlek som hello server).</span><span class="sxs-lookup"><span data-stu-id="ce701-200">hello preceding example returns all results (up toohello maximum page size set by hello server).</span></span>  <span data-ttu-id="ce701-201">Hej `.execute()` metoden Kör hello fråga på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-201">hello `.execute()` method executes hello query on hello backend.</span></span>  <span data-ttu-id="ce701-202">hello frågan är konverterade tooan [OData v3] [ 19] fråga innan överföring toohello Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-202">hello query is converted tooan [OData v3][19] query before transmission toohello Mobile Apps backend.</span></span>  <span data-ttu-id="ce701-203">Erhåller konverterar hello Mobile Apps-serverdel hello frågan till en SQL-instruktion innan den körs på hello SQL Azure-instans.</span><span class="sxs-lookup"><span data-stu-id="ce701-203">On receipt, hello Mobile Apps backend converts hello query into an SQL statement before executing it on hello SQL Azure instance.</span></span>  <span data-ttu-id="ce701-204">Eftersom nätverksaktivitet tar en stund, hello `.execute()` metoden returnerar en [ `ListenableFuture<E>` ] [ 18].</span><span class="sxs-lookup"><span data-stu-id="ce701-204">Since network activity takes some time, hello `.execute()` method returns a [`ListenableFuture<E>`][18].</span></span>

### <span data-ttu-id="ce701-205"><a name="filtering"></a>Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="ce701-205"><a name="filtering"></a>Filter returned data</span></span>

<span data-ttu-id="ce701-206">hello efter körning av fråga returnerar alla objekt från hello **ToDoItem** tabellen var **fullständig** är lika med **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="ce701-206">hello following query execution returns all items from hello **ToDoItem** table where **complete** equals **false**.</span></span>

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

<span data-ttu-id="ce701-207">**mToDoTable** är hello toohello Mobiltjänst referenstabellen som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ce701-207">**mToDoTable** is hello reference toohello mobile service table that we created previously.</span></span>

<span data-ttu-id="ce701-208">Definiera ett filter som använder hello **där** metodanrop på hello tabellreferens.</span><span class="sxs-lookup"><span data-stu-id="ce701-208">Define a filter using hello **where** method call on hello table reference.</span></span> <span data-ttu-id="ce701-209">Hej **där** metoden följs av en **fältet** metoden följt av en metod som anger hello logiska predikat.</span><span class="sxs-lookup"><span data-stu-id="ce701-209">hello **where** method is followed by a **field** method followed by a method that specifies hello logical predicate.</span></span> <span data-ttu-id="ce701-210">Möjliga metoder kan predikat **eq** (motsvarar) **ne** (inte lika med), **gt** (större än), **ge** (större än eller lika med), **lt** (minst), **le** (mindre än eller lika med).</span><span class="sxs-lookup"><span data-stu-id="ce701-210">Possible predicate methods include **eq** (equals), **ne** (not equal), **gt** (greater than), **ge** (greater than or equal to), **lt** (less than), **le** (less than or equal to).</span></span> <span data-ttu-id="ce701-211">Dessa metoder kan du jämföra antalet och strängen toospecific värdena i fälten.</span><span class="sxs-lookup"><span data-stu-id="ce701-211">These methods let you compare number and string fields toospecific values.</span></span>

<span data-ttu-id="ce701-212">Du kan filtrera efter datum.</span><span class="sxs-lookup"><span data-stu-id="ce701-212">You can filter on dates.</span></span> <span data-ttu-id="ce701-213">hello följande metoder kan du jämföra hello datumfält för hela eller delar av hello datum: **år**, **månad**, **dag**, **timme**, **minut**, och **andra**.</span><span class="sxs-lookup"><span data-stu-id="ce701-213">hello following methods let you compare hello entire date field or parts of hello date: **year**, **month**, **day**, **hour**, **minute**, and **second**.</span></span> <span data-ttu-id="ce701-214">hello följande exempel lägger till ett filter för objekt vars *förfallodatum* är lika med 2013.</span><span class="sxs-lookup"><span data-stu-id="ce701-214">hello following example adds a filter for items whose *due date* equals 2013.</span></span>

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

<span data-ttu-id="ce701-215">hello följande metoder stöder komplexa filter på strängfält: **startsWith**, **endsWith**, **concat**, **delsträngen**, **indexOf**, **ersätta**, **toLower**, **toUpper**, **trim**, och ** längden**.</span><span class="sxs-lookup"><span data-stu-id="ce701-215">hello following methods support complex filters on string fields: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, and **length**.</span></span> <span data-ttu-id="ce701-216">följande exempel filter för tabellen rader där hello hello *text* kolumnen som börjar med ”PRI0”.</span><span class="sxs-lookup"><span data-stu-id="ce701-216">hello following example filters for table rows where hello *text* column starts with "PRI0."</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="ce701-217">hello följande metoder för operatorn stöds på fälten: **lägga till**, **sub**, **mul**, **div**, **mod**, **våning**, **tak**, och **avrunda**.</span><span class="sxs-lookup"><span data-stu-id="ce701-217">hello following operator methods are supported on number fields: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**, and **round**.</span></span> <span data-ttu-id="ce701-218">följande exempel filter för tabellen rader där hello hello **varaktighet** är ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="ce701-218">hello following example filters for table rows where hello **duration** is an even number.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

<span data-ttu-id="ce701-219">Du kan kombinera predikat med metoderna logiska: **och**, **eller** och **inte**.</span><span class="sxs-lookup"><span data-stu-id="ce701-219">You can combine predicates with these logical methods: **and**, **or** and **not**.</span></span> <span data-ttu-id="ce701-220">följande exempel hello kombinerar två hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="ce701-220">hello following example combines two of hello preceding examples.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="ce701-221">Gruppera och kapsla logiska operatorer:</span><span class="sxs-lookup"><span data-stu-id="ce701-221">Group and nest logical operators:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

<span data-ttu-id="ce701-222">Mer detaljerad information och exempel på filtrering finns [utforska hello informationen hello Android klienten frågemodell][20].</span><span class="sxs-lookup"><span data-stu-id="ce701-222">For more detailed discussion and examples of filtering, see [Exploring hello richness of hello Android client query model][20].</span></span>

### <span data-ttu-id="ce701-223"><a name="sorting"></a>Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="ce701-223"><a name="sorting"></a>Sort returned data</span></span>

<span data-ttu-id="ce701-224">hello följande kod returnerar alla objekt från en tabell med **ToDoItems** Sortera stigande efter hello *text* fältet.</span><span class="sxs-lookup"><span data-stu-id="ce701-224">hello following code returns all items from a table of **ToDoItems** sorted ascending by hello *text* field.</span></span> <span data-ttu-id="ce701-225">*mToDoTable* är hello toohello backend referenstabellen som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="ce701-225">*mToDoTable* is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

<span data-ttu-id="ce701-226">Hej första parametern för hello **orderBy** metoden är en sträng lika toohello namnet på vilka toosort hello fält.</span><span class="sxs-lookup"><span data-stu-id="ce701-226">hello first parameter of hello **orderBy** method is a string equal toohello name of hello field on which toosort.</span></span> <span data-ttu-id="ce701-227">hello andra parametern använder hello **QueryOrder** uppräkningen toospecify om toosort stigande eller fallande.</span><span class="sxs-lookup"><span data-stu-id="ce701-227">hello second parameter uses hello **QueryOrder** enumeration toospecify whether toosort ascending or descending.</span></span>  <span data-ttu-id="ce701-228">Om du filtrerar med hello ***där*** metod, hello ***där*** metod måste anropas innan hello ***orderBy*** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-228">If you are filtering using hello ***where*** method, hello ***where*** method must be invoked before hello ***orderBy*** method.</span></span>

### <span data-ttu-id="ce701-229"><a name="selection"></a>Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="ce701-229"><a name="selection"></a>Select specific columns</span></span>

<span data-ttu-id="ce701-230">hello följande kod visar hur tooreturn alla objekt från en tabell med **ToDoItems**, men bara visar hello **fullständig** och **text** fält.</span><span class="sxs-lookup"><span data-stu-id="ce701-230">hello following code illustrates how tooreturn all items from a table of **ToDoItems**, but only displays hello **complete** and **text** fields.</span></span> <span data-ttu-id="ce701-231">**mToDoTable** är hello toohello backend referenstabellen som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ce701-231">**mToDoTable** is hello reference toohello backend table that we created previously.</span></span>

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

<span data-ttu-id="ce701-232">hello parametrar toohello väljer funktionen är hello sträng namnen på hello tabellens kolumner som du vill tooreturn.</span><span class="sxs-lookup"><span data-stu-id="ce701-232">hello parameters toohello select function are hello string names of hello table's columns that you want tooreturn.</span></span>  <span data-ttu-id="ce701-233">Hej **Välj** metod måste toofollow metoder som **där** och **orderBy**.</span><span class="sxs-lookup"><span data-stu-id="ce701-233">hello **select** method needs toofollow methods like **where** and **orderBy**.</span></span> <span data-ttu-id="ce701-234">Det kan följas av sidindelning metoder som **hoppa över** och **upp**.</span><span class="sxs-lookup"><span data-stu-id="ce701-234">It can be followed by paging methods like **skip** and **top**.</span></span>

### <span data-ttu-id="ce701-235"><a name="paging"></a>Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="ce701-235"><a name="paging"></a>Return data in pages</span></span>

<span data-ttu-id="ce701-236">Data är **alltid** returneras i sidor.</span><span class="sxs-lookup"><span data-stu-id="ce701-236">Data is **ALWAYS** returned in pages.</span></span>  <span data-ttu-id="ce701-237">hello högsta antal poster returneras har angetts av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="ce701-237">hello maximum number of records returned is set by hello server.</span></span>  <span data-ttu-id="ce701-238">Om hello klient begär fler poster, returnerar hello servern hello högsta antal poster.</span><span class="sxs-lookup"><span data-stu-id="ce701-238">If hello client requests more records, then hello server returns hello maximum number of records.</span></span>  <span data-ttu-id="ce701-239">Hej maximala sidstorleken på hello server är 50 poster som standard.</span><span class="sxs-lookup"><span data-stu-id="ce701-239">By default, hello maximum page size on hello server is 50 records.</span></span>

<span data-ttu-id="ce701-240">hello första exemplet visar hur tooselect hello översta fem posterna från en tabell.</span><span class="sxs-lookup"><span data-stu-id="ce701-240">hello first example shows how tooselect hello top five items from a table.</span></span> <span data-ttu-id="ce701-241">hello frågan returnerar hello objekt från en tabell med **ToDoItems**.</span><span class="sxs-lookup"><span data-stu-id="ce701-241">hello query returns hello items from a table of **ToDoItems**.</span></span> <span data-ttu-id="ce701-242">**mToDoTable** är hello toohello backend referenstabellen som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="ce701-242">**mToDoTable** is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

<span data-ttu-id="ce701-243">Här är en fråga som hoppar över hello fem första objekten och sedan returnerar hello nästa fem:</span><span class="sxs-lookup"><span data-stu-id="ce701-243">Here's a query that skips hello first five items, and then returns hello next five:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

<span data-ttu-id="ce701-244">Om du vill tooget alla poster i en tabell kan du implementera kod tooiterate över alla sidor:</span><span class="sxs-lookup"><span data-stu-id="ce701-244">If you wish tooget all records in a table, implement code tooiterate over all pages:</span></span>

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

<span data-ttu-id="ce701-245">En begäran om alla poster med den här metoden skapar minst två begäranden toohello Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-245">A request for all records using this method creates a minimum of two requests toohello Mobile Apps backend.</span></span>

> [!TIP]
> <span data-ttu-id="ce701-246">Att välja hello rätt sidstorleken är en avvägning mellan minnesanvändning när hello begäran pågår, bandbreddsanvändning och fördröjning i hello data togs emot helt.</span><span class="sxs-lookup"><span data-stu-id="ce701-246">Choosing hello right page size is a balance between memory usage while hello request is happening, bandwidth usage and delay in receiving hello data completely.</span></span>  <span data-ttu-id="ce701-247">hello standard (50 poster) är lämplig för alla enheter.</span><span class="sxs-lookup"><span data-stu-id="ce701-247">hello default (50 records) is suitable for all devices.</span></span>  <span data-ttu-id="ce701-248">Om du använder uteslutande på större minnesenheter, öka upp too500.</span><span class="sxs-lookup"><span data-stu-id="ce701-248">If you exclusively operate on larger memory devices, increase up too500.</span></span>  <span data-ttu-id="ce701-249">Vi har hittat den ökande hello sidstorleken utöver 500 poster resultat i oacceptabel fördröjningar och stora minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="ce701-249">We have found that increasing hello page size beyond 500 records results in unacceptable delays and large memory issues.</span></span>

### <span data-ttu-id="ce701-250"><a name="chaining"></a>Så här: sammanfoga frågan metoder</span><span class="sxs-lookup"><span data-stu-id="ce701-250"><a name="chaining"></a>How to: Concatenate query methods</span></span>

<span data-ttu-id="ce701-251">hello-metoder som används i frågor till backend-tabeller kan sammanfogas.</span><span class="sxs-lookup"><span data-stu-id="ce701-251">hello methods used in querying backend tables can be concatenated.</span></span> <span data-ttu-id="ce701-252">Länkning frågan metoder kan du tooselect specifika kolumner filtrerade rader som sorteras och växlingsbart systemminne.</span><span class="sxs-lookup"><span data-stu-id="ce701-252">Chaining query methods allows you tooselect specific columns of filtered rows that are sorted and paged.</span></span> <span data-ttu-id="ce701-253">Du kan skapa komplexa logiska filter.</span><span class="sxs-lookup"><span data-stu-id="ce701-253">You can create complex logical filters.</span></span>  <span data-ttu-id="ce701-254">Varje fråga-metoden returnerar ett frågeobjekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-254">Each query method returns a Query object.</span></span> <span data-ttu-id="ce701-255">tooend hello serie metoder och faktiskt kör hello frågan, anropet hello **köra** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-255">tooend hello series of methods and actually run hello query, call hello **execute** method.</span></span> <span data-ttu-id="ce701-256">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ce701-256">For example:</span></span>

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

<span data-ttu-id="ce701-257">Hej sammankedjade frågan metoder måste ordnas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ce701-257">hello chained query methods must be ordered as follows:</span></span>

1. <span data-ttu-id="ce701-258">Filtrering (**där**) metoder.</span><span class="sxs-lookup"><span data-stu-id="ce701-258">Filtering (**where**) methods.</span></span>
2. <span data-ttu-id="ce701-259">Sortering (**orderBy**) metoder.</span><span class="sxs-lookup"><span data-stu-id="ce701-259">Sorting (**orderBy**) methods.</span></span>
3. <span data-ttu-id="ce701-260">Markeringen (**Välj**) metoder.</span><span class="sxs-lookup"><span data-stu-id="ce701-260">Selection (**select**) methods.</span></span>
4. <span data-ttu-id="ce701-261">växling (**hoppa över** och **upp**) metoder.</span><span class="sxs-lookup"><span data-stu-id="ce701-261">paging (**skip** and **top**) methods.</span></span>

## <span data-ttu-id="ce701-262"><a name="binding"></a>Binda data toohello användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="ce701-262"><a name="binding"></a>Bind data toohello user interface</span></span>

<span data-ttu-id="ce701-263">Databindning omfattar tre komponenter:</span><span class="sxs-lookup"><span data-stu-id="ce701-263">Data binding involves three components:</span></span>

* <span data-ttu-id="ce701-264">hello-datakälla</span><span class="sxs-lookup"><span data-stu-id="ce701-264">hello data source</span></span>
* <span data-ttu-id="ce701-265">hello skärmlayout</span><span class="sxs-lookup"><span data-stu-id="ce701-265">hello screen layout</span></span>
* <span data-ttu-id="ce701-266">hello kort att ties hello två tillsammans.</span><span class="sxs-lookup"><span data-stu-id="ce701-266">hello adapter that ties hello two together.</span></span>

<span data-ttu-id="ce701-267">I vårt exempelkod returnerar vi hello data från hello Mobile Apps SQL Azure table **ToDoItem** i en matris.</span><span class="sxs-lookup"><span data-stu-id="ce701-267">In our sample code, we return hello data from hello Mobile Apps SQL Azure table **ToDoItem** into an array.</span></span> <span data-ttu-id="ce701-268">Den här aktiviteten är ett vanligt mönster för program.</span><span class="sxs-lookup"><span data-stu-id="ce701-268">This activity is a common pattern for data applications.</span></span>  <span data-ttu-id="ce701-269">Databasfrågor returnera ofta en mängd rader som hello hämtar klienten i en lista eller en matris.</span><span class="sxs-lookup"><span data-stu-id="ce701-269">Database queries often return a collection of rows that hello client gets in a list or array.</span></span> <span data-ttu-id="ce701-270">I det här exemplet är hello matris hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="ce701-270">In this sample, hello array is hello data source.</span></span>  <span data-ttu-id="ce701-271">hello-kod anger skärmlayout som definierar hello vy över hello data som visas på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="ce701-271">hello code specifies a screen layout that defines hello view of hello data that appears on hello device.</span></span>  <span data-ttu-id="ce701-272">hello två är bundna tillsammans med en kort, som i den här koden är en utökning av hello **ArrayAdapter&lt;ToDoItem&gt; ** klass.</span><span class="sxs-lookup"><span data-stu-id="ce701-272">hello two are bound together with an adapter, which in this code is an extension of hello **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span>

#### <span data-ttu-id="ce701-273"><a name="layout"></a>Definiera hello Layout</span><span class="sxs-lookup"><span data-stu-id="ce701-273"><a name="layout"></a>Define hello Layout</span></span>

<span data-ttu-id="ce701-274">hello layout definieras av flera kodavsnitt för XML-koden.</span><span class="sxs-lookup"><span data-stu-id="ce701-274">hello layout is defined by several snippets of XML code.</span></span> <span data-ttu-id="ce701-275">En befintlig layout får hello följande kod representerar hello **ListView** vi vill toopopulate med våra serverdata.</span><span class="sxs-lookup"><span data-stu-id="ce701-275">Given an existing layout, hello following code represents hello **ListView** we want toopopulate with our server data.</span></span>

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

<span data-ttu-id="ce701-276">I föregående kod hello, hello *listitem* attribut anger hello-id för hello layout för en enskild rad i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="ce701-276">In hello preceding code, hello *listitem* attribute specifies hello id of hello layout for an individual row in hello list.</span></span> <span data-ttu-id="ce701-277">Den här koden anger en kryssruta och tillhörande text och hämtar instansieras en gång för varje objekt i listan hello.</span><span class="sxs-lookup"><span data-stu-id="ce701-277">This code specifies a check box and its associated text and gets instantiated once for each item in hello list.</span></span> <span data-ttu-id="ce701-278">Den här layouten visas inte hello **id** fältet och en mer komplicerad layout anger ytterligare fält i hello visas.</span><span class="sxs-lookup"><span data-stu-id="ce701-278">This layout does not display hello **id** field, and a more complex layout would specify additional fields in hello display.</span></span> <span data-ttu-id="ce701-279">Den här koden är i hello **row_list_to_do.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="ce701-279">This code is in hello **row_list_to_do.xml** file.</span></span>

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <span data-ttu-id="ce701-280"><a name="adapter"></a>Definiera hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="ce701-280"><a name="adapter"></a>Define hello adapter</span></span>
<span data-ttu-id="ce701-281">Eftersom hello datakällan i vår vyn är en matris med **ToDoItem**, vi underklass våra kort från en **ArrayAdapter&lt;ToDoItem&gt; ** klass.</span><span class="sxs-lookup"><span data-stu-id="ce701-281">Since hello data source of our view is an array of **ToDoItem**, we subclass our adapter from an **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span> <span data-ttu-id="ce701-282">Den här underklass producerar en vy för varje **ToDoItem** med hello **row_list_to_do** layout.</span><span class="sxs-lookup"><span data-stu-id="ce701-282">This subclass produces a View for every **ToDoItem** using hello **row_list_to_do** layout.</span></span>  <span data-ttu-id="ce701-283">I vår kod vi definiera hello efter klass som är en utökning av hello **ArrayAdapter&lt;E&gt; ** klass:</span><span class="sxs-lookup"><span data-stu-id="ce701-283">In our code, we define hello following class that is an extension of hello **ArrayAdapter&lt;E&gt;** class:</span></span>

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

<span data-ttu-id="ce701-284">Åsidosätt hello kort **getView** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-284">Override hello adapters **getView** method.</span></span> <span data-ttu-id="ce701-285">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ce701-285">For example:</span></span>

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

<span data-ttu-id="ce701-286">Vi skapa en instans av den här klassen i vår aktiviteten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ce701-286">We create an instance of this class in our Activity as follows:</span></span>

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

<span data-ttu-id="ce701-287">hello andra parameter toohello ToDoItemAdapter konstruktorn är en referens toohello layout.</span><span class="sxs-lookup"><span data-stu-id="ce701-287">hello second parameter toohello ToDoItemAdapter constructor is a reference toohello layout.</span></span> <span data-ttu-id="ce701-288">Vi kan nu skapa en instans av hello **ListView** och tilldela hello kortet toohello **ListView**.</span><span class="sxs-lookup"><span data-stu-id="ce701-288">We can now instantiate hello **ListView** and assign hello adapter toohello **ListView**.</span></span>

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <span data-ttu-id="ce701-289"><a name="use-adapter"></a>Använd hello kortet tooBind toohello UI</span><span class="sxs-lookup"><span data-stu-id="ce701-289"><a name="use-adapter"></a>Use hello Adapter tooBind toohello UI</span></span>

<span data-ttu-id="ce701-290">Du är nu redo toouse databindning.</span><span class="sxs-lookup"><span data-stu-id="ce701-290">You are now ready toouse data binding.</span></span> <span data-ttu-id="ce701-291">hello följande kod visar hur tooget objekt i hello tabell och fyllning hello lokala kortet med hello returnerade objekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-291">hello following code shows how tooget items in hello table and fills hello local adapter with hello returned items.</span></span>

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

<span data-ttu-id="ce701-292">Anropa hello kortet när du ändrar hello **ToDoItem** tabell.</span><span class="sxs-lookup"><span data-stu-id="ce701-292">Call hello adapter any time you modify hello **ToDoItem** table.</span></span> <span data-ttu-id="ce701-293">Eftersom ändringar görs på en post med basis, kan du hantera en enskild rad i stället för en samling.</span><span class="sxs-lookup"><span data-stu-id="ce701-293">Since modifications are done on a record by record basis, you handle a single row instead of a collection.</span></span> <span data-ttu-id="ce701-294">När du infogar ett objekt kan anropa hello **lägga till** metod på hello kortet; när du tar bort, anropa hello **ta bort** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-294">When you insert an item, call hello **add** method on hello adapter; when deleting, call hello **remove** method.</span></span>

<span data-ttu-id="ce701-295">Du hittar ett komplett exempel i hello [Android Snabbstartsprojekt][21].</span><span class="sxs-lookup"><span data-stu-id="ce701-295">You can find a complete example in hello [Android Quickstart Project][21].</span></span>

## <span data-ttu-id="ce701-296"><a name="inserting"></a>Infoga data i hello backend</span><span class="sxs-lookup"><span data-stu-id="ce701-296"><a name="inserting"></a>Insert data into hello backend</span></span>

<span data-ttu-id="ce701-297">Skapa en instans av en instans av hello *ToDoItem* klassen och ange dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ce701-297">Instantiate an instance of hello *ToDoItem* class and set its properties.</span></span>

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

<span data-ttu-id="ce701-298">Använd sedan **insert()** tooinsert ett objekt:</span><span class="sxs-lookup"><span data-stu-id="ce701-298">Then use **insert()** tooinsert an object:</span></span>

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="ce701-299">hello returnerade entitet matchar hello data infogas i hello backend tabell, inkluderade hello-ID och andra värden (till exempel hello `createdAt`, `updatedAt`, och `version` fält) inställd på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-299">hello returned entity matches hello data inserted into hello backend table, included hello ID and any other values (such as hello `createdAt`, `updatedAt`, and `version` fields) set on hello backend.</span></span>

<span data-ttu-id="ce701-300">Mobile Apps tabeller kräver en primärnyckelkolumn med namnet **id**. Den här kolumnen måste vara en sträng.</span><span class="sxs-lookup"><span data-stu-id="ce701-300">Mobile Apps tables require a primary key column named **id**. This column must be a string.</span></span> <span data-ttu-id="ce701-301">hello standardvärdet hello ID-kolumnen är ett GUID.</span><span class="sxs-lookup"><span data-stu-id="ce701-301">hello default value of hello ID column is a GUID.</span></span>  <span data-ttu-id="ce701-302">Du kan ange andra unika värden, till exempel e-postadresser eller användarnamn.</span><span class="sxs-lookup"><span data-stu-id="ce701-302">You can provide other unique values, such as email addresses or usernames.</span></span> <span data-ttu-id="ce701-303">När ett sträng-ID-värde har angetts för en infogad post, genererar hello backend en ny GUID.</span><span class="sxs-lookup"><span data-stu-id="ce701-303">When a string ID value is not provided for an inserted record, hello backend generates a new GUID.</span></span>

<span data-ttu-id="ce701-304">Strängvärden ID innehåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ce701-304">String ID values provide hello following advantages:</span></span>

* <span data-ttu-id="ce701-305">ID: N kan skapas utan att göra en onödig kommunikation toohello databas.</span><span class="sxs-lookup"><span data-stu-id="ce701-305">IDs can be generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="ce701-306">Posterna är enklare toomerge från olika tabeller eller databaser.</span><span class="sxs-lookup"><span data-stu-id="ce701-306">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="ce701-307">ID-värden integrera bättre med en programlogik.</span><span class="sxs-lookup"><span data-stu-id="ce701-307">ID values integrate better with an application's logic.</span></span>

<span data-ttu-id="ce701-308">Sträng-ID-värden är **REQUIRED** för offlinesynkronisering support.</span><span class="sxs-lookup"><span data-stu-id="ce701-308">String ID values are **REQUIRED** for offline sync support.</span></span>  <span data-ttu-id="ce701-309">Du kan inte ändra ett Id när den är lagrad i hello backend-databas.</span><span class="sxs-lookup"><span data-stu-id="ce701-309">You cannot change an Id once it is stored in hello backend database.</span></span>

## <span data-ttu-id="ce701-310"><a name="updating"></a>Uppdatera data i en mobil app</span><span class="sxs-lookup"><span data-stu-id="ce701-310"><a name="updating"></a>Update data in a mobile app</span></span>

<span data-ttu-id="ce701-311">tooupdate data i en tabell, skicka hello nya objekt toohello **update()** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-311">tooupdate data in a table, pass hello new object toohello **update()** method.</span></span>

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="ce701-312">I det här exemplet *objektet* är en referens tooa rad i hello *ToDoItem* tabellen som har haft tooit vissa ändringar som gjorts.</span><span class="sxs-lookup"><span data-stu-id="ce701-312">In this example, *item* is a reference tooa row in hello *ToDoItem* table, which has had some changes made tooit.</span></span>  <span data-ttu-id="ce701-313">hello rad med hello samma **id** uppdateras.</span><span class="sxs-lookup"><span data-stu-id="ce701-313">hello row with hello same **id** is updated.</span></span>

## <span data-ttu-id="ce701-314"><a name="deleting"></a>Ta bort data i en mobil app</span><span class="sxs-lookup"><span data-stu-id="ce701-314"><a name="deleting"></a>Delete data in a mobile app</span></span>

<span data-ttu-id="ce701-315">hello följande kod visar hur toodelete data från en tabell genom att ange hello-dataobjektet.</span><span class="sxs-lookup"><span data-stu-id="ce701-315">hello following code shows how toodelete data from a table by specifying hello data object.</span></span>

```java
mToDoTable
    .delete(item);
```

<span data-ttu-id="ce701-316">Du kan också ta bort ett objekt genom att ange hello **id** i hello raden toodelete.</span><span class="sxs-lookup"><span data-stu-id="ce701-316">You can also delete an item by specifying hello **id** field of hello row toodelete.</span></span>

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <span data-ttu-id="ce701-317"><a name="lookup"></a>Leta upp ett specifikt objekt-ID: t</span><span class="sxs-lookup"><span data-stu-id="ce701-317"><a name="lookup"></a>Look up a specific item by Id</span></span>

<span data-ttu-id="ce701-318">Leta upp ett objekt med en specifik **id** med hello **LETAUPP()** metod:</span><span class="sxs-lookup"><span data-stu-id="ce701-318">Look up an item with a specific **id** field with hello **lookUp()** method:</span></span>

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <span data-ttu-id="ce701-319"><a name="untyped"></a>Så här: arbeta med någon data</span><span class="sxs-lookup"><span data-stu-id="ce701-319"><a name="untyped"></a>How to: Work with untyped data</span></span>

<span data-ttu-id="ce701-320">hello ej typbestämd programmeringsmodell ger exakt kontroll över JSON-serialisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-320">hello untyped programming model gives you exact control over JSON serialization.</span></span>  <span data-ttu-id="ce701-321">Det finns några vanliga scenarier där du toouse en ej typbestämd programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="ce701-321">There are some common scenarios where you may wish toouse an untyped programming model.</span></span> <span data-ttu-id="ce701-322">Till exempel om backend-tabellen innehåller många kolumner och behöver du bara tooreference en delmängd av hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="ce701-322">For example, if your backend table contains many columns and you only need tooreference a subset of hello columns.</span></span>  <span data-ttu-id="ce701-323">hello skrivna model kräver toodefine alla hello kolumner har definierats i hello Mobile Apps-serverdel i dataklass.</span><span class="sxs-lookup"><span data-stu-id="ce701-323">hello typed model requires you toodefine all hello columns defined in hello Mobile Apps backend in your data class.</span></span>  <span data-ttu-id="ce701-324">De flesta hello API-anrop för att få åtkomst till data är liknande toohello skrev API-anrop.</span><span class="sxs-lookup"><span data-stu-id="ce701-324">Most of hello API calls for accessing data are similar toohello typed programming calls.</span></span> <span data-ttu-id="ce701-325">hello största skillnaden är att i hello någon modell du anropa metoder i hello **MobileServiceJsonTable** -objektet, hello **MobileServiceTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-325">hello main difference is that in hello untyped model you invoke methods on hello **MobileServiceJsonTable** object, instead of hello **MobileServiceTable** object.</span></span>

### <span data-ttu-id="ce701-326"><a name="json_instance"></a>Skapa en instans av en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-326"><a name="json_instance"></a>Create an instance of an untyped table</span></span>

<span data-ttu-id="ce701-327">Liknande toohello skrev modellen, du börja med att hämta en tabellreferens, men i det här fallet är det en **MobileServicesJsonTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="ce701-327">Similar toohello typed model, you start by getting a table reference, but in this case it's a **MobileServicesJsonTable** object.</span></span> <span data-ttu-id="ce701-328">Hämta hello referens genom att anropa hello **getTable** metod i en instans av hello klienten:</span><span class="sxs-lookup"><span data-stu-id="ce701-328">Obtain hello reference by calling hello **getTable** method on an instance of hello client:</span></span>

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

<span data-ttu-id="ce701-329">När du har skapat en instans av hello **MobileServiceJsonTable**, den har praktiskt taget hello samma API som är tillgängliga som med hello skrivna programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="ce701-329">Once you have created an instance of hello **MobileServiceJsonTable**, it has virtually hello same API available as with hello typed programming model.</span></span> <span data-ttu-id="ce701-330">I vissa fall kan ta en utan angiven parameter i stället för en typifierad parameter i hello metoder.</span><span class="sxs-lookup"><span data-stu-id="ce701-330">In some cases, hello methods take an untyped parameter instead of a typed parameter.</span></span>

### <span data-ttu-id="ce701-331"><a name="json_insert"></a>Infoga i en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-331"><a name="json_insert"></a>Insert into an untyped table</span></span>
<span data-ttu-id="ce701-332">Hej följande kod visar hur toodo insert.</span><span class="sxs-lookup"><span data-stu-id="ce701-332">hello following code shows how toodo an insert.</span></span> <span data-ttu-id="ce701-333">hello första steget är toocreate en [JsonObject][1], vilket är en del av hello [gson] [ 3] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ce701-333">hello first step is toocreate a [JsonObject][1], which is part of hello [gson][3] library.</span></span>

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

<span data-ttu-id="ce701-334">Använd sedan **insert()** tooinsert hello ej typbestämd objekt i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="ce701-334">Then, Use **insert()** tooinsert hello untyped object into hello table.</span></span>

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

<span data-ttu-id="ce701-335">Om du behöver tooget hello-ID för hello infogat objekt kan använda hello **getAsJsonPrimitive()** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-335">If you need tooget hello ID of hello inserted object, use hello **getAsJsonPrimitive()** method.</span></span>

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <span data-ttu-id="ce701-336"><a name="json_delete"></a>Ta bort från en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-336"><a name="json_delete"></a>Delete from an untyped table</span></span>
<span data-ttu-id="ce701-337">hello följande kod visar hur toodelete en instans, i det här fallet hello samma instans av en **JsonObject** som har skapats i hello före *infoga* exempel.</span><span class="sxs-lookup"><span data-stu-id="ce701-337">hello following code shows how toodelete an instance, in this case, hello same instance of a **JsonObject** that was created in hello prior *insert* example.</span></span> <span data-ttu-id="ce701-338">hello koden är hello samma sätt som om hello skrivit skiftläge, men hello-metoden har en annan signatur eftersom den refererar till en **JsonObject**.</span><span class="sxs-lookup"><span data-stu-id="ce701-338">hello code is hello same as with hello typed case, but hello method has a different signature since it references an **JsonObject**.</span></span>

```java
mToDoTable
    .delete(insertedItem);
```

<span data-ttu-id="ce701-339">Du kan också ta bort en instans direkt med ID:</span><span class="sxs-lookup"><span data-stu-id="ce701-339">You can also delete an instance directly by using its ID:</span></span>

```java
mToDoTable.delete(ID);
```

### <span data-ttu-id="ce701-340"><a name="json_get"></a>Returnera alla rader från en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-340"><a name="json_get"></a>Return all rows from an untyped table</span></span>
<span data-ttu-id="ce701-341">Hej följande kod visar hur tooretrieve hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="ce701-341">hello following code shows how tooretrieve an entire table.</span></span> <span data-ttu-id="ce701-342">Eftersom du använder en JSON-tabellen kan hämta du selektivt endast en del av hello tabellkolumner.</span><span class="sxs-lookup"><span data-stu-id="ce701-342">Since you are using a JSON Table, you can selectively retrieve only some of hello table's columns.</span></span>

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

<span data-ttu-id="ce701-343">hello samma uppsättning filtrering, filtrering och växling metoder som är tillgängliga för hello skrivna modellen är tillgängliga för hello någon modell.</span><span class="sxs-lookup"><span data-stu-id="ce701-343">hello same set of filtering, filtering and paging methods that are available for hello typed model are available for hello untyped model.</span></span>

## <span data-ttu-id="ce701-344"><a name="offline-sync"></a>Implementera offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="ce701-344"><a name="offline-sync"></a>Implement Offline Sync</span></span>

<span data-ttu-id="ce701-345">hello Azure Mobile Apps klient-SDK implementerar också offlinesynkronisering av data med hjälp av en SQLite-databas toostore en kopia av data från hello lokalt.</span><span class="sxs-lookup"><span data-stu-id="ce701-345">hello Azure Mobile Apps Client SDK also implements offline synchronization of data by using a SQLite database toostore a copy of hello server data locally.</span></span>  <span data-ttu-id="ce701-346">Åtgärder som utförs på en offline-tabell kräver inte mobil anslutning toowork.</span><span class="sxs-lookup"><span data-stu-id="ce701-346">Operations performed on an offline table do not require mobile connectivity toowork.</span></span>  <span data-ttu-id="ce701-347">Offlinesynkronisering aids återhämtning och prestanda på bekostnad av hello av mer komplex logik för konfliktlösning.</span><span class="sxs-lookup"><span data-stu-id="ce701-347">Offline sync aids in resilience and performance at hello expense of more complex logic for conflict resolution.</span></span>  <span data-ttu-id="ce701-348">hello Azure Mobile Apps klient-SDK implementerar hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="ce701-348">hello Azure Mobile Apps Client SDK implements hello following features:</span></span>

* <span data-ttu-id="ce701-349">Inkrementell synkronisering: Endast uppdaterade och nya poster har hämtats, spara bandbredd och minne.</span><span class="sxs-lookup"><span data-stu-id="ce701-349">Incremental Sync: Only updated and new records are downloaded, saving bandwidth and memory consumption.</span></span>
* <span data-ttu-id="ce701-350">Optimistisk samtidighet: Operations antas toosucceed.</span><span class="sxs-lookup"><span data-stu-id="ce701-350">Optimistic Concurrency: Operations are assumed toosucceed.</span></span>  <span data-ttu-id="ce701-351">Konfliktlösning skjuts upp tills uppdateringar utförs på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="ce701-351">Conflict Resolution is deferred until updates are performed on hello server.</span></span>
* <span data-ttu-id="ce701-352">Konfliktlösning: hello SDK identifierar när en motstridig ändring har gjorts på hello server och ger skapar tooalert hello användare.</span><span class="sxs-lookup"><span data-stu-id="ce701-352">Conflict Resolution: hello SDK detects when a conflicting change has been made at hello server and provides hooks tooalert hello user.</span></span>
* <span data-ttu-id="ce701-353">Mjuk borttagning: Borttagna poster är markerade tagits bort, så att andra enheter tooupdate sina offline cache.</span><span class="sxs-lookup"><span data-stu-id="ce701-353">Soft Delete: Deleted records are marked deleted, allowing other devices tooupdate their offline cache.</span></span>

### <a name="initialize-offline-sync"></a><span data-ttu-id="ce701-354">Initiera synkronisering Offline</span><span class="sxs-lookup"><span data-stu-id="ce701-354">Initialize Offline Sync</span></span>

<span data-ttu-id="ce701-355">Varje tabell som är offline måste definieras i hello offline cachen innan de används.</span><span class="sxs-lookup"><span data-stu-id="ce701-355">Each offline table must be defined in hello offline cache before use.</span></span>  <span data-ttu-id="ce701-356">Normalt görs tabelldefinitionen omedelbart efter hello generering av hello klienten:</span><span class="sxs-lookup"><span data-stu-id="ce701-356">Normally, table definition is done immediately after hello creation of hello client:</span></span>

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with hello model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define hello table in hello local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize hello local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-toohello-offline-cache-table"></a><span data-ttu-id="ce701-357">Hämta en referens toohello Offline cachelagrad tabell</span><span class="sxs-lookup"><span data-stu-id="ce701-357">Obtain a reference toohello Offline Cache Table</span></span>

<span data-ttu-id="ce701-358">Ett online tabell kan du använda `.getTable()`.</span><span class="sxs-lookup"><span data-stu-id="ce701-358">For an online table, you use `.getTable()`.</span></span>  <span data-ttu-id="ce701-359">För en offline-tabell, använder du `.getSyncTable()`:</span><span class="sxs-lookup"><span data-stu-id="ce701-359">For an offline table, use `.getSyncTable()`:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

<span data-ttu-id="ce701-360">Alla hello metoder som är tillgängliga för online-tabeller (inklusive filtrering, sortering, växling, infoga data, uppdatera data och ta bort data) fungerar lika bra på tabellerna online och offline.</span><span class="sxs-lookup"><span data-stu-id="ce701-360">All hello methods that are available for online tables (including filtering, sorting, paging, inserting data, updating data, and deleting data) work equally well on online and offline tables.</span></span>

### <a name="synchronize-hello-local-offline-cache"></a><span data-ttu-id="ce701-361">Synkronisera hello Offline lokalt cacheminne</span><span class="sxs-lookup"><span data-stu-id="ce701-361">Synchronize hello Local Offline Cache</span></span>

<span data-ttu-id="ce701-362">Synkronisering är inom hello kontroll över din app.</span><span class="sxs-lookup"><span data-stu-id="ce701-362">Synchronization is within hello control of your app.</span></span>  <span data-ttu-id="ce701-363">Här är ett exempel synkroniseringsmetoden:</span><span class="sxs-lookup"><span data-stu-id="ce701-363">Here is an example synchronization method:</span></span>

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

<span data-ttu-id="ce701-364">Om det finns ett namn på frågan toohello `.pull(query, queryname)` metod och inkrementell synkronisering är används tooreturn endast poster som har skapats eller ändrats sedan pull hello som senast har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ce701-364">If a query name is provided toohello `.pull(query, queryname)` method, then incremental sync is used tooreturn only records that have been created or changed since hello last successfully completed pull.</span></span>

### <a name="handle-conflicts-during-offline-synchronization"></a><span data-ttu-id="ce701-365">Hantera konflikter under offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="ce701-365">Handle Conflicts during Offline Synchronization</span></span>

<span data-ttu-id="ce701-366">Om en konflikt uppstår under en `.push()` åtgärd, en `MobileServiceConflictException` genereras.</span><span class="sxs-lookup"><span data-stu-id="ce701-366">If a conflict happens during a `.push()` operation, a `MobileServiceConflictException` is thrown.</span></span>   <span data-ttu-id="ce701-367">Hej server-utfärdade objektet är inbäddat i hello undantag och kan hämtas av `.getItem()` på hello undantag.</span><span class="sxs-lookup"><span data-stu-id="ce701-367">hello server-issued item is embedded in hello exception and can be retrieved by `.getItem()` on hello exception.</span></span>  <span data-ttu-id="ce701-368">Justera hello push genom att anropa hello följande objekt på hello MobileServiceSyncContext objekt:</span><span class="sxs-lookup"><span data-stu-id="ce701-368">Adjust hello push by calling hello following items on hello MobileServiceSyncContext object:</span></span>

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

<span data-ttu-id="ce701-369">När alla konflikter markeras som du vill, anropa `.push()` igen tooresolve alla hello konflikter.</span><span class="sxs-lookup"><span data-stu-id="ce701-369">Once all conflicts are marked as you wish, call `.push()` again tooresolve all hello conflicts.</span></span>

## <span data-ttu-id="ce701-370"><a name="custom-api"></a>Anropa anpassade API</span><span class="sxs-lookup"><span data-stu-id="ce701-370"><a name="custom-api"></a>Call a custom API</span></span>

<span data-ttu-id="ce701-371">En anpassad API kan du toodefine anpassade slutpunkter som exponerar serverfunktioner som inte mappa tooan infoga, uppdatera, ta bort eller Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="ce701-371">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="ce701-372">Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.</span><span class="sxs-lookup"><span data-stu-id="ce701-372">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="ce701-373">I en Android-klient du anropa hello **invokeApi** metod-toocall hello anpassade API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ce701-373">From an Android client, you call hello **invokeApi** method toocall hello custom API endpoint.</span></span> <span data-ttu-id="ce701-374">hello följande exempel visas hur toocall en API-slutpunkt med namnet **completeAll**, som returnerar en samlingsklass som heter **MarkAllResult**.</span><span class="sxs-lookup"><span data-stu-id="ce701-374">hello following example shows how toocall an API endpoint named **completeAll**, which returns a collection class named **MarkAllResult**.</span></span>

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

<span data-ttu-id="ce701-375">Hej **invokeApi** metoden anropas för hello-klient som skickar en POST-begäran toohello nya anpassade API.</span><span class="sxs-lookup"><span data-stu-id="ce701-375">hello **invokeApi** method is called on hello client, which sends a POST request toohello new custom API.</span></span> <span data-ttu-id="ce701-376">hello resultatet som returneras av hello anpassade API visas i en dialogruta för meddelande som fel.</span><span class="sxs-lookup"><span data-stu-id="ce701-376">hello result returned by hello custom API is displayed in a message dialog, as are any errors.</span></span> <span data-ttu-id="ce701-377">Andra versioner av **invokeApi** kan du om du vill skicka ett objekt i hello begärantext, ange hello HTTP-metod och skicka frågeparametrar Hej förfrågan.</span><span class="sxs-lookup"><span data-stu-id="ce701-377">Other versions of **invokeApi** let you optionally send an object in hello request body, specify hello HTTP method, and send query parameters with hello request.</span></span> <span data-ttu-id="ce701-378">Utan angiven versioner av **invokeApi** tillhandahålls också.</span><span class="sxs-lookup"><span data-stu-id="ce701-378">Untyped versions of **invokeApi** are provided as well.</span></span>

## <span data-ttu-id="ce701-379"><a name="authentication"></a>Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="ce701-379"><a name="authentication"></a>Add authentication tooyour app</span></span>

<span data-ttu-id="ce701-380">Självstudiekurser redan beskrivs i detalj hur tooadd dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="ce701-380">Tutorials already describe in detail how tooadd these features.</span></span>

<span data-ttu-id="ce701-381">Har stöd för Apptjänst [autentisera användarna](app-service-mobile-android-get-started-users.md) med olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ce701-381">App Service supports [authenticating app users](app-service-mobile-android-get-started-users.md) using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="ce701-382">Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="ce701-382">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="ce701-383">Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i din serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-383">You can also use hello identity of authenticated users tooimplement authorization rules in your backend.</span></span>

<span data-ttu-id="ce701-384">Två autentisering flöden stöds: en **server** flöde och en **klienten** flöde.</span><span class="sxs-lookup"><span data-stu-id="ce701-384">Two authentication flows are supported: a **server** flow and a **client** flow.</span></span> <span data-ttu-id="ce701-385">hello server flödet ger hello enklaste autentiseringsupplevelse som använder webbgränssnitt för hello identitets-providers.</span><span class="sxs-lookup"><span data-stu-id="ce701-385">hello server flow provides hello simplest authentication experience, as it relies on hello identity providers web interface.</span></span>  <span data-ttu-id="ce701-386">Inga ytterligare SDK: er krävs tooimplement flödet för serverautentisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-386">No additional SDKs are required tooimplement server flow authentication.</span></span> <span data-ttu-id="ce701-387">Flöde för serverautentisering ger inte djupgående integrering i hello mobil enhet och rekommenderas endast för bevis på koncept scenarier.</span><span class="sxs-lookup"><span data-stu-id="ce701-387">Server flow authentication does not provide a deep integration into hello mobile device and is only recommended for proof of concept scenarios.</span></span>

<span data-ttu-id="ce701-388">hello flödet tillåter djupare integrering med specifika funktioner, till exempel enkel inloggning som använder SDK: er som tillhandahålls av hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="ce701-388">hello client flow allows for deeper integration with device-specific capabilities such as single sign-on as it relies on SDKs provided by hello identity provider.</span></span>  <span data-ttu-id="ce701-389">Exempelvis kan du integrera hello Facebook SDK i din mobila program.</span><span class="sxs-lookup"><span data-stu-id="ce701-389">For example, you can integrate hello Facebook SDK into your mobile application.</span></span>  <span data-ttu-id="ce701-390">hello mobila klienten växlingar i hello Facebook-app och bekräftar din inloggning innan du växlar tillbaka tooyour mobila appar.</span><span class="sxs-lookup"><span data-stu-id="ce701-390">hello mobile client swaps into hello Facebook app and confirms your sign-on before swapping back tooyour mobile app.</span></span>

<span data-ttu-id="ce701-391">Fyra steg är nödvändiga tooenable autentisering i appen:</span><span class="sxs-lookup"><span data-stu-id="ce701-391">Four steps are required tooenable authentication in your app:</span></span>

* <span data-ttu-id="ce701-392">Registrera din app för autentisering med en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="ce701-392">Register your app for authentication with an identity provider.</span></span>
* <span data-ttu-id="ce701-393">Konfigurera din App Service-serverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-393">Configure your App Service backend.</span></span>
* <span data-ttu-id="ce701-394">Begränsa tabellen behörighet tooauthenticated användarna bara på hello Apptjänst backend.</span><span class="sxs-lookup"><span data-stu-id="ce701-394">Restrict table permissions tooauthenticated users only on hello App Service backend.</span></span>
* <span data-ttu-id="ce701-395">Lägg till autentisering kod tooyour app.</span><span class="sxs-lookup"><span data-stu-id="ce701-395">Add authentication code tooyour app.</span></span>

<span data-ttu-id="ce701-396">Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="ce701-396">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="ce701-397">Du kan också använda hello SID för en autentiserad användare toomodify begäranden.</span><span class="sxs-lookup"><span data-stu-id="ce701-397">You can also use hello SID of an authenticated user toomodify requests.</span></span>  <span data-ttu-id="ce701-398">Mer information hittar [komma igång med autentisering] och hello servern ta för SDK-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="ce701-398">For more information, review [Get started with authentication] and hello Server SDK HOWTO documentation.</span></span>

### <span data-ttu-id="ce701-399"><a name="caching"></a>: Server Autentiseringsflödet</span><span class="sxs-lookup"><span data-stu-id="ce701-399"><a name="caching"></a>Authentication: Server Flow</span></span>

<span data-ttu-id="ce701-400">hello startar följande kod en server flödet inloggningen med hello Google-providern.</span><span class="sxs-lookup"><span data-stu-id="ce701-400">hello following code starts a server flow login process using hello Google provider.</span></span>  <span data-ttu-id="ce701-401">Ytterligare konfiguration krävs på grund av hello säkerhetskrav för hello Google-providern:</span><span class="sxs-lookup"><span data-stu-id="ce701-401">Additional configuration is required because of hello security requirements for hello Google provider:</span></span>

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

<span data-ttu-id="ce701-402">Dessutom lägga till hello följa metoden toohello huvudsakliga Activity-klassen:</span><span class="sxs-lookup"><span data-stu-id="ce701-402">In addition, add hello following method toohello main Activity class:</span></span>

```java
// You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check hello request code matches hello one we send in hello login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check hello error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

<span data-ttu-id="ce701-403">Hej `GOOGLE_LOGIN_REQUEST_CODE` definierats i din huvudsakliga aktivitet används för hello `login()` metod och inom hello `onActivityResult()` metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-403">hello `GOOGLE_LOGIN_REQUEST_CODE` defined in your main Activity is used for hello `login()` method and within hello `onActivityResult()` method.</span></span>  <span data-ttu-id="ce701-404">Du kan välja alla unikt nummer som hello samma nummer används i hello `login()` metod och hello `onActivityResult()` metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-404">You can choose any unique number, as long as hello same number is used within hello `login()` method and hello `onActivityResult()` method.</span></span>  <span data-ttu-id="ce701-405">Om du abstrakt hello klientkod till ett service-kort (som visas tidigare) bör du anropa hello lämpliga metoder på hello service nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="ce701-405">If you abstract hello client code into a service adapter (as shown earlier), you should call hello appropriate methods on hello service adapter.</span></span>

<span data-ttu-id="ce701-406">Du måste också tooconfigure hello projekt för customtabs.</span><span class="sxs-lookup"><span data-stu-id="ce701-406">You also need tooconfigure hello project for customtabs.</span></span>  <span data-ttu-id="ce701-407">Ange en omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="ce701-407">First specify a redirect-URL.</span></span>  <span data-ttu-id="ce701-408">Lägg till följande fragment för hello`AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="ce701-408">Add hello following snippet too`AndroidManifest.xml`:</span></span>

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

<span data-ttu-id="ce701-409">Lägg till hello **redirectUriScheme** toohello `build.gradle` filen för tillämpningsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="ce701-409">Add hello **redirectUriScheme** toohello `build.gradle` file for your application:</span></span>

```text
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

<span data-ttu-id="ce701-410">Slutligen lägger du till `com.android.support:customtabs:23.0.1` toohello programberoenden i hello `build.gradle` fil:</span><span class="sxs-lookup"><span data-stu-id="ce701-410">Finally, add `com.android.support:customtabs:23.0.1` toohello dependencies list in hello `build.gradle` file:</span></span>

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.2.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

<span data-ttu-id="ce701-411">Hämta hello-ID för hello inloggade användare från en **MobileServiceUser** med hello **getUserId** metod.</span><span class="sxs-lookup"><span data-stu-id="ce701-411">Obtain hello ID of hello logged-in user from a **MobileServiceUser** using hello **getUserId** method.</span></span> <span data-ttu-id="ce701-412">Ett exempel på hur toouse Futures toocall hello asynkron inloggningen API: er, se [komma igång med autentisering].</span><span class="sxs-lookup"><span data-stu-id="ce701-412">For an example of how toouse Futures toocall hello asynchronous login APIs, see [Get started with authentication].</span></span>

> [!WARNING]
> <span data-ttu-id="ce701-413">hello URL-schema som nämns är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="ce701-413">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="ce701-414">Se till att alla förekomster av `{url_scheme_of_you_app}` gemener/versaler.</span><span class="sxs-lookup"><span data-stu-id="ce701-414">Ensure that all occurrences of `{url_scheme_of_you_app}` match case.</span></span>

### <span data-ttu-id="ce701-415"><a name="caching"></a>Cache-autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="ce701-415"><a name="caching"></a>Cache authentication tokens</span></span>

<span data-ttu-id="ce701-416">Cachelagring autentiseringstoken måste du toostore hello användar-ID och autentiseringstoken lokalt på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="ce701-416">Caching authentication tokens requires you toostore hello User ID and authentication token locally on hello device.</span></span> <span data-ttu-id="ce701-417">hello hello appen startas nästa gång du kontrollera hello cache och om dessa värden finns kan du hoppa över hello logg i proceduren och rehydrate hello klienten med dessa data.</span><span class="sxs-lookup"><span data-stu-id="ce701-417">hello next time hello app starts, you check hello cache, and if these values are present, you can skip hello log in procedure and rehydrate hello client with this data.</span></span> <span data-ttu-id="ce701-418">Men dessa data är känsligt och lagras krypterad för säkerhet om hello phone hämtar blir stulen.</span><span class="sxs-lookup"><span data-stu-id="ce701-418">However this data is sensitive, and it should be stored encrypted for safety in case hello phone gets stolen.</span></span>  <span data-ttu-id="ce701-419">Du kan se en komplett exempel på hur toocache autentisering token i [cachelagra autentisering tokenavsnittet][7].</span><span class="sxs-lookup"><span data-stu-id="ce701-419">You can see a complete example of how toocache authentication tokens in [Cache authentication tokens section][7].</span></span>

<span data-ttu-id="ce701-420">När du försöker toouse en token som har upphört att gälla, får du en *401-Ej behörig* svar.</span><span class="sxs-lookup"><span data-stu-id="ce701-420">When you try toouse an expired token, you receive a *401 unauthorized* response.</span></span> <span data-ttu-id="ce701-421">Du kan hantera med hjälp av filter-autentiseringsfel.</span><span class="sxs-lookup"><span data-stu-id="ce701-421">You can handle authentication errors using filters.</span></span>  <span data-ttu-id="ce701-422">Filter hantera begäranden toohello Apptjänst backend.</span><span class="sxs-lookup"><span data-stu-id="ce701-422">Filters intercept requests toohello App Service backend.</span></span> <span data-ttu-id="ce701-423">hello filter code testar hello-svar för en 401, utlöser hello inloggningsprocessen och återupptar sedan hello-begäran som genererade hello 401.</span><span class="sxs-lookup"><span data-stu-id="ce701-423">hello filter code tests hello response for a 401, triggers hello sign-in process, and then resumes hello request that generated hello 401.</span></span>

### <span data-ttu-id="ce701-424"><a name="refresh"></a>Använd Uppdateringstoken</span><span class="sxs-lookup"><span data-stu-id="ce701-424"><a name="refresh"></a>Use Refresh Tokens</span></span>

<span data-ttu-id="ce701-425">hello-token som returnerades av Azure App Service-autentisering och auktorisering har en definierad livslängden för en timme.</span><span class="sxs-lookup"><span data-stu-id="ce701-425">hello token returned by Azure App Service Authentication and Authorization has a defined life time of one hour.</span></span>  <span data-ttu-id="ce701-426">Du måste autentiseras hello användare för denna period.</span><span class="sxs-lookup"><span data-stu-id="ce701-426">After this period, you must reauthenticate hello user.</span></span>  <span data-ttu-id="ce701-427">Om du hello använder en långlivade token som du har fått med klient-flöde för autentisering och du kan autentiseras med Azure App Service-autentisering och auktorisering med hjälp av samma token.</span><span class="sxs-lookup"><span data-stu-id="ce701-427">If you are using a long-lived token that you have received via client-flow authentication, then you can reauthenticate with Azure App Service Authentication and Authorization using hello same token.</span></span>  <span data-ttu-id="ce701-428">En annan Azure App Service-token genereras med nya livslängden.</span><span class="sxs-lookup"><span data-stu-id="ce701-428">Another Azure App Service token is generated with a new lifetime.</span></span>

<span data-ttu-id="ce701-429">Du kan också registrera hello providern toouse uppdatera token.</span><span class="sxs-lookup"><span data-stu-id="ce701-429">You can also register hello provider toouse Refresh Tokens.</span></span>  <span data-ttu-id="ce701-430">En uppdatera Token är inte alltid tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="ce701-430">A Refresh Token is not always available.</span></span>  <span data-ttu-id="ce701-431">Det krävs ytterligare konfiguration:</span><span class="sxs-lookup"><span data-stu-id="ce701-431">Additional configuration is required:</span></span>

* <span data-ttu-id="ce701-432">För **Azure Active Directory**, konfigurera en klienthemlighet för hello Azure Active Directory-App.</span><span class="sxs-lookup"><span data-stu-id="ce701-432">For **Azure Active Directory**, configure a client secret for hello Azure Active Directory App.</span></span>  <span data-ttu-id="ce701-433">Ange hello klienthemlighet i hello Azure App Service när du konfigurerar Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-433">Specify hello client secret in hello Azure App Service when configuring Azure Active Directory Authentication.</span></span>  <span data-ttu-id="ce701-434">När du anropar `.login()`, skicka `response_type=code id_token` som en parameter:</span><span class="sxs-lookup"><span data-stu-id="ce701-434">When calling `.login()`, pass `response_type=code id_token` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="ce701-435">För **Google**, skicka hello `access_type=offline` som en parameter:</span><span class="sxs-lookup"><span data-stu-id="ce701-435">For **Google**, pass hello `access_type=offline` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="ce701-436">För **Account**väljer hello `wl.offline_access` omfång.</span><span class="sxs-lookup"><span data-stu-id="ce701-436">For **Microsoft Account**, select hello `wl.offline_access` scope.</span></span>

<span data-ttu-id="ce701-437">toorefresh en token ring `.refreshUser()`:</span><span class="sxs-lookup"><span data-stu-id="ce701-437">toorefresh a token, call `.refreshUser()`:</span></span>

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

<span data-ttu-id="ce701-438">Ett bra tips är att skapa ett filter som identifierar ett 401 svar från hello server och försöker toorefresh hello användar-token.</span><span class="sxs-lookup"><span data-stu-id="ce701-438">As a best practice, create a filter that detects a 401 response from hello server and tries toorefresh hello user token.</span></span>

## <a name="log-in-with-client-flow-authentication"></a><span data-ttu-id="ce701-439">Logga in med klient-flöde för autentisering</span><span class="sxs-lookup"><span data-stu-id="ce701-439">Log in with Client-flow Authentication</span></span>

<span data-ttu-id="ce701-440">hello allmänna processen för att logga in med klient-flöde för autentisering är följande:</span><span class="sxs-lookup"><span data-stu-id="ce701-440">hello general process for logging in with client-flow authentication is as follows:</span></span>

* <span data-ttu-id="ce701-441">Konfigurera Azure App Service-autentisering och auktorisering som server-flöde för autentisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-441">Configure Azure App Service Authentication and Authorization as you would server-flow authentication.</span></span>
* <span data-ttu-id="ce701-442">Integrera hello autentiseringsprovider SDK för autentisering tooproduce en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="ce701-442">Integrate hello authentication provider SDK for authentication tooproduce an access token.</span></span>
* <span data-ttu-id="ce701-443">Anropa hello `.login()` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ce701-443">Call hello `.login()` method as follows:</span></span>

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

<span data-ttu-id="ce701-444">Ersätt hello `onSuccess()` metod med oavsett kod som du vill att toouse på en genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="ce701-444">Replace hello `onSuccess()` method with whatever code you wish toouse on a successful login.</span></span>  <span data-ttu-id="ce701-445">Hej `{provider}` strängen är en giltig provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, eller **twitter**.</span><span class="sxs-lookup"><span data-stu-id="ce701-445">hello `{provider}` string is a valid provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, or **twitter**.</span></span>  <span data-ttu-id="ce701-446">Om du har implementerat anpassad autentisering, kan du också använda hello anpassad autentisering providern taggen.</span><span class="sxs-lookup"><span data-stu-id="ce701-446">If you have implemented custom authentication, then you can also use hello custom authentication provider tag.</span></span>

### <span data-ttu-id="ce701-447"><a name="adal"></a>Autentisera användare med hello Active Directory Authentication Library (ADAL)</span><span class="sxs-lookup"><span data-stu-id="ce701-447"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library (ADAL)</span></span>

<span data-ttu-id="ce701-448">Du kan använda hello Active Directory Authentication Library (ADAL) toosign användare i ditt program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ce701-448">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="ce701-449">Med hjälp av en klient flödet för inloggning är ofta bättre toousing hello `loginAsync()` metoder som ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="ce701-449">Using a client flow login is often preferable toousing hello `loginAsync()` methods as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="ce701-450">Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] [ 22] kursen.</span><span class="sxs-lookup"><span data-stu-id="ce701-450">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][22] tutorial.</span></span> <span data-ttu-id="ce701-451">Se till att toocomplete hello valfritt steg för att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="ce701-451">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="ce701-452">Installera ADAL genom att ändra din build.gradle filen tooinclude hello följande definitioner:</span><span class="sxs-lookup"><span data-stu-id="ce701-452">Install ADAL by modifying your build.gradle file tooinclude hello following definitions:</span></span>

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. <span data-ttu-id="ce701-453">Lägg till hello följande kod tooyour program, vilket gör följande ersättningar hello:</span><span class="sxs-lookup"><span data-stu-id="ce701-453">Add hello following code tooyour application, making hello following replacements:</span></span>

* <span data-ttu-id="ce701-454">Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce701-454">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="ce701-455">hello format bör vara https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="ce701-455">hello format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span>
* <span data-ttu-id="ce701-456">Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="ce701-456">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="ce701-457">Du kan hämta hello klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ce701-457">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
* <span data-ttu-id="ce701-458">Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.</span><span class="sxs-lookup"><span data-stu-id="ce701-458">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
* <span data-ttu-id="ce701-459">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="ce701-459">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="ce701-460">Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="ce701-460">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <span data-ttu-id="ce701-461"><a name="filters"></a>Justera hello klient-/ serverkommunikation</span><span class="sxs-lookup"><span data-stu-id="ce701-461"><a name="filters"></a>Adjust hello Client-Server Communication</span></span>

<span data-ttu-id="ce701-462">hello klientanslutning är vanligtvis en grundläggande HTTP-anslutning med hello underliggande HTTP-biblioteket som medföljer hello Android SDK.</span><span class="sxs-lookup"><span data-stu-id="ce701-462">hello Client connection is normally a basic HTTP connection using hello underlying HTTP library supplied with hello Android SDK.</span></span>  <span data-ttu-id="ce701-463">Det finns flera orsaker till varför du vill ha toochange som:</span><span class="sxs-lookup"><span data-stu-id="ce701-463">There are several reasons why you would want toochange that:</span></span>

* <span data-ttu-id="ce701-464">Vill du toouse en alternativ tooadjust tidsgränser för HTTP-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ce701-464">You wish toouse an alternate HTTP library tooadjust timeouts.</span></span>
* <span data-ttu-id="ce701-465">Vill du tooprovide en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="ce701-465">You wish tooprovide a progress bar.</span></span>
* <span data-ttu-id="ce701-466">Vill du tooadd en anpassad rubrik toosupport API management-funktioner.</span><span class="sxs-lookup"><span data-stu-id="ce701-466">You wish tooadd a custom header toosupport API management functionality.</span></span>
* <span data-ttu-id="ce701-467">Vill du toointercept misslyckade svar så att du kan implementera omautentisering.</span><span class="sxs-lookup"><span data-stu-id="ce701-467">You wish toointercept a failed response so that you can implement reauthentication.</span></span>
* <span data-ttu-id="ce701-468">Vill du toolog backend begäranden tooan analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ce701-468">You wish toolog backend requests tooan analytics service.</span></span>

### <a name="using-an-alternate-http-library"></a><span data-ttu-id="ce701-469">Med hjälp av en annan HTTP-bibliotek</span><span class="sxs-lookup"><span data-stu-id="ce701-469">Using an alternate HTTP Library</span></span>

<span data-ttu-id="ce701-470">Anropa hello `.setAndroidHttpClientFactory()` metoden omedelbart när du har skapat din klient-referens.</span><span class="sxs-lookup"><span data-stu-id="ce701-470">Call hello `.setAndroidHttpClientFactory()` method immediately after creating your client reference.</span></span>  <span data-ttu-id="ce701-471">Till exempel tooset hello anslutning timeout too60 sekunder (i stället för hello standard 10 sekunder):</span><span class="sxs-lookup"><span data-stu-id="ce701-471">For example, tooset hello connection timeout too60 seconds (instead of hello default 10 seconds):</span></span>

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a><span data-ttu-id="ce701-472">Implementera ett Filter för pågår</span><span class="sxs-lookup"><span data-stu-id="ce701-472">Implement a Progress Filter</span></span>

<span data-ttu-id="ce701-473">Du kan implementera en skärningspunkt för varje begäran genom att implementera en `ServiceFilter`.</span><span class="sxs-lookup"><span data-stu-id="ce701-473">You can implement an intercept of every request by implementing a `ServiceFilter`.</span></span>  <span data-ttu-id="ce701-474">Hello följande uppdateringar till exempel en förskapad förloppsindikator:</span><span class="sxs-lookup"><span data-stu-id="ce701-474">For example, hello following updates a pre-created progress bar:</span></span>

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    pubic void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

<span data-ttu-id="ce701-475">Du kan koppla filter toohello klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ce701-475">You can attach this filter toohello client as follows:</span></span>

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a><span data-ttu-id="ce701-476">Anpassa huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="ce701-476">Customize Request Headers</span></span>

<span data-ttu-id="ce701-477">Använder följande hello `ServiceFilter` och bifoga hello filter i hello samma sätt som hello `ProgressFilter`:</span><span class="sxs-lookup"><span data-stu-id="ce701-477">Use hello following `ServiceFilter` and attach hello filter in hello same way as hello `ProgressFilter`:</span></span>

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <span data-ttu-id="ce701-478"><a name="conversions"></a>Konfigurera automatisk serialisering</span><span class="sxs-lookup"><span data-stu-id="ce701-478"><a name="conversions"></a>Configure Automatic Serialization</span></span>

<span data-ttu-id="ce701-479">Du kan ange en konvertering strategi som gäller tooevery kolumn med hjälp av hello [gson] [ 3] API.</span><span class="sxs-lookup"><span data-stu-id="ce701-479">You can specify a conversion strategy that applies tooevery column by using hello [gson][3] API.</span></span> <span data-ttu-id="ce701-480">hello Android klientbiblioteket använder [gson] [ 3] bakgrunden hello tooserialize Java objekt tooJSON data innan hello data skickas tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="ce701-480">hello Android client library uses [gson][3] behind hello scenes tooserialize Java objects tooJSON data before hello data is sent tooAzure App Service.</span></span>  <span data-ttu-id="ce701-481">hello följande kod använder hello **setFieldNamingStrategy()** metoden tooset hello strategi.</span><span class="sxs-lookup"><span data-stu-id="ce701-481">hello following code uses hello **setFieldNamingStrategy()** method tooset hello strategy.</span></span> <span data-ttu-id="ce701-482">Det här exemplet tar bort hello första tecken (”m”) och sedan gemena hello nästa tecken för varje fältnamn.</span><span class="sxs-lookup"><span data-stu-id="ce701-482">This example will delete hello initial character (an "m"), and then lower-case hello next character, for every field name.</span></span> <span data-ttu-id="ce701-483">Till exempel skulle den göra ”mId” till ”id”.</span><span class="sxs-lookup"><span data-stu-id="ce701-483">For example, it would turn "mId" into "id."</span></span>  <span data-ttu-id="ce701-484">Implementera en konvertering strategi tooreduce hello behöver för `SerializedName()` anteckningar i de flesta fält.</span><span class="sxs-lookup"><span data-stu-id="ce701-484">Implement a conversion strategy tooreduce hello need for `SerializedName()` annotations on most fields.</span></span>

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStategy)
);
```

<span data-ttu-id="ce701-485">Den här koden måste köras innan du skapar en mobil klient-referens med hello **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="ce701-485">This code must be executed before creating a mobile client reference using hello **MobileServiceClient**.</span></span>

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Komma igång med autentisering]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
