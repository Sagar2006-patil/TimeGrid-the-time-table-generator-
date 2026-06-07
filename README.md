# 📘 Project Logbook

## Project Title: Academic Timetable Generator — TimeGrid (Institute Edition)

---

## Index

| Sr.No | Week No | Contents | Date |
|-------|---------|----------|------|
| 1 | Week 01 | Project Group Formation | — |
| 2 | Week 02 | Project Topic Finalization | — |
| 3 | Week 03 | Requirement Analysis | — |
| 4 | Week 04 | UI Design & Layout Planning | — |
| 5 | Week 05 | Implementation Phase – I (Timing & Breaks Module) | — |
| 6 | Week 06 | Implementation Phase – II (Department & Subject Module) | — |
| 7 | Week 07 | Implementation Phase – III (Scheduling Algorithm) | — |
| 8 | Week 08 | Implementation Phase – IV (Practical Rotation Logic) | — |
| 9 | Week 09 | Implementation Phase – V (Timetable Rendering & PDF Export) | — |
| 10 | Week 10 | System Testing | — |
| 11 | Week 11 | Results & Analysis | — |
| 12 | Week 12 | Final Report & Conclusion | — |

---

## Week 01: Project Group Formation

**Date:** —

- Formed the project group and assigned roles:
  - Frontend design & layout
  - Scheduling logic & algorithm
  - PDF generation & export
  - Documentation

- Brainstormed real-world problems faced by college departments in scheduling timetables manually.

### Guide Interaction

Guide suggested choosing a problem that directly solves an administrative pain point in academic institutes.

### Next Plan

Finalize project topic.

---

## Week 02: Project Topic Finalization

**Date:** —

- Explored domains:
  - Academic scheduling
  - Automated timetable generation
  - Browser-based tools for institutes

- Identified that most timetable tools require heavy software or paid licenses.

- Finalized project:

### Academic Timetable Generator — TimeGrid (Institute Edition)

### Guide Interaction

Guide suggested making it fully browser-based with no backend requirement, so any staff member can use it directly.

### Next Plan

Requirement analysis.

---

## Week 03: Requirement Analysis

**Date:** —

- Defined system inputs:
  - Lecture start/end time and slot duration
  - Practical start/end time and slot duration
  - Break names, start times, durations, and colors
  - Working days (Mon–Sun toggleable)
  - Department name, class, semester, room, W.E.F. date
  - Theory subjects (code, name, teacher, periods/week, color, room)
  - Practical subjects (code, teacher, batches/groups, sessions/week)

- Defined system outputs:
  - Auto-generated weekly timetable per department
  - Downloadable PDF per department or all departments combined

- Studied existing tools and identified limitations:
  - Manual Excel-based timetables with no automation
  - Paid software with no offline support
  - No support for rotating practical batches

### Guide Interaction

Guide suggested focusing on rotating practicals and conflict-free scheduling as key differentiators.

### Next Plan

UI design and layout planning.

---

## Week 04: UI Design & Layout Planning

**Date:** —

- Designed a 3-step wizard layout:
  - Step 1: Global Timing Setup (lectures + practicals + breaks + working days)
  - Step 2: Departments configuration (subjects, practicals, smart options)
  - Step 3: Preview and PDF download

- Designed system components:
  - Top navigation bar with step indicators
  - Card-based form sections
  - Color-coded day pills for day selection
  - Department tab system for multi-department support
  - Timetable preview table with color-coded cells

- Defined CSS variable system for consistent theming:
  - `--bg`, `--white`, `--border`, `--dark`, `--muted`, `--accent`

### Guide Interaction

Guide suggested keeping the UI clean and form-based to minimise errors during data entry.

### Next Plan

Start implementation — Timing & Breaks module.

---

## Week 05: Implementation Phase – I (Timing & Breaks Module)

**Date:** —

- Implemented `addBreak()` function for dynamically adding lecture and practical breaks with:
  - Name, start time, duration, and color picker
  - Delete button per row

- Implemented `collectBreaks()` to read all break rows into structured objects.

