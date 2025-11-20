# 🐛 Known Issues & Future Development for Regula App Toolkit (v0.1.1)

This document tracks known issues, current limitations, and planned future features for the Regula App Toolkit.

---

## 🛑 Known Issues (Bugs)

* **Ttk Style Limitations:** Applying custom colors (like `tab_color` in the `Frame` function) to **Ttk widgets** (especially `Notebook`) often fails or has limited effect due to the operating system's theme overriding custom styles.
    * *Workaround:* Stick to default Ttk themes or use classic Tk widgets for custom coloring.
* **Root Window Configuration:** The `Window()` function might be unable to configure certain properties (like changing the title after the window has been initialized) without requiring a `Run()` restart, depending on the OS and Tkinter version.
* **Grid Expansion Initialization:** The internal grid configuration in `_handle_layout` may not always correctly handle subsequent widgets placed via grid after the initial widget, requiring manual `columnconfigure` and `rowconfigure` calls on the parent frame for complex layouts.

---

## 🚧 Current Limitations (Design Choices)

These are not bugs, but limitations based on the goal of being a "simplified wrapper."

* **No Event Binding Abstraction:** The library does not provide simple wrappers for non-command events (e.g., `<Return>`, mouse clicks, window events). Users must rely on standard Tkinter's `.bind()` method for complex event handling.
* **Limited Custom Widget Support:** Widgets like **Checkbutton** and **Radiobutton** are currently not included. Users must instantiate them directly from `tkinter`.
* **No Image Support:** There are currently no helper functions for loading, managing, and displaying image files (GIF, PNG, etc.) in labels or buttons.
* **Simplified Layout Only:** Only basic `grid` and `pack` management is supported. Complex `place` geometry manager is omitted.