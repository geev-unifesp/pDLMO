markdown
# Sleep Data Processor

This script processes raw `.txt` files containing physiological or behavioral state data (e.g., sleep/wake states), extracts datetime information, filters full-day records (00:00–23:59), adds a binary `Sleep` column, and exports cleaned data as `.csv` files.

## Features

- Automatically detects header row containing `DATE/TIME`
- Handles files with `;` delimiter and `latin1` encoding
- Splits `DATE/TIME` into separate `DATE` and `TIME` columns
- Extracts hour and minute to identify full-day records (00:00 to 23:59)
- Creates a `Sleep` column based on the `STATE` column:
  - `0` or `4` → `0` (awake or other)
  - `1` or `2` → `1` (sleep)
- Saves processed data as semicolon-separated `.csv` files in a dedicated output folder

## Requirements

Install the required Python package:

```bash
pip install pandas
Folder Structure
text
.
├── process_sleep_data.py   # Main script
└── README.md
Input Format
The script expects .txt files with:

A header row starting with DATE/TIME

Columns separated by ;

A STATE column containing integer codes (0, 1, 2, 4)

Encoding: latin1

Example:

text
DATE/TIME;STATE;OTHER
2023-01-01 00:00;0;...
2023-01-01 00:01;1;...
Output
Processed files are saved in:

text
/Users/Your Folder Name
Each output file is named: [original_name]_processed.csv

Output columns order:

text
DATE, TIME, Sleep, [remaining original columns]
Usage
Update the folder_path variable inside the script to point to your folder containing .txt files.

Run the script:

bash
python process_sleep_data.py
Customization
To change the output folder location, modify the output_folder variable.

To alter sleep classification rules, edit the lambda function in:

python
df['Sleep'] = df['STATE'].apply(lambda x: 0 if x in [0, 4] else 1 if x in [1, 2] else None)
Error Handling
The script will:

Skip files without a DATE/TIME header

Skip files missing 00:00 or 23:59 timestamps (and process the full file instead)

Print an error message for any file that fails to process

Limitations
Assumes data are recorded in chronological order

Only the first 00:00 and last 23:59 are used for filtering

Does not handle time zones or daylight saving transitions

Author
Developed for processing actigraphy/sleep diary exports from systems using STATE codes.

License
Feel free to use and modify as needed.
