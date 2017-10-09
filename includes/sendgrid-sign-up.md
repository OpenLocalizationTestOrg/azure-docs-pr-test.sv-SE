Azure-kunder kan låsa upp 25 000 kostnadsfria e-postmeddelanden varje månad. Dessa 25 000 ledigt månatliga e-postmeddelanden ger dig åtkomst tooadvanced rapportering och analys och [alla API: er] [ all APIs] (webb-, SMTP-, händelse, Parse och mer). Information om ytterligare tjänster som tillhandahålls av SendGrid finns hello [SendGrid lösningar] [ SendGrid Solutions] sidan.

### <a name="toosign-up-for-a-sendgrid-account"></a>toosign för ett SendGrid-konto
1. Logga in toohello [Azure-hanteringsportalen][Azure Management Portal].
2. Hello-menyn hello vänster **ny**.

    ![command-bar-new][command-bar-new]
3. Klicka på **Tillägg** och sedan **SendGrid Email Delivery** (E-postleverans med SendGrid).

    ![sendgrid-store][sendgrid-store]
4. Avsluta hello signup formuläret och markera **skapa**.

    ![sendgrid-create][sendgrid-create]
5. Ange en **namn** tooidentify din SendGrid-tjänsten i din Azure-inställningar. Namnet måste vara mellan 1 och 100 tecken långt och får bara innehålla alfanumeriska tecken, bindestreck, punkter och understreck. hello-namnet måste vara unikt i din lista över objekt de prenumererar på Azure Store.
6. Ange och bekräfta ett **lösenord**.
7. Välj din **prenumeration**.
8. Skapa en ny **resursgrupp** eller välj en befintlig.
9. I hello **prisnivå** avsnittet Välj hello SendGrid plan som du vill toosign dig för.

    ![sendgrid-pricing][sendgrid-pricing]
10. Ange en **kampanjkod** om du har en.
11. Ange dina **kontaktuppgifter**.
12. Granska och Godkänn hello **juridiska villkor**.
13. När du har bekräftat köpet visas en **distributionen lyckades** popup-fönster och du ser ditt konto som anges i hello **alla resurser** avsnitt.

    ![all-resources][all-resources]

    När du har slutfört köpet och klickar på hello **hantera** knappen verifieringsprocessen tooinitiate hello e-post, du får ett e-postmeddelande från SendGrid ber dig tooverify ditt konto. Om du inte får det här e-postmeddelandet eller får problem när du ska verifiera ditt konto kan du läsa dessa vanliga frågor och svar.

    ![manage][manage]

    **Du kan bara skicka in too100 e-postmeddelanden och dag förrän du har verifierat ditt konto.**

    toomodify din prenumeration plan eller se hello SendGrid kontaktinställningar och klickar på din SendGrid service tooopen hello SendGrid Marketplace instrumentpanelen hello.

    ![settings][settings]

    toosend en e-post med SendGrid måste du ange din API-nyckel.

### <a name="toofind-your-sendgrid-api-key"></a>toofind din SendGrid API-nyckel
1. Klicka på **Hantera**.

    ![manage][manage]
2. Markera i instrumentpanelen SendGrid **inställningar** och sedan **API-nycklar** hello menyn hello vänster.

    ![api-keys][api-keys]

3. Klicka på hello **skapa API-nyckel** listrutan och välj **allmänna API-nyckeln**.

    ![general-api-key][general-api-key]
4. Ange hello minst **namnet på den här nyckeln** och ger fullständig åtkomst för**skicka e-post** och välj **spara**.

    ![access][access]
5. Din API-nyckel visas nu en enda gång. Du är säker på att toostore den på ett säkert sätt.

### <a name="toofind-your-sendgrid-credentials"></a>toofind dina inloggningsuppgifter
1. Klicka på hello nyckelikonen toofind din **användarnamn**.

    ![key][key]
2. hello lösenordet är hello något du angav vid installationen. Du kan välja **ändra lösenord** eller **Återställ lösenord** toomake ändringar.

toomanage din e-deliverability klickar du på hello **hantera knappen**. Detta kommer att omdirigera tooyour SendGrid instrumentpanelen.

    ![manage][manage]

    For more information on sending email through SendGrid, visit hello [Email API Overview][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure Management Portal]: https://manage.windowsazure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
