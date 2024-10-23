# import libraries
import tkinter
import PIL
from customtkinter import *
from tkinter import messagebox, ttk
from tkcalendar import DateEntry
from PIL import Image
import re
import sqlite3


# declare global variables
window_width = 630
window_height = 400
bg_color1 = "#4085AD"
bg_color2 = "#8ECAEE"




class IncomeExpenseApp:
   def __init__(self, root):
       self.root = root
       # Set the geometry of the window
       self.root.geometry(f"{window_width}x{window_height}")


       # Set appearance mode and default color theme
       set_appearance_mode("light")


       # connect to db
       self.db = sqlite3.connect('income_expense.db')
       self.cursor = self.db.cursor()
       self.create_tables()  # function call


       #self.create_dashboard() #for editing purposes
       self.create_login_screen()


# function for creating tables in the database
   def create_tables(self):
       self.cursor.execute("""
               CREATE TABLE IF NOT EXISTS user (
                   user_id INTEGER PRIMARY KEY,
                   username TEXT NOT NULL,
                   password TEXT NOT NULL,
                   email TEXT NOT NULL,
                   date_created TIMESTAMP NOT NULL
               )
               """)
       self.cursor.execute("""
               CREATE TABLE IF NOT EXISTS user_update_log (
                   log_id INTEGER PRIMARY KEY,
                   user_id INTEGER,
                   update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                   old_username TEXT,
                   old_password TEXT,
                   old_email TEXT,
                   new_username TEXT,
                   new_password TEXT,
                   new_email TEXT,
                   FOREIGN KEY (user_id) REFERENCES user(user_id)
               )
               """)
       self.cursor.execute("""
             CREATE TABLE IF NOT EXISTS category (
                 id INTEGER PRIMARY KEY,
                 name TEXT NOT NULL
             )
          """)
       self.cursor.execute("""
             CREATE TABLE IF NOT EXISTS TXN (
                 id INTEGER PRIMARY KEY,
                 transaction_description TEXT,
                 transaction_date TIMESTAMP,
                 amount REAL,
                 transaction_type TEXT,
                 account_id INTEGER,
                 category_id INTEGER,
                 txn_status TEXT,
                 FOREIGN KEY (category_id) REFERENCES category(id)
             )
          """)
       self.cursor.execute("""
             CREATE TABLE IF NOT EXISTS income (
                 id INTEGER PRIMARY KEY,
                 source TEXT,
                 transaction_id INTEGER,
                 FOREIGN KEY (transaction_id) REFERENCES TXN(id)
             )
          """)
       self.cursor.execute("""
             CREATE TABLE IF NOT EXISTS expense (
                 id INTEGER PRIMARY KEY,
                 payment_method TEXT,
                 transaction_id INTEGER,
                 FOREIGN KEY (transaction_id) REFERENCES TXN(id)
             )
          """)
       self.db.commit()  # commit changes to the db


   # function for clearing the screen
   def clear_screen(self):
       for widget in self.root.winfo_children():
           widget.destroy()


   # password visiblity function
   def toggle_password_visibility(self):
       if self.show_password_var.get():
           self.password_entry.configure(show="*")
           self.eye_button.configure(image=self.eye_closed)
       else:
           self.password_entry.configure(show="")
           self.eye_button.configure(image=self.eye_open)
       self.show_password_var.set(not self.show_password_var.get())


   # Login Screen
   def create_login_screen(self):
       self.clear_screen()


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD", width=100, height=window_height)
       frame1.grid(row=0, column=0, columnspan=1, sticky="nswe", ipadx=20, ipady=window_height)
       frame1.pack_propagate(False)
       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=0, column=1, columnspan=2, sticky="nswe", ipadx=200, ipady=window_height)
       frame2.pack_propagate(False)


       image = PIL.Image.open("fflowbluWhite.png")  # image name
       logo = CTkImage(image, size=(200, 200))


       # create label for logo, title of the login screen, and Register label
       CTkLabel(frame1, text="", image=logo).grid(row=0, column=0, padx=(40, 10), pady=(20, 15))
       CTkLabel(frame1, text=" Income and Expense \nManager ", font=("Arial", 18, "bold"), text_color="white",
                justify="center").grid(row=2, column=0, rowspan=2, columnspan=1, padx=(30, 10), pady=12, sticky="nsew")
       CTkButton(frame1, text="Register", text_color="black", fg_color="#8ECAEE",
                 command=self.create_registration_screen).grid(row=5, column=0, columnspan=1, pady=15, sticky="s")


       CTkLabel(frame2, text="  LOG IN", font=("Arial", 24, "bold")).grid(row=0, column=1, columnspan=1, padx=20,
                                                                          pady=(30, 15), sticky="nw")
       CTkLabel(frame2, text=" Username:", font=("Arial", 12)).grid(row=1, column=1, sticky="w", padx=30, pady=8)
       self.username_entry = CTkEntry(frame2, width=250)
       self.username_entry.grid(row=2, column=1, padx=35, pady=8)


       CTkLabel(frame2, text=" Password:", font=("Arial", 12)).grid(row=3, column=1, sticky="w", padx=30, pady=8)
       self.password_entry = CTkEntry(frame2, show="*", width=250)
       self.password_entry.grid(row=4, column=1, padx=35, pady=8)
       self.show_password_var = IntVar()
       image_eye_open = PIL.Image.open("view.png")
       image_eye_closed = PIL.Image.open("hide.png")
       self.eye_open = CTkImage(image_eye_open, size=(20, 20))
       self.eye_closed = CTkImage(image_eye_closed, size=(20, 20))
       self.eye_button = CTkButton(frame2, text="", image=self.eye_closed, fg_color="transparent", width=20,
                                   command=self.toggle_password_visibility)
       self.eye_button.grid(row=4, column=1, columnspan=2, pady=5, sticky="e")


       CTkButton(frame2, text="Forgot Password?", font=("Arial", 11, "underline"), text_color="black",
                 fg_color="transparent", width=100, command=self.forgot_password).grid(row=6, column=1, columnspan=2,
                                                                                       padx=(10, 30), pady=8,
                                                                                       sticky="e")
       CTkButton(frame2, text="Log in", text_color="black", fg_color="#4085AD", command=self.login).grid(row=7,
                                                                                                         column=1,
                                                                                                         sticky="s",
                                                                                                         columnspan=2,
                                                                                                         padx=30,
                                                                                                         pady=20)


   # method for forgot password
   def forgot_password(self):
       self.clear_screen()


       frame = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame.grid(row=0, column=0, sticky="nsew", ipadx=window_width, ipady=window_height)
       CTkLabel(frame, text=" Forgot Password", font=("Arial", 16, "bold")).grid(row=1, column=0, sticky="w",
                                                                                 padx=30, pady=(25, 10))


       # Enter email label and email text box
       CTkLabel(frame, text=" Enter Email", font=("Arial", 12)).grid(row=2, column=0, sticky="w", padx=30, pady=3)
       self.reg_email_entry = CTkEntry(frame, width=250)
       self.reg_email_entry.grid(row=3, column=0, pady=(3, 10))


       # Enter username label and username text box
       CTkLabel(frame, text=" Enter Username:", font=("Arial", 12)).grid(row=4, column=0, sticky="w", padx=30, pady=3)
       self.username_entry = CTkEntry(frame, width=250)
       self.username_entry.grid(row=5, column=0, padx=15, pady=(3, 10))


       # Enter new password label and password text box
       CTkLabel(frame, text=" Enter New Password:", font=("Arial", 12)).grid(row=6, column=0, sticky="w", padx=30,
                                                                             pady=3)
       self.password_entry = CTkEntry(frame, show="*", width=250)
       self.password_entry.grid(row=7, column=0, padx=35, pady=(3, 10))
       self.show_password_var = IntVar()
       image_eye_open = PIL.Image.open("view.png")
       image_eye_closed = PIL.Image.open("hide.png")
       self.eye_open = CTkImage(image_eye_open, size=(20, 20))
       self.eye_closed = CTkImage(image_eye_closed, size=(20, 20))
       self.eye_button = CTkButton(frame, text="", image=self.eye_closed, fg_color="transparent", width=20,
                                   command=self.toggle_password_visibility)
       self.eye_button.grid(row=7, column=0, columnspan=2, pady=(3, 10), sticky="e")


       # Change password label and back to log in label
       CTkButton(frame, text="Change Password", text_color="black", fg_color="#4085AD",
                 command=self.change_password).grid(row=9, column=0, columnspan=2, pady=(15, 5))
       CTkButton(frame, text="Back to Login", text_color="black", fg_color="#4085AD",
                 command=self.create_login_screen).grid(row=10, column=0, columnspan=2, pady=10)


   # function for changing password
   def change_password(self):
       # Use the get method to get the values for email, username, and new password then store to respective variables
       email = self.reg_email_entry.get()
       username = self.username_entry.get()
       new_password = self.password_entry.get()


       if not email or not username or not new_password:
           messagebox.showerror("Error", "All fields are required.")
           return


       self.cursor.execute("SELECT * FROM user WHERE email = ? AND username = ?", (email, username))
       user = self.cursor.fetchone()
       if user:
           self.cursor.execute("UPDATE user SET password = ? WHERE email = ? AND username = ?",
                               (new_password, email, username))
           self.db.commit()
           messagebox.showinfo("Success", "Password changed successfully.")
           self.create_login_screen()
       else:
           messagebox.showerror("Error", "Invalid email or username.")


