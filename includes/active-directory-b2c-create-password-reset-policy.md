<span data-ttu-id="a494f-101">Om du vill kunna aktivera detaljerad lösenordsåterställning i programmet måste du skapa en princip för återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="a494f-101">To enable fine-grained password reset on your application, you will need to create a password reset policy.</span></span> <span data-ttu-id="a494f-102">Observera att alternativet för lösenordsåterställning för hela klientorganisationen beskrivs [här](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="a494f-102">Note that the tenant-wide password reset option specified [here](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span></span> <span data-ttu-id="a494f-103">Principen beskriver hur lösenordsåterställningen går till för konsumenterna och innehållet i de token som programmet tar emot vid genomförda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="a494f-103">This policy describes the experiences that the consumers will go through during password reset and the contents of tokens that the application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="a494f-104">I principavsnittet i inställningar väljer du **Principer för lösenordsåterställning** och klickar på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a494f-104">In the policies section of settings, select **Password reset policies** and click **+ Add**.</span></span>

![Välj registrerings- eller inloggningsprinciper och klicka på Lägg till](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

<span data-ttu-id="a494f-106">Ange ett **namn** på principen för programmet.</span><span class="sxs-lookup"><span data-stu-id="a494f-106">Enter a policy **Name** for your application to reference.</span></span> <span data-ttu-id="a494f-107">Ange till exempel `SSPR`.</span><span class="sxs-lookup"><span data-stu-id="a494f-107">For example, enter `SSPR`.</span></span>

<span data-ttu-id="a494f-108">Välj **Identitetsprovidrar** och markera **Återställ lösenord med e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="a494f-108">Select **Identity providers** and check **Reset password using email address**.</span></span> <span data-ttu-id="a494f-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a494f-109">Click **OK**.</span></span>

![Välj återställning av lösenord med e-postadress som identitetsprovider och klicka på OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

<span data-ttu-id="a494f-111">Välj **Programanspråk**.</span><span class="sxs-lookup"><span data-stu-id="a494f-111">Select **Application claims**.</span></span> <span data-ttu-id="a494f-112">Välj anspråk som du vill ska returneras i de auktoriseringstoken som skickas tillbaka till programmet efter en genomförd återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="a494f-112">Choose claims you want returned in the authorization tokens sent back to your application after a successful password reset experience.</span></span> <span data-ttu-id="a494f-113">Välj till exempel **Användarobjekt-id**.</span><span class="sxs-lookup"><span data-stu-id="a494f-113">For example, select **User's Object ID**.</span></span>

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

<span data-ttu-id="a494f-115">Klicka på **Skapa** för att lägga till principen.</span><span class="sxs-lookup"><span data-stu-id="a494f-115">Click **Create** to add the policy.</span></span> <span data-ttu-id="a494f-116">Principen visas i listan som **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="a494f-116">The policy is listed as **B2C_1_SSPR**.</span></span> <span data-ttu-id="a494f-117">Prefixet **B2C_1_** läggs till i namnet.</span><span class="sxs-lookup"><span data-stu-id="a494f-117">The **B2C_1_** prefix is appended to the name.</span></span>

<span data-ttu-id="a494f-118">Öppna principen genom att välja **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="a494f-118">Open the policy by selecting **B2C_1_SSPR**.</span></span> <span data-ttu-id="a494f-119">Kontrollera inställningarna i tabellen och klicka på **Kör nu**.</span><span class="sxs-lookup"><span data-stu-id="a494f-119">Verify the settings specified in the table then click **Run now**.</span></span>

![Välj princip och kör den](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| <span data-ttu-id="a494f-121">Inställning</span><span class="sxs-lookup"><span data-stu-id="a494f-121">Setting</span></span>      | <span data-ttu-id="a494f-122">Värde</span><span class="sxs-lookup"><span data-stu-id="a494f-122">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="a494f-123">**Program**</span><span class="sxs-lookup"><span data-stu-id="a494f-123">**Applications**</span></span> | <span data-ttu-id="a494f-124">Contoso B2C-app</span><span class="sxs-lookup"><span data-stu-id="a494f-124">Contoso B2C app</span></span> |
| <span data-ttu-id="a494f-125">**Välj svarswebbadress**</span><span class="sxs-lookup"><span data-stu-id="a494f-125">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="a494f-126">En ny webbläsarflik öppnas och du kan kontrollera hur lösenordsåterställningen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a494f-126">A new browser tab opens, and you can verify the password reset consumer experience in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="a494f-127">Det kan ta någon minut att skapa en princip och innan uppdateringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="a494f-127">It takes up to a minute for policy creation and updates to take effect.</span></span>
>
