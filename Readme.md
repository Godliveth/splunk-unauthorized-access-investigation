
# 🔐 Splunk Project: Investigating Unauthorized Access  
**Day 19 of #30DaysOfSOC Challenge**

---

## 🧭 Project Description

This project demonstrates how to use **Splunk Enterprise** to investigate **unauthorized access attempts** on Linux systems.  
By analyzing authentication and audit logs, I identified access denials, visualized event trends, and extracted security insights that mimic real SOC workflows.

The project combines **log analysis** and **dashboard creation** to detect suspicious activities like repeated file access or failed permissions.

---

## 🧰 Tools and Environment

| Component | Details |
|------------|----------|
| **Platform** | Windows 10 (Host) |
| **SIEM Tool** | Splunk Enterprise |
| **Dataset** | Linux_UnAuthorized_Auditd_logs.json |
| **Sourcetype** | `_json` |
| **Index** | `linux_auth` |

---

## 🎯 Objective

- Ingest and analyze Linux audit logs using Splunk.  
- Detect unauthorized access attempts and repeated access patterns.  
- Visualize findings using an interactive Splunk dashboard.

---

## ⚙️ Lab Setup and Configuration

1. Installed **Splunk Enterprise** on Windows.  
2. Added dataset via **Add Data → Upload**.  
3. Selected **_json** as the source type for automatic field extraction.  
4. Indexed data into **linux_auth**.  
5. Verified data with:
   ```spl
   index=linux_auth | head 5
````

---

## 🧠 Investigation Queries and Results

### 🔹 Q1: Total Number of Success Events

```spl
index=linux_auth result=success | stats count as "Total Success Events"
```

**Result:**
A total of **914 successful events** were recorded.

**Interpretation:**
These represent successful authentication or permitted actions within the audit logs, indicating normal system operations.

📸 *Screenshot:*
![Total Success Events](./screenshots/Total_Success_Events.png)

---

### 🔹 Q2: Most Common Event Triggered

```spl
index=linux_auth | stats count by event_type | sort - count | head 1
```

**Result:**

| event_type | count |
| ---------- | ----- |
| AVC_DENIED | 103   |

**Interpretation:**
The most frequent event type is **AVC_DENIED**, which originates from **SELinux** and signifies blocked access attempts.
Such logs are key indicators of **unauthorized activity or policy enforcement** on Linux systems.

📸 *Screenshot:*
![Most Common Triggered Event](./screenshots/Most_Common_Triggered_Event.png)

---

### 🔹 Q3: File Paths Accessed Twice by UID=1010

```spl
index=linux_auth uid=1010 | stats count by path | where count=2
```

**Result:**

| path                       | count |
| -------------------------- | ----- |
| /bin/bash                  | 2     |
| /etc/shadow                | 2     |
| /etc/ssh/sshd_config       | 2     |
| /etc/sudoers               | 2     |
| /root/.ssh/authorized_keys | 2     |
| /usr/bin/curl              | 2     |

**Interpretation:**
User `uid=1010` attempted to access sensitive paths multiple times — including **/etc/shadow** and **/etc/sudoers** — which often indicate **privilege escalation or reconnaissance** attempts.

📸 *Screenshot:*
![File Path Accessed Twice by User](./screenshots/File_Path_Accessed_Twice_by_User.png)

---

## 📊 Dashboard: Unauthorized Access Investigation

All findings were compiled into a Splunk dashboard for better visibility and quick analysis.

### 🧩 Dashboard Panels

| Panel                              | SPL Query                  | Visualization             | Description    |                                      |                                            |                                 |
| ---------------------------------- | -------------------------- | ------------------------- | -------------- | ------------------------------------ | ------------------------------------------ | ------------------------------- |
| **1. Success vs Denied Events**    | `index=linux_auth          | stats count by result`    | Pie Chart      | Displays event outcomes distribution |                                            |                                 |
| **2. Most Common Event Types**     | `index=linux_auth          | stats count by event_type | sort - count`  | Bar Chart                            | Highlights frequent security event types   |                                 |
| **3. Top Accessed Paths by UID**   | `index=linux_auth uid=*    | stats count by path       | sort - count   | head 10`                             | Table                                      | Lists frequently accessed files |
| **4. Repeated Access by UID=1010** | `index=linux_auth uid=1010 | stats count by path       | where count=2` | Table                                | Detects repeated access to sensitive files |                                 |

---

## 📸 Dashboard Visuals

![Dashboard Overview](./screenshots/dashboard_overview.png)
![Top Event Types](./screenshots/dashboard_event_type_chart.png)
![Denied Access Table](./screenshots/dashboard_denied_table.png)

📁 View all visuals in the [screenshots folder](./screenshots).

---

## 🔍 Key Insights

* Over **900 successful** and **450+ denied** events were detected.
* `AVC_DENIED` was the most common event, showing **active SELinux protection**.
* User **UID=1010** repeatedly accessed sensitive system files, a potential red flag.
* Dashboards provided quick visual context for security monitoring and incident analysis.

---

## 🧩 Skills Demonstrated

* SPL Query Writing (`stats`, `sort`, `table`, `where`)
* Data Ingestion and Field Extraction in Splunk
* Linux Audit Log Analysis
* Dashboard & Visualization Creation
* Incident Investigation Workflow

---

## ✅ Conclusion

This project shows how Splunk can transform Linux audit logs into **actionable insights** for detecting unauthorized access.
By combining SPL, dashboards, and visual analysis, a SOC analyst can quickly pinpoint high-risk users and suspicious activities.

> “Every denied log entry tells a story — Splunk helps turn that story into security intelligence.”

---

## 📂 Repository Structure

```
├── Linux_UnAuthorized_Auditd_logs.json
├── screenshots/
│   ├── Total_Success_Events.png
│   ├── Most_Common_Triggered_Event.png
│   ├── File_Path_Accessed_Twice_by_User.png
│   ├── dashboard_overview.png
│   ├── dashboard_event_type_chart.png
│   └── dashboard_denied_table.png
└── README.md
```

---

## 🏷️ Tags

#Splunk #SIEM #SOCAnalyst #CyberSecurity #LogAnalysis #IncidentResponse #BlueTeam #LinuxSecurity #30DaysOfSOC #HandsOnLearning

```

---


