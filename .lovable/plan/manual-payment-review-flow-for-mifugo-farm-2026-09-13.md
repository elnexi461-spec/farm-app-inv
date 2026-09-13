# Manual payment review flow for Mifugo Farm

## Goal
Import the existing app from GitHub into this project, remove the automatic M-Pesa (Daraja) integration, and replace it with a manual payment flow that matches your screenshots: customer sends money to the company number, submits their transaction ID, and an admin approves or rejects each deposit.

## Steps

### 1. Import the existing app
- Copy the repo code (pages, components, styles, database migrations) into this project so nothing already built is lost.
- Enable Lovable Cloud and apply the existing database migrations (profiles, roles, packages, investments, transactions, etc.).

### 2. Remove automatic M-Pesa
- Delete the Daraja/STK-push server code and the M-Pesa callback endpoint.
- Remove M-Pesa credential requirements — no real credentials needed anymore.

### 3. New deposit flow (matches your screenshots)
- **Recharge page**: Recharge/Withdraw tabs, deposit balance display, quick amounts (1,200 / 2,500 / 5,000 / 10,000 / 20,000 / 50,000), amount + phone inputs, minimum deposit note. Proceed button.
- **Deposit Details page**: 30-minute countdown timer, Amount to Pay, Number Sending, Company Account Number **0700000000** (demo, with Copy button), Company Account Name, and an "I Have Paid" button.
- **Confirm Payment page**: shows Amount Paid, fields for Transaction ID / confirmation message and the phone number used to pay, "Paid" submit button.
- After submitting, the deposit shows as **Pending review** in the customer's wallet history until an admin acts.

### 4. Admin review dashboard
- New admin section listing pending deposits with amount, phone number, and transaction ID.
- **Approve** credits the user's wallet balance; **Reject** marks it rejected. Both recorded on the transaction.
- Admin access controlled by the existing roles system (server-side checked).

### 5. Layout and bug fixes
- Fix layout issues so every page works well on both mobile and desktop.
- Run a build check and fix any errors.

## Technical details
- Database: extend the existing `transactions` table with `transaction_ref`, `payer_phone`, and review status (`pending` / `approved` / `rejected`) plus reviewer info; row-level security so users see only their own deposits and only admins can approve.
- Payments are demo-only: no real money moves; approving a deposit just updates the balance in the database.
- Server functions via TanStack Start `createServerFn` with `requireSupabaseAuth`; admin actions verify the admin role server-side.
- Credit-conscious: single import pass, one migration, minimal iterations.