# Register Screen
   def create_registration_screen(self):
       self.clear_screen()


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, columnspan=1, sticky="nswe", ipadx=20, ipady=window_height)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=0, column=1, columnspan=2, sticky="nswe", ipadx=200, ipady=window_height)


       image = PIL.Image.open("fflowbluWhite.png")  # image name
       logo = CTkImage(image, size=(200, 200))


       CTkLabel(frame1, text="", image=logo).grid(row=0, column=0, padx=(40, 10), pady=(20, 15))
       CTkLabel(frame1, text=" Income and Expense \nManager ", font=("Arial", 18, "bold"), text_color="white",
                justify="center").grid(row=2, column=0, rowspan=2, columnspan=1, padx=(30, 10), pady=15, sticky="nsew")
       CTkButton(frame1, text="Back to Login", text_color="black", fg_color="#8ECAEE",
                 command=self.create_login_screen).grid(row=5, column=0, columnspan=1, padx=20, pady=20, sticky="s")


       CTkLabel(frame2, text="  REGISTER", font=("Arial", 24, "bold")).grid(row=0, column=0, columnspan=1, padx=20,
                                                                            pady=(30, 10), sticky="nw")
       CTkLabel(frame2, text=" Email", font=("Arial", 12)).grid(row=1, column=0, sticky="w", padx=30, pady=5)
       self.reg_email_entry = CTkEntry(frame2, width=250)
       self.reg_email_entry.grid(row=2, column=0, pady=(0, 10))


       CTkLabel(frame2, text=" Username:", font=("Arial", 12)).grid(row=3, column=0, sticky="w", padx=30, pady=5)
       self.reg_username_entry = CTkEntry(frame2, width=250)
       self.reg_username_entry.grid(row=4, column=0, padx=15, pady=(0, 10))


       CTkLabel(frame2, text=" Password:", font=("Arial", 12)).grid(row=5, column=0, sticky="w", padx=30, pady=5)
       self.password_entry = CTkEntry(frame2, show="*", width=250)
       self.password_entry.grid(row=6, column=0, padx=35, pady=(0, 15))


       self.show_password_var = IntVar()
       image_eye_open = PIL.Image.open("view.png")
       image_eye_closed = PIL.Image.open("hide.png")
       self.eye_open = CTkImage(image_eye_open, size=(20, 20))
       self.eye_closed = CTkImage(image_eye_closed, size=(20, 20))
       self.eye_button = CTkButton(frame2, text="", image=self.eye_closed, fg_color="transparent", width=20,
                                   command=self.toggle_password_visibility)
       self.eye_button.grid(row=6, column=0, columnspan=2, pady=(0, 15), sticky="e")


       CTkButton(frame2, text="Register", text_color="black", fg_color="#4085AD", command=self.register).grid(row=8,
                                                                                                              column=0,
                                                                                                          columnspan=2,
                                                                                                              pady=20)


   # function for login button
   def login(self):
       username = self.username_entry.get()
       password = self.password_entry.get()
       encrypted_password = self.encrypt_password(password)
       # decrypted_password = self.decrypt_password(password)


       if not username or not password:
           messagebox.showerror("Error", "Username and password are required.")
           return


       self.cursor.execute("SELECT * FROM user WHERE username = ? AND password = ?", (username, encrypted_password))
       #decrypted_password = self.decrypt_password(password)
       user = self.cursor.fetchone()
       if user:
           self.current_user_id = user[0]  # Store the current user ID for future use
           self.create_dashboard()
       else:
           messagebox.showerror("Login Failed", "Invalid username or password.")


   def encrypt_password(self, password):
       password = self.password_entry.get()
       encrypted_password = ''.join(chr(ord(char) + 1) for char in password)
       return encrypted_password




   # function for register button
   def register(self):
       username = self.reg_username_entry.get()
       password = self.password_entry.get()
       encrypted_password = self.encrypt_password(password)
       email = self.reg_email_entry.get()


       if not username or not password or not email:
           messagebox.showerror("Error", "All fields are required.")
           return


       # Check if the email is valid and ends with either '@gmail.com' or '@yahoo.com'
       if not re.match(r"[^@]+@[^@]+\.[^@]+", email) or not (
               email.endswith("gmail.com") or email.endswith("yahoo.com")):
           messagebox.showwarning("Input Error", "Please enter a valid Gmail or Yahoo address.")
           return


       # Check if the username already exists
       self.cursor.execute("SELECT * FROM user WHERE username = ?", (username,))
       if self.cursor.fetchone():
           messagebox.showerror("Error", "Username already exists. Please choose a different username.")
           return


       self.cursor.execute(
           "INSERT INTO user (username, password, email, date_created) VALUES (?, ?, ?, datetime('now'))",
           (username, encrypted_password, email))
       self.db.commit()
       messagebox.showinfo("Success", "Registration successful")
       self.create_login_screen()




