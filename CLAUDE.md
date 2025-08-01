# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains a sophisticated multi-agent orchestration system with 11 specialized Claude Code subagents. The system is designed to handle complex tasks through intelligent agent coordination while preventing infinite loops and excessive processing cycles.

## Agent System Architecture

The core system consists of specialized agents in `.claude/agents/` with embedded orchestration awareness:

**Pipeline Agents (Sequential Flow):**
- `clarifier-agent.md` - Transforms vague requests into precise specifications (max 3 iterations)
- `planner-agent.md` - Breaks down clarified goals into structured plans (max 2 iterations)  
- `generator-agent.md` - Implements plans into concrete outputs (max 2 iterations)
- `critic-agent.md` - Reviews outputs for quality and correctness (calibrated to pipeline length)
- `refiner-agent.md` - Applies targeted fixes based on critic feedback (max 3 refinements)
- `tester-agent.md` - Validates implementations through testing (depth scales with pipeline investment)

**Specialized Agents (Variable Position):**
- `promptcoach-agent.md` - Teaches effective prompt engineering (max 3 sessions)
- `promptrefiner-agent.md` - Improves prompts automatically (max 2 refinements, 80% threshold)
- `blender-agent.md` - Synthesizes multiple outputs (only if >20% improvement over best input)
- `retrospectivecritic-agent.md` - Analyzes prompt effectiveness (max 2 analyses)
- `meta-agent.md` - Generates new subagent configurations

## Orchestration Safeguards

Each agent includes embedded orchestration awareness with:

**Loop Prevention:**
- Hard iteration limits (2-3 per agent type)
- Diminishing returns detection (typically 10-20% improvement thresholds)
- Pipeline length monitoring (warnings at 8+ agents)
- User decision points at natural stopping places

**Efficiency Controls:**
- Quality vs effort calibration based on pipeline investment
- "Accept current state" options throughout
- Automatic escalation when thresholds exceeded
- Alternative pathway recommendations

## Testing System

**Test Documentation Location:** `ai_docs/tests/`
- `orchestration_safeguards_test_plan.md` - Comprehensive test scenarios
- `orchestration_safeguards_test_execution_*.md` - Results with timestamps

**Critical Test Scenarios:**
- Clarifier ↔ Planner loop prevention
- Critic → Refiner → Critic cycle detection
- Pipeline runaway prevention (>10 agents)
- Diminishing returns detection
- User decision point validation

## Agent Usage Patterns

**Individual Agent Use:** Each agent functions independently for focused tasks
**Pipeline Orchestration:** Agents coordinate automatically through embedded awareness
**Loop Detection:** System prevents infinite cycles and excessive refinement
**User Control:** Decision points preserve user agency throughout complex workflows

## Custom Commands

**Available Commands:**
- `checkpoint.md` - Automated project checkpoint workflow with 7-step process including conversation saving, retention policy, documentation updates, and git commits

## Key Design Principles

- **Embedded Orchestration:** Safeguards built into each agent, no separate framework needed
- **Quality-Effort Balance:** Agents calibrate depth to overall pipeline investment  
- **User Agency:** Always provide "good enough" and alternative options
- **Fail-Safe Defaults:** Hard limits prevent runaway processing
- **Validated Safeguards:** 100% test success rate on critical loop prevention scenarios

## Current Project Status

**Documentation:** Professional-grade README.md and CLAUDE.md suitable for senior AI engineers  
**Agent System:** 11 optimized agents with embedded orchestration awareness  
**Testing:** Comprehensive safeguard validation with documented test results  
**Commands:** Production-ready checkpoint workflow with comprehensive error handling and retention policy  
**Quality Assurance:** Critic-refiner pipeline proven effective for iterative improvement  
**Directory Structure:** Fully consistent ai_docs/ naming throughout project files  
**Integration:** Full Claude Code command system compatibility with YAML frontmatter  
**Conversation Management:** Automated retention policy prevents unlimited file growth  
**Error Handling:** Comprehensive git operation safeguards prevent silent failures