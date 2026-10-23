# Kalpa Health: Data to Insight (Brief A, Growth)

A plain-language walkthrough of what we are doing, why we are doing it, and what we found.

Prepared 22 October 2026. All numbers come from the script `brief_a_steps_1_to_4.py`, run on the data pack as released.

> Before you use this: the handbook says every use of an assistant must be logged in `docs/ai-log.md`, 
	and every member must be able to explain every number. Use this document to understand the work, 
	then rewrite it in your own words and rerun the code yourself.

## 1. What is the task

### The situation

Kalpa Health (a fictional company) is a diagnostics chain: labs and walk-in clinics in six cities (Ahmedabad, Bangalore, Chennai, Hyderabad, Jaipur, Pune), selling through two channels (clinic and home collection). Last year the board approved a plan for **18% growth in test volume** in the first half of this year. The half closed at **5%**. The COO, Dr Priya Menon, has to tell the group board in Singapore where the 13 points went and what is being done about it.

### Our brief

The project has five pain points (A to E). Each group answers one end to end. We are working on 
**Brief A (Growth): Which part of my business is short?** 
	Dr Menon’s request is: “Show me the part of my business that is short, by how much, and where.”

The other four briefs are B (why bookings fell in two cities), C (why invoices and collections disagree), D (whether one clinic’s no-show rate is really worse) and E (whether free home collection lifted bookings 9%). Briefs B and E overlap with what we found in Brief A, which is why they appear in the conclusion.

### What we must deliver

- **One sentence** Dr Menon can say to her board, with a number, a denominator and a period.
- **Evidence** where every number shows its denominator.
- **One caveat** that would change the claim.
- **An action** and what it costs.

Alongside that the handbook asks for six slides, numbered notebooks that run top to bottom in under ten minutes, three logs (approach, decisions, challenges) plus an assistant log, one write-up per member, and a video.

### The five targets every group is held to

| Target | What it means |
| --- | --- |
| T1 Reproduce the 5 percent | Our test-volume growth matches Dr Menon’s figure, or we can say exactly why it does not |
| T2 One sentence | A headline claim with a number, a denominator and a period |
| T3 Auditable | Every number traces to rows; every cleaning decision is logged with its effect on the headline |
| T4 Reproducible | The notebooks run top to bottom from a fresh Codespace in under ten minutes |
| T5 Defended | Every member can explain every number and every approach the group rejected |

### The data we were given

The data pack is an export from the company’s systems, and **nothing in it has been cleaned**.

| File | What it holds | Size (approx.) |
| --- | --- | --- |
| merged\_bookings.csv | Every booking: when, city, clinic, channel, tests ordered, prices | 1.28 million rows, 568,000 bookings |
| merged\_collections.csv | Whether a sample was collected or the patient was a no-show | 550,000 bookings |
| merged\_results.csv | Lab results per test | 1.08 million rows |
| merged\_patients.csv | Patient master: signup date, city | 309,000 patients |
| merged\_invoices.csv | Invoices raised by billing | 470,000 |
| merged\_gateway.csv and merged\_settlements.csv | Payments and bank settlements | 473,000 and 562 rows |
| merged\_campaigns.csv | Offer tagging (turned out to be an exact copy of collections) | 550,000 |

For Brief A we mainly use bookings, collections and results. A plan file (the city targets) is mentioned in the handbook but was **not in our pack**, which matters later.

## 2. What should be done

### The overall pipeline

Every brief runs the same pipeline: **Sources → Profile → Clean and reconcile → Analyse → Answer**. There are three gates: profiling done before any join; the headline claim stated early, before polishing; and a cold run (restart and run everything) before the video.

The suggested working plan is four phases: Scope (10% of the time), Profile and clean (30%), Analyse (35%), Answer and defend (25%).

### The five steps of Brief A

