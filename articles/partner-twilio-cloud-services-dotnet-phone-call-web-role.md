---
title: "aaaHow toomake ett telefonsamtal från Twilio (.NET) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i .NET."
services: 
documentationcenter: .net
author: devinrader
manager: timlt
editor: 
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 857d89961c563a51fef944f4a72828036af79b43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Hur toomake en telefon ringer med Twilio i en webbroll på Azure
Den här guiden visar hur toouse Twilio toomake ett samtal från en webbplats finns i Azure. hello uppmanar resulterande programmet hello användaren toomake ett anrop med hello baserat på antalet och meddelandet som visas i följande skärmbild hello.

![Azure anropet formuläret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Förhandskrav
Du behöver toodo hello följande toouse hello koden i det här avsnittet:

1. Skaffa ett Twilio-konto och autentisering-token från hello [Twilio konsolen][twilio_console]. tooget igång med Twilio, logga på [https://www.twilio.com/try-twilio][try_twilio]. Du kan utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing]. Information om hello API som tillhandahålls av Twilio finns [http://www.twilio.com/voice/api][twilio_api].
2. Lägg till hello *Twilio .NET-bibliotek* tooyour webbroll. Se **webbrollsprojektet för tooadd hello Twilio bibliotek tooyour**senare i det här avsnittet.

Du bör känna till hur du skapar en grundläggande [på Azure-Webbroll][azure_webroles_get_started].

## <a name="howtocreateform"></a>Så här: skapa ett webbformulär för ett samtal
<a id="use_nuget"></a>tooadd hello Twilio bibliotek tooyour webbrollsprojektet:

1. Öppna din lösning i Visual Studio.
2. Högerklicka på **referenser**.
3. Klicka på **hantera NuGet-paket**.
4. Klicka på **Online**.
5. Skriv i hello online sökrutan *twilio*.
6. Klicka på **installera** på hello Twilio-paketet.

hello följande kod visar hur toocreate en webbplats formuläret tooretrieve användardata för ett samtal. I det här exemplet, ett ASP.NET Web Role med namnet **TwilioCloud** skapas.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a id="howtocreatecode"></a>Så här: skapa hello kod toomake hello anrop
hello följande kod som anropas när hello användaren Slutför hello formuläret, skapar anropet hälsningsmeddelande och genererar hello-anrop. I det här exemplet körs hello koden i hello onclick händelsehanteraren hello knappen hello formuläret. (Använd ditt Twilio-konto och autentisering token i stället för hello platshållare för värden som har tilldelats för`accountSID` och `authToken` i hello koden nedan.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call porcessing happens here.

            // Use your account SID and authentication token instead of
            // hello placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of hello Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve hello account, used later tooretrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve hello values entered by hello user.
                var too= PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using hello Twilio message and hello user-entered
                // text. You must replace spaces in hello user's text with '%20'
                // toomake hello text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display hello endpoint, API version, and hello URL for hello message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"hello URL is {url}");

                // Place hello call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

hello-anrop och hello Twilio slutpunkt, API-versionen och hello samtalsstatus visas. följande skärmbild visar utdata från ett exempel kör hello.

![Azure anropet svaret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form_output]

Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml]. Mer information om &lt;säg&gt; och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Nästa steg
Den här koden har angetts tooshow du grundläggande funktioner med hjälp av Twilio i ett ASP.NET web role på Azure. Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion. Exempel:

* Istället för att använda ett webbformulär, kan du använda Azure Blob storage- eller ett telefonnummer för Azure SQL Database-instans toostore och anropa text. Information om hur du använder BLOB i Azure finns [hur toouse hello Azure Blob storage-tjänst i .NET][howto_blob_storage_dotnet]. Information om hur du använder SQL-databas finns [hur toouse Azure SQL-databas i .NET-program][howto_sql_azure_dotnet].
* Du kan använda `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda hello värden i formuläret. Information om hello `RoleEnvironment` klassen, se [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].
* Mer information om Twilio på [https://www.twilio.com/docs][twilio_docs].

## <a name="seealso"></a>Se även
* [Hur toouse Twilio för röst- och SMS-funktioner från Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-get-started
