
1. Öppna hello-projekt i Android Studio.

2. I **Projektutforskaren** i Android Studio öppna hello ToDoActivity.java-filen och Lägg till följande importuttryck hello:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. Lägg till följande metod toohello hello **ToDoActivity** klass:

        // You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using hello Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check hello request code matches hello one we send in hello login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check hello error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    Den här koden skapar en metod toohandle hello Google autentiseringsprocessen. En dialogruta visas hello-ID för hello autentiserade användare. Du kan bara fortsätta på en lyckad autentisering.

    > [!NOTE]
    > Om du använder en identitetsleverantör än Google ändrar hello-värdet som skickades toohello **inloggning** metoden tooone av hello följande värden: _MicrosoftAccount_, _Facebook_, _Twitter_, eller _windowsazureactivedirectory_.

4. I hello **onCreate** metod, Lägg till följande kodrad efter hello som instantierar hello hello `MobileServiceClient` objekt.

        authenticate();

    Det här anropet startar hello autentiseringsprocessen.

5. Flytta hello återstående kod efter `authenticate();` i hello **onCreate** metoden tooa nya **createTable** metod:

        private void createTable() {

            // Get hello table instance toouse.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter toobind hello items with hello view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load hello items from Azure.
            refreshItemsFromTable();
        }

6. tooensure omdirigering fungerar som förväntat, lägger du till följande fragment av hello _RedirectUrlActivity_ too_AndroidManifest.xml_:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. Lägg till redirectUriScheme too_build.gradle_ för din Android-App.

        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

8. Lägg till com.android.support:customtabs:23.0.1 toohello beroenden i din build.gradle:

      beroenden {/ /... kompilera 'com.android.support:customtabs:23.0.1'}

9. Från hello **kör** -menyn klickar du på **kör app** toostart hello appen och logga in med ditt valda identitetsleverantör.

> [!WARNING]
> hello URL-schema som nämns är skiftlägeskänslig.  Se till att alla förekomster av `{url_scheme_of_you_app}` Använd hello samma ärende.

När du har loggat in, hello app körs utan fel och du bör kunna tooquery hello backend-tjänst och göra uppdateringar toodata.
