# Advanced Prompt Engineering Guide

This guide explores five advanced prompt engineering techniques designed to enhance the reasoning, reliability, and problem-solving capabilities of Large Language Models (LLMs).

---

## 1. Chain-of-Thought (CoT) Prompting

### How it Works
**Chain-of-Thought (CoT)** prompting encourages the model to generate intermediate reasoning steps before arriving at a final answer. By breaking down a complex problem into a logical sequence, the model can handle multi-step tasks more accurately. This can be achieved through **Few-shot CoT** (providing examples with reasoning) or **Zero-shot CoT** (using the phrase "Let's think step by step").

### Example
**Prompt:**
> I have 12 apples. I give 4 to a friend and buy 6 more. Then I eat 2. How many apples do I have left? Let's think step by step.

**Output:**
> 1. You started with 12 apples.
> 2. You gave 4 away, so 12 - 4 = 8 apples remain.
> 3. You bought 6 more, so 8 + 6 = 14 apples.
> 4. You ate 2, so 14 - 2 = 12 apples.
> **Final Answer:** 12 apples.

---

## 2. Tree of Thoughts (ToT)

### How it Works
**Tree of Thoughts (ToT)** generalizes Chain-of-Thought by allowing the model to explore multiple reasoning paths simultaneously. It treats the problem-solving process as a search over a tree, where each node is a "thought" (an intermediate step). The model can generate several potential next steps, evaluate them, and then decide whether to proceed, backtrack, or look ahead, similar to how a human might solve a complex puzzle or a game of chess.

### Example
**Prompt:**
> Imagine three different experts are solving this math puzzle. Each expert will write down one step of their reasoning and share it. If an expert realizes their path is incorrect, they will backtrack.
> **Puzzle:** Use the numbers 4, 8, 9, and 2 to reach exactly 24 using basic operators (+, -, *, /).

**Output:**
> **Expert 1:** Try (9 - 4) * 8 / 2. (9-4)=5, 5*8=40, 40/2=20. (Incorrect, 20 != 24).
> **Expert 2:** Try 9 + 8 + 4 + 2. 9+8=17, 17+4=21, 21+2=23. (Incorrect, 23 != 24).
> **Expert 3:** Try (9 + 2) + (8 + 4). 11 + 12 = 23. (Incorrect).
> **Expert 1 (Backtracking):** Let's try 4 * (9 - 2) + 8. 9-2=7, 4*7=28, 28+8=36. (Incorrect).
> **Expert 2 (Backtracking):** Let's try 8 * (9 - 4 - 2). 9-4-2=3, 8*3=24. (Correct!)
> **Final Answer:** 8 * (9 - 4 - 2) = 24.

---

## 3. Self-Consistency

### How it Works
**Self-Consistency** is an extension of CoT that addresses the issue of "greedy decoding" (where the model picks only the most likely next token). Instead of generating a single reasoning path, the model generates multiple diverse reasoning paths for the same prompt. The final answer is determined by a "majority vote" among the different paths. This significantly improves performance on arithmetic and symbolic reasoning tasks where a single error in a chain can derail the entire result.

### Example
**Prompt:**
> Q: If I have 3 shirts and 2 pants, how many unique outfits can I make?
> (The model is instructed to generate 3 different reasoning paths)

**Output Path 1:** An outfit consists of 1 shirt and 1 pant. So, 3 * 2 = 6. Answer: 6.
**Output Path 2:** For each of the 3 shirts, I can wear 2 different pants. 2 + 2 + 2 = 6. Answer: 6.
**Output Path 3:** I have 3 options for the top and 2 for the bottom. Total = 3 + 2 = 5. Answer: 5.
**Final Result:** Since "6" appeared in the majority of paths, the final answer is 6.

---

## 4. ReAct (Reasoning + Acting)

### How it Works
**ReAct** combines reasoning traces with task-specific actions. The model generates a "Thought" to explain its reasoning, followed by an "Action" (like searching a database or using a tool), and then receives an "Observation" from that action. This cycle repeats until the task is complete. This allows the model to interact with the real world, retrieve up-to-date information, and correct its reasoning based on external facts.

### Example
**Prompt:**
> Who is the current CEO of the company that acquired Slack?

**Output:**
> **Thought 1:** I need to find out which company acquired Slack.
> **Action 1:** Search[company that acquired Slack]
> **Observation 1:** Salesforce acquired Slack in 2021.
> **Thought 2:** Now I need to find the current CEO of Salesforce.
> **Action 2:** Search[current CEO of Salesforce]
> **Observation 2:** Marc Benioff is the Chair and CEO of Salesforce.
> **Thought 3:** I have the answer.
> **Final Answer:** Marc Benioff.

---

## 5. Multi-Agent Prompting

### How it Works
**Multi-Agent Prompting** involves simulating a conversation or collaboration between multiple distinct AI "personas" or "agents," each with a specific role or expertise. One agent might act as a "Writer," another as a "Critic," and a third as a "Manager." By having these agents interact—critiquing each other's work or contributing specialized knowledge—the final output is often more refined, accurate, and comprehensive than what a single prompt could produce.

### Example
**Prompt:**
> Create a marketing plan for a new eco-friendly water bottle. 
> **Agent 1 (Strategist):** Propose the target audience and core message.
> **Agent 2 (Creative):** Design three catchy slogans based on the strategy.
> **Agent 3 (Critic):** Review the slogans for potential negative connotations or cliches.

**Output:**
> **Strategist:** Target audience is Gen Z outdoor enthusiasts. Core message: "Hydration that heals the planet."
> **Creative:** 1. "Drink Green, Stay Clean." 2. "The Last Bottle You'll Ever Buy." 3. "Earth's Best Friend."
> **Critic:** Slogan 1 is a bit cliché. Slogan 2 might imply the product breaks easily. Slogan 3 is strong but generic. I recommend focusing on the "healing" aspect from the strategist.
> **Final Refined Slogan:** "Heal the Earth, One Sip at a Time."
