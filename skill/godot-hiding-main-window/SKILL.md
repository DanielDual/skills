---
name: godot-hiding-main-window
description: Read this skill if you are required to hide the main window of a godot project.
---

# Godot: Hiding the Main Window

## Problem

Godot prohibits developers from hiding the main window. It hasn't provided an alternate solution for now.

## Workaround

For Windows, the code below is usable to hide the main window:

```gdscript
win_hWnd = DisplayServer.window_get_native_handle(DisplayServer.WINDOW_HANDLE)

# 0 hide 1 show
arguments = ["\"Add-Type -MemberDefinition '[DllImport(\\\"user32.dll\\\")] public static extern bool ShowWindow(IntPtr handle, int state);' -Name Window -Namespace Win32;[Win32.Window]::ShowWindow("+str(win_hWnd)+",0)\""]

Thread.new().start(Callable(self, "_show_window_execution_callback").bindv(arguments))

func _show_window_execution_callback(arguments:String) -> void:
	var output = []
	var exit_code = OS.execute("powershell.exe", ["-Command", arguments], output, false, false)
	print("Exit Code:", exit_code)
	print("Output:", output)
```

It is unsure whether macOS and Linux support this way, so it is encouraged to write different solutions for different platforms.

## Reference

[Allow hiding the main project window temporarily · Issue #9032 · godotengine/godot-proposals](https://github.com/godotengine/godot-proposals/issues/9032)