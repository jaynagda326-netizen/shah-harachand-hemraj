# shah-harachand-hemraj
Local wholesale grocery app for Shah Harachand Hemraj — delivery available only in Asalpha &amp; Ghatkopar Village. Minimum order ₹500, Cash on Delivery.
/* Shah Harachand Hemraj — Local Wholesale PWA (Single-file React component)

Instructions:

1. This is a single-file React component (default export). Create a new React app (Vite or Create React App) and replace App.jsx/App.tsx with this file.


2. Tailwind CSS is used for styling. If you don't have Tailwind, the component will still work with basic styles.


3. Customize at the top of the file:

OWNER_PHONE: WhatsApp number in international format (no +, e.g. 919876543210)

DELIVERY_AREAS: allowed area names (Asalpha, Ghatkopar Village)

PRODUCTS: edit or replace with your full wholesale product list and prices.



4. To make an installable Android app: use Capacitor or build as PWA and add to home screen.



Features implemented:

Product listing (sample wholesale items)

Cart with quantity, subtotal

Minimum order enforcement (₹500) — prevents placing orders below threshold

Area restriction: only lets users from allowed areas place orders

Order via WhatsApp: generates a wa.me link with order details for you to receive via WhatsApp (COD assumed)

Simple responsive layout */


import React, { useEffect, useState } from "react";

const OWNER_PHONE = "919999999999"; // <-- replace with your WhatsApp phone number (country code + number, no + or spaces) const DELIVERY_AREAS = ["Asalpha", "Ghatkopar Village"]; const MIN_ORDER = 500; // minimum order amount for delivery (₹)

const PRODUCTS = [ { id: 1, name: "Parle-G Biscuits (2 kg) - Wholesale", price: 220, stock: 50, unit: "2 kg", category: "Biscuits" }, { id: 2, name: "Balaji Wafers (Assorted) 500g - Wholesale", price: 120, stock: 100, unit: "500 g", category: "Snacks" }, { id: 3, name: "Kurkure (1 kg) - Wholesale", price: 200, stock: 80, unit: "1 kg", category: "Snacks" }, { id: 4, name: "Farsan (Mixed) 1 kg - Wholesale", price: 300, stock: 40, unit: "1 kg", category: "Farsan" }, { id: 5, name: "Aata (Atta) 10 kg - Wholesale", price: 420, stock: 30, unit: "10 kg", category: "Staples" }, { id: 6, name: "Aata (Maida) 10 kg - Wholesale", price: 400, stock: 30, unit: "10 kg", category: "Staples" }, { id: 7, name: "Mixed Biscuit Box (12 pcs) - Wholesale", price: 600, stock: 20, unit: "12 pcs", category: "Biscuits" }, { id: 8, name: "Assorted Namkeen Box (5 kg) - Wholesale", price: 950, stock: 15, unit: "5 kg", category: "Farsan" }, ];

