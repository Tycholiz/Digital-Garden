
## Projects at a high level
1. Create a detailed PRD, with help from AI
2. Get the AI to break down the PRD into high level implementation goals
3. Get the AI to craft a detailed spec prompt for each goal
    - Prompt should include, well a prompt (one sentence), context (needed files, documentation, dependencies,…), tasks (CREATE / UPDATE files and functions, other key actions), cross-cutting concerns (to make sure the prompts are well wired together and don’t leave any gaps), and finally the prompt must include expected output (files/function created/updated, any other important output expected from submitting the prompt).

# Actionable Tips for Coding with AI

## Core Mindset Shifts

### 1. Assume AI is Wrong Until Proven Right
- **Expect 70% of AI output to need correction** - this is normal and productive
- **Read generated code as it's being written** - don't just accept and run
- **Use AI as a conversation partner**, not an infallible code generator

### 2. Master the "Reset and Refine" Approach
Josh Nelands shared a particularly effective technique for implementing this reset and refine approach: Let the AI work on the problem for 10 to 20 minutes, nudging it along as needed. Then, ask it to reflect on the entire conversation, analysing what went wrong, why it struggled, and what information it wished it had known at the start to avoid these problems. With this reflection in hand, revert the conversation and start fresh armed with those insights. He reports that this meta-learning approach often leads to dramatically better results than continuing to iterate on a struggling conversation.

- When AI output isn't quite right, get it to generate some lessons about the context and what we're struggling with, then **start fresh with a more specific prompt and provide the lessons**
- Avoid continuous iteration that creates complex, meandering context
- Let AI work for 10-20 minutes, then ask it to reflect on what went wrong and restart with those insights. After these conversations yield insights, ask the AI to document what we have learned. This serves as both documentation for humans and AI.

## Setup and Configuration

### 3. Create Repository-Specific AI Rules
- Set up **Cursor Rules** tailored to each project's patterns and constraints
- Only add rules when you notice **repeated AI mistakes**
- Include specific architectural decisions (e.g., "don't run database migrations directly")
- Document your preferred coding styles, frameworks, and patterns

### 4. Strengthen Your Quality Guardrails
- **Don't trust AI-written tests blindly** - write test names yourself, let AI handle implementation
- Use **strict typing and linting** to guide AI toward better code generation
- Implement **type hints even in dynamic languages** to give AI better context
- Maintain rigorous code review processes for AI-generated code

## Workflow Optimization

### 5. Keep Changes Extremely Small
- **Ship to production multiple times daily** with tiny, focused changes
- Break complex tasks into small pieces - ask AI how to break them down
- **Read full diffs** of every change before merging
- Small incremental changes compound quickly and maintain confidence

### 6. Use Documentation as an AI Enhancement Tool
- Keep **all documentation in the repository** alongside code
- Document **architectural decisions and project-specific patterns** upfront
- Ask AI to document complex decisions after technical discussions
- Use **clear READMEs** as the cornerstone of your documentation strategy

### 7. Be Explicit About State Management and Data Models
- **Document state management patterns** and data flow expectations
- Maintain **clear schemas in the repository** and reference them explicitly
- Establish **clear boundaries** around which components can mutate state
- Use database constraints and type hints to provide AI guardrails

## Advanced Techniques

### 8. Manage Context Strategically
- **Less context is often more effective** than overwhelming the AI
- Provide **precise, curated context** upfront rather than accumulated context
- Recognize when to **reset the conversation** vs. continue iterating
- Avoid letting context become noise rather than signal

### 9. Use Voice-to-Text for Better AI Communication
- Try tools like **Wispr Flow** for dictating thoughts and instructions
- **Brain dump ideas verbally** then let AI structure them coherently
- Preserve insights that might be lost due to typing overhead
- Leverage external processing tendencies for clearer AI instructions

### 10. Combine AI with Traditional Development Practices
- **Write using AI**: Brain dump → AI structures → Human refines → AI polishes
- Use **Test-Driven Development** patterns to maintain cognitive rhythm
- Create **deliberate breaks** in AI coding sessions to manage cognitive load
- Apply **all good coding practices more rigorously** when working with AI

## Safety and Control

### 11. Maintain Manual Control Over Actions
- **Avoid "YOLO mode"** that automatically runs terminal commands
- **Understand every command** before execution, especially if less experienced
- Watch AI actions "like a hawk" when automation is enabled
- Maintain explicit permission for environment-changing actions

### 12. Focus on Human-Unique Skills
- Develop **complexity recognition** and architectural intuition
- Practice **empathy for users** and understanding business constraints
- Cultivate **creative problem-solving** abilities that AI struggles with
- Build **deep product knowledge** that transcends code implementation

## Knowledge Management

### 13. Create Living Documentation Systems
- Use AI to **connect ideas across your entire knowledge base**
- Maintain **markdown repositories** that AI can read and understand
- Let AI **spot forgotten commitments** and surface missed connections
- Build **semantic maps** of your thinking with AI assistance

### 14. Prepare for Rapid AI Evolution
- **Monitor AI capability improvements** - the 50% task completion horizon doubles every 7 months
- **Practice higher-order thinking skills** that remain valuable as AI improves
- **Focus on computational thinking** rather than implementation details
- **Prepare for AI that can maintain focus for increasingly extended periods**

## Team and Learning Considerations

### 15. Rethink Junior Developer Training
- **Pair with juniors** to show them how to use AI safely
- Focus on **architectural patterns and complexity recognition** over syntax
- Emphasize **production concerns**: testing, deployment, security, data modeling
- Create **new forms of deliberate practice** for higher-order skills

### 16. Maintain Critical Engagement
- **Never stop critically evaluating AI output** regardless of AI improvement
- **Exercise System 2 thinking** - slow, deliberate analysis of AI suggestions
- **Articulate why something feels wrong** when AI suggestions seem plausible but off
- **Prepare for AI slop** as tools become more capable and tempting to trust blindly

## Productivity Hacks

### 17. Optimize Your AI Workflow
- **Keep conversations focused and bounded** to prevent AI confusion
- **Use structured prompts** rather than vague requirements
- **Leverage AI for writing and knowledge work** beyond just coding
- **Create style guides** that AI can follow for consistent output

### 18. Balance Speed with Understanding
- **Get really good at reading code fast** to keep up with AI generation
- **Understand what code actually does** rather than just accepting it works
- **Make intentional architectural decisions** - don't let AI drive architecture
- **Maintain expertise in fundamentals** even as mechanics become automated