1. **Count tests for both halves** (first half of last year and of this year) and check our growth against the 5%. If it differs, find out why before going on.
2. **Split the growth into branches:** patients served × visits per patient × tests per visit. The branches must multiply back to the total.
3. **Split by city and by channel.** The plan was set city by city, so which cities, and which channel, own most of the gap?
4. **Check whether it is a business problem or a recording problem.** If the gap sits in one or two cities, how do we know it is not just how the data was recorded?
5. **Name the short branch** and write the one sentence for the board.

Running beside every step are the logs: an approach log (two options per part, the one chosen and why), a decisions log (every cleaning rule and its effect), a challenges log (what broke and what we changed) and an assistant log.

## 3. Why we are doing it

### Why count tests, and not bookings or revenue

The handbook defines the 5% as **test volume**. A test needs a booking, a collected sample and a result, so we have to decide which table records a “test”. Early in the work we looked at bookings and booked revenue (bookings +2.1%, booked revenue +10.7%). That was useful for orientation, but it is not the number Dr Menon quotes, so we moved to counting tests. Revenue rose much faster than volume mainly because the average booking value rose by about 10%.

### Why compare the same months in both years

Growth is two numbers and a period. Comparing January to June of this year with January to June of last year removes seasonality, so the difference is growth and not the calendar.

### Why clean the data first

The data was exported from several systems and is deliberately messy. If we skipped cleaning, the numbers would be wrong in ways we could not defend to the finance controller. Problems we found:

- **Two booking systems.** Two cities (Chennai and Jaipur) moved to a new vendor (called “nova” in the data) in March and April 2026. The old system stores one row per test; the new one stores one row per booking with a list of tests. Counting rows would show a fake collapse after the move.
- **Inconsistent labels.** Bangalore and Bengaluru are the same city; channel and source appear in both upper and lower case.
- **Two date formats**, repeated header rows inside the file, and rows in the results table that point to bookings that do not exist.

### Why a tree: patients × visits per patient × tests per visit

If the total is short, there are only three ways it can be short: fewer people came, the same people came less often, or each visit carried fewer tests. Each needs a different fix (marketing, retention, or what is being ordered). The middle branch, visits per patient, is what shows whether the same people came less often. If the branches do not multiply back to the total, a branch is missing or a denominator is wrong, which makes this a built-in error check.

### Why split by city, then channel

The plan was set city by city, so the shortfall has to be shown against each city. Channel (clinic vs home) shows whether growth is real or just moving between channels.

### Why test for recording vs real

Dr Menon may cut targets and headcount in the cities that look weak. If the weakness is only a counting difference between two software systems, she would be cutting a healthy business. So before saying a city is short, we check whether the fall began before the systems changed, whether the new system counts differently, and how much of the gap is a recording difference.

### The industry context

The handbook compares us with listed chains. Dr Lal PathLabs reported revenue growth of 19.1% on 8.2% more patients, and Metropolis Healthcare reported 16.6% on 10% more patients, so a board will not accept that the market was simply soft. The same three numbers (patients, tests per patient, revenue per test) describe the whole business for every chain.

## 4. The coding part

Everything is Python with pandas. The full script is `brief_a_steps_1_to_4.py`. Below, each block is followed by what it does and why. Set `DATA_DIR` to the folder with the unzipped files.

### 4.0 Complete data-cleaning record (everything we did to your files)

This is the full record of what we found in the eight files and what we did about it. The table lists every step; the code for each step follows. Sections 4.1 to 4.8 then show the same steps as they appear in the final script.

