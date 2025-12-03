#Sample Schemas & Example Submissions (Edge Case Focused)

#this section provides sample schemas, sample data submissions, and explicit edge cases that validate the system’s correctness, stability, validation logic, rule interactions, concurrency behavior, and workflow state transitions.

ex-1
{
  "schema_id": "CustomerRecord",
  "version": 1,
  "fields": {
    "customer_id": { "type": "string", "required": true },
    "name": { "type": "string", "required": true, "min_length": 2 },
    "email": { "type": "string", "format": "email", "required": true },
    "age": { "type": "integer", "min": 0 },
    "country": { "type": "string", "default": "Unknown" },
    "created_at": { "type": "datetime", "computed": "now()" },
    "risk_score": { "type": "float", "computed": "age > 60 ? 0.8 : 0.2" }
  },
  "compatibility": "backward"
}
ex-2
{
  "schema_id": "InvoiceRecord",
  "version": 2,
  "fields": {
    "invoice_id": { "type": "string", "required": true },
    "customer_id": { "type": "string", "required": true },
    "items": {
      "type": "array",
      "items": {
        "description": "string",
        "price": "float",
        "quantity": "integer"
      },
      "min_items": 1
    },
    "total_amount": { "type": "float", "computed": "sum(items.price * items.quantity)" },
    "currency": { "type": "string", "allowed": ["USD", "EUR", "INR"], "default": "USD" },
    "status": { "type": "string", "allowed": ["draft", "submitted", "paid"] }
  },
  "compatibility": "forward"
}
ex-3 valid sumission
{
  "schema": "CustomerRecord.v1",
  "payload": {
    "customer_id": "CUST-00122",
    "name": "John Doe",
    "email": "john@example.com",
    "age": 67
  }
}
Example Submission (Invalid — Multiple Edge Cases)
{
  "schema": "InvoiceRecord.v2",
  "payload": {
    "invoice_id": "",
    "customer_id": "CUST-00122",
    "items": [], 
    "currency": "YEN"
  }
}
/////////////////////////////////////////////////////////////////////////
Workflow Edge Case
Submission intentionally causes a workflow partial failure:

{
  "schema": "InvoiceRecord.v2",
  "payload": {
    "invoice_id": "INV-00102",
    "customer_id": "CUST-09",
    "items": [
      {"description": "X", "price": -10, "quantity": 2}
    ]
  }
}