# Transaction Query and List Screen
   def create_dashboard(self):


       self.clear_screen()
       self.root.geometry("1000x500")  # Change the size as needed


       window_height = 500
       window_width = 700


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("fflowbluWhite.png")  # image name
       logo = CTkImage(image, size=(30, 30))
       settings = PIL.Image.open("settings.png")
       setting = CTkImage(settings, size=(30, 30))
       CTkLabel(frame1, text="", image=logo, bg_color="white").grid(row=0, column=0, padx=10, pady=5)
       CTkLabel(frame1, text="FINANCEFLOW", text_color="white", font=("Arial", 14, "bold")).grid(row=0, column=1,
                                                                                                 padx=(5, 50), pady=5)
       CTkLabel(frame1, text="                                                               ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2,
                                                                       padx=(5, 50), pady=5)
       CTkButton(frame1, image=setting, text="", fg_color="transparent", width=50, command=self.user_settings).grid(
           row=0, column=5, columnspan=3, pady=5, sticky="ne", padx=(415, 0))


       image = PIL.Image.open("searchblack.png")
       search = CTkImage(image, size=(20, 20))
       CTkLabel(frame2, text="TRANSACTION QUERY", font=("Arial", 24, "bold")).grid(row=0, column=0, sticky="nw",
                                                                                   padx=10,
                                                                                   pady=10)
       CTkButton(frame2, text="Add Income", width=130, command=self.add_income_screen).grid(row=0, column=1, columnspan=1,
                                                                                            pady=5, sticky="w",
                                                                                            padx=(400, 0))
       CTkButton(frame2, text="Add Expense", width=130, command=self.add_expense_screen).grid(row=0, column=2,
                                                                                              columnspan=1, rowspan=1,
                                                                                              pady=5, sticky="w",
                                                                                              padx=(15, 560))


       self.search_entry = CTkEntry(frame2, placeholder_text="Search...", width=260)
       self.search_entry.grid(row=1, column=0, columnspan=1, rowspan=1, padx=(15, 0), pady=5, sticky="nw")
       CTkButton(frame2, text="", image=search, width=30, command=self.search_transactions).grid(row=1, column=1,
                                                                                                 columnspan=1, rowspan=2, pady=5,
                                                                                                 sticky="nw",
                                                                                                 padx=(0, 250))


       CTkButton(frame2, text="Manage Accounts", width=130, command=self.manage_accounts).grid(row=1, column=1,
                                                                                               columnspan=1, pady=5,
                                                                                               sticky="w",
                                                                                               padx=(400, 0))
       CTkButton(frame2, text="Manage Categories", width=130, command=self.manage_categories).grid(row=1, column=2,
                                                                                                   columnspan=1,
                                                                                                   pady=5, sticky="w",
                                                                                                   padx=(15, 0))


       # Fetch and display total income and total expense
       total_income = self.calculate_total_income()
       total_expense = self.calculate_total_expense()




       CTkLabel(frame2, text=f"Total Income: {total_income:.2f}", font=("Arial", 18, "bold")).grid(row=3, column=0,
                                                                                                    padx=20, pady=10,
                                                                                                    sticky="w")
       CTkLabel(frame2, text=f"Total Expense: {total_expense:.2f}", font=("Arial", 18, "bold")).grid(row=4, column=0,
                                                                                                      padx=20, pady=10,
                                                                                                      sticky="w")
       CTkLabel(frame2, text=f"Current Balance: {total_income-total_expense:.2f}", font=("Arial", 18, "bold")).grid(
                                                                                                    row=5, column=0,
                                                                                                    padx=20, pady=10,
                                                                                                    sticky="w")
       # Tree View
       columns = ('date', 'type', 'account', 'category', 'amount', 'description')
       self.transaction_tree = ttk.Treeview(frame2, columns=columns, show='headings')
       self.transaction_tree.grid(row=2, column=0, columnspan=3, padx=(20, 0), pady=10, sticky="w")
       # Create a vertical scrollbar
       scrollbar = ttk.Scrollbar(frame2, orient="vertical", command=self.transaction_tree.yview)
       scrollbar.grid(row=2, column=2, sticky="nsw", padx=(200, 10), pady=10)
       self.transaction_tree.configure(yscrollcommand=scrollbar.set)


       for col in columns:
           self.transaction_tree.heading(col, text=col.capitalize())


       self.cursor.execute("""
                               SELECT TXN.transaction_date,  TXN.transaction_type, ACCOUNT.account_name,
                               category.name, TXN.amount, TXN.transaction_description
                               FROM TXN
                               LEFT JOIN category ON TXN.category_id = category.id
                               LEFT JOIN ACCOUNT ON TXN.account_id = ACCOUNT.account_id
                           """)


       transactions = self.cursor.fetchall()


       for transaction in transactions:
           self.transaction_tree.insert('', END, values=transaction)


   # function for calculating Total Income, Expense, and Current Balance
   def calculate_totals(self) -> object:
       try:
           # Calculate total income
           self.cursor.execute("SELECT SUM(amount) FROM TXN WHERE txn_type = 'income'")
           total_income = self.cursor.fetchone()[0] or 0.0  # Use 0.0 if no income records exist


           # Calculate total expenses
           self.cursor.execute("SELECT SUM(amount) FROM TXN WHERE txn_type = 'expense'")
           total_expenses = self.cursor.fetchone()[0] or 0.0  # Use 0.0 if no expense records exist


           # Calculate current account balance
           current_balance = total_income - total_expenses


           return total_income, total_expenses, current_balance


       except sqlite3.Error as e:
           print(f"SQLite error: {e}")
           # Handle error (e.g., log it, show error message to user)
           return 0.0, 0.0, 0.0  # Return default values or handle error as needed


   def calculate_total_income(self):
       self.cursor.execute("SELECT SUM(amount) FROM TXN WHERE transaction_type = 'Income'")
       result = self.cursor.fetchone()
       return result[0] if result[0] is not None else 0.0


   def calculate_total_expense(self):
       self.cursor.execute("SELECT SUM(amount) FROM TXN WHERE transaction_type = 'Expense'")
       result = self.cursor.fetchone()
       return result[0] if result[0] is not None else 0.0


   # function for editing Transaction
   # def edit_txn(self):
   #     selected_item = self.transaction_tree.selection()
   #     if selected_item:
   #         transaction_id = self.transaction_tree.item(selected_item)['values'][0]  # Assuming id is the first column
   #         # Fetch transaction details using transaction_id
   #         self.cursor.execute("SELECT * FROM TXN WHERE id = ?", (transaction_id,))
   #         transaction = self.cursor.fetchone()
   #         if transaction:
   #             # Create a new window to edit transaction details
   #             edit_window = Toplevel(self.root)
   #             edit_window.title("Edit Transaction")
   #             # Add form fields and populate with existing transaction data
   #             # Save changes to database and refresh the transaction tree view
   #         else:
   #             messagebox.showerror("Error", "Transaction not found.")
   #     else:
   #         messagebox.showwarning("Error", "Please select a transaction to edit.")


   # function for deleting Transaction
   # def delete_txn(self): # convert to Soft_Delete
   #     selected_item = self.transaction_tree.selection()
   #     if selected_item:
   #         transaction_id = self.transaction_tree.item(selected_item)['values'][0]  # Assuming id is the first column
   #         self.cursor.execute("DELETE FROM TXN WHERE id = ?", (transaction_id,))
   #         self.db.commit()
   #         self.transaction_tree.delete(selected_item)
   #         messagebox.showinfo("Success", "Transaction deleted successfully.")
   #     else:
   #         messagebox.showwarning("Error", "Please select a transaction to delete.")


   # function for searching a Transaction
   def search_transactions(self):
       search_query = self.search_entry.get()
       self.transaction_tree.delete(*self.transaction_tree.get_children())
       query = """
           SELECT TXN.id, TXN.transaction_date, TXN.transaction_type, ACCOUNT.account_name, category.name,
           TXN.amount, TXN.transaction_description
           FROM TXN
           LEFT JOIN category ON TXN.category_id = category.id
           LEFT JOIN ACCOUNT ON TXN.account_id = ACCOUNT.account_id
           WHERE TXN.transaction_date LIKE ?
           OR TXN.transaction_type LIKE ?
           OR TXN.transaction_description LIKE ?
           OR ACCOUNT.account_name LIKE ?
           OR TXN.amount LIKE ?
           OR category.name LIKE ?
       """
       search_term = f"%{search_query}%"
       self.cursor.execute(query, (search_term, search_term, search_term, search_term, search_term, search_term))
       transactions = self.cursor.fetchall()
       for transaction in transactions:
           self.transaction_tree.insert('', 'end', values=transaction)




