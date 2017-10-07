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
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Skapa Multifaktorautentisering i anpassade appar (SDK)

hello Azure Multi-Factor Authentication Software Development Kit (SDK) kan du skapa tvåstegsverifiering direkt i hello inloggning eller transaktionen bearbetar för program i Azure AD-klienten.

Hej Multi-Factor Authentication SDK är tillgängligt för C#, Visual Basic (.NET), Java, Perl, PHP och Ruby. hello SDK innehåller en tunn wrapper runt tvåstegsverifiering. Den innehåller allt du behöver toowrite din kod, inklusive kommenterad källkodsfiler, exempel filer och en detaljerad viktigt-fil. Varje SDK innehåller också ett certifikat och privat nyckel för att kryptera transaktioner som är unikt tooyour Flerfunktionsautentiseringsleverantören. Så länge som du har en provider, kan du hämta hello SDK i så många språk och format som du behöver.

hello strukturen för hello API: er i hello Multi-Factor Authentication SDK är enkelt. Gör en enskild funktion anropa tooan API hello Multi-Factor alternativet parametrar (t.ex. verifieringen läge) och användardata (till exempel hello telefon nummer toocall eller antalet toovalidate för hello PIN-kod). hello API: er översätta hello funktionsanrop i web services begäranden toohello molnbaserad Azure Multi-Factor Authentication-tjänsten. Alla anrop måste innehålla en referens toohello privat certifikat som ingår i varje SDK.

Eftersom hello API: er inte har åtkomst toousers registreras i Azure Active Directory, måste du ange användarinformation i en fil eller databas. Dessutom hello API: er ger inte funktioner för registrering eller användare, så du måste toobuild dessa processer i ditt program.

> [!IMPORTANT]
> toodownload hello SDK måste du toocreate ett Azure leverantör av Multifaktorautent även om du har licenser för Azure MFA eller AAD Premium EMS. Om du skapar ett Azure leverantör av Multifaktorautent för detta ändamål och redan har licenser, göra att toocreate hello providern med hello **Per aktiverad användare** modell. Länka hello providern toohello katalog som innehåller hello Azure MFA, Azure AD Premium eller EMS-licenser. Den här konfigurationen garanterar att du endast debiteras om du har flera unika användare som använder hello SDK än hello antalet licenser du äger.


## <a name="download-hello-sdk"></a>Hämta hello SDK
Hämta hello Azure Multi-Factor SDK kräver en [Azure leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md).  Detta kräver en fullständig Azure-prenumeration, även om Azure MFA, Azure AD Premium eller Enterprise Mobility Suite licenser äger.  toodownload hello SDK, navigera toohello Multi-Factor hanteringsportalen. Du kan nå hello portal genom att hantera hello leverantör av Multifaktorautent direkt eller genom att klicka på hello **”gå toohello portal”** länk på inställningssidan för hello MFA-tjänsten.

### <a name="download-from-hello-azure-classic-portal"></a>Ladda ned från hello klassiska Azure-portalen
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.
2. Hello vänster markerar **Active Directory**.
3. På hello Active Directory-sidan vid hello översta Välj **Flerfunktionsautentiseringsleverantörer**
4. Längst ned hello Välj **hantera**. En ny sida öppnas.
5. Klicka på hello kvar hello längst ned i **SDK**.
   <center>![Ladda ned](./media/multi-factor-authentication-sdk/download.png)</center>
6. Välj hello språk du vill använda och klicka på en hello associerade länkar.
7. Spara hello hämtning.

### <a name="download-from-hello-service-settings"></a>Ladda ned från hello tjänstinställningar
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.
2. Hello vänster markerar **Active Directory**.
3. Dubbelklicka på din instans av Azure AD.
4. Vid hello översta Klicka **konfigurera**
5. Välj under multifaktorautentisering, **hantera tjänstinställningar**
   ![hämta](./media/multi-factor-authentication-sdk/download2.png)
6. Hello tjänster på sidan Inställningar hello ned hello-skärmen klickar du på **gå toohello portal**. En ny sida öppnas.
   ![Ladda ned](./media/multi-factor-authentication-sdk/download3a.png)
7. Klicka på hello kvar hello längst ned i **SDK**.
8. Välj hello språk du vill använda och klicka på en hello associerade länkar.
9. Spara hello hämtning.

