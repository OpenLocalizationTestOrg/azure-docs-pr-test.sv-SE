---
title: "aaaAzure AD Android komma igång | Microsoft Docs"
description: "Hur skyddade toobuild ett Android-program som kan integreras med Azure AD för inloggnings- och Azure AD-anrop API: er med hjälp av OAuth."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a><span data-ttu-id="ce855-103">Integrera Azure AD i en Android-app</span><span class="sxs-lookup"><span data-stu-id="ce855-103">Integrate Azure AD into an Android app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="ce855-104">Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/Android), som hjälper dig att komma igång med Azure AD i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ce855-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/Android), which will help you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="ce855-105">hello developer-portalen tar dig igenom hello process för att registrera en app och integrera Azure AD i din kod.</span><span class="sxs-lookup"><span data-stu-id="ce855-105">hello developer portal will walk you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="ce855-106">När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen.</span><span class="sxs-lookup"><span data-stu-id="ce855-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a back end that can accept tokens and perform validation.</span></span>
>
>

<span data-ttu-id="ce855-107">Om du utvecklar ett skrivbordsprogram gör Azure Active Directory (AD Azure) det lätt för tooauthenticate du dina användare genom att använda sina lokala Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="ce855-107">If you're developing a desktop application, Azure Active Directory (Azure AD) makes it simple and straightforward for you tooauthenticate your users by using their on-premises Active Directory accounts.</span></span> <span data-ttu-id="ce855-108">Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="ce855-108">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="ce855-109">Azure AD innehåller hello Active Directory Authentication Library (ADAL) för Android-klienter som behöver tooaccess skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="ce855-109">For Android clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="ce855-110">hello uteslutande av ADAL är toomake det enkelt för din app tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="ce855-110">hello sole purpose of ADAL is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="ce855-111">toodemonstrate hur lätt det är vi ska skapa ett program för Android att göra-lista som:</span><span class="sxs-lookup"><span data-stu-id="ce855-111">toodemonstrate how easy it is, we’ll build an Android To-Do List application that:</span></span>

* <span data-ttu-id="ce855-112">Hämtar åtkomst till token för att anropa API en att göra-lista med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce855-112">Gets access tokens for calling a To-Do List API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="ce855-113">Hämtar uppgiftslista för en användare.</span><span class="sxs-lookup"><span data-stu-id="ce855-113">Gets a user's to-do list.</span></span>
* <span data-ttu-id="ce855-114">Loggar ut användare.</span><span class="sxs-lookup"><span data-stu-id="ce855-114">Signs out users.</span></span>

<span data-ttu-id="ce855-115">tooget igång, behöver du en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="ce855-115">tooget started, you need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="ce855-116">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ce855-116">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a><span data-ttu-id="ce855-117">Steg 1: Hämta och köra hello Node.js REST API TODO exempelserver</span><span class="sxs-lookup"><span data-stu-id="ce855-117">Step 1: Download and run hello Node.js REST API TODO sample server</span></span>
<span data-ttu-id="ce855-118">Hej Node.js REST API TODO exempel skrivs specifikt toowork mot vår befintliga exemplet för att skapa en enskild klient uppgiften REST API för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce855-118">hello Node.js REST API TODO sample is written specifically toowork against our existing sample for building a single-tenant To-Do REST API for Azure AD.</span></span> <span data-ttu-id="ce855-119">Det här är en förutsättning för hello Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="ce855-119">This is a prerequisite for hello Quick Start.</span></span>

