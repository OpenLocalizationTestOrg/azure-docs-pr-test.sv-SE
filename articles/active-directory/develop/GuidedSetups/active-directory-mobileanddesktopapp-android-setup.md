---
title: "aaaAzure AD v2 Android komma igång - inställningar | Microsoft Docs"
description: "Hur en Android-App kan hämta en åtkomst-token och anropa API: erna som kräver åtkomst-token från Azure Active Directory v2 slutpunkten eller Microsoft Graph API"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: df2670d6d35b7a9a81158d4d7eb190540ca9c695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="13905-103">Konfigurera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="13905-103">Set up your project</span></span>

> <span data-ttu-id="13905-104">Föredrar toodownload det här exemplet Android Studio-projekt i stället?</span><span class="sxs-lookup"><span data-stu-id="13905-104">Prefer toodownload this sample's Android Studio project instead?</span></span> <span data-ttu-id="13905-105">[Hämta ett projekt](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.</span><span class="sxs-lookup"><span data-stu-id="13905-105">[Download a project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing    .</span></span>


### <a name="create-a-new-project"></a><span data-ttu-id="13905-106">Skapa ett nytt projekt</span><span class="sxs-lookup"><span data-stu-id="13905-106">Create a new project</span></span> 
1.  <span data-ttu-id="13905-107">Öppna Android Studio, gå till:`File` > `New` > `New Project`</span><span class="sxs-lookup"><span data-stu-id="13905-107">Open Android Studio, go to: `File` > `New` > `New Project`</span></span>
2.  <span data-ttu-id="13905-108">Namnge ditt program och klicka på`Next`</span><span class="sxs-lookup"><span data-stu-id="13905-108">Name your application and click `Next`</span></span>
3.  <span data-ttu-id="13905-109">Se till att tooselect *API 21 eller nyare (Android 5.0)* och klicka på`Next`</span><span class="sxs-lookup"><span data-stu-id="13905-109">Make sure tooselect *API 21 or newer (Android 5.0)* and click `Next`</span></span>
4.  <span data-ttu-id="13905-110">Lämna `Empty Activity`, klickar du på `Next`, sedan`Finish`</span><span class="sxs-lookup"><span data-stu-id="13905-110">Leave `Empty Activity`, click `Next`, then `Finish`</span></span>


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="13905-111">Lägga till hello Microsoft Authentication Library (MSAL) tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="13905-111">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1.  <span data-ttu-id="13905-112">Gå till i Android Studio:`Gradle Scripts` > `build.gradle (Module: app)`</span><span class="sxs-lookup"><span data-stu-id="13905-112">In Android Studio, go to: `Gradle Scripts` > `build.gradle (Module: app)`</span></span>
2.  <span data-ttu-id="13905-113">Kopiera och klistra in hello följande kod `Dependencies`:</span><span class="sxs-lookup"><span data-stu-id="13905-113">Copy and paste hello following code under `Dependencies`:</span></span>

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a><span data-ttu-id="13905-114">Om det här paketet</span><span class="sxs-lookup"><span data-stu-id="13905-114">About this package</span></span>

<span data-ttu-id="13905-115">hello-paketet ovan installerar hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="13905-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="13905-116">MSAL hanterar införskaffa, cachelagring och uppdatera användaren tokens som används för tooaccess API: er som skyddas av Azure Active Directory v2 slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="13905-116">MSAL handles acquiring, caching and refreshing user tokens used tooaccess APIs protected by Azure Active Directory v2 endpoint.</span></span>
<!--end-collapse-->

## <a name="create-your-applications-ui"></a><span data-ttu-id="13905-117">Skapa ditt programs gränssnitt</span><span class="sxs-lookup"><span data-stu-id="13905-117">Create your application’s UI</span></span>

1.  <span data-ttu-id="13905-118">Öppna: `activity_main.xml` under`res` > `layout`</span><span class="sxs-lookup"><span data-stu-id="13905-118">Open: `activity_main.xml` under `res` > `layout`</span></span>
2.  <span data-ttu-id="13905-119">Ändra hello aktivitet layout från `android.support.constraint.ConstraintLayout` eller andra för`LinearLayout`</span><span class="sxs-lookup"><span data-stu-id="13905-119">Change hello activity layout from `android.support.constraint.ConstraintLayout` or other too`LinearLayout`</span></span>
3.  <span data-ttu-id="13905-120">Lägg till `android:orientation="vertical"` egenskapen för`LinearLayout` nod</span><span class="sxs-lookup"><span data-stu-id="13905-120">Add `android:orientation="vertical"` property too`LinearLayout` node</span></span>
4.  <span data-ttu-id="13905-121">Kopiera och klistra in hello följande kod till hello `LinearLayout` nod, ersätter hello aktuella innehåll:</span><span class="sxs-lookup"><span data-stu-id="13905-121">Copy and paste hello following code into hello `LinearLayout` node, replacing hello current content:</span></span>

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