export default function App() { const [products] = useState(PRODUCTS); const [cart, setCart] = useState(() => { try { const saved = localStorage.getItem("shh_cart"); return saved ? JSON.parse(saved) : []; } catch (e) { return []; } });

const [area, setArea] = useState(DELIVERY_AREAS[0]); const [customerName, setCustomerName] = useState(""); const [customerPhone, setCustomerPhone] = useState(""); const [note, setNote] = useState(""); const [infoMsg, setInfoMsg] = useState("");

useEffect(() => { localStorage.setItem("shh_cart", JSON.stringify(cart)); }, [cart]);

function addToCart(product) { setCart((c) => { const found = c.find((x) => x.id === product.id); if (found) { return c.map((x) => (x.id === product.id ? { ...x, qty: x.qty + 1 } : x)); } return [...c, { ...product, qty: 1 }]; }); setInfoMsg("Item added to cart"); setTimeout(() => setInfoMsg(""), 1400); }

function updateQty(id, qty) { if (qty < 1) return removeFromCart(id); setCart((c) => c.map((x) => (x.id === id ? { ...x, qty } : x))); }

function removeFromCart(id) { setCart((c) => c.filter((x) => x.id !== id)); }

const subtotal = cart.reduce((s, item) => s + item.price * item.qty, 0);

function canPlaceOrder() { if (!DELIVERY_AREAS.includes(area)) return false; if (subtotal < MIN_ORDER) return false; if (!customerName || !customerPhone) return false; if (cart.length === 0) return false; return true; }

function generateWhatsAppLink() { const lines = []; lines.push(Order for Shah Harachand Hemraj); lines.push(Name: ${customerName}); lines.push(Phone: ${customerPhone}); lines.push(Area: ${area}); lines.push("--- Items ---"); cart.forEach((it) => { lines.push(${it.name} — ${it.qty} x ₹${it.price} = ₹${it.qty * it.price}); }); lines.push(Subtotal: ₹${subtotal}); lines.push(Payment: Cash on Delivery); if (note) lines.push(Note: ${note});

const message = encodeURIComponent(lines.join("\n"));
return `https://wa.me/${OWNER_PHONE}?text=${message}`;

}

return ( <div style={{fontFamily: 'Inter, sans-serif'}} className="min-h-screen bg-gray-100 p-4"> <div className="max-w-5xl mx-auto bg-white shadow rounded-xl p-6"> <header className="flex flex-col md:flex-row items-start md:items-center justify-between gap-4 mb-6"> <div> <h1 className="text-2xl font-bold">Shah Harachand Hemraj</h1> <p className="text-gray-600">Local wholesale grocery — Delivery only inside Asalpha & Ghatkopar Village</p> <p className="text-sm text-gray-600 mt-1">Minimum order for delivery: <strong>₹{MIN_ORDER}</strong> — Wholesale pricing</p> </div> <div className="text-right"> <p className="text-sm">Contact (WhatsApp): <a className="text-blue-600 underline" href={https://wa.me/${OWNER_PHONE}}>{OWNER_PHONE}</a></p> <p className="text-sm">Payment: <strong>Cash on Delivery</strong></p> </div> </header>

<div className="grid md:grid-cols-3 gap-6">
      <main className="md:col-span-2">
        <div className="mb-4">
          <h2 className="font-semibold mb-2">Products</h2>
          {infoMsg && <div className="text-sm text-green-700 mb-2">{infoMsg}</div>}
          <div className="grid sm:grid-cols-2 gap-3">
            {products.map((p) => (
              <div key={p.id} className="border rounded p-3 bg-white">
                <div className="flex justify-between items-start">
                  <div>
                    <h3 className="font-medium">{p.name}</h3>
                    <div className="text-sm text-gray-600">{p.unit} • Stock: {p.stock}</div>
                    <div className="text-lg font-semibold mt-2">₹{p.price}</div>
                  </div>
                  <div className="flex flex-col items-end gap-2">
                    <button onClick={() => addToCart(p)} className="px-3 py-1 bg-blue-600 text-white rounded">Add</button>
                    <div className="text-xs text-gray-500">{p.category}</div>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </main>

      <aside className="md:col-span-1">
        <div className="border rounded p-4 bg-gray-50">
          <h2 className="font-semibold mb-2">Your Cart</h2>
          {cart.length === 0 && <div className="text-sm text-gray-600">Cart is empty</div>}
          {cart.map((it) => (
            <div key={it.id} className="flex items-center justify-between gap-2 mb-2">
              <div>
                <div className="font-medium">{it.name}</div>
                <div className="text-sm text-gray-500">₹{it.price} x {it.qty}</div>
              </div>
              <div className="flex items-center gap-2">
                <button onClick={() => updateQty(it.id, it.qty - 1)} className="px-2 py-1 border rounded">-</button>
                <div className="px-2">{it.qty}</div>
                <button onClick={() => updateQty(it.id, it.qty + 1)} className="px-2 py-1 border rounded">+</button>
                <button onClick={() => removeFromCart(it.id)} className="ml-2 text-sm text-red-600">Remove</button>
              </div>
            </div>
          ))}

          <div className="border-t pt-3 mt-3">
            <div className="flex justify-between">
              <div className="text-sm text-gray-600">Subtotal</div>
              <div className="font-semibold">₹{subtotal}</div>
            </div>
            <div className="text-xs text-gray-500 mt-1">Minimum order for delivery: ₹{MIN_ORDER}</div>
          </div>

          <div className="mt-4">
            <h3 className="font-medium">Delivery Details</h3>
            <div className="mt-2">
              <label className="text-sm">Area</label>
              <select className="w-full mt-1 p-2 border rounded" value={area} onChange={(e) => setArea(e.target.value)}>
                {DELIVERY_AREAS.map((a) => <option key={a} value={a}>{a}</option>)}
              </select>
            </div>

            <div className="mt-2">
              <label className="text-sm">Name</label>
              <input className="w-full p-2 border rounded mt-1" value={customerName} onChange={(e) => setCustomerName(e.target.value)} placeholder="Your name" />
            </div>
            <div className="mt-2">
              <label className="text-sm">Phone</label>
              <input className="w-full p-2 border rounded mt-1" value={customerPhone} onChange={(e) => setCustomerPhone(e.target.value)} placeholder="Mobile number" />
            </div>
            <div className="mt-2">
              <label className="text-sm">Note (optional)</label>
              <input className="w-full p-2 border rounded mt-1" value={note} onChange={(e) => setNote(e.target.value)} placeholder="Any note for delivery" />
            </div>

            <div className="mt-4">
              {!DELIVERY_AREAS.includes(area) && <div className="text-sm text-red-600">Delivery not available for selected area.</div>}
              {subtotal < MIN_ORDER && <div className="text-sm text-orange-600">Add ₹{MIN_ORDER - subtotal} more to reach minimum order.</div>}

              <a
                href={canPlaceOrder() ? generateWhatsAppLink() : "#"}
                onClick={(e) => { if (!canPlaceOrder()) { e.preventDefault(); setInfoMsg("Please complete order details and reach minimum order of ₹"+MIN_ORDER); setTimeout(()=>setInfoMsg(""),2000);} }}
                className={`block text-center mt-3 py-2 rounded ${canPlaceOrder() ? 'bg-green-600 text-white' : 'bg-gray-300 text-gray-700 cursor-not-allowed'}`}>
                Place Order via WhatsApp
              </a>

            </div>
          </div>

        </div>
      </aside>
    </div>

    <footer className="mt-6 text-center text-sm text-gray-500">
      <div>Shah Harachand Hemraj — Wholesale only inside Asalpha & Ghatkopar Village. Delivery for orders above ₹{MIN_ORDER}.</div>
    </footer>
  </div>
</div>

); }
