# সমবায় ফান্ড প্রো — Complete App Guide

# Somobay Fund Pro — সম্পূর্ণ অ্যাপ গাইড

---

## 📖 Table of Contents / সূচিপত্র

1. [App Overview / অ্যাপের সারসংক্ষেপ](#1-app-overview--অ্যাপের-সারসংক্ষেপ)
2. [Data Architecture / ডেটা আর্কিটেকচার](#2-data-architecture--ডেটা-আর্কিটেকচার)
3. [Financial Logic / আর্থিক যুক্তি](#3-financial-logic--আর্থিক-যুক্তি)
4. [Transaction Types / লেনদেনের ধরন](#4-transaction-types--লেনদেনের-ধরন)
5. [Due System / বকেয়া ব্যবস্থা](#5-due-system--বকেয়া-ব্যবস্থা)
6. [Member Management / সদস্য ব্যবস্থাপনা](#6-member-management--সদস্য-ব্যবস্থাপনা)
7. [Loan System / ঋণ ব্যবস্থা](#7-loan-system--ঋণ-ব্যবস্থা)
8. [App Views & Navigation / অ্যাপের ভিউ ও নেভিগেশন](#8-app-views--navigation--অ্যাপের-ভিউ-ও-নেভিগেশন)
9. [Settings / সেটিংস](#9-settings--সেটিংস)
10. [Developer Reference / ডেভেলপার রেফারেন্স](#10-developer-reference--ডেভেলপার-রেফারেন্স)

---

## 1. App Overview / অ্যাপের সারসংক্ষেপ

### English

**Somobay Fund Pro** is a cooperative society (সমবায় সমিতি) financial management Progressive Web App. It tracks:

- **11 members** with individual deposits, profiles, and financial history
- **7 transaction types**: Deposit, Loan Given, Loan Repaid, Profit, Fine, Expense, Loss
- **Monthly installment dues** (৳8,000/member/month)
- **Loan management** with profit tracking per loan holder
- **Financial summaries** (overall + monthly breakdown)

**Tech Stack**: Pure HTML + CSS + Vanilla JavaScript, localStorage for persistence.

### বাংলা

**সমবায় ফান্ড প্রো** হলো একটি সমবায় সমিতির আর্থিক ব্যবস্থাপনা অ্যাপ। এটি ট্র্যাক করে:

- **১১ জন সদস্য** — তাদের জমা, প্রোফাইল ও আর্থিক ইতিহাস
- **৭ ধরনের লেনদেন**: জমা, ঋণ প্রদান, ঋণ ফেরত, আয়, জরিমানা, খরচ, ক্ষতি
- **দ্বৈত তারিখ ট্র্যাকিং (Dual-Date)**: প্রকৃত জমার তারিখ বনাম অ্যাকাউন্টিং মাস ট্র্যাকিং
- **লাভ বণ্টন ও জরিমানা**: জমা অনুপাতে প্রফিট শেয়ার এবং বিলম্বের জন্য অটো-পেনাল্টি
- **আর্থিক সারসংক্ষেপ** (সামগ্রিক + মাসিক ভাঙানো)

**প্রযুক্তি**: HTML + CSS + Vanilla JavaScript, localStorage এ ডেটা সংরক্ষিত হয়।

---

## 2. Data Architecture / ডেটা আর্কিটেকচার

### Storage Key / স্টোরেজ কী

```
localStorage key: "somobayFundDataV4"
```

### Data Structure / ডেটা কাঠামো

```javascript
{
  config: {
    monthlyInstallment: 8000,  // মাসিক কিস্তি (৳)
    fundName: "সমবায় ফান্ড প্রো"  // সমিতির নাম
  },
  members: [
    {
      id: 1,                    // Unique ID / অনন্য আইডি
      name: "মিজান ১ (Mizan 1)", // Name / নাম
      deposited: 17000,          // Total deposited amount / মোট জমা
      phone: "01788378146",      // Phone number / ফোন নম্বর
      accountNo: "FD0002",       // Account number / একাউন্ট নম্বর
      openingDate: "2023-01-01"  // Joining date / যোগদানের তারিখ
    }
  ],
  transactions: [
    {
      id: 101,                     // Unique ID
      type: "profit",              // Transaction type / লেনদেনের ধরন
      desc: "B-এর ঋণের লাভ",       // Description / বিবরণ
      amount: 12000,               // Amount in BDT / পরিমাণ (টাকা)
      date: "2026-01-31T...",      // ISO date string / তারিখ
      source: "B"                  // (Optional) Profit source / আয়ের উৎস
    }
  ],
  depositHistory: [
    {
      id: 1709...,               // Timestamp ID
      memberId: 4,               // Which member / কোন সদস্য
      memberName: "রহিম ১",       // Member name / সদস্যের নাম
      desc: "রহিম ১ জমা প্রদান",   // Description
      amount: 8000,              // Deposit amount / জমার পরিমাণ
      date: "2026-03-01T..."     // Date / তারিখ
    }
  ]
}
```

### Default Members / ডিফল্ট সদস্য তালিকা

| ID        | Name / নাম        | Deposited / জমা | Phone / ফোন |
| --------- | ----------------- | --------------- | ----------- |
| 1         | মিজান ১ (Mizan 1) | ৳17,000         | —           |
| 2         | মিজান ২ (Mizan 2) | ৳17,000         | —           |
| 3         | বাপ্পি (Bappi)    | ৳16,000         | —           |
| 4         | রহিম ১ (Rahim 1)  | ৳26,000         | 01788378146 |
| 5         | রহিম ২ (Rahim 2)  | ৳26,000         | —           |
| 6         | রাকিব ১ (Rakib 1) | ৳29,000         | —           |
| 7         | রাকিব ২ (Rakib 2) | ৳29,000         | —           |
| 8         | জুয়েল (Juwel)    | ৳27,000         | —           |
| 9         | আখতার (Akhter)    | ৳21,000         | —           |
| 10        | আনোয়ার (Anowar)  | ৳23,000         | —           |
| 11        | জিকু (Ziku)       | ৳29,000         | —           |
| **Total** | **১১ জন**         | **৳260,000**    |             |

---

## 3. Financial Logic / আর্থিক যুক্তি

### 3.1 Core Formulas / মূল সূত্রসমূহ

#### Liquid Cash / নগদ তহবিল

```
English:
Liquid Cash = (Total Deposits + Profit + Fines + Loans Repaid)
            − (Loans Given + Expenses + Losses)

বাংলা:
নগদ তহবিল = (মোট জমা + আয় + জরিমানা + ঋণ ফেরত)
           − (ঋণ প্রদান + খরচ + ক্ষতি)
```

**Explanation / ব্যাখ্যা:**

- EN: This is the actual cash available in the fund right now. It counts all money that came in (deposits, profits, fines, loan repayments) minus all money that went out (loans given, expenses, losses).
- বাং: এটি হলো তহবিলে এই মুহূর্তে প্রকৃতপক্ষে কত নগদ আছে। সব আসা টাকা (জমা, আয়, জরিমানা, ঋণ ফেরত) থেকে সব যাওয়া টাকা (ঋণ, খরচ, ক্ষতি) বাদ দেওয়া হয়।

#### Fund Equity / মূলধন

```
English:
Fund Equity = Total Deposits + Profit + Fines − Expenses − Losses

বাংলা:
মূলধন = মোট জমা + আয় + জরিমানা − খরচ − ক্ষতি
```

**Explanation / ব্যাখ্যা:**

- EN: The total value of the fund excluding outstanding loans. This represents what the fund is worth in terms of member deposits plus net earnings.
- বাং: বকেয়া ঋণ বাদ দিয়ে তহবিলের মোট মূল্য। এটি সদস্যদের জমা এবং নিট আয়ের ভিত্তিতে তহবিলের মূল্য প্রকাশ করে।

#### Net Profit / নিট লাভ

```
English:
Net Profit = Profit + Fines − Expenses − Losses

বাংলা:
নিট লাভ = আয় + জরিমানা − খরচ − ক্ষতি
```

**Explanation / ব্যাখ্যা:**

- EN: Total fund Earnings. This is the pool used for distribution among members after penalty adjustments.
- বাং: খরচ ও ক্ষতি বাদ দেওয়ার পর তহবিলের প্রকৃত আয়। এটিই সদস্যদের মধ্যে বণ্টনের মূল পুল।

#### Weighted Profit Share / আনুপাতিক লভ্যাংশ

```
English:
Member Share = (Member's Total Deposit ÷ Total Fund Deposit) × Net Profit

বাংলা:
সদস্যের অংশ = (সদস্যের মোট জমা ÷ তহবিলের মোট জমা) × নিট লাভ
```

**Explanation / ব্যাখ্যা:**

- EN: Each member's share is now proportional to their contribution (capital logic).
- বাং: লভ্যাংশ এখন জমার অনুপাত অনুযায়ী বণ্টন করা হয়। যার জমা যত বেশি, তার লাভের অংশ তত বড়।

#### Discipline Penalty & Bonus / ডিসিপ্লিন পেনাল্টি ও বোনাস

**Explanation / ব্যাখ্যা:**

- EN: If a member pays after the target month ends, a daily penalty (default 0.1%) is deducted from their share. This deducted pool is redistributed to members who paid on time.
- বাং: কোনো সদস্য টার্গেট মাসের মধ্যে জমা না দিলে পরবর্তী মাসের ১ তারিখ থেকে প্রতিদিন ০.১% হারে লভ্যাংশ থেকে পেনাল্টি কাটা হয়। এই টাকা সময়মতো জমা দেওয়া সদস্যদের বাড়তি বোনাস হিসেবে দেওয়া হয়।

#### Outstanding Loans / বকেয়া ঋণ

```
English:
Outstanding Loans = Total Loans Given − Total Loans Repaid

বাংলা:
বকেয়া ঋণ = মোট প্রদত্ত ঋণ − মোট ফেরত
```

### 3.2 Numerical Example / সংখ্যাগত উদাহরণ

With the default data / ডিফল্ট ডেটা দিয়ে:

| Item / বিষয়                      | Value / মান                                                            |
| --------------------------------- | ---------------------------------------------------------------------- |
| Total Deposits / মোট জমা          | ৳260,000 (11 members)                                                  |
| Profit / আয়                      | ৳14,000 (৳12,000 Jan + ৳2,000 Feb)                                     |
| Fines / জরিমানা                   | ৳1,000                                                                 |
| Expenses / খরচ                    | ৳100                                                                   |
| Losses / ক্ষতি                    | ৳0                                                                     |
| Loans Given / ঋণ প্রদান           | ৳138,000                                                               |
| Loans Repaid / ঋণ ফেরত            | ৳0                                                                     |
|                                   |                                                                        |
| **Liquid Cash / নগদ তহবিল**       | ৳260,000 + ৳14,000 + ৳1,000 + ৳0 − ৳138,000 − ৳100 − ৳0 = **৳136,900** |
| **Fund Equity / মূলধন**           | ৳260,000 + ৳14,000 + ৳1,000 − ৳100 − ৳0 = **৳274,900**                 |
| **Outstanding Loans / বকেয়া ঋণ** | ৳138,000 − ৳0 = **৳138,000**                                           |
| **Net Profit / নিট লাভ**          | ৳14,000 + ৳1,000 − ৳100 − ৳0 = **৳14,900**                             |
| **Profit Per Member / মাথাপিছু**  | ৳14,900 ÷ 11 = **≈৳1,354.55**                                          |

---

## 4. Transaction Types / লেনদেনের ধরন

### 4.1 All Types / সব ধরন

| Type Key      | Bengali / বাংলা | English       | Icon | Effect on Cash / নগদে প্রভাব                              |
| ------------- | --------------- | ------------- | ---- | --------------------------------------------------------- |
| `deposit`     | জমা             | Deposit       | 💵   | ↑ Increases member's deposited amount / সদস্যের জমা বাড়ে |
| `loan_given`  | ঋণ প্রদান       | Loan Given    | ✈️   | ↓ Cash decreases / নগদ কমে                                |
| `loan_repaid` | ঋণ ফেরত         | Loan Repaid   | 🔄   | ↑ Cash increases / নগদ বাড়ে                              |
| `profit`      | আয়             | Profit/Income | 📈   | ↑ Cash + Equity increase / নগদ ও মূলধন বাড়ে              |
| `fine`        | জরিমানা         | Fine          | ⚖️   | ↑ Cash + Equity increase / নগদ ও মূলধন বাড়ে              |
| `expense`     | খরচ             | Expense       | 💸   | ↓ Cash + Equity decrease / নগদ ও মূলধন কমে                |
| `loss`        | ক্ষতি           | Loss          | 📉   | ↓ Cash + Equity decrease / নগদ ও মূলধন কমে                |

### 4.2 Cash Flow Classification / নগদ প্রবাহ শ্রেণীবিভাগ

```
INFLOWS (আসা / ইনফ্লো):
  ├── Deposits (জমা) — member savings
  ├── Profits (আয়) — interest/returns from loans
  ├── Fines (জরিমানা) — penalty income
  └── Loan Repaid (ঋণ ফেরত) — loan principal returned

OUTFLOWS (যাওয়া / আউটফ্লো):
  ├── Loan Given (ঋণ প্রদান) — money lent out
  ├── Expenses (খরচ) — operational costs
  └── Losses (ক্ষতি) — bad debts or irrecoverable money
```

### 4.3 Deposit vs Other Transactions / জমা বনাম অন্যান্য লেনদেন

**Deposits are handled differently / জমা ভিন্নভাবে পরিচালিত হয়:**

- EN: When you add a deposit, it directly increases the member's `deposited` field AND creates a record in `depositHistory[]`. Other transactions go into `transactions[]`.
- বাং: জমা যোগ করলে তা সরাসরি সদস্যের `deposited` ক্ষেত্র বাড়ায় এবং `depositHistory[]`-তে রেকর্ড তৈরি করে। অন্যান্য লেনদেন `transactions[]`-তে যায়।

```
Deposit Flow:
  1. User selects member + amount + date
  2. member.deposited += amount
  3. Record added to depositHistory[]
  4. NO entry in transactions[]

Other Transaction Flow:
  1. User enters type + description + amount + date
  2. Record added to transactions[]
  3. No member field changes
```

---

## 5. Due System / বকেয়া ব্যবস্থা

### English

The due system uses a **flat monthly installment** model:

- Each member owes **৳8,000 per month** (configurable in settings as `monthlyInstallment`)
- This is a **fixed amount** regardless of how much the member has already deposited
- Total due = Number of members × Monthly installment
- Example: 11 members × ৳8,000 = ৳88,000 total due per month

### বাংলা

বকেয়া ব্যবস্থা একটি **ক্যালেন্ডার মাস** ভিত্তিক মডেল ব্যবহার করে:

- **গ্রেস পিরিয়ড**: মাসের যেকোনো দিন জমা দিলে তা ডিউ হিসেবে ধরা হবে না।
- **বিলম্ব গণনা**: পরবর্তী মাসের ১ তারিখ থেকে বিলম্বের দিন (Late Days) গণনা শুরু হবে।
- **পেনাল্টি ইমপ্যাক্ট**: বিলম্বের কারণে সদস্যের লভ্যাংশ (Profit Share) কমে যাবে, যা তার ফাইনাল আউটপুটে প্রভাব ফেলবে।

```
Due Formula:
  Per member due = monthlyInstallment (default: ৳8,000)
  Total due = members.length × monthlyInstallment
```

---

## 6. Member Management / সদস্য ব্যবস্থাপনা

### 6.1 Member Profile Fields / সদস্য প্রোফাইল ক্ষেত্র

| Field / ক্ষেত্র | Type   | Description (EN)                  | বর্ণনা (বাং)            |
| --------------- | ------ | --------------------------------- | ----------------------- |
| `id`            | Number | Auto-generated unique ID          | স্বয়ংক্রিয় অনন্য আইডি |
| `name`          | String | Member's full name                | সদস্যের পূর্ণ নাম       |
| `deposited`     | Number | Cumulative total deposits         | সঞ্চিত মোট জমা          |
| `phone`         | String | Contact number                    | যোগাযোগ নম্বর           |
| `accountNo`     | String | Fund account number (e.g. FD0002) | ফান্ড একাউন্ট নম্বর     |
| `openingDate`   | String | Date member joined (ISO format)   | যোগদানের তারিখ          |

### 6.2 Adding a Member / সদস্য যোগ করা

**Steps / ধাপসমূহ:**

1. Go to Settings → Member Management / সেটিংস → সদস্য ব্যবস্থাপনা
2. Fill in: Name, Phone, Account No, Joining Date, Initial Deposit / নাম, ফোন, একাউন্ট নং, তারিখ, প্রারম্ভিক জমা পূরণ করুন
3. Click "সদস্য যুক্ত করুন" / "Add Member" ক্লিক করুন
4. If initial deposit > 0, it's automatically recorded in deposit history / প্রারম্ভিক জমা > ০ হলে স্বয়ংক্রিয়ভাবে জমার ইতিহাসে রেকর্ড হয়

### 6.3 Removing a Member / সদস্য অপসারণ

- EN: Removing a member **permanently deletes** their data and all deposit history. This action cannot be undone.
- বাং: সদস্য অপসারণ করলে তার ডেটা ও সকল জমার ইতিহাস **স্থায়ীভাবে মুছে যায়**। এই কাজ পূর্বাবস্থায় ফেরানো যায় না।

---

## 7. Loan System / ঋণ ব্যবস্থা

### 7.1 How Loans Work / ঋণ কিভাবে কাজ করে

```
Loan Lifecycle / ঋণ জীবনচক্র:

  1. LOAN GIVEN (ঋণ প্রদান)
     → Record: type="loan_given", amount=X
     → Effect: Liquid cash ↓ by X
     → Outstanding loans ↑ by X

  2. MONTHLY PROFIT (মাসিক আয়)
     → Record: type="profit", amount=Y, source="borrower"
     → Effect: Liquid cash ↑ by Y, Equity ↑ by Y
     → This is the INTEREST earned from the loan

  3. LOAN REPAID (ঋণ ফেরত)
     → Record: type="loan_repaid", amount=Z
     → Effect: Liquid cash ↑ by Z
     → Outstanding loans ↓ by Z
```

### 7.2 Profit from Loans / ঋণ থেকে লাভ

- EN: Profits are tracked with an optional `source` field to identify which borrower generated the profit. The monthly summary groups profits by month so you can see trends.
- বাং: কোন ঋণগ্রহীতা থেকে লাভ এসেছে তা চিহ্নিত করতে ঐচ্ছিক `source` ক্ষেত্র ব্যবহার করে লাভ ট্র্যাক করা হয়। মাসিক সারসংক্ষেপ মাস অনুযায়ী লাভ গ্রুপ করে প্রবণতা দেখায়।

### 7.3 Example / উদাহরণ

```
Default loan record / ডিফল্ট ঋণের রেকর্ড:

  Loan to "B": ৳138,000 on Jan 5 (ঋণ প্রদান)
  Profit from "B": ৳12,000 on Jan 31 (জানুয়ারির আয়)
  Profit from "B": ৳2,000 on Feb 28 (ফেব্রুয়ারির আয়)

  Outstanding = ৳138,000 (no repayment yet / এখনো ফেরত হয়নি)
  Total profit from this loan = ৳14,000
```

---

## 8. App Views & Navigation / অ্যাপের ভিউ ও নেভিগেশন

### 8.1 Views / ভিউসমূহ

| View / ভিউ        | Nav Tab | Description (EN)                                                    | বর্ণনা (বাং)                                                       |
| ----------------- | ------- | ------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Dashboard**     | হোম     | Overview: stats cards, quick actions, recent transactions, formulas | সারসংক্ষেপ: স্ট্যাট কার্ড, দ্রুত অ্যাকশন, সাম্প্রতিক লেনদেন, সূত্র |
| **Members**       | সদস্য   | Member list with deposits, equity, due status; searchable           | সদস্যদের তালিকা; জমা, পাওনা, বকেয়া; সার্চযোগ্য                    |
| **Member Detail** | —       | Individual member profile, stats, deposit history, loan involvement | ব্যক্তিগত প্রোফাইল, পরিসংখ্যান, জমার ইতিহাস, ঋণ সম্পর্কিত          |
| **Transactions**  | লেনদেন  | All transactions with type filter chips                             | সব লেনদেন; ধরন অনুযায়ী ফিল্টার                                    |
| **Loans**         | ঋণ      | Loan overview: given, repaid, outstanding; loan detail list         | ঋণ সারসংক্ষেপ: প্রদত্ত, ফেরত, বকেয়া; বিস্তারিত তালিকা             |
| **Summary**       | সারাংশ  | Financial summary card + monthly breakdown                          | আর্থিক সারসংক্ষেপ + মাসিক বিবরণ                                    |
| **Settings**      | ⚙️      | Member management, financial config, data export/import             | সদস্য ব্যবস্থাপনা, আর্থিক কনফিগারেশন, ডেটা এক্সপোর্ট/ইম্পোর্ট      |

### 8.2 Month Filter / মাস ফিল্টার

- EN: Use the `◀ Month ▶` arrows at the top to filter data by month. Click the month label to reset to "All Time" (সর্বকালীন). When filtered, dashboard stats and transactions only show that month's data. Total deposits and equity always show cumulative totals.
- বাং: উপরের `◀ মাস ▶` তীর দিয়ে মাস অনুযায়ী ডেটা ফিল্টার করুন। "সর্বকালীন"-এ ফিরে যেতে মাসের নামে ক্লিক করুন। ফিল্টার করলে ড্যাশবোর্ড স্ট্যাট ও লেনদেন শুধু সেই মাসের ডেটা দেখায়। মোট জমা ও মূলধন সবসময় সঞ্চিত মোট দেখায়।

### 8.3 Quick Actions / দ্রুত কার্যক্রম

6 quick action buttons on dashboard / ড্যাশবোর্ডে ৬টি দ্রুত অ্যাকশন বোতাম:

| Button / বোতাম | Opens Modal / মডাল খোলে         |
| -------------- | ------------------------------- |
| জমা            | Deposit modal (member dropdown) |
| ঋণ দান         | Loan given modal (text input)   |
| ঋণ ফেরত        | Loan repaid modal (text input)  |
| খরচ            | Expense modal (text input)      |
| আয়            | Profit modal (text input)       |
| জরিমানা        | Fine modal (text input)         |

---

## 9. Settings / সেটিংস

### 9.1 Financial Settings / আর্থিক সেটিংস

| Setting / সেটিং                    | Default / ডিফল্ট  | Description (EN)                    | বর্ণনা (বাং)                       |
| ---------------------------------- | ----------------- | ----------------------------------- | ---------------------------------- |
| Monthly Installment / মাসিক কিস্তি | ৳8,000            | Amount each member must pay monthly | প্রতি সদস্যের মাসিক প্রদেয় পরিমাণ |
| Fund Name / সমিতির নাম             | সমবায় ফান্ড প্রো | Displayed in header                 | হেডারে প্রদর্শিত                   |

### 9.2 Data Management / ডেটা ব্যবস্থাপনা

| Action / কাজ | Description (EN)                   | বর্ণনা (বাং)                       |
| ------------ | ---------------------------------- | ---------------------------------- |
| Export JSON  | Downloads all data as JSON file    | সব ডেটা JSON ফাইল হিসেবে ডাউনলোড   |
| Import JSON  | Replaces all data from a JSON file | JSON ফাইল থেকে সব ডেটা প্রতিস্থাপন |
| Delete All   | Factory reset to default data      | ডিফল্ট ডেটায় ফ্যাক্টরি রিসেট      |

---

## 10. Developer Reference / ডেভেলপার রেফারেন্স

### 10.1 File Structure / ফাইল কাঠামো

```
somiti/
├── index.html          # Main HTML structure (views, modals, nav)
├── style.css           # Complete CSS (1570+ lines, mobile-first)
├── app.js              # All application logic (1270+ lines)
├── GUIDE.md            # This documentation file
└── Old google sheet data/
    └── সমবায় সমিতি হিসাব - 2026 (1).csv  # Original spreadsheet export
```

### 10.2 Code Architecture / কোড আর্কিটেকচার

```
app.js sections (21 sections):

 1. CONFIGURATION & DEFAULT DATA — Constants, default config/data
 2. STATE MANAGEMENT — loadData(), saveData(), state variables
 3. FORMATTING UTILITIES — Bengali numbers, currency, dates
 4. FINANCIAL CALCULATIONS — calculateFinances() — THE CORE
 5. UI RENDERING — All render functions
 6. SETTINGS RENDERING — loadSettings()
 7. NAVIGATION & TAB SWITCHING — switchTab()
 8. MONTH FILTER — changeMonth(), resetMonthFilter()
 9. TRANSACTION FILTER — filterTransactions()
10. MEMBER SEARCH — filterMembers()
11. MEMBER DETAIL — showMemberDetail()
12. MODAL MANAGEMENT — openModal(), closeModal()
13. FORM SUBMISSION — tx-form submit handler
14. DELETE TRANSACTION — deleteTransaction()
15. MEMBER MANAGEMENT — addMember(), removeMember()
16. SETTINGS MANAGEMENT — saveSettings()
17. DATA EXPORT/IMPORT — exportData(), importData()
18. TOAST NOTIFICATIONS — showToast()
19. DROPDOWN POPULATION — populateMemberDropdown()
20. MASTER REFRESH — refreshUI() — calls all render functions
21. INITIALIZATION — loadData(), refreshUI(), event listeners
```

### 10.3 Key Functions / গুরুত্বপূর্ণ ফাংশন

| Function                         | Purpose                                                                                               |
| -------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `calculateFinances()`            | The main financial engine. Calculates ALL metrics from raw data. Must be called before any render.    |
| `refreshUI()`                    | Master function that calls calculateFinances() then all render functions. Call after any data change. |
| `loadData()`                     | Loads from localStorage with migration support (v2 → v3 → v4).                                        |
| `saveData()`                     | Writes appState to localStorage.                                                                      |
| `getAllTransactionsForDisplay()` | Merges transactions[] + depositHistory[] for display.                                                 |
| `getFilteredTransactions()`      | Returns transactions filtered by currentMonthFilter.                                                  |
| `fmtCurrency(num)`               | Formats number as Bengali currency (e.g., ৳২৬,০০০).                                                   |

### 10.4 Data Migration / ডেটা মাইগ্রেশন

The app supports automatic migration from older versions:

```
v2 (somobayFundDataV2) → v4:
  - Copies members, transactions
  - Adds config with defaults
  - Adds empty depositHistory
  - Adds profile fields (phone, accountNo, openingDate)

v3 (somobayFundDataV3) → v4:
  - Same as above
  - Renames config.expectedDeposit → config.monthlyInstallment
  - Adds profile fields to existing members

v4 (somobayFundDataV4):
  - Current version, loaded directly
  - Auto-migrates missing fields on load
```

### 10.5 CSS Design System / CSS ডিজাইন সিস্টেম

| Variable          | Value   | Usage                   |
| ----------------- | ------- | ----------------------- |
| `--primary`       | #047857 | Main green color        |
| `--primary-light` | #10b981 | Lighter green           |
| `--danger`        | #dc2626 | Error/delete/loss color |
| `--blue`          | #2563eb | Deposit/info color      |
| `--accent`        | #d97706 | Warning/loan color      |
| `--radius-md`     | 12px    | Standard border radius  |
| `--radius-lg`     | 16px    | Large card radius       |

### 10.6 Adding a New Transaction Type / নতুন ট্রানজ্যাকশন টাইপ যোগ করা

If you need to add a new type (e.g., "donation"):

1. **app.js**: Add to `typeTranslations` and `typeIcons` objects
2. **app.js**: Add handling in `calculateFinances()` (increment the right counter)
3. **app.js**: Classify as inflow/outflow in `renderTransactionItem()`
4. **app.js**: Add title in `handleTypeChange()` titles object
5. **index.html**: Add `<option>` in `#tx-type` select
6. **index.html**: Add filter `<button class="chip">` in `#tx-filter-chips`
7. **style.css**: Add color class if needed (e.g., `.tx-donation`)

---

## 📱 User Guide (Ordinary Users) / সাধারণ ব্যবহারকারীর গাইড

### How to Add a Deposit / কিভাবে জমা যোগ করবেন

1. Dashboard-এ "জমা" বোতামে ক্লিক করুন / Click "জমা" button on Dashboard
2. সদস্য নির্বাচন করুন / Select the member
3. টাকার পরিমাণ লিখুন / Enter the amount
4. তারিখ নির্বাচন করুন / Select the date
5. "সেভ করুন" ক্লিক করুন / Click "সেভ করুন"

### How to Record a Loan / কিভাবে ঋণ রেকর্ড করবেন

1. "ঋণ দান" বোতামে ক্লিক করুন / Click "ঋণ দান" button
2. ঋণগ্রহীতার নাম/বিবরণ লিখুন / Enter borrower name/description
3. ঋণের পরিমাণ লিখুন / Enter the loan amount
4. "সেভ করুন" ক্লিক করুন / Click "সেভ করুন"

### How to Record Loan Interest/Profit / কিভাবে ঋণের সুদ/লাভ রেকর্ড করবেন

1. "আয়" বোতামে ক্লিক করুন / Click "আয়" button
2. বিবরণে ঋণগ্রহীতার নাম লিখুন / Write the borrower's name in description
3. আয়ের পরিমাণ লিখুন / Enter the profit amount
4. "সেভ করুন" ক্লিক করুন / Click "সেভ করুন"

### How to Export/Backup Data / কিভাবে ডেটা ব্যাকআপ করবেন

1. ⚙️ সেটিংস-এ যান / Go to ⚙️ Settings
2. "ডেটা এক্সপোর্ট (JSON)" ক্লিক করুন / Click "ডেটা এক্সপোর্ট (JSON)"
3. JSON ফাইল ডাউনলোড হবে / A JSON file will download
4. এটি নিরাপদ স্থানে রাখুন / Keep it in a safe place

### How to Restore Data / কিভাবে ডেটা পুনরুদ্ধার করবেন

1. ⚙️ সেটিংস-এ যান / Go to ⚙️ Settings
2. "ডেটা ইম্পোর্ট" ক্লিক করুন / Click "ডেটা ইম্পোর্ট"
3. আগে এক্সপোর্ট করা JSON ফাইল নির্বাচন করুন / Select previously exported JSON file
4. নিশ্চিত করুন / Confirm

---

_Last Updated: March 8, 2026 / সর্বশেষ আপডেট: ৮ মার্চ, ২০২৬_
_Version: 3.0 (V4 Data Format - Penalty Module Active) / সংস্করণ: ৩.০ (বিলম্ব পেনাল্টি মডিউল যুক্ত)_
