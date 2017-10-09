## <a name="using-vault-credentials-tooauthenticate-with-hello-azure-backup-service"></a>Med valvet autentiseringsuppgifter tooauthenticate med hello Azure Backup-tjänsten
hello lokal server (Windows-klient eller server för Windows Server eller Data Protection Manager) måste autentiseras med ett säkerhetskopieringsvalv innan den kan säkerhetskopiera data tooAzure toobe. hello autentisering uppnås med hjälp av ”valvet autentiseringsuppgifter”. hello begreppet valvautentiseringsuppgifter är liknande toohello begreppet ”Publiceringsinställningar” fil som används i Azure PowerShell.

### <a name="what-is-hello-vault-credential-file"></a>Vad är hello valvautentiseringsfilen?
Hej valvautentiseringsfilen är ett certifikat som genereras av hello portal för varje säkerhetskopieringsvalvet. hello portal sedan överför hello offentliga nyckel toohello Access Control Service (ACS). hello privata nyckeln för certifikatet för hello görs tillgängliga toohello användaren som en del av hello arbetsflöde som angetts som indata i hello arbetsflöde för registrering av datorn. Detta autentiserar hello datorn toosend säkerhetskopieringsdata tooan identifieras valvet i hello Azure Backup-tjänsten.

Hej valvautentiseringen används endast under hello arbetsflöde för registrering. Det är hello användarens ansvar tooensure som hello valvautentiseringsuppgifter filen inte har komprometterats. Om den hamnar i hello händer en obehörig användare hello valvautentiseringsfilen kan vara används tooregister andra datorer mot hello samma valvet. Dock som hello säkerhetskopierade data krypteras med en lösenfras som tillhör toohello kunden, kan inte befintlig säkerhetskopierad data vara hotad. toomitigate problem, valvautentiseringsuppgifter ställs tooexpire i 48hrs. Du kan hämta hello valvet autentiseringsuppgifterna för ett säkerhetskopieringsvalv valfritt antal gånger – men endast hello senaste valvautentiseringsfilen gäller under hello arbetsflöde för registrering.

### <a name="download-hello-vault-credential-file"></a>Hämta valvautentiseringsfilen hello
Hej valvautentiseringsfilen hämtas via en säker kanal från hello Azure-portalen. hello Azure Backup-tjänsten inte känner till hello privat nyckel för hello certifikat och hello privata nyckeln beständig inte i hello-portalen eller hello-tjänsten. Använd hello följande steg toodownload hello valvet autentiseringsuppgifter filen tooa lokal dator.

1. Logga in toohello [hanteringsportalen](https://manage.windowsazure.com/)
2. Klicka på **återställningstjänster** i hello vänstra navigeringsfönstret och välj hello säkerhetskopieringsvalv som du har skapat. Klicka på hello molnet ikonen tooget toohello Snabbstart vy över hello säkerhetskopieringsvalvet.
   
   ![Snabb överblick](./media/backup-download-credentials/quickview.png)
3. På hello Snabbstart klickar du på **ladda ned valvautentiseringsuppgifter**. hello portal genererar hello valvautentiseringsfilen, vilket görs tillgänglig för hämtning.
   
   ![Ladda ned](./media/backup-download-credentials/downloadvc.png)
4. hello portal kommer att generera en valvautentiseringen med hjälp av en kombination av hello valvnamnet och hello aktuellt datum. Klicka på **spara** toodownload hello valvet autentiseringsuppgifter toohello lokala kontots hämtar mappen eller välj Spara som från hello spara menyn toospecify en plats för hello valvautentiseringsuppgifter.

### <a name="note"></a>Obs!
* Se till att hello valvautentiseringsuppgifter sparas på en plats som kan nås från datorn. Om den är lagrad i en fil resursen/SMB Kontrollera hello åtkomstbehörighet.
* Hej valvautentiseringsfilen används endast under hello arbetsflöde för registrering.
* Hej valvautentiseringsfilen upphör att gälla efter 48hrs och kan hämtas från hello-portalen.
* Se toohello Azure Backup [vanliga frågor och svar](../articles/backup/backup-azure-backup-faq.md) för frågor i hello arbetsflödet.

