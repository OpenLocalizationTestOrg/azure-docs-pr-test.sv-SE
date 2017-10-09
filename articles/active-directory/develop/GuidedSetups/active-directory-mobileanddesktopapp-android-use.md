---
title: "aaaAzure AD v2 Android komma igång - Använd | Microsoft Docs"
description: "Hur en Android-app kan få en åtkomst-token och anropa API: erna som kräver åtkomst-token från Azure Active Directory v2 slutpunkten eller Microsoft Graph API"
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
ms.openlocfilehash: 4480d89eb7638fe7d588c8cebd2b1e3c9d4c6e3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Använd hello Microsoft Authentication Library (MSAL) tooget en token för hello Microsoft Graph API

1.  Öppna: `MainActivity` (under `app`  >  `java`  >  `{domain}.{appname}`)
2.  Lägg till hello följande importer:

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Ersätt hello `MainActivity` klassen med nedan:
</li>
</ol>

```java
public class MainActivity extends AppCompatActivity {

    final static String CLIENT_ID = "[Enter hello application Id here]";
    final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
    final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

    /* UI & Debugging Variables */
    private static final String TAG = MainActivity.class.getSimpleName();
    Button callGraphButton;
    Button signOutButton;

    /* Azure AD Variables */
    private PublicClientApplication sampleApp;
    private AuthenticationResult authResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        callGraphButton = (Button) findViewById(R.id.callGraph);
        signOutButton = (Button) findViewById(R.id.clearCache);

        callGraphButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onCallGraphClicked();
            }
        });

        signOutButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onSignOutClicked();
            }
        });

  /* Configure your sample app and save state for this activity */
        sampleApp = null;
        if (sampleApp == null) {
            sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    CLIENT_ID);
        }

  /* Attempt tooget a user and acquireTokenSilent
   * If this fails we do an interactive request
   */
        List<User> users = null;

        try {
            users = sampleApp.getUsers();

            if (users != null && users.size() == 1) {
          /* We have 1 user */

                sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
            } else {
          /* We have no user */

          /* Let's do an interactive request */
                sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
            }
        } catch (MsalClientException e) {
            Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

        } catch (IndexOutOfBoundsException e) {
            Log.d(TAG, "User at this position does not exist: " + e.toString());
        }

    }

//
// App callbacks for MSAL
// ======================
// getActivity() - returns activity so we can acquireToken within a callback
// getAuthSilentCallback() - callback defined toohandle acquireTokenSilent() case
// getAuthInteractiveCallback() - callback defined toohandle acquireToken() case
//

    public Activity getActivity() {
        return this;
    }

    /* Callback method for acquireTokenSilent calls 
     * Looks if tokens are in hello cache (refreshes if necessary and if we don't forceRefresh)
     * else errors that we need toodo an interactive request.
     */
    private AuthenticationCallback getAuthSilentCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call Graph now */
                Log.d(TAG, "Successfully authenticated");

            /* Store hello authResult */
                authResult = authenticationResult;

            /* call graph */
                callGraphAPI();

            /* update hello UI toopost call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed tooacquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with hello STS, likely config issue */
                } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled hello authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }


    /* Callback used for interactive request.  If succeeds we use hello access
         * token toocall hello Microsoft Graph. Does not check cache
         */
    private AuthenticationCallback getAuthInteractiveCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
                Log.d(TAG, "Successfully authenticated");
                Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store hello auth result */
                authResult = authenticationResult;

            /* call Graph */
                callGraphAPI();

            /* update hello UI toopost call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed tooacquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with hello STS, likely config issue */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled hello authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }

    /* Set hello UI for successful token acquisition data */
    private void updateSuccessUI() {
        callGraphButton.setVisibility(View.INVISIBLE);
        signOutButton.setVisibility(View.VISIBLE);
        findViewById(R.id.welcome).setVisibility(View.VISIBLE);
        ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                authResult.getUser().getName());
        findViewById(R.id.graphData).setVisibility(View.VISIBLE);
    }

    /* Use MSAL tooacquireToken for hello end-user
     * Callback will call Graph api w/ access token & update UI
     */
    private void onCallGraphClicked() {
        sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
    }

    /* Handles hello redirect from hello System Browser */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

}
```
<!--start-collapse-->
### <a name="more-information"></a>Mer information
#### <a name="getting-a-user-token-interactive"></a>Hämta en användartoken interaktiva
Anropar hello `AcquireTokenAsync` metoden resulterar i ett fönster som uppmanar hello användaren toosign i. Program kräver vanligen en användare toosign i interaktivt hello första gången som de behöver tooaccess en skyddad resurs eller när en tyst åtgärden tooacquire en token misslyckas (t.ex. hello användarens lösenord har upphört att gälla).