- Implemented `buildSlots()` — core slot builder that:
  - Iterates from day start to day end
  - Inserts breaks at their exact start times
  - Splits periods when a break falls mid-slot
  - Returns an ordered array of period and break objects

- Implemented `t2m()` (time-to-minutes), `m2t()` (minutes-to-time), and `fmtT()` (12-hour format) helpers.

- Pre-loaded default breaks:
  - Lunch Break at 11:00 AM (50 min)
  - Short Recess at 1:30 PM (15 min)

### Guide Interaction

Guide suggested handling edge cases where a break's start time falls mid-period, splitting it automatically rather than skipping.

### Next Plan

Department and subject management module.

---

## Week 06: Implementation Phase – II (Department & Subject Module)

**Date:** —

- Implemented `addDepartment()` with a tab-based multi-department UI.

- Built `buildDeptPanel()` generating:
  - Institution details form (name, dept, semester, year, class, room, W.E.F. date)
  - Smart options (no consecutive same subject, spread evenly, zero hour, Monday first, Saturday last)
  - Theory subjects table with add/delete rows
  - Practicals table with batch/group configuration

- Implemented `addSubjectToPanel()` and `addPracticalToPanel()` for dynamic row insertion.

- Implemented `collectSubjectsForPanel()` and `collectPracticalsForPanel()` for reading form data.

- Pre-loaded sample data for first department:
  - Subjects: DS, ML-I, SDS, OE, EFM, PBC
  - Practicals: DS, ML-I, SDS with batches S1, S2, S3

### Guide Interaction

Guide suggested pre-filling sample data so users immediately understand the expected format without reading documentation.

### Next Plan

Core scheduling algorithm.

---

## Week 07: Implementation Phase – III (Scheduling Algorithm)

**Date:** —

- Implemented `generateForDept()` — main scheduler for a single department:
  - Collects all subjects, practicals, and options from the form
  - Identifies lecture period indices blocked by practical time window
  - Handles Zero Hour (last slot fixed across all days)
  - Handles Monday First Period (Assembly / Library Hr.)
  - Handles Saturday Last Period (Sports / Club Activity)

- Implemented subject pool generation — each subject repeated according to its `periods/week` value.

- Implemented two placement strategies:
  - **Spread mode**: round-robin across days for even distribution
  - **Sequential mode**: fills day by day

- Implemented `noConsec` constraint: avoids placing the same subject in back-to-back slots on the same day.

- Implemented overflow fill: remaining empty slots filled with the least-assigned subject to balance the table.

### Guide Interaction

Guide suggested the no-consecutive constraint should apply to fill slots as well, not just the primary pool placement.

### Next Plan

Practical rotation logic.

---

## Week 08: Implementation Phase – IV (Practical Rotation Logic)

**Date:** —

- Implemented rotating practical batch scheduling:

  ```
  Day 1: S1 → DS (PR),  S2 → ML-I (PR),  S3 → SDS (PR)
  Day 2: S1 → ML-I (PR), S2 → SDS (PR),  S3 → DS (PR)
  Day 3: S1 → SDS (PR),  S2 → DS (PR),   S3 → ML-I (PR)
  ```

- Logic: for group `gi` on practical day `pdi`, assigned practical = `pracDefs[(pdi + gi) % pracDefs.length]`

- Groups sharing the same practical on a day are merged into one combined cell.

- Implemented `buildUnifiedSlots()`:
  - Morning lecture slots (before practical time window)
  - Practical block placeholder
  - Afternoon lecture slots (after practical time window)
  - Continuous period numbering across the split

- Practical days spread evenly across the week based on `sessions/week`.

### Guide Interaction

Guide suggested displaying all batches doing different practicals simultaneously in the same row for clarity.

### Next Plan

Timetable rendering and PDF export.

---

## Week 09: Implementation Phase – V (Timetable Rendering & PDF Export)

**Date:** —

