# Public.com API - Postman Collection

This repository contains a Postman collection and environment for interacting with the Public.com API. The collection includes endpoints for authorization, account management, market data, trading operations, and options trading.

## Prerequisites

- [Postman](https://www.postman.com/downloads/) desktop app or web version
- A Public.com account with API access
- Your Public.com API secret key

## Getting Started

### Step 1: Import the Collection

1. Open Postman
2. Click the **Import** button in the top left corner
3. Select **File** or drag and drop `publicdotcom-api.json` into the import window
4. Click **Import** to add the collection to your workspace

### Step 2: Import the Environment

1. Click the **Import** button again
2. Select **File** or drag and drop `publicdotcom-postman-environment.json` into the import window
3. Click **Import** to add the environment

### Step 3: Configure Environment Variables

1. Click the **Environments** tab in the left sidebar (or the environment dropdown in the top right)
2. Select **Public.com** environment
3. Set the `secret_key` variable to your Public.com API secret key
   - This is the only variable you need to configure manually
   - Other variables (`accessToken`, `expirationTime`, `orderId`) are auto-populated by the collection

### Step 4: Select the Environment

1. In the top right corner, use the environment dropdown
2. Select **Public.com** to activate the environment

## Using the Collection

### Authorization Flow

**Start here!** Before making any other API calls:

1. Navigate to **Authorization** → **Create personal access token**
2. Click **Send**
3. The access token will automatically be saved to your environment and used for subsequent requests

> **Note**: Access tokens expire based on the `validityInMinutes` parameter (default: 123 minutes). You'll need to generate a new token when it expires.

### API Endpoints Overview

The collection is organized into the following folders:

#### 1. **Authorization**
- `Create personal access token` - Generate an access token for API authentication

#### 2. **List Accounts**
- `Get accounts` - Retrieve all trading accounts associated with your Public.com profile

#### 3. **Account Details**
- `Get account portfolio v2` - View portfolio holdings and positions
- `Get history` - Retrieve account transaction history with optional pagination

#### 4. **Tax Lot Selling**
- `Get unrealized tax lots` - Retrieve the account's unrealized tax lot summary
- `Get unrealized tax lots for symbol` - Retrieve unrealized tax lot detail for a specific symbol (optional `price` query param to compute unrealized values)
- `Get unrealized tax lots CSV` - Export unrealized tax lots as a base64-encoded CSV

#### 5. **Instrument Details**
- `Get all instruments` - List all available trading instruments
- `Get instrument` - Get details for a specific instrument by symbol and type

#### 6. **Market Data**
- `Get quotes` - Fetch real-time quotes for specified instruments
- `Get option expirations` - Retrieve available expiration dates for options on an underlying
- `Get option chain` - Get option chain data for a specific expiration date

#### 7. **Order Placement**
- `Preflight single leg` - Validate a single-leg order before placement
- `Preflight multi leg` - Validate a multi-leg option order before placement
- `Place order` - Submit a single-leg equity or option order
- `Place multileg order` - Submit a multi-leg option order (spreads, etc.)
- `Get order` - Check the status of an order
- `Cancel order` - Cancel a pending order
- `Modify order` - Cancel-replace an existing order

> **Note**: Equity `Place order` and `Preflight single leg` requests optionally accept a `taxLotMatchingInstructions` array (each entry: `taxLotId`, `quantity`) to specify which tax lots to sell when closing a position. It is omitted from the sample bodies because it requires real tax-lot IDs — obtain them from the **Tax Lot Selling** endpoints and add the array yourself when needed.

#### 8. **Historic Data**
- `Get bars` - Fetch bar data for a given symbol and period
- `Get bars with aggregation` - Fetch bar data with a specific aggregation interval

#### 9. **Option Details**
- `Get option greeks` - Retrieve Greeks (delta, gamma, theta, vega, rho) for an option
- `Get strategy quote` - Get a quote for an option strategy (multi-leg) from a base symbol and option legs

## Collection Variables

The collection includes variables that can be set at the collection level:

- `accountId` - Your trading account ID (obtained from the "Get accounts" endpoint)
- `symbol` - Stock/instrument symbol (e.g., "AAPL")
- `type` - Instrument type (e.g., "EQUITY", "OPTION")
- `osiOptionSymbol` - OSI-formatted option symbol (e.g., "AAPL260220P00350000")
- `period` - Time period for historic bar data (e.g., "DAY", "WEEK", "MONTH", "YEAR", "YTD", "SINCE_PURCHASE")
- `aggregation` - Bar aggregation interval (e.g., "ONE_MINUTE", "FIVE_MINUTES", "ONE_HOUR", "ONE_DAY")
- `purchaseDate` - Date in `YYYY-MM-DD` format, required when `period` is `SINCE_PURCHASE`
- `price` - Optional price used by "Get unrealized tax lots for symbol" to compute unrealized values

### Setting Collection Variables

1. Right-click on the **Public API** collection
2. Select **Edit**
3. Go to the **Variables** tab
4. Update the **Current Value** column for each variable

## Tips & Best Practices

### Workflow for Trading

1. **Authenticate** - Generate an access token
2. **Get Account ID** - Use "Get accounts" to find your `accountId` and set it as a collection variable
3. **Browse Instruments** - Use "Get all instruments" or "Get instrument" to find tradable assets
4. **Check Market Data** - Use "Get quotes" to see current prices
5. **Preflight Your Order** - Validate your order parameters before placing
6. **Place Order** - Submit your order (the `orderId` is auto-generated)
7. **Monitor Order** - Use "Get order" to check status

### Working with Options

1. Use **Get option expirations** to find available expiration dates
2. Use **Get option chain** to browse strikes and option symbols
3. Copy the OSI option symbol (e.g., `AAPL260220P00350000`) to use in order requests
4. For multi-leg orders (spreads), use the **Preflight multi leg** and **Place multileg order** endpoints

### Auto-Generated Variables

Several requests include pre-request scripts that automatically:
- Generate a unique `orderId` using UUID v4
- Set the `accessToken` after successful authentication
- Calculate `expirationTime` for orders (24 hours from now)

## API Documentation

For detailed API documentation, parameter descriptions, and response schemas, visit the [Public.com API documentation](https://public.com/api).

## Troubleshooting

### Common Issues

**"Unauthorized" errors**
- Check that your `secret_key` is correctly set in the environment
- Verify your access token hasn't expired - regenerate if needed

**"Account not found" errors**
- Ensure `accountId` is set as a collection variable
- Run "Get accounts" to retrieve your account ID

**Invalid order errors**
- Use the preflight endpoints to validate your order parameters
- Check that instrument symbols and types are correct
- Verify quantity and price formats

## License

This collection is provided as-is for use with the Public.com API. Please refer to Public.com's terms of service for API usage guidelines.

## Resources

- [Public.com](https://public.com/)
- [Public.com API Documentation](https://public.com/api)
- [Postman Documentation](https://learning.postman.com/)