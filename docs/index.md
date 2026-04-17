# Continuous Intelligence

This site provides documentation for this project.
Use the navigation to explore module-specific materials.

## How-To Guide

Many instructions are common to all our projects.

See
[⭐ **Workflow: Apply Example**](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
to get these projects running on your machine.

## Project Documentation Pages (docs/)

- **Home** - this documentation landing page
- **Project Instructions** - instructions specific to this module
- **Glossary** - project terms and concepts

## Additional Resources

- [Suggested Datasets](https://denisecase.github.io/pro-analytics-02/reference/datasets/cintel/)



## Custom Project

### Dataset
Daily financial transaction records from a bank, including total transactions processed,
flagged suspicious transactions, and total dollar volume per day (50 observations).

### Signals
- flag_rate: ratio of flagged to total transactions — detects fraud burst events
- avg_amount_usd: average dollar value per transaction — detects unusual spending patterns

### Experiments
Pipeline that uses financial transaction data and with
thresholds to match the financial domain:
flag rate capped at 1% and average transaction amount at $160.

### Results
The pipeline returned a DEGRADED state. Average flag rate was 1.44%,
slightly above the 1% threshold,
driven by a handful of days with major fraud spikes (300+ flagged transactions).

### Interpretation
The bank's transaction system is under stress from periodic fraud bursts.
Even though most days are normal,
a few high-fraud days are enough to degrade the overall assessment.
