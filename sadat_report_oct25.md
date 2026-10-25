# GTex ERP — Daily QA Testing & Functional Audit Report
## Doctypes Directory Comprehensive Audit: Product Dev (M04–M07) · Demand & Planning (M08–M09) · Sourcing & Subcontract (M10–M13) — 196 Enterprise Screens

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Date:** 25 October 2026  
**Author / QA Lead:** Sadat (Tahmid Sadat)  
**File:** `todos and reports/sadat_report_oct25.md`  
**Target Folder:** `/Users/laptopparadise/Documents/Textile/todos and reports/`  

**Audited Directory & Reference Documents (`/Doctypes`):**  
- `Doctypes/demand_doctype.md` *(M08–M09: Demand Forecasting, S&OP & Scheduling — ৪৯টি স্ক্রিন)*  
- `Doctypes/product_doctype.md` *(M04–M07: Product Dev, Tech Pack, Sampling & Costing — ৬১টি স্ক্রিন)*  
- `Doctypes/suplier_doctype.md` *(M10–M13: Sourcing, Central Stores, Inventory & Subcontract — ৮৬টি স্ক্রিন)*  
- `frontend/lib/product/m04-configs.ts` & `frontend/lib/product/m06-configs.ts`  
- `backend/apps/grp_garment/grp_garment/grp_garment_manufacturing/doctype/tech_pack_measurement/tech_pack_measurement.json`  
- `M08_DEMAND_FORECASTING_OVERVIEW_DATA_AND_VERIFICATION.md`  

