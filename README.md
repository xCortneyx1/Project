#include <iostream>
#include <fstream>
#include <map>
#include <string>

class GroceryTracker {
private:
    std::string inputFileName;
    std::map<std::string, int> itemFrequencies;
    bool dataFileCreated;

    void LoadDataFromFile() {
        std::ifstream inputFile(inputFileName);
        if (inputFile.is_open()) {
            std::string item;
            while (std::getline(inputFile, item)) {
                itemFrequencies[item]++;
            }
            inputFile.close();
        }
    }

    void CreateDataFile() {
        std::ofstream outputFile("frequency.dat");
        if (outputFile.is_open()) {
            for (const auto& pair : itemFrequencies) {
                outputFile << pair.first << " " << pair.second << std::endl;
            }
            outputFile.close();
            dataFileCreated = true;
        }
    }

public:
    GroceryTracker(const std::string& fileName)
        : inputFileName(fileName), dataFileCreated(false) {
        LoadDataFromFile();
    }

    void Run() {
        int choice;
        while (true) {
            std::cout << "Menu:\n"
                      << "1. Search for an item\n"
                      << "2. Print item frequencies\n"
                      << "3. Print item histogram\n"
                      << "4. Exit\n"
                      << "Enter your choice: ";
            std::cin >> choice;

            switch (choice) {
                case 1: {
                    std::string searchItem;
                    std::cout << "Enter the item to search for: ";
                    std::cin.ignore();
                    std::getline(std::cin, searchItem);
                    int frequency = itemFrequencies[searchItem];
                    std::cout << "Frequency of " << searchItem << ": " << frequency << std::endl;
                    break;
                }
                case 2: {
                    for (const auto& pair : itemFrequencies) {
                        std::cout << pair.first << " " << pair.second << std::endl;
                    }
                    break;
                }
                case 3: {
                    for (const auto& pair : itemFrequencies) {
                        std::cout << pair.first << std::endl;
                        for (int i = 0; i < pair.second; ++i) {
                            std::cout << "*";
                        }
                        std::cout << std::endl;
                    }
                    break;
                }
                case 4: {
                    if (!dataFileCreated) {
                        CreateDataFile();
                    }
                    std::cout << "Exiting the program..." << std::endl;
                    return;
                }
                default:
                    std::cout << "Invalid choice. Please try again." << std::endl;
            }
        }
    }
};

int main() {
    GroceryTracker tracker("CS210_Project_Three_Input_File.txt");
    tracker.Run();
    return 0;
}
