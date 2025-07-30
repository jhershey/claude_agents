# Checkpoint Project Status

Follow the following process to create a project checkpoint.

## Process
Create a todo list for these steps and mark them off as accomplished. Do these steps in order. If a step has an error or fails, summarize what has been completed and ask the user how to proceed with specific options for resolution.

1. **Compact Claude Conversation Context.** Use Claude Code's conversation compacting feature on current conversation, paying particular attention to maintain clarity, consistency, and meaning of the current conversation.

2. **Save Claude Conversation Context.** Save the conversation and context to a markdown file in './ai docs/claude conversations'. Create the directory if it doesn't exist. Use a descriptive file name based on the main topic discussed since the last checkpoint, followed by a datetime stamp (format: YYYY-MM-DD_HHMM).

3. **Update Claude Memory.** Update CLAUDE.md to reflect current project snapshot state.

4. **Update README.md.** Update README.md to reflect current project description. Remember this file is optimized for the senior AI engineer who's looking to quickly onboard and understand the project. Be concise, clear, complete, and accurate.  Check for internal logical and semantic consistency.  Include information text, document links, web references, and pictures/diagrams (png format). Ask the user to resolve any ambiguity. When merging with existing README.md content: preserve valuable information that remains relevant, retain next steps and insights unless obviated by recent changes, and maintain consistent formatting throughout. Use critic and refiner agents to achieve at least B+ quality.

5. **Stage and Commit.** On success, stage and commit recent changes with an appropriate message.

6. **Summarize Checkpoint.** Make a concise summary of what was done for this particular checkpoint command execution. Call out any particular strengths or opportunities for improvement to this checkpoint command based on the outcome from the current execution.


## Future Enhancements
* **Cost Tracking.** Use ccusage command to calculate model usage and cost since last checkpoint. Include this information in the checkpoint summary.