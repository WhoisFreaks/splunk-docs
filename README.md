# WhoisFreaks App for Splunk and Splunk ES
## Quick Start Guide
### App Installation 
The latest app is available on Splunkbase. Please ensure the prerequisites are met. For Splunk Cloud deployments, install the app directly from Splunkbase. For on-prem distributed environments, deploy the WhoisFreaks App to both indexer and search head cluster members using the standard process for deploying apps and add-ons to clusters. See the App Installation section for more information.  
### Configure the Base Search
The base search is an SPL Query that allows users to define which log sources are to be monitored by the WhoisFreaks App. It should output the required fields the WhoisFreaks App uses to populate WHOIS and DNS dashboards. The app arrives with a pre-configured performance-optimized query. This query will work well in
environments where data sources are Common Information Model (CIM) compliant.  
To configure the base search, go to WF Settings→ Configure Log Source. The required fields are: url, src, dest, log_source, domain, and _time. See Configuring Base Search Using the Pre-Configured Query for more information.  
### Adding a WhoisFreaks API Key
Navigate to WF Settings → API Key to enter your WhoisFreaks API Key. WhoisFreaks API Key is available on WhoisFreaks billing dashboard. If this is your first time setting up Splunk with WhoisFreaks, please contact our [Enterprise Support](https://whoisfreaks.com/contact) to ensure your API key is appropriately provisioned.  
Saving new API credentials will prompt you to enable default saved searches. Please click the "Enable" button to enable the minimum set of saved searches that run the enrichment process.  
### Configure Enrichment
We recommend leaving the current settings as a default. If you wish to customize the Enrichment Settings:  
1. Go to WF Settings → Configure Enrichment.  
2. Once any of the following settings are changed, select the Save button.  

#### Select WHOIS / DNS fields to monitor
1. Go to WF Settings → Configure Enrichment. 
2. Select or deselect the fields you want to monitor from the WHOIS and DNS records of a specific domain. Only those fields will be shown on the WHOIS and DNS enrichment.  
#### Configure Cache and Queue Wait Time
 1. Go to WF Settings → Configure Enrichment. 
 2. Select the Queue Wait Time to configure it. Queue Wait Time is how often the app enriches Domain information. Default is 5 minutes. Decreasing the frequency can be helpful to reduce API usage or if the enrichment is taking longer than 5 minutes to run on a higher volume Splunk cluster. 
 3. Select Cache settings to configure it. WhoisFreaks maintains a cache to reduce API query usage. A user may wish to disable or reduce the cache retention period times when monitoring volatile domains. 
 4. **Enable Cache** - Enabled by default to optimize API consumption. Disable the cache to monitor for changes < 1 day old. (CAUTION: this can result in high API consumption)
 5. **Add the Cache Retention Period** - Sets how long domain enrichment should live in the cache before being re-queried. 30 days is the default.  

 ## Deployment Guide 
 ### Overview 
 The following sections outline some background architecture and deployment information that is helpful for new users to understand. Additional information covering the app components: configuration files, stanzas and fields,
KV store, macros, and saved searches is contained in [Appendix A]().  
[Image]  
The Saved Searches configuration file (`savedsearches.conf`) defines the processes for enrichment and the Queue Builder for the `wf_enrich_queue` KV store. In the Queue Builder process, raw logs in the Splunk Indexes are queried from the Web data model as defined by the DomainTools base search configuration (`wf_basesearch`). This process includes checking to see if the domain already exists when comparing to existing wf Enrich data, as
that would indicate if the domain has already been enriched. If not, the new domain is queued for enrichment. Each domain is stored with the enriched data in the KV store.  
### Prerequisites
The WhoisFreaks App works best with Splunk Enterprise Security (ES), which makes it easy for an analyst to set up alerts on specific newly registered domains or any specific registrant or hsoting IP, but can function as a standalone monitoring platform without Splunk ES. Customers who have not yet deployed ES can still realize significant value from the WhoisFreaks solution.  
The app works best when installed on indexers, in addition to previous requirements, during deployment. Our [Enterprise Support](https://whoisfreaks.com/contact) team can assist with workarounds if such a setup is not feasible for your environment.
### Firewall Rule
Ensure you can reach `https://api.whoisfreaks.com/` from the Splunk server. If required, update firewall rules to allow access to this endpoint for the app to be functional.  
If you are on a managed infrastructure and cannot connect to the WhoisFreaks endpoint, please reach out to us so we can help verify any additional IP allow-listing activities that may be needed. 
### Splunk Credentials to Install App
A Splunk account with admin access is required to successfully install and configure the app. After installation, most user functions should be available with less privileged accounts.  
You may also need command-line access (like SSH access) to perform some deployment and diagnostics functions, especially if deploying in a clustered environment.  
### Splunk Permissions to Operate App
Ensure that the `list_storage_passwords` privilege is added to the user operating the app. The admin role may need to be used to access the password storage within Splunk.  
Users within the WhoisFreaks App must have read privileges to all the components of the app. If a user expects to add, update, or append values in any of the internal stores, their user profiles must include write privileges to the KV stores involved. For the list of KV stores and descriptions, please see the [App Components]() Appendix.  
### Validating the App in Non-Production Environments
If you use a staging environment or development environment to test new Splunk apps, ensure the same data sources you plan to use in production are also available to the Splunk `search heads` in the test environment.  


