---
{"dg-publish":true,"permalink":"/0-inbox/timestamp-tunch-brainstorm/"}
---

tags:: #output/lecture [[on/programming\|on/programming]]
up:: [[4 Archive/Notes/Tunch\|Tunch]]
[[4 Archive/Notes/Timestamp - Tunch\|Timestamp - Tunch]]

# Brainstorm
[[4 Archive/Notes/The Hero's Journey\|The Hero's Journey]]
- Finding some weird things in how times are shown
- Call: tickets with wrong dates
- Realization: Your day depends on your hour (and month and year etc)
- Weird shit from [[4 Archive/Notes/Timezones\|Timezones]] - road of trials
- Deepest challenge: ceci n'est pas une timestamp?
- Offset calculations are wrong, use zones (DST)
- Java LocalDate calculations: show how they might be wrong (DST, local vs utc)
- Reborn: I understand it's hard. I understand there are APIs for it and I should use the one that most closely resembles the semantics of what I want to do. KISS, don't reinvent the wheel because you'll be wrong.
- A language is not enough
- Locales
	- JavaScript: Angular locales
	- JavaScript: Intl object
	- Java: Date, LocalDate, Instant, OffsetDateTime, ZonedDateTime
[[4 Archive/Notes/Timezones\|Timezones]]
[[0 Inbox/Internationalization\|Internationalization]]

- The Ordinary World
	- Happily coding
	- Adding up nanoseconds
	- Something feels odd
- The Call to Adventure
	- Saving a date and it ends up changing
- Refusal of the Call
	- No I don't care about the time
	- But then I do need to save time and it changes
	- Fake Meeting with the Mentor: Tom Scott's Warning
- Meeting with the Mentor
	- Meme: before / after understanding timezones
	- Yoda is our Mentor
- Crossing the First Threshold
	- Okay let's do this shit
	- EVERYTHING UTC
	- Shit it's still wrong
- Tests, Allies and Enemies
	- Examples of weird stuff [[4 Archive/Notes/Timezones\|Timezones]]
- Approach to the Innermost Cave
	- Ceci n'est pas une Timestamp: why?
- The Ordeal
	- How do we fix this? It looks like all is lost.
	- Yoda: Use the built-in classes, Luke.
- Actual explanation
- The Road Back: JavaScript
- Return with the Elixir