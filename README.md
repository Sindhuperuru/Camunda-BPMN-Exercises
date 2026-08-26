# Camunda BPMN & DMN Exercises

This repository contains BPMN process models and DMN decision tables created using Camunda Modeler and tested using Camunda 8.

## Exercise 1: Loan Application Risk Routing

### Objective
To model an end-to-end loan approval workflow where a DMN decision evaluates the loan risk and an Exclusive Gateway routes the application based on the risk level.

### Input Variables
- `applicantAge`
- `creditScore`
- `loanAmount`

### DMN Decision
**Decision:** `evaluate-loan-risk`

### DMN Outputs
- `riskTier`
- `requiresManualReview`

### Risk Rules
- Credit score >= 750 and loan amount <= 50000 → LOW, no manual review
- Credit score >= 750 and loan amount > 50000 → MEDIUM, manual review
- Credit score 600–749 → MEDIUM, manual review
- Credit score < 600 → HIGH, no manual review

### BPMN Flow
Start → Submit Loan Application → Evaluate Loan Risk → Exclusive Gateway

The gateway routes the application to:
- Auto-Approve and Disburse
- Underwriter Review
- Auto-Reject Notification

---

## Exercise 2: Multi-Item Order Discount & Fulfillment Orchestration

### Objective
To calculate applicable discounts using a multi-hit DMN decision table and route the order based on the final invoice amount.

### Input Variables
- `customerTier`
- `cartValue`
- `promoCode`

### DMN Decision
**Decision:** `calculate-discounts`

### Hit Policy
**Collect Sum (C+)**

### Output
- `discountPercent`

### Discount Rules
- PREMIUM customer → 10% discount
- Cart value >= 500 → 5% discount
- FESTIVE10 or SPECIAL10 promo code → 10% discount
- Default → 0%

### BPMN Flow
Start → Calculate Total Discount → Calculate Final Amount → Exclusive Gateway

If final amount >= 1000:
- Manager Sign-off → Send Order to Warehouse

If final amount < 1000:
- Send Order to Warehouse

Finally:
- Send Order to Warehouse → End

### Test Case
```text
customerTier = PREMIUM
cartValue = 600
promoCode = FESTIVE10