# Income Screen
   def add_income_screen(self):


       self.clear_screen()
       self.root.geometry("700x400")


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("back.png")  # image name
       back = CTkImage(image, size=(25, 25))


       settings = PIL.Image.open("settings.png")
       setting = CTkImage(settings, size=(30, 30))
       CTkButton(frame1, text="", image=back, fg_color="transparent", width=40, command=self.create_dashboard).grid(
                                                                               row=0, column=0, padx=(5, 50), pady=5)


       CTkLabel(frame1,
                text="                                                                                                               ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2,
                                                                       padx=(5, 50), pady=5)
       (CTkButton(frame1, image=setting, text="", fg_color="transparent", width=50, command=self.user_settings).grid
        (row=0, column=5, padx=50, pady=5, sticky="e"))
       CTkLabel(frame2, text="Add Income", font=("Arial", 24, "bold")).grid(row=0, column=0, columnspan=2, padx=20,
                                                                            pady=10, sticky="w")


       CTkLabel(frame2, text="Source").grid(row=1, column=0, sticky="w", padx=20, pady=5)
       source = self.load_source()
       self.income_source_combo = (CTkComboBox(frame2, values=source, width=200))
       self.income_source_combo.grid(row=1, column=1, padx=10, pady=5, sticky="w")


       CTkLabel(frame2, text="Account").grid(row=2, column=0, sticky="w", padx=20, pady=3)


       accounts = self.load_accounts()
       self.income_account_combo = CTkComboBox(frame2, values=accounts, width=200)  #["Cash", "GCash", "Landbank", "Seabank", "Maya", "GoTyme"]
       self.income_account_combo.grid(row=2, column=1, sticky="w", padx=10, pady=5)  # command=combobox_callback, variable=combobox_var


       # Load categories from the database
       categories = self.load_categories()


       CTkLabel(frame2, text="Category").grid(row=3, column=0, sticky="w", padx=20, pady=5)
       self.income_category_combo = CTkComboBox(frame2, values=categories, width=200)
       self.income_category_combo.grid(row=3, column=1, padx=10, pady=5, sticky="w")




       CTkLabel(frame2, text="Amount").grid(row=4, column=0, sticky="w", padx=20, pady=3)
       self.income_amount_entry = CTkEntry(frame2, width=200)
       self.income_amount_entry.grid(row=4, column=1,  padx=10, pady=3, sticky="w")


       CTkLabel(frame2, text="Date").grid(row=5, column=0, sticky="w", padx=20, pady=3)
       self.income_date_entry = DateEntry(frame2, width=32, height=20, background='#4085AD', foreground='white', borderwidth=2)
       self.income_date_entry.grid(row=5, column=1, padx=10, pady=10, sticky="w")


       CTkLabel(frame2, text="Description").grid(row=6, column=0, sticky="w", padx=20, pady=5)
       self.income_description_text = CTkTextbox(frame2, width=200, height=50)
       self.income_description_text.grid(row=6, column=1, padx=10, pady=5, sticky="w")


       CTkButton(frame2, text="Add Income", command=self.add_income).grid(row=8, column=0, columnspan=2, padx=10, pady=5, sticky="e")


       self.source_listbox = tkinter.Listbox(frame2, height=15, width=35)
       self.source_listbox.grid(row=2, column=2, columnspan=1, rowspan=6, padx=8, pady=2)
       self.load_source_listbox()


       self.source_name_entry = CTkEntry(frame2, width=200)
       self.source_name_entry.grid(row=1, column=2, padx=10, pady=5, sticky="e")
       CTkButton(frame2, text="Add Source", font=("Arial", 12), width=120, command=self.add_source).grid(row=1,
                                                                                                         column=3,
                                                                                                         padx=10,
                                                                                                         pady=5)


       CTkButton(frame2, text="Edit Source", font=("Arial", 12), width=120, command=self.edit_source).grid(row=2,
                                                                                                           column=3,
                                                                                                           padx=10,
                                                                                                           pady=5)
       CTkButton(frame2, text="Remove Source", font=("Arial", 12), width=120, command=self.remove_source).grid(row=3,
                                                                                                               column=3,
                                                                                                               padx=10,
                                                                                                              pady=5)


       # function for the add income button
       '''def add_income(self):
           source = self.income_source_combo.get()
           account = self.income_account_combo.get()
           category = self.income_category_combo.get()
           amount = self.income_amount_entry.get()
           date = self.income_date_entry.get_date()
           description = self.income_description_text.get("1.0", END).strip()


           if not (source and account and category and amount and date and description):
               messagebox.showerror("Error", "All fields are required.")
               return


           # validations
           if str(amount).isalpha():
               messagebox.showerror("Error", "Invalid input for amount. Must be a number.")
               return


           self.cursor.execute("INSERT INTO category (name) VALUES (?)", (category,))
           category_id = self.cursor.lastrowid


           self.cursor.execute("""
               INSERT INTO TXN (transaction_description, transaction_date, amount, transaction_type, account_id, category_id)
               VALUES (?, ?, ?, ?, ?, ?)
           """, (description, date, amount, "Income", 1, category_id))
           transaction_id = self.cursor.lastrowid


           self.cursor.execute("INSERT INTO income (source, transaction_id) VALUES (?, ?)", (source, transaction_id))
           self.db.commit()


           messagebox.showinfo("Success", "Income added successfully!")
           self.create_dashboard()'''


   # function for add income button
   def add_income(self):
       source = self.income_source_combo.get()
       account = self.income_account_combo.get()
       category = self.income_category_combo.get()
       amount = self.income_amount_entry.get()
       date = self.income_date_entry.get_date()
       description = self.income_description_text.get("1.0", END).strip()


       if not (source and account and category and amount and date and description):
           messagebox.showerror("Error", "All fields are required.")
           return


       if amount.isalpha():
           messagebox.showerror("Error", "Invalid input for amount. Must be a number.")
           return


       self.cursor.execute("INSERT INTO category (name) VALUES (?)", (category,))
       category_id = self.cursor.lastrowid


       self.cursor.execute("""
           INSERT INTO TXN (transaction_description, transaction_date, amount, transaction_type, account_id, category_id, transaction_status)
           VALUES (?, ?, ?, ?, ?, ?, ?)
       """, (description, date, amount, "Income", 1, category_id, 'Active'))
       transaction_id = self.cursor.lastrowid


       self.cursor.execute("INSERT INTO income (source, transaction_id) VALUES (?, ?)", (source, transaction_id))
       self.db.commit()


       messagebox.showinfo("Success", "Income added successfully!")
       self.create_dashboard()




   def load_accounts(self):
       # Execute the query to fetch categories
       self.cursor.execute("SELECT account_name FROM account")


       # Fetch all rows from the executed query
       rows = self.cursor.fetchall()


       # Close the database connection
       # self.db.close()


       # Extract the categories from the rows
       account_name = [row[0] for row in rows]


       return account_name


   def load_source_listbox(self):
       self.cursor.execute("SELECT source FROM income")
       sources = [row[0] for row in self.cursor.fetchall()]
       self.source_listbox.delete(1, END)
       for source in sources:
           self.source_listbox.insert(END, source + "\n")


   def load_source(self):
       # Execute the query to fetch categories
       self.cursor.execute("SELECT DISTINCT source FROM income")


       # Fetch all rows from the executed query
       rows = self.cursor.fetchall()


       # Extract the categories from the rows
       source = [row[0] for row in rows]


       return source




   # function for adding source
   def add_source(self):
       name = self.source_name_entry.get()
       if not name:
           messagebox.showerror("Error", "Source name is required")
           return


       self.cursor.execute("SELECT COUNT(*) FROM income WHERE source = ?", (name,))
       result = self.cursor.fetchone()


       if result[0]>0:
           messagebox.showerror("Error", "Source name already exists")
       else:
           self.cursor.execute("INSERT INTO income (source) VALUES (?)", (name,))
           self.db.commit()
           messagebox.showinfo("Success", "Source added successfully")
           self.add_income_screen()




   # function for editing selected source
   def edit_source(self):
       selected_indices = self.source_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No source selected")
           return


       selected_index = selected_indices[0]
       selected_source = self.source_listbox.get(selected_index).strip()
       new_source_name = self.source_name_entry.get()
       if new_source_name:
           self.cursor.execute("UPDATE income SET source = ? WHERE source = ?", ( new_source_name,  selected_source))
           self.db.commit()
           self.load_source_listbox()
           messagebox.showinfo("Success", "Source updated successfully")
       else:
           messagebox.showwarning("Warning", "New source name cannot be empty")


       source = self.source_name_entry.get()
       if not source:
           messagebox.showerror("Error", "Source name is required")
           return




   # function for deleting selected source
   def remove_source(self):
       selected_indices = self.source_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No category selected")
           return


       if messagebox.askyesno("Delete Source", "Are you sure you want to delete this source?"):
           selected_index = selected_indices[0]
           selected_source = self.source_listbox.get(selected_index).strip()
           self.cursor.execute("DELETE FROM income WHERE source = ?", (selected_source,))
           self.db.commit()
           self.load_source_listbox()
           messagebox.showinfo("Success", "Source deleted successfully")




