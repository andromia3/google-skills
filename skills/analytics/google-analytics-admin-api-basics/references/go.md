# Google Analytics Admin API Go Client Library Installation

This guide provides specific instructions for installing and setting up the
Google Analytics Admin API client library for Go.

## Prerequisites

*   **Go:** Use a toolchain compatible with the selected `cloud.google.com/go/analytics` module. Release v0.37.0 declares Go 1.26.0 (checked October 11, 2026); verify the selected module's `go.mod` before installing.
*   **Modules:** Go modules enabled.
*   **Authentication:** Application Default Credentials (ADC) configured via
    `gcloud auth application-default login`.

## Installation

Install the official Go client library using `go get`.

Run the following command in your module directory:

```bash
go get cloud.google.com/go/analytics/admin/apiv1alpha
```

If `go` is not available, prompt the user to install `Go` before installing the
package.

*Why: This adds the Google Analytics Admin API client package and necessary gRPC
transport dependencies to your `go.mod` file.*

## Quickstart / Usage

```go
package main

import (
    "context"
    "fmt"
    "log"

    admin "cloud.google.com/go/analytics/admin/apiv1alpha"
    "cloud.google.com/go/analytics/admin/apiv1alpha/adminpb"
    "google.golang.org/api/iterator"
)

func main() {
    ctx := context.Background()

    // Initialize the client. Automatically authenticates via ADC.
    client, err := admin.NewAnalyticsAdminClient(ctx)
    if err != nil {
        log.Fatalf("Failed to create client: %v", err)
    }
    defer client.Close()

    req := &adminpb.ListAccountSummariesRequest{}
    it := client.ListAccountSummaries(ctx, req)
    for {
        resp, err := it.Next()
        if err == iterator.Done {
            break
        }
        if err != nil {
            log.Fatalf("Error iterating account summaries: %v", err)
        }
        fmt.Printf("Account: %s (%s)\n", resp.DisplayName, resp.Name)
    }
}
```

Modified October 11, 2026: corrected runtime prerequisites against the released module's go.mod.

Modified October 11, 2026: corrected the Go package path to the available generated `apiv1alpha` client. This is an alpha API client; do not imply that the Go package provides a beta surface. Verify method availability for the selected version.
