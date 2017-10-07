---
title: "aaaMFA software development kit för anpassade appar | Microsoft Docs"
description: "Den här artikeln visar hur toodownload och använda hello Azure MFA SDK tooenable tvåstegsverifiering för dina egna appar."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 10e02e844bf3928575bfca79dbc34717a31a08b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a><span data-ttu-id="70315-103">Skapa Multifaktorautentisering i anpassade appar (SDK)</span><span class="sxs-lookup"><span data-stu-id="70315-103">Building Multi-Factor Authentication into Custom Apps (SDK)</span></span>

<span data-ttu-id="70315-104">hello Azure Multi-Factor Authentication Software Development Kit (SDK) kan du skapa tvåstegsverifiering direkt i hello inloggning eller transaktionen bearbetar för program i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="70315-104">hello Azure Multi-Factor Authentication Software Development Kit (SDK) lets you build two-step verification directly into hello sign-in or transaction processes of applications in your Azure AD tenant.</span></span>

<span data-ttu-id="70315-105">Hej Multi-Factor Authentication SDK är tillgängligt för C#, Visual Basic (.NET), Java, Perl, PHP och Ruby.</span><span class="sxs-lookup"><span data-stu-id="70315-105">hello Multi-Factor Authentication SDK is available for C#, Visual Basic (.NET), Java, Perl, PHP, and Ruby.</span></span> <span data-ttu-id="70315-106">hello SDK innehåller en tunn wrapper runt tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="70315-106">hello SDK provides a thin wrapper around two-step verification.</span></span> <span data-ttu-id="70315-107">Den innehåller allt du behöver toowrite din kod, inklusive kommenterad källkodsfiler, exempel filer och en detaljerad viktigt-fil.</span><span class="sxs-lookup"><span data-stu-id="70315-107">It includes everything you need toowrite your code, including commented source code files, example files, and a detailed ReadMe file.</span></span> <span data-ttu-id="70315-108">Varje SDK innehåller också ett certifikat och privat nyckel för att kryptera transaktioner som är unikt tooyour Flerfunktionsautentiseringsleverantören.</span><span class="sxs-lookup"><span data-stu-id="70315-108">Each SDK also includes a certificate and private key for encrypting transactions that are unique tooyour Multi-Factor Authentication Provider.</span></span> <span data-ttu-id="70315-109">Så länge som du har en provider, kan du hämta hello SDK i så många språk och format som du behöver.</span><span class="sxs-lookup"><span data-stu-id="70315-109">As long as you have a provider, you can download hello SDK in as many languages and formats as you need.</span></span>

<span data-ttu-id="70315-110">hello strukturen för hello API: er i hello Multi-Factor Authentication SDK är enkelt.</span><span class="sxs-lookup"><span data-stu-id="70315-110">hello structure of hello APIs in hello Multi-Factor Authentication SDK is simple.</span></span> <span data-ttu-id="70315-111">Gör en enskild funktion anropa tooan API hello Multi-Factor alternativet parametrar (t.ex. verifieringen läge) och användardata (till exempel hello telefon nummer toocall eller antalet toovalidate för hello PIN-kod).</span><span class="sxs-lookup"><span data-stu-id="70315-111">Make a single function call tooan API with hello multi-factor option parameters (like verification mode) and user data (like hello telephone number toocall or hello PIN number toovalidate).</span></span> <span data-ttu-id="70315-112">hello API: er översätta hello funktionsanrop i web services begäranden toohello molnbaserad Azure Multi-Factor Authentication-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="70315-112">hello APIs translate hello function call into web services requests toohello cloud-based Azure Multi-Factor Authentication Service.</span></span> <span data-ttu-id="70315-113">Alla anrop måste innehålla en referens toohello privat certifikat som ingår i varje SDK.</span><span class="sxs-lookup"><span data-stu-id="70315-113">All calls must include a reference toohello private certificate that is included in every SDK.</span></span>