- Implemented `buildTTHTML()` — HTML timetable renderer:
  - Institution header rows (name, dept, class, room, W.E.F.)
  - Column headers (period numbers + time ranges in 12-hr format)
  - Section rows separating lecture and practical blocks
  - Break rows with custom background colors
  - Color-coded subject cells with subject code, teacher name, and room
  - Practical cells showing all batch-to-practical assignments
  - Free period indicator for unscheduled slots

- Implemented PDF export using **jsPDF** + **jsPDF-AutoTable**:
  - `downloadCurrentPDF()` — exports the active department tab
  - `downloadAllPDF()` — loops through all departments and generates combined PDF
  - Cell background colors preserved in PDF output
  - Staff/teacher summary table appended at the end of each department's PDF

- Implemented toast notifications for user feedback.

### Guide Interaction

Guide suggested including a teacher-wise summary section in the PDF showing each subject's teacher alongside their assigned periods per week.

### Next Plan

System testing.

---

## Week 10: System Testing

**Date:** —

- Tested:
  - Timing setup with various slot durations (30 min to 180 min)
  - Break insertion at start, middle, and end of day
  - Multi-department add/remove/switch
  - Subject pool overflow and fill behavior
  - No-consecutive constraint under high-subject-count loads
  - Practical rotation across 2, 3, 4, and 5 batch groups
  - PDF export for single and multiple departments
  - Working day toggles (all on, weekdays only, custom)

- Used sample institute data (SES's R. C. Patel Institute of Technology, Shirpur) for real-world validation.

### Guide Interaction

Guide suggested testing edge cases: zero subjects, single subject, and more periods/week than available slots.

### Next Plan

Results analysis.

---

## Week 11: Results & Analysis

**Date:** —

- Smart scheduling algorithm successfully placed all subjects within the week without conflicts.

- Rotating practical logic correctly assigned different practicals to different batches each day.

- No-consecutive constraint reduced back-to-back repetition significantly.

- PDF output maintained accurate colors, layout, and teacher summary.

- Planned future enhancements:
  - Save/load timetable configuration as JSON
  - Teacher conflict detection across departments
  - Import subjects from CSV
  - Per-department working day overrides
  - Dark mode

### Guide Interaction

Guide suggested adding teacher conflict detection as a priority enhancement to prevent the same teacher being scheduled in two departments simultaneously.

### Next Plan

Final documentation.

---

## Week 12: Final Report & Conclusion

**Date:** —

- Completed documentation and project README.

- Prepared final demo walkthrough.

- Project implemented successfully — fully browser-based with no server or installation required.

### Guide Interaction

Guide approved final submission.

### Next Plan

Project evaluation.

---

## Project Description

### TimeGrid — Institute Edition

TimeGrid is a browser-based academic timetable generation tool for colleges and institutes. It requires no server, no installation, and runs entirely in the browser using plain HTML, CSS, and JavaScript.

### Features

- Separate configurable timing for lectures and practicals
- Named break/interval management with color coding
- Multi-department support with tabbed navigation
- Theory subjects with code, teacher, periods/week, color, and room
- Practical subjects with batch-wise rotating group scheduling
- Smart options: no-consecutive, spread evenly, zero hour, Monday first, Saturday last
- Color-coded interactive timetable preview
- One-click PDF export per department or all departments combined
- Fully offline — no internet needed after loading fonts/libraries

### Tech Stack

| Layer | Technology |
|-------|------------|
| Structure | HTML5 |
| Styling | CSS3 (custom variables, grid, flexbox) |
| Logic | Vanilla JavaScript (ES5 compatible) |
| PDF Export | jsPDF 2.5.1 + jsPDF-AutoTable 3.8.2 |
| Fonts | Google Fonts (Inter + Syne) |

---

## Future Scope

- Save and load timetable configuration (JSON export/import)
- Teacher conflict detection across multiple departments
- Import subjects from CSV file
- Dark mode support
- Mobile-responsive layout improvements
- Per-department custom working day overrides
- Printable timetable without PDF library dependency

---

## Team Members

-Pranjal Ravindra Mali
-Sagar Devidas Patil
-Devang Bhavesh Khairnar
-Darshan Vijay Mahajan

---

## Guide

Dr.Ujwala Patil
