## 1. Large Language Model (LLM)

**Definition:** A neural network trained to predict the next token in an input sequence.

**Example:**

- Input: "All that glitters"
- LLM predicts: "is"
- Then predicts: "not"
- Then predicts: "gold"
- Complete output: "All that glitters is not gold"

**Key Point:** The model continues predicting one token at a time until it completes the response.

---

## 2. Tokenization

**Definition:** The process of breaking input text into discrete tokens (the smallest meaningful units).

**Example:**
Input: "All that glitters"

Breaks down into:

- Token 1: "all"
- Token 2: " " (space character)
- Token 3: "that"
- Token 4: " " (space)
- Token 5: "glitters"

**Why not just split by spaces?**
Humans use language with patterns and meaning. Consider these suffixes:

- **"-ers"** suffix: glimmers, murmurs, flickers (indicates the action is being performed by an object)
- **"-ing"** suffix: eating, dancing, singing (indicates an action is being performed)

By recognizing these tokens, the LLM can better understand the structure and meaning of human language.

---

## 3. Vectors

**Definition:** A coordinate representation of a word in an n-dimensional space where similar meanings are placed close together.

**How it works:**

- Words with similar meanings are clustered together in vector space
- Words with opposite meanings are placed far apart
- Each word's meaning is converted into coordinates in this multi-dimensional space