| # | What we found | Rows affected | What we did | Why it matters |
| --- | --- | --- | --- | --- |
| C1 | Repeated header rows inside bookings (the city column literally says city) | 21 | Rejected them | They would count as bookings in a city called city |
| C2 | Exact duplicate rows in collections (all of them collected visits) | 2,894 of 552,492 | Dropped duplicates, leaving one row per booking (549,598) | Collected visits would be counted twice |
| C3 | The campaigns file is identical to the collections file | 552,492 rows | Used collections only | No extra information in campaigns |
| C4 | One city, two spellings in bookings: Bangalore and Bengaluru | 193,558 and 107,661 | Mapped Bengaluru to Bangalore | Bangalore would show as two cities |
| C5 | Channel and source in capitals (CLINIC, HOME, APP, PHONE, WALKIN) | 26,543, exactly the new-system rows | Lower-cased | The clinic/home split would be wrong |
| C6 | Two date formats: day/month/year with time (old system) and ISO with a +05:30 suffix (new system) | 1,252,765 and 26,543 | Parsed each format separately; removed the suffix | Wrong months, or failed dates, change the halves |
| C7 | Prices and fees loaded as text | all rows | Converted to numbers | Needed for the revenue view |
| C8 | Old system: one row per test. New system: one row per booking with the tests in a text list and the patient in a text dictionary | 1,252,765 rows and 26,543 bookings | Read the lists and dictionaries, then split into one row per test | Without this the new-system months look like a collapse |
| C9 | Old-system rows with a blank test code | 1,290 | Left out of test counts | Not a countable test |
| C10 | Need one row per booking (patient, city, channel, tests) | 568,007 bookings | Grouped the old rows by booking id; appended the new ones | Needed for patients, visits and tests per visit |
| C11 | Joining bookings to collections | row count unchanged; about 4% of tests have no record (8.4% in the new system) | Left join with a row-count check | Tells us which tests were collected |
| C12 | Result rows pointing to bookings that do not exist | 15,612 of 1,078,420 | Kept out of the analysis | Results are only a cross-check |

Checks that changed nothing: every date parsed (0 failures); no non-numeric prices; no missing patient ids among collected visits; collected\_at is filled for every collected visit and blank only for the 79,172 no-shows.



#### C1 and C2 Header rows and duplicates, in every file

```python
for f in files:
    d = pd.read_csv(f'Data_folder/merged_{f}.csv', dtype=str)   # dtype=str: no type guessing
    first = d.columns[0]
    header_rows = (d[first] == first).sum()        # a row that repeats the column name
    body = d[d[first] != first]
    dups = body.duplicated().sum()
    print(f, len(d), 'header rows', header_rows, 'duplicates', dups,
          'nulls', body.isna().sum()[lambda s: s > 0].to_dict())
```

Result: only bookings has header rows (21). Only collections and campaigns have duplicates (2,894). 
In bookings the blanks are expected: the old system leaves patient, tests and status blank, 
and the new system leaves the test code and price blank.

#### C3 Campaigns is a copy of collections

```python
coll = pd.read_csv('Data_folder/merged_collections.csv', dtype=str)
camp = pd.read_csv('Data_folder/merged_campaigns.csv', dtype=str)
print(coll.equals(camp))        # True: identical

coll = coll.drop_duplicates()   # C2 fix: one row per booking
assert coll['booking_id'].is_unique
print(len(coll))                # 549,598
```

#### C4 to C7 Standardise the bookings columns

```python
raw = pd.read_csv('Data_folder/merged_bookings.csv', dtype=str)
rejected = raw[raw['city'] == 'city']                       # C1
b = raw[raw['city'] != 'city'].drop_duplicates()

print(b['city'].value_counts())                              # shows Bangalore and Bengaluru
b['city'] = b['city'].replace({'Bengaluru': 'Bangalore'})   # C4

b['channel'] = b['channel'].str.lower()                      # C5
b['source'] = b['source'].str.lower()


b['test_price'] = pd.to_numeric(b['test_price'], errors='coerce').fillna(0)
b['home_fee'] = pd.to_numeric(b['home_fee'], errors='coerce').fillna(0)
```

The city column in the other files (collections, invoices, patients) only says Bengaluru, and results use lower case (bengaluru). Those spellings matter only if we join on city, so we join on booking id instead.

