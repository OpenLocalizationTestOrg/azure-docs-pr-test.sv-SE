---
title: aaaAdd en SSL-certifikatet tooyour Azure App Service-appen | Microsoft Docs
description: "Lär dig hur tooadd en SSL-certifikatet tooyour Apptjänst-app."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: f4652794ba745790a073264f6a102c64c73e8db0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Köp och konfigurera ett SSL-certifikat för din Azure Apptjänst

I den här självstudiekursen kommer du skyddar ditt webbprogram genom att köpa ett SSL-certifikat för din  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, på ett säkert sätt lagra det i [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), och associera den med en anpassad domän.

## <a name="step-1---log-in-tooazure"></a>Steg 1 – Logga in tooAzure

Logga in toohello Azure-portalen på http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Steg 2 – beställning SSL-certifikat

Du kan placera en order för SSL-certifikat genom att skapa en ny [App tjänstcertifikat](https://portal.azure.com/#create/Microsoft.SSL) i hello **Azure-portalen**.

![Skapande av certifikat](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Ange ett eget **namn** för SSL-certifikatet och ange hello **domännamn**

> [!NOTE]
> Detta är en hello viktigaste delarna i hello köpet. Se till att tooenter korrigera värdnamn (anpassad domän) som du vill tooprotect med det här certifikatet. **INTE** bifoga hello värdnamn med WWW. 
>

Välj din **prenumeration**, **resursgruppen**, och **certifikat SKU**

> [!WARNING]
> Apptjänstcertifikat kan endast användas av andra App-tjänster inom hello samma prenumeration.  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a>Steg 3 – Store hello certifikat i Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) är en Azure-tjänst som hjälper dig skydda krypteringsnycklar och hemligheter som används av molnprogram och tjänster.
>

När hello SSL-certifikat köpet har slutförts måste tooopen [Apptjänstcertifikat](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) resursbladet.

![Infoga en bild av redo toostore i KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Du kommer att märka att certifikatstatus är **”väntande utfärdande”** eftersom det inte finns några fler steg du måste toocomplete innan du kan börja använda det här certifikatet.

Klicka på **Certifikatkonfigureringen** inuti certifikategenskaper bladet och klicka på **steg 1: Store** toostore det här certifikatet i Azure Key Vault.

Från **Key Vault Status** bladet, klickar du på **Key Vault databasen** toochoose en befintlig Key Vault-toostore certifikatet **eller skapa nya Key Vault** toocreate ny nyckel valvet inom samma prenumeration och resurs.

> [!NOTE]
> Azure Key Vault har minimal kostnader för lagring av det här certifikatet.
> Mer information finns i  **[prisinformation för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

När du har valt det här certifikatet i hello Key Vault databasen toostore, hello **Store** alternativet ska visa lyckades.

![infoga bilden av store KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a>Steg 4 – Kontrollera hello ägarskap för domänen

> [!NOTE]
> Det finns tre typer av domänverifiering som stöds av Apptjänstcertifikat: verifiering av domän, e-post manuellt. Dessa beskrivs mer detaljerat i hello [avancerade avsnitt](#advanced).

Hej från samma **Certifikatkonfigureringen** bladet som du använde i steg3 klickar du på **steg 2: Kontrollera**.

**Verifiering av domän** det här är hello enklaste process **endast om** du har  **[köpt din anpassade domän från Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**
Klicka på **Kontrollera** knappen toocomplete det här steget.

![infoga bilden för verifiering av domän](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

När du klickar på **Kontrollera**, använda hello **uppdatera** knappen tills hello **Kontrollera** alternativet ska visa lyckades.

![Infoga bild av verifiera i KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a>Steg 5 – tilldela certifikat tooApp Service-appen

> [!NOTE]
> Innan du utför hello stegen i det här avsnittet måste du har associerat ett anpassat domännamn med din app. Mer information finns i  **[konfigurera ett anpassat domännamn för en webbapp.](app-service-web-tutorial-custom-domain.md)**
>

I hello  **[Azure-portalen](https://portal.azure.com/)**, klicka på hello **Apptjänst** alternativet hello vänster hello-sidan.

Hello klickar du på din app toowhich som du vill tooassign det här certifikatet.

I hello **inställningar**, klickar du på **SSL-certifikat**.

Klicka på **importera App Service certifikat** och välj hello-certifikat som du precis har köpt.

![Infoga bild av Importera certifikat](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

I hello **ssl-bindningar** avsnittet Klicka på **lägga till bindningar**, och använda hello nedrullningsbara listorna tooselect hello domain name toosecure med SSL och hello certifikat toouse. Du kan också välja om toouse  **[Server Servernamnsindikation (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  eller IP-baserade SSL.

![infoga bilden av SSL-bindningar](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klicka på **Lägg till bindning för** toosave hello ändringar och aktivera SSL.

> [!NOTE]
> Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post, måste du utföra följande ytterligare hello. Dessa beskrivs mer detaljerat i hello [avancerade avsnitt](#Advanced).

Nu är du ska kunna toovisit din app med hjälp av `HTTPS://` i stället för `HTTP://` tooverify som hello certifikat har konfigurerats korrekt.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Steg 6 - administrativa uppgifter

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Advanced

### <a name="verifying-domain-ownership"></a>Verifiera domänen ägarskap

Det finns två flera typer av domänverifiering som stöds av Apptjänstcertifikat: e-post och manuell kontroll.

#### <a name="mail-verification"></a>Verifiering av e-post

E-postmeddelandet har redan skickats toohello e-adresser som är associerade med den här domänen.
Verifieringssteg toocomplete hello e-post, öppna hello e-postmeddelande och klicka på hello verifieringslänken.

![Infoga bild av e-Postverifiering](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Om du behöver tooresend hello e-postmeddelandet klickar du på hello **skicka e-post** knappen.

#### <a name="manual-verification"></a>Manuell kontroll

> [!IMPORTANT]
> HTML-webbsida verifiering (fungerar bara med certifikat SKU: N)
>

1. Skapa en HTML-fil med namnet **”starfield.html”**

1. Innehållet för den här filen ska vara hello exakta namnet på domänen verifiering Token hello. (Du kan kopiera hello-token från hello domän verifiering Status bladet)

1. Överföra filen hello roten i hello webbservern som värd för din domän`/.well-known/pki-validation/starfield.html`

1. Klicka på **uppdatera** tooupdate hello certifikatstatus när verifieringen är klar. Det kan ta några minuter för verifiering toocomplete.

> [!TIP]
> Kontrollera i en terminal `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello svaret ska innehålla hello `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Verifiering av DNS TXT-post

1. Med hjälp av DNS-hanteraren, skapa en TXT-post på hello `@` underdomänen med värde lika toohello domän verifiering Token.
1. Klicka på **”uppdatera”** tooupdate hello certifikatstatus när verifieringen är klar.

> [!TIP]
> Du behöver toocreate en TXT-post på `@.<domain>` med värdet `<verification-token>`.

### <a name="assign-certificate-tooapp-service-app"></a>Tilldela certifikat tooApp Service-appen

Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post, måste du utföra följande ytterligare hello:

När du har konfigurerat en IP-baserade SSL-bindning, en dedicerad IP-adress tilldelas tooyour app. Du hittar den här IP-adressen på hello **anpassad domän** sidan under inställningar för din app, precis ovanför hello **värdnamn** avsnitt. Den visas som **extern IP-adress**

![infoga bilden av IP-SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Observera att denna IP-adress skiljer sig från hello virtuella IP-adressen används tidigare tooconfigure hello A-posten för din domän. Om du konfigurerat toouse SNI baserad SSL, eller är inte konfigurerad toouse SSL, ingen adress visas för den här posten.

Använda hello verktyg som tillhandahålls av din domännamnsregistrator kan ändra hello en post för din domän namn toopoint toohello IP-adress från hello föregående steg.

## <a name="rekey-and-sync-hello-certificate"></a>Genererar nya nycklar och synkronisera hello certifikat

Om du någon gång behöver tooRekey ditt certifikat markerar **genererar nya nycklar och synkronisera** alternativet från **certifikategenskaper** bladet.

Klicka på **genererar nya nycklar** knappen tooinitiate hello processen. Den här processen kan ta 1-10 minuter toocomplete.

![infoga bilden av nyckelförnyelse SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Omgenerering ditt certifikat samlar hello certifikat med ett nytt certifikat utfärdade av certifikatutfärdaren hello.

## <a name="next-steps"></a>Nästa steg

* [Lägg till ett nätverk för Innehållsleverans](app-service-web-tutorial-content-delivery-network.md)