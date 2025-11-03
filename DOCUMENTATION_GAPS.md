# Documentation gaps analysis

## Summary

The documentation is **significantly out of date** compared to the main application. Several endpoints are missing, and some documented endpoints don't exist.

## Missing endpoints

The following endpoints exist in the codebase but are not documented:

### 1. `/api/models` (GET)
- **Purpose**: List all available models with their configurations
- **Authentication**: None (public endpoint)
- **Response**: Array of model configurations with pricing, categories, etc.
- **Location**: `app/api/models/route.ts`

### 2. `/api/models-config/[...modelid]` (GET)
- **Purpose**: Get detailed configuration for a specific model
- **Authentication**: None (public endpoint)
- **Parameters**: `modelid` as path segments (e.g., `flux/dev` or `firemoon-studio/background-remover`)
- **Response**: Full model configuration JSON
- **Location**: `app/api/models-config/[...modelid]/route.ts`

### 3. `/api/generations` (GET)
- **Purpose**: Get generation history for authenticated user
- **Authentication**: Required (session-based)
- **Query Parameters**: 
  - `limit` (max 100, default 50)
  - `offset` (default 0)
  - `endpoint` (optional filter)
- **Response**: Generations array with stats and pagination
- **Location**: `app/api/generations/route.ts`

### 4. `/api/usage` (GET)
- **Purpose**: Get usage statistics for authenticated user
- **Authentication**: Required (session-based)
- **Query Parameters**: 
  - `period` (today/week/month/year/all, default: all)
- **Response**: Usage breakdown by model, totals, generation stats
- **Location**: `app/api/usage/route.ts`

### 5. `/api/sample-images` (GET)
- **Purpose**: List available sample images
- **Authentication**: None (public endpoint)
- **Response**: Array of sample image URLs
- **Location**: `app/api/sample-images/route.ts`

### 6. `/api/credits/balance` (GET)
- **Purpose**: Get credit balance for authenticated user
- **Authentication**: Required (API key)
- **Response**: Current credit balance
- **Location**: `app/api/credits/balance/route.ts`

## Non-existent documented endpoints

The following endpoints are documented but **do not exist** in the codebase:

### 1. `/api/v1` (GET)
- **Documented**: "API information and available endpoints"
- **Status**: ❌ Does not exist

### 2. `/api/v1/providers` (GET)
- **Documented**: "List all available providers"
- **Status**: ❌ Does not exist

### 3. `/api/v1/providers/{provider}/models` (GET)
- **Documented**: "List models for a specific provider"
- **Status**: ❌ Does not exist

## Existing endpoints that need updates

### 1. `/api/v1/{provider}/{model}` (GET & POST)
- **Status**: ✅ Exists
- **Issues**: 
  - GET endpoint is a stub (returns minimal info)
  - Documentation doesn't match actual implementation
  - Actual endpoint path: `/api/(publicapi)/v1/[provider]/[model]/route.ts`
  - POST forwards to `/api/generate` internally

## Stub/placeholder documentation

### 1. API endpoint reference pages
- `api-reference/endpoint/create.mdx` - Just placeholder content
- `api-reference/endpoint/get.mdx` - Just placeholder content
- `api-reference/endpoint/delete.mdx` - Not checked but likely placeholder
- `api-reference/endpoint/webhook.mdx` - Not checked but likely placeholder

### 2. OpenAPI specification
- `api-reference/openapi.json` - Still contains placeholder "Plant Store" example

## Recent features not documented

Based on git history, these features were added but aren't documented:

1. **Credits system improvements**
   - Sub-cent cost support (3 decimal places for costs under $0.10)
   - Decimal precision improvements
   - Credit balance restoration utilities

2. **Generations tracking**
   - Generation history endpoint
   - Latency tracking
   - Cost tracking per generation

3. **Usage analytics**
   - Usage statistics endpoint
   - Period-based filtering (today/week/month/year/all)
   - Model breakdown

4. **Model configuration endpoints**
   - Public model listing
   - Individual model config retrieval

## Documentation structure issues

1. **Base URL inconsistency**
   - Docs say: `https://firemoon.studio/api/v1`
   - Actual: `/api/v1/{provider}/{model}` exists, but other endpoints are at `/api/...`

2. **Missing API reference pages**
   - No actual documentation for the generation endpoint
   - No documentation for model listing
   - No documentation for usage/analytics

## Recommendations

### High priority
1. Document `/api/models` endpoint
2. Document `/api/models-config/[...modelid]` endpoint
3. Document `/api/generations` endpoint
4. Document `/api/usage` endpoint
5. Document `/api/credits/balance` endpoint
6. Remove or implement `/api/v1/providers` endpoints
7. Update `/api/v1/{provider}/{model}` documentation to match actual implementation

### Medium priority
1. Complete stub endpoint reference pages
2. Update OpenAPI specification with actual endpoints
3. Document credits system improvements
4. Document error codes (402 for insufficient credits)

### Low priority
1. Add examples for new endpoints
2. Update quickstart to include new endpoints
3. Add usage analytics examples

## Files that need updates

1. `api-reference/overview.mdx` - Update endpoint list
2. `api-reference/endpoint/*.mdx` - Complete stub pages
3. `api-reference/openapi.json` - Replace with actual API spec
4. New files needed:
   - `api-reference/endpoint/models.mdx`
   - `api-reference/endpoint/models-config.mdx`
   - `api-reference/endpoint/generations.mdx`
   - `api-reference/endpoint/usage.mdx`
   - `api-reference/endpoint/credits-balance.mdx`
5. `essentials/error-handling.mdx` - Add 402 error code for insufficient credits

