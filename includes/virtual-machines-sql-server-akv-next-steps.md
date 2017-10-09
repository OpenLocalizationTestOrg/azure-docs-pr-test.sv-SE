## <a name="next-steps"></a>Nästa steg

När du har aktiverat Azure Key Vault-integrering kan du aktivera SQL Server-kryptering på din SQL-VM. Först måste toocreate en asymmetrisk nyckel i nyckelvalvet och en symmetrisk nyckel i SQL Server på den virtuella datorn. Du kommer sedan att kan tooexecute T-SQL-instruktioner tooenable kryptering för dina databaser och säkerhetskopieringar.

Det finns flera typer av kryptering som du kan dra nytta av:

* [Transparent datakryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Krypterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449489.aspx)
* [Kryptering på kolumnen (radera)](https://msdn.microsoft.com/library/ms173744.aspx)

hello innehåller följande Transact-SQL-skript exempel för dessa olika områden.

### <a name="prerequisites-for-examples"></a>Krav för exempel

Varje exempel är baserad på hello två krav: kallas för en asymmetrisk nyckel från nyckelvalvet **CONTOSO_KEY** och en autentiseringsuppgift som skapats av hello AKV-integreringen funktion som kallas **Azure_EKM_TDE_cred**. hello installationsprogrammet följande Transact-SQL-kommandon förutsättningarna för att köra hello exempel.

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

### <a name="transparent-data-encryption-tde"></a>Transparent datakryptering (TDE)

1. Skapa en toobe för SQL Server-inloggning som används av hello databasmotorn för TDE och lägger till hello autentiseringsuppgifter tooit.

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

1. Skapa hello databaskrypteringsnyckeln som ska användas för TDE.

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

### <a name="encrypted-backups"></a>Krypterad säkerhetskopiering

1. Skapa en toobe för SQL Server-inloggning som används av hello databasmotorn för kryptering av säkerhetskopieringar och Lägg till hello autentiseringsuppgifter tooit.

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

1. Säkerhetskopiering hello databas ange kryptering med hello asymmetrisk nyckel som lagras i hello nyckelvalvet.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   tooDISK = N'[PATH tooBACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Kryptering på kolumnen (radera)

Det här skriptet skapar en symmetrisk nyckel som skyddas av hello asymmetrisk nyckel i hello nyckelvalvet och använder sedan hello symmetrisk nyckel tooencrypt data i hello-databasen.

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

## <a name="additional-resources"></a>Ytterligare resurser

Mer information om hur toouse krypteringsfunktionerna Se [med hjälp av EKM med SQL Server-krypteringsfunktioner](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Observera att hello stegen i den här artikeln förutsätter att du redan har SQL Server körs på en virtuell Azure-dator. Om inte, se [etablera en virtuell dator med SQL Server i Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md). Andra anvisningar om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).
