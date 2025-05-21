# Mechanical Testing Data Analysis

## Overview
This Python script processes mechanical testing data from the Excel file `experiment\excel_file.xlsx`. It cleans the data, segments it into cycles based on significant changes in force (`Kraft [N]`), generates time-force plots for each cycle, and exports the segmented data to separate Excel files. The script is designed to analyze time-series data related to force, stress, displacement, length, and strain.

## Dependencies
To run the script, ensure the following Python libraries are installed:
- `pandas`: For data manipulation and Excel file handling
- `matplotlib`: For creating plots

Install the dependencies using pip:
```bash
pip install pandas matplotlib
```

## Data
The script processes data from the Excel file `experiment\excel_file.xlsx`, which contains the following columns:
- `Zeit`: Time in seconds (renamed to `Zeit [s]`)
- `Kraft`: Force in Newtons (renamed to `Kraft [N]`)
- `Spannung`: Stress in N/mm² (renamed to `Spannung [N/mm²]`)
- `Weg`: Displacement in millimeters (renamed to `Weg [mm]`)
- `Länge`: Length in millimeters (renamed to `Länge [mm]`)
- `Dehnung`: Strain in percentage (renamed to `Dehnung [%]`)

## Script Functionality
The script performs the following tasks:

### 1. Data Loading and Cleaning
- Loads the Excel file using `pandas`.
- Drops the first row (likely a header or invalid data).
- Sorts the data by `Zeit` (time) and removes rows with missing values in `Zeit` or `Kraft`.
- Renames columns for clarity and consistency:
  - `Zeit` → `Zeit [s]`
  - `Kraft` → `Kraft [N]`
  - `Spannung` → `Spannung [N/mm²]`
  - `Weg` → `Weg [mm]`
  - `Länge` → `Länge [mm]`
  - `Dehnung` → `Dehnung [%]`
- Displays basic data information (`head`, `describe`).

### 2. Cycle Segmentation
- Identifies cycle boundaries based on significant force changes (>6000 N between consecutive points, followed by a smaller change <6000 N).
- Adjusts thresholds by selecting indices 8 rows before detected changes to account for cycle starts.
- Removes the second threshold (index 1) to refine cycle segmentation.
- Splits the DataFrame into separate DataFrames for each cycle based on the thresholds.

### 3. Visualizations
- Creates a time-force plot for each cycle, plotting `Zeit [s]` vs. `Kraft [N]`.
- Each plot is labeled with the cycle number and saved as a PNG file in the `experiment\Plots` directory:
  - `cycle 1.png`, `cycle 2.png`, etc.
- Plots are displayed and then closed to free memory.

### 4. Data Export
- Saves all cycle DataFrames to a single Excel file (`experiment\excel_file (edited).xlsx`) with each cycle in a separate sheet named `cycle 1`, `cycle 2`, etc.
- Saves each cycle DataFrame to individual Excel files in the `experiment\Cycles_ExcelFiles` directory:
  - `cycle 1.xlsx`, `cycle 2.xlsx`, etc.

## Usage
1. Ensure the Excel file `experiment\excel_file.xlsx` is in the `experiment` directory.
2. Create the `experiment\Plots` and `experiment\Cycles_ExcelFiles` directories in the working directory to store output plots and Excel files.
3. Run the script in a Python environment (e.g., Jupyter Notebook or a Python IDE).
4. The script will generate and save plots and Excel files in the specified directories.

Example command to run the script (if saved as `mechanical_analysis.py`):
```bash
python mechanical_analysis.py
```

## Notes
- The script assumes the Excel file has the specified column structure. Verify the file format if errors occur.
- The `%matplotlib inline` command is included for Jupyter Notebook compatibility. Remove it if running in a different environment.
- The cycle detection algorithm uses a force change threshold of 6000 N and a look-back of 8 rows. Adjust these parameters if needed for different datasets.
- Ensure the `experiment\Plots` and `experiment\Cycles_ExcelFiles` directories exist before running the script to avoid file-saving errors.
- The script removes the second threshold (index 1) from the cycle boundaries. Modify this step if all detected thresholds are needed.

## Output Files
- **Plots** (in `experiment\Plots`):
  - `cycle 1.png`, `cycle 2.png`, etc.: Time-force plots for each cycle.
- **Excel Files**:
  - `experiment\excel_file (edited).xlsx`: Single Excel file with each cycle in a separate sheet (`cycle 1`, `cycle 2`, etc.).
  - `experiment\Cycles_ExcelFiles\cycle 1.xlsx`, `cycle 2.xlsx`, etc.: Individual Excel files for each cycle.