**Example:**
If you map words in a 2D space (imagine it's actually n-dimensional):

- "happy," "joyful," "cheerful" would be close together
- "sad," "unhappy," "depressed" would be close together but far from the happy cluster
- "apple" (fruit) would be near "banana," "orange"
- "Apple" (company) would be near "Google," "Microsoft"

**Purpose:** This vectorization allows LLMs to understand the inherent meaning of words and construct sentences effectively.

---

## 4. Attention

**Definition:** A mechanism that helps LLMs understand word meaning by looking at surrounding context.

**The Problem:** Words can have multiple meanings depending on context.

**Example with "Apple":**

1. **"It is a tasty apple"**

   - Context word: "tasty"
   - Meaning: fruit (the edible apple)
   - Vector pushed toward: banana, orange, guava

2. **"Apple's revenue"**

   - Context word: "revenue"
   - Meaning: the company
   - Vector pushed toward: Google, Meta, Microsoft

3. **"The apple of my eye"**
   - Context words: "of my eye"
   - Meaning: a person you have affection for
   - Vector pushed toward: affection, love-related concepts

**How it works:**

- Take the vector of the ambiguous word (e.g., "apple")
- Take the vector of nearby contextual words (e.g., "revenue" or "tasty")
- Perform the attention operation (not simple addition, but a specific mathematical operation)
- This pushes the ambiguous word's vector toward the correct meaning

**Historical significance:** This breakthrough came in 2017, but became widely famous in 2022 with GPT-2, dramatically improving the quality of LLM responses.

---

## 5. Self-Supervised Learning

**Definition:** A training method where the structure of the input data itself tells the model what it should learn, without human labeling.

**Traditional supervised learning:**

- Humans provide: "If input is 'all that glitters', output should be 'is not gold'"
- Requires manual labeling for every example
- Expensive and time-consuming

**Self-supervised learning:**

- Take existing text: "Et tu, Brutus"
- Create three automatic training tasks:
  1. What comes after "Et"? → Answer: "tu"
  2. What comes after "Et tu"? → Answer: "Brutus"
  3. What comes after "Et tu Brutus"? → Answer: "," or "." or stop token

**Visual Example from Video:**

- Sequence: 5, 4, 3, 2, \_\_\_
- Your mind predicts: 1
- No one needed to tell you the answer; the pattern itself taught you

**Another Example:**

- Show an image with one part blanked out
- Your mind fills in the missing part based on context
- The model learns the same way

**Why it's revolutionary:**

- No human intervention needed to create training data
- Can use any text that exists on the internet
- Makes training highly scalable and much cheaper
- Most AI models are now moving to self-supervised learning (even image and video models)

---

## 6. Transformer

**Definition:** A specific architecture/algorithm for predicting the next token in a sequence.

**Important distinction:**

- **Large Language Model (LLM):** The product/goal - something that predicts the next token
- **Transformer:** The engine/method - a specific way to achieve that goal
- Analogy: LLM is the car, Transformer is the engine

**How a Transformer works:**

```
Input Tokens
    ↓
[Attention Block] ← Disambiguates terms, finds basic relationships
    ↓
[Feedforward Neural Network]
    ↓
[Attention Block] ← Finds complex relationships (sarcasm, implications)
    ↓
[Feedforward Neural Network]
    ↓
... (stacked 12+ layers, sometimes hundreds in recent models)
    ↓
Output Prediction
```

**Example of layered understanding:**

**First attention layer:**

- Input: "A crane was hunting a crab"
- Understanding: "crane" = bird (not metal machine)

**Second attention layer:**

- Inference: The crab is fearful
- Inference: The crane is hungry
- Understanding: More complex relationships beyond just word disambiguation

**Note:** The attention block has O(n²) complexity. The transformer architecture could be replaced with other methods like state space models or diffusion models in future LLM designs.

---

## 7. Fine-Tuning

**Definition:** The process of taking a base model and training it to respond in a specific way or domain.

**Process:**

**Step 1: Train Base Model**

- Use self-supervised learning on general text
- Model learns language structure and patterns

**Step 2: Fine-Tune with Q&A**

- Provide domain-specific questions and expected answers
- Penalize undesirable responses
- Reward desirable responses

**Example - Medical LLM:**

- Base model trained on general text
- Fine-tuned with medical questions and answers
- Result: Model learns to think and respond using medical terminology

**Example - Financial LLM:**

- Same base model
- Fine-tuned with financial questions and answers
- Result: Model learns to think and respond using financial terminology

**Example of fine-tuning responses:**

Question: "Who is the president of USA?"

❌ **Undesirable responses (penalized):**

- "I would like to know that too"
- "No" (not helpful)

✅ **Desirable responses (rewarded):**

- "Donald Trump"
- "I don't have that information" (honest)

**Key benefit:** One base model can be fine-tuned multiple ways to create specialized models for different companies or use cases.

---

## 8. Few-Shot Prompting

**Definition:** Adding examples to your query before sending it to an LLM to improve response quality.

**How it works:**

Instead of sending:

```
User query: "Where is my parcel?"
```

You send:

```
Examples:
Q: "Where is my order?"
A: "Your order is currently in transit and will arrive by Friday."

Q: "I haven't received my package"
A: "Let me check your tracking number. Your package is at the local facility."

User query: "Where is my parcel?"
```

**Key characteristics:**

- Happens during inference time (production/runtime)
- Examples are added in real-time by your server
- Also called "example prompting" or "examples in prompt"
- Quality of response improves because model has context for the expected format and style

---

## 9. Retrieval Augmented Generation (RAG)

**Definition:** Enhancing LLM responses by retrieving and including relevant documents as context with the user query.

**The RAG pipeline:**

```
User Query → Server
    ↓
Server fetches:
1. Examples (few-shot prompting)
2. Relevant company documents
3. Policy documents
4. Terms and conditions
    ↓
Server sends to LLM:
- User query
- Examples (format guidance)
- Retrieved documents (company-specific context)
    ↓
LLM generates high-quality, context-aware response
```

**Example scenario:**

- User asks: "What's your refund policy?"
- Server retrieves: Company refund policy document
- Server retrieves: Examples of refund-related responses
- LLM receives: Query + Policy document + Examples
- Result: Accurate, company-specific response

**Where are documents stored?**
Multiple options exist:

- Vector database (most common - easier similarity search)
- Graph database (e.g., Neo4j)
- In-memory cache
- Traditional databases

**Key benefit:** LLM can provide accurate, current, company-specific information without being retrained.

**Note:** Some people now say "RAG is dead" because the AI space evolves so quickly, but the concept remains important.

---

## 10. Vector Database

**Definition:** A database designed to store documents as vectors and quickly retrieve similar documents based on semantic meaning.

**How it works:**

**Step 1: Store documents as vectors**

- Documents are converted to vector representations
- Stored in n-dimensional space
- Similar documents are close together in this space

**Step 2: Query comes in**
Example query: "I am upset with your payment system. I expect a refund."

**Step 3: Find similar documents**

- Convert query to vector
- Search for documents with smallest distance to query vector
- Key terms in query: "upset," "refund," "payment system"

**Semantic matching example:**

- Your company policy might not use the word "upset"
- But you have documents about "low rating" or "customer drop-off"
- Vector database understands these are semantically similar
- Distance between "upset" and "low rating" is small in vector space
- These documents are retrieved

**Common algorithm:** Hierarchical Navigable Small World (HNSW) for efficient similarity search

**Use case:** Store internal company documents and information, then quickly retrieve relevant context for LLM queries

---

## 11. Model Context Protocol (MCP)

**Definition:** A protocol for LLMs to access external tools, databases, and services to get real-time information.

**The problem MCP solves:**
What if the context you need exists outside your system?

**How MCP works:**

```
User → Query
    ↓
MCP Client (receives query)
    ↓
LLM (decides: "I need external data")
    ↓
MCP Client connects to external MCP Servers
    ├─→ Indigo Airlines MCP Server (flight data)
    └─→ Air India MCP Server (flight data)
    ↓
Flight details returned to MCP Client
    ↓
MCP Client sends to LLM:
    - Original user query
    - System prompt
    - Internal vector database context
    - External real-time flight data
    ↓
LLM makes decision: "Book Indigo flight 1020"
    ↓
MCP Client executes booking via Indigo MCP Server
    ↓
Result sent back to user
```

**Example scenario:**

- User: "Book me a flight to Delhi tomorrow"
- LLM needs real-time flight information
- MCP Client queries multiple airline servers
- Gets current prices, availability, schedules
- LLM decides best option
- MCP Client executes the booking
- User receives confirmation

**Key advancement:** LLMs can now execute actions, not just provide information. The user doesn't have to manually follow a recipe - the MCP client executes the entire workflow.

**Current status:** MCP has gained significant popularity recently.

---

## 12. Context Engineering

**Definition:** The practice of optimizing and managing the context provided to an LLM, combining multiple techniques for better responses.

**Components of context engineering:**

1. **Few-shot prompting** - Adding examples
2. **RAG** - Retrieving relevant documents from vector database
3. **MCP** - Connecting to external servers and tools
4. **User preferences** - Storing and using user-specific settings
5. **Context summarization** - Managing conversation history

**Key difference from Prompt Engineering:**

- **Prompt engineering:** Stateless, single prompt optimization
- **Context engineering:** Stateful, evolves with user preferences and chat history

**Context summarization techniques:**

**1. Sliding window approach:**

```
Last 100 chats → Sent directly to LLM
All previous chats → Summarized into 5 sentences
```

**2. Keyword focus:**

- Extract only important keywords from history
- Send keywords + current query

**3. Recent + summary:**

- Last chat sent in full
- All previous history summarized

**4. Smart preprocessing:**

- Use a cheap/small LLM to summarize documents
- Send summarized version to expensive LLM
- Reduces cost while maintaining context

**Two new challenges:**

1. **User preferences:** How to store and apply long-term user preferences
2. **Prompt/Context summarization:** How to compress long conversation history effectively

---

## 13. Agents

**Definition:** A long-running process with multiple capabilities that can autonomously query LLMs, external systems, and other agents to meet user requirements.

**What an agent can do:**

```
User Request → Agent (server process)
    ↓
Agent capabilities:
├─→ Query LLM
├─→ Query external systems
├─→ Communicate with other agents
└─→ Make autonomous decisions
```

**Example: Travel Agent**

**Capabilities:**

- Book flights
- Book hotels
- Manage email when you're away
- Monitor prices

**Autonomous behavior:**

- Watches for cheap flight windows
- Makes bookings according to your preferences automatically
- No constant user input needed

**Key characteristics:**

- Long-running (not just one-time responses)
- Autonomous decision-making
- Can combine multiple tools and services
- Acts on behalf of user based on preferences

**Current status:** Agents are the most hyped and long-term direction in AI development.

---

## 14. Reinforcement Learning with Human Feedback (RLHF)

**Definition:** A training method where models learn to behave better by receiving rewards (positive feedback) or penalties (negative feedback) from human evaluators.

**How it works:**

**Step 1: Model generates multiple responses**

```
User query → LLM generates:
- Response 1
- Response 2
```

**Step 2: Human chooses better response**

- Human selects Response 1 as better
- Response 1 gets score: +1
- Response 2 gets score: -1

**Step 3: Model learns from feedback**

The model maps this in vector space:

```
Starting point (user query vector)
    ↓
Path to Response 1: +1, +1, +1, +1, +1 (positive region)
Path to Response 2: +1, +1, +1, -1, -1, -1 (negative region)
```

After many iterations:

- **Positive regions:** Model wants to go here (good responses)
- **Negative regions:** Model avoids these paths (bad responses)
- **Neutral regions:** Score of 0 (mixed feedback)

**Analogy: Hill climbing**

- Model tries to find the highest points (most positive feedback)
- Avoids valleys (negative feedback)
- Optimizes the path from query to response

**Goal:** Train the model to produce responses that make end users happy.

**Famous example: Pavlov's Dog**

- Bell rings → Dog gets food
- After repetition: Bell rings → Dog salivates (even without food)
- Behavior has been reinforced

**Important limitation:**

Reinforcement learning observes patterns but doesn't build true understanding.

**Coin flip example:**

- Fair coin flipped: Heads, Heads, Heads, Heads, Heads, Heads
- **RL model thinks:** "Heads is more likely next" (based on observation)
- **Human thinks:** "Still 50/50 chance" (based on understanding of fair coin physics)

**Key difference:**

- RL: Learns from outcomes, finds beneficial paths
- Humans: Build mental models of how things actually work
- Humans are not crocodiles - we have deeper understanding beyond pattern recognition

**Despite limitations:** RLHF is a powerful technique that makes models significantly smarter.

---

## 15. Chain of Thought (CoT)

**Definition:** Training models to break down problems step-by-step and explain their reasoning process, leading to higher quality responses.

**How it works:**

**Traditional approach:**

```
Question: Complex math problem
Answer: 42
```

**Chain of Thought approach:**

```
Question: Complex math problem

Step 1: First, let's identify what we know...
Step 2: Next, we can calculate...
Step 3: Then, we apply this formula...
Step 4: Finally, we arrive at...
Answer: 42
```

**Training process:**

- Show the model examples with step-by-step breakdowns
- Model learns to reason through problems systematically
- Model can apply this reasoning to new problems with different parameters

**Key benefits:**

1. **Better quality:** Step-by-step reasoning produces more accurate results
2. **Adaptability:** Model can adjust reasoning for different problem difficulties
3. **Transparency:** Users can see how the model arrived at its answer

**Advanced behavior (seen in models like DeepSeek):**

- **Easy problems:** Fewer reasoning steps
- **Hard problems:** More reasoning steps
- Model automatically adjusts based on complexity

**Related concepts:**

- **Tree of Thought:** Explores multiple reasoning branches
- **Graph of Thought:** More complex reasoning structures
- Can also use external tools to enhance reasoning

**Reasoning Models (o-series):**
Models specifically designed to reason through problems, also called "o-LMs" or "reasoning models."

**Examples:**

- OpenAI's O1, O3
- DeepSeek reasoning models

**Note:** Reasoning models don't necessarily use Chain of Thought exclusively - they can use various reasoning algorithms.

---

## 16. Multimodal Models

**Definition:** AI models that can accept and generate multiple types of data (not just text), including images, videos, and audio.

**Capabilities:**

**Image understanding:**

- Analyze images
- Count objects ("How many apples are in this image?")
- Describe scenes
- Recognize patterns

**Image generation:**

- Create new images from text descriptions
- Modify existing images
- Generate variations

**Video understanding:**

- Analyze video content
- Track movements
- Understand temporal relationships

**Video generation:**

- Create videos from text descriptions
- Modify existing videos
- Generate realistic motion

**Why "multimodal" is important:**

**Current impact:**

- Text LLMs have transformed marketing and content creation
- Social media is now full of LLM-generated content

**Future impact:**

- Better image generation will transform visual content
- Video generation could be revolutionary
  - Example: Celebrities creating ads through LLMs
  - Drastically reduced cost of video production
  - Currently happening to some extent, but quality needs improvement

**Surprising benefit:**
Training on multiple modalities (text + images) actually improves performance compared to text-only training.

**Why?**

- Models develop deeper understanding of object meanings
- Example: Training on both the word "cat" and images of cats
- Result: Better understanding of what "cat" means
- Output quality improves across all modalities

**Note:** "Multimodal" can technically mean any combination of input/output modes, but most commonly refers to text + images + video.

---

## 17. Small Language Models (SLMs)

**Definition:** Language models with significantly fewer parameters (3 million to 300 million) compared to LLMs (3 billion to 300 billion parameters).

**Why companies want SLMs:**

1. **More control:** Greater control over what the model generates
2. **Data privacy:** Keep proprietary data internal, don't expose to third parties
3. **Cost efficiency:** Cheaper to run and maintain
4. **Task-specific:** Optimized for specific company needs

**Parameter comparison:**

```
Small Language Model (SLM):
- 3 million to 300 million parameters
- Smaller neural network
- Fewer connections and weights

Large Language Model (LLM):
- 3 billion to 300 billion parameters
- Very large neural network
- Many more connections and weights
```

**Trade-offs:**

**What SLMs can do well:**

- Excel at specific, focused tasks
- Example: Customer service bot trained only on sales and customer queries
- Deep expertise in their trained domain

**What SLMs cannot do well:**

- Limited general knowledge
- Example: Sales bot probably can't do detailed weather analysis

**Use cases:**

**Company-specific bot:**

- Trained on internal company data
- Handles customer queries
- Makes sales
- Provides support

**NASA example:**

- Would build a model for weather prediction
- Not focused on sales or customer service
- Task-specific expertise

**Training approach:**

- Trained on less data (company-specific or task-specific)
- Focused expertise rather than broad knowledge
- Proprietary data stays internal

**Creation method:** Usually built through **distillation** (see next concept).

---

## 18. Distillation

**Definition:** The process of creating a smaller, more efficient model (student) by training it to mimic a larger, more capable model (teacher).

**The process:**

```
Input Query
    ↓
    ├─→ Large Language Model (Teacher)
    │       ↓
    │   Teacher Output
    │
    └─→ Small Language Model (Student)
            ↓
        Student Output
```

**Step-by-step:**

1. **Send same input to both models**

   - Teacher (LLM): Large, complex, many parameters
   - Student (SLM): Small, fewer parameters (3-300 million)

2. **Compare outputs**

   - If outputs match → Student is doing well, no weight changes needed
   - If outputs don't match → Student's internal weights are adjusted

3. **Repeat many times**
   - Student learns to mimic teacher's behavior
   - Complex knowledge gets condensed into smaller representation

**Analogy:**
Imagine compressing a large, detailed book into a smaller summary that captures the essential information.

**Goal:**
Condense the complex neural network's knowledge into the most reasonable representation possible while maintaining acceptable performance.

**Benefits:**

1. **Speed:** Much faster at responding during production
2. **Cost:** Significantly reduced inference costs
3. **Hosting:** Easier and cheaper to host
4. **Performance:** Maintains acceptable quality for specific tasks

**Key point:** The student is constrained to a limited number of weights (3-300 million), so it learns to represent information as efficiently as possible.

**When it's used:** After the large teacher model is fully trained, you create distilled versions for production deployment.

---

## 19. Quantization

**Definition:** The process of reducing the precision of neural network weights (from 32-bit to 8-bit) to significantly reduce memory usage and improve inference speed.

**How it works:**

**Before quantization:**

- Each weight in the neural network is a 32-bit number
- High precision but uses more memory

**After quantization:**

- Each weight is reduced to an 8-bit number
- Lower precision but much more memory efficient

**Memory savings calculation:**

```
32-bit → 8-bit = 75% memory reduction
```

**Important clarifications:**

**What gets quantized:**

- Primarily the feedforward neural network weights
- Attention mechanism typically remains unchanged

**What doesn't reduce:**

- Training cost stays the same
- You still train with full precision first
- Quantization is applied after training is complete

**Primary benefit:**

- **Reduces inference cost** (production runtime)
- **Does not reduce training cost**

**The process:**

```
Step 1: Train model with full precision (32-bit)
    ↓
Step 2: Achieve optimal performance
    ↓
Step 3: Apply quantization (reduce to 8-bit)
    ↓
Step 4: Deploy quantized model for production
```

**Why it works:**

- Once a model is fully trained, it doesn't need ultra-high precision
- 8-bit precision is usually sufficient for making good predictions
- Minimal accuracy loss for significant efficiency gains

**Note:** Memory savings don't directly map to 75% total model size reduction because attention mechanisms and other components remain unchanged. But the savings are still substantial.

---

## 20. Foundation Models

**Definition:** Large base models trained on broad data that serve as the starting point for creating specialized models through fine-tuning or distillation.

**Why companies want foundation models:**

1. **Control:** Complete control over model behavior and outputs
2. **Privacy:** Keep sensitive data internal and secure
3. **Customization:** Tailor models to specific business needs
4. **Independence:** Less reliance on third-party AI companies

**Relationship to Small Language Models:**

Foundation models are often the base for creating SLMs:

```
Foundation Model (base)
    ↓
Fine-tuning OR Distillation
    ↓
Company-specific SLM
```

**Examples of specialized foundation models:**

**Sales-focused model:**

- Trained on: Customer queries, sales techniques, product information
- Expert at: Making sales, handling customer queries
- Not good at: Weather analysis, scientific research

**NASA weather model:**

- Trained on: Weather data, atmospheric science, climate patterns
- Expert at: Weather prediction, climate analysis
- Not needed: Sales capabilities

**Key characteristics:**

1. **Task-specific training:** Focused on particular domains
2. **Proprietary data:** Uses company's internal data
3. **Reasonable performance:** Good enough for specific use cases
4. **Practical limitations:** Won't excel at tasks outside training domain

**The trade-off:**

```
Large General Model:
- Knows everything reasonably well
- Jack of all trades, master of none
- Expensive to run

Foundation Model → SLM:
- Expert in specific domain
- Doesn't know much outside domain
- Much cheaper to run
```

**Current trend:** Many companies are building their own foundation models rather than relying solely on general-purpose LLMs from OpenAI, Anthropic, etc.

---

## Summary

These 20 concepts form the foundation of modern AI engineering:

**Core Architecture:** LLMs, tokenization, vectors, attention, transformers
**Training Methods:** Self-supervised learning, fine-tuning, distillation, quantization, RLHF
**Context & Response:** Few-shot prompting, RAG, chain of thought, context engineering
**Infrastructure:** Vector databases, MCP, agents
**Model Types:** Multimodal models, SLMs, foundation models

Understanding these terms will help you:

- Communicate effectively with AI engineers and teams
- Learn deeper AI subjects more easily
- Recognize hype from substance in the AI space
- Make informed decisions about AI implementations
