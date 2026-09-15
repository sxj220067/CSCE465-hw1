# Task 4 — Threat Model

## Assets
1. TAMUS API key
2. Local files inside the Ubuntu VM
3. Agent context and prompts
4. Tool execution capability

## Principals
1. User
2. OpenClaw agent/gateway
3. TAMUS model provider

## Trust Boundaries
1. User → OpenClaw Gateway
2. OpenClaw Gateway → TAMUS model provider
3. Local webpage → Agent context
4. Agent/tool decision → Shell or file operation

## Threats and Controls

### Threat 1 — Indirect Prompt Injection
Untrusted webpage content could contain instructions that the model mistakes for trusted commands.

Control:
Clearly separate external data from user instructions and enforce tool authorization independently of model output.

### Threat 2 — Unauthorized Shell Execution
The model could propose an unsafe or unintended command.

Control:
Use OpenClaw's exec-policy and restrict the safe-marker skill to one fixed command.

### Threat 3 — API Key Exposure
The TAMUS API key could accidentally be stored in configuration files or Git.

Control:
Keep the API key only in the shim environment and never commit it to the repository.

### Threat 4 — Malicious File Modification
An agent could overwrite or modify files outside the intended lab directory.

Control:
Restrict tools and skills to specific paths and commands.

### Threat 5 — Untrusted Web Content
A webpage could provide misleading or malicious data to the agent.

Control:
Treat retrieved webpage content as untrusted data rather than authoritative instructions.

### Threat 6 — Weak Tool Approval Policy
A permissive exec-policy could allow a model-generated command to run without sufficient authorization.

Control:
Use the effective OpenClaw exec-policy as a security boundary between a proposed tool call and actual execution.