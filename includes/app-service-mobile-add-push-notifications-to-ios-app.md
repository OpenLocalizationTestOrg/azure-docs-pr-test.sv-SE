
<span data-ttu-id="0c717-101">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="0c717-101">**Objective-C**:</span></span>

1. <span data-ttu-id="0c717-102">I **QSAppDelegate.m**, importera hello iOS SDK och **QSTodoService.h**:</span><span class="sxs-lookup"><span data-stu-id="0c717-102">In **QSAppDelegate.m**, import hello iOS SDK and **QSTodoService.h**:</span></span>
   
        #import <MicrosoftAzureMobile/MicrosoftAzureMobile.h>
        #import "QSTodoService.h"
2. <span data-ttu-id="0c717-103">I `didFinishLaunchingWithOptions` i **QSAppDelegate.m**, infoga hello följande rader precis innan `return YES;`:</span><span class="sxs-lookup"><span data-stu-id="0c717-103">In `didFinishLaunchingWithOptions` in **QSAppDelegate.m**, insert hello following lines right before `return YES;`:</span></span>
   
        UIUserNotificationSettings* notificationSettings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeAlert | UIUserNotificationTypeBadge | UIUserNotificationTypeSound categories:nil];
        [[UIApplication sharedApplication] registerUserNotificationSettings:notificationSettings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
3. <span data-ttu-id="0c717-104">I **QSAppDelegate.m**, lägga till hello följande metoder för hanteraren.</span><span class="sxs-lookup"><span data-stu-id="0c717-104">In **QSAppDelegate.m**, add hello following handler methods.</span></span> <span data-ttu-id="0c717-105">Appen är nu uppdaterade toosupport push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0c717-105">Your app is now updated toosupport push notifications.</span></span> 
   
        // Registration with APNs is successful
        - (void)application:(UIApplication *)application
        didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
   
            QSTodoService *todoService = [QSTodoService defaultService];
            MSClient *client = todoService.client;
   
            [client.push registerDeviceToken:deviceToken completion:^(NSError *error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
            }];
        }
   
        // Handle any failure tooregister
        - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:
        (NSError *)error {
            NSLog(@"Failed tooregister for remote notifications: %@", error);
        }
   
        // Use userInfo in hello payload toodisplay an alert.
        - (void)application:(UIApplication *)application
              didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
   
            NSDictionary *apsPayload = userInfo[@"aps"];
            NSString *alertString = apsPayload[@"alert"];
   
            // Create alert with notification content.
            UIAlertController *alertController = [UIAlertController
                                          alertControllerWithTitle:@"Notification"
                                          message:alertString
                                          preferredStyle:UIAlertControllerStyleAlert];
   
            UIAlertAction *cancelAction = [UIAlertAction
                                           actionWithTitle:NSLocalizedString(@"Cancel", @"Cancel")
                                           style:UIAlertActionStyleCancel
                                           handler:^(UIAlertAction *action)
                                           {
                                               NSLog(@"Cancel");
                                           }];
   
            UIAlertAction *okAction = [UIAlertAction
                                       actionWithTitle:NSLocalizedString(@"OK", @"OK")
                                       style:UIAlertActionStyleDefault
                                       handler:^(UIAlertAction *action)
                                       {
                                           NSLog(@"OK");
                                       }];
   
            [alertController addAction:cancelAction];
            [alertController addAction:okAction];
   
            // Get current view controller.
            UIViewController *currentViewController = [[[[UIApplication sharedApplication] delegate] window] rootViewController];
            while (currentViewController.presentedViewController)
            {
                currentViewController = currentViewController.presentedViewController;
            }
   
            // Display alert.
            [currentViewController presentViewController:alertController animated:YES completion:nil];
   
        }

<span data-ttu-id="0c717-106">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="0c717-106">**Swift**:</span></span>

1. <span data-ttu-id="0c717-107">Lägg till fil **ClientManager.swift** med hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="0c717-107">Add file **ClientManager.swift** with hello following contents.</span></span> <span data-ttu-id="0c717-108">Ersätt *AppUrl %* med hello URL hello Azure mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="0c717-108">Replace *%AppUrl%* with hello URL of hello Azure Mobile App backend.</span></span>
   
        class ClientManager {
            static let sharedClient = MSClient(applicationURLString: "%AppUrl%")
        }
2. <span data-ttu-id="0c717-109">I **ToDoTableViewController.swift**, Ersätt hello `let client` raden som initierar en `MSClient` med den här raden:</span><span class="sxs-lookup"><span data-stu-id="0c717-109">In **ToDoTableViewController.swift**, replace hello `let client` line that initializes an `MSClient` with this line:</span></span>
   
        let client = ClientManager.sharedClient
3. <span data-ttu-id="0c717-110">I **AppDelegate.swift**, ersätta hello brödtexten i `func application` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0c717-110">In **AppDelegate.swift**, replace hello body of `func application` as follows:</span></span>
   
        func application(application: UIApplication,
          didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
           application.registerUserNotificationSettings(
               UIUserNotificationSettings(forTypes: [.Alert, .Badge, .Sound],
                   categories: nil))
           application.registerForRemoteNotifications()
           return true
        }
4. <span data-ttu-id="0c717-111">I **AppDelegate.swift**, lägga till hello följande metoder för hanteraren.</span><span class="sxs-lookup"><span data-stu-id="0c717-111">In **AppDelegate.swift**, add hello following handler methods.</span></span> <span data-ttu-id="0c717-112">Appen är nu uppdaterade toosupport push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0c717-112">Your app is now updated toosupport push notifications.</span></span>
   
        func application(application: UIApplication,
           didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
            ClientManager.sharedClient.push?.registerDeviceToken(deviceToken) { error in
                print("Error registering for notifications: ", error?.description)
            }
        }
   
        func application(application: UIApplication,
           didFailToRegisterForRemoteNotificationsWithError error: NSError) {
            print("Failed tooregister for remote notifications: ", error.description)
        }
   
        func application(application: UIApplication,
           didReceiveRemoteNotification userInfo: [NSObject: AnyObject]) {
   
            print(userInfo)
   
            let apsNotification = userInfo["aps"] as? NSDictionary
            let apsString       = apsNotification?["alert"] as? String
   
            let alert = UIAlertController(title: aaa"Alert", message: apsString, preferredStyle: .Alert)
            let okAction = UIAlertAction(title: aaa"OK", style: .Default) { _ in
                print("OK")
            }
            let cancelAction = UIAlertAction(title: aaa"Cancel", style: .Default) { _ in
                print("Cancel")
            }
   
            alert.addAction(okAction)
            alert.addAction(cancelAction)
   
            var currentViewController = self.window?.rootViewController
            while currentViewController?.presentedViewController != nil {
                currentViewController = currentViewController?.presentedViewController
            }
   
            currentViewController?.presentViewController(alert, animated: true) {}
   
        }

