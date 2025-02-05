# FashioNest - E-commerce Platform

FashioNest is an e-commerce website for selling clothes, built using the **MERN stack** with **Next.js** as the frontend framework. The project integrates **Sanity.io** as the CMS for managing products and uses **Redux** for state management, including the shopping cart functionality.

## 🚀 Features

- **User Authentication** (Login/Register) Using Clerk Auth
- **Product Listings** (Fetched from Sanity.io)
- **Add to Cart** (Using Redux for state management)
- **Cart Management** (Update quantity, remove items)
- **Checkout Process** (Stripe Integration)

## 🛠️ Tech Stack

- **Frontend:** Next.js, React, Tailwind CSS
- **CMS:** Sanity.io
- **State Management:** Redux Toolkit
- **Payment Gateway:** Stripe
- **Authentication:** Clerk

---

## 🔧 Installation & Setup

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/SHAJAR5110/FashioNest-Ecommerce.git
cd FashioNest
```

### 2️⃣ Install Dependencies
```bash
npm install
```

### 3️⃣ Set Up Environment Variables
Create a `.env.local` file in the root directory and add:
```env
NEXT_PUBLIC_SANITY_PROJECT_ID=your_sanity_project_id
NEXT_PUBLIC_SANITY_DATASET=production
STRIPE_SECRET_KEY=your_stripe_secret_key
```

### 4️⃣ Run the Development Server
```bash
npm run dev
```
The project will be available at **http://localhost:3000**

---

## 🛍️ Fetching Products from Sanity.io

### **Setup Sanity Client**
Create a file `sanityClient.js`:
```javascript
import { createClient } from "@sanity/client";

export const client = createClient({
  projectId: process.env.NEXT_PUBLIC_SANITY_PROJECT_ID,
  dataset: process.env.NEXT_PUBLIC_SANITY_DATASET,
  useCdn: true,
  apiVersion: "2023-01-01",
});
```

### **Fetching Products in Next.js**
```javascript
import { client } from "../sanityClient";

export async function getServerSideProps() {
  const query = `*[_type == "product"]`;
  const products = await client.fetch(query);

  return { props: { products } };
}
```

---

## 🛒 Add to Cart Functionality (Redux)

### **Redux Store Setup**
Create `redux/cartSlice.js`:
```javascript
import { createSlice } from "@reduxjs/toolkit";

const cartSlice = createSlice({
  name: "cart",
  initialState: [],
  reducers: {
    add: (state, action) => {
      const item = state.find((p) => p._id === action.payload._id);
      if (item) item.quantity += 1;
      else state.push({ ...action.payload, quantity: 1 });
    },
    remove: (state, action) => state.filter((p) => p._id !== action.payload),
    updateQuantity: (state, action) => {
      const item = state.find((p) => p._id === action.payload._id);
      if (item) item.quantity = action.payload.quantity;
    },
  },
});

export const { add, remove, updateQuantity } = cartSlice.actions;
export default cartSlice.reducer;
```

### **Use Redux in Next.js**
In `_app.js`:
```javascript
import { Provider } from "react-redux";
import store from "../redux/store";

function MyApp({ Component, pageProps }) {
  return (
    <Provider store={store}>
      <Component {...pageProps} />
    </Provider>
  );
}

export default MyApp;
```

### **Add to Cart Button in Product Page**
```javascript
import { useDispatch } from "react-redux";
import { add } from "../redux/cartSlice";

const ProductCard = ({ product }) => {
  const dispatch = useDispatch();
  return (
    <div>
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => dispatch(add(product))}>Add to Cart</button>
    </div>
  );
};

export default ProductCard;
```

---

## 📦 Deployment

### **1. Build the Project**
```bash
npm run build
```

### **2. Deploy on Vercel**
- Install Vercel CLI: `npm install -g vercel`
- Run `vercel` and follow the instructions.

---

## 📜 License
This project is licensed under the **MIT License**.

---

## 👨‍💻 Author
- **Your Name** - [GitHub Profile](https://github.com/SHAJAR5110)

---

### 🎉 Happy Coding! 🚀