## <a name="whats-in-hello-sdk"></a>Vad är i hello SDK
hello SDK innehåller hello följande objekt:

* **VIKTIGT**. Förklarar hur toouse hello Multi-Factor Authentication-API: er i ett nytt eller befintligt program.
* **Källfiler** för Multifaktorautentisering
* **Klientcertifikatet** att du använder toocommunicate med hello Multi-Factor Authentication-tjänsten
* **Privat nyckel** för hello certifikat
* **Anropa resultat.** En lista över resultatkoder för webbtjänstanrop. tooopen den här filen, Använd ett program med textformatering, till exempel WordPad. Använd hello anropa resultatet koder tootest och felsöka hello implementering av Multi-Factor Authentication i ditt program. De är inte statuskoder för autentisering.
* **Exempel.** Exempelkod för en grundläggande fungerande implementeringen av Multi-Factor Authentication.

> [!WARNING]
> hello klientcertifikat är ett unikt privat certifikat som har genererats särskilt för dig. Dela eller inte förlorar den här filen. Det är viktiga tooensuring hello säkerheten i din kommunikation med hello Multi-Factor Authentication-tjänsten.

## <a name="code-sample"></a>Kodexempel
Det här kodexemplet visar hur toouse hello API: er i hello Azure Multi-Factor Authentication SDK tooadd standardläge röst anropa verifiering tooyour program. Standardläge är ett telefonsamtal som hello användaren svarar tooby hello # tangenttryckning.

Det här exemplet använder hello C# .NET 2.0 Multi-Factor Authentication SDK i ett grundläggande ASP.NET-program med C# serversidan logik men hello processen påminner om på andra språk. Eftersom hello SDK innehåller källfiler, inte körbara filer, kan skapa hello filer och referera till dem eller lägga till dem direkt i ditt program.

> [!NOTE]
> När du implementerar Multifaktorautentisering använda hello ytterligare metoder (telefonsamtal eller SMS) som sekundär eller tertiär verifiering toosupplement primära autentiseringsmetod (användarnamn och lösenord). Dessa metoder är inte avsedda som primära autentiseringsmetoder.

### <a name="code-sample-overview"></a>Översikt över kod-exempel
Den här exempelkoden för en enkel demo-webbapp använder ett telefonsamtal med en # tangenterna tooverify hello användarautentiseringen. Denna telefonsamtal faktor kallas i Multi-Factor Authentication standardläge.

hello klientkod innehåller inte några Multi-Factor Authentication-specifika element. Eftersom hello ytterligare autentiseringsfaktorer är oberoende av hello primär autentisering, du kan lägga till dem utan att ändra hello befintligt inloggning gränssnitt. hello API: er i hello Multi-Factor SDK kan du anpassa hello användarupplevelse, men du kanske inte behöver toochange något alls.

hello serverkod lägger till standard-autentisering i steg 2. Den skapar ett PfAuthParams-objekt med hello-parametrar som krävs för verifiering i standard-läge: användarnamn, telefon antal och läge och hello sökvägen toohello klientcertifikat (CertFilePath) som krävs i varje anrop. En demonstration av alla parametrar i PfAuthParams finns hello exempelfil i hello SDK.

Därefter skickar hello kod hello PfAuthParams toohello pf_authenticate() med den här funktionen. hello returvärdet anger hello lyckad eller misslyckad hello-autentisering. Hej parametrar, callStatus och samtalsstatus, innehålla ytterligare information om resultatet. hello resultatkoder för webbtjänstanrop dokumenteras i hello anropet resultatfilen i hello SDK.

Den här minimal implementeringen kan skrivas i några rader. I produktionskod, skulle du dock inkludera mer felhantering, ytterligare databaskod och en förbättrad användarupplevelse.

### <a name="web-client-code"></a>Web klientkod
hello följer web klientkod för en Demonstrationssida.

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


### <a name="server-side-code"></a>Kod på serversidan
I följande serverkod hello, har Multi-Factor Authentication konfigurerats och kör i steg 2. Standardläge (MODE_STANDARD) är ett telefonsamtal toowhich hello användaren svarar genom att trycka på fyrkant hello.

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
