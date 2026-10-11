# Google Analytics Data API Python Client Library Installation

This guide provides specific instructions for installing and setting up the
Google Analytics Data API (v1beta) client library for Python.

## Prerequisites

*   **Python:** Use a supported Python release compatible with the selected package version. The current package requires Python 3.10 or higher (checked October 11, 2026).
*   **Package Manager:** `pip`
*   **Authentication:** Application Default Credentials (ADC) configured via
    `gcloud auth application-default login`.

## Installation

Install the official Google Analytics Data client library within a virtual
environment.

> [!NOTE] For complete documentation, see the
> [Python Analytics Data README](https://github.com/googleapis/google-cloud-python/blob/main/packages/google-analytics-data/README.rst#installation).

### 1. Create and Activate a Virtual Environment

On POSIX shells:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If activation is unavailable, run the virtual environment interpreter directly; do not change execution policy solely to activate it:

```powershell
.\.venv\Scripts\python.exe -m pip install google-analytics-data
```

### 2. Install the Client Library

```bash
pip install google-analytics-data
```

If `pip` is not available, prompt the user to install Python and `pip` before
installing the client library.

*Why: Installing `google-analytics-data` in a clean virtual environment ensures
repeatable builds and prevents dependency conflicts.*

## Quickstart / Usage

```python
from google.analytics.data_v1beta import BetaAnalyticsDataClient
from google.analytics.data_v1beta.types import DateRange, Dimension, Metric, RunReportRequest

def run_report(property_id: str):
    # Initialize the client. Uses ADC from environment.
    client = BetaAnalyticsDataClient()

    request = RunReportRequest(
        property=f"properties/{property_id}",
        dimensions=[Dimension(name="city")],
        metrics=[Metric(name="activeUsers")],
        date_ranges=[DateRange(start_date="2026-05-01", end_date="today")],
    )
    response = client.run_report(request)

    for row in response.rows:
        print(f"City: {row.dimension_values[0].value}, Users: {row.metric_values[0].value}")

if __name__ == "__main__":
    run_report("1234567")
```

## Runtime verification

Modified October 11, 2026: corrected outdated runtime guidance and added Windows virtual environment instructions. Check the selected release's `Requires-Python` metadata on [PyPI](https://pypi.org/project/google-analytics-data/) before installation; do not assume the latest SDK supports older Python versions.
