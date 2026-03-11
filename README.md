Genesis

2. Granular OODA Loop Data Sequence
sequenceDiagram
    autonumber
    participant ChatPanel
    participant ScreenMirror
    participant OODACore as OODA Orchestrator
    participant Orient as Orient Phase
    participant Decide as Decide Phase
    participant Act as Act Phase
    participant Aegis as Aegis Security
    participant Cortex as Gemini 2.0 Flash
    participant Motor as Motor Proxy

    Note over ChatPanel,OODACore: User Interaction
    ChatPanel->>OODACore: WS type=chat, message=Initialize production server
    ScreenMirror->>OODACore: WS type=frame, image=data:image/jpeg;base64,...

    Note over OODACore,Orient: 1. OBSERVE and 2. ORIENT
    OODACore->>Orient: Pass text, frame, context window
    Orient->>Orient: Fetch RAG context from ChromaDB
    Orient->>Orient: Get current mood from Anima
    Orient->>Orient: Append user text to rolling 20-turn ContextManager
    Orient-->>OODACore: Return enriched_prompt

    Note over OODACore,Decide: 3. DECIDE
    OODACore->>Decide: stream_task(enriched_prompt, frame)
    Decide->>Cortex: google-genai generation stream request
    Cortex-->>Decide: Yield chunk Text: Okay I will run the server
    Decide-->>ChatPanel: WS type=text_response
    Cortex-->>Decide: Yield chunk ToolCall: run_command(npm run start)
    Decide->>Act: Dispatch to Tools Registry

    Note over Act,Motor: 4. ACT
    Act->>Aegis: Request clearance for run_command
    Act->>Aegis: Gate 1 - Regex check (pass)
    Act->>Aegis: Gate 2 - Path Sandbox (pass)
    Act->>Aegis: Gate 3 - Network check (pass)
    Act->>Cortex: Gate 4 - Quantify Risk Score (0-10)
    Cortex-->>Act: Score: 4 (Safe)
    Act->>Aegis: Gate 5 - Budget Check (pass)
    Act->>Aegis: Gate 6 - Rate Limit (pass)
    Act->>Aegis: Gate 7 - Dependency Scan (n/a)
    Act->>Aegis: Gate 8 - Secret Scan (n/a)
    Aegis-->>Act: Clearance Granted

    Act->>Act: Trigger Chronos (pre-action git commit)
    Act->>Motor: HTTP POST /motor/terminal/run {command: npm run start}
    Motor-->>Act: {exit_code: 0, output: Server listening on port 3000}
    Act->>OODACore: Return tool result string
    OODACore->>OODACore: Trigger Anima record_success()
    OODACore-->>ChatPanel: WS type=tool_result, output=Server listening
