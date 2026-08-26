# Step Functions + JSONata: Automate Complex Workflows Like a Pro

Building modern cloud applications often means orchestrating multiple services, handling complex data transformations, and managing conditional logic across distributed systems. If you've ever tried to coordinate several Lambda functions with custom code managing the flow between them, you know how quickly things can become tangled.

Enter AWS Step Functions and JSONata—a powerful duo that transforms workflow orchestration from a maintenance nightmare into an elegant, visual, and maintainable solution.

In this post, I'll walk you through both technologies using a real-world document processing pipeline. By the end, you'll understand not just *what* these tools do, but *how* they work and *when* to use them.

## The Use Case: Intelligent Document Processing

Let's ground our learning in a practical scenario that many organizations face:

**The Business Need:** Automatically process documents uploaded to S3, classify them by type (invoice, purchase order, contract, etc.), extract relevant data, and trigger appropriate business actions—all without manual intervention.

**The Challenge:** This workflow requires:
- Event-driven triggers
- Multiple processing stages
- Conditional logic (different document types need different handling)
- Data transformation between steps
- Error handling and retry logic
- Visibility into what's happening

This is exactly the type of problem Step Functions and JSONata were designed to solve.

## Understanding AWS Step Functions

### What Are Step Functions?

AWS Step Functions is a serverless orchestration service that lets you coordinate multiple AWS services into serverless workflows. Think of it as a state machine—a visual flowchart where each box represents a state (a step in your process), and arrows show how data flows from one state to the next.

### Core Concepts

**1. States**

A state is a single step in your workflow. Step Functions supports several state types:

- **Task**: Performs work (calls a Lambda function, starts an ECS task, etc.)
- **Choice**: Implements branching logic based on input
- **Parallel**: Runs multiple branches simultaneously
- **Wait**: Delays execution for a specified time
- **Pass**: Passes input to output, optionally transforming data
- **Succeed/Fail**: Terminal states that end execution

**2. State Machine**

The state machine is your entire workflow definition—a JSON document that describes all states and how they connect.

**3. Execution**

Each time your state machine runs, it creates an execution—a single run-through of your workflow with specific input data.

**4. Input/Output Processing**

Each state receives JSON input, processes it, and produces JSON output. You control this flow using:
- `InputPath`: Filters which part of the input to process
- `OutputPath`: Filters which part of the output to pass forward
- `ResultPath`: Where to place the state's result in the original input
- `Parameters`: Constructs the input for the state's task

### How Data Flows Through Step Functions

Here's where it gets interesting. Imagine you have this input:

```json
{
  "document": {
    "bucket": "my-docs",
    "key": "invoice.pdf"
  },
  "metadata": {
    "uploadedBy": "user123"
  }
}
```

In each state, you can:

1. **Select specific data** using InputPath
2. **Transform it** using Parameters
3. **Store results** at a specific location using ResultPath
4. **Filter the final output** using OutputPath

This is powerful, but the transformation capabilities in vanilla Step Functions are limited. That's where JSONata comes in.

## Understanding JSONata

### What Is JSONata?

JSONata is a lightweight query and transformation language for JSON data. If you've used jq, XPath, or SQL, the concept will feel familiar—but JSONata is specifically designed for JSON with an elegant, functional syntax.

### Why JSONata Matters for Step Functions

AWS Step Functions recently added native JSONata support, which is a game-changer. Previously, you had to:
- Write Lambda functions for simple transformations
- Use clunky `States.JsonToString` and string manipulation
- Deploy custom code just to reshape data

Now you can do complex transformations directly in your state machine definition.

### JSONata Basics

Let's learn JSONata through examples:

**Simple Selection:**
```jsonata
document.key
/* Returns: "invoice.pdf" */
```

**Object Construction:**
```jsonata
{
  "filename": document.key,
  "location": document.bucket
}
/* Returns: {"filename": "invoice.pdf", "location": "my-docs"} */
```

**Array Transformation:**
```jsonata
items.{
  "description": description,
  "total": quantity * unit_price
}
/* Transforms each item in the array */
```

**Aggregation:**
```jsonata
$sum(items.total)
/* Sums all totals */
```

**Conditional Logic:**
```jsonata
total > 1000 ? "high-value" : "standard"
/* Returns category based on amount */
```

**Merging Objects:**
```jsonata
$merge([document, metadata, {"status": "processed"}])
/* Combines multiple objects */
```

## Building Our Document Processing Workflow

Now let's bring it all together. Here's how our document processing system works:

### Architecture Overview

```
S3 Upload → Lambda Trigger → Step Functions → Classification → Extraction → Business Action
                                    ↓
                            (JSONata Transformations)
```

### Step 1: The Trigger

When a document arrives in S3, an event notification triggers our Lambda function:

```javascript
exports.handler = async (event) => {
  const bucket = event.Records[0].s3.bucket.name;
  const key = event.Records[0].s3.object.key;

  const stepFunctions = new AWS.StepFunctions();

  // Start the workflow with initial context
  return stepFunctions.startExecution({
    stateMachineArn: process.env.DOCUMENT_WORKFLOW_ARN,
    name: `doc-${Date.now()}`,
    input: JSON.stringify({
      document: {
        bucket: bucket,
        key: key,
        uploadedAt: new Date().toISOString()
      },
      workflowStatus: "STARTED"
    })
  }).promise();
};
```

This Lambda does one thing: kick off the Step Functions workflow with context about the uploaded document.

### Step 2: The State Machine

Here's our workflow definition, which demonstrates key Step Functions concepts:

```json
{
  "Comment": "Document Processing Workflow with JSONata",
  "StartAt": "ClassifyDocument",
  "States": {
    "ClassifyDocument": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:document-classifier",
      "ResultPath": "$.classificationResult",
      "Next": "CheckClassification",
      "Retry": [{
        "ErrorEquals": ["States.TaskFailed"],
        "IntervalSeconds": 2,
        "MaxAttempts": 3,
        "BackoffRate": 2
      }],
      "Catch": [{
        "ErrorEquals": ["States.ALL"],
        "ResultPath": "$.error",
        "Next": "ClassificationFailed"
      }]
    },
    "CheckClassification": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.classificationResult.documentType",
          "IsNull": true,
          "Next": "ClassificationFailed"
        },
        {
          "Variable": "$.classificationResult.confidence",
          "NumericLessThan": 0.7,
          "Next": "ManualReview"
        }
      ],
      "Default": "TransformForExtraction"
    },
    "TransformForExtraction": {
      "Type": "Pass",
      "Parameters": {
        "document.$": "$.document",
        "documentType.$": "$.classificationResult.documentType",
        "confidence.$": "$.classificationResult.confidence",
        "QueryLanguage": "JSONata",
        "Query": "$merge([$.document, {'metadata': {'classified': true, 'type': $.classificationResult.documentType}}])"
      },
      "ResultPath": "$.extractionInput",
      "Next": "ExtractDocumentData"
    },
    "ExtractDocumentData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:data-extractor",
      "InputPath": "$.extractionInput",
      "ResultPath": "$.extractionResult",
      "Next": "ProcessExtractedData"
    },
    "ProcessExtractedData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:business-action",
      "Parameters": {
        "documentType.$": "$.classificationResult.documentType",
        "data.$": "$.extractionResult.extractedData",
        "QueryLanguage": "JSONata",
        "Query": "{'action': documentType = 'INVOICE' ? 'processPayment' : 'createOrder', 'payload': data}"
      },
      "End": true
    },
    "ManualReview": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "arn:aws:sns:us-east-1:123456789012:manual-review",
        "Message.$": "$.document.key"
      },
      "End": true
    },
    "ClassificationFailed": {
      "Type": "Fail",
      "Error": "ClassificationError",
      "Cause": "Unable to classify document"
    }
  }
}
```

### Breaking Down the Workflow

**ClassifyDocument (Task State)**

This Task state calls our classification Lambda. Notice:
- `ResultPath: "$.classificationResult"` — adds the Lambda's output to the input without replacing it
- `Retry` — automatically retries on failures with exponential backoff
- `Catch` — handles errors gracefully by routing to a failure state

**CheckClassification (Choice State)**

This demonstrates conditional branching:
- If `documentType` is null → Classification failed
- If confidence < 0.7 → Send for manual review
- Otherwise → Continue to extraction

**TransformForExtraction (Pass State with JSONata)**

This is where JSONata shines. The `Query` field uses JSONata to merge the document object with new metadata:

```jsonata
$merge([
  $.document,
  {
    'metadata': {
      'classified': true,
      'type': $.classificationResult.documentType
    }
  }
])
```

Without JSONata, you'd need a Lambda function just to add these fields!

**ProcessExtractedData (Task State with JSONata)**

Another JSONata transformation that determines which action to take based on document type:

```jsonata
{
  'action': documentType = 'INVOICE' ? 'processPayment' : 'createOrder',
  'payload': data
}
```

