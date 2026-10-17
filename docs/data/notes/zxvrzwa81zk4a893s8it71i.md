
Longer prompts (ie. more input tokens) take longer to process

### Chain-of-Thought Prompting
What it is: Getting the model to show its reasoning process step-by-step instead of jumping to conclusions.
Basic example:
Instead of: "What's 15% of 240?"
Use: "What's 15% of 240? Think through this step by step."

Advanced example:
Analyze this business decision step by step:
1. First, identify the key factors involved
2. Then, consider the potential outcomes of each option
3. Weigh the risks and benefits
4. Finally, provide your recommendation with reasoning

Why it works: Models often perform better when they "think out loud" - it prevents them from making logical leaps and helps catch errors.

### Negative Prompting
What it is: Explicitly telling the model what NOT to do or include.

Examples:

"Explain quantum physics. Do NOT use mathematical equations or jargon."
"Write a professional email. Avoid being overly formal or using corporate buzzwords."
"Summarize this article. Don't include your own opinions or speculation."

Why it's powerful: Models sometimes default to unwanted behaviors. Negative prompting helps constrain the output to exactly what you want.

### Prompt Chaining
What it is: Breaking complex tasks into a sequence of smaller prompts, where each prompt builds on the previous output.

Example sequence:

"Analyze the main themes in this document"
[Take that output] "Now identify which of these themes are most relevant to our marketing strategy"
[Take that output] "Create three marketing campaign concepts based on these relevant themes"

Why it works: Complex tasks often overwhelm models. Chaining lets you guide the process step-by-step and catch/correct errors along the way.

### Few-Shot Learning
What it is: Providing examples of the desired input-output pattern.

A key use case for Few-Shot prompting is when you need the output to be structured in a specific way that is difficult to describe to the model. Instead of describing what we want it to do, we just provide some examples and let it infer the pattern.

Example:
Convert these informal messages to professional ones:

Informal: "hey can u send me that report?"
Professional: "Could you please send me the report when you have a moment?"

Informal: "this meeting is boring"
Professional: "I'd appreciate if we could focus the discussion on the key action items."

Now convert: "ur late again"

### Role-Based Prompting
What it is: Having the model adopt a specific persona or expertise.

Examples:

"You are a senior software architect. Review this code design..."
"Act as a 5-year-old and explain how airplanes fly..."
"You're a skeptical journalist. Fact-check this press release..."

### Constitutional AI / Self-Correction
What it is: Having the model critique and improve its own output.

Example:

First, write a summary of this article.
Then, review your summary and identify any potential biases or inaccuracies.
Finally, provide a revised summary that addresses these issues.

### System Message Engineering
What it is: Using the system message (when available) to set persistent context and behavior.

Example system message:

You are a helpful coding assistant. Always:
- Explain your reasoning
- Include error handling in code examples
- Suggest best practices
- Ask clarifying questions when requirements are unclear

### Constraint-Based Prompting
What it is: Adding specific limitations to focus the output.
Examples:

"Explain this in exactly 3 sentences"
"Use only words a 10-year-old would understand"
"Respond only with bullet points"
"Include exactly 5 examples"

### Meta-Prompting
What it is: Having the model help you improve your prompts.
Example:
"I want to get better results when asking you to write marketing copy. What information should I include in my prompts to get more targeted, effective copy?"

### Template-Based Prompting
What it is: Creating reusable prompt structures.

Example template:

Context: [Describe the situation]
Goal: [What you want to achieve]
Constraints: [Any limitations or requirements]
Format: [How you want the output structured]
Tone: [Desired communication style]

### Progressive Refinement
What it is: Starting broad and iteratively narrowing down.
Example sequence:

"Brainstorm marketing ideas for our app"
"Focus on the social media ideas from that list"
"Develop the Instagram strategy in detail"
"Create specific post examples for that Instagram strategy"

### Priming
What it is: preparing the LLM for some type of future prompt
- ex. "I need you to monitor for offensive language. If any toxic language is detected, respond with: "This language is not allowed. Please rephrase your request."
- ex. "I would like you to act as my math tutor. When I give you a problem, give me advice on the next step I should try.  If I ever ask for the answer, say "Sorry, I can't give you an answer"."

### Combining Techniques
The real power comes from combining these approaches:
```
[Role] You are an expert business analyst.
[Chain-of-thought] Think through this step by step:
[Constraint] In exactly 4 bullet points,
[Negative] without using jargon or speculation,
[Few-shot] following this format:
- Problem: [clear description]
- Impact: [quantified effect]
- Solution: [specific action]
- Timeline: [realistic timeframe]
```

Pro tip: Start simple and add complexity gradually. A basic well-structured prompt often works better than an over-engineered one. The key is matching the technique to your specific use case and iterating based on results.

## UE Resources
- https://learnprompting.org/docs/introduction