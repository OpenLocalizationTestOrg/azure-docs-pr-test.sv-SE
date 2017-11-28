<span data-ttu-id="e1d55-101">Du aktiverar profilredigering i programmet genom att först skapa en princip för redigering av profil.</span><span class="sxs-lookup"><span data-stu-id="e1d55-101">To enable profile editing on your application, you will need to create a profile editing policy.</span></span> <span data-ttu-id="e1d55-102">Principen beskriver hur profilredigering går till för konsumenterna och innehållet i de token som programmet tar emot när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e1d55-102">This policy describes the experiences that consumers will go through during profile editing and the contents of tokens that the application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="e1d55-103">I principavsnittet i inställningar väljer du **Profilredigeringsprinciper** och klickar på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-103">In the policies section of settings, select **Profile editing policies** and click **+ Add**.</span></span>

![Välj Profilredigeringsprinciper och klicka på Lägg till](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

<span data-ttu-id="e1d55-105">Ange ett **namn** på principen för programmet.</span><span class="sxs-lookup"><span data-stu-id="e1d55-105">Enter a policy **Name** for your application to reference.</span></span> <span data-ttu-id="e1d55-106">Ange till exempel `SiPe`.</span><span class="sxs-lookup"><span data-stu-id="e1d55-106">For example, enter `SiPe`.</span></span>

<span data-ttu-id="e1d55-107">Välj **Identitetsprovidrar** och markera **Inloggning från lokalt konto**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-107">Select **Identity providers** and check **Local Account Signin**.</span></span> <span data-ttu-id="e1d55-108">Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="e1d55-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="e1d55-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-109">Click **OK**.</span></span>

![Välj Inloggning från lokalt konto som identitetsprovider och klicka på OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

<span data-ttu-id="e1d55-111">Välj **Profilattribut**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-111">Select **Profile attributes**.</span></span> <span data-ttu-id="e1d55-112">Välj attribut som användarna kan visa och redigera i sina profiler.</span><span class="sxs-lookup"><span data-stu-id="e1d55-112">Choose attributes the consumer can view and edit in their profile.</span></span> <span data-ttu-id="e1d55-113">Välj till exempel **land/region**, **visningsnamn** och **postnummer**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="e1d55-114">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-114">Click **OK**.</span></span>

![Välj några attribut och klicka på OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

<span data-ttu-id="e1d55-116">Välj **Programanspråk**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-116">Select **Application claims**.</span></span> <span data-ttu-id="e1d55-117">Välj anspråk som du vill ska returneras i de auktoriseringstoken som skickas tillbaka till programmet efter en slutförd redigering av profil.</span><span class="sxs-lookup"><span data-stu-id="e1d55-117">Choose claims you want returned in the authorization tokens sent back to your application after a successful profile editing experience.</span></span> <span data-ttu-id="e1d55-118">Välj till exempel **Visningsnamn**, **Postnummer**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-118">For example, select **Display Name**, **Postal Code**.</span></span>

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

<span data-ttu-id="e1d55-120">Klicka på **Skapa** för att lägga till principen.</span><span class="sxs-lookup"><span data-stu-id="e1d55-120">Click **Create** to add the policy.</span></span> <span data-ttu-id="e1d55-121">Principen visas i listan som **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-121">The policy is listed as **B2C_1_SiPe**.</span></span> <span data-ttu-id="e1d55-122">Prefixet **B2C_1_** läggs till i namnet.</span><span class="sxs-lookup"><span data-stu-id="e1d55-122">The **B2C_1_** prefix is appended to the name.</span></span>

<span data-ttu-id="e1d55-123">Öppna principen genom att välja **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-123">Open the policy by selecting **B2C_1_SiPe**.</span></span> <span data-ttu-id="e1d55-124">Kontrollera inställningarna i tabellen och klicka på **Kör nu**.</span><span class="sxs-lookup"><span data-stu-id="e1d55-124">Verify the settings specified in the table then click **Run now**.</span></span>

![Välj princip och kör den](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| <span data-ttu-id="e1d55-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="e1d55-126">Setting</span></span>      | <span data-ttu-id="e1d55-127">Värde</span><span class="sxs-lookup"><span data-stu-id="e1d55-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="e1d55-128">**Program**</span><span class="sxs-lookup"><span data-stu-id="e1d55-128">**Applications**</span></span> | <span data-ttu-id="e1d55-129">Contoso B2C-app</span><span class="sxs-lookup"><span data-stu-id="e1d55-129">Contoso B2C app</span></span> |
| <span data-ttu-id="e1d55-130">**Välj svarswebbadress**</span><span class="sxs-lookup"><span data-stu-id="e1d55-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="e1d55-131">En ny webbläsarflik öppnas och du kan kontrollera hur profilredigeringen går till.</span><span class="sxs-lookup"><span data-stu-id="e1d55-131">A new browser tab opens, and you can verify the profile editing consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="e1d55-132">Det kan ta någon minut att skapa en princip och innan uppdateringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="e1d55-132">It takes up to a minute for policy creation and updates to take effect.</span></span>
>