This conditional logic happens inline—no extra code needed.

### Step 3: The Classification Lambda

```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

exports.handler = async (event) => {
  const { bucket, key } = event.document;

  // Retrieve document from S3
  const response = await s3.getObject({ 
    Bucket: bucket, 
    Key: key 
  }).promise();
  
  const documentContent = response.Body.toString('utf-8');

  // In production, use Amazon Textract, Comprehend, or a custom ML model
  // For this example, simple keyword detection
  let documentType = null;
  let confidence = 0;

  if (documentContent.includes('INVOICE') || documentContent.includes('Invoice Number')) {
    documentType = 'INVOICE';
    confidence = 0.95;
  } else if (documentContent.includes('PURCHASE ORDER') || documentContent.includes('PO Number')) {
    documentType = 'PURCHASE_ORDER';
    confidence = 0.87;
  } else if (documentContent.includes('CONTRACT') || documentContent.includes('Agreement')) {
    documentType = 'CONTRACT';
    confidence = 0.80;
  }

  return {
    documentType,
    confidence,
    processedAt: new Date().toISOString()
  };
};
```

### Step 4: The Extraction Lambda with JSONata

```javascript
const jsonata = require('jsonata');
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

exports.handler = async (event) => {
  const { bucket, key } = event.document;
  const documentType = event.documentType;

  // Get document content
  const response = await s3.getObject({ 
    Bucket: bucket, 
    Key: key 
  }).promise();
  
  const documentJson = JSON.parse(response.Body.toString('utf-8'));

  // Define JSONata expressions for different document types
  const extractionTemplates = {
    INVOICE: `{
      "invoiceNumber": invoice_id,
      "totalAmount": total_amount,
      "vendor": {
        "name": vendor.name,
        "id": vendor.vendor_id
      },
      "issueDate": issue_date,
      "dueDate": due_date,
      "lineItems": items.{
        "description": description,
        "quantity": quantity,
        "unitPrice": unit_price,
        "lineTotal": quantity * unit_price
      },
      "calculatedTotal": $sum(items.(quantity * unit_price))
    }`,
    
    PURCHASE_ORDER: `{
      "poNumber": po_number,
      "requestor": requested_by,
      "department": department,
      "requestDate": request_date,
      "items": line_items.{
        "itemId": item_id,
        "description": description,
        "quantity": quantity,
        "estimatedCost": estimated_cost
      },
      "totalEstimate": $sum(line_items.estimated_cost)
    }`,
    
    CONTRACT: `{
      "contractId": contract_id,
      "parties": parties,
      "effectiveDate": effective_date,
      "expirationDate": expiration_date,
      "value": contract_value,
      "terms": terms.{
        "clause": clause_number,
        "description": description
      }
    }`
  };

  // Get the appropriate template
  const template = extractionTemplates[documentType];
  
  if (!template) {
    throw new Error(`No extraction template for document type: ${documentType}`);
  }

  // Execute JSONata transformation
  const expression = jsonata(template);
  const extractedData = await expression.evaluate(documentJson);

  return {
    documentType,
    extractedData,
    extractedAt: new Date().toISOString()
  };
};
```

## The Power of This Approach

### 1. **Separation of Concerns**

- **Orchestration** lives in Step Functions (visual, declarative)
- **Business Logic** lives in Lambda functions
- **Data Transformation** happens inline with JSONata

Each component does one thing well.

### 2. **Reduced Code**

Without JSONata, you'd need Lambda functions for simple transformations. Compare:

**Before (Lambda required):**
```javascript
// Separate Lambda just to add metadata
exports.handler = async (event) => {
  return {
    ...event.document,
    metadata: {
      classified: true,
      type: event.classificationResult.documentType
    }
  };
};
```

**After (inline JSONata):**
```jsonata
$merge([$.document, {'metadata': {'classified': true, 'type': $.classificationResult.documentType}}])
```

### 3. **Visual Debugging**

Step Functions provides a visual execution history. You can see:
- Which state failed
- What input each state received
- What output each state produced
- How long each step took

This is invaluable for debugging production issues.

### 4. **Built-in Reliability**

Step Functions handles:
- Automatic retries with exponential backoff
- Error handling and fallback logic
- State persistence across failures
- Exactly-once execution semantics

### 5. **Scalability**

The entire system scales automatically:
- Lambda functions scale with load
- Step Functions handles concurrent executions
- No servers to manage or provision

## JSONata Pro Tips

