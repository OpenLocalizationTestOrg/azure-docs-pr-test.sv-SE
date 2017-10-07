---
title: "aaaHow toouse hello Azure Mobile Apps-SDK för Android | Microsoft Docs"
description: "Hur toouse hello Azure Mobile Apps-SDK för Android"
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: glenga
ms.openlocfilehash: 56eb73c4e1703d69877be499a09fc2130f1d68e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a>Hur toouse hello Azure Mobile Apps-SDK för Android

Den här guiden visar hur toouse hello Android klient-SDK för Mobile Apps tooimplement vanliga scenarier, såsom:

* Frågar efter data (Infoga, uppdatera och ta bort).
* Autentisering.
* Hantera fel.
* Anpassa hello-klienten.

Den här guiden fokuserar på hello klientsidan Android SDK.  toolearn mer om hello serversidan SDK: er för Mobile Apps finns i avsnittet [arbeta med .NET-serverdel SDK] [ 10] eller [hur toouse hello Node.js-serverdel SDK] [ 11].

## <a name="reference-documentation"></a>Referensdokumentationen

Du kan hitta hello [Javadocs API-referens] [ 12] för hello Android klientbiblioteket på GitHub.

## <a name="supported-platforms"></a>Plattformar som stöds

hello Azure Mobile Apps-SDK för Android stöder API nivåer 19 till 24 (KitKat via Nougat) för Telefoner och surfplattor formfaktorer.  Autentisering, i synnerhet använder ett vanliga web framework metoden toogather autentiseringsuppgifter.  Server-flöde autentisering fungerar inte med liten faktor enheter, till exempel ur.

## <a name="setup-and-prerequisites"></a>Installationen och förutsättningar

Fullständig hello [Mobile Apps quickstart](app-service-mobile-android-get-started.md) kursen.  Den här åtgärden säkerställer att alla förutsättningar för att utveckla Azure Mobile Apps har uppfyllts.  hello Quickstart hjälper dig att konfigurera ditt konto och skapa din första mobilappsserverdel.

Om du inte toocomplete hello Snabbstartsguide slutföra hello följande uppgifter:

