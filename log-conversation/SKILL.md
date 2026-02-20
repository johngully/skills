---
name: log-conversation
description: Logs and summarizes a conversation to a markdown document. Use when the user asks to log a conversation or provides a conversation transcript.
---

# Log Conversation

Log and summarize a conversation transcript into a structured markdown note.

## When to use this skill

- The user requests a conversation to be logged
- The user provides a conversation transcript

## Transcript
- Review the transcript, break up the conversation into chunks by speaker
- Do not modify or summarize the transcript, other than spelling or grammatical corrections.
- Prepend the speaker prior to the chunk
```
${speaker_name}: ${transcript_chunk}
```
- After reviewing and chunking the transcript, make another pass ensuring that the speaker names are correct
- Avoid creating new names when similar-sounding names may refer to the same person

## Conversation frontmatter

- Review the transcript and identify the primary attendee of the meeting
- If the primary attendee is unclear, ask the user for clarification
- Populate additional frontmatter fields only when the transcript clearly provides them

## Conversation summarization

### Summary

- Write a brief single-paragraph summary of the meeting
- Keep it to 2-5 sentences

### Attendees

- Create a unique list of all participants by name from the transcript
- The primary attendee should not be the person logging the conversation
- If it is unclear who is the primary attendee, request clarification


### Action Items

- Extract only explicit action items from the transcript
- Do not infer or invent action items
- Keep the list brief

## File and format

1. Name the file with this exact pattern:
   - `yyyy-mm-dd hhmm - ${person}.md`
2. Use the conversation template below.

## Conversation template

```markdown
---
when: ${timestamp}
name: ${person}
# Optional
title: ${title}
email: ${email_address}
phone: ${phone_number}
web: ${website_url}
linkedin: ${linkedin_url}
---

# Summary
${summary}

## Action Items
- ${action_item_n}

## Attendees
- ${attendee_n}

# Meeting transcript
${transcript}
```
