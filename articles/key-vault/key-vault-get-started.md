---
title: "aaaGet igång med Azure Key Vault | Microsoft Docs"
description: "Använd den här självstudiekursen toohelp du få igång med Azure Key Vault toocreate en förstärkt behållare i Azure, toostore och hantera krypteringsnycklar och hemligheter i Azure."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 865853b778dec5fca5c7db0d060627554c0a9cb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-key-vault"></a>Komma igång med Azure Key Vault
Azure Key Vault är tillgängligt i de flesta regioner. Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Introduktion
Använd den här självstudiekursen toohelp du få igång med Azure Key Vault toocreate en förstärkt behållare (ett valv) i Azure, toostore och hantera krypteringsnycklar och hemligheter i Azure. Den vägleder dig genom hello processen med att använda Azure PowerShell toocreate ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program. Därefter tittar vi på hur ett program kan använda den här nyckeln eller lösenordet.

**Uppskattad tid toocomplete:** 20 minuter

> [!NOTE]
> Den här självstudiekursen innehåller inte instruktioner för hur toowrite hello Azure-program med något av hello steg, nämligen hur tooauthorize ett program toouse en nyckel eller hemlighet i hello nyckeln valvet.
>
> I den här kursen används Azure PowerShell. Anvisningar för plattformsoberoende kommandoradsgränssnitt finns i [den här självstudiekursen](key-vault-manage-with-cli2.md).
>
>

Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Krav
toocomplete den här självstudien måste du ha hello följande:

