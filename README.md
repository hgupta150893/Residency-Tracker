# Residency-Tracker

## Residency Obligation Calculation Logic - Technical Documentation

**1. Introduction**

This document outlines the logic used to calculate residency obligations for Canadian Permanent Residents (PRs) and Citizenship eligibility. The calculations are based on a user-provided travel history and a chosen reference date.

**2. Input Data: `travel_log.csv`**

The program reads travel history from a CSV file named `travel_log.csv`. This file must contain the following columns:

*   `start_date`: Date of departure from Canada (YYYY-MM-DD format).
*   `end_date`: Date of return to Canada (YYYY-MM-DD format).
*   `trip_name`:  A descriptive name for the trip (string), can indicate 'work' or 'personal'.

**3. Core Calculation Steps**

*   **Reference Date:** The user provides a reference date (or defaults to today). All calculations are performed relative to this date.
*   **5-Year Lookback Window:** A 5-year lookback window is defined, starting 5 years prior to the reference date and ending on the reference date. This window is used to assess residency obligations. Leap years are accounted for in the window calculation.
*   **Days Outside Canada within Lookback Window:**
    *   The program iterates through each trip in the `travel_log.csv`.
    *   For each trip, it determines the period that falls within the 5-year lookback window.
    *   The duration of each trip segment within the lookback window is calculated **inclusively** (both start and end dates are counted). These durations are summed up to get the total days spent outside Canada within the 5-year window.
*   **Days Inside Canada within Lookback Window:** The total number of days in the 5-year lookback window is calculated. Days spent inside Canada are then derived by subtracting the 'Days Outside Canada within Lookback Window' from the 'Total days in the lookback window'.
*   **Residency Obligation Shortfalls:**
    *   **PR Obligation Shortfall:** Calculated as `max(0, 730 - Days Inside Canada)`.  PRs need to be physically present in Canada for at least 730 days within the last 5 years to maintain PR status.
    *   **Citizenship Eligibility Shortfall:** Calculated as `max(0, 1095 - Days Inside Canada)`.  Applicants for Canadian citizenship generally need to be physically present in Canada for at least 1095 days within the last 5 years prior to application.
*   **Projected Eligibility Dates:**
    *   To estimate projected dates for meeting PR obligation and Citizenship eligibility, the program calculates the 'shortfall' days.
    *   It then projects forward from the 'Reference Date', assuming continuous physical presence in Canada from the reference date onwards. The projected date is simply the reference date plus the number of 'shortfall' days.

**4. Key Assumptions**

*   **5-Year Lookback:** The calculations are based on a 5-year lookback period, consistent with common residency requirements.
*   **PR and Citizenship Day Requirements:**  The program uses 730 days for PR obligation and 1095 days for Citizenship eligibility as target thresholds. These are general guidelines and specific requirements may vary based on individual circumstances and changes in legislation.
*   **Continuous Stay for Projections:** Projected PR obligation met and Citizenship eligibility dates are estimated assuming continuous physical presence in Canada from the 'Reference Date'. This is a simplification, and actual eligibility dates may vary based on future travel patterns.

**5. Output**

The program provides:

*   Summary of calculated days inside and outside Canada within the 5-year lookback window.
*   Shortfalls from PR Residency Obligation and Citizenship Eligibility.
*   Projected dates for meeting PR obligation and Citizenship eligibility (with the above assumptions).
*   A Gantt chart visualizing the travel history within the 5-year lookback window, color-coded by trip type ('work' or other).