#### C8 to C10 One table of tests, and one table of bookings

```python
import ast

# old system: already one row per test (blank test codes dropped: C9)
legacy = b[(b['booking_type'] == 'legacy') & b['test_code'].notna()]
leg_tests = legacy[['booking_id', 'created', 'city', 'channel', 'test_code']]

# new system: tests are a text list, the patient a text dictionary (C8)
nova = b[b['booking_type'] == 'nova'].copy()
nova['patient_id'] = nova['patient'].map(lambda x: ast.literal_eval(x)['patient_id'])
nova['codes'] = nova['tests'].map(lambda s: [t['code'] for t in ast.literal_eval(s)])
nova_tests = (nova.drop(columns=['test_code'])        # empty column would clash with the new name
                  .explode('codes')
                  .rename(columns={'codes': 'test_code'}))

tests = pd.concat([leg_tests.reset_index(drop=True),
                   nova_tests[['booking_id', 'created', 'city', 'channel', 'test_code']]
                   .reset_index(drop=True)], ignore_index=True)

# one row per booking (C10)
bk = b.drop_duplicates('booking_id').set_index('booking_id')[['created', 'slot', 'city', 'channel']]
bk['patient_id'] = pd.concat([
    b[b['booking_type'] == 'legacy'].drop_duplicates('booking_id').set_index('booking_id')['patient_id'],
    nova.set_index('booking_id')['patient_id']])
bk['n'] = tests.groupby('booking_id').size()          # tests per booking
bk['n'] = bk['n'].fillna(0)
print(len(bk))                                         # 568,007 bookings
```

#### C11 and C12 Joins and orphan results

```python
tests_c = tests.merge(coll[['booking_id', 'status']], on='booking_id', how='left')
assert len(tests_c) == len(tests)                      # the join must not change the row count
tests_c['status'] = tests_c['status'].fillna('no_collection_record')
print(tests_c['status'].value_counts(normalize=True))

res = pd.read_csv('Data_folder/merged_results.csv', dtype=str)
orphans = ~res['booking_id'].isin(set(bk.index))      # results whose booking does not exist
print(orphans.sum(), 'of', len(res))                   # 15,612 of 1,078,420
```

#### Files inspected but not cleaned (not needed for Brief A)

We looked at these for Brief C (money) and left them untouched:

- **Invoices:** the amount is text with thousands separators (for example 1,070.00 in 287,946 rows), so it needs the commas removed before it can be added up. City is spelled Bengaluru.
- **Gateway payments:** amounts are in paise, not rupees; 18,038 failed payments have no settlement id; payment ids look nothing like invoice ids.
- **Settlements:** 562 rows, with the fee and tax taken out of each settlement.
- **Patients:** 309,217 patients, one row each; the signup date is the field needed to tell new patients from returning ones (our first attempt without it was wrong and was dropped).

```python
inv = pd.read_csv('Data_folder/merged_invoices.csv', dtype=str)
inv['amount'] = inv['amount'].str.replace(',', '').astype(float)   # for Brief C, not used here

gw = pd.read_csv('Data_folder/merged_gateway.csv', dtype=str)
gw['amount_rs'] = gw['amount_paise'].astype(float) / 100             # for Brief C, not used here
```

#### Earlier exploratory cut: bookings and booked revenue

Before we switched to counting tests, we measured bookings and booked value (tests plus home fee). This was useful orientation but is not the handbook’s measure.

```python
leg_rev = (b[b['booking_type'] == 'legacy'].groupby('booking_id')
           .agg(created=('created', 'first'), city=('city', 'first'), channel=('channel', 'first'),
                tests=('test_price', 'sum'), fee=('home_fee', 'max')))
nova['tests_value'] = nova['tests'].map(lambda s: sum(t['price'] for t in ast.literal_eval(s)))
nova_rev = nova.set_index('booking_id')[['created', 'city', 'channel', 'tests_value', 'home_fee']]
nova_rev.columns = ['created', 'city', 'channel', 'tests', 'fee']

rev = pd.concat([leg_rev, nova_rev])
rev['value'] = rev['tests'] + rev['fee']
h = rev[rev['created'].dt.month <= 6]
print(h.groupby(h['created'].dt.year).agg(bookings=('value', 'size'), value=('value', 'sum')))
```

