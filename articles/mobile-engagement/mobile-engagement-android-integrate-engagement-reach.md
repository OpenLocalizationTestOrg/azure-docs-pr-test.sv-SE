---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a><span data-ttu-id="a5fb3-103">Hur tooIntegrate Engagement nå på Android</span><span class="sxs-lookup"><span data-stu-id="a5fb3-103">How tooIntegrate Engagement Reach on Android</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a5fb3-104">Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> 

## <a name="standard-integration"></a><span data-ttu-id="a5fb3-105">Standard-integrering</span><span class="sxs-lookup"><span data-stu-id="a5fb3-105">Standard integration</span></span>

<span data-ttu-id="a5fb3-106">Kopiera Reach resursfiler från hello SDK i projektet:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-106">Copy Reach resource files from hello SDK in your project :</span></span>

* <span data-ttu-id="a5fb3-107">Kopiera hello filer från hello `res/layout` mappen levereras med hello SDK i hello `res/layout` för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-107">Copy hello files from hello `res/layout` folder delivered with hello SDK into hello `res/layout` folder of your application.</span></span>
* <span data-ttu-id="a5fb3-108">Kopiera hello filer från hello `res/drawable` mappen levereras med hello SDK i hello `res/drawable` för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-108">Copy hello files from hello `res/drawable` folder delivered with hello SDK into hello `res/drawable` folder of your application.</span></span>

<span data-ttu-id="a5fb3-109">Redigera din `AndroidManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-109">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="a5fb3-110">Lägg till följande avsnitt hello (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="a5fb3-110">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/plain" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/html" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
              <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
              <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
          </activity>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
              <action android:name="android.intent.action.BOOT_COMPLETED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
          </receiver>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
              <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
          </receiver>
* <span data-ttu-id="a5fb3-111">Du behöver den här behörigheten tooreplay systemmeddelanden som inte har klickat på vid start (annars de sparas på disk men visas inte längre behöver du verkligen tooinclude detta).</span><span class="sxs-lookup"><span data-stu-id="a5fb3-111">You need this permission tooreplay system notifications that were not clicked at boot (otherwise they will be kept on disk but won't be displayed anymore, you really have tooinclude this).</span></span>
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* <span data-ttu-id="a5fb3-112">Ange en ikon som används för meddelanden (både i appen och system som) genom att kopiera och redigera efter avsnittet hello (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="a5fb3-112">Specify an icon used for notifications (both in app and system ones) by copying and editing hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> <span data-ttu-id="a5fb3-113">Det här avsnittet är **obligatoriska** om du tänker använda systemmeddelanden när du skapar Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-113">This section is **mandatory** if you plan on using system notifications when creating Reach campaigns.</span></span> <span data-ttu-id="a5fb3-114">Android förhindrar systemmeddelanden utan ikoner som visas.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-114">Android prevents system notifications without icons from being shown.</span></span> <span data-ttu-id="a5fb3-115">Så om du tar bort det här avsnittet slutanvändarna inte kommer att kunna tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-115">So if you omit this section, your end users will not be able tooreceive them.</span></span>
> 
> 

* <span data-ttu-id="a5fb3-116">Om du skapar kampanjer med systemmeddelanden med stor bild, behöver du tooadd hello följande behörigheter (efter hello `</application>` tagg) om de saknas:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-116">If you create campaigns with system notifications using big picture, you need tooadd hello following permissions (after hello `</application>` tag) if missing:</span></span>
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * <span data-ttu-id="a5fb3-117">På Android M och om ditt program riktar sig till Android API-nivå 23 eller högre, ``WRITE_EXTERNAL_STORAGE`` behörighet kräver användargodkännande av.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-117">On Android M and if your application targets Android API level 23 or greater, ``WRITE_EXTERNAL_STORAGE`` permission requires user approval.</span></span> <span data-ttu-id="a5fb3-118">Läs [i det här avsnittet](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="a5fb3-118">Please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>
* <span data-ttu-id="a5fb3-119">Nå kampanj för systemmeddelanden som du kan också ange i hello om hello enhet ska ringa och/eller vibrerar.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-119">For system notifications you can also specify in hello Reach campaign if hello device should ring and/or vibrate.</span></span> <span data-ttu-id="a5fb3-120">För den toowork, du har toomake att du deklarerat hello följande behörighet (efter hello `</application>` tagg):</span><span class="sxs-lookup"><span data-stu-id="a5fb3-120">For it toowork, you have toomake sure you declared hello following permission (after hello `</application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  <span data-ttu-id="a5fb3-121">Utan den här behörigheten Android förhindrar systemmeddelanden visas när du har markerat hello ring eller hello vibrerar alternativ i hello Reach-kampanjhanterare.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-121">Without this permission, Android prevents system notifications from being shown if you checked hello ring or hello vibrate option in hello Reach Campaign manager.</span></span>

## <a name="native-push"></a><span data-ttu-id="a5fb3-122">Intern Push</span><span class="sxs-lookup"><span data-stu-id="a5fb3-122">Native Push</span></span>
<span data-ttu-id="a5fb3-123">Nu när du har konfigurerat Reach-modulen måste tooconfigure interna push toobe kan tooreceive hello kampanjer på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-123">Now that you configured Reach module, you need tooconfigure native push toobe able tooreceive hello campaigns on hello device.</span></span>

