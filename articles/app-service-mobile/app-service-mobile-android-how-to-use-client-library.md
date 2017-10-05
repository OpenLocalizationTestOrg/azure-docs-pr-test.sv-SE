---
title: "Hur du använder Azure Mobile Apps-SDK för Android | Microsoft Docs"
description: "Hur du använder Azure Mobile Apps-SDK för Android"
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
ms.openlocfilehash: 4b15d024ca6d5bbafe83d321a64021aecd78c4a8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a><span data-ttu-id="78dff-103">Hur du använder Azure Mobile Apps-SDK för Android</span><span class="sxs-lookup"><span data-stu-id="78dff-103">How to use the Azure Mobile Apps SDK for Android</span></span>

<span data-ttu-id="78dff-104">Den här guiden visar hur du använder Android klient-SDK för Mobile Apps för att implementera vanliga scenarier som:</span><span class="sxs-lookup"><span data-stu-id="78dff-104">This guide shows you how to use the Android client SDK for Mobile Apps to implement common scenarios, such as:</span></span>

* <span data-ttu-id="78dff-105">Frågar efter data (Infoga, uppdatera och ta bort).</span><span class="sxs-lookup"><span data-stu-id="78dff-105">Querying for data (inserting, updating, and deleting).</span></span>
* <span data-ttu-id="78dff-106">Autentisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-106">Authentication.</span></span>
* <span data-ttu-id="78dff-107">Hantera fel.</span><span class="sxs-lookup"><span data-stu-id="78dff-107">Handling errors.</span></span>
* <span data-ttu-id="78dff-108">Anpassning av klienten.</span><span class="sxs-lookup"><span data-stu-id="78dff-108">Customizing the client.</span></span>

<span data-ttu-id="78dff-109">Den här guiden fokuserar på klientsidan Android SDK.</span><span class="sxs-lookup"><span data-stu-id="78dff-109">This guide focuses on the client-side Android SDK.</span></span>  <span data-ttu-id="78dff-110">Läs mer om serversidan-SDK: er för Mobile Apps i [arbeta med .NET-serverdel SDK] [ 10] eller [hur du använder SDK för Node.js-serverdel][11].</span><span class="sxs-lookup"><span data-stu-id="78dff-110">To learn more about the server-side SDKs for Mobile Apps, see [Work with .NET backend SDK][10] or [How to use the Node.js backend SDK][11].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="78dff-111">Referensdokumentationen</span><span class="sxs-lookup"><span data-stu-id="78dff-111">Reference Documentation</span></span>

<span data-ttu-id="78dff-112">Du hittar den [Javadocs API-referens] [ 12] för Android klientbiblioteket på GitHub.</span><span class="sxs-lookup"><span data-stu-id="78dff-112">You can find the [Javadocs API reference][12] for the Android client library on GitHub.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="78dff-113">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="78dff-113">Supported Platforms</span></span>

<span data-ttu-id="78dff-114">Azure Mobile Apps-SDK för Android stöder API nivåer 19 till 24 (KitKat via Nougat) för Telefoner och surfplattor formfaktorer.</span><span class="sxs-lookup"><span data-stu-id="78dff-114">The Azure Mobile Apps SDK for Android supports API levels 19 through 24 (KitKat through Nougat) for phone and tablet form factors.</span></span>  <span data-ttu-id="78dff-115">Autentisering, i synnerhet använder en gemensam web framework strategi för att samla in autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="78dff-115">Authentication, in particular, utilizes a common web framework approach to gather credentials.</span></span>  <span data-ttu-id="78dff-116">Server-flöde autentisering fungerar inte med liten faktor enheter, till exempel ur.</span><span class="sxs-lookup"><span data-stu-id="78dff-116">Server-flow authentication does not work with small form factor devices such as watches.</span></span>

## <a name="setup-and-prerequisites"></a><span data-ttu-id="78dff-117">Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="78dff-117">Setup and Prerequisites</span></span>

