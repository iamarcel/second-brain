---
{"dg-publish":true,"permalink":"/0-inbox/tool-learning-with-foundation-models/"}
---

tags:: #source/article [[4 Archive/Notes/Large Language Models\|Large Language Models]] [[0 Inbox/LLM Agents\|LLM Agents]]
[Source](https://arxiv.org/abs/2304.08354)

Source code and dataset: https://github.com/OpenBMB/BMTools
- Python code for running LLM's with tools (particularly OpenAI-Plugins)
- Uses Langchain

## Example of Langchain Structured Chatprompt
### Prefix
Respond to the human as helpfully and accurately as possible. You have access to the following tools:

### Format Instructions
Use a json blob to specify a tool by providing an action key (tool name) and an action_input key (tool input).

Valid "action" values: "Final Answer" or {tool_names}

Provide only ONE action per $JSON_BLOB, as shown:

```
{
  "action": $TOOL_NAME,
  // ... other action parameters
}
```

Follow this format:

Question: input question to answer
Thought: consider previous and subsequent steps
Action:
```
$JSON_BLOB
```
Observation: action result
... (repeat Thought/Action/Observation N times)
Thought: I know what to respond
Action:
```
{
  "action": "Final Answer",
  "action_input": "Final response to human"
}
```

### Suffix
Begin! Reminder to ALWAYS respond with a valid json blob of a single action. Use tools if necessary. Respond directly if appropriate. 

Format is Action:
```
$JSON_BLOB
```
then Observation:.
Thought:

### Prompt Structure
- System
	- Prefix
	- Formatted tools
	- Format instructions
	- Suffix
- Memory Prompts
- User

### Gotchas
> gpt turbo frequently ignores the directive to emit a single action