# Expense Screen
   def add_expense_screen(self):
       self.clear_screen()
       self.root.geometry("750x410")


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("back.png")  # image name
       back = CTkImage(image, size=(25, 25))


       settings = PIL.Image.open("settings.png")
       setting = CTkImage(settings, size=(30, 30))
       CTkButton(frame1, text="", image=back, fg_color="transparent", width=40, command=self.create_dashboard).grid(
           row=0, column=0, padx=(5, 50), pady=5)


       CTkLabel(frame1, text="                                                                                                                          ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2,
                                                                       padx=(5, 50), pady=5)
       (CTkButton(frame1, image=setting, text="", fg_color="transparent", width=50, command=self.user_settings).grid
        (row=0, column=5, padx=50, pady=5, sticky="e"))
       CTkLabel(frame2, text="Add Expense", font=("Arial", 24, "bold")).grid(row=0, column=0, columnspan=2, padx=20,
                                                                             pady=10, sticky="w")


       CTkLabel(frame2, text="Payment Method").grid(row=1, column=0, sticky="w", padx=20, pady=5)
       payment_method = self.load_payment_method()
       self.expense_payment_method_entry = (CTkComboBox(frame2, values=payment_method, width=200))
       self.expense_payment_method_entry.grid(row=1, column=1, sticky="w", padx=10,
                                              pady=5)  # command=combobox_callback, variable=combobox_var




       categories = self.load_categories()
       CTkLabel(frame2, text="Category").grid(row=3, column=0, sticky="w", padx=20, pady=5)
       self.income_category_combo = CTkComboBox(frame2, values=categories, width=200)
       self.income_category_combo.grid(row=3, column=1, padx=10, pady=5, sticky="w")


       # self.income_source_entry = CTkEntry(frame2, width=200)
       # edit to match with the db and the predefined Categories
       # self.income_source_entry.grid(row=1, column=1, pady=5)


       CTkLabel(frame2, text="Account").grid(row=2, column=0, sticky="w", padx=20, pady=5)
       self.expense_account_combo = CTkComboBox(frame2,
                                                values=["Cash", "GCash", "Landbank", "Seabank", "Maya", "GoTyme"],
                                                width=200)  # ["Cash", "Card"]
       self.expense_account_combo.grid(row=2, column=1, sticky="w", padx=10,
                                       pady=5)  # command=combobox_callback, variable=combobox_var


       # Load categories from the database
       categories = self.load_categories()
       CTkLabel(frame2, text="Category").grid(row=3, column=0, sticky="w", padx=20, pady=5)
       self.expense_category_combo = CTkComboBox(frame2, values=categories, width=200)
       self.expense_category_combo.grid(row=3, column=1, padx=10, pady=5, sticky="w")


       CTkLabel(frame2, text="Amount").grid(row=4, column=0, sticky="w", padx=20, pady=5)
       self.expense_amount_entry = CTkEntry(frame2, width=200)
       self.expense_amount_entry.grid(row=4, column=1, padx=10, pady=5, sticky="w")


       CTkLabel(frame2, text="Date").grid(row=5, column=0, sticky="w", padx=20, pady=5)
       self.expense_date_entry = DateEntry(frame2, width=32, height=20, background='#4085AD', foreground='white',
                                           borderwidth=2)
       self.expense_date_entry.grid(row=5, column=1, padx=10, pady=10, sticky="w")


       CTkLabel(frame2, text="Description").grid(row=6, column=0, sticky="w", padx=20, pady=5)
       self.expense_desc_entry = CTkEntry(frame2, width=200, height=40)
       self.expense_desc_entry.grid(row=6, column=1, rowspan=2, padx=10, pady=5, sticky="w")


       CTkButton(frame2, text="Add Expense", command=self.add_expense).grid(row=8, column=1, padx=10, pady=10,
                                                                            sticky="e")


       self.pmethod_listbox = tkinter.Listbox(frame2, height=15, width=35)
       self.pmethod_listbox.grid(row=2, column=2, columnspan=1, rowspan=6, padx=8, pady=2)
       self.load_pmethod_listbox()


       self.add_pmethod_entry = CTkEntry(frame2, width=200)
       self.add_pmethod_entry.grid(row=1, column=2, padx=10, pady=5, sticky="e")
       CTkButton(frame2, text="Add Payment Method", font=("Arial", 12), width=120, command=self.add_pmethod).grid(
           row=1, column=3,
           padx=10, pady=5)


       CTkButton(frame2, text="Edit", font=("Arial", 12), width=120, command=self.edit_pmethod).grid(row=2, column=3,
                                                                                                     padx=10,
                                                                                                     pady=5)
       CTkButton(frame2, text="Remove", font=("Arial", 12), width=120, command=self.remove_pmethod).grid(row=3,
                                                                                                         column=3,
                                                                                                         padx=10,
                                                                                                         pady=5)


   # function for adding expense button
   def add_expense(self):
       try:
           payment_method = self.expense_payment_method_entry.get()
           account = self.expense_account_combo.get()
           amount = self.expense_amount_entry.get()
           date = self.expense_date_entry.get_date()
           category = self.expense_category_combo.get()
           desc = self.expense_desc_entry.get()


           if not payment_method or not amount or not date or not category:
               raise ValueError("All fields must be filled out")


           if amount.isalpha():
               messagebox.showerror("Error", "Invalid input for amount. Must be a number.")
               return


           self.cursor.execute("SELECT id FROM category WHERE name = ?", (category,))
           category_id = self.cursor.fetchone()


           if category_id is None:
               raise ValueError("Selected category does not exist")


           category_id = category_id[0]


           self.cursor.execute("""
               INSERT INTO TXN (transaction_description, transaction_date, amount, transaction_type, account_id, category_id, transaction_status)
               VALUES (?, ?, ?, ?, ?, ?, ?)
               """, (desc, date, amount, 'Expense', account, category_id, 'Active'))


           transaction_id = self.cursor.lastrowid
           self.cursor.execute("INSERT INTO expense (payment_method, transaction_id) VALUES (?, ?)",
                               (payment_method, transaction_id))
           self.db.commit()
           messagebox.showinfo("Success", "Expense added successfully")
           self.create_dashboard()


       except Exception as e:
           messagebox.showerror("Error", f"Failed to add expense: {str(e)}")




   def load_pmethod_listbox(self):
       self.cursor.execute("SELECT payment_method FROM expense")
       payment_methods = [row[0] for row in self.cursor.fetchall()]
       self.pmethod_listbox.delete(1, END)
       for payment_method in payment_methods:
           self.pmethod_listbox.insert(END, payment_method + "\n")


   def load_payment_method(self):
       # Execute the query to fetch categories
       self.cursor.execute("SELECT DISTINCT payment_method FROM expense")


       # Fetch all rows from the executed query
       rows = self.cursor.fetchall()


       # Extract the categories from the rows
       payment_method = [row[0] for row in rows]


       return payment_method


   def add_pmethod(self):
       name = self.add_pmethod_entry.get()
       if not name:
           messagebox.showerror("Error", "Payment method name is required")
           return


       self.cursor.execute("INSERT INTO expense (payment_method) VALUES (?)", (name,))
       result = self.cursor.fetchone()


       if result[0] > 0:
           messagebox.showerror("Error", "Payment Method name already exists")
       else:
           self.cursor.execute("INSERT INTO expense (payment_method) VALUES (?)", (name,))
           self.db.commit()
           messagebox.showinfo("Success", "Payment Method added successfully")
           self.add_expense_screen()




   def edit_pmethod(self):
       selected_indices = self.pmethod_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No payment method selected")
           return


       selected_index = selected_indices[0]
       selected_pmethod = self.pmethod_listbox.get(selected_index).strip()
       new_pmethod_name = self.add_pmethod_entry.get()
       if new_pmethod_name:
           self.cursor.execute("UPDATE expense SET payment_method = ? WHERE payment_method = ?",
                               (new_pmethod_name, selected_pmethod))
           self.db.commit()
           self.load_pmethod_listbox()
           messagebox.showinfo("Success", "Payment Method updated successfully")
       else:
           messagebox.showwarning("Warning", "New source name cannot be empty")


       payment_method = self.add_pmethod_entry.get()
       if not payment_method:
           messagebox.showerror("Error", "Payment method name is required")
           return


   def remove_pmethod(self):
       selected_indices = self.pmethod_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No payment method selected")
           return


       if messagebox.askyesno("Delete Payment Method", "Are you sure you want to delete this payment method?"):
           selected_index = selected_indices[0]
           selected_pmethod = self.pmethod_listbox.get(selected_index).strip()
           self.cursor.execute("DELETE FROM expense WHERE payment_method = ?", (selected_pmethod,))
           self.db.commit()
           self.load_pmethod_listbox()
           messagebox.showinfo("Success", "Payment Method deleted successfully")


   def combobox_callback(choice):
       print("combobox dropdown clicked:", choice)


   # def remove_pmethod(self, pmethod_id):
   #     if messagebox.askyesno("Delete Payment Method", "Are you sure you want to delete this payment method?"):
   #         self.cursor.execute("DELETE FROM payment_method WHERE pmethod_id = ?", (pmethod_id,))
   #         self.db.commit()
   #         messagebox.showinfo("Success", "Payment method deleted successfully")
   #         self.manage_pmethods()
   #
   # def combobox_callback(choice):
   #     print("combobox dropdown clicked:", choice)