<span data-ttu-id="70315-114">Eftersom hello API: er inte har åtkomst toousers registreras i Azure Active Directory, måste du ange användarinformation i en fil eller databas.</span><span class="sxs-lookup"><span data-stu-id="70315-114">Because hello APIs do not have access toousers registered in Azure Active Directory, you must provide user information in a file or database.</span></span> <span data-ttu-id="70315-115">Dessutom hello API: er ger inte funktioner för registrering eller användare, så du måste toobuild dessa processer i ditt program.</span><span class="sxs-lookup"><span data-stu-id="70315-115">Also, hello APIs do not provide enrollment or user management features, so you need toobuild these processes into your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70315-116">toodownload hello SDK måste du toocreate ett Azure leverantör av Multifaktorautent även om du har licenser för Azure MFA eller AAD Premium EMS.</span><span class="sxs-lookup"><span data-stu-id="70315-116">toodownload hello SDK, you need toocreate an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.</span></span> <span data-ttu-id="70315-117">Om du skapar ett Azure leverantör av Multifaktorautent för detta ändamål och redan har licenser, göra att toocreate hello providern med hello **Per aktiverad användare** modell.</span><span class="sxs-lookup"><span data-stu-id="70315-117">If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, make sure toocreate hello Provider with hello **Per Enabled User** model.</span></span> <span data-ttu-id="70315-118">Länka hello providern toohello katalog som innehåller hello Azure MFA, Azure AD Premium eller EMS-licenser.</span><span class="sxs-lookup"><span data-stu-id="70315-118">Then, link hello Provider toohello directory that contains hello Azure MFA, Azure AD Premium, or EMS licenses.</span></span> <span data-ttu-id="70315-119">Den här konfigurationen garanterar att du endast debiteras om du har flera unika användare som använder hello SDK än hello antalet licenser du äger.</span><span class="sxs-lookup"><span data-stu-id="70315-119">This configuration ensures that you are only billed if you have more unique users using hello SDK than hello number of licenses you own.</span></span>


## <a name="download-hello-sdk"></a><span data-ttu-id="70315-120">Hämta hello SDK</span><span class="sxs-lookup"><span data-stu-id="70315-120">Download hello SDK</span></span>
<span data-ttu-id="70315-121">Hämta hello Azure Multi-Factor SDK kräver en [Azure leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md).</span><span class="sxs-lookup"><span data-stu-id="70315-121">Downloading hello Azure Multi-Factor SDK requires an [Azure Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md).</span></span>  <span data-ttu-id="70315-122">Detta kräver en fullständig Azure-prenumeration, även om Azure MFA, Azure AD Premium eller Enterprise Mobility Suite licenser äger.</span><span class="sxs-lookup"><span data-stu-id="70315-122">This requires a full Azure subscription, even if Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses are owned.</span></span>  <span data-ttu-id="70315-123">toodownload hello SDK, navigera toohello Multi-Factor hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="70315-123">toodownload hello SDK, navigate toohello Multi-Factor Management Portal.</span></span> <span data-ttu-id="70315-124">Du kan nå hello portal genom att hantera hello leverantör av Multifaktorautent direkt eller genom att klicka på hello **”gå toohello portal”** länk på inställningssidan för hello MFA-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="70315-124">You can reach hello portal either by managing hello Multi-Factor Auth Provider directly, or by clicking hello **"Go toohello portal"** link on hello MFA service settings page.</span></span>

### <a name="download-from-hello-azure-classic-portal"></a><span data-ttu-id="70315-125">Ladda ned från hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="70315-125">Download from hello Azure classic portal</span></span>
1. <span data-ttu-id="70315-126">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="70315-126">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="70315-127">Hello vänster markerar **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="70315-127">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="70315-128">På hello Active Directory-sidan vid hello översta Välj **Flerfunktionsautentiseringsleverantörer**</span><span class="sxs-lookup"><span data-stu-id="70315-128">On hello Active Directory page, at hello top select **Multi-Factor Auth Providers**</span></span>
4. <span data-ttu-id="70315-129">Längst ned hello Välj **hantera**.</span><span class="sxs-lookup"><span data-stu-id="70315-129">At hello bottom select **Manage**.</span></span> <span data-ttu-id="70315-130">En ny sida öppnas.</span><span class="sxs-lookup"><span data-stu-id="70315-130">A new page opens.</span></span>
5. <span data-ttu-id="70315-131">Klicka på hello kvar hello längst ned i **SDK**.</span><span class="sxs-lookup"><span data-stu-id="70315-131">On hello left, at hello bottom, click **SDK**.</span></span>
   <span data-ttu-id="70315-132"><center>![Ladda ned](./media/multi-factor-authentication-sdk/download.png)</center></span><span class="sxs-lookup"><span data-stu-id="70315-132"><center>![Download](./media/multi-factor-authentication-sdk/download.png)</center></span></span>
6. <span data-ttu-id="70315-133">Välj hello språk du vill använda och klicka på en hello associerade länkar.</span><span class="sxs-lookup"><span data-stu-id="70315-133">Select hello language you want and click one hello associated download links.</span></span>
7. <span data-ttu-id="70315-134">Spara hello hämtning.</span><span class="sxs-lookup"><span data-stu-id="70315-134">Save hello download.</span></span>

