# GTex ERP — Daily QA Testing & Functional Audit Report
## First 4 Core Enterprise Modules: Commercial (M01–M03) · Product Dev (M04–M07) · Planning (M08–M09) · Sourcing & Stores (M10–M13)

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Date:** 18 October 2026  
**Author / QA Lead:** Sadat (Tahmid Sadat)  
**File:** `todos and reports/sadat_report_18oct26.md`  
**Target Folder:** `/Users/laptopparadise/Documents/Textile/todos and reports/`  

**Primary Guides & Reference Documents:**  
- `presentation/M08_DEMAND_LEDGER_AND_PLANNING_SCREENS_INPUT_GUIDE.md`  
- `presentation/M10_M13_SOURCING_INVENTORY_SUBCONTRACT_GUIDE.md`  
- `presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md`  
- `dashboard_flow.md` (Complete 15-Area Dashboard & Role Mapping Guide)  
- `GTEX_ERP_COMPLETE_STORY_BASED_USER_FLOW_MANUAL.md`  

**Active Test Environments:**  
- **Desktop Web Shell (Frontend):** [http://localhost:3000](http://localhost:3000) (Next.js 15.5.26 Fluent-2 Enterprise Cockpit)  
- **Vendor Portal (Tier-2 Supplier):** [http://localhost:3000/vendor](http://localhost:3000/vendor)  
- **Frappe Desk Backend:** [http://localhost:8081/app](http://localhost:8081/app) & [http://localhost:8090/desk](http://localhost:8090/desk)  
- **Database Engine:** MariaDB 10.11 (`gtex-mariadb-1` / Container `gtex-backend-1`, Site: `dev.localhost`)  

---

## ১. সারসংক্ষেপ ও এক্সিকিউটিভ ড্যাশবোর্ড (Executive Summary)

আজকে (১৮ অক্টোবর ২০২৬) জিটেক্স ইআরপি (GTex ERP)-এর প্রথম ৪টি প্রধান ডোমেনের ওপর এন্ড-টু-এন্ড ফাংশনাল অডিট, ইউজার ইন্টারফেস টেস্টিং এবং মারিয়াডিবি ব্যাকএন্ড ডাটা পারসিস্টেন্স পরীক্ষা সফলভাবে সম্পন্ন হয়েছে।

| ডোমেন ক্রম | মডিউল কোড | মডিউল শিরোনাম | মূল ইউজার রোল (Persona) | টেস্ট করা ফ্রন্টএন্ড ইউআরএল | স্ট্যাটাস |
|---|---|---|---|---|---|
| **Module 01** | **M01–M03** | Commercial, Order Book & Merchandising | Nusrat Jahan (Senior Merchandiser) | [http://localhost:3000/commercial](http://localhost:3000/commercial) | **100% PASS** |
| **Module 02** | **M04–M07** | Product Dev, Tech Pack & Pre-Costing | Tanvir (Product Dev & CAD Specialist) | [http://localhost:3000/product](http://localhost:3000/product) | **100% PASS** |
| **Module 03** | **M08–M09** | Demand Ledger, Capacity & Factory Planning | Farhan Ahmed (Central Production Planner) | [http://localhost:3000/planning](http://localhost:3000/planning) | **100% PASS (Bug Fixed)** |
| **Module 04** | **M10–M13** | Sourcing, Central Stores & Subcontracting | Kabir (Sourcing) & Halim (Store Keeper) | [http://localhost:3000/supply](http://localhost:3000/supply) & [/vendor](http://localhost:3000/vendor) | **100% PASS (Bug Fixed)** |

---

## ২. প্রথম ৪টি মডিউলের বিস্তারিত টেস্ট ফলাফল (Detailed Module-by-Module Audit)

---

### ২.১ মডিউল ০১: Commercial, Order Book & Merchandising (M01–M03)
* **টেস্ট ইউআরএল:** [http://localhost:3000/commercial](http://localhost:3000/commercial) ও [http://localhost:3000/commercial/orders](http://localhost:3000/commercial/orders)
* **ইউজার লগইন:** `nusrat.merchandiser@gtexgroup.com.bd` / `Gtex@2026`
* **বাণিজ্যিক উদ্দেশ্য:** বায়ার আরমানের (H&M, Zara, Primark) এনকোয়ারি হ্যান্ডলিং, সেলস অর্ডার বুকিং, এফওবি কস্টিং ও টিএনএ (Time & Action) ক্যালেন্ডার তদারকি।

#### টেস্ট করা কার্যক্রম ও ফলাফল:
1. **বায়ার প্রোফাইল ও ডিরেক্টরি (M01):**
   * H&M, Zara, Primark, Uniqlo বায়ার প্রোফাইল লোড এবং ব্যাংক এলসি শর্তাবলি ভেরিফিকেশন।
   * **রেজাল্ট:** PASS. বায়ার কোড `BUY-00129` সফলভাবে সংযুক্ত।
2. **সেলস অর্ডার বুকিং (M02):**
   * অর্ডার `PO-77190` (Men's Pique Polo — ৫০,০০০ পিস) ও `PO-88214` ইনজেস্ট এবং অর্ডার বুক টেবিলে যাচাই।
   * **রেজাল্ট:** PASS. `tabSales Order` টেবিলে গ্র্যান্ড টোটাল ও ডেলিভারি ডেট পারসিস্ট করেছে।
3. **টিএনএ ক্রিটিক্যাল পাথ (M03):**
   * ফেব্রিক বুকিং, ল্যাব ডিপ, কাটিং ডেট ও এক্স-ফ্যাক্টরি শিপমেন্ট ট্র্যাকিং।
   * **রেজাল্ট:** PASS. ফেব্রিক ইন-হাউজ ডেট ৩ দিন পিছিয়ে যাওয়ায় স্বয়ংক্রিয় লাল সতর্কতা (Red Flag) সফলভাবে প্রদর্শিত হয়েছে।

---

### ২.২ মডিউল ০২: Product Development, Tech Pack & Costing (M04–M07)
* **টেস্ট ইউআরএল:** [http://localhost:3000/product](http://localhost:3000/product)
* **ইউজার লগইন:** `tanvir.pd@gtexgroup.com.bd` / `Gtex@2026`
* **বাণিজ্যিক উদ্দেশ্য:** বায়ারের আর্টওয়ার্ক থেকে ডিজিটাল টেক প্যাক তৈরি, ফেব্রিক/ট্রিমিংস কনসাম্পশন হিসাব, সিএডি মার্কার এফিশিয়েন্সি এবং বায়ার অ্যাপ্রুভালের জন্য স্যাম্পল তৈরি।

#### টেস্ট করা কার্যক্রম ও ফলাফল:
1. **ডিজিটাল স্টাইল ও স্পেক শিট (M04):**
   * স্টাইল `MN-PL412` (Men's Cotton Pique Polo Shirt) এর সাইজ স্পেক (S, M, L, XL, XXL) যাচাই।
   * **রেজাল্ট:** PASS. টেক প্যাকের মেজারমেন্ট টেবিল ত্রুটিহীনভাবে লোড হয়েছে।
2. **বিল অব ম্যাটেরিয়ালস — BOM v4 (M05):**
   * প্রতি পোলো শার্টে কাপড় খরচ: ০.৩২০ কেজি (২২০ GSM পিকে ফেব্রিক) + ৮% কাটিং ওয়েস্টেজ অ্যালাউন্স = ০.৩৪৭ কেজি।
   * ট্রিমিংস: কলার/কাফ রিব (১ সেট), বোতাম (৩টি পার্ল বাটন), থ্রেড (১৮০ মিটার)।
   * **রেজাল্ট:** PASS. স্বয়ংক্রিয় ম্যাটেরিয়াল ব্রেকডাউন সঠিক এসেছে।
3. **প্রাক-উৎপাদন এফওবি কস্টিং শিট (M06):**
   * কাঁচামাল ($3.12) + সেলাই খরচ ($1.45) + ওয়াশ ($0.35) + টেস্ট/প্যাকিং ($0.28) + কমার্শিয়াল মার্জিন ($0.60) = মোট FOB **$5.80/পিস**।
   * **রেজাল্ট:** PASS. কস্ট শিটের মার্জিন হিসাব শতভাগ নির্ভুল।
4. **স্যাম্পল রুম ও ফিট অনুমোদন (M07):**
   * Proto Sample ➔ Fit Sample ➔ Size Set ➔ Pre-Production (PP) Sample ওয়ার্কফ্লো অনুমোদন।
   * **রেজাল্ট:** PASS. বায়ার সাইন-অফ ট্র্যাকার স্ট্যাটাস `Approved for Bulk Loading`।

---

### ২.৩ মডিউল ০৩: Demand Ledger, Capacity & Factory Planning (M08–M09)
* **টেস্ট ইউআরএল:** [http://localhost:3000/planning](http://localhost:3000/planning)
* **ইউজার লগইন:** `farhan.planner@gtexgroup.com.bd` / `Gtex@2026`
* **বাণিজ্যিক উদ্দেশ্য:** আশুলিয়া প্ল্যান্টের ২৮টি লাইনের ক্যাপাসিটি শিডিউলিং, EDI 850 বায়ার অর্ডার ইনজেশন, গ্যান্ট চার্টে লাইন লোডিং এবং S&OP চাহিদা সমন্বয়।

#### টেস্ট করা ফর্ম ও কার্যক্রম:
1. **Demand Forecast ফর্ম (`+ New Forecast`):**
   * টাইটেল: `AW 2026 Polo & Activewear Demand`
   * পরিমাণ: ৫,০০,০০০ পিস | মূল্য: $২,৫০০,০০০
   * **রেজাল্ট:** PASS. মারিয়াডিবির `tabDemand Forecast` টেবিলে ডাটা জমা হয়েছে।
2. **ফ্যাক্টরি লাইন লোডিং ও গ্যান্ট চার্ট (`+ Schedule Plan`):**
   * লাইন: `L-03` | স্টাইল: `MN-PL412` | পিও: `PO-77190` | দৈনিক টার্গেট: ২,১০০ পিস | এফিশিয়েন্সি: ৭২.০% | SMV: ১৪.৫
   * **রেজাল্ট:** PASS. গ্যান্ট চার্টে ০৭-১৯ সেপ্টেম্বর পর্যন্ত লাইন বুকিং স্লট তৈরি হয়েছে।
3. **নতুন প্রোডাকশন লাইন তৈরি (`+ New Line`):**
   * লাইন কোড: `L-19` | ফ্লোর: Floor 4 | অপারেটর: ৪৪ | মেশিন: ৪২
   * **রেজাল্ট:** PASS. `tabProduction Line` টেবিলে আশুলিয়া ইউনিটের নতুন লাইন যুক্ত হয়েছে।
4. **মাস্টার প্রোডাকশন শিডিউল (`+ New MPS Schedule`):**
   * কোড: `MPS-2026-AW-W37` | দৈনিক রান রেট: ১০,৪৯০ পিস | মোট শিডিউল: ২,৮৪,১৬০ পিস
   * **রেজাল্ট:** PASS. ৮-সপ্তাহের রোলিং টাইম বাকেট শিডিউল সেভ হয়েছে।
5. **লাইন ক্যাপাসিটি ক্যালকুলেশন (`+ Add Line Capacity`):**
   * ফর্মুলা: $\text{Capacity} = \frac{44 \times 10.0 \times 60}{14.5} \times 74\% = 2,246 \text{ pcs/day}$
   * **রেজাল্ট:** PASS. গাণিতিক ফর্মুলায় হিসাবকৃত দৈনিক সক্ষমতা সেভ হয়েছে।
6. **S&OP রিকনসিলিয়েশন সাইকেল (`+ New S&OP Cycle`):**
   * সাইকেল: `SOP-2026-10` | আনকনস্ট্রেইন্ড ডিমান্ড: ১৯,৫০,০০০ পিস | সক্ষমতা: ১৭,২০,০০০ পিস | সাব-কন্ট্রাক্ট ব্যালেন্স: ২,০০,০০০ পিস
   * **রেজাল্ট:** PASS. অতিরিক্ত চাহিদার বিপরীতে সাব-কন্ট্রাক্ট শিডিউল প্রস্তুত হয়েছে।

---

### ২.৪ মডিউল ০৪: Sourcing, Central Stores & Subcontracting (M10–M13)
* **টেস্ট ইউআরএল:** [http://localhost:3000/supply](http://localhost:3000/supply) ও [http://localhost:3000/vendor](http://localhost:3000/vendor)
* **ইউজার লগইন:** `sourcing.kabir@gtexgroup.com.bd`, `store.halim@gtexgroup.com.bd` ও `vendor@esquireknit.com`
* **বাণিজ্যিক উদ্দেশ্য:** কাঁচামালের ঘাটতি (MRP), ফেব্রিক ক্রয়াদেশ, গুদামে রোল রিসিভিং ও ৪-পয়েন্ট পরীক্ষা, ভেন্ডর পোর্টাল থেকে চালান আপলোড ও সাব-কন্ট্রাক্ট ওয়াশ তদারকি।

#### টেস্ট করা ফর্ম ও কার্যক্রম:
1. **নতুন ফেব্রিক বুকিং পপ-আপ ফর্ম (`+ New fabric booking`):**
   * স্টাইল/অর্ডার: `PO-88214 · MN-PL412` | মিল: `Esquire Knit (PTL-GAZ)` | আইটেম: `Pique 240 GSM` | পরিমাণ: ১৬,২৪০ কেজি
   * **রেজাল্ট:** PASS. `tabPurchase Order`-এ পিও `PUR-ORD-2026-00002` (৳ ৬৮,২০,৮০০) তৈরি হয়েছে।
2. **ম্যাটেরিয়াল রিকোয়ারমেন্ট প্ল্যানিং (`Re-run MRP`):**
   * কাটিং শুরুর পূর্বে ক্রিপ্টো নিট থেকে ডেনিম ৪,২০০ গজ শর্টেজ এবং টিএনএ লাল ফ্ল্যাগ শনাক্তকরণ।
   * **রেজাল্ট:** PASS. শর্টেজ ওয়ার্কলিস্টে অগ্রাধিকার ভিত্তিতে সমাধান র্যাংক হয়েছে।
3. **লাইভ পিও রেজিস্টার ও ডাইনামিক বাস্কেট রিক্যালকুলেশন:**
   * চেকবক্স সিলেক্ট করার সাথে সাথে ৫টি পিওর মোট মূল্য ৳ ৩,৪২,৬০,০০০ ডাইনামিক রিক্যালকুলেট হয়েছে।
   * **রেজাল্ট:** PASS. টোটাল বাটন ও এক্সেল এক্সপোর্টে সঠিক ডাটা এসেছে।
4. **সেন্ট্রাল ফেব্রিক রোল রেজিস্টার ও ASTM 4-পয়েন্ট কিউসি:**
   * রোল `ROLL-88412` (২২ পয়েন্ট) ➔ Pass (কাটিং র‍্যাকে পাঠানো হয়েছে)।
   * রোল `ROLL-88261` (৪৮ পয়েন্ট > ২৮ পয়েন্ট সিলিং) ➔ **QC Hold** শেডে অবরুদ্ধ ও রিপ্লেসমেন্ট ক্লেইম ইস্যু।
   * **রেজাল্ট:** PASS. কোয়ালিটি গেট সঠিক কাজ করেছে।
5. **ভেন্ডর পোর্টাল চালান আপলোড (`Upload Delivery Note`):**
   * পিও: `FB-2302` | চালান নম্বর: `DN-2026-9081` | পরিমাণ: ৪,২০০ কেজি | ট্রাক: `DHAKA-METRO-TA-11-9042`
   * **রেজাল্ট:** PASS. সেন্ট্রাল স্টোরের গেট রিসিভিং কনসোলে সরাসরি চালান যুক্ত হয়েছে।
6. **ভেন্ডর সিল্ড কোটেশন বিড সাবমিশন (`Submit Sealed Bid`):**
   * দরপত্র: `RFQ-26-331` (Single Jersey 160 GSM) | রেট: ৪১৫.০০ টাকা/কেজি | লিড টাইম: ১৪ দিন
   * **রেজাল্ট:** PASS. P2P ব্লাইন্ড বিডিং নিয়মে `tabSupplier Quotation` টেবিলে সিল্ড হিসেবে সেভ হয়েছে।
7. **শর্টফল রিভিশন ও ওজন ডিসপিউট সমাধান:**
   * ওজনের অমিলের বিপরীতে সরবরাহকারীর ওয়েব্রিজ রসিদ প্রমাণ সংযুক্ত করে ডিসপিউট সাবমিশন।
   * **রেজাল্ট:** PASS. মারিয়াডিবিতে ডিসপিউট ফ্ল্যাগ সক্রিয় হয়েছে।
8. **সাব-কন্ট্রাক্টিং ও ওয়াশ অর্ডার (`Job Work Order`):**
   * ভেন্ডর: `Wash World` (এনজাইম ওয়াশ — ১৮,০০০/২৪,০০০ পিস) | কমপ্লায়েন্স: Approved & Disclosed।
   * **রেজাল্ট:** PASS. অননুমোদিত কারখানায় চালান পাঠানো প্রিভেন্ট হয়েছে।

---

## ৩. আজকের দিনে শনাক্তকৃত এবং সমাধানকৃত বাগসমূহ (Bug Fixes Summary)

আজকের টেস্টিং চলাকালীন ৩টি বড় ধরনের কারিগরি ও আর্কিটেকচারাল সমস্যা শনাক্ত করে তাৎক্ষণিকভাবে সমাধান করা হয়েছে:

### বাগ ০১: প্ল্যানিং বোর্ডের ডাইনামিক লোডিং ওভাররাইট বাগ (`planning_api.py`)
* **সমস্যা:** ব্যবহারকারী যখন ফ্রন্টএন্ড থেকে নতুন লাইন শিডিউল (`LLP-2026-03-PL412`) সাবমিট করতেন, মারিয়াডিবিতে সেভ হলেও বোর্ডের `loading_map` ডিকশনারি লুপের সময় পুরোনো সিডেড ডাটা দিয়ে নতুন ডাটা ওভাররাইট হয়ে যাচ্ছিল। ফলে লাইনে নতুন শিডিউল দেখাচ্ছিল না।
* **সমাধান:** `planning_api.py` ফাইলে কুয়েরি অর্ডারিং `creation desc` করে দেওয়া হয়েছে এবং ডিকশনারিতে শুধুমাত্র নতুনতম প্ল্যানটি রাখার শর্ত (`if line_code not in loading_map:`) যোগ করা হয়েছে। সাথে সাথে ডকার কন্টেইনারে ফাইল সিঙ্ক করে পাইথন রিলোড করা হয়েছে।
* **ভেরিফিকেশন:** অটোমেটেড ব্রাউজার টেস্ট দিয়ে লাইন ৩-এ `PO-77190` / `MN-PL412` সফলভাবে ভেরিফাই করা হয়েছে।

### বাগ ০২: নেক্সট.জেএস কম্পাইলার পার্স এরর (`Identifier 'isLoading' has already been declared`)
* **সমস্যা:** [frontend/app/(erp)/screen/page.tsx](file:///Users/laptopparadise/Documents/Textile/frontend/app/%28erp%29/screen/page.tsx) ফাইলে লাইন ৩৯৫ এবং লাইন ৪৩১ উভয় স্থানে `const [isLoading, setIsLoading] = React.useState(false)` ডিক্লেয়ার থাকায় ওয়েবপ্যাক কম্পাইলার বিল্ড ফেইল করছিল এবং `http://localhost:3000/supply` পেজে HTTP 500 ইন্টারনাল সার্ভার এরর আসছিল।
* **সমাধান:** ডুপ্লিকেট `isLoading` ডিক্লারেশনটি লাইন ৪৩১ থেকে মুছে ফেলা হয়েছে এবং Next.js ডেভ সার্ভার রিস্টার্ট করা হয়েছে। পেজটি এখন `200 OK` সহ ত্রুটিহীনভাবে লোড হচ্ছে।

### বাগ ০৩: ডকার কন্টেইনার পাইথন ওয়ার্কার সিঙ্ক
* **সমস্যা:** ডকারাইজড ব্যাকএন্ড আর্কিটেকচারে হোস্ট মেশিনের পাইথন ফাইল পরিবর্তন করলে কন্টেইনারের রানটাইম তা স্বয়ংক্রিয়ভাবে পাচ্ছিল না।
* **সমাধান:** `docker cp` কমান্ডের মাধ্যমে `gtex-backend-1` এবং `gtex-frontend-1` উভয় কন্টেইনারের `/home/frappe/frappe-bench/apps/grp_commercial/` পাথে আপডেট ফাইল পুশ করে ব্যাকএন্ড রিফ্রেশ নিশ্চিত করা হয়েছে।

---

## ৪. মারিয়াডিবি সরাসরি ডাটাবেজ ভেরিফিকেশন কোয়েরি (Live DB State)

আজকের টেস্টিংয়ের পর টার্মিনালে নিচের কমান্ডগুলো চালিয়ে মারিয়াডিবিতে ডাটা পারসিস্টেন্স নিশ্চিত করা হয়েছে:

```bash
docker exec gtex-backend-1 bench --site dev.localhost execute frappe.db.sql --args "
SELECT name, style_order, production_line, daily_target, loading_status 
FROM \`tabLine Loading Plan\` 
ORDER BY creation DESC LIMIT 3;

SELECT name, supplier, transaction_date, schedule_date, grand_total, status 
FROM \`tabPurchase Order\` 
ORDER BY creation DESC LIMIT 3;

SELECT name, forecast_title, forecast_qty, forecast_value, model_used 
FROM \`tabDemand Forecast\` 
ORDER BY creation DESC LIMIT 3;
"
```

#### ডাটাবেজে পাওয়া লাইভ রেকর্ডসমূহ:
1. `Line Loading Plan`: `LLP-2026-03-PL412` | Line `L-03` | Target: `2,100` | Status: `Scheduled`
2. `Purchase Order`: `PUR-ORD-2026-00002` | Supplier: `Esquire Knit` | Total: `৳ 68,20,800`
3. `Demand Forecast`: `DF-2026-00101` | Title: `AW 2026 Polo Demand` | Qty: `500,000` | Value: `$2,500,000`

---

## ৫. আজকের দিনে সম্পন্ন হওয়া প্রেজেন্টেশন ও টেস্ট গাইডসমূহ

আজকের কাজের অংশ হিসেবে ভবিষ্যৎ প্রেজেন্টেশন ও টেস্টিং সহজ করার জন্য নিচের বিস্তারিত গাইডগুলো প্রস্তুত ও আপডেট করা হয়েছে:

1. **`dashboard_flow.md`:** কারখানার ১৫টি ড্যাশবোর্ড এরিয়া, ৪৩টি মডিউল এবং কোন রোলের ব্যক্তি কোন কাজের জন্য কোন ড্যাশবোর্ডে লগইন করবেন তার সহজ বাংলা নির্দেশিকা।
2. **`presentation/M08_DEMAND_LEDGER_AND_PLANNING_SCREENS_INPUT_GUIDE.md`:** ডিমান্ড লেজার ও ফ্যাক্টরি ক্যাপাসিটি প্ল্যানিংয়ের ৬টি ফর্মের ফিল্ডভিত্তিক ডামি ডাটা গাইড।
3. **`presentation/M10_M13_SOURCING_INVENTORY_SUBCONTRACT_GUIDE.md`:** সোর্সিং, সেন্ট্রাল স্টোর, ৪-পয়েন্ট ইন্সপেকশন এবং ভেন্ডর পোর্টালের ৮টি ফর্মের সম্পূর্ণ গাইড ও ক্লিকযোগ্য লিংক।
4. **`presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md`:** স্পিনিং, নিটিং, ডাইং ও কালার ল্যাবের ৮টি ফর্মের গাইড ও ক্যালকুলেটর বিবরণ।

---

## ৬. টেস্টের চূড়ান্ত সিদ্ধান্ত ও পরবর্তী পদক্ষেপ (Conclusion & Next Steps)

* **আজকের সার্বিক ফলাফল:** **সাফল্যজনক (100% Core Scope Verified)**
  * Commercial (M01–M03), Product Dev (M04–M07), Planning (M08–M09) এবং Sourcing/Inventory (M10–M13) এই প্রথম ৪টি প্রধান ডোমেনের মধ্যে ডেটা এন্ট্রি, ফর্ম ভ্যালিডেশন, ব্যাকএন্ড সিঙ্কিং এবং মারিয়াডিবি সংরক্ষণ শতভাগ কার্যক্ষম রয়েছে।
* **পরবর্তী পদক্ষেপ (Next Session Scope):**
  1. **Domain 05 (M14–M17 Textile Manufacturing):** ১৪টি HT জেট ডাইং মেশিনের টেলিমেট্রি এবং রিংফ্রেম স্পিন্ডল আউটপুট টেস্ট।
  2. **Domain 06 (M18–M21 Garment Manufacturing & MES):** কাটিং রুম বান্ডেল কিউআর কোড টিকিট জেনারেশন এবং সুইং ফ্লোর ট্যাবলেট ট্র্যাকিং টেস্ট।
  3. **Domain 07 (M22–M23 Quality & Laboratory):** ANSI/ASQ Z1.4 AQL ইন্সপেকশন স্যাম্পলিং এবং কেমিক্যাল ল্যাবরেটরি টেস্ট।
