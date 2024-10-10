
using namespace std;

// Structure to represent an item
    struct Item {
    string name;
    string description;
    bool is_taken;
};

// Structure to represent a location
struct Location {
    string name;
    string description;
    vector<Item> items;
    vector<string> exits; // Directions to other locations
    string on_enter; // Message displayed when entering the location
}

// Function to display a message with line breaks
void displayMessage(string message) {
    cout << message << endl << endl;
}

// Function to get user input
string getInput() {
    string input;
    getline(cin, input);
    return input;
}

// Function to check if an item exists in the location
bool itemExists(vector<Item>& items, string itemName) {
    for (Item& item : items) {
        if (item.name == itemName) {
            return true;
        }
    }
    return false;
}

// Function to get an item from a location
Item* getItem(vector<Item>& items, string itemName) {
    for (Item& item : items) {
        if (item.name == itemName) {
            return &item;
        }
    }
    return nullptr;
}

int main() {
    srand(time(0)); // Seed random number generator for randomness

    // Define the locations
    vector<Location> locations = {
        {
            "Forest Clearing", 
            "You stand in a sun-dappled clearing in a dense forest. Sunlight filters through the leaves, casting intricate patterns on the forest floor.",
            {
                {"Key", "A rusty key.", false}, 
                {"Sword", "A gleaming steel sword.", false}
            }
            {"North", "East", "South", "West"},
            "You hear the rustling of leaves and the distant chirping of birds."
        }
        {
            "Haunted House", 
            "A crumbling mansion stands before you, its windows boarded up and its paint peeling. A cold breeze whistles through the broken panes, carrying with it a sense of foreboding.",
            {
                {"Gold Coin", "A shimmering gold coin.", false}
            },
            {"South"},
            "A faint moan echoes from within the house, sending shivers down your spine."
        },
        {
            "Hidden Cave", 
            "You descend into a dark and damp cave. The air is thick with the smell of moss and earth. You hear the dripping of water and the occasional squeaking of bats.",
            {


                {"Treasure Chest", "A heavy wooden chest, secured with a lock.", false}
            },
            {"North"},
            "The walls of the cave seem to close in around you, amplifying the sounds of your own breath."
        }
    };

    // Start the game
    displayMessage("Welcome to the Text-Based Adventure Game!");
    displayMessage("You find yourself in a mysterious world. Explore, gather items, and solve puzzles to escape.");

    // Current location index
    int currentLocation = 0;
    
    // Main game loop
    while (true) {
        // Display the current location
        displayMessage(locations[currentLocation].name);
        displayMessage(locations[currentLocation].description);

        // Display items in the location
        if (!locations[currentLocation].items.empty()) {
            displayMessage("You see the following items:");
            for (Item& item : locations[currentLocation].items) {
                if (!item.is_taken) {
                    cout << "- " << item.name << ": " << item.description << endl;
                }
            }
            cout << endl;
        }

        // Display exits from the location
        if (!locations[currentLocation].exits.empty()) {
            displayMessage("You can go:");
            for (string exit : locations[currentLocation].exits) {
                cout << "- " << exit << endl;
            }
            cout << endl;
        }

        // Get user input
        displayMessage("What do you do?");
        string input = getInput();

        // Process user input
        if (input == "look") {
            displayMessage(locations[currentLocation].description);
        } else if (input == "take") {
            displayMessage("What do you want to take?");
            string itemName = getInput();

            if (itemExists(locations[currentLocation].items, itemName)) {
                Item* item = getItem(locations[currentLocation].items, itemName);
                if (item != nullptr) {
                    if (item->is_taken) {
                        displayMessage("You have already taken the " + item->name + ".");
                    } else {
                        item->is_taken = true;
                        displayMessage("You take the " + itemName + ".");
                    }
                }
            } else {
                displayMessage("You don't see a " + itemName + " here.");
            }
        } else if (input == "north" || input == "south" || input == "east" || input == "west") {
            // Find the index of the next location
            int nextLocation = -1;
            for (int i = 0; i < locations[currentLocation].exits.size(); i++) {
                if (locations[currentLocation].exits[i] == input) {
                    nextLocation = i;
                    break;
                }
            }

            // Move to the next location
            if (nextLocation != -1) {
                currentLocation = nextLocation;
                displayMessage(locations[currentLocation].on_enter);
            } else {
                displayMessage("You can't go " + input + " from here.");
            }
        } else if (input == "help") {
            displayMessage("You can use the following commands:\n- look\n- take [item_name]\n- [north/south/east/west]\n- help");
        } else if (input == "quit") {
            displayMessage("Thanks for playing!");
            break; 
        } else {
            displayMessage("I don't understand.");
        }
    }
    
    return 0;
}
