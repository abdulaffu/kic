import streamlit as st

# --- PAGE CONFIG ---
st.set_page_config(page_title="Aziz Quick-Shop | Solapur", layout="wide")

# --- CUSTOM STYLE (Green Theme) ---
st.markdown("""
    <style>
    .stButton>button { width: 100%; border-radius: 8px; background-color: #2e7d32; color: white; height: 45px; }
    .product-box { border: 1px solid #eee; padding: 15px; border-radius: 15px; text-align: center; background: white; margin-bottom: 10px; }
    .cat-header { background-color: #e8f5e9; padding: 5px 15px; border-radius: 10px; color: #1b5e20; margin: 20px 0; }
    .cart-box { background-color: #f9f9f9; padding: 15px; border-radius: 10px; border: 1px dashed #2e7d32; }
    </style>
    """, unsafe_allow_html=True)

# --- THE PRODUCT LIST ---
inventory = [
    {"name": "Sharbati Wheat", "price": 50, "unit": "kg", "cat": "🌾 Wheat & Grains", "img": "https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?w=400"},
    {"name": "Lokwan Wheat", "price": 45, "unit": "kg", "cat": "🌾 Wheat & Grains", "img": "https://images.unsplash.com/photo-1593113616828-6f22bca04804?w=400"},
    {"name": "Full Cream Milk", "price": 66, "unit": "Litre", "cat": "🥛 Dairy", "img": "https://images.unsplash.com/photo-1550583760-586910d04449?w=400"},
    {"name": "Fresh Paneer", "price": 120, "unit": "200g", "cat": "🥛 Dairy", "img": "https://images.unsplash.com/photo-1631452180519-c014fe946bc7?w=400"},
    {"name": "Tomato", "price": 30, "unit": "kg", "cat": "🍅 Vegetables", "img": "https://images.unsplash.com/photo-1518977676601-b53f02ac6d31?w=400"},
    {"name": "Potato", "price": 25, "unit": "kg", "cat": "🍅 Vegetables", "img": "https://images.unsplash.com/photo-1518977676601-b53f02ac6d31?w=400"},
]

# --- SESSION STATE FOR CART ---
if 'cart' not in st.session_state:
    st.session_state.cart = {}

# --- MAIN APP ---
st.title("🛒 Aziz Quick-Shop")
st.write("Home Delivery in Solapur | Fast & Fresh")

# --- CATEGORY SECTIONS ---
categories = ["🌾 Wheat & Grains", "🥛 Dairy", "🍅 Vegetables"]

for cat in categories:
    st.markdown(f"<div class='cat-header'><h3>{cat}</h3></div>", unsafe_allow_html=True)
    cat_items = [i for i in inventory if i['cat'] == cat]
    
    cols = st.columns(2) 
    for idx, item in enumerate(cat_items):
        with cols[idx % 2]:
            st.markdown(f"""
                <div class="product-box">
                    <img src="{item['img']}" width="100%" style="border-radius:10px; height:120px; object-fit:cover;">
                    <h4 style="color:black;">{item['name']}</h4>
                    <p style="color:green; font-weight:bold;">₹{item['price']} per {item['unit']}</p>
                </div>
            """, unsafe_allow_html=True)
            
            # Quantity Selector
            qty = st.number_input(f"Qty for {item['name']}", min_value=0, step=1, key=f"qty_{item['name']}")
            
            if st.button(f"Add to Cart", key=f"btn_{item['name']}"):
                if qty > 0:
                    st.session_state.cart[item['name']] = {"price": item['price'], "qty": qty, "unit": item['unit']}
                    st.success(f"Added {qty} {item['unit']} of {item['name']}")
                else:
                    st.error("Select quantity first")

# --- CHECKOUT SECTION (The Sidebar) ---
with st.sidebar:
    st.header("🛍️ My Shopping Bag")
    
    if not st.session_state.cart:
        st.info("Your bag is empty. Please add items.")
    else:
        st.markdown("<div class='cart-box'>", unsafe_allow_html=True)
        total_bill = 0
        for name, details in st.session_state.cart.items():
            item_total = details['price'] * details['qty']
            total_bill += item_total
            st.write(f"**{name}**")
            st.write(f"{details['qty']} {details['unit']} x ₹{details['price']} = ₹{item_total}")
            st.write("---")
        
        st.subheader(f"Total: ₹{total_bill}")
        st.markdown("</div>", unsafe_allow_html=True)
        
        if st.button("🗑️ Empty Bag"):
            st.session_state.cart = {}
            st.rerun()

        st.divider()
        st.header("📍 Delivery Details")
        c_name = st.text_input("Customer Name")
        c_phone = st.text_input("Customer Phone Number")
        c_address = st.text_area("Full Delivery Address")

        if st.button("🚀 PLACE ORDER ON WHATSAPP"):
            if c_name and c_phone and c_address and st.session_state.cart:
                # Prepare WhatsApp Text
                order_items = ""
                for name, details in st.session_state.cart.items():
                    order_items += f"- {name}: {details['qty']} {details['unit']} (₹{details['price'] * details['qty']})%0A"
                
                wa_message = (
                    f"🟢 *NEW ORDER FROM AZIZ SHOP*%0A%0A"
                    f"*Customer:* {c_name}%0A"
                    f"*Phone:* {c_phone}%0A"
                    f"*Address:* {c_address}%0A%0A"
                    f"*Items Ordered:*%0A{order_items}%0A"
                    f"*GRAND TOTAL:* ₹{total_bill}%0A%0A"
                    f"Please confirm this order."
                )
                
                # REPLACE 91XXXXXXXXXX with your real WhatsApp number
                my_number = "91XXXXXXXXXX" 
                wa_url = f"https://wa.me/{my_number}?text={wa_message}"
                
                st.markdown(f'<meta http-equiv="refresh" content="0;URL={wa_url}">', unsafe_allow_html=True)
            else:
                st.error("Please fill Name, Phone, and Address.")
