<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Web Kasir - Kontrol Barang dengan Serial, Kadaluarsa, dan Pencarian</title>
<link href="https://fonts.googleapis.com/css2?family=Inter&display=swap" rel="stylesheet" />
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
<style>
  /* Reset & base */
  * {
    margin: 0; padding: 0; box-sizing: border-box;
  }
  body {
    font-family: 'Inter', sans-serif;
    background: linear-gradient(135deg, #24243e, #302b63, #24243e);
    color: #eaeaea;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  a, button {
    font-family: inherit;
  }

  /* Header */
  header {
    background-color: rgba(30, 30, 50, 0.95);
    backdrop-filter: saturate(180%) blur(10px);
    color: #f0f0f0;
    padding: 12px 24px;
    position: sticky;
    top: 0;
    z-index: 1100;
    box-shadow: 0 2px 8px rgba(0,0,0,0.65);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .header-title {
    font-weight: 700;
    font-size: 1.5rem;
    letter-spacing: 1.2px;
  }
  .header-actions {
    display: flex;
    align-items: center;
    gap: 12px;
  }
  button.icon-btn {
    background: none;
    border: none;
    color: #eaeaea;
    cursor: pointer;
    font-size: 28px;
    line-height: 1;
    padding: 6px;
    border-radius: 8px;
    transition: background-color 0.3s ease;
  }
  button.icon-btn:hover, button.icon-btn:focus-visible {
    background-color: rgba(127, 105, 249, 0.4);
    outline-offset: 2px;
    outline-color: #7f69f9;
    outline-style: solid;
    outline-width: 2px;
  }

  /* Layout container */
  main {
    flex-grow: 1;
    max-width: 1200px;
    margin: 24px auto 48px;
    padding: 0 24px;
    display: flex;
    flex-direction: column;
  }

  /* Sub-header inside content for page navigation */
  .subpage-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  .subpage-header h2 {
    font-size: 1.75rem;
    border-bottom: 2px solid #7f69f9;
    padding-bottom: 6px;
    font-weight: 700;
  }
  button.back-btn {
    background-image: linear-gradient(135deg, #7f69f9, #4e39ad);
    border: none;
    color: white;
    font-weight: 700;
    font-size: 1rem;
    padding: 8px 20px;
    border-radius: 12px;
    cursor: pointer;
    transition: background-image 0.3s ease, transform 0.3s ease;
  }
  button.back-btn:hover, button.back-btn:focus-visible {
    background-image: linear-gradient(135deg, #4e39ad, #7f69f9);
    transform: translateY(-2px);
    outline-offset: 4px;
    outline-color: #a692f9;
  }

  /* Search container */
  .search-container {
    margin-bottom: 16px;
  }
  .search-container label {
    font-weight: 600;
    margin-bottom: 6px;
    display: block;
  }
  .search-container input[type="text"] {
    padding: 10px 14px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
    background-color: #202040;
    color: #eaeaea;
    width: 100%;
    transition: box-shadow 0.3s ease;
  }
  .search-container input[type="text"]:focus {
    outline: none;
    box-shadow: 0 0 8px #7f69f9;
    background-color: #2c2c58;
  }

  /* Sections hidden by default */
  section.page {
    display: none;
  }
  section.page.active {
    display: block;
  }

  /* Common card style */
  .card {
    background-color: rgba(40, 40, 60, 0.85);
    border-radius: 16px;
    padding: 24px;
    box-shadow: 0 8px 20px rgb(80 72 161 / 0.5);
  }

  /* Forms */
  form {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
    margin-bottom: 24px;
  }
  label {
    flex: 1 1 100%;
    font-weight: 600;
    margin-bottom: 4px;
  }
  input[type="text"],
  input[type="number"],
  select,
  input[type="date"] {
    padding: 10px 14px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
    background-color: #202040;
    color: #eaeaea;
    transition: box-shadow 0.3s ease;
  }
  input[type="text"]:focus,
  input[type="number"]:focus,
  select:focus,
  input[type="date"]:focus {
    outline: none;
    box-shadow: 0 0 8px #7f69f9;
    background-color: #2c2c58;
  }
  button {
    cursor: pointer;
    border-radius: 12px;
    border: none;
    color: white;
    font-weight: 700;
    font-size: 1.05rem;
    background-image: linear-gradient(135deg, #7f69f9, #4e39ad);
    padding: 12px 32px;
    transition: background-image 0.3s ease, transform 0.3s ease;
  }
  button:hover, button:focus-visible {
    background-image: linear-gradient(135deg, #4e39ad, #7f69f9);
    transform: translateY(-2px);
    outline-offset: 4px;
    outline-color: #a692f9;
  }

  /* Inventory List */
  .inventory-list {
    max-height: 380px;
    overflow-y: auto;
    border-radius: 12px;
    border: 1px solid #5555cc;
  }
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    text-align: left;
    padding: 12px 16px;
    border-bottom: 1px solid #444;
    font-size: 0.95rem;
  }
  th {
    background-color: #3a3790;
  }
  tbody tr:hover {
    background-color: #4b4b8f88;
  }
  .btn-small {
    padding: 6px 12px;
    font-size: 0.85rem;
    background-image: linear-gradient(135deg, #ff5555, #bb2b2b);
    user-select: none;
  }
  .btn-small:hover, .btn-small:focus-visible {
    background-image: linear-gradient(135deg, #bb2b2b, #ff5555);
  }
  .btn-disabled {
    background: #555;
    cursor: not-allowed;
    opacity: 0.6;
  }
  .btn-history {
    background-image: linear-gradient(135deg, #4e90f9, #2b52bb);
  }
  .btn-history:hover, .btn-history:focus-visible {
    background-image: linear-gradient(135deg, #2b52bb, #4e90f9);
  }

  /* Checkout */
  .checkout-cart {
    max-height: 300px;
    overflow-y: auto;
    margin-bottom: 24px;
    border-radius: 12px;
    border: 1px solid #5555cc;
  }
  .cart-table th,
  .cart-table td {
    padding: 10px 14px;
    font-size: 0.95rem;
  }
  .total-summary {
    font-size: 1.3rem;
    font-weight: 700;
    text-align: right;
    margin-top: 12px;
    color: #9a98ff;
  }
  .empty-message {
    font-size: 0.9rem;
    color: #a0a0a0;
    text-align: center;
    padding: 40px 0;
  }
  input.qty-input {
    width: 72px;
    padding: 10px 14px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
    background-color: #202040;
    color: #eaeaea;
    text-align: center;
    margin-left: 12px;
  }
  input.qty-input:focus {
    outline: none;
    box-shadow: 0 0 8px #7f69f9;
    background-color: #2c2c58;
  }
  .remove-item-btn {
    background-image: linear-gradient(135deg, #ff5555, #bb2b2b);
    padding: 6px 14px;
    font-weight: 700;
    border-radius: 8px;
    cursor: pointer;
    border: none;
    color: white;
  }
  .remove-item-btn:hover, .remove-item-btn:focus-visible {
    background-image: linear-gradient(135deg, #bb2b2b, #ff5555);
  }

  /* Scrollbar */
  ::-webkit-scrollbar {
    width: 8px;
  }
  ::-webkit-scrollbar-track {
    background: #2c2c58;
    border-radius: 8px;
  }
  ::-webkit-scrollbar-thumb {
    background: #7f69f9;
    border-radius: 8px;
  }

  /* Modal overlay for history */
  .modal-overlay {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background-color: rgba(20, 20, 40, 0.8);
    display: none;
    justify-content: center;
    align-items: center;
    z-index: 1200;
  }
  .modal-overlay.active {
    display: flex;
  }
  .modal-content {
    background-color: rgba(40, 40, 60, 0.95);
    border-radius: 16px;
    max-width: 600px;
    width: 90%;
    max-height: 80vh;
    overflow-y: auto;
    padding: 24px 32px 32px;
    box-shadow: 0 12px 30px rgb(80 72 161 / 0.8);
    position: relative;
  }
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 24px;
  }
  .modal-header h3 {
    margin: 0;
    font-weight: 700;
    font-size: 1.5rem;
  }
  button.modal-close-btn {
    background: none;
    border: none;
    color: #eaeaea;
    font-size: 30px;
    cursor: pointer;
    line-height: 1;
    border-radius: 8px;
    padding: 4px 8px;
    transition: background-color 0.3s ease;
  }
  button.modal-close-btn:hover, button.modal-close-btn:focus-visible {
    background-color: rgba(127, 105, 249, 0.5);
    outline-offset: 2px;
    outline-color: #7f69f9;
    outline-style: solid;
    outline-width: 2px;
  }
  .history-table {
    width: 100%;
    border-collapse: collapse;
  }
  .history-table th, .history-table td {
    padding: 12px 14px;
    border-bottom: 1px solid #555;
    text-align: left;
    font-size: 0.95rem;
  }
  .history-table th {
    background-color: #3a3790;
    position: sticky;
    top: 0;
    z-index: 10;
  }
  .history-type-in {
    color: #4caf50;
    font-weight: 700;
  }
  .history-type-out {
    color: #f44336;
    font-weight: 700;
  }
  .no-history-msg {
    text-align: center;
    padding: 40px 0;
    font-size: 1rem;
    color: #aaa;
  }
</style>
</head>
<body>
<header aria-label="Site header">
  <div class="header-title">Web Kasir &amp; Inventory Control</div>
  <div class="header-actions">
    <button class="icon-btn" id="btnShowInventory" aria-label="Buka halaman kontrol barang">
      <span class="material-icons" aria-hidden="true">settings</span>
    </button>
    <button class="icon-btn" id="btnShowCheckout" aria-label="Buka halaman kasir" style="display:none;">
      <span class="material-icons" aria-hidden="true">shopping_cart</span>
    </button>
  </div>
</header>

<main>
  <!-- Kasir Page -->
  <section id="pageKasir" class="page active card" aria-labelledby="kasirHeading" tabindex="0">
    <h2 id="kasirHeading" style="margin-bottom:20px;">Kasir (Checkout)</h2>
    <form id="addToCartForm" aria-label="Form untuk menambahkan barang ke keranjang" style="display:flex;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:24px;">
      <label for="selectItem" style="flex:1 1 auto; min-width: 180px;">Pilih Barang</label>
      <select id="selectItem" required aria-required="true" style="flex:3 1 auto;min-width:180px;">
        <!-- Options added dynamically -->
      </select>
      <label for="selectQty" style="min-width: 60px;">Jumlah</label>
      <input type="number" id="selectQty" class="qty-input" min="1" value="1" required style="width: 72px;" />
      <button type="submit" aria-label="Tambahkan Barang ke Keranjang" style="flex-shrink: 0;">Tambah ke Keranjang</button>
    </form>

    <div class="checkout-cart" role="region" aria-live="polite" aria-label="Daftar keranjang belanja">
      <table class="cart-table" aria-describedby="kasirHeading" style="width: 100%; border-collapse: collapse;">
        <thead>
          <tr>
            <th scope="col" style="padding: 10px 14px;">Barang</th>
            <th scope="col" style="padding: 10px 14px;">Harga Satuan (Rp)</th>
            <th scope="col" style="padding: 10px 14px;">Jumlah</th>
            <th scope="col" style="padding: 10px 14px;">Subtotal (Rp)</th>
            <th scope="col" style="width: 80px; padding: 10px 14px;">Aksi</th>
          </tr>
        </thead>
        <tbody id="cartTableBody"></tbody>
      </table>
      <div id="emptyCartMessage" class="empty-message">Keranjang kosong</div>
    </div>

    <div class="total-summary" aria-live="polite" aria-atomic="true" style="margin-top:12px;">
      Total: Rp <span id="totalAmount">0</span>
    </div>

    <div style="margin-top: 24px; text-align: right;">
      <button id="completeSaleBtn" aria-label="Selesaikan transaksi pembelian" disabled>Selesaikan Transaksi</button>
      <button id="clearCartBtn" aria-label="Kosongkan keranjang" style="margin-left: 12px;" disabled>Kosongkan Keranjang</button>
    </div>
  </section>

  <!-- Inventory Control Page -->
  <section id="pageInventory" class="page card" aria-labelledby="inventoryHeading" tabindex="0">
    <div class="subpage-header">
      <h2 id="inventoryHeading">Kontrol Barang (Inventory)</h2>
      <button class="back-btn" id="btnBackToCheckout" aria-label="Kembali ke halaman kasir">
        <span class="material-icons" aria-hidden="true" style="vertical-align: middle;">arrow_back</span>
        Kembali
      </button>
    </div>

    <form id="addItemForm" aria-label="Form untuk menambah item ke inventory" style="display:flex; flex-wrap: wrap; gap:16px; margin-bottom: 16px;">
      <div style="flex: 1 1 100%;">
        <label for="itemName">Nama Barang</label>
        <input list="existingItemNames" type="text" id="itemName" name="itemName" required autocomplete="off" placeholder="Contoh: Minuman Botol" />
        <datalist id="existingItemNames">
          <!-- options added dynamically -->
        </datalist>
      </div>
      <div style="flex: 1 1 48%;">
        <label for="itemPrice">Harga per Unit (Rp)</label>
        <input type="number" id="itemPrice" name="itemPrice" min="0" required placeholder="10000" />
      </div>
      <div style="flex: 1 1 48%;">
        <label for="itemQuantity">Jumlah Masuk</label>
        <input type="number" id="itemQuantity" name="itemQuantity" min="1" required placeholder="10" />
      </div>
      <div style="flex: 1 1 48%;">
        <label for="itemSerialCode">Kode Seri Barang</label>
        <input type="text" id="itemSerialCode" name="itemSerialCode" required placeholder="Contoh: SN123456" autocomplete="off" />
      </div>
      <div style="flex: 1 1 48%;">
        <label for="itemExpiryDate">Tanggal Kadaluarsa</label>
        <input type="date" id="itemExpiryDate" name="itemExpiryDate" required />
      </div>
      <div style="flex: 1 1 100%; text-align: right;">
        <label for="searchInventory" style="margin-right: 12px; font-weight: 600;">Cari Nama Barang</label>
        <input type="text" id="searchInventory" placeholder="Cari barang di bawah..." aria-label="Pencarian nama barang pada daftar inventori" style="max-width: 300px;" />
      </div>
      <div style="flex: 1 1 100%; text-align: right; margin-top: 12px;">
        <button type="submit" aria-label="Tambah Barang Baru ke Inventory">Tambah Barang</button>
      </div>
    </form>

    <div class="inventory-list" role="region" aria-live="polite" aria-label="Daftar inventori barang">
      <table aria-describedby="inventoryHeading">
        <thead>
          <tr>
            <th scope="col">Nama Barang</th>
            <th scope="col">Kode Seri</th>
            <th scope="col">Kadaluarsa</th>
            <th scope="col">Harga (Rp)</th>
            <th scope="col">Stok</th>
            <th scope="col" style="width: 120px;">Kelola</th>
          </tr>
        </thead>
        <tbody id="inventoryTableBody">
          <!-- Dynamic rows here -->
        </tbody>
      </table>
    </div>
  </section>
</main>

<!-- Modal for stock history -->
<div id="modalStockHistory" class="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modalStockHistoryTitle" tabindex="-1">
  <div class="modal-content">
    <div class="modal-header">
      <h3 id="modalStockHistoryTitle">Riwayat Stok</h3>
      <button class="modal-close-btn" aria-label="Tutup riwayat stok">&times;</button>
    </div>
    <div id="modalStockHistoryBody">
      <!-- History content populated dynamically -->
      <p class="no-history-msg">Memuat riwayat...</p>
    </div>
  </div>
</div>

<footer aria-label="Site footer">
  &copy; 2024 Web Kasir &amp; Inventory Control - All rights reserved
</footer>

<script>
  (function () {
    // Utility to format number as IDR currency
    function formatIDR(number) {
      return number.toLocaleString('id-ID');
    }
    // Format date-time human readable
    function formatDate(dateStr) {
      const d = new Date(dateStr);
      if (isNaN(d)) return '';
      return d.toLocaleString('id-ID', {
        year: 'numeric', month: 'short', day: 'numeric',
        hour: '2-digit', minute: '2-digit', second: '2-digit'
      });
    }
    // Format expiry date only (date only, local Indonesian)
    function formatExpiry(dateStr) {
      if (!dateStr) return '';
      const d = new Date(dateStr);
      if (isNaN(d)) return '';
      return d.toLocaleDateString('id-ID', {
        year: 'numeric', month: 'long', day: 'numeric'
      });
    }

    // DOM elements
    const inventoryTableBody = document.getElementById('inventoryTableBody');
    const addItemForm = document.getElementById('addItemForm');
    const itemNameInput = document.getElementById('itemName');
    const existingItemNamesDatalist = document.getElementById('existingItemNames');
    const searchInventoryInput = document.getElementById('searchInventory');
    const selectItem = document.getElementById('selectItem');
    const selectQty = document.getElementById('selectQty');
    const addToCartForm = document.getElementById('addToCartForm');
    const cartTableBody = document.getElementById('cartTableBody');
    const totalAmountSpan = document.getElementById('totalAmount');
    const completeSaleBtn = document.getElementById('completeSaleBtn');
    const clearCartBtn = document.getElementById('clearCartBtn');
    const emptyCartMessage = document.getElementById('emptyCartMessage');

    const pageKasir = document.getElementById('pageKasir');
    const pageInventory = document.getElementById('pageInventory');
    const btnShowInventory = document.getElementById('btnShowInventory');
    const btnShowCheckout = document.getElementById('btnShowCheckout');
    const btnBackToCheckout = document.getElementById('btnBackToCheckout');

    // Modal elements
    const modal = document.getElementById('modalStockHistory');
    const modalCloseBtn = modal.querySelector('.modal-close-btn');
    const modalBody = document.getElementById('modalStockHistoryBody');
    const modalTitle = document.getElementById('modalStockHistoryTitle');

    // Storage keys
    const STORAGE_KEYS = {
      inventory: "webKasirInventory",
      salesHistory: "webKasirSalesHistory"
    };

    // Load saved data
    let inventory = JSON.parse(localStorage.getItem(STORAGE_KEYS.inventory)) || [];
    inventory.forEach(item => {
      if (!Array.isArray(item.history)) item.history = [];
    });
    let salesHistory = JSON.parse(localStorage.getItem(STORAGE_KEYS.salesHistory)) || [];
    let cart = [];

    // Save functions
    function saveInventory() {
      localStorage.setItem(STORAGE_KEYS.inventory, JSON.stringify(inventory));
    }
    function saveSalesHistory() {
      localStorage.setItem(STORAGE_KEYS.salesHistory, JSON.stringify(salesHistory));
    }

    // Add stock history entry helper
    function addStockHistory(itemIndex, type, qty, note) {
      if (!inventory[itemIndex]) return;
      if (!Array.isArray(inventory[itemIndex].history))
        inventory[itemIndex].history = [];

      inventory[itemIndex].history.unshift({
        timestamp: new Date().toISOString(),
        type,
        quantity: qty,
        note
      });
      saveInventory();
    }

    // Update datalist for existing item names
    function updateDatalist() {
      existingItemNamesDatalist.innerHTML = '';
      const names = [...new Set(inventory.map(item => item.name))];
      names.forEach(name => {
        const option = document.createElement('option');
        option.value = name;
        existingItemNamesDatalist.appendChild(option);
      });
    }

    // Render Inventory Table
    function renderInventory(filter = '') {
      inventoryTableBody.innerHTML = "";
      const filteredInventory = inventory.filter(item => item.name.toLowerCase().includes(filter.toLowerCase()));
      if (filteredInventory.length === 0) {
        inventoryTableBody.innerHTML = `<tr><td colspan="6" style="text-align:center; padding: 20px; color: #bbb;">Tidak ada barang dalam inventori.</td></tr>`;
        updateSelectOptions();
        updateDatalist();
        return;
      }
      filteredInventory.forEach((item, idxFiltered) => {
        // Need the index in original inventory to correctly handle events
        const idx = inventory.indexOf(item);
        const expiryFormatted = formatExpiry(item.expiryDate);
        const serialCode = item.serialCode || '';
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${item.name}</td>
          <td>${serialCode}</td>
          <td>${expiryFormatted}</td>
          <td>${formatIDR(item.price)}</td>
          <td>${item.quantity}</td>
          <td>
            <button class="btn-small" data-action="removeStock" data-idx="${idx}" aria-label="Kurangi stok barang ${item.name}">-</button>
            <button class="btn-small" data-action="addStock" data-idx="${idx}" aria-label="Tambah stok barang ${item.name}" style="margin-left:4px;">+</button>
            <button class="btn-small btn-history" data-action="showHistory" data-idx="${idx}" aria-label="Tampilkan riwayat stok barang ${item.name}" style="margin-left:8px;">Riwayat</button>
          </td>
        `;
        inventoryTableBody.appendChild(tr);
      });
      updateSelectOptions();
      updateDatalist();
    }

    // Update Select Options for Checkout
    function updateSelectOptions() {
      selectItem.innerHTML = '';
      const defaultOption = document.createElement('option');
      defaultOption.value = '';
      defaultOption.disabled = true;
      defaultOption.selected = true;
      defaultOption.textContent = 'Pilih barang...';
      selectItem.appendChild(defaultOption);

      inventory.forEach((item, idx) => {
        if (item.quantity > 0) {
          const option = document.createElement('option');
          option.value = idx;
          option.textContent = `${item.name} - Stok: ${item.quantity} - Rp ${formatIDR(item.price)}`;
          selectItem.appendChild(option);
        }
      });

      const disableForm = selectItem.options.length < 2;
      selectItem.disabled = disableForm;
      selectQty.disabled = disableForm;
      addToCartForm.querySelector('button[type="submit"]').disabled = disableForm;
      if (disableForm) selectQty.value = 1;
    }

    // Render Cart
    function renderCart() {
      cartTableBody.innerHTML = "";

      if (cart.length === 0) {
        emptyCartMessage.style.display = "block";
        cartTableBody.style.display = "none";
        completeSaleBtn.disabled = true;
        clearCartBtn.disabled = true;
        totalAmountSpan.textContent = '0';
        return;
      }

      emptyCartMessage.style.display = "none";
      cartTableBody.style.display = "table-row-group";
      completeSaleBtn.disabled = false;
      clearCartBtn.disabled = false;

      let total = 0;
      cart.forEach((cartItem, idx) => {
        const inventoryItem = inventory[cartItem.index];
        const price = inventoryItem.price;
        const subTotal = price * cartItem.qty;
        total += subTotal;

        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${inventoryItem.name}</td>
          <td>${formatIDR(price)}</td>
          <td>${cartItem.qty}</td>
          <td>${formatIDR(subTotal)}</td>
          <td><button class="remove-item-btn" data-cart-idx="${idx}" aria-label="Hapus ${inventoryItem.name} dari keranjang">Hapus</button></td>
        `;
        cartTableBody.appendChild(tr);
      });
      totalAmountSpan.textContent = total;
    }

    // Add Item Form Submission - to Inventory
    addItemForm.addEventListener('submit', event => {
      event.preventDefault();
      const name = addItemForm.itemName.value.trim();
      const price = Number(addItemForm.itemPrice.value);
      const quantity = Number(addItemForm.itemQuantity.value);
      const serialCode = addItemForm.itemSerialCode.value.trim();
      const expiryDate = addItemForm.itemExpiryDate.value;

      if (!name || price < 0 || quantity < 1 || !serialCode || !expiryDate) {
        alert('Mohon lengkapi semua kolom termasuk kode seri dan tanggal kadaluarsa.');
        return;
      }

      const existingIndex = inventory.findIndex(i => i.name.toLowerCase() === name.toLowerCase());
      if (existingIndex !== -1) {
        inventory[existingIndex].price = price;
        inventory[existingIndex].quantity += quantity;
        inventory[existingIndex].serialCode = serialCode;
        inventory[existingIndex].expiryDate = expiryDate;
        addStockHistory(existingIndex, 'in', quantity, 'Tambah stok via kontrol barang');
      } else {
        const newItem = {
          name,
          price,
          quantity,
          serialCode,
          expiryDate,
          history: [{
            timestamp: new Date().toISOString(),
            type: 'in',
            quantity,
            note: 'Tambah stok awal saat barang ditambahkan'
          }]
        };
        inventory.push(newItem);
      }
      saveInventory();
      renderInventory(searchInventoryInput.value);
      addItemForm.reset();
      addItemForm.itemName.focus();
    });

    // Inventory Table Buttons (Add/remove stock / show history)
    inventoryTableBody.addEventListener('click', e => {
      const button = e.target.closest('button');
      if (!button) return;
      const idx = Number(button.dataset.idx);
      if (isNaN(idx) || idx < 0 || idx >= inventory.length) return;
      const action = button.dataset.action;
      if (action === 'removeStock') {
        if (inventory[idx].quantity > 0) {
          inventory[idx].quantity -= 1;
          addStockHistory(idx, 'out', 1, 'Kurangi stok via kontrol barang');
          saveInventory();
          renderInventory(searchInventoryInput.value);
          validateCart();
        }
      } else if (action === 'addStock') {
        inventory[idx].quantity += 1;
        addStockHistory(idx, 'in', 1, 'Tambah stok via kontrol barang');
        saveInventory();
        renderInventory(searchInventoryInput.value);
      } else if (action === 'showHistory') {
        showStockHistoryModal(idx);
      }
    });

    // Add to Cart Form Submission
    addToCartForm.addEventListener('submit', e => {
      e.preventDefault();
      const selectedIndex = Number(selectItem.value);
      const qty = Number(selectQty.value);

      if (isNaN(selectedIndex) || selectedIndex < 0 || qty < 1) return;

      const product = inventory[selectedIndex];
      if (!product || product.quantity < qty) {
        alert('Stok barang tidak cukup.');
        return;
      }

      const existingCartIndex = cart.findIndex(c => c.index === selectedIndex);
      if (existingCartIndex >= 0) {
        let newQty = cart[existingCartIndex].qty + qty;
        if (newQty > product.quantity) {
          alert('Jumlah di keranjang melebihi stok tersedia.');
          return;
        }
        cart[existingCartIndex].qty = newQty;
      } else {
        cart.push({ index: selectedIndex, qty });
      }
      renderCart();
      addToCartForm.reset();
    });

    // Remove item from cart
    cartTableBody.addEventListener('click', e => {
      const btn = e.target.closest('button');
      if (!btn) return;
      const cartIdx = Number(btn.dataset.cartIdx);
      if (isNaN(cartIdx) || cartIdx < 0 || cartIdx >= cart.length) return;

      cart.splice(cartIdx, 1);
      renderCart();
    });

    // Clear Cart
    clearCartBtn.addEventListener('click', () => {
      cart = [];
      renderCart();
    });

    // Complete Sale
    completeSaleBtn.addEventListener('click', () => {
      if (cart.length === 0) return;

      for (const cartItem of cart) {
        const invItem = inventory[cartItem.index];
        invItem.quantity -= cartItem.qty;
        addStockHistory(cartItem.index, 'out', cartItem.qty, 'Penjualan via kasir');
      }
      const saleRecord = {
        id: Date.now(),
        timestamp: new Date().toISOString(),
        items: cart.map(c => ({
          name: inventory[c.index].name,
          price: inventory[c.index].price,
          qty: c.qty
        }))
      };
      salesHistory.push(saleRecord);

      saveInventory();
      saveSalesHistory();

      cart = [];
      renderInventory(searchInventoryInput.value);
      renderCart();
      alert("Transaksi berhasil diselesaikan.");
    });

    // Validate cart items if inventory changed
    function validateCart() {
      let changed = false;
      cart = cart.filter(cartItem => {
        const invQty = inventory[cartItem.index]?.quantity ?? 0;
        if (cartItem.qty > invQty) {
          changed = true;
          return false;
        }
        return true;
      });
      if (changed) renderCart();
    }

    // Navigation buttons - show inventory
    btnShowInventory.addEventListener('click', () => {
      pageKasir.classList.remove('active');
      pageInventory.classList.add('active');
      btnShowInventory.style.display = 'none';
      btnShowCheckout.style.display = '';
      pageInventory.focus();
    });
    // Show checkout page
    btnShowCheckout.addEventListener('click', () => {
      pageInventory.classList.remove('active');
      pageKasir.classList.add('active');
      btnShowInventory.style.display = '';
      btnShowCheckout.style.display = 'none';
      pageKasir.focus();
    });
    // Back button on inventory page
    btnBackToCheckout.addEventListener('click', () => {
      btnShowCheckout.click();
    });

    // Modal control functions
    function showStockHistoryModal(itemIndex) {
      const item = inventory[itemIndex];
      if (!item) return;

      modalTitle.textContent = `Riwayat Stok: ${item.name}`;
      if (!item.history || item.history.length === 0) {
        modalBody.innerHTML = `<p class="no-history-msg">Belum ada riwayat stok untuk barang ini.</p>`;
      } else {
        let html = `<table class="history-table" aria-describedby="modalStockHistoryTitle">
          <thead>
            <tr>
              <th scope="col">Tanggal &amp; Waktu</th>
              <th scope="col">Tipe</th>
              <th scope="col">Jumlah</th>
              <th scope="col">Keterangan</th>
            </tr>
          </thead>
          <tbody>`;
        item.history.forEach(entry => {
          html += `<tr>
            <td>${formatDate(entry.timestamp)}</td>
            <td class="${entry.type === 'in' ? 'history-type-in' : 'history-type-out'}">${entry.type === 'in' ? 'Masuk' : 'Keluar'}</td>
            <td>${entry.quantity}</td>
            <td>${entry.note || '-'}</td>
          </tr>`;
        });
        html += '</tbody></table>';
        modalBody.innerHTML = html;
      }
      modal.classList.add('active');
      modal.focus();
    }
    function closeModal() {
      modal.classList.remove('active');
      pageInventory.focus();
    }
    modalCloseBtn.addEventListener('click', closeModal);
    modal.addEventListener('click', e => {
      if (e.target === modal) closeModal();
    });
    window.addEventListener('keydown', e => {
      if (e.key === 'Escape' && modal.classList.contains('active')) {
        closeModal();
      }
    });

    // Search filter for inventory table
    searchInventoryInput.addEventListener('input', (e) => {
      const query = e.target.value.trim().toLowerCase();
      renderInventory(query);
    });

    // Initial render
    renderInventory();
    renderCart();

  })();
</script>
</body>
</html>

