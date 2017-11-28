---
title: "aaaAzure AD Cordova komma igång | Microsoft Docs"
description: "Hur toobuild ett Cordova-program som kan integreras med Azure AD för inloggning och anropar Azure AD-skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a><span data-ttu-id="af5d1-103">Integrera Azure AD med en Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="af5d1-103">Integrate Azure AD with an Apache Cordova app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="af5d1-104">Du kan använda Apache Cordova toodevelop HTML5 eller JavaScript-program som kan köras på mobila enheter som fullständiga program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-104">You can use Apache Cordova toodevelop HTML5/JavaScript applications that can run on mobile devices as full-fledged native applications.</span></span> <span data-ttu-id="af5d1-105">Du kan lägga till företagsklass autentisering funktioner tooyour Cordova program med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="af5d1-105">With Azure Active Directory (Azure AD), you can add enterprise-grade authentication capabilities tooyour Cordova applications.</span></span>

<span data-ttu-id="af5d1-106">Ett plugin-programmet Cordova radbryts Azure AD plattformsspecifika SDK: er på iOS, Android, Windows Store och Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="af5d1-106">A Cordova plug-in wraps Azure AD native SDKs on iOS, Android, Windows Store, and Windows Phone.</span></span> <span data-ttu-id="af5d1-107">Genom att använda att plugin-program, kan du utöka din programmet toosupport inloggning med Windows Server Active Directory-konton för dina användare kan få åtkomst tooOffice 365 och Azure API: er och även skydda anrop tooyour egna anpassade-webb-API.</span><span class="sxs-lookup"><span data-stu-id="af5d1-107">By using that plug-in, you can enhance your application toosupport sign-in with your users' Windows Server Active Directory accounts, gain access tooOffice 365 and Azure APIs, and even help protect calls tooyour own custom web API.</span></span>

<span data-ttu-id="af5d1-108">I den här självstudiekursen kommer använder vi hello Apache Cordova-pluginprogrammet för Active Directory Authentication Library (ADAL) tooimprove en enkel app genom att lägga till hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="af5d1-108">In this tutorial, we'll use hello Apache Cordova plug-in for Active Directory Authentication Library (ADAL) tooimprove a simple app by adding hello following features:</span></span>

* <span data-ttu-id="af5d1-109">Autentisera en användare och få en token med bara några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="af5d1-109">With just a few lines of code, authenticate a user and obtain a token.</span></span>
* <span data-ttu-id="af5d1-110">Använder den token tooinvoke hello Graph API tooquery katalogen och visa resultat som hello.</span><span class="sxs-lookup"><span data-stu-id="af5d1-110">Use that token tooinvoke hello Graph API tooquery that directory and display hello results.</span></span>  
* <span data-ttu-id="af5d1-111">Använd hello ADAL token-cache toominimize autentisering efterfrågar hello användare.</span><span class="sxs-lookup"><span data-stu-id="af5d1-111">Use hello ADAL token cache toominimize authentication prompts for hello user.</span></span>

<span data-ttu-id="af5d1-112">toomake dessa förbättringar som du behöver:</span><span class="sxs-lookup"><span data-stu-id="af5d1-112">toomake those improvements, you need to:</span></span>

1. <span data-ttu-id="af5d1-113">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af5d1-113">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="af5d1-114">Lägg till kod tooyour toorequest apptoken.</span><span class="sxs-lookup"><span data-stu-id="af5d1-114">Add code tooyour app toorequest tokens.</span></span>
3. <span data-ttu-id="af5d1-115">Lägg till kod toouse hello token för att fråga efter hello Graph API och visa resultat.</span><span class="sxs-lookup"><span data-stu-id="af5d1-115">Add code toouse hello token for querying hello Graph API and display results.</span></span>
4. <span data-ttu-id="af5d1-116">Skapa hello Cordova-projekt för distribution med alla hello plattformar du vill tootarget, lägga till hello Cordova ADAL plugin-program och testa hello lösning i emulatorerna.</span><span class="sxs-lookup"><span data-stu-id="af5d1-116">Create hello Cordova deployment project with all hello platforms you want tootarget, add hello Cordova ADAL plug-in, and test hello solution in emulators.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af5d1-117">Krav</span><span class="sxs-lookup"><span data-stu-id="af5d1-117">Prerequisites</span></span>
<span data-ttu-id="af5d1-118">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="af5d1-118">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="af5d1-119">En Azure AD-klient där du har ett konto med app development rättigheter.</span><span class="sxs-lookup"><span data-stu-id="af5d1-119">An Azure AD tenant where you have an account with app development rights.</span></span>
* <span data-ttu-id="af5d1-120">En utvecklingsmiljö som har konfigurerats toouse Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="af5d1-120">A development environment that's configured toouse Apache Cordova.</span></span>  

