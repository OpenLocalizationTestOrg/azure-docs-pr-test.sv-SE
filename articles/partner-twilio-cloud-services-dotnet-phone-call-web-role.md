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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="41f36-104">Hur toomake en telefon ringer med Twilio i en webbroll på Azure</span><span class="sxs-lookup"><span data-stu-id="41f36-104">How toomake a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="41f36-105">Den här guiden visar hur toouse Twilio toomake ett samtal från en webbplats finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="41f36-105">This guide demonstrates how toouse Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="41f36-106">hello uppmanar resulterande programmet hello användaren toomake ett anrop med hello baserat på antalet och meddelandet som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="41f36-106">hello resulting application prompts hello user toomake a call with hello given number and message, as shown in hello following screenshot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form]

## <span data-ttu-id="41f36-108"><a name="twilio-prereqs"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="41f36-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="41f36-109">Du behöver toodo hello följande toouse hello koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="41f36-109">You will need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="41f36-110">Skaffa ett Twilio-konto och autentisering-token från hello [Twilio konsolen][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="41f36-110">Acquire a Twilio account and authentication token from hello [Twilio Console][twilio_console].</span></span> <span data-ttu-id="41f36-111">tooget igång med Twilio, logga på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="41f36-111">tooget started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="41f36-112">Du kan utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="41f36-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="41f36-113">Information om hello API som tillhandahålls av Twilio finns [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="41f36-113">For information about hello API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="41f36-114">Lägg till hello *Twilio .NET-bibliotek* tooyour webbroll.</span><span class="sxs-lookup"><span data-stu-id="41f36-114">Add hello *Twilio .NET libary* tooyour web role.</span></span> <span data-ttu-id="41f36-115">Se **webbrollsprojektet för tooadd hello Twilio bibliotek tooyour**senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="41f36-115">See **tooadd hello Twilio libraries tooyour web role project**, later in this topic.</span></span>

<span data-ttu-id="41f36-116">Du bör känna till hur du skapar en grundläggande [på Azure-Webbroll][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="41f36-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="41f36-117"><a name="howtocreateform"></a>Så här: skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="41f36-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="41f36-118"><a id="use_nuget"></a>tooadd hello Twilio bibliotek tooyour webbrollsprojektet:</span><span class="sxs-lookup"><span data-stu-id="41f36-118"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour web role project:</span></span>

1. <span data-ttu-id="41f36-119">Öppna din lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41f36-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="41f36-120">Högerklicka på **referenser**.</span><span class="sxs-lookup"><span data-stu-id="41f36-120">Right-click **References**.</span></span>
3. <span data-ttu-id="41f36-121">Klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="41f36-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="41f36-122">Klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="41f36-122">Click **Online**.</span></span>
5. <span data-ttu-id="41f36-123">Skriv i hello online sökrutan *twilio*.</span><span class="sxs-lookup"><span data-stu-id="41f36-123">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="41f36-124">Klicka på **installera** på hello Twilio-paketet.</span><span class="sxs-lookup"><span data-stu-id="41f36-124">Click **Install** on hello Twilio package.</span></span>

<span data-ttu-id="41f36-125">hello följande kod visar hur toocreate en webbplats formuläret tooretrieve användardata för ett samtal.</span><span class="sxs-lookup"><span data-stu-id="41f36-125">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="41f36-126">I det här exemplet, ett ASP.NET Web Role med namnet **TwilioCloud** skapas.</span><span class="sxs-lookup"><span data-stu-id="41f36-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="41f36-127"><a id="howtocreatecode"></a>Så här: skapa hello kod toomake hello anrop</span><span class="sxs-lookup"><span data-stu-id="41f36-127"><a id="howtocreatecode"></a>How to: Create hello code toomake hello call</span></span>
<span data-ttu-id="41f36-128">hello följande kod som anropas när hello användaren Slutför hello formuläret, skapar anropet hälsningsmeddelande och genererar hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="41f36-128">hello following code, which is called when hello user completes hello form, creates hello call message and generates hello call.</span></span> <span data-ttu-id="41f36-129">I det här exemplet körs hello koden i hello onclick händelsehanteraren hello knappen hello formuläret.</span><span class="sxs-lookup"><span data-stu-id="41f36-129">In this example, hello code is run in hello onclick event handler of hello button on hello form.</span></span> <span data-ttu-id="41f36-130">(Använd ditt Twilio-konto och autentisering token i stället för hello platshållare för värden som har tilldelats för`accountSID` och `authToken` i hello koden nedan.)</span><span class="sxs-lookup"><span data-stu-id="41f36-130">(Use your Twilio account and authentication token instead of hello placeholder values assigned too`accountSID` and `authToken` in hello code below.)</span></span>

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

<span data-ttu-id="41f36-131">hello-anrop och hello Twilio slutpunkt, API-versionen och hello samtalsstatus visas.</span><span class="sxs-lookup"><span data-stu-id="41f36-131">hello call is made, and hello Twilio endpoint, API version, and hello call status are displayed.</span></span> <span data-ttu-id="41f36-132">följande skärmbild visar utdata från ett exempel kör hello.</span><span class="sxs-lookup"><span data-stu-id="41f36-132">hello following screenshot shows output from a sample run.</span></span>

![Azure anropet svaret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="41f36-134">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="41f36-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="41f36-135">Mer information om &lt;säg&gt; och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="41f36-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="41f36-136"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41f36-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="41f36-137">Den här koden har angetts tooshow du grundläggande funktioner med hjälp av Twilio i ett ASP.NET web role på Azure.</span><span class="sxs-lookup"><span data-stu-id="41f36-137">This code was provided tooshow you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="41f36-138">Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion.</span><span class="sxs-lookup"><span data-stu-id="41f36-138">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="41f36-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="41f36-139">For example:</span></span>

* <span data-ttu-id="41f36-140">Istället för att använda ett webbformulär, kan du använda Azure Blob storage- eller ett telefonnummer för Azure SQL Database-instans toostore och anropa text.</span><span class="sxs-lookup"><span data-stu-id="41f36-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance toostore phone numbers and call text.</span></span> <span data-ttu-id="41f36-141">Information om hur du använder BLOB i Azure finns [hur toouse hello Azure Blob storage-tjänst i .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="41f36-141">For information about using Blobs in Azure, see [How toouse hello Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="41f36-142">Information om hur du använder SQL-databas finns [hur toouse Azure SQL-databas i .NET-program][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="41f36-142">For information about using SQL Database, see [How toouse Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="41f36-143">Du kan använda `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda hello värden i formuläret.</span><span class="sxs-lookup"><span data-stu-id="41f36-143">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in your form.</span></span> <span data-ttu-id="41f36-144">Information om hello `RoleEnvironment` klassen, se [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="41f36-144">For information about hello `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="41f36-145">Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="41f36-145">Read hello Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="41f36-146">Mer information om Twilio på [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="41f36-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="41f36-147"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="41f36-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="41f36-148">Hur toouse Twilio för röst- och SMS-funktioner från Azure</span><span class="sxs-lookup"><span data-stu-id="41f36-148">How toouse Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
