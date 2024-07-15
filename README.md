# MediVault
school project.

Progress:
-When searching for patient, show billing info and prescription information
-Fix UI when adding a patient, show all info on prompts and clear function
-When adding another new patient, clear terminal function
-When the prompt patient has been discharged shows, if you select no, it will then give u a prompt to enter prescription info, but it doesnt show if u get the billing/discharged prompt
-When no patient is found in the search function, create a delay animation to automatically return to the main page.
-When No More patients to add prompt is shown, clear terminal.
-When searching for a patient, the patient searched for doesn't show prescription info. Why?
-When Billing Information and Prescription Information is set to no, make sure to write a No Entry in the data folder
-Create Logout Animation
-Add better functions.

Bugs:
-Resetting loop when there is a rare chance that you put in wrong input
-Password bug when u leave the terminal in the middle of registering, leaving an unmatched line so it’ll crash the whole program leading to having to manually reformatting the cred.txt file
-Displaying patients basic info bug.
-Idk i think there’ll be more bugs than listed so i apologize guys :3

Directories needed:
data/profiles/username
data/profiles/username/patients
data/profiles/username/billing
data/profiles/username/prescriptions
data/cred

Code:
#include <iostream>
#include <fstream>
#include <string>
#include <direct.h>
#include <windows.h>
#include <vector>

using namespace std;

void delay(int milliseconds);
void loadingFunc();
bool validateCredentials(const string& username, const string& password);
void registerUser();
void loginUser();
void homePage();
void loginSession();
void createPatientDatabase();
void addPatient();
void addedPatient();
void addAnotherPatientPrompt();
void viewPatients();
void searchPatient();
void beenDischarged();
void prescriptionInformation();

string userLoggedIn;
string pName, pAge, pGender, pCity;

void delay(int milliseconds) {
    Sleep(milliseconds);
}

void loadingFunc() {
    delay(950);
    cout << "Loading";
    delay(500);
    cout << ". ";
    delay(500);
    cout << ". ";
    delay(500);
    cout << "." << endl;
    delay(1200);
    system("cls");
}

bool validateCredentials(const string& username, const string& password) {
    ifstream loginfile("data/cred.txt");
    if (!loginfile.is_open()) {
        cerr << "Error checking credentials." << endl;
        return false;
    }

    string fileUsername, filePassword;
    while (loginfile >> fileUsername >> filePassword) {
        if (username == fileUsername && password == filePassword) {
            return true;
        }
    }
    return false;
}

void registerUser() {
    ofstream loginfile("data/cred.txt", ios::app);
    if (!loginfile.is_open()) {
        cerr << "Error opening file." << endl;
        return;
    }

    string username, password;
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << endl << "             Register" << endl;
    cout << "___________________________________" << endl;
    cout << "| Enter a username:  ";
    cin >> username;
    cout << "| Enter password:    ";
    cin >> password;
    cout << "-----------------------------------" << endl;

    string userDirectory = "data/profiles/" + username;
    if (_mkdir(userDirectory.c_str()) != 0) {
        cerr << "Error creating profile." << endl;
        return;
    }

    loginfile << username << " " << password << endl;
    cout << "User registered Successfully!";
    delay(950);
    cout << " Heading to login page";
    delay(500);
    cout << ". ";
    delay(500);
    cout << ". ";
    delay(500);
    cout << "." << endl;
    delay(1200);
    system("cls");
    loginUser();
}

void loginUser() {
    string username, password;
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << endl << "              Login" << endl;
    cout << "___________________________________" << endl;
    cout << "| Enter Username:  ";
    cin >> username;
    cout << "| Enter Password:  ";
    cin >> password;
    cout << "-----------------------------------" << endl;

    if (validateCredentials(username, password)) {
        userLoggedIn = username;
        createPatientDatabase();
        cout << "Access Granted!";
        delay(950);
        cout << " Logging in";
        delay(500);
        cout << ". ";
        delay(500);
        cout << ". ";
        delay(500);
        cout << "." << endl;
        delay(1200);
        system("cls");
        loginSession();
    }
    else {
        cout << "Access denied!";
        delay(950);
        cout << " Heading back to home page";
        delay(500);
        cout << ". ";
        delay(500);
        cout << ". ";
        delay(500);
        cout << "." << endl;
        delay(1200);
        system("cls");
        homePage();
    }
}

