# Package Delivery Tracking System
## Overview Lab Objective

## Assignment Code
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <unordered_map>
#include <iomanip>
#include <limits>
#include <cctype>

using namespace std;

struct Package
{
    string trackingNumber;
    string recipient;
    string destination;
    double weight;
    string status;
};

vector<Package> packages;
queue<string> processingQueue;

unordered_map<string, size_t> packageIndex;

string trim(const string& text)
{
    size_t start = text.find_first_not_of(" \t\r\n");

    if (start == string::npos)
    {
        return "";
    }

    size_t end = text.find_last_not_of(" \t\r\n");

    return text.substr(start, end - start + 1);
}

string readNonEmptyString(const string& prompt)
{
    string input;

    while (true)
    {
        cout << prompt;
        getline(cin, input);

        input = trim(input);

        if (!input.empty())
        {
            return input;
        }

        cout << "Error: This field cannot be empty.\n";
    }
}

double readPositiveWeight()
{
    double weight;

    while (true)
    {
        cout << "Enter package weight: ";

        if (cin >> weight)
        {
            cin.ignore(
                numeric_limits<streamsize>::max(),
                '\n'
            );

            if (weight > 0)
            {
                return weight;
            }

            cout << "Error: Weight must be greater than zero.\n";
        }
        else
        {
            cout << "Error: Enter a valid numeric weight.\n";

            cin.clear();

            cin.ignore(
                numeric_limits<streamsize>::max(),
                '\n'
            );
        }
    }
}

int readMenuChoice()
{
    int choice;

    while (true)
    {
        cout << "Enter your choice: ";

        if (cin >> choice)
        {
            cin.ignore(
                numeric_limits<streamsize>::max(),
                '\n'
            );

            return choice;
        }

        cout << "Error: Enter a whole number.\n";

        cin.clear();

        cin.ignore(
            numeric_limits<streamsize>::max(),
            '\n'
        );
    }
}

Package* findPackageByHash(const string& trackingNumber)
{
    auto it = packageIndex.find(trackingNumber);

    if (it == packageIndex.end())
    {
        return nullptr;
    }

    size_t index = it->second;

    if (index >= packages.size())
    {
        return nullptr;
    }

    return &packages[index];
}

Package* linearSearch(const string& trackingNumber)
{
    for (size_t i = 0; i < packages.size(); i++)
    {
        if (packages[i].trackingNumber == trackingNumber)
        {
            return &packages[i];
        }
    }

    return nullptr;
}

void displayPackage(const Package& package)
{
    cout << "\n----------------------------------------\n";

    cout << "Tracking Number: " << package.trackingNumber << '\n';
    cout << "Recipient:       " << package.recipient << '\n';
    cout << "Destination:     " << package.destination << '\n';

    cout << "Weight:          "
        << fixed << setprecision(2)
        << package.weight << '\n';

    cout << "Status:          " << package.status << '\n';

    cout << "----------------------------------------\n";
}

void displayPackageList(const vector<Package>& list)
{
    if (list.empty())
    {
        cout << "\nNo packages to display.\n";
        return;
    }

    cout << "\n";
    cout << left
        << setw(18) << "Tracking Number"
        << setw(22) << "Recipient"
        << setw(22) << "Destination"
        << setw(12) << "Weight"
        << "Status\n";

    cout << string(100, '-') << '\n';

    for (const Package& package : list)
    {
        cout << left
            << setw(18) << package.trackingNumber
            << setw(22) << package.recipient
            << setw(22) << package.destination
            << setw(12) << fixed << setprecision(2)
            << package.weight
            << package.status << '\n';
    }
}

void addPackage()
{
    cout << "\n========== ADD PACKAGE ==========\n";

    string trackingNumber =
        readNonEmptyString("Enter tracking number: ");

    if (packageIndex.find(trackingNumber) != packageIndex.end())
    {
        cout << "Error: Tracking number already exists.\n";
        return;
    }

    string recipient =
        readNonEmptyString("Enter recipient name: ");

    string destination =
        readNonEmptyString("Enter destination: ");

    double weight = readPositiveWeight();

    Package newPackage;

    newPackage.trackingNumber = trackingNumber;
    newPackage.recipient = recipient;
    newPackage.destination = destination;
    newPackage.weight = weight;
    newPackage.status = "Waiting";

    size_t index = packages.size();

    packages.push_back(newPackage);

    packageIndex[trackingNumber] = index;

    processingQueue.push(trackingNumber);

    cout << "\nPackage added successfully!\n";

    displayPackage(newPackage);
}

