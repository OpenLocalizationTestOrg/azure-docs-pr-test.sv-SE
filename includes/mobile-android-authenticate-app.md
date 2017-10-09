
1. <span data-ttu-id="31421-101">Öppna hello-projekt i Android Studio.</span><span class="sxs-lookup"><span data-stu-id="31421-101">Open hello project in Android Studio.</span></span>

2. <span data-ttu-id="31421-102">I **Projektutforskaren** i Android Studio öppna hello ToDoActivity.java-filen och Lägg till följande importuttryck hello:</span><span class="sxs-lookup"><span data-stu-id="31421-102">In **Project Explorer** in Android Studio, open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. <span data-ttu-id="31421-103">Lägg till följande metod toohello hello **ToDoActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="31421-103">Add hello following method toohello **ToDoActivity** class:</span></span>

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

    <span data-ttu-id="31421-104">Den här koden skapar en metod toohandle hello Google autentiseringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="31421-104">This code creates a method toohandle hello Google authentication process.</span></span> <span data-ttu-id="31421-105">En dialogruta visas hello-ID för hello autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="31421-105">A dialog displays hello ID of hello authenticated user.</span></span> <span data-ttu-id="31421-106">Du kan bara fortsätta på en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="31421-106">You can only proceed on a successful authentication.</span></span>

    > [!NOTE]
    > <span data-ttu-id="31421-107">Om du använder en identitetsleverantör än Google ändrar hello-värdet som skickades toohello **inloggning** metoden tooone av hello följande värden: _MicrosoftAccount_, _Facebook_, _Twitter_, eller _windowsazureactivedirectory_.</span><span class="sxs-lookup"><span data-stu-id="31421-107">If you are using an identity provider other than Google, change hello value passed toohello **login** method tooone of hello following values: _MicrosoftAccount_, _Facebook_, _Twitter_, or _windowsazureactivedirectory_.</span></span>

4. <span data-ttu-id="31421-108">I hello **onCreate** metod, Lägg till följande kodrad efter hello som instantierar hello hello `MobileServiceClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="31421-108">In hello **onCreate** method, add hello following line of code after hello code that instantiates hello `MobileServiceClient` object.</span></span>

        authenticate();

    <span data-ttu-id="31421-109">Det här anropet startar hello autentiseringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="31421-109">This call starts hello authentication process.</span></span>

5. <span data-ttu-id="31421-110">Flytta hello återstående kod efter `authenticate();` i hello **onCreate** metoden tooa nya **createTable** metod:</span><span class="sxs-lookup"><span data-stu-id="31421-110">Move hello remaining code after `authenticate();` in hello **onCreate** method tooa new **createTable** method:</span></span>

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

6. <span data-ttu-id="31421-111">tooensure omdirigering fungerar som förväntat, lägger du till följande fragment av hello _RedirectUrlActivity_ too_AndroidManifest.xml_:</span><span class="sxs-lookup"><span data-stu-id="31421-111">tooensure redirection works as expected, add hello following snippet of _RedirectUrlActivity_ too_AndroidManifest.xml_:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. <span data-ttu-id="31421-112">Lägg till redirectUriScheme too_build.gradle_ för din Android-App.</span><span class="sxs-lookup"><span data-stu-id="31421-112">Add redirectUriScheme too_build.gradle_ of your Android application.</span></span>

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

8. <span data-ttu-id="31421-113">Lägg till com.android.support:customtabs:23.0.1 toohello beroenden i din build.gradle:</span><span class="sxs-lookup"><span data-stu-id="31421-113">Add com.android.support:customtabs:23.0.1 toohello dependencies in your build.gradle:</span></span>

      <span data-ttu-id="31421-114">beroenden {/ /... kompilera 'com.android.support:customtabs:23.0.1'}</span><span class="sxs-lookup"><span data-stu-id="31421-114">dependencies {        // ...        compile 'com.android.support:customtabs:23.0.1'    }</span></span>

9. <span data-ttu-id="31421-115">Från hello **kör** -menyn klickar du på **kör app** toostart hello appen och logga in med ditt valda identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="31421-115">From hello **Run** menu, click **Run app** toostart hello app and sign in with your chosen identity provider.</span></span>

> [!WARNING]
> <span data-ttu-id="31421-116">hello URL-schema som nämns är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="31421-116">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="31421-117">Se till att alla förekomster av `{url_scheme_of_you_app}` Använd hello samma ärende.</span><span class="sxs-lookup"><span data-stu-id="31421-117">Ensure that all occurrences of `{url_scheme_of_you_app}` use hello same case.</span></span>

<span data-ttu-id="31421-118">När du har loggat in, hello app körs utan fel och du bör kunna tooquery hello backend-tjänst och göra uppdateringar toodata.</span><span class="sxs-lookup"><span data-stu-id="31421-118">When you are successfully signed in, hello app should run without errors, and you should be able tooquery hello back-end service and make updates toodata.</span></span>