# Manage Account Screen
   def manage_accounts(self):
       self.clear_screen()
       self.root.geometry("630x400")


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("back.png")  # image name
       back = CTkImage(image, size=(25, 25))


       settings = PIL.Image.open("settings.png")
       setting = CTkImage(settings, size=(30, 30))
       CTkButton(frame1, text="", image=back, fg_color="transparent", width=40, command=self.create_dashboard).grid(
           row=0, column=0, padx=(5, 50), pady=5)


       CTkLabel(frame1,
                text="                                                                                            ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2,
                                                                       padx=(5, 50), pady=5)
       (CTkButton(frame1, image=setting, text="", fg_color="transparent", width=50, command=self.user_settings).grid
        (row=0, column=5, padx=50, pady=5, sticky="e"))
       CTkLabel(frame2, text="Manage Accounts", font=("Arial", 24, "bold")).grid(row=0, column=0, columnspan=2,
                                                                                   padx=20,
                                                                                   pady=10, sticky="w")


       self.account_listbox =  tkinter.Listbox(frame2, height=20, width=40)
       self.account_listbox.grid(row=1, column=0, columnspan=1, rowspan=6, padx=10, pady=5)
       self.load_account_listbox()


       CTkLabel(frame2, text="Account Name").grid(row=1, column=1, columnspan=1, rowspan=1, padx=5, pady=(5,0), sticky="w")
       self.account_name_entry = CTkEntry(frame2, width=200)
       self.account_name_entry.grid(row=1, column=2, rowspan=1, padx=5, pady=5)


       CTkLabel(frame2, text="Account Type").grid(row=2, column=1, columnspan=1, sticky="w", padx=20, pady=(5,0))
       self.account_type_combo = CTkComboBox(frame2, values=["Cash", "Card", "Savings", "Debit"], width=200)
       self.account_type_combo.grid(row=2, column=2, rowspan=1, sticky="w", padx=10, pady=5)


       CTkLabel(frame2, text="Initial Balance").grid(row=3, column=1, columnspan=1,  padx=5, pady=(5,0), sticky="w")
       self.initial_balance_entry = CTkEntry(frame2, width=200)
       self.initial_balance_entry.grid(row=3, column=2, rowspan=1,  padx=5, pady=(5,0), sticky="w")


       CTkButton(frame2, text="Add Account", width=160, command=self.add_account).grid(row=4, column=1, columnspan=1, pady=6
                                                                           )
       CTkButton(frame2, text="Update Selected Account", width=160, command=self.edit_account).grid(row=5, column=1, columnspan=1,
                                                                                         pady=(1,0))
       CTkButton(frame2, text="Delete Selected Account", width=160, command=self.delete_account).grid(row=6, column=1, pady=(2,0)
                                                                                          )




   def load_account_listbox(self):
       self.cursor.execute("SELECT account_name FROM account")
       accounts = [row[0] for row in self.cursor.fetchall()]
       self.account_listbox.delete(1, END)
       for account in accounts:
           self.account_listbox.insert(END, account + "\n")


   def add_account(self):
       account_name = self.account_name_entry.get()
       account_type = self.account_type_combo.get()
       initial_bal = self.initial_balance_entry.get()
       self.cursor.execute("INSERT INTO account (account_name, account_type, initial_balance) VALUES (?, ?, ?)", (account_name, account_type, initial_bal,))
       self.db.commit()
       self.load_account_listbox()
       messagebox.showinfo("Success", "Account added successfully")


   def edit_account(self):
       try:
           # Get the selected account
           selected_indices = self.account_listbox.curselection()
           if not selected_indices:
               raise IndexError("No account selected")


           selected_index = selected_indices[0]
           selected_account = self.account_listbox.get(selected_index).strip()
           new_account_name = self.account_name_entry.get()


           if not new_account_name:
               messagebox.showerror("Error", "New account name cannot be empty")
               return


           else:
               # Update the account name in the database
               self.cursor.execute("UPDATE account SET account_name = ? WHERE account_name = ?",
                                   (new_account_name, selected_account))
               self.db.commit()


               # Reload the listbox with updated data
               self.load_account_listbox()


           messagebox.showinfo("Success", "Account updated successfully")
       except IndexError as e:
           messagebox.showerror("Error", str(e))
       except Exception as e:
           messagebox.showerror("Error", f"An error occurred: {e}")


   def delete_account(self):
       try:
           # Get the selected account
           selected_indices = self.account_listbox.curselection()
           if not selected_indices:
               raise IndexError("No account selected")


           selected_index = selected_indices[0]
           selected_account = self.account_listbox.get(selected_index).strip()


           # Delete the account from the database
           self.cursor.execute("DELETE FROM account WHERE account_name = ?", (selected_account,))
           self.db.commit()


           # Reload the listbox with updated data
           self.load_account_listbox()


           messagebox.showinfo("Success", "Account deleted successfully")
       except IndexError as e:
           messagebox.showerror("Error", str(e))
       except Exception as e:
           messagebox.showerror("Error", f"An error occurred: {e}")