#### <a name="getting-a-user-token-silently"></a>Hämta token för en användare tyst
`AcquireTokenSilentAsync`hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion. Efter `AcquireTokenAsync` för hello körs första gången `AcquireTokenSilentAsync` hello metod som används ofta tooobtain token tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.
Slutligen `AcquireTokenSilentAsync` misslyckas – t.ex. hello användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet. När MSAL känner av att hello problemet kan lösas genom att kräva en interaktiv åtgärd den utlöses en `MsalUiRequiredException`. Programmet kan hantera det här undantaget på två sätt:

1.  Gör ett anrop mot `AcquireTokenAsync` omedelbart, vilket innebär att fråga användaren hello toosign i. Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i hello program tillgängliga för hello användare. hello exemplet skapas vid interaktiv installationen använder det här mönstret: du kan se den i åtgärden hello första gången du kör hello exempel: eftersom ingen användare har använt hello programmet `PublicClientApp.Users.FirstOrDefault` innehåller ett null-värde och ett `MsalUiRequiredException` undantag. Hej koden i hello exempel sedan handtag hello undantag genom att anropa `AcquireTokenAsync` vilket resulterar i att fråga användaren hello toosign i.
2.  Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `AcquireTokenSilentAsync` vid ett senare tillfälle. Detta används vanligtvis när hello användaren är kan tooaccess funktion hello programmet utan att avbrytas – t.ex, det finns offline innehåll i programmet hello. I det här fallet hello användare kan bestämma när de vill toosign i tooaccess hello skyddade resursen toorefresh hello inaktuell information eller ditt program kan bestämma tooretry `AcquireTokenSilentAsync` när nätverket återställs efter att ha tillfälligt otillgänglig.
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Anropa hello Microsoft Graph API använder du bara fick hello-token
1.  Lägg till följande metoder i hello hello `MainActivity` klass:

```java
/* Use Volley toomake an HTTP request toohello /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request toograph");

    /* Make sure we have a token toosend toograph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed tooput parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send tooUI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET tooQueue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets hello Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Mer information om hur du använder ett REST-anrop mot ett skyddade API

I det här exempelprogrammet `callGraphAPI` anrop `getAccessToken` och gör ett HTTP `GET` begäran mot en resurs som kräver ett token och returnerar hello innehåll. Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*. För det här exemplet hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.
<!--end-collapse-->

## <a name="setup-sign-out"></a>Installationsprogrammet utloggning

1.  Lägg till följande metoder i hello hello `MainActivity` klass:

```java
/* Clears a user's tokens from hello cache.
 * Logically similar too"sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt tooget a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set hello UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information"></a>Mer information

`onSignOutClicked`ovan tar bort hello användare från MSAL användarcachen – detta effektivt talar MSAL tooforget hello aktuell användare så att en begäran om framtida tooacquire en token lyckas bara om det görs toobe interaktiva.
Hello-programmet i det här exemplet stöder en enskild användare, MSAL har stöd för scenarier där flera konton kan vara inloggad på hello samtidigt – ett exempel är ett e-postprogram där en användare har flera konton.
<!--end-collapse-->
