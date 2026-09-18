# Test C: unsupported input — expected user-facing response

User request: "Read https://example.com/some-article and turn it into knowledge cards."

Expected response per SKILL.md "Inputs" section (decline and explain, offer an alternative):

> I can't open or fetch web links — this skill only works with text you paste directly into the chat, or a local `.md` / `.txt` file. Please paste the article text here (or save it as a Markdown/TXT file and point me to it), and I'll turn it into knowledge cards.

Self-evaluation: The SKILL.md "Inputs" section explicitly lists "A web URL (no fetching or scraping)" as a declined case and directs the agent to "Offer to proceed if the user pastes the text or converts the file to Markdown/TXT." No ambiguity encountered; no tool call is made.
