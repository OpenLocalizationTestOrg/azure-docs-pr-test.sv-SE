---
title: "aaaHow tooConfigure TLS ömsesidig autentisering för webbprogram"
description: "Lär dig hur tooconfigure web app toouse klienten certifikatautentisering på TLS."
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.openlocfilehash: 8aeb9b35058fac50b8b38f6428207ad4a82d8637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-tls-mutual-authentication-for-web-app"></a>Hur tooConfigure TLS ömsesidig autentisering för webbprogram
## <a name="overview"></a>Översikt
Du kan begränsa åtkomst tooyour Azure webbapp genom att aktivera olika typer av autentisering för den. Enkelriktade toodo är därför tooauthenticate använder ett klientcertifikat när hello-begäran är över TLS/SSL. Den här mekanismen kallas TLS ömsesidig autentisering eller autentisering av klientcertifikat och den här artikeln beskriver detaljerat hur toosetup din web app toouse förautentisering av klientcertifikat.

> **Obs:** om du har åtkomst till webbplatsen via HTTP och HTTPS inte visas inte alla klientcertifikat. Så om ditt program kräver klientcertifikat bör du inte tillåta begäranden tooyour program via HTTP.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Konfigurera Webbappen för autentisering av klientcertifikat
toosetup din web app toorequire klientcertifikat behöver du tooadd hello clientCertEnabled plats för ditt webbprogram och ange den tootrue. Den här inställningen är inte tillgänglig via hello hanteringen i hello Portal och hello REST API behöver toobe används tooaccomplish detta.

Du kan använda hello [ARMClient verktyget](https://github.com/projectkudu/ARMClient) toomake den enkelt toocraft hello REST API-anrop. När du har loggat in med hello verktyget måste tooissue hello följande kommando:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

Ersätt allt i {} med information för ditt webbprogram och skapa en fil med namnet enableclientcert.json med hello följande JSON innehåll:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Kontrollera att toochange hello värdet för ”plats” toowherever ditt webbprogram är finns t.ex. norra centrala USA eller västra USA osv.

Du kan också använda https://resources.azure.com tooflip hello `clientCertEnabled` egenskapen för`true`.

> **Obs:** om du kör ARMClient från Powershell, behöver du tooescape hello @-tecknet för hello JSON-fil med en bakre skalstreck '.
> 
> 

## <a name="accessing-hello-client-certificate-from-your-web-app"></a>Åtkomst till hello klienten certifikat från ditt webbprogram
Om du använder ASP.NET och konfigurera din app toouse autentisering av klientcertifikat hello certifikat kommer att vara tillgängliga via hello **HttpRequest.ClientCertificate** egenskapen. Andra program-stackar ska hello klienten certifikat vara tillgänglig i appen genom en base64-kodad värdet i huvudet i begäran är hello ”X-ARR-ClientCert”. Ditt program kan skapa ett certifikat från det här värdet och sedan använda datorn för autentisering och auktorisering ändamål i ditt program.

## <a name="special-considerations-for-certificate-validation"></a>Speciella överväganden vid valideringen av servercertifikatet
hello-klientcertifikat som skickas toohello program går inte via en verifiering av hello Azure Web Apps-plattformen. Verifiera det här certifikatet är hello ansvar hello webbprogrammet. Här är exempelkod för ASP.NET som validerar egenskaper för certifikat för autentisering.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read hello certificate from hello header into an X509Certificate2 object
            // Display properties of hello certificate on hello page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept hello certificate as a valid certificate if all hello conditions below are met:
                // 1. hello certificate is not expired and is active for hello current time on server.
                // 2. hello subject name of hello certificate has hello common name nildevecc
                // 3. hello issuer name of hello certificate has hello common name nildevecc and organization name Microsoft Corp
                // 4. hello thumbprint of hello certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained tooa Trusted Root Authority (or revoked) on hello server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want tootest if hello certificate chains tooa Trusted Root Authority you can uncomment hello code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
