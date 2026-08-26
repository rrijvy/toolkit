# Step Functions + JSONata: Automate Complex Workflows Like a Pro

In today's cloud-native world, processing data efficiently through multiple stages requires powerful orchestration. AWS Step Functions combined with JSONata's transformation capabilities creates a robust solution for document processing pipelines. Let me walk you through a practical example that demonstrates this powerful combination.

## The Challenge: Automated Document Processing

Imagine this scenario: Your system needs to process documents uploaded to S3, classify them by type, extract relevant information, and then perform actions based on the content. This workflow involves multiple services and decision points—perfect for Step Functions and JSONata.

## The Solution: Step Functions Orchestration with JSONata Transformations

Here's our simplified workflow:

1. A document is uploaded to an S3 bucket
2. This triggers a Lambda function
3. The Lambda initiates a Step Function workflow
4. Step Function classifies the document
5. If classification succeeds, it extracts data
6. Based on extracted data, it performs a business action
7. Results are stored and notifications sent

Let's see how this works in practice.

## Implementing the Workflow

### 1. S3 Upload and Lambda Trigger

When a document lands in our S3 bucket, an event triggers our processor Lambda:

```javascript
// Lambda function triggered by S3 event
exports.handler = async (event) => {
  const bucket = event.Records[0].s3.bucket.name;
  const key = event.Records[0].s3.object.key;

  console.log(`New document uploaded: ${key}`);

  // Start the document processing workflow
  const stepFunctions = new AWS.StepFunctions();

  const params = {
    stateMachineArn: process.env.DOCUMENT_WORKFLOW_ARN,
    name: `doc-${Date.now()}`,
    input: JSON.stringify({
      document: {
        bucket: bucket,
        key: key,
      },
      workflowStatus: "STARTED",
    }),
  };

  // Trigger the step function
  return stepFunctions.startExecution(params).promise();
};
```

### 2. Step Function Definition with JSONata

Our Step Function uses JSONata for data transformations throughout the workflow:

```json
{
  "Comment": "Document Processing Workflow",
  "StartAt": "ClassifyDocument",
  "States": {
    "ClassifyDocument": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:document-classifier",
      "Next": "CheckClassification",
      "ResultPath": "$.classificationResult"
    },
    "CheckClassification": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.classificationResult.documentType",
          "IsNull": true,
          "Next": "WorkflowEnd"
        }
      ],
      "Default": "ExtractDocumentData"
    },
    "ExtractDocumentData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:data-extractor",
      "Parameters": {
        "document.$": "$.document",
        "documentType.$": "$.classificationResult.documentType",
        "QueryLanguage": "JSONata",
        "Query": "$merge([$, {'confidence': $.classificationResult.confidence}])"
      },
      "ResultPath": "$.extractionResult",
      "Next": "ProcessExtractedData"
    },
    "ProcessExtractedData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:business-action",
      "End": true
    },
    "WorkflowEnd": {
      "Type": "Pass",
      "End": true
    }
  }
}
```

Notice the use of JSONata in the `ExtractDocumentData` state. This allows us to transform data directly within the workflow definition.

### 3. Document Classification Lambda

Our classification Lambda might look something like this:

```javascript
exports.handler = async (event) => {
  const { bucket, key } = event.document;

  // Get document from S3
  const s3 = new AWS.S3();
  const response = await s3.getObject({ Bucket: bucket, Key: key }).promise();
  const documentContent = response.Body.toString("utf-8");

  // Use a service like Amazon Comprehend or a custom model to classify
  let documentType = null;
  let confidence = 0;

  try {
    // Classification logic here
    // For this example, we'll do a simple check
    if (documentContent.includes("INVOICE")) {
      documentType = "INVOICE";
      confidence = 0.95;
    } else if (documentContent.includes("ORDER")) {
      documentType = "PURCHASE_ORDER";
      confidence = 0.87;
    }
  } catch (error) {
    console.error("Classification error:", error);
  }

  return {
    documentType,
    confidence,
  };
};
```

### 4. Data Extraction with JSONata

The extraction Lambda utilizes JSONata's power to transform document content:

```javascript
const jsonata = require("jsonata");

exports.handler = async (event) => {
  const { bucket, key } = event.document;
  const documentType = event.documentType;

  // Get document from S3
  const s3 = new AWS.S3();
  const response = await s3.getObject({ Bucket: bucket, Key: key }).promise();
  const documentContent = response.Body.toString("utf-8");

  // Parse the document content (assuming JSON for this example)
  const documentJson = JSON.parse(documentContent);

  // Use JSONata to extract data based on document type
  let extractionExpression;

  if (documentType === "INVOICE") {
    extractionExpression = jsonata(`{
      "invoiceNumber": invoice_id,
      "amount": total_amount,
      "vendor": vendor.name,
      "date": issue_date,
      "lineItems": items.{
        "description": description,
        "quantity": quantity,
        "unitPrice": unit_price,
        "total": quantity * unit_price
      }
    }`);
  } else if (documentType === "PURCHASE_ORDER") {
    extractionExpression = jsonata(`{
      "poNumber": po_number,
      "requestor": requested_by,
      "items": line_items.{
        "itemId": item_id,
        "description": description,
        "quantity": quantity
      }
    }`);
  }

  const extractedData = extractionExpression.evaluate(documentJson);

  return {
    documentType,
    extractedData,
  };
};
```

## Why This Architecture Works So Well

This combination of Step Functions and JSONata provides several key benefits:

1. **Visual Workflow Management**: Step Functions offers a visual representation of your document processing workflow, making it easier to understand and debug.

2. **Powerful Data Transformations**: JSONata allows complex data transformations without writing custom code for each scenario.

3. **Conditional Logic**: As shown in the `CheckClassification` state, we can easily implement decision points in our workflow.

4. **Error Handling**: Step Functions provides robust error handling and retry capabilities (not fully illustrated in this simplified example).

5. **Serverless Scalability**: The entire workflow scales automatically with demand without managing any infrastructure.

## When to Use This Pattern

This architecture is particularly effective for:

- Document processing pipelines
- Multi-stage approval workflows
- Data enrichment processes
- Systems requiring complex decision trees
- Scenarios where workflow visualization is important

## Conclusion

The combination of AWS Step Functions and JSONata creates a powerful platform for building maintainable, observable document processing workflows. By separating orchestration (Step Functions) from transformation logic (JSONata), we get a clean architecture that's easier to maintain and extend over time.

For your next document processing challenge, consider this approach. You'll benefit from better visualization, more maintainable code, and the ability to adapt your workflow as requirements evolve—all without rebuilding your infrastructure.

---

_Have you implemented Step Functions in your projects? What challenges did you face? Share your experiences in the comments below!_