### 1. Testing Expressions

Use [JSONata Exerciser](https://try.jsonata.org/) to test expressions before deploying:

```jsonata
/* Test with sample data */
{
  "items": [
    {"name": "Widget", "price": 10, "qty": 5},
    {"name": "Gadget", "price": 20, "qty": 3}
  ]
}

/* Expression */
{
  "total": $sum(items.(price * qty)),
  "itemCount": $count(items)
}

/* Result */
{
  "total": 110,
  "itemCount": 2
}
```

### 2. Reusable Transformations

Define complex expressions as variables:

```jsonata
(
  $calculateTotal := function($items) {
    $sum($items.(quantity * unit_price))
  };
  
  {
    "subtotal": $calculateTotal(line_items),
    "tax": $calculateTotal(line_items) * 0.08,
    "total": $calculateTotal(line_items) * 1.08
  }
)
```

### 3. Handling Missing Data

Use the existence operator:

```jsonata
{
  "hasDiscount": discount ? true : false,
  "finalPrice": price * (1 - (discount ? discount : 0))
}
```

## When to Use This Pattern

This Step Functions + JSONata architecture excels when you have:

✅ **Multi-step workflows** with conditional logic
✅ **Data transformations** between services
✅ **Mixed AWS service integrations** (Lambda, SNS, SQS, DynamoDB, etc.)
✅ **Long-running processes** that need state management
✅ **Workflows that change frequently** (easier to modify JSON than code)
✅ **Need for execution visibility** and audit trails

Consider alternatives when you have:

❌ **Simple single-function tasks** (just use Lambda)
❌ **Real-time, sub-millisecond requirements** (Step Functions adds latency)
❌ **Extremely high-throughput events** (consider EventBridge + Lambda)
❌ **Workflows that need to run for days** (Step Functions has a 1-year max)

## Cost Considerations

Step Functions pricing is based on state transitions:
- Standard Workflows: $0.025 per 1,000 state transitions
- Express Workflows: $1.00 per million requests + duration charges

For our document processing example with 6 states per execution:
- 10,000 documents/month = 60,000 transitions = $1.50
- Plus Lambda execution costs
- Plus S3 storage and data transfer

Significantly cheaper than running and managing dedicated servers!

## Monitoring and Observability

### CloudWatch Integration

Step Functions automatically logs:
- Execution start/end times
- State transitions
- Input/output for each state
- Errors and retries

### X-Ray Tracing

Enable X-Ray tracing to visualize:
- End-to-end execution flow
- Service call latency
- Bottlenecks in your workflow

### Custom Metrics

Emit custom CloudWatch metrics from your Lambda functions:

```javascript
const AWS = require('aws-sdk');
const cloudwatch = new AWS.CloudWatch();

await cloudwatch.putMetricData({
  Namespace: 'DocumentProcessing',
  MetricData: [{
    MetricName: 'DocumentsProcessed',
    Value: 1,
    Unit: 'Count',
    Dimensions: [{
      Name: 'DocumentType',
      Value: documentType
    }]
  }]
}).promise();
```

## Getting Started

Ready to build your own workflow? Here's a quick checklist:

1. **Design your workflow** on paper first
   - Identify states and transitions
   - Mark decision points
   - Plan error handling

2. **Start simple**
   - Build a basic 2-3 state workflow
   - Test with sample data
   - Add complexity incrementally

3. **Use the AWS Console**
   - Workflow Studio provides a visual designer
   - Generates JSON for you
   - Makes testing easier

4. **Leverage JSONata**
   - Use it for data transformation
   - Keep business logic in Lambda
   - Test expressions separately

5. **Implement proper error handling**
   - Add Retry configurations
   - Define Catch blocks
   - Create meaningful failure states

## Conclusion

AWS Step Functions and JSONata together create a powerful platform for building complex workflows without complex code. By separating orchestration from logic and using JSONata for transformations, you get:

- **Visual workflows** that serve as living documentation
- **Less code** to write, test, and maintain
- **Built-in reliability** with retries and error handling
- **Automatic scaling** without infrastructure management
- **Clear visibility** into execution state and history

Whether you're building document processing pipelines, approval workflows, data enrichment processes, or any multi-step automation, this pattern provides a solid foundation that's easy to understand, modify, and operate.

Start with a simple workflow, add JSONata transformations where they make sense, and watch your orchestration code transform from tangled functions into elegant, visual workflows.

---

*What workflows are you building? Share your Step Functions experiences in the comments below!*