---
{"dg-publish":true,"permalink":"/0-inbox/chatbot-tunch/"}
---

up:: [[4 Archive/Notes/Tunch\|Tunch]]

Links:

- [[4 Archive/Notes/Large Language Models\|Large Language Models]]
- [[4 Archive/Notes/Prompt Engineering\|Prompt Engineering]]
- [[LLM Agents\|LLM Agents]]
- [[0 Inbox/ChatGPT\|ChatGPT]]
- # Presentation
- Explain Chat API
- completion(message[])
	- { role: "system" | "assistant" | "user", content: string }
- How do we make it do *more*?
	- Improve your prompting
		- [[4 Archive/Notes/Chain of Thought Reasoning\|Chain of Thought Reasoning]]
		- [[4 Archive/Notes/ReAct - Language Models\|ReAct - Language Models]]
		- [[4 Archive/Notes/Reflexion\|Reflexion]]
	- System prompt
	- Message history
	- History: summarizing itself
	- Large-scale knowledge: vector databases
	- Agentic behavior
- Langchain
	- Connecting inputs
	- Connecting tools
	- Custom tool
- LabGPT: Workshop
	- terminal TS file to play around with it
	- Build a cool tool
	- Integrate this with the Slack API so we can use it
- My explorations
	- Auto-code
	- Langstream