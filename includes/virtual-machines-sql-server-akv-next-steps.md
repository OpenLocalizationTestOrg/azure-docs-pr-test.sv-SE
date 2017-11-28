## <a name="next-steps"></a><span data-ttu-id="2be53-101">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2be53-101">Next steps</span></span>

<span data-ttu-id="2be53-102">När du har aktiverat Azure Key Vault-integrering kan du aktivera SQL Server-kryptering på din SQL-VM.</span><span class="sxs-lookup"><span data-stu-id="2be53-102">After enabling Azure Key Vault Integration, you can enable SQL Server encryption on your SQL VM.</span></span> <span data-ttu-id="2be53-103">Du måste först skapa en asymmetrisk nyckel i nyckelvalvet och en symmetrisk nyckel i SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2be53-103">First, you will need to create an asymmetric key inside your key vault and a symmetric key within SQL Server on your VM.</span></span> <span data-ttu-id="2be53-104">Sedan kommer du att kunna köra T-SQL-uttryck för att aktivera kryptering för dina databaser och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="2be53-104">Then, you will be able to execute T-SQL statements to enable encryption for your databases and backups.</span></span>

<span data-ttu-id="2be53-105">Det finns flera typer av kryptering som du kan dra nytta av:</span><span class="sxs-lookup"><span data-stu-id="2be53-105">There are several forms of encryption you can take advantage of:</span></span>

* [<span data-ttu-id="2be53-106">Transparent datakryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="2be53-106">Transparent Data Encryption (TDE)</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="2be53-107">Krypterad säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="2be53-107">Encrypted backups</span></span>](https://msdn.microsoft.com/library/dn449489.aspx)
* [<span data-ttu-id="2be53-108">Kryptering på kolumnen (radera)</span><span class="sxs-lookup"><span data-stu-id="2be53-108">Column Level Encryption (CLE)</span></span>](https://msdn.microsoft.com/library/ms173744.aspx)

<span data-ttu-id="2be53-109">Följande Transact-SQL-skript innehåller exempel för dessa olika områden.</span><span class="sxs-lookup"><span data-stu-id="2be53-109">The following Transact-SQL scripts provide examples for each of these areas.</span></span>

### <a name="prerequisites-for-examples"></a><span data-ttu-id="2be53-110">Krav för exempel</span><span class="sxs-lookup"><span data-stu-id="2be53-110">Prerequisites for examples</span></span>

<span data-ttu-id="2be53-111">Varje exempel är baserad på två krav: kallas för en asymmetrisk nyckel från nyckelvalvet **CONTOSO_KEY** och kallas för en autentiseringsuppgift som skapas av funktionen AKV-integreringen **Azure_EKM_TDE_cred**.</span><span class="sxs-lookup"><span data-stu-id="2be53-111">Each example is based on the two prerequisites: an asymmetric key from your key vault called **CONTOSO_KEY** and a credential created by the AKV Integration feature called **Azure_EKM_TDE_cred**.</span></span> <span data-ttu-id="2be53-112">Följande Transact-SQL-kommandon installationsprogrammet förutsättningarna för att köra exemplen.</span><span class="sxs-lookup"><span data-stu-id="2be53-112">The following Transact-SQL commands setup these prerequisites for running the examples.</span></span>

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

### <a name="transparent-data-encryption-tde"></a><span data-ttu-id="2be53-113">Transparent datakryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="2be53-113">Transparent Data Encryption (TDE)</span></span>

1. <span data-ttu-id="2be53-114">Skapa en SQL Server-inloggning som ska användas av databasmotorn för TDE och sedan lägga till autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2be53-114">Create a SQL Server login to be used by the Database Engine for TDE, then add the credential to it.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
   GO
   ```

1. <span data-ttu-id="2be53-115">Skapa krypteringsnyckeln för databasen som ska användas för TDE.</span><span class="sxs-lookup"><span data-stu-id="2be53-115">Create the database encryption key that will be used for TDE.</span></span>

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a><span data-ttu-id="2be53-116">Krypterad säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="2be53-116">Encrypted backups</span></span>

1. <span data-ttu-id="2be53-117">Skapa en SQL Server-inloggning som ska användas av databasmotorn för kryptering av säkerhetskopieringar och Lägg till autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="2be53-117">Create a SQL Server login to be used by the Database Engine for encrypting backups, and add the credential to it.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
   GO
   ```

1. <span data-ttu-id="2be53-118">Säkerhetskopiera databasen att ange kryptering med den asymmetriska nyckeln lagras i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2be53-118">Backup the database specifying encryption with the asymmetric key stored in the key vault.</span></span>

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a><span data-ttu-id="2be53-119">Kryptering på kolumnen (radera)</span><span class="sxs-lookup"><span data-stu-id="2be53-119">Column Level Encryption (CLE)</span></span>

<span data-ttu-id="2be53-120">Det här skriptet skapar en symmetrisk nyckel som skyddas av den asymmetriska nyckeln i nyckelvalvet och använder sedan den symmetriska nyckeln för att kryptera data i databasen.</span><span class="sxs-lookup"><span data-stu-id="2be53-120">This script creates a symmetric key protected by the asymmetric key in the key vault, and then uses the symmetric key to encrypt data in the database.</span></span>

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a><span data-ttu-id="2be53-121">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2be53-121">Additional resources</span></span>

<span data-ttu-id="2be53-122">Mer information om hur du använder dessa krypteringsfunktioner finns [med hjälp av EKM med SQL Server-krypteringsfunktioner](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span><span class="sxs-lookup"><span data-stu-id="2be53-122">For more information on how to use these encryption features, see [Using EKM with SQL Server Encryption Features](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span></span>

<span data-ttu-id="2be53-123">Observera att anvisningarna i den här artikeln förutsätter att du redan har SQL Server körs på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="2be53-123">Note that the steps in this article assume that you already have SQL Server running on an Azure virtual machine.</span></span> <span data-ttu-id="2be53-124">Om inte, se [etablera en virtuell dator med SQL Server i Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="2be53-124">If not, see [Provision a SQL Server virtual machine in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="2be53-125">Andra anvisningar om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2be53-125">For other guidance on running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>