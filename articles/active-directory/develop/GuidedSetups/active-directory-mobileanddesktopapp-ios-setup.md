---
title: "aaaAzure AD v2 iOS komma igång - inställningar | Microsoft Docs"
description: "Hur iOS (Swift)-program kan anropa ett API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 62c4ee9a2d4ccaec780bee09fb4bc34cff2eb6df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-ios-application"></a><span data-ttu-id="f61c3-103">Konfigurera iOS-program</span><span class="sxs-lookup"><span data-stu-id="f61c3-103">Setting up your iOS application</span></span>

<span data-ttu-id="f61c3-104">Det här avsnittet innehåller stegvisa instruktioner för hur toocreate ett nytt projekt toodemonstrate hur toointegrate ett iOS-program (Swift) med *logga In med Microsoft* så att den kan fråga webb-API: er som kräver en token.</span><span class="sxs-lookup"><span data-stu-id="f61c3-104">This section provides step-by-step instructions for how toocreate a new project toodemonstrate how toointegrate an iOS application (Swift) with *Sign-In with Microsoft* so it can query Web APIs that require a token.</span></span>

> <span data-ttu-id="f61c3-105">Föredrar toodownload det här exemplet XCode-projekt i stället?</span><span class="sxs-lookup"><span data-stu-id="f61c3-105">Prefer toodownload this sample's XCode project instead?</span></span> <span data-ttu-id="f61c3-106">[Hämta ett projekt](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.</span><span class="sxs-lookup"><span data-stu-id="f61c3-106">[Download a project](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>


## <a name="install-carthage-toodownload-and-build-msal"></a><span data-ttu-id="f61c3-107">Installera Carthage toodownload och skapa MSAL</span><span class="sxs-lookup"><span data-stu-id="f61c3-107">Install Carthage toodownload and build MSAL</span></span>
<span data-ttu-id="f61c3-108">Carthage Pakethanteraren används under hello förhandsversionen av MSAL – den kan integreras med XCode samtidigt hello möjligheten för Microsoft toomake ändringar toohello library.</span><span class="sxs-lookup"><span data-stu-id="f61c3-108">Carthage package manager is used during hello preview period of MSAL – it integrates with XCode while maintaining hello ability for Microsoft toomake changes toohello library.</span></span>

