<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Kenny's Online Shop</title>
  <style>
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: linear-gradient(120deg, #e0c3fc 0%, #8ec5fc 100%);
      margin: 0; padding: 0;
    }
    header {
      background: #fff;
      padding: 1rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.07);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    nav button {
      margin: 0 0.5rem;
      padding: 0.6rem 1.2rem;
      border: none;
      background: #8ec5fc;
      color: #fff;
      border-radius: 25px;
      font-size: 1rem;
      cursor: pointer;
      transition: background 0.3s;
    }
    nav button:hover {
      background: #e0c3fc;
      color: #333;
    }
    main {
      max-width: 900px;
      margin: 2rem auto;
      background: #fff;
      border-radius: 20px;
      box-shadow: 0 4px 24px rgba(0,0,0,0.09);
      padding: 2rem;
    }
    #product-list, #cart-list {
      display: flex;
      flex-wrap: wrap;
      gap: 1.5rem;
    }
    .product, .cart-item {
      background: #f6f6f6;
      border-radius: 12px;
      padding: 1rem;
      box-shadow: 0 2px 12px rgba(0,0,0,0.07);
      width: 220px;
      text-align: center;
    }
    .product img {
      width: 120px;
      height: 120px;
      object-fit: contain;
      border-radius: 8px;
      margin-bottom: 0.5rem;
    }
    .product .price {
      color: #8ec5fc;
      font-weight: bold;
      font-size: 1.2rem;
    }
    button.add-cart {
      background: #e0c3fc;
      color: #fff;
      border: none;
      border-radius: 20px;
      padding: 0.4rem 1rem;
      margin-top: 0.5rem;
      cursor: pointer;
      transition: background 0.3s;
    }
    button.add-cart:hover {
      background: #8ec5fc;
    }
    footer {
      text-align: center;
      padding: 1rem;
      color: #666;
      font-size: 0.9rem;
    }
    #analytics-dashboard {
      margin-top: 2rem;
    }
    .analytics-row {
      display: flex;
      gap: 2rem;
      margin-bottom: 1.5rem;
    }
    .analytics-card {
      background: #fcf8e3;
      padding: 1rem 2rem;
      border-radius: 10px;
      box-shadow: 0 2px 8px #8ec5fc33;
      text-align: center;
      min-width: 120px;
    }
    .analytics-card .title {
      font-size: .9rem;
      color: #8ec5fc;
    }
    .analytics-card .value {
      font-size: 1.4rem;
      font-weight: bold;
      color: #e0c3fc;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
    }
    th, td {
      border: 1px solid #e0c3fc;
      padding: 0.5rem;
      text-align: center;
    }
    th {
      background: #f2e5ff;
    }
  </style>
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
  <script>
    // Demo products
    const PRODUCTS = [
      {id: 1, name: "Wireless Headphones", price: 49.99, img: "https://cdn-icons-png.flaticon.com/512/1042/1042459.png"},
      {id: 2, name: "Smart Watch", price: 99.99, img: "https://cdn-icons-png.flaticon.com/512/1042/1042480.png"},
      {id: 3, name: "Sneakers", price: 39.99, img: "https://cdn-icons-png.flaticon.com/512/1042/1042462.png"},
      {id: 4, name: "Backpack", price: 29.99, img: "https://cdn-icons-png.flaticon.com/512/1042/1042490.png"},
      {id: 5, name: "Bluetooth Speaker", price: 44.99, img: "https://cdn-icons-png.flaticon.com/512/1042/1042449.png"}
    ];

    // Cart and sales data
    let cart = [];
    let sales = [];

    // Show sections
    function showSection(section) {
      document.getElementById('shop-section').style.display = section === 'shop' ? '' : 'none';
      document.getElementById('cart-section').style.display = section === 'cart' ? '' : 'none';
      document.getElementById('analytics-section').style.display = section === 'analytics' ? '' : 'none';
      if(section === 'cart') renderCart();
      if(section === 'analytics') renderAnalytics();
    }

    // Render products
    function renderProducts() {
      const list = document.getElementById('product-list');
      list.innerHTML = '';
      PRODUCTS.forEach(product => {
        const el = document.createElement('div');
        el.className = 'product';
        el.innerHTML = `
          <img src="${product.img}" alt="${product.name}">
          <h3>${product.name}</h3>
          <div class="price">$${product.price.toFixed(2)}</div>
          <button class="add-cart" onclick="addToCart(${product.id})">Add to Cart</button>
        `;
        list.appendChild(el);
      });
    }
    renderProducts();

    // Add to cart
    function addToCart(id) {
      const product = PRODUCTS.find(p => p.id === id);
      const item = cart.find(c => c.id === id);
      if (item) {
        item.qty += 1;
      } else {
        cart.push({ ...product, qty: 1 });
      }
      document.getElementById('cart-count').textContent = cart.reduce((sum, c) => sum + c.qty, 0);
      alert('Added to cart!');
    }

    // Render cart
    function renderCart() {
      const list = document.getElementById('cart-list');
      if (cart.length === 0) {
        list.innerHTML = '<p>Your cart is empty.</p>';
        return;
      }
      list.innerHTML = '';
      cart.forEach(item => {
        const el = document.createElement('div');
        el.className = 'cart-item';
        el.innerHTML = `
          <img src="${item.img}" alt="${item.name}">
          <h4>${item.name}</h4>
          <div>Qty: ${item.qty}</div>
          <div>Total: $${(item.qty * item.price).toFixed(2)}</div>
          <button onclick="removeFromCart(${item.id})">Remove</button>
        `;
        list.appendChild(el);
      });
    }

    // Remove from cart
    function removeFromCart(id) {
      cart = cart.filter(c => c.id !== id);
      document.getElementById('cart-count').textContent = cart.reduce((sum, c) => sum + c.qty, 0);
      renderCart();
    }

    // Checkout
    function checkout() {
      if (cart.length === 0) return alert('Cart is empty!');
      // Save sales data
      cart.forEach(item => {
        let sale = sales.find(s => s.id === item.id);
        if (sale) {
          sale.qty += item.qty;
          sale.total += item.qty * item.price;
        } else {
          sales.push({
            id: item.id,
            name: item.name,
            qty: item.qty,
            total: item.qty * item.price,
          });
        }
      });
      cart = [];
      document.getElementById('cart-count').textContent = '0';
      renderCart();
      alert('Thank you for your purchase!');
    }

    // Render analytics (simple dashboard)
    function renderAnalytics() {
      const dashboard = document.getElementById('analytics-dashboard');
      const totalSales = sales.reduce((sum, s) => sum + s.total, 0);
      const totalItems = sales.reduce((sum, s) => sum + s.qty, 0);
      let topProduct = sales.sort((a,b) => b.total - a.total)[0];
      dashboard.innerHTML = `
        <div class="analytics-row">
          <div class="analytics-card">
            <div class="title">Total Sales</div>
            <div class="value">$${totalSales.toFixed(2)}</div>
          </div>
          <div class="analytics-card">
            <div class="title">Items Sold</div>
            <div class="value">${totalItems}</div>
          </div>
          <div class="analytics-card">
            <div class="title">Top Product</div>
            <div class="value">${topProduct ? topProduct.name : '--'}</div>
          </div>
        </div>
        <h3>Sales Breakdown</h3>
        <table>
          <tr><th>Product</th><th>Qty Sold</th><th>Total</th></tr>
          ${sales.map(s => `
            <tr>
              <td>${s.name}</td>
              <td>${s.qty}</td>
              <td>$${s.total.toFixed(2)}</td>
            </tr>
          `).join('')}
        </table>
      `;
    }
  </script>
</body>
</html> 