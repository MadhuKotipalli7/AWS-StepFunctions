# Serverless Order Processing Workflow (AWS Step Functions + Lambda + SNS)

A production-ready, fault-tolerant, and event-driven **serverless order processing workflow** leveraging **AWS Step Functions** for orchestration, **AWS Lambda** for compute, and **Amazon SNS** for real-time notifications.

This workflow simulates a complete e-commerce order lifecycle: receive order → check inventory → charge payment → send confirmation → publish notification.

---

## Architecture Diagram

![Order Processing Workflow](flow.png)

---

## Key Features

* **Event-driven orchestration** with AWS Step Functions.
* **Pay-per-use** compute with AWS Lambda.
* **Built-in error handling** using `Catch` and `Default` paths.
* **Real-time notifications** via Amazon SNS.
* **Loose coupling** between workflow steps for maintainability.
* **Observability** through Amazon CloudWatch Logs.

---

## Workflow Steps

1. **ReceiveOrder (Lambda)** – Validates incoming order request and starts the process.
2. **CheckInventory (Lambda)** – Verifies if the requested product is available.

   * On failure → `InventoryFailed`.
3. **InventoryConfirmation (Choice)** – Proceeds to payment if available, else fails.
4. **ChargePayment (Lambda)** – Processes the customer payment.

   * On failure → `PaymentFailed`.
5. **PaymentConfirmation (Choice)** – Sends confirmation if charged, else fails.
6. **SendConfirmation (Lambda)** – Sends order confirmation to the customer.
7. **SNS Publish** – Publishes order completion notification.

---

## Sample Input

```json
{
  "order_id": "12345",
  "product_id": "sku-001",
  "quantity": 1,
  "customer_email": "user@example.com"
}
```

---

## Deployment Steps

1. Deploy Lambda functions (`ReceiveOrder`, `CheckInventory`, `ChargePayment`, `SendConfirmation`).
2. Create SNS topic for notifications.
3. Define Step Functions state machine using the provided ASL definition.
4. Assign least privilege IAM roles for Lambdas and Step Functions.
5. Test using sample payload.

---

MIT
