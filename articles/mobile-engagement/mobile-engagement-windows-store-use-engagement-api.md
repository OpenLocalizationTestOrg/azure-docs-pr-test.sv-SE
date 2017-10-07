---
title: "aaaHow tooUse hello Engagement API på Windows Universal"
description: "Hur tooUse hello Engagement API på Windows Universal"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0256b839c28e4ef6c530106408d744038fa711ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-universal"></a>Hur tooUse hello Engagement API på Windows Universal
Det här dokumentet är tillägg toohello [hur tooIntegrate Engagement för universella Windows-](mobile-engagement-windows-store-integrate-engagement.md): ger i djup information om hur hello toouse Engagement API tooreport programstatistik.

Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information sedan hello enklaste sättet är toomake alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klass.

Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementPage` klasser måste toouse hello Engagement API.

Hej Engagement API tillhandahålls av hello `EngagementAgent` klass. Du kan komma åt toothose metoder via `EngagementAgent.Instance`.

Även om hello agent modulen inte har initierats, varje anrop toohello API skjuts upp och köras igen när hello agent är tillgänglig.

## <a name="engagement-concepts"></a>Koncept i engagement
hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello universella Windows-plattformen.

### <a name="session-and-activity"></a>`Session` och `Activity`
En *aktiviteten* är ofta kopplade till en sida på hello-program som är toosay hello *aktiviteten* när hello sidan visas och stoppas när hello sidan stängs: hello fall när hello Engagement SDK är integrerad med hjälp av hello `EngagementPage` klass.

Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API. Detta gör att du toosplit som en viss sida i flera sub delar tooget mer information om hello användning av den här sidan (till exempel hur ofta tooknow och hur länge dialogrutor används i den här sidan).

## <a name="reporting-activities"></a>Rapportering
### <a name="user-starts-a-new-activity"></a>Användaren startar en ny aktivitet
#### <a name="reference"></a>Referens
            void StartActivity(string name, Dictionary<object, object> extras = null)

Du behöver toocall `StartActivity()` varje gång hello användaraktivitet ändras. hello första anropet toothis funktionen startar en ny session.

> [!IMPORTANT]
> hello SDK anropar automatiskt hello EndActivity metod när hello programmet stängs. Därför rekommenderas toocall hello StartActivity metod när hello aktiviteten för hello användare ändrar och tooNEVER anrop hello EndActivity metod, eftersom den här metoden anropas tvingar hello aktuell session toobe avslutades.
> 
> 

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Användaren avslutar sin aktuella aktiviteten
#### <a name="reference"></a>Referens
            void EndActivity()

Detta avslutar hello aktivitet och hello-sessionen. Du bör inte anropa den här metoden om du inte verkligen vet vad du gör.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Rapportering av jobb
### <a name="start-a-job"></a>Starta ett jobb
#### <a name="reference"></a>Referens
            void StartJob(string name, Dictionary<object, object> extras = null)

Du kan använda hello jobbet tootrack vissa aktiviteter under en viss tidsperiod.

#### <a name="example"></a>Exempel
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Avsluta ett jobb
#### <a name="reference"></a>Referens
            void EndJob(string name)

Som en aktivitet som spåras av ett jobb har avslutats, bör du anropa hello EndJob metod för det här jobbet genom att ange hello jobbnamn.

#### <a name="example"></a>Exempel
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Rapporteringshändelser
Det finns tre typer av händelser:

* Fristående händelser
* Sessionshändelser
* Jobbhändelser

### <a name="standalone-events"></a>Fristående händelser
#### <a name="reference"></a>Referens
            void SendEvent(string name, Dictionary<object, object> extras = null)

Fristående händelser kan inträffa utanför hello kontexten för en session.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sessionshändelser
#### <a name="reference"></a>Referens
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.

#### <a name="example"></a>Exempel
**Utan data:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Med data:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Jobbhändelser
#### <a name="reference"></a>Referens
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Jobbhändelser är oftast används tooreport hello åtgärder som utförs av en användare under ett jobb.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Rapporterat fel
Det finns tre typer av fel:

* Fristående fel
* Sessionen fel
* Jobbfel

### <a name="standalone-errors"></a>Fristående fel
#### <a name="reference"></a>Referens
            void SendError(string name, Dictionary<object, object> extras = null)

Motstridiga toosession fel, fristående kan det uppstå fel utanför hello kontexten för en session.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sessionen fel
#### <a name="reference"></a>Referens
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Jobbfel
#### <a name="reference"></a>Referens
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.

#### <a name="example"></a>Exempel
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Rapportering krascher
hello agent ger två metoder toodeal kraschar.

### <a name="send-an-exception"></a>Skicka ett undantag
#### <a name="reference"></a>Referens
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Exempel
Du kan skicka ett undantag när som helst genom att anropa:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Du kan också använda en valfri parameter tooterminate hello engagement session på hello samtidigt än att skicka hello krascher. toodo anropa så:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Om du gör det stängs jobb och hello sessionen när du skickar hello krascher.

### <a name="send-an-unhandled-exception"></a>Skicka ett ohanterat undantag
#### <a name="reference"></a>Referens
            void SendCrash(Exception e)

Engagement innehåller också en metod toosend ohanterat undantag om du har **inaktiverad** Engagement automatisk **krasch** reporting. Detta är särskilt användbar när den används i händelsehanteraren för hello program UnhandledException.

Den här metoden kommer **alltid** avslutas hello engagement sessionen och jobb efter anropas.

#### <a name="example"></a>Exempel
Du kan använda den tooimplement egna UnhandledExceptionEventArgs hanterare. Lägg exempelvis till hello `Current_UnhandledException` metod för hello `App.xaml.cs` fil:

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

Lägg till i App.xaml.cs i ”gemensamma App() {}”:

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a>Enhets-Id
            String EngagementAgent.Instance.GetDeviceId()

Du kan hämta hello engagement enhets-id genom att anropa den här metoden.

## <a name="extras-parameters"></a>Tillägg parametrar
Godtycklig data kan vara anslutna tooan händelse, ett fel, en aktivitet eller ett jobb. Dessa data strukturerade med hjälp av en ordlista. Nycklar och värden som kan vara av valfri typ.

Extra data serialiseras så om du vill tooinsert din egen typ i tillägg du tooadd ett datakontrakt för den här typen.

### <a name="example"></a>Exempel
Vi skapar en ny klass ”Person”.

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Sedan lägger vi till en `Person` instans tooan extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Om du placerar andra typer av objekt, kontrollera att deras toString ()-metoden är implementerad tooreturn en mänsklig läsbar sträng.
> 
> 

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Tillägg är begränsad för**1024** tecken per anrop.

## <a name="reporting-application-information"></a>Rapportering programinformation
### <a name="reference"></a>Referens
            void SendAppInfo(Dictionary<object, object> appInfos)

Du kan manuellt rapportera spåra information (eller andra program specifik information) med hello SendAppInfo()-funktionen.

Observera att dessa data kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet. Använda en ordlista som händelsen tillägg\<objekt, objektet\> tooattach data.

### <a name="example"></a>Exempel
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Information om programmet är begränsad för**1024** tecken per anrop.

I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a>Loggning
### <a name="enable-logging"></a>Aktivera loggning
hello SDK kan vara konfigurerade tooproduce test loggar i hello IDE-konsolen.
Dessa loggar är inte aktiverade som standard. toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

