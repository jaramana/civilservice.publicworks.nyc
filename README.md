# NYC Civil Service Exams

[NYC Civil Service Exams](https://civilservice.publicworks.nyc) is an independent
[publicworks.nyc](https://publicworks.nyc) site that brings New York City exam
schedules, civil service lists and title salaries together. It shows whether an
exam is open, coming soon or closed, and what a job title pays.

## Data sources

| Source | Used for |
| --- | --- |
| [Annual Examination Schedule](https://data.cityofnewyork.us/d/4ptz-hmtc), `4ptz-hmtc` | Exams and application periods |
| [Civil Service List (Active)](https://data.cityofnewyork.us/d/vx8i-nprf), `vx8i-nprf` | Active lists and their size |
| [Civil Service List Certification](https://data.cityofnewyork.us/d/a9md-ynri), `a9md-ynri` | List use and hiring salaries |
| [NYC Civil Service Titles](https://data.cityofnewyork.us/d/nzjr-3966), `nzjr-3966` | Titles, hours, pay ranges and unions |
| [DCAS exam pages](https://www.nyc.gov/examsforjobs) | Application dates when the City's pages are ahead of open data |
| [The Pay Gap](https://paygap.publicworks.nyc) | Median pay actually received, shown as separate context |

## Method and limits

- The pipeline deduplicates exam-schedule snapshots and reconciles application
  dates with the DCAS pages. Open, coming-soon and closed labels are calculated
  from those dates.
- The active-list source contains candidate names, scores and sensitive family
  information. The fetch stage does not request, cache or publish those fields.
  The site has no candidate-name or list-number lookup.
- Title links use exact matches after normalization. An unmatched title is left
  without a linked salary rather than given a possible but unverified match.
- The site links to official Notices of Examination instead of copying their
  requirements. Calendar files give reminders on a subscriber's device without
  collecting an email address.

The [methodology page](https://civilservice.publicworks.nyc/methodology.html)
defines the fields and limits. Confirm an exam date on
[NYC.gov](https://www.nyc.gov/examsforjobs) before relying on it.

## Updates

A [daily GitHub workflow](.github/workflows/refresh.yml) rebuilds the data and
calendar files and commits only when they change. The pipeline uses live DCAS
dates when they differ from open data; it fails if those pages cannot be read,
source columns disappear or a dataset is unexpectedly small. Every page displays
the source's own “current as of” date, which is different from the build date.
Check the Actions history if refreshes appear to have stopped.

The Python pipeline starts at `run.py`. Dataset IDs, required columns and
validation thresholds are in `config.py`.

## Tools

Data pipeline: Python, `pandas` and `requests`. Site: HTML, CSS and JavaScript.
Claude was used in development.

## License and reuse

Code is [BSD 3-Clause licensed](LICENSE). Generated data and calendar files can be
reused with attribution. NYC Open Data and DCAS source material retain their own
terms; salary context comes from The Pay Gap.
