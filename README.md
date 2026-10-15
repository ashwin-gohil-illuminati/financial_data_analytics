# Financial Data Pipeline: SQL, Pandas, and Power BI Analysis

**Project Overview**  
This project analyzes Accounts Payable (AP) data to uncover business insights regarding expense distribution, vendor risk, and cash flow timelines. The data pipeline utilizes SQL for relational data extraction, Pandas for data structuring, and Power BI for executive visualizations.

---

## KPI 1: Total Spend by Vendor

**Objective:**  
To identify the highest-billing vendors and visualize the absolute distribution of company expenditure across the vendor base.

**Data Source & Schema:** 
Invoices & Vendors table.
<img width="1006" height="46" alt="Screenshot_2026-10-15_11-04-21" src="https://github.com/user-attachments/assets/36f5f3c0-2940-4cdf-8708-4b9d3fa2c1da" />

<img width="846" height="44" alt="Screenshot_2026-10-15_11-12-02" src="https://github.com/user-attachments/assets/60066629-7a47-44b7-b8e7-06532f4aab37" />
<img width="779" height="43" alt="Screenshot_2026-10-15_11-14-14" src="https://github.com/user-attachments/assets/bc5c45c3-91a2-485a-845e-93f58c3c5235" />

**SQL Extraction Query:**  
```sql
-- [SELECT invoices.vendor_id, vendors.vendor_name, vendors.vendor_state, SUM(invoices.invoice_total) AS Invoices
             FROM invoices, vendors WHERE vendors.vendor_id = invoices.vendor_id
             GROUP BY invoices.vendor_id, vendors.vendor_name, vendors.vendor_state
             ORDER BY Invoices DESC]
```

**Visualization & Insight:**  
<img width="1968" height="1128" alt="Total_Invoices_By_Vendors" src="https://github.com/user-attachments/assets/a3a28b2a-2427-46a7-82a3-e4851818cfcd" />

*The performance of vendors by Invoice amount at a scale from 0 to 120K. Clearly, there are only a couple of vendors contributing at the highest. It also means that the mean of invoices will not be the true depiction. This performance can also be shown as a Pareto represenation.*

---

## KPI 2: Geographic Distribution of Spend

**Objective:**  
To map accounts payable expenses by vendor state, highlighting regional financial dependencies and localized spend concentrations across the vendor network.

**Data Source & Schema:**  
Invoices & Vendors table.
<img width="1006" height="46" alt="Screenshot_2026-10-15_11-04-21" src="https://github.com/user-attachments/assets/36f5f3c0-2940-4cdf-8708-4b9d3fa2c1da" />

<img width="846" height="44" alt="Screenshot_2026-10-15_11-12-02" src="https://github.com/user-attachments/assets/60066629-7a47-44b7-b8e7-06532f4aab37" />
<img width="779" height="43" alt="Screenshot_2026-10-15_11-14-14" src="https://github.com/user-attachments/assets/bc5c45c3-91a2-485a-845e-93f58c3c5235" />

**SQL Extraction Query:**  
```sql
-- [SELECT invoices.vendor_id, vendors.vendor_name, vendors.vendor_state, SUM(invoices.invoice_total) AS Invoices
             FROM invoices, vendors WHERE vendors.vendor_id = invoices.vendor_id
             GROUP BY invoices.vendor_id, vendors.vendor_name, vendors.vendor_state
             ORDER BY Invoices DESC]
```

**Visualization & Insight:**
<img width="1968" height="1128" alt="StatesByInvoices" src="https://github.com/user-attachments/assets/9da338e4-7d49-4fd0-96a4-0618ced0a3df" />
  
*The performance of spend as presented by individual states of USA where some account for large number of spend.*

---

## KPI 3: Pareto Analysis (80/20 Rule) of Vendor Spend

**Objective:**  
To isolate the critical top 20% of vendors that account for roughly 80% of total expenses, allowing the finance team to prioritize strategic cost-saving negotiations and targeted audits.

**Data Source & Schema:**  
Invoices & Vendors table.
<img width="1006" height="46" alt="Screenshot_2026-10-15_11-04-21" src="https://github.com/user-attachments/assets/36f5f3c0-2940-4cdf-8708-4b9d3fa2c1da" />

<img width="846" height="44" alt="Screenshot_2026-10-15_11-12-02" src="https://github.com/user-attachments/assets/60066629-7a47-44b7-b8e7-06532f4aab37" />
<img width="779" height="43" alt="Screenshot_2026-10-15_11-14-14" src="https://github.com/user-attachments/assets/bc5c45c3-91a2-485a-845e-93f58c3c5235" />

**SQL Extraction Query:**  
```sql
-- [SELECT invoices.vendor_id, vendors.vendor_name, vendors.vendor_state, SUM(invoices.invoice_total) AS Invoices
             FROM invoices, vendors WHERE vendors.vendor_id = invoices.vendor_id
             GROUP BY invoices.vendor_id, vendors.vendor_name, vendors.vendor_state
             ORDER BY Invoices DESC]
```

