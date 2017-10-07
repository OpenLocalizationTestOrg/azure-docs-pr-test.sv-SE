---
title: aaaSending Secure Push-meddelanden med Azure Notification Hubs
description: "Lär dig hur säker toosend push-meddelanden tooan Android-app från Azure. Kodexempel som skrivits i Java- och C#."
documentationcenter: android
keywords: push-meddelande, push-meddelanden, push-meddelanden, android push-meddelanden
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d07943c4691ed07acb987086228ef565e6281d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Skicka säkra Push-meddelanden med Azure Notification Hubs
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Översikt
> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push meddelandet infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila plattformar.

På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur. Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klienten Android-enhet och hello-appserverdelen.

På en hög nivå är hello flödet följande:

1. hello appens serverdel:
   * Lagrar säker nyttolast i backend-databas.
   * Skickar hello-ID för det här meddelandet toohello Android-enhet (ingen säker information skickas).
2. hello appen på hello enhet, när du tar emot hello-meddelande:
   * hello Android-enhet kontaktar hello backend-begärande hello säker nyttolast.
   * hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.

Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in. Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token. Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello push-meddelanden visa ett allmänt meddelande som uppmanar hello användaren toolaunch hello app. hello app sedan autentiserar hello användare och visar hello notification nyttolast.

Den här kursen visar hur säker toosend push-meddelanden. Den bygger på hello [meddela användare](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först om du inte redan har gjort.

> [!NOTE]
> Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a>Ändra hello Android-projekt
Nu när du har ändrat din app backend-toosend bara hello *id* för push-meddelanden du har toochange din Android-app toohandle att meddelanden och motringning backend-tooretrieve-hello säkra meddelandet toobe visas.
tooachieve det här målet, har toomake till att din Android-app vet hur tooauthenticate med din serverdel när den tar emot hello push-meddelanden.

Vi kommer nu att ändra hello *inloggning* flödet i ordning toosave hello autentisering huvudets värde i hello delade inställningar för din app. Liknande mekanismer kan vara används toostore någon autentiseringstoken (t.ex. OAuth-token) som hello app har toouse utan att användarens autentiseringsuppgifter.

1. Lägg till följande konstanter hello överst i hello hello i projektet Android-app **MainActivity** klass:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Fortfarande i hello **MainActivity** klass, uppdatering hello `getAuthorizationHeader()` metoden toocontain hello följande kod:
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. Lägg till följande hello `import` instruktioner överst hello i hello **MainActivity** fil:
   
        import android.content.SharedPreferences;

Nu ska vi ändra hello hanterare som anropas när hello meddelandet tas emot.

1. I hello **MyHandler** klassen ändra hello `OnReceive()` metoden toocontain:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Lägg sedan till hello `retrieveNotification()` metod, ersätter hello platshållare `{back-end endpoint}` med hello backend-slutpunkt som erhålls vid distribution av din serverdel:
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed tooretrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

Den här metoden anropar app backend-tooretrieve hello-meddelande innehåll med hello autentiseringsuppgifter lagras i hello delade inställningar och visar den som ett vanligt meddelande. hello-meddelande verkar toohello app användaren exakt samma sätt som andra push-meddelande.

Observera att det är bättre toohandle hello fall egenskap som saknas autentisering sidhuvud eller avvisning hello serverdel. hello Särskild hantering av dessa fall beror huvudsakligen på användarupplevelsen mål. Ett alternativ är toodisplay ett meddelande med en allmän fråga om hello tooauthenticate tooretrieve hello faktiska användarmeddelande.

## <a name="run-hello-application"></a>Kör hello program
toorun Hej program, hello följande:

1. Kontrollera att **AppBackend** distribueras i Azure. Om du använder Visual Studio, köra hello **AppBackend** Web API-program. En ASP.NET-webbsida visas.
2. Kör hello app på en fysisk enhet eller hello androidemulator i Eclipse.
3. Ange ett användarnamn och lösenord i hello Android app Användargränssnittet. Dessa kan vara valfri sträng, men de måste vara hello samma värde.
4. I hello Android UI-app, klickar du på **logga in**. Klicka på **skicka push**.

