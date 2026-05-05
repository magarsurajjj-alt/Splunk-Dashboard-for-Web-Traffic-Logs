# 📊 Splunk Web Traffic Dashboard (JSON Log Analysis)
<img width="1365" height="767" alt="Dashboard" src="https://github.com/user-attachments/assets/497a20a0-f7a5-44b6-8246-798bc198b7d4" />

## 📌 Overview
This project demonstrates the creation of a web traffic monitoring dashboard using **Splunk** by analyzing JSON-based Apache access logs.  
The dashboard provides insights into web activity, performance metrics, error rates, and geographic traffic distribution.

---

## 🛠️ Tools & Technologies
- JSON Web Logs (Apache Access Logs)
- Splunk Dashboard Studio / Classic Dashboard
- iplocation & geom visualization

---

## 🧱 Lab Setup

Logs were ingested using:


source="apache_logs.json" host="webserver" sourcetype="_json"


---

## ⏱️ Task 0: Time Range Setup
- Added Time Range Input
- Token name: `time_range`
- Applied shared time filter across all dashboard panels

---

## 📊 Task 1: Web Activities

### 🔢 Total Web Requests
```spl
source="apache_logs.json" host="webserver" sourcetype="_json"
| stats count AS "Total Web Requests"

✅ Successful Responses (200)
source="apache_logs.json" host="webserver" sourcetype="_json" method=GET status=200
| stats count AS "Successful Responses"

❌ Client Errors (4xx)
source="apache_logs.json" host="webserver" sourcetype="_json"
| where status>=400 AND status<500
| stats count AS "Client Errors"

🔥 Server Errors (5xx)
source="apache_logs.json" host="webserver" sourcetype="_json"
| where status>=500
| stats count AS "Server Errors"
```
📈 Task 2: Web Statistics
🔗 Top Requested URIs
```
source="apache_logs.json" host="webserver" sourcetype="_json"
| top uri

👤 Top Users by IP Address
source="apache_logs.json" host="webserver" sourcetype="_json"
| stats count AS Requests by ip
```
🌍 Task 3: Web Traffic Geo Visualization
🗺️ Client IP Location Map
```
source="apache_logs.json" host="webserver" sourcetype="_json" method=GET
| spath
| iplocation ip
| stats count by Country
| geom geo_countries featureIdField="Country"
```
📊 Dashboard Panels

The Splunk dashboard includes:
```
📈 Total Web Requests (Single Value)
📊 Successful Responses
❌ Client Errors (4xx)
🔥 Server Errors (5xx)
📊 Top Requested URIs (Bar Chart)
👤 Top IP Addresses (Bar Chart)
🗺️ Geo Traffic Map
```
---------------------------------
📁 Project Structure
```
splunk-web-traffic-dashboard/
│
├── logs/
│   └── apache_logs.json
│
├── screenshots/
│   ├── dashboard.png
│   └── panels.png
│
├── queries/
│   └── splunk_queries.txt
│
└── README.md
```
⚙️ Setup Instructions
Install Splunk Enterprise / Free version
Go to Add Data → Upload
Upload apache_logs.json
Set:
Index: web_logs
Sourcetype: _json
Build dashboard using SPL queries above
Add panels using time range token time_range