<span data-ttu-id="a5fb3-124">Vi stöder två tjänster på Android:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-124">We support two services on Android:</span></span>

* <span data-ttu-id="a5fb3-125">Google Play-enheter: Använd [Google Cloud Messaging] av följande hello [hur tooIntegrate GCM med Engagement hjälper](mobile-engagement-android-gcm-integrate.md) guide.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-125">Google Play devices: Use [Google Cloud Messaging] by following hello [How tooIntegrate GCM with Engagement guide](mobile-engagement-android-gcm-integrate.md) guide.</span></span>
* <span data-ttu-id="a5fb3-126">Amazon-enheter: Använd [Amazon Device Messaging] av följande hello [hur tooIntegrate ADM med Engagement hjälper](mobile-engagement-android-adm-integrate.md) guide.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-126">Amazon devices: Use [Amazon Device Messaging] by following hello [How tooIntegrate ADM with Engagement guide](mobile-engagement-android-adm-integrate.md) guide.</span></span>

<span data-ttu-id="a5fb3-127">Om du vill tootarget Amazon- och Google Play-enheter, dess möjliga toohave allt inuti 1 AndroidManifest.xml/APK för utveckling.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-127">If you want tootarget both Amazon and Google Play devices, its possible toohave everything inside 1 AndroidManifest.xml/APK for development.</span></span> <span data-ttu-id="a5fb3-128">Men när tooAmazon, de kan avvisa tillämpningsprogrammet om de hittar GCM-kod.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-128">But when submitting tooAmazon, they may reject your application if they find GCM code.</span></span>

<span data-ttu-id="a5fb3-129">I så fall bör du använda flera APKs.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-129">You should use multiple APKs in that case.</span></span>

<span data-ttu-id="a5fb3-130">**Tillämpningsprogrammet är nu redo tooreceive och visa nå kampanjer!**</span><span class="sxs-lookup"><span data-stu-id="a5fb3-130">**Your application is now ready tooreceive and display reach campaigns!**</span></span>

