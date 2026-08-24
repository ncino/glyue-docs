# Integration Gateway Release Notes

This page tracks features, bug fixes, and other improvements included in each Integration Gateway release.

## v4.22.0 — August 2026

### New Features

* **Fiserv XP2 Adapter**: Added support for the Fiserv XP2 core banking system
* **nCino Mortgage Adapter**: Added a system-specific adapter for the nCino Mortgage solution

### MCP

* Adds an MCP tool for Claude to search the IG User Docs (`ig_platform_documentation`)
* Adds an MCP overwrite warning for merge conflicts on write tools

### Bug Fixes

* Resolved an issue where the Dashboard page blocked navigation while it loaded

## v4.21.0 — July 2026

### New Features

* **[Integration Gateway Workbench](integration-gateway-workbench/README.md)**: Launched the Integration Gateway Workbench, a visual canvas that displays an integration's components as an interactive node diagram. You can review an integration's full structure directly on the canvas instead of on the Build page. The Workbench is available behind a feature flag. Contact your nCino representative to enable it for your environment.
  * Navigate the canvas with zoom, pan, and minimap controls
  * View color-coded, labeled nodes for the Integration, Service Request, Field Mapping, and Validation Rule components
  * Click any node to open a read-only properties panel that displays its configuration, with an **Open in Build** link to the underlying record
  * Export integration diagrams as SVG, PNG, or JSON files
* **ETL Mapping Import**: Added support for importing ETL field mappings directly from CSV or Excel files
* **SFTP Trust On First Use**: Added Trust On First Use (TOFU) host key verification to the SFTP adapter

### Improvements

* **Integration Permission Bulk Change**: Added a new UI for changing integration permissions in bulk
* **Mapping Page Edit Access**: Added a mapping-page config setting that controls which users and groups can edit directly on the mapping page
* **Mapping Page Default Sort**: Updated the mapping page to default to sorting by Field instead of Status
* **nCino fireCallback**: Updated the nCino `fireCallback` helper to be rate-limit aware

### Bug Fixes

* Resolved an unhelpful 400 error when creating a blank Shared Module
* Fixed missing log lines in the Corelation KeyBridge adapter

## v4.20.0 — June 2026

### New Features

* **Integration Retry**: Launched configurable Integration Retry, which supports both exponential back-off and fixed interval retry schedules
* **SDLC Environment Banner**: Added SDLC environment banner to the IG UI in production environments

### Improvements

* **JXChange OAuth 2.0**: Updated JXChange SOAP adapter to use OAuth 2.0 authentication
* **JXChange WSDL**: Added new WSDL version support for the jXchange adapter
* **Custom Scheduler**: Replaced Celery with a custom scheduler for improved scheduling reliability
* **Django 5.2**: Upgraded Django framework to v5.2
* **Mapping Dashboard**: Improved calculation of Project Status and Fields per Day in the Mapping Dashboard

### Bug Fixes

* Resolved issue where ETL workflow failure emails did not send during validation failures
* Fixed improper formatting in the View Changes diff display

<details>

<summary>v4.19.0 — May 2026</summary>

* **Custom Adapters**: Build and deploy user-defined Python adapters with custom configuration schemas and file field support
* **Mapping Progress Dashboard**: Track field mapping completion across service requests with forecast data, target dates, and status breakdowns
* **Quick Mapping**: Review unmapped fields one at a time through a card-based interface with keyboard shortcuts
* **Salesforce Bulk API v2.0**: Adds Bulk API 2.0 support to the Salesforce adapter
* **Equifax Adapter**: Introduces a new Equifax adapter with multi-scope support
* **Fiserv Optis Adapter**: Introduces a new Fiserv Optis adapter
* **Join Node (ETL)**: Adds a new JOIN node to the ETL workflow builder
* Adds MCP tools for building and managing custom adapters (`custom_adapter_code`, `custom_adapter_config`)
* **CMC Adapter Auth Type 3**: Extends the CMC adapter to support authentication type 3
* **BuildHelper: Salesforce Bulk API v2.0**: Adds BuildHelper metadata for Salesforce Bulk API v2.0
* **BuildHelper: jXchange ReST**: Adds BuildHelper metadata for Jack Henry jXchange ReST services
* **BuildHelper: SymXchange**: Adds BuildHelper metadata for Jack Henry Symitar SymXchange
* Adds descriptive error messages for adapter configurator permissions

</details>

<details>

<summary>v4.18.0 — March 17th, 2026</summary>

