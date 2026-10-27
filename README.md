# ITE_260_P3
students = []

class Student:
    def stud_info(stud, student_id, name, section):
        stud.id = student_id
        stud.name = name
        stud.section = section
        stud.present = 0
        stud.absent = 0


def show_message(message):
    print(message)


def display_student(student):
    print("Student ID:", student.id)
    print("Name:", student.name)
    print("Section:", student.section)


def add_student():
    print()
    print("~~~~ REGISTER STUDENT ~~~~")

    student_id = input("Enter Student ID: ").strip()

    if student_id == "":
        show_message("ERROR: Student ID cannot be empty.")
        return

    for student in students:
        if student.id == student_id:
            show_message("ERROR: Student ID already exists.")
            return

    name = input("Enter Student Name: ").strip()
    section = input("Enter Section: ").strip()

    if name == "" or section == "":
        show_message("ERROR: Name and section are required.")
        return

    student = Student()
    student.stud_info(student_id, name, section)
    students.append(student)

    show_message("SUCCESS: Student registered successfully!")


def mark_attendance(student, choice):
    if choice == "1":
        student.present += 1
        status = "Present"

    elif choice == "2":
        student.absent += 1
        status = "Absent"

    else:
        show_message("ERROR: Invalid attendance status.")
        return

    show_message(
        "SUCCESS: " + student.name + " marked " + status + "."
    )


def record_attendance():
    print()
    print("~~~~ RECORD ATTENDANCE ~~~~")

    if not students:
        show_message("ERROR: No students registered yet.")
        return

    for i, current_student in enumerate(students, start=1):
        print(
            str(i) + ". " + current_student.id + " - "
            + current_student.name + " ("
            + current_student.section + ")"
        )

    try:
        number = int(input("Select student number: "))

        student = None

        for i, current_student in enumerate(students, start=1):
            if i == number:
                student = current_student
                break

        if student is None:
            show_message("ERROR: Student not found.")
            return

        print()
        display_student(student)
        print()
        print("1. Present")
        print("2. Absent")

        choice = input("Select attendance status: ")
        mark_attendance(student, choice)

    except ValueError:
        show_message("ERROR: Please enter a valid number.")


def view_attendance():
    print()
    print("~~~~ ATTENDANCE SUMMARY ~~~~")

    if not students:
        show_message("No student records available.")
        return

    for student in students:
        total = student.present + student.absent

        print("----------------------------------------")
        display_student(student)
        print("Present:", student.present)
        print("Absent:", student.absent)
        print("Total Meetings:", total)

        if total > 0:
            rate = (student.present / total) * 100
            print("Attendance Rate:", str(round(rate, 2)) + "%")
        else:
            print("Attendance Rate: No records yet")


def view_absences():
    print()
    print("~~~~ STUDENT ABSENCES ~~~~")

    if not students:
        show_message("No student records available.")
        return

    found = False

    for student in students:
        if student.absent > 0:
            display_student(student)
            print("Absences:", student.absent)
            print("----------------------------------------")
            found = True

    if not found:
        show_message("No recorded absences.")


def main():
    while True:
        print()
        print("=" * 40)
        print("       ATTENDANCE TRACKER")
        print("=" * 40)
        print("1. Register Student")
        print("2. Record Attendance")
        print("3. View Attendance Summary")
        print("4. View Student Absences")
        print("5. Exit")

        choice = input("Select option (1-5): ")

        if choice == "1":
            add_student()

        elif choice == "2":
            record_attendance()

        elif choice == "3":
            view_attendance()

        elif choice == "4":
            view_absences()

        elif choice == "5":
            show_message(
                "Thank you for using Attendance Tracker!"
            )
            break

        else:
            show_message("ERROR: Invalid option. Choose 1-5.")


if __name__ == "__main__":
    main()