void processNextPackage()
{
    cout << "\n========== PROCESS NEXT PACKAGE ==========\n";

    if (processingQueue.empty())
    {
        cout << "No packages are waiting to be processed.\n";
        return;
    }

    string trackingNumber = processingQueue.front();

    processingQueue.pop();

    Package* package = findPackageByHash(trackingNumber);

    if (package == nullptr)
    {
        cout << "Error: Package record could not be located.\n";
        return;
    }

    if (package->status != "Waiting")
    {
        cout << "Warning: Package is no longer waiting.\n";
        return;
    }

    package->status = "Processed";

    cout << "\nPackage processed successfully!\n";

    displayPackage(*package);
}

void findPackage()
{
    cout << "\n========== FIND PACKAGE ==========\n";

    string trackingNumber =
        readNonEmptyString("Enter tracking number: ");

    Package* package = findPackageByHash(trackingNumber);

    if (package == nullptr)
    {
        cout << "Package not found.\n";
        return;
    }

    cout << "\nPackage found using hash-table lookup.\n";

    displayPackage(*package);
}

void updatePackageStatus()
{
    cout << "\n========== UPDATE PACKAGE STATUS ==========\n";

    string trackingNumber =
        readNonEmptyString("Enter tracking number: ");

    Package* package = findPackageByHash(trackingNumber);

    if (package == nullptr)
    {
        cout << "Package not found.\n";
        return;
    }

    cout << "\nCurrent status: " << package->status << '\n';

    if (package->status == "Waiting")
    {
        cout << "A waiting package must be processed through "
            << "the main queue.\n";

        return;
    }

    if (package->status == "Processed")
    {
        package->status = "In Transit";

        cout << "Status updated to In Transit.\n";
    }
    else if (package->status == "In Transit")
    {
        package->status = "Delivered";

        cout << "Status updated to Delivered.\n";
    }
    else if (package->status == "Delivered")
    {
        cout << "Package has already been delivered.\n";
        return;
    }
    else
    {
        cout << "Error: Invalid package status.\n";
        return;
    }

    displayPackage(*package);
}

void displayWaitingPackages()
{
    cout << "\n========== WAITING PACKAGES ==========\n";

    if (processingQueue.empty())
    {
        cout << "No packages are waiting.\n";
        return;
    }

    queue<string> temporaryQueue = processingQueue;

    cout << "\nWaiting packages in FIFO order:\n";

    while (!temporaryQueue.empty())
    {
        string trackingNumber = temporaryQueue.front();

        temporaryQueue.pop();

        Package* package = findPackageByHash(trackingNumber);

        if (package != nullptr && package->status == "Waiting")
        {
            displayPackage(*package);
        }
    }

    cout << "\nOriginal processing queue remains unchanged.\n";
}

void displayAllPackages()
{
    cout << "\n========== ALL PACKAGES ==========\n";

    displayPackageList(packages);
}

void insertionSortByTrackingNumber(vector<Package>& list)
{
    for (size_t i = 1; i < list.size(); i++)
    {
        Package key = list[i];

        int j = static_cast<int>(i) - 1;

        while (j >= 0 &&
            list[j].trackingNumber > key.trackingNumber)
        {
            list[j + 1] = list[j];

            j--;
        }

        list[j + 1] = key;
    }
}

void selectionSortByRecipient(vector<Package>& list)
{
    for (size_t i = 0; i < list.size(); i++)
    {
        size_t minimumIndex = i;

        for (size_t j = i + 1; j < list.size(); j++)
        {
            if (list[j].recipient < list[minimumIndex].recipient)
            {
                minimumIndex = j;
            }
        }

        if (minimumIndex != i)
        {
            Package temporary = list[i];

            list[i] = list[minimumIndex];

            list[minimumIndex] = temporary;
        }
    }
}

void displaySortedReport()
{
    cout << "\n========== SORTED PACKAGE REPORT ==========\n";

    cout << "1. Sort by Tracking Number (Insertion Sort)\n";
    cout << "2. Sort by Recipient (Selection Sort)\n";
    cout << "3. Sort by Weight (Insertion Sort)\n";

    int choice = readMenuChoice();

    vector<Package> sortedPackages = packages;

    if (sortedPackages.empty())
    {
        cout << "No packages available for sorting.\n";
        return;
    }

    if (choice == 1)
    {
        insertionSortByTrackingNumber(sortedPackages);

        cout << "\nSorted by tracking number using insertion sort.\n";
    }
    else if (choice == 2)
    {
        selectionSortByRecipient(sortedPackages);

        cout << "\nSorted by recipient using selection sort.\n";
    }
    else if (choice == 3)
    {
        
        for (size_t i = 1; i < sortedPackages.size(); i++)
        {
            Package key = sortedPackages[i];

            int j = static_cast<int>(i) - 1;

            while (j >= 0 && sortedPackages[j].weight > key.weight)
            {
                sortedPackages[j + 1] = sortedPackages[j];

                j--;
            }

            sortedPackages[j + 1] = key;
        }

        cout << "\nSorted by weight using insertion sort.\n";
    }
    else
    {
        cout << "Invalid sorting option.\n";
        return;
    }

    displayPackageList(sortedPackages);

    cout << "\nThe original package collection was not changed.\n";
}

