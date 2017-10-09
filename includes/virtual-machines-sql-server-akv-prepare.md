## <a name="prepare-for-akv-integration"></a>Förbereda för AKV-integreringen
toouse Azure Key Vault-integrering tooconfigure SQL Server-VM finns flera förutsättningar: 

1. [Installera Azure Powershell](#install-azure-powershell)
2. [Skapa en Azure Active Directory](#create-an-azure-active-directory)
3. [Skapa ett nyckelvalv](#create-a-key-vault)

hello följande avsnitt beskrivs dessa krav och hello information du behöver toocollect toolater kör hello PowerShell-cmdlets.

### <a name="install-azure-powershell"></a>Installera Azure PowerShell
Kontrollera att du har installerat hello senaste Azure PowerShell SDK. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="create-an-azure-active-directory"></a>Skapa en Azure Active Directory
Du måste först toohave en [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) i din prenumeration. Bland många fördelar kan du toogrant behörighet tooyour nyckelvalv för vissa användare och program.

Registrera ett program med AAD. Detta ger ett huvudnamn för tjänsten-konto som har åtkomst tooyour nyckelvalv som behöver den virtuella datorn. I hello Azure Key Vault artikeln du hittar de här stegen i hello [registrera ett program med Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) avsnitt eller du kan se hello steg med skärmbilder i hello **hämta en identitet för hello program avsnittet** av [i det här blogginlägget](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Innan du slutför Observera stegen, att du måste toocollect hello följande information under registreringen som behövs för senare när du aktiverar Azure Key Vault-integrering på din SQL-VM.

* När programmet hello läggs hitta hello **klient-ID** på hello **konfigurera** fliken.   ![Azure Active Directory klient-ID](./media/virtual-machines-sql-server-akv-prepare/aad-client-id.png)
  
    hello klient-ID tilldelas senare toohello **$spName** (Service Principal name)-parametern i hello PowerShell-skriptet tooenable Azure Key Vault-integrering. 
* Dessutom under de här stegen när du skapar din nyckel kopiera hello hemligheten för din nyckel som visas i följande skärmbild hello. Den här nyckeln hemlig tilldelas senare toohello **$spSecret** (Service Principal hemliga) parameter i hello PowerShell-skript.  
    ![Azure Active Directory hemlighet](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)
* Du måste auktorisera den här nya klient-ID toohave hello följande behörigheter: **kryptera**, **dekryptera**, **wrapKey**, **unwrapKey**, **logga**, och **Kontrollera**. Detta görs med hello [Set AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) cmdlet. Mer information finns i [auktorisera hello programmet toouse hello nyckel eller hemlighet](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Skapa ett nyckelvalv
I ordning toouse Azure Key Vault toostore hello nycklar används för kryptering i den virtuella datorn, behöver du åtkomst tooa nyckelvalvet. Om du inte redan har installerat nyckelvalvet, skapa en genom att följa stegen hello i hello [komma igång med Azure Key Vault](../articles/key-vault/key-vault-get-started.md) avsnittet. Observera att det finns vissa information du behöver toocollect under den här uppsättningen som krävs senare när du aktiverar Azure Key Vault-integrering på din SQL-VM innan du utför de här stegen.

När du får toohello skapa ett nyckelvalv steg Obs hello returnerade **vaultUri** -egenskap som är hello key vault-URL. I hello exemplet i det steg som visas nedan, hello nyckelvalv namn är ContosoKeyVault, därför hello key vault-URL att https://contosokeyvault.vault.azure.net/.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

hello key vault-URL är tilldelad senare toohello **$akvURL** parameter i hello PowerShell-skriptet tooenable Azure Key Vault-integrering.

