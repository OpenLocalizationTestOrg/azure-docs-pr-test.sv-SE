---
title: aaaInput validering - hotet Modeling verktyget - Azure | Microsoft Docs
description: "ändringar för hot som exponeras i hello hot Modeling verktyget"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 823503881f4bae292ef021834d5e64acf2a0f54a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-input-validation--mitigations"></a>Säkerhet ram: Indata validering | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Webbprogram** | <ul><li>[Inaktivera XSLT-skript för alla transformeringar med obetrodda formatmallar](#disable-xslt)</li><li>[Se till att varje sida som kan innehålla användare kan kontrolleras innehåll bort automatisk MIME-kontroll](#out-sniffing)</li><li>[Skydda eller inaktivera XML-entitet upplösning](#xml-resolution)</li><li>[Program som använder http.sys utföra verifiering för URL-auktorisering](#app-verification)</li><li>[Se till att lämpliga kontroller som finns på plats när accepterar filer från användare](#controls-users)</li><li>[Se till att typen säkert parametrar används i webbprogram för dataåtkomst](#typesafe)</li><li>[Använda separat modell bindning klasser eller bindningsfilter listar tooprevent MVC masslagring tilldelning säkerhetsproblem](#binding-mvc)</li><li>[Koda obetrodda web utdata tidigare toorendering](#rendering)</li><li>[Utföra verifiering av indata- och filtrering på alla strängtyp modellegenskaper](#typemodel)</li><li>[Rensning ska tillämpas på formulärfält som accepterar tecken, t.ex., textredigeraren](#richtext)</li><li>[Tilldela inte DOM-element toosinks som inte har inbyggda kodning](#inbuilt-encode)</li><li>[Verifiera att alla omdirigeringar i programmet hello är stängda eller göra på ett säkert sätt](#redirect-safe)</li><li>[Implementera verifiering av indata på alla typen strängparametrar accepteras av kontrollantmetoder](#string-method)</li><li>[Ange övre gräns för tidsgränsen för reguljärt uttryck som bearbetar tooprevent DoS på grund av toobad reguljära uttryck](#dos-expression)</li><li>[Undvik att använda Html.Raw i Razor vyer](#html-razor)</li></ul> | 
| **Databas** | <ul><li>[Använd inte dynamisk frågor i lagrade procedurer](#stored-proc)</li></ul> |
| **Webb-API** | <ul><li>[Se till att modellen verifieringen är klar för Web API-metoder](#validation-api)</li><li>[Implementera verifiering av indata på alla typen strängparametrar accepteras av Web API-metoder](#string-api)</li><li>[Se till att typen säkert parametrar används i Web API för dataåtkomst](#typesafe-api)</li></ul> | 
| **Azure dokumentet DB** | <ul><li>[Använd innehåller parametrar SQL-frågor för DocumentDB](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[WCF-indata verifiering via schemabindning](#schema-binding)</li><li>[WCF - indata verifiering via parametern kontrollanter](#parameters)</li></ul> |

## <a id="disable-xslt"></a>Inaktivera XSLT-skript för alla transformeringar med obetrodda formatmallar

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [XSLT-säkerhet](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [egenskapen XsltSettings.EnableScript](http://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Steg** | XSLT har stöd för skript i formatmallar med hello `<msxml:script>` element. Detta gör att anpassade funktioner toobe används i en XSLT-transformation. hello skriptet körs under hello kontext processens hello utför hello transformeringen. XSLT-skriptet måste inaktiveras när i en ej betrodd miljö tooprevent körning av icke betrodd kod. *Om du använder .NET:* XSLT-skript är inaktiverat som standard, men du måste se till att den inte har uttryckligen aktiverats via hello `XsltSettings.EnableScript` egenskapen.|

### <a name="example"></a>Exempel 

```C#
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Exempel
Om du använder med hjälp av MSXML 6.0, är XSLT-skript inaktiverat som standard. dock måste du kontrollera att den inte har uttryckligen aktiverats via hello XML DOM-objektegenskaper AllowXsltScript. 

```C#
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Exempel
Om du använder MSXML 5 eller nedan, XSLT-skript är aktiverat som standard och du måste uttryckligen inaktivera den. Ange hello XML DOM-objekt egenskapen AllowXsltScript toofalse. 

```C#
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting toofalse disables XSLT scripting.
```

## <a id="out-sniffing"></a>Se till att varje sida som kan innehålla användare kan kontrolleras innehåll bort automatisk MIME-kontroll

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [IE8 Säkerhet del V - heltäckande skydd](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Steg** | <p>För varje sida som kan innehålla användare kan kontrolleras innehåll, måste du använda hello HTTP-huvudet `X-Content-Type-Options:nosniff`. toocomply med detta krav kan du antingen ange hello obligatorisk rubrik varje sida för de sidor som kan innehålla användare kan kontrolleras innehåll eller kan du ändra det globalt för alla sidor i hello program.</p><p>Varje typ av fil från en server som har en associerad [MIME-typ](http://en.wikipedia.org/wiki/Mime_type) (kallas även en *innehållstypen*) som beskriver hello uppbyggnad hello innehåll (det vill säga image, text, program osv.)</p><p>hello X-innehåll-typ-alternativ rubrik är ett HTTP-huvud som gör att utvecklare toospecify att innehållet inte får någon MIME-lyssnar. Det här sidhuvudet är utformad toomitigate MIME-kontroll attacker. Stöd för det här huvudet har lagts till i Internet Explorer 8 (IE8)</p><p>Endast användare av Internet Explorer 8 (IE8) drar nytta av X-innehåll-typ-alternativ. Tidigare versioner av Internet Explorer hänsyn inte för närvarande hello X-innehåll-typ-alternativ rubrik</p><p>Internet Explorer 8 (och senare) är hello endast större webbläsare tooimplement en MIME--kontroll CEIP funktionen. Om andra större webbläsare (Firefox, Safari, Chrome) implementerar liknande funktioner, och när den här rekommendationen kommer att vara uppdaterade tooinclude syntaxen för de här webbläsarna samt</p>|

### <a name="example"></a>Exempel
tooenable hello obligatorisk rubrik globalt för alla sidor i hello program, du kan göra något av följande hello: 

* Lägg till hello sidhuvud i hello Web.config om programmet hello finns av Internet Information Services (IIS) 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Lägg till sidhuvud hello via hello globala programmet\_BeginRequest 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Implementera anpassade HTTP-modul 

``` 
public class XContentTypeOptionsModule : IHttpModule 
  {
    #region IHttpModule Members 
    public void Dispose() 
    { 

    } 
    public void Init(HttpApplication context)
    { 
      context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders); 
    } 
    #endregion 
    void context_PreSendRequestHeaders(object sender, EventArgs e) 
      { 
        HttpApplication application = sender as HttpApplication; 
        if (application == null) 
          return; 
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* Du kan aktivera hello obligatorisk rubrik endast för specifika sidor genom att lägga till den tooindividual svar: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Skydda eller inaktivera XML-entitet upplösning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [XML-entitet Expansion](http://capec.mitre.org/data/definitions/197.html), [XML Denial of Service-attacker och försvar](http://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML Säkerhetsöversikt](http://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [bästa praxis för att skydda MSXML kod](http://msdn.microsoft.com/library/ms759188(VS.85).aspx), [ Protokollreferens för NSXMLParserDelegate](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [lösa externa referenser](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Steg**| <p>Även om den inte används ofta, finns det en funktion i XML som tillåter hello XML-parsern tooexpand makrot entiteter med värden som definieras i själva hello dokumentet eller från externa källor. Hello dokument kan till exempel definiera en entitet ”företagsnamn” med hello värdet ”Microsoft”, så att varje gång hello text ”&companyname;” visas i hello dokumentet ersätts automatiskt med hello text Microsoft. Eller hello dokument kan definiera en entitet ”MSFTStock” som refererar till en extern webbplats service toofetch hello aktuella värdet för Microsoft-lager.</p><p>Sedan helst ”&MSFTStock;” visas i hello dokumentet ersätts automatiskt med hello aktuella lager priset. Den här funktionen kan dock vara missbrukat toocreate DOS-service (DoS) villkor. En angripare kan kapsla flera entiteter toocreate en exponentiell expansion XML bomb som förbrukar allt tillgängligt minne på hello system. </p><p>Du kan också han kan skapa en extern referens som strömmas tillbaka en oändlig mängd data eller som bara låser sig hello tråd. Alla team måste därför inaktivera interna eller externa XML-entitet upplösning helt om sina program inte använda den, eller begränsa manuellt hello mängden minne och den tidpunkt då hello program kan använda för matchning av entiteten om den här funktionen är absolut nödvändigt. Om entiteten upplösning inte krävs av programmet, inaktiverar du den. </p>|

### <a name="example"></a>Exempel
Du kan använda hello följande metoder för .NET Framework-kod:

```C#
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
Observera att hello standardvärdet `ProhibitDtd` i `XmlReaderSettings` är true, men i `XmlTextReader` det är false. Om du använder XmlReaderSettings du behöver inte tooset ProhibitDtd tootrue explicit, men det rekommenderas för saké säkerhet att göra. Observera också att hello XmlDocument klass gör entitet upplösning som standard. 

### <a name="example"></a>Exempel
toodisable entitet lösning för XmlDocuments, Använd hello `XmlDocument.Load(XmlReader)` överlagring för hello läsa in metod och ange hello lämpliga egenskaper i hello XmlReader argumentet toodisable upplösning, enligt beskrivningen i följande kod hello: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Exempel
Om det inte går att inaktivera entiteten matchning för ditt program, ange hello XmlReaderSettings.MaxCharactersFromEntities tooa rimliga egenskapsvärdet enligt tooyour programbehov. Detta begränsar hello effekten av potentiella exponentiell expansion DoS-attacker. hello följande kod innehåller ett exempel på den här metoden: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Exempel
Om du behöver tooresolve infogade entiteter men tooresolve externa enheter behöver inte ange hello XmlReaderSettings.XmlResolver egenskapen toonull. Exempel: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Observera att i MSXML6, ProhibitDTD är standardinställningen tootrue (inaktivera DTD-bearbetning). Det finns två XML-Parser kan du använda för Apple OSX/iOS kod: NSXMLParser och libXML2. 

## <a id="app-verification"></a>Program som använder http.sys utföra verifiering för URL-auktorisering

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Alla program som använder http.sys bör följa dessa riktlinjer:</p><ul><li>Begränsa hello URL-längd toono fler än 16 384 tecken (ASCII eller Unicode). Detta är hello absolut URL maxlängd baserat på hello standardinställningen för Internet Information Services (IIS) 6. Webbplatser bör sträva efter en kortare än detta om möjligt</li><li>Använd hello standard .NET Framework-filen i/o-klasser (till exempel FileStream) som dessa drar nytta av hello regler för auktorisering i hello .NET FX</li><li>Explicit Bygg en Tillåt-lista över kända filnamn</li><li>Explicit avvisa kända filtyper som du inte kommer att uppfylla UrlScan avvisar: exe, bat, cmd, com, htw, ida, idq, htr, idc, shtm-[l], stm, skrivare, ini, pol, dat filer</li><li>Fånga hello följande undantag:<ul><li>System.ArgumentException (för enhetsnamn)</li><li>System.NotSupportedException (för dataströmmar)</li><li>System.IO.FileNotFoundException (för ogiltig ESC filnamn)</li><li>System.IO.DirectoryNotFoundException (för ogiltig ESC kataloger)</li></ul></li><li>*Inte* anropa ut tooWin32 fil I/O APIs. På en ogiltig URL att returnera en 400-fel toohello användare och logga hello verkliga fel.</li></ul>|

## <a id="controls-users"></a>Se till att lämpliga kontroller som finns på plats när accepterar filer från användare

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Obegränsad filöverföringen](https://www.owasp.org/index.php/Unrestricted_File_Upload), [filen signatur tabell](http://www.garykessler.net/library/file_sigs.html) |
| **Steg** | <p>Överförda filer utgör en betydande risk tooapplications.</p><p>hello första steget i många attacker är tooget vissa kod toohello system toobe angrepp. Hello attack endast måste toofind en sätt tooget hello kod körs. Om du använder en filöverföring kan hello angripare utföra hello första steget. hello konsekvenserna av obegränsad filöverföringen kan variera, inklusive hela systemet övertag en överlagrad filsystem eller databasen, vidarebefordrar attacker tooback slutpunkt system och enkel defacement.</p><p>Det beror på vilken hello-programmet har med hello upp filen och särskilt där den lagras. Servern validering av filöverföringar saknas. Följande kontroller för informationssäkerhet bör implementeras för filöverföring funktioner:</p><ul><li>Kontrollen av tillägget (endast en giltig uppsättning tillåtna filtyp ska accepteras)</li><li>Maxstorleken</li><li>Filen får inte vara överförda toowebroot; hello måste vara en katalog på icke-systemenhet</li><li>Namnkonventionen måste följas så att hello överförda filnamnet har vissa slumpmässighet så som tooprevent filen skrivs över</li><li>Filer bör genomsökas efter ett virusskyddsprogram innan du skriver toohello disk</li><li>Se till att verifieras hello filnamnet och andra metadata (t.ex. sökväg) för skadliga tecken</li><li>Filens format signatur ska kontrolleras tooprevent en användare från att överföra en masqueraded fil (t.ex. ladda upp ett exe-filen genom att ändra tillägget tootxt)</li></ul>| 

### <a name="example"></a>Exempel
Hello senaste avseende signaturverifiering format för filen, finns i klassen toohello nedan för mer information: 

```C#
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a id="typesafe"></a>Se till att typen säkert parametrar används i webbprogram för dataåtkomst

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Om du använder hello Parametersamlingen är SQL hanterar hello indata som ett litteralvärde stället som körbar kod. hello Parametersamlingen kan vara används tooenforce typ och längd begränsningar på indata. Värden som ligger utanför intervallet för hello utlöser ett undantag. Om typen säkert SQL-parametrar används kanske angripare kan tooexecute injektion attacker som är inbäddade i hello ofiltrerad indata.</p><p>Använd säker typparametrar när konstruera SQL frågar tooavoid SQL injection attacker som kan uppstå med ofiltrerade indata. Du kan använda säker typparametrar med lagrade procedurer och dynamiska SQL-uttryck. Parametrarna behandlas som litterala värden av hello databasen och inte som körbar kod. Parametrar kontrolleras även för typ och längd.</p>|

### <a name="example"></a>Exempel 
hello följande kod visar hur toouse Skriv säker parametrar med hello SqlParameterCollection när du anropar en lagrad procedur. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
I föregående exempel hello, kan inte hello indatavärdet vara längre än 11 tecken. Om hello data inte följer toohello typ eller längd som anges av parametern hello genereras hello SqlParameter klass ett undantag. 

## <a id="binding-mvc"></a>Använda separat modell bindning klasser eller bindningsfilter listar tooprevent MVC masslagring tilldelning säkerhetsproblem

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Metadataattribut](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [offentliga nyckel säkerhet säkerhetsproblem och minskning](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [fullständig Guide tooMass tilldelning i ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [komma igång med EF med MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Steg** | <ul><li>**När ska leta för över bokföring säkerhetsproblem? -** Över bokföring säkerhetsproblem kan inträffa varje plats du binda modellen klasser från användaren. Ramverk som MVC kan representera användardata i anpassade .NET-klasser, inklusive vanlig gamla CLR-objekt (POCOs). MVC fyller automatiskt dessa klasser för modellen med data från hello begäran att tillhandahålla en lämplig representation för att hantera indata från användaren. När dessa klasser innehåller egenskaper som inte ska ställas in av hello användare, kan hello programmet vara sårbara tooover bokföring attacker som gör att kontrollera data som hello programmet aldrig avsedd. Som MVC modellbindning stöd databastekniker, till exempel Objektrelationer/mappers som Entity Framework ofta också för användning av POCO objekt toorepresent databasdata. Dessa data model klasser ange hello samma bekvämlighet behandlar databasdata som MVC med indata från användaren. Eftersom både MVC och hello databasen stöder liknande modeller som POCO objekt verkar enkelt tooreuse hello samma klasserna i båda fallen. Den här övningen misslyckas toopreserve avgränsning av säkerhetsskäl och är en gemensam område där oönskade egenskaper är exponerad toomodel bindning, aktivera över bokföring attacker.</li><li>**Varför bör inte använda min ofiltrerade databasklasser för modellen som parametrar toomy MVC åtgärder? -** Eftersom MVC modellbindning binda något i den här klassen. Även om hello data inte visas i din vy kan en angripare kan skicka en HTTP-begäran med dessa data ingår och MVC kommer gladeligen binda eftersom åtgärden säger att databasen klass är hello form av data ska accepteras för indata från användaren.</li><li>**Varför bör hand om hello form som används för modellbindning? -** Med ASP.NET MVC modellbindning med alltför många modeller exponerar ett program tooover bokföring attacker. Överdrivet bokföring möjliggör angripare toochange programdata utanför vilka hello avsedda som åsidosätter hello priset för ett objekt eller hello säkerhetsbehörighet för ett konto. Program ska använda åtgärden-specifik bindning modeller (eller specifika tillåtna filter egenskapslistor) tooprovide ett explicit kontrakt för vilka obetrodda inkommande tooallow via modellbindning.</li><li>**Har separat bindning modeller bara kopiera koden? -** Nej, det är en fråga om avgränsning av säkerhetsskäl. Om du återanvänder databasmodeller i åtgärdsmetoder säger du alla egenskapen (eller underordnade) i den klass som kan anges av hello användare i en HTTP-begäran. Om du inte vill MVC toodo behöver du en filterlista eller en separat klass formen tooshow MVC vilka data kan komma från användarindata i stället.</li><li>**Om jag har en separat bindning modeller för indata från användaren, finns tooduplicate alla mina data anteckningens attribut? -** Inte nödvändigtvis. Du kan använda MetadataTypeAttribute på hello databasen klassen toolink toohello modellmetadata på en modellklass för bindningen. Bara Observera att hello typen som refereras av hello MetadataTypeAttribute måste vara en delmängd av hello refererar till typen (det kan ha färre egenskaper, men inte mer).</li><li>**Det är tråkigt att flytta data fram och tillbaka mellan användaren inkommande modeller och databasen. Kan jag bara kopiera över alla egenskaper med hjälp av reflektion? -** Ja. hello är egenskaperna som visas i hello bindning modeller hello som du har konstaterat toobe säker för användaren. Det finns ingen säkerhet anledning som förhindrar med hjälp av reflektion toocopy över alla egenskaper som finns i vanliga mellan dessa två modeller.</li><li>**Vad händer om [binda (exkludera = ”â €...”)]. Kan jag använda som i stället för att separat bindning modeller? -** Rekommenderas inte den här metoden. Med hjälp av [binda (exkludera = ”â €...”)] innebär att alla nya egenskapen kan bindas till som standard. När en ny egenskap läggs en extra steg tooremember tookeep saker är säker, snarare än hello design vara säker som standard. Beroende på hello developer är kontrollerar den här listan varje gång en egenskap läggs riskfyllda.</li><li>**Är [binda (inkludera = ”â €...”)] användbara för att redigera åtgärder? -** Nej. [Bindning (inkludera = ”â €...”)] är endast tillgänglig för INSERT-format (lägga till nya data). Använd en annan metod som har en separat bindning modeller eller skicka en lista med tillåtna egenskaper tooUpdateModel eller TryUpdateModel för UPDATE-format (ändra befintliga data). Lägga till en [binda (inkludera = ”â €...”)]-attributet på en redigeringsåtgärd innebär att MVC skapar en objektinstans och ange det bara finns hello egenskaper, lämnar du alla andra standardinställningarna. Om hello data sparas ersätts helt hello befintlig entitet återställer hello värden för alla utelämnade egenskaper tootheir standardvärden. Om exempelvis IsAdmin utelämnades från en [binda (inkludera = ”â €...”)] attributet på en Redigera åtgärd, alla användare vars namn har redigerats via den här åtgärden skulle vara Återställ tooIsAdmin = false (alla redigerade användare förlorar Administratörsstatus). Om du vill tooprevent uppdaterar toocertain egenskaper, med någon av hello andra lösningar ovan. Observera att vissa versioner av MVC-tooling generera controller klasser med [binda (inkludera = ”â €...”)] redigera åtgärder och innebär att ta bort en egenskap från listan för att överdrivet bokföring attacker. Som beskrivs ovan, kan den metoden fungerar inte som avsett och i stället återställs alla data i hello utelämnas egenskaper tootheir standardvärden</li><li>**För att skapa åtgärder, finns det några varningar med [binda (inkludera = ”â €...”)] i stället för en separat bindning modeller? -** Ja. Den här metoden fungerar inte redigera scenarier som kräver underhåll av två separata tillvägagångssätt för att minimera alla över bokföring sårbarheter först. Andra, separat bindning modeller tillse åtskillnad mellan för frågor mellan hello form som används för användaren indata och hello form som används för beständiga, något [binda (inkludera = ”â €...”)] påverkas inte. Observera att det tredje [binda (inkludera = ”â €...”)] kan endast hantera översta egenskaper. Du kan inte tillåta endast delar av underordnade egenskaper (till exempel ”Details.Name”) i hello-attribut. Slutligen och kanske använder viktigast av allt [binda (inkludera = ”â €...”)] lägger till ytterligare ett steg som måste registreras någon gång hello klass används för modellbindning. Om en ny åtgärdsmetod Binder toohello dataklassen direkt och glömmer tooinclude en [binda (inkludera = ”â €...”)] attribut, kan det bli sårbart tooover bokföring attacker så hello [binda (inkludera = ”â €...”)] metod är mindre säker som standard. Om du använder [binda (inkludera = ”â €...”)], ta hand alltid tooremember toospecify det varje gång din dataklasser visas som åtgärden metodparametrar.</li><li>**Skapa operations hello vad om att publicera [binda (inkludera = ”â €...”)]-attributet på hello modellklass själva? Inte den här metoden undvika hello måste tooremember tas hello-attributet på varje åtgärdsmetod? -** Den här metoden fungerar i vissa fall. Med hjälp av [binda (inkludera = ”â €...”)] i hello modelltypen sig själv (istället för på åtgärdsparametrar med den här klassen), undvika hello måste tooremember tooinclude hello [binda (inkludera = ”â €...”)]-attributet på varje åtgärdsmetod. Använda hello attributet direkt på hello klassen effektivt skapar en separat ytan på den här klassen för modellen bindning. Den här metoden kan dock endast för en modell bindning form per modellklass. Den här metoden fungerar inte om en åtgärdsmetod måste tooallow modellbindning för ett fält (till exempel en administrativ åtgärd som uppdaterar användarroller) och andra åtgärder måste tooprevent modellbindning i det här fältet. Varje klass kan bara ha en modell bindning form. Om olika åtgärder behöver annan modell bindning former, måste de toorepresent dessa separata former med hjälp av antingen separata modellen bindning klasser eller olika [binda (inkludera = ”â €...”)]-attribut i hello åtgärdsmetoder.</li><li>**Vad bindning modeller? Är de hello detsamma som att visa modeller? -** Dessa är två begreppen. hello termen bindning modellen refererar tooa modellklass som används i en åtgärd är parameterlista (hello form som överförts från MVC modellen bindning toohello åtgärdsmetod). hello termen vymodellen refererar tooa modellklass som överförts från en åtgärd metoden tooa vy. Med hjälp av en vy-specifika modellen är en vanlig metod för att skicka data från en åtgärd metoden tooa vy. Ofta formen passar också för modellbindning och hello termen vymodellen kan vara används toorefer hello används för samma modell på båda platserna. toobe exakt, den här proceduren nämns specifikt bindning modeller, fokuserar på hello form skickades toohello åtgärden, vilket är det som är viktigt för masslagring tilldelning.</li></ul>| 

## <a id="rendering"></a>Koda obetrodda web utdata tidigare toorendering

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk Web Forms, MVC5, MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Hur tooprevent webbplatser skript i ASP.NET](http://msdn.microsoft.com/library/ms998274.aspx), [webbplatser skript](http://cwe.mitre.org/data/definitions/79.html), [XSS (skriptkörning över flera) förebyggande Cheat blad](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Steg** | Globala webbplatsskript (förkortas ofta XSS) är en angreppsvinkel för alla/programkomponent som förbrukar indata från hello web eller online services. XSS-problem kan en angripare tooexecute skript på en annan användares dator via ett sårbara webbprogram. Skadliga skript kan använda toosteal cookies och annars manipulera en drabbade datorn via JavaScript. XSS förhindras genom att verifiera indata från användaren, se till att den är felfritt och kodning innan den återges på en webbsida. Verifiering av indata och utdatakodning kan göras med hjälp av bibliotek för skydd av webbprogram. För förvaltad kod (C\#, VB.net osv), använda en eller flera lämpliga kodning metoder från hello Web Protection (skydd mot XSS)-biblioteket, beroende på hello kontext där hello användarindata hämtar manifesterad:| 

### <a name="example"></a>Exempel

```C#
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a id="typemodel"></a>Utföra verifiering av indata- och filtrering på alla strängtyp modellegenskaper

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och MVC5, MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Lägga till verifiering](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [verifierar modelldata i ett MVC-program](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [riktlinjer för ASP.NET MVC-program](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Steg** | <p>Alla hello indataparametrar måste verifieras innan de används i hello programmet tooensure att programmet hello skyddas mot skadliga användarindata. Validera hello med reguljära uttryck verifieringar på serversidan med en strategi för verifiering av godkända värden. Unsanitized användarindata / parametrar toohello metoder kan orsaka kod injektion säkerhetsproblem.</p><p>För webbprogram, kan även startpunkter inkludera formulärfält, QueryStrings, cookies, HTTP-huvuden och webbtjänstparametrar.</p><p>hello måste följande kontroller för verifiering av indata utföras när modellbindning:</p><ul><li>hello modellegenskaper ska förses med reguljärt uttryck anteckningen, för att acceptera tillåtna tecken och högsta tillåtna längden</li><li>Hej kontrollantmetoder utför ModelState giltighetsperiod</li></ul>|

## <a id="richtext"></a>Rensning ska tillämpas på formulärfält som accepterar tecken, t.ex., textredigeraren

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Koda osäkra indata](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML rensare](https://github.com/mganss/HtmlSanitizer) |
| **Steg** | <p>Identifiera alla statiska taggar som du vill toouse. Ett vanligt är toorestrict formatering toosafe HTML-element som `<b>` (fetstil) och `<i>` (kursiv).</p><p>Innan du skriver hello data, HTML-koda den. Detta gör alla skadliga skript säker genom att göra det toobe hanteras som text, inte som körbar kod.</p><ol><li>Inaktivera ASP.NET-begäran verifiering genom att lägga till hello hello ValidateRequest = ”false” attributet toohello @ Page-direktivet</li><li>Koda hello stränginmatning med hello HtmlEncode metod</li><li>Använda en StringBuilder och dess Ersätt metoden tooselectively ta bort hello kodning för hello HTML-element som du vill toopermit anrop</li></ol><p>hello sida i hello referenser inaktiverar ASP.NET valideringen genom att ange `ValidateRequest="false"`. Den HTML-kodas hello indata och selektivt tillåter hello `<b>` och `<i>` också en .NET-bibliotek för HTML-rensning också kan användas.</p><p>HtmlSanitizer är en .NET-bibliotek för rensning av HTML-kod och dokument från konstruktioner som kan leda till tooXSS attacker. Den använder AngleSharp tooparse, ändra och återge HTML- och CSS. HtmlSanitizer kan installeras som en NuGet-paketet och hello användarindata kan skickas via relevanta HTML- eller CSS rensning metoder som tillämpas på hello på serversidan. Observera som rensning som en säkerhetskontroll ska betraktas som ett sista alternativ.</p><p>Verifiering av indata och utdatakodning anses bättre säkerhetsåtgärder.</p> |

## <a id="inbuilt-encode"></a>Tilldela inte DOM-element toosinks som inte har inbyggda kodning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Många javascript-funktioner göra inte kodning som standard. När du tilldelar obetrodda inkommande tooDOM element via sådana funktioner kan orsaka mellan platsen skript (XSS) körningar.| 

### <a name="example"></a>Exempel
Följande är osäker exempel: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Använd inte `innerHtml`; i stället använda `innerText`. På samma sätt i stället för `$("#elm").html()`, använda`$("#elm").text()` 

## <a id="redirect-safe"></a>Verifiera att alla omdirigeringar i programmet hello är stängda eller göra på ett säkert sätt

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [hello OAuth 2.0 auktorisering Framework - öppna omdirigerare](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Steg** | <p>Programdesign som kräver omdirigering tooa användardefinierat plats måste begränsa hello möjliga omdirigering mål tooa fördefinierade ”säker” listan över platser eller domäner. Alla omdirigeringar i hello program måste vara stängd/säkert.</p><p>toodo detta:</p><ul><li>Identifiera alla omdirigeringar</li><li>Implementera en lämplig lösning för varje omdirigeras. Lämpliga åtgärder omfattar omdirigering godkända eller användare bekräftelse. Om en webbplats eller tjänst med en öppen omdirigering säkerhetsproblem använder OpenID-Facebook/OAuth identitetsleverantörer, kan en angripare stjäl en användares inloggningstoken och personifiera användaren. Detta är en inbyggd risk när du använder OAuth som beskrivs i RFC 6749 ”hello OAuth 2.0 auktorisering Framework”, avsnittet 10.15 ”öppna omdirigerar” på samma sätt, användarnas autentiseringsuppgifter kan äventyras av spear nätfiskeattacker med öppna omdirigeringar</li></ul>|

## <a id="string-method"></a>Implementera verifiering av indata på alla typen strängparametrar accepteras av kontrollantmetoder

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och MVC5, MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Validera modelldata i ett MVC-program](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [riktlinjer för ASP.NET MVC-program](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Steg** | Verifiering av indata med reguljära uttryck ska göras för metoder som bara godkänner primitiv datatyp och inte modeller som argument. Här ska Regex.IsMatch användas med ett giltigt regex-mönster. Om hello-indata stämmer inte med hello angivna reguljära uttrycket, kontrollen inte ska kunna fortsätta och en förvarning om valideringsfelet ska visas.| 

## <a id="dos-expression"></a>Ange övre gräns för tidsgränsen för reguljärt uttryck som bearbetar tooprevent DoS på grund av toobad reguljära uttryck

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk Web Forms, MVC5, MVC6  |
| **Attribut**              | Saknas  |
| **Referenser**              | [Egenskapen DefaultRegexMatchTimeout](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Steg** | tooensure DOS-attacker mot ange felaktigt skapade reguljära uttryck som orsaka mycket slipper tidsgränsen för hello globalt standardvärde. Om hello bearbetningstid tar längre tid än hello definierade övre gräns, skulle det genereras ett undantag. Om ingenting har konfigurerats skulle hello timeout vara oändligt.| 

### <a name="example"></a>Exempel
Till exempel hello följande konfiguration kommer att kasta ett RegexMatchTimeoutException om hello bearbetning tar mer än 5 sekunder: 

```C#
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Undvik att använda Html.Raw i Razor vyer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Steg | ASP.Net-webbsidor (Razor) utför automatisk HTML-kodningen. Alla strängar som skrivits ut av inbäddad kod nuggets (@ block) är automatiskt HTML-kodas. Men när `HtmlHelper.Raw` -metoden har anropats, returneras markup som inte är HTML-kodat. Om `Html.Raw()` hjälpmetod används kan det kringgår hello automatisk kodning skydd som Razor.|

### <a name="example"></a>Exempel
Följande är en osäker exempel: 

```C#
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Använd inte `Html.Raw()` om du inte behöver toodisplay markering. Den här metoden utför inte utdata kodning implicit. Använda andra ASP.NET hjälpprogram t.ex.`@Html.DisplayFor()` 

## <a id="stored-proc"></a>Använd inte dynamisk frågor i lagrade procedurer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>En attack med SQL injection utnyttjar sårbarheter i verifiering av indata toorun godtycklig kommandon i hello-databasen. Det kan inträffa när ditt program använder inkommande tooconstruct dynamisk SQL-instruktioner tooaccess hello databasen. Det kan också inträffa om din kod använder lagrade procedurer som skickas strängar som innehåller rådata användarindata. Med hello SQL injection attack köra hello angripare godtycklig kommandon i hello-databasen. Alla SQL-instruktioner (inklusive hello SQL-instruktioner i lagrade procedurer) måste vara parametriserade. SQL-instruktioner accepterar tecken som har en särskild innebörd tooSQL (till exempel enkelt citattecken) utan problem Eftersom de har strikt typkontroll. |

### <a name="example"></a>Exempel
Följande är ett exempel på osäker dynamisk lagrade proceduren: 

```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Exempel
Nedan följer hello samma lagrade proceduren införts på ett säkert sätt: 
```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a id="validation-api"></a>Se till att modellen verifieringen är klar för Web API-metoder

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Modellverifiering i ASP.NET webb-API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Steg** | När en klient skickar data tooa webb-API, är obligatoriska toovalidate hello data innan du gör några bearbetning. Använd data anteckningar på modeller tooset valideringsregler för hello modellen hello egenskaper för ASP.NET Web API-gränssnitt som acceptera modeller som indata.|

### <a name="example"></a>Exempel
hello följande kod visar hello samma: 

```C#
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Exempel
I hello åtgärdsmetod av hello API-kontrollanter har giltighet hello modellen toobe explicit kontrolleras enligt nedan: 

```C#
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with hello product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a id="string-api"></a>Implementera verifiering av indata på alla typen strängparametrar accepteras av Web API-metoder

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och MVC 5, MVC 6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Validera modelldata i ett MVC-program](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [riktlinjer för ASP.NET MVC-program](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Steg** | Verifiering av indata med reguljära uttryck ska göras för metoder som bara godkänner primitiv datatyp och inte modeller som argument. Här ska Regex.IsMatch användas med ett giltigt regex-mönster. Om hello-indata stämmer inte med hello angivna reguljära uttrycket, kontrollen inte ska kunna fortsätta och en förvarning om valideringsfelet ska visas.|

## <a id="typesafe-api"></a>Se till att typen säkert parametrar används i Web API för dataåtkomst

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Om du använder hello Parametersamlingen är SQL hanterar hello indata som ett litteralvärde stället som körbar kod. hello Parametersamlingen kan vara används tooenforce typ och längd begränsningar på indata. Värden som ligger utanför intervallet för hello utlöser ett undantag. Om typen säkert SQL-parametrar används kanske angripare kan tooexecute injektion attacker som är inbäddade i hello ofiltrerad indata.</p><p>Använd säker typparametrar när konstruera SQL frågar tooavoid SQL injection attacker som kan uppstå med ofiltrerade indata. Du kan använda säker typparametrar med lagrade procedurer och dynamiska SQL-uttryck. Parametrarna behandlas som litterala värden av hello databasen och inte som körbar kod. Parametrar kontrolleras även för typ och längd.</p>|

### <a name="example"></a>Exempel
hello följande kod visar hur toouse Skriv säker parametrar med hello SqlParameterCollection när du anropar en lagrad procedur. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
I föregående exempel hello, kan inte hello indatavärdet vara längre än 11 tecken. Om hello data inte följer toohello typ eller längd som anges av parametern hello genereras hello SqlParameter klass ett undantag. 

## <a id="sql-docdb"></a>Använd innehåller parametrar SQL-frågor för Cosmos DB

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure dokumentet DB | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Om SQL-Parameterisering i DocumentDB](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Steg** | DocumentDB stöder bara skrivskyddade frågor, är SQL injection fortfarande möjligt om frågor skapas genom att sammanbinda indata från användaren. Det är möjligt för användaren toogain åtkomst toodata inte får komma åt inom hello samma samling genom att utforma skadliga SQL-frågor. Använd parametriserade SQL-frågor om frågor baserat på indata från användaren. |

## <a id="schema-binding"></a>WCF-indata verifiering via schemabindning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Steg** | <p>Brist på verifiering leder injection toodifferent typen av attacker.</p><p>Meddelandevalidering representerar en försvarslinje i hello skydd av WCF-program. Med den här metoden kan du validera meddelanden med scheman tooprotect WCF-tjänståtgärder mot angrepp av en obehörig klient. Validera alla meddelanden som tagits emot av hello klient tooprotect hello klient mot angrepp av skadlig service. Meddelandevalidering gör det möjligt toovalidate meddelanden när aktiviteter kan förbruka meddelandet avtal eller datakontrakt som inte kan göras med Parametervalidering. Meddelandevalidering kan toocreate valideringslogik inuti scheman, vilket ger bättre flexibilitet och minska tid. Scheman kan återanvändas i olika program i hello organisation, skapa normer för data representation. Dessutom tillåter meddelandevalidering du tooprotect åtgärder när de använder mer komplexa datatyper som involverar kontrakt som representerar affärslogik.</p><p>tooperform meddelandevalidering först skapar du ett schema som representerar hello åtgärder för din tjänst och hello datatyper som används av dessa åtgärder. Sedan kan du skapa en .NET-klass som implementerar en anpassad klient meddelandet inspector och anpassade dispatcher meddelande inspector toovalidate hello meddelanden skickas/tas emot från hello-tjänsten. Därefter kan du implementera en anpassad endpoint beteende tooenable meddelande verifiering på både hello klient- och hello. Slutligen kan du implementera anpassade konfigurationselementet på hello-klass tooexpose hello utökad anpassade slutpunkt i hello konfigurationsfilen för hello tjänst eller hello klienten ”</p>|

## <a id="parameters"></a>WCF - indata verifiering via parametern kontrollanter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Steg** | <p>Indata och dataverifiering representerar en viktig försvarslinje i hello skydd av WCF-program. Du bör verifiera alla parametrar som visas i WCF operations tooprotect hello-tjänsten från angrepp av en obehörig klient. Däremot bör du också verifiera alla returvärden som tagits emot av hello klient tooprotect hello klient mot angrepp av skadlig service</p><p>WCF erbjuder olika punkter som tillåter toocustomize hello WCF runtime beteende genom att skapa anpassade tillägg. Meddelandet kontrollanter och parametern-kontrollanter är två utökningsbarhet mekanismer användas toogain större kontroll över hello data som skickas mellan en klient och en tjänst. Du bör använda parametern-kontrollanter för verifiering av indata och använder meddelandet kontrollanter endast när du behöver tooinspect hela hälsningsmeddelande flödar till och från en tjänst.</p><p>tooperform indata verifiering, du skapar en .NET-klass och implementera en anpassad parameter inspector i ordning toovalidate parametrar på åtgärder i din tjänst. Du kommer sedan att implementera en anpassad endpoint beteende tooenable validering på både hello klient- och hello. Du kommer slutligen att implementera anpassade konfigurationselementet på hello-klass tooexpose hello utökad anpassade slutpunkt i hello konfigurationsfilen för hello tjänst eller hello-klienten</p>|