# Manage Categories Screen
   def manage_categories(self):
       self.clear_screen()
       self.root.geometry("630x400")


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("back.png")  # image name
       back = CTkImage(image, size=(25, 25))


       settings = PIL.Image.open("settings.png")
       setting = CTkImage(settings, size=(30, 30))
       CTkButton(frame1, text="", image=back, fg_color="transparent", width=40, command=self.create_dashboard).grid(
           row=0, column=0, padx=(5, 50), pady=5)


       CTkLabel(frame1,
                text="                                                                                            ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2,
                                                                       padx=(5, 50), pady=5)
       (CTkButton(frame1, image=setting, text="", fg_color="transparent", width=50, command=self.user_settings).grid
        (row=0, column=5, padx=50, pady=5, sticky="e"))
       CTkLabel(frame2, text="Manage Categories", font=("Arial", 24, "bold")).grid(row=0, column=0, columnspan=2,
                                                                                   padx=20,
                                                                                   pady=10, sticky="w")


       self.category_listbox = tkinter.Listbox(frame2, height=20, width=40)
       self.category_listbox.grid(row=1, column=0, columnspan=2, rowspan=6, pady=5)
       self.load_categories_listbox()


       CTkLabel(frame2, text="Category Name").grid(row=1, column=2, columnspan=1, pady=2)
       self.category_name_entry = CTkEntry(frame2, width=200)
       self.category_name_entry.grid(row=2, column=2, columnspan=1, pady=2)


       # CTkLabel(frame2, text="").grid(row=1, column=1, columnspan=1, pady=5)


       CTkButton(frame2, text="Add Category", command=self.add_category).grid(row=3, column=2, columnspan=1, pady=5)
       CTkButton(frame2, text="Edit Selected Category", command=self.edit_category).grid(row=4, column=2, columnspan=1,
                                                                                         pady=5)
       CTkButton(frame2, text="Delete Selected Category", command=self.delete_category).grid(row=5, column=2, pady=5)




   def load_categories(self):
       # Execute the query to fetch categories
       self.cursor.execute("SELECT name FROM category")


       # Fetch all rows from the executed query
       rows = self.cursor.fetchall()


       # Extract the categories from the rows
       categories = [row[0] for row in rows]


       return categories


   def load_categories_listbox(self):
       self.cursor.execute("SELECT name FROM category")
       categories = [row[0] for row in self.cursor.fetchall()]
       self.category_listbox.delete(1, END)
       for category in categories:
           self.category_listbox.insert(END, category + "\n")


   def add_category(self):
       category_name = self.category_name_entry.get()
       self.cursor.execute("INSERT INTO category (name) VALUES (?)", (category_name,))
       self.db.commit()
       self.load_categories_listbox()
       messagebox.showinfo("Success", "Category added successfully")




   def edit_category(self):
       selected_indices = self.category_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No category selected")
           return


       selected_index = selected_indices[0]
       selected_category = self.category_listbox.get(selected_index).strip()
       new_category_name = self.category_name_entry.get()
       if new_category_name:
           self.cursor.execute("UPDATE category SET name = ? WHERE name = ?", (new_category_name, selected_category))
           self.db.commit()
           self.load_categories_listbox()
           messagebox.showinfo("Success", "Category updated successfully")
       else:
           messagebox.showwarning("Warning", "New category name cannot be empty")




   def delete_category(self):
       selected_indices = self.category_listbox.curselection()
       if not selected_indices:
           messagebox.showwarning("Warning", "No category selected")
           return


       selected_index = selected_indices[0]
       selected_category = self.category_listbox.get(selected_index).strip()
       self.cursor.execute("DELETE FROM category WHERE name = ?", (selected_category,))
       self.db.commit()
       self.load_categories_listbox()
       messagebox.showinfo("Success", "Category deleted successfully")




