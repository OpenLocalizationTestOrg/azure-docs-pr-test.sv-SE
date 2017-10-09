På grund av pågående utveckling kan hello Android SDK-versionen installerad i Android Studio inte matchar hello version i hello kod. hello Android SDK som refereras till i den här kursen är version 23 hello senaste vid hello tiden för skrivning. hello versionsnumret kan öka när nya versioner av Hej SDK visas och vi rekommenderar att du använder hello senaste versionen.

Två symptom på versionsmatchningsfel är:

- När du skapar eller återskapa hello-projekt kan du få Gradle felmeddelanden som ”**misslyckades toofind mål Google Inc.:Google APIs:n**”.
- Android Standardobjekt i koden som ska matcha baserat på `import` instruktioner kan generera felmeddelanden.

Om något av dessa visas, kan hello-versionen av hello Android SDK är installerat i Android Studio inte matchar hello SDK målet för hello hämtade projektet. tooverify hello version att Hej följande ändringar:

1. I Android Studio klickar du på **verktyg** > **Android** > **SDK Manager**. Om du inte har installerat hello senaste versionen av hello SDK-plattformen, tooinstall klicka sedan på den. Anteckna hello versionsnumret.
2. På hello **Projektutforskaren** fliken, under **Gradle skript**öppnar hello filen **build.gradle (modeule: app)**. Se till att hello **compileSdkVersion** och **buildToolsVersion** ställs toohello senaste SDK-versionen installerad. hello taggar kan se ut så här:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. Välj i hello Android Studio Project Explorer högerklickar du på projektnoden hello, **egenskaper**, och välj i hello vänstra kolumnen **Android**. Se till att hello **mål för projektgenerering** anges toohello samma SDK-version som hello **targetSdkVersion**.

I Android Studio är hello manifestfilen inte längre använda toospecify hello mål SDK och minsta SDK-version, till skillnad från hello fallet med Eclipse.
