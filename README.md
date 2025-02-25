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

 ## Deployment
 
