---
{"dg-publish":true,"permalink":"/0-inbox/re-act-language-models/"}
---

[[0 Inbox/GPT\|GPT]]
[[3 Resources/AI\|AI]]

ReAct enables language models to generate both verbal reasoning traces and text actions in an interleaved manner.

![graph](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_0lCKXSvFq4yyjM5PGdM27OF6LWco9qFGQS1dwa3DtEF8AnAuXg9Q_nPDVyAArYwl9sGsB000-iuKJuSsNjo--fi1ZCJbrj-KwsZ6M569nWg-h2xRGHkdvQobUY9RiIr4MYkathIFyiAHZSnHAwVUfeijU-tCLyaHRgqXQah1XObtE71a00IbGdywVw/s16000/image1.png)

Example in [[Langchain\|Langchain]]

```
ALWAYS use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action:
\`\`\`
$JSON_BLOB
\`\`\`
Observation: the result of the action
... (this Thought/Action/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question"""
```