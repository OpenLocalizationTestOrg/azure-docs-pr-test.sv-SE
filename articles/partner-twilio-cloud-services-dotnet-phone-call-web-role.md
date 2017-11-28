---
title: "Hur du gör ett telefonsamtal från Twilio (.NET) | Microsoft Docs"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Kodexempel som skrivits i .NET."
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
ms.openlocfilehash: 0899a49cbfda775017dab7fc6d8963bbeb86d74c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="c63df-104">Hur du gör ett telefonsamtal med Twilio i en webbroll på Azure</span><span class="sxs-lookup"><span data-stu-id="c63df-104">How to make a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="c63df-105">Den här guiden visar hur du använder Twilio för att ringa ett samtal från en webbsida som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="c63df-105">This guide demonstrates how to use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="c63df-106">Exempelprogrammet uppmanar användaren att göra anrop med angivet tal och meddelandet som visas i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="c63df-106">The resulting application prompts the user to make a call with the given number and message, as shown in the following screenshot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form]

## <span data-ttu-id="c63df-108"><a name="twilio-prereqs"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="c63df-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="c63df-109">Du måste göra följande för att använda koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="c63df-109">You will need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="c63df-110">Skaffa ett Twilio-konto och autentisering-token från den [Twilio konsolen][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="c63df-110">Acquire a Twilio account and authentication token from the [Twilio Console][twilio_console].</span></span> <span data-ttu-id="c63df-111">Registrera dig för att komma igång med Twilio på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="c63df-111">To get started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="c63df-112">Du kan utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="c63df-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="c63df-113">Mer information om API som tillhandahålls av Twilio finns [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="c63df-113">For information about the API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="c63df-114">Lägg till den *Twilio .NET-bibliotek* till web-roll.</span><span class="sxs-lookup"><span data-stu-id="c63df-114">Add the *Twilio .NET libary* to your web role.</span></span> <span data-ttu-id="c63df-115">Se **att lägga till Twilio-biblioteken i din webbrollsprojektet**senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c63df-115">See **To add the Twilio libraries to your web role project**, later in this topic.</span></span>

<span data-ttu-id="c63df-116">Du bör känna till hur du skapar en grundläggande [på Azure-Webbroll][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="c63df-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="c63df-117"><a name="howtocreateform"></a>Så här: skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="c63df-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="c63df-118"><a id="use_nuget"></a>Lägg till Twilio-bibliotek i webbprojektet för rollen:</span><span class="sxs-lookup"><span data-stu-id="c63df-118"><a id="use_nuget"></a>To add the Twilio libraries to your web role project:</span></span>

1. <span data-ttu-id="c63df-119">Öppna din lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c63df-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="c63df-120">Högerklicka på **referenser**.</span><span class="sxs-lookup"><span data-stu-id="c63df-120">Right-click **References**.</span></span>
3. <span data-ttu-id="c63df-121">Klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="c63df-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="c63df-122">Klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="c63df-122">Click **Online**.</span></span>
5. <span data-ttu-id="c63df-123">Skriv i sökrutan online *twilio*.</span><span class="sxs-lookup"><span data-stu-id="c63df-123">In the search online box, type *twilio*.</span></span>
6. <span data-ttu-id="c63df-124">Klicka på **installera** på Twilio-paketet.</span><span class="sxs-lookup"><span data-stu-id="c63df-124">Click **Install** on the Twilio package.</span></span>

<span data-ttu-id="c63df-125">Följande kod visar hur du skapar ett webbformulär för att hämta användardata för ett samtal.</span><span class="sxs-lookup"><span data-stu-id="c63df-125">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="c63df-126">I det här exemplet, ett ASP.NET Web Role med namnet **TwilioCloud** skapas.</span><span class="sxs-lookup"><span data-stu-id="c63df-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="c63df-127"><a id="howtocreatecode"></a>Så här: skapa koden för att göra anrop</span><span class="sxs-lookup"><span data-stu-id="c63df-127"><a id="howtocreatecode"></a>How to: Create the code to make the call</span></span>
<span data-ttu-id="c63df-128">Följande kod, som kallas när användaren slutför formuläret, skapar anropet meddelandet och genererar anropet.</span><span class="sxs-lookup"><span data-stu-id="c63df-128">The following code, which is called when the user completes the form, creates the call message and generates the call.</span></span> <span data-ttu-id="c63df-129">I det här exemplet körs koden i händelsehanteraren onclick för knappen i formuläret.</span><span class="sxs-lookup"><span data-stu-id="c63df-129">In this example, the code is run in the onclick event handler of the button on the form.</span></span> <span data-ttu-id="c63df-130">(Använd ditt Twilio-konto och autentisering token i stället för platshållarvärdena för `accountSID` och `authToken` i koden nedan.)</span><span class="sxs-lookup"><span data-stu-id="c63df-130">(Use your Twilio account and authentication token instead of the placeholder values assigned to `accountSID` and `authToken` in the code below.)</span></span>

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
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

<span data-ttu-id="c63df-131">Anropet görs och Twilio-slutpunkten och API-versionen av samtalsstatus visas.</span><span class="sxs-lookup"><span data-stu-id="c63df-131">The call is made, and the Twilio endpoint, API version, and the call status are displayed.</span></span> <span data-ttu-id="c63df-132">Följande skärmbild visar utdata från en exempel-körning.</span><span class="sxs-lookup"><span data-stu-id="c63df-132">The following screenshot shows output from a sample run.</span></span>

![Azure anropet svaret med hjälp av Twilio och ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="c63df-134">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="c63df-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="c63df-135">Mer information om &lt;säg&gt; och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="c63df-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="c63df-136"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c63df-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="c63df-137">Den här koden har angetts för att visa grundläggande funktioner med hjälp av Twilio i ett ASP.NET web role på Azure.</span><span class="sxs-lookup"><span data-stu-id="c63df-137">This code was provided to show you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="c63df-138">Innan du distribuerar till Azure i produktion, kanske du vill lägga till flera felhantering eller andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="c63df-138">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="c63df-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c63df-139">For example:</span></span>

* <span data-ttu-id="c63df-140">Istället för att använda ett webbformulär, kunde du använda Azure Blob-lagring eller en Azure SQL Database-instans för att lagra telefonnummer och anropa text.</span><span class="sxs-lookup"><span data-stu-id="c63df-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance to store phone numbers and call text.</span></span> <span data-ttu-id="c63df-141">Information om hur du använder BLOB i Azure finns [hur du använder tjänsten Azure Blob storage i .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c63df-141">For information about using Blobs in Azure, see [How to use the Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="c63df-142">Information om hur du använder SQL-databas finns [hur du använder Azure SQL Database i .NET-program][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c63df-142">For information about using SQL Database, see [How to use Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="c63df-143">Du kan använda `RoleEnvironment.getConfigurationSettings` för att hämta Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda värden i formuläret.</span><span class="sxs-lookup"><span data-stu-id="c63df-143">You could use `RoleEnvironment.getConfigurationSettings` to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in your form.</span></span> <span data-ttu-id="c63df-144">Information om den `RoleEnvironment` klassen, se [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c63df-144">For information about the `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="c63df-145">Läs Twilio-riktlinjer för säkerhet på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="c63df-145">Read the Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="c63df-146">Mer information om Twilio på [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="c63df-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="c63df-147"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="c63df-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="c63df-148">Hur du använder Twilio för röst- och SMS-funktioner från Azure</span><span class="sxs-lookup"><span data-stu-id="c63df-148">How to use Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