* **Git Adapter**: Introduces a new Git adapter
* **BuildHelper: Fiserv Optis**: Adds BuildHelper support for Fiserv Optis
* **Join Node (ETL)**: Adds a new JOIN node to the workflow builder, which enables more complex workflows
* **Integration Version Manager**: Provides a new version deployment mechanism for integrations and integration components
* **SymXchange Adapter**: Supports the new "versionless" URL scheme
* **Autocomplete**: Improves tab completion
* **AI Code Completion**: Switches the build page backend from OpenAI to Anthropic
* Adds a new MCP tool to directly reference BuildHelper adapter metadata
* Adds a new MCP tool for run history search
* Adds a new MCP tool to create and edit Frontends
* Adds a new MCP tool for Integration Gateway administration
* MCP integration read and write tools now support status, tag, and comment fields
* MCP admin page can now auto-generate a Claude-compatible configuration
* Optimizes the MCP Run History tool for reduced context window usage
* Optimizes the MCP build page read tool for reduced context window usage
* Improves dashboard page performance
* Improves log page performance
* Improves run history redaction job performance
* BuildHelper button on the build page loads immediately on page load
* Improves error messages when configuring schedules with insufficient permissions
* `add_run_label` now ignores empty labels rather than throwing an exception
* MCP integration search now works with service requests and field mappings
* Multiple performance, visibility, and usability enhancements

</details>

<details>

<summary>v4.17.0 — December 16th, 2025</summary>

* Launched Shared Modules, a new feature for implementing reusable modules of code across multiple integrations
* Launched MCP tooling capability, which allows for AI agents to perform actions within an Integration Gateway environment. MCP Tools must be enabled by an administrator and include the following:
  * Read Integration
  * Write Integration
  * Search Integration
  * List Integration
  * Execute Integration
  * Delete Integration
* ETL: Added support for multiple Extract and Load nodes in the workflow builder
* ETL: Added multiple performance, visibility, and usability enhancements
* Improved validation and error visibility for the Oauth and REST adapters
* Oauth 2.0 Authentication flow has been adjusted to support Chromium browsers

</details>

<details>

<summary>v4.16.0 — October 21st, 2025</summary>

* GlyuETL: Increased support for the Salesforce Bulk API connector
* GlyuETL: New Creatio API connector
* GlyuETL: Enhanced data schema enforcement
* GlyuETL: Performance and usability enhancements for the HubSpot connector
* AI Capability Enhancement: Added functionality to allow the triggering of integrations via MCP tooling
* Email Alerting: Added a configurable UI for email alerting
* Added support for triggering integrations via Kafka
* Added version 7 compatibility to the FICO Liquid adapter
* Improved performance of the run history page
* Updates to the Fiserv DNA CoreAPI Adapter (Prefixed paths)
* Bugfix: Increased SFTP Adapter reliability
* Bugfix: Missing value mapping sets on the ETL builder

</details>

<details>

<summary>v4.15.0 — July 12th, 2025</summary>

* GlyuETL: UI Overhaul, new node-based ETL workflow editor
* GlyuETL: New Salesforce Bulk API connector
* GlyuETL: Improved efficiency of python transforms and filters.
* Added several new system to the buildhelper service, including Jack Henry Silverlake jXchange and CSI NuPoint Bridge
* Improved rendering of some items in buildhelper
* Run history page data lineage feature displays information about how request payload fields were calculated.
* Salesforce adapter now supports client credential auth flow.
* Fiserv DNA adapter now supports 2025.1
* Improved monitoring of Adapter Config Validation failures
* Bugfix: Improved reliability of job scheduler
* Bugfix: Users added to a group before the group was saved are now persisted.
* Bugfix: Fixed UI issue on migration page that caused difficulty selecting integrations

</details>

<details>

<summary>v4.14.0 — July 1st, 2025</summary>

* Buildhelper UI is streamlined, can be triggered from any integration component
* Catalog now distinguishes "ready to install" integrations
* Integration debug logs can now be filtered by run history ID
* Integration configs are now created automatically
* Integrations may now use the jsonschema package for validation
* Integration namespace now includes the name of the trigger OAuth application
* Sorting and filtering added to adapter config binding admin UI
* GlyuETL: New permission level for limited access
* GlyuETL: New Postgres DB Connector
* GlyuETL: SFTP connector now supports wildcard file matching
* GlyuETL: "Source Preview" feature allows for previewing extract data
* GlyuETL: Hashlib is now available in python transformations and filters
* GlyuETL: Pagination and filtering added to workflow run history page

</details>

<details>

<summary>v4.13.0 — May 27th, 2025</summary>

* New GlyuETL row filter functionality
* GlyuETL Hubspot connector now supports streaming uploads
* GlyuETL Now supports importing/exporting transforms to JSON
* Run History Page now supports filtering by multiple integrations
* Admin page adapter config bindings reorganized
* Diff Page - better messaging for long-running operations

</details>

<details>

<summary>v4.12.0 — April 15th, 2025</summary>

* GlyuETL: New job scheduling page
* GlyuETL: New "Value Mapping" UI
* GlyuETL: New "Run History" UI
* GlyuETL: Improved user experience for running workflows in test mode
* GlyuETL: UI now integrated with Glyue, can switch between Glyue and GlyuETL experience
* GlyuETL: Hubspot connector now supports multi-object loads
* Glyue Frontend integration with Zoom Contact Center
* New Adapter: AMS 360
* New Adapter: SQL Server
* CodeConnect FCM and CodeConnect Horizon Adapter updated to support client cert auth.
* Improved Run History page performance