<span data-ttu-id="ce855-120">Mer information om hur tooset detta, se vår befintliga prover i [Microsoft Azure Active Directory exempel REST API-tjänsten för Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ce855-120">For information on how tooset this up, see our existing samples in [Microsoft Azure Active Directory Sample REST API Service for Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span></span>


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a><span data-ttu-id="ce855-121">Steg 2: Registrera ditt webb-API med Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="ce855-121">Step 2: Register your web API with your Azure AD tenant</span></span>
<span data-ttu-id="ce855-122">Active Directory har stöd för att lägga till två typer av program:</span><span class="sxs-lookup"><span data-stu-id="ce855-122">Active Directory supports adding two types of applications:</span></span>

- <span data-ttu-id="ce855-123">Web API: er som erbjuder tjänster toousers</span><span class="sxs-lookup"><span data-stu-id="ce855-123">Web APIs that offer services toousers</span></span>
- <span data-ttu-id="ce855-124">Program (som körs på hello web eller på en enhet) som har åtkomst till de webb-API: er</span><span class="sxs-lookup"><span data-stu-id="ce855-124">Applications (running either on hello web or on a device) that access those web APIs</span></span>

<span data-ttu-id="ce855-125">I det här steget du registrera hello webb-API som du kör lokalt för att testa det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ce855-125">In this step, you're registering hello web API that you're running locally for testing this sample.</span></span> <span data-ttu-id="ce855-126">Den här webb-API är normalt en REST-tjänst som är en funktion för erbjudande som du vill att en app tooaccess.</span><span class="sxs-lookup"><span data-stu-id="ce855-126">Normally, this web API is a REST service that's offering functionality that you want an app tooaccess.</span></span> <span data-ttu-id="ce855-127">Azure AD kan du skydda valfri slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ce855-127">Azure AD can help protect any endpoint.</span></span>

<span data-ttu-id="ce855-128">Förutsätter vi att att du registrera hello TODO REST-API som refererar till tidigare.</span><span class="sxs-lookup"><span data-stu-id="ce855-128">We're assuming that you're registering hello TODO REST API referenced earlier.</span></span> <span data-ttu-id="ce855-129">Men det här fungerar för alla webb-API som du vill skydda toohelp för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ce855-129">But this works for any web API that you want Azure Active Directory toohelp protect.</span></span>

1. <span data-ttu-id="ce855-130">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ce855-130">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ce855-131">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="ce855-131">On hello top bar, click your account.</span></span> <span data-ttu-id="ce855-132">I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce855-132">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="ce855-133">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ce855-133">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ce855-134">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ce855-134">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ce855-135">Ange ett eget namn för programmet hello (till exempel **TodoListService**), Välj **webbprogram och/eller webb-API**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ce855-135">Enter a friendly name for hello application (for example, **TodoListService**), select **Web Application and/or Web API**, and click **Next**.</span></span>
6. <span data-ttu-id="ce855-136">Ange hello bas-URL för hello inloggnings-URL, hello exempel.</span><span class="sxs-lookup"><span data-stu-id="ce855-136">For hello sign-on URL, enter hello base URL for hello sample.</span></span> <span data-ttu-id="ce855-137">Detta är som standard `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="ce855-137">By default, this is `https://localhost:8080`.</span></span>
7. <span data-ttu-id="ce855-138">Klicka på **OK** toocomplete hello registrering.</span><span class="sxs-lookup"><span data-stu-id="ce855-138">Click **OK** toocomplete hello registration.</span></span>
8. <span data-ttu-id="ce855-139">Gå tooyour program sida fortfarande i hello Azure-portalen, hitta hello program-ID-värde och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="ce855-139">While still in hello Azure portal, go tooyour application page, find hello application ID value, and copy it.</span></span> <span data-ttu-id="ce855-140">Du behöver det senare när du konfigurerar ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce855-140">You'll need this later when configuring your application.</span></span>
9. <span data-ttu-id="ce855-141">Från hello **inställningar** -> **egenskaper** sidan, uppdatera hello app-ID URI - ange `https://<your_tenant_name>/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="ce855-141">From hello **Settings** -> **Properties** page, update hello app ID URI - enter `https://<your_tenant_name>/TodoListService`.</span></span> <span data-ttu-id="ce855-142">Ersätt `<your_tenant_name>` med hello namn för din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="ce855-142">Replace `<your_tenant_name>` with hello name of your Azure AD tenant.</span></span>

## <a name="step-3-register-hello-sample-android-native-client-application"></a><span data-ttu-id="ce855-143">Steg 3: Registrera hello exempelprogrammet Android Native Client</span><span class="sxs-lookup"><span data-stu-id="ce855-143">Step 3: Register hello sample Android Native Client application</span></span>
<span data-ttu-id="ce855-144">I det här exemplet måste du registrera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ce855-144">You must register your web application in this sample.</span></span> <span data-ttu-id="ce855-145">Detta gör att dina program toocommunicate med hello just registrerade webb-API.</span><span class="sxs-lookup"><span data-stu-id="ce855-145">This allows your application toocommunicate with hello just-registered web API.</span></span> <span data-ttu-id="ce855-146">Azure AD att neka tooeven bevilja ditt program tooask för inloggning om den är registrerad.</span><span class="sxs-lookup"><span data-stu-id="ce855-146">Azure AD will refuse tooeven allow your application tooask for sign-in unless it's registered.</span></span> <span data-ttu-id="ce855-147">Som är del av hello säker hello modellen.</span><span class="sxs-lookup"><span data-stu-id="ce855-147">That's part of hello security of hello model.</span></span>

<span data-ttu-id="ce855-148">Förutsätter vi att att du registrerar hello exempelprogrammet refererar till tidigare.</span><span class="sxs-lookup"><span data-stu-id="ce855-148">We're assuming that you're registering hello sample application referenced earlier.</span></span> <span data-ttu-id="ce855-149">Men den här metoden fungerar för alla appar som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="ce855-149">But this procedure works for any app that you're developing.</span></span>

> [!NOTE]
> <span data-ttu-id="ce855-150">Du kanske undrar varför du ska lägga ett program och ett webb-API i en klient.</span><span class="sxs-lookup"><span data-stu-id="ce855-150">You might wonder why you're putting both an application and a web API in one tenant.</span></span> <span data-ttu-id="ce855-151">Du kan skapa en app som ansluter till en extern API som är registrerad i Azure AD från en annan klient som du kan ha gissa.</span><span class="sxs-lookup"><span data-stu-id="ce855-151">As you might have guessed, you can build an app that accesses an external API that is registered in Azure AD from another tenant.</span></span> <span data-ttu-id="ce855-152">Om du gör det kan kunderna blir ombedd tooconsent toohello användning av hello API i hello program.</span><span class="sxs-lookup"><span data-stu-id="ce855-152">If you do that, your customers will be prompted tooconsent toohello use of hello API in hello application.</span></span> <span data-ttu-id="ce855-153">Active Directory Authentication Library för iOS hand tar om detta godkännande för dig.</span><span class="sxs-lookup"><span data-stu-id="ce855-153">Active Directory Authentication Library for iOS takes care of this consent for you.</span></span> <span data-ttu-id="ce855-154">Får du veta mer avancerade funktioner, visas detta är en viktig del av hello arbete krävs tooaccess hello uppsättning Microsoft APIs från Azure och Office, samt-leverantör.</span><span class="sxs-lookup"><span data-stu-id="ce855-154">As we explore more advanced features, you'll see that this is an important part of hello work needed tooaccess hello suite of Microsoft APIs from Azure and Office, as well as any other service provider.</span></span> <span data-ttu-id="ce855-155">Just nu eftersom du har registrerat både webb-API och ditt program under hello samma klient visas inte några uppmaningar om samtycke.</span><span class="sxs-lookup"><span data-stu-id="ce855-155">For now, because you registered both your web API and your application under hello same tenant, you won't see any prompts for consent.</span></span> <span data-ttu-id="ce855-156">Detta är vanligtvis hello fallet om du utvecklar ett program för egna företagets toouse.</span><span class="sxs-lookup"><span data-stu-id="ce855-156">This is usually hello case if you're developing an application just for your own company toouse.</span></span>

1. <span data-ttu-id="ce855-157">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ce855-157">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ce855-158">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="ce855-158">On hello top bar, click your account.</span></span> <span data-ttu-id="ce855-159">I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce855-159">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="ce855-160">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ce855-160">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ce855-161">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ce855-161">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ce855-162">Ange ett eget namn för programmet hello (till exempel **TodoListClient Android**), Välj **internt klientprogram**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ce855-162">Enter a friendly name for hello application (for example, **TodoListClient-Android**), select **Native Client Application**, and click **Next**.</span></span>
6. <span data-ttu-id="ce855-163">För hello omdirigerings-URI, ange `http://TodoListClient`.</span><span class="sxs-lookup"><span data-stu-id="ce855-163">For hello redirect URI, enter `http://TodoListClient`.</span></span> <span data-ttu-id="ce855-164">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ce855-164">Click **Finish**.</span></span>
7. <span data-ttu-id="ce855-165">Hitta hello program-ID-värde från hello program sida och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="ce855-165">From hello application page, find hello application ID value and copy it.</span></span> <span data-ttu-id="ce855-166">Du behöver det senare när du konfigurerar ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce855-166">You'll need this later when configuring your application.</span></span>
8. <span data-ttu-id="ce855-167">Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ce855-167">From hello **Settings** page, select **Required Permissions** and select **Add**.</span></span>  <span data-ttu-id="ce855-168">Leta upp och välj TodoListService, Lägg till hello **åtkomst TodoListService** behörighet under **delegerade behörigheter**, och klicka på **klar**.</span><span class="sxs-lookup"><span data-stu-id="ce855-168">Locate and select TodoListService, add hello **Access TodoListService** permission under **Delegated Permissions**, and click **Done**.</span></span>

<span data-ttu-id="ce855-169">toobuild med Maven kan du använda pom.xml toppnivå hello:</span><span class="sxs-lookup"><span data-stu-id="ce855-169">toobuild with Maven, you can use pom.xml at hello top level:</span></span>

1. <span data-ttu-id="ce855-170">Klona den här lagringsplatsen i en katalog som du väljer:</span><span class="sxs-lookup"><span data-stu-id="ce855-170">Clone this repo into a directory of your choice:</span></span>

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. <span data-ttu-id="ce855-171">Åtgärderna i hello hello [krav tooset konfigurera Maven miljön för Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span><span class="sxs-lookup"><span data-stu-id="ce855-171">Follow hello steps in hello [prerequisites tooset up your Maven environment for Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span></span>
3. <span data-ttu-id="ce855-172">Ställ in hello emulator med SDK-19.</span><span class="sxs-lookup"><span data-stu-id="ce855-172">Set up hello emulator with SDK 19.</span></span>
4. <span data-ttu-id="ce855-173">Gå toohello rotmappen där du har klonat hello lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ce855-173">Go toohello root folder where you cloned hello repo.</span></span>
5. <span data-ttu-id="ce855-174">Kör det här kommandot:`mvn clean install`</span><span class="sxs-lookup"><span data-stu-id="ce855-174">Run this command: `mvn clean install`</span></span>
6. <span data-ttu-id="ce855-175">Ändra hello directory toohello Snabbstart exempel:`cd samples\hello`</span><span class="sxs-lookup"><span data-stu-id="ce855-175">Change hello directory toohello Quick Start sample: `cd samples\hello`</span></span>
7. <span data-ttu-id="ce855-176">Kör det här kommandot:`mvn android:deploy android:run`</span><span class="sxs-lookup"><span data-stu-id="ce855-176">Run this command: `mvn android:deploy android:run`</span></span>

   <span data-ttu-id="ce855-177">Du bör se hello app startar.</span><span class="sxs-lookup"><span data-stu-id="ce855-177">You should see hello app starting.</span></span>
8. <span data-ttu-id="ce855-178">Ange test användarens autentiseringsuppgifter tootry.</span><span class="sxs-lookup"><span data-stu-id="ce855-178">Enter test user credentials tootry.</span></span>

<span data-ttu-id="ce855-179">JAR paket skickas bredvid hello AAR paketet.</span><span class="sxs-lookup"><span data-stu-id="ce855-179">JAR packages will be submitted beside hello AAR package.</span></span>

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a><span data-ttu-id="ce855-180">Steg 4: Hämta hello Android ADAL och lägga till den tooyour Eclipse arbetsytan</span><span class="sxs-lookup"><span data-stu-id="ce855-180">Step 4: Download hello Android ADAL and add it tooyour Eclipse workspace</span></span>
<span data-ttu-id="ce855-181">Vi har gjort det enkelt för toohave du flera alternativ toouse ADAL i din Android-projekt:</span><span class="sxs-lookup"><span data-stu-id="ce855-181">We've made it easy for you toohave multiple options toouse ADAL in your Android project:</span></span>

* <span data-ttu-id="ce855-182">Du kan använda hello källa kod tooimport det här biblioteket i Eclipse och länka tooyour program.</span><span class="sxs-lookup"><span data-stu-id="ce855-182">You can use hello source code tooimport this library into Eclipse and link tooyour application.</span></span>
* <span data-ttu-id="ce855-183">Om du använder Android Studio kan du använda hello AAR paketet format och referens hello binärfiler.</span><span class="sxs-lookup"><span data-stu-id="ce855-183">If you're using Android Studio, you can use hello AAR package format and reference hello binaries.</span></span>

### <a name="option-1-source-zip"></a><span data-ttu-id="ce855-184">Alternativ 1: Källa Zip</span><span class="sxs-lookup"><span data-stu-id="ce855-184">Option 1: Source Zip</span></span>
<span data-ttu-id="ce855-185">toodownload en kopia av hello källkoden klickar du på **hämta ZIP** hello höger på sidan hello.</span><span class="sxs-lookup"><span data-stu-id="ce855-185">toodownload a copy of hello source code, click **Download ZIP** on hello right side of hello page.</span></span> <span data-ttu-id="ce855-186">Du kan också [ladda ned från GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span><span class="sxs-lookup"><span data-stu-id="ce855-186">Or you can [download from GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span></span>

### <a name="option-2-source-via-git"></a><span data-ttu-id="ce855-187">Alternativ 2: Källa via Git</span><span class="sxs-lookup"><span data-stu-id="ce855-187">Option 2: Source via Git</span></span>
<span data-ttu-id="ce855-188">tooget hello källkoden för hello SDK via Git, skriver du:</span><span class="sxs-lookup"><span data-stu-id="ce855-188">tooget hello source code of hello SDK via Git, type:</span></span>

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a><span data-ttu-id="ce855-189">Alternativ 3: Binärfiler via Gradle</span><span class="sxs-lookup"><span data-stu-id="ce855-189">Option 3: Binaries via Gradle</span></span>
<span data-ttu-id="ce855-190">Du kan hämta hello binärfiler från hello Maven centrala lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ce855-190">You can get hello binaries from hello Maven central repo.</span></span> <span data-ttu-id="ce855-191">Hej AAR paketet kan ingå i ditt projekt i Android Studio enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ce855-191">hello AAR package can be included as follows in your project in Android Studio:</span></span>

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a><span data-ttu-id="ce855-192">Alternativ 4: AAR via Maven</span><span class="sxs-lookup"><span data-stu-id="ce855-192">Option 4: AAR via Maven</span></span>
<span data-ttu-id="ce855-193">Om du använder hello M2Eclipse plugin-program kan du ange hello beroende i filen pom.xml:</span><span class="sxs-lookup"><span data-stu-id="ce855-193">If you're using hello M2Eclipse plug-in, you can specify hello dependency in your pom.xml file:</span></span>

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a><span data-ttu-id="ce855-194">Alternativ 5: JAR-paketet i hello biblioteksmappen</span><span class="sxs-lookup"><span data-stu-id="ce855-194">Option 5: JAR package inside hello libs folder</span></span>
<span data-ttu-id="ce855-195">Du kan hämta hello JAR-filen från hello Maven lagringsplatsen och släpp hello **libs** mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="ce855-195">You can get hello JAR file from hello Maven repo and drop it into hello **libs** folder in your project.</span></span> <span data-ttu-id="ce855-196">Du behöver toocopy hello nödvändiga resurser tooyour projekt, eftersom hello JAR paket inte inkluderas.</span><span class="sxs-lookup"><span data-stu-id="ce855-196">You need toocopy hello required resources tooyour project as well, because hello JAR packages don't include them.</span></span>

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a><span data-ttu-id="ce855-197">Steg 5: Lägg till referenser tooAndroid ADAL tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="ce855-197">Step 5: Add references tooAndroid ADAL tooyour project</span></span>
1. <span data-ttu-id="ce855-198">Lägga till en referens tooyour projekt och ange den som en Android-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ce855-198">Add a reference tooyour project and specify it as an Android library.</span></span> <span data-ttu-id="ce855-199">Om du är osäker på hur toodo, du kan få mer information om hello [Android Studio plats](http://developer.android.com/tools/projects/projects-eclipse.html).</span><span class="sxs-lookup"><span data-stu-id="ce855-199">If you're uncertain how toodo this, you can get more information on hello [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).</span></span>
2. <span data-ttu-id="ce855-200">Lägg till hello projektet beroende för felsökning i dina Projektinställningar.</span><span class="sxs-lookup"><span data-stu-id="ce855-200">Add hello project dependency for debugging into your project settings.</span></span>
3. <span data-ttu-id="ce855-201">Uppdatera ditt projekt AndroidManifest.xml filen tooinclude:</span><span class="sxs-lookup"><span data-stu-id="ce855-201">Update your project's AndroidManifest.xml file tooinclude:</span></span>

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. <span data-ttu-id="ce855-202">Skapa en instans av AuthenticationContext på din huvudaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="ce855-202">Create an instance of AuthenticationContext at your main activity.</span></span> <span data-ttu-id="ce855-203">hello information om det här anropet är utanför hello i det här avsnittet, men du får en bra start genom att titta på hello [Android Native Client exempel](https://github.com/AzureADSamples/NativeClient-Android).</span><span class="sxs-lookup"><span data-stu-id="ce855-203">hello details of this call are beyond hello scope of this topic, but you can get a good start by looking at hello [Android Native Client sample](https://github.com/AzureADSamples/NativeClient-Android).</span></span> <span data-ttu-id="ce855-204">I följande exempel hello, SharedPreferences är hello standard cache och utfärdare är i hello form av `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span><span class="sxs-lookup"><span data-stu-id="ce855-204">In hello following example, SharedPreferences is hello default cache, and Authority is in hello form of `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span></span>

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. <span data-ttu-id="ce855-205">Kopiera den här koden block toohandle hello slutet av AuthenticationActivity när hello användaren anger autentiseringsuppgifter och tar emot en Auktoriseringskoden:</span><span class="sxs-lookup"><span data-stu-id="ce855-205">Copy this code block toohandle hello end of AuthenticationActivity after hello user enters credentials and receives an authorization code:</span></span>

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. <span data-ttu-id="ce855-206">tooask för en token definierar ett motanropskontrakt:</span><span class="sxs-lookup"><span data-stu-id="ce855-206">tooask for a token, define a callback:</span></span>

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. <span data-ttu-id="ce855-207">Slutligen be om en token med hjälp av att motringning:</span><span class="sxs-lookup"><span data-stu-id="ce855-207">Finally, ask for a token by using that callback:</span></span>

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

<span data-ttu-id="ce855-208">Här följer en förklaring av hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="ce855-208">Here's an explanation of hello parameters:</span></span>

* <span data-ttu-id="ce855-209">*resursen* krävs och är hello-resurs som du försöker tooaccess.</span><span class="sxs-lookup"><span data-stu-id="ce855-209">*resource* is required and is hello resource you're trying tooaccess.</span></span>
* <span data-ttu-id="ce855-210">*clientid* krävs och kommer från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce855-210">*clientid* is required and comes from Azure AD.</span></span>
* <span data-ttu-id="ce855-211">*RedirectUri* är inte obligatoriska toobe för hello acquireToken anrop.</span><span class="sxs-lookup"><span data-stu-id="ce855-211">*RedirectUri* is not required toobe provided for hello acquireToken call.</span></span> <span data-ttu-id="ce855-212">Du kan konfigurera den som ditt paketnamn.</span><span class="sxs-lookup"><span data-stu-id="ce855-212">You can set it up as your package name.</span></span>
* <span data-ttu-id="ce855-213">*PromptBehavior* hjälper tooask för autentiseringsuppgifter tooskip hello cache och cookie.</span><span class="sxs-lookup"><span data-stu-id="ce855-213">*PromptBehavior* helps tooask for credentials tooskip hello cache and cookie.</span></span>
* <span data-ttu-id="ce855-214">*motringning* anropas efter hello auktoriseringskod byts ut mot en token.</span><span class="sxs-lookup"><span data-stu-id="ce855-214">*callback* is called after hello authorization code is exchanged for a token.</span></span> <span data-ttu-id="ce855-215">Den har ett objekt av AuthenticationResult som har åtkomst-token, datum har upphört att gälla och ID tokeninformation.</span><span class="sxs-lookup"><span data-stu-id="ce855-215">It has an object of AuthenticationResult, which has access token, date expired, and ID token information.</span></span>
* <span data-ttu-id="ce855-216">*acquireTokenSilent* är valfritt.</span><span class="sxs-lookup"><span data-stu-id="ce855-216">*acquireTokenSilent* is optional.</span></span> <span data-ttu-id="ce855-217">Du kan anropa den toohandle cachelagring och uppdatera token.</span><span class="sxs-lookup"><span data-stu-id="ce855-217">You can call it toohandle caching and token refresh.</span></span> <span data-ttu-id="ce855-218">Det ger också hello synkroniserade versionen.</span><span class="sxs-lookup"><span data-stu-id="ce855-218">It also provides hello sync version.</span></span> <span data-ttu-id="ce855-219">Den godkänner *userId* som en parameter.</span><span class="sxs-lookup"><span data-stu-id="ce855-219">It accepts *userId* as a parameter.</span></span>

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="ce855-220">Du bör ha vad du behöver toosuccessfully integrera med Azure Active Directory med hjälp av den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="ce855-220">By using this walkthrough, you should have what you need toosuccessfully integrate with Azure Active Directory.</span></span> <span data-ttu-id="ce855-221">Fler exempel på detta fungerar finns i hello AzureADSamples / databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="ce855-221">For more examples of this working, visit hello AzureADSamples/ repository on GitHub.</span></span>

## <a name="important-information"></a><span data-ttu-id="ce855-222">Viktig information</span><span class="sxs-lookup"><span data-stu-id="ce855-222">Important information</span></span>
### <a name="customization"></a><span data-ttu-id="ce855-223">Anpassning</span><span class="sxs-lookup"><span data-stu-id="ce855-223">Customization</span></span>
<span data-ttu-id="ce855-224">Programresurser kan skriva över biblioteksresurser för projektet.</span><span class="sxs-lookup"><span data-stu-id="ce855-224">Your application resources can overwrite library project resources.</span></span> <span data-ttu-id="ce855-225">Detta händer när din app skapas.</span><span class="sxs-lookup"><span data-stu-id="ce855-225">This happens when your app is being built.</span></span> <span data-ttu-id="ce855-226">Därför kan du anpassa autentisering aktivitet layout hello önskemål.</span><span class="sxs-lookup"><span data-stu-id="ce855-226">For this reason, you can customize authentication activity layout hello way you want.</span></span> <span data-ttu-id="ce855-227">Vara säker på att tookeep hello ID hello kontroller använder som ADAL (Webbvy).</span><span class="sxs-lookup"><span data-stu-id="ce855-227">Be sure tookeep hello ID of hello controls that ADAL uses (WebView).</span></span>

### <a name="broker"></a><span data-ttu-id="ce855-228">Service Broker</span><span class="sxs-lookup"><span data-stu-id="ce855-228">Broker</span></span>
<span data-ttu-id="ce855-229">hello Microsoft Intunes företagsportalapp innehåller hello broker komponent.</span><span class="sxs-lookup"><span data-stu-id="ce855-229">hello Microsoft Intune Company Portal app provides hello broker component.</span></span> <span data-ttu-id="ce855-230">hello-konto har skapats i AccountManager.</span><span class="sxs-lookup"><span data-stu-id="ce855-230">hello account is created in AccountManager.</span></span> <span data-ttu-id="ce855-231">hello kontotypen är ”com.microsoft.workaccount”.</span><span class="sxs-lookup"><span data-stu-id="ce855-231">hello account type is "com.microsoft.workaccount."</span></span> <span data-ttu-id="ce855-232">AccountManager kan bara ett enda SSO-konto.</span><span class="sxs-lookup"><span data-stu-id="ce855-232">AccountManager allows only a single SSO account.</span></span> <span data-ttu-id="ce855-233">En SSO-cookie för hello användare skapas när du har slutfört hello enheten utmaning för någon av hello appar.</span><span class="sxs-lookup"><span data-stu-id="ce855-233">It creates an SSO cookie for hello user after completing hello device challenge for one of hello apps.</span></span>

<span data-ttu-id="ce855-234">ADAL använder hello broker konto om ett användarkonto skapas vid denna autentiserare och du väljer att inte tooskip den.</span><span class="sxs-lookup"><span data-stu-id="ce855-234">ADAL uses hello broker account if one user account is created at this authenticator and you choose not tooskip it.</span></span> <span data-ttu-id="ce855-235">Du kan hoppa över hello broker användare med:</span><span class="sxs-lookup"><span data-stu-id="ce855-235">You can skip hello broker user with:</span></span>

   `AuthenticationSettings.Instance.setSkipBroker(true);`

<span data-ttu-id="ce855-236">Du behöver tooregister särskilda RedirectUri för broker användning.</span><span class="sxs-lookup"><span data-stu-id="ce855-236">You need tooregister a special RedirectUri for broker usage.</span></span> <span data-ttu-id="ce855-237">RedirectUri har formatet hello av `msauth://packagename/Base64UrlencodedSignature`.</span><span class="sxs-lookup"><span data-stu-id="ce855-237">RedirectUri is in hello format of `msauth://packagename/Base64UrlencodedSignature`.</span></span> <span data-ttu-id="ce855-238">Du kan hämta din RedirectUri för din app med hjälp av hello skriptet brokerRedirectPrint.ps1 eller mContext.getBrokerRedirectUri för hello API-anrop.</span><span class="sxs-lookup"><span data-stu-id="ce855-238">You can get your RedirectUri for your app by using hello script brokerRedirectPrint.ps1 or hello API call mContext.getBrokerRedirectUri.</span></span> <span data-ttu-id="ce855-239">hello signaturen är relaterade tooyour signera certifikat.</span><span class="sxs-lookup"><span data-stu-id="ce855-239">hello signature is related tooyour signing certificates.</span></span>

<span data-ttu-id="ce855-240">hello aktuella broker modellen är för en användare.</span><span class="sxs-lookup"><span data-stu-id="ce855-240">hello current broker model is for one user.</span></span> <span data-ttu-id="ce855-241">AuthenticationContext ger hello API-metoden tooget hello broker användare.</span><span class="sxs-lookup"><span data-stu-id="ce855-241">AuthenticationContext provides hello API method tooget hello broker user.</span></span>

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

<span data-ttu-id="ce855-242">App-manifest ska ha följande behörigheter toouse AccountManager konton hello.</span><span class="sxs-lookup"><span data-stu-id="ce855-242">Your app manifest should have hello following permissions toouse AccountManager accounts.</span></span> <span data-ttu-id="ce855-243">Mer information finns i hello [AccountManager information på webbplatsen för Android hello](http://developer.android.com/reference/android/accounts/AccountManager.html).</span><span class="sxs-lookup"><span data-stu-id="ce855-243">For details, see hello [AccountManager information on hello Android site](http://developer.android.com/reference/android/accounts/AccountManager.html).</span></span>

* <span data-ttu-id="ce855-244">GET_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="ce855-244">GET_ACCOUNTS</span></span>
* <span data-ttu-id="ce855-245">USE_CREDENTIALS</span><span class="sxs-lookup"><span data-stu-id="ce855-245">USE_CREDENTIALS</span></span>
* <span data-ttu-id="ce855-246">MANAGE_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="ce855-246">MANAGE_ACCOUNTS</span></span>

### <a name="authority-url-and-ad-fs"></a><span data-ttu-id="ce855-247">URL: en utfärdare och AD FS</span><span class="sxs-lookup"><span data-stu-id="ce855-247">Authority URL and AD FS</span></span>
<span data-ttu-id="ce855-248">Active Directory Federation Services (AD FS) identifieras inte som produktion STS, så du behöver tooturn för identifiering av instansen och ange parametern false på hello AuthenticationContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ce855-248">Active Directory Federation Services (AD FS) is not recognized as production STS, so you need tooturn of instance discovery and pass false at hello AuthenticationContext constructor.</span></span>

<span data-ttu-id="ce855-249">hello myndigheten URL måste en STS-instans och en [klientnamn](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="ce855-249">hello authority URL needs an STS instance and a [tenant name](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span></span>

### <a name="querying-cache-items"></a><span data-ttu-id="ce855-250">Fråga cacheobjekt</span><span class="sxs-lookup"><span data-stu-id="ce855-250">Querying cache items</span></span>
<span data-ttu-id="ce855-251">ADAL tillhandahåller en standard-cache i SharedPreferences med några enkla cache frågan funktioner.</span><span class="sxs-lookup"><span data-stu-id="ce855-251">ADAL provides a default cache in SharedPreferences with some simple cache query functions.</span></span> <span data-ttu-id="ce855-252">Du kan hämta hello aktuell cache från AuthenticationContext med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="ce855-252">You can get hello current cache from AuthenticationContext by using:</span></span>

    ITokenCacheStore cache = mContext.getCache();

<span data-ttu-id="ce855-253">Du kan också tillhandahålla implementeringen cachen om du vill toocustomize den.</span><span class="sxs-lookup"><span data-stu-id="ce855-253">You can also provide your cache implementation, if you want toocustomize it.</span></span>

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a><span data-ttu-id="ce855-254">Fråga beteende</span><span class="sxs-lookup"><span data-stu-id="ce855-254">Prompt behavior</span></span>
<span data-ttu-id="ce855-255">ADAL ger ett alternativ toospecify fråga beteende.</span><span class="sxs-lookup"><span data-stu-id="ce855-255">ADAL provides an option toospecify prompt behavior.</span></span> <span data-ttu-id="ce855-256">PromptBehavior.Auto visar hello Användargränssnittet om hello uppdateringstoken är ogiltig och autentiseringsuppgifter krävs.</span><span class="sxs-lookup"><span data-stu-id="ce855-256">PromptBehavior.Auto will show hello UI if hello refresh token is invalid and user credentials are required.</span></span> <span data-ttu-id="ce855-257">PromptBehavior.Always hoppa över hello cache användning och Visa alltid hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ce855-257">PromptBehavior.Always will skip hello cache usage and always show hello UI.</span></span>

### <a name="silent-token-request-from-cache-and-refresh"></a><span data-ttu-id="ce855-258">Tyst tokenbegäran från cache och uppdatera</span><span class="sxs-lookup"><span data-stu-id="ce855-258">Silent token request from cache and refresh</span></span>
<span data-ttu-id="ce855-259">En tyst tokenbegäran använder inte hello popup-Användargränssnittet och kräver inte en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ce855-259">A silent token request does not use hello UI pop-up and does not require an activity.</span></span> <span data-ttu-id="ce855-260">Den returnerar en token från hello cachen om de är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ce855-260">It returns a token from hello cache if available.</span></span> <span data-ttu-id="ce855-261">Om hello token har upphört att gälla den här metoden försöker toorefresh den.</span><span class="sxs-lookup"><span data-stu-id="ce855-261">If hello token is expired, this method tries toorefresh it.</span></span> <span data-ttu-id="ce855-262">Om hello uppdateringstoken har upphört att gälla eller inte fungerar, returnerar AuthenticationException.</span><span class="sxs-lookup"><span data-stu-id="ce855-262">If hello refresh token is expired or failed, it returns AuthenticationException.</span></span>

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="ce855-263">Du kan också göra en synkronisering anropet med hjälp av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="ce855-263">You can also make a sync call by using this method.</span></span> <span data-ttu-id="ce855-264">Du kan ange null toocallback eller använda acquireTokenSilentSync.</span><span class="sxs-lookup"><span data-stu-id="ce855-264">You can set null toocallback or use acquireTokenSilentSync.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="ce855-265">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="ce855-265">Diagnostics</span></span>
<span data-ttu-id="ce855-266">Dessa är hello primära informationskällor för att diagnostisera problem:</span><span class="sxs-lookup"><span data-stu-id="ce855-266">These are hello primary sources of information for diagnosing issues:</span></span>

* <span data-ttu-id="ce855-267">Undantag</span><span class="sxs-lookup"><span data-stu-id="ce855-267">Exceptions</span></span>
* <span data-ttu-id="ce855-268">Logs</span><span class="sxs-lookup"><span data-stu-id="ce855-268">Logs</span></span>
* <span data-ttu-id="ce855-269">Nätverksspår</span><span class="sxs-lookup"><span data-stu-id="ce855-269">Network traces</span></span>

<span data-ttu-id="ce855-270">Observera att Korrelations-ID: N är central toohello diagnostik i hello-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ce855-270">Note that correlation IDs are central toohello diagnostics in hello library.</span></span> <span data-ttu-id="ce855-271">Du kan ange dina Korrelations-ID: N på grundval av per begäran om du vill toocorrelate en ADAL begäran med andra åtgärder i koden.</span><span class="sxs-lookup"><span data-stu-id="ce855-271">You can set your correlation IDs on a per-request basis if you want toocorrelate an ADAL request with other operations in your code.</span></span> <span data-ttu-id="ce855-272">Om du inte anger en Korrelations-ID, att ADAL generera ett slumpmässigt.</span><span class="sxs-lookup"><span data-stu-id="ce855-272">If you don't set a correlation ID, ADAL will generate a random one.</span></span> <span data-ttu-id="ce855-273">Alla loggar meddelanden och samtal nätverket kommer sedan stämplad med hello Korrelations-ID</span><span class="sxs-lookup"><span data-stu-id="ce855-273">All log messages and network calls will then be stamped with hello correlation ID.</span></span> <span data-ttu-id="ce855-274">Hej självgenererat ID ändringar för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="ce855-274">hello self-generated ID changes on each request.</span></span>

#### <a name="exceptions"></a><span data-ttu-id="ce855-275">Undantag</span><span class="sxs-lookup"><span data-stu-id="ce855-275">Exceptions</span></span>
<span data-ttu-id="ce855-276">Undantag är först hello diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ce855-276">Exceptions are hello first diagnostic.</span></span> <span data-ttu-id="ce855-277">Vi försök tooprovide användbara felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="ce855-277">We try tooprovide helpful error messages.</span></span> <span data-ttu-id="ce855-278">Om du hittar en som inte är användbara du filen ett problem och berätta för oss.</span><span class="sxs-lookup"><span data-stu-id="ce855-278">If you find one that is not helpful, please file an issue and let us know.</span></span> <span data-ttu-id="ce855-279">Innehåller enheten modellen och SDK-nummer.</span><span class="sxs-lookup"><span data-stu-id="ce855-279">Include device information such as model and SDK number.</span></span>

#### <a name="logs"></a><span data-ttu-id="ce855-280">Logs</span><span class="sxs-lookup"><span data-stu-id="ce855-280">Logs</span></span>
<span data-ttu-id="ce855-281">Du kan konfigurera hello biblioteket toogenerate loggmeddelanden som du kan använda toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="ce855-281">You can configure hello library toogenerate log messages that you can use toohelp diagnose issues.</span></span> <span data-ttu-id="ce855-282">Du kan konfigurera loggning genom att göra hello följande anropa tooconfigure återanrop ADAL som använder toohand av varje loggmeddelande som genereras.</span><span class="sxs-lookup"><span data-stu-id="ce855-282">You configure logging by making hello following call tooconfigure a callback that ADAL will use toohand off each log message as it's generated.</span></span>

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

<span data-ttu-id="ce855-283">Meddelanden kan skrivas tooa anpassade loggfil som visas i följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="ce855-283">Messages can be written tooa custom log file, as shown in hello following code.</span></span> <span data-ttu-id="ce855-284">Tyvärr, det går inte standard för att få loggar från en enhet.</span><span class="sxs-lookup"><span data-stu-id="ce855-284">Unfortunately, there is no standard way of getting logs from a device.</span></span> <span data-ttu-id="ce855-285">Det finns vissa tjänster som kan hjälpa dig med detta.</span><span class="sxs-lookup"><span data-stu-id="ce855-285">There are some services that can help you with this.</span></span> <span data-ttu-id="ce855-286">Du kan också Skriv ditt eget, exempelvis skicka hello tooa filserver.</span><span class="sxs-lookup"><span data-stu-id="ce855-286">You can also invent your own, such as sending hello file tooa server.</span></span>

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

<span data-ttu-id="ce855-287">Dessa är hello loggningsnivåer:</span><span class="sxs-lookup"><span data-stu-id="ce855-287">These are hello logging levels:</span></span>
* <span data-ttu-id="ce855-288">Fel (undantag)</span><span class="sxs-lookup"><span data-stu-id="ce855-288">Error (exceptions)</span></span>
* <span data-ttu-id="ce855-289">Varna (varning)</span><span class="sxs-lookup"><span data-stu-id="ce855-289">Warn (warning)</span></span>
* <span data-ttu-id="ce855-290">Info (kännedom)</span><span class="sxs-lookup"><span data-stu-id="ce855-290">Info (information purposes)</span></span>
* <span data-ttu-id="ce855-291">Utförlig (Mer information)</span><span class="sxs-lookup"><span data-stu-id="ce855-291">Verbose (more details)</span></span>

<span data-ttu-id="ce855-292">Du kan ange hello loggningsnivån så här:</span><span class="sxs-lookup"><span data-stu-id="ce855-292">You set hello log level like this:</span></span>

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 <span data-ttu-id="ce855-293">Alla loggmeddelanden skickas toologcat, tillägg tooany anpassad logg återanrop.</span><span class="sxs-lookup"><span data-stu-id="ce855-293">All log messages are sent toologcat, in addition tooany custom log callbacks.</span></span>
<span data-ttu-id="ce855-294">Du kan få en loggfil tooa från logcat på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ce855-294">You can get a log tooa file from logcat as follows:</span></span>

    adb logcat > "C:\logmsg\logfile.txt"

 <span data-ttu-id="ce855-295">Mer information om GDB kommandon finns hello [logcat information på webbplatsen för Android hello](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span><span class="sxs-lookup"><span data-stu-id="ce855-295">For details about adb commands, see hello [logcat information on hello Android site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span></span>

#### <a name="network-traces"></a><span data-ttu-id="ce855-296">Nätverksspår</span><span class="sxs-lookup"><span data-stu-id="ce855-296">Network traces</span></span>
<span data-ttu-id="ce855-297">Du kan använda olika verktyg toocapture hello HTTP-trafik som genereras av ADAL.</span><span class="sxs-lookup"><span data-stu-id="ce855-297">You can use various tools toocapture hello HTTP traffic that ADAL generates.</span></span>  <span data-ttu-id="ce855-298">Detta är mest användbart om du är bekant med hello OAuth-protokollet eller om du behöver tooprovide diagnostikinformation tooMicrosoft eller andra supportkanaler.</span><span class="sxs-lookup"><span data-stu-id="ce855-298">This is most useful if you're familiar with hello OAuth protocol or if you need tooprovide diagnostic information tooMicrosoft or other support channels.</span></span>

<span data-ttu-id="ce855-299">Fiddler är hello enklaste HTTP spårning verktyg.</span><span class="sxs-lookup"><span data-stu-id="ce855-299">Fiddler is hello easiest HTTP tracing tool.</span></span> <span data-ttu-id="ce855-300">Använd hello följande länkar tooset den upp toocorrectly post ADAL nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="ce855-300">Use hello following links tooset it up toocorrectly record ADAL network traffic.</span></span> <span data-ttu-id="ce855-301">För ett spårning verktyg som Fiddler eller Charles toobe användbar måste du konfigurera den toorecord okrypterade SSL-trafik.</span><span class="sxs-lookup"><span data-stu-id="ce855-301">For a tracing tool like Fiddler or Charles toobe useful, you must configure it toorecord unencrypted SSL traffic.</span></span>  

> [!NOTE]
> <span data-ttu-id="ce855-302">Spår som skapats på det här sättet kan innehålla mycket Privilegierade information, till exempel åtkomsttoken, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ce855-302">Traces generated in this way might contain highly privileged information such as access tokens, usernames, and passwords.</span></span> <span data-ttu-id="ce855-303">Om du använder Produktionskonton, dela inte de med tredje part.</span><span class="sxs-lookup"><span data-stu-id="ce855-303">If you're using production accounts, do not share these traces with third parties.</span></span> <span data-ttu-id="ce855-304">Om du behöver toosupply en trace toosomeone i ordning tooget stöd kan återskapa hello problemet med hjälp av ett tillfälligt konto med användarnamn och lösenord som du inte gör något delning.</span><span class="sxs-lookup"><span data-stu-id="ce855-304">If you need toosupply a trace toosomeone in order tooget support, reproduce hello issue by using a temporary account with usernames and passwords that you don't mind sharing.</span></span>

* <span data-ttu-id="ce855-305">Från hello Telerik webbplats: [ställa in Fiddler för Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span><span class="sxs-lookup"><span data-stu-id="ce855-305">From hello Telerik website: [Setting Up Fiddler For Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span></span>
* <span data-ttu-id="ce855-306">Från GitHub: [konfigurera Fiddler regler för ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span><span class="sxs-lookup"><span data-stu-id="ce855-306">From GitHub: [Configure Fiddler Rules For ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span></span>

### <a name="dialog-mode"></a><span data-ttu-id="ce855-307">Dialogrutan läge</span><span class="sxs-lookup"><span data-stu-id="ce855-307">Dialog mode</span></span>
<span data-ttu-id="ce855-308">Hej acquireToken metoden utan aktivitet stöder Kommandotolken dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ce855-308">hello acquireToken method without activity supports a dialog prompt.</span></span>

### <a name="encryption"></a><span data-ttu-id="ce855-309">Kryptering</span><span class="sxs-lookup"><span data-stu-id="ce855-309">Encryption</span></span>
<span data-ttu-id="ce855-310">ADAL krypterar hello token och lagra i SharedPreferences som standard.</span><span class="sxs-lookup"><span data-stu-id="ce855-310">ADAL encrypts hello tokens and store in SharedPreferences by default.</span></span> <span data-ttu-id="ce855-311">Du kan titta på hello StorageHelper toosee hello information.</span><span class="sxs-lookup"><span data-stu-id="ce855-311">You can look at hello StorageHelper class toosee hello details.</span></span> <span data-ttu-id="ce855-312">Android introducerade Android Keystore för 4.3 (API-18) säker lagring av privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="ce855-312">Android introduced Android Keystore for 4.3 (API 18) secure storage of private keys.</span></span> <span data-ttu-id="ce855-313">ADAL används för API-18 och högre.</span><span class="sxs-lookup"><span data-stu-id="ce855-313">ADAL uses that for API 18 and higher.</span></span> <span data-ttu-id="ce855-314">Om du vill använda toouse ADAL för lägre SDK-versioner måste tooprovide en hemlig nyckel på AuthenticationSettings.INSTANCE.setSecretKey.</span><span class="sxs-lookup"><span data-stu-id="ce855-314">If you want toouse ADAL for lower SDK versions, you need tooprovide a secret key at AuthenticationSettings.INSTANCE.setSecretKey.</span></span>

### <a name="oauth2-bearer-challenge"></a><span data-ttu-id="ce855-315">OAuth2 ägar utmaning</span><span class="sxs-lookup"><span data-stu-id="ce855-315">OAuth2 bearer challenge</span></span>
<span data-ttu-id="ce855-316">Hej AuthenticationParameters klassen innehåller funktioner tooget authorization_uri från hello OAuth2 ägar-anrop.</span><span class="sxs-lookup"><span data-stu-id="ce855-316">hello AuthenticationParameters class provides functionality tooget authorization_uri from hello OAuth2 bearer challenge.</span></span>

### <a name="session-cookies-in-webview"></a><span data-ttu-id="ce855-317">Cookies i webbvy</span><span class="sxs-lookup"><span data-stu-id="ce855-317">Session cookies in WebView</span></span>
<span data-ttu-id="ce855-318">Android webbvy rensas inte sessionscookies när hello appen är stängd.</span><span class="sxs-lookup"><span data-stu-id="ce855-318">Android WebView does not clear session cookies after hello app is closed.</span></span> <span data-ttu-id="ce855-319">Du kan hantera som med hjälp av den här exempelkoden:</span><span class="sxs-lookup"><span data-stu-id="ce855-319">You can handle that by using this sample code:</span></span>

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

<span data-ttu-id="ce855-320">Mer information om cookies finns hello [CookieSyncManager information på webbplatsen för Android hello](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span><span class="sxs-lookup"><span data-stu-id="ce855-320">For details about cookies, see hello [CookieSyncManager information on hello Android site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span></span>

### <a name="resource-overrides"></a><span data-ttu-id="ce855-321">Resursen åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="ce855-321">Resource overrides</span></span>
<span data-ttu-id="ce855-322">Hej ADAL-biblioteket innehåller engelska strängar för ProgressDialog meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ce855-322">hello ADAL library includes English strings for ProgressDialog messages.</span></span> <span data-ttu-id="ce855-323">Ditt program ska skriva över om du vill använda lokaliserade strängar.</span><span class="sxs-lookup"><span data-stu-id="ce855-323">Your application should overwrite them if you want localized strings.</span></span>

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a><span data-ttu-id="ce855-324">Dialogrutan för NTLM</span><span class="sxs-lookup"><span data-stu-id="ce855-324">NTLM dialog box</span></span>
<span data-ttu-id="ce855-325">ADAL version 1.1.0 stöder en dialogruta för NTLM som bearbetas via hello onReceivedHttpAuthRequest händelse från WebViewClient.</span><span class="sxs-lookup"><span data-stu-id="ce855-325">ADAL version 1.1.0 supports an NTLM dialog box that is processed through hello onReceivedHttpAuthRequest event from WebViewClient.</span></span> <span data-ttu-id="ce855-326">Du kan anpassa hello layout och strängar för hello dialogruta.</span><span class="sxs-lookup"><span data-stu-id="ce855-326">You can customize hello layout and strings for hello dialog box.</span></span>

### <a name="cross-app-sso"></a><span data-ttu-id="ce855-327">Enkel inloggning mellan appar</span><span class="sxs-lookup"><span data-stu-id="ce855-327">Cross-app SSO</span></span>
<span data-ttu-id="ce855-328">Läs [hur tooenable enkel inloggning mellan appar på Android med hjälp av ADAL](active-directory-sso-android.md).</span><span class="sxs-lookup"><span data-stu-id="ce855-328">Learn [how tooenable cross-app SSO on Android by using ADAL](active-directory-sso-android.md).</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
