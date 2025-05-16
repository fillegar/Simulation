# 🛒 ShoppingCartService – API Simulation Example

This repository provides a complete example of simulating and testing a simple shopping cart API using Tricentis API Simulation. It includes the simulation YAML, a contract test, and a data file that tracks shopping cart state.

---

## 📁 Files

| File | Description |
|------|-------------|
| `simpleState.yml` | The simulation definition that handles POST, PUT, GET, and DELETE operations on a shopping cart |
| `simpleStateTest.yml` | A contract test that validates the simulation's behavior from an external perspective |
| `cart.csv` | A backing table resource used to persist cart data across requests |

---

## 📦 Simulated API Endpoints

### `POST /cart/create`
- **Purpose:** Creates a new shopping cart with a unique `cartId`
- **Behavior:** 
  - Random 5-digit ID is generated
  - Row is inserted into the `cart` table with empty item data
- **Response:**
  ```json
  {
    "message": "Cart Created",
    "cartId": "12345"
  }
  ```

---

### `PUT /cart/{cartId}`
- **Purpose:** Updates the contents of a specific cart
- **Expected Payload:**
  ```json
  {
    "itemId": "12345",
    "itemDesc": "nothing",
    "quantity": 1
  }
  ```
- **Behavior:** Updates the matching row in the `cart` table
- **Response:** Echoes back the updated values

---

### `GET /cart/{cartId}`
- **Purpose:** Retrieves cart data for the specified ID
- **Response Example:**
  ```json
  {
    "message": "Cart Details",
    "cartId": "12345",
    "itemId": "abc",
    "itemDesc": "Widget",
    "quantity": 2
  }
  ```

---

### `DELETE /cart/{cartId}`
- **Purpose:** Removes the specified cart from the data store
- **Response:**
  ```json
  {
    "message": "Cart Deleted",
    "cartId": "12345"
  }
  ```

---

## ✅ Contract Test

The `simpleStateTest.yml` file includes 4 tests:
1. **Create Cart** – Verifies `POST /cart/create`
2. **Update Cart** – Verifies `PUT /cart/{cartId}`
3. **Get Cart** – Verifies `GET /cart/{cartId}`
4. **Delete Cart** – Verifies `DELETE /cart/{cartId}`

📝 *Note: You'll need to manually insert a valid `cartId` where `CARTID` is used in the test script.*

---

## 📚 Resource Table: `cart.csv`

The simulation reads from and writes to a CSV-based resource, defined as:

| cartId | itemId | itemDesc | quantity |
|--------|--------|----------|----------|
| string | string | string   | integer  |

---

## 🛠 How to Use

1. Start the API Simulator and load `simpleState.yml`
2. Open the API Playground or a test runner and run `simpleStateTest.yml`
3. Observe responses and verify changes in `cart.csv` if desired

---

## 💡 Highlights

- Uses dynamic value generation (`{RND[5]}`) and path extraction (`{XB[cartId]}`)
- Demonstrates stateful behavior using a backing resource
- Provides both simulation and test definition for full workflow coverage

---

🔒 All examples are sanitized and safe for public sharing. You’re welcome to extend this to simulate more complex e-commerce scenarios.