* [Skapa en mobilappsserverdel] [ 13] toouse med din Android-app.
* I Android Studio [uppdatering hello Gradle skapa filer](#gradle-build).
* [Aktivera internet behörigheten](#enable-internet).

### <a name="gradle-build"></a>Uppdatera hello Gradle skapa filen

Ändra både **build.gradle** filer:

1. Lägg till den här koden toohello *projekt* nivå **build.gradle** fil i hello *buildscript* tagg:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Lägg till den här koden toohello *modulen app* nivå **build.gradle** fil i hello *beroenden* tagg:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    Hello senaste versionen är för närvarande 3.3.0. hello stöds versioner visas [på bintray][14].

### <a name="enable-internet"></a>Aktivera internet behörighet

tooaccess Azure din app måste behörighet hello INTERNET aktiverad. Om den inte redan är aktiverat lägger du till följande rad med kod tooyour hello **AndroidManifest.xml** fil:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>Skapa en klientanslutning

Azure Mobile Apps innehåller fyra funktioner tooyour mobila program:

* Dataåtkomst och offlinesynkronisering med en Azure Mobile Apps-tjänst.
* Anropa anpassade API: er skrivs med hello Azure Mobile Apps Server SDK.
* Autentisering med Azure App Service-autentisering och auktorisering.
* Registrera push-meddelande med Notification Hubs.

Dessa funktioner först måste du skapa en `MobileServiceClient` objekt.  Endast en `MobileServiceClient` objekt ska skapas i din mobila klienten (det vill säga att det ska vara en Singleton-mönster).  toocreate en `MobileServiceClient` objekt:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

Hej `<MobileAppUrl>` är en sträng eller ett URL-objekt som pekar tooyour mobilserverdel.  Om du använder Azure App Service toohost din mobila serverdel sedan kontrollera att du använder hello säker `https://` version av hello-URL.

hello klienten kräver också åtkomst toohello aktivitet eller kontext - hello `this` parameter i hello exempel.  Hej MobileServiceClient konstruktionen ska ske inom hello `onCreate()` metod för hello aktiviteten som refereras i hello `AndroidManifest.xml` fil.

Som bästa praxis bör du abstrakt serverkommunikation i sin egen (singleton-mönster)-klassen.  I det här fallet bör du skickar hello aktiviteten inom hello konstruktorn tooappropriately konfigurera hello-tjänsten.  Exempel:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

Nu kan du anropa `AzureServiceAdapter.Initialize(this);` i hello `onCreate()` metod för den huvudsakliga aktiviteten.  Använda andra metoder som behöver toohello åtkomstklienten `AzureServiceAdapter.getInstance();` tooobtain ett referens toohello service-kort.

## <a name="data-operations"></a>Dataåtgärder

hello core av hello Azure Mobile Apps-SDK är tooprovide åtkomst toodata lagras i SQL Azure på hello mobilappsserverdel.  Du kan komma åt dessa data med strikt typkontroll klasser (rekommenderas) eller utan angiven frågor (rekommenderas inte).  hello huvuddelen av det här avsnittet behandlar med strikt typkontroll klasser.

### <a name="define-client-data-classes"></a>Definiera dataklasser som klienten

tooaccess data från SQL Azure-tabeller, definiera dataklasser som klienten som motsvarar toohello tabeller i hello mobilappsserverdel. Exemplen i det här avsnittet förutsätter att en tabell med namnet **MyDataTable**, som har hello följande kolumner:

* id
* Text
* Slutför

hello motsvarande skrivna klientsidan objektet finns i en fil med namnet **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Lägg till get och set-metoder för varje fält som du lägger till.  Om din SQL Azure-tabellen innehåller fler kolumner, kan du lägga till hello motsvarande fält toothis klass.  Till exempel, om hello DTO (data transfer objekt) hade en heltalskolumn prioritet och sedan kan du lägga till det här fältet och dess get- och set-metoder:

```java
private Integer priority;

/**
* Returns hello item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets hello item priority
*
* @param priority
*            priority tooset
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

toolearn hur toocreate ytterligare tabeller i Mobile Apps-serverdel Se [så här: definiera en tabell styrenhet] [ 15] (.NET-serverdel) eller [definiera tabeller med en dynamisk Schema] [ 16] (Node.js-serverdel).

En Azure Mobile Apps serverdel tabell definierar fem särskilda fält fyra som är tillgängliga tooclients:

* `String id`: hello globalt unikt ID för hello-post.  Som bästa praxis, se hello id hello strängrepresentation av en [UUID] [ 17] objekt.
* `DateTimeOffset updatedAt`: hello datum/tid för senaste uppdatering av hello.  Hej updatedAt fältet anges av hello-servern och aldrig ställas in av din klientkod.
* `DateTimeOffset createdAt`: hello tidsvärdet hello objektet skapades.  Hej createdAt fältet anges av hello-servern och aldrig ställas in av din klientkod.
* `byte[] version`: Hello-version ställs normalt representeras som en sträng, också in av hello-servern.
* `boolean deleted`: Anger att hello post har tagits bort men inte bort ännu.  Använd inte `deleted` som en egenskap i klassen.

Hej `id` fältet är obligatoriskt.  Hej `updatedAt` fält och `version` används för offlinesynkronisering (för matchning av inkrementell synkronisering och konflikt respektive).  Hej `createdAt` fältet är en referensfält och används inte av hello-klienten.  hello namn är ”över överföring” namnen på hello egenskaper och är inte ställas in.  Du kan dock skapa en mappning mellan objekt och hello ”över överföring” namn genom att använda hello [gson] [ 3] bibliotek.  Exempel:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getText() { return mVersion; }
    public final void setText(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>Skapa en tabellreferens

tooaccess en tabell, först skapa en [MobileServiceTable] [ 8] objekt genom att anropa hello **getTable** metod på hello [MobileServiceClient][9].  Den här metoden har två överlagringar:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

I följande kod, hello **mClient** är ett referensobjekt tooyour MobileServiceClient.  hello första överlagring används där hello klassnamn och hello tabellnamnet är hello samma och hello en används i hello Snabbstart:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

hello andra överlagring används när hello tabellnamn skiljer sig från hello klassnamn: hello första parametern är hello tabellnamn.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Fråga en Backend-tabell

Skaffa först en tabellreferens.  Kör en fråga på hello tabellreferens.  En fråga är en kombination av:

* En `.where()` [filtersats](#filtering).
* En `.orderBy()` [ordning satsen](#sorting).
* En `.select()` [fältet markeringen satsen](#selection).
* En `.skip()` och `.top()` för [växlingsbart systemminne resultat](#paging).

hello-satser måste vara angiven i hello föregående ordning.

### <a name="filter"></a>Filtrerar resultaten

hello allmänna form av en fråga är:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

hello returnerar föregående exempel alla resultat (upp toohello största storlek som hello server).  Hej `.execute()` metoden Kör hello fråga på hello serverdel.  hello frågan är konverterade tooan [OData v3] [ 19] fråga innan överföring toohello Mobile Apps-serverdel.  Erhåller konverterar hello Mobile Apps-serverdel hello frågan till en SQL-instruktion innan den körs på hello SQL Azure-instans.  Eftersom nätverksaktivitet tar en stund, hello `.execute()` metoden returnerar en [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Filtret returnerade data

hello efter körning av fråga returnerar alla objekt från hello **ToDoItem** tabellen var **fullständig** är lika med **FALSKT**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** är hello toohello Mobiltjänst referenstabellen som vi skapade tidigare.

Definiera ett filter som använder hello **där** metodanrop på hello tabellreferens. Hej **där** metoden följs av en **fältet** metoden följt av en metod som anger hello logiska predikat. Möjliga metoder kan predikat **eq** (motsvarar) **ne** (inte lika med), **gt** (större än), **ge** (större än eller lika med), **lt** (minst), **le** (mindre än eller lika med). Dessa metoder kan du jämföra antalet och strängen toospecific värdena i fälten.

Du kan filtrera efter datum. hello följande metoder kan du jämföra hello datumfält för hela eller delar av hello datum: **år**, **månad**, **dag**, **timme**, **minut**, och **andra**. hello följande exempel lägger till ett filter för objekt vars *förfallodatum* är lika med 2013.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

hello följande metoder stöder komplexa filter på strängfält: **startsWith**, **endsWith**, **concat**, **delsträngen**, **indexOf**, **ersätta**, **toLower**, **toUpper**, **trim**, och ** längden**. följande exempel filter för tabellen rader där hello hello *text* kolumnen som börjar med ”PRI0”.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

hello följande metoder för operatorn stöds på fälten: **lägga till**, **sub**, **mul**, **div**, **mod**, **våning**, **tak**, och **avrunda**. följande exempel filter för tabellen rader där hello hello **varaktighet** är ett jämnt tal.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

Du kan kombinera predikat med metoderna logiska: **och**, **eller** och **inte**. följande exempel hello kombinerar två hello föregående exempel.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Gruppera och kapsla logiska operatorer:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Mer detaljerad information och exempel på filtrering finns [utforska hello informationen hello Android klienten frågemodell][20].

### <a name="sorting"></a>Sortera returnerade data

hello följande kod returnerar alla objekt från en tabell med **ToDoItems** Sortera stigande efter hello *text* fältet. *mToDoTable* är hello toohello backend referenstabellen som du skapade tidigare:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

Hej första parametern för hello **orderBy** metoden är en sträng lika toohello namnet på vilka toosort hello fält. hello andra parametern använder hello **QueryOrder** uppräkningen toospecify om toosort stigande eller fallande.  Om du filtrerar med hello ***där*** metod, hello ***där*** metod måste anropas innan hello ***orderBy*** metod.

### <a name="selection"></a>Markera specifika kolumner

hello följande kod visar hur tooreturn alla objekt från en tabell med **ToDoItems**, men bara visar hello **fullständig** och **text** fält. **mToDoTable** är hello toohello backend referenstabellen som vi skapade tidigare.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

hello parametrar toohello väljer funktionen är hello sträng namnen på hello tabellens kolumner som du vill tooreturn.  Hej **Välj** metod måste toofollow metoder som **där** och **orderBy**. Det kan följas av sidindelning metoder som **hoppa över** och **upp**.

### <a name="paging"></a>Returnera data på sidor

Data är **alltid** returneras i sidor.  hello högsta antal poster returneras har angetts av hello-servern.  Om hello klient begär fler poster, returnerar hello servern hello högsta antal poster.  Hej maximala sidstorleken på hello server är 50 poster som standard.

hello första exemplet visar hur tooselect hello översta fem posterna från en tabell. hello frågan returnerar hello objekt från en tabell med **ToDoItems**. **mToDoTable** är hello toohello backend referenstabellen som du skapade tidigare:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Här är en fråga som hoppar över hello fem första objekten och sedan returnerar hello nästa fem:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Om du vill tooget alla poster i en tabell kan du implementera kod tooiterate över alla sidor:

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

En begäran om alla poster med den här metoden skapar minst två begäranden toohello Mobile Apps-serverdel.

> [!TIP]
> Att välja hello rätt sidstorleken är en avvägning mellan minnesanvändning när hello begäran pågår, bandbreddsanvändning och fördröjning i hello data togs emot helt.  hello standard (50 poster) är lämplig för alla enheter.  Om du använder uteslutande på större minnesenheter, öka upp too500.  Vi har hittat den ökande hello sidstorleken utöver 500 poster resultat i oacceptabel fördröjningar och stora minnesproblem.

### <a name="chaining"></a>Så här: sammanfoga frågan metoder

hello-metoder som används i frågor till backend-tabeller kan sammanfogas. Länkning frågan metoder kan du tooselect specifika kolumner filtrerade rader som sorteras och växlingsbart systemminne. Du kan skapa komplexa logiska filter.  Varje fråga-metoden returnerar ett frågeobjekt. tooend hello serie metoder och faktiskt kör hello frågan, anropet hello **köra** metod. Exempel:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

Hej sammankedjade frågan metoder måste ordnas på följande sätt:

1. Filtrering (**där**) metoder.
2. Sortering (**orderBy**) metoder.
3. Markeringen (**Välj**) metoder.
4. växling (**hoppa över** och **upp**) metoder.

## <a name="binding"></a>Binda data toohello användargränssnitt

Databindning omfattar tre komponenter:

* hello-datakälla
* hello skärmlayout
* hello kort att ties hello två tillsammans.

I vårt exempelkod returnerar vi hello data från hello Mobile Apps SQL Azure table **ToDoItem** i en matris. Den här aktiviteten är ett vanligt mönster för program.  Databasfrågor returnera ofta en mängd rader som hello hämtar klienten i en lista eller en matris. I det här exemplet är hello matris hello datakälla.  hello-kod anger skärmlayout som definierar hello vy över hello data som visas på hello enhet.  hello två är bundna tillsammans med en kort, som i den här koden är en utökning av hello **ArrayAdapter&lt;ToDoItem&gt; ** klass.

#### <a name="layout"></a>Definiera hello Layout

hello layout definieras av flera kodavsnitt för XML-koden. En befintlig layout får hello följande kod representerar hello **ListView** vi vill toopopulate med våra serverdata.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

I föregående kod hello, hello *listitem* attribut anger hello-id för hello layout för en enskild rad i hello-listan. Den här koden anger en kryssruta och tillhörande text och hämtar instansieras en gång för varje objekt i listan hello. Den här layouten visas inte hello **id** fältet och en mer komplicerad layout anger ytterligare fält i hello visas. Den här koden är i hello **row_list_to_do.xml** fil.

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="adapter"></a>Definiera hello nätverkskort
Eftersom hello datakällan i vår vyn är en matris med **ToDoItem**, vi underklass våra kort från en **ArrayAdapter&lt;ToDoItem&gt; ** klass. Den här underklass producerar en vy för varje **ToDoItem** med hello **row_list_to_do** layout.  I vår kod vi definiera hello efter klass som är en utökning av hello **ArrayAdapter&lt;E&gt; ** klass:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Åsidosätt hello kort **getView** metod. Exempel:

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

Vi skapa en instans av den här klassen i vår aktiviteten enligt följande:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

hello andra parameter toohello ToDoItemAdapter konstruktorn är en referens toohello layout. Vi kan nu skapa en instans av hello **ListView** och tilldela hello kortet toohello **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Använd hello kortet tooBind toohello UI

Du är nu redo toouse databindning. hello följande kod visar hur tooget objekt i hello tabell och fyllning hello lokala kortet med hello returnerade objekt.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

Anropa hello kortet när du ändrar hello **ToDoItem** tabell. Eftersom ändringar görs på en post med basis, kan du hantera en enskild rad i stället för en samling. När du infogar ett objekt kan anropa hello **lägga till** metod på hello kortet; när du tar bort, anropa hello **ta bort** metod.

Du hittar ett komplett exempel i hello [Android Snabbstartsprojekt][21].

## <a name="inserting"></a>Infoga data i hello backend

Skapa en instans av en instans av hello *ToDoItem* klassen och ange dess egenskaper.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Använd sedan **insert()** tooinsert ett objekt:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

hello returnerade entitet matchar hello data infogas i hello backend tabell, inkluderade hello-ID och andra värden (till exempel hello `createdAt`, `updatedAt`, och `version` fält) inställd på hello serverdel.

Mobile Apps tabeller kräver en primärnyckelkolumn med namnet **id**. Den här kolumnen måste vara en sträng. hello standardvärdet hello ID-kolumnen är ett GUID.  Du kan ange andra unika värden, till exempel e-postadresser eller användarnamn. När ett sträng-ID-värde har angetts för en infogad post, genererar hello backend en ny GUID.

Strängvärden ID innehåller hello följande fördelar:

* ID: N kan skapas utan att göra en onödig kommunikation toohello databas.
* Posterna är enklare toomerge från olika tabeller eller databaser.
* ID-värden integrera bättre med en programlogik.

Sträng-ID-värden är **REQUIRED** för offlinesynkronisering support.  Du kan inte ändra ett Id när den är lagrad i hello backend-databas.

## <a name="updating"></a>Uppdatera data i en mobil app

tooupdate data i en tabell, skicka hello nya objekt toohello **update()** metod.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

I det här exemplet *objektet* är en referens tooa rad i hello *ToDoItem* tabellen som har haft tooit vissa ändringar som gjorts.  hello rad med hello samma **id** uppdateras.

## <a name="deleting"></a>Ta bort data i en mobil app

hello följande kod visar hur toodelete data från en tabell genom att ange hello-dataobjektet.

```java
mToDoTable
    .delete(item);
```

Du kan också ta bort ett objekt genom att ange hello **id** i hello raden toodelete.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Leta upp ett specifikt objekt-ID: t

Leta upp ett objekt med en specifik **id** med hello **LETAUPP()** metod:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Så här: arbeta med någon data

hello ej typbestämd programmeringsmodell ger exakt kontroll över JSON-serialisering.  Det finns några vanliga scenarier där du toouse en ej typbestämd programmeringsmodell. Till exempel om backend-tabellen innehåller många kolumner och behöver du bara tooreference en delmängd av hello kolumner.  hello skrivna model kräver toodefine alla hello kolumner har definierats i hello Mobile Apps-serverdel i dataklass.  De flesta hello API-anrop för att få åtkomst till data är liknande toohello skrev API-anrop. hello största skillnaden är att i hello någon modell du anropa metoder i hello **MobileServiceJsonTable** -objektet, hello **MobileServiceTable** objekt.

### <a name="json_instance"></a>Skapa en instans av en ej typbestämd tabell

Liknande toohello skrev modellen, du börja med att hämta en tabellreferens, men i det här fallet är det en **MobileServicesJsonTable** objekt. Hämta hello referens genom att anropa hello **getTable** metod i en instans av hello klienten:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

När du har skapat en instans av hello **MobileServiceJsonTable**, den har praktiskt taget hello samma API som är tillgängliga som med hello skrivna programmeringsmodell. I vissa fall kan ta en utan angiven parameter i stället för en typifierad parameter i hello metoder.

### <a name="json_insert"></a>Infoga i en ej typbestämd tabell
Hej följande kod visar hur toodo insert. hello första steget är toocreate en [JsonObject][1], vilket är en del av hello [gson] [ 3] bibliotek.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Använd sedan **insert()** tooinsert hello ej typbestämd objekt i hello-tabellen.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Om du behöver tooget hello-ID för hello infogat objekt kan använda hello **getAsJsonPrimitive()** metod.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Ta bort från en ej typbestämd tabell
hello följande kod visar hur toodelete en instans, i det här fallet hello samma instans av en **JsonObject** som har skapats i hello före *infoga* exempel. hello koden är hello samma sätt som om hello skrivit skiftläge, men hello-metoden har en annan signatur eftersom den refererar till en **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

Du kan också ta bort en instans direkt med ID:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Returnera alla rader från en ej typbestämd tabell
Hej följande kod visar hur tooretrieve hela tabellen. Eftersom du använder en JSON-tabellen kan hämta du selektivt endast en del av hello tabellkolumner.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

hello samma uppsättning filtrering, filtrering och växling metoder som är tillgängliga för hello skrivna modellen är tillgängliga för hello någon modell.

## <a name="offline-sync"></a>Implementera offlinesynkronisering

hello Azure Mobile Apps klient-SDK implementerar också offlinesynkronisering av data med hjälp av en SQLite-databas toostore en kopia av data från hello lokalt.  Åtgärder som utförs på en offline-tabell kräver inte mobil anslutning toowork.  Offlinesynkronisering aids återhämtning och prestanda på bekostnad av hello av mer komplex logik för konfliktlösning.  hello Azure Mobile Apps klient-SDK implementerar hello följande funktioner:

* Inkrementell synkronisering: Endast uppdaterade och nya poster har hämtats, spara bandbredd och minne.
* Optimistisk samtidighet: Operations antas toosucceed.  Konfliktlösning skjuts upp tills uppdateringar utförs på hello-servern.
* Konfliktlösning: hello SDK identifierar när en motstridig ändring har gjorts på hello server och ger skapar tooalert hello användare.
* Mjuk borttagning: Borttagna poster är markerade tagits bort, så att andra enheter tooupdate sina offline cache.

### <a name="initialize-offline-sync"></a>Initiera synkronisering Offline

Varje tabell som är offline måste definieras i hello offline cachen innan de används.  Normalt görs tabelldefinitionen omedelbart efter hello generering av hello klienten:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with hello model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define hello table in hello local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize hello local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-toohello-offline-cache-table"></a>Hämta en referens toohello Offline cachelagrad tabell

Ett online tabell kan du använda `.getTable()`.  För en offline-tabell, använder du `.getSyncTable()`:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

Alla hello metoder som är tillgängliga för online-tabeller (inklusive filtrering, sortering, växling, infoga data, uppdatera data och ta bort data) fungerar lika bra på tabellerna online och offline.

### <a name="synchronize-hello-local-offline-cache"></a>Synkronisera hello Offline lokalt cacheminne

Synkronisering är inom hello kontroll över din app.  Här är ett exempel synkroniseringsmetoden:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

Om det finns ett namn på frågan toohello `.pull(query, queryname)` metod och inkrementell synkronisering är används tooreturn endast poster som har skapats eller ändrats sedan pull hello som senast har slutförts.

### <a name="handle-conflicts-during-offline-synchronization"></a>Hantera konflikter under offlinesynkronisering

Om en konflikt uppstår under en `.push()` åtgärd, en `MobileServiceConflictException` genereras.   Hej server-utfärdade objektet är inbäddat i hello undantag och kan hämtas av `.getItem()` på hello undantag.  Justera hello push genom att anropa hello följande objekt på hello MobileServiceSyncContext objekt:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

När alla konflikter markeras som du vill, anropa `.push()` igen tooresolve alla hello konflikter.

## <a name="custom-api"></a>Anropa anpassade API

En anpassad API kan du toodefine anpassade slutpunkter som exponerar serverfunktioner som inte mappa tooan infoga, uppdatera, ta bort eller Läsåtgärd. Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.

I en Android-klient du anropa hello **invokeApi** metod-toocall hello anpassade API-slutpunkt. hello följande exempel visas hur toocall en API-slutpunkt med namnet **completeAll**, som returnerar en samlingsklass som heter **MarkAllResult**.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

Hej **invokeApi** metoden anropas för hello-klient som skickar en POST-begäran toohello nya anpassade API. hello resultatet som returneras av hello anpassade API visas i en dialogruta för meddelande som fel. Andra versioner av **invokeApi** kan du om du vill skicka ett objekt i hello begärantext, ange hello HTTP-metod och skicka frågeparametrar Hej förfrågan. Utan angiven versioner av **invokeApi** tillhandahålls också.

## <a name="authentication"></a>Lägg till autentisering tooyour app

Självstudiekurser redan beskrivs i detalj hur tooadd dessa funktioner.

Har stöd för Apptjänst [autentisera användarna](app-service-mobile-android-get-started-users.md) med olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory. Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare. Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i din serverdel.

Två autentisering flöden stöds: en **server** flöde och en **klienten** flöde. hello server flödet ger hello enklaste autentiseringsupplevelse som använder webbgränssnitt för hello identitets-providers.  Inga ytterligare SDK: er krävs tooimplement flödet för serverautentisering. Flöde för serverautentisering ger inte djupgående integrering i hello mobil enhet och rekommenderas endast för bevis på koncept scenarier.

hello flödet tillåter djupare integrering med specifika funktioner, till exempel enkel inloggning som använder SDK: er som tillhandahålls av hello identitetsleverantör.  Exempelvis kan du integrera hello Facebook SDK i din mobila program.  hello mobila klienten växlingar i hello Facebook-app och bekräftar din inloggning innan du växlar tillbaka tooyour mobila appar.

Fyra steg är nödvändiga tooenable autentisering i appen:

* Registrera din app för autentisering med en identitetsleverantör.
* Konfigurera din App Service-serverdel.
* Begränsa tabellen behörighet tooauthenticated användarna bara på hello Apptjänst backend.
* Lägg till autentisering kod tooyour app.

Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare. Du kan också använda hello SID för en autentiserad användare toomodify begäranden.  Mer information hittar [komma igång med autentisering] och hello servern ta för SDK-dokumentationen.

### <a name="caching"></a>: Server Autentiseringsflödet

hello startar följande kod en server flödet inloggningen med hello Google-providern.  Ytterligare konfiguration krävs på grund av hello säkerhetskrav för hello Google-providern:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Dessutom lägga till hello följa metoden toohello huvudsakliga Activity-klassen:

```java
// You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

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
```

Hej `GOOGLE_LOGIN_REQUEST_CODE` definierats i din huvudsakliga aktivitet används för hello `login()` metod och inom hello `onActivityResult()` metod.  Du kan välja alla unikt nummer som hello samma nummer används i hello `login()` metod och hello `onActivityResult()` metod.  Om du abstrakt hello klientkod till ett service-kort (som visas tidigare) bör du anropa hello lämpliga metoder på hello service nätverkskortet.

Du måste också tooconfigure hello projekt för customtabs.  Ange en omdirigerings-URL.  Lägg till följande fragment för hello`AndroidManifest.xml`:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Lägg till hello **redirectUriScheme** toohello `build.gradle` filen för tillämpningsprogrammet:

```text
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
```

Slutligen lägger du till `com.android.support:customtabs:23.0.1` toohello programberoenden i hello `build.gradle` fil:

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.2.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Hämta hello-ID för hello inloggade användare från en **MobileServiceUser** med hello **getUserId** metod. Ett exempel på hur toouse Futures toocall hello asynkron inloggningen API: er, se [komma igång med autentisering].

> [!WARNING]
> hello URL-schema som nämns är skiftlägeskänslig.  Se till att alla förekomster av `{url_scheme_of_you_app}` gemener/versaler.

### <a name="caching"></a>Cache-autentiseringstoken

Cachelagring autentiseringstoken måste du toostore hello användar-ID och autentiseringstoken lokalt på hello enhet. hello hello appen startas nästa gång du kontrollera hello cache och om dessa värden finns kan du hoppa över hello logg i proceduren och rehydrate hello klienten med dessa data. Men dessa data är känsligt och lagras krypterad för säkerhet om hello phone hämtar blir stulen.  Du kan se en komplett exempel på hur toocache autentisering token i [cachelagra autentisering tokenavsnittet][7].

När du försöker toouse en token som har upphört att gälla, får du en *401-Ej behörig* svar. Du kan hantera med hjälp av filter-autentiseringsfel.  Filter hantera begäranden toohello Apptjänst backend. hello filter code testar hello-svar för en 401, utlöser hello inloggningsprocessen och återupptar sedan hello-begäran som genererade hello 401.

### <a name="refresh"></a>Använd Uppdateringstoken

hello-token som returnerades av Azure App Service-autentisering och auktorisering har en definierad livslängden för en timme.  Du måste autentiseras hello användare för denna period.  Om du hello använder en långlivade token som du har fått med klient-flöde för autentisering och du kan autentiseras med Azure App Service-autentisering och auktorisering med hjälp av samma token.  En annan Azure App Service-token genereras med nya livslängden.

Du kan också registrera hello providern toouse uppdatera token.  En uppdatera Token är inte alltid tillgängligt.  Det krävs ytterligare konfiguration:

* För **Azure Active Directory**, konfigurera en klienthemlighet för hello Azure Active Directory-App.  Ange hello klienthemlighet i hello Azure App Service när du konfigurerar Azure Active Directory-autentisering.  När du anropar `.login()`, skicka `response_type=code id_token` som en parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* För **Google**, skicka hello `access_type=offline` som en parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* För **Account**väljer hello `wl.offline_access` omfång.

toorefresh en token ring `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

Ett bra tips är att skapa ett filter som identifierar ett 401 svar från hello server och försöker toorefresh hello användar-token.

## <a name="log-in-with-client-flow-authentication"></a>Logga in med klient-flöde för autentisering

hello allmänna processen för att logga in med klient-flöde för autentisering är följande:

* Konfigurera Azure App Service-autentisering och auktorisering som server-flöde för autentisering.
* Integrera hello autentiseringsprovider SDK för autentisering tooproduce en åtkomst-token.
* Anropa hello `.login()` metoden på följande sätt:

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Ersätt hello `onSuccess()` metod med oavsett kod som du vill att toouse på en genomförd inloggning.  Hej `{provider}` strängen är en giltig provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, eller **twitter**.  Om du har implementerat anpassad autentisering, kan du också använda hello anpassad autentisering providern taggen.

### <a name="adal"></a>Autentisera användare med hello Active Directory Authentication Library (ADAL)

Du kan använda hello Active Directory Authentication Library (ADAL) toosign användare i ditt program med Azure Active Directory. Med hjälp av en klient flödet för inloggning är ofta bättre toousing hello `loginAsync()` metoder som ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.

1. Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] [ 22] kursen. Se till att toocomplete hello valfritt steg för att registrera en native client-program.
2. Installera ADAL genom att ändra din build.gradle filen tooinclude hello följande definitioner:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. Lägg till hello följande kod tooyour program, vilket gör följande ersättningar hello:

* Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program. hello format bör vara https://login.microsoftonline.com/contoso.onmicrosoft.com.
* Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel. Du kan hämta hello klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.
* Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.
* Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema. Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="filters"></a>Justera hello klient-/ serverkommunikation

hello klientanslutning är vanligtvis en grundläggande HTTP-anslutning med hello underliggande HTTP-biblioteket som medföljer hello Android SDK.  Det finns flera orsaker till varför du vill ha toochange som:

* Vill du toouse en alternativ tooadjust tidsgränser för HTTP-biblioteket.
* Vill du tooprovide en förloppsindikator.
* Vill du tooadd en anpassad rubrik toosupport API management-funktioner.
* Vill du toointercept misslyckade svar så att du kan implementera omautentisering.
* Vill du toolog backend begäranden tooan analytics-tjänsten.

### <a name="using-an-alternate-http-library"></a>Med hjälp av en annan HTTP-bibliotek

Anropa hello `.setAndroidHttpClientFactory()` metoden omedelbart när du har skapat din klient-referens.  Till exempel tooset hello anslutning timeout too60 sekunder (i stället för hello standard 10 sekunder):

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>Implementera ett Filter för pågår

Du kan implementera en skärningspunkt för varje begäran genom att implementera en `ServiceFilter`.  Hello följande uppdateringar till exempel en förskapad förloppsindikator:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    pubic void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

Du kan koppla filter toohello klienten på följande sätt:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>Anpassa huvuden för begäran

Använder följande hello `ServiceFilter` och bifoga hello filter i hello samma sätt som hello `ProgressFilter`:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="conversions"></a>Konfigurera automatisk serialisering

Du kan ange en konvertering strategi som gäller tooevery kolumn med hjälp av hello [gson] [ 3] API. hello Android klientbiblioteket använder [gson] [ 3] bakgrunden hello tooserialize Java objekt tooJSON data innan hello data skickas tooAzure Apptjänst.  hello följande kod använder hello **setFieldNamingStrategy()** metoden tooset hello strategi. Det här exemplet tar bort hello första tecken (”m”) och sedan gemena hello nästa tecken för varje fältnamn. Till exempel skulle den göra ”mId” till ”id”.  Implementera en konvertering strategi tooreduce hello behöver för `SerializedName()` anteckningar i de flesta fält.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStategy)
);
```

Den här koden måste köras innan du skapar en mobil klient-referens med hello **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Komma igång med autentisering]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