- <span data-ttu-id="f61c3-109">Hämta och installera hello senaste versionen av Carthage [här](https://github.com/Carthage/Carthage/releases "Carthage nedladdnings-URL")</span><span class="sxs-lookup"><span data-stu-id="f61c3-109">Download and install hello latest release of Carthage [here](https://github.com/Carthage/Carthage/releases "Carthage download URL")</span></span>

## <a name="creating-your-application"></a><span data-ttu-id="f61c3-110">Skapar ditt program</span><span class="sxs-lookup"><span data-stu-id="f61c3-110">Creating your application</span></span>

1.  <span data-ttu-id="f61c3-111">Öppna Xcode och välj`Create a new Xcode project`</span><span class="sxs-lookup"><span data-stu-id="f61c3-111">Open Xcode and select `Create a new Xcode project`</span></span>
2.  <span data-ttu-id="f61c3-112">Välj `iOS`  >  `Single view Application` och på *nästa*</span><span class="sxs-lookup"><span data-stu-id="f61c3-112">Select `iOS` > `Single view Application` and click *Next*</span></span>
3.  <span data-ttu-id="f61c3-113">Ge ett produktnamn och klicka på *nästa*</span><span class="sxs-lookup"><span data-stu-id="f61c3-113">Give a product name and click *Next*</span></span>
4.  <span data-ttu-id="f61c3-114">Välj en mapp toocreate appen och klicka på *skapa*</span><span class="sxs-lookup"><span data-stu-id="f61c3-114">Select a folder toocreate your app and click *Create*</span></span>

## <a name="build-hello-msal-framework"></a><span data-ttu-id="f61c3-115">Skapa hello MSAL Framework</span><span class="sxs-lookup"><span data-stu-id="f61c3-115">Build hello MSAL Framework</span></span>

<span data-ttu-id="f61c3-116">Följ instruktionerna nedan toopull för hello och skapa hello senaste versionen av MSAL bibliotek med Carthage:</span><span class="sxs-lookup"><span data-stu-id="f61c3-116">Follow hello instructions below toopull and then build hello latest version of MSAL libraries using Carthage:</span></span>

1.  <span data-ttu-id="f61c3-117">Öppna hello bash terminal och gå toohello App rotmappen</span><span class="sxs-lookup"><span data-stu-id="f61c3-117">Open hello bash terminal and go toohello App’s root folder</span></span>
2.  <span data-ttu-id="f61c3-118">Kopiera hello nedan och klistra in i hello bash terminal toocreate en 'Cartfile'-fil:</span><span class="sxs-lookup"><span data-stu-id="f61c3-118">Copy hello below and paste in hello bash terminal toocreate a ‘Cartfile’ file:</span></span>

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
<span data-ttu-id="f61c3-119">Kopiera och klistra in hello nedan.</span><span class="sxs-lookup"><span data-stu-id="f61c3-119">Copy and paste hello below.</span></span> <span data-ttu-id="f61c3-120">Detta kommando hämtar beroenden till en Carthage/utcheckningar mapp och sedan skapar hello MSAL bibliotek:</span><span class="sxs-lookup"><span data-stu-id="f61c3-120">This command fetches dependencies into a Carthage/Checkouts folder, then builds hello MSAL library:</span></span>
</li>
</ol>

```bash
carthage update
```

> <span data-ttu-id="f61c3-121">hello processen ovan är används toodownload och bygga hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="f61c3-121">hello process above is used toodownload and build hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="f61c3-122">MSAL hanterar införskaffa, cachelagring och uppdatera användaren tokens som används för tooaccess API: er som skyddas av hello Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="f61c3-122">MSAL handles acquiring, caching and refreshing user tokens used tooaccess APIs protected by hello Azure Active Directory v2.</span></span>

## <a name="add-hello-msal-framework-tooyour-application"></a><span data-ttu-id="f61c3-123">Lägg till hello MSAL framework tooyour program</span><span class="sxs-lookup"><span data-stu-id="f61c3-123">Add hello MSAL framework tooyour application</span></span>
1.  <span data-ttu-id="f61c3-124">I Xcode öppnar hello `General` fliken</span><span class="sxs-lookup"><span data-stu-id="f61c3-124">In Xcode, open hello `General` tab</span></span>
2.  <span data-ttu-id="f61c3-125">Gå toohello `Linked Frameworks and Libraries` avsnittet och klicka på`+`</span><span class="sxs-lookup"><span data-stu-id="f61c3-125">Go toohello `Linked Frameworks and Libraries` section and click `+`</span></span>
3.  <span data-ttu-id="f61c3-126">Välj `Add other…`</span><span class="sxs-lookup"><span data-stu-id="f61c3-126">Select `Add other…`</span></span>
4.  <span data-ttu-id="f61c3-127">Välj: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` och på *öppna*.</span><span class="sxs-lookup"><span data-stu-id="f61c3-127">Select: `Carthage` > `Build` > `iOS` > `MSAL.framework` and click *Open*.</span></span> <span data-ttu-id="f61c3-128">Du bör se `MSAL.framework` läggs toohello lista.</span><span class="sxs-lookup"><span data-stu-id="f61c3-128">You should see `MSAL.framework` added toohello list.</span></span>
5.  <span data-ttu-id="f61c3-129">Gå för`Build Phases` och på `+` ikon, Välj`New Run Script Phase`</span><span class="sxs-lookup"><span data-stu-id="f61c3-129">Go too`Build Phases` tab, and click `+` icon, choose `New Run Script Phase`</span></span>
6.  <span data-ttu-id="f61c3-130">Lägg till följande innehåll toohello hello *skript området*:</span><span class="sxs-lookup"><span data-stu-id="f61c3-130">Add hello following contents toohello *script area*:</span></span>

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="f61c3-131">Lägg till följande hello för<code>Input Files</code> genom att klicka på <code>+</code>:</span><span class="sxs-lookup"><span data-stu-id="f61c3-131">Add hello following too<code>Input Files</code> by clicking <code>+</code>:</span></span>
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a><span data-ttu-id="f61c3-132">Skapa programmets användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="f61c3-132">Creating your application’s UI</span></span>
<span data-ttu-id="f61c3-133">En Main.storyboard-fil ska skapas automatiskt som en del av projektmallen för.</span><span class="sxs-lookup"><span data-stu-id="f61c3-133">A Main.storyboard file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="f61c3-134">Följ hello anvisningar nedan toocreate hello app UI:</span><span class="sxs-lookup"><span data-stu-id="f61c3-134">Follow hello instructions below toocreate hello app UI:</span></span>

1.  <span data-ttu-id="f61c3-135">CTRL + klicka `Main.storyboard` toobring upp hello popup-menyn och klicka sedan på:`Open As` > `Source Code`</span><span class="sxs-lookup"><span data-stu-id="f61c3-135">Control+click `Main.storyboard` toobring up hello contextual menu, and then click: `Open As` > `Source Code`</span></span>
2.  <span data-ttu-id="f61c3-136">Ersätt hello `<scenes>` nod med hello koden nedan:</span><span class="sxs-lookup"><span data-stu-id="f61c3-136">Replace hello `<scenes>` node with hello code below:</span></span>

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign-Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
