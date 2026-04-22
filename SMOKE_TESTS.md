# Manual Smoke Tests

This file provides quick manual checks for M2: POST add product.

## Prerequisites

1. API is running on http://localhost:4000
2. SQL Server is running and schema is deployed

## Test 1: Happy path (product + metadata, no category links)

Command:

curl -i -X POST "http://localhost:4000/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Trail Running Shoe",
    "description": "Lightweight shoe for mixed terrain",
    "productImageUris": ["https://img.example.com/shoe-1.jpg"],
    "validSkus": ["TRAIL-001", "TRAIL-002"],
    "metadata": [
      { "key": "Color", "value": "Black" },
      { "key": "Brand", "value": "Sparc" }
    ],
    "categoryIds": []
  }'

Expected:

1. HTTP status 201 Created
2. Response body contains ProductId with a positive integer

## Test 2: Validation failure (missing required Name)

Command:

curl -i -X POST "http://localhost:4000/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "",
    "description": "Should fail",
    "productImageUris": [],
    "validSkus": [],
    "metadata": [],
    "categoryIds": []
  }'

Expected:

1. HTTP status 400 Bad Request
2. Response message indicates Name is required

## Test 3: Validation failure (duplicate metadata key)

Command:

curl -i -X POST "http://localhost:4000/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "City Backpack",
    "description": "Should fail duplicate metadata keys",
    "productImageUris": [],
    "validSkus": ["PACK-001"],
    "metadata": [
      { "key": "Color", "value": "Green" },
      { "key": "Color", "value": "Blue" }
    ],
    "categoryIds": []
  }'

Expected:

1. HTTP status 400 Bad Request
2. Response message indicates duplicate metadata key is not allowed

## Test 4: Validation failure (category does not exist)

Command:

curl -i -X POST "http://localhost:4000/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Desk Lamp",
    "description": "Should fail due to missing category",
    "productImageUris": [],
    "validSkus": ["LAMP-001"],
    "metadata": [
      { "key": "Color", "value": "White" }
    ],
    "categoryIds": [999999]
  }'

Expected:

1. HTTP status 400 Bad Request
2. Response message indicates category id does not exist

## Test 5: Search by general text

Command:

curl -i "http://localhost:4000/api/v1/products/search?q=Trail"

Expected:

1. HTTP status 200 OK
2. Response body contains Products array (possibly empty)

## Test 6: Search by metadata and category

Command:

curl -i "http://localhost:4000/api/v1/products/search?metadataKey=Color&metadataValue=Black&categoryIds=1,2"

Expected:

1. HTTP status 200 OK when filters are valid
2. Products only include items matching metadata and at least one listed category

## Test 7: Search validation failure for bad category list

Command:

curl -i "http://localhost:4000/api/v1/products/search?categoryIds=1,abc"

Expected:

1. HTTP status 400 Bad Request
2. Response message indicates categoryIds must be comma-separated positive integers
