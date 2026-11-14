# Inventory Management System
A RESTful backend for Assignment 3 (Web Development III, Unit 3), built with Node.js, Express.js, MongoDB, Mongoose and dotenv.

## Requirements
- Node.js 18 or later
- MongoDB running locally, or a MongoDB Atlas connection string
- npm

## Setup
1. Open this folder in VS Code.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Copy `.env.example` to `.env`.
4. Set `MONGODB_URI` in `.env` to your local MongoDB URL or Atlas connection string.
5. Start the server:
   ```bash
   npm run dev
   ```
6. Open `http://localhost:5000` to check the API status.

## Product fields
- `name`: required string, 2–100 characters
- `description`: optional string, maximum 500 characters
- `category`: required string
- `price`: required number, minimum 0
- `quantity`: required non-negative whole number
- `sku`: required unique string, stored uppercase
- `supplier`: optional string
- `createdAt` and `updatedAt`: automatically generated

## API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/products` | Create a product |
| GET | `/api/products` | List products with filters, sorting and pagination |
| GET | `/api/products/:id` | Get one product |
| PUT | `/api/products/:id` | Update product details |
| DELETE | `/api/products/:id` | Delete a product |
| PATCH | `/api/products/:id/restock` | Increase stock |
| PATCH | `/api/products/:id/sale` | Record a sale and decrease stock |
| GET | `/api/reports/low-stock` | List products at or below threshold |
| GET | `/api/reports/category-summary` | Category-wise aggregation report |

## Example requests

### Create product
`POST http://localhost:5000/api/products`
```json
{
  "name": "Wireless Mouse",
  "description": "2.4 GHz wireless mouse",
  "category": "Accessories",
  "price": 799,
  "quantity": 12,
  "sku": "ACC-MOU-001",
  "supplier": "Tech Supplies"
}
```

### List, filter, sort and paginate
`GET http://localhost:5000/api/products?category=Accessories&minPrice=200&maxPrice=2000&sort=price&order=asc&page=1&limit=5`

Text search:
`GET http://localhost:5000/api/products?search=wireless`

Allowed sort fields: `name`, `price`, `quantity`, `category`, `createdAt`. Allowed order values: `asc`, `desc`.

### Get one product
`GET http://localhost:5000/api/products/PRODUCT_ID`

### Update product
`PUT http://localhost:5000/api/products/PRODUCT_ID`
```json
{
  "price": 849,
  "description": "Updated product description"
}
```

### Restock
`PATCH http://localhost:5000/api/products/PRODUCT_ID/restock`
```json
{
  "quantity": 10
}
```

### Record a sale
`PATCH http://localhost:5000/api/products/PRODUCT_ID/sale`
```json
{
  "quantity": 2
}
```
The sale uses a conditional database update so stock cannot become negative.

### Delete
`DELETE http://localhost:5000/api/products/PRODUCT_ID`

### Low-stock report
`GET http://localhost:5000/api/reports/low-stock`
Optional threshold:
`GET http://localhost:5000/api/reports/low-stock?threshold=10`

Default threshold is `LOW_STOCK_THRESHOLD` from `.env`, or 5 if not configured.

### Category summary
`GET http://localhost:5000/api/reports/category-summary`

The aggregation returns product count, total units, inventory value (price × quantity), and average price for each category.

## Suggested practical test order
1. Create 3–5 products with different categories and quantities.
2. Retrieve all products.
3. Test category filtering, text search, price range, sorting and pagination.
4. Retrieve one product using its MongoDB `_id`.
5. Update a product.
6. Restock a product.
7. Sell some units and try selling more than the available quantity to test the stock safety check.
8. Delete a product.
9. Open the low-stock report.
10. Open the category summary report.

## Notes
- All API responses are JSON.
- Validation, invalid IDs, duplicate SKUs, missing routes and unexpected errors are handled centrally.
- This is a backend-only assignment; no frontend or admin panel is included.
- The assignment PDF specifies the functional requirements but does not prescribe exact product field names, route names, or a particular API testing tool. The choices above are implementation decisions.
