# ChatAndBuild: Agent Runtime Architecture

> **A stateful chat thread driving model orchestration via the Responses API, with server-side tool execution, scoped memory retrieval, and event streaming to the UI.**

---

## 🏗️ Architecture Overview

ChatAndBuild's agent runtime is built on a sophisticated control plane that orchestrates AI interactions through a stateful thread system. This architecture ensures secure, observable, and context-aware AI operations with real-time streaming capabilities.

---

## 🎯 Core Components

### 1. **Thread (State Container)**
The foundational stateful container that maintains:
- Agent specifications and configurations
- Tool execution policies and permissions
- Memory scope and context boundaries
- User identity and session state
- Execution receipts and audit trails

### 2. **Router (Intelligence Layer)**
Dynamic model selection based on:
- **Task Classification**: Reasoning, JSON generation, microtasks, voice, code diffs
- **Risk Assessment**: Security implications and data sensitivity
- **Latency Targets**: Real-time vs. batch processing requirements
- **Budget Constraints**: Token limits and cost optimization

### 3. **Responses API (Control Plane)**
Unified interface providing:
- **Tool Calling**: First-class tool outputs with structured execution
- **Structured Outputs**: Schema-locked JSON artifacts
- **Streaming Events**: Observable UX with real-time updates
- **Multimodal Support**: Text, voice, and future modalities

### 4. **Tool Runner (Execution Engine)**
Server-side tool execution with:
- **Allowlist Enforcement**: Strict permission boundaries
- **Schema Validation**: Type-safe input/output contracts
- **Scope & Time Bounds**: Resource and temporal constraints
- **Sanitization**: Input validation and output cleaning
- **Receipt Generation**: Audit trail for every execution

### 5. **Memory / RAG System**
Embeddings-backed retrieval featuring:
- **text-embedding-3-small**: Efficient vector representations
- **MongoDB Vector Index**: Scalable similarity search
- **Scoped Retrieval**: Permission-aware context fetching
- **Ranked Snippets**: Relevance-ordered results

### 6. **Event Stream (Observable UX)**
Real-time state broadcasting:
- **Token Deltas**: Incremental response generation
- **Tool Lifecycle**: Request → Running → Result → Applied
- **JSON Blocks**: Structured intent/spec/suggestions
- **Performance Metrics**: TTFT, tool time, end-to-end latency

### 7. **Realtime Worker (Voice)**
Optional voice interaction layer:
- **LiveKit Session**: WebRTC-based audio streaming
- **OpenAI RealtimeModel**: Low-latency voice processing
- **Unified Policy**: Same tool and memory constraints
- **I/O Adaptation**: Voice-specific input/output handling

---

## 📊 System Flow Diagram

```mermaid
flowchart TB
    %% Styling
    classDef threadClass fill:#667eea,stroke:#764ba2,stroke-width:3px,color:#fff
    classDef routerClass fill:#f093fb,stroke:#f5576c,stroke-width:2px,color:#fff
    classDef apiClass fill:#4facfe,stroke:#00f2fe,stroke-width:3px,color:#fff
    classDef toolClass fill:#43e97b,stroke:#38f9d7,stroke-width:2px,color:#000
    classDef memoryClass fill:#fa709a,stroke:#fee140,stroke-width:2px,color:#fff
    classDef streamClass fill:#30cfd0,stroke:#330867,stroke-width:2px,color:#fff
    classDef voiceClass fill:#a8edea,stroke:#fed6e3,stroke-width:2px,color:#000

    %% Main Components
    A["🧵 Thread<br/><small>State Container</small><br/>━━━━━━━━━━━━━<br/>• agent_spec<br/>• tool_policy<br/>• memory_scope<br/>• identity<br/>• receipts"]:::threadClass
    
    B["🎯 Router<br/><small>Intelligence Layer</small><br/>━━━━━━━━━━━━━<br/>• task_class<br/>• risk assessment<br/>• latency target<br/>→ model + budgets"]:::routerClass
    
    C["⚡ Responses API<br/><small>Control Plane</small><br/>━━━━━━━━━━━━━<br/>• tool calls<br/>• structured outputs<br/>• streaming events<br/>• multimodal support"]:::apiClass
    
    D["🔧 Tool Runner<br/><small>Execution Engine</small><br/>━━━━━━━━━━━━━<br/>• allowlist enforcement<br/>• schema validation<br/>• scope/time bounds<br/>• sanitization<br/>• receipts"]:::toolClass
    
    E["🧠 Memory / RAG<br/><small>Context Retrieval</small><br/>━━━━━━━━━━━━━<br/>• text-embedding-3-small<br/>• MongoDB vector index<br/>• scoped retrieval<br/>• ranked snippets"]:::memoryClass
    
    F["📡 Event Stream<br/><small>Observable UX</small><br/>━━━━━━━━━━━━━<br/>• token deltas<br/>• tool lifecycle<br/>• JSON blocks<br/>• metrics"]:::streamClass
    
    G["🎤 Realtime Worker<br/><small>Voice Layer</small><br/>━━━━━━━━━━━━━<br/>• LiveKit session<br/>• OpenAI RealtimeModel<br/>• unified policy<br/>• I/O adaptation"]:::voiceClass

    %% Primary Flow
    A -->|"1. Initialize with state"| B
    B -->|"2. Select model & config"| C
    
    %% Tool Execution Loop
    C -->|"3a. Execute tool calls"| D
    D -->|"3b. Return results"| C
    
    %% Memory Integration
    A -->|"4a. Retrieve context"| E
    E -->|"4b. Inject snippets"| C
    
    %% Streaming to UI
    C -->|"5a. Stream tokens & events"| F
    D -->|"5b. Tool lifecycle events"| F
    
    %% Voice Path (Optional)
    A -.->|"6a. Voice session"| G
    G -.->|"6b. Realtime interaction"| C
    G -.->|"6c. Voice events"| F
    
    %% Feedback Loop
    C -.->|"7. Update receipts & memory"| A

    %% Annotations
    subgraph "Core Agent Loop"
        A
        B
        C
    end
    
    subgraph "Execution & Context"
        D
        E
    end
    
    subgraph "Observable Interface"
        F
        G
    end
```