void homePage() {
    int choice;
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << endl << "        Enter your choice:" << endl;
    cout << "___________________________________";
    cout << endl << "| 1. Login                        |" << endl;
    cout << "| 2. Register                     |" << endl;
    cout << "-----------------------------------" << endl;
    cout << endl << "Select your choice: ";
    cin >> choice;

    if (choice == 1) {
        system("cls");
        loginUser();
    }
    else if (choice == 2) {
        system("cls");
        registerUser();
    }
    else {
        delay(950);
        cout << endl << "Invalid choice!";
        cout << " Resetting";
        delay(500);
        cout << ". ";
        delay(500);
        cout << ". ";
        delay(500);
        cout << "." << endl;
        delay(1200);
        system("cls");
        homePage();
    }
}

void createPatientDatabase() {
    ofstream patientsDatabase("data/profiles/" + userLoggedIn + "/patients.txt");
    if (!patientsDatabase) {
        ofstream patientsDatabase("data/profiles/" + userLoggedIn + "/patients.txt");
    }
}

void addPatient() {
    string filePath = "data/profiles/" + userLoggedIn + "/patients.txt";
    string userDirectory = "data/profiles/" + userLoggedIn;
    if (_mkdir(userDirectory.c_str()) != 0 && errno != EEXIST) {
        cerr << "Error creating directory." << endl;
        return;
    }

    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  Basic Information                                  |" << endl;
    cout << "-------------------------------------------------------" << endl;
    delay(1200);

    cout << "Patient's Name: ";
    cin.ignore();
    getline(cin, pName);

    cout << "Patient's Age: ";
    cin >> pAge;
    cin.ignore();

    cout << "Patient's Gender: ";
    getline(cin, pGender);

    cout << "Patient's City: ";
    getline(cin, pCity);

    ofstream patientsDatabase(filePath, ios::app);
    if (!patientsDatabase.is_open()) {
        cerr << "Error opening patients.txt for writing." << endl;
        return;
    }

    patientsDatabase << pName << ", " << pAge << ", " << pGender << ", " << pCity << endl;
    patientsDatabase.close();

    addedPatient();
}

void addedPatient() {
    int input = 0;
    system("cls");
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  Basic Information Added!                           |" << endl;
    cout << "-------------------------------------------------------" << endl;
    loadingFunc();
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  Patient Details                                    |" << endl;
    cout << "-------------------------------------------------------" << endl;
    cout << "| Name: " << pName << endl;
    cout << "| Age: " << pAge << endl;
    cout << "| Gender: " << pGender << endl;
    cout << "| City: " << pCity << endl;
    cout << "-------------------------------------------------------" << endl;
    cout << "|  Has this patient been discharged?                  |" << endl;
    cout << "|    [1] Yes         [0] No.                          |" << endl;
    cout << "-------------------------------------------------------" << endl;
    cout << "  Select a choice: ";
    cin >> input;
    if (input == 1) {
        beenDischarged();
    }
    else {
        ofstream prescriptionFile("data/profiles/" + userLoggedIn + "/prescriptions.txt", ios::app);
        if (!prescriptionFile.is_open()) {
            cerr << "Error opening prescriptions file." << endl;
            return;
        }

        ofstream billingFile("data/profiles/" + userLoggedIn + "/billing.txt", ios::app);
        if (!billingFile.is_open()) {
            cerr << "Error opening billing file." << endl;
            return;
        }
        billingFile << pName << ": No Entry." << endl;
        billingFile.close();
        input = 0;
        system("cls");
        cout << "-=-     MediVault Solutions     -=-" << endl << endl;
        cout << "_______________________________________________________" << endl;
        cout << "|  Add Prescription Information?                      |" << endl;
        cout << "|    [1] Yes         [0] No.                          |" << endl;
        cout << "-------------------------------------------------------" << endl << endl;
        cout << "  Select a choice: ";
        cin >> input;
        if (input == 1) {
            prescriptionInformation();
        }
        else {
            prescriptionFile << pName << ": " << "No Entry." << endl;
            prescriptionFile.close();
            addAnotherPatientPrompt();
        }
    }
}