void displaySystemSummary()
{
    cout << "\n========== SYSTEM SUMMARY ==========\n";

    size_t totalPackages = packages.size();

    size_t waitingCount = 0;
    size_t processedCount = 0;
    size_t inTransitCount = 0;
    size_t deliveredCount = 0;

    double totalWeight = 0.0;

    const Package* heaviestPackage = nullptr;

    for (const Package& package : packages)
    {
        totalWeight += package.weight;

        if (package.status == "Waiting")
        {
            waitingCount++;
        }
        else if (package.status == "Processed")
        {
            processedCount++;
        }
        else if (package.status == "In Transit")
        {
            inTransitCount++;
        }
        else if (package.status == "Delivered")
        {
            deliveredCount++;
        }

        if (heaviestPackage == nullptr ||
            package.weight > heaviestPackage->weight)
        {
            heaviestPackage = &package;
        }
    }

    cout << "\nTotal packages:      " << totalPackages << '\n';
    cout << "Waiting packages:    " << waitingCount << '\n';
    cout << "Processed packages:  " << processedCount << '\n';
    cout << "In-transit packages: " << inTransitCount << '\n';
    cout << "Delivered packages:  " << deliveredCount << '\n';

    cout << fixed << setprecision(2);

    cout << "Total weight:        " << totalWeight << '\n';

    if (totalPackages > 0)
    {
        double averageWeight =
            totalWeight / static_cast<double>(totalPackages);

        cout << "Average weight:      " << averageWeight << '\n';

        cout << "Heaviest package:    "
            << heaviestPackage->trackingNumber
            << " (" << heaviestPackage->weight
            << " weight units)\n";
    }
    else
    {
        cout << "Average weight:      N/A (no packages)\n";
        cout << "Heaviest package:    N/A (no packages)\n";
    }

    cout << "\n========================================\n";
}

void compareSearchMethods()
{
    cout << "\n========== SEARCH ALGORITHM COMPARISON ==========\n";

    string trackingNumber =
        readNonEmptyString("Enter tracking number: ");

    Package* hashResult = findPackageByHash(trackingNumber);

    Package* linearResult = linearSearch(trackingNumber);

    if (hashResult != nullptr)
    {
        cout << "\nHash-table search: Package found.\n";
    }
    else
    {
        cout << "\nHash-table search: Package not found.\n";
    }

    if (linearResult != nullptr)
    {
        cout << "Linear search: Package found.\n";
    }
    else
    {
        cout << "Linear search: Package not found.\n";
    }

    cout << "\nSearch complexity:\n";
    cout << "Linear search: O(N)\n";
    cout << "Hash-table lookup: O(1) average, O(N) worst case\n";
}

void displayMenu()
{
    cout << "\n";
    cout << "============================================\n";
    cout << "      PACKAGE DELIVERY TRACKING SYSTEM\n";
    cout << "============================================\n";

    cout << "1. Add Package\n";
    cout << "2. Process Next Package\n";
    cout << "3. Find Package by Tracking Number\n";
    cout << "4. Update Package Status\n";
    cout << "5. Display Waiting Packages\n";
    cout << "6. Display All Packages\n";
    cout << "7. Display Sorted Package Report\n";
    cout << "8. Display System Summary\n";
    cout << "9. Exit\n";

    cout << "============================================\n";
}

int main()
{
    cout << "Welcome to the Package Delivery Tracking System!\n";

    bool running = true;

    while (running)
    {
        displayMenu();

        int choice = readMenuChoice();

        switch (choice)
        {
        case 1:
            addPackage();
            break;

        case 2:
            processNextPackage();
            break;

        case 3:
            compareSearchMethods();
            break;

        case 4:
            updatePackageStatus();
            break;

        case 5:
            displayWaitingPackages();
            break;

        case 6:
            displayAllPackages();
            break;

        case 7:
            displaySortedReport();
            break;

        case 8:
            displaySystemSummary();
            break;

        case 9:
            cout << "\nThank you for using the system!\n";
            cout << "Program terminated successfully.\n";

            running = false;
            break;

        default:
            cout << "Invalid selection. Choose a number from 1 to 9.\n";
            break;
        }
    }

    return 0;
}
```
## Analysis, and Reflection

## Testing, and Process

## Flowchart

## Challenges

## Assignment Video Link:
