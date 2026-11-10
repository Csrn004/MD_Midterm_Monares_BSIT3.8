import 'dart:io';

mixin TimestampMixin {
  DateTime createdAt = DateTime.now();

  String getFormattedDate() {
    return "${createdAt.year}-${createdAt.month.toString().padLeft(2, '0')}-${createdAt.day.toString().padLeft(2, '0')} "
        "${createdAt.hour.toString().padLeft(2, '0')}:${createdAt.minute.toString().padLeft(2, '0')}";
  }
}


abstract class CenterRecord with TimestampMixin {
  String _id;
  String _name;

  CenterRecord(this._id, this._name);

  String get id => _id;
  String get name => _name;

  set name(String newName) {
    if (newName.trim().isNotEmpty) {
      _name = newName;
    }
  }

  void displayDetails();
}

class PatientRecord extends CenterRecord {
  int _age;
  String _ailment;

  PatientRecord(String id, String name, this._age, this._ailment) : super(id, name);

  int get age => _age;
  String get ailment => _ailment;

  @override
  void displayDetails() {
    print("----------------------------------------");
    print("[Patient Record]");
    print("ID         : $id");
    print("Name       : $name");
    print("Age        : $_age");
    print("Ailment    : $_ailment");
    print("Registered : ${getFormattedDate()}");
    print("----------------------------------------");
  }
}

class StaffRecord extends CenterRecord {
  String _role;
  double _salary;

  StaffRecord(String id, String name, this._role, this._salary) : super(id, name);

  String get role => _role;
  double get salary => _salary;

  @override
  void displayDetails() {
    print("----------------------------------------");
    print("[Staff Record]");
    print("ID         : $id");
    print("Name       : $name");
    print("Role       : $_role");
    print("Salary     : PHP ${_salary.toStringAsFixed(2)}");
    print("Registered : ${getFormattedDate()}");
    print("----------------------------------------");
  }
}

void main() {
  List<CenterRecord> recordsList = [];

  Map<String, CenterRecord> recordsMap = {};

  bool running = true;

  print("========================================");
  print("   CENTER RECORD MANAGEMENT SYSTEM      ");
  print("========================================");

  while (running) {
    print("\n--- MENU ---");
    print("[1] Add Patient Record");
    print("[2] Add Staff Record");
    print("[3] View All Records");
    print("[4] Exit");
    stdout.write("Enter your choice (1-5): ");

    try {
      String? input = stdin.readLineSync();
      if (input == null) continue;
      int choice = int.parse(input);

      switch (choice) {
        case 1:
          stdout.write("Enter Patient ID (e.g., P001): ");
          String pId = stdin.readLineSync()!.trim();
          if (pId.isEmpty || recordsMap.containsKey(pId)) {
            print("Error: Invalid ID or ID already exists.");
            break;
          }

          stdout.write("Enter Patient Name: ");
          String pName = stdin.readLineSync()!.trim();

          stdout.write("Enter Patient Age: ");
          int pAge = int.parse(stdin.readLineSync()!.trim());

          stdout.write("Enter Ailment/Condition: ");
          String pAilment = stdin.readLineSync()!.trim();

          PatientRecord patient = PatientRecord(pId, pName, pAge, pAilment);
          recordsList.add(patient);
          recordsMap[pId] = patient;
          print("Success: Patient record added successfully!");
          break;

        case 2:
          stdout.write("Enter Staff ID (e.g., S001): ");
          String sId = stdin.readLineSync()!.trim();
          if (sId.isEmpty || recordsMap.containsKey(sId)) {
            print("Error: Invalid ID or ID already exists.");
            break;
          }

          stdout.write("Enter Staff Name: ");
          String sName = stdin.readLineSync()!.trim();

          stdout.write("Enter Job Role: ");
          String sRole = stdin.readLineSync()!.trim();

          stdout.write("Enter Monthly Salary: ");
          double sSalary = double.parse(stdin.readLineSync()!.trim());

          StaffRecord staff = StaffRecord(sId, sName, sRole, sSalary);
          recordsList.add(staff);
          recordsMap[sId] = staff;
          print("Success: Staff record added successfully!");
          break;

        case 3:
          if (recordsList.isEmpty) {
            print("\nNo records found in the system.");
          } else {
            print("\n=== SYSTEM RECORDS (${recordsList.length}) ===");
            for (var record in recordsList) {
              record.displayDetails();
            }
          }
          break;
          
        case 4:
          running = false;
          print("\nExiting program. Goodbye!");
          break;

        default:
          print("\nError: Please enter a valid number between 1 and 5.");
      }
    } catch (e) {
      print("\n[Input Exception Caught]: Invalid input format. Please try again. (${e.toString()})");
    }
  }
}
