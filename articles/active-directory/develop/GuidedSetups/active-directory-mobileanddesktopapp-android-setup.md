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
## <a name="set-up-your-project"></a>Konfigurera ditt projekt

> Föredrar toodownload det här exemplet Android Studio-projekt i stället? [Hämta ett projekt](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.


### <a name="create-a-new-project"></a>Skapa ett nytt projekt 
1.  Öppna Android Studio, gå till:`File` > `New` > `New Project`
2.  Namnge ditt program och klicka på`Next`
3.  Se till att tooselect *API 21 eller nyare (Android 5.0)* och klicka på`Next`
4.  Lämna `Empty Activity`, klickar du på `Next`, sedan`Finish`


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Lägga till hello Microsoft Authentication Library (MSAL) tooyour projekt
1.  Gå till i Android Studio:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Kopiera och klistra in hello följande kod `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>Om det här paketet

hello-paketet ovan installerar hello Microsoft Authentication Library (MSAL). MSAL hanterar införskaffa, cachelagring och uppdatera användaren tokens som används för tooaccess API: er som skyddas av Azure Active Directory v2 slutpunkt.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Skapa ditt programs gränssnitt

1.  Öppna: `activity_main.xml` under`res` > `layout`
2.  Ändra hello aktivitet layout från `android.support.constraint.ConstraintLayout` eller andra för`LinearLayout`
3.  Lägg till `android:orientation="vertical"` egenskapen för`LinearLayout` nod
4.  Kopiera och klistra in hello följande kod till hello `LinearLayout` nod, ersätter hello aktuella innehåll:

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

