Varje klientdator som ansluter tooa VNet med punkt-till-plats måste ha ett certifikat installerat. hello klientcertifikat genereras från hello rotcertifikat och installeras på varje klientdator. Om ett giltigt klientcertifikat inte har installerats och hello klienten försöker tooconnect toohello VNet, misslyckas autentiseringen.

Du kan antingen generera ett unikt certifikat för varje klient eller du kan använda hello samma certifikat för flera klienter. hello nytta toogenerating unika klientcertifikat är hello möjlighet toorevoke ett enskilt certifikat. Annars, om flera klienter med hjälp av hello samma klientcertifikatet och du behöver toorevoke, du har toogenerate och installerar nya certifikat för alla hello klienter som använder detta certifikat tooauthenticate.

Du kan generera klientcertifikat med hjälp av hello följande metoder:

- **Företagscertifikat:**

  - Om du använder en företagslösning för certifikat, generera ett klientcertifikat med hello vanliga värdet namnformat 'name@yourdomain.com', i stället för hello ”domännamn\användarnamn” format.
  - Kontrollera att hello klientcertifikat baserat på certifikatmallen för hello ”användare” som har klienten-autentisering som hello första objektet i listan för hello används i stället Smart Card inloggning, osv. Du kan kontrollera hello certifikat genom att dubbelklicka på hello klientcertifikatet och visa **information > förbättrad nyckelanvändning**.

- **Självsignerade rotcertifikat:** är det viktigt att du följer hello stegen i ett av hello P2S certifikat artiklarna nedan. Annars hello klientcertifikat som du skapar är inte kompatibla med P2S-anslutningar och klienter tar emot ett fel vid försök tooconnect. hello stegen i något av följande artiklar hello generera ett kompatibelt klientcertifikat: 

  * [Windows 10 PowerShell-instruktioner](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): dessa instruktioner kräver Windows 10 och PowerShell toogenerate certifikat. hello-certifikat som genereras kan installeras på alla P2S-klient som stöds.
  * [MakeCert instruktioner](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Använd MakeCert om du inte har åtkomst tooa Windows 10 toouse toogenerate certifikat. MakeCert föråldrad, men du kan fortfarande använda MakeCert toogenerate certifikat. hello-certifikat som genereras kan installeras på alla P2S-klient som stöds.

  När du skapar ett klientcertifikat från ett självsignerat rotcertifikat med hello föregående anvisningar har den installeras automatiskt på hello-dator som du använde toogenerate den. Om du vill tooinstall ett klientcertifikat på en annan klientdator tooexport måste den som en PFX-fil, tillsammans med hello hela certifikatkedjan. Detta skapar en .pfx-fil som innehåller hello root certifikatinformation som krävs för att autentisera hello klienten toosuccessfully. Steg tooexport ett certifikat, se [certifikat - exportera ett certifikat för](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).