void prescriptionInformation() {
    system("cls");
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "| Prescription Information                            |" << endl;
    cout << "-------------------------------------------------------" << endl;

    string prescriptionDetails;
    cout << "Enter prescription details: ";
    cin.ignore();
    getline(cin, prescriptionDetails);

    ofstream prescriptionFile("data/profiles/" + userLoggedIn + "/prescriptions.txt", ios::app);
    if (!prescriptionFile.is_open()) {
        cerr << "Error opening prescriptions file." << endl;
        return;
    }
    prescriptionFile << pName << ": " << prescriptionDetails << endl;
    prescriptionFile.close();

    cout << "Prescription information added." << endl;
    delay(1500);
    addAnotherPatientPrompt();
}

void beenDischarged() {
    int input;
    system("cls");
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "| Billing Information                                 |" << endl;
    cout << "-------------------------------------------------------" << endl;

    double totalBill = 0.0;
    cout << "Enter total bill amount: $";
    cin >> totalBill;

    ofstream prescriptionFile("data/profiles/" + userLoggedIn + "/prescriptions.txt", ios::app);
    if (!prescriptionFile.is_open()) {
        cerr << "Error opening prescriptions file." << endl;
        return;
    }

    ofstream billingFile("data/profiles/" + userLoggedIn + "/billing.txt", ios::app);
    if (!billingFile.is_open()) {
        cerr << "Error opening billing file." << endl;
        return;
    }
    billingFile << pName << ": $" << totalBill << endl;
    billingFile.close();

    cout << "Billing information processed." << endl;
    delay(1500);
    system("cls");
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  Add Prescription Information?                      |" << endl;
    cout << "|    [1] Yes         [0] No.                          |" << endl;
    cout << "-------------------------------------------------------" << endl << endl;
    cout << "  Select a choice: ";
    cin >> input;
    if (input == 1) {
        prescriptionInformation();
    }
    else {
        prescriptionFile << pName << ": " << "No Entry." << endl;
        prescriptionFile.close();
        addAnotherPatientPrompt();
    }
}

void addAnotherPatientPrompt() {
    system("cls");
    int input = 0;
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|      Add Another Patient?                           |" << endl;
    cout << "|    [1] Yes         [0] No.                          |" << endl;
    cout << "-------------------------------------------------------" << endl;
    cout << "  Select a choice: ";
    cin >> input;

    if (input == 1) {
        system("cls");
        addPatient();
    }
    else {
        input = 0;
        system("cls");
        cout << "-=-     MediVault Solutions     -=-" << endl << endl;
        cout << "_______________________________________________________" << endl;
        cout << "|      No more patients to add.                       |" << endl;
        cout << "-------------------------------------------------------" << endl;
        delay(1500);
        system("cls");
        loginSession();
    }
}