<span data-ttu-id="af5d1-121">Om du har redan skapat, fortsätta direkt toostep 1.</span><span class="sxs-lookup"><span data-stu-id="af5d1-121">If you have both already set up, proceed directly toostep 1.</span></span>

<span data-ttu-id="af5d1-122">Om du inte har en Azure AD-klient kan använda hello [anvisningar om hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="af5d1-122">If you don't have an Azure AD tenant, use hello [instructions on how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="af5d1-123">Om du inte har Apache Cordova ställa in på din dator, installera hello följande:</span><span class="sxs-lookup"><span data-stu-id="af5d1-123">If you don't have Apache Cordova set up on your machine, install hello following:</span></span>

* [<span data-ttu-id="af5d1-124">Git</span><span class="sxs-lookup"><span data-stu-id="af5d1-124">Git</span></span>](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [<span data-ttu-id="af5d1-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="af5d1-125">Node.js</span></span>](https://nodejs.org/download/)
* <span data-ttu-id="af5d1-126">[Cordova CLI](https://cordova.apache.org/) (enkelt kan installeras via NPM Pakethanteraren: `npm install -g cordova`)</span><span class="sxs-lookup"><span data-stu-id="af5d1-126">[Cordova CLI](https://cordova.apache.org/) (can be easily installed via NPM package manager: `npm install -g cordova`)</span></span>

<span data-ttu-id="af5d1-127">hello före installationer bör fungera både på hello dator och på hello Mac.</span><span class="sxs-lookup"><span data-stu-id="af5d1-127">hello preceding installations should work both on hello PC and on hello Mac.</span></span>

<span data-ttu-id="af5d1-128">Varje målplattform har olika förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="af5d1-128">Each target platform has different prerequisites:</span></span>

* <span data-ttu-id="af5d1-129">toobuild och kör en app för Windows Surfplattors eller Windows Phone:</span><span class="sxs-lookup"><span data-stu-id="af5d1-129">toobuild and run an app for Windows Tablet/PC or Windows Phone:</span></span>
  * <span data-ttu-id="af5d1-130">Installera [Visual Studio 2013 för Windows med Update 2 eller senare](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express eller en annan version) eller [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span><span class="sxs-lookup"><span data-stu-id="af5d1-130">Install [Visual Studio 2013 for Windows with Update 2 or later](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express or another version) or [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span></span>

* <span data-ttu-id="af5d1-131">toobuild och kör en app för iOS:</span><span class="sxs-lookup"><span data-stu-id="af5d1-131">toobuild and run an app for iOS:</span></span>

  * <span data-ttu-id="af5d1-132">Installera Xcode 6.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="af5d1-132">Install Xcode 6.x or later.</span></span> <span data-ttu-id="af5d1-133">Ladda ned det från hello [Apple-utvecklare plats](http://developer.apple.com/downloads) eller hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span><span class="sxs-lookup"><span data-stu-id="af5d1-133">Download it from hello [Apple Developer site](http://developer.apple.com/downloads) or hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span></span>
  * <span data-ttu-id="af5d1-134">Installera [ios-sim](https://www.npmjs.org/package/ios-sim).</span><span class="sxs-lookup"><span data-stu-id="af5d1-134">Install [ios-sim](https://www.npmjs.org/package/ios-sim).</span></span> <span data-ttu-id="af5d1-135">Du kan använda den toostart iOS-appar i iOS-simulatorn hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="af5d1-135">You can use it toostart iOS apps in iOS Simulator from hello command line.</span></span> <span data-ttu-id="af5d1-136">(Du kan installera den via hello terminal: `npm install -g ios-sim`.)</span><span class="sxs-lookup"><span data-stu-id="af5d1-136">(You can easily install it via hello terminal: `npm install -g ios-sim`.)</span></span>
* <span data-ttu-id="af5d1-137">toobuild och kör en app för Android:</span><span class="sxs-lookup"><span data-stu-id="af5d1-137">toobuild and run an app for Android:</span></span>

  * <span data-ttu-id="af5d1-138">Installera [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller senare.</span><span class="sxs-lookup"><span data-stu-id="af5d1-138">Install [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or later.</span></span> <span data-ttu-id="af5d1-139">Kontrollera att `JAVA_HOME` (miljövariabeln) är korrekt inställd enligt toohello JDK-installationssökvägen (till exempel C:\Program Files\Java\jdk1.7.0_75).</span><span class="sxs-lookup"><span data-stu-id="af5d1-139">Make sure `JAVA_HOME` (environment variable) is correctly set according toohello JDK installation path (for example, C:\Program Files\Java\jdk1.7.0_75).</span></span>
  * <span data-ttu-id="af5d1-140">Installera [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) och Lägg till hello `<android-sdk-location>\tools` plats (till exempel C:\tools\Android\android-sdk\tools) tooyour `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="af5d1-140">Install [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) and add hello `<android-sdk-location>\tools` location (for example, C:\tools\Android\android-sdk\tools) tooyour `PATH` environment variable.</span></span>
  * <span data-ttu-id="af5d1-141">Öppna Android SDK Manager (till exempel via hello terminal: `android`) och installera:</span><span class="sxs-lookup"><span data-stu-id="af5d1-141">Open Android SDK Manager (for example, via hello terminal: `android`) and install:</span></span>
    * <span data-ttu-id="af5d1-142">*Android 5.0.1 (API 21)* platform SDK</span><span class="sxs-lookup"><span data-stu-id="af5d1-142">*Android 5.0.1 (API 21)* platform SDK</span></span>
    * <span data-ttu-id="af5d1-143">*Android SDK Build Tools* version 19.1.0 eller senare</span><span class="sxs-lookup"><span data-stu-id="af5d1-143">*Android SDK Build Tools* version 19.1.0 or later</span></span>
    * <span data-ttu-id="af5d1-144">*Stöd för Android databasen* (tillägg)</span><span class="sxs-lookup"><span data-stu-id="af5d1-144">*Android Support Repository* (Extras)</span></span>

  <span data-ttu-id="af5d1-145">hello Android SDK ger inte några emulatorn standardinstansen.</span><span class="sxs-lookup"><span data-stu-id="af5d1-145">hello Android SDK doesn't provide any default emulator instance.</span></span> <span data-ttu-id="af5d1-146">Skapa en genom att köra `android avd` från hello terminal och sedan välja **skapa**om du vill toorun hello Android-app på en emulator.</span><span class="sxs-lookup"><span data-stu-id="af5d1-146">Create one by running `android avd` from hello terminal and then selecting **Create**, if you want toorun hello Android app on an emulator.</span></span> <span data-ttu-id="af5d1-147">Vi rekommenderar en API-nivå för 19 eller högre.</span><span class="sxs-lookup"><span data-stu-id="af5d1-147">We recommend an API level of 19 or higher.</span></span> <span data-ttu-id="af5d1-148">Läs mer om alternativen för hello Android-emulatorn och skapa [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) på webbplatsen för Android hello.</span><span class="sxs-lookup"><span data-stu-id="af5d1-148">For more information about hello Android emulator and creation options, see [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) on hello Android site.</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="af5d1-149">Steg 1: Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5d1-149">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="af5d1-150">Det här steget är valfritt.</span><span class="sxs-lookup"><span data-stu-id="af5d1-150">This step is optional.</span></span> <span data-ttu-id="af5d1-151">Den här självstudiekursen innehåller företablerad värden som du kan använda toosee hello exemplet i åtgärden utan att behöva göra några allokering i din egen klient.</span><span class="sxs-lookup"><span data-stu-id="af5d1-151">This tutorial provides pre-provisioned values that you can use toosee hello sample in action without doing any provisioning in your own tenant.</span></span> <span data-ttu-id="af5d1-152">Vi rekommenderar dock att du utför det här steget och bekanta dig med hello process, eftersom den är obligatorisk när du skapar dina egna program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-152">However, we recommend that you do perform this step and become familiar with hello process, because it will be required when you create your own applications.</span></span>

<span data-ttu-id="af5d1-153">Azure AD utfärdar token tooonly kända program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-153">Azure AD issues tokens tooonly known applications.</span></span> <span data-ttu-id="af5d1-154">Innan du kan använda Azure AD från din app, behöver du toocreate en post för den i din klient.</span><span class="sxs-lookup"><span data-stu-id="af5d1-154">Before you can use Azure AD from your app, you need toocreate an entry for it in your tenant.</span></span> <span data-ttu-id="af5d1-155">tooregister ett nytt program i din klient:</span><span class="sxs-lookup"><span data-stu-id="af5d1-155">tooregister a new application in your tenant:</span></span>

1. <span data-ttu-id="af5d1-156">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af5d1-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="af5d1-157">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="af5d1-157">On hello top bar, click your account.</span></span> <span data-ttu-id="af5d1-158">I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-158">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="af5d1-159">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-159">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="af5d1-160">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-160">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="af5d1-161">Följ anvisningarna för hello och skapa en **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-161">Follow hello prompts and create a **Native Client Application**.</span></span> <span data-ttu-id="af5d1-162">(Men Cordova-appar är HTML-baserad, vi skapar en här native client-program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-162">(Although Cordova apps are HTML based, we're creating a native client application here.</span></span> <span data-ttu-id="af5d1-163">Hej **internt klientprogram** alternativet måste väljas eller hello program fungerar inte.)</span><span class="sxs-lookup"><span data-stu-id="af5d1-163">hello **Native Client Application** option must be selected, or hello application won't work.)</span></span>
  * <span data-ttu-id="af5d1-164">**Namnet** beskriver toousers ditt program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-164">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="af5d1-165">**Omdirigerings-URI** är hello URI som har använt tooreturn token tooyour app.</span><span class="sxs-lookup"><span data-stu-id="af5d1-165">**Redirect URI** is hello URI that's used tooreturn tokens tooyour app.</span></span> <span data-ttu-id="af5d1-166">Ange **http://MyDirectorySearcherApp**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-166">Enter **http://MyDirectorySearcherApp**.</span></span>

<span data-ttu-id="af5d1-167">När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af5d1-167">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="af5d1-168">Du behöver det här värdet i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="af5d1-168">You’ll need this value in hello next sections.</span></span> <span data-ttu-id="af5d1-169">Du hittar den på hello programmet flik i hello nyskapad app.</span><span class="sxs-lookup"><span data-stu-id="af5d1-169">You can find it on hello application tab of hello newly created app.</span></span>

<span data-ttu-id="af5d1-170">toorun `DirSearchClient Sample`, bevilja hello nyligen skapade appen behörighet tooquery hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="af5d1-170">toorun `DirSearchClient Sample`, grant hello newly created app permission tooquery hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="af5d1-171">Från hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-171">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>  
2. <span data-ttu-id="af5d1-172">Hello Azure Active Directory-program, Välj **Microsoft Graph** som hello API och Lägg till hello **Access hello-katalogen som hello inloggade användare** behörighet under **delegerade Behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="af5d1-172">For hello Azure Active Directory application, select **Microsoft Graph** as hello API and add hello **Access hello directory as hello signed-in user** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="af5d1-173">Detta gör att dina program tooquery hello Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="af5d1-173">This enables your application tooquery hello Graph API for users.</span></span>

## <a name="step-2-clone-hello-sample-app-repository"></a><span data-ttu-id="af5d1-174">Steg 2: Klonar hello exempel app lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="af5d1-174">Step 2: Clone hello sample app repository</span></span>
<span data-ttu-id="af5d1-175">Ange hello följande kommando från din gränssnitt eller en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="af5d1-175">From your shell or command line, type hello following command:</span></span>

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a><span data-ttu-id="af5d1-176">Steg 3: Skapa hello Cordova-app</span><span class="sxs-lookup"><span data-stu-id="af5d1-176">Step 3: Create hello Cordova app</span></span>
<span data-ttu-id="af5d1-177">Det finns flera sätt toocreate Cordova program.</span><span class="sxs-lookup"><span data-stu-id="af5d1-177">There are multiple ways toocreate Cordova applications.</span></span> <span data-ttu-id="af5d1-178">I den här kursen använder vi hello Cordova-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="af5d1-178">In this tutorial, we'll use hello Cordova command-line interface (CLI).</span></span>

1. <span data-ttu-id="af5d1-179">Ange hello följande kommando från din gränssnitt eller en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="af5d1-179">From your shell or command line, type hello following command:</span></span>

        cordova create DirSearchClient

   <span data-ttu-id="af5d1-180">Detta kommando skapar hello mappstrukturen och scaffold-teknik för hello Cordova-projekt.</span><span class="sxs-lookup"><span data-stu-id="af5d1-180">That command creates hello folder structure and scaffolding for hello Cordova project.</span></span>

2. <span data-ttu-id="af5d1-181">Flytta toohello ny DirSearchClient mapp:</span><span class="sxs-lookup"><span data-stu-id="af5d1-181">Move toohello new DirSearchClient folder:</span></span>

        cd .\DirSearchClient

3. <span data-ttu-id="af5d1-182">Kopiera hello innehållet hello starter Project i hello www undermapp med hjälp av en Filhanteraren eller hello följande kommando i gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="af5d1-182">Copy hello content of hello starter project in hello www subfolder by using a file manager or hello following command in your shell:</span></span>

  * <span data-ttu-id="af5d1-183">Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span><span class="sxs-lookup"><span data-stu-id="af5d1-183">Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span></span>
  * <span data-ttu-id="af5d1-184">Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span><span class="sxs-lookup"><span data-stu-id="af5d1-184">Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span></span>

4. <span data-ttu-id="af5d1-185">Lägg till hello godkända plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="af5d1-185">Add hello whitelist plug-in.</span></span> <span data-ttu-id="af5d1-186">Detta är nödvändigt för att anropa hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="af5d1-186">This is necessary for invoking hello Graph API.</span></span>

        cordova plugin add cordova-plugin-whitelist

5. <span data-ttu-id="af5d1-187">Lägg till alla hello plattformar som du vill toosupport.</span><span class="sxs-lookup"><span data-stu-id="af5d1-187">Add all hello platforms that you want toosupport.</span></span> <span data-ttu-id="af5d1-188">toohave ett exempel på en fungerande måste tooexecute minst en av följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="af5d1-188">toohave a working sample, you need tooexecute at least one of hello following commands.</span></span> <span data-ttu-id="af5d1-189">Observera att du inte kommer att kunna tooemulate iOS i Windows eller emulera Windows på en Mac.</span><span class="sxs-lookup"><span data-stu-id="af5d1-189">Note that you won't be able tooemulate iOS on Windows or emulate Windows on a Mac.</span></span>

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. <span data-ttu-id="af5d1-190">Lägg till hello ADAL för Cordova-pluginprogrammet tooyour projekt:</span><span class="sxs-lookup"><span data-stu-id="af5d1-190">Add hello ADAL for Cordova plug-in tooyour project:</span></span>

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a><span data-ttu-id="af5d1-191">Steg 4: Lägga till kod tooauthenticate användare och hämta token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5d1-191">Step 4: Add code tooauthenticate users and obtain tokens from Azure AD</span></span>
<span data-ttu-id="af5d1-192">hello-program som du utvecklar i den här kursen får du en enkel directory sökfunktionen.</span><span class="sxs-lookup"><span data-stu-id="af5d1-192">hello application that you're developing in this tutorial will provide a simple directory search feature.</span></span> <span data-ttu-id="af5d1-193">hello-användare kan sedan Skriv hello-alias för alla användare i katalogen, hello och visualisera vissa grundläggande attribut.</span><span class="sxs-lookup"><span data-stu-id="af5d1-193">hello user can then type hello alias of any user in hello directory and visualize some basic attributes.</span></span> <span data-ttu-id="af5d1-194">hello starter projektet innehåller hello definition av hello grundläggande användargränssnitt hello-appen (i www/index.html) och hello scaffold-teknik som dra kablar in grundläggande app händelse cykler användaren gränssnittet bindningar och resultatet visas logik (i www/js/index.js).</span><span class="sxs-lookup"><span data-stu-id="af5d1-194">hello starter project contains hello definition of hello basic user interface of hello app (in www/index.html) and hello scaffolding that wires up basic app event cycles, user interface bindings, and results display logic (in www/js/index.js).</span></span> <span data-ttu-id="af5d1-195">hello är endast uppgift kvar för du tooadd hello logik som implementerar identitet uppgifter.</span><span class="sxs-lookup"><span data-stu-id="af5d1-195">hello only task left for you is tooadd hello logic that implements identity tasks.</span></span>

<span data-ttu-id="af5d1-196">hello måste du först toodo i koden är införa hello protokollet värden som Azure AD använder för att identifiera din app och hello resurser som du mål.</span><span class="sxs-lookup"><span data-stu-id="af5d1-196">hello first thing you need toodo in your code is introduce hello protocol values that Azure AD uses for identifying your app and hello resources that you target.</span></span> <span data-ttu-id="af5d1-197">Dessa värden kommer att använda tooconstruct hello tokenbegäranden vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="af5d1-197">Those values will be used tooconstruct hello token requests later on.</span></span> <span data-ttu-id="af5d1-198">Infoga följande fragment hello överst i hello index.js filen hello:</span><span class="sxs-lookup"><span data-stu-id="af5d1-198">Insert hello following snippet at hello top of hello index.js file:</span></span>

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

<span data-ttu-id="af5d1-199">Hej `redirectUri` och `clientId` värden ska matcha hello värden som beskriver din app i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af5d1-199">hello `redirectUri` and `clientId` values should match hello values that describe your app in Azure AD.</span></span> <span data-ttu-id="af5d1-200">Du hittar dem från hello **konfigurera** fliken i hello Azure-portalen, enligt beskrivningen i steg 1 tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="af5d1-200">You can find those from hello **Configure** tab in hello Azure portal, as described in step 1 earlier in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="af5d1-201">Om du valt för att registrera en ny app inte i en egen klient kan du bara klistra in hello fördefinierade värden som är.</span><span class="sxs-lookup"><span data-stu-id="af5d1-201">If you opted for not registering a new app in your own tenant, you can simply paste hello preconfigured values as is.</span></span> <span data-ttu-id="af5d1-202">Sedan visas hello exempel körs, men du bör alltid skapa din egen post för dina appar som är avsedda för produktion.</span><span class="sxs-lookup"><span data-stu-id="af5d1-202">You can then see hello sample running, though you should always create your own entry for your apps that are meant for production.</span></span>

<span data-ttu-id="af5d1-203">Lägg till hello tokenbegäran kod.</span><span class="sxs-lookup"><span data-stu-id="af5d1-203">Next, add hello token request code.</span></span> <span data-ttu-id="af5d1-204">Infoga följande fragment mellan hello hello `search` och `renderData` definitioner:</span><span class="sxs-lookup"><span data-stu-id="af5d1-204">Insert hello following snippet between hello `search` and `renderData` definitions:</span></span>

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
<span data-ttu-id="af5d1-205">Låt oss nu undersöka som fungerar genom att bryta ned den i sina två delar.</span><span class="sxs-lookup"><span data-stu-id="af5d1-205">Let's examine that function by breaking it down in its two main parts.</span></span>
<span data-ttu-id="af5d1-206">Det här exemplet är utformad toowork med innehavare, som skillnad från toobeing knutna tooa viss en.</span><span class="sxs-lookup"><span data-stu-id="af5d1-206">This sample is designed toowork with any tenant, as opposed toobeing tied tooa particular one.</span></span> <span data-ttu-id="af5d1-207">Den använder hello ”/ vanliga” slutpunkt som tillåter hello användaren tooenter något konto vid autentisering och dirigerar hello begäran toohello klient där den tillhör.</span><span class="sxs-lookup"><span data-stu-id="af5d1-207">It uses hello "/common" endpoint, which allows hello user tooenter any account at authentication time and directs hello request toohello tenant where it belongs.</span></span>

<span data-ttu-id="af5d1-208">Den här första delen av hello metoden kontrollerar om hello ADAL cache toosee om det finns redan en token.</span><span class="sxs-lookup"><span data-stu-id="af5d1-208">This first part of hello method inspects hello ADAL cache toosee if a token is already stored.</span></span> <span data-ttu-id="af5d1-209">I så fall, använder hello-metoden hello klienter där hello-token kommer från för att ADAL.</span><span class="sxs-lookup"><span data-stu-id="af5d1-209">If so, hello method uses hello tenants where hello token came from for reinitializing ADAL.</span></span> <span data-ttu-id="af5d1-210">Detta är nödvändigt tooavoid extra prompter eftersom hello användning av ”/ vanliga” alltid resulterar i ber hello användaren tooenter ett nytt konto.</span><span class="sxs-lookup"><span data-stu-id="af5d1-210">This is necessary tooavoid extra prompts, because hello use of "/common" always results in asking hello user tooenter a new account.</span></span>

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
<span data-ttu-id="af5d1-211">hello andra delen av hello metoden utför hello rätt tokenbegäran.</span><span class="sxs-lookup"><span data-stu-id="af5d1-211">hello second part of hello method performs hello proper token request.</span></span> <span data-ttu-id="af5d1-212">Hej `acquireTokenSilentAsync` metoden ber ADAL tooreturn en token för hello angetts resursen utan att visa alla UX.</span><span class="sxs-lookup"><span data-stu-id="af5d1-212">hello `acquireTokenSilentAsync` method asks ADAL tooreturn a token for hello specified resource without showing any UX.</span></span> <span data-ttu-id="af5d1-213">Som kan inträffa om hello cache redan har en lämplig åtkomst-token som lagras, eller om en uppdateringstoken kan användas tooget en ny åtkomsttoken utan visar prompten.</span><span class="sxs-lookup"><span data-stu-id="af5d1-213">That can happen if hello cache already has a suitable access token stored, or if a refresh token can be used tooget a new access token without showing any prompt.</span></span> <span data-ttu-id="af5d1-214">Om som försöker misslyckas vi återställs `acquireTokenAsync`--som uppmanas synligt hello användaren tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="af5d1-214">If that attempt fails, we fall back on `acquireTokenAsync`--which will visibly prompt hello user tooauthenticate.</span></span>

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
<span data-ttu-id="af5d1-215">Nu när vi har hello token kan vi slutligen anropa hello Graph API och utföra hello sökfråga som vi vill.</span><span class="sxs-lookup"><span data-stu-id="af5d1-215">Now that we have hello token, we can finally invoke hello Graph API and perform hello search query that we want.</span></span> <span data-ttu-id="af5d1-216">Infoga följande fragment nedan hello hello `authenticate` definition:</span><span class="sxs-lookup"><span data-stu-id="af5d1-216">Insert hello following snippet below hello `authenticate` definition:</span></span>

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
<span data-ttu-id="af5d1-217">hello startpunkt filer tillhandahålls en enkel UX för att ange alias för en användare i en textruta.</span><span class="sxs-lookup"><span data-stu-id="af5d1-217">hello starting-point files supplied a simple UX for entering a user's alias in a text box.</span></span> <span data-ttu-id="af5d1-218">Den här metoden används värdet tooconstruct en fråga, kombinera det med hello åtkomst-token, skickar den tooMicrosoft diagram och parsa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="af5d1-218">This method uses that value tooconstruct a query, combine it with hello access token, send it tooMicrosoft Graph, and parse hello results.</span></span> <span data-ttu-id="af5d1-219">Hej `renderData` -metoden finns redan i hello startpunkt filen tar hand om visualisera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="af5d1-219">hello `renderData` method, already present in hello starting-point file, takes care of visualizing hello results.</span></span>

## <a name="step-5-run-hello-app"></a><span data-ttu-id="af5d1-220">Steg 5: Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="af5d1-220">Step 5: Run hello app</span></span>
<span data-ttu-id="af5d1-221">Appen är redo slutligen toorun.</span><span class="sxs-lookup"><span data-stu-id="af5d1-221">Your app is finally ready toorun.</span></span> <span data-ttu-id="af5d1-222">Den är enkel: när hello app startar ange hello-alias för hello-användare som du vill att toolook och klicka sedan på hello.</span><span class="sxs-lookup"><span data-stu-id="af5d1-222">Operating it is simple: when hello app starts, enter hello alias of hello user you want toolook up, and then click hello button.</span></span> <span data-ttu-id="af5d1-223">Du uppmanas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="af5d1-223">You're prompted for authentication.</span></span> <span data-ttu-id="af5d1-224">Hello-attribut för hello genomsöks användare visas på lyckad autentisering och lyckad sökning.</span><span class="sxs-lookup"><span data-stu-id="af5d1-224">Upon successful authentication and successful search, hello attributes of hello searched user are displayed.</span></span>

<span data-ttu-id="af5d1-225">Efterföljande körningar utför hello sökning utan att visa prompten, tack vare toohello förekomst av hello tidigare skaffat token i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="af5d1-225">Subsequent runs will perform hello search without showing any prompt, thanks toohello presence of hello previously acquired token in cache.</span></span>

<span data-ttu-id="af5d1-226">hello konkreta stegen för att köra hello app varierar beroende på plattform.</span><span class="sxs-lookup"><span data-stu-id="af5d1-226">hello concrete steps for running hello app vary by platform.</span></span>

### <a name="windows-10"></a><span data-ttu-id="af5d1-227">Windows 10</span><span class="sxs-lookup"><span data-stu-id="af5d1-227">Windows 10</span></span>
   <span data-ttu-id="af5d1-228">Surfplattors:`cordova run windows --archs=x64 -- --appx=uap`</span><span class="sxs-lookup"><span data-stu-id="af5d1-228">Tablet/PC: `cordova run windows --archs=x64 -- --appx=uap`</span></span>

   <span data-ttu-id="af5d1-229">Mobile (kräver en Windows 10 Mobile-enhet som är ansluten tooa PC):`cordova run windows --archs=arm -- --appx=uap --phone`</span><span class="sxs-lookup"><span data-stu-id="af5d1-229">Mobile (requires a Windows 10 Mobile device connected tooa PC): `cordova run windows --archs=arm -- --appx=uap --phone`</span></span>

   > [!NOTE]
   > <span data-ttu-id="af5d1-230">Under hello kör först, kan du bli ombedd toosign i för utvecklarlicens.</span><span class="sxs-lookup"><span data-stu-id="af5d1-230">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="af5d1-231">Mer information finns i [Developer licens](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="af5d1-231">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-81-tabletpc"></a><span data-ttu-id="af5d1-232">Windows 8.1 Surfplattors</span><span class="sxs-lookup"><span data-stu-id="af5d1-232">Windows 8.1 Tablet/PC</span></span>
   `cordova run windows`

   > [!NOTE]
   > <span data-ttu-id="af5d1-233">Under hello kör först, kan du bli ombedd toosign i för utvecklarlicens.</span><span class="sxs-lookup"><span data-stu-id="af5d1-233">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="af5d1-234">Mer information finns i [Developer licens](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="af5d1-234">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-phone-81"></a><span data-ttu-id="af5d1-235">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="af5d1-235">Windows Phone 8.1</span></span>
   <span data-ttu-id="af5d1-236">toorun på en ansluten enhet:`cordova run windows --device -- --phone`</span><span class="sxs-lookup"><span data-stu-id="af5d1-236">toorun on a connected device: `cordova run windows --device -- --phone`</span></span>

   <span data-ttu-id="af5d1-237">toorun på hello standard-emulatorn:`cordova emulate windows -- --phone`</span><span class="sxs-lookup"><span data-stu-id="af5d1-237">toorun on hello default emulator: `cordova emulate windows -- --phone`</span></span>

   <span data-ttu-id="af5d1-238">Använd `cordova run windows --list -- --phone` toosee alla tillgängliga mål och `cordova run windows --target=<target_name> -- --phone` toorun hello program på en viss enhet eller emulator (till exempel `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span><span class="sxs-lookup"><span data-stu-id="af5d1-238">Use `cordova run windows --list -- --phone` toosee all available targets and `cordova run windows --target=<target_name> -- --phone` toorun hello application on a specific device or emulator (for example, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span></span>

### <a name="android"></a><span data-ttu-id="af5d1-239">Android</span><span class="sxs-lookup"><span data-stu-id="af5d1-239">Android</span></span>
   <span data-ttu-id="af5d1-240">toorun på en ansluten enhet:`cordova run android --device`</span><span class="sxs-lookup"><span data-stu-id="af5d1-240">toorun on a connected device: `cordova run android --device`</span></span>

   <span data-ttu-id="af5d1-241">toorun på hello standard-emulatorn:`cordova emulate android`</span><span class="sxs-lookup"><span data-stu-id="af5d1-241">toorun on hello default emulator: `cordova emulate android`</span></span>

   <span data-ttu-id="af5d1-242">Kontrollera att du har skapat en instans av emulatorn med AVD Manager, som beskrivits tidigare i hello avsnittet ”förutsättningar”.</span><span class="sxs-lookup"><span data-stu-id="af5d1-242">Make sure you've created an emulator instance by using AVD Manager, as described earlier in hello "Prerequisites" section.</span></span>

   <span data-ttu-id="af5d1-243">Använd `cordova run android --list` toosee alla tillgängliga mål och `cordova run android --target=<target_name>` toorun hello program på en viss enhet eller emulator (till exempel `cordova run android --target="Nexus4_emulator"`).</span><span class="sxs-lookup"><span data-stu-id="af5d1-243">Use `cordova run android --list` toosee all available targets and `cordova run android --target=<target_name>` toorun hello application on a specific device or emulator (for example, `cordova run android --target="Nexus4_emulator"`).</span></span>

### <a name="ios"></a><span data-ttu-id="af5d1-244">iOS</span><span class="sxs-lookup"><span data-stu-id="af5d1-244">iOS</span></span>
   <span data-ttu-id="af5d1-245">toorun på en ansluten enhet:`cordova run ios --device`</span><span class="sxs-lookup"><span data-stu-id="af5d1-245">toorun on a connected device: `cordova run ios --device`</span></span>

   <span data-ttu-id="af5d1-246">toorun på hello standard-emulatorn:`cordova emulate ios`</span><span class="sxs-lookup"><span data-stu-id="af5d1-246">toorun on hello default emulator: `cordova emulate ios`</span></span>

   > [!NOTE]
   > <span data-ttu-id="af5d1-247">Kontrollera att du har hello `ios-sim` package installerad toorun på hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="af5d1-247">Make sure you have hello `ios-sim` package installed toorun on hello emulator.</span></span> <span data-ttu-id="af5d1-248">Mer information finns i ”hello” krav ”avsnittet.</span><span class="sxs-lookup"><span data-stu-id="af5d1-248">For more information, see hello "Prerequisites" section.</span></span>

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a><span data-ttu-id="af5d1-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af5d1-249">Next steps</span></span>
<span data-ttu-id="af5d1-250">Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span><span class="sxs-lookup"><span data-stu-id="af5d1-250">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span></span>

<span data-ttu-id="af5d1-251">Du kan nu flytta på toomore avancerade (och mer intressant) scenarier.</span><span class="sxs-lookup"><span data-stu-id="af5d1-251">You can now move on toomore advanced (and more interesting) scenarios.</span></span> <span data-ttu-id="af5d1-252">Du kanske vill tootry: [skydda en Node.js-webb-API med Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="af5d1-252">You might want tootry: [Secure a Node.js Web API with Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