Result: bookings +2.1% and booked value +10.7% (H1 2026 vs H1 2025), with the average booking value up about 10%. Revenue grew faster than volume mainly because of higher value per booking, not more tests.

### 4.1 Load the bookings and reject junk

```python
raw = pd.read_csv(f'{DATA_DIR}/merged_bookings.csv', dtype=str)
rejected = raw[raw['city'] == 'city']          # repeated header rows
body = raw[raw['city'] != 'city']
b = body.drop_duplicates()
exact_dups = len(body) - len(b)
assert len(raw) == len(b) + len(rejected) + exact_dups   # input = clean + rejected
```

`dtype=str` stops pandas guessing column types on messy data; we convert the columns we need ourselves. The assert is the handbook’s reconciliation rule: every row in the raw file is either kept or rejected, and we can say which. Result: 21 header rows rejected, no exact duplicates.

### 4.2 Standardise cities, channels and dates

```python
b['city'] = b['city'].replace({'Bengaluru': 'Bangalore'})
b['channel'] = b['channel'].str.lower()

def parse_dt(s):
    iso = s.str.match(r'^\d{4}-', na=False)
    a = pd.to_datetime(s[iso].str.replace(r'\+05:30$', '', regex=True))
    c = pd.to_datetime(s[~iso], format='%d/%m/%Y %H:%M')
    return pd.concat([a, c]).reindex(s.index)
```

One city has two spellings, so we merge them. Dates come in two formats (day/month/year, and ISO with a timezone), so we parse each format separately and put them back in the original order. Result: every date parsed.

### 4.3 Build one table with one row per test

```python
nova['codes'] = nova['tests'].map(lambda s: [t['code'] for t in ast.literal_eval(s)])
nova_tests = (nova.drop(columns=['test_code'])
                  .explode('codes')
                  .rename(columns={'codes': 'test_code'}))
tests = pd.concat([leg_tests, nova_tests], ignore_index=True)
```

The old system already has one row per test. The new system stores the tests as a text list, so we read the list (`ast.literal_eval`) and `explode` it into one row per test. We drop the empty `test_code` column first, because otherwise two columns end up with the same name and the concat fails. That failure is a good entry for the challenges log.

### 4.4 Join to collections, without changing the row count

```python
assert coll['booking_id'].is_unique
tests_c = tests.merge(coll[['booking_id', 'status']], on='booking_id', how='left')
assert len(tests_c) == len(tests), 'join changed the row count'
```

A left join keeps every test even if it has no collection record. The assert checks that the join neither dropped nor repeated rows, which is the handbook’s join rule. The `status` is either `collected` or `no_show`, or missing.

### 4.5 Step 1: count tests both halves

```python
ordered   = h1(tests_c).groupby('yr').size()
collected = h1(tests_c[tests_c['status'] == 'collected']).groupby('yr').size()
```

`h1` keeps January to June. We count tests ordered, and tests ordered on bookings where the sample was collected. We also compare other date bases (slot date, result date) to see why 5.0% cannot be reproduced exactly.

### 4.6 Step 2: the tree

```python
def tree(d):
    return pd.Series({'tests': d['n'].sum(), 'visits': len(d),
                      'patients': d['patient_id'].nunique()})

t['visits_per_patient'] = t['visits'] / t['patients']
t['tests_per_visit']    = t['tests'] / t['visits']
product = ((1 + g['patients']) * (1 + g['visits_per_patient'])
           * (1 + g['tests_per_visit']) - 1)
assert abs(product - g['tests']) < 1e-9
```

