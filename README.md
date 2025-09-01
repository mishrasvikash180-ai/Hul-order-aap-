import React, { useState } from "react";

export default function HULOrderApp() {
  const products = [
    { id: 1, name: "Surf Excel (1kg)", price: 120 },
    { id: 2, name: "Lux Soap (4 pack)", price: 60 },
    { id: 3, name: "Dove Shampoo (180ml)", price: 110 },
    { id: 4, name: "Wheel Detergent (1kg)", price: 55 },
    { id: 5, name: "Pepsodent Toothpaste (150g)", price: 95 },
  ];

  const [cart, setCart] = useState([]);
  const [orderPlaced, setOrderPlaced] = useState(false);

  const addToCart = (product) => {
    setCart((prevCart) => {
      const existing = prevCart.find((item) => item.id === product.id);
      if (existing) {
        return prevCart.map((item) =>
          item.id === product.id ? { ...item, qty: item.qty + 1 } : item
        );
      } else {
        return [...prevCart, { ...product, qty: 1 }];
      }
    });
    setOrderPlaced(false);
  };

  const removeFromCart = (id) => {
    setCart((prevCart) =>
      prevCart
        .map((item) =>
          item.id === id ? { ...item, qty: item.qty - 1 } : item
        )
        .filter((item) => item.qty > 0)
    );
  };

  const placeOrder = () => {
    setOrderPlaced(true);
    setCart([]);
  };

  const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);

  return (
    <div className="p-6 max-w-2xl mx-auto">
      <h1 className="text-2xl font-bold mb-4">🛒 HUL Product Order App</h1>

      {/* Product List */}
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        {products.map((p) => (
          <div key={p.id} className="border rounded-xl shadow p-4 flex justify-between items-center">
            <div>
              <p className="font-semibold">{p.name}</p>
              <p className="text-sm text-gray-600">₹{p.price}</p>
            </div>
            <button
              onClick={() => addToCart(p)}
              className="px-3 py-1 bg-blue-500 text-white rounded-lg"
            >
              Add
            </button>
          </div>
        ))}
      </div>

      {/* Cart */}
      <div className="mt-6 p-4 border rounded-lg shadow">
        <h2 className="text-xl font-semibold mb-2">🛍️ Cart</h2>
        {cart.length === 0 ? (
          <p className="text-gray-500">Cart is empty</p>
        ) : (
          <>
            <ul className="mb-2">
              {cart.map((item) => (
                <li
                  key={item.id}
                  className="flex justify-between items-center border-b py-2"
                >
                  <span>
                    {item.name} (x{item.qty})
                  </span>
                  <div className="flex items-center gap-2">
                    <button
                      className="px-2 py-1 bg-red-500 text-white rounded"
                      onClick={() => removeFromCart(item.id)}
                    >
                      -
                    </button>
                    <span>₹{item.price * item.qty}</span>
                    <button
                      className="px-2 py-1 bg-green-500 text-white rounded"
                      onClick={() => addToCart(item)}
                    >
                      +
                    </button>
                  </div>
                </li>
              ))}
            </ul>
            <p className="font-semibold mb-2">Total: ₹{total}</p>
            <button
              onClick={placeOrder}
              className="px-4 py-2 bg-purple-600 text-white rounded-lg"
            >
              Place Order
            </button>
          </>
        )}
      </div>

      {/* Order Placed Message */}
      {orderPlaced && (
        <div className="mt-4 p-4 bg-green-100 border border-green-400 text-green-700 rounded">
          ✅ Your order has been placed successfully!
        </div>
      )}
    </div>
  );
}
