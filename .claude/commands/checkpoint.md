---
name: checkpoint
description: Create a comprehensive project checkpoint with conversation archiving and documentation updates
version: 1.0
type: command
max_iterations: 1
orchestration_limits:
  critic_refiner_cycles: 2
  total_agents_per_execution: 8
  auto_accept_threshold: 3
---

# Checkpoint Project Status

Follow the following process to create a project checkpoint.

## Process
Create a todo list for these steps and mark them off as accomplished. Do these steps in order. If a step has an error or fails, summarize what has been completed and ask the user how to proceed with specific options for resolution. You can skip a step when no meaningful changes are needed.

**Orchestration Safeguards:** This command includes automatic loop prevention for agent chaining. Critic-refiner cycles are limited to 2 iterations maximum. If quality targets aren't met within these limits, the command will present options to either accept current quality or manually intervene.

1. **Compact Claude Conversation Context.** Use Claude Code's conversation compacting feature on current conversation, paying particular attention to maintain clarity, consistency, and meaning of the current conversation.

2. **Save Claude Conversation Context File.** Save the conversation and context to a markdown file in './ai_docs/claude conversations'. Create the directory if it doesn't exist. Use a descriptive file name based on the main topic discussed since the last checkpoint, followed by the current datetime stamp. Use the bash command `date +"%Y-%m-%d_%H%M"` to get the accurate timestamp.

3. **Apply Claude Conversation Context File Retention Policy.** Manage conversation file storage to prevent unlimited growth:
   - List all conversation files in './ai_docs/claude conversations' (excluding digest files)
   - Identify files to retain: Keep all files from the last 7 days OR the 5 most recent files, whichever is larger
   - For files beyond retention period: Append their content to a monthly digest file (`conversation_digest_YYYY-MM.md`)
   - Delete the original files after successful digest append
   - Log retention actions: "Retained X files, archived Y files to digest"

4. **Update Claude Memory.** Update CLAUDE.md to reflect current project snapshot state.

5. **Update README.md.** Update README.md to reflect current project description. Remember this file is optimized for the senior AI engineer who's looking to quickly onboard and understand the project. Be concise, clear, complete, and accurate. Check for internal logical and semantic consistency. Include information text, document links, web references, and pictures/diagrams (png format). Check for broken links. Ask the user to resolve any ambiguity. When merging with existing README.md content: preserve valuable information that remains relevant, retain next steps and insights unless obviated by recent changes, and maintain consistent formatting throughout. Use critic and refiner agents to achieve at least B+ quality, with automatic acceptance if refinement cycles exceed limit.

6. **Stage and Commit.** On success, stage and commit recent changes with an appropriate message.

7. **Summarize Checkpoint.** Make a concise summary of what was done for this particular checkpoint command execution. Call out any particular strengths or opportunities for improvement to this checkpoint command based on the outcome from the current execution.

## Future Enhancements
* Cost Tracking:  Use ccusage command to calculate model usage and cost since last checkpoint. Include this information in the checkpoint summary.
* Improvement Metrics:  Create metrics to show improvements since last checkpoint.  Don't know what to measure across checkpoints yet.
* Parallel Processing: Some steps (like CLAUDE.md and README.md updates) could potentially run concurrently
* Change Detection: Could skip steps when no meaningful changes are needed
* Progress Persistence: Could handle interruption/resume scenarios
* Directory Migration Detection: Could automatically detect and handle directory structure changes
* Quality Metrics: Add quantitative measurement of documentation improvements
* Validation Automation: Could include automated link checking and path validation
* Template Integration: Standardized templates for conversation summaries