* En prenumeration tooMicrosoft Azure. Om du inte har en prenumeration kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell, **minst version 1.1.0**. tooinstall Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Om du redan har installerat Azure PowerShell och inte vet hello version från hello Azure PowerShell-konsolen, Skriv `(Get-Module azure -ListAvailable).Version`. Du kan använda den här självstudiekursen med några mindre ändringar även om du har Azure PowerShell version 0.9.1 till och med 0.9.8 installerad. Till exempel måste du använda hello `Switch-AzureMode AzureResourceManager` kommandot och hello Azure Key Vault kommandona har ändrats. En lista över hello Key Vault-cmdlets för versioner 0.9.1 via 0.9.8, se [Azure Key Vault-Cmdlets](/powershell/module/azurerm.keyvault/#key_vault).
* Ett program som är konfigurerade toouse hello nyckel eller lösenord som du skapar i den här kursen. Ett exempelprogram som är tillgänglig från hello [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Instruktioner finns i hello tillhörande Readme-filen.

Den här kursen är avsedd för Azure PowerShell nybörjare, men förutsätter att du förstår hello grundläggande begrepp som moduler, cmdletar och sessioner. Mer information finns i [Komma igång med Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

tooget utförlig information för alla cmdletar som du ser i den här kursen används hello **Get-Help** cmdlet.

    Get-Help <cmdlet-name> -Detailed

Till exempel tooget hjälp för hello **Login-AzureRmAccount** cmdlet, skriv:

    Get-Help Login-AzureRmAccount -Detailed

Du kan också läsa följande kurser tooget bekant med Azure Resource Manager i Azure PowerShell hello:

* [Hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)
* [Använda Azure PowerShell med Resource Manager](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Ansluta tooyour prenumerationer
Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:  

    Login-AzureRmAccount

Observera att om du använder en specifik instans av Azure, till exempel Azure Government använder hello - miljö parametern med det här kommandot. Exempel: `Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret. Azure PowerShell får alla hello-prenumerationer som är associerade med det här kontot och som standard, använder hello första.

Om du har flera prenumerationer och vill toospecify en specifik en toouse för Azure Key Vault, skriver du hello följande toosee hello prenumerationer för ditt konto:

    Get-AzureRmSubscription

Sedan toospecify hello prenumeration toouse, typ:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Mer information om hur du konfigurerar Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a id="resource"></a>Skapa en ny resursgrupp
När du använder Azure Resource Manager skapas alla relaterade resurser inuti en resursgrupp. Vi ska skapa en ny resursgrupp med namnet **ContosoResourceGroup** i den här självstudiekursen:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Skapa ett nyckelvalv
Använd hello [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) cmdlet toocreate en nyckelvalvet. Denna cmdlet har tre obligatoriska parametrar: en **resursgruppnamn**, **nyckelvalv namn**, och hello **geografisk plats**.

Om du använder hello valvnamnet för till exempel **ContosoKeyVault**, hello resursgruppens namn av **ContosoResourceGroup**, och hello plats **Östasien**, typ:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

hello resultatet av denna cmdlet visar egenskaperna för hello nyckelvalv som du nyss skapade. hello två viktigaste egenskaper är:

* **Valvet namnet**: I hello exempelvis är **ContosoKeyVault**. Du ska använda det här namnet för andra Key Vault-cmdlets.
* **Valvet URI**: I hello exempelvis är https://contosokeyvault.vault.azure.net/. Program som använder ditt valv via dess REST-API måste använda denna URI.

Azure-konto är nu behörig tooperform några åtgärder på den här nyckeln valvet. Vilket ingen annan har ännu.

> [!NOTE]
> Om du ser hello fel **hello prenumerationen är inte registrerad toouse namnområdet 'Microsoft.KeyVault'** när du försöker toocreate nya nyckelvalvet, kör `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` och kör sedan kommandot Nytt AzureRmKeyVault. Mer information finns i [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Lägga till en nyckel eller Hemlig toohello nyckelvalv
Om du vill Azure Key Vault toocreate programvaruskyddad nyckel som du kan använda hello [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet, och skriver hello följande:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Men om du har en befintlig programvaruskyddad nyckel i en. PFX-fil som sparats tooyour C:\ enhet i en fil med namnet softkey.pfx som du vill tooupload tooAzure Key Vault typen hello följande tooset hello variabeln **securepfxpwd** för ett lösenord på **123** för hello. PFX-filen:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Skriv sedan följande tooimport hello nyckeln från hello hello. PFX-fil, som skyddar hello nyckeln av programvara i hello Key Vault-tjänsten:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Nu kan du referera den här nyckeln att du har skapat eller överföra tooAzure Key Vault med hjälp av dess URI. Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hämta hello aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget den här specifika versionen.  

toodisplay hello URI för den här nyckeln typ:

    $Key.key.kid

tooadd en hemlig toohello valvet, som är ett lösenord med namnet SQLPassword och har hello Pa$ w0rd tooAzure Key Vault-värde, konvertera först hello värdet för Pa$ w0rd tooa säker sträng genom att skriva hello följande:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Skriv hello följande:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nu kan du referera lösenordet du lagt till tooAzure Key Vault med dess URI. Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hämta hello aktuella versionen och använda **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget den här specifika versionen.

toodisplay hello URI för den här hemligheten typ:

    $secret.Id

Nu ska vi visa hello nyckel eller hemlighet som du just skapat:

* tooview nyckeltypen,:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* tooview din hemliga, typ:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Ditt nyckelvalv och nyckel eller hemlighet är nu redo för program toouse. Du måste auktorisera program toouse dem.  

## <a id="register"></a>Registrera ett program med Azure Active Directory
Det här steget utförs normalt av en utvecklare, på en separat dator. Det är inte specifik tooAzure Key Vault men finns här för fullständighetens skull.

> [!IMPORTANT]
> toocomplete hello kursen, ditt konto, hello valvet och hello-program som du registrerar i det här steget måste vara i hello samma Azure-katalogen.
>
>

Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory. toodo detta, hello ägare av programmet hello måste först registrera programmet hello i sina Azure Active Directory. Hello slutet av registrering hämtar hello programägaren hello följande värden:

* En **program-ID** (även kallat ett klient-ID) och **autentiseringsnyckel** (även kallat hello delad hemlighet). programmet hello måste båda dessa värden tooAzure Active Directory tooget presentera en token. Hur konfigureras programmet hello toodo detta beror på programmet hello. För hello Key Vault exempelprogrammet anger hello programägaren värdena i hello app.config-fil.

tooregister hello program i Azure Active Directory:

1. Logga in toohello klassiska Azure-portalen.
2. Klicka på vänster hello **Active Directory**, och välj sedan hello katalog där du ska registrera ditt program. <br> <br> **Obs:** måste du välja hello samma katalog som innehåller hello Azure-prenumeration som du skapade nyckelvalvet. Om du inte vet vilken katalog detta är klickar du på **inställningar**, identifiera hello prenumeration som du skapade ditt nyckelvalv och Observera hello namnet på hello katalog visas i hello sista kolumnen.
3. Klicka på **Program**. Om inga appar har lagts till tooyour directory kan den här sidan visar endast hello **Lägg till en App** länk. Klicka på länken hello eller också kan du klicka på **lägga till** i hello kommandofält.
4. I hello **Lägg till program** på hello **vad vill du vill toodo?** klickar du på **Lägg till ett program som min organisation utvecklar**.
5. På hello **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och väljer sedan **WEB APPLICATION och/eller webb-API** (hello standard). Klicka på hello **nästa** ikon.
6. På hello **appegenskaper** anger hello **SIGN-ON-URL** och **APP-ID URI** för webbprogram. Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna). Det spelar ingen roll om dessa platser finns eller inte. Vad är viktigt är hello appen ID URI för varje program är olika för varje program i din katalog. hello directory använder denna sträng tooidentify din app.
7. Klicka på hello **Slutför** ikonen toosave ändringarna i hello guiden.
8. På hello **Snabbstart** klickar du på **konfigurera**.
9. Rulla toohello **nycklar** avsnittet väljer hello varaktighet och klicka sedan på **spara**. hello sidan uppdateras och innehåller nu ett nyckelvärde. Du måste konfigurera ditt program med det här värdet för nyckeln och hello **klient-ID** värde. (Anvisningar för den här konfigurationen är programspecifika.)
10. Kopiera hello klient-ID-värde från den här sidan som du vill använda i hello nästa steg tooset behörigheter på ditt valv.

## <a id="authorize"></a>Auktorisera hello programmet toouse hello nyckel eller hemlighet.
tooauthorize hello programmet tooaccess hello nyckel eller hemlighet i hello valvet, Använd den [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.

Om din valvnamnet är till exempel **ContosoKeyVault** och hello-program som du vill tooauthorize har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tooauthorize hello programmet toodecrypt och logga med nycklar i ditt valv kör hello följande:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Om du vill tooauthorize att samma program tooread hemligheter i ditt valv, kör du hello följande:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Om du vill toouse en maskinvarusäkerhetsmodul (HSM)
För ytterligare säkerhet kan du importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns. hello HSM är FIPS 140-2 Level 2-verifierade. Om det här kravet inte gäller tooyou, hoppa över det här avsnittet och gå för[ta bort hello nyckelvalvet och associerade nycklar och hemligheter](#delete).

toocreate nycklarna HSM-skyddad måste du använda hello [Azure Key Vault Premium service tier toosupport HSM-skyddade nycklar](https://azure.microsoft.com/pricing/free-trial/). Observera även att den här funktionen inte är tillgänglig för Azure i Kina.

När du skapar hello nyckelvalvet, lägger du till hello **- SKU** parameter:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Du kan lägga till programvara-skyddade nycklar (som visas tidigare) och HSM-skyddade nycklar toothis nyckelvalvet. toocreate HSM-skyddad nyckel set hello **-målet** parametern too'HSM':

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Du kan använda följande kommando tooimport hello en nyckel från en. PFX-filen på datorn. Detta kommando importerar hello nyckel till HSM: er i hello Key Vault-tjänsten:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


hello nästa kommando importerar en ”bring your own key” paketet (BYOK). Det här scenariot kan du skapa din nyckel i din lokala HSM och överföra det tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Detaljerade instruktioner om hur toogenerate BYOK paketet, se [hur toogenerate och överför HSM-skyddade nycklar för Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Ta bort hello nyckelvalvet och associerade nycklar och hemligheter
Om du behöver inte längre hello nyckelvalvet och hello nyckel eller hemlighet som den innehåller, kan du ta bort hello nyckelvalv med hello [ta bort AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) cmdlet:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Eller, du kan ta bort en hela Azure resursgrupp, vilket innefattar hello nyckelvalvet och andra resurser som du ingår i gruppen:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Andra Azure PowerShell-cmdletar
Andra kommandon som kan vara användbara för att hantera Azure Key Vault:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Det här kommandot hämtar en tabellvy över alla nycklar och valda egenskaper.
* `$Keys[0]`: Det här kommandot visar en fullständig lista över egenskaper för hello angiven nyckel
* `Get-AzureKeyVaultSecret`: Det här kommandot visar en tabellvy över alla hemliga namn och valda egenskaper.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Exempel hur tooremove en viss nyckel.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Exempel hur tooremove en specifik hemlighet.

## <a id="next"></a>Nästa steg
En uppföljningskurs där Azure Key Vault används i en webbapp finns i [Använda Azure Key Vault från en webbapp](key-vault-use-from-web-application.md).

toosee hur nyckelvalvet används finns i [Azure Key Vault-loggning](key-vault-logging.md).

En lista över hello senaste Azure PowerShell-cmdlets för Azure Key Vault finns [Azure Key Vault-Cmdlets](/powershell/module/azurerm.keyvault/#key_vault).

Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).