### <a name="download-from-hello-service-settings"></a><span data-ttu-id="70315-135">Ladda ned från hello tjänstinställningar</span><span class="sxs-lookup"><span data-stu-id="70315-135">Download from hello service settings</span></span>
1. <span data-ttu-id="70315-136">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="70315-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="70315-137">Hello vänster markerar **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="70315-137">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="70315-138">Dubbelklicka på din instans av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70315-138">Double-click your instance of Azure AD.</span></span>
4. <span data-ttu-id="70315-139">Vid hello översta Klicka **konfigurera**</span><span class="sxs-lookup"><span data-stu-id="70315-139">At hello top click **Configure**</span></span>
5. <span data-ttu-id="70315-140">Välj under multifaktorautentisering, **hantera tjänstinställningar**
   ![hämta](./media/multi-factor-authentication-sdk/download2.png)</span><span class="sxs-lookup"><span data-stu-id="70315-140">Under multi-factor authentication, select **Manage service settings**
![Download](./media/multi-factor-authentication-sdk/download2.png)</span></span>
6. <span data-ttu-id="70315-141">Hello tjänster på sidan Inställningar hello ned hello-skärmen klickar du på **gå toohello portal**.</span><span class="sxs-lookup"><span data-stu-id="70315-141">On hello services settings page, at hello bottom of hello screen click **Go toohello portal**.</span></span> <span data-ttu-id="70315-142">En ny sida öppnas.</span><span class="sxs-lookup"><span data-stu-id="70315-142">A new page opens.</span></span>
   <span data-ttu-id="70315-143">![Ladda ned](./media/multi-factor-authentication-sdk/download3a.png)</span><span class="sxs-lookup"><span data-stu-id="70315-143">![Download](./media/multi-factor-authentication-sdk/download3a.png)</span></span>
7. <span data-ttu-id="70315-144">Klicka på hello kvar hello längst ned i **SDK**.</span><span class="sxs-lookup"><span data-stu-id="70315-144">On hello left, at hello bottom, click **SDK**.</span></span>
8. <span data-ttu-id="70315-145">Välj hello språk du vill använda och klicka på en hello associerade länkar.</span><span class="sxs-lookup"><span data-stu-id="70315-145">Select hello language you want and click one hello associated download links.</span></span>
9. <span data-ttu-id="70315-146">Spara hello hämtning.</span><span class="sxs-lookup"><span data-stu-id="70315-146">Save hello download.</span></span>

## <a name="whats-in-hello-sdk"></a><span data-ttu-id="70315-147">Vad är i hello SDK</span><span class="sxs-lookup"><span data-stu-id="70315-147">What's in hello SDK</span></span>
<span data-ttu-id="70315-148">hello SDK innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="70315-148">hello SDK includes hello following items:</span></span>

* <span data-ttu-id="70315-149">**VIKTIGT**.</span><span class="sxs-lookup"><span data-stu-id="70315-149">**README**.</span></span> <span data-ttu-id="70315-150">Förklarar hur toouse hello Multi-Factor Authentication-API: er i ett nytt eller befintligt program.</span><span class="sxs-lookup"><span data-stu-id="70315-150">Explains how toouse hello Multi-Factor Authentication APIs in a new or existing application.</span></span>
* <span data-ttu-id="70315-151">**Källfiler** för Multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="70315-151">**Source files** for Multi-Factor Authentication</span></span>
* <span data-ttu-id="70315-152">**Klientcertifikatet** att du använder toocommunicate med hello Multi-Factor Authentication-tjänsten</span><span class="sxs-lookup"><span data-stu-id="70315-152">**Client certificate** that you use toocommunicate with hello Multi-Factor Authentication service</span></span>
* <span data-ttu-id="70315-153">**Privat nyckel** för hello certifikat</span><span class="sxs-lookup"><span data-stu-id="70315-153">**Private key** for hello certificate</span></span>
* <span data-ttu-id="70315-154">**Anropa resultat.**</span><span class="sxs-lookup"><span data-stu-id="70315-154">**Call results.**</span></span> <span data-ttu-id="70315-155">En lista över resultatkoder för webbtjänstanrop.</span><span class="sxs-lookup"><span data-stu-id="70315-155">A list of call result codes.</span></span> <span data-ttu-id="70315-156">tooopen den här filen, Använd ett program med textformatering, till exempel WordPad.</span><span class="sxs-lookup"><span data-stu-id="70315-156">tooopen this file, use an application with text formatting, such as WordPad.</span></span> <span data-ttu-id="70315-157">Använd hello anropa resultatet koder tootest och felsöka hello implementering av Multi-Factor Authentication i ditt program.</span><span class="sxs-lookup"><span data-stu-id="70315-157">Use hello call result codes tootest and troubleshoot hello implementation of Multi-Factor Authentication in your application.</span></span> <span data-ttu-id="70315-158">De är inte statuskoder för autentisering.</span><span class="sxs-lookup"><span data-stu-id="70315-158">They are not authentication status codes.</span></span>
* <span data-ttu-id="70315-159">**Exempel.**</span><span class="sxs-lookup"><span data-stu-id="70315-159">**Examples.**</span></span> <span data-ttu-id="70315-160">Exempelkod för en grundläggande fungerande implementeringen av Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="70315-160">Sample code for a basic working implementation of Multi-Factor Authentication.</span></span>