<span data-ttu-id="78dff-118">Slutför den [Mobile Apps quickstart](app-service-mobile-android-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="78dff-118">Complete the [Mobile Apps quickstart](app-service-mobile-android-get-started.md) tutorial.</span></span>  <span data-ttu-id="78dff-119">Den här åtgärden säkerställer att alla förutsättningar för att utveckla Azure Mobile Apps har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="78dff-119">This task ensures all pre-requisites for developing Azure Mobile Apps have been met.</span></span>  <span data-ttu-id="78dff-120">Snabbstart hjälper dig att konfigurera ditt konto och skapa din första mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-120">The Quickstart also helps you configure your account and create your first Mobile App backend.</span></span>

<span data-ttu-id="78dff-121">Om du inte att slutföra Snabbstartsguide kan du utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="78dff-121">If you decide not to complete the Quickstart tutorial, complete the following tasks:</span></span>

* <span data-ttu-id="78dff-122">[Skapa en mobilappsserverdel] [ 13] ska användas med din Android-app.</span><span class="sxs-lookup"><span data-stu-id="78dff-122">[create a Mobile App backend][13] to use with your Android app.</span></span>
* <span data-ttu-id="78dff-123">I Android Studio [uppdatering av Gradle skapa filer](#gradle-build).</span><span class="sxs-lookup"><span data-stu-id="78dff-123">In Android Studio, [update the Gradle build files](#gradle-build).</span></span>
* <span data-ttu-id="78dff-124">[Aktivera internet behörigheten](#enable-internet).</span><span class="sxs-lookup"><span data-stu-id="78dff-124">[Enable internet permission](#enable-internet).</span></span>

### <span data-ttu-id="78dff-125"><a name="gradle-build"></a>Uppdatera Gradle build-filen</span><span class="sxs-lookup"><span data-stu-id="78dff-125"><a name="gradle-build"></a>Update the Gradle build file</span></span>

<span data-ttu-id="78dff-126">Ändra både **build.gradle** filer:</span><span class="sxs-lookup"><span data-stu-id="78dff-126">Change both **build.gradle** files:</span></span>

1. <span data-ttu-id="78dff-127">Lägg till denna kod till den *projekt* nivå **build.gradle** filen i den *buildscript* tagg:</span><span class="sxs-lookup"><span data-stu-id="78dff-127">Add this code to the *Project* level **build.gradle** file inside the *buildscript* tag:</span></span>

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. <span data-ttu-id="78dff-128">Lägg till denna kod till den *modulen app* nivå **build.gradle** filen i den *beroenden* tagg:</span><span class="sxs-lookup"><span data-stu-id="78dff-128">Add this code to the *Module app* level **build.gradle** file inside the *dependencies* tag:</span></span>

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    <span data-ttu-id="78dff-129">Den senaste versionen är för närvarande 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="78dff-129">Currently the latest version is 3.3.0.</span></span> <span data-ttu-id="78dff-130">Versionerna som stöds anges [på bintray][14].</span><span class="sxs-lookup"><span data-stu-id="78dff-130">The supported versions are listed [on bintray][14].</span></span>

### <span data-ttu-id="78dff-131"><a name="enable-internet"></a>Aktivera internet behörighet</span><span class="sxs-lookup"><span data-stu-id="78dff-131"><a name="enable-internet"></a>Enable internet permission</span></span>

<span data-ttu-id="78dff-132">För att komma åt Azure måste appen ha behörigheten INTERNET aktiverad.</span><span class="sxs-lookup"><span data-stu-id="78dff-132">To access Azure, your app must have the INTERNET permission enabled.</span></span> <span data-ttu-id="78dff-133">Om den inte redan är aktiverat lägger du till följande rad med kod till din **AndroidManifest.xml** fil:</span><span class="sxs-lookup"><span data-stu-id="78dff-133">If it's not already enabled, add the following line of code to your **AndroidManifest.xml** file:</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a><span data-ttu-id="78dff-134">Skapa en klientanslutning</span><span class="sxs-lookup"><span data-stu-id="78dff-134">Create a Client Connection</span></span>

<span data-ttu-id="78dff-135">Azure Mobile Apps innehåller fyra funktioner i mobila programmet:</span><span class="sxs-lookup"><span data-stu-id="78dff-135">Azure Mobile Apps provides four functions to your mobile application:</span></span>

* <span data-ttu-id="78dff-136">Dataåtkomst och offlinesynkronisering med en Azure Mobile Apps-tjänst.</span><span class="sxs-lookup"><span data-stu-id="78dff-136">Data Access and Offline Synchronization with an Azure Mobile Apps Service.</span></span>
* <span data-ttu-id="78dff-137">Anropa anpassade API: er skrivs med Azure Mobile Apps Server SDK.</span><span class="sxs-lookup"><span data-stu-id="78dff-137">Call Custom APIs written with the Azure Mobile Apps Server SDK.</span></span>
* <span data-ttu-id="78dff-138">Autentisering med Azure App Service-autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-138">Authentication with Azure App Service Authentication and Authorization.</span></span>
* <span data-ttu-id="78dff-139">Registrera push-meddelande med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="78dff-139">Push Notification Registration with Notification Hubs.</span></span>

<span data-ttu-id="78dff-140">Dessa funktioner först måste du skapa en `MobileServiceClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="78dff-140">Each of these functions first requires that you create a `MobileServiceClient` object.</span></span>  <span data-ttu-id="78dff-141">Endast en `MobileServiceClient` objekt ska skapas i din mobila klienten (det vill säga att det ska vara en Singleton-mönster).</span><span class="sxs-lookup"><span data-stu-id="78dff-141">Only one `MobileServiceClient` object should be created within your mobile client (that is, it should be a Singleton pattern).</span></span>  <span data-ttu-id="78dff-142">Så här skapar du en `MobileServiceClient` objekt:</span><span class="sxs-lookup"><span data-stu-id="78dff-142">To create a `MobileServiceClient` object:</span></span>

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

<span data-ttu-id="78dff-143">Den `<MobileAppUrl>` är en sträng eller ett objekt i URL som pekar på din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-143">The `<MobileAppUrl>` is either a string or a URL object that points to your mobile backend.</span></span>  <span data-ttu-id="78dff-144">Om du använder Azure App Service som värd för din mobila serverdel sedan kontrollera att du använder den säkra `https://` version av URL: en.</span><span class="sxs-lookup"><span data-stu-id="78dff-144">If you are using Azure App Service to host your mobile backend, then ensure you use the secure `https://` version of the URL.</span></span>

<span data-ttu-id="78dff-145">Klienten kräver också tillgång till aktivitet eller kontext - den `this` parameter i exemplet.</span><span class="sxs-lookup"><span data-stu-id="78dff-145">The client also requires access to the Activity or Context - the `this` parameter in the example.</span></span>  <span data-ttu-id="78dff-146">MobileServiceClient konstruktionen ska ske inom den `onCreate()` metod för aktiviteten som refereras till i den `AndroidManifest.xml` filen.</span><span class="sxs-lookup"><span data-stu-id="78dff-146">The MobileServiceClient construction should happen within the `onCreate()` method of the Activity referenced in the `AndroidManifest.xml` file.</span></span>

<span data-ttu-id="78dff-147">Som bästa praxis bör du abstrakt serverkommunikation i sin egen (singleton-mönster)-klassen.</span><span class="sxs-lookup"><span data-stu-id="78dff-147">As a best practice, you should abstract server communication into its own (singleton-pattern) class.</span></span>  <span data-ttu-id="78dff-148">I det här fallet bör du överföra aktiviteten i konstruktorn för att konfigurera tjänsten på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="78dff-148">In this case, you should pass the Activity within the constructor to appropriately configure the service.</span></span>  <span data-ttu-id="78dff-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="78dff-149">For example:</span></span>

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

<span data-ttu-id="78dff-150">Nu kan du anropa `AzureServiceAdapter.Initialize(this);` i den `onCreate()` metod för den huvudsakliga aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="78dff-150">You can now call `AzureServiceAdapter.Initialize(this);` in the `onCreate()` method of your main activity.</span></span>  <span data-ttu-id="78dff-151">Andra metoder som behöver åtkomst till klienten använder `AzureServiceAdapter.getInstance();` att hämta en referens till tjänsten kortet.</span><span class="sxs-lookup"><span data-stu-id="78dff-151">Any other methods needing access to the client use `AzureServiceAdapter.getInstance();` to obtain a reference to the service adapter.</span></span>

## <a name="data-operations"></a><span data-ttu-id="78dff-152">Dataåtgärder</span><span class="sxs-lookup"><span data-stu-id="78dff-152">Data Operations</span></span>

<span data-ttu-id="78dff-153">Kärnan i Azure Mobile Apps-SDK är att ge åtkomst till data som lagras i SQL Azure på serverdelen för Mobilappen.</span><span class="sxs-lookup"><span data-stu-id="78dff-153">The core of the Azure Mobile Apps SDK is to provide access to data stored within SQL Azure on the Mobile App backend.</span></span>  <span data-ttu-id="78dff-154">Du kan komma åt dessa data med strikt typkontroll klasser (rekommenderas) eller utan angiven frågor (rekommenderas inte).</span><span class="sxs-lookup"><span data-stu-id="78dff-154">You can access this data using strongly typed classes (preferred) or untyped queries (not recommended).</span></span>  <span data-ttu-id="78dff-155">Den största delen av det här avsnittet behandlar med strikt typkontroll klasser.</span><span class="sxs-lookup"><span data-stu-id="78dff-155">The bulk of this section deals with using strongly typed classes.</span></span>

### <a name="define-client-data-classes"></a><span data-ttu-id="78dff-156">Definiera dataklasser som klienten</span><span class="sxs-lookup"><span data-stu-id="78dff-156">Define client data classes</span></span>

<span data-ttu-id="78dff-157">Definiera klienten dataklasser som är kopplade till tabeller i serverdelen för Mobilappen för att komma åt data från SQL Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="78dff-157">To access data from SQL Azure tables, define client data classes that correspond to the tables in the Mobile App backend.</span></span> <span data-ttu-id="78dff-158">Exemplen i det här avsnittet förutsätter att en tabell med namnet **MyDataTable**, som innehåller följande kolumner:</span><span class="sxs-lookup"><span data-stu-id="78dff-158">Examples in this topic assume a table named **MyDataTable**, which has the following columns:</span></span>

* <span data-ttu-id="78dff-159">id</span><span class="sxs-lookup"><span data-stu-id="78dff-159">id</span></span>
* <span data-ttu-id="78dff-160">Text</span><span class="sxs-lookup"><span data-stu-id="78dff-160">text</span></span>
* <span data-ttu-id="78dff-161">Slutför</span><span class="sxs-lookup"><span data-stu-id="78dff-161">complete</span></span>

<span data-ttu-id="78dff-162">Motsvarande skrivna klientsidan objektet finns i en fil med namnet **MyDataTable.java**:</span><span class="sxs-lookup"><span data-stu-id="78dff-162">The corresponding typed client-side object resides in a file called **MyDataTable.java**:</span></span>

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

<span data-ttu-id="78dff-163">Lägg till get och set-metoder för varje fält som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="78dff-163">Add getter and setter methods for each field that you add.</span></span>  <span data-ttu-id="78dff-164">Om din SQL Azure-tabellen innehåller fler kolumner, och Lägg till motsvarande fält på den här klassen.</span><span class="sxs-lookup"><span data-stu-id="78dff-164">If your SQL Azure table contains more columns, you would add the corresponding fields to this class.</span></span>  <span data-ttu-id="78dff-165">Till exempel om DTO (data transfer objekt) hade en heltalskolumn prioritet och sedan kan du lägga till det här fältet och dess get- och set-metoder:</span><span class="sxs-lookup"><span data-stu-id="78dff-165">For example, if the DTO (data transfer object) had an integer Priority column, then you might add this field, along with its getter and setter methods:</span></span>

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

<span data-ttu-id="78dff-166">Information om hur du skapar ytterligare tabeller i Mobile Apps-serverdel finns [så här: definiera en tabell styrenhet] [ 15] (.NET-serverdel) eller [definiera tabeller med en dynamisk Schema] [ 16] (Node.js-serverdel).</span><span class="sxs-lookup"><span data-stu-id="78dff-166">To learn how to create additional tables in your Mobile Apps backend, see [How to: Define a table controller][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js backend).</span></span>

<span data-ttu-id="78dff-167">En Azure Mobile Apps serverdel tabell definierar fem särskilda fält fyra som är tillgängliga för klienter:</span><span class="sxs-lookup"><span data-stu-id="78dff-167">An Azure Mobile Apps backend table defines five special fields, four of which are available to clients:</span></span>

* <span data-ttu-id="78dff-168">`String id`: Globalt unikt ID för posten.</span><span class="sxs-lookup"><span data-stu-id="78dff-168">`String id`: The globally unique ID for the record.</span></span>  <span data-ttu-id="78dff-169">Som bästa praxis, se id strängrepresentation av en [UUID] [ 17] objekt.</span><span class="sxs-lookup"><span data-stu-id="78dff-169">As a best practice, make the id the String representation of a [UUID][17] object.</span></span>
* <span data-ttu-id="78dff-170">`DateTimeOffset updatedAt`: Det datum/tid för senaste uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="78dff-170">`DateTimeOffset updatedAt`: The date/time of the last update.</span></span>  <span data-ttu-id="78dff-171">Fältet updatedAt anges av servern och aldrig ställas in av din klientkod.</span><span class="sxs-lookup"><span data-stu-id="78dff-171">The updatedAt field is set by the server and should never be set by your client code.</span></span>
* <span data-ttu-id="78dff-172">`DateTimeOffset createdAt`: Det datum/tid som objektet skapades.</span><span class="sxs-lookup"><span data-stu-id="78dff-172">`DateTimeOffset createdAt`: The date/time that the object was created.</span></span>  <span data-ttu-id="78dff-173">Fältet createdAt anges av servern och aldrig ställas in av din klientkod.</span><span class="sxs-lookup"><span data-stu-id="78dff-173">The createdAt field is set by the server and should never be set by your client code.</span></span>
* <span data-ttu-id="78dff-174">`byte[] version`: Normalt representeras som en sträng, anges versionen också av servern.</span><span class="sxs-lookup"><span data-stu-id="78dff-174">`byte[] version`: Normally represented as a string, the version is also set by the server.</span></span>
* <span data-ttu-id="78dff-175">`boolean deleted`: Anger att posten har tagits bort men inte bort ännu.</span><span class="sxs-lookup"><span data-stu-id="78dff-175">`boolean deleted`: Indicates that the record has been deleted but not purged yet.</span></span>  <span data-ttu-id="78dff-176">Använd inte `deleted` som en egenskap i klassen.</span><span class="sxs-lookup"><span data-stu-id="78dff-176">Do not use `deleted` as a property in your class.</span></span>

<span data-ttu-id="78dff-177">Den `id` fältet är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="78dff-177">The `id` field is required.</span></span>  <span data-ttu-id="78dff-178">Den `updatedAt` fält och `version` används för offlinesynkronisering (för matchning av inkrementell synkronisering och konflikt respektive).</span><span class="sxs-lookup"><span data-stu-id="78dff-178">The `updatedAt` field and `version` field are used for offline synchronization (for incremental sync and conflict resolution respectively).</span></span>  <span data-ttu-id="78dff-179">Den `createdAt` fältet är en referensfält som inte används av klienten.</span><span class="sxs-lookup"><span data-stu-id="78dff-179">The `createdAt` field is a reference field and is not used by the client.</span></span>  <span data-ttu-id="78dff-180">Namnen är ”över överföring” namnen på egenskaperna och är inte ställas in.</span><span class="sxs-lookup"><span data-stu-id="78dff-180">The names are "across-the-wire" names of the properties and are not adjustable.</span></span>  <span data-ttu-id="78dff-181">Du kan dock skapa en mappning mellan objektet och ”över överföring”-namn med hjälp av den [gson] [ 3] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="78dff-181">However, you can create a mapping between your object and the "across-the-wire" names using the [gson][3] library.</span></span>  <span data-ttu-id="78dff-182">Exempel:</span><span class="sxs-lookup"><span data-stu-id="78dff-182">For example:</span></span>

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

### <a name="create-a-table-reference"></a><span data-ttu-id="78dff-183">Skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="78dff-183">Create a Table Reference</span></span>

<span data-ttu-id="78dff-184">Om du vill få åtkomst till en tabell måste först skapa en [MobileServiceTable] [ 8] objekt genom att anropa den **getTable** -metoden i den [MobileServiceClient] [9].</span><span class="sxs-lookup"><span data-stu-id="78dff-184">To access a table, first create a [MobileServiceTable][8] object by calling the **getTable** method on the [MobileServiceClient][9].</span></span>  <span data-ttu-id="78dff-185">Den här metoden har två överlagringar:</span><span class="sxs-lookup"><span data-stu-id="78dff-185">This method has two overloads:</span></span>

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

<span data-ttu-id="78dff-186">I följande kod **mClient** är en referens till MobileServiceClient-objektet.</span><span class="sxs-lookup"><span data-stu-id="78dff-186">In the following code, **mClient** is a reference to your MobileServiceClient object.</span></span>  <span data-ttu-id="78dff-187">Första överlagring används där klassnamnet och tabellens namn är desamma, och är den som används i Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="78dff-187">The first overload is used where the class name and the table name are the same, and is the one used in the Quickstart:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

<span data-ttu-id="78dff-188">Andra överlagring används när tabellens namn skiljer sig från namnet på klassen: den första parametern är namnet på tabellen.</span><span class="sxs-lookup"><span data-stu-id="78dff-188">The second overload is used when the table name is different from the class name: the first parameter is the table name.</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <span data-ttu-id="78dff-189"><a name="query"></a>Fråga en Backend-tabell</span><span class="sxs-lookup"><span data-stu-id="78dff-189"><a name="query"></a>Query a Backend Table</span></span>

<span data-ttu-id="78dff-190">Skaffa först en tabellreferens.</span><span class="sxs-lookup"><span data-stu-id="78dff-190">First, obtain a table reference.</span></span>  <span data-ttu-id="78dff-191">Kör en fråga på tabellreferensen.</span><span class="sxs-lookup"><span data-stu-id="78dff-191">Then execute a query on the table reference.</span></span>  <span data-ttu-id="78dff-192">En fråga är en kombination av:</span><span class="sxs-lookup"><span data-stu-id="78dff-192">A query is any combination of:</span></span>

* <span data-ttu-id="78dff-193">En `.where()` [filtersats](#filtering).</span><span class="sxs-lookup"><span data-stu-id="78dff-193">A `.where()` [filter clause](#filtering).</span></span>
* <span data-ttu-id="78dff-194">En `.orderBy()` [ordning satsen](#sorting).</span><span class="sxs-lookup"><span data-stu-id="78dff-194">An `.orderBy()` [ordering clause](#sorting).</span></span>
* <span data-ttu-id="78dff-195">En `.select()` [fältet markeringen satsen](#selection).</span><span class="sxs-lookup"><span data-stu-id="78dff-195">A `.select()` [field selection clause](#selection).</span></span>
* <span data-ttu-id="78dff-196">En `.skip()` och `.top()` för [växlingsbart systemminne resultat](#paging).</span><span class="sxs-lookup"><span data-stu-id="78dff-196">A `.skip()` and `.top()` for [paged results](#paging).</span></span>

<span data-ttu-id="78dff-197">Satserna måste vara angiven i ordningen som föregående.</span><span class="sxs-lookup"><span data-stu-id="78dff-197">The clauses must be presented in the preceding order.</span></span>

### <span data-ttu-id="78dff-198"><a name="filter"></a>Filtrerar resultaten</span><span class="sxs-lookup"><span data-stu-id="78dff-198"><a name="filter"></a> Filtering Results</span></span>

<span data-ttu-id="78dff-199">Den allmänna formen av en fråga är:</span><span class="sxs-lookup"><span data-stu-id="78dff-199">The general form of a query is:</span></span>

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

<span data-ttu-id="78dff-200">Föregående exempel returnerar alla resultat (upp till den maximala sidstorleken som angetts av servern).</span><span class="sxs-lookup"><span data-stu-id="78dff-200">The preceding example returns all results (up to the maximum page size set by the server).</span></span>  <span data-ttu-id="78dff-201">Den `.execute()` metoden Kör frågan på serverdelen.</span><span class="sxs-lookup"><span data-stu-id="78dff-201">The `.execute()` method executes the query on the backend.</span></span>  <span data-ttu-id="78dff-202">Frågan har konverterats till ett [OData v3] [ 19] fråga innan informationen överförs till Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-202">The query is converted to an [OData v3][19] query before transmission to the Mobile Apps backend.</span></span>  <span data-ttu-id="78dff-203">Erhåller konverterar Mobile Apps-serverdel frågan till en SQL-instruktion innan den körs på SQL Azure-instansen.</span><span class="sxs-lookup"><span data-stu-id="78dff-203">On receipt, the Mobile Apps backend converts the query into an SQL statement before executing it on the SQL Azure instance.</span></span>  <span data-ttu-id="78dff-204">Eftersom nätverksaktivitet tar en stund, den `.execute()` metoden returnerar en [ `ListenableFuture<E>` ] [ 18].</span><span class="sxs-lookup"><span data-stu-id="78dff-204">Since network activity takes some time, The `.execute()` method returns a [`ListenableFuture<E>`][18].</span></span>

### <span data-ttu-id="78dff-205"><a name="filtering"></a>Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="78dff-205"><a name="filtering"></a>Filter returned data</span></span>

<span data-ttu-id="78dff-206">Följande frågan returnerar alla objekt från den **ToDoItem** tabellen var **fullständig** är lika med **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="78dff-206">The following query execution returns all items from the **ToDoItem** table where **complete** equals **false**.</span></span>

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

<span data-ttu-id="78dff-207">**mToDoTable** är referens till tabellen Mobiltjänst som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="78dff-207">**mToDoTable** is the reference to the mobile service table that we created previously.</span></span>

<span data-ttu-id="78dff-208">Definiera ett filter som använder den **där** metodanrop på tabellreferensen.</span><span class="sxs-lookup"><span data-stu-id="78dff-208">Define a filter using the **where** method call on the table reference.</span></span> <span data-ttu-id="78dff-209">Den **där** metoden följs av en **fältet** metoden följt av en metod som anger logiska predikatet.</span><span class="sxs-lookup"><span data-stu-id="78dff-209">The **where** method is followed by a **field** method followed by a method that specifies the logical predicate.</span></span> <span data-ttu-id="78dff-210">Möjliga metoder kan predikat **eq** (motsvarar) **ne** (inte lika med), **gt** (större än), **ge** (större än eller lika med), **lt** (minst), **le** (mindre än eller lika med).</span><span class="sxs-lookup"><span data-stu-id="78dff-210">Possible predicate methods include **eq** (equals), **ne** (not equal), **gt** (greater than), **ge** (greater than or equal to), **lt** (less than), **le** (less than or equal to).</span></span> <span data-ttu-id="78dff-211">Dessa metoder kan du jämföra antalet och strängen fält till specifika värden.</span><span class="sxs-lookup"><span data-stu-id="78dff-211">These methods let you compare number and string fields to specific values.</span></span>

<span data-ttu-id="78dff-212">Du kan filtrera efter datum.</span><span class="sxs-lookup"><span data-stu-id="78dff-212">You can filter on dates.</span></span> <span data-ttu-id="78dff-213">Följande metoder kan du jämföra datumfält hela eller delar av datum: **år**, **månad**, **dag**, **timme**,  **minut**, och **andra**.</span><span class="sxs-lookup"><span data-stu-id="78dff-213">The following methods let you compare the entire date field or parts of the date: **year**, **month**, **day**, **hour**, **minute**, and **second**.</span></span> <span data-ttu-id="78dff-214">I följande exempel lägger till ett filter för objekt vars *förfallodatum* är lika med 2013.</span><span class="sxs-lookup"><span data-stu-id="78dff-214">The following example adds a filter for items whose *due date* equals 2013.</span></span>

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

<span data-ttu-id="78dff-215">Följande metoder stöder komplexa filter på strängfält: **startsWith**, **endsWith**, **concat**, **delsträngen**, **indexOf**, **ersätta**, **toLower**, **toUpper**, **trim**, och **längd** .</span><span class="sxs-lookup"><span data-stu-id="78dff-215">The following methods support complex filters on string fields: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, and **length**.</span></span> <span data-ttu-id="78dff-216">Följande exempel filter för tabellen rader där den *text* kolumnen som börjar med ”PRI0”.</span><span class="sxs-lookup"><span data-stu-id="78dff-216">The following example filters for table rows where the *text* column starts with "PRI0."</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="78dff-217">Följande metoder för operatorn stöds på fälten: **lägga till**, **sub**, **mul**, **div**, **mod**, **våning**, **tak**, och **avrunda**.</span><span class="sxs-lookup"><span data-stu-id="78dff-217">The following operator methods are supported on number fields: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**, and **round**.</span></span> <span data-ttu-id="78dff-218">Följande exempel filter för tabellen rader där den **varaktighet** är ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="78dff-218">The following example filters for table rows where the **duration** is an even number.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

<span data-ttu-id="78dff-219">Du kan kombinera predikat med metoderna logiska: **och**, **eller** och **inte**.</span><span class="sxs-lookup"><span data-stu-id="78dff-219">You can combine predicates with these logical methods: **and**, **or** and **not**.</span></span> <span data-ttu-id="78dff-220">I följande exempel kombinerar två av föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="78dff-220">The following example combines two of the preceding examples.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="78dff-221">Gruppera och kapsla logiska operatorer:</span><span class="sxs-lookup"><span data-stu-id="78dff-221">Group and nest logical operators:</span></span>

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

<span data-ttu-id="78dff-222">Mer detaljerad information och exempel på filtrering finns [utforska informationen i Android klienten frågemodell][20].</span><span class="sxs-lookup"><span data-stu-id="78dff-222">For more detailed discussion and examples of filtering, see [Exploring the richness of the Android client query model][20].</span></span>

### <span data-ttu-id="78dff-223"><a name="sorting"></a>Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="78dff-223"><a name="sorting"></a>Sort returned data</span></span>

<span data-ttu-id="78dff-224">Följande kod returnerar alla objekt från en tabell med **ToDoItems** Sortera stigande efter den *text* fältet.</span><span class="sxs-lookup"><span data-stu-id="78dff-224">The following code returns all items from a table of **ToDoItems** sorted ascending by the *text* field.</span></span> <span data-ttu-id="78dff-225">*mToDoTable* är referens till tabellen backend som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="78dff-225">*mToDoTable* is the reference to the backend table that you created previously:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

<span data-ttu-id="78dff-226">Den första parametern för den **orderBy** metoden är en sträng som är lika med namnet på fältet som du vill sortera.</span><span class="sxs-lookup"><span data-stu-id="78dff-226">The first parameter of the **orderBy** method is a string equal to the name of the field on which to sort.</span></span> <span data-ttu-id="78dff-227">Den andra parametern använder den **QueryOrder** uppräkningen för att ange om du vill sortera stigande eller fallande.</span><span class="sxs-lookup"><span data-stu-id="78dff-227">The second parameter uses the **QueryOrder** enumeration to specify whether to sort ascending or descending.</span></span>  <span data-ttu-id="78dff-228">Om du filtrerar med hjälp av den ***där*** -metoden i ***där*** metod måste anropas innan den ***orderBy*** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-228">If you are filtering using the ***where*** method, the ***where*** method must be invoked before the ***orderBy*** method.</span></span>

### <span data-ttu-id="78dff-229"><a name="selection"></a>Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="78dff-229"><a name="selection"></a>Select specific columns</span></span>

<span data-ttu-id="78dff-230">Följande kod visar hur du återställer alla objekt från en tabell med **ToDoItems**, men visar endast den **fullständig** och **text** fält.</span><span class="sxs-lookup"><span data-stu-id="78dff-230">The following code illustrates how to return all items from a table of **ToDoItems**, but only displays the **complete** and **text** fields.</span></span> <span data-ttu-id="78dff-231">**mToDoTable** är referens till tabellen backend som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="78dff-231">**mToDoTable** is the reference to the backend table that we created previously.</span></span>

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

<span data-ttu-id="78dff-232">Parametrarna för funktionen väljer är sträng namnen på kolumnerna i tabellen som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="78dff-232">The parameters to the select function are the string names of the table's columns that you want to return.</span></span>  <span data-ttu-id="78dff-233">Den **Välj** metod måste följa metoder som **där** och **orderBy**.</span><span class="sxs-lookup"><span data-stu-id="78dff-233">The **select** method needs to follow methods like **where** and **orderBy**.</span></span> <span data-ttu-id="78dff-234">Det kan följas av sidindelning metoder som **hoppa över** och **upp**.</span><span class="sxs-lookup"><span data-stu-id="78dff-234">It can be followed by paging methods like **skip** and **top**.</span></span>

### <span data-ttu-id="78dff-235"><a name="paging"></a>Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="78dff-235"><a name="paging"></a>Return data in pages</span></span>

<span data-ttu-id="78dff-236">Data är **alltid** returneras i sidor.</span><span class="sxs-lookup"><span data-stu-id="78dff-236">Data is **ALWAYS** returned in pages.</span></span>  <span data-ttu-id="78dff-237">Det maximala antalet poster som returneras anges av servern.</span><span class="sxs-lookup"><span data-stu-id="78dff-237">The maximum number of records returned is set by the server.</span></span>  <span data-ttu-id="78dff-238">Om klienten begär fler poster, returnerar servern det maximala antalet poster.</span><span class="sxs-lookup"><span data-stu-id="78dff-238">If the client requests more records, then the server returns the maximum number of records.</span></span>  <span data-ttu-id="78dff-239">Som standard är den maximala sidstorleken på servern 50 poster.</span><span class="sxs-lookup"><span data-stu-id="78dff-239">By default, the maximum page size on the server is 50 records.</span></span>

<span data-ttu-id="78dff-240">Det första exemplet visar hur du väljer de översta fem posterna från en tabell.</span><span class="sxs-lookup"><span data-stu-id="78dff-240">The first example shows how to select the top five items from a table.</span></span> <span data-ttu-id="78dff-241">Frågan returnerar objekten från en tabell med **ToDoItems**.</span><span class="sxs-lookup"><span data-stu-id="78dff-241">The query returns the items from a table of **ToDoItems**.</span></span> <span data-ttu-id="78dff-242">**mToDoTable** är referens till tabellen backend som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="78dff-242">**mToDoTable** is the reference to the backend table that you created previously:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

<span data-ttu-id="78dff-243">Här är en fråga som hoppar över de fem första objekt och returnerar sedan nästa fem:</span><span class="sxs-lookup"><span data-stu-id="78dff-243">Here's a query that skips the first five items, and then returns the next five:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

<span data-ttu-id="78dff-244">Om du vill hämta alla poster i en tabell kan du implementera kod för att iterera över alla sidor:</span><span class="sxs-lookup"><span data-stu-id="78dff-244">If you wish to get all records in a table, implement code to iterate over all pages:</span></span>

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

<span data-ttu-id="78dff-245">En begäran om alla poster med den här metoden skapar minst två begäranden till Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-245">A request for all records using this method creates a minimum of two requests to the Mobile Apps backend.</span></span>

> [!TIP]
> <span data-ttu-id="78dff-246">Att välja rätt sidstorleken är en avvägning mellan minnesanvändning när begäran pågår, bandbreddsanvändning och fördröjning ta emot data helt.</span><span class="sxs-lookup"><span data-stu-id="78dff-246">Choosing the right page size is a balance between memory usage while the request is happening, bandwidth usage and delay in receiving the data completely.</span></span>  <span data-ttu-id="78dff-247">Standardvärdet (50 poster) är lämplig för alla enheter.</span><span class="sxs-lookup"><span data-stu-id="78dff-247">The default (50 records) is suitable for all devices.</span></span>  <span data-ttu-id="78dff-248">Om du använder uteslutande på större minnesenheter, öka upp till 500.</span><span class="sxs-lookup"><span data-stu-id="78dff-248">If you exclusively operate on larger memory devices, increase up to 500.</span></span>  <span data-ttu-id="78dff-249">Vi har hittat som ökar storleken på utöver 500 poster resulterar i oacceptabel fördröjningar och stora minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="78dff-249">We have found that increasing the page size beyond 500 records results in unacceptable delays and large memory issues.</span></span>

### <span data-ttu-id="78dff-250"><a name="chaining"></a>Så här: sammanfoga frågan metoder</span><span class="sxs-lookup"><span data-stu-id="78dff-250"><a name="chaining"></a>How to: Concatenate query methods</span></span>

<span data-ttu-id="78dff-251">De metoder som används i frågor till backend-tabeller kan sammanfogas.</span><span class="sxs-lookup"><span data-stu-id="78dff-251">The methods used in querying backend tables can be concatenated.</span></span> <span data-ttu-id="78dff-252">Länkning frågan metoder kan du markera specifika kolumner filtrerade rader som sorteras och växlingsbart systemminne.</span><span class="sxs-lookup"><span data-stu-id="78dff-252">Chaining query methods allows you to select specific columns of filtered rows that are sorted and paged.</span></span> <span data-ttu-id="78dff-253">Du kan skapa komplexa logiska filter.</span><span class="sxs-lookup"><span data-stu-id="78dff-253">You can create complex logical filters.</span></span>  <span data-ttu-id="78dff-254">Varje fråga-metoden returnerar ett frågeobjekt.</span><span class="sxs-lookup"><span data-stu-id="78dff-254">Each query method returns a Query object.</span></span> <span data-ttu-id="78dff-255">Avsluta serien av metoder och faktiskt kör frågan genom att anropa den **köra** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-255">To end the series of methods and actually run the query, call the **execute** method.</span></span> <span data-ttu-id="78dff-256">Exempel:</span><span class="sxs-lookup"><span data-stu-id="78dff-256">For example:</span></span>

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

<span data-ttu-id="78dff-257">Metoderna länkad fråga måste sorteras på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="78dff-257">The chained query methods must be ordered as follows:</span></span>

1. <span data-ttu-id="78dff-258">Filtrering (**där**) metoder.</span><span class="sxs-lookup"><span data-stu-id="78dff-258">Filtering (**where**) methods.</span></span>
2. <span data-ttu-id="78dff-259">Sortering (**orderBy**) metoder.</span><span class="sxs-lookup"><span data-stu-id="78dff-259">Sorting (**orderBy**) methods.</span></span>
3. <span data-ttu-id="78dff-260">Markeringen (**Välj**) metoder.</span><span class="sxs-lookup"><span data-stu-id="78dff-260">Selection (**select**) methods.</span></span>
4. <span data-ttu-id="78dff-261">växling (**hoppa över** och **upp**) metoder.</span><span class="sxs-lookup"><span data-stu-id="78dff-261">paging (**skip** and **top**) methods.</span></span>

## <span data-ttu-id="78dff-262"><a name="binding"></a>Binda data till användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="78dff-262"><a name="binding"></a>Bind data to the user interface</span></span>

<span data-ttu-id="78dff-263">Databindning omfattar tre komponenter:</span><span class="sxs-lookup"><span data-stu-id="78dff-263">Data binding involves three components:</span></span>

* <span data-ttu-id="78dff-264">Datakällan</span><span class="sxs-lookup"><span data-stu-id="78dff-264">The data source</span></span>
* <span data-ttu-id="78dff-265">Skärmlayout</span><span class="sxs-lookup"><span data-stu-id="78dff-265">The screen layout</span></span>
* <span data-ttu-id="78dff-266">Kort som kopplar samman två tillsammans.</span><span class="sxs-lookup"><span data-stu-id="78dff-266">The adapter that ties the two together.</span></span>

<span data-ttu-id="78dff-267">I vårt exempelkod vi returnera data från tabellen Mobile Apps SQL Azure **ToDoItem** i en matris.</span><span class="sxs-lookup"><span data-stu-id="78dff-267">In our sample code, we return the data from the Mobile Apps SQL Azure table **ToDoItem** into an array.</span></span> <span data-ttu-id="78dff-268">Den här aktiviteten är ett vanligt mönster för program.</span><span class="sxs-lookup"><span data-stu-id="78dff-268">This activity is a common pattern for data applications.</span></span>  <span data-ttu-id="78dff-269">Databasfrågor returnera ofta en mängd rader som klienten hämtar i en lista eller en matris.</span><span class="sxs-lookup"><span data-stu-id="78dff-269">Database queries often return a collection of rows that the client gets in a list or array.</span></span> <span data-ttu-id="78dff-270">I det här exemplet är matrisen datakällan.</span><span class="sxs-lookup"><span data-stu-id="78dff-270">In this sample, the array is the data source.</span></span>  <span data-ttu-id="78dff-271">Koden anger skärmlayout som definierar vyn för de data som visas på enheten.</span><span class="sxs-lookup"><span data-stu-id="78dff-271">The code specifies a screen layout that defines the view of the data that appears on the device.</span></span>  <span data-ttu-id="78dff-272">Två är bundna tillsammans med en kort, som i den här koden är en utökning av den **ArrayAdapter&lt;ToDoItem&gt;**  klass.</span><span class="sxs-lookup"><span data-stu-id="78dff-272">The two are bound together with an adapter, which in this code is an extension of the **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span>

#### <span data-ttu-id="78dff-273"><a name="layout"></a>Definiera layouten</span><span class="sxs-lookup"><span data-stu-id="78dff-273"><a name="layout"></a>Define the Layout</span></span>

<span data-ttu-id="78dff-274">Layouten definieras av flera kodavsnitt för XML-koden.</span><span class="sxs-lookup"><span data-stu-id="78dff-274">The layout is defined by several snippets of XML code.</span></span> <span data-ttu-id="78dff-275">En befintlig layout får följande kod visar den **ListView** vi vill fylla i med vår serverdata.</span><span class="sxs-lookup"><span data-stu-id="78dff-275">Given an existing layout, the following code represents the **ListView** we want to populate with our server data.</span></span>

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

<span data-ttu-id="78dff-276">I föregående kod i *listitem* attribut anger id på layout för en enskild rad i listan.</span><span class="sxs-lookup"><span data-stu-id="78dff-276">In the preceding code, the *listitem* attribute specifies the id of the layout for an individual row in the list.</span></span> <span data-ttu-id="78dff-277">Den här koden anger en kryssruta och tillhörande text och hämtar instansieras en gång för varje objekt i listan.</span><span class="sxs-lookup"><span data-stu-id="78dff-277">This code specifies a check box and its associated text and gets instantiated once for each item in the list.</span></span> <span data-ttu-id="78dff-278">Den här layouten visas inte i **id** fältet och en mer komplicerad layout anger ytterligare fält i vyn.</span><span class="sxs-lookup"><span data-stu-id="78dff-278">This layout does not display the **id** field, and a more complex layout would specify additional fields in the display.</span></span> <span data-ttu-id="78dff-279">Den här koden finns i den **row_list_to_do.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="78dff-279">This code is in the **row_list_to_do.xml** file.</span></span>

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

#### <span data-ttu-id="78dff-280"><a name="adapter"></a>Definiera kortet</span><span class="sxs-lookup"><span data-stu-id="78dff-280"><a name="adapter"></a>Define the adapter</span></span>
<span data-ttu-id="78dff-281">Eftersom datakällan för våra vyn är en matris med **ToDoItem**, vi underklass våra kort från en **ArrayAdapter&lt;ToDoItem&gt;**  klass.</span><span class="sxs-lookup"><span data-stu-id="78dff-281">Since the data source of our view is an array of **ToDoItem**, we subclass our adapter from an **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span> <span data-ttu-id="78dff-282">Den här underklass producerar en vy för varje **ToDoItem** med hjälp av den **row_list_to_do** layout.</span><span class="sxs-lookup"><span data-stu-id="78dff-282">This subclass produces a View for every **ToDoItem** using the **row_list_to_do** layout.</span></span>  <span data-ttu-id="78dff-283">I vår kod vi definiera följande klass som är en utökning av den **ArrayAdapter&lt;E&gt;**  klass:</span><span class="sxs-lookup"><span data-stu-id="78dff-283">In our code, we define the following class that is an extension of the **ArrayAdapter&lt;E&gt;** class:</span></span>

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

<span data-ttu-id="78dff-284">Åsidosätt korten **getView** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-284">Override the adapters **getView** method.</span></span> <span data-ttu-id="78dff-285">Exempel:</span><span class="sxs-lookup"><span data-stu-id="78dff-285">For example:</span></span>

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

<span data-ttu-id="78dff-286">Vi skapa en instans av den här klassen i vår aktiviteten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="78dff-286">We create an instance of this class in our Activity as follows:</span></span>

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

<span data-ttu-id="78dff-287">Den andra parametern till konstruktorn ToDoItemAdapter är en referens till layouten.</span><span class="sxs-lookup"><span data-stu-id="78dff-287">The second parameter to the ToDoItemAdapter constructor is a reference to the layout.</span></span> <span data-ttu-id="78dff-288">Vi kan nu skapa en instans av den **ListView** och tilldela kortet så att den **ListView**.</span><span class="sxs-lookup"><span data-stu-id="78dff-288">We can now instantiate the **ListView** and assign the adapter to the **ListView**.</span></span>

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <span data-ttu-id="78dff-289"><a name="use-adapter"></a>Använda kort att binda till Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="78dff-289"><a name="use-adapter"></a>Use the Adapter to Bind to the UI</span></span>

<span data-ttu-id="78dff-290">Du är nu redo att använda databindning.</span><span class="sxs-lookup"><span data-stu-id="78dff-290">You are now ready to use data binding.</span></span> <span data-ttu-id="78dff-291">Följande kod visar hur du hämtar objekt i tabellen och fylls det lokala kortet med returnerade artiklar.</span><span class="sxs-lookup"><span data-stu-id="78dff-291">The following code shows how to get items in the table and fills the local adapter with the returned items.</span></span>

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

<span data-ttu-id="78dff-292">Anropa kortet när du ändrar den **ToDoItem** tabell.</span><span class="sxs-lookup"><span data-stu-id="78dff-292">Call the adapter any time you modify the **ToDoItem** table.</span></span> <span data-ttu-id="78dff-293">Eftersom ändringar görs på en post med basis, kan du hantera en enskild rad i stället för en samling.</span><span class="sxs-lookup"><span data-stu-id="78dff-293">Since modifications are done on a record by record basis, you handle a single row instead of a collection.</span></span> <span data-ttu-id="78dff-294">När du infogar ett objekt kan anropa den **lägga till** metod på kortet; när du tar bort, anropa den **ta bort** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-294">When you insert an item, call the **add** method on the adapter; when deleting, call the **remove** method.</span></span>

<span data-ttu-id="78dff-295">Du hittar ett komplett exempel i den [Android Snabbstartsprojekt][21].</span><span class="sxs-lookup"><span data-stu-id="78dff-295">You can find a complete example in the [Android Quickstart Project][21].</span></span>

## <span data-ttu-id="78dff-296"><a name="inserting"></a>Infoga data i serverdelen</span><span class="sxs-lookup"><span data-stu-id="78dff-296"><a name="inserting"></a>Insert data into the backend</span></span>

<span data-ttu-id="78dff-297">Skapa en instans av en instans av den *ToDoItem* klassen och ange dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="78dff-297">Instantiate an instance of the *ToDoItem* class and set its properties.</span></span>

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

<span data-ttu-id="78dff-298">Använd sedan **insert()** att infoga ett objekt:</span><span class="sxs-lookup"><span data-stu-id="78dff-298">Then use **insert()** to insert an object:</span></span>

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="78dff-299">Returnerade enheten matchar de data som infogats i backend-tabellen med ID och andra värden (som den `createdAt`, `updatedAt`, och `version` fält) inställd på serverdelen.</span><span class="sxs-lookup"><span data-stu-id="78dff-299">The returned entity matches the data inserted into the backend table, included the ID and any other values (such as the `createdAt`, `updatedAt`, and `version` fields) set on the backend.</span></span>

<span data-ttu-id="78dff-300">Mobile Apps tabeller kräver en primärnyckelkolumn med namnet **id**.</span><span class="sxs-lookup"><span data-stu-id="78dff-300">Mobile Apps tables require a primary key column named **id**.</span></span> <span data-ttu-id="78dff-301">Den här kolumnen måste vara en sträng.</span><span class="sxs-lookup"><span data-stu-id="78dff-301">This column must be a string.</span></span> <span data-ttu-id="78dff-302">Standardvärdet för kolumnen ID är ett GUID.</span><span class="sxs-lookup"><span data-stu-id="78dff-302">The default value of the ID column is a GUID.</span></span>  <span data-ttu-id="78dff-303">Du kan ange andra unika värden, till exempel e-postadresser eller användarnamn.</span><span class="sxs-lookup"><span data-stu-id="78dff-303">You can provide other unique values, such as email addresses or usernames.</span></span> <span data-ttu-id="78dff-304">När ett strängvärde ID inte finns för en infogad post är genererar serverdelen en ny GUID.</span><span class="sxs-lookup"><span data-stu-id="78dff-304">When a string ID value is not provided for an inserted record, the backend generates a new GUID.</span></span>

<span data-ttu-id="78dff-305">ID strängvärden ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="78dff-305">String ID values provide the following advantages:</span></span>

* <span data-ttu-id="78dff-306">ID: N kan skapas utan att göra onödig kommunikation till databasen.</span><span class="sxs-lookup"><span data-stu-id="78dff-306">IDs can be generated without making a round trip to the database.</span></span>
* <span data-ttu-id="78dff-307">Poster är enklare att koppla från olika tabeller eller databaser.</span><span class="sxs-lookup"><span data-stu-id="78dff-307">Records are easier to merge from different tables or databases.</span></span>
* <span data-ttu-id="78dff-308">ID-värden integrera bättre med en programlogik.</span><span class="sxs-lookup"><span data-stu-id="78dff-308">ID values integrate better with an application's logic.</span></span>

<span data-ttu-id="78dff-309">Sträng-ID-värden är **REQUIRED** för offlinesynkronisering support.</span><span class="sxs-lookup"><span data-stu-id="78dff-309">String ID values are **REQUIRED** for offline sync support.</span></span>  <span data-ttu-id="78dff-310">Du kan inte ändra ett Id när den är lagrad i databasen.</span><span class="sxs-lookup"><span data-stu-id="78dff-310">You cannot change an Id once it is stored in the backend database.</span></span>

## <span data-ttu-id="78dff-311"><a name="updating"></a>Uppdatera data i en mobil app</span><span class="sxs-lookup"><span data-stu-id="78dff-311"><a name="updating"></a>Update data in a mobile app</span></span>

<span data-ttu-id="78dff-312">Om du vill uppdatera data i en tabell kan skicka det nya objektet ska den **update()** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-312">To update data in a table, pass the new object to the **update()** method.</span></span>

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="78dff-313">I det här exemplet *objektet* är en referens till en rad i den *ToDoItem* tabellen som har haft vissa ändringar.</span><span class="sxs-lookup"><span data-stu-id="78dff-313">In this example, *item* is a reference to a row in the *ToDoItem* table, which has had some changes made to it.</span></span>  <span data-ttu-id="78dff-314">Raden med samma **id** uppdateras.</span><span class="sxs-lookup"><span data-stu-id="78dff-314">The row with the same **id** is updated.</span></span>

## <span data-ttu-id="78dff-315"><a name="deleting"></a>Ta bort data i en mobil app</span><span class="sxs-lookup"><span data-stu-id="78dff-315"><a name="deleting"></a>Delete data in a mobile app</span></span>

<span data-ttu-id="78dff-316">Följande kod visar hur du tar bort data från en tabell genom att ange dataobjektet.</span><span class="sxs-lookup"><span data-stu-id="78dff-316">The following code shows how to delete data from a table by specifying the data object.</span></span>

```java
mToDoTable
    .delete(item);
```

<span data-ttu-id="78dff-317">Du kan också ta bort ett objekt genom att ange den **id** för att ta bort raden.</span><span class="sxs-lookup"><span data-stu-id="78dff-317">You can also delete an item by specifying the **id** field of the row to delete.</span></span>

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <span data-ttu-id="78dff-318"><a name="lookup"></a>Leta upp ett specifikt objekt-ID: t</span><span class="sxs-lookup"><span data-stu-id="78dff-318"><a name="lookup"></a>Look up a specific item by Id</span></span>

<span data-ttu-id="78dff-319">Leta upp ett objekt med en specifik **id** med den **LETAUPP()** metod:</span><span class="sxs-lookup"><span data-stu-id="78dff-319">Look up an item with a specific **id** field with the **lookUp()** method:</span></span>

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <span data-ttu-id="78dff-320"><a name="untyped"></a>Så här: arbeta med någon data</span><span class="sxs-lookup"><span data-stu-id="78dff-320"><a name="untyped"></a>How to: Work with untyped data</span></span>

<span data-ttu-id="78dff-321">Ej typbestämd programmeringsmiljö ger exakt kontroll över JSON-serialisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-321">The untyped programming model gives you exact control over JSON serialization.</span></span>  <span data-ttu-id="78dff-322">Det finns några vanliga scenarier där kan du använda en ej typbestämd programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="78dff-322">There are some common scenarios where you may wish to use an untyped programming model.</span></span> <span data-ttu-id="78dff-323">Till exempel om backend-tabellen innehåller många kolumner och du behöver bara att referera till en delmängd av kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="78dff-323">For example, if your backend table contains many columns and you only need to reference a subset of the columns.</span></span>  <span data-ttu-id="78dff-324">Den angivna modellen måste du definiera alla kolumner som definierats i Mobile Apps-serverdel i dataklass.</span><span class="sxs-lookup"><span data-stu-id="78dff-324">The typed model requires you to define all the columns defined in the Mobile Apps backend in your data class.</span></span>  <span data-ttu-id="78dff-325">De flesta av API-anrop för att komma åt data liknar skrivna API-anrop.</span><span class="sxs-lookup"><span data-stu-id="78dff-325">Most of the API calls for accessing data are similar to the typed programming calls.</span></span> <span data-ttu-id="78dff-326">Den största skillnaden är att i ej typbestämd modellen du anropa metoder på det **MobileServiceJsonTable** objekt, i stället för den **MobileServiceTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="78dff-326">The main difference is that in the untyped model you invoke methods on the **MobileServiceJsonTable** object, instead of the **MobileServiceTable** object.</span></span>

### <span data-ttu-id="78dff-327"><a name="json_instance"></a>Skapa en instans av en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="78dff-327"><a name="json_instance"></a>Create an instance of an untyped table</span></span>

<span data-ttu-id="78dff-328">Liknar skrivna modellen, börja med att hämta en tabellreferens, men i det här fallet är det en **MobileServicesJsonTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="78dff-328">Similar to the typed model, you start by getting a table reference, but in this case it's a **MobileServicesJsonTable** object.</span></span> <span data-ttu-id="78dff-329">Hämta referensen genom att anropa den **getTable** metod i en instans av klienten:</span><span class="sxs-lookup"><span data-stu-id="78dff-329">Obtain the reference by calling the **getTable** method on an instance of the client:</span></span>

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

<span data-ttu-id="78dff-330">När du har skapat en instans av den **MobileServiceJsonTable**, har nästan samma API: N som är tillgängliga med skrivna programmeringsmiljö.</span><span class="sxs-lookup"><span data-stu-id="78dff-330">Once you have created an instance of the **MobileServiceJsonTable**, it has virtually the same API available as with the typed programming model.</span></span> <span data-ttu-id="78dff-331">I vissa fall kan ta metoderna som en utan angiven parameter i stället för en typifierad parameter.</span><span class="sxs-lookup"><span data-stu-id="78dff-331">In some cases, the methods take an untyped parameter instead of a typed parameter.</span></span>

### <span data-ttu-id="78dff-332"><a name="json_insert"></a>Infoga i en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="78dff-332"><a name="json_insert"></a>Insert into an untyped table</span></span>
<span data-ttu-id="78dff-333">Följande kod visar hur du gör en infogning.</span><span class="sxs-lookup"><span data-stu-id="78dff-333">The following code shows how to do an insert.</span></span> <span data-ttu-id="78dff-334">Det första steget är att skapa en [JsonObject][1], vilket är en del av den [gson] [ 3] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="78dff-334">The first step is to create a [JsonObject][1], which is part of the [gson][3] library.</span></span>

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

<span data-ttu-id="78dff-335">Använd sedan **insert()** att infoga någon objektet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="78dff-335">Then, Use **insert()** to insert the untyped object into the table.</span></span>

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

<span data-ttu-id="78dff-336">Om du behöver hämta infogat objekt-ID, använder du den **getAsJsonPrimitive()** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-336">If you need to get the ID of the inserted object, use the **getAsJsonPrimitive()** method.</span></span>

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <span data-ttu-id="78dff-337"><a name="json_delete"></a>Ta bort från en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="78dff-337"><a name="json_delete"></a>Delete from an untyped table</span></span>
<span data-ttu-id="78dff-338">Följande kod visar hur du tar bort en instans, i det här fallet samma instans av en **JsonObject** som skapades i föregående *infoga* exempel.</span><span class="sxs-lookup"><span data-stu-id="78dff-338">The following code shows how to delete an instance, in this case, the same instance of a **JsonObject** that was created in the prior *insert* example.</span></span> <span data-ttu-id="78dff-339">Koden är densamma som i fallet skrivna, men metoden har en annan signatur eftersom den refererar till en **JsonObject**.</span><span class="sxs-lookup"><span data-stu-id="78dff-339">The code is the same as with the typed case, but the method has a different signature since it references an **JsonObject**.</span></span>

```java
mToDoTable
    .delete(insertedItem);
```

<span data-ttu-id="78dff-340">Du kan också ta bort en instans direkt med ID:</span><span class="sxs-lookup"><span data-stu-id="78dff-340">You can also delete an instance directly by using its ID:</span></span>

```java
mToDoTable.delete(ID);
```

### <span data-ttu-id="78dff-341"><a name="json_get"></a>Returnera alla rader från en ej typbestämd tabell</span><span class="sxs-lookup"><span data-stu-id="78dff-341"><a name="json_get"></a>Return all rows from an untyped table</span></span>
<span data-ttu-id="78dff-342">Följande kod visar hur du hämtar en hel tabell.</span><span class="sxs-lookup"><span data-stu-id="78dff-342">The following code shows how to retrieve an entire table.</span></span> <span data-ttu-id="78dff-343">Eftersom du använder en JSON-tabellen kan hämta du selektivt endast en del av kolumnerna i tabellen.</span><span class="sxs-lookup"><span data-stu-id="78dff-343">Since you are using a JSON Table, you can selectively retrieve only some of the table's columns.</span></span>

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

<span data-ttu-id="78dff-344">Samma uppsättning filtrering, filtrering och växling metoder som är tillgängliga för den angivna modellen är tillgängliga för modellen som argument.</span><span class="sxs-lookup"><span data-stu-id="78dff-344">The same set of filtering, filtering and paging methods that are available for the typed model are available for the untyped model.</span></span>

## <span data-ttu-id="78dff-345"><a name="offline-sync"></a>Implementera offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="78dff-345"><a name="offline-sync"></a>Implement Offline Sync</span></span>

<span data-ttu-id="78dff-346">Azure Mobile Apps klient-SDK implementerar också offlinesynkronisering av data med hjälp av en SQLite-databas för att spara en kopia av data på servern lokalt.</span><span class="sxs-lookup"><span data-stu-id="78dff-346">The Azure Mobile Apps Client SDK also implements offline synchronization of data by using a SQLite database to store a copy of the server data locally.</span></span>  <span data-ttu-id="78dff-347">Åtgärder som utförs på en offline-tabell kräver inte mobil anslutning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="78dff-347">Operations performed on an offline table do not require mobile connectivity to work.</span></span>  <span data-ttu-id="78dff-348">Offlinesynkronisering aids återhämtning och prestanda på bekostnad av mer komplex logik för konfliktlösning.</span><span class="sxs-lookup"><span data-stu-id="78dff-348">Offline sync aids in resilience and performance at the expense of more complex logic for conflict resolution.</span></span>  <span data-ttu-id="78dff-349">Azure Mobile Apps klient-SDK innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="78dff-349">The Azure Mobile Apps Client SDK implements the following features:</span></span>

* <span data-ttu-id="78dff-350">Inkrementell synkronisering: Endast uppdaterade och nya poster har hämtats, spara bandbredd och minne.</span><span class="sxs-lookup"><span data-stu-id="78dff-350">Incremental Sync: Only updated and new records are downloaded, saving bandwidth and memory consumption.</span></span>
* <span data-ttu-id="78dff-351">Optimistisk samtidighet: Operations antas lyckas.</span><span class="sxs-lookup"><span data-stu-id="78dff-351">Optimistic Concurrency: Operations are assumed to succeed.</span></span>  <span data-ttu-id="78dff-352">Konfliktlösning skjuts upp tills uppdateringar utförs på servern.</span><span class="sxs-lookup"><span data-stu-id="78dff-352">Conflict Resolution is deferred until updates are performed on the server.</span></span>
* <span data-ttu-id="78dff-353">Konfliktlösning: SDK identifierar när en motstridig ändring har gjorts på servern och ger hook för att varna användaren.</span><span class="sxs-lookup"><span data-stu-id="78dff-353">Conflict Resolution: The SDK detects when a conflicting change has been made at the server and provides hooks to alert the user.</span></span>
* <span data-ttu-id="78dff-354">Mjuk borttagning: Borttagna poster är markerade tagits bort, så att andra enheter att uppdatera deras offline cache.</span><span class="sxs-lookup"><span data-stu-id="78dff-354">Soft Delete: Deleted records are marked deleted, allowing other devices to update their offline cache.</span></span>

### <a name="initialize-offline-sync"></a><span data-ttu-id="78dff-355">Initiera synkronisering Offline</span><span class="sxs-lookup"><span data-stu-id="78dff-355">Initialize Offline Sync</span></span>

<span data-ttu-id="78dff-356">Varje tabell som är offline måste definieras i offline-cachen innan de används.</span><span class="sxs-lookup"><span data-stu-id="78dff-356">Each offline table must be defined in the offline cache before use.</span></span>  <span data-ttu-id="78dff-357">Normalt görs tabelldefinitionen omedelbart efter att klienten:</span><span class="sxs-lookup"><span data-stu-id="78dff-357">Normally, table definition is done immediately after the creation of the client:</span></span>

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

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
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

### <a name="obtain-a-reference-to-the-offline-cache-table"></a><span data-ttu-id="78dff-358">Hämta en referens till tabellen Cache</span><span class="sxs-lookup"><span data-stu-id="78dff-358">Obtain a reference to the Offline Cache Table</span></span>

<span data-ttu-id="78dff-359">Ett online tabell kan du använda `.getTable()`.</span><span class="sxs-lookup"><span data-stu-id="78dff-359">For an online table, you use `.getTable()`.</span></span>  <span data-ttu-id="78dff-360">För en offline-tabell, använder du `.getSyncTable()`:</span><span class="sxs-lookup"><span data-stu-id="78dff-360">For an offline table, use `.getSyncTable()`:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

<span data-ttu-id="78dff-361">De metoder som är tillgängliga för online-tabeller (inklusive filtrering, sortering, växling, infoga data, uppdatera data och ta bort data) fungerar lika bra på tabellerna online och offline.</span><span class="sxs-lookup"><span data-stu-id="78dff-361">All the methods that are available for online tables (including filtering, sorting, paging, inserting data, updating data, and deleting data) work equally well on online and offline tables.</span></span>

### <a name="synchronize-the-local-offline-cache"></a><span data-ttu-id="78dff-362">Synkronisera den lokala cachen för Offline</span><span class="sxs-lookup"><span data-stu-id="78dff-362">Synchronize the Local Offline Cache</span></span>

<span data-ttu-id="78dff-363">Synkronisering är inom din app kontroll.</span><span class="sxs-lookup"><span data-stu-id="78dff-363">Synchronization is within the control of your app.</span></span>  <span data-ttu-id="78dff-364">Här är ett exempel synkroniseringsmetoden:</span><span class="sxs-lookup"><span data-stu-id="78dff-364">Here is an example synchronization method:</span></span>

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

<span data-ttu-id="78dff-365">Om ett namn på frågan har angetts för den `.pull(query, queryname)` metoden och inkrementell synkronisering används för att returnera endast poster som har skapats eller ändrats sedan senast slutförts pull.</span><span class="sxs-lookup"><span data-stu-id="78dff-365">If a query name is provided to the `.pull(query, queryname)` method, then incremental sync is used to return only records that have been created or changed since the last successfully completed pull.</span></span>

### <a name="handle-conflicts-during-offline-synchronization"></a><span data-ttu-id="78dff-366">Hantera konflikter under offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="78dff-366">Handle Conflicts during Offline Synchronization</span></span>

<span data-ttu-id="78dff-367">Om en konflikt uppstår under en `.push()` åtgärd, en `MobileServiceConflictException` genereras.</span><span class="sxs-lookup"><span data-stu-id="78dff-367">If a conflict happens during a `.push()` operation, a `MobileServiceConflictException` is thrown.</span></span>   <span data-ttu-id="78dff-368">Utfärdat av server-objektet är inbäddat i undantaget och kan hämtas av `.getItem()` undantaget.</span><span class="sxs-lookup"><span data-stu-id="78dff-368">The server-issued item is embedded in the exception and can be retrieved by `.getItem()` on the exception.</span></span>  <span data-ttu-id="78dff-369">Justera push-meddelandet genom att anropa följande objekt i MobileServiceSyncContext-objektet:</span><span class="sxs-lookup"><span data-stu-id="78dff-369">Adjust the push by calling the following items on the MobileServiceSyncContext object:</span></span>

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

<span data-ttu-id="78dff-370">När alla konflikter markeras som du vill, anropa `.push()` igen för att lösa alla konflikter.</span><span class="sxs-lookup"><span data-stu-id="78dff-370">Once all conflicts are marked as you wish, call `.push()` again to resolve all the conflicts.</span></span>

## <span data-ttu-id="78dff-371"><a name="custom-api"></a>Anropa anpassade API</span><span class="sxs-lookup"><span data-stu-id="78dff-371"><a name="custom-api"></a>Call a custom API</span></span>

<span data-ttu-id="78dff-372">En anpassad API kan du definiera anpassade slutpunkter som exponerar serverfunktioner som inte mappas till en infoga, uppdatera, ta bort eller Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="78dff-372">A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation.</span></span> <span data-ttu-id="78dff-373">Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.</span><span class="sxs-lookup"><span data-stu-id="78dff-373">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="78dff-374">I en Android-klient du anropar den **invokeApi** metod för att anropa anpassade API-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="78dff-374">From an Android client, you call the **invokeApi** method to call the custom API endpoint.</span></span> <span data-ttu-id="78dff-375">I följande exempel visas hur du anropar en API-slutpunkt med namnet **completeAll**, som returnerar en samlingsklass som heter **MarkAllResult**.</span><span class="sxs-lookup"><span data-stu-id="78dff-375">The following example shows how to call an API endpoint named **completeAll**, which returns a collection class named **MarkAllResult**.</span></span>

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

<span data-ttu-id="78dff-376">Den **invokeApi** metoden anropas på klienten, som skickar en POST-begäran till den nya anpassade API.</span><span class="sxs-lookup"><span data-stu-id="78dff-376">The **invokeApi** method is called on the client, which sends a POST request to the new custom API.</span></span> <span data-ttu-id="78dff-377">Resultatet som returneras av anpassade API visas i en dialogruta för meddelande som fel.</span><span class="sxs-lookup"><span data-stu-id="78dff-377">The result returned by the custom API is displayed in a message dialog, as are any errors.</span></span> <span data-ttu-id="78dff-378">Andra versioner av **invokeApi** kan du om du vill skicka ett objekt i begärandetexten anger HTTP-metoden och skicka frågeparametrar med begäran.</span><span class="sxs-lookup"><span data-stu-id="78dff-378">Other versions of **invokeApi** let you optionally send an object in the request body, specify the HTTP method, and send query parameters with the request.</span></span> <span data-ttu-id="78dff-379">Utan angiven versioner av **invokeApi** tillhandahålls också.</span><span class="sxs-lookup"><span data-stu-id="78dff-379">Untyped versions of **invokeApi** are provided as well.</span></span>

## <span data-ttu-id="78dff-380"><a name="authentication"></a>Lägg till autentisering i appen</span><span class="sxs-lookup"><span data-stu-id="78dff-380"><a name="authentication"></a>Add authentication to your app</span></span>

<span data-ttu-id="78dff-381">Självstudiekurser beskrivs redan i detalj hur du lägger till dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="78dff-381">Tutorials already describe in detail how to add these features.</span></span>

<span data-ttu-id="78dff-382">Har stöd för Apptjänst [autentisera användarna](app-service-mobile-android-get-started-users.md) med olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="78dff-382">App Service supports [authenticating app users](app-service-mobile-android-get-started-users.md) using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="78dff-383">Du kan ange behörigheter på tabeller för att begränsa åtkomsten för specifika åtgärder endast autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="78dff-383">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="78dff-384">Du kan också använda identiteten för autentiserade användare för att implementera auktoriseringsregler i din serverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-384">You can also use the identity of authenticated users to implement authorization rules in your backend.</span></span>

<span data-ttu-id="78dff-385">Två autentisering flöden stöds: en **server** flöde och en **klienten** flöde.</span><span class="sxs-lookup"><span data-stu-id="78dff-385">Two authentication flows are supported: a **server** flow and a **client** flow.</span></span> <span data-ttu-id="78dff-386">Server-flöde ger den enklaste autentiseringsupplevelse som använder webbgränssnitt för identitets-providers.</span><span class="sxs-lookup"><span data-stu-id="78dff-386">The server flow provides the simplest authentication experience, as it relies on the identity providers web interface.</span></span>  <span data-ttu-id="78dff-387">Inga ytterligare SDK krävs för att implementera flödet för serverautentisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-387">No additional SDKs are required to implement server flow authentication.</span></span> <span data-ttu-id="78dff-388">Flöde för serverautentisering ger inte djupgående integrering i den mobila enheten och rekommenderas endast för bevis på koncept scenarier.</span><span class="sxs-lookup"><span data-stu-id="78dff-388">Server flow authentication does not provide a deep integration into the mobile device and is only recommended for proof of concept scenarios.</span></span>

<span data-ttu-id="78dff-389">Flödet tillåter djupare integrering med specifika funktioner, till exempel enkel inloggning som använder SDK: er som tillhandahållits av identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="78dff-389">The client flow allows for deeper integration with device-specific capabilities such as single sign-on as it relies on SDKs provided by the identity provider.</span></span>  <span data-ttu-id="78dff-390">Exempelvis kan du integrera Facebook SDK i din mobila program.</span><span class="sxs-lookup"><span data-stu-id="78dff-390">For example, you can integrate the Facebook SDK into your mobile application.</span></span>  <span data-ttu-id="78dff-391">Den mobila klienten växlingar i Facebook-app och bekräftar din inloggning innan du växlar tillbaka till din mobila app.</span><span class="sxs-lookup"><span data-stu-id="78dff-391">The mobile client swaps into the Facebook app and confirms your sign-on before swapping back to your mobile app.</span></span>

<span data-ttu-id="78dff-392">Fyra steg krävs för att aktivera autentisering i appen:</span><span class="sxs-lookup"><span data-stu-id="78dff-392">Four steps are required to enable authentication in your app:</span></span>

* <span data-ttu-id="78dff-393">Registrera din app för autentisering med en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="78dff-393">Register your app for authentication with an identity provider.</span></span>
* <span data-ttu-id="78dff-394">Konfigurera din App Service-serverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-394">Configure your App Service backend.</span></span>
* <span data-ttu-id="78dff-395">Begränsa tabellbehörigheter till autentiserade användare endast på App Service-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="78dff-395">Restrict table permissions to authenticated users only on the App Service backend.</span></span>
* <span data-ttu-id="78dff-396">Lägg till Autentiseringskod i appen.</span><span class="sxs-lookup"><span data-stu-id="78dff-396">Add authentication code to your app.</span></span>

<span data-ttu-id="78dff-397">Du kan ange behörigheter på tabeller för att begränsa åtkomsten för specifika åtgärder endast autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="78dff-397">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="78dff-398">Du kan också använda SID för en autentiserad användare för att ändra begäranden.</span><span class="sxs-lookup"><span data-stu-id="78dff-398">You can also use the SID of an authenticated user to modify requests.</span></span>  <span data-ttu-id="78dff-399">Mer information hittar [komma igång med autentisering] och servern ta för SDK-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="78dff-399">For more information, review [Get started with authentication] and the Server SDK HOWTO documentation.</span></span>

### <span data-ttu-id="78dff-400"><a name="caching"></a>: Server Autentiseringsflödet</span><span class="sxs-lookup"><span data-stu-id="78dff-400"><a name="caching"></a>Authentication: Server Flow</span></span>

<span data-ttu-id="78dff-401">Följande kod startar en server flödet inloggningen med hjälp av Google-providern.</span><span class="sxs-lookup"><span data-stu-id="78dff-401">The following code starts a server flow login process using the Google provider.</span></span>  <span data-ttu-id="78dff-402">Ytterligare konfiguration krävs på grund av säkerhetskraven för Google-provider:</span><span class="sxs-lookup"><span data-stu-id="78dff-402">Additional configuration is required because of the security requirements for the Google provider:</span></span>

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

<span data-ttu-id="78dff-403">Lägg till följande metod i klassen huvudsakliga aktivitet dessutom:</span><span class="sxs-lookup"><span data-stu-id="78dff-403">In addition, add the following method to the main Activity class:</span></span>

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

<span data-ttu-id="78dff-404">Den `GOOGLE_LOGIN_REQUEST_CODE` definierats i din huvudsakliga aktivitet används för den `login()` metod och inom den `onActivityResult()` metoden.</span><span class="sxs-lookup"><span data-stu-id="78dff-404">The `GOOGLE_LOGIN_REQUEST_CODE` defined in your main Activity is used for the `login()` method and within the `onActivityResult()` method.</span></span>  <span data-ttu-id="78dff-405">Du kan välja alla unikt nummer som samma nummer som används i den `login()` metod och `onActivityResult()` metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-405">You can choose any unique number, as long as the same number is used within the `login()` method and the `onActivityResult()` method.</span></span>  <span data-ttu-id="78dff-406">Om du abstrakt klientkoden till ett service-kort (som visas tidigare) bör du anropa lämpliga-metoder för service-kort.</span><span class="sxs-lookup"><span data-stu-id="78dff-406">If you abstract the client code into a service adapter (as shown earlier), you should call the appropriate methods on the service adapter.</span></span>

<span data-ttu-id="78dff-407">Du måste också konfigurera projektet för customtabs.</span><span class="sxs-lookup"><span data-stu-id="78dff-407">You also need to configure the project for customtabs.</span></span>  <span data-ttu-id="78dff-408">Ange en omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="78dff-408">First specify a redirect-URL.</span></span>  <span data-ttu-id="78dff-409">Lägg till följande kodavsnitt till `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="78dff-409">Add the following snippet to `AndroidManifest.xml`:</span></span>

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

<span data-ttu-id="78dff-410">Lägg till den **redirectUriScheme** till den `build.gradle` filen för tillämpningsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="78dff-410">Add the **redirectUriScheme** to the `build.gradle` file for your application:</span></span>

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

<span data-ttu-id="78dff-411">Slutligen lägger du till `com.android.support:customtabs:23.0.1` i beroendelistan i den `build.gradle` filen:</span><span class="sxs-lookup"><span data-stu-id="78dff-411">Finally, add `com.android.support:customtabs:23.0.1` to the dependencies list in the `build.gradle` file:</span></span>

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

<span data-ttu-id="78dff-412">Hämta ID för den inloggade användaren från en **MobileServiceUser** med hjälp av den **getUserId** metod.</span><span class="sxs-lookup"><span data-stu-id="78dff-412">Obtain the ID of the logged-in user from a **MobileServiceUser** using the **getUserId** method.</span></span> <span data-ttu-id="78dff-413">Ett exempel på hur du använder Futures för att anropa asynkron inloggningen API: er finns [komma igång med autentisering].</span><span class="sxs-lookup"><span data-stu-id="78dff-413">For an example of how to use Futures to call the asynchronous login APIs, see [Get started with authentication].</span></span>

> [!WARNING]
> <span data-ttu-id="78dff-414">URL-schemat som nämns är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="78dff-414">The URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="78dff-415">Se till att alla förekomster av `{url_scheme_of_you_app}` gemener/versaler.</span><span class="sxs-lookup"><span data-stu-id="78dff-415">Ensure that all occurrences of `{url_scheme_of_you_app}` match case.</span></span>

### <span data-ttu-id="78dff-416"><a name="caching"></a>Cache-autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="78dff-416"><a name="caching"></a>Cache authentication tokens</span></span>

<span data-ttu-id="78dff-417">Cachelagring autentiseringstoken måste du lagra användar-ID och autentiseringstoken lokalt på enheten.</span><span class="sxs-lookup"><span data-stu-id="78dff-417">Caching authentication tokens requires you to store the User ID and authentication token locally on the device.</span></span> <span data-ttu-id="78dff-418">Nästa gång appen startas du kontrollera cachen, och om dessa värden finns kan du hoppa över loggen i proceduren och rehydrate klienten med dessa data.</span><span class="sxs-lookup"><span data-stu-id="78dff-418">The next time the app starts, you check the cache, and if these values are present, you can skip the log in procedure and rehydrate the client with this data.</span></span> <span data-ttu-id="78dff-419">Men dessa data är känsligt och lagras krypterad för säkerhet om telefonen blir stulen.</span><span class="sxs-lookup"><span data-stu-id="78dff-419">However this data is sensitive, and it should be stored encrypted for safety in case the phone gets stolen.</span></span>  <span data-ttu-id="78dff-420">Du kan se en komplett exempel på hur till cache autentiseringstoken i [cachelagra autentisering tokenavsnittet][7].</span><span class="sxs-lookup"><span data-stu-id="78dff-420">You can see a complete example of how to cache authentication tokens in [Cache authentication tokens section][7].</span></span>

<span data-ttu-id="78dff-421">När du försöker använda en utgången token visas en *401-Ej behörig* svar.</span><span class="sxs-lookup"><span data-stu-id="78dff-421">When you try to use an expired token, you receive a *401 unauthorized* response.</span></span> <span data-ttu-id="78dff-422">Du kan hantera med hjälp av filter-autentiseringsfel.</span><span class="sxs-lookup"><span data-stu-id="78dff-422">You can handle authentication errors using filters.</span></span>  <span data-ttu-id="78dff-423">Filter hantera begäranden till App Service-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="78dff-423">Filters intercept requests to the App Service backend.</span></span> <span data-ttu-id="78dff-424">Filter koden testar svar för en 401, utlöser inloggningsprocessen och återupptar sedan den begäran som genereras av 401.</span><span class="sxs-lookup"><span data-stu-id="78dff-424">The filter code tests the response for a 401, triggers the sign-in process, and then resumes the request that generated the 401.</span></span>

### <span data-ttu-id="78dff-425"><a name="refresh"></a>Använd Uppdateringstoken</span><span class="sxs-lookup"><span data-stu-id="78dff-425"><a name="refresh"></a>Use Refresh Tokens</span></span>

<span data-ttu-id="78dff-426">Den token som returnerades av Azure App Service-autentisering och auktorisering har en definierad livslängden för en timme.</span><span class="sxs-lookup"><span data-stu-id="78dff-426">The token returned by Azure App Service Authentication and Authorization has a defined life time of one hour.</span></span>  <span data-ttu-id="78dff-427">Du måste autentiseras användaren efter denna period.</span><span class="sxs-lookup"><span data-stu-id="78dff-427">After this period, you must reauthenticate the user.</span></span>  <span data-ttu-id="78dff-428">Om du använder en långlivade token som du har fått via flödet autentisering kan du autentiseras med Azure App Service-autentisering och auktorisering med samma token.</span><span class="sxs-lookup"><span data-stu-id="78dff-428">If you are using a long-lived token that you have received via client-flow authentication, then you can reauthenticate with Azure App Service Authentication and Authorization using the same token.</span></span>  <span data-ttu-id="78dff-429">En annan Azure App Service-token genereras med nya livslängden.</span><span class="sxs-lookup"><span data-stu-id="78dff-429">Another Azure App Service token is generated with a new lifetime.</span></span>

<span data-ttu-id="78dff-430">Du kan också registrera providern om du vill använda uppdatera token.</span><span class="sxs-lookup"><span data-stu-id="78dff-430">You can also register the provider to use Refresh Tokens.</span></span>  <span data-ttu-id="78dff-431">En uppdatera Token är inte alltid tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="78dff-431">A Refresh Token is not always available.</span></span>  <span data-ttu-id="78dff-432">Det krävs ytterligare konfiguration:</span><span class="sxs-lookup"><span data-stu-id="78dff-432">Additional configuration is required:</span></span>

* <span data-ttu-id="78dff-433">För **Azure Active Directory**, konfigurera en klienthemlighet för Azure Active Directory-appen.</span><span class="sxs-lookup"><span data-stu-id="78dff-433">For **Azure Active Directory**, configure a client secret for the Azure Active Directory App.</span></span>  <span data-ttu-id="78dff-434">Ange klienthemligheten i Azure App Service när du konfigurerar Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-434">Specify the client secret in the Azure App Service when configuring Azure Active Directory Authentication.</span></span>  <span data-ttu-id="78dff-435">När du anropar `.login()`, skicka `response_type=code id_token` som en parameter:</span><span class="sxs-lookup"><span data-stu-id="78dff-435">When calling `.login()`, pass `response_type=code id_token` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="78dff-436">För **Google**, skickar den `access_type=offline` som en parameter:</span><span class="sxs-lookup"><span data-stu-id="78dff-436">For **Google**, pass the `access_type=offline` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="78dff-437">För **Account**, Välj den `wl.offline_access` omfång.</span><span class="sxs-lookup"><span data-stu-id="78dff-437">For **Microsoft Account**, select the `wl.offline_access` scope.</span></span>

<span data-ttu-id="78dff-438">Om du vill uppdatera en token ring `.refreshUser()`:</span><span class="sxs-lookup"><span data-stu-id="78dff-438">To refresh a token, call `.refreshUser()`:</span></span>

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

<span data-ttu-id="78dff-439">Ett bra tips är att skapa ett filter som identifierar ett 401 svar från servern och försök att uppdatera användar-token.</span><span class="sxs-lookup"><span data-stu-id="78dff-439">As a best practice, create a filter that detects a 401 response from the server and tries to refresh the user token.</span></span>

## <a name="log-in-with-client-flow-authentication"></a><span data-ttu-id="78dff-440">Logga in med klient-flöde för autentisering</span><span class="sxs-lookup"><span data-stu-id="78dff-440">Log in with Client-flow Authentication</span></span>

<span data-ttu-id="78dff-441">Den allmänna processen för att logga in med klient-flöde för autentisering är följande:</span><span class="sxs-lookup"><span data-stu-id="78dff-441">The general process for logging in with client-flow authentication is as follows:</span></span>

* <span data-ttu-id="78dff-442">Konfigurera Azure App Service-autentisering och auktorisering som server-flöde för autentisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-442">Configure Azure App Service Authentication and Authorization as you would server-flow authentication.</span></span>
* <span data-ttu-id="78dff-443">Integrera autentiseringsprovider SDK för autentisering för att skapa en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="78dff-443">Integrate the authentication provider SDK for authentication to produce an access token.</span></span>
* <span data-ttu-id="78dff-444">Anropa den `.login()` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="78dff-444">Call the `.login()` method as follows:</span></span>

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

<span data-ttu-id="78dff-445">Ersätt den `onSuccess()` metod med oavsett kod som du vill använda på en genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="78dff-445">Replace the `onSuccess()` method with whatever code you wish to use on a successful login.</span></span>  <span data-ttu-id="78dff-446">Den `{provider}` strängen är en giltig provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, eller **twitter**.</span><span class="sxs-lookup"><span data-stu-id="78dff-446">The `{provider}` string is a valid provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, or **twitter**.</span></span>  <span data-ttu-id="78dff-447">Om du har implementerat anpassad autentisering, kan du också använda taggen för anpassad autentisering-providern.</span><span class="sxs-lookup"><span data-stu-id="78dff-447">If you have implemented custom authentication, then you can also use the custom authentication provider tag.</span></span>

### <span data-ttu-id="78dff-448"><a name="adal"></a>Autentisera användarna med Active Directory Authentication Library (ADAL)</span><span class="sxs-lookup"><span data-stu-id="78dff-448"><a name="adal"></a>Authenticate users with the Active Directory Authentication Library (ADAL)</span></span>

<span data-ttu-id="78dff-449">Du kan använda Active Directory Authentication Library (ADAL) för att logga in användare på ditt program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="78dff-449">You can use the Active Directory Authentication Library (ADAL) to sign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="78dff-450">Med hjälp av en klient flödet för inloggning är ofta bättre än att använda den `loginAsync()` metoder som ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="78dff-450">Using a client flow login is often preferable to using the `loginAsync()` methods as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="78dff-451">Konfigurera mobilappsserverdelen för AAD-inloggning genom att följa den [konfigurera App Service för Active Directory-inloggningen] [ 22] kursen.</span><span class="sxs-lookup"><span data-stu-id="78dff-451">Configure your mobile app backend for AAD sign-in by following the [How to configure App Service for Active Directory login][22] tutorial.</span></span> <span data-ttu-id="78dff-452">Se till att slutföra det valfria steget med att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="78dff-452">Make sure to complete the optional step of registering a native client application.</span></span>
2. <span data-ttu-id="78dff-453">Installera ADAL genom att ändra filen build.gradle för att inkludera följande definitioner:</span><span class="sxs-lookup"><span data-stu-id="78dff-453">Install ADAL by modifying your build.gradle file to include the following definitions:</span></span>

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

1. <span data-ttu-id="78dff-454">Lägg till följande kod i ditt program, gör följande ersättningar:</span><span class="sxs-lookup"><span data-stu-id="78dff-454">Add the following code to your application, making the following replacements:</span></span>

* <span data-ttu-id="78dff-455">Ersätt **INSERT-UTFÄRDARE-här** med namnet på klienten som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="78dff-455">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="78dff-456">Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="78dff-456">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span>
* <span data-ttu-id="78dff-457">Ersätt **INSERT-resurs-ID-här** med klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="78dff-457">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="78dff-458">Du kan hämta klient-ID från den **Avancerat** fliken **inställningarna för Azure Active Directory** i portalen.</span><span class="sxs-lookup"><span data-stu-id="78dff-458">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
* <span data-ttu-id="78dff-459">Ersätt **INSERT-klient-ID-här** med klient-ID som du kopierade från native client-program.</span><span class="sxs-lookup"><span data-stu-id="78dff-459">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
* <span data-ttu-id="78dff-460">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="78dff-460">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="78dff-461">Det här värdet ska vara liknar *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="78dff-461">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

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

## <span data-ttu-id="78dff-462"><a name="filters"></a>Justera klient-/ serverkommunikation</span><span class="sxs-lookup"><span data-stu-id="78dff-462"><a name="filters"></a>Adjust the Client-Server Communication</span></span>

<span data-ttu-id="78dff-463">Klientanslutningen är vanligtvis en grundläggande HTTP-anslutning med hjälp av den underliggande http-bibliotek som medföljer Android SDK.</span><span class="sxs-lookup"><span data-stu-id="78dff-463">The Client connection is normally a basic HTTP connection using the underlying HTTP library supplied with the Android SDK.</span></span>  <span data-ttu-id="78dff-464">Det finns flera skäl till varför du vill ändra som:</span><span class="sxs-lookup"><span data-stu-id="78dff-464">There are several reasons why you would want to change that:</span></span>

* <span data-ttu-id="78dff-465">Du vill använda ett alternativt HTTP-bibliotek för att justera timeout.</span><span class="sxs-lookup"><span data-stu-id="78dff-465">You wish to use an alternate HTTP library to adjust timeouts.</span></span>
* <span data-ttu-id="78dff-466">Du vill ange en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="78dff-466">You wish to provide a progress bar.</span></span>
* <span data-ttu-id="78dff-467">Du vill lägga till en anpassad rubrik för att stödja API management-funktioner.</span><span class="sxs-lookup"><span data-stu-id="78dff-467">You wish to add a custom header to support API management functionality.</span></span>
* <span data-ttu-id="78dff-468">Du vill fånga misslyckade svar så att du kan implementera omautentisering.</span><span class="sxs-lookup"><span data-stu-id="78dff-468">You wish to intercept a failed response so that you can implement reauthentication.</span></span>
* <span data-ttu-id="78dff-469">Du vill logga backend-begäranden till en analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="78dff-469">You wish to log backend requests to an analytics service.</span></span>

### <a name="using-an-alternate-http-library"></a><span data-ttu-id="78dff-470">Med hjälp av en annan HTTP-bibliotek</span><span class="sxs-lookup"><span data-stu-id="78dff-470">Using an alternate HTTP Library</span></span>

<span data-ttu-id="78dff-471">Anropa den `.setAndroidHttpClientFactory()` metoden omedelbart när du har skapat din klient-referens.</span><span class="sxs-lookup"><span data-stu-id="78dff-471">Call the `.setAndroidHttpClientFactory()` method immediately after creating your client reference.</span></span>  <span data-ttu-id="78dff-472">Till exempel ange anslutningstidsgränsen för till 60 sekunder (i stället för standard 10 sekunder):</span><span class="sxs-lookup"><span data-stu-id="78dff-472">For example, to set the connection timeout to 60 seconds (instead of the default 10 seconds):</span></span>

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

### <a name="implement-a-progress-filter"></a><span data-ttu-id="78dff-473">Implementera ett Filter för pågår</span><span class="sxs-lookup"><span data-stu-id="78dff-473">Implement a Progress Filter</span></span>

<span data-ttu-id="78dff-474">Du kan implementera en skärningspunkt för varje begäran genom att implementera en `ServiceFilter`.</span><span class="sxs-lookup"><span data-stu-id="78dff-474">You can implement an intercept of every request by implementing a `ServiceFilter`.</span></span>  <span data-ttu-id="78dff-475">Följande uppdateringar en förskapad förloppsindikator:</span><span class="sxs-lookup"><span data-stu-id="78dff-475">For example, the following updates a pre-created progress bar:</span></span>

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

<span data-ttu-id="78dff-476">Du kan koppla det här filtret till klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="78dff-476">You can attach this filter to the client as follows:</span></span>

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a><span data-ttu-id="78dff-477">Anpassa huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="78dff-477">Customize Request Headers</span></span>

<span data-ttu-id="78dff-478">Använd följande `ServiceFilter` och bifoga filtret på samma sätt som den `ProgressFilter`:</span><span class="sxs-lookup"><span data-stu-id="78dff-478">Use the following `ServiceFilter` and attach the filter in the same way as the `ProgressFilter`:</span></span>

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

### <span data-ttu-id="78dff-479"><a name="conversions"></a>Konfigurera automatisk serialisering</span><span class="sxs-lookup"><span data-stu-id="78dff-479"><a name="conversions"></a>Configure Automatic Serialization</span></span>

<span data-ttu-id="78dff-480">Du kan ange en konvertering strategi som gäller för alla kolumner med hjälp av den [gson] [ 3] API.</span><span class="sxs-lookup"><span data-stu-id="78dff-480">You can specify a conversion strategy that applies to every column by using the [gson][3] API.</span></span> <span data-ttu-id="78dff-481">Android klientbiblioteket använder [gson] [ 3] i bakgrunden att serialisera Java-objekt till JSON-data innan informationen skickas till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="78dff-481">The Android client library uses [gson][3] behind the scenes to serialize Java objects to JSON data before the data is sent to Azure App Service.</span></span>  <span data-ttu-id="78dff-482">I följande kod används den **setFieldNamingStrategy()** metod för att ange strategin.</span><span class="sxs-lookup"><span data-stu-id="78dff-482">The following code uses the **setFieldNamingStrategy()** method to set the strategy.</span></span> <span data-ttu-id="78dff-483">Det här exemplet tar bort det första tecknet (en ”m”), och sedan nästa tecken för varje fältnamn.</span><span class="sxs-lookup"><span data-stu-id="78dff-483">This example will delete the initial character (an "m"), and then lower-case the next character, for every field name.</span></span> <span data-ttu-id="78dff-484">Till exempel skulle den göra ”mId” till ”id”.</span><span class="sxs-lookup"><span data-stu-id="78dff-484">For example, it would turn "mId" into "id."</span></span>  <span data-ttu-id="78dff-485">Implementera en konvertering strategi för att minska behovet av `SerializedName()` anteckningar i de flesta fält.</span><span class="sxs-lookup"><span data-stu-id="78dff-485">Implement a conversion strategy to reduce the need for `SerializedName()` annotations on most fields.</span></span>

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

<span data-ttu-id="78dff-486">Den här koden måste köras innan du skapar en mobil klient referens med hjälp av den **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="78dff-486">This code must be executed before creating a mobile client reference using the **MobileServiceClient**.</span></span>

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
<span data-ttu-id="78dff-487">[komma igång med autentisering]: app-service-mobile-android-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="78dff-487">[Get started with authentication]: app-service-mobile-android-get-started-users.md</span></span>
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
