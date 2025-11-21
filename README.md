# Regula

**Regula** is a lightweight, simplified Python wrapper around the **Tkinter** library, designed to make creating basic desktop applications faster and more readable, especially for small projects or educational purposes.

It abstracts away some of the complexities of Tkinter's layout managers (like `grid` and `pack`), offering a streamlined, declarative way to build GUIs.

## ✨ Features

* **Simplified Widget Creation:** One function call for common widgets (`Label`, `Button`, `Entry`, `Frame`, etc.).
* **Automatic Layout:** Automatically handles placement using `grid` (when `line` and `position` are provided) or `pack`.
* **Built-in Utilities:** Simple functions for message boxes (`Info`, `Error`), file dialogues (`FileOpener`), and input prompts (`Ask`).
* **Unified Value Retrieval:** Use the `Get(widget)` function to easily retrieve the current value from all input widgets.

## 🚀 Installation

Assuming you have Python 3.8+ installed:

```bash
pip install regula

```

# Example Usage

```python

import regula as sg
import random
import sys # Needed for file reading error handling

# Set the window title and size using the fluent interface for Window configuration
sg.Window().Title("Regula Example App - Full Demo").Size("890x380").Background("#E6E6FA")

# --- Global Widgets for Tab 7 & 8 ---
# Need a global reference to the Textarea so the button command can update it
text_output_area = None
# Global reference for the Entry widget in Tab 8
password_entry = None


def open_file_and_read():
    """Opens a file dialog, reads the selected file, and updates Tab 7 Textarea."""
    global text_output_area
    
    # Use sg.FileOpener() to open the native OS file dialog.
    filepath = sg.FileOpener(
        ('Text Files', '*.txt'), 
        ('All Files', '*.*')
    )

    if filepath:
        try:
            with open(filepath, 'r', encoding='utf-8') as f:
                content = f.read()
            
            # Clear Textarea and insert new content
            # Accessing the underlying Tk constant via the regula alias 'sg'
            text_output_area.delete('1.0', sg.tk.END)
            text_output_area.insert('1.0', content)
            
            sg.Info("File Loaded", f"Content from file loaded successfully.")
            
        except FileNotFoundError:
            sg.Error("Error", f"File not found: {filepath}")
        except Exception as e:
            sg.Error("Read Error", f"An error occurred while reading the file: {e}")
    else:
        sg.Info("Cancelled", "File selection was cancelled.")

# --- Tab Setup ---

notebook = sg.Notebook(expand=True)
# Tabs 1-6 remain the same size for consistency
tab1 = sg.Frame(notebook, 'Button, Label and Info', expand=True, tab_color='lightgreen', _height=350, _width=500)
tab2 = sg.Frame(notebook, 'Textarea and Error', expand=True, tab_color='lightgreen', _height=350, _width=500)
tab3 = sg.Frame(notebook, 'Entry and Scale', expand=True, tab_color='lightgreen', _height=350, _width=500)
tab4 = sg.Frame(notebook, 'Listbox', expand=True, tab_color='lightgreen', _height=350, _width=500)
tab5 = sg.Frame(notebook, 'Ask', expand=True, tab_color='lightgreen', _height=350, _width=500)
tab6 = sg.Frame(notebook, 'Wait and Configure', expand=True, tab_color='lightgreen', _height=350, _width=500)

# --- Tab 7: FileOpener ---
tab7 = sg.Frame(notebook, 'FileOpener', expand=True, tab_color='#ADD8E6', _height=350, _width=500)

# --- Tab 8: New Configure (is_password) ---
tab8 = sg.Frame(notebook, 'New Configure', expand=True, tab_color='#FFD700', _height=350, _width=500)


# -----------------------------------------------------
## Tab 1 Content: Button, Label, Info
# -----------------------------------------------------
def to_do():
    sg.Info("Yay!", "Haha!")
    
# Demonstrating 'bold' configuration on a Label
sg.Label(tab1, line=0, position=0, align='left', expand=True, bold=True, content='Press the button: ')
# Demonstrating Button configuration args (_width, _height)
sg.Button(tab1, _command=to_do, line=0, position=1, align='center', expand=True, _width=15, _height=1)

# -----------------------------------------------------
## Tab 2 Content: Textarea, Error
# -----------------------------------------------------
texter = sg.Textarea(tab2, 'Do not enter text!', expand=True)
def get_texter():
    global texter
    results = sg.Get(texter)
    if not results == 'Do not enter text!':
        sg.Error('Hey!', f'Hey! you should not enter text here, especially not "{results}"!')
    else:
        sg.Error('Hey!', 'Thanks for not entering text, but WHY DID YOU PRESS THE BUTTON?!')
        
# Demonstrating passing direct Tkinter args (like background) to Button
sg.Button(tab2, "And don't press here", expand=True, _command=get_texter, background_color='red')

# -----------------------------------------------------
## Tab 3 Content: Entry, Scale, Get
# -----------------------------------------------------
sg.Label(tab3, 'Your username: ', line=0, position=0, expand=True, align='left')
# Demonstrating initial_content arg in Entry
username = sg.Entry(tab3, 'superprogrammer', expand=True, align='center', line=0, position=1)
sg.Label(tab3, 'Your age: ', line=1, position=0, expand=True, align='left')
# Demonstrating from_value and to_value in Scale
age = sg.Scale(tab3, from_value=5, to_value=100, line=1, position=1, expand=True, align='center')

def get_settings():
    global username, age
    yuser = sg.Get(username)
    yager = sg.Get(age)
    yager = int(yager)
    if yuser:
        if yager >= 18:
            sg.Info('Hello!', f'Hello!, {yuser}, you are an adult (Age: {yager})!')
        else:
            sg.Info('Hello!', f'Hello, {yuser}, you are a child (Age: {yager})!')
    else:
        sg.Info('Hey!', f'You need to enter a username!')

sg.Button(tab3, 'Get Data', _command=get_settings, position=1, line=2, expand=True, align='center')

# -----------------------------------------------------
## Tab 4 Content: Listbox
# -----------------------------------------------------
sg.Label(tab4, 'Select the best GUI wrapper:', line=0, position=0, align='left', expand=True, content_size=14, bold=True)
# Demonstrating Listbox creation and values argument
my_list = sg.Listbox(tab4, ['Regula', 'Tkinter', 'PySimpleGUI'], line=1, position=0, align='left', expand=True, height=5)

def get_gui():
    global my_list
    results = sg.Get(my_list)
    
    # Handle both single string (single mode) or list of strings (multi/extended mode)
    selected_value = results if isinstance(results, str) else results[0] if results else None
    
    if selected_value and selected_value != 'Regula':
        sg.Error('Hey!', f'Hey! Regula is better than {selected_value}!')
    elif selected_value == 'Regula':
        sg.Info('Yeah!', 'Yeah! Regula is the best!')
    else:
        sg.Info('Selection', 'Nothing was selected.')

sg.Button(tab4, 'Choose', _command=get_gui, line=2, position=0, align='center')

# -----------------------------------------------------
## Tab 5 Content: Ask (BMI Calculator)
# -----------------------------------------------------
def ask():
    name = sg.Ask('Name', 'Name:', 'string')
    if name:
        age = sg.Ask('Age', 'Age:', 'int')
        if age:
            if age > 17:
                weight = sg.Ask('Weight', 'Weight (kg):', 'float')
                if weight:
                    height = sg.Ask('Height', 'Height in meters (e.g., 1.75):', 'float')
                    if height and height > 0:
                        
                        def imc():
                            # Corrected BMI calculation: weight / (height ** 2)
                            return weight / (height**2)
                        
                        healthy = imc()
                        
                        if healthy > 18.4 and healthy < 25.0:
                            status = 'healthy'
                        elif healthy < 18.5:
                            status = 'not so healthy, too slim'
                        elif healthy >= 25.0 and healthy < 30.0:
                            status = 'not so healthy, a little overweight (or too many muscles)'
                        else:
                            status = 'not healthy, a lot overweight (or too many muscles)'

                        healthy = round(healthy, 1)

                        sg.Info('Health Condition', f'We calculated your IMC, {name}, and it returned: {status} (Your IMC is {healthy})')
            
            else:
                sg.Info('Sorry', f"Sorry, {name}, but we can't calculate the IMC of kids")
                return
                
sg.Button(tab5, 'Calculate health condition', _command=ask, position=0, line=0, expand=True, align='center')

# -----------------------------------------------------
## Tab 6 Content: Wait and Configure
# -----------------------------------------------------
def changer():
    timer = random.randint(1500, 3000)
    # Uses sg.Wait()
    sg.Wait(timer, change)

# Demonstrating initial button configuration
my_Button = sg.Button(tab6, 'I will change some seconds after you click me!', content_size=12, content_color='black', _command=changer)

def change():
    global my_Button
    colors = ['red', 'green', 'yellow', 'blue', 'orange', 'purple', 'magenta']
    color = random.choice(colors) # foreground color
    color2 = random.choice(colors) # background color
    if color2 == color:
        new_colors = colors.copy()
        new_colors.remove(color)
        color2 = random.choice(new_colors)
        
    messages = ['Changed! (I can change again)', 'Told ya I would change!', 'CHAAANGEED!', 'New look!']
    message = random.choice(messages)
    
    # Uses sg.Configure() with Regula arguments: content, content_color (fg), background_color (bg)
    sg.Configure(my_Button, 
                 content=message, 
                 content_color=color, 
                 background_color=color2,
                 bold=True, # New Configure arg
                 _width=40) # New Configure arg

# -----------------------------------------------------
## Tab 7 Content: FileOpener
# -----------------------------------------------------

# Button to trigger the file dialog
sg.Button(
    parent=tab7,
    content="Select and Load Text File (.txt)", 
    _command=open_file_and_read,
    line=0,
    position=0,
    align='n'
)

# Textarea to display the file content
text_output_area = sg.Textarea(
    parent=tab7,
    initial_content="File content will appear here here after you select a file...",
    line=1,
    position=0,
    height=15,
    width=75,
    expand=True,
    align='center' 
)

# -----------------------------------------------------
## Tab 8 Content: New Configure (is_password)
# -----------------------------------------------------

# Create the password Entry widget globally in the tab
password_entry = sg.Entry(
    parent=tab8, 
    initial_content='RegulaIsAwesome', 
    is_password=True, # Initial setting: hidden
    width=30,
    line=0, 
    position=0, 
    expand=True, 
    align='center'
)

def show_password():
    """Configures the Entry widget to show characters."""
    global password_entry
    # Configure using the Regula argument 'is_password=False'
    sg.Configure(password_entry, is_password=False)

def hide_password():
    """Configures the Entry widget to hide characters."""
    global password_entry
    # Configure using the Regula argument 'is_password=True'
    sg.Configure(password_entry, is_password=True)

# Create the Checkbutton, linking select_command and unselect_command to the configuration functions
sg.Checkbutton(
    parent=tab8,
    content='Show password', 
    is_checked=False, # Initial state: hidden
    select_command=show_password, 
    unselect_command=hide_password, # New Checkbutton command logic
    line=1, 
    position=0, 
    expand=True, 
    align='w'
)


# -----------------------------------------------------
## Run the application
# -----------------------------------------------------
sg.Run()

```

# 🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

# 📄 License
Distributed under the MIT License. See LICENSE for more information.