**Visualization & Insight:** 
<img width="1968" height="1128" alt="Pareto Graph" src="https://github.com/user-attachments/assets/cab4cdc9-07a8-46a1-8638-69b875b47b3b" />

*The performance shows the 80-20 rule from Pareto principle and shows that merely 2-3 vendors are contributing to 80 percent of the entire invoices amount.*

---

## KPI 4: Accounts Payable Aging Report (Cash Flow Risk)

**Objective:**  
To categorize outstanding and late invoices into standard 30-day time buckets (Current, 1-30, 31-60+ Days). This assesses immediate cash flow requirements and mitigates the risk of late fees or damaged vendor relationships.

**Data Source & Schema:**  
Invoices & Vendors table.
<img width="1006" height="46" alt="Screenshot_2026-10-15_11-04-21" src="https://github.com/user-attachments/assets/36f5f3c0-2940-4cdf-8708-4b9d3fa2c1da" />

<img width="846" height="44" alt="Screenshot_2026-10-15_11-12-02" src="https://github.com/user-attachments/assets/60066629-7a47-44b7-b8e7-06532f4aab37" />
<img width="779" height="43" alt="Screenshot_2026-10-15_11-14-14" src="https://github.com/user-attachments/assets/bc5c45c3-91a2-485a-845e-93f58c3c5235" />

**SQL Extraction Query:**  
```sql
-- [SELECT 
    invoice_number, 
    invoice_due_date, 
    payment_date,
    DATEDIFF(payment_date, invoice_due_date) AS days_past_due,
    
    CASE 
        
        WHEN DATEDIFF(payment_date, invoice_due_date) <= 0 THEN 'On Time'        
        
        WHEN DATEDIFF(payment_date, invoice_due_date) BETWEEN 1 AND 30 THEN '1-30 Days Late'        
        
        WHEN DATEDIFF(payment_date, invoice_due_date) BETWEEN 31 AND 60 THEN '31-60 Days Late'        
        
        WHEN DATEDIFF(payment_date, invoice_due_date) > 60 THEN 'Over 60 Days Late'        
        
        ELSE 'Unpaid' 
    END AS aging_bucket,
    invoice_total,
    vendors.vendor_name    
FROM invoices, vendors
WHERE invoices.vendor_id = vendors.vendor_id]
```

**Visualization & Insight:** 
<img width="1968" height="1128" alt="InvoicesByDueTime" src="https://github.com/user-attachments/assets/04a8ad2e-185c-437f-984b-83ece6a1e9c0" />

*The performance of payment of invoices done for the due dates. Seen as intervals of days like "Being On Time', 'Unpaid' and '1-30 Days Late'. The payments done on time fall averagely while almost same percentage of payments being unpaid and in the '1-30 Days late' bracket.*

---

## KPI 5: Spend by General Ledger Category (Expense Tracking)

**Objective:**  
To break down total spend by specific operational categories (e.g., Freight, Advertising, Software) using proportional visualization. This instantly highlights functional cost distributions and potential budget overruns.

**Data Source & Schema:** 
Invoice Line Items, General ledger accounts, Invoices.
<img width="627" height="43" alt="Screenshot_2026-10-15_12-03-35" src="https://github.com/user-attachments/assets/4181c193-dcf8-4a18-a2e0-b965e1e41e85" />

<img width="325" height="41" alt="Screenshot_2026-10-15_12-06-31" src="https://github.com/user-attachments/assets/d2869c88-c20b-481f-aa01-c3b4028cd761" />

<img width="1006" height="46" alt="Screenshot_2026-10-15_11-04-21" src="https://github.com/user-attachments/assets/36f5f3c0-2940-4cdf-8708-4b9d3fa2c1da" />

**SQL Extraction Query:**  
```sql
-- [SELECT invoice_line_items.account_number, YEAR(invoices.invoice_date) AS Year, MONTH(invoices.invoice_date) AS Month,
                                    SUM(invoice_line_items.line_item_amount) AS Total_Amount, general_ledger_accounts.account_description
                                   FROM invoices INNER JOIN invoice_line_items
                                    ON invoices.invoice_id = invoice_line_items.invoice_id 
                                    LEFT OUTER JOIN general_ledger_accounts
                                    ON invoice_line_items.account_number = general_ledger_accounts.account_number
                                    GROUP BY invoice_line_items.account_number, Year, Month, general_ledger_accounts.account_description]
```

**Visualization & Insight:**
<img width="1968" height="1128" alt="TotalSpend-Slicer" src="https://github.com/user-attachments/assets/3bbea626-1572-4304-8e25-f05bf854e27c" />
 
*By selecting the month, a user can see the distribution of spend amounts categorized as Accounts (names of the accounts).*