</details>

<details>

<summary>v4.11.0 — March 4th, 2025</summary>

* New Glyue ETL workflow product
* ETL Hubspot connector introduced
* ETL SFTP connector introduced
* FLEXBridge adapter introduced
* Temenos LOS adapter introduced
* COCC (ConnectSuite) adapter introduced
* Generic OAuth adapter introduced
* Codeconnect Horizon and FCM adapters updated to support certificate-based authentication
* Adapter config bindings may now have the user field set to 'required'
* Splunk logging export support

</details>

<details>

<summary>v4.10.0 — February 5th, 2025</summary>

* Updated naming of banking core adapters for consistency. Adapters will now follow the convention "\[Company] \[Core name] (\[middleware])"
* Added three new adapters, COCC (Core API), LoanPro, and RedTail
* Improved the performance of saving large (>1000 lines) changes on the build page, including when importing large integrations or multiple integrations
* The *Jobs* page has been deprecated, and all jobs have been migrated to the *Scheduler*
* The UX of the *Scheduler* has been modified to more clearly indicate the first time a schedule will be run
* Fixed a bug where changes to an environment's domain wouldn't be reflected in links generated by the environment
* Fixed a bug where users added to an organization during the organization's creation wouldn't be saved

</details>

<details>

<summary>v4.9.0 — November 19th, 2024</summary>

* Run Histories now have a "Code wrap" toggle to better display long lines of text
* Service accounts now have a "No password" option to turn off basic auth if OAuth is being used
* Glyue's Python version has been upgraded to version 3.12
* The build page now automatically trims whitespace on formula variables
* Email capitalization across the platform is now standardized, making it agnostic to user-inputted capitalization
* Fixed a bug that caused mismatched timezones on scheduler
* Changed the default behavior of the Status column on Field mappings, now defaults to "Not Started"

</details>

<details>

<summary>v4.8.0 — September 17th, 2024</summary>