For collected visits only: how many tests, visits and distinct patients in each half. The last assert is the multiply-back check: if it fails, a branch is wrong.

### 4.7 Step 3: split by city and channel, and measure the shortfall

```python
out['shortfall'] = out['tests_2025'] * PLAN - (out['tests_2026'] - out['tests_2025'])
out['share_of_shortfall'] = out['shortfall'] / out['shortfall'].sum()
```

For each city, `tests_2025 × 18%` is how many extra tests the plan expected, and the second part is how many extra tests actually arrived. The difference is that city’s shortfall, and dividing by the total gives its share. **Assumption:** because the plan file is missing, `PLAN` is a flat 0.18 for every city.

### 4.8 Step 4: recording or business problem

```python
originals = set(bk['rescheduled_from'].dropna())     # originals of rescheduled bookings
minus = hb[~hb.index.isin(originals)]                # remove the doubled visits

m = bk[...].groupby(['group', 'yr', 'mo']).size().unstack('yr')
yoy = (m[2026] / m[2025] - 1) * 100                  # year-on-year bookings by month
```

Three checks: does the new system count a reschedule as a second booking, do the systems carry a different number of tests per booking, and when did each city’s fall begin compared with when the system changed.

## 5. Observed results

### Step 1: how many tests?

| How tests are counted | H1 2025 | H1 2026 | Growth |
| --- | --- | --- | --- |
| Tests ordered (by booking-created date) | 436,094 | 454,873 | +4.3% |
| Tests collected (our chosen basis) | 357,804 | 373,266 | +4.3% |
| Tests ordered, by slot date | 435,880 | 461,080 | +5.8% |
| Tests resulted, by result date | 356,442 | 378,160 | +6.1% |

No basis gives exactly 5.0%. The date basis alone moves the answer by about two points. We chose collected tests by booking-created date because a test needs a booking and a collected sample, and this ties each test to the visit that ordered it.

### Step 2: the tree (collected tests)

| Branch | H1 2025 | H1 2026 | Growth |
| --- | --- | --- | --- |
| Patients served | 84,079 | 86,829 | +3.3% |
| Visits per patient | 1.853 | 1.833 | -1.1% |
| Tests per visit | 2.297 | 2.346 | +2.1% |
| **Tests** | 357,804 | 373,266 | **+4.3%** |

The branches multiply back: 1.033 × 0.989 × 1.021 = 1.043. Volume grew because more patients came and each visit carried slightly more tests, while patients came back a little less often. There is no sign of a collapse in demand overall.

### Step 3: by city

The shortfall against an 18% plan is about 48,900 tests (18% of 357,804 is 64,405, and actual growth added 15,462).

| City | Tests growth | Patients growth | Share of the shortfall |
| --- | --- | --- | --- |
| Ahmedabad | +9.1% | +8.8% | 11.5% |
| Bangalore | +7.3% | +7.0% | 17.8% |
| Hyderabad | +6.4% | +7.4% | 14.2% |
| Pune | +6.9% | +6.9% | 11.3% |
| Chennai | +0.4% | -4.8% | 23.6% |
| Jaipur | -10.4% | -11.7% | 21.5% |

Every city missed 18%, even the best (Ahmedabad at +9.1%). Jaipur and Chennai together own about 45% of the shortfall.

### Step 3: by channel

| Channel | Tests 2025 | Tests 2026 | Growth |
| --- | --- | --- | --- |
| Clinic | 229,195 | 199,052 | -13.2% |
| Home | 128,609 | 174,214 | +35.5% |

Clinic lost about 30,000 tests and home collection gained about 45,600. Almost all of the net growth is home collection replacing clinic visits, not new demand.

### Step 4: real or a recording problem?