---

## 🔄 Execution Flow

### Phase 1: Initialization
1. **Thread Creation**: Establish stateful container with agent spec, permissions, and scope
2. **Router Analysis**: Classify task, assess risk, determine optimal model and budget
3. **Context Loading**: Retrieve relevant memory snippets based on scope

### Phase 2: Orchestration
4. **API Invocation**: Send request to Responses API with full context
5. **Streaming Start**: Begin token-by-token response generation
6. **Tool Detection**: Identify and validate tool call requests

### Phase 3: Execution
7. **Tool Running**: Execute tools server-side with strict validation
8. **Result Integration**: Inject tool results back into conversation
9. **Memory Updates**: Store new context and execution receipts

### Phase 4: Delivery
10. **Event Broadcasting**: Stream all state changes to UI
11. **Metrics Collection**: Track TTFT, tool time, end-to-end latency
12. **Session Persistence**: Update thread state with complete history

---

## 🛡️ Security & Governance

### Tool Execution Safety
- ✅ **Server-side Only**: No client-side tool execution
- ✅ **Allowlist Enforcement**: Explicit permission required
- ✅ **Schema Validation**: Type-safe inputs/outputs
- ✅ **Credential Isolation**: Model never sees secrets
- ✅ **Audit Trails**: Complete execution receipts

### Memory & Context
- ✅ **Scoped Retrieval**: Permission-aware access
- ✅ **User Isolation**: Thread/user/workspace boundaries
- ✅ **Ranked Results**: Relevance-based context injection
- ✅ **Embedding Security**: Isolated vector spaces

---

## 📈 Observable Operations

### Real-time Visibility
```
Token Deltas        → Incremental response generation
Tool Lifecycle      → requested → running → result → applied
Structured Blocks   → Intent, specs, suggestions (JSON)
Performance Metrics → TTFT, tool time, end-to-end latency
```

### Event Types
- **Generation Events**: Token streams, completion markers
- **Tool Events**: Invocation, execution, results, errors
- **Memory Events**: Retrieval requests, context injection
- **System Events**: Model selection, budget tracking, errors

---

## 🎤 Voice Integration

The Realtime Worker extends the core runtime to voice interactions:

- **Same Policies**: Tool permissions and memory scope apply
- **LiveKit Transport**: WebRTC-based audio streaming
- **OpenAI RealtimeModel**: Low-latency voice processing
- **Unified Events**: Voice interactions stream to same UI layer

---

## 🚀 Key Advantages

1. **Stateful Intelligence**: Thread-based context preservation
2. **Observable UX**: Real-time visibility into AI operations
3. **Secure Execution**: Server-side tool running with audit trails
4. **Scoped Memory**: Permission-aware context retrieval
5. **Unified Interface**: Single API for text, tools, and voice
6. **Performance Tracking**: Built-in latency and cost monitoring
7. **Multimodal Ready**: Architecture supports future modalities

---

## 🔧 Technical Stack

- **Control Plane**: OpenAI Responses API
- **Embeddings**: text-embedding-3-small
- **Vector Store**: MongoDB with vector indexes
- **Voice**: LiveKit + OpenAI RealtimeModel
- **Streaming**: Server-Sent Events (SSE)
- **State**: Thread-based persistence

---

## 📝 Builder's Perspective

> "Everything starts with the chat thread as a stateful agent container. It has a compiled agent spec, tool permissions, and memory scope. That's the contract."

The architecture prioritizes:
- **Observability**: Users see process, not just answers
- **Security**: Tools execute server-side with strict governance
- **Context**: Memory retrieval is scoped and permission-aware
- **Performance**: Streaming provides immediate feedback
- **Extensibility**: Unified interface supports new capabilities

---

## 🎯 Use Cases

- **Conversational AI**: Context-aware chat with tool access
- **Code Generation**: Secure execution of development tools
- **Voice Assistants**: Real-time voice with tool capabilities
- **RAG Applications**: Scoped memory retrieval with citations
- **Agent Workflows**: Multi-step orchestration with observability

---

## 📚 Further Reading

- [OpenAI Responses API Documentation](https://platform.openai.com/docs/api-reference/responses)
- [LiveKit Real-time Communication](https://livekit.io/)
- [MongoDB Vector Search](https://www.mongodb.com/products/platform/atlas-vector-search)
- [Server-Sent Events (SSE)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

---

<div align="center">

**Built with ❤️ by the ChatAndBuild Team**

*Empowering developers with observable, secure, and intelligent AI operations*

</div>
