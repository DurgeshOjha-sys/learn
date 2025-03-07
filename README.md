import tkinter as tk
from tkinter import ttk, messagebox
from PIL import Image, ImageTk  

class PachGuideDashboard(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("Pach Guide - Parental Control App")
        self.geometry("800x500")
        self.resizable(False, False)

        # 🎨 Load Background Image
        self.bg_image = Image.open("istockphoto-1060580214-612x612.jpg")  
        self.bg_image = self.bg_image.resize((800, 500), Image.LANCZOS)
        self.bg_photo = ImageTk.PhotoImage(self.bg_image)

        # 🖼 Canvas for Background
        self.canvas = tk.Canvas(self, width=800, height=500)
        self.canvas.pack(fill="both", expand=True)
        self.canvas.create_image(0, 0, image=self.bg_photo, anchor="nw")

        # 🏠 NAVIGATION BAR
        nav_frame = tk.Frame(self.canvas, bg="#003366", height=50)
        self.canvas.create_window(400, 25, window=nav_frame, width=800, height=50)

        # 🌟 Logo Section
        logo_label = tk.Label(nav_frame, text="🎯 Pach Guide", font=("Arial", 14, "bold"), fg="white", bg="#003366")
        logo_label.pack(side="left", padx=20, pady=10)

        # 🖱 Navigation Buttons
        btn_frame = tk.Frame(nav_frame, bg="#003366")
        btn_frame.pack(side="right", padx=10)

        self.buttons = []  
        for text, cmd in [("Home", self.home_action), 
                          ("Login", self.show_login_form), 
                          ("Sign Up", self.show_signup_form),  
                          ("About Us", self.about_action), 
                          ("Contact Us", self.contact_action)]:
            btn = tk.Button(btn_frame, text=text, command=cmd, bg="#0077CC", fg="white", 
                            font=("Arial", 10, "bold"), padx=10, pady=5, borderwidth=1, relief="solid")
            btn.pack(side="left", padx=5)
            btn.bind("<Enter>", lambda e, b=btn: b.config(bg="#0099FF"))  
            btn.bind("<Leave>", lambda e, b=btn: b.config(bg="#0077CC"))  
            self.buttons.append(btn)

        # 🔐 LOGIN & SIGN-UP FORMS
        self.login_frame = self.create_login_form()
        self.signup_frame = self.create_signup_form()

        self.login_frame.place_forget()
        self.signup_frame.place_forget()

    # 🟢 LOGIN FORM
    def create_login_form(self):
        frame = tk.Frame(self, bg="white", padx=20, pady=20, relief="ridge", borderwidth=2)

        tk.Label(frame, text="🔑 Login", font=("Arial", 14, "bold"), fg="#003366", bg="white").pack(pady=5)
        tk.Label(frame, text="Username:", font=("Arial", 12), bg="white").pack(anchor="w")
        self.username_entry = ttk.Entry(frame, width=25)
        self.username_entry.pack(pady=5)

        tk.Label(frame, text="Password:", font=("Arial", 12), bg="white").pack(anchor="w")
        self.password_entry = ttk.Entry(frame, width=25, show="*")
        self.password_entry.pack(pady=5)

        # 🔹 Buttons (Only Login & Sign Up)
        tk.Button(frame, text="Login", command=self.login_action, bg="#0077CC", fg="white", 
                  font=("Arial", 10, "bold"), padx=5, pady=2).pack(pady=5, fill="x")
        tk.Button(frame, text="Sign Up", command=self.show_signup_form, bg="#FFA500", fg="white", 
                  font=("Arial", 10, "bold"), padx=5, pady=2).pack(pady=5, fill="x")

        return frame

    # 🔵 SIGN-UP FORM
    def create_signup_form(self):
        frame = tk.Frame(self, bg="white", padx=20, pady=20, relief="ridge", borderwidth=2)

        tk.Label(frame, text="📝 Sign Up", font=("Arial", 14, "bold"), fg="#003366", bg="white").pack(pady=5)
        tk.Label(frame, text="Username:", font=("Arial", 12), bg="white").pack(anchor="w")
        self.signup_username = ttk.Entry(frame, width=25)
        self.signup_username.pack(pady=5)

        tk.Label(frame, text="Email:", font=("Arial", 12), bg="white").pack(anchor="w")
        self.signup_email = ttk.Entry(frame, width=25)
        self.signup_email.pack(pady=5)

        tk.Label(frame, text="Password:", font=("Arial", 12), bg="white").pack(anchor="w")
        self.signup_password = ttk.Entry(frame, width=25, show="*")
        self.signup_password.pack(pady=5)

        # 🔹 Buttons (Create Account & Back to Login)
        tk.Button(frame, text="Create Account", command=self.signup_action, bg="#0077CC", fg="white", 
                  font=("Arial", 10, "bold"), padx=5, pady=2).pack(pady=5, fill="x")
        tk.Button(frame, text="Back to Login", command=self.show_login_form, bg="#FFA500", fg="white", 
                  font=("Arial", 10, "bold"), padx=5, pady=2).pack(pady=5, fill="x")

        return frame

    # 🔄 Switch between Login & Sign-Up
    def show_login_form(self):
        self.signup_frame.place_forget()
        self.login_frame.place(x=270, y=150, width=260, height=200)  # Adjusted height

    def show_signup_form(self):
        self.login_frame.place_forget()
        self.signup_frame.place(x=270, y=150, width=260, height=260)

    # 📜 Login Action
    def login_action(self):
        username = self.username_entry.get()
        password = self.password_entry.get()
        if username and password:
            messagebox.showinfo("Success", f"Logged in as: {username}")
        else:
            messagebox.showwarning("Login Failed", "Please enter username and password!")

    # ✨ Sign-Up Action with Validation
    def signup_action(self):
        username = self.signup_username.get()
        email = self.signup_email.get()
        password = self.signup_password.get()

        if not username or not email or not password:
            messagebox.showwarning("Sign Up Failed", "All fields are required!")
        else:
            messagebox.showinfo("Success", f"Account created for {username}!")
            self.show_login_form()

    def home_action(self):
        messagebox.showinfo("Home", "Navigating to Home...")

    def about_action(self):
        messagebox.showinfo("About Us", "Opening About Us...")

    def contact_action(self):
        messagebox.showinfo("Contact Us", "Opening Contact Us...")

if __name__ == "__main__":
    app = PachGuideDashboard()
    app.mainloop()
