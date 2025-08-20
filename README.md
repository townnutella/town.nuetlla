<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Nutella Market</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; background: #fffaf6; color:#333; }
    h1 { color: #6b3e2e; }
    .product { border:1px solid #ddd; padding:10px; border-radius:8px; margin:10px 0; }
    button { background:#6b3e2e; color:#fff; border:none; padding:8px 12px; border-radius:6px; cursor:pointer; }
    #cart { margin:20px 0; font-weight:bold; }
    form { border:1px solid #ddd; padding:10px; border-radius:8px; background:#fff; max-width:300px; }
    input, select { width:100%; padding:8px; margin:5px 0; border:1px solid #ccc; border-radius:5px; }
  </style>
</head>
<body>
  <h1>Nutella Market</h1>

  <div class="product">
    <h3>Nutella Jar 350g - $4.99</h3>
    <button onclick="addToCart(4.99)">Add to Cart</button>
  </div>
  <div class="product">
    <h3>Nutella Jar 750g - $8.99</h3>
    <button onclick="addToCart(8.99)">Add to Cart</button>
  </div>

  <div id="cart">Cart Total: $0.00</div>

  <h2>Checkout</h2>
  <form onsubmit="submitOrder(event)">
    <input type="text" placeholder="Full Name" required>
    <input type="email" placeholder="Email" required>
    <input type="text" placeholder="Address">
    <select required>
      <option value="">Payment Method</option>
      <option>Cash on Delivery</option>
      <option>Card</option>
    </select>
    <button type="submit">Place Order</button>
  </form>

  <script>
    let total = 0;
    function addToCart(price) {
      total += price;
      document.getElementById('cart').innerText = "Cart Total: $" + total.toFixed(2);
    }
    function submitOrder(e){
      e.preventDefault();
      alert("Order placed! Total: $" + total.toFixed(2));
    }
  </script>
</body>
</html>
