# Inventory Management System

A RESTful backend application developed for **Assignment 3 – Web Development III (Unit 3)** using Node.js, Express.js, MongoDB, Mongoose, and dotenv.

The system provides APIs for product management, inventory tracking, stock operations, filtering, sorting, pagination, and inventory reports.

## Technology Stack

| Technology | Purpose |
|---|---|
| Node.js | JavaScript runtime |
| Express.js | Backend framework |
| MongoDB | NoSQL database |
| Mongoose | Database modeling and validation |
| dotenv | Environment variable management |
| Postman | API testing |

## Features

- Create, retrieve, update, and delete products.
- Product schema validation.
- Unique SKU validation.
- Retrieve all products.
- Search products by name.
- Filter products by category and price.
- Sort product records.
- Pagination support.
- Restock products.
- Record sales and update stock.
- Prevent stock from becoming negative during sales.
- Generate low-stock reports.
- Generate category-wise inventory summaries.
- Centralized error handling.
- JSON-based REST API responses.

## Project Structure

```text
inventory-management-system/
│
├── config/
├── controllers/
├── middleware/
├── models/
├── routes/
├── screenshots/
│
├── .gitignore
├── package.json
├── package-lock.json
├── server.js
└── README.md
```

## Installation and Setup

### Prerequisites

- Node.js 18 or later
- MongoDB running locally or MongoDB Atlas
- npm

### 1. Clone the Repository

```bash
git clone https://github.com/aradhana-bit/inventory-management-system.git
```

### 2. Navigate to the Project

```bash
cd inventory-management-system
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Configure Environment Variables

Create a `.env` file in the project root directory.

Add your MongoDB connection details:

```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/inventory_management
LOW_STOCK_THRESHOLD=5
```

For MongoDB Atlas, use your Atlas connection string as `MONGODB_URI`.

### 5. Start the Development Server

```bash
npm run dev
```

### 6. Check the API

Open:

```text
http://localhost:5000
```

## Product Schema

| Field | Description |
|---|---|
| `name` | Required string, 2–100 characters |
| `description` | Optional string, maximum 500 characters |
| `category` | Required string |
| `price` | Required number, minimum 0 |
| `quantity` | Required non-negative whole number |
| `sku` | Required unique string, stored uppercase |
| `supplier` | Optional string |
| `createdAt` | Automatically generated timestamp |
| `updatedAt` | Automatically generated timestamp |

## API Endpoints

### Product Management

| HTTP Method | Endpoint | Description |
|---|---|---|
| POST | `/api/products` | Create a product |
| GET | `/api/products` | Retrieve all products |
| GET | `/api/products/:id` | Retrieve a product by ID |
| PUT | `/api/products/:id` | Update product details |
| DELETE | `/api/products/:id` | Delete a product |
| PATCH | `/api/products/:id/restock` | Increase product stock |
| PATCH | `/api/products/:id/sale` | Record a sale and decrease stock |

### Inventory Reports

| HTTP Method | Endpoint | Description |
|---|---|---|
| GET | `/api/reports/low-stock` | Retrieve low-stock products |
| GET | `/api/reports/category-summary` | Generate category-wise inventory summary |

## API Request Examples

### 1. Create Product

**POST** `/api/products`

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

### 2. Retrieve All Products

**GET** `/api/products`

### 3. Retrieve Product by ID

**GET** `/api/products/PRODUCT_ID`

### 4. Update Product

**PUT** `/api/products/PRODUCT_ID`

```json
{
  "price": 849,
  "description": "Updated product description"
}
```

### 5. Restock Product

**PATCH** `/api/products/PRODUCT_ID/restock`

```json
{
  "quantity": 10
}
```

### 6. Record a Sale

**PATCH** `/api/products/PRODUCT_ID/sale`

```json
{
  "quantity": 2
}
```

The sale operation uses a conditional database update to prevent stock from becoming negative.

### 7. Delete Product

**DELETE** `/api/products/PRODUCT_ID`

### 8. Filter Products

```text
GET /api/products?category=Accessories
```

### 9. Search Products

```text
GET /api/products?search=wireless
```

### 10. Filter by Price Range

```text
GET /api/products?minPrice=200&maxPrice=2000
```

### 11. Sort Products

```text
GET /api/products?sort=price&order=asc
```

### 12. Pagination

```text
GET /api/products?page=1&limit=5
```

Allowed sort fields: `name`, `price`, `quantity`, `category`, `createdAt`.

Allowed order values: `asc`, `desc`.

### 13. Low-Stock Report

```text
GET /api/reports/low-stock?threshold=10
```

The default threshold is configured through `LOW_STOCK_THRESHOLD`, or 5 if not configured.

### 14. Category-Wise Summary

```text
GET /api/reports/category-summary
```

The aggregation report returns:

- Product count.
- Total available units.
- Total inventory value.
- Average product price.

## Postman API Testing

The APIs are tested using Postman. The screenshots below document the request methods, responses, and API operations.

### Product Management

| Test Case | Screenshot |
|---|---|
| Create Product | `01-create-product.png` |
| Get All Products | `02-get-all-products.png` |
| Get Product by ID | `03-get-product-by-id.png` |
| Update Product | `04-update-product.png` |
| Restock Product | `05-restock-product.png` |
| Record a Sale | `06-sale-product.png` |
| Delete Product | `07-delete-product.png` |

### Reports and Additional Testing

| Test Case | Screenshot |
|---|---|
| Low-Stock Report | `08-low-stock-report.png` |
| Category Summary | `09-category-summary.png` |
| Category Filtering | `10-filter-category.png` |
| Product Search | `11-search-product.png` |
| Price Filtering | `12-price-filter.png` |
| Sorting | `13-sorting.png` |
| Pagination | `14-pagination.png` |
| Product Validation | `15-validation-test.png` |
| Duplicate SKU Validation | `16-duplicate-sku.png` |

All Postman screenshots are maintained in the `screenshots/` directory.

## Testing Workflow

1. Create multiple products.
2. Retrieve all products.
3. Test category filtering.
4. Test product search.
5. Test price filtering.
6. Test sorting and pagination.
7. Retrieve a product by ID.
8. Update product details.
9. Restock a product.
10. Record a sale.
11. Test stock validation.
12. Delete a product.
13. Generate the low-stock report.
14. Generate the category-wise summary.
15. Test invalid product data.
16. Test duplicate SKU validation.

## Error Handling

- Schema-based product validation.
- Duplicate SKU handling.
- Invalid MongoDB ID handling.
- Missing product handling.
- Missing route handling.
- Centralized error middleware.
- JSON error responses.

## Project Notes

- This is a backend-only application.
- No frontend or admin panel is included.
- MongoDB is used for persistent data storage.
- Express.js manages API routes and middleware.
- Mongoose manages product schemas and validation.
- The application follows a modular backend architecture.

---

## Submitted By

**Name:** Aradhana  
**Roll Number:** 2501010358

**Assignment:** Web Development III – Assignment 3