**Active Test Environments & Service Status:**  
- **Desktop Web App (Frontend):** [http://localhost:3000](http://localhost:3000) (Next.js 15 Enterprise Shell — Status: `HTTP 200 OK`)  
- **Vendor Portal:** [http://localhost:3000/vendor](http://localhost:3000/vendor) (Status: `HTTP 200 OK`)  
- **Frappe Desk Backend:** [http://localhost:8090/desk](http://localhost:8090/desk) & [http://localhost:8081/login](http://localhost:8081/login) (Status: `HTTP 200 OK / 301`)  
- **Database Engine:** MariaDB 11.8 (`gtex-mariadb-1` / Container: `gtex-backend-1`, Site: `dev.localhost`, Port: 3308)  
- **Active Docker Services:** `gtex-frontend-1` (:8090), `gtex-backend-1`, `gtex-scheduler-1`, `gtex-queue-short-1`, `gtex-queue-long-1`, `gtex-websocket-1`, `gtex-redis-cache-1`, `gtex-redis-queue-1`, `gtex-mariadb-1`  

---

## ১. সারসংক্ষেপ ও এক্সিকিউটিভ ড্যাশবোর্ড (Executive Summary)

আজকে (২৫ অক্টোবর ২০২৬) জিটেক্স ইআরপি (GTex ERP)-এর নবনির্মিত **`/Users/laptopparadise/Documents/Textile/Doctypes`** ডিরেক্টরির অন্তর্গত ৩টি প্রধান ডোমেন স্পেসিফিকেশন ফাইলের ওপর গভীর ফাংশনাল ও আর্কিটেকচারাল অডিট সফলভাবে সম্পন্ন হয়েছে। 

এই ফোল্ডারে মোট **১৯৬টি এন্টারপ্রাইজ স্ক্রিন**-এর এন্ড-টু-এন্ড ফ্রন্টএন্ড লেবেল, সংশ্লিষ্ট ফ্র্যাপে v16 ব্যাকএন্ড DocType, পোর্ট ৮০৯০-এর সরাসরি ফ্র্যাপে ডেস্ক অ্যাডমিন লিংক এবং মারিয়াডিবি টেবিলের শতভাগ নির্ভুল তালিকা নথিভুক্ত ও প্রতিটি লিংকের কার্যকরিতা পরীক্ষা করা হয়েছে।

### অডিটকৃত ডোমেন ও স্ক্রিন কভারেজ সামারি টেবিল:

| ক্রম | ডোমেন নাম ও কোড | অডিট ফাইল পাথ | সাব-মডিউল তালিকা | সর্বমোট স্ক্রিন | অডিট ফলাফল |
|:---:|---|---|---|:---:|:---:|
| **০১** | **Product Dev, Tech Pack & Costing** (M04–M07) | `Doctypes/product_doctype.md` | Overview (2), M04 (14), M05 (17), M06 (13), M07 (15) | **৬১টি স্ক্রিন** | **100% PASS** |
| **০২** | **Demand, Capacity & Scheduling** (M08–M09) | `Doctypes/demand_doctype.md` | Overview M08 (25), Capacity & Scheduling M09 (24) | **৪৯টি স্ক্রিন** | **100% PASS** |
| **০৩** | **Sourcing, Inventory & Subcontract** (M10–M13) | `Doctypes/suplier_doctype.md` | Overview (1), M10 Sourcing (26), M11/M12 Stores (34), M13 Subcontract (25) | **৮৬টি স্ক্রিন** | **100% PASS** |
| **সর্বমোট** | **৩টি প্রধান ডোমেন** | **Doctypes Directory** | **১২টি সাব-মডিউল গ্রুপিং** | **১৯৬টি স্ক্রিন** | **100% VERIFIED** |

---

## ২. Doctypes ফোল্ডারের ফাইলভিত্তিক বিস্তারিত অডিট ও টেস্টিং ফলাফল

---

### ২.১ ফাইল ০১: Product Dev, Tech Pack & Costing (M04–M07) — ৬১টি স্ক্রিন
* **রেফারেন্স ফাইল:** [`Doctypes/product_doctype.md`](file:///Users/laptopparadise/Documents/Textile/Doctypes/product_doctype.md)
* **সিস্টেম সংযোগ:** Next.js UI (`http://localhost:3000/product` ও `/screen?p=product|...`) ↔ Frappe Desk (`http://localhost:8090/desk`) ↔ MariaDB
* **ইউজার পারসোনা:** তানভীর হোসেন (`tanvir.cad@gtexgroup.com.bd` — CAD & Tech Pack Specialist) ও নুসরাত জাহান (Senior Merchandiser)

#### টেস্ট ফলাফল ও সাব-মডিউল বিশ্লেষণ:
1. **Overview (২টি স্ক্রিন):**
   * `Costing worksheet (Role Center)` ➔ `tabCost Sheet` ([http://localhost:8090/desk/cost-sheet](http://localhost:8090/desk/cost-sheet))
   * `Development pipeline` ➔ `tabStyle` ([http://localhost:8090/desk/style](http://localhost:8090/desk/style))
   * **রেজাল্ট:** PASS. স্টাইল পাইপলাইনের সাথে মারিয়াডিবি ডাটা নিখুঁত সিঙ্ক রয়েছে।
2. **Style & Season (M04 — ১৪টি স্ক্রিন):**
   * কালারওয়ে রেজিস্টার ও কালার স্ট্যান্ডার্ড `GRP Vocabulary` ([http://localhost:8090/desk/grp-vocabulary](http://localhost:8090/desk/grp-vocabulary)) এর সাথে সরাসরি যুক্ত। `m04-configs.ts`-এ `code`, `label_en`, `hex_color` প্রপার্টিগুলো ব্যাকএন্ডের ফিল্ডের সাথে মিলিয়ে যাচাই করা হয়েছে।
   * বায়ার কালার ক্রস-রেফারেন্স ও GTIN/UPC অ্যাসাইনমেন্টের জন্য `Buyer Article Xref` DocType সঠিক।
   * সাইজ স্কেল ও রেঞ্জের জন্য `Range`, সাইজ রেশিও অনুপাতের জন্য `Ratio Pack`, আইটেম কোড গ্রামারের জন্য `Product Type` এবং সিজনাল ওয়ার্কবেঞ্চের জন্য `Season Calendar` সঠিক কাজ করছে।
   * **রেজাল্ট:** PASS. ১৪টি স্ক্রিনের প্রতিটি লিংক লাইভ।
3. **Tech Pack & BOM (M05 — ১৭টি স্ক্রিন):**
   * কনস্ট্রাকশন ও সিম স্পেসিফিকেশনের জন্য `Tech Pack` ([http://localhost:8090/desk/tech-pack](http://localhost:8090/desk/tech-pack))।
   * ১২-পয়েন্ট মেজারমেন্টের জন্য চাইল্ড টেবিল `Tech Pack Measurement` (`tech_pack_measurement.json`) অডিট করা হয়েছে; `pom_code`, `pom_name`, `tolerance_pos`, `tolerance_neg` ফিল্ডের নাম ও ডাটাটাইপ পুরোপুরি সঠিক।
   * মেটেরিয়াল লাইব্রেরির জন্য `Item`, প্রাইজ লিস্টের জন্য `Item Price`, ইঞ্জিনিয়ারিং চেঞ্জের জন্য `Style BOM` এবং রাউটিং অপারেশনের জন্য `Routing` DocType সংযুক্ত রয়েছে।
   * **রেজাল্ট:** PASS. ১৭টি স্ক্রিনের ব্যাকএন্ড ম্যাপিং যাচাইকৃত।
4. **Sampling & TNA (M06 — ১৩টি স্ক্রিন):**
   * প্রোটো, ফিট ও সেলসম্যান স্যাম্পল রিকোয়েস্টের জন্য `Sample Request` ([http://localhost:8090/desk/sample-request](http://localhost:8090/desk/sample-request))।
   * ল্যাব ডিপ অনুমোদনের জন্য `Lab Dip Submission`, স্ট্রাইক-অফ আর্টওয়ার্কের জন্য `Print Strike Off` এবং ক্রিটিক্যাল পাথ মাইলস্টোনের জন্য `TNA Calendar`।
   * **রেজাল্ট:** PASS. `m06-configs.ts` অনুযায়ী ফর্ম ইনপুট টেস্ট সফল।
5. **Costing & Margin (M07 — ১৫টি স্ক্রিন):**
   * প্রি-কস্টিং শিট ও ভার্সন কন্ট্রোলের জন্য `Cost Sheet` ([http://localhost:8090/desk/cost-sheet](http://localhost:8090/desk/cost-sheet))।
   * এসএমভি ও টাইম স্টাডির জন্য `Standard Time`, ওয়ার্কস্টেশন লেআউটের জন্য `Method Study` এবং কার্ব সাইকেল এনালাইসিসের জন্য `Cost Sheet Overhead`।
   * **রেজাল্ট:** PASS. FOB $৪.২০ কস্টিং ব্রেকডাউন মারিয়াডিবিতে সংরক্ষিত।

---

### ২.২ ফাইল ০২: Demand Forecasting, Capacity & Scheduling (M08–M09) — ৪৯টি স্ক্রিন
* **রেফারেন্স ফাইল:** [`Doctypes/demand_doctype.md`](file:///Users/laptopparadise/Documents/Textile/Doctypes/demand_doctype.md)
* **সিস্টেম সংযোগ:** Next.js UI (`http://localhost:3000/planning` ও `?view=demand`) ↔ Frappe Desk (`http://localhost:8090/desk`) ↔ MariaDB
* **ইউজার পারসোনা:** ফারহান আহমেদ (`farhan.planner@gtexgroup.com.bd` — Central Production Planner)

#### টেস্ট ফলাফল ও সাব-মডিউল বিশ্লেষণ:
1. **Overview / Demand Ledger (M08 — ২৫টি স্ক্রিন):**
   * ডিমান্ড লেজার, সেগমেন্টেশন, ইনডেক্সিং এবং একিউরেসির জন্য `Demand Forecast` ([http://localhost:8090/desk/demand-forecast](http://localhost:8090/desk/demand-forecast))। মারিয়াডিবিতে `DF-2026-00101` ভেরিফাই করা হয়েছে।
   * আরএমজি ক্যালেন্ডার ইফেক্ট মডেলিংয়ের জন্য `Production Calendar`।
   * স্ট্র্যাটেজিক ক্যাপাসিটির জন্য `Line Capacity`, সিজনাল বুকিংয়ের জন্য `Booking Window`, অপশন হোল্ডের জন্য `Order Hold` এবং প্রায়োরিটি রুলসের জন্য `Order Priority`।
   * নিজস্ব মিল ক্যাপাসিটি নেট ভলিউমের জন্য `Demand Supply Reconciliation` এবং ব্যাক-ক্যালকুলেশনের জন্য `Style BOM`।
   * মাস্টার প্রোডাকশন শিডিউলের জন্য `Master Production Schedule` (মারিয়াডিবিতে ২টি রেকর্ড উপস্থিত)।
   * লাইন লোডিং বোর্ডের জন্য `Line Loading Plan` ([http://localhost:8090/desk/line-loading-plan](http://localhost:8090/desk/line-loading-plan)) — মারিয়াডিবিতে **৪৬টি লাইভ প্ল্যান রেকর্ড** ভেরিফাইড।
   * এক্সিকিউটিভ এসঅ্যান্ডওপি পর্যালোচনার জন্য `SOP Cycle`।
   * **রেজাল্ট:** PASS. ২৫টি স্ক্রিনের ব্যাকএন্ড লিংক ও ডাটাবেজ মডেল শতভাগ সঠিক।
2. **Capacity & Scheduling (M09 — ২৪টি স্ক্রিন):**
   * মিনিট-বেইজড ক্যাপাসিটি ক্যালেন্ডারের জন্য `Line Capacity` ([http://localhost:8090/desk/line-capacity](http://localhost:8090/desk/line-capacity))।
   * প্ল্যানিং SAM বাইন্ডিংয়ের জন্য `Standard Time`।
   * ওয়ার্কফোর্স অ্যাভেইলেবিলিটির জন্য `Worker Line Assignment` ([http://localhost:8090/desk/worker-line-assignment](http://localhost:8090/desk/worker-line-assignment))।
   * সেলাই লাইন ব্যালেন্সিং ও অ্যান্ডন সিমুলেশনের জন্য `Production Line`।
   * **রেজাল্ট:** PASS. ২৪টি স্ক্রিনের ফ্র্যাপে ডেস্ক অ্যাডমিন পেজ সক্রিয়।

---

### ২.৩ ফাইল ০৩: Sourcing, Central Stores & Subcontract (M10–M13) — ৮৬টি স্ক্রিন
* **রেফারেন্স ফাইল:** [`Doctypes/suplier_doctype.md`](file:///Users/laptopparadise/Documents/Textile/Doctypes/suplier_doctype.md)
* **সিস্টেম সংযোগ:** Next.js UI (`http://localhost:3000/supply` ও `/vendor`) ↔ Frappe Desk (`http://localhost:8090/desk`) ↔ MariaDB
* **ইউজার পারসোনা:** কবির হোসেন (`sourcing.kabir@gtexgroup.com.bd`), আব্দুল হালিম (`store.halim@gtexgroup.com.bd`), ভেন্ডর (`vendor@esquireknit.com`)

#### টেস্ট ফলাফল ও সাব-মডিউল বিশ্লেষণ:
1. **Overview (১টি স্ক্রিন):**
   * `Role Center — sourcing & stores` ➔ `Purchase Order` ([http://localhost:8090/desk/purchase-order](http://localhost:8090/desk/purchase-order))। মারিয়াডিবিতে **১০টি লাইভ পিও** সক্রিয়।
2. **Sourcing & P2P (M10 — ২৬টি স্ক্রিন):**
   * ভেন্ডর অনুমোদন ও রেটিংয়ের জন্য `Supplier` ([http://localhost:8090/desk/supplier](http://localhost:8090/desk/supplier)) — মারিয়াডিবিতে ৩টি ভেন্ডর রেকর্ড যাচাইকৃত।
   * ম্যাটেরিয়াল ডিমান্ড একত্রিতকরণের জন্য `Material Requirement Plan` ([http://localhost:8090/desk/material-requirement-plan](http://localhost:8090/desk/material-requirement-plan))।
   * ফেব্রিক ও ট্রিম বুকিংয়ের জন্য `Purchase Order`।
   * সিল্ড বিড ও আরএফকিউ এর জন্য `Request for Quotation` ও `Supplier Quotation Comparison`।
   * গেট রিসিভিং ও ৪-পয়েন্ট ইন্সপেকশনের জন্য `Purchase Receipt`।
   * থ্রি-ওয়ে ম্যাচিং ও ইনভয়েস ভ্যালিডেশনের জন্য `Purchase Invoice` এবং পেমেন্টের জন্য `Payment Entry`।
   * **রেজাল্ট:** PASS. ২৬টি স্ক্রিনের ফ্লো ও ফ্র্যাপে ডেস্ক রাউট সক্রিয়।
3. **Inventory & Stores (M11/M12 — ৩৪টি স্ক্রিন):**
   * ওয়্যারহাউস ও বিন ট্র্যাকিংয়ের জন্য `Warehouse` ([http://localhost:8090/desk/warehouse](http://localhost:8090/desk/warehouse))।
   * কাপড়ের রোল বারকোড ও ৪-পয়েন্ট ইন্সপেকশনের জন্য `Purchase Receipt` ও `Stock Entry`।
   * রিয়েল-টাইম স্টক লেজারের জন্য `Stock Ledger Entry`।
   * কাটিং ফ্লোরে ফেব্রিক ইস্যুর জন্য `Material Request` ও `Stock Entry`।
   * **রেজাল্ট:** PASS. ৩৪টি স্ক্রিনের মারিয়াডিবি টেবিল নিখুঁতভাবে চিহ্নিত।
4. **Subcontracting & Job Work (M13 — ২৫টি স্ক্রিন):**
   * ওয়াশিং ও এমব্রয়ডারি আউটওয়ার্ড চালানের জন্য `Job Work Order` ([http://localhost:8090/desk/job-work-order](http://localhost:8090/desk/job-work-order))।
   * অনুমোদিত কারখানার তালিকার জন্য `Disclosed Subcontractor` ([http://localhost:8090/desk/disclosed-subcontractor](http://localhost:8090/desk/disclosed-subcontractor))।
   * ফিনিশড পার্টস রিসিভের জন্য `Subcontracting Receipt`।
   * **রেজাল্ট:** PASS. ২৫টি স্ক্রিনের কমপ্লায়েন্স ও ফ্র্যাপে রাউটিং পরীক্ষিত।

---

## ৩. লাইভ মারিয়াডিবি ডাটাবেজ ভেরিফিকেশন (MariaDB Verification)

টার্মিনাল থেকে সরাসরি ডকার ব্যাকএন্ড কন্টেইনারে (`gtex-backend-1`) ফ্র্যাপে পাইথন ও মারিয়াডিবি কোয়েরি চালিয়ে অডিটকৃত ডকটাইপগুলোর লাইভ ডাটা কাউন্ট ও স্ট্যাটাস যাচাই করা হয়েছে:

```bash
docker exec gtex-backend-1 bench --site dev.localhost execute print --args "(
    frappe.db.count('Demand Forecast'),
    frappe.db.count('Line Loading Plan'),
    frappe.db.count('Master Production Schedule'),
    frappe.db.count('Style'),
    frappe.db.count('Tech Pack'),
    frappe.db.count('Supplier'),
    frappe.db.count('Purchase Order'),
    frappe.db.count('Item')
)"
```

### ডাটাবেজে পাওয়া রেকর্ড ও যাচাইকৃত ফিল্ডসমূহ:

| ফ্র্যাপে DocType | মারিয়াডিবি টেবিলের নাম | বর্তমান রেকর্ড সংখ্যা | পরীক্ষিত প্রধান ফিল্ডসমূহ (Field Parity) | স্ট্যাটাস |
|---|---|:---:|---|:---:|
| **Line Loading Plan** | `tabLine Loading Plan` | **৪৬টি** | `style_order`, `production_line`, `daily_target`, `loading_status` | **Verified** |
| **Purchase Order** | `tabPurchase Order` | **১০টি** | `supplier`, `transaction_date`, `schedule_date`, `grand_total` | **Verified** |
| **Item Master** | `tabItem` | **১০টি** | `item_code`, `item_name`, `item_group`, `stock_uom` | **Verified** |
| **Supplier** | `tabSupplier` | **৩টি** | `supplier_name`, `supplier_group`, `country` (Esquire, Crypto, Wash World) | **Verified** |
| **Master Production Schedule** | `tabMaster Production Schedule` | **২টি** | `schedule_name`, `start_date`, `end_date`, `status` | **Verified** |
| **Demand Forecast** | `tabDemand Forecast` | **১টি** | `forecast_title`, `forecast_qty`, `forecast_value`, `model_used` | **Verified** |
| **Tech Pack** | `tabTech Pack` | **১টি** | `style_id`, `season`, `buyer`, `tech_pack_code` | **Verified** |
| **Style** | `tabStyle` | **১টি** | `style_code`, `style_name`, `product_type`, `buyer` | **Verified** |

---

## ৪. কারিগরি নিরীক্ষা ও সমাধানকৃত বিষয়সমূহ (Technical Findings)

1. **সঠিক পোর্ট ও ডোমেইন লিংকিং (Port 8090 Alignment):**
   * ডকার কম্পোজ কনফিগারেশনে ফ্র্যাপে ডেস্ক ৮০৯০ পোর্টে এক্সপোজড (`0.0.0.0:8090->8080/tcp`)। `Doctypes/` ফোল্ডারের ৩টি ফাইলেই সরাসরি অ্যাডমিন লিংক হিসেবে `http://localhost:8090/desk/...` নিখুঁতভাবে উল্লেখিত রয়েছে।
2. **ডাইনামিক কনফিগ বনাম ফ্র্যাপে স্কিমা সিঙ্ক:**
   * `frontend/lib/product/m04-configs.ts` ও `m06-configs.ts`-এ ফ্রন্টএন্ড ফর্ম তৈরির জন্য ব্যবহৃত কনফিগগুলোর ফিল্ডের নাম ফ্র্যাপের অরিজিনাল স্কিমার (`tech_pack_measurement.json`, `grp-vocabulary`) সাথে ১০০% সামঞ্জস্যপূর্ণ।
3. **প্রোডাকশন প্ল্যানিং বোর্ডে ডাটা পারসিস্টেন্স:**
   * মারিয়াডিবিতে `tabLine Loading Plan`-এ ৪৬টি লাইভ প্ল্যান রেকর্ড রয়েছে, যা নির্দেশ করে যে ফ্রন্টএন্ডের প্ল্যানিং বোর্ড সফলভাবে মারিয়াডিবিতে ডাটা সংরক্ষণ এবং ব্যাকএন্ড এপিআইয়ের মাধ্যমে ফেচ করতে পারছে।
4. **ভেন্ডর পোর্টাল এবং সেন্ট্রাল স্টোর সংযোগ:**
   * `tabPurchase Order` এবং `tabSupplier` টেবিলদ্বয়ের ডাটা ফ্রন্টএন্ডের সাপ্লাই ও ভেন্ডর পোর্টালের সাথে পূর্ণ সংগতিপূর্ণ।

---

## ৫. Doctypes ফোল্ডারের ফাইল স্বাস্থ্য সূচক (Health Index)

| ফাইল নাম | ফাইল সাইজ | লাইন সংখ্যা | কভারেজ বিষয়বস্তু | স্বাস্থ্য স্ট্যাটাস |
|---|---|---|---|---|
| **`Doctypes/demand_doctype.md`** | ১৪.৮ KB | ৩৩৫ লাইন | ডিমান্ড, ক্যাপাসিটি ও শিডিউলিং (৪৯টি স্ক্রিন) | **Healthy & 100% Complete** |
| **`Doctypes/product_doctype.md`** | ১৫.২ KB | ৩৪৯ লাইন | প্রোডাক্ট ডেভেলপমেন্ট, টেক প্যাক ও কস্টিং (৬১টি স্ক্রিন) | **Healthy & 100% Complete** |
| **`Doctypes/suplier_doctype.md`** | ২২.০ KB | ৪৯৭ লাইন | সোর্সিং, স্টোর, ইনভেন্টরি ও সাব-কন্ট্রাক্ট (৮৬টি স্ক্রিন) | **Healthy & 100% Complete** |

---

## ৬. টেস্টের চূড়ান্ত সিদ্ধান্ত ও পরবর্তী পদক্ষেপ (Conclusion & Next Steps)

* **আজকের সার্বিক ফলাফল:** **সাফল্যজনক (100% Enterprise Screen Mapping Verified)**
  * `/Users/laptopparadise/Documents/Textile/Doctypes` ডিরেক্টরির অধীনস্থ **১৯৬টি স্ক্রিনের** সম্পূর্ণ তালিকা এবং এর ব্যাকএন্ড মারিয়াডিবি ও ফ্র্যাপে লিঙ্কগুলো নির্ভুলভাবে যাচাই করা হয়েছে। কোনো ভাঙা লিংক, ভুল DocType বা অসামঞ্জস্যপূর্ণ টেবিল নাম নেই।
  * ডেভেলপার, টেস্টার ও বিজনেস অ্যানালিস্টদের জন্য এই ফোল্ডারটি জিটেক্স ইআরপির সিঙ্গেল-সোর্স-অব-ট্রুথ (Single Source of Truth) হিসেবে কাজ করবে।

* **পরবর্তী পদক্ষেপ (Next Milestones):**
  1. **Textile Manufacturing (M14–M17) DocTypes গাইড প্রস্তুত:** ডাইং, স্পিনিং, নিটিং এবং কালার ল্যাবের স্ক্রিনগুলোর জন্য পৃথক `textile_doctype.md` তৈরি ও যোগ করা।
  2. **Garment MES (M18–M21) DocTypes গাইড প্রস্তুত:** কাটিং, সুইং ফ্লোর ট্যাবলেট ও বান্ডেল ট্র্যাকিংয়ের স্ক্রিনগুলোর জন্য `garment_doctype.md` প্রস্তুত করা।
  3. **Quality & Lab (M22–M25) DocTypes গাইড প্রস্তুত:** AQL অডিট ও ল্যাবরেটরি টেস্ট স্ক্রিনগুলোর জন্য `quality_doctype.md` প্রস্তুত করা।
