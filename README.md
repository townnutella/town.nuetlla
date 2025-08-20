<html lang="en"> 
<head>
  <meta charset="UTF-8">
  <title>Town Nutella</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0; padding: 0;
      background: white;
      color: #333;
    }
    header {
      background: #b30000;
      color: white;
      padding: 10px;
      text-align: center;
      border: 3px solid #b30000; /* added border */
      border-radius: 10px; /* optional rounded corners */
    }
    header img {
      width: 250px; height: 250px; /* bigger logo */
      border-radius: 0%;
      display: block;
      margin: 20px auto; /* centered */
    }
    header h1 {
      margin: 0;
      font-size: 1.5em;
    }
    .menu {
      display: grid;
      grid-template-columns: repeat(auto-fill,minmax(150px,1fr));
      gap: 10px;
      padding: 10px;
    }
    .item {
      background: black;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      padding: 10px;
      text-align: center;
      cursor: pointer;
      transition: transform 0.2s;
      color: white;
    }
    .item:hover { transform: scale(1.05); }
    .item img {
      width: 100%; height: 100px; object-fit: cover;
      border-radius: 8px;
    }
    .cart {
      background: white;
      padding: 15px;
      border-top: 2px solid #ddd;
      position: sticky;
      bottom: 0;
    }
    .cart h2 { margin-top: 0; font-size: 1.2em; }
    .order-list { max-height: 150px; overflow-y: auto; font-size: 0.9em; }
    input, button {
      width: 100%; padding: 10px; margin: 5px 0;
      border-radius: 5px; border: 1px solid #ccc;
      font-size: 1em;
    }
    button {
      background: #b30000; color: white;
      border: none; cursor: pointer;
    }
    button:hover { background: black; }
    footer {
      text-align: center;
      padding: 15px;
      background: #f0f0f0;
      margin-top: 20px;
    }
    footer a {
      margin: 0 10px;
      text-decoration: none;
      font-size: 1.2em;
      color: #b30000;
    }
  </style>
</head>
<body>

<header>
  <!-- 👇 لێرە logo خۆت دابنێ -->
  <img src="n.jpg" alt="Town Nutella Logo">
  <h1>T</h1>
</header>

<div class="menu">
  <div class="item" onclick="addToCart('Nutella Crepe', 4000)">
    <img src="nutella-crepe.jpg" alt="Nutella Crepe">
    <h3>Nutella Crepe</h3>
    <p>٤٠٠٠ دینار</p>
  </div>

  <div class="item" onclick="addToCart('Nutella Waffle', 5000)">
    <img src="w.jpg" alt="Nutella Waffle">
    <h3>Nutella Waffle</h3>
    <p>٥٠٠٠ دینار</p>
  </div>

  <div class="item" onclick="addToCart('Nutella Pancake', 4500)">
    <img src="nutella-pancake.jpg" alt="Nutella Pancake">
    <h3>Nutella Pancake</h3>
    <p>٤٥٠٠ دینار</p>
  </div>
</div>

<div class="cart">
  <h2>🛒 داواکارییەکەت</h2>
  <div class="order-list" id="orderList"></div>
  <p><b>کۆی گشتی:</b> <span id="total">٠</span> دینار</p>
  <input type="text" id="address" placeholder="ناونیشان">
  <input type="tel" id="phone" placeholder="ژمارەی مۆبایل (07xxxxxxxxx)">
  <button onclick="sendOrder()">ناردن</button>
  <button onclick="undoLast()">↩️ پاشگەزبوونەوە</button>
</div>

<footer>
  <p>📱 پەیوەندیمان پێوەبکە</p>
  <a href="https://instagram.com/yourpage" target="_blank">Instagram</a>
  <a href="https://snapchat.com/add/yourpage" target="_blank">Snapchat</a>
</footer>

<script>
  // 👇 لێرە توکن و chat_id بنووسە
  const BOT_TOKEN = "YOUR_BOT_TOKEN";
  const CHAT_ID   = "YOUR_CHAT_ID";

  let cart = [];

  function addToCart(name, price){
    cart.push({name, price});
    renderCart();
  }

  function undoLast(){
    cart.pop();
    renderCart();
  }

  function renderCart(){
    const list = document.getElementById("orderList");
    list.innerHTML = "";
    let total = 0;
    cart.forEach((item,i)=>{
      total += item.price;
      list.innerHTML += `${i+1}. ${item.name} - ${item.price} دینار<br>`;
    });
    document.getElementById("total").innerText = total;
  }

  async function sendOrder(){
    if(cart.length===0){ alert("تکایە شتێک هەڵبژێرە."); return; }
    const address = document.getElementById("address").value.trim();
    const phone   = document.getElementById("phone").value.trim();
    if(!address){ alert("ناونیشان بنووسە."); return; }
    if(!/^07[0-9]{9}$/.test(phone)){ alert("ژمارەی مۆبایل هەڵەیە."); return; }

    let msg = "🛒 داواکاریی نوێ - Town Nutella\n\n";
    cart.forEach((it,i)=>{ msg += `${i+1}. ${it.name} - ${it.price} دینار\n`; });
    let total = cart.reduce((sum,it)=> sum+it.price,0);
    msg += `\nکۆی گشتی: ${total} دینار\n\n🏠 ناونیشان: ${address}\n📞 ژمارە: ${phone}`;

    try {
      const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
        method: "POST",
        headers: { "Content-Type":"application/json" },
        body: JSON.stringify({ chat_id: CHAT_ID, text: msg })
      });
      if(res.ok){
        alert("✅ داواکاریەکەت نێردرا بۆ Town Nutella");
        cart=[]; renderCart();
        document.getElementById("address").value="";
        document.getElementById("phone").value="";
      } else {
        alert("❌ هەڵەی ناردن");
      }
    } catch(err){
      alert("❌ هەڵەی ئینتەرنێت: " + err.message);
    }
  }
</script>

</body>
</html>
