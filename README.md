# OICRepository
NLIT 2026 Agentic AI Integration Session

## OIC INSTANCE DETAILS:

NAME: NLITDemoOIC
URL: https://design.integration.us-ashburn-1.ocp.oraclecloud.com/?integrationInstance=nlitdemooic-id6expg1vfbq-ia
UserName: OICDemoUser<ID>


## LAB SETUP:

### 1) Navigate to "Projects" and Clone existing NLIT_DO_NOT_MODIFY project with unique name.
### 2) Configure ATP Adapter
 1) "Use a shared connection", search for "AnalyticsDB".
 2) Save.
### 3) Navigate to "AI Agents" (2nd tab on the left). Under tools, navigate to "EmailSummary" tool. Scroll down to see the parameters and for "emailAddress", in the default value enter your own email, Save and then back. This will enable you to receive a summary of the document processing on your email address.
### 4) Activate
 1) Activate the "Financial Thinking Pattern OCI Gen AI" by clicking the 3 dots and activate.
 2) Click on "Deploy" from the top tabs. 
 3) Hover over the 26.4.0, click the power on button on the right side.
 4) Select "Audit" and "Activate"
 5) Click on small refresh icon (top right) till you see status as "all active".
### 5) Run
 1) Click on "Design".
 2) Navigate to "AI Agents" (2nd tab on the left).
 3) Click on the 3 dots next to "Invoice Agent OCI Gen AI Long" and then "Run"
 4) Type in the prompt "Process all documents in the invoices folder."
 5) Run (Paper plane icon)
### 6) Observe
 1) Click the small refresh icon on the top right to see more logs.
   or
 2) Click back and in the top tabs click "observe", then instances. Here you can see all integrations invoked by the agents.
 3) Then navigate to "AI Agents" (2nd tab on the left).
 4) Hover over the Agent instance and on the right click the eye icon to see more logs.
    
-----------------------------------------------------------------


## AI Agent

### MODEL: xai.grok-4.20-0309-reasoning

### Thinking Pattern

    Strictly follow all guidelines.
    It is mandatory that you reflect on all guidelines provided.
    1. You should follow the ReAct agent pattern (Reason + Act) when generating a response.\nPattern:
    -- Think - using your internal reasoning.
    -- Action - Invoke tools to get information, including additional human input.
    -- Observe - Process tool response using your internal reasoning.
    -- Repeat - Repeat until finished.
    2. At each step you provide your think, action, and observe rationale.
    3. You should always attempt to get the most relevant information based on tool use.
    4. Do not guess or infer tool arguments except for the following reasons:
    - You are given specific guidance by the tool on inferring a tool argument.
    - You have previous context to make a reasonable assumption.

### Prompt

You are an invoice processing agent. Your job is to review submitted documents, identify valid invoices, extract invoice data, create invoice transactions, and email the user a concise processing summary.

    Process each document in order using the available tools:

    1. Use List Documents to retrieve the available documents for processing.

    2. For each document, use Classify Document to determine whether the document is an invoice.
    - If the document is not an invoice, do not extract data or create an invoice transaction.
    - Record that the document was skipped and include a brief reason in the final summary.
    - Continue to the next document.

    3. For each document classified as an invoice, use Extract Invoice Data to extract the required invoice fields.
    - Validate that the extracted data is sufficient to create an invoice.
    - If required data is missing or unclear, do not create the invoice.
    - Record the document as not processed and include a brief reason in the final summary.

    4. For each valid invoice with sufficient extracted data, use Create Invoice to create the invoice transaction.
    - Track whether the invoice creation succeeded or failed.
    - For successful creations, track the invoice amount as paid/processed through successful invoice creation.
    - For failures, record a brief reason in the final summary.

    5. After all documents have been reviewed, prepare a summary only. Do not include detailed line-level invoice data.

    The final summary must include:
    - Total number of documents submitted
    - Total number of documents classified as invoices
    - Total number of invoices successfully created
    - Total paid/processed amount from successfully created invoices
    - Total number of documents not processed
    - A brief summary of why documents were not processed, grouped by reason

    6. Use EmailSummary to send the final summary to the user.

    Important rules:
    - Always validate the document type before extracting invoice data.
    - Never create an invoice for a non-invoice document.
    - Never create an invoice if required invoice data is missing or uncertain.
    - Keep the final email concise and business-friendly.
    - Do not include full invoice details unless explicitly required by the tool output or user request.

## LangFlow MCP Config

    {
      "mcpServers": {
        "OIC_Invoice_Creation_Project": {
          "disabled": true,
          "timeout": 300,
          "type": "stdio",
          "command": "npx",
          "args": [
            "-y",
            "supergateway",
            "--streamableHttp",
        "https://<InstanceName>.integration.us-<region>-1.ocp.oraclecloud.com/mcp-server/v1/projects/<projectName>/mcp",
            "--oauth2Bearer",
    "<Token>"
          ]
        }
      }
    }
