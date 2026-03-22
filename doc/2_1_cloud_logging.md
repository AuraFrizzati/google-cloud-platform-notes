
# Cloud Logging 

- Cloud Logging is a **real-time log-management system** with **storage**, **search**, **analysis**, and **monitoring** support.

- Cloud Logging **automatically collects log data** from G**oogle Cloud resources**. 
- You can also **configure alerting policies** so that **Cloud Monitoring** notifies you when certain kinds of events are reported in your log data. 


## Cloud Logging Use Cases

- **Gather data from various workloads**: This data is required to **troubleshoot** and **understand the workload** and **application needs**.
- **Analyze large volumes of data**: Tools like **Error Reporting**, **Log Explorer**, and **Log Analytics** let you focus from large sets of data.
- **Route and store logs**: Route your logs to the region or service of your choice for additional compliance or business benefits.
- **Get Compliance Insights**: Leverage **audit** and **app logs** for **compliance patterns** and **issues**. 

## Cloud Logging Architecture

Cloud Logging architecture consists of several components:

![alt text](img/cloud_logging_architecture.png)

1. **Log Collections**: These are the places where **log data originates**. **Log sources** can be **Google Cloud services**, such as Compute Engine, App Engine, and Kubernetes Engine, or **your own applications**.
2. **Log Routing**: The Log Router is responsible for **routing log data to its destination**. The Log Router uses a combination of **inclusion filters** and e**xclusion filters** to determine which log data is routed to each destination.
3. **Log sinks**: Log sinks are **destinations** where **log data** is **stored**.
4. **Store Logs**: Cloud Logging supports a **variety of log sinks**, including:
    - **Cloud Logging log buckets**: These are **storage buckets** that are **specifically designed** for **storing log data**.
    - **Pub/Sub topics**: These topics can be used to **route log data to other services**, such as third-party logging solutions.
    - **BigQuery**: This is a fully-managed, petabyte-scale **analytics data warehouse** that can be used to **store** and **analyze** log data.
    - **Cloud Storage buckets**: Provides storage of log data in **Cloud Storage**. Log entries are stored as **JSON files**.
5. **Log Analysis**: Cloud Logging provides several **tools** to **analyze logs**:  
    - **Logs Explorer** is optimized for **troubleshooting** use cases with features like **log streaming**, a **log resource explorer** and a **histogram for visualization**. 
    - **Error Reporting** help users react to critical application errors through **automated error grouping** and **notifications**. 
    - **Logs-based metrics**, **dashboards** and **alerting** provide other ways to understand and make logs actionable. 
    - **Log Analytics** feature expands the toolset to include **ad hoc log analysis capabilities**.

## References:

- [Cloud Logging overview](https://docs.cloud.google.com/logging/docs/overview)
- [Cloud Logging overview and architecture](https://www.skills.google/course_templates/99/html_bundles/621228)