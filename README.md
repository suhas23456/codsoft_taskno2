# codsoft_taskno2
#task 2 : Calculator app using python

import tkinter as tk
from tkinter import ttk, messagebox

class CalculatorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Simple Calculator")
        self.dark_mode = True  # Initial mode is dark

        self.create_widgets()

    def create_widgets(self):
        # Entry widget for user input
        self.entry = tk.Entry(self.root, width=20, font=("Helvetica", 16), bd=5, relief=tk.SUNKEN, bg='#2E2E2E', fg='white')
        self.entry.grid(row=0, column=0, columnspan=4, padx=10, pady=10, ipady=5)  # Increased internal padding

        # Style for rounded buttons
        self.style = ttk.Style()
        self.style.theme_use('clam')  # Use the 'clam' theme for ttk widgets (similar to classic)
        self.toggle_mode_button = ttk.Button(self.root, text="Dark Mode", command=self.toggle_mode)
        self.toggle_mode_button.grid(row=0, column=5, padx=5, pady=5)

        self.update_style()  # Initial style update

        # Buttons for digits and operations
        buttons = [
            ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
            ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3),
            ('C', 5, 0)
        ]

        for (text, row, column) in buttons:
            button = ttk.Button(self.root, text=text, command=lambda t=text: self.on_button_click(t),
                                style='Rounded.TButton', width=5)

            if text.isdigit():
                button.configure(style='Rounded.TButton', padding=(20, 15))
            else:
                button.configure(style='Rounded.TButton', padding=(18, 15))

            button.grid(row=row, column=column, padx=5, pady=5, sticky="nsew")
            self.root.grid_columnconfigure(column, weight=1)
            self.root.grid_rowconfigure(row, weight=1)

    def on_button_click(self, value):
        current_text = self.entry.get()

        if value == 'C':
            self.entry.delete(0, tk.END)
        elif value == '=':
            try:
                result = eval(current_text)
                self.entry.delete(0, tk.END)
                self.entry.insert(tk.END, str(result))
            except Exception as e:
                messagebox.showerror("Error", "Invalid input")
        else:
            self.entry.insert(tk.END, value)

    def toggle_mode(self):
        self.dark_mode = not self.dark_mode
        self.update_style()

    def update_style(self):
        if self.dark_mode:
            self.root.configure(bg='#1E1E1E')  # Dark mode
            self.entry.configure(bg='#2E2E2E', fg='white')  # Dark mode entry
            self.style.configure('Rounded.TButton', background='#FF8C00', foreground='white')  # Orange button with white text
            self.toggle_mode_button.configure(text="Light Mode")
        else:
            self.root.configure(bg='white')  # Light mode
            self.entry.configure(bg='white', fg='black')  # Light mode entry
            self.style.configure('Rounded.TButton', background='#FF8C00', foreground='black')  # Orange button with black text
            self.toggle_mode_button.configure(text="Dark Mode")

if __name__ == "__main__":
    root = tk.Tk()
    app = CalculatorApp(root)
    root.mainloop()
