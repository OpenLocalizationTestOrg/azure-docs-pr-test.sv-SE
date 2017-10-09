
### <a name="installation-failures"></a>Installationsfel
| **Exempel på felmeddelande** | **Rekommenderad åtgärd** |
|--------------------------|------------------------|
|FEL misslyckades tooload konton. Fel: System.IO.IOException: Det gick inte tooread data från hello transport anslutningen när installera och registrera hello CS-server.| Kontrollera att TLS 1.0 är aktiverat på hello-dator. |

### <a name="registration-failures"></a>Registreringsfel
Registreringsfel kan felsökas genom att granska hello loggar i hello **%ProgramData%\ASRLogs** mapp.

| **Exempel på felmeddelande** | **Rekommenderad åtgärd** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Meddelande: ACS50008: SAML-token är ogiltig.<br>Spårnings-ID: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Korrelations-ID: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Tidsstämpel: **2016-12-12 14:50:08Z<br>** | Kontrollera att hello tid på att din systemklocka inte är mer än 15 minuter av hello lokal tid. Kör hello installer toocomplete hello registrering.|
|**09:35:27** : DRRegistrationException uppstod tooget alla disaster recovery-valvet för hello valt certifikat:: utlöste Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: SAML-token är ogiltig.<br>Spårnings-ID: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Korrelations-ID: abe9deb8-3e64-464d-8375-36db9816427a<br>Tidsstämpel: **2016-05-19 01:35:39Z**<br> | Kontrollera att hello tid på att din systemklocka inte är mer än 15 minuter av hello lokal tid. Kör hello installer toocomplete hello registrering.|
|06:28:45: misslyckade toocreate certifikat<br>06:28:45: Installationen måste avbrytas. Ett certifikat krävs tooauthenticate tooSite återställning inte kan skapas. Kör installationen igen | Se till att du kör installationen som lokal administratör. |
