# 🐛 Known Issues & Future Development for Regula App Toolkit (v0.1.2)

This document tracks known issues, current limitations, and planned future features for the Regula App Toolkit.

---

## 🛑 Known Issues (Bugs)

* **Ttk Style Limitations:** Applying custom colors (like `tab_color` in the `Frame` function) to **Ttk widgets** (especially `Notebook`) often fails or has limited effect due to the operating system's theme overriding custom styles.
    * *Workaround:* Stick to default Ttk themes or use classic Tk widgets for custom coloring.
* **Root Window Configuration:** The `Window()` function might be unable to configure certain properties (like changing the title after the window has been initialized) without requiring a `Run()` restart, depending on the OS and Tkinter version.
* **Grid Expansion Initialization:** The internal grid configuration in `_handle_layout` may not always correctly handle subsequent widgets placed via grid after the initial widget, requiring manual `columnconfigure` and `rowconfigure` calls on the parent frame for complex layouts.
* **Configure Function**: The Configure function used tkinter deafaul arguments in version 0.1.1 but in the latest verison (0.1.2) it uses Regula arguments.
* **Window Function**: Do not expect to return anything. The window function can create and configure the Tk root (with Window().Size(width, height), Background(color) and Title(text)) but it does not return the root
  

---

## 🚧 Current Limitations (Design Choices)

These are not bugs, but limitations based on the goal of being a "simplified wrapper."

* **No Event Binding Abstraction:** The library does not provide simple wrappers for non-command events (e.g., `<Return>`, mouse clicks, window events). Users must rely on standard Tkinter's `.bind()` method for complex event handling.
* **No Image Support:** There are currently no helper functions for loading, managing, and displaying image files (GIF, PNG, etc.) in labels or buttons.
* **Simplified Layout Only:** Only basic `grid` and `pack` management is supported. Complex `place` geometry manager is omitted.
