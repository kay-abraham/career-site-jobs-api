![Career Site Jobs API: every open job from company job boards, with filters and a hiring surge signal](https://raw.githubusercontent.com/kay-abraham/actor-assets/main/career-site-jobs/banner.png)

# Career Site Jobs API

Every open job from a company's own job board: Greenhouse, Lever, Ashby, Workday and 11 more. Job seeker filters, a hiring surge signal per company, and a keyword search across 12,241 employers.

**Run it on Apify Store: [apify.com/kay_abraham/career-site-jobs-scraper](https://apify.com/kay_abraham/career-site-jobs-scraper)**

This repository holds the documentation and ready-to-run example inputs. The Actor's source code is private. Questions and bugs: open an issue on the [Actor's Issues tab](https://apify.com/kay_abraham/career-site-jobs-scraper/issues).

## Example inputs

Each file in [`examples/`](examples) is an input you can paste into the Actor, or run as a published example task:

- [Find SaaS companies hiring sales reps this month](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/companies-hiring-sales-reps): [`examples/companies-hiring-sales-reps.json`](examples/companies-hiring-sales-reps.json)
- [Get this week's new jobs from tech career sites](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/greenhouse-ashby-jobs-this-week): [`examples/greenhouse-ashby-jobs-this-week.json`](examples/greenhouse-ashby-jobs-this-week.json)
- [Weekly new-job alerts for your target accounts](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/weekly-new-job-alerts): [`examples/weekly-new-job-alerts.json`](examples/weekly-new-job-alerts.json)
- [Track competitors' hiring by department](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/competitor-hiring-by-department): [`examples/competitor-hiring-by-department.json`](examples/competitor-hiring-by-department.json)
- [Get startup jobs with salary ranges](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/startup-jobs-with-salary-ranges): [`examples/startup-jobs-with-salary-ranges.json`](examples/startup-jobs-with-salary-ranges.json)
- [Get Ashby jobs by company](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/ashby-jobs-by-company): [`examples/ashby-jobs-by-company.json`](examples/ashby-jobs-by-company.json)
- [Get Lever jobs by company](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/lever-jobs-api): [`examples/lever-jobs-api.json`](examples/lever-jobs-api.json)
- [Find data engineer jobs at 2,000 big employers](https://apify.com/kay_abraham/career-site-jobs-scraper/examples/data-engineer-jobs-tech-companies): [`examples/data-engineer-jobs-tech-companies.json`](examples/data-engineer-jobs-tech-companies.json)

## Full documentation

Give it company domains. Get back every open job from each company's own job board, plus one row per company that says who's hiring, for what, and whether it's a hiring surge. Built for job seekers who work through AI assistants and job boards, for recruiters and staffing agencies, and for sales and RevOps teams. **$2 per 1,000 jobs plus $0.003 per company read.** Companies without a supported job board cost nothing.

It reads the public job board APIs of 15 systems: Greenhouse, Lever, Ashby, Workday, Workable, SmartRecruiters, Recruitee, Personio, BambooHR, Teamtailor, Breezy HR, Rippling, Pinpoint, JazzHR, and Freshteam. It finds the right board by itself. No login, no proxy, no API key. Filter by country, remote, department, seniority, salary, skills, and more. Export the company table as a CSV for Clay or HubSpot.

## What does Career Site Jobs API do?

- **Read every open job** from a company's own job board on 15 systems, from startups on Greenhouse and Ashby to large employers on Workday and small trades, retail, and staffing firms on Teamtailor, BambooHR, and JazzHR.
- **Find the board for you.** Give it a domain like stripe.com, a careers page URL, a job board URL, or `ats:slug`.
- **Filter like a job seeker.** Seniority, minimum salary in USD a year, skills with a match score, description keywords, words to exclude, visa sponsorship, relocation support, and stale postings.
- **Say it when data is missing.** A job without a published salary or level isn't silently dropped. It's kept and marked partial, or dropped if you pick strict mode.
- **Read big employers on Workday.** In a test on November 11, 2026, large US enterprises went from 1 of 40 readable to 9 of 40, and NVIDIA's roughly 2,000 open roles were read in about 2 minutes.
- **Flag a hiring surge** per company, so you can sort a target list by who's growing.
- **Watch a list** and get only new and closed roles since your last run.
- **Search 12,241 company job boards by keyword** (Greenhouse, Lever, Ashby, and Workday) when you have no company list.

## Who uses it

- **A job seeker working through an AI assistant.** Connect the Actor to Claude, Cursor, or any MCP client. Ask for senior data roles in the US that pay at least $150,000 and match your skills. The assistant gets rows ranked best match first, with full matches ahead of partial ones.
- **A niche job board or newsletter.** Pull every open role from the companies you cover, with salary where it's published. `jobId` stays the same between runs, so refreshing your database is an upsert.
- **A recruiting or staffing agency.** Read the boards of trades, healthcare, logistics, and retail employers on BambooHR, Teamtailor, JazzHR, Breezy HR, and Rippling to see who needs drivers, electricians, or nurses.
- **A sales or RevOps team.** Run your account list weekly and sort by hiring surge. A company that posted 44 roles in two weeks is growing, and its top departments tell you which team to call.

## How to use it

1. Paste companies into **Companies**, one per line, or turn on **Search all companies** and add keywords.
2. Optional: add keywords and job filters (country, remote, seniority, salary, and so on).
3. Press **Start**. The prefilled example finishes in seconds and costs about $0.121.
4. Open the **Output** tab: jobs in one table, one row per company in the other, and a CSV link for your CRM.
5. To watch companies, add a monitor name, turn on **Only return new jobs**, and schedule the run weekly.

## How it works

```text
domains / careers URLs / ats:slug
        |
        v
find the company's job board (Greenhouse, Lever, Ashby, Workday, Workable,
SmartRecruiters, Recruitee, Personio, BambooHR, Teamtailor, Breezy HR,
Rippling, Pinpoint, JazzHR, Freshteam)
        |
        v
read the public job board API  (every open job, live)
        |
        v
your filters: countries, remote, departments, employment type, seniority,
salary, skills, keywords, posted in the last N days
        |
        +--> job rows        (default dataset: one row per open role,
        |                     marked full or partial match)
        +--> company rows    (dataset "companies": one row per company,
                              hiring surge, top departments, CRM view)
```

Filters shape the job rows. The company row still counts every open job and adds how many passed your filters (`jobsMatchingFilters`, split into `jobsFullMatch` and `jobsPartialMatch`).

## What you get

**Jobs** (the main table). One row per open role: title, department, team, location, country, remote or hybrid, employment type, seniority, salary when the company publishes it (also converted to USD a year), posted date, how many days it's been open, job link, and apply link. Plain-text descriptions if you want them. With job seeker filters on, each row also carries `matchQuality`, `unknownFields`, and, when you set skills, `skillsMatched` and `skillsMatchScore`.

**Companies** (the second table). One row per company:

- which job board system it uses, and the board link
- open roles now, posted in the last 7 days, posted in the last 30 days
- **hiring surge**: `hiringSurge` is true when the company posted at least 5 roles in the last 14 days (you can change both numbers), plus `rolesPostedInSurgeWindow`
- top departments and locations
- your keyword hits, with sample titles
- with monitoring on: new roles and closed roles since the last run
- `staleJobs` and `medianDaysOpen`
- one sentence you can paste into a CRM note

**CRM view.** The company table has a `crm` view with flat columns (company name, domain, open roles, surge flag, top departments as text). The run output has a link to it as a CSV, ready to import into Clay or HubSpot.

A real job row from a test on November 11, 2026 (trimmed):

```json
{
  "companyName": "Ramp",
  "title": "Technical Consultant, Commercial",
  "department": "Sales",
  "team": "Solutions Consulting",
  "location": "Remote (US); San Francisco, CA; New York, NY (HQ)",
  "country": "US",
  "remote": true,
  "employmentType": "full-time",
  "salaryText": "$124,560 - $190,300",
  "postedAt": "2026-11-03T22:33:38.811Z",
  "url": "https://jobs.ashbyhq.com/ramp/100c09be-f179-406f-893e-8a212f32c642",
  "ats": "ashby"
}
```

## Real output: hiring surge on 10 target accounts

In a test on November 11, 2026, I gave it 10 SaaS domains and set job rows to `none`. It took 14 seconds, returned 10 company rows and 0 job rows, and cost 10 x $0.003 = **$0.03** at the current prices.

```json
{
  "companies": ["figma.com", "vercel.com", "ramp.com", "notion.so", "linear.app",
                "supabase.com", "datadoghq.com", "cloudflare.com", "airtable.com", "cursor.com"],
  "jobsOutput": "none"
}
```

CRM view columns from that run (8 of the 10 rows shown):

| companyName | openJobs | jobsPostedLast30Days | hiringSurge | rolesPostedInSurgeWindow | topDepartmentsText |
|---|---|---|---|---|---|
| Cloudflare | 473 | 150 | true | 81 | Field Sales 66, Engineering 65, Solution Engineering 65 |
| Datadog | 393 | 87 | true | 44 | Enterprise Sales 80, Enterprise Sales Engineering 27, People 21 |
| Ramp | 176 | 62 | true | 37 | Sales 82, Engineering 36, People & Talent 12 |
| Notion | 156 | 53 | true | 35 | Sales 59, Outcomes Architect 18, Engineering 16 |
| Cursor | 116 | 106 | true | 104 | Sales 29, Engineering 24, Solutions 22 |
| Figma | 142 | 49 | true | 23 | Sales 44, Engineering 26, Design 10 |
| Linear | 33 | 11 | false | 3 | GTM 18, Product 13, Operations 2 |
| Airtable | 8 | 3 | false | 3 | Customer Success & Services 4, Sales 3, Marketing 1 |

The summary sentence on each company row reads like this:

```text
Linear (Ashby): 33 open roles, 11 posted in the last 30 days (2 in the last 7), most in GTM (18).
```

When a company is in surge, the sentence adds "Hiring surge: 23 roles in the last 14 days."

## Use cases

### 1. Hiring surge as a buying signal for sales and RevOps

Run your account list weekly with `jobsOutput: "none"`. Sort the CRM CSV by `hiringSurge` and `rolesPostedInSurgeWindow`. A company that posted 44 roles in two weeks is growing, and its top departments tell you which team to call. The run above is the sample output. To count only roles you care about, add a department filter: the surge is counted inside your job filters.

### 2. Job seeker: senior roles in the US with a minimum salary

You want senior roles in the US that pay at least $150,000 a year. Many boards don't publish pay or a level, so you also want to see which rows are confirmed and which aren't.

```json
{
  "companies": ["ramp.com", "figma.com", "vercel.com", "notion.so", "datadoghq.com"],
  "seniority": ["senior"],
  "countries": ["US"],
  "minSalaryUsdYear": 150000
}
```

In a test on November 11, 2026, this took 5 seconds and returned 275 job rows: **48 full matches and 227 partial matches**. It cost 5 x $0.003 + 275 x $0.002 = **$0.565**. Full matches come first inside each company. A full match, trimmed:

```json
{ "companyName": "Vercel", "title": "Senior Brand Designer", "country": "US",
  "seniority": "senior", "salaryText": "$156,000-$234,000", "salaryMaxUsdYear": 234000,
  "matchQuality": "full", "unknownFields": [],
  "url": "https://job-boards.greenhouse.io/vercel/jobs/5579560004" }
```

A partial match. The title doesn't say a level, and Notion didn't publish pay:

```json
{ "companyName": "Notion", "title": "Software Engineer, AI Connections (Developer Platform)",
  "country": "US", "seniority": "not-stated", "salaryText": "Not published by the company",
  "matchQuality": "partial", "unknownFields": ["seniority", "salary"],
  "url": "https://jobs.ashbyhq.com/notion/1fc309c8-da20-4ff2-84c7-8b863ece2b0a" }
```

`RUN_SUMMARY` said why in plain words: "Seniority: 224 of 275 jobs don't say a level in the title. They were kept and marked partial. Set Unknown values to exclude to drop them." With `"unknownValues": "exclude"` the same run returns only the 48 full matches.

Add `skills` (for example `["python", "sql"]`) and rows get a `skillsMatchScore` from 0 to 100 and are sorted best match first. Add `minSkillsMatch` to drop weak matches. The skills match reads the job description, so the Actor fetches descriptions on its own when you set skills.

### 3. Agency or trades: drivers, electricians, and nurses on small-company boards

Small employers in trades, retail, logistics, and healthcare staffing often run on Teamtailor, BambooHR, JazzHR, Breezy HR, or Rippling. Name the boards you want, or give domains and let the Actor find them.

```json
{
  "companies": ["teamtailor:wenzels", "bamboohr:laketownelectric",
                "jazzhr:dropoffinc", "jazzhr:exceptionalhealthcareinc"],
  "keywords": ["driver", "electrician", "nurse"],
  "jobsOutput": "matching"
}
```

In a test on November 11, 2026, this took 2 seconds, read 4 boards, and returned 92 job rows (Dropoff 66, Exceptional Healthcare 21 nurses, Laketown Electric 3 electricians, Wenzel's 2 van drivers). Cost: 4 x $0.003 + 92 x $0.002 = **$0.196**. Three rows, trimmed:

```json
{ "companyName": "Laketown Electric", "title": "Journeymen Electrician", "location": "Mankato, Minnesota",
  "employmentType": "full-time", "salaryText": "Not published by the company",
  "url": "https://laketownelectric.bamboohr.com/careers/148" }
```

```json
{ "companyName": "Exceptional Healthcare Inc.", "title": "ER Registered Nurse (Nights)", "location": "Bullhead City, AZ",
  "employmentType": "not-stated", "salaryText": "Not published by the company",
  "url": "https://exceptionalhealthcareinc.applytojob.com/apply/TM4fwojMlN/ER-Registered-Nurse-Nights" }
```

```json
{ "companyName": "Wenzel's The Bakers", "title": "Van Delivery Driver - Nights", "employmentType": "full-time",
  "postedAt": "2026-10-15T05:24:45.000Z", "salaryText": "Not published by the company",
  "url": "https://wenzels.teamtailor.com/jobs/8548585-van-delivery-driver-nights" }
```

Some of these boards don't give a posted date in their list. BambooHR, Rippling, and JazzHR only give it with a second call per job (made when you turn on descriptions or use a text filter), and Pinpoint never has one. Under a "posted in the last N days" filter those jobs are kept and marked partial. In another test on November 11, 2026, I ran Teamtailor, BambooHR, Breezy HR, Pinpoint, JazzHR, Freshteam, and Rippling boards together (`teamtailor:wenzels`, `bamboohr:laketownelectric`, `breezy:embraer`, `pinpoint:upway`, `jazzhr:dropoffinc`, `freshteam:talent360`, `rippling:pace`). It returned 272 jobs in about 2 seconds. 192 had no posted date on the board, and they were kept as partial under a 30-day filter, with a plain warning.

### 4. Big employers on Workday

Workday runs many large employers. Name the site as `workday:tenant.wdN/site`, or paste a careers page that links to one.

```json
{
  "companies": ["workday:nvidia.wd5/NVIDIAExternalCareerSite",
                "workday:mastercard.wd1/CorporateCareers"],
  "seniority": ["senior", "staff"],
  "countries": ["US"],
  "postedWithinDays": 7
}
```

In a test on November 11, 2026, this read NVIDIA's 1,999 open roles and Mastercard's 898 in 118 seconds. It returned 151 job rows: 37 full matches and 114 partial. Partial here mostly means the job says only "4 Locations" or "Remote", so there's no country to confirm. Cost: 2 x $0.003 + 151 x $0.002 = **$0.308**. A full match, trimmed:

```json
{ "companyName": "Mastercard", "title": "Senior Analyst, CAO Program Management",
  "location": "Purchase, New York", "country": "US", "seniority": "senior",
  "postedAt": "2026-11-05T00:00:00.000Z", "matchQuality": "full", "unknownFields": [],
  "url": "https://mastercard.wd1.myworkdayjobs.com/CorporateCareers/job/Purchase-New-York/Senior-Business-Analyst--Chief-Administrative-Office_R-294497" }
```

Workday only gives the description with one extra call per job, so the Actor makes those calls only when you turn on descriptions or use a text filter (skills, description keywords, visa, relocation). That's slower on a 2,000-job employer. A Workday board costs one company fee like any other.

### 5. Remote US sales roles at target accounts

Want to see who's hiring sellers in the US? Filter by department and country.

```json
{
  "companies": ["figma.com", "vercel.com", "ramp.com", "notion.so", "linear.app",
                "supabase.com", "datadoghq.com", "cloudflare.com", "airtable.com"],
  "departments": ["sales"],
  "countries": ["US"],
  "remoteOnly": true,
  "postedWithinDays": 30
}
```

In a test on November 11, 2026, this returned 30 job rows (Ramp 23, Datadog 6, Notion 1), and 9 company rows. It took 13 seconds and cost 9 x $0.003 + 30 x $0.002 = **$0.087**. Two of the rows, trimmed:

```json
{ "companyName": "Ramp", "title": "Technical Consultant, Commercial", "department": "Sales",
  "location": "Remote (US); San Francisco, CA; New York, NY (HQ)", "country": "US",
  "remote": true, "workplaceType": "remote", "employmentType": "full-time",
  "salaryText": "$124,560 - $190,300", "postedAt": "2026-11-03T22:33:38.811Z",
  "url": "https://jobs.ashbyhq.com/ramp/100c09be-f179-406f-893e-8a212f32c642" }
```

```json
{ "companyName": "Ramp", "title": "Solutions Consultant, Commercial", "department": "Sales",
  "location": "Remote (US); Remote (Canada); San Francisco, CA; New York, NY (HQ); Toronto, ON; Miami, FL",
  "country": "US", "remote": true, "workplaceType": "remote", "employmentType": "full-time",
  "salaryText": "$119K - $189K", "postedAt": "2026-11-03T17:04:10.236Z",
  "url": "https://jobs.ashbyhq.com/ramp/220e674a-fc70-439f-bacf-687553ca59c9" }
```

Note the `remote: true` flag comes from the board. A role can be marked remote and still show a hybrid `workplaceType` when the company lists several setups. That's the board's data, passed through as it is.

### 6. No company list: account executive roles in the UK and Germany

Turn on Search all companies and let the Actor scan the built-in list of 12,241 Greenhouse, Lever, Ashby, and Workday boards. Workday employers are searched with Workday's own keyword search. Set **How many companies to search** to Quick for the 2,000 largest boards.

```json
{
  "searchAllCompanies": true,
  "searchScope": "quick",
  "keywords": ["account executive"],
  "countries": ["United Kingdom", "Germany"],
  "postedWithinDays": 14
}
```

In a test on November 11, 2026, the quick scope scanned the 2,000 largest boards in about 9 minutes and returned 170 job rows: 137 full matches in the UK or Germany. The rest were remote roles with no country, kept and marked partial. At the current prices that's the $0.10 search fee + 170 x $0.002 = **$0.44**. 45 of the 2,000 boards didn't answer in that run, and `RUN_SUMMARY` counted them instead of hiding them. A full search of all 12,241 boards takes about 15 to 20 minutes, so schedule it or run it through the API rather than waiting on it. Two rows from an earlier test the same day, trimmed:

```json
{ "companyName": "Harvey", "title": "Strategic Account Executive, DACH", "department": "Go to Market",
  "location": "Munich", "country": "DE", "workplaceType": "hybrid", "employmentType": "full-time",
  "postedAt": "2026-11-02T08:38:24.748Z",
  "url": "https://jobs.ashbyhq.com/harvey/c34ee114-fd2d-4507-9888-711397e037bd" }
```

```json
{ "companyName": "Anthropic", "title": "Enterprise Account Executive - Semiconductors", "department": "Sales",
  "location": "London, UK", "country": "GB", "salaryText": "£330,000 - £380,000 GBP",
  "postedAt": "2026-10-29T19:27:18.000Z",
  "url": "https://job-boards.greenhouse.io/anthropic/jobs/5461024008" }
```

The run wrote 29 company rows, only for companies with matches. Their counts cover every open job at that company, not only the jobs you got back. The built-in list covers Greenhouse, Lever, Ashby, and Workday. If you also list Workday sites in **Companies**, the Actor searches each one with Workday's own keyword search instead of paging through every job.

### 7. Salary benchmarks

Greenhouse, Ashby, Lever, Recruitee, and Personio jobs carry a pay range when the board exposes it. Many of the newer systems do too (see the table below). Keep only roles with a published salary and compare the same title across companies.

```json
{
  "companies": ["figma.com", "ramp.com", "vercel.com", "supabase.com", "datadoghq.com"],
  "salaryOnly": true,
  "keywords": ["engineer"],
  "jobsOutput": "matching"
}
```

In a test on November 11, 2026, this took 8 seconds, returned 98 job rows (Ramp 33, Figma 23, Vercel 22, Datadog 20), and 5 company rows. Cost: 5 x $0.003 + 98 x $0.002 = **$0.211**. Sample `salaryText` values:

| Company | Title | salaryText |
|---|---|---|
| Figma | Software Engineer - Full Stack | $153,000 - $376,000 USD |
| Figma | Security Engineer | $153,000 - $376,000 USD |
| Ramp | Software Engineer, iOS | $189K - $330K |
| Ramp | Machine Learning Engineer | $200K - $330K |
| Vercel | DevRel Engineer, Agentic Infrastructure | $168,000 - $252,000 |

The text is whatever the company wrote, so formats differ (`$189K` versus `$153,000`). Each job row also carries a structured `salary` object with min, max, and currency, plus `salaryMinUsdYear` and `salaryMaxUsdYear` for easy sorting. Internship rows can show a single number like `$55 - $55 USD`. When the board doesn't say the period, the Actor guesses from the size of the number (under 1,000 is read as hourly), so check those by hand. Greenhouse pay is read from the job text, so unusual formats can be missed.

### 8. Weekly new-role alerts with monitoring

Set a monitor name and turn on **Only return new jobs**, then schedule it weekly.

```json
{ "companies": ["figma.com", "vercel.com", "supabase.com"], "monitorName": "target-accounts", "onlyNewJobs": true }
```

I ran this twice in the same storage. Run 1 (first run) returned 27 job rows, the roles posted in the last 7 days, and recorded every open job. At the current prices it costs 3 x $0.003 + 27 x $0.002 = $0.063. Run 2, straight after, returned 0 job rows and cost $0.009 (just the three companies). Each company row said "Since last run: 0 new, 0 closed." That's the honest result when nothing changed in the minutes between runs. On a weekly schedule, run 2 would return the roles posted since run 1. Monitoring is off in Search all companies mode.

### 9. A live job feed for your job board or newsletter

Pull every open role from the companies you list, with salary where it's published, and refresh it daily. `jobId` stays the same between runs, so updating your database is an upsert.

## Filters and options

| Field | What it does |
|---|---|
| `companies` | Domains, careers URLs, job board URLs, or `ats:slug`. Required unless you search all companies. |
| `keywords` | Whole-word matches in job title, department and team. Each company row counts the hits. |
| `jobsOutput` | `all`, `matching` (keyword hits only) or `none` (company rows only, the cheapest way to watch many companies). Search all companies always uses matching. |
| `searchAllCompanies` | Searches the built-in list of 12,241 Greenhouse, Lever, Ashby, and Workday boards for your keywords. Needs at least one keyword. |
| `searchScope` | `all` (every board, about 15 to 20 minutes) or `quick` (the 2,000 largest boards, about 9 minutes). Same price. |
| `atsPlatforms` | Search mode only. Limit to Greenhouse, Lever, Ashby, or Workday. Empty means all four. |
| `locations` | Keep jobs whose location text contains any of these, like Berlin or Bay Area. Case doesn't matter. |
| `countries` | Country names or two-letter codes (`United States`, `DE`, `UK`). A job listed only as "Remote" or a region like EMEA has no country: it's kept and marked partial, or dropped in strict mode. |
| `remoteOnly` | Keep only jobs the board marks as remote, or with Remote in the location. Always strict. |
| `departments` | Keep jobs whose department or team contains any of these, like Sales or Data. |
| `employmentTypes` | `full-time`, `part-time`, `contract`, `internship`, `temporary`. A job whose board and title don't say shows `not-stated`. |
| `seniority` | `intern`, `entry`, `mid`, `senior`, `staff`, `lead`, `manager`, `director`, `vp`, `executive`. Read from the job title. A plain "Software Engineer" says no level, so it shows `not-stated`. |
| `minSalaryUsdYear` | Keep jobs whose top published pay is at least this much a year in USD. Other currencies and hourly or monthly pay are converted with fixed approximate rates (hourly x 2,080, monthly x 12). 0 means off. |
| `salaryOnly` | Keep only jobs with a published pay range. Always strict. |
| `descriptionKeywords` | Keep jobs whose title or description contains any of these words, like python or kubernetes. Whole-word match, so `sql` doesn't match NoSQL. Each row lists what matched in `descriptionMatches`. |
| `excludeKeywords` | Drop jobs whose title contains any of these words, like intern or recruiter. |
| `skills` | Skills to look for in the title and description, like python, sql, C++, or .NET. Each row gets `skillsMatched` and `skillsMatchScore` (0 to 100), and rows are sorted best match first within each company. |
| `minSkillsMatch` | Keep jobs that match at least this share of your skills. 0 only ranks. Needs `skills`. |
| `visaSponsorshipOnly` | Keep jobs whose text says the company sponsors visas. Jobs that say they can't, or say nothing, are left out. Always strict. |
| `relocationOnly` | Keep jobs whose text offers relocation help. Always strict. |
| `staleAfterDays` | A job open longer than this gets `possiblyStale: true` and is counted in `staleJobs` on the company row. Default 60. |
| `excludeStaleJobs` | Drop jobs open longer than `staleAfterDays`. Jobs with no posted date are kept, because the age is unknown. |
| `unknownValues` | `keep` (default) or `exclude`. What to do when a filter needs something the company didn't publish. See the next section. |
| `surgeMinRoles` | Roles needed to flag a hiring surge. Default 5. |
| `surgeWindowDays` | Days back to count for the surge. Default 14. |
| `monitorName` | Turns on change tracking. Job IDs are kept in the `hiring-signals-state` key-value store in your account. |
| `onlyNewJobs` | With a monitor name: only roles not seen before. First run returns roles posted in the last 7 days. |
| `postedWithinDays` | Date filter for job rows. 0 means off. A job with no posted date is kept and marked partial, or dropped in strict mode. |
| `maxJobs` | Stops after this many job rows. 1,000 by default in search mode, no limit otherwise. |
| `includeDescriptions` | Adds the plain-text description to each job row. Bigger dataset, same price. |

Two more: `guessSlugs` (on by default, see "How it finds the job board") and `maxJobsPerCompany` (safety cap, default 2,000).

Filters shape the job rows and the surge count. The other company numbers (open roles, posted in the last 7 and 30 days, top departments) always cover every open job.

## When the company doesn't publish it

Many boards leave out pay, a level, a country, or a posted date. The rule:

- **By default, a job is never dropped because a value is missing.** It's kept and marked `matchQuality: "partial"`, and `unknownFields` lists what was missing, for example `["salary", "seniority"]`. A job where every active filter is confirmed is `full`.
- **Strict mode.** Set `unknownValues` to `exclude` and only jobs that prove they match are returned.
- **"Only" switches are always strict.** Salary published only, visa sponsorship only, relocation only, and remote only ask for proof, whatever `unknownValues` says.
- **Order.** Inside each company, full matches come first, then partial ones (after the skills score when skills are set). The company row has `jobsFullMatch` and `jobsPartialMatch`.

What a missing value looks like on the row:

| Field | When the company doesn't publish it |
|---|---|
| Salary | `salary: null`, `salaryText: "Not published by the company"`, `salaryStatus: "not-published"` |
| Seniority | `seniority: "not-stated"` |
| Employment type | `employmentType: "not-stated"` |
| Country | `country: null`, plus `locationScope: "remote-anywhere"` or `"region"` when the text says so |
| Posted date | `postedAt: null`, `daysOpen: null` |
| Visa sponsorship | `visaSponsorship: null` |

**Data completeness in `RUN_SUMMARY`.** The run summary shows the share of returned jobs that have a salary, seniority, country, posted date, employment type, and department, per job board system. When a filter keeps many jobs only because data was missing, a plain warning says so, like this one from the test above: "Seniority: 224 of 275 jobs don't say a level in the title. They were kept and marked partial. Set Unknown values to exclude to drop them."

In the same test, the 5 companies published a salary on 80.7 percent of jobs and a level in the title on 18.5 percent. Expect gaps, and decide with the `unknownValues` switch how strict you want to be.

## Supported job board systems

| System | Typical employers | What it gives | Notes |
|---|---|---|---|
| Greenhouse | Tech companies and scaleups | Title, department, location, posted date, description | Pay is read from the job text |
| Lever | Tech companies and scaleups | Title, team, location, workplace type, pay when listed | |
| Ashby | Startups and scaleups | Title, department, location, remote, posted date, pay | |
| Workday | Large employers, many in the US | Title, location, posted date. Description and pay with extra calls | Name it as `workday:tenant.wdN/site`. In search mode it uses Workday's own keyword search |
| Workable | Small and mid-size companies | Title, department, location, posted date | No pay |
| SmartRecruiters | Mid-size and large employers | Title, department, location, posted date | No pay or description |
| Recruitee | European SMBs | Title, department, location, pay when listed | |
| Personio | European SMBs | Title, department, location, pay when listed | |
| BambooHR | Trades, engineering firms, and SMBs | Title, department, employment type, city, workplace type. Pay text and posted date with a second call | No posted date in the list |
| Teamtailor | Retail, bakery, hospitality, logistics | Description, date, employment type, pay, address, department | Own-domain careers sites need `teamtailor:subdomain` |
| Breezy HR | Mid-size manufacturers, agencies, SMBs | Title, type, location, remote, department, pay text, date | Description needs one call per job |
| Rippling | SMBs and staffing firms | Title, department, location, workplace type. Description, pay, and date with a second call | |
| Pinpoint | Banks, hospitality, professional services | Description, type, workplace type, department, location, pay when shown | No posted date |
| JazzHR | Healthcare staffing, logistics, small agencies | Title, location, department. Date and type with a second call | Read from the public job board page, since JazzHR has no feed |
| Freshteam | SMBs, many outside the US | Description, date, remote flag, city and country, job role | No pay. Never guessed from the domain, so use a link or `freshteam:slug` |

"A second call" means one extra request per job. The Actor makes those only when you turn on descriptions or use a text filter (skills, description keywords, visa, relocation).

## How much does it cost?

Pay per event. No subscription, no rental, and you don't pay for compute on top.

| You pay for | Free and Bronze | Silver | Gold and up |
|---|---|---|---|
| Each job row | $0.002 ($2 per 1,000) | $0.0017 | $0.0014 |
| Each company whose job board was found and read | $0.003 | $0.00255 | $0.0021 |
| Each run in Search all companies mode | $0.10 | $0.085 | $0.07 |
| Companies with no supported job board | free | free | free |

In search mode there's no per-company fee. You pay the search fee once per run plus the job rows.

**Worked example: 100 companies every week**, monitoring on, only new jobs:

- 100 company boards read: 100 x $0.003 = **$0.30**
- Say each company posts 3 new roles a week: 300 x $0.002 = **$0.60**
- **$0.90 a run, about $3.90 a month.**

**Search example** (use case 6 above): one search for "account executive" in the UK and Germany over 14 days. $0.10 + 170 jobs x $0.002 = **$0.44**.

**Prefilled example in Apify Console:** 5 boards and 53 jobs. 5 x $0.003 + 53 x $0.002 = **$0.121**. It takes about 3 seconds.

Other sizes:

- 100 companies, company table only: 100 x $0.003 = $0.30
- 100 companies, every open job (about 80 each): $0.30 + 8,000 x $0.002 = about $16.30

Set a maximum cost per run in the run options. The Actor stops cleanly when it gets there and says how far it got.

## Integrations

**Clay.** Clay lists Apify as a data provider: you can import data from an Apify Actor into a table, or run an Actor from an enrichment step. Connect your Apify API key under Add Connection in Clay, pick `kay_abraham/career-site-jobs-scraper`, and paste the input JSON. (I confirmed from Clay's own docs that the Apify connection exists. I haven't built a Clay table with this Actor yet, so check the field mapping on your first run.) If you'd rather not use the connector, Clay can also import the CSV link from the run output, or call the run-sync API below from an HTTP API column.

**HubSpot.** Run the Actor, open the run output, download the `crm` view as CSV, and import it as Companies in HubSpot. Match on `domain`. Put `hiringSurge` and `rolesPostedInSurgeWindow` into custom company properties and you can build a list of accounts in surge.

**n8n.**

1. A Schedule Trigger (weekly).
2. The Apify node: run `kay_abraham/career-site-jobs-scraper` with your companies, a monitor name and `onlyNewJobs: true`, and wait for it to finish.
3. The Apify node again: get the dataset items of that run. Those are the new jobs.
4. Send them to Slack, a Google Sheet or your CRM.

**Make.** Use the Apify app: Run an Actor with the same input, then Get Dataset Items, then route the rows to your CRM or a sheet. Put the scenario on a weekly schedule.

**API.** Run it and get the jobs in one call:

```bash
curl -X POST "https://api.apify.com/v2/acts/kay_abraham~career-site-jobs-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"companies": ["figma.com", "vercel.com"], "departments": ["sales"], "postedWithinDays": 7}'
```

The company table is in the run's second dataset (`companies`). The run object's `output.companies` field holds its URL, and `output.companiesCrmCsv` holds the CSV.

**MCP (AI agents).** Add the Apify MCP server to Claude, Cursor, or any MCP client with this Actor as a tool:

```text
https://mcp.apify.com?tools=kay_abraham/career-site-jobs-scraper
```

Then ask: "Which of figma.com, vercel.com, and linear.app are in a hiring surge, and what are they hiring for?" Or, as a job seeker: "Find senior data roles in the US that pay at least $150,000 and match python and sql." The agent sends the input and reads back the rows, full matches first.

### Already know the board? Use the by-board-name API

Skip detection and name the board: `greenhouse:airbnb`, `lever:palantir`, `ashby:ramp`, `workable:acme`, `smartrecruiters:BoschGroup`, `recruitee:bunq`, `personio:tado`, `workday:nvidia.wd5/NVIDIAExternalCareerSite`, `teamtailor:wenzels`, `bamboohr:laketownelectric`. The same pattern works for `breezy`, `rippling`, `pinpoint`, `jazzhr`, and `freshteam`. A board URL works too (`jobs.lever.co/palantir`).

```bash
curl -X POST "https://api.apify.com/v2/acts/kay_abraham~career-site-jobs-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"companies": ["greenhouse:airbnb", "lever:palantir", "ashby:ramp", "workday:nvidia.wd5/NVIDIAExternalCareerSite"], "postedWithinDays": 14}'
```

## How it finds the job board

1. If you give a job board URL or `ats:slug`, it uses that.
2. If you give a domain, it reads the company's homepage and careers pages and looks for a link or embed from a supported system.
3. If the site doesn't show one, it tries the company name on the systems where a name can be guessed. A match is accepted only when the board's own company name equals the brand (ignoring words like Inc or GmbH) or the job ads name the company. Those rows say `detectedBy: slug-guess` with confidence `medium` or `low`. Low ones are worth a glance. Workday and Freshteam can't be guessed, so they need a careers page link or a name like `workday:tenant.wdN/site`.
4. The same board given twice (`figma.com` and `greenhouse:figma`) is charged once. The second row says `duplicate`.
5. If the careers page uses a system this Actor doesn't read (iCIMS, Taleo, SuccessFactors, Phenom, Eightfold, Comeet, Dover, Homerun, JOIN, and others), the row names the system, with status `unsupported-ats`. Not charged.

The run's `RUN_SUMMARY` record shows results per job board system, data completeness, and any warnings. If one system fails for every company in a run, the summary names it, so a broken source never looks like "no jobs".

## How it compares with other job scrapers

| | This Actor | Typical ATS job scraper |
|---|---|---|
| Input | Company domains. It finds the board itself. | Board slugs or URLs you look up first |
| Job board systems | 15, including Workday | Usually 1 to 3 |
| Wrong-company guesses | Checked against the brand. Unconfirmed guesses are free. | Varies |
| One row per company (surge, departments, keyword hits) | Yes | Rare |
| Filters: country, remote, department, type, salary | Yes | Varies |
| Seniority, skills match, visa and relocation filters | Yes | Rare |
| Missing salary or level | Kept and marked partial, or dropped in strict mode | Usually dropped or left blank |
| CSV view for Clay or HubSpot | Yes | Rare |
| New and closed roles since last run | Yes, with a monitor name | Some |
| Search by keyword with no company list | Yes, 12,241 boards | Rare |
| Companies on iCIMS, Taleo and others | Named on the row, not charged | Varies |
| Price | $2 per 1,000 jobs + $0.003 per company read | Mostly $1.50 to $5 per 1,000 jobs (Store prices checked November 8, 2026) |

## FAQ

**Where does the data come from?** It reads public job postings through each job board system's own public job-board API, the same feeds companies use to show jobs on their own websites. For JazzHR it reads the public job board page, since JazzHR has no feed. No login, and no personal data beyond what a company puts in its own job ads. You're responsible for how you use the data.

**Is this Actor affiliated with Greenhouse, Lever, Ashby, or the other job board companies?** No. It's an independent tool that reads public job boards. Greenhouse, Lever, Ashby, Workday, Workable, SmartRecruiters, Recruitee, Personio, BambooHR, Teamtailor, Breezy HR, Rippling, Pinpoint, JazzHR, and Freshteam are trademarks of their owners, and so are Clay and HubSpot.

**It returned no jobs. What should I check?** Open the run's `RUN_SUMMARY` record. It shows results per job board system and names any system that failed for every company. Then check your filters, and whether `unknownValues` is on `exclude`: strict mode drops jobs with no posted date, no country, no level, or no pay. A company with no supported job board costs nothing, and its row says which system it saw. If it still looks wrong, open an issue on the Issues tab with the company and what you expected.

**Which companies won't it find?** Companies on iCIMS, Taleo, SuccessFactors, Phenom, Eightfold, Comeet, Dover, Homerun, or JOIN, and companies with a fully custom careers site. In a test on November 11, 2026, with 120 well-known companies, it read the job board for 30 of 40 SaaS companies, 21 of 40 European scaleups, and 9 of 40 large US enterprises (it was 1 of 40 before Workday). Misses are free, and the row says which system it saw. Staffing agencies on Bullhorn or JobAdder aren't covered either.

**How does missing data work?** A job isn't dropped because a company didn't publish a salary, level, country, or date. It's kept, marked `partial`, and `unknownFields` says what was missing. Set `unknownValues` to `exclude` for strict results. The "only" switches (salary, visa, relocation, remote) are always strict. See "When the company doesn't publish it" above.

**Why don't the numbers match LinkedIn?** LinkedIn counts reposts, agency ads and roles synced from other sites. This reads the company's own board, so it's the company's own list.

**How fresh is the data?** Live. Every run reads the job boards at that moment. There's no cached database in between. Each row has `checkedAt`, and `daysOpen` tells you how long a role has been posted. A role open longer than `staleAfterDays` (default 60) is marked `possiblyStale`, since old postings are often filled or never closed. In a platform test on November 11, 2026, 3 of 3 runs passed, and 10 of 10 spot-checked jobs existed with the same title.

**How is the country detected?** From the board's own country field when it has one. Otherwise from the location text (`London, UK` becomes GB, `Munich` becomes DE). A job listed in several places can match several countries. A job listed only as "Remote" or a region has no country, so a country filter keeps it as partial, or drops it in strict mode.

**How is the salary converted to USD?** The structured min and max are converted to USD a year with fixed approximate exchange rates. Hourly pay is multiplied by 2,080, monthly by 12. The rates are rough, so treat `minSalaryUsdYear` as a filter, not an exact figure.

**What does hiring surge mean?** The company posted at least `surgeMinRoles` roles (default 5) in the last `surgeWindowDays` days (default 14). It's counted inside your job filters, so "sales roles only" gives a sales surge. It's a simple count of posted dates, not a forecast. Boards without posted dates can't show a surge.

**Something's wrong or missing?** Open an issue on the Issues tab with the company and what you expected. I read every one.

**Did it save you time?** A short review on the Reviews tab helps other people find it.
