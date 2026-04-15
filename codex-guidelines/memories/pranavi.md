# Claude Memory: pranavi

> Migrated from Claude Memories via ccmigrate.

**Work context**

Pranavi works at CloudFuze, a SaaS company offering two core products: CloudFuze Migrate and CloudFuze Manage. Her work involves creating sales and demo materials, webinar presentations, and interactive product demos, suggesting a role at the intersection of product marketing, sales enablement, or solutions engineering.

**Personal context**

No personal context has been shared outside of work.

**Top of mind**

Pranavi recently completed a major push for a March 18 webinar titled "From Migration to Governance: How to Modernize Microsoft 365 with CloudFuze." Deliverables included slide modifications to `CloudFuze_Mar18_Webinar_V2.pptx`, a webinar-specific demo script for the CloudFuze Manage console (sales.cloudfuzehost.com), and an interactive HTML mock dashboard. The demo narrative centers on a post-migration CIO discovering governance failures ahead of a Microsoft Copilot deployment, structured around four pillars: Discover & Audit, Remediate & Enforce, Control Shadow IT & AI, and Lifecycle & Workflows.

**Brief history**

*Recent months*

Pranavi systematically walked Claude through every page of the CloudFuze Manage platform via screenshots for a UI recreation project, with an explicit standing instruction to retain all screenshots and pages from sales.cloudfuzehost.com. Pages documented include:

- **Dashboard**: KPI cards, donut chart, bar charts, drill-downs to Spent Analytics and App Insights
- **Integrations**: Add Applications grid, Manage Applications table, SSO app discovery via Entra ID and Okta
- **Applications**: Approved Apps and Shadow Apps (detected via Microsoft/Google OAuth logs), split-panel user drill-down
- **SaaS Management detail view**: User Management, Group Management, License Management (with AI-powered Invoice Parser), Domains, Download Reports
- **Data Dashboard**: Data Sprawl metrics, platform comparison, Data Deep Drive file explorer with sensitivity labels and collaborator management, Data Policy for external sharing alerts and stale content
- **Manage Workflows**: Visual flowchart builder triggered from identity providers (Entra ID)
- **Browser Activity**: Browser extension-based shadow IT detection feed
- **AI Management**: Specialized view for tools like Microsoft Copilot and Cursor, with usage metrics (lines added, tabs accepted, model usage, day-wise per-user breakdowns)
- **Copilot Hub page**: Dark navy sidebar (#0E2A5C); KPI cards (Total Users, Active Users, Monthly Spend, Avg Productivity); Tool Usage Distribution donut (Teams 22, Outlook 20, Word 18, Excel 15, Copilot Chat 10, PowerPoint 8, Teams Copilot 4, Loop 2, OneNote 1); Adoption by Department donut (Engineering 14, Sales 10, Marketing 8, Operations 5, Finance 4, HR 3); Daily Usage Pattern with Quality Metrics; Usage Trend sections

Consistent design patterns across the platform: dark navy sidebar and header, white cards, split-panel master-detail interactions, pill badges, toggles, and stat cards at the top of each page.

For the webinar, Pranavi built demo materials using a contoso.com demo tenant with M365-specific apps (Entra ID, SharePoint, OneDrive, Teams, Copilot, Azure DevOps, Dynamics 365, Power BI, Intune) and shadow apps detected via OAuth (Notion, Slack, ChatGPT, Canva, Zoom). Key demo data points: 847 overshared files, 312 public links, 1,200 external shares, 3,247 stale permissions, and the file "Q4_Salary_Review_2026.xlsx" shared with "Everyone except external users" as the central Copilot risk example. The React mock dashboard was built as a single JSX file with full page navigation and live interactivity.

**Other instructions**

- Pranavi wants Claude to remember all screenshots and pages shared from sales.cloudfuzehost.com for an ongoing UI recreation project. All platform pages and design details shared should be retained and available for future reference.
