import tkinter as tk
from tkinter import messagebox


class TicketBookingApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Ticket Booking System")
        self.root.geometry("500x400")

        # Initialize tickets
        self.total_tickets = 100
        self.booked_tickets = {}

        # Title
        self.title_label = tk.Label(root, text="Ticket Booking System", font=("Arial", 16, "bold"))
        self.title_label.pack(pady=10)

        # Ticket availability
        self.tickets_label = tk.Label(root, text=f"Available Tickets: {self.total_tickets}", font=("Arial", 12))
        self.tickets_label.pack(pady=5)

        # Booking section
        self.name_label = tk.Label(root, text="Name:", font=("Arial", 12))
        self.name_label.pack(pady=5)
        self.name_entry = tk.Entry(root, font=("Arial", 12))
        self.name_entry.pack(pady=5)

        self.tickets_entry_label = tk.Label(root, text="Number of Tickets:", font=("Arial", 12))
        self.tickets_entry_label.pack(pady=5)
        self.tickets_entry = tk.Entry(root, font=("Arial", 12))
        self.tickets_entry.pack(pady=5)

        self.book_button = tk.Button(root, text="Book Tickets", font=("Arial", 12), command=self.book_tickets)
        self.book_button.pack(pady=10)

        # Cancel booking
        self.cancel_label = tk.Label(root, text="Cancel Booking (Name):", font=("Arial", 12))
        self.cancel_label.pack(pady=5)
        self.cancel_entry = tk.Entry(root, font=("Arial", 12))
        self.cancel_entry.pack(pady=5)

        self.cancel_button = tk.Button(root, text="Cancel Booking", font=("Arial", 12), command=self.cancel_booking)
        self.cancel_button.pack(pady=10)

        # Booking details
        self.details_button = tk.Button(root, text="View Booking Details", font=("Arial", 12), command=self.view_details)
        self.details_button.pack(pady=10)

        # Exit button
        self.exit_button = tk.Button(root, text="Exit", font=("Arial", 12), command=root.quit)
        self.exit_button.pack(pady=10)

    def update_available_tickets(self):
        """Update the label showing available tickets."""
        self.tickets_label.config(text=f"Available Tickets: {self.total_tickets}")

    def book_tickets(self):
        """Book tickets for a user."""
        name = self.name_entry.get().strip()
        try:
            num_tickets = int(self.tickets_entry.get().strip())
            if not name:
                messagebox.showerror("Error", "Name cannot be empty.")
                return
            if num_tickets <= 0:
                messagebox.showerror("Error", "Number of tickets must be positive.")
                return
            if num_tickets > self.total_tickets:
                messagebox.showerror("Error", f"Only {self.total_tickets} tickets are available.")
                return

            # Process booking
            self.total_tickets -= num_tickets
            if name in self.booked_tickets:
                self.booked_tickets[name] += num_tickets
            else:
                self.booked_tickets[name] = num_tickets
            self.update_available_tickets()
            messagebox.showinfo("Success", f"Successfully booked {num_tickets} ticket(s) for {name}.")
        except ValueError:
            messagebox.showerror("Error", "Please enter a valid number for tickets.")

    def cancel_booking(self):
        """Cancel a booking for a user."""
        name = self.cancel_entry.get().strip()
        if name in self.booked_tickets:
            num_tickets = self.booked_tickets[name]
            self.total_tickets += num_tickets
            del self.booked_tickets[name]
            self.update_available_tickets()
            messagebox.showinfo("Success", f"Successfully canceled booking for {name}.")
        else:
            messagebox.showerror("Error", f"No booking found under the name '{name}'.")

    def view_details(self):
        """View all booking details."""
        if not self.booked_tickets:
            messagebox.showinfo("Booking Details", "No bookings have been made yet.")
        else:
            details = "\n".join(f"{name}: {tickets} ticket(s)" for name, tickets in self.booked_tickets.items())
            messagebox.showinfo("Booking Details", details)


# Create the application
if __name__ == "__main__":
    root = tk.Tk()
    app = TicketBookingApp(root)
    root.mainloop()
