import tkinter as tk
from tkinter import messagebox

class Doctor:
    def __init__(self, name, specialty, availability):
        self.name = name
        self.specialty = specialty
        self.availability = availability  # List of available time slots

class AppointmentSystem:
    def __init__(self):
        self.doctors = []
        self.appointments = {}

    def add_doctor(self, doctor):
        self.doctors.append(doctor)
        self.appointments[doctor.name] = []

    def book_appointment(self, doctor_name, time_slot):
        for doctor in self.doctors:
            if doctor.name == doctor_name:
                if time_slot in doctor.availability:
                    doctor.availability.remove(time_slot)
                    self.appointments[doctor.name].append(time_slot)
                    return f"Appointment booked with {doctor.name} at {time_slot}"
        return "Doctor or time slot not found or unavailable"

class AppointmentApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Doctor Appointment System")
        self.geometry("600x400")

        # Customize window
        self.configure(bg="#F8F8F8")  # Set background color

        # Background image for main window
        self.bg_image = tk.PhotoImage(file=r"C:\Users\anshu\Desktop\New folder\bg.image.png")
        self.bg_label = tk.Label(self, image=self.bg_image)
        self.bg_label.place(relwidth=1, relheight=1)

        self.appointment_system = AppointmentSystem()

        # Create doctors
        doctor1 = Doctor("Dr. Smith", "Cardiologist", ["9AM", "10AM", "11AM"])
        doctor2 = Doctor("Dr. Johnson", "Dermatologist", ["2PM", "3PM", "4PM"])
        self.appointment_system.add_doctor(doctor1)
        self.appointment_system.add_doctor(doctor2)

        # Widgets
        self.create_widgets()

    def create_widgets(self):
        # Title label
        title_label = tk.Label(self, text="Welcome to Doctor Appointment System", bg="#333", fg="white", font=("Arial", 18))
        title_label.pack(fill=tk.X)

        # Register Patient button
        self.register_patient_button = tk.Button(self, text="Register Patient", command=self.open_registration_window, bg="#4CAF50", fg="white", font=("Arial", 14))
        self.register_patient_button.pack(pady=10)

        # Book Appointment button
        self.book_appointment_button = tk.Button(self, text="Book Appointment", command=self.open_booking_window, bg="#008CBA", fg="white", font=("Arial", 14))
        self.book_appointment_button.pack(pady=10)

    def open_registration_window(self):
        registration_window = RegistrationWindow(self)

    def open_booking_window(self):
        doctor_selection_window = DoctorSelectionWindow(self.appointment_system)

class DoctorSelectionWindow(tk.Toplevel):
    def __init__(self, appointment_system):
        super().__init__()
        self.title("Select Doctor")
        self.geometry("400x200")

        # Background image for doctor selection window
        self.bg_image = tk.PhotoImage(file=r"C:\Users\anshu\Desktop\New folder\bg.image.png")
        self.bg_label = tk.Label(self, image=self.bg_image)
        self.bg_label.place(relwidth=1, relheight=1)

        self.appointment_system = appointment_system

        self.doctor_label = tk.Label(self, text="Select Doctor:")
        self.doctor_label.pack()

        self.doctor_name_var = tk.StringVar(self)
        self.doctor_name_var.set("Dr. Smith")

        self.doctor_option_menu = tk.OptionMenu(self, self.doctor_name_var, *[doctor.name for doctor in self.appointment_system.doctors])
        self.doctor_option_menu.pack()

        self.time_label = tk.Label(self, text="Select Time Slot:")
        self.time_label.pack()

        self.time_slot_var = tk.StringVar(self)
        self.time_slot_var.set("9AM")

        self.time_option_menu = tk.OptionMenu(self, self.time_slot_var, [])
        self.time_option_menu.pack()

        self.book_button = tk.Button(self, text="Book Appointment", command=self.book, bg="#008CBA", fg="white", font=("Arial", 12))
        self.book_button.pack(pady=10)

    def book(self):
        doctor_name = self.doctor_name_var.get()
        time_slot = self.time_slot_var.get()
        result = self.appointment_system.book_appointment(doctor_name, time_slot)
        messagebox.showinfo("Appointment Booking", result)

class RegistrationWindow(tk.Toplevel):
    def __init__(self, parent):
        super().__init__(parent)
        self.title("Patient Registration")
        self.geometry("400x200")

        # Background image for registration window
        self.bg_image = tk.PhotoImage(file=r"C:\Users\anshu\Desktop\New folder\bg.image.png")
        self.bg_label = tk.Label(self, image=self.bg_image)
        self.bg_label.place(relwidth=1, relheight=1)

        self.patient_name_label = tk.Label(self, text="Enter Patient Name:")
        self.patient_name_label.pack()

        self.patient_name_entry = tk.Entry(self)
        self.patient_name_entry.pack()

        self.register_button = tk.Button(self, text="Register", command=self.register_patient, bg="#4CAF50", fg="white", font=("Arial", 12))
        self.register_button.pack(pady=10)

    def register_patient(self):
        patient_name = self.patient_name_entry.get()
        messagebox.showinfo("Registration Successful", f"Patient {patient_name} registered!")
        self.destroy()

if __name__ == "__main__":
    app = AppointmentApp()
    app.mainloop()
