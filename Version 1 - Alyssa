  import time
  import json
  
  class StudentEvent_Reservation:
      def __init__(self):
          self.available_seats = {}
          self.reserved_student_numbers = set()
          self.reservation_records = {}
          self.load_reservation_records()
  
      def add_seat(self, row, col):
          if (row, col) not in self.available_seats:
              self.available_seats[(row, col)] = True
              print(f"\nSeat ({row}, {col}) has been added to the available seats.")
              self.save_reservation_records()
  
      def add_seats_auto(self, rows, cols):
          for row in range(1, rows + 1):
              for col in range(1, cols + 1):
                  self.add_seat(row, col)
  
      def reserve_seat(self, name, student_number):
          for (row, col), available in self.available_seats.items():
              if available:
                  self.available_seats[(row, col)] = False
                  self.reserved_student_numbers.add(student_number)
                  self.reservation_records[(row, col)] = (name, student_number)
                  self.save_reservation_records()
                  return (row, col), name, student_number
          return None
  
      def display_available_seats(self):
          max_row = max(row for row, _ in self.available_seats.keys()) if self.available_seats else 0
          max_col = max(col for _, col in self.available_seats.keys()) if self.available_seats else 0
          for row in range(1, max_row + 1):
              for col in range(1, max_col + 1):
                  if self.available_seats.get((row, col), False):
                      print("O", end=" ")
                  else:
                      print("X", end=" ")
              print()
  
          print("\nAvailable seats:")
          for (row, col), available in self.available_seats.items():
              if available:
                  print(f"Seat ({row}, {col})")
  
      def display_records(self):
          return self.reservation_records
  
      def save_reservation_records(self):
          records = {str(key): value for key, value in self.reservation_records.items()}
          with open("reservation_records.json", "w") as file:
              json.dump(records, file)
  
      def load_reservation_records(self):
          try:
              with open("reservation_records.json", "r") as file:
                  records = json.load(file)
                  self.reservation_records = {tuple(map(int, key.strip('()').split(', '))): value for key, value in records.items()}
                  self.available_seats = {key: False for key in self.reservation_records}
                  self.reserved_student_numbers = {value[1] for value in self.reservation_records.values()}
          except FileNotFoundError:
              pass
  
  reservation_system = StudentEvent_Reservation()
  
  while True:
      print("\nWelcome to the Student Event Ticket Reservation System!")
      print("1. Add a seat")
      print("2. Reserve a seat")
      print("3. Display available seats")
      print("4. Exit")
  
      try:
          choice = int(input("Enter your choice: "))
      except ValueError:
          print("Invalid input. Please enter a number between 1 and 4.")
          continue
  
      if choice == 1:
          while True:
              print("Press 'a' if you want to add seats automatically")
              print("Press 'x' if you want to return to the menu")
              subchoice = input("Enter choice: ").lower()
              if subchoice == 'a':
                  while True:
                      try:
                          rows = int(input("Enter the number of rows you want to add automatically: "))
                          cols = int(input("Enter the number of columns you want to add automatically: "))
                          if not (1 <= rows <= 100) or not (1 <= cols <= 100):
                              print("Invalid input. Rows and columns should be between 1 and 100.")
                              continue
                          break
                      except ValueError:
                          print("Invalid input. Please enter valid numbers.")
                          continue
  
                  reservation_system.add_seats_auto(rows, cols)
  
                  print(f"\n{rows * cols} seats have been added automatically.")
                  break
              elif subchoice == 'x':
                  break
              else:
                  print("Invalid choice. Please enter 'a', or 'x'.")
  
      elif choice == 2:
          if not any(reservation_system.available_seats.values()):
              print("No available seats yet.")
              continue
  
          print("\nReserving a seat:")
          name = input("Enter your name: ")
          section = input("Enter your section: ")
  
          while True:
              student_number = input("Enter your student number: ")
              if len(student_number) != 11 or not student_number.isdigit():
                  print("Invalid student number. Please enter an 11-digit number.")
              elif student_number in reservation_system.reserved_student_numbers:
                  print("This student number has already been used for a reservation.")
              else:
                  break
  
          reserved_seat_info = reservation_system.reserve_seat(name, student_number)
  
          if reserved_seat_info is not None:
              seat_number, name, student_number = reserved_seat_info
              time.sleep(2)
              print(f"\nSeat ({seat_number[0]}, {seat_number[1]}) reserved successfully for {name} from section {section} with student number {student_number}.")
          else:
              print("No available seats or the student number has already been used.")
  
      elif choice == 3:
          available_seats = reservation_system.available_seats
          if available_seats:
              print("\nAvailable seats:")
              reservation_system.display_available_seats()
          else:
              print("\nNo more seats available.")
  
      elif choice == 4:
          print("Thank you for using the system. Goodbye!")
          break
  
      else:
          print("Invalid choice. Please choose a valid option.")
