# im-suppost-to-do-ths 

code below

import time
import threading
from pynput.mouse import Button, Controller
from pynput.keyboard import Listener, KeyCode

# --- Configuration ---
delay = 0.01  # Delay between clicks in seconds
button = Button.left  # Mouse button to click
start_stop_key = KeyCode(char='s')  # Key to start/stop clicking
exit_key = KeyCode(char='e')  # Key to exit the program

# --- Auto Clicker Class (using threading) ---
class ClickMouse(threading.Thread):
    def __init__(self, delay, button):
        super(ClickMouse, self).__init__()
        self.delay = delay
        self.button = button
        self.running = False  # Flag to indicate if clicking is active
        self.program_running = True  # Flag to keep the program running

    def start_clicking(self):
        self.running = True  # Start clicking

    def stop_clicking(self):
        self.running = False  # Stop clicking

    def exit(self):
        self.stop_clicking()  # Stop clicking before exiting
        self.program_running = False  # Exit the program

    def run(self):
        while self.program_running:  # Continue while the program is running
            while self.running:  # Click while the clicking is active
                mouse.click(self.button)  # Perform the mouse click
                time.sleep(self.delay)  # Wait for the specified delay
            time.sleep(0.1)  # Sleep briefly when not clicking to avoid high CPU usage

# --- Mouse Controller and Clicker Thread ---
mouse = Controller()  # Create a mouse controller instance
click_thread = ClickMouse(delay, button)  # Create the clicker thread
click_thread.start()  # Start the clicker thread

# --- Keyboard Listener ---
def on_press(key):
    if key == start_stop_key:
        if click_thread.running:
            click_thread.stop_clicking()  # Toggle clicking off
            print("[INFO] Clicker Stopped.")
        else:
            click_thread.start_clicking()  # Toggle clicking on
            print("[INFO] Clicker Started.")
    elif key == exit_key:
        click_thread.exit()  # Exit the program
        print("[INFO] Exiting.")
        return False  # Stop the keyboard listener

with Listener(on_press=on_press) as listener:
    listener.join()  # Start listening for keyboard events


