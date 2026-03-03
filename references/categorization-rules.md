# Transaction Categorization Rules

Use these rules as a starting point when categorizing raw transaction data. Adapt based on the user's specific business and chart of accounts.

---

## Categorization Strategy

### Step 1: Pattern Match on Vendor/Description
Use the vendor mapping tables below as a first pass. Match on partial strings (vendor names often appear truncated or with extra characters in bank feeds).

### Step 2: Amount-Based Rules
- Recurring identical amounts on the same day each month → Likely subscription or lease payment
- Round numbers ($500, $1,000) → May be transfers, owner draws, or loan payments (investigate)
- Very large one-off amounts → Flag for review (could be asset purchase, loan, or error)

### Step 3: Flag What You Can't Classify
Never guess on ambiguous transactions. Put them in a "Flagged for Review" tab with your best-guess category and a note explaining the ambiguity.

---

## Common Vendor → Account Mappings

### Technology & Software (6100 - Software & Subscriptions)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| AWS, Amazon Web Services | Hosting / Infrastructure | Could also be COGS for tech companies |
| Google Cloud, GCP | Hosting / Infrastructure | |
| Microsoft Azure | Hosting / Infrastructure | |
| Heroku, DigitalOcean, Vercel, Netlify | Hosting / Infrastructure | |
| Slack, Zoom, Microsoft 365 | Software & Subscriptions | |
| Salesforce, HubSpot | Software & Subscriptions | May be S&M if CRM |
| GitHub, Atlassian, Jira | Software & Subscriptions | R&D if engineering tools |
| Figma, Adobe, Canva | Software & Subscriptions | |
| QuickBooks, Xero, FreshBooks | Software & Subscriptions | G&A |
| Datadog, PagerDuty, New Relic | Software & Subscriptions | R&D / Infrastructure |
| OpenAI, Anthropic | Software & Subscriptions | Could be COGS if AI is in the product |

### Payroll & Benefits (6200 - Payroll Expenses)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| ADP, Gusto, Rippling, Paychex | Payroll Processing | |
| Justworks, TriNet, Deel | PEO / Payroll | |
| United Health, Aetna, Blue Cross, Kaiser | Health Insurance | Benefits expense |
| Principal, Fidelity, Vanguard 401k | Retirement Benefits | |
| IRS, EFTPS, State Tax | Payroll Tax Payments | |

### Office & Facilities (6300 - Rent & Office)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| WeWork, Regus, Industrious | Rent / Office Space | |
| [Landlord names] | Rent | |
| Staples, Office Depot | Office Supplies | |
| FedEx, UPS, USPS | Shipping / Postage | Could be COGS if product-related |
| Comcast, AT&T, Verizon | Internet / Telecom | Office = G&A, remote workers = varies |

### Professional Services (6400 - Professional Fees)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| [Law firm names] | Legal Fees | |
| [CPA firm names] | Accounting Fees | |
| Deloitte, EY, KPMG, PwC | Audit / Consulting Fees | Distinguish audit vs. advisory |
| Carta, Pulley | Cap Table / Equity Admin | |

### Sales & Marketing (6500 - S&M)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| Google Ads, Facebook Ads, Meta | Digital Advertising | |
| LinkedIn, Twitter/X Ads | Digital Advertising | |
| Mailchimp, SendGrid, Brevo | Email Marketing | |
| Intercom, Drift | Customer Engagement | |
| SEMrush, Ahrefs, Moz | Marketing Tools | |
| Eventbrite | Events / Conferences | |

### Travel & Entertainment (6600 - T&E)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| United, Delta, American, Southwest | Airfare | |
| Uber, Lyft | Ground Transportation | |
| Marriott, Hilton, Airbnb, Hotels.com | Lodging | |
| Restaurant names | Meals & Entertainment | Check if business purpose documented |
| Expensify, Navan, Brex Travel | T&E / Expense Platform | |

### Banking & Finance (7000 - Other Expense)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| Stripe, Square, PayPal | Payment Processing Fees | COGS if tied to revenue transactions |
| Mercury, SVB, Chase | Bank Fees | Or interest income/expense |
| Brex, Ramp, Corporate Card | Credit Card Fees / Payments | Categorize underlying charges, not the payment |
| Wire Transfer Fee | Bank Fees | |

### Insurance (6700 - Insurance)
| Vendor Pattern | Category | Notes |
|---------------|----------|-------|
| Hartford, Travelers, Hiscox | General / Liability Insurance | |
| Embroker, Vouch | Startup Insurance | |
| State Fund, Workers Comp | Workers Compensation | |

---

## Categorization by Transaction Type

### Transfers (Not Expenses!)
Transfers between the company's own accounts are NOT income or expenses:
```
Dr. [Destination Bank Account]
    Cr. [Source Bank Account]
```
**How to identify:** Same amount appears as debit in one account and credit in another on the same or adjacent dates. Common patterns: "Transfer to/from", "ACH Transfer", round amounts between accounts.

### Owner Transactions (Equity, Not Expense)
- Owner deposits → Capital Contribution (Equity)
- Owner withdrawals → Owner's Draw (Equity)
- Personal expenses paid from business account → Owner's Draw (flag for user)

### Loan Transactions
- Loan proceeds → Not revenue (Liability credit)
- Loan payments → Split: principal (reduces liability) and interest (expense)
- Line of credit draws/repayments → Liability, not revenue/expense

### Tax Payments
- Federal/state income tax → Income Tax Expense (or owner's draw if pass-through entity and personal taxes paid from business account — flag this)
- Payroll taxes → Payroll Tax Expense
- Sales tax remittance → Reduces Sales Tax Payable (not an expense — you collected it from customers)

---

## Red Flags to Highlight

Always flag these for user review:
- **Duplicate transactions:** Same amount, same vendor, same date (or within 1 day)
- **Round number payments** without clear vendor/purpose
- **Large one-off transactions** > 3× the monthly average for that category
- **Transactions to individuals** (possible contractor payments needing 1099 tracking)
- **Cash withdrawals** (ATM, cash back) — need documentation
- **Payments to the owner or related parties** — must be properly classified
- **Foreign currency transactions** — may need FX adjustment
- **Negative amounts in unexpected places** — could be refunds, reversals, or errors

---

## When You're Unsure

If a transaction doesn't match any pattern:

1. Check if the description contains any industry-specific terms
2. Look at the amount — does it match any recurring pattern?
3. Check the date — is it near payroll dates, month-end, or quarter-end?
4. If still unsure, categorize as **"Uncategorized Expense"** (6999) and flag for review

It's better to flag 20 transactions for review than to miscategorize 5.
