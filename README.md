import random
import string
import tkinter as tk
from tkinter import messagebox

def generate_password(length, use_letters, use_numbers, use_symbols):
    character_set = ''
    
    if use_letters:
        character_set += string.ascii_letters
    if use_numbers:
        character_set += string.digits
    if use_symbols:
        character_set += string.punctuation

    if not character_set:
        raise ValueError("At least one character type must be selected.")

    password = ''.join(random.choice(character_set) for _ in range(length))
    return password

def on_generate():
    try:
        length = int(length_entry.get())
        use_letters = letters_var.get()
        use_numbers = numbers_var.get()
        use_symbols = symbols_var.get()

        password = generate_password(length, use_letters, use_numbers, use_symbols)
        password_entry.delete(0, tk.END)
        password_entry.insert(0, password)
    except ValueError as e:
        messagebox.showerror("Error", str(e))

def copy_to_clipboard():
    root.clipboard_clear()
    root.clipboard_append(password_entry.get())
    messagebox.showinfo("Copied", "Password copied to clipboard!")

# GUI Setup
root = tk.Tk()
root.title("Password Generator")

tk.Label(root, text="Password Length:").grid(row=0, column=0)
length_entry = tk.Entry(root)
length_entry.grid(row=0, column=1)

letters_var = tk.BooleanVar()
tk.Checkbutton(root, text="Include Letters", variable=letters_var).grid(row=1, columnspan=2)

numbers_var = tk.BooleanVar()
tk.Checkbutton(root, text="Include Numbers", variable=numbers_var).grid(row=2, columnspan=2)

symbols_var = tk.BooleanVar()
tk.Checkbutton(root, text="Include Symbols", variable=symbols_var).grid(row=3, columnspan=2)

tk.Button(root, text="Generate Password", command=on_generate).grid(row=4, columnspan=2)

tk.Label(root, text="Generated Password:").grid(row=5, column=0)
password_entry = tk.Entry(root)
password_entry.grid(row=5, column=1)

tk.Button(root, text="Copy to Clipboard", command=copy_to_clipboard).grid(row=6, columnspan=2)

root.mainloop()