* Introducing the built-in [Glyue Idempotency Layer](idempotency-layer.md), making idempotency protection available to all integrations.
* Introducing [Integration Run Labels](../reference/special_functions/add_run_label.md), which allow dynamic label text to be applied to integrations at run-time, enabling faster searching and tagging of completed integration runs.
* Glyue Admins now have the ability to set a default permissions group for new [regular (non-staff) users](permissions/README.md#user-type).
* Glyue now supports the [OAuth 2.0 Client Credentials](authentication.md#oauth-2.0-client-credentials) authentication flow ("browserless OAuth") for more flexible authentication patterns across our support systems.
* Environments with a [connected SAML (SSO) login method](../how-to-guides/how-to-set-up-single-sign-on-sso) will now automatically create user accounts upon first SSO-based login of a user, removing the need to pre-create user accounts.
* All [Frontends](../how-to-guides/how-to-build-and-deploy-a-custom-frontend) now support SAML (SSO) login.
* Performance of the *Build page* when operating on large selections of rows has been dramatically improved.
* [Value Mappings](../how-to-guides/how-to-create-a-value-mapping-set) now have statuses, and their approval progress is reflected in *Bookmarks.*
* Fixed a bug where bookmarks with 0 completed fields would display as "1%" complete.

</details>

<details>

<summary>v4.7.0 — August 27th, 2024</summary>

* Introducing Governance Reports, offering an audit trail of permissions across all users in your organization
* Multi-cell paste toggle has been removed, and its behavior has been set as the default (if multiple cells are copied, their contents will be pasted into multiple cells, rather than as text in a single cell)
* Links to entries on the Build Page will ensure the linked row is always in view when opened
* Improved the performance of the text-search feature on the Run History
* The Find/Replace panel on the Build Page now has a collapsed state to prevent covering the topmost row
* Bug fixes
  * Tab navigation between cells on build page has been restored
  * Fixed a bug that caused ID values to be copied instead of cell contents on the Build Page
  * Fixed a bug that stopped webservice endpoints that begin with a `/` from being served
  * Fixed a bug where the bookmarks progress bar would be missing
* Adapter updates
  * CodeConnect Horizon now uses the same auth token for its full lifetime, reducing the number of core instances used by integrations
  * Salesforces host name limit increased to 128 characters

</details>

<details>

<summary>v4.6.0 — July 23rd, 2024</summary>

* Launched Frontends, a lightweight deployment solution for simple, custom frontend applications hosted within Glyue's infrastructure.
* Introduced Service Accounts to allow more secure access to Glyue from 3rd-party applications
* Introduced Organizations, a way for multi-customer iPaaS users to manage access of users from different companies within the same Glyue environment
* Extended default invitation expiration to 14 days
* Expired invitations now allow the user to re-obtain a valid invitation on their own, without needing Glyue support
* Hovering over a bookmark's progress bar now shows a detailed breakdown of each status within the category
* Fixed a bug that caused pasted statuses to not populate despite a success message
* Fixed a bug that caused duplicate, empty systems to appear on the integration setup wizard
* Improved the comments window to prevent saving of duplicate comments
* Fixed a bug where resequenced validation rules did not register as changes

</details>

<details>

<summary>v4.5.0 — June 4th, 2024</summary>

* Sandbox is launching a sample banking core for integration testing and demonstration purposes.
* Bookmarks now include a visual progress indicator, reflecting the status of their field mappings.
* The integration scheduler introduces a simplified interface for creating and managing schedules.
* Run History's "Steps" filter now includes options for filtering by the label and/or status of a service request.
* Run History's Debug toggle has been reinstated.
* Staff users gain the ability to view the Migrate, View Changes, User Invite, and Admin pages.
* Glyue admins can now designate a default user group for newly invited Staff users to automatically join.
* Migrations now offer the ability to select specific changes to include/exclude from being migrated.
* OAuth support has been added as an authentication method for calling Glyue integrations.
* GlyueNet™ Partner Ecosystem Highlights
  * Creatio adapter built
  * Docusign adapter built

</details>

<details>

<summary>v4.4.0 — April 23rd, 2024</summary>

* Fiserv Portico adapter built
* Harland Clarke adapter built
* MeridianLink adapter extended with timeout functionality
* Log level selector on the Log page
* Frontend tech stack updates to latest versions of dependencies

</details>

<details>

<summary>v4.3.0 — March 12th, 2024</summary>

* Run Integration modal redesigned to support file uploads, multi-part payloads, and to improve usability of request headers
* Run History entries can now filter steps according to type
* Run History steps extended with link to calling row for Debug lines
* Build page JSON Path display full path / relative path toggle
* Build page now supports Find and Replace across all visible cells
* Configuration Wizard for new integrations to consolidate all adapter configs and bindings in a central UI
* Bookmark page redesigned to support drag-and-drop reorganizing
* Keyboard shortcuts for macOS users updated to use `cmd` instead of `ctrl`
* Invite link now pre-populates users' emails on the signup form
* New users now require email to create Glyue account
* Comments @-tagging feature bugfix to address notifications not sending
* Glyue landing page bugfix to correctly show customer logos alongside Sandbox logo
* Copy/Paste bugfix to show changes on Field Mappings without refreshing
* Integration migration diff engine enhancements for more granular changelogs
* Build page bugfix to not run Python linter on Value Mapping fields
* CSI Bridge adapter bugfix to support empty JSON bodies

</details>

<details>

<summary>v4.2.0 — January 24th, 2024</summary>

* Build page column header overflow text display improvements for readability
* Build page now supports copy/paste of cells with multiple lines of text
* Build page AI code completion model upgraded to GPT 3.5 Turbo
* Documentation search bugfix to prevent timeout errors
* Browser tab titles have improved consistency across the application
* Bookmarks page bugfix to display missing comment counts
* Invite email styling improvements to improve readability
* Celero CUFX adapter extended with more robust timeout error handling
* Admin page bugfix to prevent delays when deleting a user

</details>

<details>

<summary>v4.1.0 — December 27th, 2023</summary>

* UI and API authentication extended to accept user email addresses in addition to usernames
* Builder page extended to support Excel-compatible multi-line copy/paste
* Builder page extended to provide dropdown picklists for selectable fields
* Run history page extended to facilitate visualization and copying of payload element JSON paths
* Integration job scheduler UI added
* Integration job calendar visualizer for historic and future runs added
* Dashboard page bugfix to ensure validation logs are correctly displayed
* Admin page bugfix to ensure notification preferences are correctly displayed
* Snowflake adapter added
* Encompass adapter extended to support token reuse
* Q2 Gro adapter bugfix to ensure accurate adapter validation
* Fiserv Precision Sonic adapter extended to support debug logging
* Corelation KeyBridge adapter extended to better support error scenarios in which underlying API returns HTML content
* nCino to Fiserv Communicator for Signature commercial loan booking solution template implemented
* APIs and integration engine extended to accept files within integration execution requests
* Integration engine performance improved via database interaction optimization

</details>

<details>

<summary>v4.0.2.15 — October 27th, 2023</summary>

* UI "Glyue Help Center" icon link tooltip text shortened to just "Help Center"
* Usage page renamed to "Dashboard" and overhauled to include line graphs that more effectively shows integration execution history
* Dashboard page extended to present most recent adapter validation results and provide button to re-validate on demand
* Builder page right-click menu extended to enable users to run integrations directly from page
* Builder page extended to show warning before first logic change saved in production environment session
* Builder page field column improved to middle-clip text so the beginning and end of the field text is visible
* Builder page filters improved to preview the number of records that will be displayed when a filter value is selected
* Builder page filters for tags extended to include "All Tagged Rows" and "All Untagged Rows" options
* Builder comment bugfix to allow "@" user mentions to work
* Run history page filters for users and integrations extended to include "All" value
* Admin pages extended to support management of integration permission groups
* Admin page bugfix to prevent backend errors related to password override form fields
* DCI core processor adapter implemented
* FICO Liquid Credit adapter extended to support MTLS with client authentication certificates
* MeridianLink Consumer (a.k.a LoansPQ) adapter bugfix to better support usage of the Decision Loan 2.0 web service
* Application web services and integration engine extended to support integration permission groups
* Metrics capture logic for documentation search extended to include UI track event that includes search text

</details>

<details>

<summary>v4.0.2.14 — October 9th, 2023</summary>

* In-app documentation search bugfix to prevent unnecessary 404 messages
* In-app documentation search error display and message content improved
* UI stylistic improvement to top navigation-bar dropdown. Relevant menu items now properly aligned and behave as links
* UI bugfix to prevent polling for notifications when a user's Glyue session expires
* Build page bugfix to ensure integration import functionality works with status columns
* Build page bugfix to ensure comment submission works consistently
* Finastra Phoenix RESTful API adapter implemented
* FIS Horizon Code Connect adapter implemented
* FIS FCM Code Connect adapter extended to return header content within responses
* FIS FCM Code Connect adapter bugfix to prevent certain authentication failures
* Fiserv DNA CoreAPI adapter extended to support latest version
* MeridianLink Consumer (a.k.a LoansPQ) adapter extended to support disablement of XSD validation so payloads can be sent to non-conformant endpoints
* ICE Encompass adapter extended to support authentication for accounts of type "API User"
* Blend adapter extended to support endpoints returning XML, HTML or plain text
* Glyue users accounts can now be restricted to only permit SSO authentication
* Glyue SSO bugfix to ensure effectiveness with SQLite database backend
* Glyue now supports accepting arbitrary content and file types as integration input
* Integration engine special function `get_sdlc_phase` added
* Integration engine special function `get_installation_name` added
* Integration engine now permits specification of response mime-type
* Integration engine debug log message maximum length increased to 64 characters from 32
* GlobalConfig model extended to include SDLC phase of the installation

</details>

<details>

<summary>v4.0.2.13 — August 30th, 2023</summary>

* In-app documentation search implemented
  * Initial rollout will configure Glyue User Docs and Public Support Center as the default search targets
  * Environment-specific repository search targets can be configured as useful
* Build page field mapping "Approved" field replaced with "Status" field with multiple picklist values to better track SDLC phases
* Build page code-editor now includes autocomplete functionality
* Build page service request cells with invalid values are now highlighted
* Build page "Parent Service Request" picklist enhanced to no longer display invalid items
* Build page new field mapping and service request records now have their sequence numbers auto-populated
* Build page search text is now per-component instead of global
* Build page tag editor dialog bugfix to prevent "stage changes" action from inappropriately creating new tags from search field text
* Build page bugfix to remove invalid values in filter query parameters
* Build page bugfix to ensure proper code-editor resizing
* Run history page search enhanced to automatically show sub-integrations during text searches
* Login page enhanced to remember username
* Usage page chart column bugfix to ensure correct linking to run history page
* Admin page extended to support management of auth hooks
* Admin page for integration permission bulk management improved. Related functionality includes searching, multi-selection, coherent page URL patterns, ensuring efficient load times, and new useful bulk actions
* Admin page SAML 2.0 config extended to intuitively support metadata downloads
* Swagger page extended to support URL path parameters for web-service endpoint calls
* UI delete-confirmation dialog bugfixes to prevent unintuitive behavior upon dialog close
* New user onboarding form bugfix to prevent error message truncation
* Upstart adapter implemented
* Digital Onboarding adapter implemented
* Corelation KeyBridge adapter implemented
* Covalent adapter extended to support OAuth
* SymXchange adapter performance improved via client-side caching
* Fiserv Communicator Open adapter extended to support handling of non-200 HTTPS JSON response bodies
* Fiserv Communicator Open adapter bugfix to support effective config validation
* BakerHill NextGen adapter extended to support endpoints returning XML
* Integration engine versioning system implemented to enable development and deployment of otherwise backwards-incompatible changes
  * Integration configs now possess an engine version field that defaults to value `1.0.0`
  * Previously developed integrations will run successfully on engine version `1.0.0`
* Integration engine version `1.1.0` implemented
  * Bugfix to ensure integration "Before Hook" doesn't run if input validation or input masking errors occur
  * Bugfix to ensure INPUT-type masks respect their "Apply If" field
* Integration engine performance improved via removal of unsent email content generation
* Integration mask components extended to support top-level bracket-based references (e.g. `['foo-bar']` is now a valid "Field" value)
* Metrics capture added to new user onboarding flow to support internal analytics and contact record maintenance
* CI/CD pipeline Cypress front-end testing reliability issues mitigated
* Python package versions of `cryptography`, `urlib3`, and `requests` are updated

</details>

<details>

<summary>v4.0.2.12 — June 13th, 2023</summary>

* Single sign-on (SSO) via SAML 2.0 protocol added. Supports arbitrary identity providers (IdPs) and was tested against Okta, JumpCloud, and Azure AD
* User profile page created
* In-app product feedback form added
* In-app Glyue Help Center link now enabled in UI by default
* UI date formatting bugfixes to ensure consistency across the application
* Notifications for comment creation and resolution added
* Login page self-service password reset flow added
* Build page bugfix to ensure copy, paste, and save key-bindings work consistently
* Build page bugfix to prevent cell formatting from breaking in multiline cells when double-clicked
* Run history page responses are now displayed expanded by default
* Run history page now requires a user press "Enter" to perform a text search
* Run history page bugfix to respect "Start Date" filter
* Run history page bugfix so that "End Date" filter assumes "End of Day" when no time of day is provided
* Run history page bugfix to ensure sub-integration visibility toggle state is preserved across page refreshes
* Run history page bugfix to ensure re-running an integration prepopulates the request method field
* Run history page bugfix to prevent polling when a run history id is specified
* Run history page bugfix to allow submitting requests without a body
* User invitation flow extended to allow integration permission assignment prior to onboarding
* Fiserv Communicator Open adapter implemented
* SFTP adapter extended to enable prevention of multiple simultaneous connections
* Metrics capture added for solution template catalog
* Improved application deployment automation

</details>

<details>

<summary>v4.0.2.11 — May 11th, 2023</summary>

* In-app user notifications implemented
* UI profile menu bugfix to ensure correct Git branch and commit information displayed
* UI profile menu bugfix to ensure tour restart functionality works
* Builder page editor pane maximization bugfix to eliminate incorrect clipping on pane resizing
* Builder page bugfix to ensure right-click user action operates on correct row
* Builder page comment dialog improved to allow users to edit their own comments
* Bookmark page editor bugfix to ensure field overflow text is correctly handled
* Run history page extended to provide usable file listing and download functionality
* Run history page extended to display step items for `callint` special function invocations or `CALLINT` service requests
* Run history page bugfix for steps column scrolling
* Run history integration runner dialog extended to support firing HTTP methods with empty bodies
* Login page help message improved for clarification purposes
* Diff page bugfixes to help ensure correct component row display and comparison dialog launching
* Admin page bugfixes to ensure all configuration models are displayed and correctly ordered
* New user enrollment flow improved to include more helpful invitation email
* New user enrollment flow improved to include better Glyue UI for user information capture
* AppOne adapter extended to support Lender Connect authentication and credit decision services
* AppOne adapter bugfix to correctly derive web service URLs
* AppOne adapter bugfix to include stack traces in responses as a result of session errors
* AppOne adapter bugfix to ensure config validation behaves correctly in all error scenarios
* Jira adapter implemented
* Smartsheet adapter bugfixes to ensure useful debug logging
* Smartsheet adapter bugfix to ensure config validation behaves correctly
* Fiserv DNA CoreAPI adapter updated to support CoreAPI version `2023_1`
* Application Apache worker lifecycle default configuration changes to help ensure environment stability
* Application bugfix to prevent existence of multiple long-lived Git processes
* Application bugfix to ensure run history information still correctly displayed after migration
* Application extended to support service requests with a system value of `CALLINT`
* Application extended to include file handle abstraction and related `open_glyuefile` and `list_files` special functions
* Github workflow database schema migration bugfix to support running of automated tests
* Automated integration migration logic test suite expanded

</details>

<details>

<summary>v4.0.2.10 — March 8th, 2023</summary>

* Unified representation of dates across application UI
* Builder page export extended to support JSON files and entire integrations
* Builder page export extended to warn users when attempting to export Excel files with content exceeding cell limits
* Builder page extended to allow importing of rows or integrations from Excel or JSON files.
* Builder page bugfix to prevent newlines being stripped from cells that are double-clicked
* Builder page bugfix to prevent improper HTML rendering within cells
* View changes page bugfix of comparisons between versions of a given cell
* Admin page enhanced to allow bulk management of integration permissions
* CSI Bridge adapter updated to support v3 endpoints
* Application bugfix to prevent excessive disk usage for OpenVPN logs
* Application bugfix to close idle database connections
* Application metrics code reorganized to reduce code duplication.

</details>

<details>

<summary>v4.0.2.9 — February 6th, 2023</summary>

* UI profile dropdown menu added
* UI user preference configuration functionality added
* UI tour added to help on board users
* Builder page extended to support Python auto-generation using OpenAI's `text-davinci-003` GPT-3 model
* Builder page editors now exposed via docked panels and tabs (like most IDEs or debuggers) instead of only dialogs
* Builder page extended to support on-demand component resequencing
* Builder page dropdown menu signifiers added
* Builder page tag sizes increased from 32 to 64 characters
* Builder page highlights improperly populated child service request fields
* Builder page replaces all variations of empty content (i.e. empty strings, whitespace-only strings) with NULL
* Builder page bugfix to prevent concurrent user save failures arising from ID collisions
* Run history page extended to automatically refresh displayed information
* Run history page ID filter added
* Run history page integration rerun dialog added
* Migration page error messages improved for clarity
* Migration page extension to support migration of value mapping sets affiliated with multiple integrations
* Migration page bugfix to ensure correct migration of value mapping sets associated with validation rules
* Vault page enhanced with better sorting, dialog, and cursor focus behavior
* Login page improved via more informative error messages and authentication request debouncing
* Login page redirection bugfix
* Logo redirection bugfix
* Web service endpoint functionality extended to support SOAP-specific XML, generic XML, and HTML request/response content types
* Admin page spelling mistakes partially corrected
* Integration engine `callint` special function extended to support forcing synchronous or asynchronous integration execution
* Integration engine extended to support `TRANSFORM` service requests so field mappings can be used for pure data manipulation
* Smartsheet adapter implemented
* Fiserv CCM adapter implemented
* Alkami online banking adapter implemented
* Alloy adapter extended to support OAuth, log useful content, and enforce communication timeouts
* Fiserv DNA CoreAPI adapter extended to support COCC-flavored CoreAPI variant
* FIS Horizon adapter request payload validation extended to gracefully handle newline characters
* Connectware adapter bugfix to ensure single commit endpoint error messages are returned within adapter responses
* jXchange adapter bugfix to config validation method
* Adapter payload XML serialization utility function bugfix
* Application modified to use AES-256 GCM as default database encryption algorithm for sensitive information in lieu of Fernet-128
* Application deployment script messages corrected
* Application deployment script modified to allow component resequencing
* Application deployment AWS metadata retrieval utility functions consolidated.
* Application deployment bugfixes to ensure correct schema migrations of third-party Django app models
* Application account lockout logic extended to cover all endpoints and authentication failures
* Application Python package set extended to include `jira`, `matplotlib`, and `pypdf2`
* Authentication hook execution logic extended to log useful information
* Django version bumped to 3.2.0

</details>

<details>

<summary>v4.0.2.8 — December 6th, 2022</summary>

* UI dialogs extended to include upper-right close (`X`) buttons where appropriate
* UI aesthetics improved via the removal of the app bar shadow
* UI extended to include Glyue Help Center link in the top-right corner
* Builder page extended to empower deep searching across all logic in a Glyue environment
* Builder page right-click "Go To" menu behavior extended and improved
* Builder page extended to include Python syntax checking during formula field editing
* Builder page extended to include line and column counters during formula field editing
* Builder page extended to include tags in Excel export
* Builder page extended to highlight selected row
* Builder page comment dialog improvements focused on widget layout, URL functionality, and button behavior
* Builder page comment text formatting improved
* Builder page extended to filter tag list by user input
* Builder page modified to clip text in a pleasing and intuitive manner
* Run history page optimizations to prevent Glyue backend crashes
* Migration page modified to use more accurate language in the resulting output
* Migration page was modified to sort listed integrations alphabetically
* Usage report extended to include the name of the Glyue user account that executed an integration
* Admin page modified to leverage Sandbox Banking look-and-feel theme
* Admin page extended to include tag models
* Wolters Kluwers iLien adapter added
* Code Connect IBS-flavored adapter bugfix to cover authentication edge cases
* SymXchange adapter bugfix to correct message ID parsing from error responses
* nCino-to-IBS Connectware solution template implemented
* Integration engine was modified to respect integration permissions when executed via `callint`
* Integration engine was modified to respect integration permissions when executed via a scheduled job
* Integration engine extended to support the maintenance of the adapter state across request executions during an integration run
* Integration migration bugfix to correctly remap vault permissions
* Metrics capture logic extended to include Glyue component and LOC change counts during migration
* Metrics capture logic extended to include Glyue component and LOC change counts during builder page saves
* Flex data structure extended to correctly support the Python `in` operator
* GitHub workflow bugfix for nightly scans of PyPI and NPM package vulnerabilities

</details>

<details>

<summary>v4.0.2.7 — October 11th, 2022</summary>

* UI page routing and component sizing improved
* UI logo updated to reflect the latest Sandbox Banking branding
* Diff page was added to facilitate a comparison of integration logic across Glyue installations
* Builder page extended to include component version histories
* Builder page bugfixes to eliminate improper highlighting in the comment component view
* Builder page bugfix to prevent improper deep cloning of pending/unsaved components
* Run history page extended to include the username of the Glyue account that initiated each integration run
* Migration page was extended to support the generation of integration logic change diffs
* Migration page validation logic extended to prevent conflicting value mapping set migrations
* Swagger page bugfix to prevent the display of unnecessary API groups
* Admin site title updated to replace references to "Django" with "Glyue"
* Integration engine bugfix to prevent process halting from invalid child service request logic
* FTPS adapter implemented
* Fiserv DNA CoreAPI adapter updated to support CoreAPI version `2022_2`
* Salesforce adapter bugfix to ensure user password expirations captured by config validation
* NACHA adapter bugfix to correctly handle foreign exchange reference indicator fields
* FIS Horizon Xchange adapter TLS settings hardened to improve transport security
* SFTP solution template implemented
* Deployment logic bugfix to ensure the correct SMTP server host is established in new environments
* Deployment logic bugfix to ensure Django database schema migrations succeed in certain edge cases
* Metrics capture logic extended to leverage UUIDs for HTTP request/response tracking
* Metrics capture logic bugfix to ensure session username and start timestamp are included within log-in events
* Metrics capture logic bugfix to prevent sensitive URL parameter data leaks into downstream destinations
* Metrics initiation logic extended to incorporate network interaction timeouts
* Usage report job bugfix to ensure job parameters are applied correctly
* HTTP request/response record purge job bugfix to ensure recurring deletions succeed in certain edge cases
* Jitter added to default scheduled jobs in new environments to solve thundering herd failures
* Python package versions of `Pygments` and `pytz` updated

</details>

<details>

<summary>v4.0.2.6 — August 17th, 2022</summary>

* MeridianLink Mortgage (a.k.a LendingQB) adapter added
* Integration special functions `info` and `debug` added with intention to replace `historize` function
* UI modified to reduce authentication alerts from the metrics capture endpoint
* UI missing fonts bugfix implemented
* UI missing icons bugfix implemented
* Builder page extended to support save keyboard shortcut (ctrl + s) within open dialogs
* Builder page confirmation dialog for change cancellation added
* Builder page editor syntax highlighting for non-code fields improved
* Run history page performance improved
* Run history page date filter bugfix implemented
* Adapter config validation page now accessible to all Glyue users
* OpenVPN page user experience greatly improved via required field enforcement, picklists, and other tweaks
* Pydantic package added to provide an integration data structure validation alternative to validation rules
* Application logging configuration extended to support easy overriding for development and debugging purposes
* Code Connect IBS-flavored adapter bugfixes implemented
* RESTful API URL path mount compatibility bugfix implemented
* Tag migration bugfix related to duplicate keys implemented
* Integration debug logging field reordering bugfix implemented
* Application deployment error messages improved
* Application deployment IMDS V1 compatibility bugfix implemented
* Application deployment log silencing bugfix implemented
* Application deployment Git authentication bugfix implemented

</details>

<details>

<summary>v4.0.2.5 — June 27th, 2022</summary>

* Special record_metric function implemented for capturing business metrics via Segment
* Product usage metrics now captured via Segment
* Gro adapter implemented
* NACHA adapter extended to support parsing
* OpenVPN access provisioning error messages improved
* Postgres user creation script extended to grant appropriate sequence permissions
* Bookmark creation dialog no longer populates the folder field with the last utilized value
* Improved error messages and admin override behavior for integration HTTP invocations where HTTP Api field is set to false
* Frontend compatibility extended to include older browsers without the hasOwn JavaScript function
* Frontend compilation output files removed from source control

</details>

<details>

<summary>v4.0.2.4 — June 7th, 2022</summary>

* UI snackbar for user messages added
* UI filter menu items sorted
* Browser back/forward button support for filter selections added
* Builder page shift-click selection bugfixes implemented
* Builder page pending records now added immediately after the current selection
* Builder page scrolls to newly added pending rows
* Builder page tag values sorted within the column
* Builder page context menu includes the ability to add or remove tags from all selected rows
* Builder page tag modal escapes now cancel action instead of saving changes
* Builder page hierarchical tagging support added
* Builder page tags starting with a period character supported
* Builder page tag value validation added
* Builder page style indicators added for required columns and missing required values
* Builder page required boolean values now prefilled with defaults in all scenarios
* Builder page add/edit/delete counters are now accurate
* Builder page bugfix related to special copy
* Builder page no longer records re-sorts upon changes
* Builder page save modal undo functionality removed
* Builder page unexpected scrolling behavior removed
* Builder page field mapping visibility bugfix for child service requests
* Builder page convenience bookmarking UX added
* Builder page frontend code refactored
* Builder page significant performance improvements implemented
* Migration page sources UX added
* End-to-end application build, release, and deployment pipeline for CI/CD added
* Login failure monitoring alert email logic expanded to include usernames in additional scenarios
* Installation-specific DNS override capability added
* PTP adapter stack trace credential leak fixed
* BakerHill NextGen adapter supported request types expanded

</details>

<details>

<summary>v4.0.2.3 — May 20th, 2022</summary>

* RESTful API integrations supported
* Comment count and status information incorporated into the bookmark page
* Cypress frontend UI testing added
* CodeConnect adapter for IBS extended to provide better error messaging
* Validation rule bugfix to ensure correct engine behavior when rule formulas raise exceptions

</details>

<details>

<summary>v4.0.2.2 — April 26th, 2022</summary>

* Browser page titles now convey the relevant page
* Nortridge Loan Servicing adapter added
* Builder page performance greatly enhanced for large data sets
* Builder code formatting and whitespace mutation bug fixed
* Comment author status maintained after user deletions
* Github workflows for Docker image builds added
* Django version updated from 2.2.27 to 2.2.28

</details>
