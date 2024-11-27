import tkinter as tk

class TimerApp:
    def __init__(self, master):
        self.master = master
        master.title("Timer App")
        master.geometry("300x200")
        master.configure(bg="black")

        self.time_label = tk.Label(master, text="00:00:00", font=("Arial", 36), bg="black", fg="white")
        self.time_label.pack(pady=20)

        self.time_entry = tk.Entry(master, font=("Arial", 12), width=10, bd=2, bg="#0a2b4e", fg="white")
        self.time_entry.pack(pady=10)

        self.start_button = tk.Button(master, text="Start", command=self.start_timer, font=("Arial", 12), bg="#4caf50", fg="white", padx=10, bd=4, relief=tk.RAISED)
        self.start_button.pack(side=tk.LEFT, padx=10)

        self.stop_button = tk.Button(master, text="Stop", command=self.stop_timer, font=("Arial", 12), bg="#f44336", fg="white", padx=10, bd=4, relief=tk.RAISED)
        self.stop_button.pack(side=tk.LEFT, padx=10)

        self.reset_button = tk.Button(master, text="Reset", command=self.reset_timer, font=("Arial", 12), bg="#2196f3", fg="white", padx=10, bd=4, relief=tk.RAISED)
        self.reset_button.pack(side=tk.LEFT, padx=10)

        self.is_running = False
        self.seconds = 0
        self.timer_id = None

    def start_timer(self):
        if not self.is_running:
            self.is_running = True
            self.start_button.config(state=tk.DISABLED)
            self.stop_button.config(state=tk.NORMAL)
            self.reset_button.config(state=tk.DISABLED)
            self.seconds = self.parse_time_input()
            self.update_timer()

    def stop_timer(self):
        if self.is_running:
            self.is_running = False
            self.start_button.config(state=tk.NORMAL)
            self.stop_button.config(state=tk.DISABLED)
            self.reset_button.config(state=tk.NORMAL)
            if self.timer_id:
                self.master.after_cancel(self.timer_id)

    def reset_timer(self):
        self.stop_timer()
        self.seconds = 0
        self.time_entry.delete(0, tk.END)
        self.update_time()

    def update_timer(self):
        self.seconds -= 1
        self.update_time()
        if self.seconds > 0:
            self.timer_id = self.master.after(1000, self.update_timer)
        else:
            self.stop_timer()

    def update_time(self):
        hours = self.seconds // 3600
        minutes = (self.seconds % 3600) // 60
        seconds = self.seconds % 60
        time_string = "{:02d}:{:02d}:{:02d}".format(hours, minutes, seconds)
        self.time_label.config(text=time_string)

    def parse_time_input(self):
        try:
            hours, minutes, seconds = map(int, self.time_entry.get().split(":"))
            return hours * 3600 + minutes * 60 + seconds
        except ValueError:
            return 0

if __name__ == "__main__":
    root = tk.Tk()
    app = TimerApp(root)
    root.mainloop()
