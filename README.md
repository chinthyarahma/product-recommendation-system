# product-recommendation-system
A Python CLI-based product recommendation system with user authentication, search history, and budget-based filtering.
class User:
    def __init__(self, name, password):
        self.name = name
        self.password = password
        self.history = [] 
        self.budget = None 


users = [] 


def signup():
    print("\n=== SIGN UP ===")
    name = input("Masukkan username: ")
    password = input("Masukkan password: ")

    for u in users:
        if u.name == name:
            print(" Username sudah dipakai!")
            return None

    new_user = User(name, password)
    users.append(new_user)
    print(" Akun berhasil dibuat!\n")


def login():
    print("\n=== LOGIN ===")
    name = input("Username: ")
    password = input("Password: ")

    for u in users:
        if u.name == name and u.password == password:
            print(f" Selamat datang, {name}!\n")
            return u

    print(" Username atau password salah.\n")
    return None

class Product:
    def __init__(self, name, category, price, popularity):
        self.name = name
        self.category = category
        self.price = price
        self.popularity = popularity

products = [

    Product("ASUS Zenbook 14", "Laptop", 13000000, 90),
    Product("ASUS TUF Gaming Laptop", "Laptop", 12000000, 92),
    Product("ASUS VivoBook 15", "Laptop", 8500000, 88),
    Product("ASUS ExpertBook B1", "Laptop", 9500000, 87),
    Product("ASUS ROG Strix G15", "Laptop", 16000000, 95),

    Product("ASUS ROG Phone 7", "Phone", 15000000, 95),
    Product("ASUS Zenfone 10", "Phone", 10000000, 88),
    Product("ASUS ROG Phone 6D", "Phone", 14000000, 90),
    Product("ASUS Zenfone 9", "Phone", 9000000, 85),
    Product("ASUS ROG Phone 5s", "Phone", 12000000, 87),

    Product("ASUS ROG Ally", "Gaming Handhelds", 9500000, 89),
    Product("ASUS ROG Flow Z13", "Gaming Handhelds", 14000000, 94),
    Product("ASUS ROG Flow X13", "Gaming Handhelds", 15500000, 92),
    Product("ASUS ROG Handheld Pro", "Gaming Handhelds", 10000000, 86),
    Product("ASUS ROG Mini Console", "Gaming Handhelds", 8500000, 84),

    Product("ASUS Gaming Mouse", "Accessories", 750000, 85),
    Product("ASUS ROG Headset", "Accessories", 1200000, 90),
    Product("ASUS ROG Keyboard", "Accessories", 1500000, 88),
    Product("ASUS ROG Mousepad", "Accessories", 350000, 80),
    Product("ASUS ROG Cooling Pad", "Accessories", 500000, 82),
]

def recommend(user):
    category = input("\nKategori yang dicari (Laptop/Phone/Gaming Handhelds/Accessories): ").title()
    budget = int(input("Budget maksimal (angka): "))

    user.history.append(category) 
    user.budget = budget

    print("\n Mencari rekomendasi...\n")

    filtered = []
    for p in products:
        if p.category == category and p.price <= budget:
            filtered.append(p)

    filtered.sort(key=lambda x: x.popularity, reverse=True)

    trending = max([p for p in products if p.category == category], key=lambda x: x.popularity)

    return filtered, trending

def main():
    print("=== SELAMAT DATANG DI TOKO ASUS ONLINE ===")

    while True:
        print("\n1. Sign Up")
        print("2. Login")
        print("3. Keluar")
        menu = input("Pilih menu: ")

        if menu == "1":
            signup()
        elif menu == "2":
            user = login()
            if user:
                user_menu(user)
        elif menu == "3":
            print("\n Terima kasih sudah menggunakan sistem ini!")
            break
        else:
            print(" Pilihan tidak valid!")

def user_menu(user):
    while True:
        print("\n=== MENU ===")
        print("1. Cari Produk")
        print("2. Lihat History Pencarian")
        print("3. Logout")
        choice = input("Pilih menu: ")

        if choice == "1":
            rec, trending = recommend(user)

            print(f"\n Halo {user.name}!")
            print("Berdasarkan pilihan kamu dan budget kamu, rekomendasi kami:")

            if len(rec) == 0:
                print(" Tidak ada produk yang cocok dengan budget kamu.")
            else:
                for i, p in enumerate(rec):
                    print(f"{i+1}. {p.name} - Rp{p.price} (Popularity {p.popularity})")

            print("\n Produk yang lagi populer di kategori ini:")
            print(f"{trending.name} (Popularity {trending.popularity})\n")

        elif choice == "2":
            print("\n History pencarian kategori kamu:")
            print(user.history if user.history else "Belum ada history.\n")

        elif choice == "3":
            print(f"\n Bye {user.name}! Logout berhasil.\n")
            break

        else:
            print(" Pilihan tidak valid.\n")

main()