## <a name="how-toohandle-data-push"></a><span data-ttu-id="a5fb3-131">Hur toohandle data push</span><span class="sxs-lookup"><span data-stu-id="a5fb3-131">How toohandle data push</span></span>
### <a name="integration"></a><span data-ttu-id="a5fb3-132">Integrering</span><span class="sxs-lookup"><span data-stu-id="a5fb3-132">Integration</span></span>
<span data-ttu-id="a5fb3-133">Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden måste du toocreate en underklass till `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` och referens i hello `AndroidManifest.xml` fil (mellan hello `<application>` och/eller `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="a5fb3-133">If you want your application toobe able tooreceive Reach data pushes, you have toocreate a sub-class of `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` and reference it in hello `AndroidManifest.xml` file (between hello `<application>` and/or `</application>` tags):</span></span>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

<span data-ttu-id="a5fb3-134">Sedan kan du åsidosätta hello `onDataPushStringReceived` och `onDataPushBase64Received` återanrop.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-134">Then you can override hello `onDataPushStringReceived` and `onDataPushBase64Received` callbacks.</span></span> <span data-ttu-id="a5fb3-135">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-135">Here is an example:</span></span>

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a><span data-ttu-id="a5fb3-136">Kategori</span><span class="sxs-lookup"><span data-stu-id="a5fb3-136">Category</span></span>
<span data-ttu-id="a5fb3-137">hello kategori parametern är valfri när du skapar en Data-Push-kampanj och gör att du toofilter data push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-137">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="a5fb3-138">Detta är användbart om du har flera broadcast mottagare hantera olika typer av data-push, eller om du vill toopush olika typer av `Base64` data och vill tooidentify typ innan parsningen dem.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-138">This is useful if you have several broadcast receivers handling different types of data pushes, or if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

### <a name="callbacks-return-parameter"></a><span data-ttu-id="a5fb3-139">Återanrop returparameter</span><span class="sxs-lookup"><span data-stu-id="a5fb3-139">Callbacks' return parameter</span></span>
<span data-ttu-id="a5fb3-140">Här följer några riktlinjer tooproperly referensen hello returparameter av `onDataPushStringReceived` och `onDataPushBase64Received`:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-140">Here are some guidelines tooproperly handle hello return parameter of `onDataPushStringReceived` and `onDataPushBase64Received`:</span></span>

* <span data-ttu-id="a5fb3-141">En broadcast mottagare ska returnera `null` i hello återanrop om den inte vet hur toohandle en data-push.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-141">A broadcast receiver should return `null` in hello callback if it does not know how toohandle a data push.</span></span> <span data-ttu-id="a5fb3-142">Du bör använda hello kategori toodetermine om broadcast mottagaren ska hantera hello data-push eller inte.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-142">You should use hello category toodetermine whether your broadcast receiver should handle hello data push or not.</span></span>
* <span data-ttu-id="a5fb3-143">En av hello broadcast mottagare ska returnera `true` i hello återanrop om den godkänner hello data-push.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-143">One of hello broadcast receiver should return `true` in hello callback if it accepts hello data push.</span></span>
* <span data-ttu-id="a5fb3-144">En av hello broadcast mottagare ska returnera `false` i hello återanrop om den identifierar hello data-push, men ignorerar oavsett orsak.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-144">One of hello broadcast receiver should return `false` in hello callback if it recognizes hello data push, but discards it for whatever reason.</span></span> <span data-ttu-id="a5fb3-145">Till exempel returnera `false` när hello mottagna data är ogiltiga.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-145">For example, return `false` when hello received data is invalid.</span></span>
* <span data-ttu-id="a5fb3-146">Om en broadcast-mottagare returnerar `true` medan en annan en returnerar `false` för hello samma data-push, hello beteendet är odefinierad gör aldrig som.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-146">If one broadcast receiver returns `true` while another one returns `false` for hello same data push, hello behavior is undefined, you should never do that.</span></span>

<span data-ttu-id="a5fb3-147">hello returtyp används endast för hello Reach statistik:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-147">hello return type is used only for hello Reach statistics:</span></span>

* <span data-ttu-id="a5fb3-148">`Replied`ökas om något av hello broadcast mottagare returneras antingen `true` eller `false`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-148">`Replied` is incremented if one of hello broadcast receivers returned either `true` or `false`.</span></span>
* <span data-ttu-id="a5fb3-149">`Actioned`ökas endast om en av hello broadcast mottagare returnerade `true`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-149">`Actioned` is incremented only if one of hello broadcast receivers returned `true`.</span></span>

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="a5fb3-150">Hur toocustomize kampanjer</span><span class="sxs-lookup"><span data-stu-id="a5fb3-150">How toocustomize campaigns</span></span>
<span data-ttu-id="a5fb3-151">Du kan ändra hello layouter i hello nå SDK toocustomize kampanjer.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-151">toocustomize campaigns, you can modify hello layouts provided in hello Reach SDK.</span></span>

<span data-ttu-id="a5fb3-152">Du bör hålla alla hello-identifierare som används i hello layouter och hålla hello typer av hello vyer som använder en identifierare, särskilt för text vyer och avbildningen vyer.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-152">You should keep all hello identifiers used in hello layouts and keep hello types of hello views that use an identifier, especially for text views and image views.</span></span> <span data-ttu-id="a5fb3-153">Vissa vyer är bara använda toohide eller visa områden så att deras typen kan ändras.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-153">Some views are just used toohide or show areas so their type may be changed.</span></span> <span data-ttu-id="a5fb3-154">Kontrollera hello källkoden om du avser toochange hello typ av en vy i hello tillhandahålls layouter.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-154">Please check hello source code if you intend toochange hello type of a view in hello provided layouts.</span></span>

### <a name="notifications"></a><span data-ttu-id="a5fb3-155">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="a5fb3-155">Notifications</span></span>
<span data-ttu-id="a5fb3-156">Det finns två typer av aviseringar: systemet och i appen meddelanden som använder olika layoutfiler.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-156">There are two types of notifications: system and in-app notifications which use different layout files.</span></span>

#### <a name="system-notifications"></a><span data-ttu-id="a5fb3-157">Systemmeddelanden</span><span class="sxs-lookup"><span data-stu-id="a5fb3-157">System notifications</span></span>
<span data-ttu-id="a5fb3-158">toocustomize systemmeddelanden måste toouse hello **kategorier**.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-158">toocustomize system notifications you need toouse hello **categories**.</span></span> <span data-ttu-id="a5fb3-159">Du kan gå för[kategorier](#categories).</span><span class="sxs-lookup"><span data-stu-id="a5fb3-159">You can jump too[Categories](#categories).</span></span>

#### <a name="in-app-notifications"></a><span data-ttu-id="a5fb3-160">Meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="a5fb3-160">In-app notifications</span></span>
<span data-ttu-id="a5fb3-161">Som standard ett meddelande i appen är en vy som läggs till dynamiskt toohello aktuell aktivitet användaren tack toohello Android gränssnittsmetod `addContentView()`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-161">By default, an in-app notification is a view that is dynamically added toohello current activity user interface thanks toohello Android method `addContentView()`.</span></span> <span data-ttu-id="a5fb3-162">Detta kallas en meddelandet överlägget.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-162">This is called a notification overlay.</span></span> <span data-ttu-id="a5fb3-163">Meddelande överlägg är bra för en snabb integrering eftersom de inte kräver någon du toomodify alla layouter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-163">Notification overlays are great for a fast integration because they do not require you toomodify any layout in your application.</span></span>

<span data-ttu-id="a5fb3-164">toomodify hello utseendet på din anmälan överlägg, du kan bara ändra hello filen `engagement_notification_area.xml` tooyour måste.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-164">toomodify hello look of your notification overlays, you can simply modify hello file `engagement_notification_area.xml` tooyour needs.</span></span>

> [!NOTE]
> <span data-ttu-id="a5fb3-165">hello filen `engagement_notification_overlay.xml` är hello ett som är används toocreate en meddelandet överlägget hello-filen innehåller `engagement_notification_area.xml`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-165">hello file `engagement_notification_overlay.xml` is hello one that is used toocreate a notification overlay, it includes hello file `engagement_notification_area.xml`.</span></span> <span data-ttu-id="a5fb3-166">Du kan också anpassa den toosuit dina behov (till exempel för att placera hello meddelandefältet inom hello överlägget).</span><span class="sxs-lookup"><span data-stu-id="a5fb3-166">You can also customize it toosuit your needs (such as for positioning hello notification area within hello overlay).</span></span>
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a><span data-ttu-id="a5fb3-167">Inkludera meddelande layout som en del av en aktivitet layout</span><span class="sxs-lookup"><span data-stu-id="a5fb3-167">Include notification layout as part of an activity layout</span></span>
<span data-ttu-id="a5fb3-168">Överlägg kan är bra för en snabb integrering men vara olämplig eller ha sidoeffekter i särskilda fall.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-168">Overlays are great for a fast integration but can be inconvenient or have side effects in special cases.</span></span> <span data-ttu-id="a5fb3-169">hello överlägget system kan anpassas på en aktivitetsnivå, vilket gör det enkelt tooprevent sidoeffekter för särskilda aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-169">hello overlay system can be customized at an activity level, making it easy tooprevent side effects for special activities.</span></span>

<span data-ttu-id="a5fb3-170">Du kan bestämma tooinclude layout för våra meddelanden i din befintliga layout tack toohello Android **inkluderar** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-170">You can decide tooinclude our notification layout in your existing layout thanks toohello Android **include** statement.</span></span> <span data-ttu-id="a5fb3-171">hello följande är ett exempel på en ändrade `ListActivity` layout som bara innehåller en `ListView`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-171">hello following is an example of a modified `ListActivity` layout containing just a `ListView`.</span></span>

<span data-ttu-id="a5fb3-172">**Innan du integrering av Engagement:**</span><span class="sxs-lookup"><span data-stu-id="a5fb3-172">**Before Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

<span data-ttu-id="a5fb3-173">**Efter integrering av Engagement:**</span><span class="sxs-lookup"><span data-stu-id="a5fb3-173">**After Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

<span data-ttu-id="a5fb3-174">I det här exemplet vi har lagt till en överordnad behållare eftersom hello ursprungliga layouten används en listvy som hello element på översta nivån.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-174">In this example we added a parent container since hello original layout used a list view as hello top level element.</span></span> <span data-ttu-id="a5fb3-175">Vi har även lagt till `android:layout_weight="1"` toobe kan tooadd vyn under en listvy konfigurerats med `android:layout_height="fill_parent"`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-175">We also added `android:layout_weight="1"` toobe able tooadd a view below a list view configured with `android:layout_height="fill_parent"`.</span></span>

<span data-ttu-id="a5fb3-176">Hej Engagement nå SDK identifierar automatiskt hello notification layouten ingår i den här aktiviteten och kommer inte att lägga till ett överlägg för den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-176">hello Engagement Reach SDK automatically detects that hello notification layout is included in this activity and will not add an overlay for this activity.</span></span>

> [!TIP]
> <span data-ttu-id="a5fb3-177">Om du använder en ListActivity i ditt program hindrar ett synliga Reach-överlägg dig från att korsreagerande tooclicked objekt i listvyn hello längre.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-177">If you use a ListActivity in your application, a visible Reach overlay will prevent you from reacting tooclicked items in hello list view anymore.</span></span> <span data-ttu-id="a5fb3-178">Detta är ett känt problem.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-178">This is a known issue.</span></span> <span data-ttu-id="a5fb3-179">toowork problemet vi rekommenderar att du tooembed hello notification layout i listan aktivitet layouten som i föregående exempel för hello.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-179">toowork around this problem we suggest you tooembed hello notification layout in your own list activity layout like in hello previous sample.</span></span>
> 
> 

##### <a name="disabling-application-notification-per-activity"></a><span data-ttu-id="a5fb3-180">Inaktivera programmet meddelanden per aktivitet</span><span class="sxs-lookup"><span data-stu-id="a5fb3-180">Disabling application notification per activity</span></span>
<span data-ttu-id="a5fb3-181">Om du inte vill hello överlägget toobe lagt till tooyour aktivitet och om du inte anger layout för hello-meddelande med din egen layout, kan du inaktivera hello överlägget för aktiviteten i hello `AndroidManifest.xml` genom att lägga till en `meta-data` avsnittet precis som i följande hello Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-181">If you don't want hello overlay toobe added tooyour activity, and if you don't include hello notification layout in your own layout, you can disable hello overlay for this activity in hello `AndroidManifest.xml` by adding a `meta-data` section like in hello following example:</span></span>

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <span data-ttu-id="a5fb3-182"><a name="categories"></a>Kategorier</span><span class="sxs-lookup"><span data-stu-id="a5fb3-182"><a name="categories"></a> Categories</span></span>
<span data-ttu-id="a5fb3-183">När du ändrar hello tillhandahålls layouter ändra hello utseende på alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-183">When you modify hello provided layouts, you modify hello look of all your notifications.</span></span> <span data-ttu-id="a5fb3-184">Kategorier kan toodefine olika riktas söks meddelanden (möjligen beteenden).</span><span class="sxs-lookup"><span data-stu-id="a5fb3-184">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="a5fb3-185">En kategori kan anges när du skapar en Reach-kampanj.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-185">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="a5fb3-186">Tänk på att kategorier kan du anpassa meddelanden och avsökningar, som beskrivs senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-186">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="a5fb3-187">tooregister en kategori hanterare för dina meddelanden behöver du tooadd ett anrop när programmet hello har initierats.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-187">tooregister a category handler for your notifications, you need tooadd a call when hello application is initialized.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5fb3-188">Läs hello varning om hello android: processen attributet \<android-sdk-engagement-process\> i hello hur tooIntegrate Engagement på Android avsnittet innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-188">Please read hello warning about hello android:process attribute \<android-sdk-engagement-process\> in hello How tooIntegrate Engagement on Android topic before proceeding.</span></span>
> 
> 

<span data-ttu-id="a5fb3-189">hello följande exempel förutsätter att du bekräftade hello föregående varning och använder en underklass till `EngagementApplication`:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-189">hello following example assumes you acknowledged hello previous warning and use a sub-class of `EngagementApplication`:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

<span data-ttu-id="a5fb3-190">Hej `MyNotifier` objektet är hello implementering av hello meddelandehanteraren kategori.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-190">hello `MyNotifier` object is hello implementation of hello notification category handler.</span></span> <span data-ttu-id="a5fb3-191">Det är antingen en implementering av hello `EngagementNotifier` -gränssnittet eller en underordnad klass av hello standardimplementering: `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-191">It is either an implementation of hello `EngagementNotifier` interface or a sub class of hello default implementation: `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="a5fb3-192">Observera att hello samma meddelaren kan hantera flera kategorier, kan du registrera dem så här:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-192">Note that hello same notifier can handle several categories, you can register them like this:</span></span>

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

<span data-ttu-id="a5fb3-193">tooreplace hello kategori standardimplementering, kan du registrera din implementering som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-193">tooreplace hello default category implementation, you can register your implementation like in hello following example:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

<span data-ttu-id="a5fb3-194">hello aktuella kategori som används i en hanterare skickas som en parameter i de flesta metoder som du kan åsidosätta i `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-194">hello current category used in a handler is passed as a parameter in most methods you can override in `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="a5fb3-195">Den skickas antingen som en `String` parametern eller indirekt i en `EngagementReachContent` objekt som har en `getCategory()` metod.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-195">It is passed either as a `String` parameter or indirectly in a `EngagementReachContent` object which has a `getCategory()` method.</span></span>

<span data-ttu-id="a5fb3-196">Du kan ändra de flesta av processen för hello-meddelande med omdefiniering metoder på `EngagementDefaultNotifier`för mer avancerad anpassning känna sig fria tootake en titt på hello teknisk dokumentation och hello källkoden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-196">You can change most of hello notification creation process by redefining methods on `EngagementDefaultNotifier`, for more advanced customization feel free tootake a look at hello technical documentation and at hello source code.</span></span>

##### <a name="in-app-notifications"></a><span data-ttu-id="a5fb3-197">Meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="a5fb3-197">In-app notifications</span></span>
<span data-ttu-id="a5fb3-198">Om du bara vill toouse alternativa layouter för en viss kategori, kan du implementera detta som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-198">If you just want toouse alternate layouts for a specific category, you can implement this as in hello following example:</span></span>

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

<span data-ttu-id="a5fb3-199">**Exempel på `my_notification_overlay.xml` :**</span><span class="sxs-lookup"><span data-stu-id="a5fb3-199">**Example of `my_notification_overlay.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

<span data-ttu-id="a5fb3-200">Som du ser skiljer hello överlägget visa identifierare sig hello standard en.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-200">As you can see, hello overlay view identifier is different than hello standard one.</span></span> <span data-ttu-id="a5fb3-201">Det är viktigt att varje layout använder en unik identifierare för överlägg.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-201">It is important that each layout use a unique identifier for overlays.</span></span>

<span data-ttu-id="a5fb3-202">**Exempel på `my_notification_area.xml` :**</span><span class="sxs-lookup"><span data-stu-id="a5fb3-202">**Example of `my_notification_area.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

<span data-ttu-id="a5fb3-203">Som du ser skiljer hello notification området Visa identifierare sig hello standard en.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-203">As you can see, hello notification area view identifier is different than hello standard one.</span></span> <span data-ttu-id="a5fb3-204">Det är viktigt att varje layout använder en unik identifierare för meddelandefält.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-204">It is important that each layout uses a unique identifier for notification areas.</span></span>

<span data-ttu-id="a5fb3-205">Det här enkla exemplet på kategorin gör ett program (eller i appen) meddelanden som visas överst hello i hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-205">This simple example of category makes application (or in-app) notifications displayed at hello top of hello screen.</span></span> <span data-ttu-id="a5fb3-206">Vi har inte ändrats hello standard identifierare som används i hello meddelandefältet sig själv.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-206">We did not change hello standard identifiers used in hello notification area itself.</span></span>

<span data-ttu-id="a5fb3-207">Om du vill att du har tooredefine hello toochange `EngagementDefaultNotifier.prepareInAppArea` metod.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-207">If you want toochange that, you have tooredefine hello `EngagementDefaultNotifier.prepareInAppArea` method.</span></span> <span data-ttu-id="a5fb3-208">Det rekommenderas toolook på hello teknisk dokumentation och hello källkoden för `EngagementNotifier` och `EngagementDefaultNotifier` om du vill använda denna nivå av avancerad anpassning.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-208">It's recommended toolook at hello technical documentation and at hello source code of `EngagementNotifier` and `EngagementDefaultNotifier` if you want this level of advanced customization.</span></span>

##### <a name="system-notifications"></a><span data-ttu-id="a5fb3-209">Systemmeddelanden</span><span class="sxs-lookup"><span data-stu-id="a5fb3-209">System notifications</span></span>
<span data-ttu-id="a5fb3-210">Genom att utöka `EngagementDefaultNotifier`, kan du åsidosätta `onNotificationPrepared` tooalter hello-meddelande som har sammanställts av hello standardimplementering.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-210">By extending `EngagementDefaultNotifier`, you can override `onNotificationPrepared` tooalter hello notification that was prepared by hello default implementation.</span></span>

<span data-ttu-id="a5fb3-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-211">For example:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

<span data-ttu-id="a5fb3-212">Det här exemplet gör en systemavisering för en innehåll visas som en pågående händelse när hello ”pågående” kategori används.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-212">This example makes a system notification for a content being displayed as an ongoing event when hello "ongoing" category is used.</span></span>

<span data-ttu-id="a5fb3-213">Om du vill toobuild hello `Notification` objekt från början, du kan returnera `false` toohello metod och anropet `notify` själv på hello `NotificationManager`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-213">If you want toobuild hello `Notification` object from scratch, you can return `false` toohello method and call `notify` yourself on hello `NotificationManager`.</span></span> <span data-ttu-id="a5fb3-214">I så fall är det viktigt att du behåller en `contentIntent`, `deleteIntent` och hello meddelande-ID som används av `EngagementReachReceiver`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-214">In that case it's important that you keep a `contentIntent`, a `deleteIntent` and hello notification identifier used by `EngagementReachReceiver`.</span></span>

<span data-ttu-id="a5fb3-215">Här är en korrekt exempel på sådana implementering:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-215">Here is a correct example of such an implementation:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a><span data-ttu-id="a5fb3-216">Meddelande endast meddelanden</span><span class="sxs-lookup"><span data-stu-id="a5fb3-216">Notification only announcements</span></span>
<span data-ttu-id="a5fb3-217">hello hantering av hello klickar du på ett meddelande som endast meddelande kan anpassas genom att åsidosätta `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello förberedd `Intent`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-217">hello management of hello click on a notification only announcement can be customized by overriding `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello prepared `Intent`.</span></span> <span data-ttu-id="a5fb3-218">Med den här metoden kan du tootune hello flaggor enkelt.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-218">Using this method allows you tootune hello flags easily.</span></span>

<span data-ttu-id="a5fb3-219">Till exempel tooadd hello `SINGLE_TOP` flaggan:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-219">For example tooadd hello `SINGLE_TOP` flag:</span></span>

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

<span data-ttu-id="a5fb3-220">För äldre Engagement användare, Observera att systemmeddelanden utan åtgärd URL nu startar programmet hello om den var i bakgrunden, så den här metoden kan anropas med ett meddelande utan åtgärds-URL.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-220">For legacy Engagement users, please note that system notifications without action URL now launches hello application if it was in background, so this method can be called with an announcement without action URL.</span></span> <span data-ttu-id="a5fb3-221">Du bör som när du anpassar hello avsikt.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-221">You should consider that when customizing hello intent.</span></span>

<span data-ttu-id="a5fb3-222">Du kan också implementeras `EngagementNotifier.executeNotifAnnouncementAction` från början.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-222">You can also implement `EngagementNotifier.executeNotifAnnouncementAction` from scratch.</span></span>

##### <a name="notification-life-cycle"></a><span data-ttu-id="a5fb3-223">Livscykel för meddelande</span><span class="sxs-lookup"><span data-stu-id="a5fb3-223">Notification life cycle</span></span>
<span data-ttu-id="a5fb3-224">När du använder hello standardkategori, vissa livscykel metoder anropas på hello `EngagementReachInteractiveContent` objekt tooreport statistik och uppdatera hello kampanj tillstånd:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-224">When using hello default category, some life cycle methods are called on hello `EngagementReachInteractiveContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="a5fb3-225">När hello-meddelande visas i programmet eller placera i hello statusfältet, hello `displayNotification` metoden anropas (som rapporterar statistik) av `EngagementReachAgent` om `handleNotification` returnerar `true`.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-225">When hello notification is displayed in application or put in hello status bar, hello `displayNotification` method is called (which reports statistics) by `EngagementReachAgent` if `handleNotification` returns `true`.</span></span>
* <span data-ttu-id="a5fb3-226">Om hello-meddelande avvisas hello `exitNotification` metoden anropas statistik rapporteras och nästa kampanjer kan nu bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-226">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="a5fb3-227">Om du hello-meddelande `actionNotification` är anropas statistik rapporteras och hello associerade avsikten har startats.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-227">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated intent is launched.</span></span>

<span data-ttu-id="a5fb3-228">Om din implementering av `EngagementNotifier` kringgår hello standardbeteendet har du toocall metoderna livscykel själv.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-228">If your implementation of `EngagementNotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="a5fb3-229">hello som följande exempel visar tillfällen där hello standardbeteendet kringgås:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-229">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="a5fb3-230">Du inte utökar `EngagementDefaultNotifier`, t.ex. du har implementerat kategori hantering från grunden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-230">You don't extend `EngagementDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="a5fb3-231">För systemmeddelanden, du overrode hello `onNotificationPrepared` och du ändrat `contentIntent` eller `deleteIntent` i hello `Notification` objekt.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-231">For system notifications, you overrode hello `onNotificationPrepared` and you modified `contentIntent` or `deleteIntent` in hello `Notification` object.</span></span>
* <span data-ttu-id="a5fb3-232">För meddelanden i appen, overrode `prepareInAppArea`, vara säker på att toomap minst `actionNotification` tooone U.I-kontroller.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-232">For in-app notifications, you overrode `prepareInAppArea`, be sure toomap at least `actionNotification` tooone of your U.I controls.</span></span>

> [!NOTE]
> <span data-ttu-id="a5fb3-233">Om `handleNotification` returnerar ett undantag, hello innehåll tas bort och `dropContent` anropas.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-233">If `handleNotification` throws an exception, hello content is deleted and `dropContent` is called.</span></span> <span data-ttu-id="a5fb3-234">Detta rapporteras i statistik och nästa kampanjer kan nu bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-234">This is reported in statistics and next campaigns can now be processed.</span></span>
> 
> 

### <a name="announcements-and-polls"></a><span data-ttu-id="a5fb3-235">Meddelanden och avsökningar</span><span class="sxs-lookup"><span data-stu-id="a5fb3-235">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="a5fb3-236">Layouter</span><span class="sxs-lookup"><span data-stu-id="a5fb3-236">Layouts</span></span>
<span data-ttu-id="a5fb3-237">Du kan ändra hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` och `engagement_poll.xml` filer toocustomize text meddelanden, web meddelanden och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-237">You can modify hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` and `engagement_poll.xml` files toocustomize text announcements, web announcements and polls.</span></span>

<span data-ttu-id="a5fb3-238">De här filerna dela två vanliga layouter för hello rubrikområdet och hello knappområde.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-238">These files share two common layouts for hello title area and hello button area.</span></span> <span data-ttu-id="a5fb3-239">hello layouten för hello är `engagement_content_title.xml` och använder hello eponymous drawable-filen för hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-239">hello layout for hello title is `engagement_content_title.xml` and uses hello eponymous drawable file for hello background.</span></span> <span data-ttu-id="a5fb3-240">hello layout hello åtgärd och avsluta knappar är `engagement_button_bar.xml` och använder hello eponymous drawable-filen för hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-240">hello layout for hello action and exit buttons is `engagement_button_bar.xml` and uses hello eponymous drawable file for hello background.</span></span>

<span data-ttu-id="a5fb3-241">I en omröstning hello fråga layout och sina val är dynamiskt förstoras med flera gånger hello `engagement_question.xml` layoutfilen för hello frågor och hello `engagement_choice.xml` -filen för hello val.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-241">In a poll, hello question layout and their choices are dynamically inflated using several times hello `engagement_question.xml` layout file for hello questions and hello `engagement_choice.xml` file for hello choices.</span></span>

#### <a name="categories"></a><span data-ttu-id="a5fb3-242">Kategorier</span><span class="sxs-lookup"><span data-stu-id="a5fb3-242">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="a5fb3-243">Alternativa layouter</span><span class="sxs-lookup"><span data-stu-id="a5fb3-243">Alternate layouts</span></span>
<span data-ttu-id="a5fb3-244">Hello kampanj kategori kan vara används toohave alternativa layouter för meddelanden och avsökningar som meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-244">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="a5fb3-245">Till exempel toocreate en kategori för en SMS-meddelande, kan du utöka `EngagementTextAnnouncementActivity` och hänvisar det hello `AndroidManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-245">For example, toocreate a category for a text announcement, you can extend `EngagementTextAnnouncementActivity` and reference it hello `AndroidManifest.xml` file:</span></span>

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

<span data-ttu-id="a5fb3-246">Observera hello kategorin i hello avsikt filter används toomake hello skillnaden med hello standardaktivitet meddelande.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-246">Note that hello category in hello intent filter is used toomake hello difference with hello default announcement activity.</span></span>

<span data-ttu-id="a5fb3-247">hello nå SDK använder hello avsiktshantering system tooresolve hello rätt aktivitet för en viss kategori och använder hello standard kategori om hello matchningen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-247">hello Reach SDK uses hello intent system tooresolve hello right activity for a specific category and it falls back on hello default category if hello resolution failed.</span></span>

<span data-ttu-id="a5fb3-248">Du kan ha tooimplement `MyCustomTextAnnouncementActivity`, om du bara vill toochange hello layout (men behåll hello samma vy identifierare), du precis har toodefine hello klassen som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-248">Then you have tooimplement `MyCustomTextAnnouncementActivity`, if you just want toochange hello layout (but keep hello same view identifiers), you just have toodefine hello class like in hello following example:</span></span>

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

<span data-ttu-id="a5fb3-249">tooreplace hello standardkategori av text-meddelanden, ersätts `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` av din implementering.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-249">tooreplace hello default category of text announcements, simply replace `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` by your implementation.</span></span>

<span data-ttu-id="a5fb3-250">Web meddelanden och avsökningar kan anpassas på ett liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-250">Web announcements and polls can be customized in a similar fashion.</span></span>

<span data-ttu-id="a5fb3-251">För web-meddelanden kan du utöka `EngagementWebAnnouncementActivity` och deklarera dina aktiviteter i hello `AndroidManifest.xml` precis som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-251">For web announcements you can extend `EngagementWebAnnouncementActivity` and declare your activity in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

<span data-ttu-id="a5fb3-252">För avsökningar kan du utöka `EngagementPollActivity` och deklarera dina i hello `AndroidManifest.xml` precis som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-252">For polls you can extend `EngagementPollActivity` and declare your in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a><span data-ttu-id="a5fb3-253">Implementeringen från grunden</span><span class="sxs-lookup"><span data-stu-id="a5fb3-253">Implementation from scratch</span></span>
<span data-ttu-id="a5fb3-254">Du kan implementera kategorier för dina aktiviteter för meddelande (och avsökning) utan att utöka en hello `Engagement*Activity` klasser som tillhandahålls av hello nå SDK.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-254">You can implement categories for your announcement (and poll) activities without extending one of hello `Engagement*Activity` classes provided by hello Reach SDK.</span></span> <span data-ttu-id="a5fb3-255">Detta är användbart till exempel om du vill toodefine en layout som inte använder samma visar hello som hello standard layouter.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-255">This is useful for example if you want toodefine a layout that does not use hello same views as hello standard layouts.</span></span>

<span data-ttu-id="a5fb3-256">För avancerade notification anpassning bör som toolook på hello källkoden för hello standardimplementeringen.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-256">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

<span data-ttu-id="a5fb3-257">Här följer några saker tookeep i åtanke: Reach startas hello aktivitet med ett specifikt syfte (motsvarande toohello avsiktshantering filter) plus en extra parameter som är hello innehåll identifierare.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-257">Here are some things tookeep in mind: Reach will launch hello activity with a specific intent (corresponding toohello intent filter) plus an extra parameter which is hello content identifier.</span></span>

<span data-ttu-id="a5fb3-258">tooretrieve hello innehållsobjekt som innehåller hello fält som du angav när skapar hello kampanj på hello-webbplatsen du kan göra detta:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-258">tooretrieve hello content object which contain hello fields you specified when creating hello campaign on hello web site you can do this:</span></span>

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

<span data-ttu-id="a5fb3-259">För statistik, ska du rapportera hello innehållet visas i hello `onResume` händelse:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-259">For statistics, you should report hello content is displayed in hello `onResume` event:</span></span>

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

<span data-ttu-id="a5fb3-260">Sedan Glöm inte toocall antingen `actionContent(this)` eller `exitContent(this)` på hello innehållsobjekt innan hello aktivitet försätts i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-260">Then, don't forget toocall either `actionContent(this)` or `exitContent(this)` on hello content object before hello activity goes into background.</span></span>

<span data-ttu-id="a5fb3-261">Om du inte anropa antingen `actionContent` eller `exitContent`, statistik inte skickas (dvs. inga analyser på hello kampanj) och flera allt hello nästa kampanjer meddelas inte förrän hello programprocessen har startats om.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-261">If you don't call either `actionContent` or `exitContent`, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly, hello next campaigns will not be notified until hello application process is restarted.</span></span>

<span data-ttu-id="a5fb3-262">Orientering eller andra ändringar i konfigurationen kan fatta hello kod komplicerade toodetermine om hello aktivitet försätts i bakgrunden eller inte, hello standardimplementeringen gör att hello innehåll rapporteras som avslutats om hello användaren lämnar hello-aktivitet (antingen med När du trycker på `HOME` eller `BACK`) men inte om hello orientering ändras.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-262">Orientation or other configuration changes can make hello code tricky toodetermine whether hello activity goes into background or not, hello standard implementation makes sure hello content is reported as exited if hello user leaves hello activity (either by pressing `HOME` or `BACK`) but not if hello orientation changes.</span></span>

<span data-ttu-id="a5fb3-263">Här ingår hello intressanta i hello implementering:</span><span class="sxs-lookup"><span data-stu-id="a5fb3-263">Here is hello interesting part of hello implementation:</span></span>

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

<span data-ttu-id="a5fb3-264">Som du ser om du anropas `actionContent(this)` sedan klar hello aktiviteten `exitContent(this)` på ett säkert sätt kan anropas utan att ha någon effekt.</span><span class="sxs-lookup"><span data-stu-id="a5fb3-264">As you can see, if you called `actionContent(this)` then finished hello activity, `exitContent(this)` can be safely called without having any effect.</span></span>

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
