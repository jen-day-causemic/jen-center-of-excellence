# Integration users and licenses

### Which profile should an integration user get?

**Type:** Tip
**Answer:** Salesforce's current baseline is the **Minimum Access – API Only Integrations** profile with the Salesforce Integration user license. It's true least privilege: no access to any object by default, so you assign permission sets for exactly the objects the integration needs. The older **Salesforce API Only** profile had broad create/read/edit/delete on everything: easier, but not least privilege.
**Why it matters:** Use the newer profile on new integrations. Expect to build a permission set per integration, and expect some vendors to say they don't support it; check product files for workarounds (e.g. [Fundraise Up](../products/fundraise-up.md)).
**Who knows:** Roger Miller, Shanks
**Source:** #techies, 2026-11-05 (https://causemicworkspace.slack.com/archives/C03TZULPCJ0/p1793906677793809)
**Captured:** 2026-11-05
