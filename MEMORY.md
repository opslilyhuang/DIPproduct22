# MEMORY.md - Long-Term Memory

This file serves as your long-term memory. Use it to record significant events, learnings, preferences, and context that should persist across sessions.

## Structure

### Personal Context
- **User preferences**: Things the user likes/dislikes, their working style, communication preferences
- **Important relationships**: Key people, roles, and relationships in the user's life/work
- **Ongoing projects**: Current projects, their status, and next steps
- **Recurring tasks**: Regular activities, schedules, and routines

### Learning & Improvement
- **Mistakes made**: Errors to avoid repeating, with context and solutions
- **Successful patterns**: Approaches that worked well, with details
- **User feedback**: Direct feedback, corrections, and preferences
- **Skill improvements**: New capabilities learned or enhanced

### System Knowledge
- **Environment details**: System configurations, tools, dependencies
- **Workflow optimizations**: Efficient ways to accomplish common tasks
- **Integration patterns**: How different tools and systems work together
- **Troubleshooting**: Known issues and their solutions

## Usage Guidelines

1. **Be concise but complete**: Include enough context to understand later
2. **Timestamp entries**: Note when things happened or were learned
3. **Prioritize**: Focus on what's most likely to be useful later
4. **Review regularly**: Periodically clean up outdated information
5. **Connect related items**: Use cross-references when helpful

## Recent Updates

### 2026-03-07: Skills Installation & Configuration
- **10 Essential Skills**: Extracted from "OpenClaw避坑指南" WeChat article
- **Successfully Installed (via skillhub)**:
  - Clawsec: HTTP/HTTPS proxy monitor for security auditing
  - Multi Search Engine: 17 search engines (8 CN + 9 Global) for web crawling
  - Self-Improving Agent: Automated learning recording and improvement
  - Proactive Agent: Anticipates needs and provides proactive service
  - Find-Skills: Already installed, for discovering new skills
  - GitHub: Already installed, for repository management
  - Tavily Search: Already installed, for AI-optimized web search
- **Installation Failed**: Ontology, Office-Automation (404 errors in skillhub)
- **Not Yet Attempted**: Systematic-Debugging (expected to fail via skillhub)

### 2026-03-07: Self-Improving Agent Configuration
- Created `.learnings/` directory with three log files:
  - LEARNINGS.md: Records successful patterns and learnings
  - ERRORS.md: Tracks errors, root causes, and solutions
  - FEATURE_REQUESTS.md: Captures requested functionality
- Initial entries added for the skills installation process

### 2026-03-07: Proactive Agent Configuration
- Enhanced HEARTBEAT.md with comprehensive periodic checks
- Created `memory/heartbeat-state.json` to track check timestamps
- System now configured for:
  - System health & security monitoring
  - Information monitoring (email, calendar, weather, GitHub)
  - Memory maintenance (daily notes review, MEMORY.md updates)
  - Proactive work (tasks without asking, file organization)
- Respects quiet hours (23:00-08:00) and busy signals

### 2026-03-07: WebTwin Status & Heartbeat Checks
#### First Heartbeat Check (20:01)
- WebTwin service (gentle-meadow) is not running (PID 745850 terminated). Need to verify if service should be restarted.
- System health check completed.
- Background processes monitored; no failures detected.
- Memory maintenance: daily notes reviewed, MEMORY.md updated.
- Apple China website capture terminated (wild-kelp session killed).

#### Second Heartbeat Check (21:31)
- WebTwin service still not running. Service may need restart if user requires website analysis.
- Nighttime check: Only critical security/error checks performed.
- No failed background processes detected in logs.
- Clawsec security audit not available (skill not installed or not in PATH).
- Recent skill installations verification: Clawsec not found in extensions, may need reinstallation.
- Memory maintenance: Daily notes reviewed, MEMORY.md updated with second check.
- Proactive work: Git commits and pushes performed during first check.
- Quiet hours (23:00-08:00) approaching; will reduce checks to essential only.

---

*Last updated: 2026-03-07*