---
title: "aaaManage Azure nyckeln valvet med hjälp av CLI | Microsoft Docs"
description: "Använd den här självstudiekursen tooautomate vanliga uppgifter i Nyckelvalvet med hjälp av hello CLI 2.0"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Hantera Nyckelvalv med hjälp av CLI 2.0
Azure Key Vault är tillgängligt i de flesta regioner. Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Introduktion
Använd den här självstudiekursen toohelp du få igång med Azure Key Vault toocreate en förstärkt behållare (ett valv) i Azure, toostore och hantera krypteringsnycklar och hemligheter i Azure. Den vägleder dig genom hello processen med att använda Azure plattformsoberoende kommandoradsgränssnittet toocreate ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program. Den sedan visar hur ett program kan sedan använda denna nyckel eller lösenord.

**Uppskattad tid toocomplete:** 20 minuter

> [!NOTE]
> Den här självstudiekursen innehåller inte instruktioner för hur toowrite hello Azure-program med något av hello steg, som visar hur tooauthorize ett program toouse en nyckel eller hemlighet i hello nyckeln valvet.
>
> Den här självstudiekursen använder hello senaste Azure CLI 2.0.
>
>

Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Krav
toocomplete den här självstudien måste du ha hello följande:

* En prenumeration tooMicrosoft Azure. Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).
* Kommandoradsgränssnitt version 2.0 eller senare. tooinstall hello senaste versionen och ansluta tooyour Azure-prenumeration, se [installera och konfigurera hello Azure plattformsoberoende kommandoradsgränssnittet 2.0](/cli/azure/install-azure-cli).
* Ett program som är konfigurerade toouse hello nyckel eller lösenord som du skapar i den här kursen. Ett exempelprogram som är tillgänglig från hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Instruktioner finns i hello tillhörande Readme-filen.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Få hjälp med Azure plattformsoberoende-kommandoradsgränssnittet
Den här kursen förutsätter att du är bekant med hello-kommandoradsgränssnittet (Bash, Terminal, Kommandotolken)

hello--hjälp eller -h kan vara används tooview hjälpen för specifika kommandon. Alternativt kan hello hello azure hjälp [kommando] [alternativ] format kan också använda tooreturn samma information. Till exempel hello följande kommandon för alla returnera hello samma information:

```
az account set --help
az account set -h
```

När osäkra om hello-parametrar som krävs av ett kommando, se toohelp med--hjälp, -h eller az help [kommando].

Du kan också läsa följande kurser tooget bekant med Azure Resource Manager i Azure plattformsoberoende-kommandoradsgränssnittet hello:

