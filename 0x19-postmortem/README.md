#0x19-postmortem
Postmortem: Outage of E-commerce Website Search Functionality
Issue Summary
Duration:

Start: May 25, 2024, 10:00 AM UTC
End: May 25, 2024, 12:30 PM UTC
Impact:

The search functionality of our e-commerce website was down.
Users were unable to search for products, leading to a significant decrease in user engagement and transactions during the outage.
Approximately 75% of users were affected, as search is a primary feature for product discovery.
Root Cause:

The root cause was a misconfigured index in the Elasticsearch service, which led to failed queries and timeouts.
Timeline
10:00 AM: Issue detected via monitoring alert indicating a spike in search query timeouts.
10:05 AM: Engineering team received alerts and began investigating.
10:10 AM: Initial assumption was a network issue, investigated network logs and infrastructure.
10:30 AM: Network issue ruled out, focus shifted to application logs.
10:45 AM: Misleading path: suspected an overload of the search service due to a marketing campaign.
11:00 AM: Escalated to the backend engineering team specializing in Elasticsearch.
11:15 AM: Identified multiple timeout errors in Elasticsearch logs.
11:30 AM: Investigated Elasticsearch index configurations.
12:00 PM: Discovered a recent update to the index configuration that caused inefficient indexing.
12:15 PM: Reverted the index configuration to the previous stable state.
12:30 PM: Search functionality restored, and confirmed by monitoring tools.
Root Cause and Resolution
Root Cause:

The issue was caused by a recent change in the Elasticsearch index configuration. An optimization attempt led to an incorrect setting that caused inefficient indexing and slow query responses. Specifically, the setting for refresh_interval was set too frequently, leading to high resource consumption and subsequent query timeouts.
Resolution:

The resolution involved reverting the refresh_interval setting to its previous value. Additionally, the engineering team re-indexed the affected indices to ensure optimal performance. A rollback was executed through the configuration management system, and the changes were applied without the need for a full system restart.
Corrective and Preventative Measures
Improvements:

Enhanced Monitoring: Implement more granular monitoring for Elasticsearch to detect configuration-related performance issues earlier.
Configuration Management: Establish a more rigorous review process for changes to search index configurations.
Load Testing: Perform thorough load testing with new configurations before applying them in the production environment.
Documentation: Update documentation to include the impact of critical Elasticsearch settings and best practices for configuration changes.
Tasks:

 Patch Elasticsearch cluster to the latest stable version.
 Add monitoring alerts for Elasticsearch configuration changes.
 Create a configuration change review protocol, including peer reviews and automated testing.
 Conduct a training session for the engineering team on Elasticsearch best practices.
 Implement a canary deployment process for Elasticsearch configuration changes to test on a small subset of data before full rollout.
 Review and optimize the current indexing strategy for future-proofing.
