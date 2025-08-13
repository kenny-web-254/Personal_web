<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Kenny's Online Shop</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Kenny's Shop</h1>
    <nav>
      <button onclick="showSection('shop')">Shop</button>
      <button onclick="showSection('cart')">Cart (<span id="cart-count">0</span>)</button>
      <button onclick="showSection('analytics')">Analytics</button>
    </nav>
  </header>
  <main>
    <!-- Shop Section -->
    <section id="shop-section">
      <h2>Products</h2>
      <div id="product-list"></div>
    </section>
    <!-- Cart Section -->
    <section id="cart-section" style="display:none;">
      <h2>Your Cart</h2>
      <div id="cart-list"></div>
      <button onclick="checkout()">Checkout</button>
    </section>
    <!-- Analytics Section -->
    <section id="analytics-section" style="display:none;">
      <h2>Sales Analytics</h2>
      <div id="analytics-dashboard"></div>
    </section>
  </main>
  <footer>
    <p>&copy; Kenny's Shop 2025</p>
  </footer>
  <script src="shop.js"></script>
</body>
</html>