* [Installera Azure CLI](/cli/azure/install-azure-cli)
* [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a>Ansluta tooyour prenumerationer
toolog in med ett organisationskonto, Använd hello följande kommando:

```
az login -u username@domain.com -p password
```

eller om du vill toolog i genom att skriva interaktivt

```
az login
```

Om du har flera prenumerationer och vill toospecify en specifik en toouse för Azure Key Vault, skriver du hello följande toosee hello prenumerationer för ditt konto:

```
az account list
```

Sedan toospecify hello prenumeration toouse, typ:

```
az account set --subscription <subscription name or ID>
```

Mer information om hur du konfigurerar Azure plattformsoberoende kommandoradsgränssnittet finns [installera Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Skapa en ny resursgrupp
När du använder Azure Resource Manager, skapas alla relaterade resurser i en resursgrupp. Vi skapar en ny resursgrupp 'ContosoResourceGroup' för den här självstudiekursen.

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

hello första parameter resursgruppens namn och andra hello-parametern är hello plats. För plats kommandot hello `az account list-locations` tooidentify hur toospecify en alternativ plats toohello något i det här exemplet. Om du behöver mer information skriver du: `az account list-locations -h`.

## <a name="register-hello-key-vault-resource-provider"></a>Registerresursleverantören hello Key Vault
Kontrollera att Nyckelvalvet resursprovidern har registrerats i din prenumeration:

```
az provider register -n Microsoft.KeyVault
```

På så sätt behöver bara toobe gjort en gång per prenumeration.

## <a name="create-a-key-vault"></a>Skapa ett nyckelvalv
Använd hello `az keyvault create` kommandot toocreate en nyckelvalvet. Det här skriptet har tre obligatoriska parametrar: en resursgruppens namn, ett nyckelvalv namn och hello geografisk plats.

Skriv till exempel om du använder hello valvnamnet av ContosoKeyVault hello resursgruppens namn för ContosoResourceGroup och hello placeringen av Östasien:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

hello kommandots utdata visar egenskaperna för hello nyckelvalv som du nyss skapade. hello två viktigaste egenskaper är:

* **namnet**: I hello exempel är ContosoKeyVault. Du använder det här namnet för andra Key Vault-kommandon.
* **vaultUri**: I hello exempel är https://contosokeyvault.vault.azure.net. Program som använder ditt valv via dess REST-API måste använda denna URI.

Azure-konto är nu behörig tooperform några åtgärder på den här nyckeln valvet. Vilket ingen annan har ännu.

## <a name="add-a-key-or-secret-toohello-key-vault"></a>Lägga till en nyckel eller Hemlig toohello nyckelvalv
Om du vill Azure Key Vault toocreate programvaruskyddad nyckel som du kan använda hello `az key create` kommando och skriver hello följande:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Om du har en befintlig nyckel i en PEM-filen sparas som en lokal fil i en fil med namnet softkey.pem som du vill tooupload tooAzure Key Vault skriver du följande tooimport hello nyckeln från hello hello. PEM-filen som skyddar hello nyckeln av programvara i hello Key Vault-tjänsten:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
Nu kan du referera hello nyckel som du har skapat eller överföra tooAzure Key Vault med hjälp av dess URI. Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hämta hello aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget den här specifika versionen.

tooadd en hemlig toohello valvet, som är ett lösenord som heter SQLPassword och som har hello värdet för Pa$ w0rd tooAzure Key Vault typen hello följande:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Nu kan du referera lösenordet du lagt till tooAzure Key Vault med dess URI. Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hämta hello aktuella versionen och använda **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget den här specifika versionen.

Nu ska vi visa hello nyckel eller hemlighet som du just skapat:

* tooview nyckeltypen,:`az keyvault key list --vault-name 'ContosoKeyVault'`
* tooview din hemliga, typ:`az keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Registrera ett program med Azure Active Directory
Det här steget utförs normalt av en utvecklare, på en separat dator. Det är inte specifik tooAzure Key Vault men ingår här, för fullständighetens skull.

> [!IMPORTANT]
> toocomplete hello kursen, ditt konto, hello valvet och hello-program som du registrerar i det här steget måste vara i hello samma Azure-katalogen.
>
>

Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory. toodo detta, hello ägare av programmet hello måste först registrera programmet hello i sina Azure Active Directory. Hello slutet av registrering hämtar hello programägaren hello följande värden:

* En **program-ID** (även kallat ett klient-ID) och **autentiseringsnyckel** (även kallat hello delad hemlighet). hello programmet måste visa båda dessa värden tooAzure Active Directory, tooget en token. Hur konfigureras programmet hello toodo detta beror på programmet hello. För hello Key Vault exempelprogrammet anger hello programägaren värdena i hello app.config-fil.

tooregister hello program i Azure Active Directory:

1. Logga in toohello Azure-portalen.
2. Klicka på vänster hello **Azure Active Directory**, och välj sedan hello katalog där du ska registrera ditt program. <br> <br> 

> [!Note] 
> Du måste välja hello samma katalog som innehåller hello Azure-prenumeration som du skapade nyckelvalvet. Om du inte vet vilken katalog detta är klickar du på **inställningar**, identifiera hello prenumeration som du skapade ditt nyckelvalv och Observera hello namnet på hello katalog visas i hello sista kolumnen.

3. Klicka på **Program**. Om inga appar har lagts till tooyour directory kan den här sidan visar endast hello **Lägg till en App** länk. På hello länk, eller också kan du klicka på hello **lägga till** i hello kommandofält.
4. I hello **Lägg till program** på hello **vad vill du vill toodo?** klickar du på **Lägg till ett program som min organisation utvecklar**.
5. På hello **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och välja **WEB APPLICATION och/eller webb-API** (hello standard). Klicka på nästa hello-ikonen.
6. På hello **appegenskaper** anger hello **SIGN-ON-URL** och **APP-ID URI** för webbprogram. Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna). Det spelar ingen roll om dessa webbplatser finns; Vad är viktigt är hello appen ID URI för varje program är olika för varje program i din katalog. hello directory använder denna sträng tooidentify din app.
7. Klicka på hello fullständig ikonen toosave ändringarna i hello guiden.
8. På hello Snabbstart klickar du på **konfigurera**.
9. Rulla toohello **nycklar** avsnittet väljer hello varaktighet och klicka sedan på **spara**. hello sidan uppdateras och innehåller nu ett nyckelvärde. Du måste konfigurera ditt program med det här värdet för nyckeln och hello **klient-ID** värde. (Anvisningar för den här konfigurationen är programspecifika.)
10. Kopiera hello klient-ID-värde från den här sidan som du vill använda i hello nästa steg tooset behörigheter på ditt valv.

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a>Auktorisera hello programmet toouse hello nyckel eller hemlighet.
tooauthorize hello programmet tooaccess hello nyckel eller hemlighet i hello valvet, Använd hello `az keyvault set-policy` kommando.

Till exempel om din valvnamnet är ContosoKeyVault och hello program som du vill tooauthorize har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tooauthorize hello programmet toodecrypt och logga med nycklar i ditt valv, kör hello följande:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Om du vill tooauthorize att samma program tooread hemligheter i ditt valv, kör du hello följande:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a>Om du vill toouse en maskinvarusäkerhetsmodul (HSM)
För ytterligare säkerhet kan du importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns. hello HSM är FIPS 140-2 Level 2-verifierade. Om det här kravet inte gäller tooyou, hoppa över det här avsnittet och gå för[ta bort hello nyckelvalvet och associerade nycklar och hemligheter](#delete-the-key-vault-and-associated-keys-and-secrets).

toocreate nycklarna HSM-skyddad måste du ha en prenumeration för valvet som har stöd för HSM-skyddade nycklar.

Lägg till hello ”sku”-parametern när du skapar hello keyvault:

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Du kan lägga till programvara-skyddade nycklar (som visas tidigare) och HSM-skyddade nycklar toothis valvet. toocreate HSM-skyddad nyckel set hello mål parametern too'HSM':

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

Du kan använda hello efter kommandot tooimport en nyckel från en PEM-filen på datorn. Detta kommando importerar hello nyckel till HSM: er i hello Key Vault-tjänsten:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
hello nästa kommando importerar en ”bring your own key” paketet (BYOK). På så sätt kan du skapa din nyckel i din lokala HSM och överföra den tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Detaljerade instruktioner om hur toogenerate BYOK paketet, se [hur toouse HSM-Protected nycklar med Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a>Ta bort hello nyckelvalvet och associerade nycklar och hemligheter
Om du behöver inte längre hello nyckelvalvet och hello nyckel eller hemlighet som den innehåller, kan du ta bort hello nyckelvalv med hello `az keyvault delete` kommando:

```
az keyvault delete --name 'ContosoKeyVault'
```

Eller, du kan ta bort en hela Azure resursgrupp, vilket innefattar hello nyckelvalvet och andra resurser som du ingår i gruppen:

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andra Azure plattformsoberoende kommandoradsgränssnittet kommandon
Andra kommandon som du kan användbart för att hantera Azure Key Vault.

Det här kommandot visas en tabell visning av alla nycklar och valda egenskaper:

AZ keyvault Nyckellista--valvnamnet ContosoKeyVault

Detta kommando visar en fullständig lista över egenskaper för hello angiven nyckel:

Visa AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'

Det här kommandot visas en tabell visning av alla hemliga namn och egenskaper för valda:

AZ keyvault hemliga lista--valvnamnet ContosoKeyVault

Här är ett exempel på hur tooremove en viss nyckel:

borttagning av AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'

Här är ett exempel på hur tooremove en specifik hemlighet:

AZ keyvault hemlighet ta bort--valvnamnet ContosoKeyVault--name 'SQLPassword'


## <a name="next-steps"></a>Nästa steg
Fullständiga Azure CLI-referens för nyckelvalvet kommandon, se [Key Vault CLI-referens](/cli/azure/keyvault)

Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).
