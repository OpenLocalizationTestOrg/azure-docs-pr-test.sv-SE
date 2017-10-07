---
title: aaaUsing certifikat med Enterprise-Integrationspaket | Microsoft Docs
description: "Lär dig hur toouse certifikat med hello Enterprise-Integrationspaket | Azure Logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Läs om certifikat och Enterprise-integreringspaket
## <a name="overview"></a>Översikt
Enterprise integration använder certifikat toosecure B2B-kommunikation. Du kan använda två typer av certifikat i dina enterprise integration-appar:

* Offentliga certifikat, som måste köpas från en certifikatutfärdare (CA).
* Privata certifikat kan du utfärda själv. Dessa certifikat kan ibland hänvisade tooas självsignerade certifikat.

## <a name="what-are-certificates"></a>Vad är certifikat?
Certifikat är digitalt dokument som verifierar hello hello deltagare i elektronisk kommunikation och som även säker elektronisk kommunikation.

## <a name="why-use-certificates"></a>Varför använda certifikat?
Ibland måste B2B-kommunikation vara konfidentiella. Enterprise integration använder certifikat toosecure dessa meddelanden på två sätt:

* Genom att kryptera hello innehållet i meddelanden
* Genom att signera meddelanden  

## <a name="upload-a-public-certificate"></a>Ladda upp ett offentligt certifikat

toouse en *certifikat med offentlig* i dina logic apps med B2B-funktioner, måste du först tooupload hello certifikatet till ditt konto för integrering.  

När du har överfört ett certifikat är det tillgängliga toohelp som du skyddar dina B2B-meddelanden när du definierar deras egenskaper i hello [avtal](logic-apps-enterprise-integration-agreements.md) som du skapar.  

Här följer hello detaljerade anvisningar för uppladdning av din offentliga certifikat till kontot integration när du loggar in toohello Azure-portalen:

1. Välj **fler tjänster** och ange **integrering** i sökrutan för hello filter. Välj **Integrationskonton** från hello resultatlistan     
![Välj Bläddra](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Välj hello integration konto toowhich du vill ha tooadd hello certifikat.  
![Välj hello integration konto toowhich du vill ha tooadd hello certifikat](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Välj hello **certifikat** panelen.  
![Välj hello certifikat panelen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. I hello **certifikat** bladet som öppnas väljer hello **Lägg till** knappen.   
![Välj knappen hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Ange en **namn** för certifikatet och välj sedan hello certifikattyp som **offentliga** hello listrutan.  
6. Välj hello mappikon hello höger på hello **certifikat** textruta. När hello filväljaren öppnas, söka efter och väljer hello certifikatfil som du vill tooupload tooyour integrering konto.
7. Välj hello certifikatet och välj sedan **OK** i hello filväljaren. Detta verifierar och laddar upp hello certifikat tooyour integrering konto.
8. Slutligen tillbaka på hello **Lägg till certifikat** bladet, Välj hello **OK** knappen.  
![Välj hello OK-knapp](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Välj hello **certifikat** panelen. Du bör se hello nyligen tillagda certifikatet.  
![Se hello nya certifikat](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Ladda upp ett privat certifikat

toouse en *privat certifikat* i dina logic apps med B2B-funktioner, kan du överföra ett privat certifikat tooyour integrering konto av tar hello följande

1. [Ladda upp din privata nyckel tooKey valvet](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault") och ger en **nyckelnamn** 
   
   > [!TIP]
   > Du måste auktorisera Logic Apps tooperform åtgärder på Nyckelvalvet. Du kan bevilja åtkomst toohello Logic Apps tjänstens huvudnamn med hjälp av hello följande PowerShell-kommando:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Lägg till ett privat certifikat toointegration konto när du har gjort hello föregående steg.

Följande är hello detaljerade anvisningar för uppladdning av dina privata certifikat till kontot integration när du loggar in toohello Azure-portalen:  
 
1. Välj hello integration konto toowhich tooadd hello certifikat och markera hello **certifikat** panelen.  
![Välj hello certifikat panelen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. I hello **certifikat** bladet som öppnas väljer hello **Lägg till** knappen.   
![Välj knappen hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Ange en **namn** för certifikatet och välj hello certifikattyp som **privata** hello listrutan.   
4. Välj hello mappikon hello höger på hello **certifikat** textruta. Hitta hello motsvarande offentliga certifikat som du vill tooupload tooyour integrering konto när hello filväljaren öppnas.   
   
   > [!Note]
   > När du lägger till ett privat certifikat är det viktigt tooadd motsvarande offentliga certifikat tooshow i [AS2-avtal](logic-apps-enterprise-integration-as2.md) ta emot och skicka inställningarna för signering och kryptering hälsningsmeddelande.
   > 
   >   

5. Välj hello **resursgruppen**, **Nyckelvalvet**, **nyckelnamn** och välj hello **OK** knappen.  
![Lägg till certifikat](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Välj hello **certifikat** panelen. Du bör se hello nyligen tillagda certifikatet.
![Se hello nya certifikat](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Skapa en B2B-avtal](logic-apps-enterprise-integration-agreements.md)  
* [Mer information om Key Vault](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault")  

