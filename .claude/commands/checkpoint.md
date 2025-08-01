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

**Error Handling Protocol:**
If any step has an error or fails, immediately:
1. **Halt progression** - Do not continue to subsequent steps
2. **Status report** - Clearly summarize what was completed successfully
3. **Problem identification** - Explain what specifically failed and why
4. **Present options** - Offer specific, actionable choices:
   - Retry the failed step after addressing the issue
   - Skip the failed step with clear warnings about consequences
   - Modify the approach (alternative method)
   - Abort the checkpoint and preserve current state
   - Request manual user intervention
5. **Wait for explicit user decision** - Never assume or default to any action
6. **Document the resolution** - Note what the user chose for future reference

**Critical Rule:** Git failures in step 6 are considered checkpoint-critical errors. The process must not silently fail or proceed with incomplete commits.

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

6. **Stage and Commit.** Stage and commit recent changes with comprehensive error handling:

   **Pre-staging Verification:**
   - Ensure you're working from the git repository root directory
   - Identify expected changed files from this checkpoint:
     * `.claude/commands/checkpoint.md` (if modified)
     * `CLAUDE.md` (project memory updates)
     * `README.md` (documentation updates)
     * New conversation file from step 2 in `ai_docs/claude conversations/`
   - Check current git status to confirm these files show as modified/untracked

   **Staging Process:**
   - Stage all repository changes using appropriate git command
   - Immediately verify what was actually staged
   - Compare staged files against expected files list
   - **If any expected files are missing from staging:**
     * **STOP the checkpoint process**
     * Report which files were expected vs. actually staged
     * Ask user: "Git staging incomplete. How would you like to proceed?"
       - A) Investigate and retry staging
       - B) Manually stage specific files
       - C) Continue without committing (risky)
       - D) Abort checkpoint entirely

   **Commit Process:**
   - Attempt to commit staged changes with descriptive message
   - **If commit fails for any reason:**
     * **STOP the checkpoint process**
     * Report the specific git error message
     * Ask user: "Git commit failed. How would you like to proceed?"
       - A) Fix the underlying issue and retry commit
       - B) Skip commit step and continue to summary (changes remain staged)
       - C) Unstage changes and abort checkpoint
       - D) Request manual intervention for git operations

   **Success Verification:**
   - Confirm commit was created successfully
   - Verify working directory is clean (no uncommitted changes)
   - **Only proceed to step 7 if:**
     * Commit succeeded completely, OR
     * User explicitly chose to continue despite git issues

   **Failure Recovery:**
   - If step 6 cannot complete successfully, follow the checkpoint command's error protocol:
     * Summarize what steps were completed (1-5)
     * Clearly state that git operations failed
     * Preserve all file changes in their current state
     * Ask user for guidance on how to proceed

7. **Review and Update GitHub Issues.** Analyze recent changes against open GitHub issues for potential resolution:
   - Use WebFetch to retrieve current open issues from the repository
   - Extract repository URL from git remote to construct issue links
   - **IMPORTANT: Only analyze against the open issues returned by WebFetch - ignore any closed/resolved issues**
   - Compare commit messages, changed files, and implemented features against **only the open issue descriptions**
   - Identify potential matches using keyword analysis and logical inference
   - Present findings to user with emoji indicators, confidence levels, and direct links:
     * **HIGH CONFIDENCE MATCHES**
       - ✅ High confidence: Issue #11 'claude conversation file saved with wrong datetime stamp' appears resolved by timestamp implementation  
         → https://github.com/jhershey/claude_agents/issues/11
     * **POSSIBLE MATCHES**  
       - ⚠️ Possible match: Issue #3 'add hooks with apple system sounds' may be completed by .claude/settings.json changes  
         → https://github.com/jhershey/claude_agents/issues/3
     * **UNCLEAR/NEEDS CLARIFICATION**
       - ❌ Unclear: Issue #12 'user yaml errors on agent.md's' needs clearer acceptance criteria  
         → https://github.com/jhershey/claude_agents/issues/12
   - **Manual Resolution:** User clicks links to review and manually close/update issues on GitHub
   - **No Automation:** This step only identifies potential matches for manual action

8. **Summarize Checkpoint.** Make a comprehensive summary of what was accomplished in this checkpoint command execution, including both the traditional checkpoint activities (steps 1-6) and GitHub issue analysis findings (step 7). Call out any particular strengths or opportunities for improvement to this checkpoint command based on the outcome from the current execution. Include key GitHub issue insights and potential resolutions identified for immediate user action.

## Risks

### Performance Scaling (Step 7: GitHub Issue Analysis)
* **WebFetch Latency Growth:** As open issues increase, WebFetch pagination will slow down Step 7 execution
* **Current Performance:** 13 open issues = ~3-5 seconds total
* **Projected Timeline:** 15+ second execution time in ~10-12 months at current issue creation rate (~0.4 issues/day)
* **Bottleneck:** GitHub web interface pagination, not analysis logic
* **Mitigation Options:** Filter by labels, time-based filtering, or upgrade to gh CLI for API access
* **Monitoring:** Watch for 6+ second execution times as early warning (25+ open issues)

## Possible Enhancements
* Cost Tracking:  Use ccusage command to calculate model usage and cost since last checkpoint. Include this information in the checkpoint summary.
* Improvement Metrics:  Create metrics to show improvements since last checkpoint.  Don't know what to measure across checkpoints yet.
* Parallel Processing: Some steps (like CLAUDE.md and README.md updates) could potentially run concurrently
* Change Detection: Could skip steps when no meaningful changes are needed
* Progress Persistence: Could handle interruption/resume scenarios
* Directory Migration Detection: Could automatically detect and handle directory structure changes
* Quality Metrics: Add quantitative measurement of documentation improvements
* Validation Automation: Could include automated link checking and path validation
* Template Integration: Standardized templates for conversation summaries
* GitHub CLI Integration: Install gh CLI for automated issue closing/commenting with commit traceability. Would enable: `gh issue close #11 --comment "Fixed by commit [hash]: implemented accurate timestamps"`. Currently deferred - WebFetch approach provides identification without additional dependencies.