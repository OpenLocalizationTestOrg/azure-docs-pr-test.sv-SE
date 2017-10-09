## <a name="next-steps"></a><span data-ttu-id="e0f30-101">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0f30-101">Next steps</span></span>

<span data-ttu-id="e0f30-102">När du har aktiverat Azure Key Vault-integrering kan du aktivera SQL Server-kryptering på din SQL-VM.</span><span class="sxs-lookup"><span data-stu-id="e0f30-102">After enabling Azure Key Vault Integration, you can enable SQL Server encryption on your SQL VM.</span></span> <span data-ttu-id="e0f30-103">Först måste toocreate en asymmetrisk nyckel i nyckelvalvet och en symmetrisk nyckel i SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e0f30-103">First, you will need toocreate an asymmetric key inside your key vault and a symmetric key within SQL Server on your VM.</span></span> <span data-ttu-id="e0f30-104">Du kommer sedan att kan tooexecute T-SQL-instruktioner tooenable kryptering för dina databaser och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="e0f30-104">Then, you will be able tooexecute T-SQL statements tooenable encryption for your databases and backups.</span></span>

<span data-ttu-id="e0f30-105">Det finns flera typer av kryptering som du kan dra nytta av:</span><span class="sxs-lookup"><span data-stu-id="e0f30-105">There are several forms of encryption you can take advantage of:</span></span>

* [<span data-ttu-id="e0f30-106">Transparent datakryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="e0f30-106">Transparent Data Encryption (TDE)</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="e0f30-107">Krypterad säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e0f30-107">Encrypted backups</span></span>](https://msdn.microsoft.com/library/dn449489.aspx)
* [<span data-ttu-id="e0f30-108">Kryptering på kolumnen (radera)</span><span class="sxs-lookup"><span data-stu-id="e0f30-108">Column Level Encryption (CLE)</span></span>](https://msdn.microsoft.com/library/ms173744.aspx)

<span data-ttu-id="e0f30-109">hello innehåller följande Transact-SQL-skript exempel för dessa olika områden.</span><span class="sxs-lookup"><span data-stu-id="e0f30-109">hello following Transact-SQL scripts provide examples for each of these areas.</span></span>

### <a name="prerequisites-for-examples"></a><span data-ttu-id="e0f30-110">Krav för exempel</span><span class="sxs-lookup"><span data-stu-id="e0f30-110">Prerequisites for examples</span></span>

<span data-ttu-id="e0f30-111">Varje exempel är baserad på hello två krav: kallas för en asymmetrisk nyckel från nyckelvalvet **CONTOSO_KEY** och en autentiseringsuppgift som skapats av hello AKV-integreringen funktion som kallas **Azure_EKM_TDE_cred**.</span><span class="sxs-lookup"><span data-stu-id="e0f30-111">Each example is based on hello two prerequisites: an asymmetric key from your key vault called **CONTOSO_KEY** and a credential created by hello AKV Integration feature called **Azure_EKM_TDE_cred**.</span></span> <span data-ttu-id="e0f30-112">hello installationsprogrammet följande Transact-SQL-kommandon förutsättningarna för att köra hello exempel.</span><span class="sxs-lookup"><span data-stu-id="e0f30-112">hello following Transact-SQL commands setup these prerequisites for running hello examples.</span></span>

``` sql
USE master;
GO

sp_configure [show advanced options], 1;
GO
RECONFIGURE;
GO

-- Enable EKM provider
sp_configure [EKM provider enabled], 1;
GO
RECONFIGURE;

--create provider
CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov
FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';
GO

--create credential
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--must have sysadmin
ALTER LOGIN [TDE_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'keytestvault',  --key name
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a><span data-ttu-id="e0f30-113">Transparent datakryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="e0f30-113">Transparent Data Encryption (TDE)</span></span>

1. <span data-ttu-id="e0f30-114">Skapa en toobe för SQL Server-inloggning som används av hello databasmotorn för TDE och lägger till hello autentiseringsuppgifter tooit.</span><span class="sxs-lookup"><span data-stu-id="e0f30-114">Create a SQL Server login toobe used by hello Database Engine for TDE, then add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello TDE Login tooadd hello credential for use by the
   -- Database Engine tooaccess hello key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
   GO
   ```

1. <span data-ttu-id="e0f30-115">Skapa hello databaskrypteringsnyckeln som ska användas för TDE.</span><span class="sxs-lookup"><span data-stu-id="e0f30-115">Create hello database encryption key that will be used for TDE.</span></span>

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello database tooenable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a><span data-ttu-id="e0f30-116">Krypterad säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e0f30-116">Encrypted backups</span></span>

1. <span data-ttu-id="e0f30-117">Skapa en toobe för SQL Server-inloggning som används av hello databasmotorn för kryptering av säkerhetskopieringar och Lägg till hello autentiseringsuppgifter tooit.</span><span class="sxs-lookup"><span data-stu-id="e0f30-117">Create a SQL Server login toobe used by hello Database Engine for encrypting backups, and add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it is encrypting hello backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello Encrypted Backup Login tooadd hello credential for use by
   -- hello Database Engine tooaccess hello key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
   GO
   ```

1. <span data-ttu-id="e0f30-118">Säkerhetskopiering hello databas ange kryptering med hello asymmetrisk nyckel som lagras i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="e0f30-118">Backup hello database specifying encryption with hello asymmetric key stored in hello key vault.</span></span>

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   tooDISK = N'[PATH tooBACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a><span data-ttu-id="e0f30-119">Kryptering på kolumnen (radera)</span><span class="sxs-lookup"><span data-stu-id="e0f30-119">Column Level Encryption (CLE)</span></span>

<span data-ttu-id="e0f30-120">Det här skriptet skapar en symmetrisk nyckel som skyddas av hello asymmetrisk nyckel i hello nyckelvalvet och använder sedan hello symmetrisk nyckel tooencrypt data i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e0f30-120">This script creates a symmetric key protected by hello asymmetric key in hello key vault, and then uses hello symmetric key tooencrypt data in hello database.</span></span>

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open hello symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data tooencrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close hello symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a><span data-ttu-id="e0f30-121">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e0f30-121">Additional resources</span></span>

<span data-ttu-id="e0f30-122">Mer information om hur toouse krypteringsfunktionerna Se [med hjälp av EKM med SQL Server-krypteringsfunktioner](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span><span class="sxs-lookup"><span data-stu-id="e0f30-122">For more information on how toouse these encryption features, see [Using EKM with SQL Server Encryption Features](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span></span>

<span data-ttu-id="e0f30-123">Observera att hello stegen i den här artikeln förutsätter att du redan har SQL Server körs på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="e0f30-123">Note that hello steps in this article assume that you already have SQL Server running on an Azure virtual machine.</span></span> <span data-ttu-id="e0f30-124">Om inte, se [etablera en virtuell dator med SQL Server i Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="e0f30-124">If not, see [Provision a SQL Server virtual machine in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="e0f30-125">Andra anvisningar om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e0f30-125">For other guidance on running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
