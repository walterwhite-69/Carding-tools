# Carding Tool 🛠️

Welcome to **Carding Tool**, a powerful command-line utility designed for carding enthusiasts and security researchers. This tool provides a suite of features to generate credit card numbers, check the validity of cards, and look up BIN (Bank Identification Number) details using a local database. Built with Python, this tool offers a colorful and user-friendly terminal interface, making it easy to use for both beginners and advanced users.

**Note**: This tool is for educational and research purposes only. Misuse of this tool for illegal activities is strictly prohibited. Always comply with local laws and regulations.

---

## Features 🌟

### 1. **Local BIN Checker**
The Local BIN Checker allows you to look up details about a credit card's BIN (first 6 digits of the card number) using a local database (`bin-list-data.csv`). This feature provides detailed information about the card, including:
- **Brand**: The card brand (e.g., Visa, Mastercard, American Express).
- **Type**: The card type (e.g., Credit, Debit, Prepaid).
- **Category**: The card category (e.g., Classic, Gold, Platinum).
- **Issuer**: The issuing bank or financial institution.
- **Issuer Contact**: Phone number and website of the issuer.
- **Country**: The country of issuance, including ISO codes (2-letter and 3-letter) and the country name.
- **Transaction Usability**: Determines if the card is usable for transactions based on the issuer and country (e.g., restricted countries like North Korea are flagged).

#### How It Works:
- The tool loads BIN data from `bin-list-data.csv` on the first run and saves it as a `bin_data.pkl` file for faster loading in future runs.
- You can input BINs manually or load them from a text file (one BIN per line).
- The tool checks each BIN against the database and displays detailed information for valid BINs.

#### Example Output:
```
BIN: 424182
Brand: Visa
Type: Debit
Category: Classic
Prepaid: No
Issuer: Sample Bank
Issuer Phone: 123-456-7890
Issuer URL: http://samplebank.com
Country: United States (US/USA)
Usable for Transactions: Yes
```

### 2. **Card Generator**
The Card Generator feature allows you to generate credit card numbers based on a given BIN. Generated cards include a valid card number (passing the Luhn algorithm), a random expiration date, and a CVV code.

#### Key Features:
- **Custom BIN Input**: Enter any BIN (e.g., `424182`) to generate cards starting with that BIN.
- **Luhn Algorithm**: Generated card numbers are valid according to the Luhn checksum algorithm.
- **Expiration Date**: Randomly generates a future expiration date (month and year) within the next 5 years.
- **CVV**: Generates a 3-digit CVV code.
- **Save Results**: Option to save generated cards to a text file.

#### How It Works:
- Input a BIN and the number of cards to generate.
- The tool generates cards in the format `cardnumber|month|year|cvv`.
- You can save the results to a file for later use.

#### Example Output:
```
Generated Cards:
4241821234567890|03|2027|123
4241829876543210|11|2028|456
```

### 3. **Card Checker**
The Card Checker feature validates credit card details and determines if a card is "Live" or "Dead" based on several criteria. This is useful for testing generated cards or checking existing ones.

#### Validation Criteria:
- **Luhn Algorithm**: Verifies the card number using the Luhn checksum.
- **Length Check**: Ensures the card number is 16 digits (customizable for other lengths if needed).
- **Expiration Date**: Checks if the card is not expired (must be valid as of the current date, April 2025).
- **CVV Length**: Ensures the CVV is 3 or 4 digits (4 for American Express, 3 for others).
- **Custom Heuristic**: Uses a hash-based method to simulate a "Live" or "Dead" status (30% chance of being "Dead").

#### How It Works:
- Input cards manually or load them from a text file (format: `cardnumber|month|year|cvv`).
- The tool checks each card against the validation criteria.
- Cards passing all checks are marked as "Live" or "Dead" based on the heuristic.
- Results can be saved to a text file.

#### Example Output:
```
Checking Cards...
Live: 4241821234567890|03|2027|123
Dead: 4241829876543210|11|2024|456

Live Cards:
1. 4241821234567890|03|2027|123

Dead Cards:
1. 4241829876543210|11|2024|456

1 cards found live out of 2
```

### 4. **Colorful Terminal UI**
The tool features a vibrant terminal interface with color-coded output for better readability:
- **Red**: Used for errors, "Dead" cards, and invalid BINs.
- **Green**: Used for "Live" cards, valid BINs, and successful operations.
- **Yellow**: Used for prompts and informational messages.
- **Cyan**: Used for menu options and secondary information.
- **Blue**: Used for headers and progress messages.

The UI is enhanced with ASCII art titles for each section, making the tool visually appealing.

### 5. **File I/O Support**
- **Load from Files**: Load BINs or cards from text files (one entry per line).
- **Save Results**: Save BIN lookup results, generated cards, or card checker results to text files using a file dialog.

