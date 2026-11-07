# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**AI_HELPear** is a multimedia viewer application designed to provide visual and tactile (vibration) feedback for deaf and hard-of-hearing users, as well as for users in silent environments (public transit, classrooms, etc.).

**Core Problem:** Audio-based information (warnings, sound effects, voice cues, alerts) is inaccessible to deaf/hard-of-hearing users, causing them to miss critical information during video lectures, streaming content, and public announcements.

**Solution:** Real-time conversion of audio signals into visual (color, waveform visualization) and tactile (vibration patterns) feedback through a responsive web interface.

## Project Deliverables & Evaluation Criteria

The coursework requires:

1. **Functional Implementation:** A working web service/website that implements the planned features (not just static mockups)
2. **Alignment with Planning:** Implementation must match the documented requirements from:
   - `03_프로젝트제안서_223628_조우성.md` - Project scope, goals, and success metrics
   - `04_사용자연구계획_223628_조우성.md` - User research findings and personas
   - `06_작업분석어피니티_223628_조우성.md` - Task analysis and user workflows
3. **UI/UX Quality:** Polish and completeness of the user interface and design
4. **Prompt Documentation:** Complete record of all Claude Code prompts used (initial, iterations, bug fixes, improvements)

## Key Feature Requirements

From the project proposal, the implementation must include:

- **Audio Visualization:** Display of audio waveforms/frequency patterns in real-time
- **Alert Icons & Color Coding:** Visual indicators for different audio event types (warnings, notifications, speech)
- **Vibration Pattern Settings:** Customizable haptic feedback patterns (when applicable)
- **Media Player Interface:** Core viewer for multimedia content with integrated feedback visualization
- **Accessibility-First Design:** High contrast, clear typography, intuitive interactions (WCAG guidelines)
- **Sample Data Simulation:** Prototyping with simulated audio data rather than real-time audio processing

**Explicitly Out of Scope:**
- Real-time audio stream processing algorithms
- Speech-to-text (STT) engine implementation
- OS-level hardware vibration driver control (simulate with CSS animations)

## Development Guidelines

### Accessibility & Inclusive Design

- **Contrast & Readability:** Ensure sufficient color contrast (WCAG AA minimum) for all UI elements
- **Clear Feedback:** Provide immediate, unambiguous visual feedback for all user interactions
- **Icon & Color Usage:** Use combinations of icons + text, not color alone
- **Testing:** Manually verify that critical workflows work without sound

### User-Centered Development

- Development decisions should prioritize **how users perceive and interact** with the interface, not just technical implementation details
- When adding features or fixing issues, consider: "How does this affect the deaf/hard-of-hearing user experience?"
- Test interactive features in the running application—visual behavior and responsiveness matter as much as code correctness

### Technology Stack

This project should be implemented as a **web application** (HTML/CSS/JavaScript or a modern framework like React). Choose technologies that support:
- Responsive design for mobile/tablet viewing
- Smooth animations and visual feedback
- Easy prototyping and iteration

### How to Build, Run & Test

Add specific build, run, and test commands as the implementation develops. Example structure:

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Run tests
npm test

# Run a specific test
npm test -- <test-file>
```

Once implementation begins, update this section with the actual commands.

## Project Documentation Structure

```
.
├── CLAUDE.md                                    # This file
├── README.md                                    # Assignment requirements
├── Prompt_record.md                             # All Claude Code prompts (TO BE FILLED)
├── 01_인터랙션분석_*.md                         # Prior work: interaction analysis
├── 02_UI디자인방법론_*.md                       # Prior work: design methodology
├── 03_프로젝트제안서_*.md                       # Project scope & goals
├── 04_사용자연구계획_*.md                       # User research & personas
├── 05_맥락질의_*.md                             # Contextual inquiry results
├── 06_작업분석어피니티_*.md                     # Task analysis findings
└── [implementation code]                        # Web app source code (to be added)
```

## Important Context

- **6-Week Timeline:** The project plan spans 6 weeks (Weeks 1–6). Implementation is the primary focus of Weeks 5–6, but code should be ready for testing and iteration.
- **Success Metrics:** 
  - 100% recognition rate of critical audio events during user testing
  - 1.5-second response time to alerts (from event occurrence)
  - All major features from the proposal working without errors
- **User Testing:** The implementation will be tested with actual users; code stability and intuitive interactions are critical.

## Before Implementing

When starting implementation, ensure you have reviewed:

1. The project proposal (`03_프로젝트제안서_*.md`) for feature scope and SMART goals
2. User research findings (`04_사용자연구계획_*.md`) to understand personas and key workflows
3. Task analysis (`06_작업분석어피니티_*.md`) for primary use cases and pain points

This context should inform all feature prioritization and design decisions.

## Prompt Recording

Each time you develop, fix, or enhance the application, add the following to `Prompt_record.md`:

```markdown
### [Date] — [Feature/Fix Title]

**Prompt:**
[Exact Claude Code prompt used]

**Changes:**
[Brief description of what was implemented/fixed]

**Result:**
[Success/notes for future iteration]
```

This is a required deliverable for the coursework.
