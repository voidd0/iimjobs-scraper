# IIMJobs Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape India finance, sales, consulting, HR and operations jobs from iimjobs.com: company, locations, experience in years, salary in lakhs when shown, skills, category, posting time, the employer's AmbitionBox rating and profile, and the description. Search by keyword, category, location.

This repository has ready scripts for the [IIMJobs Scraper: India Finance and MBA Jobs | $0.50 per 1,000](https://apify.com/v0iddo/iimjobs-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/iimjobs-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "keywords": [
    "finance manager"
  ],
  "locations": [
    "Mumbai"
  ],
  "includeDetails": true,
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `keywords` | array | Job titles, skills or words such as chartered accountant, product manager or brand manager. |
| `category` | string | Optional. With search terms it keeps the results of this category; alone it lists the category, for example Banking & Finance or Consulting. |
| `categories` | array | Optional, to pick several categories at once. |
| `locations` | array | Optional cities or states such as Bangalore, Mumbai, Delhi NCR, Pune, Hyderabad, Chennai or Remote. |
| `experienceMin` | integer | Optional. Postings whose experience band overlaps the range you give. |
| `experienceMax` | integer | Optional. Upper end of the experience range in years. |
| `postedWithin` | string | Only postings from the last 3, 7, 15 or 30 days. |
| `salaryOnly` | boolean | Keep only postings whose employer shows the salary range. |
| `remoteOnly` | boolean | Keep only postings marked work from home or located in Remote. |
| `includeDetails` | boolean | Read each posting once more for its description text. |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "jobId": 1746229,
  "site": "iimjobs.com",
  "title": "Senior Executive/Assistant Manager - Finance",
  "postingTitle": "Nexdigm - Senior Executive/Assistant Manager - Finance - CA",
  "company": "Nexdigm",
  "companyLogo": "https://recruiter-static-content.s3.ap-south-1.amazonaws.com/CompanyLogos/centralised-4798.webp",
  "companyRating": 3.6,
  "companyReviews": 949,
  "companyFounded": null,
  "companyEmployeesIndia": "1001-5000",
  "companyType": "Indian MNC",
  "companyIndustry": "Management Consulting",
  "companyHeadquarters": "Pune, Maharashtra",
  "companyAbout": "Nexdigm is a privately held, independent global organization that helps companies across geographies meet the needs of a dynamic business environment. Our focus on problem-solving, supported by our multifunctional expertise, enables us to deliver customized solutions tailored...",
  "companyReviewsUrl": "https://www.ambitionbox.com/reviews/nexdigm-reviews",
  "locations": [
    "Mumbai"
  ],
  "location": "Mumbai",
  "remote": false,
  "experienceMin": 3,
  "experienceMax": 5,
  "salaryMinLakhs": null,
  "salaryMaxLakhs": null,
  "salaryCurrency": null,
  "salaryHidden": true,
  "skills": [
    "Accounting",
    "Financial Control",
    "Financial Analysis",
    "Financial Reporting",
    "CA"
  ],
  "mandatorySkills": [
    "Accounting",
    "Financial Control",
    "Financial Analysis",
    "CA"
  ],
  "category": "Banking & Finance",
  "premium": true,
  "postedAt": "2026-11-17T11:00:20Z",
  "descriptionText": "Educational Qualification: Chartered Accountant\n\nWork Mode: Hybrid\n\nAbout the department:\n\nOur CFO support service team provide various services in finance and accounting function like ERP implementation, legacy accounting, audit assistance, GAAP advisory, Finance transformati...",
  "url": "https://www.iimjobs.com/j/nexdigm-senior-executiveassistant-manager-finance-ca-1746229",
  "scrapedAt": "2026-11-20T02:42:08Z"
}
```

Fields: `jobId`, `site`, `title`, `postingTitle`, `company`, `companyLogo`, `companyRating`, `companyReviews`, `companyFounded`, `companyEmployeesIndia`, `companyType`, `companyIndustry`, `companyHeadquarters`, `companyAbout`, `companyReviewsUrl`, `locations`, `location`, `remote`, `experienceMin`, `experienceMax`, `salaryMinLakhs`, `salaryMaxLakhs`, `salaryCurrency`, `salaryHidden`, `skills`, `mandatorySkills`, `category`, `premium`, `postedAt`, `descriptionText`, `url`, `scrapedAt`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/iimjobs-scraper').call({"keywords": ["finance manager"], "locations": ["Mumbai"], "includeDetails": true, "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~iimjobs-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"keywords": ["finance manager"], "locations": ["Mumbai"], "includeDetails": true, "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