# User Management Screen
   def user_settings(self):
       # Set the desired window size
       # self.root.geometry("745x550")  # Change the size as needed
       self.root.geometry("650x400")
       self.clear_screen()


       frame1 = CTkFrame(self.root, corner_radius=0, fg_color="#4085AD")
       frame1.grid(row=0, column=0, sticky="nwe", ipadx=window_width, ipady=2)


       frame2 = CTkFrame(self.root, corner_radius=0, fg_color="#8ECAEE")
       frame2.grid(row=1, column=0, sticky="swe", ipadx=window_width, ipady=window_height)


       image = PIL.Image.open("fflowbluWhite.png")
       logo = CTkImage(image, size=(30, 30))


       image = PIL.Image.open("homee.png")
       home = CTkImage(image, size=(30, 30))


       CTkLabel(frame1, text="", image=logo, bg_color="white").grid(row=0, column=0, padx=10, pady=5)
       CTkLabel(frame1, text="FINANCEFLOW", text_color="white", font=("Arial", 14, "bold")).grid(row=0, column=1,
                                                                                                 padx=(5, 50), pady=5)
       CTkLabel(frame1, text="                                                                          ",
                text_color="#4085AD", font=("Arial", 14, "bold")).grid(row=0, column=2, padx=(5, 50), pady=5)
       CTkButton(frame1, image=home, text="", fg_color="transparent", width=50, command=self.create_dashboard).grid(
           row=0, column=10, padx=(0, 10), pady=(5, 0), sticky="ne")


       CTkLabel(frame2, text="USER MANAGEMENT", font=("Arial", 24, "bold")).grid(row=0, column=0, sticky="nw", padx=15,
                                                                          pady=10)


       CTkLabel(frame2, text="Update Account", font=("Arial", 18)).grid(row=1, column=0, padx=15, pady=10)


       CTkLabel(frame2, text="Username:").grid(row=2, column=0, sticky="w", padx=25, pady=5)
       self.username_entry = CTkEntry(frame2, width=200)
       self.username_entry.grid(row=2, column=1, pady=5, padx=(0, 10), sticky="w")


       CTkLabel(frame2, text="Password:").grid(row=3, column=0, sticky="w", padx=25, pady=5)
       self.password_entry = CTkEntry(frame2, show="*", width=200)
       self.password_entry.grid(row=3, column=1, pady=5, padx=(0, 10), sticky="w")


       self.show_password_var = IntVar()
       image_eye_open = PIL.Image.open("view.png")
       image_eye_closed = PIL.Image.open("hide.png")
       self.eye_open = CTkImage(image_eye_open, size=(20, 20))
       self.eye_closed = CTkImage(image_eye_closed, size=(20, 20))
       self.eye_button = CTkButton(frame2, text="", image=self.eye_closed, fg_color="transparent", width=20,
                                   command=self.toggle_password_visibility)
       self.eye_button.grid(row=3, column=2, columnspan=1, sticky="w", padx=(0, 5), pady=5)


       CTkLabel(frame2, text="Email:").grid(row=4, column=0, sticky="w", padx=25, pady=5)
       self.email_entry = CTkEntry(frame2, width=200)
       self.email_entry.grid(row=4, column=1, pady=5, padx=(0, 10), sticky="w")


       CTkButton(frame2, text="Save Changes", text_color="black", fg_color="#4085AD", command=self.update_user).grid(
           row=2, column=2, columnspan=1, padx=10, pady=5, sticky="e")


       CTkButton(frame2, text="Log Out", text_color="white", fg_color="#292929", command=self.Confirm_logout).grid(
           row=4, column=2, columnspan=1, padx=10, pady=5, sticky="e")


       # Create a frame for the Treeview and scrollbar
       log_frame = CTkFrame(frame2)
       log_frame.grid(row=5, column=0, columnspan=7, padx=10, pady=10, sticky="nsew")


       # Table to display user update logs with smaller columns
       self.update_log_table = ttk.Treeview(log_frame, columns=(
           "update_time", "old_username", "new_username", "old_password", "new_password", "old_email", "new_email"),
                                            show="headings", height=5)
       self.update_log_table.heading("update_time", text="Update Time")
       self.update_log_table.heading("old_username", text="Old Username")
       self.update_log_table.heading("new_username", text="New Username")
       self.update_log_table.heading("old_password", text="Old Password")
       self.update_log_table.heading("new_password", text="New Password")
       self.update_log_table.heading("old_email", text="Old Email")
       self.update_log_table.heading("new_email", text="New Email")


       # Adjust column widths
       self.update_log_table.column("update_time", width=100)
       self.update_log_table.column("old_username", width=100)
       self.update_log_table.column("new_username", width=100)
       self.update_log_table.column("old_password", width=100)
       self.update_log_table.column("new_password", width=100)
       self.update_log_table.column("old_email", width=100)
       self.update_log_table.column("new_email", width=100)


       # Add the Treeview to the grid
       self.update_log_table.grid(row=0, column=0, padx=(5, 0), sticky="nsew")


       # Create a vertical scrollbar
       scrollbar = ttk.Scrollbar(log_frame, orient="vertical", command=self.update_log_table.yview)
       scrollbar.grid(row=0, column=1, sticky="ns")
       self.update_log_table.configure(yscrollcommand=scrollbar.set)


       self.load_update_log()




   def update_user(self):
       new_username = self.username_entry.get()
       new_password = self.password_entry.get()
       new_email = self.email_entry.get()


       if not new_username or not new_password or not new_email:
           messagebox.showwarning("Input Error", "Please fill in all fields.")
           return


       # Check if the email is valid and ends with either '@gmail.com' or '@yahoo.com'
       if not re.match(r"[^@]+@[^@]+\.[^@]+", new_email) or not (
               new_email.endswith("gmail.com") or new_email.endswith("yahoo.com")):
           messagebox.showwarning("Input Error", "Please enter a valid email address.")
           return


       # Check if the new username already exists
       self.cursor.execute("SELECT user_id FROM user WHERE username = ?", (new_username,))
       result = self.cursor.fetchone()
       if result and result[0] != self.current_user_id:
           messagebox.showwarning("Input Error", "Username already taken. Please choose a different username.")
           return


       self.cursor.execute("SELECT username, password, email FROM user WHERE user_id = ?", (self.current_user_id,))
       old_username, old_password, old_email = self.cursor.fetchone()


       self.cursor.execute("UPDATE user SET username = ?, password = ?, email = ? WHERE user_id = ?",
                           (new_username, new_password, new_email, self.current_user_id))
       self.db.commit()


       self.cursor.execute("""
              INSERT INTO user_update_log (user_id, old_username, old_password, old_email, new_username, new_password, new_email)
              VALUES (?, ?, ?, ?, ?, ?, ?)""",
                           (self.current_user_id, old_username, old_password, old_email, new_username, new_password,
                            new_email))
       self.db.commit()


       messagebox.showinfo("Update Successful", "Your account has been updated.")
       self.load_update_log()




   def load_update_log(self):
       self.update_log_table.delete(*self.update_log_table.get_children())
       self.cursor.execute(
           "SELECT update_time, old_username, new_username, old_password, new_password, old_email, new_email FROM user_update_log WHERE user_id = ?",
           (self.current_user_id,))
       rows = self.cursor.fetchall()
       for row in rows:
           self.update_log_table.insert("", "end", values=row)




# Logout Screen
   def Confirm_logout(self):
       self.clear_screen()
       frame = CTkFrame(self.root, corner_radius=10, fg_color="#D6EAF8")
       frame.pack(expand=True, padx=20, pady=20)
       CTkLabel(frame, text="Log Out?", font=("Arial", 24)).grid(row=0, column=0, columnspan=2, pady=10)
       CTkButton(frame, text="Yes", command=self.logout).grid(row=1, column=0, columnspan=2, pady=5)
       CTkButton(frame, text="No", command=self.create_dashboard).grid(row=2, column=0, columnspan=2, pady=5)


   # function for the logout button confirmed
   def logout(self):
       messagebox.showinfo("Exit", "Logout Successful")
       self.create_login_screen()
       self.db.close()
       #root.destroy




if __name__ == "__main__":
   root = CTk()
   root.title("Income and Expense Management System")
   app = IncomeExpenseApp(root)
   root.mainloop()