void loginSession() {
    int choice;
    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "        Enter your choice:" << endl;
    cout << "___________________________________";
    cout << endl << "| 1. Add Patient                  |" << endl;
    cout << "| 2. View Patients                |" << endl;
    cout << "| 3. Search Patient               |" << endl;
    cout << "| 4. Logout                       |" << endl;
    cout << "-----------------------------------" << endl;
    cout << endl << "Select your choice: ";
    cin >> choice;

    switch (choice) {
    case 1:
        system("cls");
        addPatient();
        break;
    case 2:
        system("cls");
        viewPatients();
        break;
    case 3:
        system("cls");
        searchPatient();
        break;
    case 4:
        cout << endl << "-----------------------------------" << endl << endl;
        cout << " Logging out";
        delay(500);
        cout << ". ";
        delay(500);
        cout << ". ";
        delay(500);
        cout << "." << endl;
        delay(1200);
        system("cls");
        homePage();
        break;
    default:
        delay(950);
        cout << endl << "Invalid choice!";
        cout << " Resetting";
        delay(500);
        cout << ". ";
        delay(500);
        cout << ". ";
        delay(500);
        cout << "." << endl;
        delay(1200);
        system("cls");
        loginSession();
        break;
    }
}

void viewPatients() {
    string line;
    ifstream patientsFile("data/profiles/" + userLoggedIn + "/patients.txt");
    if (!patientsFile.is_open()) {
        cout << "Unable to open file";
        exit(1);
    }

    ifstream patientsPrescription("data/profiles/" + userLoggedIn + "/prescriptions.txt");
    if (!patientsFile.is_open()) {
        cout << "Unable to open file";
        exit(1);
    }

    ifstream patientsBilling("data/profiles/" + userLoggedIn + "/billing.txt");
    if (!patientsFile.is_open()) {
        cout << "Unable to open file";
        exit(1);
    }

    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  List of Patients                                   |" << endl;
    cout << "-------------------------------------------------------" << endl;

    while (getline(patientsFile, line)) {
        cout << line << endl;
    }
    patientsFile.close();

    cout << "_______________________________________________________" << endl;
    cout << "|  List of Prescription Information                   |" << endl;
    cout << "-------------------------------------------------------" << endl;

    while (getline(patientsPrescription, line)) {
        cout << line << endl;
    }
    patientsPrescription.close();

    cout << "_______________________________________________________" << endl;
    cout << "|  List of Billing Information                        |" << endl;
    cout << "-------------------------------------------------------" << endl;

    while (getline(patientsBilling, line)) {
        cout << line << endl;
    }
    patientsBilling.close();

    cout << "-------------------------------------------------------" << endl << endl;
    system("pause");
    system("cls");
    loginSession();
}

void searchPatient() {
    string name;
    string line;
    int count = 0;

    cout << "-=-     MediVault Solutions     -=-" << endl << endl;
    cout << "_______________________________________________________" << endl;
    cout << "|  Enter Patient's Name to search                     |" << endl;
    cout << "-------------------------------------------------------" << endl;

    cout << "Search: ";
    cin.ignore();
    getline(cin, name);

    ifstream patientsFile("data/profiles/" + userLoggedIn + "/patients.txt");
    if (!patientsFile.is_open()) {
        cout << "Unable to open file";
        exit(1);
    }

    ifstream patientsBilling("data/profiles/" + userLoggedIn + "/billing.txt");
    if (!patientsBilling.is_open()) {
        cout << "Unable to open file";
        exit(1);
    }

    while (getline(patientsFile, line)) {
        if (line.find(name) != string::npos) {
            cout << "-------------------------------------------------------" << endl;
            cout << "Patient's Information: " << line << endl;
            cout << "Patient's Prescription: I didnt finish this function..." << endl;
            count++;
        }
    }
    patientsFile.close();

    while (getline(patientsBilling, line)) {
        if (line.find(name) != string::npos) {
            cout << "Patient's Billing:  " << line << endl;
            cout << "-------------------------------------------------------" << endl;
            count++;
        }
    }
    patientsBilling.close();

    if (count == 0) {
        cout << "-------------------------------------------------------" << endl;
        cout << "No patient found with the name: " << name << endl;
        cout << "-------------------------------------------------------" << endl << endl;
    }
    system("pause");
    system("cls");
    loginSession();
}

int main() {
    homePage();
    return 0;
}

