# TimeGrid — Institute Edition

A browser-based timetable generator for engineering institutes. Generates lecture and practical schedules for multiple departments and exports them as styled PDFs.

## Features

- Separate timing configuration for lectures and practicals
- Customizable breaks/intervals with color coding
- Multi-department support with tabbed interface
- Rotating practical group scheduling
- PDF export per department or all at once (via jsPDF)

## Usage

No build step required. Just open `index.html` in a browser.

```
index.html   → Main UI
style.css    → Styles
script.js    → All logic (scheduling, PDF generation)
```

## Dependencies (loaded via CDN)

- [jsPDF](https://github.com/parallax/jsPDF)
- [jsPDF AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable)
- [Inter & Syne fonts](https://fonts.google.com/)