> [!WARNING]
> <span data-ttu-id="70315-161">hello klientcertifikat är ett unikt privat certifikat som har genererats särskilt för dig.</span><span class="sxs-lookup"><span data-stu-id="70315-161">hello client certificate is a unique private certificate that was generated especially for you.</span></span> <span data-ttu-id="70315-162">Dela eller inte förlorar den här filen.</span><span class="sxs-lookup"><span data-stu-id="70315-162">Do not share or lose this file.</span></span> <span data-ttu-id="70315-163">Det är viktiga tooensuring hello säkerheten i din kommunikation med hello Multi-Factor Authentication-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="70315-163">It’s your key tooensuring hello security of your communications with hello Multi-Factor Authentication service.</span></span>

## <a name="code-sample"></a><span data-ttu-id="70315-164">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="70315-164">Code sample</span></span>
<span data-ttu-id="70315-165">Det här kodexemplet visar hur toouse hello API: er i hello Azure Multi-Factor Authentication SDK tooadd standardläge röst anropa verifiering tooyour program.</span><span class="sxs-lookup"><span data-stu-id="70315-165">This code sample shows you how toouse hello APIs in hello Azure Multi-Factor Authentication SDK tooadd standard mode voice call verification tooyour application.</span></span> <span data-ttu-id="70315-166">Standardläge är ett telefonsamtal som hello användaren svarar tooby hello # tangenttryckning.</span><span class="sxs-lookup"><span data-stu-id="70315-166">Standard mode is a telephone call that hello user responds tooby pressing hello # key.</span></span>

<span data-ttu-id="70315-167">Det här exemplet använder hello C# .NET 2.0 Multi-Factor Authentication SDK i ett grundläggande ASP.NET-program med C# serversidan logik men hello processen påminner om på andra språk.</span><span class="sxs-lookup"><span data-stu-id="70315-167">This example uses hello C# .NET 2.0 Multi-Factor Authentication SDK in a basic ASP.NET application with C# server-side logic, but hello process is similar in other languages.</span></span> <span data-ttu-id="70315-168">Eftersom hello SDK innehåller källfiler, inte körbara filer, kan skapa hello filer och referera till dem eller lägga till dem direkt i ditt program.</span><span class="sxs-lookup"><span data-stu-id="70315-168">Because hello SDK includes source files, not executable files, you can build hello files and reference them or include them directly in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="70315-169">När du implementerar Multifaktorautentisering använda hello ytterligare metoder (telefonsamtal eller SMS) som sekundär eller tertiär verifiering toosupplement primära autentiseringsmetod (användarnamn och lösenord).</span><span class="sxs-lookup"><span data-stu-id="70315-169">When implementing Multi-Factor Authentication, use hello additional methods (phone call or text message) as secondary or tertiary verification toosupplement your primary authentication method (username and password).</span></span> <span data-ttu-id="70315-170">Dessa metoder är inte avsedda som primära autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="70315-170">These methods are not designed as primary authentication methods.</span></span>

### <a name="code-sample-overview"></a><span data-ttu-id="70315-171">Översikt över kod-exempel</span><span class="sxs-lookup"><span data-stu-id="70315-171">Code Sample Overview</span></span>
<span data-ttu-id="70315-172">Den här exempelkoden för en enkel demo-webbapp använder ett telefonsamtal med en # tangenterna tooverify hello användarautentiseringen.</span><span class="sxs-lookup"><span data-stu-id="70315-172">This sample code for a simple web demo application uses a telephone call with a # key response tooverify hello user's authentication.</span></span> <span data-ttu-id="70315-173">Denna telefonsamtal faktor kallas i Multi-Factor Authentication standardläge.</span><span class="sxs-lookup"><span data-stu-id="70315-173">This telephone call factor is known in Multi-Factor Authentication as standard mode.</span></span>

