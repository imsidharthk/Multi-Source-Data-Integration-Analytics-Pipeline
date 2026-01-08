#Multi-Source-Data-Integration-Analytics-Pipeline
SHEET LINK https://docs.google.com/spreadsheets/d/19KEwzoDv2QHJY4V5BpuDy9FJ6PI9zbJnU-bpwZq7Aek/edit?gid=1559817499#gid=1559817499

## 📌 Project Overview

This project demonstrates an **end-to-end data pipeline** that syncs structured data from **Google Sheets** into **Supabase (PostgreSQL)** using **Google Apps Script** and Supabase's **REST API**.

It is designed to be **production-safe**, handling:

* Large datasets (batch inserts)
* Strict Supabase schema validation
* Column whitelisting to prevent API failures
* Resume-ready, real-world data engineering use case

---

## 🧠 Use Case

Organizations often maintain operational data in Google Sheets but require:

* Centralized storage
* Queryable SQL database
* API access for dashboards or apps

This project solves that by syncing sheet data directly into Supabase.

---

## 🛠 Tech Stack

* **Google Sheets** – Source data
* **Google Apps Script (JavaScript)** – ETL logic
* **Supabase (PostgreSQL)** – Destination database
* **Supabase REST API (PostgREST)** – Data ingestion

---

## 📂 Sheet Structure

The Google Sheet contains a `Master` tab with the following headers:

```
fms | sr_no | unique_id | step_no | year | doer_name | planned_time | actual_time | status | delay | remark
```

⚠️ Column names must **exactly match** Supabase table columns.

---

## 🗄 Supabase Table

**Table Name:** `fms_doer_performance`

All inserts are performed on the `public` schema using Supabase REST API.

---

## ⚙️ Features

* ✅ Batch inserts (500 rows per request)
* ✅ Column whitelisting (prevents PGRST204 errors)
* ✅ Schema-safe payload construction
* ✅ Handles empty / null values
* ✅ Resume-ready data engineering workflow

---

## 📜 Core Logic (Apps Script)

### Allowed Columns (Schema Safety)

```javascript
const ALLOWED_COLUMNS = [
  "fms",
  "sr_no",
  "unique_id",
  "step_no",
  "year",
  "doer_name",
  "planned_time",
  "actual_time",
  "status",
  "delay",
  "remark"
];
```

### Data Sync Function

```javascript
function uploadMasterDataInDB() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Master");
  const rows = sheet.getDataRange().getValues();
  const headers = rows[0];

  let payload = [];

  for (let i = 1; i < rows.length; i++) {
    let rowObj = {};

    headers.forEach((header, idx) => {
      if (ALLOWED_COLUMNS.includes(header)) {
        rowObj[header] = rows[i][idx] ?? null;
      }
    });

    payload.push(rowObj);
  }

  pushInChunks(payload);
}
```

---

### Batch Upload to Supabase

```javascript
function pushInChunks(data) {
  const chunkSize = 500;

  for (let i = 0; i < data.length; i += chunkSize) {
    const chunk = data.slice(i, i + chunkSize);

    const response = UrlFetchApp.fetch(SUPABASE_URL, {
      method: "post",
      muteHttpExceptions: true,
      contentType: "application/json",
      headers: {
        apikey: SUPABASE_API_KEY,
        Authorization: `Bearer ${SUPABASE_API_KEY}`
      },
      payload: JSON.stringify(chunk)
    });

    Logger.log(response.getResponseCode());
    Logger.log(response.getContentText());
  }
}
```

---

## 🔐 Security Notes

* Use **Anon / Publishable Key** only
* Enable **Row Level Security (RLS)** properly
* Never expose service role keys in Apps Script

---

## 🚀 How to Run

1. Open Google Sheet
2. Extensions → Apps Script
3. Paste code
4. Set Supabase URL & API key
5. Run `uploadMasterDataInDB()`

---

## 📈 Resume Highlights

* Built a **custom ETL pipeline** using Google Apps Script
* Integrated **Supabase PostgreSQL** using REST APIs
* Implemented **batch processing & schema validation**
* Solved real-world API errors (PGRST204, schema cache issues)
* Hands-on experience with **data ingestion pipelines**

---

## 🔮 Future Enhancements

* UPSERT support (on conflict)
* Incremental sync
* Error row logging back to Google Sheets
* Trigger-based automation

---

## 👤 Author

**Sidharth Kumar**

📧 Email: [imsidharthk@gmail.com](mailto:imsidharthk@gmail.com)

---

⭐ If this project helped you understand real-world data pipelines, give it a star!
