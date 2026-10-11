# Google Analytics Admin API Python Client Library Installation

This guide provides specific instructions for installing and setting up the
Google Analytics Admin API client library for Python.

## Prerequisites

*   **Python:** Use a supported Python release compatible with the selected package version. The current package requires Python 3.10 or higher (checked October 11, 2026).
*   **Package Manager:** `pip`
*   **Authentication:** Application Default Credentials (ADC) configured via
    `gcloud auth application-default login`.

## Installation

Install the official Python client library within a virtual environment.

> [!NOTE] For complete details, see the
> [Python Analytics Admin README](https://github.com/googleapis/google-cloud-python/blob/main/packages/google-analytics-admin/README.rst#installation).

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
.\.venv\Scripts\python.exe -m pip install google-analytics-admin
```

### 2. Install the Client Library

```bash
pip install google-analytics-admin
```

If `pip` is not available, prompt the user to install Python and `pip` before
installing the client library.

*Why: Installing individual packages in a virtual environment prevents version
collisions with standard system libraries.*

## Quickstart / Usage

```python
from google.analytics.admin import AnalyticsAdminServiceClient

def list_accounts():
    # Initialize client. Automatically authenticates via ADC.
    client = AnalyticsAdminServiceClient()

    account_summaries = client.list_account_summaries()

    print("Available Accounts and Properties:")
    for summary in account_summaries:
        print(f"Account: {summary.display_name} ({summary.account})")
        for prop in summary.property_summaries:
            print(f"  Property: {prop.display_name} ({prop.property})")

if __name__ == "__main__":
    list_accounts()
```

## Runtime verification

Modified October 11, 2026: corrected outdated runtime guidance and added Windows virtual environment instructions. Check the selected release's `Requires-Python` metadata on [PyPI](https://pypi.org/project/google-analytics-admin/) before installation; do not assume the latest SDK supports older Python versions.