<span data-ttu-id="70315-174">hello klientkod innehåller inte några Multi-Factor Authentication-specifika element.</span><span class="sxs-lookup"><span data-stu-id="70315-174">hello client-side code does not include any Multi-Factor Authentication-specific elements.</span></span> <span data-ttu-id="70315-175">Eftersom hello ytterligare autentiseringsfaktorer är oberoende av hello primär autentisering, du kan lägga till dem utan att ändra hello befintligt inloggning gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="70315-175">Because hello additional authentication factors are independent of hello primary authentication, you can add them without changing hello existing sign-on interface.</span></span> <span data-ttu-id="70315-176">hello API: er i hello Multi-Factor SDK kan du anpassa hello användarupplevelse, men du kanske inte behöver toochange något alls.</span><span class="sxs-lookup"><span data-stu-id="70315-176">hello APIs in hello Multi-Factor SDK let you customize hello user experience, but you might not need toochange anything at all.</span></span>

<span data-ttu-id="70315-177">hello serverkod lägger till standard-autentisering i steg 2.</span><span class="sxs-lookup"><span data-stu-id="70315-177">hello server-side code adds standard-mode authentication in Step 2.</span></span> <span data-ttu-id="70315-178">Den skapar ett PfAuthParams-objekt med hello-parametrar som krävs för verifiering i standard-läge: användarnamn, telefon antal och läge och hello sökvägen toohello klientcertifikat (CertFilePath) som krävs i varje anrop.</span><span class="sxs-lookup"><span data-stu-id="70315-178">It creates a PfAuthParams object with hello parameters that are required for standard-mode verification: username, telephone number, and mode, and hello path toohello client certificate (CertFilePath), which is required in each call.</span></span> <span data-ttu-id="70315-179">En demonstration av alla parametrar i PfAuthParams finns hello exempelfil i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="70315-179">For a demonstration of all parameters in PfAuthParams, see hello Example file in hello SDK.</span></span>

<span data-ttu-id="70315-180">Därefter skickar hello kod hello PfAuthParams toohello pf_authenticate() med den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="70315-180">Next, hello code passes hello PfAuthParams object toohello pf_authenticate() function.</span></span> <span data-ttu-id="70315-181">hello returvärdet anger hello lyckad eller misslyckad hello-autentisering.</span><span class="sxs-lookup"><span data-stu-id="70315-181">hello return value indicates hello success or failure of hello authentication.</span></span> <span data-ttu-id="70315-182">Hej parametrar, callStatus och samtalsstatus, innehålla ytterligare information om resultatet.</span><span class="sxs-lookup"><span data-stu-id="70315-182">hello out parameters, callStatus, and errorID, contain additional call result information.</span></span> <span data-ttu-id="70315-183">hello resultatkoder för webbtjänstanrop dokumenteras i hello anropet resultatfilen i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="70315-183">hello call result codes are documented in hello call results file in hello SDK.</span></span>

<span data-ttu-id="70315-184">Den här minimal implementeringen kan skrivas i några rader.</span><span class="sxs-lookup"><span data-stu-id="70315-184">This minimal implementation can be written in a few lines.</span></span> <span data-ttu-id="70315-185">I produktionskod, skulle du dock inkludera mer felhantering, ytterligare databaskod och en förbättrad användarupplevelse.</span><span class="sxs-lookup"><span data-stu-id="70315-185">However, in production code, you would include more sophisticated error handling, additional database code, and an enhanced user experience.</span></span>

### <a name="web-client-code"></a><span data-ttu-id="70315-186">Web klientkod</span><span class="sxs-lookup"><span data-stu-id="70315-186">Web Client Code</span></span>
<span data-ttu-id="70315-187">hello följer web klientkod för en Demonstrationssida.</span><span class="sxs-lookup"><span data-stu-id="70315-187">hello following is web client code for a demo page.</span></span>

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a><span data-ttu-id="70315-188">Kod på serversidan</span><span class="sxs-lookup"><span data-stu-id="70315-188">Server-Side Code</span></span>
<span data-ttu-id="70315-189">I följande serverkod hello, har Multi-Factor Authentication konfigurerats och kör i steg 2.</span><span class="sxs-lookup"><span data-stu-id="70315-189">In hello following server-side code, Multi-Factor Authentication is configured and run in Step 2.</span></span> <span data-ttu-id="70315-190">Standardläge (MODE_STANDARD) är ett telefonsamtal toowhich hello användaren svarar genom att trycka på fyrkant hello.</span><span class="sxs-lookup"><span data-stu-id="70315-190">Standard mode (MODE_STANDARD) is a telephone call toowhich hello user responds by pressing hello # key.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate hello username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from hello user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains hello private key for hello client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
