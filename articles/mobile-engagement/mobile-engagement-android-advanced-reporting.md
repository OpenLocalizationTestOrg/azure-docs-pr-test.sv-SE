---
title: "aaaAdvanced rapporteringsalternativen för Azure Mobile Engagement Android SDK"
description: "Beskriver hur toodo avancerade reporting toocapture analytics för Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 5c8f4ea36c54715f4e09fd43c96132c15019a71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a>Avancerade rapportering med Engagement på Android
> [!div class="op_single_selector"]
> * [Universell Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Det här avsnittet beskrivs ytterligare rapporteringsscenarier i din Android-App. Du kan använda dessa alternativ toohello app som skapats i hello [komma igång](mobile-engagement-android-get-started.md) kursen.

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

hello kursen har du slutfört har avsiktligt direkt och enkel, men det är avancerade alternativ som du kan välja.

## <a name="modifying-your-activity-classes"></a>Ändra din `Activity` klasser
I hello [komma igång-kursen](mobile-engagement-android-get-started.md), alla du hade toodo har toomake din `*Activity` underklasser ärver från hello motsvarande `Engagement*Activity` klasser. Till exempel om en bakåtkompatibel aktivitet utökat `ListActivity`, gör du det utöka `EngagementListActivity`.

> [!IMPORTANT]
> När du använder `EngagementListActivity` eller `EngagementExpandableListActivity`, kontrollera anrop för`requestWindowFeature(...);` görs före hello anrop för`super.onCreate(...);`, annars kraschar.
> 
> 

Du hittar de här klasserna i hello `src` mappen och kopiera dem till ditt projekt. hello klasser finns också i hello **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativ metod: anropa `startActivity()` och `endActivity()` manuellt
Om du inte kan eller inte vill att toooverload din `Activity` klasser, du kan i stället börja och sluta dina aktiviteter genom att anropa hello `EngagementAgent`'s metoder direkt.

> [!IMPORTANT]
> hello Android SDK anropar aldrig hello `endActivity()` metod, även om programmet hello avslutas (på Android program aldrig stängs). Därför är det *hög* rekommenderas toocall hello `startActivity()` metod i hello `onResume` återanrop av *alla* dina aktiviteter och hello `endActivity()` metod i hello `onPause()` återanrop av *alla* dina aktiviteter. Detta är hello endast sätt toobe till att sessioner inte sprids. Om en session har läckts kopplar hello Engagement service aldrig från hello Engagement-serverdelen (eftersom hello tjänsten fortfarande ansluten så länge en session väntar).
> 
> 

Här är ett exempel:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Det här exemplet är liknande toohello `EngagementActivity` klassen och dess varianter vars källkoden har angetts i hello `src` mapp.

## <a name="using-applicationoncreate"></a>Med hjälp av Application.onCreate()
All kod som du `Application.onCreate()` och i andra program körs återanrop för ditt programs processer, inklusive hello Engagement service. Det kan ha oönskade sidoeffekter, som inte behövs minnesallokering och trådar i hello Engagement process eller dubblett broadcast mottagare eller tjänster.

Om du åsidosätter `Application.onCreate()`, rekommenderar vi att lägga till hello följande kodavsnitt hello början av din `Application.onCreate()` funktionen:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Du kan göra samma sak för hello `Application.onTerminate()`, `Application.onLowMemory()`, och `Application.onConfigurationChanged(...)`.

Du kan också utöka `EngagementApplication` i stället för att utöka `Application`: hello återanrop `Application.onCreate()` hello processen kontroll och anrop `Application.onApplicationProcessCreate()` endast om hello aktuella processen inte hello en hello Engagement värdtjänsten hello samma regler gäller för Hej andra återanrop.

## <a name="tags-in-hello-androidmanifestxml-file"></a>Taggar i hello AndroidManifest.xml fil
I hello service-taggen i hello AndroidManifest.xml filen hello `android:label` attribut kan du toochoose hello namnet på hello Engagement tjänsten som det visas tooend användarna på skärmen för hello ”körs tjänster” i sin telefon. Vi rekommenderar att det här attributet för`"<Your application name>Service"` (till exempel `"AcmeFunGameService"`).

Att ange hello `android:process` attributet säkerställer att hello Engagement-tjänsten körs i en egen process (kör Engagement hello samma process som programmet gör main/UI-tråden potentiellt mindre känslig).

## <a name="building-with-proguard"></a>Skapa med ProGuard
Om du skapar ditt programpaket med ProGuard måste tookeep vissa klasser. Du kan använda följande konfiguration fragment hello:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
