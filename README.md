# Alexa — AI Project Management Assistant (Telegram Bot)

An n8n-based AI agent that manages email, calendar, and a full Airtable database through Telegram. Built as part of the **Digital Egypt Pioneers Initiative (DEPI)**, with Alaa EL-Dewer.

## Description

Alexa started as a simple email and calendar assistant and has been upgraded into a **project management bot** running on **Telegram**. Powered by **Google Gemini** and orchestrated through **n8n**, Alexa now manages a real Airtable database for a training program, covering courses, instructors, resources, and the relationships between them, while retaining its original email, calendar, and web search capabilities.

Users interact with Alexa directly through Telegram. Messages are processed by the AI Agent node, backed by a chat model and short-term conversation memory, which decides which of the connected tools to use.

## How It Works

1. **Trigger** — A Telegram message starts the workflow.
2. **Agent (Alexa)** — The AI Agent node interprets the request and decides which tool(s) to call, following its defined behavior rules.
3. **Chat Model** — Google Gemini powers reasoning and responses.
4. **Memory** — A buffer-window memory module (scoped per Telegram chat) retains context across the conversation.
5. **Tools** — The agent calls one or more of the following as needed.
6. **Response** — The output is sent back to the user as a Telegram message.

## Tools & Capabilities

### Email & Calendar
| Tool | Purpose |
|---|---|
| Gmail – Get All | Search and retrieve emails |
| Gmail – Send Message | Send emails on the user's behalf |
| Google Calendar – Get All | Check existing events |
| Google Calendar – Create Event | Schedule new events, with conflict checking |
| SerpAPI | Web search for information not found in email/calendar |

### Airtable Database (AMIT Data Science Training)
| Action | Tables |
|---|---|
| Get records | Courses, Instructors, Resources, Course-Instructor-Resource Links |
| Create / Update (upsert) | Courses, Instructors, Resources, Course-Instructor-Resource Links |
| Delete records | Courses, Instructors, Resources, Course-Instructor-Resource Links |

## Core Behavior Rules

- **Never guesses.** Ambiguous requests (e.g., meeting times) are clarified before any action is taken.
- **Draft vs. Send.** Emails are only drafted unless the user explicitly asks to send them.
- **Conflict checking.** The calendar is always checked before creating an event.
- **Careful database handling.** Create/update operations use explicit matching fields (e.g., Name, Course Name) to avoid duplicate records. Deletions only happen on clear, explicit request.
- **No fabrication.** The agent never invents emails, events, dates, recipients, or database records, and never claims an action succeeded unless the corresponding tool call actually completed.
- **Clarify, don't assume.** Missing information is always requested from the user instead of guessed.

## Tech Stack

n8n | Google Gemini | Telegram | Airtable | Gmail API | Google Calendar API | SerpAPI

## Use Cases

- "Check my emails from John this week."
- "Do I have any meetings tomorrow?"
- "Add a new instructor named Sarah with expertise in Machine Learning."
- "Update the Python course description."
- "Delete the old 'Intro to Excel' course record."
- "Who's assigned to teach the Data Visualization course?"

## Notes

The real challenge in this upgrade wasn't adding more tools, it was making sure the agent handles the database responsibly: avoiding duplicate records, preventing accidental deletions, and still asking for clarification whenever a request is ambiguous. As the toolset grows, the judgment layer (the system prompt) becomes more important, not less.

---
Built as part of DEPI (Digital Egypt Pioneers Initiative), with Alaa EL-Dewer.
