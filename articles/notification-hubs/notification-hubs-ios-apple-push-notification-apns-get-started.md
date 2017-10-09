---
title: aaaSending push-meddelanden tooiOS med Azure Notification Hubs | Microsoft Docs
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program."
services: notification-hubs
documentationcenter: ios
keywords: push-meddelande, push-meddelanden, push-meddelanden i ios
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a><span data-ttu-id="9693a-104">Skicka push-meddelanden tooiOS med Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="9693a-104">Sending push notifications tooiOS with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="9693a-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="9693a-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="9693a-106">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9693a-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="9693a-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="9693a-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9693a-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="9693a-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="9693a-109">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program.</span><span class="sxs-lookup"><span data-stu-id="9693a-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span> <span data-ttu-id="9693a-110">Du skapar en tom iOS-app som tar emot push-meddelanden med hjälp av hello [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span><span class="sxs-lookup"><span data-stu-id="9693a-110">You'll create a blank iOS app that receives push notifications by using hello [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span></span> 

<span data-ttu-id="9693a-111">När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="9693a-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9693a-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9693a-112">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="9693a-113">hello slutförts koden för den här självstudiekursen hittar [på GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="9693a-113">hello completed code for this tutorial can be found [on GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9693a-114">Krav</span><span class="sxs-lookup"><span data-stu-id="9693a-114">Prerequisites</span></span>
<span data-ttu-id="9693a-115">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="9693a-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="9693a-116">[Mobile Services iOS SDK version 1.2.4]</span><span class="sxs-lookup"><span data-stu-id="9693a-116">[Mobile Services iOS SDK version 1.2.4]</span></span>
* <span data-ttu-id="9693a-117">Den senaste versionen av [Xcode]</span><span class="sxs-lookup"><span data-stu-id="9693a-117">Latest version of [Xcode]</span></span>
* <span data-ttu-id="9693a-118">En enhet som är kompatibel med iOS 8 (eller senare versioner)</span><span class="sxs-lookup"><span data-stu-id="9693a-118">An iOS 8 (or later version)-capable device</span></span>
* <span data-ttu-id="9693a-119">Medlemskap i [Apple Developer Program](https://developer.apple.com/programs/).</span><span class="sxs-lookup"><span data-stu-id="9693a-119">[Apple Developer Program](https://developer.apple.com/programs/) membership.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="9693a-120">På grund av konfigurationskrav för push-meddelanden, måste du distribuera och testa push-meddelanden på en fysisk iOS-enhet (iPhone eller iPad) i stället för hello iOS-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="9693a-120">Because of configuration requirements for push notifications, you must deploy and test push notifications on a physical iOS device (iPhone or iPad) instead of hello iOS Simulator.</span></span>
  > 
  > 

<span data-ttu-id="9693a-121">Du måste slutföra den här självstudiekursen innan du börjar någon annan kurs om Notification Hubs för iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="9693a-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a><span data-ttu-id="9693a-122">Konfigurera din Notification Hub för att skicka push-meddelanden till iOS</span><span class="sxs-lookup"><span data-stu-id="9693a-122">Configure your Notification Hub for iOS push notifications</span></span>
<span data-ttu-id="9693a-123">Det här avsnittet vägleder dig genom att skapa en ny meddelandehubb och konfigurerar autentisering med APNS med hello **.p12** push-certifikat som du skapade.</span><span class="sxs-lookup"><span data-stu-id="9693a-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="9693a-124">Om du vill toouse en meddelandehubb som du redan har skapat kan du hoppa över toostep 5.</span><span class="sxs-lookup"><span data-stu-id="9693a-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p><span data-ttu-id="9693a-125">Klicka på hello <b>Notification Services</b> knapp i hello <b>inställningar</b> bladet välj <b>Apple (APNS)</b>.</span><span class="sxs-lookup"><span data-stu-id="9693a-125">Click hello <b>Notification Services</b> button in hello <b>Settings</b> blade, then select <b>Apple (APNS)</b>.</span></span> <span data-ttu-id="9693a-126">Klicka på <b>överför certifikat</b> och välj hello <b>.p12</b> -fil som du exporterade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9693a-126">Click on <b>Upload Certificate</b> and select hello <b>.p12</b> file that you exported earlier.</span></span> <span data-ttu-id="9693a-127">Kontrollera att du även ange hello rätt lösenord.</span><span class="sxs-lookup"><span data-stu-id="9693a-127">Make sure you also specify hello correct password.</span></span></p>

<p><span data-ttu-id="9693a-128">Se till att tooselect <b>Sandbox</b> läge eftersom det är för utveckling.</span><span class="sxs-lookup"><span data-stu-id="9693a-128">Make sure tooselect <b>Sandbox</b> mode since this is for development.</span></span> <span data-ttu-id="9693a-129">Använd bara hello <b>produktion</b> om du vill toosend push-meddelanden toousers som köpt din app hello butiken.</span><span class="sxs-lookup"><span data-stu-id="9693a-129">Only use hello <b>Production</b> if you want toosend push notifications toousers who purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="9693a-130">&emsp;&emsp;&emsp;&emsp;![Konfigurera APN i Azure-portalen](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span><span class="sxs-lookup"><span data-stu-id="9693a-130">&emsp;&emsp;&emsp;&emsp;![Configure APNS in Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span></span>

&emsp;&emsp;&emsp;&emsp;![Konfigurera APNS-certifikat i Azure-portalen](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

<span data-ttu-id="9693a-132">Din meddelandehubb är nu konfigurerad toowork med APNS och du har hello anslutning strängar tooregister din app och skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9693a-132">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-ios-app-toonotification-hubs"></a><span data-ttu-id="9693a-133">Ansluta tooNotification din iOS-app hub</span><span class="sxs-lookup"><span data-stu-id="9693a-133">Connect your iOS app tooNotification Hubs</span></span>
1. <span data-ttu-id="9693a-134">Skapa ett nytt iOS-projekt i Xcode och välj hello **program enkel vy** mall.</span><span class="sxs-lookup"><span data-stu-id="9693a-134">In Xcode, create a new iOS project and select hello **Single View Application** template.</span></span>
   
    ![Xcode – Program enkel vy][8]
    
2. <span data-ttu-id="9693a-136">När du anger hello alternativ för ditt nya projekt, kontrollera toouse hello samma **produktnamn** och **organisations-ID** som du använde när du har definierat hello paket-ID för hello Apple-utvecklare portalen.</span><span class="sxs-lookup"><span data-stu-id="9693a-136">When setting hello options for your new project, make sure toouse hello same **Product Name** and **Organization Identifier** that you used when you previously set hello bundle ID on hello Apple Developer portal.</span></span>
   
    ![Xcode – projektalternativ][11]
    
3. <span data-ttu-id="9693a-138">Under **mål**, klicka på ditt projektnamn, hello **Versionsinställningar** fliken och expandera **identitet för kodsignering**, och sedan under **felsöka**, ange din identitet för kodsignering.</span><span class="sxs-lookup"><span data-stu-id="9693a-138">Under **Targets**, click your project name, click hello **Build Settings** tab and expand **Code Signing Identity**, and then under **Debug**, set your code-signing identity.</span></span> <span data-ttu-id="9693a-139">Växla **nivåer** från **grundläggande** för**alla**, och ange **Etableringsprofil** toohello etableringsprofil som du skapade tidigare .</span><span class="sxs-lookup"><span data-stu-id="9693a-139">Toggle **Levels** from **Basic** too**All**, and set **Provisioning Profile** toohello provisioning profile that you created previously.</span></span>
   
    <span data-ttu-id="9693a-140">Om du inte ser hello nya etableringsprofil som du skapade i Xcode, försök att uppdatera hello profiler för din signeringsidentitet.</span><span class="sxs-lookup"><span data-stu-id="9693a-140">If you don't see hello new provisioning profile that you created in Xcode, try refreshing hello profiles for your signing identity.</span></span> <span data-ttu-id="9693a-141">Klicka på **Xcode** hello menyraden klickar du på **inställningar**, klickar du på hello **konto** klickar du på hello **visa information om** , klicka på din signering identitet och klicka sedan på hello uppdateringsknappen i hello nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="9693a-141">Click **Xcode** on hello menu bar, click **Preferences**, click hello **Account** tab, click hello **View Details** button, click your signing identity, and then click hello refresh button in hello bottom-right corner.</span></span>
   
    ![Xcode – etableringsprofil][9]
4. <span data-ttu-id="9693a-143">Hämta hello [Mobile Services iOS SDK version 1.2.4] och packa hello-filen.</span><span class="sxs-lookup"><span data-stu-id="9693a-143">Download hello [Mobile Services iOS SDK version 1.2.4] and unzip hello file.</span></span> <span data-ttu-id="9693a-144">Högerklicka på ditt projekt i Xcode och klicka på hello **Lägg till filer i** alternativet tooadd hello **WindowsAzureMessaging.framework** mappen tooyour Xcode-projekt.</span><span class="sxs-lookup"><span data-stu-id="9693a-144">In Xcode, right-click your project and click hello **Add Files to** option tooadd hello **WindowsAzureMessaging.framework** folder tooyour Xcode project.</span></span> <span data-ttu-id="9693a-145">Välj **Kopiera objekt vid behov** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9693a-145">Select **Copy items if needed**, and then click **Add**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9693a-146">hello notification hubs SDK inte stöder för närvarande bitcode på Xcode 7.</span><span class="sxs-lookup"><span data-stu-id="9693a-146">hello notification hubs SDK does not currently support bitcode on Xcode 7.</span></span>  <span data-ttu-id="9693a-147">Du måste ange **aktivera Bitcode** för**nr** i hello **Versionsalternativ** för projektet.</span><span class="sxs-lookup"><span data-stu-id="9693a-147">You must set **Enable Bitcode** too**No** in hello **Build Options** for your project.</span></span>
   > 
   > 
   
    ![Packa upp Azure SDK][10]
5. <span data-ttu-id="9693a-149">Lägg till ett nytt huvud filen tooyour projekt med namnet `HubInfo.h`.</span><span class="sxs-lookup"><span data-stu-id="9693a-149">Add a new header file tooyour project named `HubInfo.h`.</span></span> <span data-ttu-id="9693a-150">Den här filen innehåller hello konstanterna för din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="9693a-150">This file will hold hello constants for your notification hub.</span></span>  <span data-ttu-id="9693a-151">Lägg till hello följande definitioner och Ersätt hello-platshållarnas textsträngar med ditt *hubbnamn* och hello *DefaultListenSharedAccessSignature* du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9693a-151">Add hello following definitions and replace hello string literal placeholders with your *hub name* and hello *DefaultListenSharedAccessSignature* that you noted earlier.</span></span>
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. <span data-ttu-id="9693a-152">Öppna din `AppDelegate.h` filen Lägg till följande importeringsdirektiv hello:</span><span class="sxs-lookup"><span data-stu-id="9693a-152">Open your `AppDelegate.h` file add hello following import directives:</span></span>
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. <span data-ttu-id="9693a-153">I din `AppDelegate.m file`, Lägg till följande kod i hello hello `didFinishLaunchingWithOptions` metod baserat på din iOS-version.</span><span class="sxs-lookup"><span data-stu-id="9693a-153">In your `AppDelegate.m file`, add hello following code in hello `didFinishLaunchingWithOptions` method based on your version of iOS.</span></span> <span data-ttu-id="9693a-154">Den här koden registrerar din enhetshantering med APNS:</span><span class="sxs-lookup"><span data-stu-id="9693a-154">This code registers your device handle with APNs:</span></span>
   
    <span data-ttu-id="9693a-155">För iOS 8:</span><span class="sxs-lookup"><span data-stu-id="9693a-155">For iOS 8:</span></span>
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    <span data-ttu-id="9693a-156">För iOS-versioner tidigare too8:</span><span class="sxs-lookup"><span data-stu-id="9693a-156">For iOS versions prior too8:</span></span>
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. <span data-ttu-id="9693a-157">I hello samma fil, Lägg till hello följande metoder.</span><span class="sxs-lookup"><span data-stu-id="9693a-157">In hello same file, add hello following methods.</span></span> <span data-ttu-id="9693a-158">Den här koden ansluter toohello meddelandehubben genom att använda hello anslutningsinformation som du angav i HubInfo.h.</span><span class="sxs-lookup"><span data-stu-id="9693a-158">This code connects toohello notification hub using hello connection information you specified in HubInfo.h.</span></span> <span data-ttu-id="9693a-159">Den lämnar sedan hello enhet token toohello meddelandehubben så att hello ska kunna skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="9693a-159">It then gives hello device token toohello notification hub so that hello notification hub can send notifications:</span></span>
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. <span data-ttu-id="9693a-160">Hej samma fil, lägger du till följande metod toodisplay hello i en **UIAlert** om hello meddelandet tas emot medan hello appen är aktiv:</span><span class="sxs-lookup"><span data-stu-id="9693a-160">In hello same file, add hello following method toodisplay a **UIAlert** if hello notification is received while hello app is active:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. <span data-ttu-id="9693a-161">Skapa och köra hello-app på din enhet tooverify att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="9693a-161">Build and run hello app on your device tooverify that there are no failures.</span></span>

## <a name="send-test-push-notifications"></a><span data-ttu-id="9693a-162">Skicka test-push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="9693a-162">Send test push notifications</span></span>
<span data-ttu-id="9693a-163">Du kan testa att ta emot meddelanden i appen genom att skicka push-meddelanden i hello [Azure Portal] via hello **felsökning** avsnitt i hello hubbladet (Använd hello *prova att skicka* alternativet).</span><span class="sxs-lookup"><span data-stu-id="9693a-163">You can test receiving notifications in your app by sending push notifications in hello [Azure Portal] via hello **Troubleshooting** section in hello hub blade (use hello *Test Send* option).</span></span>

![Azure Portal – Prova att skicka][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a><span data-ttu-id="9693a-165">(Valfritt) Skicka push-meddelanden från hello app</span><span class="sxs-lookup"><span data-stu-id="9693a-165">(Optional) Send push notifications from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9693a-166">Det här exemplet för att skicka meddelanden från hello-klientappen tillhandahålls endast för.</span><span class="sxs-lookup"><span data-stu-id="9693a-166">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="9693a-167">Eftersom detta kräver hello `DefaultFullSharedAccessSignature` toobe finns på hello-klientappen, visar notification hub toohello risken att en användare kan få åtkomst till toosend obehörig meddelanden tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="9693a-167">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="9693a-168">Om du vill toosend push-meddelanden från en app kan det här avsnittet innehåller ett exempel på hur toodo detta med hjälp av hello REST-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9693a-168">If you want toosend push notifications from within an app, this section provides an example of how toodo this using hello REST interface.</span></span>

1. <span data-ttu-id="9693a-169">I Xcode öppnar `Main.storyboard` och Lägg till följande UI-komponenter från hello objektet biblioteket tooallow hello användaren toosend push-meddelanden i appen hello hello:</span><span class="sxs-lookup"><span data-stu-id="9693a-169">In Xcode, open `Main.storyboard` and add hello following UI components from hello object library tooallow hello user toosend push notifications in hello app:</span></span>
   
   * <span data-ttu-id="9693a-170">En etikett utan etikettext.</span><span class="sxs-lookup"><span data-stu-id="9693a-170">A label with no label text.</span></span> <span data-ttu-id="9693a-171">Den kommer att använda tooreport fel i skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9693a-171">It will be used tooreport errors in sending notifications.</span></span> <span data-ttu-id="9693a-172">Hej **rader** egenskapen ska anges för**0** så att den automatiskt kommer att storleksändras begränsad toohello högra och vänstra marginalerna samt hello överkant hello vyn.</span><span class="sxs-lookup"><span data-stu-id="9693a-172">hello **Lines** property should be set too**0** so that it will automatically size constrained toohello right and left margins and hello top of hello view.</span></span>
   * <span data-ttu-id="9693a-173">Ett textfält med **platshållare** ange text för**ange meddelande**.</span><span class="sxs-lookup"><span data-stu-id="9693a-173">A text field with **Placeholder** text set too**Enter Notification Message**.</span></span> <span data-ttu-id="9693a-174">Begränsa hello fältet precis under hello etiketten som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9693a-174">Constrain hello field just below hello label as shown below.</span></span> <span data-ttu-id="9693a-175">Ange hello View Controller som uttagsdelegat hello.</span><span class="sxs-lookup"><span data-stu-id="9693a-175">Set hello View Controller as hello outlet delegate.</span></span>
   * <span data-ttu-id="9693a-176">En knapp med titeln **skicka meddelande** begränsad precis under textfältet hello och hello vågräta center.</span><span class="sxs-lookup"><span data-stu-id="9693a-176">A button titled **Send Notification** constrained just below hello text field and in hello horizontal center.</span></span>
     
     <span data-ttu-id="9693a-177">hello vyn bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9693a-177">hello view should look as follows:</span></span>
     
     ![Xcode-designern][32]
2. <span data-ttu-id="9693a-179">[Lägg till uttag](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) för hello etikett och texten är anslutna till din vy och uppdatera din `interface` definition toosupport `UITextFieldDelegate` och `NSXMLParserDelegate`.</span><span class="sxs-lookup"><span data-stu-id="9693a-179">[Add outlets](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) for hello label and text field connected your view, and update your `interface` definition toosupport `UITextFieldDelegate` and `NSXMLParserDelegate`.</span></span> <span data-ttu-id="9693a-180">Lägg till hello tre egenskapsdeklarationer nedan toohelp stöd anropar hello REST-API och parsning hello-svar.</span><span class="sxs-lookup"><span data-stu-id="9693a-180">Add hello three property declarations shown below toohelp support calling hello REST API and parsing hello response.</span></span>
   
    <span data-ttu-id="9693a-181">Filen ViewController.h bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9693a-181">Your ViewController.h file should look as follows:</span></span>
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. <span data-ttu-id="9693a-182">Öppna `HubInfo.h` och Lägg till följande konstanter som ska användas för att skicka meddelanden tooyour hubb hello.</span><span class="sxs-lookup"><span data-stu-id="9693a-182">Open `HubInfo.h` and add hello following constants which will be used for sending notifications tooyour hub.</span></span> <span data-ttu-id="9693a-183">Ersätt hello platshållarens teckensträng med den faktiska *DefaultFullSharedAccessSignature* anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9693a-183">Replace hello placeholder string literal with your actual *DefaultFullSharedAccessSignature* connection string.</span></span>
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. <span data-ttu-id="9693a-184">Lägg till följande hello `#import` instruktioner tooyour `ViewController.h` fil.</span><span class="sxs-lookup"><span data-stu-id="9693a-184">Add hello following `#import` statements tooyour `ViewController.h` file.</span></span>
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. <span data-ttu-id="9693a-185">I `ViewController.m` Lägg till följande kod toohello implementeringsgränssnittet hello.</span><span class="sxs-lookup"><span data-stu-id="9693a-185">In `ViewController.m` add hello following code toohello interface implementation.</span></span> <span data-ttu-id="9693a-186">Den här koden kommer att parsa anslutningssträngen *DefaultFullSharedAccessSignature*.</span><span class="sxs-lookup"><span data-stu-id="9693a-186">This code will parse your *DefaultFullSharedAccessSignature* connection string.</span></span> <span data-ttu-id="9693a-187">Som anges i hello [REST API-referensen](http://msdn.microsoft.com/library/azure/dn495627.aspx), denna parsade information kommer att använda toogenerate en SaS-token för hello **auktorisering** huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="9693a-187">As mentioned in hello [REST API reference](http://msdn.microsoft.com/library/azure/dn495627.aspx), this parsed information will be used toogenerate a SaS token for hello **Authorization** request header.</span></span>
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. <span data-ttu-id="9693a-188">I `ViewController.m`, uppdatera hello `viewDidLoad` metoden tooparse hello anslutningssträngen när hello vyn läses in.</span><span class="sxs-lookup"><span data-stu-id="9693a-188">In `ViewController.m`, update hello `viewDidLoad` method tooparse hello connection string when hello view loads.</span></span> <span data-ttu-id="9693a-189">Lägg även till hello verktygsmetoderna, som visas nedan, toohello implementeringsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9693a-189">Also add hello utility methods, shown below, toohello interface implementation.</span></span>  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. <span data-ttu-id="9693a-190">I `ViewController.m`, Lägg till följande kod toohello gränssnittet implementering toogenerate hello SaS-autentiseringstoken som ska anges i hello hello **auktorisering** rubriken, som anges i hello [REST API Referens för](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="9693a-190">In `ViewController.m`, add hello following code toohello interface implementation toogenerate hello SaS authorization token that will be provided in hello **Authorization** header, as mentioned in hello [REST API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span></span>
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. <span data-ttu-id="9693a-191">CTRL -dra från hello **skicka meddelande** knappen för`ViewController.m` tooadd en åtgärd med namnet **SendNotificationMessage** för hello **Touch Down** händelse.</span><span class="sxs-lookup"><span data-stu-id="9693a-191">Ctrl+drag from hello **Send Notification** button too`ViewController.m` tooadd an action named **SendNotificationMessage** for hello **Touch Down** event.</span></span> <span data-ttu-id="9693a-192">Uppdatera metoden med hello följande kod toosend hello meddelanden med hello REST API.</span><span class="sxs-lookup"><span data-stu-id="9693a-192">Update method with hello following code toosend hello notification using hello REST API.</span></span>
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. <span data-ttu-id="9693a-193">I `ViewController.m`, lägga till hello följande ombud metoden toosupport stänger hello tangentbordet för textfältet hello.</span><span class="sxs-lookup"><span data-stu-id="9693a-193">In `ViewController.m`, add hello following delegate method toosupport closing hello keyboard for hello text field.</span></span> <span data-ttu-id="9693a-194">CTRL -dra från hello text fältet toohello View Controller ikonen i hello gränssnittet designer tooset hello visa controller som uttagsdelegat hello.</span><span class="sxs-lookup"><span data-stu-id="9693a-194">Ctrl+drag from hello text field toohello View Controller icon in hello interface designer tooset hello view controller as hello outlet delegate.</span></span>
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. <span data-ttu-id="9693a-195">I `ViewController.m`, Lägg till följande hello delegera metoder toosupport tolkning hello svaret med hjälp av `NSXMLParser`.</span><span class="sxs-lookup"><span data-stu-id="9693a-195">In `ViewController.m`, add hello following delegate methods toosupport parsing hello response by using `NSXMLParser`.</span></span>
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. <span data-ttu-id="9693a-196">Skapa hello projektet och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="9693a-196">Build hello project and verify that there are no errors.</span></span>

> [!NOTE]
> <span data-ttu-id="9693a-197">Om du får ett build-fel i Xcode7 om bitcode-support, bör du ändra hello **Versionsinställningar** > **aktivera Bitcode (ENABLE_BITCODE)** för**nr** i Xcode.</span><span class="sxs-lookup"><span data-stu-id="9693a-197">If you encounter a build error in Xcode7 about bitcode support, you should change hello **Build Settings** > **Enable Bitcode (ENABLE_BITCODE)** too**NO** in Xcode.</span></span> <span data-ttu-id="9693a-198">hello Notification Hubs SDK stöder för närvarande inte bitcode.</span><span class="sxs-lookup"><span data-stu-id="9693a-198">hello Notification Hubs SDK does not currently support bitcode.</span></span> 
> 
> 

<span data-ttu-id="9693a-199">Du hittar alla hello möjliga nyttolaster för meddelanden i hello Apple [Push Notification Programmeringsguide för lokala och].</span><span class="sxs-lookup"><span data-stu-id="9693a-199">You can find all hello possible notification payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

## <a name="checking-if-your-app-can-receive-push-notifications"></a><span data-ttu-id="9693a-200">Kontrollera om din app kan ta emot push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="9693a-200">Checking if your app can receive push notifications</span></span>
<span data-ttu-id="9693a-201">tootest push-meddelanden på iOS, måste du distribuera hello app tooa fysisk iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="9693a-201">tootest push notifications on iOS, you must deploy hello app tooa physical iOS device.</span></span> <span data-ttu-id="9693a-202">Du kan inte skicka Apple push-meddelanden med hjälp av hello iOS-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="9693a-202">You cannot send Apple push notifications by using hello iOS Simulator.</span></span>

1. <span data-ttu-id="9693a-203">Kör hello appen och verifiera att registreringen lyckas och tryck sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9693a-203">Run hello app and verify that registration succeeds, and then press **OK**.</span></span>
   
    ![Registreringstest för push-meddelanden i iOS-appar][33]
2. <span data-ttu-id="9693a-205">Du kan skicka ett test-push-meddelande från hello [Azure Portal], enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="9693a-205">You can send a test push notification from hello [Azure Portal], as described above.</span></span> <span data-ttu-id="9693a-206">Om du har lagt till kod för att skicka push-meddelanden i appen hello touch inuti hello text fältet tooenter ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="9693a-206">If you added code for sending push notifications in hello app, touch inside hello text field tooenter a notification message.</span></span> <span data-ttu-id="9693a-207">Tryck på hello **skicka** knappen på tangentbordet hello eller hello **skicka meddelande** knapp i hello visa toosend hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="9693a-207">Then press hello **Send** button on hello keyboard or hello **Send Notification** button in hello view toosend hello notification message.</span></span>
   
    ![Test av att skicka push-meddelanden i iOS-appar][34]
3. <span data-ttu-id="9693a-209">hello push-meddelanden skickas tooall-enheter som är registrerade tooreceive hello meddelanden från hello viss Notification Hub.</span><span class="sxs-lookup"><span data-stu-id="9693a-209">hello push notification is sent tooall devices that are registered tooreceive hello notifications from hello particular Notification Hub.</span></span>
   
    ![Test av att ta emot push-meddelanden i iOS-appar][35]

## <a name="next-steps"></a><span data-ttu-id="9693a-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9693a-211">Next steps</span></span>
<span data-ttu-id="9693a-212">I det här enkla exemplet skickade du push-meddelanden tooall dina registrerade iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="9693a-212">In this simple example, you broadcasted push notifications tooall your registered iOS devices.</span></span> <span data-ttu-id="9693a-213">Vi rekommenderar att som ett nästa steg i din utbildning du går vidare toohello [Azure Notification Hubs Meddelandeanvändare för iOS med .NET-serverdel] kursen vägleder dig genom att skapa en serverdel toosend push-meddelanden med taggar.</span><span class="sxs-lookup"><span data-stu-id="9693a-213">We suggest as a next step in your learning that you proceed toohello [Azure Notification Hubs Notify Users for iOS with .NET backend] tutorial, which will walk you through creating a backend toosend push notifications using tags.</span></span> 

<span data-ttu-id="9693a-214">Om du vill toosegment in användarna efter intressegrupper, kan du även gå på toohello [använda Notification Hubs toosend senaste nytt] kursen.</span><span class="sxs-lookup"><span data-stu-id="9693a-214">If you want toosegment your users by interest groups, you can additionally move on toohello [Use Notification Hubs toosend breaking news] tutorial.</span></span> 

<span data-ttu-id="9693a-215">Allmän information om Notification Hubs finns i [Riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="9693a-215">For general information about Notification Hubs, see [Notification Hubs Guidance].</span></span>

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK version 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Meddelandeanvändare för iOS med .NET-serverdel]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Push Notification Programmeringsguide för lokala och]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
