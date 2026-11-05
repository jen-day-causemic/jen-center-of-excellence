# Fundraise Up

**Works with:** any CRM. Native integrations with Salesforce and HubSpot ([HubSpot docs](https://fundraiseup.com/docs/hubspot/)).

### Does Fundraise Up work with HubSpot?

**Type:** Product news
**Answer:** Yes, a native integration since November 2021. Donations sync as Deals (in a "Fundraise Up Donations" pipeline) linked to Contacts, and recurring plans go to HubSpot's Subscriptions object. Needs a paid HubSpot plan, HubSpot admin access, and a spare pipeline. You can connect more than one HubSpot environment, which helps with sandbox testing. The HubSpot Marketplace listing says the Fundraise Up Scale or Enterprise plan is required; Fundraise Up's own page calls it free, so check the client's plan.
**Why it matters:** Fundraise Up is an option for HubSpot clients too, not just Salesforce clients.
**Source:** [Fundraise Up HubSpot docs](https://fundraiseup.com/docs/hubspot/), [HubSpot integration FAQ](https://fundraiseup.com/support/hubspot-integration-faq/), [HubSpot Marketplace listing](https://ecosystem.hubspot.com/marketplace/listing/fundraise-up-by-fundraise-up-inc-395241)
**Captured:** 2026-11-05

### Can Fundraise Up connect using the Minimum Access – API Only Integrations profile?

**Type:** Tip
**Answer:** Yes, with a workaround, even though Fundraise Up support told a client it isn't compatible. Give the integration user a full Salesforce license, connect Fundraise Up through its UI, then switch the user to the Salesforce Integration license. The connection keeps working. The user also needs permission sets for the objects Fundraise Up writes to (see [integration users](../salesforce/integration-users.md)).
**Why it matters:** Lets us follow Salesforce's least-privilege recommendation and avoid spending a full license on the integration. The catch: you repeat the dance every time you reconnect, so it's painful if you often switch Fundraise Up between sandbox and production.
**Example:** AHS (Fundraise Up support's "not compatible" answer, Nov 2026); Oregon Humane (connected on the older Salesforce API Only profile)
**Who knows:** Roger Miller, Shanks, Elizabeth Carena
**Source:** #techies, 2026-11-05 (https://causemicworkspace.slack.com/archives/C03TZULPCJ0/p1793906677793809)
**Captured:** 2026-11-05
