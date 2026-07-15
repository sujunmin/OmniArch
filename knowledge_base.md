# OmniArch Knowledge Base: GCP Architect & Partner Operations

## Section 1: Identity & Access Management (Google OIDC / SSO)
Government and enterprise clients often request strict security measures (like forced MFA) that conflict with standard Google SSO behaviors. 

* **Forcing Re-authentication/MFA:** * Google's OIDC implementation **does not support** the `prompt=login` parameter. 
  * `max_age=0` only checks the authentication time; it does not guarantee a password or MFA prompt if Google deems the session secure.
  * **Revoking Tokens:** Revoking a token only removes app authorization. It does not kill the user's browser session. Re-authenticating after a revoke will only prompt the "Consent Screen," not the password/MFA screen.
* **Interrupting Silent/Seamless SSO:** * Using `prompt=select_account` forces the user to click their profile, interrupting silent auto-login.
  * Using `prompt=consent` forces the authorization screen. 
  * Neither parameter enforces a password or MFA challenge.
* **Standard Workaround (App-Level Step-Up Authentication):** The best practice for high-security applications is to decouple identity from authorization. Rely on Google for Identity (SSO), but enforce a second layer of defense within the application (e.g., SMS OTP, PIN code, or FIDO2/Passkey) before granting session access.
* **SAML Alternative:** For internal employees using Google Workspace, standard SAML 2.0 with the `ForceAuthn="true"` attribute can be utilized to force password re-entry.

## Section 2: Statement of Work (SOW) & PSF Funding Audits
Google Partner Service Funds (PSF) require strict formatting and terminology to pass the approval process (`psfapproversAPAC@google.com`).

* **Effort and Capacity:** PSF does not fund "capacity" or "effort" (e.g., man-days). **Rule:** Always remove the "Effort (Days)" column from the SOW. Shift the focus entirely to concrete "Deliverables." Effort sizing must be moved to a separate "Fair Market Value (FMV)" spreadsheet appendix.
* **Volumetrics:** SOWs must contain quantitative infrastructure estimates to justify the funding and the target Annual Recurring Revenue (ARR). 
  * *Example Baseline (500 peak concurrent users / $420k ARR):* 3 to 9 GKE nodes (e2-standard-8), 1 TB Cloud SQL High-Availability instance, 5 TB Cloud Storage.
* **Tax Inclusions:** The SOW financial section must explicitly state: "All PSF funds are inclusive of local taxes."
* **Project Timelines:** Timeline clauses must include the caveat: "pending acceptance of all deliverables."
* **GCP Product Terminology:** Do not use generic cloud terms. Replace "managed database" with "Cloud SQL" or "AlloyDB". Replace "WebAPI server" with "Google Compute Engine (GCE)" or "Google Kubernetes Engine (GKE)".

## Section 3: Vendor Operations & Billing Support
When escalating issues to Google Sales or Partner Support, exact terminology is required to prevent misunderstandings.

* **Payment ID vs. Billing Account ID:** Google Cloud Console Billing (where projects reside) is different from the Google Sales/Partner Payment Profile.
* **Common Issue:** Sales requests a "Billing Account ID" for a new quote (e.g., Google Threat Intelligence) under a specific Payment Profile. However, existing IDs might be tied to other active customer subscriptions.
* **Resolution Strategy:** Open a support case explicitly stating that the issue is within the **Sales/Payment Profile system**, not a GCP Console error. Request guidance on whether to map an existing ID safely or provision a new dedicated sub-account to avoid disrupting active services.
