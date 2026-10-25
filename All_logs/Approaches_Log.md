# Kalpa Health — Approaches Log

**Project:** Data Analysis for Briefs A and B
**Date:** 25 October 2026
**Prepared by:** Malav, Rakesh and Chetan
**Objective:** Document the analytical methods used to evaluate the growth shortfall and assess whether Jaipur and Chennai are experiencing soft demand.

| ID  | Analytical Approach              | Method Applied                                                                                      | Key Output / Finding                                                                                                                                              | Purpose                                                                              |
| --- | -------------------------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| A01 | Half-year comparison             | Compared H1 2025 with H1 2026.                                                                      | Tests increased from 435,008 to 456,788, a growth of 5.01%.                                                                                                       | Establish the overall growth baseline.                                               |
| A02 | KPI cross-check                  | Compared growth in booked tests, collected tests, and result rows.                                  | Growth was +5.01%, +4.94%, and +4.97%, respectively.                                                                                                              | Check whether the headline conclusion depends on the selected test-count KPI.        |
| A03 | Target-gap calculation           | Compared actual growth with the 18% target specified in the brief.                                  | Approximately 78,300 additional tests were needed; 21,780 were added, leaving a shortfall of approximately 56,500.                                                | Quantify the scale of the growth gap.                                                |
| A04 | Growth decomposition             | Examined patients served, visits, visits per patient, and tests per visit.                          | Patients grew 3.76%, visits 2.75%, visits per patient declined about 1.0%, and tests per visit increased about 2.2%.                                              | Separate customer reach, visit frequency, and test intensity.                        |
| A05 | City-level comparison            | Calculated test growth, patient growth, tests added, and share of the shortfall for six cities.     | Jaipur and Chennai accounted for approximately 47% of the shortfall. Neither city reached the growth target.                                                      | Identify geographic contributors to underperformance.                                |
| A06 | City-level frequency analysis    | Compared visits per patient across cities.                                                          | Frequency declined by approximately 1–2.4% in Bengaluru, Hyderabad, Pune, and Ahmedabad; it was nearly flat in Jaipur and increased in Chennai.                   | Determine whether declining repeat frequency is a network-wide explanation.          |
| A07 | Month-by-month analysis          | Compared monthly booking and patient trends for Jaipur and Chennai across the two half-years.       | Jaipur bookings declined in all six months; Chennai bookings declined in four months, March through June.                                                         | Test whether city-level weakness is persistent or concentrated in particular months. |
| A08 | Channel breakdown                | Compared clinic bookings by App, Phone, and Walk-in sources.                                        | All three sources declined at similar rates in both cities.                                                                                                       | Assess whether a single booking source explains the clinic decline.                  |
| A09 | Clinic-level breakdown           | Examined monthly booking performance across clinics, particularly in Jaipur.                        | Four of five Jaipur clinics declined in every month.                                                                                                              | Test whether one underperforming clinic explains the city trend.                     |
| A10 | Clinic-versus-home comparison    | Compared booking trends by service setting in Jaipur and Chennai.                                   | Jaipur clinic bookings fell 26.0% while home bookings rose 14.0%; Chennai clinic bookings fell 18.0% while home bookings rose 29.4%.                              | Identify whether demand differs by service setting.                                  |
| A11 | Booking-to-result reconciliation | Compared booking statuses, collection records, and result records.                                  | The reported gap was attributed to no-shows, cancelled or missing bookings, and collected bookings without results. Collected bookings had 98.3% result coverage. | Distinguish booking demand from subsequent operational completion.                   |
| A12 | Status-gap decomposition         | Examined the composition of the booking-to-result gap.                                              | Approximately 72–76% was associated with no-shows, 16–20% with cancelled or missing bookings, and 7.5% with collected bookings without results.                   | Identify which operational categories account for the gap.                           |
| A13 | Hypothesis assessment            | Compared eight proposed explanations against the observed metrics and breakdowns.                   | Weak patient growth, geographic weakness, and clinic-channel weakness received support to varying degrees; fewer tests per visit was not supported.               | Keep the diagnosis evidence-led instead of relying on assumptions.                   |
| A14 | Data-quality review              | Flagged NOVA records, missing patient names, and changes in result coverage for further validation. | Possible migration-related effects remain untested because `data_source` was not analysed.                                                                        | Protect against misinterpreting data-quality changes as business changes.            |

## Approach by Brief

### Brief A — Which part of the business is short?

The analysis used overall growth decomposition, city comparisons, and booking-to-result reconciliation to quantify the gap and identify the largest contributors.

### Brief B — Are the Jaipur and Chennai city heads right that demand is soft?

The analysis used month-by-month, channel-level, clinic-level, and clinic-versus-home booking comparisons to evaluate the two cities' claims.

## Methodological Limitations

- The analysis is descriptive, not a causal investigation.
- The 18% growth target was supplied in the brief; it was not derived from the data.
- Revenue, test mix, new versus returning patients, and clinic capacity were outside the analysis scope.
- `data_source` was not analysed, so the possible impact of NOVA migration remains unverified.
- The conclusion notebook reportedly labels the final reconciliation as “not reconciled,” despite the summary describing a status-based explanation of the gap. This wording should be reviewed before final reporting.

**Overall status:** The methods provide a consistent descriptive view of growth, geography, demand patterns, and operational reconciliation. Additional validation is needed to explain the underlying causes.
