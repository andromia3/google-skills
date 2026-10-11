# Google Analytics Data API Go Client Library Installation

This guide provides specific instructions for installing and setting up the
Google Analytics Data API (v1beta) client library for Go.

## Prerequisites

*   **Go:** Use a supported toolchain compatible with the selected genproto, OAuth2 and gRPC module versions; inspect their `go.mod` requirements before installation.
*   **Modules:** Go modules enabled.
*   **Authentication:** Application Default Credentials (ADC) configured via
    `gcloud auth application-default login`.

## Installation

Install the official Go client library using `go get`.

Run the following command in your module directory:

```bash
go get google.golang.org/genproto/googleapis/analytics/data/v1beta golang.org/x/oauth2/google google.golang.org/grpc
```

If `go` is not available, prompt the user to install `Go` before installing the
package.

*Why: The official Data API Go reference points to the generated genproto
client. It takes a gRPC connection, rather than a context or an automatically
authenticated high-level client. The example below configures TLS and ADC
explicitly. Pin module versions compatible with your project's toolchain.*

## Quickstart / Usage

```go
package main

import (
    "context"
    "fmt"
    "log"
    "time"

    "golang.org/x/oauth2/google"
    datapb "google.golang.org/genproto/googleapis/analytics/data/v1beta"
    "google.golang.org/grpc"
    "google.golang.org/grpc/credentials"
    "google.golang.org/grpc/credentials/oauth"
)

func main() {
    ctx := context.Background()

    // ADC must have the analytics.readonly scope and access to the property.
    creds, err := google.FindDefaultCredentials(ctx, "https://www.googleapis.com/auth/analytics.readonly")
    if err != nil {
        log.Fatalf("Failed to load ADC: %v", err)
    }
    conn, err := grpc.NewClient("analyticsdata.googleapis.com:443",
        grpc.WithTransportCredentials(credentials.NewTLS(nil)),
        grpc.WithPerRPCCredentials(oauth.TokenSource{TokenSource: creds.TokenSource}),
    )
    if err != nil {
        log.Fatalf("Failed to create connection: %v", err)
    }
    defer conn.Close()
    client := datapb.NewBetaAnalyticsDataClient(conn)

    req := &datapb.RunReportRequest{
        Property:   "properties/1234567",
        Dimensions: []*datapb.Dimension{{Name: "city"}},
        Metrics:    []*datapb.Metric{{Name: "activeUsers"}},
        DateRanges: []*datapb.DateRange{{StartDate: "2026-05-01", EndDate: "today"}},
    }

    requestCtx, cancel := context.WithTimeout(ctx, 30*time.Second)
    defer cancel()
    resp, err := client.RunReport(requestCtx, req)
    if err != nil {
        log.Fatalf("RunReport failed: %v", err)
    }

    for _, row := range resp.Rows {
        fmt.Printf("%s, %s\n", row.DimensionValues[0].GetValue(), row.MetricValues[0].GetValue())
    }
}
```

Modified October 11, 2026: replaced nonexistent Cloud Analytics Data package
paths with the generated client and explicit TLS/ADC connection setup. This
lower-level example does not add high-level SDK retry or pagination behavior.
