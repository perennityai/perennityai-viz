# PerennityAI MediaPipe Data Visualizer
```perennityai-viz``` is a tool to visualize hand, face, and pose landmarks from MediaPipe data with animations and overlay capabilities. It helps developers and researchers easily view MediaPipe data in a dynamic, combined format for visual analysis and debugging.

## Features
- Animation Creation: Generate animations from landmark data for face, hand, and pose.
- Visualization of Individual Components: Separate visualizations for hands, face, and body pose landmarks.
- Combined Image Output: Overlays landmarks for all components in a single image.
- Support for Multiple File Formats: Compatible with .csv, parquet, and .tfrecord file formats.
- Automated Data Processing: Filters and cleans data for efficient visualization.

## Configuration Requirements
This tool assumes that you have already generated or extracted landmark points with the correct headers specified in the configuration file configs/config.ini. If these landmarks are not yet available, please use ```perennityai-mp-gen``` to extract and save landmark data before running this tool.

## Data Requirements
The tool expects the following columns to be present in all input files:

1. `phrase`: The target label or identifier for each frame.
2. `frame`: The frame number or timestamp associated with each set of landmarks.
3. 1629 MediaPipe landmark points: Each landmark should be represented as x, y, and z coordinates, totaling 1629 values (e.g., `x_right_hand_0`, `x_left_hand_0`,`x_pose_0`, `x_face_0`, ..., `z_right_hand_20`, `z_left_hand_20`, `z_face_467`, `z_pose_32`).

Ensure that these columns are correctly formatted in the CSV, Parquet, or TFRecord files before running the tool.

# Project Structure

```
perennity-viz/
│
├── src/                              # All source code in the 'src' folder
│   └── perennity_viz/                 # Your main package/module
│       ├── __init__.py               # Initializes the package/module
│       ├── main.py                   # Entry point for running the program
│       │
│       │── configs/                           # Central location for all configurations
│       |   └── config.ini
│       │
│       ├── utils/                    # Utilities folder (for helpers, etc.)
│       │   ├── __init__.py           # Initializes utils module
│       │   ├── csv_handler.py        # Handles CSV file operations
│       │   ├── tfrecord_processor.py # Handles TFRecord file operations
│       │   └── logger.py             # Logger configuration and functions
│       │
│       └── data_visualization/       # Folder for data visualization related code
│           ├── __init__.py           # Initializes data_visualization module
│           └── data_visualizer.py    # Main code for data visualization logic
│
├── tests/                            # Unit tests and test files
│   
│
│
├── MANIFEST.in                       # Specifies additional files for packaging
├── pyproject.toml                    # Build system configuration
├── setup.py                          # Package setup file
├── README.md                         # Project documentation
├── LICENSE                           # License file
└── requirements.txt                  # Dependencies for development

```

## Demo
Here are some demonstrations of the features and functionalities of the project:

### 1: Gift vs Girafee ASL Gesture Visualization
![Gift vs Girafee ASL Gesture Visualization](./demo/gift_girafee-demo.gif)

### 2: Girlfriend vs Girl ASL Gesture Visualization
![Girlfriend vs Girl ASL Gesture Visualization](./demo/girlfriend_girl-demo.gif)

### 3: Girlfriend vs Girl ASL Gesture Visualization
![Glass vs Glasses ASL Gesture Visualization](./demo/glass_glasses-demo.gif)


## Installation

Clone the repository and install required packages:

```bash
git clone https://github.com/your-username/perennityai-viz.git
cd perennityai-viz
pip install -r requirements.txt
```

## Usage
You can run the visualization tool directly from the command line. Use the following syntax:

```bash
python main.py --input_file <file_path> --output_dir <output_directory> # For CSV --data_input_format not needed!
python main.py --input_file <file_path> --output_dir <output_directory> --data_input_format parquet --verbose INFO
python main.py --input_dir <file_path> --output_dir <output_directory>  --csv_file_index 0 --data_input_format parquet
python main.py --input_dir <file_path> --output_dir <output_directory>  --tf_file_index 0 --data_input_format tfrecord
python main.py --input_dir <file_path> --output_dir <output_directory>  --parquet_file_index 0 --data_input_format parquet

```

## Command-Line Arguments
```bash
--input_file: Specifies the path to a single MediaPipe landmarks file, typically in .csv format (e.g., path/to/mp_landmarks_file/0.csv). 

--input_dir: Defines the directory path containing multiple MediaPipe landmarks files. 

--output_dir: The path where generated visualizations and processed files will be saved (e.g., path/to/output_directory).

--data_input_format: Specifies the input file format, typically "csv" or "tfrecord". This tells the tool what file type to expect in input_file or input_dir.

--verbose: Sets the logging level, which controls the amount of detail in console output. Use "INFO" for general information, "DEBUG" for detailed diagnostic information, or "ERROR" to show only critical errors.

--encoding: Defines the file encoding for reading CSV files (e.g., "ISO-8859-1"). 
```


```python

conda create --name data-viz-env python=3.10

conda activate data-viz-env

pip install perennityai-viz
from perennity_viz.data_visualization.data_visualizer import DataVisualizer

# Usage Example: Load from a pretrained configuration file
try:
    # Example of config file data
    config = {
    "input_file": "path/to/mp_landmarks_file/0.csv",
    "input_dir": "path/to/mp_landmarks_file",
    "output_dir": "path/to/output_directory",
    "verbose": "INFO",
    'encoding': 'ISO-8859-1' # CSV file encoding
    }


    # visualizer_from_pretrained = DataVisualizer.from_pretrained('path/to/config.json')
    visualizer_from_pretrained = DataVisualizer.from_pretrained(config)
    
    # Visualize the file, for example, with a CSV
    animation = visualizer_from_pretrained.visualize_data(
        tf_file_index=0  # index of CSV file in the input directory
    )

    # Process all samples and visualize .gif/mp4 file.
    for csv_file in glob.glob(os.path.join("path/to/mp_landmarks_file", '*.csv')):
        # Visualize the file, for example, with a CSV
        visualizer_from_pretrained.visualize_data(
            csv_file=csv_file
        )

except FileNotFoundError as e:
    print(f"Configuration loading failed: {e}")
```

## Key Classes and Methods
### DataVisualizer
Main class to handle data processing and visualization.
```
-- visualize_data: Visualizes data from a specified file by creating an animated view.
get_pose, get_face, get_hands: Methods to extract and visualize specific landmark types.

-- combine_images: Combines separate visualizations into a single, cohesive output.

-- create_animation: Generates an animation from landmark frames.
```

## License
This project is licensed under the MIT License. See the LICENSE file for details.