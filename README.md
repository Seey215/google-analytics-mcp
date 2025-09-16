# Google Analytics MCP Server

A Model Context Protocol (MCP) server that provides seamless access to Google Analytics 4 data through standard MCP interfaces. This tool allows LLM applications to easily query and analyze Google Analytics data without directly dealing with the complexities of the Google Analytics Data API.

## Features

- **Real-time Data Access**: Get real-time analytics data for current active users and activity
- **Custom Reports**: Create comprehensive reports with custom dimensions and metrics
- **Quick Insights**: Predefined analytics insights for common use cases
- **Metadata Discovery**: Get available dimensions and metrics for your Google Analytics property
- **Smart Error Handling**: Detailed error messages with actionable solutions for permission issues
- **Standard MCP Interface**: Works with any MCP-compatible client

## Installation

```bash
npm install @toolsdk.ai/google-analytics-mcp
```

## Prerequisites

- Node.js >= 18.0.0
- Google Analytics 4 property
- Google Cloud service account with appropriate permissions

## Setup

### 1. Google Cloud Service Account Setup

1. Create a Google Cloud service account with the "Viewer" role for your Google Analytics property
2. Download the service account key as JSON
3. Add the service account to your Google Analytics property with Viewer access:
   - Go to Google Analytics (analytics.google.com)
   - Navigate to Admin > Property Access Management
   - Add your service account email with Viewer access

### 2. Environment Configuration

Create a `.env` file in your project root with the following content:

```env
GOOGLE_CREDENTIALS='{"type":"service_account","project_id":"your-project-id","private_key_id":"your-private-key-id","private_key":"-----BEGIN PRIVATE KEY-----\nYOUR_PRIVATE_KEY\n-----END PRIVATE KEY-----\n","client_email":"your-service-account@your-project.iam.gserviceaccount.com","client_id":"your-client-id","auth_uri":"https://accounts.google.com/o/oauth2/auth","token_uri":"https://oauth2.googleapis.com/token","auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs","client_x509_cert_url":"https://www.googleapis.com/robot/v1/metadata/x509/your-service-account%40your-project.iam.gserviceaccount.com"}'
```

Note: Make sure to properly format the private key with `\n` for newlines.

## Usage

### Use [ToolSDK.ai](https://toolsdk.ai/)

```js
import { ToolSDKApiClient } from 'toolsdk/api';
const toolSDK = new ToolSDKApiClient();
// Get Tools
const GoogleAnalyticsMCP = await toolSDK.package('@toolsdk.ai/google-analytics-mcp', {
  GOOGLE_CREDENTIALS: process.env.GOOGLE_CREDENTIALS,
});
```

### Use Claude

```json
{
  "mcpServers": {
    "google-analytics-mcp": {
      "command": "npx",
      "args": [
        "-y",
        "@toolsdk.ai/google-analytics-mcp"
      ],
      "env": {
        "GOOGLE_CREDENTIALS": "Your Google Credentials JSON String"
      }
    }
  }
}
```

## Available Tools

### `analytics_report`
Get comprehensive Google Analytics data with custom dimensions and metrics. Can create any type of report.

Parameters:
- `propertyId` (string, required): Google Analytics property ID
- `startDate` (string, required): Start date (YYYY-MM-DD)
- `endDate` (string, required): End date (YYYY-MM-DD)
- `dimensions` (array, optional): Dimensions to query
- `metrics` (array, required): Metrics to query
- `dimensionFilter` (object, optional): Filter by dimension values
- `metricFilter` (object, optional): Filter by metric values
- `orderBy` (object, optional): Sort results by dimension or metric
- `limit` (number, optional): Limit number of results (default: 100)

### `realtime_data`
Get real-time analytics data for current active users and activity.

Parameters:
- `propertyId` (string, required): Google Analytics property ID
- `dimensions` (array, optional): Dimensions for real-time data
- `metrics` (array, optional): Real-time metrics (default: ['activeUsers'])
- `limit` (number, optional): Limit number of results (default: 50)

### `quick_insights`
Get predefined analytics insights for common use cases.

Parameters:
- `propertyId` (string, required): Google Analytics property ID
- `startDate` (string, required): Start date (YYYY-MM-DD)
- `endDate` (string, required): End date (YYYY-MM-DD)
- `reportType` (string, required): Type of quick insight report (overview, top_pages, traffic_sources, etc.)
- `limit` (number, optional): Limit number of results (default: 20)

### `get_metadata`
Get available dimensions and metrics for Google Analytics property.

Parameters:
- `propertyId` (string, required): Google Analytics property ID
- `type` (string, optional): Type of metadata to retrieve (dimensions, metrics, both)

### `search_metadata`
Search for specific dimensions or metrics by name or category.

Parameters:
- `propertyId` (string, required): Google Analytics property ID
- `query` (string, required): Search term to find dimensions/metrics
- `type` (string, optional): Type of metadata to search (dimensions, metrics, both)
- `category` (string, optional): Filter by category

## Error Handling

The server provides detailed error messages with actionable solutions:

- **Permission Errors**: Clear instructions on how to grant access to your service account
- **Configuration Errors**: Guidance on setting up environment variables correctly
- **API Errors**: Detailed information about what went wrong with the Google Analytics API

## License

MIT