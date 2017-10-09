---
title: aaaEncrypt en virtuell dator i Azure | Microsoft Docs
description: "Det här dokumentet hjälper dig att tooencrypt en virtuell Azure-dator när du har fått en avisering från Azure Security Center."
services: security, security-center
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: 
ms.assetid: f6c28bc4-1f79-4352-89d0-03659b2fa2f5
ms.service: security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2017
ms.author: tomsh
ms.openlocfilehash: 7c7c6eed39d16bde8a0dfaffe3a3331c58101634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-an-azure-virtual-machine"></a>Kryptera en virtuell Azure-dator
Azure Security Center varnar dig om du har virtuella datorer som inte är krypterade. Dessa aviseringar visas med hög angelägenhetsgrad och hello rekommendation är tooencrypt dessa virtuella datorer.

![Rekommendation för kryptering av disk](./media/security-center-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> hello informationen i det här dokumentet gäller tooencrypting virtuella datorer utan att använda en krypteringsnyckel för nyckel (som krävs för att säkerhetskopiera virtuella datorer med Azure Backup). Hello artikel [Azure Disk Encryption för Windows och Linux virtuella datorer i Azure](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) information om hur toouse en nyckel krypteringsnyckeln toosupport Azure Backup för krypterade Azure virtuella datorer.
>
>

tooencrypt Azure virtuella datorer som identifierats av Azure Security Center behov av kryptering, rekommenderar vi hello följande steg:

* Installera och konfigurera Azure PowerShell. Detta gör att du toorun hello PowerShell-kommandon krävs tooset in hello krav krävs tooencrypt Azure virtuella datorer.
* Hämta och kör hello Azure Disk Encryption krav Azure PowerShell-skript
* Kryptera dina virtuella datorer

hello syftet med det här dokumentet är tooenable du tooencrypt virtuella datorer, även om du har liten eller ingen erfarenhet av Azure PowerShell.
Det här dokumentet förutsätter att du använder Windows 10 som hello klientdatorn där du ska konfigurera Azure Disk Encryption.

Det finns flera metoder som kan vara används toosetup hello krav och tooconfigure kryptering för virtuella datorer i Azure. Om du redan känner till väl Azure PowerShell eller Azure CLI, kanske du hellre toouse alternativa metoder.

> [!NOTE]
> toolearn mer information om alternativa sätt tooconfiguring kryptering för virtuella Azure-datorer, se [Azure Disk Encryption för Windows och Linux virtuella datorer i Azure](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).
>
>

## <a name="install-and-configure-azure-powershell"></a>Installera och konfigurera Azure PowerShell
Du behöver ha Azure PowerShell version 1.2.1 eller senare installerat på datorn. hello artikel [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) innehåller alla hello stegen tooprovision din dator toowork med Azure PowerShell. hello enklaste metoden är toouse hello Web PI-installation-metoden anges i den artikeln. Även om du redan har installerat Azure PowerShell, installera igen med hello Web PI-metoden så att du har hello senaste versionen av Azure PowerShell.

## <a name="obtain-and-run-hello-azure-disk-encryption-prerequisites-configuration-script"></a>Hämta och kör krävs för hello Azure disk encryption konfigurationsskript
hello Azure Disk Encryption nödvändiga Konfigurationsskriptet ställer in alla hello förutsättningar som krävs för att kryptera dina virtuella Azure-datorer.

1. Gå toohello GitHub-sida som innehåller hello [Azure Disk Encryption nödvändiga installationsskriptet](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
2. Hej GibHub-sidan, klicka på hello **Raw** knappen.
3. Använd **CTRL-A** tooselect alla hello text på hello sida och sedan använda **CTRL-C** toocopy alla hello text hello sidan toohello Urklipp.
4. Öppna **anteckningar** och klistrar in hello kopieras i anteckningar.
5. Skapa en ny mapp på enhet C: med namnet **AzureADEScript**.
6. Spara hello anteckningsfilen: Klicka på **filen**, klicka på **Spara som**. Ange i textrutan med hello-fil, **”ADEPrereqScript.ps1”** och på **spara**. (Kontrollera att du placerar hello citattecknen runt hello namn, annars sparas hello-fil med filnamnstillägget .txt).

Nu när hello skriptinnehållet har sparats, öppna hello skript i hello PowerShell ISE:

1. I hello Start-menyn, klickar du på **Cortana**. Be **Cortana** ”PowerShell” genom att skriva **PowerShell** i hello Cortana-sökrutan.
2. Högerklicka på **Windows PowerShell ISE** och klicka på **Kör som administratör**.
3. I hello **administratör: Windows PowerShell ISE** -fönstret klickar du på **visa** och klicka sedan på **visa Skriptfönster**.
4. Om du ser hello **kommandon** hello höger på hello fönstret klicka hello **”x”** i hello övre högra hörnet av hello fönstret tooclose den. Om hello texten är för liten för att du toosee använder **CTRL + Lägg till** (”Lägg till” är hello ”plustecknet”). Om hello texten är för stor, kan du använda **CTRL + subtrahera** (subtrahera är hello ”-” tecken).
5. Klicka på **Arkiv** och sedan på **Öppna**. Navigera toohello **C:\AzureADEScript** mappen och hello Dubbelklicka på hello **ADEPrereqScript**.
6. Hej **ADEPrereqScript** innehåll bör nu visas i hello PowerShell ISE och är färgkodade toohelp du enkelt se diverse komponenter, till exempel kommandon, parametrar och variabler.

Du bör nu se något liknande hello bilden nedan.

![Fönstret PowerShell ISE](./media/security-center-disk-encryption/security-center-disk-encryption-fig2.png)

hello övre fönstret är refererad tooas hello ”Skriptfönster” hello nedre rutan är refererad tooas hello ”konsolen”. Vi använder dessa termer senare i den här artikeln.

## <a name="run-hello-azure-disk-encryption-prerequisites-powershell-command"></a>Kör PowerShell-kommando för hello Azure disk encryption krav
hello skript krävs för Azure Disk Encryption ber dig om hello följande information när du har startat hello skript:

* **Resursgruppens namn** - namn för hello resursgrupp som du vill tooput hello Nyckelvalvet i.  En ny resursgrupp med hello-namn som du anger kommer att skapas om det inte redan finns en med samma namn skapas. Om du redan har en resursgrupp som du vill toouse i den här prenumerationen ange hello namnet på resursgruppen.
* **Nyckelvalvets namn** -namnet på hello Nyckelvalv där krypteringsnycklarna nycklar är toobe placeras. Om det inte redan finns ett nyckelvalv med samma namn skapas ett nytt nyckelvalv med det här namnet. Om du redan har ett Nyckelvalv som du vill toouse ange hello namn på hello befintliga Nyckelvalvet.
* **Plats** -platsen för hello Key Vault. Kontrollera hello Nyckelvalvet och virtuella datorer toobe krypterade i hello samma plats. Om du inte vet hello plats finns steg senare i den här artikeln som visar hur toofind ut.
* **Azure Active Directory-programnamn** -namnet på hello Azure Active Directory-program som kommer att använda toowrite hemligheter toohello Key Vault. Om det inte redan finns ett program med det namnet skapas ett nytt. Om du redan har ett Azure Active Directory-program som du vill toouse ange hello namnet på det Azure Active Directory-programmet.

> [!NOTE]
> Om du är nyfiken som toowhy måste toocreate ett Azure Active Directory-program, se *registrera ett program med Azure Active Directory* i hello artikeln [komma igång med Azure Key Vault](../key-vault/key-vault-get-started.md).
>
>

Utför hello följande steg tooencrypt en virtuell Azure-dator:

1. Om du stängde hello PowerShell ISE öppnar du en upphöjd hello PowerShell ISE-instans. Följ hello anvisningarna tidigare i den här artikeln om hello PowerShell ISE inte redan är öppna. Om du har stängt hello skript och öppna sedan hello **ADEPrereqScript.ps1** på **filen**, sedan **öppna** och välja hello skript hello **c:\ AzureADEScript** mapp. Om du har följt den här artikeln från början hello, bara vidare toohello nästa steg.
2. Ändra hello fokus toohello lokala hello skriptet genom att skriva i hello PowerShell ISE (hello längst ned i fönstret hello PowerShell ISE) hello konsolträdet **cd c:\AzureADEScript** och tryck på **RETUR**.
3. Ange hello körningsprincipen på datorn så att du kan köra hello skript. Typen **Set-ExecutionPolicy Unrestricted** i hello konsolen och tryck sedan på RETUR. Om du ser en dialogruta om hello effekterna av hälsningspaket ändringsprinciper tooexecution klickar du antingen **Ja tooall** eller **Ja** (om du ser **Ja tooall**, markerar du det alternativet – om du inte ser **Ja tooall**, klicka på **Ja**).
4. Logga in på Azure-kontot. Skriv i konsolen för hello **Login-AzureRmAccount** och tryck på **RETUR**. En dialogruta visas där du anger dina autentiseringsuppgifter (Kontrollera att du har rättigheter toochange hello virtuella datorer – om du inte har rättigheter du inte kan tooencrypt dem. Om du inte är säker frågar du din prenumerationsägare eller administratör). Du bör se information om din **miljö**, ditt **konto**, **klient-ID**, **prenumerations-ID** och **aktuellt lagringskonto**. Kopiera hello **SubscriptionId** tooNotepad. Du behöver toouse detta i steg #6.
5. Sök efter vilken prenumeration den virtuella datorn tillhör tooand dess plats. Gå för[https://portal.azure.com](ttps://portal.azure.com) och logga in.  Klicka på vänster sida av hello sidan hello, **virtuella datorer**. Du kommer se en lista över dina virtuella datorer och hello prenumerationer som de tillhör.

   ![Virtuella datorer](./media/security-center-disk-encryption/security-center-disk-encryption-fig3.png)
6. Returnera toohello PowerShell ISE. Ange hello prenumerationskontext där hello skriptet ska köras. Skriv i konsolen för hello **Select-AzureRmSubscription – SubscriptionId < your_subscription_Id >** (Ersätt **< your_subscription_Id >** med ditt faktiska prenumerations-ID) och tryck på  **Ange**. Du kommer att se information om hello miljö, **konto**, **TenantId**, **SubscriptionId** och **aktuellt Lagringskonto**.
7. Du är nu redo toorun hello skript. Klicka på hello **kör skriptet** eller tryck på knappen **F5** på hello tangentbord.

   ![Köra PowerShell-skript](./media/security-center-disk-encryption/security-center-disk-encryption-fig4.png)
8. hello skriptet begär **resourceGroupName:** -anger hello namnet på *resursgruppen* du vill toouse, tryck på **RETUR**. Ange ett namn som du vill använda toouse för en ny om du inte har någon. Om du redan har en *resursgruppen* du vill toouse (till exempel hello något som den virtuella datorn är i), ange hello namnet på hello befintlig resursgrupp.
9. hello skriptet begär **keyVaultName:** -anger hello namnet på hello *Nyckelvalvet* du vill ha toouse och tryck på RETUR. Ange ett namn som du vill använda toouse för en ny om du inte har någon. Om du redan har ett Nyckelvalv som du vill toouse anger hello namnet på hello befintliga *Nyckelvalvet*.
10. hello skriptet begär **plats:** – ange hello namnet på hello var i vilka hello VM som du vill tooencrypt finns och tryck sedan på **RETUR**. Om du inte kommer ihåg hello plats, gå tillbaka toostep #5.
11. hello skriptet begär **aadAppName:** -anger hello namnet på hello *Azure Active Directory* tryck på programmet som du vill toouse, **RETUR**. Ange ett namn som du vill använda toouse för en ny om du inte har någon. Om du redan har en *Azure Active Directory-programmet* du vill toouse, anger hello namnet på hello befintliga *Azure Active Directory-programmet*.
12. En logg i dialogrutan visas. Ange dina autentiseringsuppgifter (Ja, du har loggat in en gång, men nu behöver du toodo igen).
13. hello skriptet körs och när du är klar uppmanas du toocopy hello värdena för hello **aadClientID**, **aadClientSecret**, **diskEncryptionKeyVaultUrl**, och **keyVaultResourceId**. Kopiera vart och ett av dessa värden toohello Urklipp och klistra in dem i anteckningar.
14. Returnera toohello PowerShell ISE och placera markören hello hello slutet av hello sista raden och tryck på **RETUR**.

hello utdata från skriptet hello ska se ut ungefär hello-skärmen nedan:

![PowerShell-utdata](./media/security-center-disk-encryption/security-center-disk-encryption-fig5.png)

## <a name="encrypt-hello-azure-virtual-machine"></a>Kryptera hello Azure-dator
Du är nu redo tooencrypt den virtuella datorn. Om den virtuella datorn finns i hello samma resursgrupp som ditt Nyckelvalv kan du flytta på toohello krypteringsstegen. Om den virtuella datorn inte är i hello samma resursgrupp som ditt Nyckelvalv, måste tooenter hello följande i hello hello PowerShell ISE-konsolen:

**$resourceGroupName = <’Virtual_Machine_RG’>**

Ersätt **< Virtual_Machine_RG >** med hello resursgruppen där de virtuella datorerna finns hello namnet omges av enkla citattecken. Tryck sedan på **RETUR**.
tooconfirm som Hej rätt Resursgruppsnamn har angetts, skriver följande hello i hello konsolen PowerShell ISE:

**$resourceGroupName**

Tryck på **RETUR**. Du bör se hello namnet på resursgruppen som de virtuella datorerna finns i. Exempel:

![PowerShell-utdata](./media/security-center-disk-encryption/security-center-disk-encryption-fig6.png)

### <a name="encryption-steps"></a>Krypteringssteg
Du måste först tootell PowerShell hello namn hello virtuell dator som du vill tooencrypt. I konsolen för hello skriver du:

**$vmName = <’your_vm_name’>**

Ersätt **< your_vm_name >** med hello namnet på den virtuella datorn (Kontrollera att hello namnet omges av enkla citattecken) och tryck sedan på **RETUR**.

tooconfirm som Hej rätt namn på virtuell dator angavs, skriver du:

**$vmName**

Tryck på **RETUR**. Du bör se hello namn hello virtuell dator som du vill tooencrypt. Exempel:

![PowerShell-utdata](./media/security-center-disk-encryption/security-center-disk-encryption-fig7.png)

Det finns två metoder toorun hello kryptering kommandot tooencrypt alla enheter på hello virtuell dator. hello första metoden är tootype hello följande kommando i hello konsolen PowerShell ISE:

~~~
Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
~~~

När du har skrivit det här kommandot trycker du på **RETUR**.

hello andra metoden är tooclick i hello Skriptfönster (hello översta rutan hello PowerShell ISE) och rulla ned toohello längst ned på hello skript. Markera hello-kommando som anges ovan och högerklicka på den och klicka sedan på **kör** eller tryck på **F8** på hello tangentbord.

![PowerShell ISE](./media/security-center-disk-encryption/security-center-disk-encryption-fig8.png)

Oavsett hello metod använder du en dialogruta visas som informerar dig om att det tar 10 – 15 minuter för hello åtgärden toocomplete. Klicka på **Ja**.

Medan hello krypteringsprocessen sker kan du se hello hello virtuella datorns status för returnera toohello Azure-portalen. Klicka på vänster sida av hello sidan hello, **virtuella datorer**, i hello **virtuella datorer** bladet Klicka hello namnet på hello virtuell dator som du krypterar. Hello-bladet som visas, Lägg märke till att hello **Status** säger att den **uppdatering**. Det visar att kryptering pågår.

![Mer information om hello VM](./media/security-center-disk-encryption/security-center-disk-encryption-fig9.png)

Returnera toohello PowerShell ISE. När hello skriptet har slutförts visas det som visas i hello bilden nedan.

![PowerShell-utdata](./media/security-center-disk-encryption/security-center-disk-encryption-fig10.png)

toodemonstrate som hello virtuella datorn är krypterad, returnera toohello Azure-portalen och klicka på **virtuella datorer** på vänster sida av hello sidan hello. Klicka på hello virtuella du krypterade hello namn. I hello **inställningar** bladet, klickar du på **diskar**.

![Inställningsalternativ](./media/security-center-disk-encryption/security-center-disk-encryption-fig11.png)

På hello **diskar** bladet visas **kryptering** är **aktiverad**.

![Diskegenskaper](./media/security-center-disk-encryption/security-center-disk-encryption-fig12.png)

## <a name="next-steps"></a>Nästa steg
I det här dokumentet du lärt dig hur tooencrypt en virtuell dator i Azure. toolearn mer om Azure Security Center finns hello följande:

* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/): Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