**Reschedules.** The new system saves a rescheduled visit as a new booking and keeps the original (marked “rescheduled”), so those visits are counted twice. All 1,500 originals are still in the data. Removing them makes the picture worse, not better: Jaipur bookings go from -10.8% to -13.3%, and Chennai from -0.6% to -3.4%.

**Tests per booking** are the same in both systems (2.345 old, 2.352 new), so there is no artefact in the tests-per-visit branch.

**Timing.** Bookings, 2026 against the same month of 2025:

| Month | Jaipur | Chennai | Other four cities |
| --- | --- | --- | --- |
| Jan | -12.7% | +0.9% | +5.6% |
| Feb | -16.8% | +9.9% | +10.3% |
| Mar | -16.6% | -6.9% | +5.8% |
| Apr | -1.4% | +0.9% | +5.8% |
| May | -8.1% | +1.4% | +3.3% |
| Jun | -6.5% | -8.8% | -2.9% |

Jaipur was already down 13% to 17% in January to March, on the old system. A new system cannot cause a fall that began before it was installed. Chennai is noisy, with no clean break at the migration.

**Missing collection records.** About 8.4% of new-system tests have no collection record, against about 4.0% for the old system. This undercounts collected tests in Chennai and Jaipur: roughly 1.7 points of Chennai’s weakness and 1.2 points of Jaipur’s come from this recording gap.

### Data problems found

- 21 repeated header rows in bookings; 15,612 result rows whose booking does not exist in the bookings table.
- 1,500 doubled bookings from the new system’s reschedule rule.
- About 8.4% of new-system tests with no collection record (4% in the old system).
- Campaigns file is identical to the collections file.
- The city plan file is missing from the pack.

## 6. Conclusion

### The claim

In the first half of 2026, Kalpa Health collected 373,266 tests against 357,804 in the same half of 2025, growth of 4.3% against a plan of 18%. The shortfall is spread across all six cities, but Jaipur (tests down 10.4%) and Chennai (up 0.4%) together own about 45% of it, and the growth that did happen came from home collection (up 35.5%) replacing clinic visits (down 13.2%).

### How sure are we

- **Jaipur: a real fall.** It began in January, before the new system, and counting differences in the new system actually hide part of it. Confidence: fairly high.
- **Chennai: not yet knowable.** About 1.7 points of the gap is recording, the rest is unexplained and monthly swings are large. Confidence: medium to low.
- **Overall growth:** 4.3% on our chosen basis, but 5.0% is not reproduced exactly, and the choice of date basis moves it by about two points.

### The caveats that would change the claim

1. The plan file was missing, so the city shares assume a flat 18% for every city. The real city plans could shift the shares.
2. The 5% is not reproduced exactly. If finance uses a different basis (for example slot date), the headline changes.
3. We have no competitor or cost data, so we cannot test the “Thyrocare opened nearby” explanation or price an action.

### What Dr Menon should do

- Investigate what changed in Jaipur in January (closures, staffing, pricing, a competitor).
- Ask what changed in Chennai’s collection centres and phlebotomist teams when the new system went live.
- Ask finance for the plan by city so the shortfall shares are exact.
- Treat the home collection offer carefully: most of the growth is channel shift from clinic to home, which is exactly the question in Brief E. The action’s cost is still to be priced from cost data we do not have.

### What we still have to do

- Rerun the script ourselves from a fresh Codespace and confirm every number.
- Fill the decisions, approach and challenges logs in our own words, and the assistant log.
- Settle on one clear definition of test volume and be ready to explain why it differs from 5.0%.
- Build the six slides (question, picture, claim, evidence, caveat, action), the Excel board number and the video.

### Questions to practise for the viva

1. Which table did you count from, and why not the other two?
2. Why patients × visits × tests, and what does the middle branch show?
3. Show me that your branches multiply back to the total.
4. How do you know the gap is a business problem and not a recording problem?
5. What would make you wrong, and which single check would you run with ten minutes left?