### 6. **Performance Optimization**
- The tool uses `pickle` to serialize the BIN database into a `.pkl` file (`bin_data.pkl`) after the first run, significantly speeding up subsequent loads compared to re-parsing the CSV file.

---

## Installation ⚙️

### Prerequisites
- **Python 3.6+**: Ensure Python is installed on your system.
- **Dependencies**:
  - `colorama`: For Windows CMD color support.
  - Install it using:
    ```bash
    pip install colorama
    ```

### Running the Script
1. Clone or download this repository:
   ```bash
   git clone https://github.com/yourusername/carding-tool.git
   cd carding-tool
   ```
2. Place the `bin-list-data.csv` file in the same directory as `carding_tool.py`. This file should contain the BIN database with columns: `BIN`, `Brand`, `Type`, `Category`, `Issuer`, `IssuerPhone`, `IssuerUrl`, `isoCode2`, `isoCode3`, `CountryName`.
3. Run the script:
   ```bash
   python carding_tool.py
   ```
   - The first run will load data from `bin-list-data.csv` and create `bin_data.pkl` for faster loading in future runs.

### Creating a Standalone EXE
To create a standalone EXE file that includes the script and its dependencies:
1. Install `PyInstaller`:
   ```bash
   pip install pyinstaller
   ```
2. Run the script once to generate `bin_data.pkl`:
   ```bash
   python carding_tool.py
   ```
3. Convert the script to an EXE:
   - On **Windows**:
     ```bash
     pyinstaller --onefile --add-data "bin-list-data.csv;." --add-data "bin_data.pkl;." carding_tool.py
     ```
   - On **Mac/Linux**:
     ```bash
     pyinstaller --onefile --add-data "bin-list-data.csv:." --add-data "bin_data.pkl:." carding_tool.py
     ```
4. Find the EXE in the `dist` folder (`dist/carding_tool.exe`) and run it.

---

## Usage 📖

### Main Menu
Upon running the tool, you’ll see the main menu:
```
Select an option:
1. Bin Checker
2. Carding Tools
3. Exit
Enter your choice (1-3):
```

#### 1. Bin Checker
- **Option 1**: Load BINs from a text file.
- **Option 2**: Enter BINs manually (one per line, press Enter twice to check).
- Example:
  ```
  Enter BINs (one per line). Press Enter twice to check, or type 'exit' to return to menu.
  BIN: 424182
  BIN: 531116
  BIN:
  Checking BINs...
  Found valid BIN: 424182
  BIN: 424182
  Brand: Visa
  Type: Debit
  Category: Classic
  Prepaid: No
  Issuer: Sample Bank
  Issuer Phone: 123-456-7890
  Issuer URL: http://samplebank.com
  Country: United States (US/USA)
  Usable for Transactions: Yes
  ```

#### 2. Carding Tools
This submenu offers two features:
```
Select an option:
1. Card Checker
2. Card Generator
3. Back to Main Menu
Enter your choice (1-3):
```

- **Card Checker**:
  - Load cards from a text file or enter them manually.
  - Example:
    ```
    Enter cards (format: cardnumber|month|year|cvv, double Enter to check, or type 'exit' to return):
    Card: 4241821234567890|03|2027|123
    Card:
    Checking Cards...
    Live: 4241821234567890|03|2027|123
    ```

- **Card Generator**:
  - Enter a BIN and the number of cards to generate.
  - Example:
    ```
    Enter BIN: 424182
    Amount of cards to generate: 2
    Generated Cards:
    4241821234567890|03|2027|123
    4241829876543210|11|2028|456
    ```

#### 3. Exit
Exits the tool.

---

## Contributing 🤝

Contributions are welcome! If you’d like to contribute:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeature`).
3. Make your changes and commit them (`git commit -m "Add YourFeature"`).
4. Push to your branch (`git push origin feature/YourFeature`).
5. Open a Pull Request.

### Ideas for Contributions
- Add support for online BIN lookup APIs.
- Enhance the card checker with real transaction testing (with proper permissions).
- Add more validation rules for different card types (e.g., American Express 15-digit cards).
- Improve the UI with additional color themes or a GUI using Tkinter.

---

## License 📜

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Disclaimer ⚠️

This tool is intended for educational and research purposes only. The author is not responsible for any misuse of this tool. Using this tool for illegal activities, such as unauthorized carding or fraud, is strictly prohibited and may result in legal consequences. Always adhere to ethical guidelines and local laws.

---

## Contact 📧

For questions, suggestions, or support, feel free to reach out:
- **Discord**: Join our community at [discord.gg/rgWcEw5G8a](https://discord.gg/rgWcEw5G8a)
- **Author**: Walter

Happy carding (ethically)! 🚀v
